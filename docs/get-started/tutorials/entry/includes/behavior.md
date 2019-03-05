# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Entry`](xref:Xamarin.Forms.Entry), чтобы настроить его поведение:

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    Этот код задает свойства, позволяющие настроить поведение [`Entry`](xref:Xamarin.Forms.Entry). Свойство [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) ограничивает длину входных данных, допустимую для `Entry`, а для свойства [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) установлено значение `false`, чтобы отключить проверку орфографии. Аналогичным образом для свойства [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) установлено значение `false`, чтобы отключить прогнозирование текста и автоматическое прогнозирование текста. Кроме того, свойство [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) гарантирует, что введенные символы скрыты символом пароля (черным кружком).

    > [!NOTE]
    > Для некоторых сценариев ввода текста, например ввода пароля, проверка орфографии и прогнозирование текста только мешают, и их следует отключить.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Введите текст в [`Entry`](xref:Xamarin.Forms.Entry) и обратите внимание, что каждый символ заменяется символом маски пароля и можно ввести не более 15 символов:

    [![Снимок экрана: запись с текстом, замаскированным символами пароля, в iOS и Android](../images/customize-behavior.png "Запись с замаскированными символами пароля")](../images/customize-behavior-large.png#lightbox "Запись с замаскированными символами пароля")

    Дополнительные сведения о настройке поведения [`Entry`](xref:Xamarin.Forms.Entry) см. в руководстве [Запись Xamarin.Forms](~/xamarin-forms/user-interface/text/entry.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Entry`](xref:Xamarin.Forms.Entry), чтобы настроить его поведение:

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    Этот код задает свойства, позволяющие настроить поведение [`Entry`](xref:Xamarin.Forms.Entry). Свойство [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) ограничивает длину входных данных, допустимую для `Entry`, а для свойства [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) установлено значение `false`, чтобы отключить проверку орфографии. Аналогичным образом для свойства [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) установлено значение `false`, чтобы отключить прогнозирование текста и автоматическое прогнозирование текста. Кроме того, свойство [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) гарантирует, что введенные символы скрыты символом пароля (черным кружком).

    > [!NOTE]
    > Для некоторых сценариев ввода текста, например ввода пароля, проверка орфографии и прогнозирование текста только мешают, и их следует отключить.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Введите текст в [`Entry`](xref:Xamarin.Forms.Entry) и обратите внимание, что каждый символ заменяется символом маски пароля и можно ввести не более 15 символов:

    [![Снимок экрана: запись с текстом, замаскированным символами пароля, в iOS и Android](../images/customize-behavior.png "Запись с замаскированными символами пароля")](../images/customize-behavior-large.png#lightbox "Запись с замаскированными символами пароля")

    Дополнительные сведения о настройке поведения [`Entry`](xref:Xamarin.Forms.Entry) см. в руководстве [Запись Xamarin.Forms](~/xamarin-forms/user-interface/text/entry.md).
