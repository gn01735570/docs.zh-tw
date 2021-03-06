---
title: ICLRStrongName::StrongNameKeyGenEx 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameKeyGenEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameKeyGenEx
helpviewer_keywords:
- ICLRStrongName::StrongNameKeyGenEx method [.NET Framework hosting]
- StrongNameKeyGenEx method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: 1f8b59d0-5b72-45b8-ab74-c2b43ffc806e
topic_type:
- apiref
ms.openlocfilehash: fb18b7b5ac73a1f270af6fae95a23e04b17ca5f1
ms.sourcegitcommit: c76c8b2c39ed2f0eee422b61a2ab4c05ca7771fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83763068"
---
# <a name="iclrstrongnamestrongnamekeygenex-method"></a>ICLRStrongName::StrongNameKeyGenEx 方法
產生具有指定金鑰大小的新公開/私密金鑰組，以供強式名稱使用。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT StrongNameKeyGenEx (  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  DWORD     dwFlags,  
    [in]  DWORD     dwKeySize,  
    [out] BYTE      **ppbKeyBlob,  
    [out] ULONG     *pcbKeyBlob  
);  
```  
  
## <a name="parameters"></a>參數  
 `wszKeyContainer`  
 在要求的金鑰容器名稱。 `wszKeyContainer`必須是非空白字串或 null，才能產生暫存名稱。  
  
 `dwFlags`  
 在值，指定是否要保留註冊的金鑰。 支援下列值：  
  
- 0x00000000-在為 null 時使用， `wszKeyContainer` 以產生暫存金鑰容器名稱。  
  
- 0x00000001 （ `SN_LEAVE_KEY` ）-指定應將金鑰保留為已註冊。  
  
 `dwKeySize`  
 在要求的金鑰大小（以位為單位）。  
  
 `ppbKeyBlob`  
 脫銷傳回的公開/私密金鑰組。  
  
 `pcbKeyBlob`  
 脫銷的大小（以位元組為單位） `ppbKeyBlob` 。  
  
## <a name="return-value"></a>傳回值  
 `S_OK`如果方法已成功完成，則為，否則，就是表示失敗的 HRESULT 值（請參閱清單的[一般 HRESULT 值](/windows/win32/seccrypto/common-hresult-values)）。  
  
## <a name="remarks"></a>備註  
 .NET Framework 版本1.0 和1.1 需要 `dwKeySize` 1024 位以強式名稱簽署元件; 版本2.0 新增了支援的2048位金鑰。  
  
 在取得金鑰之後，您應該呼叫[ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md)方法來釋放已配置的記憶體。  
  
## <a name="requirements"></a>規格需求  
 **平台：** 請參閱[系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** MetaHost。h  
  
 連結**庫：** 包含為 Mscoree.dll 中的資源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [StrongNameKeyGen 方法](iclrstrongname-strongnamekeygen-method.md)
- [ICLRStrongName 介面](iclrstrongname-interface.md)
