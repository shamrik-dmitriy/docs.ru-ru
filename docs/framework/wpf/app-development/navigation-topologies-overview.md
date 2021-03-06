---
title: Общие сведения о топологии переходов
ms.date: 03/30/2017
helpviewer_keywords:
- linear topology [WPF]
- fixed hierarchical topology [WPF]
- fixed linear topology [WPF]
- topologies [WPF]
- navigation topologies [WPF]
- dynamically-generated topology
ms.assetid: 5d5ee837-629a-4933-869a-186dc22ac43d
ms.openlocfilehash: 08f6342095706e5ffe9479f5236457d21474152a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2020
ms.locfileid: "79174203"
---
# <a name="navigation-topologies-overview"></a>Общие сведения о топологии переходов
<a name="introduction"></a>Этот обзор содержит введение в навигационные топологии в WPF. Последовательно рассматриваются три общие топологии навигации с примерами.  
  
> [!NOTE]
> Прежде чем читать эту тему, вы должны быть знакомы с концепцией структурированной навигации в WPF с использованием функций страницы. Для получения дополнительной информации по обеим этим темам [см.](structured-navigation-overview.md)  
  
 Этот раздел состоит из следующих подразделов.  
  
- [Топологии навигации](#Navigation_Topologies)  
  
- [Топологии структурной навигации](#Structured_Navigation_Topologies)  
  
- [Навигация при фиксированной линейной топологии](#Navigation_over_a_Fixed_Linear_Topology)  
  
- [Динамическая навигация при фиксированной иерархической топологии](#Dynamic_Navigation_over_a_Fixed_Hierarchical_Topology)  
  
- [Навигация при динамически создаваемой топологии](#Navigation_over_a_Dynamically_Generated_Topology)  
  
<a name="Navigation_Topologies"></a>
## <a name="navigation-topologies"></a>Топологии навигации  
 В WPF навигация обычно<xref:System.Windows.Controls.Page>состоит из страниц<xref:System.Windows.Documents.Hyperlink>() с гиперссылками ( ), которые перемещаются на другие страницы при нажатии. Страницы, на которые можно ориентироваться, идентифицируются по единым идентификаторам ресурсов (URИ) (см. [Pack URIs в WPF).](pack-uris-in-wpf.md) Рассмотрим следующий простой пример, который показывает страницы, гиперссылки и единые идентификаторы ресурсов (URIs):  
  
 [!code-xaml[NavigationTopologiesOverviewSnippets#Page1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationTopologiesOverviewSnippets/CS/Page1.xaml#page1)]  
  
 [!code-xaml[NavigationTopologiesOverviewSnippets#Page2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationTopologiesOverviewSnippets/CS/Page2.xaml#page2)]  
  
 Эти страницы расположены в *топологии навигации,* структура которой определяется тем, как можно перемещаться между страницами. Эта конкретная топология навигации подходит для простых сценариев, хотя навигация может требовать более сложные топологии, некоторые из которых могут быть определены только при запуске приложения.  
  
 Эта тема охватывает три общие навигационные топологии: *фиксированную линейную,* *фиксированную иерархическую*и *динамически генерируемую.* Каждая топология навигации демонстрируется [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] с помощью образца, который похож на тот, который показан на следующей цифре:  
  
 ![Страницы задач с элементами данных и кнопками навигации.](./media/navigation-topologies-overview/navigation-topology-data-items.png)  
  
<a name="Structured_Navigation_Topologies"></a>
## <a name="structured-navigation-topologies"></a>Топологии структурной навигации  
 Существует два широко известных типа топологии навигации.  
  
- **Фиксированная топология**: определяется во время компиляции и не изменяется во время выполнения. Фиксированные топологии полезны для перехода по фиксированной последовательности страниц в линейном или иерархическом порядке.  
  
- **Динамическая топология**: определяется во время выполнения на основе входных данных, собираемых от пользователей, приложений или системы. Динамические топологии полезны в тех случаях, когда на страницы можно переходить в разных последовательностях.  
  
 Хотя возможно создание топологии навигации с помощью страниц, в примерах используются страничные функции, поскольку они предоставляют дополнительные возможности, которые упрощают поддержку передачи и возврата данных по страницам топологии.  
  
<a name="Navigation_over_a_Fixed_Linear_Topology"></a>
## <a name="navigation-over-a-fixed-linear-topology"></a>Навигация при фиксированной линейной топологии  
 Фиксированная линейная топология является аналогом структуры мастера с одной или несколькими страницами, по которым можно переходить в фиксированной последовательности. Следующая цифра показывает структуру высокого уровня и поток мастера с фиксированной линейной топологией:  
  
 ![Диаграмма, понагреваемый фиксированной линейной топологией.](./media/navigation-topologies-overview/navigation-topology-fixed-linear.png)  
  
 Типичные варианты поведения для навигации по фиксированной линейной топологии могут быть следующие.  
  
- Переход из вызывающей страницы на страницу средства запуска, которое инициализирует мастер и переходит к первой странице мастера. Страница пусковой [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]установки <xref:System.Windows.Navigation.PageFunction%601>(a-less) не требуется, так как страница вызова может вызвать первую страницу мастера напрямую. Однако используя страницу средства запуска, можно упростить инициализацию мастера, особенно если инициализация сложна.  
  
- Пользователи могут переходить между страницами с помощью кнопок "Назад" и "Вперед" (или гиперссылок).  
  
- Пользователи могут переходить по страницам с помощью журнала.  
  
- Пользователи могут отменить работу мастера на любой странице, нажав кнопку "Отмена".  
  
- Пользователи могут принять работу мастера на последней странице, нажав кнопку "Готово".  
  
- Если мастер отменяется, то он возвращает соответствующий результат и не возвращает никаких данных.  
  
- Если пользователь принимает работу мастера, мастер возвращает соответствующий результат и возвращает собранные им данные.  
  
- По завершении мастера (принятия или отмены) страницы, которые составляли мастер, будут удалены из журнала. Это сохраняет каждый экземпляр мастера изолированным, тем самым позволяя избежать потенциальных ошибок данных или состояния.  
  
<a name="Dynamic_Navigation_over_a_Fixed_Hierarchical_Topology"></a>
## <a name="dynamic-navigation-over-a-fixed-hierarchical-topology"></a>Динамическая навигация при фиксированной иерархической топологии  
 В некоторых приложениях страницы позволяют навигацию на двух или более других страницах, как показано на следующем рисунке:
  
 ![Диаграмма, отображая страницу, которая может перемещаться на несколько страниц.](./media/navigation-topologies-overview/navigation-topology-multiple-pages.png)  
  
 Эта структура называется фиксированной иерархической топологией, и последовательность обхода иерархии часто определяется во время выполнения приложением или пользователем. Во время выполнения каждая страница в иерархии, позволяющая выполнять переходы на несколько страниц, собирает данные, необходимые для определения страниц для перехода. Следующая цифра иллюстрирует одну из нескольких возможных последовательностей навигации на основе предыдущей цифры:  
  
 ![Диаграмма, отображая возможную последовательность навигации.](./media/navigation-topologies-overview/navigation-topology-fixed-hierarchical.png)  
  
 Несмотря на то, что последовательность переходов по страницам в фиксированной иерархической структуре определяется во время выполнения, взаимодействие с пользователем такое же, как в фиксированной линейной топологии.  
  
- Переход из вызывающей страницы на страницу средства запуска, которое инициализирует мастер и переходит к первой странице мастера. Страница пусковой [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]установки <xref:System.Windows.Navigation.PageFunction%601>(a-less) не требуется, так как страница вызова может вызвать первую страницу мастера напрямую. Однако используя страницу средства запуска, можно упростить инициализацию мастера, особенно если инициализация сложна.  
  
- Пользователи могут переходить между страницами с помощью кнопок "Назад" и "Вперед" (или гиперссылок).  
  
- Пользователи могут переходить по страницам с помощью журнала.  
  
- Пользователи могут изменять последовательность навигации, если они перемещаются назад по журналу.  
  
- Пользователи могут отменить работу мастера на любой странице, нажав кнопку "Отмена".  
  
- Пользователи могут принять работу мастера на последней странице, нажав кнопку "Готово".  
  
- Если мастер отменяется, то он возвращает соответствующий результат и не возвращает никаких данных.  
  
- Если пользователь принимает работу мастера, мастер возвращает соответствующий результат и возвращает собранные им данные.  
  
- По завершении мастера (принятия или отмены) страницы, которые составляли мастер, будут удалены из журнала. Это сохраняет каждый экземпляр мастера изолированным, тем самым позволяя избежать потенциальных ошибок данных или состояния.  
  
<a name="Navigation_over_a_Dynamically_Generated_Topology"></a>
## <a name="navigation-over-a-dynamically-generated-topology"></a>Навигация при динамически создаваемой топологии  
 В некоторых приложениях последовательность, в которой осуществляется переход на две или более страниц, может быть определена только во время выполнения пользователем, приложением или внешними данными. Следующая цифра иллюстрирует набор страниц с неопределенной последовательностью навигации:  
  
 ![Набор страниц с неопределенной последовательностью навигации.](./media/navigation-topologies-overview/navigation-topology-dynamically-generated.png)  
  
 Следующая цифра иллюстрирует последовательность навигации, выбранную пользователем во время выполнения:  
  
 ![Диаграмма, отображая последовательность навигации, выбранную во время выполнения.](./media/navigation-topologies-overview/navigation-topology-sequence-chosen-run-time.png)  
  
 Эта последовательность навигации называется динамически создаваемой топологией. Взаимодействие с пользователем такое же, как и в предыдущих топологиях навигации.  
  
- Переход из вызывающей страницы на страницу средства запуска, которое инициализирует мастер и переходит к первой странице мастера. Страница пусковой [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]установки <xref:System.Windows.Navigation.PageFunction%601>(a-less) не требуется, так как страница вызова может вызвать первую страницу мастера напрямую. Однако используя страницу средства запуска, можно упростить инициализацию мастера, особенно если инициализация сложна.  
  
- Пользователи могут переходить между страницами с помощью кнопок "Назад" и "Вперед" (или гиперссылок).  
  
- Пользователи могут переходить по страницам с помощью журнала.  
  
- Пользователи могут отменить работу мастера на любой странице, нажав кнопку "Отмена".  
  
- Пользователи могут принять работу мастера на последней странице, нажав кнопку "Готово".  
  
- Если мастер отменяется, то он возвращает соответствующий результат и не возвращает никаких данных.  
  
- Если пользователь принимает работу мастера, мастер возвращает соответствующий результат и возвращает собранные им данные.  
  
- По завершении мастера (принятия или отмены) страницы, которые составляли мастер, будут удалены из журнала. Это сохраняет каждый экземпляр мастера изолированным, тем самым позволяя избежать потенциальных ошибок данных или состояния.  
  
## <a name="see-also"></a>См. также раздел

- <xref:System.Windows.Controls.Page>
- <xref:System.Windows.Navigation.PageFunction%601>
- <xref:System.Windows.Navigation.NavigationService>
- [Общие сведения о структурной навигации](structured-navigation-overview.md)
