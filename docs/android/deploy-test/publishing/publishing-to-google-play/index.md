---
title: Публикация в Google Play
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 04e83fc68218216fe36cce67e43b83e8ad8feaa5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "80070926"
---
# <a name="publishing-to-google-play"></a>Публикация в Google Play

Хотя существует множество площадок для распространения приложений, Google Play является, пожалуй, крупнейшим и наиболее посещаемым магазином приложений Android в мире. Google Play предоставляет единую платформу для распространения, рекламирования, продажи и анализа продаж приложений Android.

В этом разделе будут рассмотрены некоторые вопросы работы с Google Play, например, как зарегистрироваться в качестве издателя и как собрать ресурсы для продвижения и рекламы приложения в Google Play. Кроме того, в ней приводятся рекомендации по оценке приложения в Google Play и использованию фильтров для ограничения развертывания приложения на определенных устройствах.

## <a name="requirements"></a>Требования

Для распространения приложения через Google Play необходимо создать учетную запись разработчика. Это нужно сделать только один раз, причем взимается оплата в размере 25 USD.

Все приложения должны быть подписаны криптографическим ключом, срок действия которого истекает 22 октября 2033 г.

Максимальный размер пакетов APK, публикуемых в Google Play, составляет 100 МБ. Если размер приложения превышает это значение, дополнительные ресурсы можно передать в Google Play посредством *файлов расширения APK*. Файлы расширения Android позволяют добавлять 2 дополнительных файла к пакету APK, каждый размером до 2 ГБ. Эти файлы размещаются и распространяются в Google Play бесплатно. Файлы расширения будут рассмотрены в другом разделе.

Магазин Google Play доступен не во всех странах. В некоторых странах распространение приложений не поддерживается.

## <a name="becoming-a-publisher"></a>Как стать издателем

Для публикации приложений в Google Play необходимо иметь учетную запись издателя. Чтобы зарегистрировать ее, выполните указанные ниже действия:

1. Откройте [Google Play Developer Console](https://play.google.com/apps/publish).
1. Введите основные сведения о себе.
1. Прочтите и примите Соглашение о распространении ПО для своего региона.
1. Оплатите регистрацию стоимостью 25 USD.
1. Подтвердите регистрацию по электронной почте.
1. После создания учетной записи можно публиковать приложения в Google Play.

Google Play поддерживается не во всех странах мира. Наиболее актуальные списки стран можно найти по следующим ссылкам:

1. [Страны, в которых можно регистрировать аккаунты разработчиков и продавцов](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; это список стран, в которых разработчики могут регистрироваться как продавцы и продавать свои приложения.

1. [Страны, в которых можно распространять приложения через Google Play](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; это список всех стран, в которых можно распространять приложения.

### <a name="preparing-promotional-assets"></a>Подготовка рекламных ресурсов

Для эффективного продвижения и рекламы приложения в Google Play разработчики могут добавлять рекламные ресурсы, такие как снимки экрана, изображения и видео. Эти ресурсы затем будут использоваться в Google Play для продвижения и рекламы приложения.

#### <a name="launcher-icons"></a>Значки запуска

*Значок запуска* — это изображение, которое представляет приложение. Каждый значок запуска должен представлять собой 32-битный файл PNG с альфа-каналом для прозрачности. Приложение должно иметь значки для всех плотностей экрана, перечисленных ниже:

- **ldpi** (120 точек на дюйм) &ndash; 36 x 36 пикселей
- **mdpi** (160 точек на дюйм) &ndash; 48 x 48 пикселей
- **hdpi** (240 точек на дюйм) &ndash; 72 x 72 пикселя
- **xhdpi** (320 точек на дюйм) &ndash; 96 x 96 пикселей

Значок запуска — это первое, что видит пользователь, просматривая приложение в Google Play, поэтому он должен быть визуально привлекательным и понятным.

Советы в отношении значков запуска:

1. **Значки запуска должны быть простыми и без лишних деталей**. Это значит, что на значке не должно быть имени приложения. Чем проще значок, тем легче его запомнить и различать в малых размерах.

1. **Линии значков не должны быть тонкими**. Слишком тонкие линии плохо видны на некоторых цветах фона.

1. **Используйте альфа-канал**. В значках должен использоваться альфа-канал, и они не должны представлять собой полнокадровые изображения.

#### <a name="high-resolution-application-icons"></a>Значки приложения в высоком разрешении

Приложение в Google Play должно иметь версию значка высокой четкости. Она используется только в Google Play и не заменяет собой значок запуска приложения. Требования к значку в высоком разрешении:

1. 32-битный файл PNG с альфа-каналом;
1. 512 x 512 пикселей;
1. максимальный размер 1024 КБ.

[Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) — это полезное средство для создания подходящих значков запуска и значков приложения в высоком разрешении.

#### <a name="screenshots"></a>Снимки экрана

Для каждого приложения в Google Play требуется от двух до восьми снимков экрана. Они отображаются на странице со сведениями о приложении в Google Play.

Требования к снимкам экрана:

1. 24-битный файл PNG или JPG без альфа-канала;
1. 320 (Ш) x 480 (В) или 480 (Ш) x 800 (В) или 480 (Ш) x 854 (В). Изображения в альбомной ориентации обрезаются.

#### <a name="promotional-graphic"></a>Рекламное изображение

Это необязательное изображение для Google Play:

1. 24-битный файл PNG или JPG без альфа-канала размером 180 x 120 пикселей;
1. без рамки.

#### <a name="feature-graphic"></a>Картинка для раздела "Рекомендуемые"

Используется в разделе "Рекомендуемые" в Google Play. Эта картинка может отображаться отдельно без значка приложения.

1. Файл PNG или JPG без альфа-канала и прозрачности размером 1024 x 500 пикселей.
1. Все важное содержимое должно размещаться в пределах области размером 924 x 500 пикселей. Пиксели за пределами этой области могут быть обрезаны в стилистических целях.
1. Масштаб этой картинки может быть уменьшен: используйте крупный текст и избегайте лишних деталей.

#### <a name="video-link"></a>Ссылка на видео

Это URL-адрес на видео в YouTube, в котором демонстрируется приложение. Длительность видео должна составлять от 30 секунд до 2 минут, и в нем должны быть показаны наиболее интересные функции приложения.

### <a name="publishing-to-google-play"></a>Публикация в Google Play

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В Xamarin Android 7.0 появился интегрированный процесс публикации приложений в Google Play из Visual Studio. Если вы используете более раннюю версию Xamarin Android, необходимо вручную отправить пакет APK через Google Play Developer Console. Кроме того, прежде чем использовать интегрированный рабочий процесс, необходимо отправить по крайней мере один пакет APK. Если вы еще не отправили ни одного пакета APK, это необходимо сделать вручную. Дополнительные сведения см. в статье [Отправка пакета APK вручную](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

В разделе [Создание сертификата](~/android/deploy-test/signing/index.md#newcert) описывается, как создать сертификат для подписания приложений Android. Следующий шаг — публикация подписанного приложения в Google Play.

1. Войдите в учетную запись разработчика Google Play, чтобы создать связанный с ней проект.
2. Создайте **клиент OAuth** для проверки подлинности приложения.
3. Введите полученные идентификатор и секрет клиента в Visual Studio.
4. Зарегистрируйте учетную запись в Visual Studio.
5. Подпишите приложение с помощью сертификата.
6. Опубликуйте подписанное приложение в Google Play.

В разделе [Архивирование для публикации](~/android/deploy-test/release-prep/index.md#archive) в диалоговом окне **Канал распространения** предлагается два варианта распространения. **AD Hoc** и **Google Play**. Если вместо этого открывается диалоговое окно **Удостоверение подписывания**, нажмите кнопку **Назад**, чтобы вернуться к диалоговому окну **Канал распространения**. Выберите **Google Play**.

[![Диалоговое окно "Канал распространения"](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

В диалоговом окне **Удостоверение подписывания** выберите удостоверение, созданное в разделе [Создание сертификата](~/android/deploy-test/signing/index.md#newcert), и нажмите кнопку **Продолжить**:

[![Диалоговое окно "Удостоверение подписывания"](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

В диалоговом окне **Учетные записи Google Play** нажмите кнопку **+** , чтобы добавить новую учетную запись Google Play:

[![Диалоговое окно "Учетные записи Google Play"](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

В диалоговом окне **Зарегистрировать доступ к Google через API** необходимо указать _идентификатор клиента_ и _секрет клиента_, которые предоставляют доступ к вашей учетной записи разработчика Google Play через API:

[![Диалоговое окно "Зарегистрировать доступ к Google через API"](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

В следующем разделе описывается, как создать проект Google API и получить требуемые _идентификатор_ и _секрет клиента_.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac имеется интегрированный процесс публикации приложений в Google Play.

В разделе [Создание сертификата](~/android/deploy-test/signing/index.md#newcert) описывается, как создать сертификат для подписывания приложений Android. Ниже приведены инструкции по публикации приложения Xamarin.Android в Google Play:

1. Войдите в учетную запись разработчика Google Play, чтобы создать связанный с ней проект.
2. Создайте _клиент OAuth_ для проверки подлинности приложения.
3. Введите полученные _идентификатор клиента_ и _секрет клиента_ в Visual Studio для Mac.
4. Зарегистрируйте учетную запись в Visual Studio для Mac.
5. Подпишите приложение с помощью сертификата.
6. Опубликуйте подписанное приложение в Google Play.

Как указано в разделе [Архивация для публикации](~/android/deploy-test/release-prep/index.md#archive), в диалоговом окне **Подписание и распространение...** предлагаются два варианта распространения. Выберите вариант **Google Play** и нажмите кнопку **Далее**:

[![Диалоговое окно выбора способа распространения для Android](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

В диалоговом окне **Учетная запись API Google Play** необходимо указать _идентификатор клиента_ и _секрет клиента_, которые предоставляют доступ к вашей учетной записи разработчика Google Play через API:

[![Диалоговое окно "Учетная запись API Google Play"](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

В следующем разделе описывается, как создать проект Google API и получить требуемые _идентификатор_ и _секрет клиента_.

-----

#### <a name="create-a-google-api-project"></a>Создание проекта Google API

Сначала войдите в [учетную запись разработчика Google Play](https://play.google.com/apps/publish).
Если у вас ее еще нет, перейдите на страницу [Запуск](https://developer.android.com/distribute/googleplay/start.html).
В статье, посвященной [началу работы](https://developers.google.com/android-publisher/getting_started) с интерфейсом API разработчика Google Play, описывается его использование. Войдя в Google Play Developer Console, выберите пункт **СОЗДАТЬ ПРИЛОЖЕНИЕ**.

[![Кнопка "Создать проект"](images/01-create-new-project-sml.png)](images/01-create-new-project.png#lightbox)

После создания нового проекта он будет связан с вашей учетной записью консоли разработчика Google Play Developer Console.

Далее необходимо создать клиент OAuth для приложения (если он еще не создан). Когда пользователи запрашивают доступ к своим личным данным с помощью вашего приложения, идентификатор клиента OAuth используется для проверки подлинности приложения.

Перейдите на страницу **Параметры**.

[![Значок "Параметры"](images/02-google-play-developer-console-sml.png)](images/02-google-play-developer-console.png#lightbox)

На странице **Параметры** выберите **Доступ к API** и щелкните **СОЗДАТЬ КЛИЕНТ OAUTH**, чтобы создать клиент OAuth.

[![Кнопка "Создать клиент OAuth"](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

Через несколько секунд будет создан идентификатор клиента. Щелкните ссылку **Показать в Google Developers Console**, чтобы просмотреть новый идентификатор клиента в Google Developer Console:

[![Идентификатор клиента](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

Идентификатор клиента отображается вместе с именем и датой создания. Щелкните значок **Изменить клиент OAuth**, чтобы просмотреть секрет клиента для приложения:

[![Просмотр учетных данных приложения](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

Имя клиента OAuth по умолчанию — *Google Play Android Developer*. Его можно поменять на имя приложения Xamarin.Android или любое другое подходящее имя. В этом примере имя клиента OAuth изменено на имя приложения **MyApp**:

[![Идентификатор и секрет клиента](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

Щелкните **Сохранить**, чтобы сохранить изменения. Вы вернетесь на страницу **Учетные данные**, где можно скачать учетные данные, щелкнув значок **Скачать JSON-файл**:

[![Значок "Скачать JSON-файл"](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

Этот JSON-файл содержит идентификатор и секрет клиента, которые можно вырезать и вставить в диалоговом окне **Подписать и распространить** в следующем шаге.

#### <a name="register-google-api-access"></a>Регистрация доступа к Google через API

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Используйте идентификатор и секрет клиента для заполнения данных в диалоговом окне **Учетная запись API Google Play** в Visual Studio для Mac. Для учетной записи можно добавить описание &ndash;, это позволяет зарегистрировать несколько учетных записей Google Play и в дальнейшем отправлять пакеты APK в разные учетные записи. Скопируйте идентификатор и секрет клиента в это диалоговое окно и нажмите кнопку **Зарегистрировать**:

[![Диалоговое окно "Зарегистрировать доступ к Google через API"](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Откроется веб-браузер, в котором будет предложено войти в учетную запись разработчика приложений Android для Google Play (если вы еще не выполнили вход). После входа в веб-браузере появится приведенный ниже запрос.
Чтобы авторизовать приложение, нажмите кнопку **Разрешить**:

[![Диалоговое окно авторизации приложения](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>Публикация

После нажатия кнопки **Разрешить** в браузере появится сообщение _Получен код проверки. Идет закрытие..._ , и приложение добавится в список учетных записей Google Play в Visual Studio. В диалоговом окне **Учетные записи Google Play** нажмите кнопку **Продолжить**:

[![Учетная запись, добавленная в список учетных записей Google Play](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

Далее появится диалоговое окно **Дорожка Google Play**. В Google Play предлагаются пять дорожек для отправки приложения:

- **Внутренняя** &ndash; используется для быстрого распространения приложения для внутреннего тестирования и проверки качества.
- **Альфа** &ndash; служит для отправки ранней версии приложения для небольшого круга тест-инженеров.
- **Бета-версия** &ndash; служит для отправки ранней версии приложения для более широкого круга тест-инженеров.
- **Рабочая** &ndash; используется для полного распространения через магазин Google Play.
- **Пользовательская** &ndash; используется для тестирования предварительных версий приложения с привлечением определенных пользователей путем создания списка адресов электронной почты тест-инженеров.

Выберите нужную дорожку Google Play для отправки приложения и нажмите кнопку **Отправить**.

[![Учетная запись, добавленная в список учетных записей Google Play](images/vs/08-google-play-track-sml.png)](images/vs/07-account-added.png#lightbox)

Дополнительные сведения о тестировании Google Play см. в разделе [Настройка открытых/закрытых/внутренних тестов](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Далее появится диалоговое окно, в котором необходимо ввести пароль сертификата для подписи.
Введите пароль и нажмите кнопку **ОК**:

[![Диалоговое окно "Пароль для подписывания"](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

В **диспетчере архивов** будет показан ход отправки:

[![Ход отправки пакета APK](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

Когда отправка завершится, в левом нижнем углу окна Visual Studio появится соответствующее сообщение:

[![Сообщение о завершении публикации проекта](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)

### <a name="troubleshooting"></a>Устранение неполадок

Если при выборе Google Play не отображается пользовательская дорожка, убедитесь, что в консоли разработчика Google Play Developer Console создан выпуск для этой дорожки. Инструкции по созданию выпуска см. в разделе [Подготовка и развертывание выпусков](https://support.google.com/googleplay/android-developer/answer/7159011?hl=en).

Обратите внимание на то, что прежде чем использовать функцию **публикации в Google Play**, необходимо отправить в Google Play по крайней мере один пакет APK. Если пакет APK еще не отправлен, в мастере публикации в области **Ошибки** появится следующее сообщение об ошибке:

[![Необходимо вручную отправить первый APK-файл для этого приложения](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

Если возникает эта ошибка, вручную отправьте пакет APK (например, специальную сборку) через Google Play Developer Console, а для обновления пакета APK в дальнейшем используйте диалоговое окно **Канал распространения**.  Дополнительные сведения см. в статье [Отправка пакета APK вручную](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Код версии пакета APK должен меняться при каждой отправке. В противном случае произойдет следующая ошибка:

[![Пакет APK с кодом версии (1) уже отправлен](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

Чтобы устранить эту ошибку, повторно выполните сборку приложения с другим номером версии и отправьте его в Google Play через диалоговое окно **Канал распространения**.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Используйте идентификатор и секрет клиента для заполнения данных в диалоговом окне **Учетная запись API Google Play** в Visual Studio для Mac. Для учетной записи можно добавить описание &ndash;, это позволяет зарегистрировать несколько учетных записей Google Play и в дальнейшем отправлять пакеты APK в разные учетные записи. Скопируйте идентификатор и секрет клиента в это диалоговое окно и нажмите кнопку **Зарегистрировать**:

[![Диалоговое окно авторизации доступа](images/xs/03-register-sml.png)](images/xs/03-register.png#lightbox)

Если идентификатор и секрет клиента приняты, появляется сообщение **Успешная регистрация**. Нажмите кнопку **Далее**:

[![Сообщение об успешной регистрации](images/xs/04-registration-successful-sml.png)](images/xs/04-registration-successful.png#lightbox)

В диалоговом окне **Учетная запись Google Play** выберите учетную запись Google и дорожку для отправки приложения:

[![Диалоговое окно выбора учетной записи Google](images/xs/05-choose-google-account-sml.png)](images/xs/05-choose-google-account.png#lightbox)

В Google Play предлагаются пять дорожек для отправки приложения:

- **Внутренняя** &ndash; используется для быстрого распространения приложения для внутреннего тестирования и проверки качества.
- **Альфа** &ndash; служит для отправки ранней версии приложения для небольшого круга тест-инженеров.
- **Бета-версия** &ndash; служит для отправки ранней версии приложения для более широкого круга тест-инженеров.
- **Рабочая** &ndash; используется для полного распространения через магазин Google Play.
- **Пользовательская** &ndash; используется для тестирования предварительных версий приложения с привлечением определенных пользователей путем создания списка адресов электронной почты тест-инженеров.

Дополнительные сведения о тестировании в Google Play см. в статье [Как провести альфа- и бета-тестирование](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Далее выберите удостоверение подписывания, которое будет использоваться для подписывания приложения.
Чтобы использовать существующее удостоверение подписывания, выберите вариант **Использовать существующий ключ**. В противном случае обратитесь к руководству [Создание сертификата](~/android/deploy-test/signing/index.md#newcert) за сведениями о создании ключа. Выбрав сертификат для подписывания приложения, нажмите кнопку **Далее**:

[![Диалоговое окно "Удостоверение подписывания Android"](images/xs/06-android-signing-identity-sml.png)](images/xs/06-android-signing-identity.png#lightbox)

Теперь приложение можно отправить в Google Play. В диалоговом окне **Публикация в Google Play** приводятся сводные сведения о приложении. Чтобы опубликовать приложение в Google Play, нажмите кнопку **Опубликовать**:

[![Диалоговое окно "Публикация в Google Play"](images/xs/07-publish-to-google-play-sml.png)](images/xs/07-publish-to-google-play.png#lightbox)

### <a name="troubleshooting"></a>Устранение неполадок

Если при выборе Google Play не отображается пользовательская дорожка для отправки приложения, убедитесь, что в консоли разработчика Google Play Developer Console создан выпуск для этой дорожки. Инструкции по созданию выпуска см. в разделе [Подготовка и развертывание выпусков](https://support.google.com/googleplay/android-developer/answer/7159011?hl=en).

Обратите внимание на то, что прежде чем использовать функцию **публикации в Google Play**, необходимо отправить в Google Play по крайней мере один пакет APK. Если пакет APK не был отправлен, может произойти следующая ошибка:

> _Google Play требует, чтобы вы вручную отправили первый APK-файл для этого приложения. Для этого можно использовать специальный APK-файл._

or

> _Не найдено приложение с указанным именем пакета. [404]_

Чтобы устранить эту ошибку, вручную отправьте пакет APK (например, специальную сборку) через Google Play Developer Console, а для обновления пакета APK в дальнейшем используйте диалоговое окно **Публикация в Google Play**. Сведения об отправке пакета APK вручную см. в статье [Отправка пакета APK вручную](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
