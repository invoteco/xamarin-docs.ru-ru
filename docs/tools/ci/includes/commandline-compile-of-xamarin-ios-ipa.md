---
ms.openlocfilehash: e383bbccd4e76be8a208f5680e5cf21e45a0dbc3
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511925"
---

Следующая командная строка позволяет указать сборку выпуска решения **SOLUTION_FILE. sln** для iPhone. Расположение IPA можно задать, указав `IpaPackageDir` свойство в командной строке:

- На компьютере Mac с помощью **Xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

Команда **Xbuild** обычно находится в каталоге **/либрари/фрамеворкс/моно.фрамеворк/коммандс**.

- В Windows с помощью **MSBuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**MSBuild** не будет автоматически развертывать `$( )` выражения, переданные в командной строке. По этой причине рекомендуется использовать полный путь при задании в `IpaPackageDir` командной строке.

Дополнительные сведения о `IpaPackageDir` свойстве см. в заметках о выпуске [iOS 9,8](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location) .
