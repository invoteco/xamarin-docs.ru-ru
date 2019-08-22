---
title: Как вручную установить вспомогательные библиотеки Android, необходимые для пакетов Xamarin.Android.Support?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: a43a2ed4498be76a99ab4b6b54d3048f2f80af5c
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887659"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Как вручную установить вспомогательные библиотеки Android, необходимые для пакетов Xamarin.Android.Support?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Примеры шагов для Xamarin. Android. support. v4 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Скачайте нужный пакет NuGet для Xamarin. Android. support (например, установите его с помощью диспетчера пакетов NuGet).

Используйте `ildasm` , чтобы проверить, какая версия **android_m2repository. zip** требуется для пакета NuGet:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

Пример выходных данных:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Скачайте **Android\_m2repository. zip** из Google, используя URL-адрес, возвращенный программой **Ildasm**. Кроме того, можно проверить версию _репозитория поддержки Android_ , установленную в данный момент в диспетчере пакет SDK для Android.

!["Пакет SDK для Android Manager, в котором установлен репозиторий поддержки Android версии 32"](install-android-support-library-images/sdk-extras.png)

Если версия совпадает с версией, необходимой для пакета NuGet, вам не нужно скачивать ничего нового. Вместо этого можно повторно заархивировать существующий каталог **m2repository** , расположенный в разделе " **\\дополнительные устройства Android** " в _пути пакета SDK_ (как показано в верхней части окна диспетчера пакет SDK для Android).

Вычислите хэш MD5 URL-адреса, возвращенного программой **Ildasm**. Отформатируйте результирующую строку для использования всех прописных букв и без пробелов. Например, при необходимости измените `$url` переменную, а затем выполните следующие 2 строки (на основе [исходного C# кода из Xamarin. Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) в PowerShell:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

Пример выходных данных:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Скопируйте **файл\_Android m2repository. zip** в папку **% LocalAppData\\%\\Xamarin\\ zips** . Переименуйте файл, чтобы использовать хэш MD5 из предыдущего шага вычисления хэша MD5. Например:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

Используемых Распакуйте файл в **% LocalAppData%\\Xamarin\\Xamarin. Android\\ . support. v4\\23.4.0.0\\** (создание **содержимого\\m2repository** подкаталог). Если пропустить этот шаг, то первая сборка, использующая библиотеку, займет немного больше времени, так как потребуется выполнить этот шаг.
Номер версии подкаталога (**23.4.0.0** в этом примере) не совпадает с версией пакета NuGet. С помощью `ildasm` можно найти правильный номер версии:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

Пример выходных данных:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Скачайте нужный пакет NuGet для Xamarin. Android. support (например, установите его с помощью диспетчера пакетов NuGet).

Дважды щелкните сборку _Xamarin. Android. support. v4_ в разделе References проекта Android в Visual Studio для Mac, чтобы открыть сборку в браузере сборок. Убедитесь, что в раскрывающемся списке _язык_ задано _C#_ значение, и выберите сборку высшего уровня _Xamarin. Android. support. v4_ из дерева навигации браузера сборок. Нахождение `IncludeAndroidResourcesFrom` свойства по одному из атрибутов или `JavaLibraryReference`: `SourceUrl`

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Скачайте **Android\_m2repository. zip** из Google с помощью `SourceUrl` команды, полученной от **Ildasm**. Кроме того, можно проверить версию _репозитория поддержки Android_ , установленную в данный момент в диспетчере пакет SDK для Android.

!["Пакет SDK для Android Manager, в котором установлен репозиторий поддержки Android версии 32"](install-android-support-library-images/sdk-extras.png)

Если версия совпадает с версией, необходимой для пакета NuGet, вам не нужно скачивать ничего нового. Вместо этого можно повторно заархивировать существующий каталог **m2repository** , расположенный в разделе " **вспомогательныеs/Android** " в _пути пакета SDK_ (как показано в верхней части окна диспетчера пакет SDK для Android).

Вычислите хэш MD5 URL-адреса, возвращенного программой **Ildasm**. Отформатируйте результирующую строку для использования всех прописных букв и без пробелов. Например, внесите необходимые изменения в строку URL-адреса, а затем выполните следующую команду в окне **терминала. приложение** Командная строка:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Другой вариант — использовать `csharp` интерпретатор для выполнения [того же C# кода, который использует Xamarin. Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Для этого измените `url` переменную по необходимости, а затем выполните следующую команду в окне **терминала. приложение** командной строки:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

Пример выходных данных:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Скопируйте **файл\_Android m2repository. zip** в папку **$Home/.локал/шаре/ксамарин/зипс/** . Переименуйте файл, чтобы использовать хэш MD5 из предыдущего шага вычисления хэша MD5. Например:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

Используемых Распакуйте файл в: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(создание подкаталога **Content/m2repository** ). Если пропустить этот шаг, то первая сборка, использующая библиотеку, займет немного больше времени, так как потребуется выполнить этот шаг.

Номер версии подкаталога (**23.4.0.0** в этом примере) не совпадает с версией пакета NuGet. Как и на предыдущем шаге, можно использовать браузер сборок в Visual Studio для Mac, чтобы найти правильный номер версии. Найдите свойство в одном `IncludeAndroidResourcesFrom` из атрибутов или `JavaLibraryReference`: `Version`

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Дополнительные ссылки

- [Ошибка 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) — неправильное скачивание. Скачайте {0} и вставьте его {1} в каталог ". и "установите пакет:"{0}, доступный в установщике пакета SDK ". сообщения об ошибках, связанные с пакетами Xamarin. Android. support.

### <a name="next-steps"></a>Следующие шаги

В этом документе рассматривается текущее поведение по состоянию на 2016 августа. Методика, описанная в этом документе, не является частью стабильного набора тестов для Xamarin, поэтому она может прерываться в будущем.

Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости .

