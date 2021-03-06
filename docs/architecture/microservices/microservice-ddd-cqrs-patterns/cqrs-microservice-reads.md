---
title: 在 CQRS 微服務中實作讀取/查詢
description: .NET 微服務：容器化 .NET 應用程式的架構 | 了解 CQRS 查詢端使用 Dapper 在 eShopOnContainers 訂購微服務上的實作。
ms.date: 10/08/2018
ms.openlocfilehash: e6ea7b4b7b37df9ee972319f597ab045bf3bd215
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2020
ms.locfileid: "90678799"
---
# <a name="implement-readsqueries-in-a-cqrs-microservice"></a>在 CQRS 微服務中實作讀取/查詢

如需讀取/查詢，eShopOnContainers 參考應用程式的訂購微服務，會在 DDD 模型和交易區域之外實作查詢。 這項作業能夠完成，主要是因為查詢和交易的需求大不相同。 寫入的執行交易必須與網域邏輯相容。 另一方面，查詢具有等冪性，可從網域規則中隔離。

方法很簡單，如圖 7-3 所示。 API 介面是由 Web API 控制器實作，這些控制器會使用任何基礎結構，例如 Dapper 等微物件關聯式對應 (ORM)，並根據 UI 應用程式的需求傳回動態 ViewModel。

![顯示簡化的 CQRS 中高階查詢的圖表。](./media/cqrs-microservice-reads/simple-approach-cqrs-queries.png)

**圖 7-3**. CQRS 微服務中最簡單的查詢方法

以簡化的 CQRS 方法查詢端最簡單的方法，就是使用類似 Dapper 的微 ORM 查詢資料庫，傳回動態 Viewmodel。 查詢定義會查詢資料庫，並傳回每個查詢立即建立的動態 ViewModel。 因為查詢都具有等冪性，所以無論您執行查詢多少次，它們都不會變更資料。 因此，您不必受限於交易端使用的任何 DDD 模式，例如彙總及其他模式，這就是查詢與交易區域分開的原因。 您可以查詢資料庫，以找出 UI 所需的資料，並傳回不需要在任何地方以靜態方式定義的動態 ViewModel， (沒有) Viewmodel 的類別，除了 SQL 語句本身之外。

因為這種方法很簡單，所以查詢端所需的程式碼 (例如使用 [Dapper](https://github.com/StackExchange/Dapper)) 等微型 ORM 的程式碼，可以在 [相同的 Web API 專案內](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Queries/OrderQueries.cs)執行。 如圖 7-4 所示。 查詢是在 eShopOnContainers 解決方案內的 **Ordering.API** 微服務專案中所定義。

![排序. API 專案的 [查詢] 資料夾的螢幕擷取畫面。](./media/cqrs-microservice-reads/ordering-api-queries-folder.png)

**圖 7-4**。 eShopOnContainers 訂購微服務中的查詢

## <a name="use-viewmodels-specifically-made-for-client-apps-independent-from-domain-model-constraints"></a>使用專為用戶端應用程式建立的 ViewModel，獨立於網域模型限制式之外

因為執行查詢是為取得用戶端應用程式所需要的資料，所以可以根據查詢傳回的資料，專為用戶端建立傳回型別。 這些模型或資料傳輸物件 (DTO)，稱為 ViewModel。

傳回的資料 (ViewModel) 可以是多個實體或資料庫多資料表的聯結資料結果，甚至可以是跨多個在交易區域網域模型中定義的彙總結果。 在此情況下，因為您要建立與網域模型無關的查詢，所以會忽略匯總界限和條件約束，而且您可以自由地查詢任何您可能需要的資料表和資料行。 此方法為開發人員建立或更新查詢提供了極大的彈性和生產力。

Viewmodel 可以是在類別中定義的靜態類型 (如同在訂購微服務) 中執行一樣。 或者，您也可以根據執行的查詢來動態建立它們，這對開發人員而言非常靈活。

## <a name="use-dapper-as-a-micro-orm-to-perform-queries"></a>將 Dapper 用作微 ORM 以執行查詢

您可以使用任何微 ORM、Entity Framework Core 或甚至純 ADO.NET 進行查詢。 在範例應用程式中，eShopOnContainers 的訂購微服務選取 Dapper 為熱門的微 ORM 範例。 它可以執行具有絕佳效能的純 SQL 查詢，因為它是輕量的架構。 您可以使用 Dapper 撰寫能存取並聯結多份資料表的 SQL 查詢。

Dapper 是開放原始碼專案 (原創者為 Sam Saffron)，屬於 [Stack Overflow](https://stackoverflow.com/) (堆疊溢位) 的建置組塊。 若要使用 Dapper，您只需要透過 [Dapper NuGet 套件](https://www.nuget.org/packages/Dapper)安裝它即可，如下圖所示：

![NuGet 套件視圖中 Dapper 套件的螢幕擷取畫面。](./media/cqrs-microservice-reads/drapper-package-nuget.png)

您也需要加入指示詞 `using` ，讓您的程式碼可以存取 Dapper 擴充方法。

當您在程式碼中使用 Dapper 時，您可直接使用 <xref:System.Data.SqlClient> 命名空間提供的 <xref:System.Data.SqlClient.SqlConnection> 類別。 透過 QueryAsync 方法和其他擴充類別的擴充方法 <xref:System.Data.SqlClient.SqlConnection> ，您可以直接且高效能的方式執行查詢。

## <a name="dynamic-versus-static-viewmodels"></a>動態 ViewModel 與靜態 ViewModel

從伺服器端將 ViewModel 傳回用戶端應用程式時，您可以將這些 ViewModel 視為不同於您實體模型內部網域實體的 DTO (資料傳輸物件)，因為 ViewModel 是依用戶端應用程式需要的方式保留資料。 因此，在許多情況下，您可以彙總來自多個網域實體的資料，並根據用戶端應用程式需要該資料的方式精確撰寫 ViewModel。

這些 Viewmodel 或 Dto 可以明確地定義 (為數據持有者類別) ，就像 `OrderSummary` 稍後的程式碼片段中所示的類別一樣。 或者，您可以只根據查詢傳回的屬性，將動態 Viewmodel 或動態 Dto 傳回為動態類型。

### <a name="viewmodel-as-dynamic-type"></a>ViewModel 作為動態類型

如下列程式碼所示，透過傳回在內部以查詢所傳回屬性為基礎的「動態」** 類型，查詢可以直接傳回 `ViewModel`。 這表示要傳回的屬性子集是以查詢本身為基礎。 因此，如果您在查詢或聯結中新增新的資料行，該資料會以動態方式新增至傳回的 `ViewModel`。

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
    public async Task<IEnumerable<dynamic>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<dynamic>(
                @"SELECT o.[Id] as ordernumber,
                o.[OrderDate] as [date],os.[Name] as [status],
                SUM(oi.units*oi.unitprice) as total
                FROM [ordering].[Orders] o
                LEFT JOIN[ordering].[orderitems] oi ON o.Id = oi.orderid
                LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                GROUP BY o.[Id], o.[OrderDate], os.[Name]");
        }
    }
}
```

重點是，透過使用動態類型，傳回的資料集合會以動態方式組合成 ViewModel。

**專業人員：** 每當您更新查詢的 SQL 句子時，這個方法就能減少修改靜態 ViewModel 類別的需求，讓這種設計方法在撰寫程式碼、直接且快速地在未來的變更中發展時變得敏捷。

**缺點：** 長期來看，動態類型對清晰度和服務與用戶端應用程式的相容性可能造成負面影響。 此外，如果使用動態類型，像 Swashbuckle 這類的中介軟體無法提供傳回型別的同級文件。

### <a name="viewmodel-as-predefined-dto-classes"></a>ViewModel 作為預先定義的 DTO 類別

**專業人員**：擁有靜態、預先定義的 ViewModel 類別，例如以明確 DTO 類別為基礎的「合約」，對於公用 api （也適用于長期微服務）來說，也是較好的方法，即使它們只由相同的應用程式使用也是如此。

如要指定 Swagger 的回應類型，您需要使用明確的 DTO 類別作為傳回型別。 因此，預先定義的 DTO 類別可讓您提供更豐富的 Swagger 資訊。 這會在使用 API 時改善 API 文件和相容性。

**缺點：** 如前所述，更新程式碼時，它會採用更多步驟來更新 DTO 類別。

以*我們的經驗為基礎的秘訣*：在 EShopOnContainers 的訂購微服務上所執行的查詢中，我們開始使用動態 viewmodel 進行開發，因為它在早期開發階段是直接且敏捷的。 但在穩定開發之後，我們選擇重構 Api，並針對 Viewmodel 使用靜態或預先定義的 Dto，因為更清楚地讓微服務的取用者知道明確的 DTO 型別，用來作為「合約」。

在下例中，您會看到查詢如何使用明確的 ViewModel DTO 類別：OrderSummary 類別，傳回資料。

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
  public async Task<IEnumerable<OrderSummary>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<OrderSummary>(
                  @"SELECT o.[Id] as ordernumber,
                  o.[OrderDate] as [date],os.[Name] as [status],
                  SUM(oi.units*oi.unitprice) as total
                  FROM [ordering].[Orders] o
                  LEFT JOIN[ordering].[orderitems] oi ON  o.Id = oi.orderid
                  LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                  GROUP BY o.[Id], o.[OrderDate], os.[Name]
                  ORDER BY o.[Id]");
        }
    }
}
```

#### <a name="describe-response-types-of-web-apis"></a>描述 Web API 的回應類型

取用 web Api 和微服務的開發人員最關心的是傳回的內容，特別是回應類型和錯誤碼（如果不是標準)  (）。 回應類型會在 XML 批註和資料批註中處理。

Swagger UI 中沒有正確的文件，取用者就不清楚應該傳回哪些類型或可以傳回哪些 HTTP 代碼。 已藉由新增 <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute?displayProperty=nameWithType> 來修正該問題，因此 Swashbuckle 可以產生 API 傳回模型和值的更多相關資訊，如下列程式碼所示：

```csharp
namespace Microsoft.eShopOnContainers.Services.Ordering.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class OrdersController : Controller
    {
        //Additional code...
        [Route("")]
        [HttpGet]
        [ProducesResponseType(typeof(IEnumerable<OrderSummary>),
            (int)HttpStatusCode.OK)]
        public async Task<IActionResult> GetOrders()
        {
            var userid = _identityService.GetUserIdentity();
            var orders = await _orderQueries
                .GetOrdersFromUserAsync(Guid.Parse(userid));
            return Ok(orders);
        }
    }
}
```

不過，`ProducesResponseType` 屬性不能用作動態類型，卻需要使用像 `OrderSummary` ViewModel DTO 的明確類型，如下列範例所示：

```csharp
public class OrderSummary
{
    public int ordernumber { get; set; }
    public DateTime date { get; set; }
    public string status { get; set; }
    public double total { get; set; }
}
```

這是就長期而言，明確的傳回型別優於動態類型的另一個原因。 使用屬性時 `ProducesResponseType` ，您也可以指定預期的 HTTP 錯誤/代碼可能結果，例如200、400等等。

在下圖中，您會看到 Swagger UI 如何顯示 ResponseType 資訊。

![訂購 API 的 Swagger UI 頁面螢幕擷取畫面。](./media/cqrs-microservice-reads/swagger-ordering-http-api.png)

**圖 7-5**. 顯示 Web API 回應類型和可能 HTTP 狀態碼的 Swagger UI

影像會根據 ViewModel 類型以及可傳回的可能 HTTP 狀態碼，顯示一些範例值。

## <a name="additional-resources"></a>其他資源

- **Dapper**  
 <https://github.com/StackExchange/dapper-dot-net>

- **Julie Lerman。資料點-Dapper、Entity Framework 和混合式應用程式 (MSDN 雜誌文章) **  
  <https://docs.microsoft.com/archive/msdn-magazine/2016/may/data-points-dapper-entity-framework-and-hybrid-apps>

- **使用 Swagger 來 ASP.NET Core Web API 說明頁面**  
  <https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger?tabs=visual-studio>

>[!div class="step-by-step"]
>[上一個](eshoponcontainers-cqrs-ddd-microservice.md) 
>[下一步](ddd-oriented-microservice.md)
