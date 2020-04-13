---
title: Authenticate пользователей с Azure Космос DB База данных документов и Xamarin.Forms
description: В этой статье объясняется, как объединить элемент управления доступом с разделенными коллекциями Azure Cosmos DB, чтобы пользователь мог получить доступ к собственным документам только в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e79e647d64103b6d257de7233f488899bcaff40
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388607"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Authenticate пользователей с Azure Космос DB База данных документов и Xamarin.Forms

[![Скачать](~/media/shared/download.png) образец Скачать образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Базы данных документов Azure Cosmos DB поддерживают разделенные коллекции, которые могут охватывать несколько серверов и разделов, поддерживая при этом неограниченное хранение и пропускную связь. В этой статье объясняется, как объединить элемент управления доступом с разделенными коллекциями, чтобы пользователь мог получить доступ к своим собственным документам только в приложении Xamarin.Forms._

## <a name="overview"></a>Обзор

При создании разделительной коллекции необходимо указать ключ раздела, а документы с тем же ключом раздела будут храниться в одном и том же разделите. Таким образом, указание личности пользователя в качестве ключа раздела приведет к разделению коллекции, которая будет хранить только документы для этого пользователя. Это также гарантирует масштабирование базы данных документов Azure Cosmos DB по мере увеличения числа пользователей и элементов.

Доступ должен быть предоставлен к любой коллекции, а модель управления доступом к API S'L определяет два типа конструкций доступа:

- **Мастер-ключи** обеспечивают полный административный доступ ко всем ресурсам в учетной записи Cosmos DB и создаются при создании учетной записи Cosmos DB.
- **Токены ресурсов** фиксируют связь между пользователем базы данных и разрешением, полученное пользователем для определенного ресурса Cosmos DB, например коллекции или документа.

Разоблачение основного ключа открывает учетную запись Cosmos DB для возможности злонамеренного или небрежного использования. Однако токены ресурсов Azure Cosmos DB обеспечивают безопасный механизм, позволяющий клиентам читать, писать и удалять определенные ресурсы в учетной записи Azure Cosmos DB в соответствии с предоставленными разрешениями.

Типичный подход к запросу, генерации и доставке токенов ресурсов в мобильное приложение заключается в использовании брокера маркеров ресурсов. На следующей диаграмме показан обзор на высоком уровне того, как примерное приложение использует брокера маркеров ресурсов для управления доступом к данным базы данных документов:

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

Брокер токенов ресурсов — это сервис Web API среднего уровня, размещенный в Службе приложений Azure, который обладает основным ключом учетной записи Cosmos DB. В примерном приложении используется брокер маркеров ресурсов для управления доступом к данным базы данных документов следующим образом:

1. При входе в систему входа приложение Xamarin.Forms связывается с службой приложений Azure, чтобы инициировать поток аутентификации.
1. Служба приложений Azure выполняет поток аутентификации OAuth с помощью Facebook. После завершения потока проверки подлинности приложение Xamarin.Forms получает токен доступа.
1. Приложение Xamarin.Forms использует токен доступа для запроса токена ресурса у брокера токенов ресурсов.
1. Брокер маркеров ресурсов использует токен доступа для запроса личности пользователя в Facebook. Идентификация пользователя затем используется для запроса токена ресурса от Cosmos DB, который используется для предоставления доступа к чтению/записи к разделизированной коллекции подлинного пользователя.
1. Приложение Xamarin.Forms использует токен ресурса для прямого доступа к ресурсам Cosmos DB с разрешениями, определенными токеном ресурса.

> [!NOTE]
> По истечении срока действия маркера ресурса последующие запросы базы данных документов получат 401 несанкционированное исключение. На этом этапе приложения Xamarin.Forms должны восстановить идентификацию и запросить новый маркер ресурса.

Для получения дополнительной информации о разделе [How to partition and scale in Azure Cosmos DB](/azure/cosmos-db/partition-data/)Cosmos DB см. Для получения дополнительной информации о контроле доступа Cosmos DB [Access control in the SQL API](/rest/api/documentdb/access-control-on-documentdb-resources/) [см.](/azure/cosmos-db/secure-access-to-data/)

## <a name="setup"></a>Настройка

Процесс интеграции брокера токенов ресурсов в приложение Xamarin.Forms:

1. Создайте учетную запись Cosmos DB, которая будет использовать элемент управления доступом. Для получения дополнительной информации [см.](#cosmosdb_configuration)
1. Создайте службу приложений Azure для размещения брокера токенов ресурсов. Для получения дополнительной информации [см.](#app_service_configuration)
1. Создайте приложение Facebook для выполнения аутентификации. Для получения дополнительной информации [см.](#facebook_configuration)
1. Настройте службу приложений Azure для простой аутентификации с помощью Facebook. Для получения дополнительной [Azure App Service Authentication Configuration](#app_service_authentication_configuration)информации см.
1. Настройте образец приложения Xamarin.Forms для связи с Azure App Service и Cosmos DB. Для получения дополнительной [Xamarin.Forms Application Configuration](#forms_application_configuration)информации см.

> [!NOTE]
> Если у вас нет [подписки Azure,](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)создайте [бесплатную учетную запись](https://aka.ms/azfree-docs-mobileapps) перед началом.

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Конфигурация Azure Космос DB

Процесс создания учетной записи Cosmos DB, которая будет использовать элемент управления доступом, заключается в следующем:

1. Создайте учетную запись Cosmos DB. Для получения дополнительной информации [см.](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)
1. В учетной записи Cosmos DB `UserItems`создайте новую коллекцию `/userid`с именем , указав ключ раздела .

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Конфигурация службы приложений Azure

Процесс размещения брокера токенов ресурсов в Azure App Service следующий:

1. На портале Azure создайте новое веб-приложение Службы приложений. Для получения дополнительной информации смотрите [Создать веб-приложение в среде службы приложений](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. На портале Azure откройте лезвие настроек приложений для веб-приложения и добавьте следующие настройки:
    - `accountUrl`- значение должно быть COSMOs DB счет URL из ключей лезвие счета Космос DB.
    - `accountKey`Значение должно быть мастер-ключом Cosmos DB (первичным или вторичным) из лезвия ключей учетной записи Cosmos DB.
    - `databaseId`— значение должно быть названием базы данных Cosmos DB.
    - `collectionId`- значение должно быть название коллекции Космос DB (в данном случае, `UserItems`).
    - `hostUrl`— значение должно быть URL веб-приложения из лезвия службы «Обзор» учетной записи Службы app.

    Следующий скриншот демонстрирует эту конфигурацию:

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. Опубликуйте решение для маркеров ресурсов для веб-приложения Azure App Service.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Конфигурация приложения Facebook

Процесс создания приложения Facebook для выполнения аутентификации:

1. Создайте приложение Facebook. Для получения дополнительной информации можно [ознакомиться на сайте Register and Configure App](https://developers.facebook.com/docs/apps/register) в Центре разработчиков Facebook.
1. Добавьте в приложение продукт Facebook Login. Для получения дополнительной информации смотрите [Добавление входа facebook в ваше приложение или веб-сайт](https://developers.facebook.com/docs/facebook-login) в Центре разработчиков Facebook.
1. Назначай Facebook Login следующим образом:
   - Включить клиент OAuth Войти.
   - Включить Web OAuth Войти.
   - Установите действительное OAuth перенаправить URI в URI веб-приложения Службы App, с `/.auth/login/facebook/callback` приложением.

  Следующий скриншот демонстрирует эту конфигурацию:

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

Для получения дополнительной информации, смотрите [Регистрация приложения на Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Конфигурация аутентификации службы приложений Azure

Процесс настройки простой аутентификации Службы Приложений заключается в следующем:

1. На портале Azure перейдите к веб-приложению Службы app.
1. На портале Azure откройте лезвие аутентификации/авторизации и выполните следующую конфигурацию:
    - Аутентификация службы приложений должна быть включена.
    - Действие, необходимое для проверки подлинности запроса, должно быть установлено в **Facebook.**

    Следующий скриншот демонстрирует эту конфигурацию:

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

Веб-приложение Службы App также должно быть настроено для связи с приложением Facebook, чтобы обеспечить поток аутентификации. Это может быть достигнуто путем выбора поставщика идентификационных данных Facebook и ввода **идентификатора приложений** и значений **App Secret** из настроек приложения Facebook в Центре разработчиков Facebook. Для получения дополнительной информации смотрите [Добавить информацию о Facebook в ваше приложение](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Конфигурация приложения Xamarin.Forms

Процесс настройки применения образца Xamarin.Forms заключается в следующем:

1. Откройте решение Xamarin.Forms.
1. Откройте `Constants.cs` и обновите значения следующих констант:
    - `EndpointUri`- значение должно быть COSMOs DB счет URL из ключей лезвие счета Космос DB.
    - `DatabaseName`— значение должно быть названием базы данных документов.
    - `CollectionName`— значение должно быть именем сбора базы данных `UserItems`документов (в данном случае).
    - `ResourceTokenBrokerUrl`- значение должно быть URL веб-приложения для брокера токенов ресурсов из лезвия обзора учетной записи Службы приложений.

## <a name="initiating-login"></a>Ивнимание входа

Пример приложения инициирует процесс входа, перенаправляя браузер на URL-адрес поставщика идентификационных данных, как показано в следующем примере кода:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Это приводит к тому, что поток аутентификации OAuth был инициирован между службой приложений Azure и Facebook, которая отображает страницу входа в Facebook:

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

Логин может быть отменен, нажав кнопку **Отмена** на iOS или нажав кнопку **"Назад"** на Android, и в этом случае пользователь остается неаутвированным и пользовательский интерфейс поставщика идентификационных данных удаляется с экрана.

## <a name="obtaining-a-resource-token"></a>Получение токена ресурсов

После успешной аутентификации `WebRedirectAuthenticator.Completed` событие загорает. Следующий пример кода демонстрирует обработку этого события:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Результатом успешной аутентификации является токен `AuthenticatorCompletedEventArgs.Account` доступа, который является доступным свойством. Токен доступа извлекается и используется в запросе GET на `resourcetoken` API брокера токенов.

`resourcetoken` API использует токен доступа для запроса идентификации пользователя в Facebook, который, в свою очередь, используется для запроса токена ресурса от Cosmos DB. Если действительный документ разрешения уже существует для пользователя в базе данных документов, он извлекается и документ JSON, содержащий токен ресурса, возвращается в приложение Xamarin.Forms. Если допустимый документ разрешения не существует для пользователя, в базе данных документов создается пользователь и разрешение, а маркер ресурса извлекается из документа разрешения и возвращается в приложение Xamarin.Forms в документе JSON.

> [!NOTE]
> Пользователь базы данных документов — это ресурс, связанный с базой данных документов, и каждая база данных может содержать ноль или больше пользователей. Разрешение базы данных документов — это ресурс, связанный с пользователем базы данных документов, и каждый пользователь может содержать ноль или более разрешений. Ресурс разрешения предоставляет доступ к маркеру безопасности, который требуется пользователю при попытке получить доступ к ресурсу, например документу.

Если `resourcetoken` API успешно завершает работу, он отправит в ответ код статуса HTTP 200 (OK) вместе с документом JSON, содержащим токен ресурса. Следующие данные JSON показывают типичное успешное сообщение ответа:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

Обработчик `WebRedirectAuthenticator.Completed` событий читает `resourcetoken` ответ от API и извлекает маркер ресурса и идентификатор пользователя. Затем маркер ресурса передается в `DocumentClient` качестве аргумента конструктору, который инкапсулирует конечную точку, учетные данные и политику соединения, используемую для доступа к Cosmos DB, и используется для настройки и выполнения запросов против Cosmos DB. Токен ресурса отправляется с каждым запросом на прямой доступ к ресурсу и указывает, что доступ к разделу подлинной коллекции пользователей, удостоверяющимся в подлинной проверке, предоставляется.

## <a name="retrieving-documents"></a>Получение документов

Извлечение документов, принадлежащих только подлинному пользователю, может быть достигнуто путем создания запроса документа, который включает идентификатор пользователя в качестве ключа раздела и демонстрируется в следующем примере кода:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Запрос асинхронно извлекает все документы, принадлежащие аутентифицированному пользователю, `List<TodoItem>` из указанной коллекции и помещает их в коллекцию для отображения.

Метод `CreateDocumentQuery<T>` определяет аргумент, `Uri` представляющий коллекцию, который должен быть запрошен `FeedOptions` для документов, и объекта. Объект `FeedOptions` указывает, что неограниченное количество элементов может быть возвращено запросом, а идентификатор пользователя — ключом раздела. Это гарантирует возврат только документов в раздельном сборе пользователей.

> [!NOTE]
> Обратите внимание, что документы разрешения, созданные брокером маркеров ресурсов, хранятся в том же наборе документов, что и документы, созданные приложением Xamarin.Forms. Таким образом, запрос документа `Where` содержит положение, которое применяет предикат фильтрации к запросу против сбора документов. Это положение гарантирует, что документы разрешения не будут возвращены из коллекции документов.

Для получения дополнительной информации о извлечении документов из коллекции документов [см.](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)

## <a name="inserting-documents"></a>Вставка документов

Перед вводом документа в сбор `TodoItem.UserId` документов свойство должно быть обновлено со значением, используемым в качестве ключа раздела, как показано в следующем примере кода:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Это гарантирует, что документ будет вставлен в раздельную коллекцию пользователя.

Для получения дополнительной информации о включении [Inserting a Document into a Document Collection](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)документа в сбор документов см.

## <a name="deleting-documents"></a>Удаляющие документы

Значение ключа раздела должно быть указано при удалениех документа из раздельного сбора, как показано в следующем примере кода:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Это гарантирует, что Cosmos DB знает, из какого раздела коллекции удалить документ.

Для получения дополнительной информации об исначиная [Deleting a Document from a Document Collection](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)документ из коллекции документов см.

## <a name="summary"></a>Сводка

В этой статье объясняется, как объединить элемент управления доступом с разделенными коллекциями, чтобы пользователь мог получить доступ только к своим документам базы данных документов в приложении Xamarin.Forms. Указание личности пользователя в качестве ключа раздела гарантирует, что разделительная коллекция может хранить только документы для этого пользователя.

## <a name="related-links"></a>Связанные ссылки

- [Тодо Лазурный Космос DB Auth (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Использование базы данных документов Azure Cosmos DB](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Защита доступа к данным Azure Cosmos DB](/azure/cosmos-db/secure-access-to-data/)
- [Контроль доступа в API S'L](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Секционирование и масштабирование в базе данных Azure Cosmos DB](/azure/cosmos-db/partition-data/)
- [Библиотека клиентов Azure Cosmos DB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn948556.aspx)
