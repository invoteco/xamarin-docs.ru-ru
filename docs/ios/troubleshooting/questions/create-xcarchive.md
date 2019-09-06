---
title: Можно ли создать xcarchive Архив из Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 6d35827b00a4ccc9bbe3e71444536425e4e1c3b1
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288113"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Можно ли создать xcarchive Архив из Visual Studio?

## <a name="for-xamarin-4"></a>Для Xamarin 4

`.xcarchive` Начиная с Xamarin 4. x теперь можно создать из Windows, `ArchiveOnBuild` задав свойству `true`значение. Например, с помощью `MSBuild` команды в командной строке:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Будет размещен`$HOME/Library/Developer/Xcode/Archives` в каталоге на узле сборки Mac, где Xcode и Xamarin Studio Search будут отображать ранее созданные архивы.

Дополнительные замечания о `ArchiveOnBuild` свойстве см. в этой [записи в форумах Xamarin](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) . Дополнительные сведения о `ServerAddress` свойствах и `ServerUser` см. в документации по [командной строке Xamarin. iOS в Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) .

* * *

## <a name="for-xamarin-3-and-earlier"></a>Для Xamarin 3 и более ранних версий

Расширение Xamarin 3. x Visual Studio не предоставляет механизм для создания `.xcarchive` архивов. С другой стороны, логика, используемая для создания `.xcarchive` архивов в Xamarin Studio на Mac, [описана здесь](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), поэтому вы `.xcarchive` , вероятно, можете создать свой собственный вручную, если вам хотелось.

Но стоит отметить, что `.xcarchive` для отправки в магазин приложений не требуется. Вы можете отправить файл IPA, если он подписан с помощью профиля распространения App Store (нерегламентированный профиль распространения).

На самом деле, вы можете даже просто заархивировать `.app` пакет (который подписан с помощью профиля распространения App Store) и отправить этот `.zip` файл в App Store.

В любом случае можно использовать приложение загрузчика приложений для отправки приложения (а не Xcode).

