---
title: Работа с манифестом Android
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 1aaacae8cebed2396661a28c189af44c25238e7b
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887843"
---
# <a name="working-with-the-android-manifest"></a>Работа с манифестом Android

**AndroidManifest. XML** — это мощный файл на платформе Android, который позволяет описать функциональные возможности и требования приложения для Android. Однако работать с ним непросто. Xamarin. Android помогает снизить эту сложность, позволяя добавлять настраиваемые атрибуты в классы, которые затем будут использоваться для автоматического создания манифеста. Наша цель заключается в том, что 99% наших пользователей никогда не нужно вручную изменять **AndroidManifest. XML**. 

**AndroidManifest. XML** создается как часть процесса сборки, а XML-код, найденный в **свойствах/AndroidManifest. XML** , объединяется с XML, созданным на основе настраиваемых атрибутов. Полученный Объединенный **файл AndroidManifest. XML** находится в подкаталоге **obj** ; Например, он находится в **файле obj/Debug/Android/AndroidManifest. XML** для отладочных сборок. Процесс слияния является тривиальным: он использует пользовательские атрибуты в коде для создания XML-элементов и *вставляет* эти элементы в **AndroidManifest. XML**. 



## <a name="the-basics"></a>Основы

Во время компиляции сборки сканируются для`abstract` классов, не являющихся классами, которые являются производными от [Activity](xref:Android.App.Activity) и имеют [`[Activity]`](xref:Android.App.ActivityAttribute) объявленный атрибут. Затем эти классы и атрибуты используются для создания манифеста. Рассмотрим следующий пример кода: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Это приведет к невозможности создания в **AndroidManifest. XML**. Если требуется `<activity/>` создать элемент, необходимо использовать[`[Activity]`](xref:Android.App.Activity) 
настраиваемый атрибут: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Этот пример приводит к добавлению следующего фрагмента XML в **AndroidManifest. XML**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

Атрибут не влияет на `abstract` типы; `[Activity]` `abstract` типы игнорируются.



### <a name="activity-name"></a>Имя действия

Начиная с Xamarin. Android 5,1, имя типа действия основано на MD5SUM имени экспортируемого типа с указанием сборки. Это позволяет предоставлять одно и то же полное имя из двух разных сборок и не возвращать ошибку упаковки. (Перед Xamarin. Android 5,1 имя типа действия по умолчанию было создано из пространства имен в нижнем регистре и имени класса.) 

Если вы хотите переопределить это значение по умолчанию и явно указать имя действия, используйте [`Name`](xref:Android.App.ActivityAttribute.Name) свойство: 

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

*Примечание*. `Name` свойство следует использовать только в целях обратной совместимости, так как такое Переименование может замедлить Поиск во время выполнения. Если у вас есть устаревший код, который ожидает, что имя типа по умолчанию для действия должно основываться на пространстве имен в нижнем регистре и имени класса, см. раздел имя вызываемой [оболочки Android](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) для получения советов по обеспечению совместимости. 


### <a name="activity-title-bar"></a>Строка заголовка действия

По умолчанию Android предоставляет приложению заголовок при его запуске. Для этого [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label)используется значение. В большинстве случаев это значение будет отличаться от имени класса. Чтобы указать метку приложения в заголовке окна, используйте [`Label`](xref:Android.App.ActivityAttribute.Label) свойство.
Например: 

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


### <a name="launchable-from-application-chooser"></a>Запуск из средства выбора приложений

По умолчанию действие не отображается на экране запуска приложений Android. Это связано с тем, что в приложении может быть много действий, и вам не нужен значок для каждого из них. Чтобы указать, какие из них должны быть запущены в средстве запуска приложений [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher) , используйте свойство. Например: 

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

По умолчанию для действия будет задан значок запуска по умолчанию, предоставляемый системой. Чтобы использовать пользовательский значок, сначала добавьте **. png** в Resources **/Draw**, задайте для его действия сборки значение **AndroidResource**, а затем используйте [`Icon`](xref:Android.App.ActivityAttribute.Icon) свойство, чтобы указать используемый значок. Например: 

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

При добавлении разрешений в манифест Android (как описано в разделе [Добавление разрешений в манифест Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)) эти разрешения записываются в **свойствах/AndroidManifest. XML**. Например, если задать `INTERNET` разрешение, в свойствах **/AndroidManifest. XML**будет добавлен следующий элемент: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Отладочные сборки автоматически устанавливают некоторые разрешения, чтобы упростить отладку ( `INTERNET` например `READ_EXTERNAL_STORAGE`, &ndash; и) эти параметры задаются только в созданном **файле obj/Debug/Android/AndroidManifest. XML** и не отображаются как включенные в **Необходимые параметры разрешений** . 

Например, если вы изучите созданный файл манифеста в файле **obj/Debug/Android/AndroidManifest. XML**, вы можете увидеть следующие добавленные элементы разрешений: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

В версии сборки выпуска манифеста (в **файле obj/Debug/Android/AndroidManifest. XML**) эти разрешения *не* настраиваются автоматически. Если вы обнаружите, что переключение на сборку выпуска приводит к тому, что приложение теряет разрешение, доступное в отладочной сборке, убедитесь, что это разрешение явно задано в параметрах, **необходимых** для приложения (см. раздел **Сборка > Android Приложение** в Visual Studio для Mac; см. раздел **свойства > манифесте Android** в Visual Studio). 




## <a name="advanced-features"></a>Дополнительные функции


### <a name="intent-actions-and-features"></a>Действия и функции намерения

Манифест Android предоставляет способ описания возможностей действия. Это делается с помощью [целей](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) и[`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
настраиваемый атрибут. Можно указать, какие действия соответствуют действию, с помощью[`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)
и какие категории подходят для[`Categories`](xref:Android.App.IntentFilterAttribute.Categories)
. Необходимо указать по крайней мере одно действие (это объясняется тем, что действия предоставляются в конструкторе). `[IntentFilter]`может предоставляться несколько раз, и каждое использование приводит к отдельному `<intent-filter/>` элементу `<activity/>`в. Например:

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

В манифесте Android также предусмотрен способ объявления свойств для всего приложения. Это делается с `<application>` помощью элемента и его аналога, настраиваемого атрибута [приложения](xref:Android.App.ApplicationAttribute) . Обратите внимание, что это параметры уровня приложения (на уровне сборки), а не параметры для каждого действия. Как правило, вы `<application>` объявляете свойства для всего приложения, а затем переопределяете эти параметры (при необходимости) для каждого действия. 

Например, следующий `Application` атрибут добавляется в **AssemblyInfo.CS** , чтобы указать, что приложение может быть отлажено, что его понятное имя — **мое приложение**и используется `Theme.Light` стиль в качестве темы по умолчанию для всех процедур 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Это объявление приводит к созданию следующего фрагмента XML в файле **obj/Debug/Android/AndroidManifest. XML**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

В этом примере все действия в приложении будут по умолчанию `Theme.Light` применены к стилю. Если задать тему действия равным `Theme.Dialog`, то только это действие будет `Theme.Dialog` использовать стиль, тогда как все остальные действия в `Theme.Light` приложении будут по умолчанию иметь стиль, заданный в `<application>` элементе. 

Элемент не является единственным способом настройки `<application>` атрибутов. `Application` Кроме того, можно вставлять атрибуты непосредственно в `<application>` элемент **Properties/AndroidManifest. XML**. Эти параметры сливаются с последним `<application>` элементом, который находится в **obj/Debug/Android/AndroidManifest. XML**. Обратите внимание, что содержимое **Properties/AndroidManifest. XML** всегда переопределяет данные, предоставленные пользовательскими атрибутами. 

Существует множество атрибутов уровня приложения, которые можно настроить в `<application>` элементе. Дополнительные сведения об этих параметрах см. в разделе общедоступные [Свойства](xref:Android.App.ApplicationAttribute) [аппликатионаттрибуте](xref:Android.App.ApplicationAttribute). 



## <a name="list-of-custom-attributes"></a>Список настраиваемых атрибутов

- [Android. app. активитяттрибуте](xref:Android.App.ActivityAttribute) : Создает фрагмент XML [/Манифест/аппликатион/Активити](https://developer.android.com/guide/topics/manifest/activity-element.html) 
- [Android. app. аппликатионаттрибуте](xref:Android.App.ApplicationAttribute) : Создает фрагмент XML [/Манифест/аппликатион](https://developer.android.com/guide/topics/manifest/application-element.html) 
- [Android. app. инструментатионаттрибуте](xref:Android.App.InstrumentationAttribute) : Создает фрагмент XML [/Манифест/инструментатион](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) 
- [Android. app. интентфилтераттрибуте](xref:Android.App.IntentFilterAttribute) : Создает фрагмент XML [//Интент-филтер](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) 
- [Android. app. MetaDataAttribute](xref:Android.App.MetaDataAttribute) : Создает фрагмент XML [//мета-дата](https://developer.android.com/guide/topics/manifest/meta-data-element.html) 
- [Android. app. PrincipalPermissionAttribute](xref:Android.App.PermissionAttribute) : Создает фрагмент XML [//пермиссион](https://developer.android.com/guide/topics/manifest/permission-element.html) 
- [Android. app. пермиссионграупаттрибуте](xref:Android.App.PermissionGroupAttribute) : Создает фрагмент XML [//пермиссион-грауп](https://developer.android.com/guide/topics/manifest/permission-group-element.html) 
- [Android. app. пермиссионтриаттрибуте](xref:Android.App.PermissionTreeAttribute) : Создает фрагмент XML [//пермиссион-три](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) 
- [Android. app. сервицеаттрибуте](xref:Android.App.ServiceAttribute) : Создает фрагмент XML [/Манифест/аппликатион/сервице](https://developer.android.com/guide/topics/manifest/service-element.html) 
- [Android. app. усеслибраряттрибуте](xref:Android.App.UsesLibraryAttribute) : Создает фрагмент XML [/Манифест/аппликатион/усес-либрари](https://developer.android.com/guide/topics/manifest/uses-library-element.html) 
- [Android. app. усеспермиссионаттрибуте](xref:Android.App.UsesPermissionAttribute) : Создает фрагмент XML [/Манифест/усес-пермиссион](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) 
- [Android. Content. броадкастрецеивераттрибуте](xref:Android.Content.BroadcastReceiverAttribute) : Создает фрагмент XML [/Манифест/аппликатион/рецеивер](https://developer.android.com/guide/topics/manifest/receiver-element.html) 
- [Android. Content. контентпровидераттрибуте](xref:Android.Content.ContentProviderAttribute) : Создает фрагмент XML [/Манифест/аппликатион/провидер](https://developer.android.com/guide/topics/manifest/provider-element.html) 
- [Android. Content. грантурипермиссионаттрибуте](xref:Android.Content.GrantUriPermissionAttribute) : Создает фрагмент XML [/Манифест/аппликатион/провидер/Грант-Ури-пермиссион](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html)

