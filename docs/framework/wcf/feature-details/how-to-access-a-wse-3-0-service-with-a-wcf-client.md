---
title: HOW TO：使用 WCF 用戶端來存取 WSE 3.0 服務
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1f9bcd9b-8f8f-47fa-8f1e-0d47236eb800
ms.openlocfilehash: 847146c2025612689f0d69cc0c23d2be14018c0f
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556833"
---
# <a name="how-to-access-a-wse-30-service-with-a-wcf-client"></a>HOW TO：使用 WCF 用戶端來存取 WSE 3.0 服務
當 WCF 用戶端設定為使用 WS-ADDRESSING 規格的2004版本時，Windows Communication Foundation (WCF) 用戶端與 Web 服務增強功能相容 (WSE) 3.0 for Microsoft .NET 服務。 但是，WSE 3.0 服務並不支援 (MEX) 通訊協定的中繼資料交換，因此當您使用 [配置 [中繼資料公用程式] 工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 建立 wcf 用戶端類別時，安全性設定不會套用至產生的 wcf 用戶端。 因此，在產生 WCF 用戶端之後，您必須指定 WSE 3.0 服務所需的安全性設定。  
  
 您可以使用自訂系結來套用這些安全性設定，以將 wse 3.0 服務的需求和 WSE 3.0 服務和 WCF 用戶端之間的互通需求納入考慮。 這些互通性需求包含上述的 WS-Addressing August 2004 規格使用和 WSE 3.0 預設訊息保護 <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt>。 WCF 的預設訊息保護是 <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature> 。 本主題詳細說明如何建立與 WSE 3.0 服務交互操作的 WCF 系結。 WCF 也提供併入這個系結的範例。 如需此範例的詳細資訊，請參閱 [與 .Asmx Web 服務互](../samples/interoperating-with-asmx-web-services.md)操作。  
  
### <a name="to-access-a-wse-30-web-service-with-a-wcf-client"></a>若要使用 WCF 用戶端來存取 WSE 3.0 Web 服務  
  
1. [ ( # A0) 中執行 [System.servicemodel 中繼資料公用程式] 工具](../servicemodel-metadata-utility-tool-svcutil-exe.md)，以建立 WSE 3.0 Web 服務的 WCF 用戶端。  
  
     針對 WSE 3.0 Web 服務，會建立 WCF 用戶端。 因為 WSE 3.0 不支援 MEX 通訊協定，所以您無法使用此工具擷取 Web 服務的安全性需求。 應用程式開發人員必須為用戶端加入安全性設定。  
  
     如需建立 WCF 用戶端的詳細資訊，請參閱 how [to：建立用戶端](../how-to-create-a-wcf-client.md)。  
  
2. 建立類別，表示可與 WSE 3.0 Web 服務通訊的繫結。  
  
     下列類別是 [與 WSE 範例互](/previous-versions/dotnet/netframework-3.5/ms752257(v=vs.90)) 操作的一部分：  
  
    1. 建立從 <xref:System.ServiceModel.Channels.Binding> 類別衍生的類別。  
  
         下列程式碼範例會建立一個名為 `WseHttpBinding` 的類別，此類別衍生自 <xref:System.ServiceModel.Channels.Binding> 類別。  
  
         [!code-csharp[c_WCFClientToWSEService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#1)]
         [!code-vb[c_WCFClientToWSEService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#1)]  
  
    2. 將屬性加入至類別，這些屬性會指定 WSE 服務所使用的 WSE 通行判斷提示 (Turnkey Assertion)、是否需要衍生金鑰、是否使用安全工作階段、是否需要簽章確認，以及訊息保護設定。 在 WSE 3.0 中，通行判斷提示會指定用戶端或 Web 服務的安全性需求，類似于 WCF 中系結的驗證模式。  
  
         下列程式碼範例會定義 `SecurityAssertion`、`RequireDerivedKeys`、`EstablishSecurityContext` 和 `MessageProtectionOrder` 屬性，這些屬性會分別指定 WSE 通行判斷提示、是否需要衍生金鑰、是否使用安全工作階段、是否需要簽章確認，以及訊息保護設定。  
  
         [!code-csharp[c_WCFClientToWSEService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#3)]
         [!code-vb[c_WCFClientToWSEService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#3)]  
  
    3. 覆寫 <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> 方法來設定繫結屬性。  
  
         下列程式碼範例會藉由取得 `SecurityAssertion` 和 `MessageProtectionOrder` 屬性的值，指定傳輸、訊息編碼和訊息保護設定。  
  
         [!code-csharp[c_WCFClientToWSEService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#2)]
         [!code-vb[c_WCFClientToWSEService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#2)]  
  
3. 在用戶端應用程式程式碼中，加入程式碼以設定繫結屬性。  
  
     下列程式碼範例指定 WCF 用戶端必須依照 WSE 3.0 通行安全性判斷提示所定義，使用訊息保護和驗證 `AnonymousForCertificate` 。 此外，也需要安全工作階段和衍生金鑰。  
  
     [!code-csharp[c_WCFClientToWSEService#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/client.cs#4)]
     [!code-vb[c_WCFClientToWSEService#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/client.vb#4)]  
  
## <a name="example"></a>範例  
 下列程式碼範例會定義自訂的繫結，此繫結會公開 WSE 3.0 通行安全性判斷提示屬性的對應屬性。 該自訂系結（名為 `WseHttpBinding` ）接著會用來指定 WCF 用戶端的系結屬性，該用戶端會與 WSSECURITYANONYMOUS WSE 3.0 快速入門範例進行通訊。  

## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.Channels.Binding>
- [與 WSE 交互操作](/previous-versions/dotnet/netframework-3.5/ms752257(v=vs.90))
