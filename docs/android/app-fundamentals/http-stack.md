---
title: HttpClient Stack и селектор реализации SSL/TLS для Android
description: Селекторы реализации HttpClient Stack и SSL/TLS определяют реализацию HttpClient и SSL/TLS, которая будет использоваться приложениями Xamarin. Android.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 9f66764fef5c54563ffd03274b1f86a8c0bcc637
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522140"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient Stack и селектор реализации SSL/TLS для Android

Селекторы реализации HttpClient Stack и SSL/TLS определяют реализацию HttpClient и SSL/TLS, которая будет использоваться приложениями Xamarin. Android.

Проекты должны ссылаться на сборку **System .NET. http** .

> [!WARNING]
> **Апрель 2018** — из-за повышенных требований к безопасности, включая соответствие требованиям PCI, основным поставщикам облачных услуг и веб-серверам требуется отключить поддержку протоколов TLS версий старше 1,2. Проекты Xamarin, созданные в предыдущих версиях Visual Studio по умолчанию, используют более старые версии TLS.
>
> Чтобы ваши приложения продолжали работать с этими серверами и службами, необходимо  **`Android HttpClient` обновить проекты Xamarin с помощью параметров и `Native TLS 1.2` , приведенных ниже, а затем повторно создать и повторно развернуть приложения** для пользователей.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Конфигурация Xamarin. Android HttpClient находится в **параметрах проекта > параметры Android**, а затем нажмите кнопку **Дополнительные параметры** .

Ниже приведены рекомендуемые параметры для поддержки TLS 1,2.

[![Параметры Visual Studio для Android](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Конфигурация HttpClient Xamarin. Android находится в **параметрах проекта > сборка > параметры сборки Android** и перейдите на вкладку **Общие** .

Ниже приведены рекомендуемые параметры для поддержки TLS 1,2.

[![Visual Studio для Mac параметры Android](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>Альтернативные параметры конфигурации

### <a name="androidclienthandler"></a>андроидклиенсандлер

Андроидклиенсандлер — это новый обработчик, который делегирует собственный код Java/OS вместо реализации всех элементов в управляемом коде.
**Это рекомендуемый вариант.**

#### <a name="pros"></a>ИТ специалистов

- Используйте собственный API для повышения производительности и меньшего размера исполняемого файла.
- Поддержка новейших стандартов, например TLS 1,2.

#### <a name="cons"></a>Недостатки

- Требуется Android 4,1 или более поздней версии.
- Некоторые функции и параметры HttpClient недоступны.

### <a name="managed-httpclienthandler"></a>Управляемый (HttpClientHandler)

Управляемый обработчик — это полностью управляемый обработчик HttpClient, который поставлялся с предыдущими версиями Xamarin. Android.

#### <a name="pros"></a>ИТ специалистов

- Он является наиболее совместимым (функциями) с MS .NET и более старыми версиями Xamarin.

#### <a name="cons"></a>Недостатки

- Он не полностью интегрирован с операционной системой (например, ограничено TLS 1,0).
- Обычно это происходит намного медленнее (например, шифрование), чем собственный API.
- Для этого требуется более управляемый код, создавая более крупные приложения.



### <a name="choosing-a-handler"></a>Выбор обработчика

Выбор между `AndroidClientHandler` и `HttpClientHandler` зависит от потребностей вашего приложения. `AndroidClientHandler`рекомендуется использовать для обеспечения актуальной поддержки безопасности, например.

- Требуется поддержка TLS 1.2 +.
- Ваше приложение предназначено для Android 4,1 (API 16) или более поздней версии.
- Требуется поддержка TLS 1.2 + для `HttpClient`.
- Поддержка TLS 1.2 + для `WebClient`не требуется.

`HttpClientHandler`является хорошим выбором, если требуется поддержка TLS 1.2 +, но она должна поддерживать версии Android более ранней, чем Android 4,1. Это также хороший вариант, если требуется поддержка TLS 1.2 + для `WebClient`.

Начиная с Xamarin. Android 8,3, `HttpClientHandler` по умолчанию используется скучный`btls`протокол SSL () в качестве базового поставщика TLS. Скучный поставщик SSL TLS предоставляет следующие преимущества:

- Он поддерживает TLS 1.2 +.
- Она поддерживает все версии Android.
- Он обеспечивает поддержку TLS 1.2 + для `HttpClient` и. `WebClient`

Недостатком использования скучных протоколов SSL в качестве поставщика ундерлинг TLS является то, что он может увеличить размер результирующего APK (он добавляет около 1 МБ дополнительного APK размера для каждого поддерживаемого ABI).

Начиная с Xamarin. Android 8,3, поставщик TLS по умолчанию является скучным`btls`протоколом SSL (). Если вы не хотите использовать скучный протокол SSL, можно вернуться к управляемой реализации SSL с предысторией, задав `$(AndroidTlsProvider)` для `legacy` свойства значение (Дополнительные сведения о настройке свойств сборки см. в разделе [процесс сборки](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>Программное использование`AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` Является реализациейспециальнодля`HttpMessageHandler` Xamarin. Android.
Экземпляры этого класса будут использовать собственную `java.net.URLConnection` реализацию для всех HTTP-соединений. Теоретически это обеспечит увеличение производительности HTTP и небольших размеров APK.

Этот фрагмент кода является примером явного указания одного экземпляра `HttpClient` класса:

```csharp
// Android 4.1 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> Базовое устройство Android должно поддерживать TLS 1,2 (IE. Android 4,1 и более поздние версии). Обратите внимание, что официальная поддержка TLS 1,2 находится в Android 5.0 +. Однако некоторые устройства поддерживают TLS 1,2 в Android 4.1 +.


## <a name="ssltls-implementation-build-option"></a>Параметр сборки реализации SSL/TLS

Этот параметр проекта определяет, какая базовая библиотека TLS будет использоваться всеми веб-запросами, `HttpClient` и. `WebRequest` По умолчанию выбран TLS 1,2:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Поле со списком для реализации TLS/SSL в Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![Поле со списком для реализации TLS/SSL в Visual Studio для Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Например:

```csharp
var client = new HttpClient();
```

Если для реализации HttpClient задано значение **Managed** , а в качестве реализации TLS установлено значение **native TLS 1.2 +** `client` , то объект будет автоматически использовать управляемые `HttpClientHandler` и TLS 1,2 (предоставляемые библиотекой борингссл) для HTTP-запросы.

Однако если для `AndroidHttpClient` **реализации HttpClient** задано значение, то все `HttpClient` объекты будут использовать базовый класс `java.net.URLConnection` Java и не будут затронуты значением **реализации TLS/SSL** . `WebRequest`объекты будут использовать библиотеку Борингссл.

## <a name="other-ways-to-control-ssltls-configuration"></a>Другие способы управления конфигурацией SSL/TLS

Существует три способа, которыми приложение Xamarin. Android может управлять параметрами TLS:

1. Выберите реализацию HttpClient и библиотеку TLS по умолчанию в параметрах проекта.
2. Программно `Xamarin.Android.Net.AndroidClientHandler`с помощью.
3. Объявите переменные среды (необязательно).

Из трех вариантов рекомендуемый подход заключается в использовании параметров проекта Xamarin. Android для объявления по умолчанию `HttpMessageHandler` и TLS для всего приложения. Затем при необходимости программно создать экземпляры `Xamarin.Android.Net.AndroidClientHandler` объектов. Эти параметры описаны выше.

Третий вариант &ndash; использования переменных &ndash; среды описан ниже.

### <a name="declare-environment-variables"></a>Объявить переменные среды

Существует две переменные среды, связанные с использованием протокола TLS в Xamarin. Android:

- `XA_HTTP_CLIENT_HANDLER_TYPE`Эта переменная среды объявляет значение по умолчанию `HttpMessageHandler` , которое будет использоваться приложением. &ndash; Например:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER`Эта переменная среды определяет, какую библиотеку TLS будет использоваться `btls`, `legacy`или, или `default` (которая совпадает с отсутствием этой переменной): &ndash;

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Эта переменная среды задается путем добавления _файла среды_ в проект. Файл среды — это обычный текстовый файл в формате UNIX с действием сборки **андроиденвиронмент**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Снимок экрана действия сборки Андроиденвиронмент в Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Снимок экрана действия сборки Андроиденвиронмент в Visual Studio для Mac.](http-stack-images/tls03-xs.png)

-----

Дополнительные сведения о переменных среды и Xamarin. Android см. в разделе с руководством по [среде Xamarin. Android](~/android/deploy-test/environment.md) .


## <a name="related-links"></a>Связанные ссылки

- [Протокол TLS](~/cross-platform/app-fundamentals/transport-layer-security.md)
