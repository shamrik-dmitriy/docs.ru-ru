---
title: Метод ISymENCUnmanagedMethod::GetSourceExtentInDocument
ms.date: 03/30/2017
api_name:
- ISymENCUnmanagedMethod.GetSourceExtentInDocument
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymENCUnmanagedMethod::GetSourceExtentInDocument
helpviewer_keywords:
- GetSourceExtentInDocument method [.NET Framework debugging]
- ISymENCUnmanagedMethod::GetSourceExtentInDocument method [.NET Framework debugging]
ms.assetid: 9c5566ab-4ec7-4b61-9753-839bb90ae78c
topic_type:
- apiref
ms.openlocfilehash: b8e72745eff09c6707afe5a5f20a1ddf38b239ae
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448616"
---
# <a name="isymencunmanagedmethodgetsourceextentindocument-method"></a>Метод ISymENCUnmanagedMethod::GetSourceExtentInDocument
Возвращает наименьшую начальную строку и самую новую конечную строку для метода в определенном документе.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetSourceExtentInDocument(  
    [in]  ISymUnmanagedDocument *document,  
    [out] ULONG32* pstartLine,  
    [out] ULONG32* pendLine);  
```  
  
## <a name="parameters"></a>Параметры  
 `document`  
 окне Указатель на документ.  
  
 `pstartLine`  
 заполняет Указатель на `ULONG32`, который получает начальную строку.  
  
 `pendLine`  
 заполняет Указатель на `ULONG32`, который получает конечную строку.  
  
## <a name="return-value"></a>Возвращаемое значение  
 S_OK, если метод выполнен. в противном случае E_FAIL или другой код ошибки.  
  
## <a name="requirements"></a>Требования  
 **Заголовок:** Корсим. idl, Корсим. h  
  
## <a name="see-also"></a>См. также:

- [Интерфейс ISymENCUnmanagedMethod](../../../../docs/framework/unmanaged-api/diagnostics/isymencunmanagedmethod-interface.md)
