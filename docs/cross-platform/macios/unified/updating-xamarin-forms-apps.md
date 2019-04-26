---
title: Обновление имеющихся приложений Xamarin.Forms
description: В этом документе описаны шаги, которые должны выполняться для обновления приложения Xamarin.Forms из классический API на единый API.
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d5c16b034b07d3e9875412f041c16b293557438a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61211866"
---
# <a name="updating-existing-xamarinforms-apps"></a>Обновление имеющихся приложений Xamarin.Forms

_Выполните следующие действия для обновления существующего приложения Xamarin.Forms на единый API и обновить до версии 1.3.1_

> [!IMPORTANT]
> Поскольку Xamarin.Forms 1.3.1 первый выпуск, который поддерживает единый API, всего решения следует обновить для использования последней версии, в то же время, как миграция приложения iOS к единой системе. Это означает, что помимо обновления проект iOS для поддержки единой, также необходимо изменить код в _все_ проектов в решении.

Обновление выполняется в два этапа:

1. Перенос приложения iOS на единый API, с помощью Visual Studio для Mac сборки в средство миграции.

    - Используйте средство миграции, чтобы автоматически обновить проект.

    - Обновления iOS собственного API-интерфейсы, как описано в инструкциях для [обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md) (в частности в коде службы пользовательского модуля подготовки отчетов или зависимостей).

2. Обновите все решение Xamarin.Forms версии 1.3.

    1. Установите пакет NuGet Xamarin.Forms 1.3.1.

    2. Обновление `App` класс в общем коде.

    3. Обновление `AppDelegate` в проекте iOS.

    4. Обновление `MainActivity` в проекте Android.

    5. Обновление `MainPage` в проекте Windows Phone.

## <a name="1-ios-app-unified-migration"></a>1. операций ввода-вывода приложения (Unified миграции)

Часть процедуры миграции требуется выполнить обновление до версии 1.3, который поддерживает единый API Xamarin.Forms. Чтобы правильные ссылки на сборки должен быть создан необходимо сначала обновить проект iOS для использования на единый API.

### <a name="migration-tool"></a>Средство миграции

Щелкните кнопкой мыши проект iOS, чтобы он установлен, затем выберите **проекта > Переход на единый API Xamarin.iOS...**  и соглашаетесь на появится предупреждающее сообщение.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Выберите проект > мигрировать на единый API Xamarin.iOS... и принимайте предупреждающего сообщения")

Это будет автоматически:

 - Измените тип проекта для поддержки единой 64-разрядных API.
 - Изменить ссылку на framework **Xamarin.iOS** (заменив старый **monotouch** ссылку).
 - Изменить ссылки на пространства имен в коде, чтобы удалить `MonoTouch` префикс.
 - Обновление **csproj** файла целевых объектов правильную сборку для на единый API.

**Очистить** и **построения** проекта, чтобы убедиться в отсутствии ошибок, чтобы исправить. Никаких дополнительных действий не требуется. Эти действия описаны более подробно в [документы Unified API](~/cross-platform/macios/unified/updating-ios-apps.md).

### <a name="update-native-ios-apis-if-required"></a>Обновить собственного API для iOS (при необходимости)

При добавлении дополнительных операций ввода-вывода машинный код (например, пользовательские модули подготовки отчетов или служб зависимостей) может потребоваться выполнить дополнительные вручную исправления кода. Повторно скомпилируйте приложение и ссылаться на [iOS обновление существующих приложений инструкции](~/cross-platform/macios/unified/updating-ios-apps.md) Дополнительные сведения об изменениях, которые могут потребоваться. [Эти советы](~/cross-platform/macios/unified/updating-tips.md) также поможет выявить изменения, которые необходимы.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 обновления

После обновления приложения iOS на единый API, остальной части решения необходимо обновить до версии 1.3.1 Xamarin.Forms. В том числе следующее:

 - Обновление пакета Xamarin.Forms NuGet в каждом проекте.
 - Изменение кода для использования нового Xamarin.Forms `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android), и `FormsApplicationPage` классы (Windows Phone).

Эти действия описаны ниже:

### <a name="21-update-nuget-in-all-projects"></a>2.1. обновление NuGet во всех проектах

Обновите Xamarin.Forms до 1.3.1 предварительной версии с помощью диспетчера пакетов NuGet для всех проектов в решении: PCL (при его наличии), iOS, Android и Windows Phone. Рекомендуется, **удалите и повторно добавьте** пакет Xamarin.Forms NuGet для обновления до версии 1.3.

**Примечание.** Xamarin.Forms версии 1.3.1 в данный момент находится в *предварительной версии*. Это означает, что необходимо выбрать **предварительной версии** равным в NuGet с помощью (a-поле делений в Visual Studio для Mac) или раскрывающегося списка списка в Visual Studio см. в разделе последнюю предварительную версию.

> [!IMPORTANT]
> Если вы используете Visual Studio, убедитесь, что установлена последняя версия диспетчера пакетов NuGet. Более старые версии NuGet в Visual Studio не устанавливается правильно единой версии Xamarin.Forms 1.3.1. Перейдите к **Сервис > расширения и обновления...**  и щелкнуть **установленные** списка, чтобы убедиться, что **диспетчер пакетов NuGet для Visual Studio** не ниже версии 2.8.5. Если она была создана ранее, щелкните **обновления** списка, чтобы загрузить последнюю версию.

После обновления пакета NuGet Xamarin.Forms 1.3.1 внесите следующие изменения в каждом проекте для обновления до нового `Xamarin.Forms.Application` класса.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 переносимой библиотеки классов (или общего проекта)

Изменение **App.cs** файл, чтобы:

 - `App` Класс теперь наследует от `Application`.
 - `MainPage` Задано на первую страницу содержимого, которое должно отображаться.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Мы полностью удалили `GetMainPage` метод и вместо этого задайте `MainPage` *свойство* на `Application` подкласс.

Этот новый `Application` базовый класс также поддерживает `OnStart`, `OnSleep`, и `OnResume` переопределения, чтобы помочь в управлении жизненным циклом приложения.

`App` Класса передается в новый `LoadApplication` метод в каждом проекте приложения, как описано ниже:

### <a name="23-ios-app"></a>2.3 приложение iOS

Изменение **AppDelegate.cs** файл, чтобы:

 - Этот класс наследует от `FormsApplicationDelegate` (а не `UIApplicationDelegate` ранее).
 - `LoadApplication` вызывается для нового экземпляра `App`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="23-android-app"></a>2.3 приложение android

Изменение **MainActivity.cs** файл, чтобы:

 - Этот класс наследует от `FormsApplicationActivity` (а не `FormsActivity` ранее).
 - `LoadApplication` вызывается для нового экземпляра `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="24-windows-phone-app"></a>2.4 приложения Windows Phone

Необходимо обновить **MainPage** -XAML и фоновый код.

Изменение **MainPage.xaml** файл, чтобы:

 - Корневой элемент XAML должно быть `winPhone:FormsApplicationPage`.
 - `xmlns:phone` Атрибут должен быть *изменить* для `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Ниже показан обновленный пример — нужно только изменить такие вещи, (остальные атрибуты должны остаться прежними):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Изменение **MainPage.xaml.cs** файл, чтобы:

 - Этот класс наследует от `FormsApplicationPage` (а не `PhoneApplicationPage` ранее).
 - `LoadApplication` вызывается с новым экземпляром Xamarin.Forms `App` класса. Может потребоваться полные эту ссылку, так как Windows Phone имеет собственный `App` класс уже определен.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>Устранение неполадок

Иногда вы получите сообщение об ошибке следующего вида после обновления пакета Xamarin.Forms NuGet. Это происходит, когда средство обновления NuGet не полностью удаляет ссылки на более старых версий из вашей **csproj** файлов.

>ВАШ\_PROJECT.csproj: Ошибка: Данный проект ссылается на пакеты NuGet, отсутствующие на этом компьютере. Включите восстановление пакета NuGet для их загрузки.  Дополнительные сведения см. в разделе http://go.microsoft.com/fwlink/?LinkID=322105. Отсутствующий файл является... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (ВАШЕЙ\_ПРОЕКТА)

Чтобы устранить эти ошибки, откройте **csproj** файл в текстовом редакторе и поищите `<Target` элементы, которые ссылаются на более старых версиях Xamarin.Forms, такие как элемент, показанный ниже. Нужно вручную очистить этот весь элемент из **csproj** файл и сохраните изменения.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Проект должен быть построен успешно после удаления этих старые ссылки.

## <a name="considerations"></a>Особенности

Следующие факторы следует принимать во внимание при преобразовании существующего проекта Xamarin.Forms из классический API в новый единый API, если приложение зависит от того, компонент или пакет NuGet.

### <a name="components"></a>Компоненты

Любой компонент, который включен в приложении также потребуется обновить на единый API или при попытке компиляции возникнет конфликт. Для любой включен компонент замену текущей версии до новой версии из Store компонента Xamarin, который поддерживает единый API и выполните чистую сборку. Любой компонент, который еще не был преобразован автором, будут отображаться 32-разрядная версия только предупреждение в хранилище компонентов.

### <a name="nuget-support"></a>Поддержка NuGet

Хотя мы отправили изменения в NuGet для работы с поддержкой единый API, не было новую версию NuGet, поэтому мы при оценке способы получения NuGet для распознавания новых интерфейсов API.

До этого времени, так же, как компоненты вам потребуется для переключения любой пакет NuGet в проекте версию, которая поддерживает унифицированными API и после выполнения чистой сборки.

> [!IMPORTANT]
> При наличии ошибки в форме _«ошибка 3 нельзя включать в том же проекте Xamarin.iOS «monotouch.dll» и «Xamarin.iOS.dll» — «Xamarin.iOS.dll» указывается явным образом, хотя «monotouch.dll» ссылается "xxx, версия = 0.0.000, Culture = neutral, PublicKeyToken = null'»_ после преобразования приложения Unified API-интерфейсам, это обычно из-за наличия компонента или пакета NuGet в проекте, который не был обновлен на единый API. Необходимо удалить существующий компонент/NuGet, обновление до версии, которая поддерживает унифицированными API и выполните чистую сборку.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Включение 64-разрядных сборок приложений Xamarin.iOS

Для мобильного приложения Xamarin.iOS, был преобразован в единый API разработчик по-прежнему необходимо позволяют построить приложение для 64-разрядных компьютерах из параметров приложения. См. в разделе **Включение 64 разрядных сборок для приложений Xamarin.iOS** из [32 или 64-разрядных платформ](~/cross-platform/macios/32-and-64/index.md#enable-64) документа подробные инструкции по включению 64-разрядная версия сборки.

## <a name="summary"></a>Сводка

Приложение Xamarin.Forms теперь должен быть обновлен до версии 1.3.1 и переносом приложения iOS на единый API (которая поддерживает 64-разрядных архитектур на платформе iOS).

Как отмечалось выше, если приложение Xamarin.Forms включает машинного кода, таких как пользовательские модули подготовки отчетов или служб зависимостей, то они также может потребоваться обновление для использования новых типов [в единый API](~/cross-platform/macios/index.md).

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложения для iOS](~/cross-platform/macios/unified/updating-apps.md)
- [Обновление приложения для iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Обновление советы](~/cross-platform/macios/unified/updating-tips.md)
- [Классический и отличия Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
