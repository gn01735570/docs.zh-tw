---
title: 分散式資料
description: 在整合型和雲端原生應用程式中的資料儲存。
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: b7c8c43b16f2f70f9009c4fe4a8d19c52fa7ea2a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163930"
---
# <a name="distributed-data"></a>分散式資料

如同我們在本書中所見，雲端原生方法改變了您設計、部署和管理應用程式的方式。 它也會變更您管理和儲存資料的方式。

圖5-1 對比差異。

![雲端原生應用程式中的資料儲存體](./media/distributed-data.png)

**圖 5-1**。 雲端原生應用程式中的資料管理

有經驗的開發人員很容易就能辨識圖5-1 左邊的架構。 在這個整合型 *應用程式*中，商務服務元件會一起共置在共用服務層中，並從單一關係資料庫共用資料。

在許多方面，單一資料庫保持資料管理的簡易。 跨多個資料表查詢資料很簡單。 變更資料更新或全部回復。 [ACID 交易](/windows/desktop/cossdk/acid-properties) 保證強式和立即一致性。

針對雲端原生設計，我們採用不同的方法。 在圖5-1 的右側，請注意商務功能如何隔離為小型、獨立的微服務。 每個微服務都會封裝特定的商務功能及其本身的資料。 整合型資料庫分解為具有許多較小資料庫的分散式資料模型，每個資料庫都與微服務一致。 當冒煙清除時，我們會在 *每個微服務*上公開一個資料庫的設計。

## <a name="database-per-microservice-why"></a>每個微服務的資料庫，原因為何？

此資料庫每個微服務提供許多優點，特別是對於必須快速發展且支援大規模的系統。 使用此模型 .。。

- 網域資料封裝在服務內
- 資料結構描述可以演進而不會直接影響其他服務
- 每個資料存放區都可以獨立調整
- 一個服務中的資料存放區失敗不會直接影響其他服務

將資料隔離也可讓每個微服務執行最適合其工作負載、儲存體需求和讀取/寫入模式的資料存放區類型。 選項包括關聯式、檔、索引鍵/值，甚至是以圖形為基礎的資料存放區。

圖5-2 提供雲端原生系統中多語言持續性的原則。

![多語言資料持續性](./media/polyglot-data-persistence.png)

**圖 5-2**。 多語言資料持續性

請注意，在上圖中，每個微服務都支援不同類型的資料存放區。

- 產品目錄微服務使用關係資料庫來容納其基礎資料的豐富關聯式結構。
- 購物車微服務使用分散式快取，其支援簡單、索引鍵/值資料存放區。
- 訂購微服務會同時使用 NoSql 檔資料庫進行寫入作業，以及高度反正規化的索引鍵/值存放區，以容納大量的讀取作業。
  
雖然關係資料庫與複雜資料的微服務保持相關，但 NoSQL 資料庫已獲得相當的普及。 它們提供大規模和高可用性。 他們的無架構本質可讓開發人員離開具型別資料類別和 Orm 的架構，而使變更成本高昂且耗時。 本章節稍後將討論 NoSQL 資料庫。

 將資料封裝成不同的微服務可以提高靈活性、效能和擴充性，同時也會帶來許多挑戰。 在下一節中，我們將討論這些挑戰，以及可協助克服這些挑戰的模式和作法。  

## <a name="cross-service-queries"></a>跨服務查詢

雖然微服務是獨立的，而且會專注于特定的功能功能，例如清查、出貨或訂購，它們通常需要與其他微服務整合。 整合通常牽涉到一個微服務 *查詢* 另一個資料。 圖5-3 顯示案例。

![跨微服務查詢](./media/cross-service-query.png)

**圖 5-3**。 跨微服務查詢

在上圖中，我們看到一個購物籃微服務，可將專案新增至使用者的購物籃。 當此微服務的資料存放區包含購物籃和明細專案資料時，它不會維護產品或定價資料。 相反地，這些資料項目目是由目錄和定價微服務所擁有。 這會呈現問題。 購物籃微服務如何將產品新增至使用者的購物籃（若其資料庫中沒有產品或定價資料）？

第4章中所討論的一個選項，是從購物籃到目錄和定價微服務的 [直接 HTTP 呼叫](service-to-service-communication.md#queries) 。 不過，在第4章中，我們說了同步 *HTTP 呼叫會* 微服務在一起，以降低其自主性並降低其架構的優點。

我們也可以針對每個服務，使用個別的輸入和輸出佇列來執行要求-回復模式。 不過，此模式很複雜，需要進行管線以使要求和回應訊息相互關聯。
雖然它會將後端微服務呼叫分離，呼叫端服務仍必須以同步方式等待來電完成。 網路擁塞、暫時性錯誤或多載微服務，可能會導致長時間執行甚至失敗的作業。

相反地，移除跨服務相依性的廣泛接受模式是 [具體化視圖模式](/azure/architecture/patterns/materialized-view)，如圖5-4 所示。

![具體化視圖模式](./media/materialized-view-pattern.png)

**圖 5-4**。 具體化視圖模式

使用此模式時，您可以將本機資料表 (稱為「 *讀取模型*) 在「購物籃」服務中。 此資料表包含產品和定價微服務所需的反正規化資料複本。 將資料直接複製到購物籃微服務，可免除昂貴的跨服務通話需求。 使用服務的本機資料，您可以改善服務的回應時間和可靠性。 此外，擁有自己的資料複本可讓購物籃服務更具復原性。 如果目錄服務應變成無法使用，則不會直接影響購物籃服務。 購物籃可以繼續使用自己的商店中的資料進行操作。

使用這個方法時，您的系統中現在會有重複的資料。 不過，在雲端原生系統中 *策略性* 複製資料是已建立的作法，並不會被視為反模式或不良的做法。 請記住， *只有一項服務* 可以擁有資料集，並具有其許可權。 當記錄的系統更新時，您必須同步處理讀取模型。 同步處理通常透過具有 [發佈/訂閱模式](service-to-service-communication.md#events)的非同步訊息來執行，如圖5.4 所示。

## <a name="distributed-transactions"></a>分散式交易

雖然跨微服務查詢資料很困難，但是跨多個微服務執行交易更為複雜。 在不同微服務中維持獨立資料來源之間的資料一致性的固有挑戰，無法 understated。 雲端原生應用程式中沒有分散式交易，表示您必須以程式設計的方式管理分散式交易。 您可以從 *立即一致性* 的世界移至 *最終一致性*的世界。

圖5-5 顯示問題。

![Saga 模式中的交易](./media/saga-transaction-operation.png)

**圖 5-5**。 跨微服務執行交易

在上圖中，有五個獨立的微服務參與可建立訂單的分散式交易。 每個微服務都會維護它自己的資料存放區，並為其存放區執行本機交易。 若要建立訂單， *每個* 個別微服務的本機交易必須成功，或 *全部都* 必須中止並回復作業。 雖然內建交易支援可在每個微服務內使用，但不支援跨所有五個服務的分散式交易，以保持資料一致。

相反地，您必須以程式設計的 *方式*來建立此分散式交易。

新增分散式交易支援的常見模式是 Saga 模式。 它是透過以程式設計方式將本機交易群組在一起，並依序叫用每一個。 如果有任何本機交易失敗，Saga 會中止作業，並叫用一組 [補償交易](/azure/architecture/patterns/compensating-transaction)。 補償交易會復原之前的本機交易所做的變更，並還原資料一致性。 圖5-6 顯示具有 Saga 模式的失敗交易。

![在 saga 模式中復原](./media/saga-rollback-operation.png)

**圖 5-6**。 復原交易

在上圖中，清查微服務中的 *更新清查* 作業失敗。 Saga 會叫用一組以紅色)  (的補償交易來調整清查計數、取消付款和訂單，然後將每個微服務的資料傳回一致的狀態。

Saga 模式通常會單純為一系列的相關事件，或以一組相關的命令進行協調。 在第4章中，我們討論了可作為協調 saga 實施基礎的服務匯總工具模式。 我們也討論了 Azure 服務匯流排事件，以及可作為單純 saga 實作為基礎的 Azure 事件方格主題。

## <a name="high-volume-data"></a>大量資料

大型的雲端原生應用程式通常支援大量資料需求。 在這些案例中，傳統的資料儲存技術可能會造成瓶頸。 對於大規模部署的複雜系統而言，命令與查詢責任隔離 (CQRS) 和事件來源都可以改善應用程式效能。  

### <a name="cqrs"></a>CQRS

[CQRS](/azure/architecture/patterns/cqrs)是一種架構模式，可協助將效能、擴充性和安全性最大化。 此模式會分隔從寫入資料的作業讀取資料的作業。

在一般案例中，會使用相同的實體模型和資料存放庫物件來 *進行讀取和* 寫入作業。

不過，大量資料案例可以從個別的模型和資料表獲益，以進行讀取和寫入。 為了改善效能，讀取作業可以針對資料的高度反正規化表示進行查詢，以避免昂貴的重復資料表聯結和表鎖。 *寫入*作業（稱為*命令*）會針對保證一致性的資料進行完全正規化的標記法更新。 然後，您必須執行機制，讓兩個表示保持同步。一般而言，每當修改寫入資料表時，都會發行將修改複寫到讀取資料表的事件。

圖5-7 顯示 CQRS 模式的執行。

![命令與查詢責任隔離](./media/cqrs-implementation.png)

**圖 5-7**。 CQRS 執行

在上圖中，會執行個別的命令和查詢模型。 每個資料寫入作業都會儲存至寫入存放區，然後傳播到讀取存放區。 請密切注意資料傳播程式如何以 [最終一致性](https://www.cloudcomputingpatterns.org/eventual_consistency/)的原則運作。 讀取模型最後會與寫入模型進行同步處理，但程式中可能會有一些延遲。 我們將在下一節討論最終一致性。

這種分隔可讓讀取和寫入獨立調整。 讀取作業會使用針對查詢優化的架構，而寫入會使用針對更新優化的架構。 讀取查詢會針對未正規化的資料進行，而複雜的商務邏輯則可以套用至寫入模型。 同樣地，您可能會對寫入作業強加較嚴格的安全性，而不是公開讀取的作業。

實施 CQRS 可以改善雲端原生服務的應用程式效能。 但是，它會產生更複雜的設計。 請謹慎且策略性地將此原則套用至雲端原生應用程式中可從中獲益的部分。 如需 CQRS 的詳細資訊，請參閱 Microsoft book [.Net 微服務：容器化 .Net 應用程式的架構](../microservices/microservice-ddd-cqrs-patterns/apply-simplified-microservice-cqrs-ddd-patterns.md)。

### <a name="event-sourcing"></a>事件來源

優化大量資料案例的另一種方法就是 [事件來源](/azure/architecture/patterns/event-sourcing)。

系統通常會儲存資料實體的目前狀態。 例如，如果使用者變更其電話號碼，就會以新的號碼更新客戶記錄。 我們一律知道資料實體的目前狀態，但每個更新都會覆寫先前的狀態。

在大部分情況下，此模型會正常運作。 不過，在高容量系統中，交易式鎖定和頻繁更新作業的額外負荷可能會影響資料庫效能、回應性和限制擴充性。

事件來源採用不同的方法來捕捉資料。 影響資料的每個作業都會保存到事件存放區。 我們不會更新資料記錄的狀態，而是會將每個變更附加至過去事件的連續清單，類似于會計的總帳。 事件存放區會成為資料的記錄系統。 它是用來在微服務的系結內容中傳播各種具體化的觀點。 圖5.8 顯示模式。

![事件來源](./media/event-sourcing.png)

**圖 5-8**。 事件來源

在上圖中，請注意，針對使用者的購物車，每個專案的藍色)  (會附加到基礎事件存放區。 在連續的具體化視圖中，系統會重新執行所有與購物車相關聯的事件，以投射目前的狀態。 此視圖（或讀取模型）接著會公開回 UI。 事件也可以與外部系統和應用程式整合，或進行查詢以判斷實體的目前狀態。 使用這個方法，您就可以維護歷程記錄。 您不僅知道實體的目前狀態，也知道您如何達到此狀態。

以機械說而言，事件來源簡化了寫入模型。 沒有任何更新或刪除。 將每個資料項目附加為固定事件，可將與關係資料庫相關聯的爭用、鎖定和並行衝突降至最低。 使用具體化視圖模式建立讀取模型可讓您將視圖與寫入模型分離，然後選擇最佳的資料存放區，以優化應用程式 UI 的需求。

針對此模式，請考慮直接支援事件來源的資料存放區。 Azure Cosmos DB、MongoDB、Cassandra、CouchDB 和 RavenDB 都是理想的候選項目。

如同所有模式和技術，請策略性地執行，並視需要執行。 雖然事件來源可以提供更高的效能和擴充性，但代價是複雜性和學習曲線。

>[!div class="step-by-step"]
>[上一個](service-mesh-communication-infrastructure.md) 
>[下一步](relational-vs-nosql-data.md)
