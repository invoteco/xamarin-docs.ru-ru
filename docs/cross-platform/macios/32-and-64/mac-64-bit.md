---
title: Обновление Объединенных приложений Xamarin. Mac до 64-bit
description: В этом руководство описано, как обновить приложения Xamarin. Mac до версии 64. В нем также приводятся примеры ошибок, которые могут возникнуть при внесении этого изменения.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 5539bab417c5efc0064cd1753cb74c7524463ee5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765911"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Обновление Объединенных приложений Xamarin. Mac до 64-bit

По состоянию на Январь 2018 компания Apple требует, чтобы новые [отправки хранилища приложений Mac нацелились на 64-бит](https://developer.apple.com/news/?id=06282017a). Приложения, уже доступные в Mac App Store, должны быть обновлены до 64-х июня 2018.

В **файле** > **Новый** шаблон проекта Xamarin. Mac по умолчанию создаются 64-разрядные приложения, поэтому все недавно созданные приложения уже совместимы с 64 и не будут требовать каких-либо изменений.

## <a name="targeting-64-bit"></a>Нацеливание на 64-разр.

1. Откройте окно **параметров проекта** для приложения Xamarin. Mac:

   ![Контекстное меню проекта](mac-64-bit-images/1-contextual_menu-vsmac.png "Контекстное меню проекта")

2. Выберите **Сборка Mac** и задайте **Поддерживаемые архитектуры** для **\_x86 64**:

   [![Настройка поддерживаемых архитектур для x86_64](mac-64-bit-images/2-project_options-vsmac.png "Настройка поддерживаемых архитектур для x86_64")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Если приложение имеет какие-либо внешние зависимости, такие как собственные ссылки или проекты привязки, обновите их до 64-разрядных версий.

### <a name="errors"></a>Ошибки

При первой сборке или запуске приложения с 64-разрядной поддержкой вы можете столкнуться с ошибками связи из-за проблем с Clang или Runtime. Эти ошибки могут возникать, если сторонние зависимости (например, собственные ссылки в проектах Xamarin. Mac или привязок) или загруженные вручную платформы на уровне системы не были обновлены до 64-бит.

> [!TIP]
> Преобразование проекта в 64-bit является значительным изменением и может косвенным образом обнаружить различные ошибки программирования. В частности, может измениться размер и выравнивание структур данных, что повлияет на подписи p/Invoke и на машинный код, связанный в проекте. Рассмотрите возможность просмотра всех заданных предупреждений сборки и тщательного тестирования приложения, чтобы перехватить потенциальные проблемы.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Пример ошибки, полученный из динамически связанной зависимости стороннего производителя, не предназначенный для 64-разрядной платформы:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Эту ошибку можно выполнить во время выполнения, `dlopen` вернувшись `IntPtr.Zero` вместо ожидаемого маркера.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Пример ошибки, полученной из-за статической связанной сторонней зависимости, не предназначенной для 64-разрядной платформы:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Чтобы успешно выполнить сборку и выполнение, обновите эти зависимости до 64-разрядной версии и выполните повторную компиляцию приложения.
