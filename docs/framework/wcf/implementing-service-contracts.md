---
title: 實作服務合約
ms.date: 03/30/2017
helpviewer_keywords:
- implementing service contracts [WCF]
ms.assetid: aefb6f56-47e3-4f24-ab0a-9bc07bf9885f
ms.openlocfilehash: aefe146a8941d98d7d9138e4ece83c330c967034
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79183995"
---
# <a name="implementing-service-contracts"></a>實作服務合約
服務是一種類別，會在一或多個端點公開可供用戶端使用的功能。 要創建服務，請編寫實現 Windows 通信基礎 （WCF） 協定的類。 您可以使用下列其中一種作法： 您可以將合約另外定義為介面，然後建立實作該介面的類別。 或者，您可以將 <xref:System.ServiceModel.ServiceContractAttribute> 屬性置於類別本身，而將 <xref:System.ServiceModel.OperationContractAttribute> 屬性置於可供服務之用戶端使用的方法上，以直接建立類別和合約。  
  
## <a name="creating-a-service-class"></a>建立服務類別  
 以下是實作已另外定義之 `IMath` 合約的服務範例。  
  
```csharp  
// Define the IMath contract.  
[ServiceContract]  
public interface IMath  
{  
    [OperationContract]
    double Add(double A, double B);  
  
    [OperationContract]  
    double Multiply (double A, double B);  
}  
  
// Implement the IMath contract in the MathService class.  
public class MathService : IMath  
{  
    public double Add (double A, double B) { return A + B; }  
    public double Multiply (double A, double B) { return A * B; }  
}  
```  
  
 或者，服務可以直接公開合約。 以下是定義及實作 `MathService` 合約的服務類別範例。  
  
```csharp  
// Define the MathService contract directly on the service class.  
[ServiceContract]  
class MathService  
{  
    [OperationContract]  
    public double Add(double A, double B) { return A + B; }  
    [OperationContract]  
    private double Multiply (double A, double B) { return A * B; }  
}  
```  
  
 請注意，前述服務會公開不同的合約，因為合約名稱不同。 在第一個案例中，公開的合約名為 "`IMath`"，而在第二個案例中，合約名為 "`MathService`"。  
  
 您可以在服務和作業實作層級進行一些設定，例如並行和執行個體。 有關詳細資訊，請參閱[設計和實現服務](designing-and-implementing-services.md)。  
  
 在實作服務合約之後，必須為服務建立一或多個端點。 有關詳細資訊，請參閱[終結點創建概述](endpoint-creation-overview.md)。 有關如何運行服務的詳細資訊，請參閱[託管服務](hosting-services.md)。  
  
## <a name="see-also"></a>另請參閱

- [設計與實作服務](designing-and-implementing-services.md)
- [HOW TO：使用合約類別來建立服務](./feature-details/how-to-create-a-wcf-contract-with-a-class.md)
- [HOW TO：使用合約介面來建立服務](./feature-details/how-to-create-a-service-with-a-contract-interface.md)
- [指定服務執行階段行為](specifying-service-run-time-behavior.md)
