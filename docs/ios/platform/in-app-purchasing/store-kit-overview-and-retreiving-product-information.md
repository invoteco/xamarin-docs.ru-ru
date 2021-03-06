---
title: StoreKit обзор и получение сведений о продукте в Xamarin. iOS
description: В этом документе представлен обзор StoreKit. Здесь описываются классы, используемые с StoreKit, тестирование взаимодействий StoreKit, отображение продуктов для продажи, обработка недопустимых продуктов и отображение локализованных цен.
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 086dcb87f03ed4abbf3b82dc10add0f5698d52b3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032328"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>StoreKit обзор и получение сведений о продукте в Xamarin. iOS

Пользовательский интерфейс для покупки в приложении показан на снимках экрана ниже.
Перед выполнением какой бы то ни было транзакции приложение должно получить цену и описание продукта. Затем, когда пользователь нажмет кнопку " **купить**", приложение выполняет запрос к StoreKit, который управляет диалоговым окном подтверждения и именем входа Apple ID. Предполагая, что транзакция будет выполнена, StoreKit уведомляет код приложения, который должен сохранить результат транзакции и предоставить пользователю доступ к своей покупке.   

 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit notifies the application code, which must store the transaction result and provide the user with access to their purchase")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Классы

Для реализации покупок из приложения требуются следующие классы из платформы StoreKit:   
   
 **Скпродуктсрекуест** — запрос на StoreKit для продажи утвержденных продуктов (магазин приложений). Можно настроить с помощью нескольких идентификаторов продукта.

- **Скпродуктсрекуестделегате** — объявляет методы для обработки запросов и ответов продукта. 
- **Скпродуктсреспонсе** — отправляется обратно делегату из StoreKit (App Store). Содержит Скпродуктс, которые соответствуют идентификаторам продуктов, отправленным с запросом. 
- **Скпродукт** — продукт, полученный из StoreKit (настроенный в iTunes Connect). Содержит сведения о продукте, такие как идентификатор продукта, название, описание и цена. 
- **Скпаймент** — создается с идентификатором продукта и добавляется в очередь платежей для выполнения покупки. 
- **Скпайменткуеуе** — поставленные в очередь запросы на оплату, отправляемые в Apple. Уведомления активируются в результате каждого обрабатываемого платежа. 
- **Скпайменттрансактион** — представляет завершенную транзакцию (запрос на покупку, который был обработан магазином приложений и отправляется обратно в приложение через StoreKit). Транзакцию можно приобрести, восстановить или выполнить не удалось. 
- **Скпайменттрансактионобсервер** — пользовательский подкласс, реагирующий на события, создаваемые очередью платежей StoreKit. 
- **Операции StoreKit являются асинхронными** — после запуска скпродуктрекуест или добавления скпаймент в очередь Управление возвращается в код. StoreKit будет вызывать методы в подклассе Скпродуктсрекуестделегате или Скпайменттрансактионобсервер при получении данных от серверов Apple. 

На следующей схеме показаны связи между различными классами StoreKit (в приложении должны быть реализованы абстрактные классы):   

 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "The relationships between the various StoreKit classes abstract classes must be implemented in the app")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   

Эти классы более подробно описаны далее в этом документе.

## <a name="testing"></a>Тестирование

Для большинства операций StoreKit требуется реальное устройство для тестирования. Получение сведений о продукте (IE. Описание цены &amp;) будет работать в симуляторе, но операции приобретения и восстановления будут возвращать ошибку (например, код Фаиледтрансактион = 5002, что произошла неизвестная ошибка).

Примечание. StoreKit не работает в симуляторе iOS. При запуске приложения в симуляторе iOS StoreKit регистрирует предупреждение, если приложение пытается получить очередь платежей. Тестирование хранилища должно выполняться на реальных устройствах.   

Важно. не выполните вход с помощью тестовой учетной записи в приложении "Параметры". Вы можете использовать приложение параметров для выхода из любой существующей учетной записи Apple ID, а затем дождаться запроса в *вашей последовательности покупки в приложении* для входа с помощью тестового идентификатора Apple ID.   

Если вы попытаетесь войти в реальное хранилище с тестовой учетной записью, оно будет автоматически преобразовано в реальный идентификатор Apple ID. Эта учетная запись больше не будет использоваться для тестирования.

Чтобы протестировать код StoreKit, необходимо выйти из обычной тестовой учетной записи iTunes и войти с помощью специальной тестовой учетной записи (созданной в iTunes Connect), связанной с тестовым хранилищем. Чтобы выйти из текущей учетной записи, перейдите к **параметрам > iTunes и App Store** , как показано ниже:

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "To sign out of the current account visit Settings iTunes and App Store")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)

Затем войдите с помощью тестовой учетной записи *при запросе StoreKit в приложении*:

Чтобы создать тестовых пользователей в iTunes Connect, щелкните **Пользователи и роли** на главной странице.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "To create test users in iTunes Connect click on Users and Roles on the main page")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

Выбор **инженеров-испытателей "песочницы"**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "Selecting Sandbox Testers")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

Отобразится список существующих пользователей. Можно добавить нового пользователя или удалить существующую запись. Портал не (сейчас) позволяет просматривать или изменять существующих тестовых пользователей, поэтому рекомендуется сохранить хорошую запись каждого созданного тестового пользователя (особенно назначенный пароль). После удаления тестового пользователя адрес электронной почты нельзя использовать повторно для другой тестовой учетной записи.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "The list of existing users is displayed")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Новые тестовые пользователи имеют аналогичные атрибуты для реального идентификатора Apple ID (например, имя, пароль, секретный вопрос и ответ). Сохранить запись со всеми введенными здесь сведениями. Поле **Выбор магазина iTunes** определит, какая валюта и какой язык будут использоваться при входе в систему в качестве пользователя.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "The Select iTunes Store field will determine the user's currency and language for their in-app purchases")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>Получение сведений о продукте

Первым шагом в продаже продукта покупки в приложении является его отображение: получение текущей цены и описания из магазина приложений для отображения.   

Независимо от типа продуктов, которые продаются приложением (используемая, неиспользуемая или тип подписки), процесс получения сведений о продукте на экране будет одинаковым. Код Инапппурчасесампле, прилагаемый к этой статье, содержит проект с именем *расходных материалов* , демонстрирующий получение рабочей информации для отображения. В нем показано, как:

- Создайте реализацию `SKProductsRequestDelegate` и реализуйте `ReceivedResponse` абстрактный метод. В примере кода вызывается этот класс `InAppPurchaseManager`. 
- Чтобы узнать, разрешены ли платежи (с использованием `SKPaymentQueue.CanMakePayments`), проверьте с помощью StoreKit. 
- Создайте экземпляр `SKProductsRequest` с идентификаторами продуктов, которые были определены в iTunes Connect. Это делается в примере метода `InAppPurchaseManager.RequestProductData`. 
- Вызовите метод Start на `SKProductsRequest`. Это активирует асинхронный вызов серверов App Store. Делегат (`InAppPurchaseManager`) будет вызван с результатами. 
- Метод `ReceivedResponse` (`InAppPurchaseManager`) делегата обновляет пользовательский интерфейс данными, возвращенными из магазина приложений (цены на продукты & описания или сообщения о недопустимых продуктах). 

Общее взаимодействие выглядит следующим образом ( **StoreKit** встроен в iOS, а **магазин приложений** представляет серверы Apple):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Retrieving Product Information graph")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Пример отображения сведений о продукте

Пример кода [инапппурчасесампле](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) *расходных материалов* показывает, как можно получить сведения о продукте. На главном экране образца отображаются сведения о двух продуктах, получаемых из магазина приложений:   

 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "The main screen displays information products  retrieved from the App Store")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   

Пример кода для извлечения и отображения сведений о продукте более подробно описан ниже.

#### <a name="viewcontroller-methods"></a>Методы ViewController

Класс `ConsumableViewController` будет управлять отображением цен для двух продуктов, идентификаторы продуктов которых жестко закодированы в классе.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

На уровне класса также должен быть объявлен Нсобжект, который будет использоваться для настройки наблюдателя `NSNotificationCenter`:

```csharp
NSObject priceObserver;
```

В методе Виеввиллаппеар наблюдатель создается и назначается с помощью центра уведомлений по умолчанию:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

В конце метода `ViewWillAppear` вызовите метод `RequestProductData`, чтобы инициировать запрос StoreKit. После выполнения этого запроса StoreKit будет асинхронно связываться с серверами Apple, чтобы получить информацию и передать ее обратно в приложение. Это достигается подклассом `SKProductsRequestDelegate` (`InAppPurchaseManager`), который описан в следующем разделе.

```csharp
iap.RequestProductData(products);
```

Код для вывода цены и описания просто извлекает информацию из Скпродукт и назначает ее элементам управления UIKit (Обратите внимание, что мы отображаем `LocalizedTitle` и `LocalizedDescription` – StoreKit автоматически разрешает правильный текст и цены на основе Параметры учетной записи). Следующий код относится к уведомлению, созданному выше:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

Наконец, метод `ViewWillDisappear` должен обеспечить удаление наблюдателя:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>Методы Скпродуктрекуестделегате (Инапппурчасеманажер)

Метод `RequestProductData` вызывается, когда приложению нужно получить цены на продукт и другую информацию. Он выполняет синтаксический анализ коллекции идентификаторов продуктов в правильном типе данных, а затем создает `SKProductsRequest` с этими сведениями. Вызов метода Start приводит к тому, что сетевой запрос к серверам Apple будет выполнен. Запрос будет выполняться асинхронно и вызвать метод `ReceivedResponse` делегата после успешного завершения.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS автоматически направляет запрос в версию App Store "песочницу" или "Рабочая" в зависимости от того, с каким профилем подготовки выполняется приложение — поэтому при разработке или тестировании приложения запрос будет иметь доступ к каждому продукту. настроено в iTunes Connect (даже те, которые еще не были отправлены или утверждены компанией Apple). Когда приложение находится в рабочей среде, запросы StoreKit будут возвращать только сведения о **утвержденных** продуктах.   

Переопределенный метод `ReceivedResponse` вызывается после того, как серверы Apple ответили на данные. Так как этот метод вызывается в фоновом режиме, код должен проанализировать допустимые данные и использовать уведомление для отправки сведений о продукте в любой Виевконтроллерс, который прослушивает это уведомление. Ниже приведен код для получения действительных сведений о продукте и отправки уведомления.

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

Несмотря на то, что не показано на схеме, метод `RequestFailed` также должен быть переопределен, чтобы вы могли предоставить пользователю отзыв о том, что серверы магазина приложений недоступны (или возникает какая-либо другая ошибка). В примере кода просто выполняется запись в консоль, но реальное приложение может запросить `error.Code` свойство и реализовать пользовательское поведение (например, оповещение для пользователя).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

На этом снимке экрана показан пример приложения сразу после загрузки (если нет доступных сведений о продукте):

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "The sample app immediately after loading when no product information is available")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Недопустимые продукты

`SKProductsRequest` также может возвращать список недопустимых идентификаторов продуктов. Недопустимые продукты обычно возвращаются по одной из следующих способов:   

**Идентификатор продукта был неправильно типизирован** — принимаются только допустимые идентификаторы продуктов.   
   
 **Продукт не утвержден** — во время тестирования все продукты, которые были удалены для продажи, должны возвращаться `SKProductsRequest`; Однако в рабочей среде возвращаются только утвержденные продукты.   
   
 **Идентификатор приложения не является явным** — идентификаторы приложений с подстановочными знаками (со звездочкой) не позволяют покупать в приложении.   
   
 **Неверный профиль подготовки** — при внесении изменений в конфигурацию приложения на портале подготовки (например, при включении покупок в приложении) не забудьте повторно создать и использовать правильный профиль подготовки при создании приложения.   
   
 **контракт платных приложений iOS не на месте** — функции StoreKit не будут работать вообще, если для вашей учетной записи разработчика Apple нет действующего контракта.   
   
 **Двоичный файл находится в состоянии "отклонено** " — Если имеется ранее отправленный двоичный файл в состоянии "Отклонено" (командой App Store или разработчиком), то функции StoreKit не будут работать.

Метод `ReceivedResponse` в примере кода выводит на консоль недопустимые продукты:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Отображение локализованных цен

Ценовые категории указывают определенную цену для каждого продукта во всех международных магазинах приложений. Чтобы обеспечить правильное отображение цен для каждой валюты, используйте следующий метод расширения (определенный в `SKProductExtension.cs`), а не свойство Price каждого `SKProduct`.

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

Код, задающий заголовок кнопки, использует метод расширения следующим образом:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Использование двух различных тестовых учетных записей iTunes (один для магазина США и One для магазина для японского языка) приводит к следующим снимкам экрана:   

 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "Two different iTunes test accounts showing language specific results")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   

Обратите внимание, что магазин влияет на язык, используемый для сведений о продукте и валюту цены, в то время как языковой параметр устройства влияет на метки и другое локализованное содержимое.   

Напомним, что для использования другой тестовой учетной записи необходимо **выйти** из **параметров > iTunes и App Store** , а затем перезапустить приложение для входа с другой учетной записью. Чтобы изменить язык устройства, перейдите в раздел **параметры > общие > международный язык >** .
