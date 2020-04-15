---
title: Начало работы с проверкой подлинности по отпечаткам пальцев
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020282"
---
# <a name="getting-started-with-fingerprint-authentication"></a>Начало работы с проверкой подлинности по отпечаткам пальцев

Чтобы приступить к работе, давайте сначала рассмотрим настройку проекта Xamarin.Android, чтобы приложение могло использовать проверку подлинности по отпечаткам пальцев.

1. Обновить **AndroidManifest.xml**, чтобы объявить разрешения, необходимые для API-интерфейсов отпечатков пальцев.
2. Получить ссылку на `FingerprintManager`.
3. Убедиться, что устройство поддерживает сканирование отпечатков пальцев.

## <a name="requesting-permissions-in-the-application-manifest"></a>Запрос разрешений в манифесте приложения

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Приложение Android должно запрашивать разрешение `USE_FINGERPRINT` в манифесте. На снимке экрана ниже показано, как добавить это разрешение в приложение в Visual Studio:

[![Включение USE\_FINGERPRINT в манифесте Android](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Приложение Android должно запрашивать разрешение `USE_FINGERPRINT` в манифесте. На снимке экрана ниже показано, как добавить это разрешение в приложение в Visual Studio для Mac:

[![Включение UseFingerprint в приложении Android](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Получение экземпляра FingerprintManager

Далее приложение должно получить экземпляр `FingerprintManager` или класс `FingerprintManagerCompat`. Чтобы обеспечить совместимость с более старыми версиями Android, приложение Android должно использовать API совместимости, которые доступны в пакете NuGet для поддержки Android версии 4. В следующем фрагменте кода показано, как получить соответствующий объект из операционной системы: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

В предыдущем фрагменте `context` — это любой `Android.Content.Context` в Android. Обычно это действие, выполняющее проверку подлинности.

## <a name="checking-for-eligibility"></a>Проверка допустимости

Приложение должно выполнить несколько проверок, чтобы определить возможность использования проверки подлинности по отпечаткам пальцев. В итоге существует пять условий, которые приложение использует для проверки допустимости:  

**Уровень API 23** &ndash; API отпечатков пальцев требуется уровень API 23 или выше. Класс `FingerprintManagerCompat` будет переносить проверку уровня API. Поэтому рекомендуется использовать **библиотеку поддержки Android версии 4** и `FingerprintManagerCompat`. Они понадобятся для одной из этих проверок.

**Оборудование** &ndash; при первом запуске приложения оно должно проверить наличие сканера отпечатков пальцев:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**Устройство защищено** &ndash; пользователь должен защитить устройство с помощью блокировки экрана. Если пользователь не защищает устройство с помощью блокировки экрана, и для приложения важна безопасность, пользователь должен получить уведомление о необходимости настройки блокировки экрана. В следующем фрагменте кода показано, как проверить выполнение этого требования:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Зарегистрированные отпечатки пальцев** &ndash; пользователь должен зарегистрировать в операционной системе по крайней мере один отпечаток пальца. Эта проверка разрешений должна выполняться перед каждой попыткой проверки подлинности:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Разрешения** &ndash; приложение перед его использованием должно запрашивать разрешение у пользователя. Для Android 5.0 и более старых версий пользователь предоставляет разрешение как условие установки приложения. В Android 6.0 появилась новая модель разрешений, которая проверяет разрешения во время выполнения. Этот фрагмент кода является примером проверки разрешений на Android 6.0:

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // https://developer.android.com/training/permissions/requesting.html
}
```

Проверка всех этих условий каждый раз, когда приложение предлагает варианты проверки подлинности, обеспечит лучшее взаимодействие с пользователем. Изменения или обновления устройства или операционной системы могут повлиять на доступность проверки подлинности по отпечаткам пальцев. Если вы решили кэшировать результаты какой бы то ни было из этих проверок, обеспечьте сценарии обновления.

Дополнительные сведения о том, как запросить разрешения в Android 6.0, см. в документации по Android [Запрос прав доступа во время выполнения](https://developer.android.com/training/permissions/requesting.html).

## <a name="related-links"></a>Связанные ссылки

- [Контекст](xref:Android.Content.Context)
- [KeyguardManager](xref:Android.App.KeyguardManager)
- [ContextCompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Запрос прав доступа во время выполнения](https://developer.android.com/training/permissions/requesting.html)
