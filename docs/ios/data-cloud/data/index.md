---
title: Доступ к данным Xamarin. iOS
description: В этом документе содержатся ссылки на руководства, в которых описывается работа с локальными базами данных в приложении Xamarin. iOS. Связанное содержимое обсуждает SQLite.NET, ADO.NET и многое другое.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: 6050320f781ea89c5455e10a8754a89d7086e92a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281746"
---
# <a name="xamarinios-data-access"></a>Доступ к данным Xamarin. iOS

Xamarin. iOS поддерживает API-интерфейсы доступа к базам данных, например:

- ADO.NET Framework.
- SQLite-NET Library стороннего производителя.

В этом руководство представлен общий обзор баз данных, прежде чем описано, как настроить ADO.NET и SQLite.NET для доступа к базам данных SQLite в приложениях Xamarin. iOS. 

Большая часть кода в этом документе полностью кросс-платформенная и будет работать на iOS или Android без изменения. Обсуждаются два примера приложений:

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) — простые операции с данными записывают результаты в элемент управления отображением текста;
- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) — интегрирует операции с данными в небольшое рабочее приложение, которое перечисляет и редактирует простую структуру данных.

Оба примера решения содержат проекты образцов приложений для iOS и Android.

Для приложений Xamarin. Forms прочтите статью [Работа с базами данных](~/xamarin-forms/data-cloud/data/databases.md) , в которой объясняется, как работать с SQLite в библиотеке PCL с Xamarin. Forms.

## <a name="sections"></a>Разделы

- [Введение](introduction.md)
- [Конфигурация](configuration.md)
- [Использование ORM для SQLite.NET](using-sqlite-orm.md)
- [Использование ADO.NET](using-adonet.md)
- [Использование данных в приложении](using-data-in-an-app.md)

## <a name="summary"></a>Сводка

В этой главе обсуждаются доступ к данным в Xamarin. iOS с использованием SQLite в качестве ядра СУБД. Доступ к базе данных можно получить напрямую с помощью синтаксиса ADO.NET. Кроме того, можно включить SQLite.NET ORM и выполнить операции C#с данными в.

Мы рассматривали два примера: один, содержащий очень простой код доступа к данным, который выводит данные в текстовое поле, и простое приложение, которое включает функции создания, чтения, обновления и удаления. Мы также обсуждали работу с потоками и как инициализировать приложение с помощью предварительно заполненной базы данных SQLite.

Дополнительные примеры межплатформенного доступа к данным см. в [статье Практическое](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) обучение для различных платформ.

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты на данные iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
