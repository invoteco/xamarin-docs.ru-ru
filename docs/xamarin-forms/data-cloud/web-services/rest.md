---
title: Использование веб-службу RESTful
description: Интеграция веб-службы в приложение является распространенным сценарием. В этой статье показано, как использовать веб-службу RESTful в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: adfc3247bce4d40d73580d6ca9baa31df4f57229
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658971"
---
# <a name="consume-a-restful-web-service"></a>Использование веб-службу RESTful

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)

_Интеграция веб-службы в приложение является распространенным сценарием. В этой статье показано, как использовать веб-службу RESTful в приложении Xamarin.Forms._

Representational State Transfer (REST) представляет собой архитектурный стиль для построения веб-служб. REST запросы выполняются по протоколу HTTP, используя те же глаголы HTTP, использующих веб-браузеры для получения веб-страниц и для отправки данных на серверы. Ниже приведены команды.

- **ПОЛУЧИТЬ** — эта операция используется для получения данных из веб-службы.
- **POST** — эта операция используется для создания нового элемента данных на веб-службы.
- **ПОМЕСТИТЕ** — эта операция используется для обновления элемента данных на веб-службы.
- **ИСПРАВЛЕНИЕ** — эта операция используется для обновления элемента данных на веб-службы, описывающий набор инструкций о том, как изменить элемент. Эта команда не используется в примере приложения.
- **Удалить** — эта операция используется для удаления элемента данных на веб-службы.

Веб-служба, вызываются API-интерфейсов RESTful API-интерфейсы, которые соответствуют REST и определяются с помощью:

- Базовый URI.
- Методы HTTP, такие как GET, POST, PUT, PATCH или DELETE.
- Тип носителя для данных, таких как JavaScript Object Notation (JSON).

Сообщения JSON обычно использовать веб-служб rESTful для возврата данных клиенту. JSON — это формат обмена данными в текстовых, что создает compact полезных данных, что приводит к сократить требования к пропускной способности при отправке данных. В примере приложения используется открытым [библиотеки NewtonSoft JSON.NET](http://www.newtonsoft.com/json) для сериализации и десериализации сообщений.

Простота REST сделала его основной метод для доступа к веб-служб в мобильных приложениях.

При запуске примера приложения, он будет подключаться к локально размещенной службы REST, как показано на следующем снимке экрана:

![](rest-images/portal.png "Пример приложения")

> [!NOTE]
> В iOS 9 и более поздней версии приложение Transport Security (ATS) обеспечивает безопасных соединений между Интернет-ресурсов (например, server серверной части приложения) и приложения, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут применяться требования к безопасности ATS. Если соединения не удовлетворяют этим требованиям, они вызовут сбой с исключением.
>
>Если невозможно использовать из могут быть присоединены ATS **HTTPS** протокола и безопасный обмен данными для Интернет-ресурсов. Это достигается путем обновления приложения **Info.plist** файла. Дополнительные сведения см. в разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Использование веб-службы

Служба REST написан с помощью ASP.NET Core и предоставляет следующие операции:

|Операция|Метод HTTP|Относительный URI|Параметры|
|--- |--- |--- |--- |
|Получение списка элементов задач|GET|/ API/todoitems /|
|Создать новый элемент списка дел|ПОМЕСТИТЬ|/ API/todoitems /|TodoItem, отформатированное по JSON|
|Обновление элемента задачи|PUT|/ API/todoitems /|TodoItem, отформатированное по JSON|
|Удаление элемента задачи|DELETE|/ API/todoitems / {id}|

Большинство из URI включают `TodoItem` идентификатор в путь. Например, чтобы удалить `TodoItem` которого является идентификатор `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, клиент отправляет запрос DELETE к `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Дополнительные сведения о модели данных, используемой в приложении-примере, см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

Когда платформа веб-API получает запрос он направляет запрос к действию. Эти действия являются простыми открытыми методами в `TodoItemsController` класса. Инфраструктура использует таблицу маршрутизации, чтобы определить, какие действие, которое вызывается в ответ на запрос, который показан в следующем примере кода:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Таблица маршрутизации содержит шаблон маршрута, а когда платформа веб-API получает запрос HTTP, она пытается сопоставить URI с шаблоном маршрута в таблице маршрутизации. Если соответствующий маршрут не найдена, клиент получает ошибку 404 (не найдено). При обнаружении подходящего маршрута веб-API выбирает контроллер и действие следующим образом:

- Чтобы найти контроллер, веб-API добавляет «controller» к значению *{controller}* переменной.
- Чтобы найти действия, веб-API проверяет метод HTTP и рассматривает действия контроллера, оформляются с использованием тот же метод HTTP, как атрибут.
- *{Id}* переменной-заполнителе сопоставляется параметра действия.

Служба REST использует обычную проверку подлинности. Дополнительные сведения см. в разделе [проверки подлинности веб-службу RESTful](~/xamarin-forms/data-cloud/authentication/rest.md). Дополнительные сведения о маршрутизации веб-API ASP.NET, см. в разделе [маршрутизации в ASP.NET Web API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) на веб-сайте ASP.NET. Дополнительные сведения о создании службы REST, с помощью ASP.NET Core см. в разделе [Создание серверных служб для собственных мобильных приложений](/aspnet/core/mobile/native-mobile-backend/).

`HttpClient` Класс используется для отправки и получения запросов по протоколу HTTP. Он предоставляет функциональные возможности для отправки HTTP-запросов и получения HTTP-ответов из URI определен ресурс. Каждый запрос отправляется в качестве асинхронной операции. Дополнительные сведения об асинхронных операциях см. в разделе [обзор поддержки асинхронного](~/cross-platform/platform/async.md).

`HttpResponseMessage` Класс представляет сообщение ответа HTTP, полученные из веб-службы, после HTTP-запроса. Он содержит сведения об ответе, включая код состояния, заголовков и любой текст. `HttpContent` Класс представляет тело HTTP и заголовки содержимого, таких как `Content-Type` и `Content-Encoding`. Можно прочитать содержимое с помощью любого из `ReadAs` методы, такие как `ReadAsStringAsync` и `ReadAsByteArrayAsync`, в зависимости от формата данных.

### <a name="creating-the-httpclient-object"></a>Создание объекта HTTPClient

`HttpClient` Экземпляр объявлен на уровне класса, что объект существует до тех пор, пока приложение должно выполнять запросы HTTP, как показано в следующем примере кода:

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    _client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>Получение данных

`HttpClient.GetAsync` Метод используется для отправки запроса GET в веб-службу, указанного в URI и последующего получения ответа от веб-службы, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  var response = await _client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Служба REST отправляет код состояния HTTP `HttpResponseMessage.IsSuccessStatusCode` свойство, чтобы указать, успешно ли выполнено HTTP-запроса. Для этой операции REST служба отправляет код состояния HTTP 200 (ОК) в ответ, который указывает, что запрос выполнен успешно и запрашиваемые данные в ответе.

Если выполнено успешно, HTTP-операции чтения содержимого ответа, для отображения. `HttpResponseMessage.Content` Свойство представляет содержимое HTTP-ответа и `HttpContent.ReadAsStringAsync` метод асинхронно записывает содержимое HTTP в строку. Это содержимое затем преобразуется из формата JSON для `List` из `TodoItem` экземпляров.

### <a name="creating-data"></a>Создание данных

`HttpClient.PostAsync` Метод используется для отправки запроса POST к веб-службе, указанного в URI, а затем для получения ответа от веб-службы, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await _client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");

  }
  ...
}
```

`TodoItem` Экземпляр преобразуется в полезные данные JSON для отправки в веб-службу. Эти полезные данные включается в тексте HTTP-содержимое, которое будет отправляться в веб-службу, перед выполнением запроса с `PostAsync` метод.

Служба REST отправляет код состояния HTTP `HttpResponseMessage.IsSuccessStatusCode` свойство, чтобы указать, успешно ли выполнено HTTP-запроса. Ниже приведены распространенные ответы для этой операции.

- **201 (СОЗДАНО)** — запрос привел к созданию нового ресурса перед отправкой ответа.
- **400 (НЕПРАВИЛЬНЫЙ запрос)** — запрос не понят сервером.
- **409 (конфликт)** — запрос не может быть выполнены из-за конфликта на сервере.

### <a name="updating-data"></a>Обновление данных

`HttpClient.PutAsync` Метод используется для отправки запроса PUT в веб-службу, указанного в URI и затем получить ответ из веб-службы, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await _client.PutAsync (uri, content);
  ...
}
```
Операция `PutAsync` метод идентичен методу `PostAsync` метод, используемый для создания данных в веб-службы. Тем не менее отличаются возможные ответы, отправленные из веб-службы.

Служба REST отправляет код состояния HTTP `HttpResponseMessage.IsSuccessStatusCode` свойство, чтобы указать, успешно ли выполнено HTTP-запроса. Ниже приведены распространенные ответы для этой операции.

- **204 (нет СОДЕРЖИМОГО)** — запрос был успешно обработан и ответ является преднамеренно пустым.
- **400 (НЕПРАВИЛЬНЫЙ запрос)** — запрос не понят сервером.
- **404 (не НАЙДЕНО)** — запрошенный ресурс не существует на сервере.

### <a name="deleting-data"></a>Удаление данных

`HttpClient.DeleteAsync` Метод используется для отправки запроса на удаление веб-службу, указанного в URI и затем получить ответ от веб-службы, как показано в следующем примере кода:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  var response = await _client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

Служба REST отправляет код состояния HTTP `HttpResponseMessage.IsSuccessStatusCode` свойство, чтобы указать, успешно ли выполнено HTTP-запроса. Ниже приведены распространенные ответы для этой операции.

- **204 (нет СОДЕРЖИМОГО)** — запрос был успешно обработан и ответ является преднамеренно пустым.
- **400 (НЕПРАВИЛЬНЫЙ запрос)** — запрос не понят сервером.
- **404 (не НАЙДЕНО)** — запрошенный ресурс не существует на сервере.

## <a name="related-links"></a>Связанные ссылки

- [Создание серверных служб для собственных мобильных приложений](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
