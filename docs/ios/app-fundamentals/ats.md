---
title: Безопасность транспорта приложений в Xamarin. iOS
description: Безопасность транспорта приложений (ATS) обеспечивает безопасные подключения между Интернет-ресурсами (например, серверным сервером приложения) и приложением.
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: 84d235bb7c6874255ea025ff5897e150bd6f023b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724687"
---
# <a name="app-transport-security-in-xamarinios"></a>Безопасность транспорта приложений в Xamarin. iOS

_Безопасность транспорта приложений (ATS) обеспечивает безопасные подключения между Интернет-ресурсами (например, серверным сервером приложения) и приложением._

В этой статье содержатся сведения об изменениях в системе безопасности, которые применяются в приложении для iOS 9, а также о том, что [это означает для проектов Xamarin. iOS](#xamarinsupport). в нем рассматриваются [Параметры конфигурации ATS](#config) , а также рассказывается о том, как [отказаться от ATS](#optout) ATS при необходимости. Поскольку ATS включен по умолчанию, любое небезопасное подключение к Интернету вызовет исключение в приложениях iOS 9 (если явно не разрешено).

## <a name="about-app-transport-security"></a>О безопасности транспорта приложений

Как упоминалось выше, ATS гарантирует, что все Интернет-подключения в iOS 9 и OS X El Capitan соответствуют рекомендациям по обеспечению безопасного подключения, тем самым предотвращая случайное раскрытие конфиденциальной информации либо непосредственно через приложение, либо с библиотекой, которую использует.

Для существующих приложений реализуйте протокол `HTTPS` везде, где это возможно. Для новых приложений Xamarin. iOS следует использовать `HTTPS` исключительно при взаимодействии с интернетными ресурсами. Кроме того, Обмен данными API высокого уровня должен быть зашифрован с помощью протокола TLS версии 1,2 и пересылки.

Любое подключение, установленное с помощью [нсурлконнектион](xref:Foundation.NSUrlConnection), [кфурл](xref:CoreFoundation.CFUrl) или [NSUrlSession](xref:Foundation.NSUrlSession) , по умолчанию будет использовать ATS в приложениях, созданных для iOS 9 и OS X 10,11 (El Capitan).

## <a name="default-ats-behavior"></a>Поведение ATS по умолчанию

Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9 и OS X 10,11 (El Capitan), все подключения, использующие [нсурлконнектион](xref:Foundation.NSUrlConnection), [кфурл](xref:CoreFoundation.CFUrl) или [NSUrlSession](xref:Foundation.NSUrlSession) , будут подвергаться требованиям к безопасности ATS. Если подключения не соответствуют этим требованиям, они завершатся сбоем с исключением.

### <a name="ats-connection-requirements"></a>Требования к соединению ATS

ATS будет применять следующие требования ко всем подключениям к Интернету:

- Все шифры подключений должны использовать прямую секретную косую. См. список допустимых шифров ниже.
- Протокол TLS должен иметь версию 1,2 или более позднюю.
- Для всех сертификатов должен использоваться по крайней мере отпечаток SHA256 с ключом RSA (2048 или выше) или с 256-битным или большим (ECC) ключом.

Опять же, поскольку ATS включена по умолчанию в iOS 9, любая попытка установить соединение, не удовлетворяющее этим требованиям, приведет к возникновению исключения.

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS совместимые шифры

Следующие типы шифров перенаправления безопасной передачи данных принимаются ATS безопасными подключениями к Интернету.

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Дополнительные сведения о работе с классами взаимодействия с Интернетом iOS см. в справочнике по [классу Нсурлконнектион](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755)Apple, Справочнику по [кфурл](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) или [NSURLSession](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Поддержка ATS в Xamarin. iOS

Так как ATS включен по умолчанию в iOS 9 и OS X El Capitan, если ваше приложение Xamarin. iOS или любая библиотека или служба используют подключение к Интернету, вам потребуется выполнить некоторые действия, иначе будет вызвано исключение.

Для существующего приложения компания Apple предлагает вам, как можно скорее, поддержку протокола `HTTPS`. Если вы не можете подключиться к веб-службе стороннего производителя, которая не поддерживает `HTTPS` или если поддержка `HTTPS` непрактична, вы можете отказаться от ATS. Дополнительные сведения см. в разделе " [отказ от ATS](#optout) " ниже.

Для нового приложения Xamarin. iOS следует использовать `HTTPS` исключительно при взаимодействии с Интернет-ресурсами. Опять же, могут возникнуть ситуации (например, использование сторонней веб-службы), где это невозможно, и вам нужно отказаться от ATS.

Кроме того, ATS обеспечивает шифрование высокоуровневого взаимодействия API с помощью TLS версии 1,2 с пересылкой. Дополнительные сведения см. в разделах [требования к подключению ATS](#ats-connection-requirements) и [совместимые с ATS шифры](#ats-compatible-ciphers) выше.

Хотя вы, возможно, не знакомы с протоколом TLS ([безопасность на транспортном уровне](https://en.wikipedia.org/wiki/Transport_Layer_Security)), он является преемником SSL ([защищенный уровень сокетов](https://en.wikipedia.org/wiki/Transport_Layer_Security)) и предоставляет набор криптографических протоколов для обеспечения безопасности сетевых подключений.

Уровень TLS контролируется используемой веб-службой и поэтому находится за пределами элемента управления приложения. Как `HttpClient`, так и `ModernHttpClient` должны автоматически использовать максимальный уровень шифрования TLS, поддерживаемый сервером.

В зависимости от сервера, на который осуществляется речь (особенно если это сторонняя служба), может потребоваться отключить пересылку или выбрать более низкий уровень TLS. Дополнительные сведения см. в разделе [Настройка параметров ATS](#configuring-ats-options) ниже.

> [!IMPORTANT]
> Безопасность транспорта приложений не применяется к приложениям Xamarin с использованием **управляемых реализаций HttpClient**. Он применяется к подключениям с использованием только **реализаций CFNetwork HttpClient** или только **реализаций NSURLSession HttpClient** .

### <a name="setting-the-httpclient-implementation"></a>Настройка реализации HTTPClient

Чтобы задать реализацию HTTPClient, используемую приложением iOS, дважды щелкните **проект** в **Обозреватель решений** , чтобы открыть **Параметры проекта**. Перейдите к элементу **сборка iOS** и выберите нужный тип клиента в раскрывающемся списке **Реализация HttpClient** :

![](ats-images/client01.png "Setting the iOS Build Options")

#### <a name="managed-handler"></a>Управляемый обработчик

Управляемый обработчик — это полностью управляемый обработчик HttpClient, который поставлялся с предыдущими версиями Xamarin. iOS и является обработчиком по умолчанию.

Преимущества.

- Это наиболее совместимо с Microsoft .NET и более старыми версиями Xamarin.

Недостатки.

- Он не полностью интегрирован с iOS (например, только TLS 1,0).
- Обычно он намного медленнее, чем собственные API.
- Он требует более управляемого кода и создает крупные приложения.

#### <a name="cfnetwork-handler"></a>Обработчик CFNetwork

Обработчик на основе CFNetwork основан на собственной платформе `CFNetwork` Framework.

Преимущества.

- Использует собственный API для повышения производительности и небольших размеров исполняемых файлов.
- Добавлена поддержка более новых стандартов, таких как TLS 1,2.

Недостатки.

- Требуется iOS 6 или более поздней версии.
- Недоступно для watchOS.
- Некоторые функции и параметры HttpClient недоступны.

#### <a name="nsurlsession-handler"></a>Обработчик NSUrlSession

Обработчик на основе NSUrlSession основан на API собственного `NSUrlSession`.

Преимущества.

- Использует собственный API для повышения производительности и небольших размеров исполняемых файлов.
- Добавлена поддержка более новых стандартов, таких как TLS 1,2.

Недостатки.

- Требуется iOS 7 или более поздней версии.
- Некоторые функции и параметры HttpClient недоступны.

## <a name="diagnosing-ats-issues"></a>Диагностика проблем ATS

При попытке подключиться к Интернету либо напрямую, либо из веб-представления в iOS 9 может появиться сообщение об ошибке в форме:

> Безопасность транспорта приложений заблокировала загрузку ресурсов HTTP с открытым текстом (`http://www.-the-blocked-domain.com`), так как она небезопасна. Временные исключения можно настроить с помощью файла INFO. plist приложения.

В iOS9 безопасность транспорта приложений (ATS) обеспечивает безопасное подключение между Интернет-ресурсами (например, серверным сервером приложения) и приложением. Кроме того, для ATS требуется обмен данными с помощью протокола `HTTPS` и высокоуровневой связи API для шифрования с помощью TLS версии 1,2 с пересылкой.

Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9 и OS X 10,11 (El Capitan), все подключения, использующие `NSURLConnection`, `CFURL` или `NSURLSession`, будут подвергаться требованиям безопасности ATS. Если подключения не соответствуют этим требованиям, они завершатся сбоем с исключением.

Apple также предоставляет [пример приложения тлстул](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) , который можно скомпилировать (или при необходимости перекодировать в Xamarin и C#) и использовать для диагностики проблем ATS/TLS. Сведения о том, как решить эту проблему, см. в разделе " [отказ от ATS](#optout) " ниже.

<a name="config" />

## <a name="configuring-ats-options"></a>Настройка параметров ATS

Можно настроить несколько функций ATS, задав значения для определенных ключей в файле **info. plist** приложения. Для управления ATS доступны следующие ключи (_с отступом, чтобы продемонстрировать, как они вложены_):

```
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Каждый ключ имеет следующий тип и имеет значение.

- **Нсапптранспортсекурити** (`Dictionary`) — содержит все ключи и значения параметра ATS.
- **Нсалловсарбитрарилоадс** (`Boolean`) — Если `YES` ATS будет отключен для любого домена, **не** указанного в `NSExceptionDomains`. Для перечисленных доменов будут использоваться указанные параметры безопасности.
- **Нсалловсарбитрарилоадсинвебконтент** (`Boolean`) — Если `YES` разрешит правильную загрузку веб-страниц, в то время как защита Apple Transport Security (ATS) по-прежнему включена для остальной части приложения.
- **Нсексцептиондомаинс** (`Dictionary`) — коллекция доменов, в которой и параметры безопасности, которые ATS должны использовать для данного домена.
- **\<домен-Name-for-Exception-in-string >** (`Dictionary`) — Коллекция исключений для данного домена (например, `www.xamarin.com`).
- **Нсексцептионминимумтлсверсион** (`String`) — Минимальная версия TLS как `TLSv1.0`, `TLSv1.1` или `TLSv1.2` (по умолчанию).
- **Нсексцептионрекуиресфорвардсекреци** (`Boolean`) — Если `NO` домену не нужно использовать шифр с прямой безопасностью. Значение по умолчанию — `YES`.
- **Нсексцептионалловсинсекурехттплоадс** (`Boolean`) — Если `NO` (по умолчанию), все связи с этим доменом должны быть в протоколе `HTTPS`.
- **Нсрекуиресцертификатетранспаренци** (`Boolean`) — если `YES` SSL домена (SSL) должен содержать допустимые данные прозрачности. Значение по умолчанию — `NO`.
- **Нсинклудессубдомаинс** (`Boolean`) — Если `YES` эти параметры переопределяют все поддомены этого домена. Значение по умолчанию — `NO`.
- **Нссирдпартексцептионминимумтлсверсион** (`String`) — версия TLS, используемая, если домен является сторонней службой за пределами элемента управления разработчика.
- **Нссирдпартексцептионрекуиресфорвардсекреци** (`Boolean`) — Если `YES` домен стороннего производителя требует пересылки секретной передачи.
- **Нссирдпартексцептионалловсинсекурехттплоадс** (`Boolean`) — Если `YES` ATS будет разрешать небезопасное взаимодействие с доменами сторонних производителей.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Отказ от ATS

Хотя компания Apple предлагает использовать протокол `HTTPS` и безопасную связь со сведениями, основанными на Интернете, в некоторых случаях это не всегда возможно. Например, если вы обмениваетесь данными с сторонней веб-службой или используете в приложении рекламу в Интернете.

Если приложение Xamarin. iOS должно выполнить запрос к незащищенному домену, следующие изменения в файле **info. plist** приложения будут отключать параметры безопасности по умолчанию, которые ATS применяют к данному домену:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

В Visual Studio для Mac дважды щелкните файл `Info.plist` в **Обозреватель решений**, перейдите в представление **исходного кода** и добавьте указанные выше ключи:

[![](ats-images/ats01.png "The Source view of the Info.plist file")](ats-images/ats01.png#lightbox)

Если приложению требуется загружать и отображать веб-содержимое с незащищенных сайтов, добавьте следующий текст в файл **info. plist** приложения, чтобы разрешить правильную загрузку веб-страниц, пока Защита транспорта Apple (ATS) по-прежнему включена для остальной части приложения:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

При необходимости можно внести следующие изменения в файл **info. plist** приложения, чтобы полностью отключить ATS для всех доменов и обмена данными через Интернет:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

В Visual Studio для Mac дважды щелкните файл `Info.plist` в **Обозреватель решений**, перейдите в представление **исходного кода** и добавьте указанные выше ключи:

[![](ats-images/ats02.png "The Source view of the Info.plist file")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> Если приложению требуется подключение к незащищенному веб-сайту, следует **всегда** вводить домен как исключение с помощью `NSExceptionDomains` вместо того, чтобы полностью ATS с помощью `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` следует использовать только в чрезвычайных ситуациях.

Опять же, отключение ATS следует использовать _только_ в качестве последнего средства, если переключение на безопасные подключения недоступно или непрактично.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье появились средства обеспечения безопасности транспорта приложений (ATS) и описывается способ обеспечения безопасной связи с Интернетом. Во-первых, мы рассмотрели изменения, которые ATS требуется для приложения Xamarin. iOS, работающего на iOS 9. Затем мы рассмотрели Управление функциями и параметрами ATS. Наконец, мы рассмотрели отказ от ATS в приложении Xamarin. iOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
