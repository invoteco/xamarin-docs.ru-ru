---
title: Метаданные привязок Java
description: C#код в Xamarin. Android вызывает библиотеки Java с помощью привязок, которые являются механизмом, который абстрагирует сведения низкого уровня, указанные в собственном интерфейсе Java (JNI). Xamarin. Android предоставляет средство, создающее эти привязки. Эта программа позволяет разработчику управлять способом создания привязки с помощью метаданных, что позволяет выполнять такие процедуры, как изменение пространств имен и переименование членов. В этом документе описывается, как работают метаданные, приводится сводка атрибутов, поддерживаемых метаданными, и объясняется, как устранить проблемы привязки, изменив эти метаданные.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 1f06601b2b419141b4bd44677826df4e64a831fc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020571"
---
# <a name="java-bindings-metadata"></a>Метаданные привязок Java

_C#код в Xamarin. Android вызывает библиотеки Java с помощью привязок, которые являются механизмом, который абстрагирует сведения низкого уровня, указанные в собственном интерфейсе Java (JNI). Xamarin. Android предоставляет средство, создающее эти привязки. Эта программа позволяет разработчику управлять способом создания привязки с помощью метаданных, что позволяет выполнять такие процедуры, как изменение пространств имен и переименование членов. В этом документе описывается, как работают метаданные, приводится сводка атрибутов, поддерживаемых метаданными, и объясняется, как устранить проблемы привязки, изменив эти метаданные._

## <a name="overview"></a>Обзор

**Библиотека привязки Java** для Xamarin. Android пытается автоматизировать большую часть работы, необходимой для привязки существующей библиотеки Android, с помощью средства, которое иногда называют _генератором привязок_. При привязке библиотеки Java Xamarin. Android проверяет классы Java и создает список всех пакетов, типов и элементов, которые должны быть привязаны. Этот список интерфейсов API хранится в XML-файле, который можно найти в **\{каталоге проекта} \обж\релеасе\апи.ксмл** для сборки **выпуска** , а также в **\{каталоге проекта} \обж\дебуг\апи.ксмл** для **отладочной** сборки.

![Расположение файла API. XML в папке obj/Debug](java-bindings-metadata-images/java-bindings-metadata-01.png)

Генератор привязок будет использовать файл **API. XML** в качестве рекомендации для создания необходимых C# классов-оболочек. Содержимое этого XML-файла является разновидностью формата _проекта с открытым кодом Android_ в Google.
В следующем фрагменте кода приведен пример содержимого файла **API. XML**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

В этом примере **API** объявляет класс в пакете `android` с именем `Manifest`, который расширяет `java.lang.Object`.

Во многих случаях требуется помощь человека, чтобы сделать API Java более "похожим" на ".NET Like", или устранить проблемы, препятствующие компиляции сборки привязки. Например, может потребоваться изменить имена пакетов Java на пространства имен .NET, переименовать класс или изменить тип возвращаемого значения метода.

Эти изменения не достигаются путем непосредственного изменения **API. XML** .
Вместо этого изменения записываются в специальные XML-файлы, предоставляемые шаблоном библиотеки привязки Java. При компиляции сборки привязки Xamarin. Android на генераторе привязок будут влиять эти файлы сопоставления при создании сборки привязки

Эти XML-файлы сопоставления можно найти в папке **TRANSFORMS** проекта:

- &ndash; **Metadata. XML** позволяет вносить изменения в окончательный API, например изменять пространство имен созданной привязки. 

- **EnumFields. xml** &ndash; содержит сопоставление между константами Java `int` и C#`enums`. 

- **Enummethods-. xml** &ndash; позволяет изменять параметры и возвращаемые типы методов из `int` константы C# Java для`enums`. 

Файл **Metadata. XML** — это самый последний импорт этих файлов, так как он позволяет вносить изменения общего назначения в привязку, например:

- Переименование пространств имен, классов, методов или полей, чтобы они следовали соглашениям .NET. 

- Удаление пространств имен, классов, методов или полей, которые не требуются. 

- Перемещение классов в разные пространства имен. 

- Добавление дополнительных классов поддержки для обеспечения разработки привязки после шаблонов .NET Framework. 

Позволяет перейти к обсуждению **Metadata. XML** более подробно.

## <a name="metadataxml-transform-file"></a>Файл преобразования Metadata. XML

Как уже было сказано, файл **Metadata. XML** используется генератором привязок для влияния на создание сборки привязки.
Формат метаданных использует синтаксис [XPath](https://www.w3.org/TR/xpath/) и практически идентичен *метаданным гапи* , описанным в разделе руководств по [метаданным гапи](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) . Эта реализация почти является полной реализацией XPath 1,0 и, таким же, поддерживает элементы в стандарте 1,0. Этот файл является мощным механизмом на основе XPath для изменения, добавления, скрытия или перемещения любого элемента или атрибута в файле API. Все элементы правил в спецификации метаданных включают атрибут Path для указания узла, к которому применяется правило. Правила применяются в следующем порядке:

- &ndash; **добавления-узла** добавляет дочерний узел к узлу, заданному атрибутом path.
- **attr** &ndash; задает значение атрибута элемента, указанного в атрибуте Path.
- **Remove-node** &ndash; удаляет узлы, соответствующие заданному XPath.

Ниже приведен пример файла **Metadata. XML** .

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Ниже перечислены некоторые из наиболее часто используемых элементов XPath для API-интерфейса Java.

- `interface` &ndash;, используемый для нахождение интерфейса Java. Например, `/interface[@name='AuthListener']`.

- `class` &ndash;, используемый для нахождение класса. Например, `/class[@name='MapView']`.

- `method` &ndash; используется для нахождение метода в классе или интерфейсе Java. Например, `/class[@name='MapView']/method[@name='setTitleSource']`.

- `parameter` &ndash; задать параметр для метода. Например, `/parameter[@name='p0']`

### <a name="adding-types"></a>Добавление типов

Элемент `add-node` сообщит проекту привязки Xamarin. Android добавить новый класс-оболочку в **API. XML**. Например, следующий фрагмент кода будет направлять генератор привязки для создания класса с конструктором и одним полем:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>Удаление типов

Можно указать генератору привязок Xamarin. Android игнорировать тип Java, а не привязывать его. Это можно сделать, добавив `remove-node` XML-элемент в файл **Metadata. XML** :

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Переименование членов

Переименование членов невозможно выполнить путем непосредственного редактирования файла **API. XML** , так как для Xamarin. Android требуются исходные имена машинного интерфейса Java (JNI). Поэтому атрибут `//class/@name` изменить нельзя. Если это так, привязка не будет работать.

Рассмотрим случай, когда нужно переименовать тип `android.Manifest`.
Для этого мы можем попытаться напрямую изменить **API. XML** и переименовать класс следующим образом:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Это приведет к созданию генератора привязок, создающего C# следующий код для класса-оболочки:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Обратите внимание, что класс-оболочка был переименован в `NewName`, а исходный тип Java все еще `Manifest`. Класс привязки Xamarin. Android больше не может получить доступ к каким-либо методам `android.Manifest`; класс-оболочка привязан к несуществующему типу Java.

Чтобы правильно изменить управляемое имя упакованного типа (или метода), необходимо задать атрибут `managedName`, как показано в следующем примере:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Переименование `EventArg` классов-оболочек

Когда генератор привязки Xamarin. Android определяет метод задания `onXXX` для _типа прослушивателя_, создается C# событие и`EventArgs`подкласс для поддержки API-интерфейса .NET флавауред для шаблона прослушивателя на основе Java. В качестве примера рассмотрим следующий класс и метод Java:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin. Android удалит префикс `on` из метода задания, а вместо этого использовать `2DSignNextManuever` в качестве основания для имени подкласса `EventArgs`. Подкласс будет называться примерно так:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Это имя C# класса не является допустимым. Чтобы устранить эту проблему, автор привязки должен использовать атрибут `argsType` и предоставить допустимое C# имя для подкласса`EventArgs`:

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>Поддерживаемые атрибуты

В следующих разделах описаны некоторые атрибуты для преобразования API-интерфейсов Java.

### <a name="argstype"></a>аргстипе

Этот атрибут помещается в методы задания, чтобы присвоить имя подклассу `EventArg`, который будет создан для поддержки прослушивателей Java. Это подробно описано ниже в разделе [переименование классов-оболочек EventArg](#Renaming_EventArg_Wrapper_Classes) далее в этом пошаговом окне.

### <a name="eventname"></a>eventName

Указывает имя для события. Если значение пустое, оно препятствует созданию события.
Это описано более подробно в разделе [переименование классов-оболочек EventArg](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>манажеднаме

Используется для изменения имени пакета, класса, метода или параметра. Например, чтобы изменить имя класса Java, `MyClass` `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

В следующем примере показано выражение XPath для переименования метода `java.lang.object.toString` в `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>Компьютерами

`managedType` используется для изменения типа возвращаемого значения метода. В некоторых ситуациях генератор привязок неправильно определяет возвращаемый тип метода Java, что приведет к ошибке времени компиляции. Одним из возможных решений в этой ситуации является изменение типа возвращаемого значения метода.

Например, генератор привязок считает, что метод Java `de.neom.neoreadersdk.resolution.compareTo()` должен возвращать `int`, что приводит к ошибке сообщения об ошибке **CS0535: ' de. Неом. Неореадерсдк. resolution не реализует член интерфейса "Java. lang. IComparable. CompareTo (Java. lang. Object)"** . В следующем фрагменте кода показано, как изменить тип параметра созданного C# метода с`DE.Neom.Neoreadersdk.Resolution`на `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>манажедретурн

Изменяет тип возвращаемого значения метода. Это не изменяет возвращаемый атрибут (так как изменения возвращаемых атрибутов могут привести к несовместимости изменений в сигнатуре JNI). В следующем примере тип возвращаемого значения метода `append` изменяется с `SpannableStringBuilder` на `IAppendable` (отзыв, который C# не поддерживает ковариантные типы возвращаемых данных):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>скрытый

Средства, которые закрывают библиотеки Java, могут конфликтовать с генератором привязок Xamarin. Android и его C# возможностью создавать классы-оболочки. Характеристики запутанных классов включают: 

- Имя класса включает **$** , т. е. **класс $.**
- Имя класса полностью скомпрометировано символов нижнего регистра, т. е. **класса.**

В этом фрагменте кода приведен пример создания "немаскированного" C# типа:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Этот атрибут можно использовать для изменения имени управляемого свойства.

Специализированным случаем использования `propertyName` является ситуация, когда у класса Java есть только метод Getter для поля. В этом случае генератору привязки нужно создать свойство только для записи, что не рекомендуется в .NET. В следующем фрагменте кода показано, как удалить свойства .NET, установив для `propertyName` пустую строку:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Обратите внимание, что методы Setter и Getter по-прежнему будут создаваться генератором привязок.

### <a name="sender"></a>отправитель

Указывает, какой параметр метода должен быть параметром `sender`, если метод сопоставлен с событием. Значение может быть `true` или `false`. Пример:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>видимость

Этот атрибут используется для изменения видимости класса, метода или свойства. Например, может потребоваться повысить уровень `protected` метода Java таким образом, чтобы его C# оболочка была`public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields. XML и Enummethods-. XML

Бывают случаи, когда библиотеки Android используют целочисленные константы для представления состояний, передаваемых в свойства или методы библиотек. Во многих случаях полезно привязать эти целочисленные константы к перечислениям в C#. Чтобы упростить это сопоставление, используйте файлы **EnumFields. XML** и **enummethods-. XML** в проекте привязки. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Определение перечисления с помощью EnumFields. XML

Файл **EnumFields. XML** содержит сопоставление между константами Java `int` и C#`enums`. Давайте рассмотрим следующий пример C# перечисления, создаваемого для набора констант`int`: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Здесь мы воспользовались классом Java `SKRealReachSettings` и определили C# перечисление с именем`SKMeasurementUnit`в`Skobbler.Ngx.Map.RealReach`пространства имен. Записи `field` определяют имя константы Java (пример `UNIT_SECOND`), имя записи перечисления (пример `Second`) и целочисленное значение, представленное обеими сущностями (например, `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Определение методов Get/Setter с помощью Enummethods-. XML

Файл **enummethods-. XML** позволяет изменять параметры метода и возвращаемые типы из `int` константы Java C# для`enums`. Иными словами, он сопоставляет чтение и запись C# перечислений (определенных в файле **EnumFields. XML** ) с Java `int`константным`get`и методами`set`.

Учитывая приведенное выше перечисление `SKRealReachSettings`, в следующем файле **enummethods-. XML** определяется метод получения или задания для этого перечисления:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

Первая строка `method` сопоставляет возвращаемое значение метода Java `getMeasurementUnit` с перечислением `SKMeasurementUnit`. Вторая строка `method` сопоставляет первый параметр `setMeasurementUnit` с тем же перечислением.

После внесения всех этих изменений можно использовать следующий код в Xamarin. Android для установки `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>Сводка

В этой статье описывается, как Xamarin. Android использует метаданные для преобразования определения API из формата *Google* *аосп*. После внесения изменений, которые возможны при использовании *Metadata. XML*, изучены ограничения, обнаруженные при переименовании элементов, и представлен список ПОДДЕРЖИВАЕМЫХ атрибутов XML, описывающих, когда следует использовать каждый атрибут.

## <a name="related-links"></a>Связанные ссылки

- [Работа с JNI](~/android/platform/java-integration/working-with-jni.md)
- [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md)
- [Метаданные ГАПИ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
