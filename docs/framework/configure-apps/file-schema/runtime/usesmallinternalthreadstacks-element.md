---
title: Элемент <UseSmallInternalThreadStacks>
ms.date: 03/30/2017
helpviewer_keywords:
- UseSmallInternalThreadStacks element
- <UseSmallInternalThreadStacks> element
ms.assetid: 1e3f6ec0-1cac-4e1c-9c81-17d948ae5874
ms.openlocfilehash: 2fd776ce8605e6dcf288dcb3852ded16638a1873
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73114925"
---
# <a name="usesmallinternalthreadstacks-element"></a>\<Усесмаллинтерналсреадстаккс > элемент
Запрашивает уменьшение использования памяти средой CLR, указывая явные размеры стека при создании определенных потоков, используемых внутренним образом, вместо использования размера стека по умолчанию для этих потоков.  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<runtime >** ](runtime-element.md) \
&nbsp;&nbsp;&nbsp;&nbsp; **\<усесмаллинтерналсреадстаккс >**  
  
## <a name="syntax"></a>Синтаксис  
  
```xml  
<UseSmallInternalThreadStacks enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>Атрибуты и элементы  
 В следующих разделах описаны атрибуты, дочерние и родительские элементы.  
  
### <a name="attributes"></a>Атрибуты  
  
|Атрибут|Описание|  
|---------------|-----------------|  
|enabled|Обязательный атрибут.<br /><br /> Указывает, следует ли запрашивать, что среда CLR использует явные размеры стека вместо размера стека по умолчанию при создании определенных потоков, используемых внутренним образом. Явные размеры стека меньше, чем размер стека по умолчанию (1 МБ).|  
  
## <a name="enabled-attribute"></a>Атрибут enabled  
  
|значения|Описание|  
|-----------|-----------------|  
|true|Запрос явных размеров стека.|  
|Ложь|Используйте размер стека по умолчанию. Это значение по умолчанию для .NET Framework 4.|  
  
### <a name="child-elements"></a>Дочерние элементы  
 Отсутствует.  
  
### <a name="parent-elements"></a>Родительские элементы  
  
|Элемент|Описание|  
|-------------|-----------------|  
|`configuration`|Корневой элемент в любом файле конфигурации, используемом средой CLR и приложениями .NET Framework.|  
|`runtime`|Содержит сведения о привязке сборок и сборке мусора.|  
  
## <a name="remarks"></a>Заметки  
 Этот элемент конфигурации используется для запроса уменьшенного использования виртуальной памяти в процессе, поскольку явные размеры потоков, используемые средой CLR для внутренних потоков, если запрос обрабатывается, меньше размера по умолчанию.  
  
> [!IMPORTANT]
> Этот элемент конфигурации является запросом к CLR, а не абсолютному требованию. В .NET Framework 4 запрос учитывается только для архитектуры x86. Этот элемент может игнорироваться полностью в будущих версиях среды CLR или заменен на явные размеры стека, которые всегда используются для выбранных внутренних потоков.  
  
 Указание этого элемента конфигурации меняет надежность для использования меньшего объема виртуальной памяти, если среда CLR учитывает запрос, так как меньшие размеры стека потенциально могут привести к большей вероятности переполняет стек.  
  
## <a name="example"></a>Пример  
 В следующем примере показано, как запросить, чтобы среда CLR использовала явные размеры стека для определенных потоков, которые он использует для внутренних целей.  
  
```xml  
<configuration>  
   <runtime>  
      <UseSmallInternalThreadStacks enabled="true" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>См. также

- [Схема параметров среды выполнения](index.md)
- [Схема файла конфигурации](../index.md)
