---
title: Привязка элементов управления к значениям базы данных DBNull
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BindingSource component [Windows Forms], binding to DBNull values
- examples [Windows Forms], BindingSource component
- controls [Windows Forms], binding to DBNull values
ms.assetid: 96494e6f-5f40-4f83-af97-bbd7192c2af8
ms.openlocfilehash: 175d7f5aee2540916480e2c55a485af1f9d16653
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746665"
---
# <a name="how-to-bind-windows-forms-controls-to-dbnull-database-values"></a>Практическое руководство. Специальная обработка значений DBNull при связывании элементов управления с данными в Windows Forms
Если элементы управления Windows Forms привязаны к источнику данных и источник данных возвращает значение <xref:System.DBNull>, соответствующее значение можно заменить без обработки, форматирования или анализа событий. Свойство <xref:System.Windows.Forms.Binding.NullValue%2A> преобразует <xref:System.DBNull> в указанный объект при форматировании или анализе значений источника данных.  
  
## <a name="example"></a>Пример  
 В примере ниже демонстрируется привязка значения <xref:System.DBNull> в двух разных случаях. В первом случае показано, как задать <xref:System.Windows.Forms.Binding.NullValue%2A> для свойства строки; во втором — как задать <xref:System.Windows.Forms.Binding.NullValue%2A> для свойства изображения.  
  
 [!code-csharp[System.Windows.Forms.BindingDBNull#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BindingDBNull/CS/form1.cs#1)]
 [!code-vb[System.Windows.Forms.BindingDBNull#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BindingDBNull/VB/form1.vb#1)]  
  
 Типы связанного свойства и свойства <xref:System.Windows.Forms.Binding.NullValue%2A> должны быть одинаковыми, иначе произойдет ошибка и следующие значения <xref:System.Windows.Forms.Binding.NullValue%2A> обрабатываться не будут. В этом случае исключение не создается.  
  
## <a name="compiling-the-code"></a>Компиляция кода  
 Для этого примера требуются:  
  
- ссылки на сборки System, System.Data, System.Drawing и System.Windows.Forms.  
  
## <a name="see-also"></a>См. также раздел

- [Компонент BindingSource](bindingsource-component.md)
- [Практическое руководство. Обработка ошибок и исключений, происходящих при связывании элементов управления с данными](how-to-handle-errors-and-exceptions-that-occur-with-databinding.md)
- [Практическое руководство. Связывание элемента управления с типом в Windows Forms](how-to-bind-a-windows-forms-control-to-a-type.md)
