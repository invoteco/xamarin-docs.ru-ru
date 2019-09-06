---
title: Можно ли добавлять или удалять файлы из файла IPA после его создания в Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: d1546b83304d8c66f7433bd77c5ebecc9dc95aaa
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278470"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Можно ли добавлять или удалять файлы из файла IPA после его создания в Visual Studio?

Да, это возможно, но обычно потребуется повторно подписать `.app` пакет после внесения изменений.

Обратите внимание, `.ipa` что изменение файла не является обязательным при нормальном использовании. Эта статья предоставляется исключительно в информационных целях.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Пример. Удаление файла из `.ipa` архива

В этом примере предполагается, что имя проекта Xamarin. iOS имеет `iPhoneApp1` значение `generated session id` , а —`cc530d20d6b19da63f6f1c6f67a0a254`

1. `.ipa` Создайте файл обычным образом из Visual Studio.

2. Переключитесь на узел сборки Mac.

3. Найдите сборку в `~/Library/Caches/Xamarin/mtbs/builds` папке. Этот путь можно вставить в **finder > go > перейти к папке** , чтобы просмотреть папку в Finder. Найдите папку, совпадающую с именем проекта. В этой папке найдите папку, совпадающую с `generated session id` версией сборки. Скорее всего, это вложенная папка с последним временем изменения.

4. Откройте новое `Terminal.app` окно.

5. Введите `cd` в окно Terminal. app, а затем перетащите & `generated session id` папку в `Terminal.app` окно.

    ![](modify-ipa-images/session-id-folder.png "Поиск созданной папки идентификатора сеанса в Finder")

6. Введите ключ возврата, чтобы изменить каталог в `generated session id` папке.

7. `old/` Распакуйте `.ipa` файл во временную папку с помощью следующей команды. При необходимости измените `iPhoneApp1` имена идляконкретногопроекта.`Ad-Hoc`

    > Дитто-КСК bin/iPhone/ад-хок/iPhoneApp1-1.0. IPA Old/

8. Не закрывайте `Terminal.app` окно.

9. Удалите нужные файлы из `.ipa`. Можно либо переместить их в корзину с помощью средства поиска, либо удалить их из командной строки с помощью `Terminal.app`команды. Чтобы просмотреть содержимое `Payload/iPhone` файла в Finder, щелкните его и выберите **Показать содержимое пакета**.

10. Используя тот же общий подход, что и на шаге 3, найдите файл `~/Library/Logs/Xamarin/MonoTouchVS/` журнала, в котором есть имя проекта `generated session id` и в имени: ![](modify-ipa-images/build-log.png "Поиск журнала сборки проекта в Finder")

11. Откройте журнал сборки из шага 10, например дважды щелкнув его.

12. Найдите строку, содержащую `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13. Введите `/usr/bin/codesign` в окно Terminal. app из шага 8.

14. Скопируйте все аргументы, начинающиеся с `-v` , из строки на шаге 12 и вставьте их в окно Terminal. app.

15. Измените последний аргумент на `.app` пакет, расположенный `old/Payload/` в папке, а затем выполните команду.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. Перейдите в каталог терминала: `old/`

    ```bash
    cd old
    ```

17. Заархивируйте содержимое каталога в новый `.ipa` файл `zip` с помощью команды. Можно изменить `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` аргумент, чтобы `.ipa` вывести файл в любом месте:

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>Распространенные сообщения об ошибках

Если вы видите `Invalid Signature. A sealed resource is missing or invalid.`, это означает, что что-то изменилось `.app` в `.app` пакете и что пакет был неправильно подписан. Также обратите внимание, что если вы хотите `.ipa` создать с помощью профиля распространения, _необходимо_ создать исходный `.ipa` профиль с профилем распространения. В противном случае — будетнеправильным.`Entitlements.xcent`

Чтобы получить конкретный пример того, как может возникнуть Эта ошибка, при выполнении следующей `codesign --verify` команды в окне терминала после шага 9 вы увидите ошибку и точную причину ошибки:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

И процесс проверки магазина приложений сообщит о похожем сообщении об ошибке:

> ОШИБКА ИТМС-90035: "Недопустимая подпись. Запечатанный ресурс отсутствует или является недопустимым. Двоичный файл в пути [iPhoneApp1. app/iPhoneApp1] содержит недопустимую сигнатуру. Убедитесь, что приложение подписано сертификатом распространения, а не прямым сертификатом или сертификатом разработки. Убедитесь, что параметры подписывания кода в Xcode верны на целевом уровне (что переопределяет любые значения на уровне проекта). Кроме того, убедитесь, что пакет, который вы отправляете, был создан с помощью целевого объекта выпуска в Xcode, а не цели симулятора. Если вы уверены, что параметры подписывания кода верны, выберите "очистить все" в Xcode, удалите каталог "Build" в Finder и перестройте цель выпуска. Дополнительные сведения см. [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)в "
