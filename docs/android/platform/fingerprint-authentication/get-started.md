---
title: начало работы с проверкой подлинности отпечатков пальцев
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020282"
---
# <a name="getting-started-with-fingerprint-authentication"></a>начало работы с проверкой подлинности отпечатков пальцев

Чтобы приступить к работе, давайте сначала рассмотрим настройку проекта Xamarin. Android, чтобы приложение могло использовать проверку подлинности отпечатков пальцев:

1. Обновите **AndroidManifest. XML** , чтобы объявить разрешения, необходимые для API-интерфейсов отпечатков пальцев.
2. Получите ссылку на `FingerprintManager`.
3. Убедитесь, что устройство поддерживает сканирование отпечатков пальцев.

## <a name="requesting-permissions-in-the-application-manifest"></a>Запрос разрешений в манифесте приложения

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Приложение Android должно запрашивать разрешение `USE_FINGERPRINT` в манифесте. На следующем снимке экрана показано, как добавить это разрешение в приложение в Visual Studio:

[![Включение использования\_ОТПЕЧАТКов пальцев на экране манифеста Android](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Приложение Android должно запрашивать разрешение `USE_FINGERPRINT` в манифесте. На следующем снимке экрана показано, как добавить это разрешение в приложение в Visual Studio для Mac:

[![включение Усефинжерпринт на экране приложения Android](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Получение экземпляра Финжерпринтманажер

Затем приложение должно получить экземпляр `FingerprintManager` или класс `FingerprintManagerCompat`. Чтобы обеспечить совместимость с более старыми версиями Android, приложение Android должно использовать API совместимости, который находится в пакете NuGet для поддержки Android версии 4. В следующем фрагменте кода показано, как получить соответствующий объект из операционной системы: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

В предыдущем фрагменте `context` `Android.Content.Context`Android. Обычно это действие, выполняющее проверку подлинности.

## <a name="checking-for-eligibility"></a>Проверка допустимости

Приложение должно выполнить несколько проверок, чтобы обеспечить возможность использования проверки подлинности отпечатков пальцев. В итоге существует пять условий, которые приложение использует для проверки допустимости:  

API **уровня 23** &ndash; интерфейсам API отпечатков пальцев требуется уровень API 23 или выше. Класс `FingerprintManagerCompat` будет переносить проверку на уровне API. По этой причине рекомендуется использовать **библиотеку поддержки Android v4** и `FingerprintManagerCompat`; Это будет учитывать одну из этих проверок.

&ndash; **оборудования** при первом запуске приложения должно проверять наличие сканера отпечатков пальцев:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**Устройство защищено** &ndash; пользователь должен защитить устройство с помощью блокировки экрана. Если пользователь не защищает устройство с помощью блокировки экрана, и для приложения важна безопасность, пользователь должен получать уведомления о необходимости настройки блокировки экрана. В следующем фрагменте кода показано, как проверить это пред-рекуисте:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

Зарегистрированные **отпечатки пальцев** &ndash; пользователь должен иметь по крайней мере один отпечаток, зарегистрированных в операционной системе. Эта проверка разрешений должна выполняться до каждой попытки проверки подлинности:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Разрешения** &ndash; приложение должно запрашивать разрешение у пользователя перед использованием приложения. Для Android 5,0 и более низкого уровня пользователь предоставляет разрешение в качестве условия установки приложения. В Android 6,0 появилась новая модель разрешений, которая проверяет разрешения во время выполнения. Этот фрагмент кода является примером проверки разрешений на Android 6,0:

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

Проверка всех этих условий каждый раз, когда приложение предлагает варианты проверки подлинности, обеспечит лучшее взаимодействие с пользователем. Изменения или обновления устройства или операционной системы могут повлиять на доступность проверки подлинности отпечатков пальцев. Если вы решили кэшировать результаты какой бы то ни было из этих проверок, следите за сценариями обновления.

Дополнительные сведения о том, как запросить разрешения в Android 6,0, см. в документации по Android, [запрашивающей разрешения во время выполнения](https://developer.android.com/training/permissions/requesting.html).

## <a name="related-links"></a>Связанные ссылки

- [Локального](xref:Android.Content.Context)
- [кэйгуардманажер](xref:Android.App.KeyguardManager)
- [контексткомпат](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [финжерпринтманажер](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [финжерпринтманажеркомпат](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Запрос разрешений во время выполнения](https://developer.android.com/training/permissions/requesting.html)
