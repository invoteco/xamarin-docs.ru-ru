---
title: Можно ли создать xcarchive Архив из Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 27697223735c4074dd508d3f603ef6aa4e47b1be
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031176"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Можно ли создать xcarchive Архив из Visual Studio?

## <a name="for-xamarin-4"></a>Для Xamarin 4

Начиная с Xamarin 4. x теперь можно создать `.xcarchive` из Windows, задав свойству `ArchiveOnBuild` значение `true`. Например, с помощью `MSBuild` в командной строке:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` будет помещен в каталог `$HOME/Library/Developer/Xcode/Archives` на узле сборки Mac, где Xcode и Xamarin Studio Search будут отображать ранее созданные архивы.

Дополнительные замечания о свойстве `ArchiveOnBuild` см. в этой [записи на форумах Xamarin](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) . Дополнительные сведения о свойствах `ServerAddress` и `ServerUser` см. в документации по [командной строке Xamarin. iOS в Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) .

* * *

## <a name="for-xamarin-3-and-earlier"></a>Для Xamarin 3 и более ранних версий

Расширение Xamarin 3. x Visual Studio не предоставляет механизм создания архивов `.xcarchive`. С другой стороны, логика, используемая для создания архивов `.xcarchive` в Xamarin Studio на Mac, [описана здесь](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), поэтому вы, вероятно, можете создать собственный `.xcarchive` вручную.

Но стоит отметить, что для отправки в App Store не требуется `.xcarchive`. Вы можете отправить файл IPA, если он подписан с помощью профиля распространения App Store (нерегламентированный профиль распространения).

На самом деле, можно даже просто заархивировать пакет `.app` (который подписан с помощью профиля распространения App Store) и отправить этот файл `.zip` в магазин приложений.

В любом случае можно использовать приложение загрузчика приложений для отправки приложения (а не Xcode).
