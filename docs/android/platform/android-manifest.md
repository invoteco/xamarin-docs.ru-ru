---
title: Работа с манифестом Android
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 1438c012608b367c21ebcc401c058b186b917f53
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027803"
---
# <a name="working-with-the-android-manifest"></a>Работа с манифестом Android

**AndroidManifest.xml** на платформе Android — это мощный файл, который позволяет описать функциональные возможности и требования приложения для Android. Но работать с ним непросто. Xamarin.Android помогает упростить работу, позволяя добавлять в классы настраиваемые атрибуты, которые будут использоваться для автоматического создания манифеста. Наша цель заключается в том, чтобы 99 % наших пользователей никогда не испытывали потребности вручную изменять **AndroidManifest.xml**. 

**AndroidManifest.xml** создается в процессе сборки, а найденный в **Properties/AndroidManifest.xml** XML-код объединяется с XML, созданным на основе настраиваемых атрибутов. Итоговый файл **AndroidManifest.xml** помещается в подкаталог **obj**, например для отладочных сборок. Например, он может находиться в папке **obj/Debug/android/AndroidManifest.xml**. Используется стандартный процесс слияния. Настраиваемые атрибуты в коде используются для создания XML-элементов, и эти элементы *вставляются* в **AndroidManifest.xml**. 

## <a name="the-basics"></a>Основы

В процессе компиляции в сборках выполняется поиск классов, которые не являются `abstract`, наследуют от класса [Activity](xref:Android.App.Activity) и имеют объявленный атрибут [`[Activity]`](xref:Android.App.ActivityAttribute). На основе этих классов и атрибутов создается манифест. Рассмотрим следующий пример кода: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

В результате файл **AndroidManifest.xml** будет пустым. Если вам нужен элемент `<activity/>`, необходимо использовать настраиваемый атрибут [`[Activity]`](xref:Android.App.Activity): 
custom attribute: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Этот пример приводит к добавлению следующего фрагмента XML в файл **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

Атрибут `[Activity]` не влияет на типы `abstract`. Все типы `abstract` игнорируются.

### <a name="activity-name"></a>Имя действия

Начиная с Xamarin.Android 5.1, имена типов для действий основаны на MD5SUM от имени экспортируемого типа с указанием сборки. Это позволяет предоставить одно и то же полное имя из двух разных сборок и не получить ошибку упаковки (до версии Xamarin.Android 5.1 имя типа действия по умолчанию составлялось из имени пространства имен и имени класса в нижнем регистре). 

Если вы хотите переопределить это поведение по умолчанию и указать имя действия явным образом, используйте свойство [`Name`](xref:Android.App.ActivityAttribute.Name): 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

В этом примере создается следующий фрагмент XML:

```xml
<activity android:name="awesome.demo.activity" />
```

> [!NOTE]
> Свойство `Name` следует использовать только для поддержки обратной совместимости, так как такое переименование может замедлить поиск типов во время выполнения. Если у вас есть старый код, который ожидает, что в качестве имени типа для действия по умолчанию используется имя пространства имен и имя класса в нижнем регистре, воспользуйтесь рекомендациями по сохранению совместимости, касающимися [имени вызываемой программы-оболочки](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming). 

### <a name="activity-title-bar"></a>Заголовок действия

По умолчанию Android присваивает приложению заголовок при выполнении. Для этого используется значение [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label). В большинстве случаев это значение отличается от имени класса. Чтобы указать в приложении метку для заголовка окна, используйте свойство [`Label`](xref:Android.App.ActivityAttribute.Label).
Пример: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

В этом примере создается следующий фрагмент XML:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

### <a name="launchable-from-application-chooser"></a>Возможность запуска из средства выбора приложений

По умолчанию действие не отображается на экране запуска приложений Android. Это связано с тем, что в приложении может быть много действий. Вам не потребуется значок для каждого из них. Чтобы указать, какие из них должны запускаться через средство запуска приложений, используйте свойство [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher). Пример: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

В этом примере создается следующий фрагмент XML:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### <a name="activity-icon"></a>Значок действия

По умолчанию действию присваивается значок запуска, предоставляемый системой. Чтобы использовать пользовательский значок, добавьте нужный файл **.png** в папку **Resources/drawable**, задайте для него действие сборки **AndroidResource** и с помощью свойства [`Icon`](xref:Android.App.ActivityAttribute.Icon) укажите этот файл в качестве значка. Пример: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

В этом примере создается следующий фрагмент XML:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### <a name="permissions"></a>Разрешения

При добавлении разрешений в манифест Android (как описано в [этой статье](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)) такие разрешения записываются в файл **Properties/AndroidManifest.xml**. Например, если задать разрешение `INTERNET`, то в **Properties/AndroidManifest.xml** будет добавлен следующий элемент: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Отладочные сборки автоматически устанавливают некоторые разрешения, чтобы упростить отладку (например `INTERNET` и `READ_EXTERNAL_STORAGE`) &ndash; эти параметры задаются только в созданном **obj/Debug/android/AndroidManifest.xml** и не отображаются как включенные в параметрах **необходимых разрешений**. 

Например, если вы изучите созданный файл манифеста в **obj/Debug/android/AndroidManifest.xml**, то увидите следующие добавленные элементы разрешений: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

В версии сборки выпуска эти разрешения *не* включаются в манифест автоматически (**obj/Debug/android/AndroidManifest.xml**). Если обнаружится, что при переключении на сборку выпуска приложение теряет разрешение, которое было доступно в сборке отладки, убедитесь, что это разрешение явным образом включено в параметрах **необходимых разрешений** для этого приложения (в разделе **Сборка > Приложение Android** в Visual Studio для Mac или **Свойства > Манифест Android** в Visual Studio). 

## <a name="advanced-features"></a>Дополнительные функции

### <a name="intent-actions-and-features"></a>Действия и функции намерения

Манифест Android предоставляет способ для описания возможностей действия. Это делается с помощью [намерений](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) и настраиваемого атрибута [`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
. Вы можете указать действия для своего действия с помощью конструктора [`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*),
а подходящие категории — через свойство [`Categories`](xref:Android.App.IntentFilterAttribute.Categories)
. Необходимо указать хотя бы одно действие (именно поэтому действия предоставляются в конструкторе). `[IntentFilter]` может предоставляться несколько раз. При каждом его использовании в `<activity/>`создается отдельный элемент `<intent-filter/>`. Пример:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

В этом примере создается следующий фрагмент XML:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```

### <a name="application-element"></a>Элемент Application

Кроме того, в манифесте Android можно объявлять свойства для всего приложения. Это делается с помощью элемента `<application>` и его аналога, настраиваемого атрибута [Application](xref:Android.App.ApplicationAttribute). Обратите внимание, что это параметры уровня приложения (сборки), а не отдельные параметры для каждого действия. Как правило, свойства `<application>` объявляются для всего приложения, а затем переопределяются (по мере необходимости) для каждого действия. 

Например, следующий атрибут `Application`, добавленный в **AssemblyInfo.cs**, означает, что приложение доступно для отладки, у него есть понятное для пользователя имя **My App** и в нем используется стиль `Theme.Light` в качестве темы по умолчанию для всех действий: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Это объявление приводит к созданию следующего фрагмента XML в файле **obj/Debug/Android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

В этом примере все действия в приложении будут по умолчанию использовать стиль `Theme.Light`. Если задать для действия тему `Theme.Dialog`, то стиль `Theme.Dialog` будет применен только к этому действию. Все остальные действия в приложении будут использовать стиль по умолчанию `Theme.Light`, который задан в элементе `<application>`. 

Элемент `Application` — не единственный способ настройки атрибутов `<application>`. Вместо этого вы можете вставлять атрибуты непосредственно в элемент `<application>` файла **Properties/AndroidManifest.xml**. Эти параметры объединяются в последний элемент `<application>` в файле **obj/Debug/Android/AndroidManifest.xml**. Обратите внимание, что содержимое **Properties/AndroidManifest.xml** всегда переопределяет данные, предоставленные пользовательскими атрибутами. 

Есть множество атрибутов на уровне приложения, которые можно настроить в элементе `<application>`. Дополнительные сведения об этих параметрах см. в разделе документации по [ApplicationAttribute](xref:Android.App.ApplicationAttribute), посвященном [открытым свойствам](xref:Android.App.ApplicationAttribute). 

## <a name="list-of-custom-attributes"></a>Список настраиваемых атрибутов

- [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute). Позволяет создать фрагмент XML [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html). 
- [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute). Позволяет создать фрагмент XML [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html). 
- [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute). Позволяет создать фрагмент XML [/manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html). 
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute). Позволяет создать фрагмент XML [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html). 
- [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute). Позволяет создать фрагмент XML [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html). 
- [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute). Позволяет создать фрагмент XML [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html). 
- [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute). Позволяет создать фрагмент XML [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html). 
- [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute). Позволяет создать фрагмент XML [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html). 
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute). Позволяет создать фрагмент XML [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html). 
- [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute). Позволяет создать фрагмент XML [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html). 
- [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute). Позволяет создать фрагмент XML [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html). 
- [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute). Позволяет создать фрагмент XML [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html). 
- [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute). Позволяет создать фрагмент XML [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html). 
- [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute). Позволяет создать фрагмент XML [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html).
