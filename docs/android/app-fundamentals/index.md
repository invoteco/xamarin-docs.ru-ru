---
title: Основы приложений Xamarin. Android
description: Основные понятия основных приложений
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: eb581d68f3b7e57975b6979fe1005b1fac411ec8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019224"
---
# <a name="xamarinandroid-application-fundamentals"></a>Основы приложений Xamarin. Android

В этом разделе приводится описание некоторых наиболее распространенных задач или концепций, которые необходимо учитывать при разработке приложений Android.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Специальные возможности](~/android/app-fundamentals/accessibility.md)

На этой странице описывается, как использовать интерфейсы API специальных возможностей Android для создания приложений в соответствии с [контрольным списком специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md).

## <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Основные сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md)

В этом руководство описано, как Android использует уровни API для управления совместимостью приложений в разных версиях Android, а также объясняет, как настроить параметры проекта Xamarin. Android для развертывания этих уровней API в приложении. Кроме того, в этом руководстве объясняется, как написать код среды выполнения, который работает с различными уровнями API, и содержит список ссылок на все уровни API Android, Номера версий (например, Android 8,0), имена кодов для Android (например, Oreo) и коды версий сборки.

## <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Ресурсы в Android](~/android/app-fundamentals/resources-in-android/index.md)

В этой статье описывается понятие ресурсов Android в Xamarin. Android и приводятся инструкции по их использованию. В нем рассматривается использование ресурсов в приложении Android для поддержки локализации приложений и нескольких устройств, включая различные размеры и плотность экранов.

## <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Жизненный цикл действия](~/android/app-fundamentals/activity-lifecycle/index.md)

Действия — это фундаментальный Стандартный блок приложений Android, который может существовать в нескольких разных состояниях. Жизненный цикл действия начинается с создания экземпляра и заканчивается на уничтожение и включает много состояний между. Когда действие изменяет состояние, вызывается соответствующий метод события жизненного цикла, уведомляющий о действии ожидающего изменения состояния и позволяя ему выполнить код для адаптации к этому изменению. В этой статье изучается жизненный цикл действий и объясняется ответственность за то, что действие в ходе каждого из этих изменений состояния является частью хорошо организованного надежного приложения.

## <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Локализация](~/android/app-fundamentals/localization.md)

В этой статье описывается локализация Xamarin. Android на другие языки путем преобразования строк и предоставления альтернативных изображений.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Службы](~/android/app-fundamentals/services/index.md)

В этой статье рассматриваются службы Android, которые являются компонентами Android, которые позволяют выполнять работу в фоновом режиме. В нем объясняются различные сценарии, которые подходят службам, и демонстрируется их реализация как для выполнения длительных фоновых задач, так и для предоставления интерфейса для удаленных вызовов процедур.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Широковещательные приемники](~/android/app-fundamentals/broadcast-receivers.md)

В этом руководстве описано, как создавать и использовать широковещательные приемники, компонент Android, реагирующий на широковещательные рассылки на уровне системы, в Xamarin. Android.

## <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Разрешения](~/android/app-fundamentals/permissions.md)

Для создания и добавления разрешений в манифест Android можно использовать встроенную поддержку средств в Visual Studio для Mac или Visual Studio. В этом документе описывается добавление разрешений в Visual Studio и Xamarin Studio.

## <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Графика и анимация](~/android/app-fundamentals/graphics-and-animation.md)

Android предоставляет обширную и разнообразную платформу для поддержки двухмерной графики и анимации. В этом документе представлены эти платформы и обсуждаются способы создания пользовательских графиков и анимации и их использования в приложении Xamarin. Android.

## <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[Архитектуры процессоров](~/android/app-fundamentals/cpu-architectures.md)

Xamarin. Android поддерживает несколько архитектур ЦП, включая 32-разрядные и 64-разрядные устройства. В этой статье объясняется, как выбрать для приложения одну или несколько архитектур ЦП, поддерживаемых Android.

## <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Обработка поворота](~/android/app-fundamentals/handling-rotation.md)

В этой статье описывается, как управлять изменением ориентации устройства в Xamarin. Android. В нем рассматривается работа с системой ресурсов Android для автоматической загрузки ресурсов для конкретной ориентации устройства, а также как программно Обрабатывайте изменения ориентации. Затем в нем описываются методы поддержания состояния при вращении устройства.

## <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Звук Android](~/android/app-fundamentals/android-audio.md)

ОС Android обеспечивает расширенную поддержку мультимедиа, охватывающую как аудио, так и видео. В этом руководстве рассказывается о звуках в Android и рассматривается воспроизведение и запись звука с помощью встроенных классов аудио Player и записывающего класса, а также низкоуровневого аудио API. Кроме того, в нем рассматривается работа с звуковыми событиями в других приложениях, что позволяет разработчикам создавать хорошо работающие приложения.

## <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Уведомления](~/android/app-fundamentals/notifications/index.md)

В этом разделе объясняется, как реализовать локальные и удаленные уведомления в Xamarin. Android. В нем описываются различные элементы пользовательского интерфейса уведомления Android и обсуждаются API, связанные с созданием и отображением уведомлений. Для удаленных уведомлений поясняются как Google Cloud Messaging, так и Firebase Cloud Messaging. Пошаговые руководства и примеры кода включены.

## <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Сенсорные технологии](~/android/app-fundamentals/touch/index.md)

В этом разделе объясняются основные понятия и сведения о реализации жестов сенсорного ввода в Android. Интерфейсы API сенсорного ввода введены и описаны, а затем изучаются распознаватели жестов.

## <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[Стек HttpClient и SSL/TLS](~/android/app-fundamentals/http-stack.md)

В этом разделе описываются селекторы реализации HttpClient Stack и SSL/TLS для Android. Эти параметры определяют реализацию HttpClient и SSL/TLS, которая будет использоваться приложениями Xamarin. Android.

## <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Написание реагирующих приложений](writing-responsive-apps.md)

В этой статье рассматривается использование потоков для сохранения скорости реагирования приложения Xamarin. Android путем перемещения длительно выполняемых задач в фоновый поток.
