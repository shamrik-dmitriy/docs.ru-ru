---
title: Размещение содержимого Direct3D9 в WPF
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- Direct3D9 [WPF interoperability], hosting Direct3D9 content
- WPF [WPF], hosting Direct3D9 content
ms.assetid: 60983736-0ab5-42cc-8b16-e9fbde261a43
ms.openlocfilehash: e65b0c59268b44abed289e54181bf0bda9355664
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742605"
---
# <a name="walkthrough-hosting-direct3d9-content-in-wpf"></a>Пошаговое руководство. Размещение содержимого Direct3D9 в WPF

В этом пошаговом руководстве показано, как разместить содержимое Direct3D9 в приложении Windows Presentation Foundation (WPF).

В этом пошаговом руководстве будут выполнены следующие задачи:

- Создайте проект WPF для размещения содержимого Direct3D9.

- Импортируйте содержимое Direct3D9.

- Отображение содержимого Direct3D9 с помощью класса <xref:System.Windows.Interop.D3DImage>.

 По завершении вы узнаете, как разместить содержимое Direct3D9 в приложении WPF.

## <a name="prerequisites"></a>предварительные требования

Для выполнения этого пошагового руководства требуются следующие компоненты:

- приведенному.

- Пакет SDK для DirectX 9 или более поздней версии.

- Библиотека DLL, содержащая содержимое Direct3D9 в формате, совместимом с WPF. Дополнительные сведения см. в статьях [взаимодействие WPF и Direct3D9](wpf-and-direct3d9-interoperation.md) и [Пошаговое руководство. Создание содержимого Direct3D9 для размещения в WPF](walkthrough-creating-direct3d9-content-for-hosting-in-wpf.md).

## <a name="creating-the-wpf-project"></a>Создание проекта WPF

Первым шагом является создание проекта для приложения WPF.

### <a name="to-create-the-wpf-project"></a>Создание проекта WPF

Создайте новый проект приложения WPF в Visual C# с именем `D3DHost`. Дополнительные сведения см. в разделе [Пошаговое руководство. Создание первого классического приложения WPF](../getting-started/walkthrough-my-first-wpf-desktop-application.md).

В конструкторе WPF откроется файл MainWindow. XAML.

## <a name="importing-the-direct3d9-content"></a>Импорт содержимого Direct3D9

Содержимое Direct3D9 импортируется из неуправляемой библиотеки DLL с помощью атрибута `DllImport`.

### <a name="to-import-direct3d9-content"></a>Импорт содержимого Direct3D9

1. Откройте MainWindow.xaml.cs в редакторе кода.

2. Замените автоматически созданный код следующим кодом.

    [!code-csharp[System.Windows.Interop.D3DImage#1](~/samples/snippets/csharp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/CS/window1.xaml.cs#1)]

## <a name="hosting-the-direct3d9-content"></a>Размещение содержимого Direct3D9

Наконец, используйте класс <xref:System.Windows.Interop.D3DImage> для размещения содержимого Direct3D9.

### <a name="to-host-the-direct3d9-content"></a>Размещение содержимого Direct3D9

1. В файле MainWindow. XAML замените автоматически созданный код XAML следующим кодом XAML.

    [!code-xaml[System.Windows.Interop.D3DImage#10](~/samples/snippets/csharp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/CS/window1.xaml#10)]

2. Создайте проект.

3. Скопируйте библиотеку DLL, содержащую содержимое Direct3D9, в папку bin/Debug.

4. Нажмите клавишу F5, чтобы запустить проект.

    Содержимое Direct3D9 отображается в приложении WPF.

## <a name="see-also"></a>См. также раздел

- <xref:System.Windows.Interop.D3DImage>
- [Вопросы производительности, связанные с взаимодействием Direct3D9 и WPF](performance-considerations-for-direct3d9-and-wpf-interoperability.md)
