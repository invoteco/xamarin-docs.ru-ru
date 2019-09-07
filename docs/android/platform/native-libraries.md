---
title: Использование собственных библиотек
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: fad17bdda9566eeabcbe173c19c4d951bed630a7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761283"
---
# <a name="using-native-libraries"></a>Использование собственных библиотек

Xamarin. Android поддерживает использование собственных библиотек с помощью стандартного механизма PInvoke. Вы также можете объединить в APK дополнительные собственные библиотеки, которые не являются частью операционной системы.

Чтобы развернуть собственную библиотеку с приложением Xamarin. Android, добавьте в проект двоичный файл библиотеки и задайте для его **действия сборки** значение **андроиднативелибрари**.

Чтобы развернуть собственную библиотеку с проектом библиотеки Xamarin. Android, добавьте в проект двоичный файл библиотеки и задайте для его **действия сборки** значение **ембеддеднативелибрари**.

Обратите внимание, что поскольку Android поддерживает несколько двоичных интерфейсов приложений (ABI), Xamarin. Android должен иметь представление о том, для какой ABI создана собственная библиотека.
Это можно сделать двумя способами:

1. Путь "перехват"
1. Использование `AndroidNativeLibrary/Abi` элемента в файле проекта

При сканировании пути имя родительского каталога собственной библиотеки используется для указания целевого ABI библиотеки. Таким образом, если добавить `lib/armeabi/libfoo.so` в проект, то ABI будет "просканировано" как. `armeabi`

Кроме того, можно изменить файл проекта, чтобы явно указать используемый ABI:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Дополнительные сведения об использовании собственных библиотек см. в разделе [взаимодействие с собственными библиотеками](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio"></a>Отладка машинного кода в Visual Studio

Если вы используете *Visual studio 2019* или *Visual Studio 2017*, вам не нужно изменять файлы проекта, как описано выше.
Вы можете выполнять сборку и C++ отладку в решении Xamarin. Android, добавив ссылку проекта в C++ проект **динамической общей библиотеки (Android)** .

Чтобы выполнить отладку машинного C++ кода в проекте, выполните следующие действия.

1. Дважды щелкните **Свойства** проекта и выберите страницу **Параметры Android** .
2. Прокрутите вниз до пункта **параметры отладки**.
3. В раскрывающемся меню **отладчика** выберите **C++** (вместо .NET по умолчанию **(Xamarin)** ).

Разработчики Visual C++ Studio могут увидеть пример [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk) , чтобы попробовать выполнить C++ отладку из visual Studio 2019 или Visual Studio 2017 с помощью Xamarin; Дополнительные сведения см. в нашей записи [блога](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) .

## <a name="related-links"></a>Связанные ссылки

- [SanAngeles_NativeDebug (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Разработка собственных приложений Xamarin Android](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
