---
title: Использование данных в приложении Android
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 922b1fa411a176df580050384e7555120fd68137
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "70754461"
---
# <a name="using-data-in-an-app"></a>Использование данных в приложении

В образце **DataAccess_Adv** показано работающее приложение, которое предоставляет функции базы данных для ввода и CRUD (создание, чтение, обновление и удаление). Приложение состоит из двух экранов: списка и формы ввода данных. Весь код доступа к данным многократно используется в iOS и Android без изменения.

После добавления данных в Android экраны приложения выглядят следующим образом:

![Список примеров Android](using-data-in-an-app-images/image11.png "Список примеров Android")

![Пример данных для Android](using-data-in-an-app-images/image12.png "Пример данных для Android")

Проект Android показан ниже &ndash; код, приведенный в этом разделе, содержится в каталоге **ORM** :

![Дерево проектов Android](using-data-in-an-app-images/image14.png "Дерево проектов Android")

Собственный код пользовательского интерфейса для действий в Android выходит за пределы области действия этого документа. Дополнительные сведения об элементах управления пользовательского интерфейса см. в руководстве по [ListView и адаптерам Android](~/android/user-interface/layouts/list-view/index.md) .

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

Android подготавливает данные к просмотру как `ListView`.

## <a name="create-and-update"></a>Создание и обновление

Для упрощения кода приложения предоставляется единственный метод Save, который выполняет вставку или обновление в зависимости от того, был ли установлен параметр PrimaryKey. Так как свойство `Id` помечено атрибутом `[PrimaryKey]`, его не следует задавать в коде. Этот метод определяет, было ли значение сохранено ранее (путем проверки свойства первичного ключа), и вставляет или обновляет объект соответствующим образом:

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

Обычно для реальных приложений требуется проверка (например, обязательные поля, минимальная длина или другие бизнес-правила). Хорошие межплатформенные приложения реализуют как можно большую часть логической проверки в общем коде, передавая ошибки проверки обратно в пользовательский интерфейс для вывода в соответствии с возможностями платформы.

## <a name="delete"></a>Удаление

В отличие от методов `Insert` и `Update`, метод `Delete<T>` может принимать только значение первичного ключа, а не полный объект `Stock`. В этом примере объект `Stock` передается в метод, но в метод `Delete<T>` передается только свойство ID.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Использование предварительно заполненного файла базы данных SQLite

Некоторые приложения поставляются с базой данных, уже заполненной данными. Это можно легко сделать в мобильном приложении, добавив существующий файл базы данных SQLite в приложение и скопировав его в доступный для записи каталог, прежде чем обращаться к нему. Так как SQLite является стандартным форматом файлов, который используется на многих платформах, для создания файла базы данных SQLite доступно несколько средств.

- **Расширение SQLite Manager Firefox** &ndash; работает на компьютерах Mac и Windows и создает файлы, совместимые с iOS и Android.

- @No__t_1 **командной строки** см. в разделе [www.SQLite.org/SQLite.HTML](http://www.sqlite.org/sqlite.html) .

При создании файла базы данных для распространения вместе с приложением следует соблюдать имена таблиц и столбцов, чтобы убедиться, что они соответствуют предполагаемому коду, особенно если вы используете SQLite.NET, который ожидает, что имена будут соответствовать вашим C# классам и свойствам ( или связанные настраиваемые атрибуты).

Чтобы убедиться, что некоторый код выполняется до чего-либо другого в приложении Android, его можно поместить в первое действие для загрузки или создать подкласс `Application`, который будет загружен перед какими-либо действиями. В приведенном ниже коде показан подкласс `Application`, который копирует существующий файл данных файла базы данных **. SQLite** из каталога **/ресаурцес/рав/** .

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты по данным Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
