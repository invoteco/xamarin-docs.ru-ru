---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Использование C/C++ Library с Xamarin
description: Visual Studio для Mac можно использовать для создания и интеграции кросс-платформенного кода CC++ /Code в мобильные приложения для Android и iOS с помощью Xamarin C#и. В этой статье объясняется, как настроить и отладить C++ проект в приложении Xamarin.
author: mikeparker104
ms.author: miparker
ms.date: 11/07/2019
ms.openlocfilehash: 42a59570d727657b2f3c23bd9d1f37e1205717d0
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842821"
---
# <a name="use-cc-libraries-with-xamarin"></a>Использование C/C++ Library с Xamarin

## <a name="overview"></a>Обзор

Xamarin позволяет разработчикам создавать кросс-платформенные собственные мобильные приложения с помощью Visual Studio. Как правило C# , привязки используются для предоставления разработчикам существующих компонентов платформы. Однако бывают случаи, когда приложениям Xamarin требуется работать с существующими базой кода. Иногда команды просто не имеют времени, бюджета или ресурсов для передачи больших, хорошо протестированных и оптимизированных баз кода C#.

[Visual C++ для кросс-платформенной разработки мобильных приложений](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) позволяет создаватьC++ код C# C/и в рамках одного решения, предлагая множество преимуществ, включая унифицированный процесс отладки. Корпорация Майкрософт использовала CC++ /и Xamarin для доставки таких приложений, как [сотовый телефон](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) и [PIX Camera](https://www.microsoft.com/microsoftpix).

Однако в некоторых случаях существует желание (или требование) для сохранения существующих C/C++ средств и процессов и сохранения кода библиотеки, отделенного от приложения, и рассматривая библиотеку как похожую на сторонние компоненты. В таких ситуациях задача не только предоставляет доступ к C# соответствующим элементам, но и управление библиотекой как зависимостью. И, конечно, автоматизируйте как можно большую часть этого процесса.  

В этой публикации рассматривается высокоуровневый подход к этому сценарию и демонстрируется простой пример.

## <a name="background"></a>Фон

В языкеC++ C/считается кросс-платформенный язык, но необходимо принять во внимание, что исходный код действительно является межплатформенным, используя только C/C++ поддерживается всеми целевыми компиляторами и содержащим небольшую или не имеющую условную, включенную в код платформу или зависящий от компилятора.

В конечном итоге код должен успешно компилироваться и выполняться на всех целевых платформах, поэтому это сводится к унификации между платформами (и компиляторами), для которых нацелена цель. Проблемы могут быть вызваны незначительными различиями между компиляторами, поэтому тщательное тестирование (желательно автоматически) на каждой целевой платформе становится все более важным.  

## <a name="high-level-approach"></a>Высокоуровневый подход

На рисунке ниже представлен подход с четырьмя этапами, используемый для преобразования кодаC++ C/source в кросс-платформенную библиотеку Xamarin, которая совместно используется в NuGet, а затем используется в приложении Xamarin. Forms.

![Высокоуровневый подход к использованию C/C++ с Xamarin](images/cpp-steps.jpg)

4 этапа:

1. Компиляция кода C/C++ Source в машинные библиотеки, зависящие от платформы.
2. Создание оболочки для собственных библиотек с помощью решения Visual Studio.
3. Упаковка и отправка пакета NuGet для оболочки .NET.
4. Использование пакета NuGet из приложения Xamarin.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>Этап 1. Компиляция кода C/C++ Source в машинные библиотеки для конкретных платформ

Цель этого этапа — создать собственные библиотеки, которые могут вызываться C# оболочкой. В зависимости от конкретной ситуации это может быть неуместным. Многие средства и процессы, которые могут быть включены в этот распространенный сценарий, выходят за рамки этой статьи. Ключевыми соображениями является обеспечение синхронизацииC++ C/CodeBase с любым собственным кодом оболочки, достаточным модульным тестированием и автоматизацией сборки. 

Библиотеки в пошаговом руководстве были созданы с помощью Visual Studio Code с сопутствующим сценарием оболочки. Расширенная версия этого пошагового руководства находится в [репозитории GitHub для Mobile Cat](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin) , в котором подробно рассматривается эта часть примера. В этом случае машинные библиотеки обрабатываются как зависимость от сторонних производителей, однако этот этап проиллюстрирован для контекста.

Для простоты в этом пошаговом руководстве предназначено только подмножество архитектур. Для iOS используется служебная программа липо для создания одного двоичного файла FAT из отдельных двоичных файлов конкретной архитектуры. Android будет использовать динамические двоичные файлы с расширением. Поэтому расширение и iOS будут использовать статический двоичный файл FAT с расширением. a. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>Этап 2. Создание оболочки для собственных библиотек с помощью решения Visual Studio

Следующий этап заключается в заключении в оболочку собственных библиотек, чтобы их можно было легко использовать из .NET. Это делается с помощью решения Visual Studio с четырьмя проектами. В общем проекте содержится общий код. Проекты, предназначенные для каждого из Xamarin. Android, Xamarin. iOS и .NET Standard позволяют ссылаться на библиотеку независимым от платформы образом.

Программа-оболочка использует[Bait и хитрость](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/), описываемые Пол Беттс. Это не единственный способ, но он упрощает ссылку на библиотеку и позволяет избежать необходимости явно управлять реализациями, зависящими от платформы, в самом приложении. Этот прием гарантирует, что целевые объекты (.NET Standard, Android, iOS) совместно используют одно и то же пространство имен, имя сборки и структуру класса. Так как NuGet всегда предпочитает библиотеку для конкретной платформы, .NET Standardная версия никогда не будет использоваться во время выполнения.

Большая часть работы на этом шаге посвящена использованию P/Invoke для вызова собственных методов библиотеки и управления ссылками на базовые объекты. Задача заключается в предоставлении функциональных возможностей библиотеки потребителю при абстракции любой сложности. Разработчикам Xamarin. Forms не нужно иметь опыт работы с внутренними обработкой неуправляемой библиотеки. Это должно быть похоже на использование любой другой управляемой C# библиотеки.

В конечном итоге, выходные данные этого этапа представляют собой набор библиотек .NET, по одному на целевой объект, а также документ nuspec, содержащий сведения, необходимые для сборки пакета на следующем шаге.

**Этап 3. Упаковка и отправка пакета NuGet для оболочки .NET**

Третий этап — создание пакета NuGet с помощью артефактов сборки из предыдущего шага. Результатом этого шага является пакет NuGet, который можно использовать из приложения Xamarin. В этом пошаговом руководстве используется локальный каталог, служащий веб-каналом NuGet. В рабочей среде этот шаг должен публиковать пакет в общедоступном или частном веб-канале NuGet и должен быть полностью автоматизирован.

**Этап 4. Использование пакета NuGet из приложения Xamarin. Forms**

Последним шагом является ссылка на пакет NuGet и его использование из приложения Xamarin. Forms. Для этого необходимо настроить канал NuGet в Visual Studio для использования канала, определенного на предыдущем шаге.

После настройки веб-канала на него необходимо ссылаться из каждого проекта в приложении Xamarin. Forms в разных платформах. ' Bait-and-Switch ' предоставляет идентичные интерфейсы, поэтому функциональность собственной библиотеки можно вызывать с помощью кода, определенного в одном месте.

Репозиторий исходного кода содержит [список дополнительных](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up) материалов, в которых содержатся статьи о настройке частного канала NuGet в Azure DevOps и о том, как отправить пакет в этот веб-канал. Хотя при настройке локального каталога потребуется немного больше времени, этот тип веб-канала лучше использовать в среде коллективной разработки.

## <a name="walk-through"></a>Пошаговое руководство

Указанные действия относятся к **Visual Studio для Mac**, но структура работает также в **Visual Studio 2017** .

### <a name="prerequisites"></a>Необходимые компоненты

Чтобы отслеживать работу, разработчику потребуется:

- [Командная строка NuGet (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Visual Studio* *для Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> Для развертывания приложений на устройстве iPhone требуется активная [**учетная запись разработчика Apple**](https://developer.apple.com/) .

## <a name="creating-the-native-libraries-stage-1"></a>Создание собственных библиотек (этап 1)

Функциональные возможности собственной библиотеки основаны на примере из [пошагового руководства: создание и использование статической библиотекиC++()](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017).

В этом пошаговом руководстве пропускается первый этап — создание собственных библиотек, так как в этом сценарии библиотека предоставляется как зависимость от стороннего разработчика. Предкомпилированные машинные библиотеки включены вместе с [образцом кода](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin) или могут быть [загружены](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts) напрямую.

### <a name="working-with-the-native-library"></a>Работа с собственной библиотекой

Исходный пример *MathFuncsLib* включает один класс с именем `MyMathFuncs` со следующим определением:

```cpp
namespace MathFuncs
{
    class MyMathFuncs
    {
    public:
        double Add(double a, double b);
        double Subtract(double a, double b);
        double Multiply(double a, double b);
        double Divide(double a, double b);
    };
}
```

Дополнительный класс определяет функции-оболочки, позволяющие потребителю .NET создавать, удалять и взаимодействовать с базовым собственным классом `MyMathFuncs`.

```cpp
#include "MyMathFuncs.h"
using namespace MathFuncs;

extern "C" {
    MyMathFuncs* CreateMyMathFuncsClass();
    void DisposeMyMathFuncsClass(MyMathFuncs* ptr);
    double MyMathFuncsAdd(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsSubtract(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsMultiply(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsDivide(MyMathFuncs *ptr, double a, double b);
}
```

Эти функции-оболочки будут использоваться на стороне [Xamarin](https://visualstudio.microsoft.com/xamarin/) .

## <a name="wrapping-the-native-library-stage-2"></a>Создание оболочки для собственной библиотеки (этап 2)

Для этого этапа требуются [предварительно скомпилированные библиотеки](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts) , описанные в [предыдущем разделе](#creating-the-native-libraries-stage-1).

### <a name="creating-the-visual-studio-solution"></a>Создание решения Visual Studio

1. В **Visual Studio для Mac**щелкните **создать проект** (на *странице приветствия*) или **новое решение** (в меню *файл* ).
2. В окне **Новый проект** выберите **общий проект** (из *многоплатформенной > Библиотека*), а затем нажмите кнопку **Далее**.
3. Обновите следующие поля и нажмите кнопку **создать**.

    - **Имя проекта:** Масфункс. Shared  
    - **Имя решения:** масфункс  
    - **Расположение:** Использовать расположение для сохранения по умолчанию (или выбрать альтернативу)   
    - **Создайте проект в каталоге решения:** Установить значение "Проверено"
4. В **Обозреватель решений**дважды щелкните проект **масфункс. Shared** и перейдите к разделу **Основные параметры**.
5. Удалить **. Предоставлен общий доступ** из **пространства имен по умолчанию** , поэтому он имеет значение **масфункс** , а затем нажмите кнопку **ОК**.
6. Откройте **MyClass.CS** (созданный шаблоном), а затем переименуйте класс и имя файла в **мимасфунксвраппер** и измените пространство имен на **масфункс**.
7. **Щелкните** решение **масфункс**, а затем выберите **Добавить новый проект...** в меню **Добавить** .
8. В окне **Новый проект** выберите **.NET Standard библиотека** (в *многоплатформенной библиотеке >* ) и нажмите кнопку **Далее**.
9. Выберите **.NET Standard 2,0** и нажмите кнопку **Далее**.
10. Обновите следующие поля и нажмите кнопку **создать**.

    - **Имя проекта:** Масфункс. Standard  
    - **Расположение:** Использовать то же расположение для сохранения, что и для общего проекта   

11. В **Обозреватель решений**дважды щелкните проект **масфункс. Standard** .
12. Перейдите к **главному параметрам**, а затем обновите **пространство имен по умолчанию** на **масфункс**.
13. Перейдите к параметрам **вывода** , а затем обновите **имя сборки** на **масфункс**.
14. Перейдите к параметрам **компилятора** , измените **конфигурацию** на **выпуск**, установив **отладочную информацию** в **SYMBOLS** , а затем нажмите кнопку **ОК**.
15. Удаление **Class1.CS/Getting, запущенного** из проекта (если один из них был включен в состав шаблона).
16. **Нажмите кнопку "Управление** " и щелкните папку " **зависимости проекта/ссылки** ", а затем выберите **изменить ссылки**.
17. Выберите **масфункс. Shared** на вкладке **проекты** , а затем нажмите кнопку **ОК**.
18. Повторите шаги 7-17 (шаг 9 пропускается), используя следующие конфигурации:

    | **ИМЯ ПРОЕКТА**  | **ИМЯ ШАБЛОНА**   | **МЕНЮ "СОЗДАТЬ ПРОЕКТ"**   |
    |-------------------| --------------------| -----------------------|
    | Масфункс. Android | Библиотека классов       | Библиотека > Android      |
    | Масфункс. iOS     | Библиотека привязки     | Библиотека > iOS          |

19. В **Обозреватель решений**дважды щелкните проект **масфункс. Android** , а затем перейдите к параметрам **компилятора** .

20. Если **Конфигурация** настроена на **отладку**, измените параметр **определить символы** , чтобы включить **Android;** .

21. Измените **конфигурацию** на **выпуск**, а затем измените **Определение символов** , чтобы также включить **Android**.

22. Повторите шаги 19-20 для **масфункс. iOS**, а затем измените **Определение символов** , чтобы включить **iOS** , а не **Android;** в обоих случаях.

23. Постройте решение в конфигурации **выпуска** (**Control + Command + B**) и убедитесь, что все три выходные сборки (Android, iOS, .NET Standard) (в соответствующих папках Bin проекта) имеют одно и то же имя **масфункс. dll**.

На этом этапе решение должно иметь три цели: один каждый для Android, iOS и .NET Standard и общий проект, на который ссылается каждая из трех целей. Они должны быть настроены на использование одного и того же пространства имен по умолчанию и выходных сборок с тем же именем. Это необходимо для подхода "Bait and Switch", упомянутого ранее.

### <a name="adding-the-native-libraries"></a>Добавление собственных библиотек

Процесс добавления собственных библиотек в решение-оболочку незначительно различается для Android и iOS.

#### <a name="native-references-for-mathfuncsandroid"></a>Собственные ссылки для Масфункс. Android

1. **Щелкните** проект **масфункс. Android** , а затем в меню **Добавить** выберите пункт **создать папку** с именем **библиотеки.**

2. Для каждого интерфейса **ABI** (двоичный интерфейс приложения) **щелкните** **папку библиотеки** , а затем в меню **Добавить** выберите пункт **создать папку** , настроив его после соответствующего **ABI**. В этом случае:

    - arm64-v8a
    - armeabi-v7a;
    - x86
    - x86_64  

    > [!NOTE]
    > Более подробные сведения см. в разделе [архитектуры и ЦП](https://developer.android.com/ndk/guides/arch) в [Руководстве разработчика NDK](https://developer.android.com/ndk/guides/), в частности в разделе об адресации [машинного кода в пакетах приложений](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages).

3. Проверьте структуру папок:  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. Добавьте соответствующие библиотеки **. Таким образом** в каждую из папок **ABI** на основе следующего сопоставления:

    **arm64-v8a:** библиотеки, Android и arm64

    **ARMEABI-V7A:** библиотеки, Android и ARM  

    **x86:** библиотеки/Android/x86

    **x86_64:** библиотеки/Android/x86_64

    > [!NOTE]
    > Чтобы добавить файлы, **щелкните** папку, представляющую соответствующий **ABI**, и выберите **Добавить файлы...** в меню " **Добавить** ". Выберите соответствующую библиотеку (из каталога **прекомпиледлибс** ), а затем нажмите кнопку **Открыть** , а затем нажмите кнопку **ОК** , чтобы *скопировать файл в каталог*.

5. Для каждого **из файлов нажмите** кнопку " **Управление** " и выберите параметр **Ембеддеднативелибрари** в меню " **действие сборки** ".

Теперь папка **библиотеки** должна выглядеть следующим образом:

```folders
- lib
    - arm64-v8a
        - libMathFuncs.so
    - armeabi-v7a
        - libMathFuncs.so
    - x86
        - libMathFuncs.so
    - x86_64
        - libMathFuncs.so
```

#### <a name="native-references-for-mathfuncsios"></a>Собственные ссылки для Масфункс. iOS

1. **Щелкните** проект **масфункс. iOS** и выберите **Добавить встроенную ссылку** в меню **Добавить** . 
2. Выберите **либмасфункс.** Library (из библиотек или iOS в каталоге **прекомпиледлибс** ) и нажмите кнопку **Открыть** . 
3. **Щелкните** файл **либмасфункс** (в папке " **машинные ссылки** ", а затем выберите в меню пункт " **Свойства** ").  
4. Настройте свойства **встроенной ссылки** , чтобы они были отмечены (на панели **свойств** отображается значок деления):

    - Принудительная Загрузка
    - РекомендуетсяC++
    - Интеллектуальная связь

    > [!NOTE]
    > Использование типа проекта библиотеки привязки вместе со [встроенной ссылкой](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) внедряет статическую библиотеку и позволяет автоматически связывать ее с приложением Xamarin. iOS, которое ссылается на него (даже если оно включено через пакет NuGet).

5. Откройте **ApiDefinition.CS**, удалив шаблонный код с комментарием (не удаляя только `MathFuncs` пространство имен), а затем выполните то же действие для **structs.CS** . 

    > [!NOTE]
    > Проект библиотеки привязки требует, чтобы эти файлы (с действиями сборки *обжкбиндингапидефинитион* и *обжкбиндингкоресаурце* ) были созданы. Однако мы напишем код, чтобы вызвать нашу собственную библиотеку за пределами этих файлов таким образом, который можно совместно использовать в целевых объектах библиотеки Android и iOS с помощью стандартного метода P/Invoke.

### <a name="writing-the-managed-library-code"></a>Написание кода управляемой библиотеки

Теперь напишите C# код для вызова собственной библиотеки. Целью является скрытие всех базовых сложностей. Потребителю не нужен никакой опыт работы с внутренними библиотеками или понятиями P/Invoke.  

#### <a name="creating-a-safehandle"></a>Создание SafeHandle

1. **Щелкните** проект **масфункс. Shared** , а затем выберите **Добавить файл...** в меню **Добавить** . 
2. В окне **новый файл** выберите **пустой класс** , назовите его **мимасфункссафехандле** и нажмите кнопку **создать** .
3. Реализуйте класс **мимасфункссафехандле** :

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) — это предпочтительный способ работы с неуправляемыми ресурсами в управляемом коде. Это абстрагирует множество стандартного кода, связанного с критическим завершением и жизненным циклом объектов. Владелец этого маркера может впоследствии обрабатывать его как любой другой управляемый ресурс и не должен реализовывать полный [шаблон для уничтожения](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose). 

#### <a name="creating-the-internal-wrapper-class"></a>Создание внутреннего класса-оболочки

1. Откройте **MyMathFuncsWrapper.CS**, изменив его на внутренний статический класс

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. В том же файле добавьте в класс следующую условную инструкцию:

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > Это задает значение константы **dllname** в зависимости от того, строится ли библиотека для **Android** или **iOS**. Это позволяет обращаться к различным соглашениям об именовании, используемым каждой соответствующей платформой, но также к типу библиотеки, используемой в этом случае. Android использует динамическую библиотеку, поэтому в нем должно быть имя файла, включая расширение. Для iOS требуется " *__Internal*", так как используется статическая библиотека.

3. Добавьте ссылку на **System. Runtime. InteropServices** в начало файла **MyMathFuncsWrapper.CS**

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. Добавьте методы-оболочки для создания и удаления класса **MyMathFuncs** :

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > Мы передаем нашу константу **dllname** в атрибут **DllImport** вместе с **точкой входа** , которая явно сообщает среде выполнения .NET имя функции для вызова в этой библиотеке. Технически не нужно предоставлять значение **EntryPoint** , если имена управляемых методов идентичны неуправляемым. Если он не указан, вместо него в качестве **точки входа** будет использоваться имя управляемого метода. Однако лучше быть явным.  

5. Добавьте методы-оболочки, чтобы позволить нам работать с классом **MyMathFuncs** , используя следующий код:

    ```csharp
    [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
    internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
    internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
    internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
    internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
    ```

    > [!NOTE]
    > В этом примере мы используем простые типы для параметров. Поскольку маршалинг является побитовой копией в этом случае, для нашей части не требуется дополнительная работа. Также обратите внимание на использование класса **мимасфункссафехандле** вместо стандартного **IntPtr**. **IntPtr** автоматически сопоставляется с **SafeHandle** как часть процесса маршалинга.

6. Убедитесь, что завершенный класс **мимасфунксвраппер** выглядит следующим образом:

    ```csharp
    using System.Runtime.InteropServices;

    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
            #if Android
                const string DllName = "libMathFuncs.so";
            #else
                const string DllName = "__Internal";
            #endif

            [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
            internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

            [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
            internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);

            [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
            internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
            internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
            internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
            internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
        }
    }
    ```

#### <a name="completing-the-mymathfuncssafehandle-class"></a>Завершение работы класса Мимасфункссафехандле

1. Откройте класс **мимасфункссафехандле** , перейдите к заполнительу комментария **TODO** в методе **ReleaseHandle** :

    ```csharp
    // TODO: Release the handle here
    ```

1. Замените строку **TODO** :

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(handle);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>Написание класса MyMathFuncs

Теперь, когда оболочка завершена, создайте класс MyMathFuncs, который будет управлять ссылкой на неуправляемый объект C++ MyMathFuncs.  

1. **Щелкните** проект **масфункс. Shared** , а затем выберите **Добавить файл...** в меню **Добавить** . 
2. В окне **новый файл** выберите **пустой класс** , назовите его **MyMathFuncs** и нажмите кнопку **создать** .
3. Добавьте следующие члены в класс **MyMathFuncs** :

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. Реализуйте конструктор для класса, чтобы он создавал и сохранял обработчик для собственного объекта **MyMathFuncs** при создании экземпляра класса:

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. Реализуйте интерфейс **IDisposable** , используя следующий код:

    ```csharp
    public class MyMathFuncs : IDisposable
    {
        ...

        protected virtual void Dispose(bool disposing)
        {
            if (handle != null && !handle.IsInvalid)
                handle.Dispose();
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        // ...
    }
    ```

6. Реализуйте методы **MyMathFuncs** с помощью класса **мимасфунксвраппер** , чтобы выполнить реальную работу за счет передачи указателя, сохраненного в базовый неуправляемый объект. Код должен выглядеть следующим образом:

    ```csharp
    public double Add(double a, double b)
    {
        return MyMathFuncsWrapper.Add(handle, a, b);
    }

    public double Subtract(double a, double b)
    {
        return MyMathFuncsWrapper.Subtract(handle, a, b);
    }

    public double Multiply(double a, double b)
    {
        return MyMathFuncsWrapper.Multiply(handle, a, b);
    }

    public double Divide(double a, double b)
    {
        return MyMathFuncsWrapper.Divide(handle, a, b);
    }
    ```

#### <a name="creating-the-nuspec"></a>Создание nuspec

Чтобы библиотека была упакована и распространена с помощью NuGet, решению требуется файл **nuspec** . Это определит, какие из результирующих сборок будут включаться для каждой поддерживаемой платформы.

1. **Щелкните** решение **масфункс**, а затем выберите **Добавить папку решения** в меню **Добавить** имя **солутионитемс**.
2. **Щелкните** папку **солутионитемс** , а затем выберите **создать файл...** в меню **Добавить** .
3. Выберите **пустой XML-файл** в окне **новый файл** , назовите его **масфункс. nuspec** и нажмите кнопку **создать**.
4. Обновите **масфункс. nuspec** , указав базовые метаданные пакета, которые будут отображаться для потребителя **NuGet** . Пример:

    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>
            <id>MathFuncs</id>
            <version>$version$</version>
            <authors>Microsoft Mobile Customer Advisory Team</authors>
            <description>Sample C++ Wrapper Library</description>
            <requireLicenseAcceptance>false</requireLicenseAcceptance>
            <copyright>Copyright 2018</copyright>
        </metadata>
    </package>
    ```

    > [!NOTE]
    > Дополнительные сведения о схеме, используемой для этого манифеста, см. в [справочном](https://docs.microsoft.com/nuget/reference/nuspec) документе по nuspec.

5. Добавьте элемент `<files>` в качестве дочернего элемента `<package>` (как показано ниже `<metadata>`), определяя каждый файл с помощью отдельного элемента `<file>`:

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > При установке пакета в проект и наличии нескольких сборок, указанных одним и тем же именем, NuGet эффективно выбирает сборку, наиболее подданную для данной платформы.

6. Добавьте `<file>` элементы для сборок **Android** :

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. Добавьте `<file>` элементы для сборок **iOS** :

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. Добавьте элементы `<file>` для сборок **netstandard 2.0** :

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. Проверьте манифест **nuspec** :

    ```xml
    <?xml version="1.0"?>
    <package>
    <metadata>
        <id>MathFuncs</id>
        <version>$version$</version>
        <authors>Microsoft Mobile Customer Advisory Team</authors>
        <description>Sample C++ Wrapper Library</description>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <copyright>Copyright 2018</copyright>
    </metadata>
    <files>

        <!-- Android -->
        <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
        <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
        
        <!-- iOS -->
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
        
        <!-- netstandard2.0 -->
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />

    </files>
    </package>
    ```

    > [!NOTE]
    > Этот файл указывает выходные пути сборки из сборки **выпуска** , поэтому не забудьте построить решение, используя эту конфигурацию.

На этом этапе решение содержит 3 сборки .NET и вспомогательный манифест **nuspec** .

## <a name="distributing-the-net-wrapper-with-nuget"></a>Распространение оболочки .NET с помощью NuGet

Следующим шагом является упаковка и распространение пакета NuGet, чтобы его можно было легко использовать в приложении и управлять им как зависимость. Перенос и потребление можно выполнять в рамках одного решения, но распространять библиотеку с помощью средств NuGet при разделении и позволяет управлять этими кодовыми библиотеками независимо друг от друга.

### <a name="preparing-a-local-packages-directory"></a>Подготовка каталога локальных пакетов

Самая простая форма веб-канала NuGet — это локальный каталог:

1. В окне **Finder**перейдите к удобному каталогу. Например, **/Users**.
2. В меню **файл** выберите пункт **создать папку** , указав Понятное имя, например **Local-NuGet-Feed**.

### <a name="creating-the-package"></a>Создание пакета

1. Задайте для **конфигурации сборки** значение **Release**и выполните сборку с помощью **команды + B**.
2. Откройте окно **терминала** и перейдите в папку, содержащую файл **nuspec** .
3. В окне **терминала**выполните команду **NuGet Pack** , указав файл **nuspec** , **версию** (например, 1.0.0) и **OutputDirectory** , используя папку, созданную на [предыдущем шаге](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed), то есть **Local-NuGet-Feed**. Пример:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **Убедитесь** в том, что **масфункс. 1.0.0. nupkg** создан в каталоге **Local-веб-канала** .

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>ИСПОЛЬЗУЕМЫХ Использование частного веб-канала NuGet с Azure DevOps

Более надежная методика описана в статье Начало [работы с пакетами NuGet в Azure DevOps](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package), в которой показано, как создать частный веб-канал и отправить пакет (созданный на предыдущем шаге) в этот канал.

Это идеальный вариант для полной автоматизации рабочего процесса, например с помощью [Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts). Дополнительные сведения см. в статье [Приступая к работе с Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts).

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Использование оболочки .NET из приложения Xamarin. Forms

Для выполнения этого пошагового руководства Создайте приложение **Xamarin. Forms** для использования пакета, только что опубликованного в локальном веб-канале **NuGet** .

### <a name="creating-the-xamarinforms-project"></a>Создание проекта **Xamarin. Forms**

1. Откройте новый экземпляр **Visual Studio для Mac**. Это можно сделать с **терминала**:

    ```bash
    open -n -a "Visual Studio"
    ```

2. В **Visual Studio для Mac**щелкните **создать проект** (на *странице приветствия*) или **новое решение** (в меню *файл* ).
3. В окне **Новый проект** выберите **пустое приложение форм** (в *многоплатформенном > приложении*) и нажмите кнопку **Далее**.
4. Обновите следующие поля и нажмите кнопку **Далее**.

    - **Имя приложения:** Масфунксапп.
    - **Идентификатор организации:** Используйте обратные пространства имен, например _com. {your_org}_ .
    - **Целевые платформы:** Используйте значения по умолчанию (цели Android и iOS).
    - **Общий код:** Присвойте этому параметру значение .NET Standard ("Общая библиотека"), но помимо области действия этого пошагового руководства.

5. Обновите следующие поля и нажмите кнопку **создать**.

    - **Имя проекта:** Масфунксапп.
    - **Имя решения:** Масфунксапп.  
    - **Расположение:** Используйте расположение для сохранения по умолчанию (или выберите альтернативный вариант).

6. В **Обозреватель решений**щелкните **целевой** объект (**масфунксапп. Android** или **масфункс. iOS**) для первоначального тестирования, а затем выберите **Назначить запускаемым проектом**.
7. Выберите предпочитаемый **эмулятор** **устройства** или **симулятора**/. 
8. Запустите решение (**команда + Return**), чтобы убедиться в том, что шаблон проекта **Xamarin. Forms** строится и работает нормально. 

    > [!NOTE]
    > **iOS** (в частности, симулятор), как правило, имеют самое быстрое время сборки или развертывания.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>Добавление локального веб-канала NuGet в конфигурацию NuGet

1. В **Visual Studio**выберите **предпочтения** (в меню **Visual Studio** ).
2. Выберите **источники** в разделе **NuGet** , а затем нажмите кнопку **добавить**.
3. Обновите следующие поля и нажмите кнопку **Добавить источник**:

    - **Имя:** Укажите понятное имя, например Local-Packages.  
    - **Расположение:** Укажите папку **локального канала NuGet** , созданную на [предыдущем шаге](#preparing-a-local-packages-directory).

    > [!NOTE]
    > В этом случае нет необходимости указывать **имя пользователя** и **пароль**. 

4. Нажмите кнопку **ОК**.

### <a name="referencing-the-package"></a>Указание ссылок на пакет

Повторите следующие шаги для каждого проекта (**масфунксапп**, **масфунксапп. Android**и **масфунксапп. iOS**).

1. **Щелкните** проект, а затем выберите **Добавить пакеты NuGet...** в меню **добавить** .
2. Выполните поиск по запросу **масфункс**. 
3. Убедитесь, что **версия** пакета **1.0.0** , а другие сведения отображаются должным образом, например **заголовок** и **Описание**, то есть *масфункс* и  *C++ примеры библиотеки оболочек*. 
4. Выберите пакет **масфункс** , а затем щелкните **Добавить пакет**.

### <a name="using-the-library-functions"></a>Использование библиотечных функций

Теперь со ссылкой на пакет **масфункс** в каждом проекте функции доступны для C# кода.

1. Откройте **MainPage.XAML.CS** в рамках общего проекта **Xamarin. Forms** **масфунксапп** (на который ссылаются как **масфунксапп. Android** , так и **масфунксапп. iOS**).
2. Добавьте операторы **using** для **System. Diagnostics** и **масфункс** в начало файла:

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. Объявите экземпляр класса `MyMathFuncs` в верхней части класса `MainPage`:

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. Переопределите методы `OnAppearing` и `OnDisappearing` из `ContentPage` базового класса:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
    }
    ```

5. Обновите метод `OnAppearing`, чтобы инициализировать переменную `myMathFuncs`, объявленную ранее:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. Обновите метод `OnDisappearing`, чтобы вызвать метод `Dispose` в `myMathFuncs`:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. Реализуйте закрытый метод с именем **тестмасфункс** следующим образом:

    ```csharp
    private void TestMathFuncs()
    {
        var numberA = 1;
        var numberB = 2;

        // Test Add function
        var addResult = myMathFuncs.Add(numberA, numberB);

        // Test Subtract function
        var subtractResult = myMathFuncs.Subtract(numberA, numberB);

        // Test Multiply function
        var multiplyResult = myMathFuncs.Multiply(numberA, numberB);

        // Test Divide function
        var divideResult = myMathFuncs.Divide(numberA, numberB);

        // Output results
        Debug.WriteLine($"{numberA} + {numberB} = {addResult}");
        Debug.WriteLine($"{numberA} - {numberB} = {subtractResult}");
        Debug.WriteLine($"{numberA} * {numberB} = {multiplyResult}");
        Debug.WriteLine($"{numberA} / {numberB} = {divideResult}");
    }
    ```

8. Наконец, вызовите `TestMathFuncs` в конце метода `OnAppearing`:

    ```csharp
    TestMathFuncs();
    ```

9. Запустите приложение на каждой целевой платформе и проверьте выходные данные на панели **выходных данных приложения** , как показано ниже.

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > При обнаружении "*DLLNotFoundException*" при тестировании на Android или при ошибке сборки в iOS убедитесь, что архитектура ЦП устройства, эмулятора и симулятора, которую вы используете, совместима с подмножеством, которое мы выбрали для поддержки. 

## <a name="summary"></a>Сводка

В этой статье объясняется, как создать приложение Xamarin. Forms, использующее собственные библиотеки, через общую оболочку .NET, распространяемую через пакет NuGet. Пример, приведенный в этом пошаговом руководстве, намеренно очень упрощен для упрощения демонстрации подхода. Реальное приложение будет работать со сложными методами, такими как обработка исключений, обратные вызовы, маршалинг более сложных типов и связывание с другими библиотеками зависимостей. Ключевым фактором является процесс, с помощью которого развитие C++ кода координируется и синхронизируется с оберткой и клиентскими приложениями. Этот процесс может отличаться в зависимости от того, отвечает ли одна команда на одну или обе. В любом случае Автоматизация является реальным преимуществом. Ниже приведены некоторые ресурсы, которые позволяют более подробно ознакомиться с некоторыми из основных концепций, а также с соответствующими загружаемыми файлами. 

### <a name="downloads"></a>Загрузки

- [Средства командной строки NuGet (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>Примеры

- [Пропромежуток от кросс-платформенной разработки мобильных приложений сC++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft PIX (C++ и Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Пример порта San для Mono](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>Дополнительные сведения

[Статьи, относящиеся к содержимому этой записи](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)