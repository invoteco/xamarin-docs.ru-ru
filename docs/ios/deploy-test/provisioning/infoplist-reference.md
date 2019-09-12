---
title: Справочник по Info.plist для Xamarin.iOS
description: Этот документ описывает различные пары "ключ-значение", которые можно задать в файле Info.plist приложения Xamarin.iOS. Эти ключи необходимы, если приложение выполняет конкретные задачи, например обращение к расположению, фотографиям, микрофону или камере.
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: e927d15e34107c19d9b4033db591742a513a963f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762571"
---
# <a name="infoplist-reference-for-xamarinios"></a>Справочник по Info.plist для Xamarin.iOS

Дополнительные сведения о работе с ключами Info.Plist см. в руководстве по [работе с безопасностью и конфиденциальностью](~/ios/app-fundamentals/security-privacy.md). 

## <a name="location"></a>Расположение 

Доступ к расположению пользователя требует изменений в файле Info.plist. Необходимо задать следующие ключи, связанные с данными о расположении: 

- **NSLocationWhenInUseUsageDescription** для доступа к расположению пользователя во время его взаимодействия с приложением. 
- **NSLocationAlwaysUsageDescription** для доступа приложения к расположению пользователя в фоновом режиме.

## <a name="photos"></a>Фотографии 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>Контакты 

NSContactsUsageDescription 

## <a name="calendar-data"></a>Данные календаря 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>Данные напоминаний 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Периферийные устройства Bluetooth 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>Микрофон 

NSMicrophoneUsageDescription 

## <a name="camera"></a>Камера 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>Чтение HealthKit  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>Фоновые режимы 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 

## <a name="related-links"></a>Связанные ссылки

- [Справочное руководство по Apple.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
