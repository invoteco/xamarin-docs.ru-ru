---
title: Как вручную установить вспомогательные библиотеки Android, необходимые для пакетов Xamarin.Android.Support?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 99571e0b62592597bb1fffdc8d3ed8336fe050b2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026930"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Как вручную установить вспомогательные библиотеки Android, необходимые для пакетов Xamarin.Android.Support?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Примеры шагов для Xamarin.Android.Support.v4 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Загрузите нужный пакет NuGet для Xamarin.Android.Support (например, установите его с помощью диспетчера пакетов NuGet).

Используйте `ildasm`, чтобы узнать, какая версия **android_m2repository.zip** требуется пакету NuGet:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

Пример выходных данных:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Загрузите **android\_m2repository.zip** из Google с помощью URL-адреса, возвращенного **ildasm**. Кроме того, вы можете проверить версию _репозитория поддержки Android_, установленного в настоящее время, в диспетчере SDK Android:

!["Диспетчер SDK Android показывает, что установлен репозиторий поддержки Android версии 32"](install-android-support-library-images/sdk-extras.png)

Если версия совпадает с версией, необходимой для пакета NuGet, вам не нужно загружать ничего нового. Вместо этого можно повторно заархивировать существующий каталог **m2repository**, расположенный в разделе **extras\\android** в _путь_к_пакету_SDK_ (как показано в верхней части окна Диспетчера SDK Android).

Вычислите хэш MD5 URL-адреса, возвращенного из **ildasm**. Отформатируйте результирующую строку, чтобы использовались только прописные буквы без пробелов в строке. Например, при необходимости измените переменную `$url`, а затем выполните следующие 2 строки (на основе [исходного кода C# из Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) в PowerShell:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

Пример выходных данных:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Скопируйте **android\_m2repository.zip** в папку **%LOCALAPPDATA%\\Xamarin\\zips\\** . Переименуйте файл, чтобы использовать хэш MD5 из предыдущего шага вычисления хэша MD5. Пример:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Необязательно) распакуйте файл в папку **%LOCALAPPDATA%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\content\\** (создавая подкаталог **content\\m2repository**). Если пропустить этот шаг, то первая сборка, использующая библиотеку, займет немного больше времени, так как потребуется выполнить этот шаг.
Номер версии для подкаталога (**23.4.0.0** в этом примере) не совпадает с версией пакета NuGet. Чтобы найти правильный номер версии, можно использовать `ildasm`:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

Пример выходных данных:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Загрузите нужный пакет NuGet для Xamarin.Android.Support (например, установите его с помощью диспетчера пакетов NuGet).

Дважды щелкните сборку _Xamarin.Android.Support.v4_ в разделе _Ссылки_ проекта Android в Visual Studio для Mac, чтобы открыть сборку в обозревателе сборок. Убедитесь, что для раскрывающегося списка _Язык_ выбрано значение _C#_ , и выберите сборку _Xamarin.Android.support.v4_ в обозревателе сборок. Выберите свойство `SourceUrl` в одном из атрибутов `IncludeAndroidResourcesFrom` или `JavaLibraryReference`:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Загрузите **android\_m2repository.zip** из Google с помощью `SourceUrl`, возвращенного **ildasm**. Кроме того, вы можете проверить версию _репозитория поддержки Android_, установленного в настоящее время, в диспетчере SDK Android:

!["Диспетчер SDK Android показывает, что установлен репозиторий поддержки Android версии 32"](install-android-support-library-images/sdk-extras.png)

Если версия совпадает с версией, необходимой для пакета NuGet, вам не нужно загружать ничего нового. Вместо этого можно повторно заархивировать существующий каталог **m2repository**, расположенный в разделе **extras/android** в _путь_к_пакету_SDK_ (как показано в верхней части окна Диспетчера SDK Android).

Вычислите хэш MD5 URL-адреса, возвращенного из **ildasm**. Отформатируйте результирующую строку, чтобы использовались только прописные буквы без пробелов в строке. Например, при необходимости исправьте строку URL-адреса, а затем выполните следующую команду в командной строке **Terminal.app**:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Другой вариант — использовать интерпретатор `csharp` для выполнения [того же кода C#, который использует Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Для этого при необходимости измените переменную `url`, а затем выполните следующую команду в командной строке **Terminal.app**:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

Пример выходных данных:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Скопируйте **android\_m2repository.zip** в папку **$HOME/.local/share/Xamarin/zips/** . Переименуйте файл, чтобы использовать хэш MD5 из предыдущего шага вычисления хэша MD5. Пример:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Необязательно) распакуйте файл в: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(создавая подкаталог **content/m2repository**). Если пропустить этот шаг, то первая сборка, использующая библиотеку, займет немного больше времени, так как потребуется выполнить этот шаг.

Номер версии для подкаталога (**23.4.0.0** в этом примере) не совпадает с версией пакета NuGet. Как и на шаге **ildasm** ранее, чтобы найти правильный номер версии, можно использовать обозреватель сборок в Visual Studio для Mac. Найдите свойство `Version` в одном из атрибутов `IncludeAndroidResourcesFrom` или `JavaLibraryReference`:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>Дополнительные ссылки

- [Ошибка 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) — неточные сообщения об ошибках "Загрузка не выполнена. Загрузите {0} и сохраните его в каталог {1}" и "Установите пакет: {0} доступен в установщике пакета SDK", связанные с пакетами Xamarin.Android.Support.

### <a name="next-steps"></a>Следующие шаги

В этом документе рассматривается текущее поведение по состоянию на август 2016 года. Методика, описанная в этом документе, не является частью стабильного набора тестов для Xamarin, поэтому в будущем ее поддержка может прерваться.

Чтобы получить дополнительную помощь, связаться с нами или если проблема остается даже после использования указанных выше сведений, сведения о способах связи, предложениях, а также о том, как при необходимости сообщить о новой ошибке, см. [здесь](~/cross-platform/troubleshooting/support-options.md).
