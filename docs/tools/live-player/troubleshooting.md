---
title: Устранение неполадок Xamarin Live Player
description: В этом документе описаны известные проблемы с Xamarin Live Player и возможные решения. В нем описывается, что проблемы с подключением, проблемы с конфигурацией и многое другое.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: bf0186b55b14d9797397b98390f4d825d669d0f4
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157692"
---
# <a name="troubleshooting-xamarin-live-player"></a>Устранение неполадок Xamarin Live Player

![Предварительная версия](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Player Предварительный просмотр завершен. Приложение больше не доступно. Приведенные ниже инструкции предоставляются для пользователей использовать предварительную версию с помощью Visual Studio 2017...

> [!TIP]
> Можно использовать [средства предварительного просмотра XAML](~/xamarin-forms/xaml/xaml-previewer/index.md) в 2019 г. Visual Studio или Visual Studio для Mac, чтобы просмотреть экран проектов при их изменении.

В этой статье описываются ограничения Live Player и некоторых распространенных проблем с действия по их устранению.

## <a name="limitations-of-xamarin-live-player"></a>Ограничения Xamarin Live Player

### <a name="ide-requirements"></a>Требования к интегрированной среде разработки

Предварительный просмотр проигрыватель доступен только в Visual Studio 2017.

### <a name="device-requirements"></a>Требования к устройствам

Приложение Xamarin Live Player поддерживает следующим устройствам Android:

- Android 4.2 или более поздней версии.
- ARM-v7a, ARM v8a, ARM64-v8a, x 86, x86_64 процессор или.

### <a name="ios-limitations"></a>ограничения операций ввода-вывода

Live Player недоступна для iOS.

### <a name="xamarinforms-limitations"></a>Ограничения Xamarin.Forms

- Пользовательские модули подготовки отчетов не поддерживаются.
- Эффекты не поддерживаются.
- Пользовательские элементы управления с помощью пользовательских привязываемые свойства не поддерживаются.
- Внедренные ресурсы не поддерживаются (т. е. внедрение изображений или других ресурсов в переносимую библиотеку Классов).
- Сторонних инфраструктур MVVM не поддерживаются (т. е. Prism, Mvvm Cross, Mvvm Light и т. д.).

### <a name="other-project-type-limitations"></a>Другие ограничения типа проекта

- Live Player не предназначен для собственных проектов Android (использующие Android XML для пользовательского интерфейса).

### <a name="miscellaneous-limitations"></a>Прочие ограничения

- Ограниченная поддержка для отражения (в настоящее время влияет на некоторые популярные пакеты NuGet, например SQLite и Json.NET). Возможно, другие пакеты NuGet по-прежнему поддерживается.
- Некоторые системные классы не могут быть переопределены (например, нельзя реализовать подкласс).
- Некоторые функции платформы, требующих подготовки не может работать в приложении Xamarin Live Player (тем не менее она настроена для выполнения распространенных операций, как доступ к коллекции фотографий).
- Пользовательские целевые объекты и этапы построения игнорируются. Например средств, таких как Fody, программу Refit, AutoFac и AutoMapper не может быть включен.
- F#проекты не поддерживаются
- Расширенные сценарии с пользовательских универсальные классы и интерфейсы могут не поддерживаться.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Мобильное устройство не подключается после сканирования штрихкода (или написания кода)

Ошибка возникает в случае запуска Xamarin Live Player мобильное устройство в той же сети, что и компьютер под управлением интегрированной среды разработки. Просмотрите следующую информацию:

- Убедитесь, что компьютер и устройство в той же сети Wi-Fi.
  - Если компьютер также подключен к проводной сети, попробуйте отключить проводного подключения.
- Сети тесно защищены (например, некоторые корпоративной сети), блокирует порты, необходимые для Xamarin Live Player.
- Закройте приложение Xamarin Live Player и перезапустите его.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>Сообщение «Ошибка при попытке развернуть» в интегрированной среде разработки

**«IOException: не удалось прочитать данные из транспортного соединения: Операция на незаблокированном сокете может вызвать блокировку»**

Эта ошибка часто возникают, когда мобильное устройство под управлением Xamarin Live Player не в той же сети, что и компьютер под управлением Visual Studio. Это часто происходит при подключении к устройству, которое ранее было успешно связано.

* Проверьте, что устройство и компьютер находятся в той же сети Wi-Fi.
* Сети тесно защищены (например, некоторые корпоративной сети), блокирует порты, необходимые для Xamarin Live Player. Следующие порты являются обязательными для Xamarin Live Player:
  * 37847 — доступ к внутренней сети 
  * 8090 — доступ к внешние сети

## <a name="manually-configure-device"></a>Вручную настроить устройство

Если вы не можете подключиться к устройству по Wi-Fi можно попытаться вручную настроить устройство с помощью файла конфигурации, выполнив следующие действия:

**Шаг 1. Открыть файл конфигурации**

Перейдите к папке данных приложения:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

В этой папке вы найдете **PlayerDeviceList.xml** Если существует необходимо будет создать его.

**Шаг 2. Получить IP-адрес**

В приложение Xamarin Live Player, перейдите в раздел **о > Проверка подключения > Начать тест подключения**.

Запишите IP-адреса, вам потребуется IP-адрес при настройке устройства.

**Шаг 3. Получить кодом связывания**

В Xamarin Live Player tap **пары** или **пары снова**, нажмите клавишу **ввести вручную**. Числовой код отображается, который необходимо обновить файл конфигурации.

**Шаг 4. Создать GUID**

Перейдите к: https://www.guidgenerator.com/online-guid-generator.aspx и создать новый guid и убедитесь, что включен верхний регистр.

**Шаг 5. Настройка устройства**

Откройте **PlayerDeviceList.xml** вверх в редакторе, например Visual Studio или Visual Studio Code. Необходимо вручную настроить устройства в этом файле. По умолчанию этот файл должен содержать следующий пустой `Devices` XML-элемент:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Добавьте устройство Android:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Закройте и снова откройте Visual Studio.** Устройство должно отображаться в списке.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>«Тип или пространство имен не может найти» сообщения в интегрированной среде разработки

Убедитесь, что вы выбрали **запускаемым проектом** , соответствующую типу вашего устройства (например) Android) и что конфигурация соответствует типа устройства (например) **Отладка** для Android).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Сообщение «Конструктор «InterpretedXamarin.Forms.Button» не найден» в проигрывателе

Некоторые системные классы не могут быть переопределены, например:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>«MainActivity.cs: «Resource.Layout» не содержит определение для «Main»»

Эта ошибка возникает для проектов Android с пользовательскими интерфейсами, определенные в AXML-файлах.
Файлы AXML в Xamarin Live Player в настоящее время не поддерживаются.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android панель инструментов и вкладках отображаются неправильно Xamarin.Forms

Проекты Xamarin.Forms Android необходимо использовать «Toolbar.axml» и «Tabbar.axml» для имен файлов, соответствующих макета. Шаблон по умолчанию использует эти имена; Переименование вызовет проблемы отрисовки.

## <a name="related-links"></a>Связанные ссылки

- [Установка](~/tools/live-player/install.md)
