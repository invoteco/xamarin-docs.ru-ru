---
ms.openlocfilehash: bd3f37082443e93f10f60d9466fe22aae8571b14
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "61373442"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Editor`](xref:Xamarin.Forms.Editor), чтобы настроить его поведение:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    Этот код задает свойства, позволяющие настроить поведение [`Editor`](xref:Xamarin.Forms.Editor). Свойство [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) имеет значение [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges); это означает, что высота `Editor` увеличивается, когда оно заполняется текстом, и уменьшается, когда текст удаляется. Свойство [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) ограничивает длину входных данных, допустимую для `Editor`. Кроме того, свойство [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) задано как `false`, чтобы отключить проверку орфографии, а свойству `IsTextPredictionEnabled` задано значение `false` для отключения прогнозирования текста и автоматического прогнозирования текста.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Введите текст в [`Editor`](xref:Xamarin.Forms.Entry) и обратите внимание, что высота `Editor` увеличивается по мере его заполнения текстом, а также уменьшается по мере удаления текста; максимальное число вводимых символов — 200:

    [![Снимок экрана редактора с автоматическим изменением размера в iOS и Android](../images/customize-behavior.png "Редактор с автоматическим изменением размера")](../images/customize-behavior-large.png#lightbox "Редактор с автоматическим изменением размера")

    Дополнительные сведения о настройке поведения [`Editor`](xref:Xamarin.Forms.Editor) см. в руководстве [Редактор Xamarin.Forms](~/xamarin-forms/user-interface/text/editor.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Editor`](xref:Xamarin.Forms.Editor), чтобы настроить его поведение:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    Этот код задает свойства, позволяющие настроить поведение [`Editor`](xref:Xamarin.Forms.Editor). Свойство [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) имеет значение [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges); это означает, что высота `Editor` увеличивается, когда оно заполняется текстом, и уменьшается, когда текст удаляется. Свойство [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) ограничивает длину входных данных, допустимую для `Editor`. Кроме того, свойство [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) задано как `false`, чтобы отключить проверку орфографии, а свойству `IsTextPredictionEnabled` задано значение `false` для отключения прогнозирования текста и автоматического прогнозирования текста.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Введите текст в [`Editor`](xref:Xamarin.Forms.Entry) и обратите внимание, что высота `Editor` увеличивается по мере его заполнения текстом, а также уменьшается по мере удаления текста; максимальное число вводимых символов — 200:

    [![Снимок экрана редактора с автоматическим изменением размера в iOS и Android](../images/customize-behavior.png "Редактор с автоматическим изменением размера")](../images/customize-behavior-large.png#lightbox "Редактор с автоматическим изменением размера")

    Дополнительные сведения о настройке поведения [`Editor`](xref:Xamarin.Forms.Editor) см. в руководстве [Редактор Xamarin.Forms](~/xamarin-forms/user-interface/text/editor.md).
