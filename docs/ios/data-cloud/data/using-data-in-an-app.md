---
title: Использование данных в приложении iOS
description: В этом документе описывается пример DataAccess_Adv, в котором демонстрируется, как собираются входные данные пользователя и выполняются операции создания, чтения, обновления и удаления (CRUD) базы данных в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: 060e4b8e7856e0024e6d236652c2b04c1da16f66
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008253"
---
# <a name="using-data-in-an-ios-app"></a>Использование данных в приложении iOS

В образце **DataAccess_Adv** показано работающее приложение, которое позволяет выполнять входные данные и *CRUD* (создание, чтение, обновление и удаление) базы данных. Приложение состоит из двух экранов: списка и формы ввода данных. Весь код доступа к данным многократно используется в iOS и Android без изменения.

После добавления данных в iOS экраны приложения выглядят следующим образом:

 ![](using-data-in-an-app-images/image9.png "iOS sample list")

 ![](using-data-in-an-app-images/image10.png "iOS sample detail")

Ниже приведен пример проекта для iOS. код, приведенный в этом разделе, содержится в каталоге **ORM** :

 ![](using-data-in-an-app-images/image13.png "iOS project tree")

Собственный код пользовательского интерфейса для Виевконтроллерс в iOS выходит за пределы области действия этого документа.
Дополнительные сведения об элементах управления пользовательского интерфейса см. в руководстве по [работе с таблицами и ячейками iOS](~/ios/user-interface/controls/tables/index.md) .

## <a name="read"></a>Чтение

В примере существует несколько операций чтения:

- Чтение списка
- Чтение отдельных записей

В классе `StockDatabase` два метода:

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

iOS по-разному визуализирует данные как `UITableView`.

## <a name="create-and-update"></a>Создание и обновление

Для упрощения кода приложения предоставляется единственный метод Save, который выполняет вставку или обновление в зависимости от того, был ли установлен параметр PrimaryKey. Так как свойство `Id` помечено атрибутом `[PrimaryKey]`, его не следует задавать в коде.
Этот метод определяет, было ли значение сохранено ранее (путем проверки свойства первичного ключа), и вставляет или обновляет объект соответствующим образом:

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```

Обычно для реальных приложений требуется проверка (например, обязательные поля, минимальная длина или другие бизнес-правила).
Хорошие межплатформенные приложения реализуют как можно большую часть логической проверки в общем коде, передавая ошибки проверки обратно в пользовательский интерфейс для вывода в соответствии с возможностями платформы.

## <a name="delete"></a>Удаление

В отличие от методов `Insert` и `Update`, метод `Delete<T>` может принимать только значение первичного ключа, а не полный объект `Stock`.
В этом примере объект `Stock` передается в метод, но в метод `Delete<T>` передается только свойство ID.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Использование предварительно заполненного файла базы данных SQLite

Некоторые приложения поставляются с базой данных, уже заполненной данными.
Это можно легко сделать в мобильном приложении, добавив существующий файл базы данных SQLite в приложение и скопировав его в доступный для записи каталог, прежде чем обращаться к нему. Так как SQLite является стандартным форматом файлов, который используется на многих платформах, для создания файла базы данных SQLite доступно несколько средств.

- **Расширение SQLite Manager Firefox** — работает в Mac и Windows и создает файлы, совместимые с iOS и Android.
- **Командная строка** — см. [www.SQLite.org/SQLite.HTML](https://www.sqlite.org/sqlite.html) .

При создании файла базы данных для распространения вместе с приложением следует соблюдать имена таблиц и столбцов, чтобы убедиться, что они соответствуют предполагаемому коду, особенно если вы используете SQLite.NET, который ожидает, что имена будут соответствовать вашим C# классам и свойствам ( или связанные настраиваемые атрибуты).

Для iOS включите в приложение SQLite-файл и убедитесь, что он помечен как **действие сборки: содержимое**. Поместите код в `FinishedLaunching`, чтобы скопировать файл в каталог, доступный для записи, *перед* вызовом методов данных. Следующий код скопирует существующую базу данных с именем **Data. SQLite**, только если она еще не существует.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Любой код доступа к данным (будь то ADO.NET или using SQLite.NET), который выполняется после завершения этого процесса, будет иметь доступ к предварительно заполненным данным.

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты на данные iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
