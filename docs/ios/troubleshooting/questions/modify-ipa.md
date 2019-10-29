---
title: Можно ли добавлять или удалять файлы из файла IPA после его создания в Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: f63cc544b251ca284a8d087e09f9cbc4dfb3f769
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030958"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Можно ли добавлять или удалять файлы из файла IPA после его создания в Visual Studio?

Да, это возможно, но обычно потребуется повторно подписать `.app` пакет после внесения изменений.

Обратите внимание, что изменение файла `.ipa` не является обязательным при нормальном использовании. Эта статья предоставляется исключительно в информационных целях.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Пример. Удаление файла из архива `.ipa`

В этом примере предполагается, что имя проекта Xamarin. iOS `iPhoneApp1`, а `generated session id` — `cc530d20d6b19da63f6f1c6f67a0a254`

1. Создайте файл `.ipa` в формате обычного из Visual Studio.

2. Переключитесь на узел сборки Mac.

3. Найдите сборку в папке `~/Library/Caches/Xamarin/mtbs/builds`. Этот путь можно вставить в **finder > go > перейти к папке** , чтобы просмотреть папку в Finder. Найдите папку, совпадающую с именем проекта. В этой папке найдите папку, соответствующую `generated session id` сборки. Скорее всего, это вложенная папка с последним временем изменения.

4. Откройте новое окно `Terminal.app`.

5. Введите `cd` в окно Terminal. app, а затем перетащите & папку `generated session id` в окно `Terminal.app`:

    ![](modify-ipa-images/session-id-folder.png "Locating the generated session id folder in Finder")

6. Введите ключ возврата, чтобы изменить каталог в папку `generated session id`.

7. Распакуйте файл `.ipa` во временную папку `old/` с помощью следующей команды. При необходимости измените имена `Ad-Hoc` и `iPhoneApp1` для конкретного проекта.

    > Дитто-КСК bin/iPhone/ад-хок/iPhoneApp1-1.0. IPA Old/

8. Не закрывайте окно `Terminal.app`.

9. Удалите нужные файлы из `.ipa`. Можно либо переместить их в корзину с помощью средства поиска, либо удалить их из командной строки с помощью `Terminal.app`. Чтобы просмотреть содержимое файла `Payload/iPhone` в Finder, щелкните его и выберите **Показать содержимое пакета**.

10. Используя тот же общий подход, что и на шаге 3, найдите файл журнала в разделе `~/Library/Logs/Xamarin/MonoTouchVS/`, в имени которого есть имя проекта и `generated session id`.![](modify-ipa-images/build-log.png "Поиск журнала сборки проекта в Finder")

11. Откройте журнал сборки из шага 10, например дважды щелкнув его.

12. Найдите строку, содержащую `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13. Введите `/usr/bin/codesign` в окно Terminal. app из шага 8.

14. Скопируйте все аргументы, начинающиеся с `-v`, из строки, приведенной в шаге 12, и вставьте их в окно Terminal. app.

15. Измените последний аргумент на `.app` пакет, расположенный в папке `old/Payload/`, а затем выполните команду.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. Перейдите в каталог `old/` в окне терминала:

    ```bash
    cd old
    ```

17. Заархивируйте содержимое каталога в новый файл `.ipa` с помощью команды `zip`. Можно изменить аргумент `"$HOME/Desktop/iPhoneApp1-1.0.ipa"`, чтобы вывести `.ipa` файл в любом месте:

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>Распространенные сообщения об ошибках

Если вы видите `Invalid Signature. A sealed resource is missing or invalid.`, это означает, что что-то изменилось в `.app`ом пакете и что `.app` пакет был неправильно подписан. Также обратите внимание, что если вы хотите создать `.ipa` с профилем распространения, _необходимо_ создать исходный `.ipa` с профилем распространения. В противном случае `Entitlements.xcent` будет неверным.

Чтобы получить конкретный пример того, как может возникнуть Эта ошибка, при выполнении следующей команды `codesign --verify` в окне терминала после шага 9 вы увидите ошибку и точную причину ошибки:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

И процесс проверки магазина приложений сообщит о похожем сообщении об ошибке:

> Ошибка ИТМС-90035: "Недопустимая подпись. Запечатанный ресурс отсутствует или является недопустимым. Двоичный файл в пути [iPhoneApp1. app/iPhoneApp1] содержит недопустимую сигнатуру. Убедитесь, что приложение подписано сертификатом распространения, а не прямым сертификатом или сертификатом разработки. Убедитесь, что параметры подписывания кода в Xcode верны на целевом уровне (что переопределяет любые значения на уровне проекта). Кроме того, убедитесь, что пакет, который вы отправляете, был создан с помощью целевого объекта выпуска в Xcode, а не цели симулятора. Если вы уверены, что параметры подписывания кода верны, выберите "очистить все" в Xcode, удалите каталог "Build" в Finder и перестройте цель выпуска. Дополнительные сведения см. в [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
