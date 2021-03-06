---
title: 安全工作階段
ms.date: 03/30/2017
ms.assetid: 7b50602f-d7b5-42e9-8e92-1f0413df0d8b
ms.openlocfilehash: cb02adc7514e27175088e7664b12e45bc8ca5cd9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590081"
---
# <a name="secure-sessions"></a>安全工作階段
Windows Communication Foundation （WCF）的一項功能是可靠會話，可確保以傳送的順序接收訊息。 本節中的主題將就建立可靠工作階段時要考量的安全性影響提供說明。 如需可靠會話的詳細資訊，請參閱[使用會話](../using-sessions.md)。  
  
> [!NOTE]
> 當 Windows XP 需要模擬時，請使用不包含具狀態之安全性內容權杖 (SCT) 的安全工作階段。 當可設定狀態的 SCT 與模擬一起使用時，就會擲回 <xref:System.InvalidOperationException>。 如需詳細資訊，請參閱[不支援的案例](unsupported-scenarios.md)。  
  
## <a name="in-this-section"></a>本節內容  
  
|||  
|-|-|  
|[安全對話與安全工作階段](secure-conversations-and-secure-sessions.md)|安全對話與安全工作階段都是一樣的意思。 本主題將說明安全對話的運作方式，以及此模式的使用時機與原因。|  
|[如何：建立安全工作階段](how-to-create-a-secure-session.md)|逐步解說建立安全工作階段的基本概念。|  
|[HOW TO：為安全工作階段建立安全性內容權杖](how-to-create-a-security-context-token-for-a-secure-session.md)|逐步解說將維護用戶端狀態及工作階段之 Web 伺服陣列的各項建立步驟。|  
|[安全工作階段的安全性考量](security-considerations-for-secure-sessions.md)|描述安全工作階段的特殊考量。|  
  
## <a name="reference"></a>參考  
 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
## <a name="related-sections"></a>相關章節  
 [工作階段、執行個體與並行](sessions-instancing-and-concurrency.md)  
  
 [設計與實作服務](../designing-and-implementing-services.md)  
  
## <a name="see-also"></a>請參閱

- [HOW TO：啟用訊息重新執行偵測](how-to-enable-message-replay-detection.md)
- [重新執行攻擊](replay-attacks.md)
- [HOW TO：建立需要工作階段的服務](how-to-create-a-service-that-requires-sessions.md)
