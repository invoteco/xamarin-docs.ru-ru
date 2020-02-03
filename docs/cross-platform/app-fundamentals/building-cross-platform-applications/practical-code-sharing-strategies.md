---
title: Часть 5. Практические стратегии совместного использования кода
description: В этом документе обсуждаются практические стратегии для сценариев, таких как базы данных, доступ к файлам, сетевые операции и асинхронного кода совместного использования кода.
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 0e37e138607fb0e00fbdc463ac7c53facf81395d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723634"
---
# <a name="part-5---practical-code-sharing-strategies"></a>Часть 5. Практические стратегии совместного использования кода

В этом разделе приведены примеры того, как совместное использование кода для распространенных сценариев приложений.

## <a name="data-layer"></a>Уровень данных

Уровень данных состоит из подсистемы хранилища и методы для чтения и записи данных. Производительность, гибкость и кросс платформенной совместимости SQLite для кроссплатформенных приложений Xamarin рекомендуется компонент database engine.
Она может работать на разнообразных платформах, включая Windows, Android, iOS и Mac.

### <a name="sqlite"></a>SQLite

SQLite — это реализация базы данных с открытым исходным кодом. Источник и документация можно найти по адресу [SQLite.org](https://www.sqlite.org/). Поддержка SQLite доступна на каждой мобильной платформе:

- **iOS** — встроена в операционную систему.
- **Android** — встроена в операционную систему с версии Android 2,2 (API уровня 10).
- **Windows** — см. в разделе [расширение SQLite для универсальная платформа Windows](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Даже с ядром СУБД, доступна на всех платформах отличаются собственные методы для доступа к базе данных. Как iOS и Android предоставляют встроенные API для доступа к SQLite, который может быть использован с Xamarin.iOS или Xamarin.Android, тем не менее с помощью собственных методов пакета SDK предлагает возможность совместного использования кода (кроме, возможно SQL-запросы, при условии, что они хранятся как строки) . Дополнительные сведения о функциях поиска в собственных базах данных см. в `CoreData` в классе `SQLiteOpenHelper` для iOS или Android. так как эти параметры не являются межплатформенными, они выходят за рамки данного документа.

### <a name="adonet"></a>ADO.NET

Как Xamarin. iOS, так и Xamarin. Android поддерживают `System.Data` и `Mono.Data.Sqlite` (Дополнительные сведения см. в [документации](~/ios/data-cloud/system.data.md) по Xamarin. IOS).
С помощью этих пространств имен позволяет писать код ADO.NET, который работает на обеих платформах. Измените ссылки проекта, чтобы включить `System.Data.dll` и `Mono.Data.Sqlite.dll` и добавьте эти операторы using в код:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

В следующем примере кода будет работать:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Реальных примерах внедрения технологии ADO.NET, очевидно, что бы быть разбиты на различные методы и классы (в этом примере — только для демонстрационных целей).

### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET-ORM платформ

ORM (или объектно-реляционным модулем сопоставления) пытается упростить хранение данных, которые моделируются в классах. Вместо вручную написания SQL-запросов, создание таблиц или SELECT, INSERT и DELETE данных, которые вручную извлечь из поля и свойства, ORM добавляет слой кода, который делает это для вас. Использование отражения для анализа структуры классов, ORM может автоматически создавать таблицы и столбцы, которые соответствуют класс и создают запросы на чтение и запись данных. Это позволяет коду приложения просто отправку и получение экземпляров объектов продукт ORM, которая берет на себя все операции SQL взгляд изнутри.

SQLite-NET выступает в роли простой продукт ORM, который позволит вам сохранять и получать ваши классы в SQLite. Она скрывает сложность кросс-доступа к SQLite платформы с помощью директивы компилятора и другие приемы.

Функции SQLite NET:

- Таблицы определяются путем добавления атрибутов к классам модели.
- Экземпляр базы данных представлен подклассом `SQLiteConnection`, основным классом в библиотеке SQLite-NET.
- Данные могут быть вставлены, запрашивать и удалять с помощью объектов. Инструкции SQL не являются обязательными (несмотря на то, что можно написать инструкций SQL, при необходимости).
- Базовые запросы Linq можно выполнять для коллекций, возвращаемых SQLite-NET.

Исходный код и документация для SQLite-NET доступны в [SQLite-NET на GitHub](https://github.com/praeclarum/sqlite-net) и реализованы в обоих примерах. Ниже приведен простой пример кода SQLite-NET (из примера *задачи Pro* ).

Во-первых, класс `TodoItem` использует атрибуты, чтобы определить поле как первичный ключ базы данных.

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Это позволяет создать `TodoItem` таблицу со следующей строкой кода (и без инструкций SQL) на экземпляре `SQLiteConnection`:

```csharp
CreateTable<TodoItem> ();
```

Данные в таблице также можно обрабатывать с помощью других методов в `SQLiteConnection` (опять же, без инструкций SQL):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Ознакомьтесь с исходным кодом практический пример полные примеры.

## <a name="file-access"></a>Доступ к файлам

Доступ к файлам быть ключевой частью любого приложения. Распространенные примеры файлов, которые могут быть частью включаемый приложения:

- Файлы базы данных SQLite.
- Пользовательские данные (текст, изображения, звуковые, видео).
- Загруженные данные для кэширования (изображения, html или PDF-файлов).

### <a name="systemio-direct-access"></a>System.IO прямой доступ

Как Xamarin. iOS, так и Xamarin. Android обеспечивают доступ к файловой системе с помощью классов в пространстве имен `System.IO`.

Каждая платформа имеет различные ограничения доступа, которые необходимо принимать во внимание:

- iOS приложения выполняются в изолированной среде с помощью доступа очень ограничен файловой системы. Дополнительно Apple определяет, как следует использовать в файловой системе, указав определенные места, сохраненный в резервной копии (и другим пользователям, которые не являются). Дополнительные сведения см. в разделе [Работа с файловой системой в руководстве по Xamarin. iOS](~/ios/app-fundamentals/file-system.md) .
- Android также ограничивает доступ к некоторые каталоги, связанные с приложением, но она также поддерживает внешние носители (например) SD-карты) и доступ к общим данным.
- Windows Phone 8 (Silverlight) запретить прямой доступ к файлам — файлы можно манипулировать только с помощью `IsolatedStorage`.
- Проекты Windows 8.1 WinRT и Windows 10 UWP предлагают только асинхронные операции с файлами с помощью интерфейсов API `Windows.Storage`, которые отличаются от других платформ.

#### <a name="example-for-ios-and-android"></a>Пример для iOS и Android

Ниже приведен упрощенный пример, в котором записывает и читает текстовый файл.
Использование `Environment.GetFolderPath` позволяет выполнять один и тот же код в iOS и Android, каждый из которых возвращает допустимый каталог на основе соглашений о файловой системе.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

Дополнительные сведения о функциональных возможностях файловой системы для iOS см. в документации по Xamarin. iOS, [работающей с документом в файловой системе](~/ios/app-fundamentals/file-system.md) . При написании кода доступа кросс платформенных файл, помните, что некоторые файловые системы чувствительны к регистру и разделителей другой каталог. Рекомендуется всегда использовать один и тот же регистр для имен файлов и метод `Path.Combine()` при создании путей к файлам или каталогам.

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage для Windows 8 и Windows 10

*Создание мобильных приложений с помощью Xamarin. Forms* [Book](https://developer.xamarin.com/r/xamarin-forms/book/)
[главы 20. Асинхронные и файловые операции ввода-вывода](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) включают [примеры для Windows 8.1 и Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

С помощью [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) можно считывать и файловые файлы на этих платформах с помощью поддерживаемых API-интерфейсов:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Дополнительные сведения см. в [разделе книги](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) .

<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Изолированное хранилище в Windows Phone 7 и 8 (Silverlight)

Изолированное хранилище — общий API для сохранения и загрузки файлов во всех iOS, Android и более старых платформ Windows Phone.

Это механизм по умолчанию для доступа к файлам в Windows Phone (Silverlight), который был реализован в Xamarin.iOS и Xamarin.Android разрешить общий доступ к файлам код для записи. На класс `System.IO.IsolatedStorage` можно ссылаться во всех трех платформах в [общем проекте](~/cross-platform/app-fundamentals/shared-projects.md).

Дополнительные сведения см. в [обзоре изолированного хранилища Windows Phone](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx) .

Интерфейсы API изолированного хранилища недоступны в [переносимых библиотеках классов](~/cross-platform/app-fundamentals/pcl.md). Одним из альтернативных вариантов для PCL является [Пклстораже NuGet](https://pclstorage.codeplex.com/) .

### <a name="cross-platform-file-access-in-pcls"></a>Доступ к файлам между различными платформами в PCL

Кроме того, существует совместимая с PCL NuGet — [пклстораже](https://www.nuget.org/packages/PCLStorage/) , которая поддерживает доступ к файлам на разных платформах для платформ, поддерживаемых Xamarin, и последних API-интерфейсов Windows.

## <a name="network-operations"></a>Операции сети

Большинство мобильных приложений будет иметь сетевой компонент, например:

- Загрузка изображений, видео и аудио (например) эскизы, фотографии, музыка).
- Загрузка документов (например) HTML, PDF).
- Отправка пользовательских данных (таких как фотографии или текст).
- Доступ к веб-служб или сторонним поставщиком API-интерфейсы (включая SOAP, XML или JSON).

.NET Framework предоставляет несколько различных классов для доступа к сетевым ресурсам: `HttpClient`, `WebClient`и `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

Класс `HttpClient` в пространстве имен `System.Net.Http` доступен в Xamarin. iOS, Xamarin. Android и большинстве платформ Windows. Существует [Клиентская библиотека Microsoft HTTP NuGet](https://www.nuget.org/packages/Microsoft.Net.Http/) , которую можно использовать для переноса этого API в переносимые библиотеки классов (и Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

Класс `WebClient` предоставляет простой API для извлечения удаленных данных с удаленных серверов.

Универсальная платформа Windows операции *должны* быть асинхронными, хотя Xamarin. iOS и Xamarin. Android поддерживают синхронные операции (которые могут выполняться в фоновых потоках).

Код для простой операции `WebClient` асинхронного:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` также имеет `DownloadFileCompleted` и `DownloadFileAsync` для получения двоичных данных.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>httpWebRequest

`HttpWebRequest` предлагает больше настроек, чем `WebClient` и в результате требует больше кода для использования.

Ниже приведен код для простой синхронной операции `HttpWebRequest`:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

В [документации по веб-службам](~/cross-platform/data-cloud/web-services/index.md)приведен пример.

 <a name="Reachability" />

### <a name="reachability"></a>Доступность

Мобильные устройства работают в различных условиях сети быстро Wi-Fi или 4G-подключениям к низкой приемных и медленными каналами данных EDGE. По этой причине рекомендуется определить, есть ли доступ к сети и если таким образом, какой тип доступен, перед попыткой подключения к удаленным серверам.

Следующие действия на мобильное приложение может уйти в таких ситуациях.

- Если сеть недоступна, попросите пользователя. Если они вручную, он был отключен (например) Режим "в самолете" или отключив Wi-Fi) они могут решить проблему.
- Если подключение установлено 3G, приложения могут вести себя иначе (например, Apple не поддерживает приложения больше 20 МБ для загрузки более 3G). Приложения могут использовать эти сведения, чтобы предупредить пользователя о чрезмерной загрузки время ожидания при получении больших файлов.
- Даже если сеть доступна, рекомендуется проверить соединение с целевым сервером перед запуском других запросов. Это многократно предотвращения сетевых операций приложения, по истечении времени ожидания и также позволит более информативное сообщение об ошибке для отображения пользователю.

## <a name="webservices"></a>Веб-службы

См. нашу документацию по [работе с веб-службами](~/cross-platform/data-cloud/web-services/index.md), которая охватывает доступ к конечным точкам RESTFUL, SOAP и WCF с помощью Xamarin. iOS. Его можно самостоятельное запросов веб-службы и обработки ответов, тем не менее есть библиотеки сделать это гораздо проще, включая Azure, RestSharp и ServiceStack. Даже основные операции WCF может осуществляться в приложениях Xamarin.

### <a name="azure"></a>Azure

Microsoft Azure — это облачная платформа, которая предоставляет широкий спектр служб для мобильных приложений, включая хранение данных и синхронизации и Push-уведомлений.

Посетите [Azure.Microsoft.com](https://azure.microsoft.com/) , чтобы испытать его бесплатно.

### <a name="restsharp"></a>RestSharp

RestSharp — это библиотека .NET, которое может быть включено в мобильных приложениях для предоставления клиенту REST, который упрощает доступ к веб-служб. Она помогает, предоставляя простой API для запроса данных и проанализировать Ответ REST. RestSharp может быть полезно

На [веб-сайте рестшарп](http://restsharp.org/) содержится [Документация](https://github.com/restsharp/RestSharp/wiki) по реализации клиента RESTful с помощью рестшарп.
Рестшарп предоставляет примеры Xamarin. iOS и Xamarin. Android на [GitHub](https://github.com/restsharp/RestSharp/).

В [документации по веб-службам](~/cross-platform/data-cloud/web-services/index.md)также есть фрагмент кода Xamarin. iOS.

 <a name="ServiceStack" />

### <a name="servicestack"></a>ServiceStack

В отличие от RestSharp ServiceStack — это обоих серверного решения для размещения веб-службы, а также клиентской библиотеки, который может быть реализован в мобильных приложениях для доступа к этим службам.

На [веб-сайте ServiceStack](http://servicestack.net/) объясняется назначение проекта и приводятся ссылки на примеры документов и кода. Примеры включают полная реализация на стороне сервера веб-службы, а также различные клиентские приложения, доступ к нему.

### <a name="wcf"></a>WCF

Средства Xamarin могут помочь вам использовать некоторые службы Windows Communication Foundation (WCF). Как правило Xamarin поддерживает то же подмножество WCF, которая поставляется вместе со средой выполнения Silverlight в стороне клиента. Сюда входят наиболее распространенные реализации кодирования и протоколов WCF: сообщения SOAP, закодированные в текстовом формате, по транспортному протоколу HTTP с помощью `BasicHttpBinding`.

Из-за размера и сложности платформы WCF могут существовать текущие и будущие службами, которые будут выходят за пределы области действия поддерживается доменом клиента подмножество Xamarin. Кроме того поддержка WCF требует применения средств доступно только в среде Windows для создания прокси-сервера.

 <a name="Threading" />

## <a name="threading"></a>Потоки

Скорость реагирования приложения важно для мобильных приложений — пользователи ожидают, что приложения для загрузки и быстро выполнять. «Замороженной» экран, на котором будет отображаться перестает принимать входные данные пользователя, чтобы указать, что произошло аварийное завершение приложения, поэтому очень важно не для перегрузки поток пользовательского интерфейса с выполняющейся длительное время блокирует вызовы, такие как сетевые запросы или медленных локальных операций (например, при распаковке файла). В частности, процесс запуска не должен содержать длительно выполняемых задач — всех мобильных платформ прервет приложение, которое занимает слишком много времени, для загрузки.

Это означает, что пользовательский интерфейс должны реализовывать «индикатор выполнения» или в противном случае «возможности» пользовательский Интерфейс, который не займет много времени для отображения и асинхронные задачи, для выполнения фоновых задач. Для выполнения фоновых задач необходимо использование потоков, что означает потребностей фоновой задачи способ обмена данными в основной поток для отображения хода выполнения, или когда они будут завершены.

 <a name="Parallel_Task_Library" />

### <a name="parallel-task-library"></a>Библиотека параллельных задач

Задачи, созданные с помощью библиотеки параллельных задач может выполняться асинхронно и возвращать их вызывающем потоке, что делает их очень полезен для активации длительных операций без блокировки пользовательского интерфейса.

Операция простых параллельных задач может выглядеть следующим образом:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Ключ `TaskScheduler.FromCurrentSynchronizationContext()`, который будет повторно использовать SynchronizationContext. текущий поток, вызывающий метод (здесь главный поток, выполняющий `MainThreadMethod`), в качестве способа для обратного маршалирования вызовов к этому потоку. Это означает, что при вызове метода в потоке пользовательского интерфейса он будет выполнять `ContinueWith`ную операцию в потоке пользовательского интерфейса.

Если код запускается задачи из других потоков, используйте следующий шаблон для создания ссылки на поток пользовательского интерфейса и задачи по-прежнему может выполнять обратный вызов к нему:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />

### <a name="invoking-on-the-ui-thread"></a>Вызов в потоке пользовательского интерфейса

Для кода, не использовать библиотеку параллельных задач каждая платформа имеет свой собственный синтаксис для маршалинга операций в поток пользовательского интерфейса:

- **iOS** — `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** — `owner.RunOnUiThread(action)`
- **Xamarin. Forms** — `Device.BeginInvokeOnMainThread(action)`
- **Windows** — `Deployment.Current.Dispatcher.BeginInvoke(action)`

IOS и Android синтаксис требует класса «context» доступен в том, что означает, что код должен передать этот объект в методы, которые требуется обратный вызов в потоке пользовательского интерфейса.

Чтобы вызвать поток пользовательского интерфейса в общем коде, выполните [Пример идиспатчонуисреад](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (с помощью [@follesoe](https://twitter.com/follesoe)). Объявите и запрограммировать в интерфейсе `IDispatchOnUIThread` в общем коде, а затем реализовать классы для конкретных платформ, как показано ниже:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Разработчики Xamarin. Forms должны использовать [`Device.BeginInvokeOnMainThread`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) в общем коде (общие проекты или PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />

## <a name="platform-and-device-capabilities-and-degradation"></a>Платформы и возможности устройств и снижение

Дополнительно конкретные примеры задач, связанных с разными возможностями приведены в документации по возможностям платформы. Он имеет дело с обнаружение различные возможности и как производительность понижалась корректно приложению предоставлять эффективное взаимодействие с пользователем, даже в том случае, если приложение не может работать на свой потенциал.
