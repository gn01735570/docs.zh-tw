---
ms.openlocfilehash: a93856aac97af5c392a2e4698d2da42413cfc3c8
ms.sourcegitcommit: 98d20cb038669dca4a195eb39af37d22ea9d008e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2020
ms.locfileid: "92434992"
---
### <a name="aspnet-core-apps-allow-deserializing-quoted-numbers"></a>ASP.NET Core apps 允許將引號數位還原序列化

從 .NET 5.0 開始，ASP.NET Core apps 會使用所指定的預設還原序列化選項 <xref:System.Text.Json.JsonSerializerDefaults.Web?displayProperty=nameWithType> 。 <xref:System.Text.Json.JsonSerializerDefaults.Web>選項組包括將設定 <xref:System.Text.Json.JsonSerializerOptions.NumberHandling> 為 <xref:System.Text.Json.Serialization.JsonNumberHandling.AllowReadingFromString?displayProperty=nameWithType> 。 這項變更表示 ASP.NET Core apps 將會成功將以 JSON 字串表示的數位還原序列化，而不是擲回例外狀況。

#### <a name="change-description"></a>變更描述

在 .NET Core 3.0-3.1 中， <xref:System.Text.Json.JsonSerializer> <xref:System.Text.Json.JsonException> 如果在 JSON 承載中遇到引號數位，則會在還原序列化期間擲回。 以引號括住的數位會用來對應物件圖形中的數位屬性。 在 .NET Core 3.0-3.1 中，只會從標記讀取數位 <xref:System.Text.Json.JsonTokenType.Number?displayProperty=nameWithType> 。

從 .NET 5.0 開始，JSON 承載中括住的數位預設會被視為適用于 ASP.NET Core apps。 在還原序列化引號中的數位時，不會擲回例外狀況。

> [!TIP]
>
> - 預設的獨立或沒有任何行為變更 <xref:System.Text.Json.JsonSerializer> <xref:System.Text.Json.JsonSerializerOptions> 。
> - 這在技術上並不是一項重大變更，因為它會讓案例更寬鬆，而不是更嚴格的 (也就是說，它會成功從 JSON 字串強制執行一個數位，而不是擲回例外狀況) 。 不過，由於這是影響許多 ASP.NET Core 應用程式的重大行為變更，因此會記載于此處。
> - <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A?displayProperty=nameWithType>和 <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A?displayProperty=nameWithType> 擴充方法也會使用 <xref:System.Text.Json.JsonSerializerDefaults.Web> 一組序列化選項。

#### <a name="version-introduced"></a>引進的版本

5.0

#### <a name="reason-for-change"></a>變更的原因

多個使用者已在中要求更寬鬆的數位處理選項 <xref:System.Text.Json.JsonSerializer> 。 這種意見反應表示許多 JSON 生產者 (例如，跨 web) 的服務會發出加引號的數位。 藉由允許將引號的數位讀取 (還原序列化的) ，.NET 應用程式在 web 內容中預設可以成功剖析這些承載。 設定會透過公開， <xref:System.Text.Json.JsonSerializerDefaults.Web?displayProperty=nameWithType> 讓您可以在不同的應用層（例如用戶端、伺服器和共用）之間指定相同的選項。

#### <a name="recommended-action"></a>建議的動作

如果這項變更是干擾性的，例如，如果您相依于驗證的嚴格數位處理，您可以重新啟用先前的行為。 將 <xref:System.Text.Json.JsonSerializerOptions.NumberHandling?displayProperty=nameWithType> 選項設定為 <xref:System.Text.Json.Serialization.JsonNumberHandling.Strict?displayProperty=nameWithType> 。

針對 ASP.NET Core MVC 和 web API 應用程式，您可以 `Startup` 使用下列程式碼來設定中的選項：

```csharp
services.AddControllers()
   .AddJsonOptions(options.NumberHandling = JsonNumberHandling.Strict);
```

#### <a name="category"></a>類別

- ASP.NET Core
- 序列化

#### <a name="affected-apis"></a>受影響的 API

- <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A?displayProperty=fullName>

<!--

#### Affected APIs

- `Overload:System.Text.Json.JsonSerializer.Deserialize`
- `Overload:System.Text.Json.JsonSerializer.DeserializeAsync`

-->
