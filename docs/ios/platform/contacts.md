---
title: Контакты и ContactsUI в Xamarin. iOS
description: В этой статье рассматривается работа с новыми платформами для контактов и контактов в приложении Xamarin. iOS. Эти платформы заменяют существующую адресную книгу и пользовательский интерфейс адресной книги, используемые в предыдущих версиях iOS.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 438ed93bafa37496e6a97ea2fe98ca6a515682cf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032561"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>Контакты и ContactsUI в Xamarin. iOS

_В этой статье рассматривается работа с новыми платформами для контактов и контактов в приложении Xamarin. iOS. Эти платформы заменяют существующую адресную книгу и пользовательский интерфейс адресной книги, используемые в предыдущих версиях iOS._

С появлением iOS 9 Компания Apple выпустила две новые платформы, `Contacts` и `ContactsUI`, которые заменяют существующие адресные книги и платформы, используемые в ОС iOS 8 и более ранних версий.

Две новые платформы содержат следующие функциональные возможности:

- [**Контакты**](#contacts) — предоставляет доступ к данным списка контактов пользователя.
  Так как большинству приложений требуется только доступ только для чтения, эта платформа оптимизирована для поточного доступа только для чтения.

- [**ContactsUI**](#contactsui) — предоставляет элементы пользовательского интерфейса Xamarin. iOS для просмотра, изменения, выбора и создания контактов на устройствах iOS.

[![](contacts-images/add01.png "An example Contact Sheet on an iOS device")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Существующие платформы `AddressBook` и `AddressBookUI`, используемые в iOS 8 (и более ранних версиях), являются устаревшими в iOS 9 и должны быть заменены новыми `Contacts` и `ContactsUI` платформами как можно скорее для любого существующего приложения Xamarin. iOS. Новые приложения должны быть написаны для новых платформ.

В следующих разделах мы рассмотрим эти новые платформы и способы их реализации в приложении Xamarin. iOS.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Платформа контактов

Платформа контактов предоставляет Xamarin. iOS доступ к контактным данным пользователя. Так как большинству приложений требуется только доступ только для чтения, эта платформа оптимизирована для поточного доступа только для чтения.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Объекты контактов

Класс `CNContact` предоставляет потокобезопасный доступ с доступом только для чтения к свойствам контакта, таким как имя, адрес или номера телефонов. `CNContact` функции, такие как `NSDictionary`, и содержит несколько коллекций свойств, которые доступны только для чтения (например, адреса или номера телефонов):

[![](contacts-images/contactobjects.png "Contact Object overview")](contacts-images/contactobjects.png#lightbox)

Для любого свойства, которое может иметь несколько значений (например, адрес электронной почты или номера телефонов), они будут представлены в виде массива объектов `NSLabeledValue`. `NSLabeledValue` — это потокобезопасный кортеж, состоящий из набора меток и значений, предназначенных только для чтения, в котором метка определяет значение для пользователя (например, домашняя или Рабочая электронная почта). Платформа Contacts предоставляет набор стандартных меток (через статические классы `CNLabelKey` и `CNLabelPhoneNumberKey`), которые можно использовать в приложении, или можно определить пользовательские метки в соответствии с вашими потребностями.

Для любого приложения Xamarin. iOS, которое должно корректировать значения существующего контакта (или создать новые), используйте `NSMutableContact` версию класса и его подклассы (например, `CNMutablePostalAddress`).

Например, следующий код создаст новый контакт и добавит его в коллекцию контактов пользователя:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Если этот код выполняется на устройстве iOS 9, в коллекцию пользователя будет добавлен новый контакт. Пример:

[![](contacts-images/add01.png "A new contact added to the user's collection")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Форматирование и локализация контактов

Платформа Contacts содержит несколько объектов и методов, которые могут помочь в форматировании и локализации содержимого для показа пользователю. Например, следующий код правильно отформатирует имя контакта и почтовый адрес для вывода:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Для меток свойств, которые будут отображаться в пользовательском интерфейсе приложения, платформа Contact Framework имеет методы для локализации этих строк. Опять же, это основано на текущем языковом стандарте устройства iOS, на котором выполняется приложение. Пример:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Получение существующих контактов

Используя экземпляр класса `CNContactStore`, можно получить контактную информацию из базы данных Contacts пользователя. `CNContactStore` содержит все методы, необходимые для выборки или обновления контактов и групп из базы данных. Поскольку эти методы являются синхронными, рекомендуется запускать их в фоновом потоке, чтобы предотвратить блокировку пользовательского интерфейса.

Используя предикаты (построенные на основе класса `CNContact`), можно отфильтровать результаты, возвращаемые при отборе контактов из базы данных. Чтобы получить только контакты, содержащие строку `Appleseed`, используйте следующий код:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Универсальные и составные предикаты не поддерживаются платформой Contacts.

Например, чтобы ограничить выборку только свойствами **givenName** и **FamilyName** контакта, используйте следующий код:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Наконец, чтобы найти базу данных и вернуть результаты, используйте следующий код:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Если этот код был выполнен после примера, созданного в разделе **объекта Contacts** выше, он вернет только что созданный контакт «Джон апплесид».

### <a name="contact-access-privacy"></a>Доступ к конфиденциальности доступа

Так как конечные пользователи могут предоставлять или запрещать доступ к своим контактным сведениям отдельно для каждого приложения, при первом вызове `CNContactStore`откроется диалоговое окно с запросом на разрешение доступа к приложению.

Запрос на разрешение будет представлен только один раз, при первом запуске приложения и последующих запусках или вызовах `CNContactStore` будет использовать разрешение, выбранное пользователем в это время.

Приложение должно быть разработано таким образом, чтобы оно корректно обрабатывал пользователя, запрещающего доступ к своей базе данных контактов.

#### <a name="fetching-partial-contacts"></a>Получение частичных контактов

_Частичный контакт_ — это контакт, который извлекает только некоторые из доступных свойств из магазина контактов для. При попытке доступа к свойству, которое ранее не было получено, это приведет к исключению.

Можно легко проверить, есть ли у данного контакта требуемое свойство, используя методы `IsKeyAvailable` или `AreKeysAvailable` экземпляра `CNContact`. Пример:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> Методы `GetUnifiedContact` и `GetUnifiedContacts` класса `CNContactStore` возвращают _только_ часть контакта, ограниченную свойствами, запрашиваемыми предоставленными ключами FETCH.

### <a name="unified-contacts"></a>Объединенные контакты

У пользователя могут быть разные источники контактных данных для одного пользователя в своей базе данных контактов (например, iCloud, Facebook или Google Mail). В приложениях iOS и OS X эти контактные данные автоматически связываются друг с другом и отображаются для пользователя как единое _единое Контактное лицо_.

[![](contacts-images/unified01.png "Unified Contacts overview")](contacts-images/unified01.png#lightbox)

Этот объединенный контакт представляет собой временное представление контактной информации по ссылке в памяти, которому будет присвоен собственный уникальный идентификатор (который должен использоваться для повторной выборки контакта при необходимости). По умолчанию платформа Contacts будет по возможности возвращать единый контакт.

### <a name="creating-and-updating-contacts"></a>Создание и обновление контактов

Как мы видели в разделе [Contact Objects](#Contact_Objects) выше, вы используете `CNContactStore` и экземпляр `CNMutableContact` для создания новых контактов, которые затем записываются в базу данных контактов пользователя с помощью `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

`CNSaveRequest` также можно использовать для кэширования нескольких изменений контактов и групп в одной операции и пакетной обработки изменений в `CNContactStore`.

Чтобы обновить неизменяемый контакт, полученный из операции выборки, сначала необходимо запросить изменяемую копию, которую затем изменить, и сохранить ее в хранилище контактов. Пример:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Уведомления об изменениях контактов

При каждом изменении контакта хранилище контактов отправляет `CNContactStoreDidChangeNotification` в центр уведомлений по умолчанию. Если вы выполнили кэширование или в настоящее время отображает контакты, необходимо обновить эти объекты из хранилища контактов (`CNContactStore`).

### <a name="containers-and-groups"></a>Контейнеры и группы

Контакты пользователя могут существовать либо локально на устройстве пользователя, либо как контакты, синхронизированные с устройством из одной или нескольких учетных записей сервера (например, Facebook или Google). Каждый пул контактов имеет собственный _контейнер_ , и данный контакт может существовать только в одном контейнере.

[![](contacts-images/containers01.png "Containers and Groups overview")](contacts-images/containers01.png#lightbox)

Некоторые контейнеры позволяют упорядочивать контакты в одну или несколько _групп_ или _подгрупп_. Это поведение зависит от резервного хранилища для данного контейнера. Например, iCloud имеет только один контейнер, но может иметь много групп (но не содержит подгрупп). Microsoft Exchange с другой стороны, не поддерживает группы, но может иметь несколько контейнеров (по одному для каждой папки Exchange).

[![](contacts-images/containers02.png "Overlap within Containers and Groups")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Платформа ContactsUI

В ситуациях, когда приложению не требуется предоставлять настраиваемый пользовательский интерфейс, можно использовать платформу ContactsUI для представления элементов пользовательского интерфейса для отображения, редактирования, выбора и создания контактов в приложении Xamarin. iOS.

Используя встроенные элементы управления Apple, вы не только уменьшаете объем кода, который нужно создать для поддержки контактов в приложении Xamarin. iOS, но предоставляете единообразный интерфейс для пользователей приложения.

### <a name="the-contact-picker-view-controller"></a>Контроллер представления средства выбора контактов

Контроллер представления средства выбора контактов (`CNContactPickerViewController`) управляет стандартным представлением средства выбора контактов, которое позволяет пользователю выбрать контакт или свойство контакта из базы данных контактов пользователя. Пользователь может выбрать одного или нескольких контактов (в зависимости от его использования), а контроллер представления средства выбора контактов не запрашивает разрешение перед отображением средства выбора.

Перед вызовом класса `CNContactPickerViewController` необходимо определить, какие свойства пользователь может выбирать и определять предикаты для управления отображением и выбором свойств контакта.

Используйте экземпляр класса, который наследует от `CNContactPickerDelegate`, чтобы реагировать на взаимодействие пользователя с помощью средства выбора. Пример:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Чтобы разрешить пользователю выбирать адрес электронной почты из контактов в своей базе данных, можно использовать следующий код:

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Контроллер представления контактов

Класс представления Contact Controller (`CNContactViewController`) предоставляет контроллер для отображения стандартного представления контактов для конечного пользователя. Представление Contact может отображать новые, неизвестные или существующие контакты, и тип необходимо указать перед отображением представления, вызвав правильный статический конструктор (`FromNewContact`, `FromUnknownContact`, `FromContact`). Пример.

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с контактными данными и обращение к платформам пользовательского интерфейса в приложении Xamarin. iOS. Во-первых, в нем были рассмотрены различные типы объектов, предоставляемых контактной платформой, и способы их использования для создания новых контактов или доступа к существующим контактам. Кроме того, мы рассмотрели платформу CONTACT ИП, чтобы выбрать существующие контакты и отобразить контактные данные.

## <a name="related-links"></a>Связанные ссылки

- [Пример контактов](https://docs.microsoft.com/samples/xamarin/ios-samples/contacts/)
- [Новые возможности iOS 9](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Справочник по инфраструктуре контактов](https://developer.apple.com/documentation/contacts?language=objc)
- [Справочник по ContactsUI Framework](https://developer.apple.com/documentation/contactsui?language=objc)
