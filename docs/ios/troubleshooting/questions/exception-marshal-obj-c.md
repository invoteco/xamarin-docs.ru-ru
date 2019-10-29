---
title: 'Почему приложение iOS 9 завершается с ошибкой: System. Exception: не удалось маршалировать объект цели-C?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 1bb67eaa884e523e96ef1015daaa6b959ea1512d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031093"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Почему приложение iOS 9 завершается с ошибкой: System. Exception: не удалось маршалировать объект цели-C?

Может появиться сообщение об ошибке этой формы:

> System. Exception: не удалось маршалировать объект цели-C... Не удалось найти существующий управляемый экземпляр для этого объекта...

Изменения API в iOS 9 потребовали использования конструктора обратного вызова при вызове неуправляемого кода, так как этот интерфейс API теперь предполагает его. Чтобы добавить конструктор обратного вызова в класс, используйте следующую строку: 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>Следующие шаги

Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости . 
