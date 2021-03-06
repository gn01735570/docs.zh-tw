---
title: 雲端原生通訊模式
description: 瞭解雲端原生應用程式中的重要服務通訊考慮
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: 5ce789924e828865f7bdf717b081b9112203293a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160914"
---
# <a name="cloud-native-communication-patterns"></a>雲端原生通訊模式

當您建立雲端原生系統時，通訊會成為很重要的設計決策。 前端用戶端應用程式如何與後端微服務通訊？ 後端微服務如何彼此通訊？ 在雲端原生應用程式中進行通訊時，要考慮的原則、模式和最佳作法為何？

## <a name="communication-considerations"></a>通訊考慮

在整合型應用程式中，通訊很簡單。 程式碼模組會在相同的可執行空間中一起執行 (處理伺服器上的) 。 當所有專案都在共用記憶體中一起執行時，此方法會有效能優勢，但會產生緊密結合的程式碼，使其變得難以維護、演進和調整。

雲端原生系統採用以微服務為基礎的架構，其具有許多小型且獨立的微服務。 每個微服務都會在個別的進程中執行，而且通常會在部署至叢集*的容器內執行。*

叢集會將虛擬機器的集區群組在一起，以形成高可用性環境。 它們是使用協調流程工具管理的，負責部署和管理容器化微服務。 圖4-1 顯示使用完全受控的[Azure Kubernetes 服務](/azure/aks/intro-kubernetes)，將[Kubernetes](https://kubernetes.io)叢集部署到 azure 雲端。

![Azure 中的 Kubernetes 叢集](./media/kubernetes-cluster-in-azure.png)

**圖 4-1**。 Azure 中的 Kubernetes 叢集

在整個叢集中，微服務會透過 Api 和訊息技術彼此通訊。

雖然它們提供許多優點，但微服務並沒有任何免費的午餐。 元件之間的本機同進程方法呼叫現在會取代為網路呼叫。 每個微服務都必須透過網路通訊協定進行通訊，這會增加系統的複雜度：

- 網路擁塞、延遲和暫時性錯誤都是固定的問題。

- 復原 (也就是，重試失敗的要求) 是不可或缺的。

- 某些呼叫必須具有 [等冪](https://www.restapitutorial.com/lessons/idempotency.html) 性，才能維持一致的狀態。

- 每個微服務都必須驗證並授權呼叫。

- 每個訊息都必須序列化然後還原序列化，這可能會很耗費資源。

- 訊息加密/解密變得很重要。

[.Net 微服務：容器化 .Net 應用程式的架構](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)（可從 Microsoft 免費取得）可為微服務應用程式提供深入的通訊模式涵蓋範圍。 在本章中，我們會提供這些模式的概要說明，以及 Azure 雲端中可用的執行選項。

在本章中，我們會先解決前端應用程式與後端微服務之間的通訊。 接著，我們會查看後端微服務彼此通訊的方式。 我們將探索 up 和 gRPC 通訊技術。 最後，我們將使用服務網格技術來尋找新的創新通訊模式。 我們也會看到 Azure 雲端如何提供不同類型的支援 *服務* ，以支援雲端原生通訊。

>[!div class="step-by-step"]
>[上一個](other-deployment-options.md) 
>[下一步](front-end-communication.md)
