---
title: Сообщения об ошибках Xamarin. Mac (MMP)
description: В этом документе перечислены ошибки, создаваемые MMP, средством, используемым для упаковки скомпилированных сборок в исполняемое приложение Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: d81435f7d693204a76249fc3fa6953766f7ab59a
ms.sourcegitcommit: 9f37dc00c2adab958025ad1cdba9c37f0acbccd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69012467"
---
# <a name="xamarinmac-error-messages-mmp"></a>Сообщения об ошибках Xamarin. Mac (MMP)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: сообщения об ошибках MMP

Например, параметры, среда, отсутствующие инструменты.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000: Непредвиденная ошибка. файл отчета об ошибке можно отправить по адресу https://github.com/xamarin/xamarin-macios/issues/new

Произошло непредвиденное условие ошибки. Создайте [отчет об ошибке](https://github.com/xamarin/xamarin-macios/issues/new) с максимально возможной информацией, включая:

* Полные журналы сборки с максимальным уровнем детализации (например `-v -v -v -v` , в **дополнительных аргументах MMP**);
* Минимальный тестовый случай, воспроизводящий ошибку; перетаскивани
* Все сведения о версиях

Самый простой способ получить точную информацию о версии — использовать меню « **Xamarin Studio** », « **Xamarin Studio** элемент», « **отобразить сведения** » и скопировать/вставить сведения о версии (можно использовать кнопку « **Копировать информацию** »).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Для этой версии Xamarin. Mac требуется Mono {0} (Текущая версия Mono — {1}). Обновите Mono. Framework с http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Имя приложения "{0}. exe" конфликтует с именем пакета SDK или сборки продукта (. dll).

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Корневая сборка "{0}" не существует

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Необходимо указать только одну корневую сборку, найденные {0} сборки: "{1}"

<a name="MM0009" />

#### <a name="mm0009-error-while-loading-assemblies-"></a>MM0009: Ошибка при загрузке сборок: *.

Произошла ошибка при загрузке сборок из ссылок на корневую сборку. Дополнительные сведения можно указать в выходных данных сборки.

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: Не удалось проанализировать аргументы командной строки:{0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: Параметр "{0}" не рекомендуется к использованию.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: Необходимо указать корневую сборку

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: Неизвестный аргумент командной строки:{0}""

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: Допустимые параметры для "{0}": "{1}".

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Имя приложения "{0}. exe" конфликтует с другой сборкой пользователя.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: Не удалось проанализировать аргумент командной строки{0}"":{1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Сборщик мусора Boehm не поддерживается. Вместо этого выбран сборщик мусора SGen.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Невозможно предоставить корневую сборку, если передается--No-root-Assembly.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Выходной каталог (--Output) требуется, если передается--No-root-Assembly.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Невозможно отключить новое значение refcount с Unified API.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Не удается найти Xcode в любом из расположений по умолчанию. Установите Xcode или передайте пользовательский путь с помощью--sdkroot добавлен =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: Не удалось найти выбранные в данный момент Xcode в системе: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: Не удалось найти выбранные в данный момент Xcode в системе. "Xcode-SELECT--Print-Path" вернул "{0}", но этот каталог не существует.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Недопустимое значение для целевой платформы {0}:.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Неизвестная платформа: *. Обычно это указывает на ошибку в Xamarin. Mac; Запишите отчет об ошибке https://bugzilla.xamarin.com в тестовом случае.

Обычно это указывает на ошибку в Xamarin. Mac; Запишите отчет об ошибке [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) в тестовом случае.

<a name="MM0073" />

#### <a name="mm0073-xamarinmac--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MM0073: Xamarin. Mac * не поддерживает цель развертывания * (минимальное значение — *). Выберите более новую цель развертывания в файле info. plist проекта.

Минимальный целевой объект развертывания — это значение, указанное в сообщении об ошибке. Выберите более новую цель развертывания в файле info. plist проекта.

Если обновление целевого объекта развертывания невозможно, используйте более старую версию Xamarin. Mac.

<a name="MM0074" />

#### <a name="mm0074-xamarinmac--does-not-support-a-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinmac"></a>MM0074: Xamarin. Mac * не поддерживает целевой объект развертывания * (максимальное значение — *). Выберите более старый целевой объект развертывания в файле info. plist проекта или обновите версию Xamarin. Mac до новой версии.

Xamarin. Mac не поддерживает установку версии минимального целевого объекта развертывания более высокого уровня, чем версия этой конкретной версии Xamarin. Mac.

Выберите старую минимальную целевую платформу развертывания в файле info. plist проекта или обновите версию Xamarin. Mac до более новой.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Внутренняя ошибка-в набор приложений не был скопирован исполняемый файл. Обратитесь к "support@xamarin.com"

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: Отключение Неврефкаунт,--New-refcount: false является устаревшим.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Для этой версии Xamarin. Mac требуется пакет SDK для * (поставляется с Xcode *). Обновите Xcode, чтобы получить требуемые файлы заголовков, или используйте динамический регистратор или настройте поведение управляемого компоновщика, чтобы связать только пакеты SDK платформы или компоновки (чтобы попытаться избежать новых API).

Для сборки приложения с помощью статического регистратора в Xamarin. Mac требуются файлы заголовков из версии пакета SDK, указанной в сообщении об ошибке. Чтобы устранить эту ошибку, рекомендуется обновить Xcode, чтобы получить необходимый пакет SDK, в который будут включены все необходимые файлы заголовков. Если установлено несколько версий Xcode или вы хотите использовать Xcode в расположении, отличном от расположения по умолчанию, убедитесь, что в настройках IDE задано правильное расположение Xcode.

Одним из возможных альтернативных решений является включение управляемого компоновщика. Это приведет к удалению неиспользуемого API, включая, в большинстве случаев, нового API, где файлы заголовков отсутствуют (или неполны). Однако это не будет работать, если в проекте используется API, который был представлен в более новом пакете SDK, чем тот, который предоставляет Xcode.

Вторым возможным альтернативным решением является использование динамического регистратора. Это накладывает затраты на запуск путем динамической регистрации типов, но при этом необходимо удалить требование файла заголовка. 

Последним страв решением будет использование более старой версии Xamarin. Mac, которая поддерживает пакет SDK, требуемый для вашего проекта.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: не удается найти файл{0}Machine. config.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: Компиляция AOT доступна только в единой среде

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: Внутренняя ошибка {0}. Запишите отчет об ошибке с тестовым случаем (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Для гибридной компиляции AOT требуется, чтобы все сборки были скомпилированы в AOT.

<a name="MM0129" />

#### <a name="mm0129-debugging-symbol-file-for--does-not-match-the-assembly-and-is-ignored"></a>MM0129: Отладочный файл символов для "*" не соответствует сборке и игнорируется.

Не удалось загрузить отладочные символы: PDB (только переносимый PDB) или MDB-файл для указанной сборки.

Обычно это означает, что сборка является более новой или старше, чем символы. Так как они не совпадают, они не могут использоваться, а символы игнорируются.

Это предупреждение не повлияет на создаваемое приложение, но может быть недоступно для полной отладки (в частности, в коде из указанной сборки). Кроме того, для исключений, трассировки стека и отчетов о сбоях могут отсутствовать некоторые сведения.

Сообщите об этой ошибке издателю пакета сборки (например, автор NuGet), чтобы это можно было исправить в будущих выпусках.

<a name="MM0130" />

#### <a name="mm0130-no-root-assemblies-found-you-should-provide-at-least-one-root-assembly"></a>MM0130: Корневые сборки не найдены. Необходимо предоставить хотя бы одну корневую сборку.

При запуске `--runregistrar`должна быть предоставлена хотя бы одна корневая сборка.

<a name="MM0131" />

#### <a name="mm0131-product-assembly-0-not-found-in-assembly-list-1"></a>MM0131: Сборка продукта "{0}" не найдена в списке сборок:{1}""

При запуске `--runregistrar`список сборок должен содержать сборку продукта, Xamarin. Mac, ксаммак.

<a name="MM0132" />

#### <a name="mm0132-unknown-optimization--valid-values-are-"></a>MM0132: Неизвестная оптимизация: *. Допустимые значения: *

Указанная оптимизация не распознана.

Допустимый формат: `[+|-]optimization-name`, где `optimization-name` — одно из значений, перечисленных в сообщении об ошибке.

Полное описание каждой оптимизации см. в разделе [Оптимизация сборки](https://developer.xamarin.com/guides/cross-platform/macios/build-optimizations) .

<a name="MM0133" />

#### <a name="mm0133-found-more-than-1-assembly-matching-0-choosing-first-1"></a>MM0133: Найдено более 1 сборки, совпадающих{0}с "" Выбор первой{1}: ""

<a name="MM0134" />

#### <a name="mm0134-32-bit-applications-should-be-migrated-to-64-bit"></a>MM0134: 32-разрядные приложения должны быть перенесены на 64-разрядную версию.

Компания Apple объявила о том, что она не допускает macOS приложений для хранения приложений в 32-разрядных приложениях (начиная с января 2018). 

Кроме того, 32-разрядные приложения не будут работать в версии macOS после высокого уровня Сьерра "без компромиссов". 

Дополнительные сведения: https://developer.apple.com/news/?id=06282017a

Рассмотрите возможность обновления приложения и всех зависимостей до 64-разр.

<a name="MM0135" />

#### <a name="mm0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MM0135: Не удалось связать системную платформу{0}"" (на которую ссылается{1}сборка ""), так как {2} она была введена в {3}, {2} и мы используем {4} пакет SDK.

Для сборки приложения Xamarin. Mac должен ссылаться на системные библиотеки, некоторые из которых зависят от версии пакета SDK, указанной в сообщении об ошибке. Поскольку используется более старая версия пакета SDK, вызовы этих API могут завершиться ошибкой во время выполнения.

Чтобы устранить эту ошибку, рекомендуется обновить Xcode, чтобы получить необходимый пакет SDK. Если у вас установлено несколько версий Xcode или вы хотите использовать Xcode в расположении, отличном от расположения по умолчанию, убедитесь, что в настройках IDE задано правильное расположение Xcode.

Кроме того, можно разрешить управляемому [компоновщику](https://docs.microsoft.com/xamarin/mac/deploy-test/linker) удалять неиспользуемые API, в том числе (в большинстве случаев) новые, для которых требуется Указанная библиотека. Однако это не будет работать, если для проекта требуются API, появившиеся в более новом пакете SDK, чем тот, который предоставляет Xcode.

В качестве последнего страв решения используйте более раннюю версию Xamarin. Mac, которая не требует наличия этих новых пакетов SDK в процессе сборки.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: File Copy/символических ссылок (связанный с проектом)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Не удалось создать символьную ссылку "{File}"-> "{Target}": ошибка {Number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Сборки продуктов

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: В ссылках{0}отсутствует обязательная сборка ""

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Сборка "{0}" несовместима с этим средством

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} не{1}удалось найти "". Целевая платформа{0}"" не может использоваться для упаковки приложения.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Недопустимая целевая платформа "{0}".

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: усефуллксаммакфрамеворк должен всегда быть целевым платформой .NET 4,5,{0}а не "", что недопустимо

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Недопустимая целевая платформа "{0}" при предназначенных Xamarin. Mac 4,5 .NET фрамворк.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Указана{0}несоответствиемежду Xamarin. Mac и целевой платформой "".{1}

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Ошибки при сборе сборок (не запрашивающих компоновщик)

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: Не удается разрешить ссылку:{0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Не является динамической библиотекой машинного ввода (неизвестный заголовок{0}"0x" {1}):.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Не является статической библиотекой (неизвестный{0}заголовок "" {1}):.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Не является динамической библиотекой машинного ввода (неизвестный заголовок{0}"0x" {1}):.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Неизвестный формат записи FAT в позиции {0} в {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Файл типа {0} не является мачо-файлом ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: Компоновщик

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Ошибки компоновщика (Общие)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Не удалось связать сборки

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: Не удается разрешить ссылку:{0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Параметр "{0}" будет пропущен, так как связь отключена

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Не удалось найти дополнительный файл определений{0}компоновщика "".

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Не удалось проанализировать определения из "{0}".

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Ссылка на собственную{0}библиотеку "" была указана, но найти ее не удалось.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Unified API Xamarin. Mac для полного профиля .NET не поддерживает связывание. Передайте флаг-with.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: {0}Ссылается.{1}     * * Это сообщение связано с MM2006 * *

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Неизвестный `{0}`HttpMessageHandler. Допустимые значения: HttpClientHandler (по умолчанию), Кфнетворкхандлер или Нсурлсессионхандлер

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Неизвестный Тлспровидер{0}".  Допустимые значения — Default или апплетлс.

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Только первые {0} из {1} указанных предупреждений. ** Это сообщение относится к 2009 г. \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Не удалось разрешить ссылку на "{0}", ссылка в "{1}". Приложение не будет включать сборку, на которую указывает ссылка, и может завершиться ошибкой во время выполнения.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Расширения Xamarin. Mac не поддерживают компоновку. Запрос на компоновку будет проигнорирован. ** Это сообщение является устаревшим в XM 3.6 + \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Недопустимый параметр тлспровидер `{0}` . Будет использовано единственное допустимое значение `{1}` .

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: Не удалось обработать XML-описание:{0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Не удалось обработать `...`оптимизатор привязки.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Classic API Xamarin. Mac не поддерживает связывание с платформой.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Ошибка при обработке сборки\*"": *

При обработке сборки произошла непредвиденная ошибка.

Сборка, вызвавшая ошибку, называется в сообщении об ошибке. Чтобы устранить эту проблему, сборка должна быть предоставлена в [отчете об ошибке](https://bugzilla.xamarin.com) вместе с полным журналом сборки с включенной детализацией (т. е `-v -v -v -v` . в **дополнительных аргументах mtouch**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Не удается связать сборку "{0}", так как она работает в смешанном режиме.

Сборки в смешанном режиме не могут быть обработаны компоновщиком.

Дополнительные https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assemblies сведения о сборках в смешанном режиме см. в разделе.

<a name="MM2106" />

#### <a name="mm2106-could-not-optimize-the-call-to-blockliteralsetupblockunsafe-in--at-offset--because-"></a>MM2106: Не удалось оптимизировать вызов Блокклитерал. Сетупблокк [unsafe] в * по смещению *, так как *.

Компоновщик сообщает об этом предупреждении, когда не удается оптимизировать вызов `BlockLiteral.SetupBlock` или. `Block.SetupBlockUnsafe`

Сообщение будет указывать на метод, вызывающий `BlockLiteral.SetupBlock[Unsafe]`, и может также дать объяснение, почему не удалось оптимизировать вызов.

Зарегистрируйте [проблему](https://github.com/xamarin/xamarin-macios/issues/new) вместе с полным журналом сборки, чтобы мы могли исследовать, что пошло не так, и, возможно, включить дополнительные сценарии в будущем.

<a name="MM2107" />

#### <a name="mm2107-its-not-safe-to-remove-the-dynamic-registrar-because-reasons"></a>MM2107: Удалить динамический регистратор необязательно, так как {reasons}

Компоновщик сообщает об этом предупреждении, когда разработчик запрашивает удаление динамического регистратора (путем передачи `--optimize:remove-dynamic-registrar` в MMP), но компоновщик определяет, что это ненадежно.

Чтобы удалить предупреждение, удалите аргумент оптимизации в MMP или передайте `--nowarn:2107` его, чтобы игнорировать.

По умолчанию этот параметр включается автоматически, когда это возможно и будет надежно.

<a name="MM2108" />

#### <a name="mm2108-0-was-stripped-of-architectures-except-1-to-comply-with-app-store-restrictions-this-could-break-exisiting-codesigning-signatures-consider-stripping-the-library-with-lipo-or-disabling-with---optimize-trim-architectures"></a>MM2108: "{0}" была удалена из архитектуры, за исключением "{1}", чтобы соответствовать ограничениям магазина приложений. Это может нарушить существующие соконструктивные подписи. Рассмотрите возможность удаления библиотеки с помощью липо или отключения с помощью архитектур--optimize =-Trim-Architecture.

В магазине приложений теперь отклоняются приложения, содержащие библиотеки и платформы, которые содержат 32-разрядные варианты. Библиотека была удалена из неиспользуемых архитектур при копировании в окончательный пакет приложений.

Это в общем случае является надежным и сокращает размер пакета приложений в качестве дополнительного преимущества. Однако любая упакованная платформа, подписанная кодом, будет иметь недействительную подпись (и повторно подписана позже, если приложение подписано).

Рассмотрите `lipo` возможность использования для окончательного удаления ненужных архитектур из исходной библиотеки. Если приложение не публикуется в App Store, это удаление можно отключить, передав `--optimize=-trim-architectures` в качестве дополнительных аргументов MMP дополнительные аргументы.

<a name="MM2109"/>

#### <a name="mm2109-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2109: Classic API Xamarin. Mac не поддерживает связывание с платформой.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Ошибки AOT (Общие)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: Не удалось AOT сборку "{0}"

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: Был запрошен,{0}но не найден AOT объекта ""

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Было запрошено исключение AOT{0}"", но оно не найдено

## <a name="mm4xxx-code-generation"></a>MM4xxx: создание кода

### <a name="mm40xx-driverm"></a>MM40xx: Driver. m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Не удалось расширить основной шаблон до `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: регистратор

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Приложение использует платформу "{0}", которая не входит в состав пакета SDK MacOS, который вы используете для создания приложения (Эта платформа была представлена в OSX {2}, при сборке с помощью пакета SDK для MacOS {1} ). Эта конфигурация не поддерживается со статическим регистратором (Pass--регистратор: dynamic в качестве дополнительного аргумента MMP в параметре сборки Mac вашего проекта для выбора). Также можно выбрать более новый пакет SDK в параметрах сборки Mac приложения.

<a name="MM4173" />

#### <a name="mm4173-the-registrar-cant-compute-the-block-signature-for-the-delegate-of-type-delegate-type-in-the-method-method-because-"></a>MM4173: Регистратор не может вычислить сигнатуру блока для делегата типа {Delegate-Type} в методе {Method}, так как *.

Это предупреждение означает, что Регистратору не удалось внедрить сигнатуру блока указанного метода в созданный код регистратора, так как регистратор не смог его вычислить.

Это означает, что подпись блока должна быть вычислена во время выполнения, что немного замедляется.

В настоящее время существует две возможные причины этого предупреждения:

1. Тип управляемого делегата — `System.Delegate` `System.MulticastDelegate`либо. Эти типы не представляют определенную сигнатуру, что означает, что регистратор не может вычислить соответствующую собственную сигнатуру. В этом случае исправлением является использование определенного типа делегата для блока (Кроме того, можно пропустить предупреждение, добавив `--nowarn:4173` в качестве дополнительного аргумента MMP в параметрах сборки Mac проекта).
2. Регистратор не может найти `Invoke` метод делегата. Это не должно происходить, поэтому напишите [вопрос](https://github.com/xamarin/xamarin-macios/issues/new) о тестовом проекте, чтобы мы могли исправить его.

<a name="MT4174" />

#### <a name="mt4174-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-methods-parameter-parameter"></a>MT4174: Не удалось разместить блок для делегирования метода преобразования для параметра # {Parameter} метода {method}.

Это предупреждение, указывающее, что статическому регистратору не удалось найти метод для создания делегата для блока цели-C. Во время выполнения будет предпринята попытка найти метод, но, скорее всего, произойдет сбой (с исключением MT8009).

Одной из возможных причин этого предупреждения является ручное написание привязок для API-интерфейса, который использует блоки. Рекомендуется использовать проект привязки для привязки кода цели-C — в частности, если он включает блоки, так как это очень сложно, чтобы его можно было сделать вручную.

Если это не так, напишите [вопрос](https://github.com/xamarin/xamarin-macios/issues/new) с тестовым случаем.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC и цепочки инструментов

### <a name="mm51xx-compilation"></a>MM51xx: компиляция

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Отсутствует компилятор{0}"". Установите компонент Xcode "средства командной строки".

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Не удалось выполнить компиляцию. Код ошибки- {0}. Отправляйте отчет об ошибках по адресу http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: связывание

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Отсутствует МДК Mono. Framework. Установите МДК для версии Mono. Framework с http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Не удается найти либксаммак. a, вероятно, из-за повреждения установки Xamarin. Mac. Переустановите Xamarin. Mac.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x86_64-is-only-supported-on-non-classic-profiles"></a>MM5204: Недопустимая архитектура. x86_64 поддерживается только для неклассических профилей.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x86_64-when---profilemobile"></a>MM5205: Недопустимая архитектура{0}"". Допустимые архитектуры: i386 и x86_64 (если--Profile = Mobile).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: Не удается проигнорировать динамический символ {symbol} (--Ignore-Dynamic-Symbol = {Symbol}), так как он не был определен как динамический символ.

См. [эквивалентное предупреждение mtouch](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: другие средства

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: не удалось найти файл pkg-config. Установите Mono. Framework из http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Отсутствует инструмент "Отул". Установите компонент Xcode "средства командной строки".

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: Отсутствуют зависимости. Установите компонент Xcode "средства командной строки".

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Возможно, лицензионное соглашение Xcode не принято.  Запустите Xcode.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Сбой встроенной компоновки с кодом ошибки 1.  Дополнительные сведения см. в журнале сборки.

<a name="MM5310" />

#### <a name="mm5310-install_name_tool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: сбой install_name_tool с кодом ошибки "{0}". Дополнительные сведения см. в журнале сборки.

<a name="MM5311" />

#### <a name="mm5311-lipo-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5311: сбой липо с кодом ошибки "{0}". Дополнительные сведения см. в журнале сборки.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: Runtime

### <a name="mm800x-misc"></a>MM800x: Прочие

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Сборщик мусора Boehm не поддерживается. Вместо этого используйте SGen.

<a name="MM8025" />

#### <a name="mm8025-failed-to-compute-the-token-reference-for-the-type-typeassemblyqualifiedname-because-reasons"></a>MM8025: Не удалось вычислить ссылку на маркер для типа "{Type". AssemblyQualifiedName} ", так как {reasons}

Это указывает на ошибку в Xamarin. Mac. Отправляйте ошибку по адресу [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac).

Возможным решением может быть отключение `register-protocols` оптимизации путем передачи `--optimize:-register-protocols` в качестве дополнительного аргумента MMP в параметрах сборки Mac проекта.

<a name="MM8026" />

#### <a name="mm8026--is-not-supported-when-the-dynamic-registrar-has-been-linked-away"></a>MM8026: * не поддерживается, если был связан динамический регистратор.
 
Обычно это указывает на ошибку в Xamarin. Mac, так как не следует связывать динамический регистратор, если это необходимо. Отправляйте ошибку по адресу [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
 
Можно заставить компоновщик обеспечить работу динамического регистратора, добавив `--optimize=-remove-dynamic-registrar` к дополнительным аргументам MMP в параметрах сборки Mac проекта.
