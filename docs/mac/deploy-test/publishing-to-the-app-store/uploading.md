---
title: Отправка в Mac App Store
description: Этот документ описывает использование iTunes Connect для отправки приложения Xamarin.Mac в Mac App Store. Он содержит сведения, необходимые iTunes Connect для завершения этого процесса.
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fede666071aad8702eda17a94a06c7de49c7397e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029477"
---
# <a name="upload-to-mac-app-store"></a>Отправка в Mac App Store

_В этом руководстве описано, как отправить приложение Xamarin.Mac для публикации в Mac App Store._

Приложения отправляются на утверждение команде Mac App Store через [iTunes Connect](https://itunesconnect.apple.com/).

1. Выберите **приложение macOS**, которое требуется создать: 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. Введите имя приложения и укажите другие сведения. Можно выбрать только значения существующего параметра **Идентификатор пакета**, который был создан ранее: 

    [![](uploading-images/image66.png "Selecting the bundle ID")](uploading-images/image66.png#lightbox)

3. Выберите дату доступности и цену. Независимо от выбранной даты приложение станет доступно для продажи только после его утверждения. Если вам требуется больший контроль над фактической датой доступности, это значение можно задать далеко в будущем. 

    [![](uploading-images/image67.png "Setting the available date and price")](uploading-images/image67.png#lightbox)

4. Введите сведения о приложении, включая категорию Магазина приложений, в которую оно входит. 

    [![](uploading-images/image68.png "Entering the app information")](uploading-images/image68.png#lightbox) 

    Выберите применимые оценки: 

    [![](uploading-images/image69.png "Setting the app ratings")](uploading-images/image69.png#lightbox) 

    Описание, ключевые слова и контактные URL-адреса: 

    [![](uploading-images/image70.png "Editing the Description, keywords and contact URLs")](uploading-images/image70.png#lightbox) 

    Контактные данные и информация для рецензентов Магазина приложений: 

    [![](uploading-images/image71.png "Editing the contact information and advice for the App Store reviewers")](uploading-images/image71.png#lightbox) 

    И, наконец, снимки экранов: 

    [![](uploading-images/image72.png "Adding the required screenshots")](uploading-images/image72.png#lightbox) 

    Снимки экрана должны иметь формат JPG, TIF или PNG и размер 1280 x 800, 1440 x 900, 2880 x 1800 или 2560 x 1600 пикселей. Нажмите клавишу **Сохранить** для завершения.

5. Сведения о приложении доступны для просмотра. Нажмите кнопку **Показать сведения**, чтобы изменить состояние: 

    [![](uploading-images/image73.png "Viewing the app details")](uploading-images/image73.png#lightbox)

6. В представлении сведений нажмите кнопку "Ready to Upload Binary" (Готово для отправки двоичного файла), чтобы отправить файл пакета приложения: 

    [![](uploading-images/image74.png "Selecting Ready to Upload Binary")](uploading-images/image74.png#lightbox)

7. Ответьте на вопрос шифрования: 

    [![](uploading-images/image75.png "Answering the cryptography question")](uploading-images/image75.png#lightbox)

8. Появится сообщение о том, когда сайт будет готов принять файл пакета приложения: 

    [![](uploading-images/image76.png "The acceptance notification")](uploading-images/image76.png#lightbox)

9. Запустите загрузчик приложений и выполните вход с помощью идентификатора Apple ID.
Чтобы продолжить, нажмите кнопку **Deliver Your App** (Доставить приложение): 

    [![](uploading-images/image77.png "The Application Loader interface")](uploading-images/image77.png#lightbox)

10. Выберите приложение в списке приложений со статусом **Ready to Upload Binary** (Готово для отправки двоичного файла) и нажмите кнопку **Далее**: 

    [![](uploading-images/image78.png "Selecting the app to load")](uploading-images/image78.png#lightbox)

11. Проверьте метаданные приложения и нажмите кнопку **Выбрать**, чтобы найти файл пакета: 

    [![](uploading-images/image79.png "Reviewing the app metadata")](uploading-images/image79.png#lightbox)

12. Найдите файл пакета, который был создан в Visual Studio для Mac с помощью конфигурации сборки Магазина приложений: 

    [![](uploading-images/image80.png "Selecting the file to upload")](uploading-images/image80.png#lightbox)

13. Нажмите кнопку **Отправить**: 

    [![](uploading-images/image81.png "Sending the app")](uploading-images/image81.png#lightbox)

14. Пакет будет проверен, после чего будут зафиксированы обнаруженные ошибки. Исправьте эти ошибки и повторно отправьте пакет. Отправленное приложение будет автоматически передано на проверку команде App Store: 

    [![](uploading-images/image82.png "An example of upload errors")](uploading-images/image82.png#lightbox)

После утверждения приложение будет доступно для скачивания или приобретения в Mac App Store.

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~//mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)
