---
title: Безопасность транспортного уровня (TLS) 1,2
description: В этом документе описано, как включить TLS 1,2 для проектов Xamarin. iOS, Xamarin. Android и Xamarin. Mac. В нем показано, как это сделать в Visual Studio 2019 и Visual Studio для Mac.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 6175725ae3eea805680b4da81aa0458aa3f8a68c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280354"
---
# <a name="transport-layer-security-tls-12"></a>Безопасность транспортного уровня (TLS) 1,2

Использование последней версии протокола [_Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) важно для обеспечения безопасности сетевых подключений приложений.

> [!WARNING]
> **Апрель 2018** — из-за повышенных требований к безопасности, включая соответствие требованиям PCI, основным поставщикам облачных услуг и веб-серверам требуется отключить поддержку протоколов TLS версий старше 1,2. Проекты Xamarin, созданные в предыдущих версиях Visual Studio по умолчанию, используют более старые версии TLS.
>
> Чтобы ваши приложения продолжали работать с этими серверами и службами, необходимо **обновить проекты Xamarin, чтобы использовать приведенные ниже параметры, а затем повторно создать и повторно развернуть приложения** для пользователей.

Проекты должны ссылаться на сборку **System .NET. http** и быть настроены, как показано ниже.

## <a name="update-xamarinandroid-to-tls-12"></a>Обновление Xamarin. Android до TLS 1,2

Обновите параметры реализации **HttpClient** и **SSL/TLS** , чтобы обеспечить безопасность TLS 1,2.

> [!NOTE]
> Требуется Android 5,0 или более поздней версии.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Эти параметры можно найти в **свойствах проекта > параметры Android** , а затем нажать кнопку **Дополнительно** :

[![Настройка HttpClient и TLS в Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Эти параметры можно найти в окне " **Параметры проекта" > "сборка > вкладка" сборка Android** ":

[![Настройка HttpClient и TLS в Visual Studio для Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Обновление Xamarin. iOS до TLS 1,2

Обновите параметр **реализации HttpClient** , чтобы включить безопасность TSL 1,2.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Этот параметр можно найти в **свойствах проекта > сборка iOS**:

[![Настройка HttpClient и TLS в Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Этот параметр можно найти в **параметрах проекта > сборка > вкладка сборки iOS** :

[![Настройка HttpClient в Visual Studio для Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Обновление Xamarin. Mac до TLS 1,2

В Visual Studio для Mac, чтобы включить TLS 1,2 в приложении Xamarin. Mac, обновите параметр **реализации HttpClient** в параметрах **проекта > сборка Mac >** .

[![Настройка HttpClient в Visual Studio для Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> Будущий выпуск Xamarin.Mac 4.8 будет поддерживать только macOS 10.9 или более поздней версии.
> Предыдущие версии Xamarin.Mac поддерживали macOS 10.7 или более поздней версии, но в этих версиях macOS нет достаточной инфраструктуры TLS для поддержки TLS 1.2. Для macOS 10.7 или macOS 10.8 используйте Xamarin.Mac 4.6 или более ранней версии.

## <a name="alternative-configuration-options"></a>Альтернативные параметры конфигурации

В этом разделе обсуждаются альтернативные варианты конфигурации, поддерживаемые TLS 1,2.
Разработчики приложений должны учитывать эти варианты только в том случае, если они понимают риски использования различных уровней поддержки TLS.

### <a name="httpclient-implementation"></a>Реализация HttpClient

Разработчики Xamarin всегда могли использовать собственные сетевые классы в своем коде, однако существует также параметр, который определяет, какой сетевой стек используется `HttpClient` классами. Это предоставляет знакомый API .NET, который обладает преимуществами скорости и безопасности собственной платформы.

Доступны следующие варианты.

- **Управляемый стек** — предоставляемая моно функциональность сети или
- **Собственный стек** — различные сетевые интерфейсы API, предоставляемые базовыми платформами (Android, iOS или macOS).

Управляемый стек обеспечивает наивысший уровень совместимости с существующим кодом .NET, однако он может быть медленнее, что приведет к увеличению размера исполняемого файла.

Собственные параметры могут быть более быстрыми и иметь более высокий уровень безопасности (включая TLS 1,2), но могут не предоставлять все функции и `HttpClient` возможности класса.

### <a name="ssltls-implementation-android"></a>Реализация SSL/TLS (Android)

Параметры проекта Android также позволяют выбрать, какую из реализаций SSL/TLS следует поддерживать:

- **Моно/управляемый** — TLS 1,1 в Android
- **Native** — TLS 1,2 в Android.

В новых проектах Xamarin по умолчанию используется собственная реализация, которая поддерживает TLS 1,2 (рекомендуется для всех проектов), однако можно переключиться обратно на управляемый код, если это необходимо для обеспечения совместимости.

> [!IMPORTANT]
> Параметр **Mono/Managed** был [удален из параметров проекта iOS и Mac](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_10/xamarin.ios_10.8.md) .
>
> Собственный параметр всегда используется на платформах iOS и Mac.

## <a name="platform-specific-details"></a>Сведения о конкретной платформе

Приведенная выше сводка описывает параметры уровня проекта для реализации HttpClient и SSL/TLS в проектах Xamarin. Реализация HttpClient также может быть задана динамически в коде. Дополнительные сведения см. в соответствующих руководствах для конкретной платформы:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS и Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>Сводка

В любом случае приложения должны использовать протокол TLS 1,2.
Необходимо обновить параметры в существующих приложениях в соответствии с инструкциями, приведенными в этой статье, а затем повторно выполнить сборку и повторное развертывание для клиентов.

## <a name="related-links"></a>Связанные ссылки

- [Безопасность транспорта приложения](~/ios/app-fundamentals/ats.md)
- [Среда Xamarin. Android](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (Февраль 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Википедии)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Заметки о выпуске Mono 4,8 — поддержка TLS 1,2](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [борингссл](https://boringssl.googlesource.com/boringssl/)
- [Описание HttpClient, HttpClientHandler и Вебрекуессандлер](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System .NET. HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System .NET. HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System .NET. WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System .NET. WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [Java. NET. Урлконнектион](https://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation. CFNetwork](xref:CoreFoundation.CFNetwork)
- [Foundation. Нсурлконнектион](xref:Foundation.NSUrlConnection)
- [System .NET. WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP-клиент (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/httpclient/)
