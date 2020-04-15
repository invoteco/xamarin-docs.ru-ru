---
title: DependencyService в Xamarin.Forms
description: Класс Xamarin.Forms DependencyService — это указатель служб, который позволяет приложениям Xamarin.Forms вызывать собственные функции платформы из общего кода.
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
ms.openlocfilehash: ea259d1ee9dc4a94322c38b3e96bee654197bb87
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "67650443"
---
# <a name="xamarinforms-dependencyservice"></a>DependencyService в Xamarin.Forms

## <a name="introduction"></a>[Введение](introduction.md)

Класс [`DependencyService`](xref:Xamarin.Forms.DependencyService) — это указатель служб, который позволяет приложениям Xamarin.Forms вызывать собственные функции платформы из общего кода.

## <a name="registration-and-resolution"></a>[Регистрация и разрешение](registration-and-resolution.md)

Реализации платформы должны быть зарегистрированы в [`DependencyService`](xref:Xamarin.Forms.DependencyService), после чего разрешены из общего кода для их вызова.

## <a name="picking-a-photo-from-the-library"></a>[Выбор фотографии в библиотеке](photo-picker.md)

В статье объясняется, как использовать класс Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) для выбора фотографии в библиотеке рисунков на телефоне.
