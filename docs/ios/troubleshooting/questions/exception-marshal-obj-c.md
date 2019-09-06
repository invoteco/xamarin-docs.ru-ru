---
title: 'Почему работа приложения iOS 9 завершается ошибкой: System.Exception: Не удалось маршалировать объект Objective-C?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 62a63dc5156d1acf9ad6ca15029978131c151726
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290469"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Почему работа приложения iOS 9 завершается ошибкой: System.Exception: Не удалось маршалировать объект Objective-C?

Может появиться сообщение об ошибке этой формы:

> System.Exception: Не удалось маршалировать объект цели-C... Не удалось найти существующий управляемый экземпляр для этого объекта...

Изменения API в iOS 9 потребовали использования конструктора обратного вызова при вызове неуправляемого кода, так как этот интерфейс API теперь предполагает его. Чтобы добавить конструктор обратного вызова в класс, используйте следующую строку: 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>Следующие шаги

Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости . 
