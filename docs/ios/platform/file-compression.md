---
title: Сжатие файлов в Xamarin.iOS
description: В этом документе описывается, как работать с libcompression API в Xamarin.iOS. В нем описывается сократятся, подобных, и различных поддерживаемых алгоритмов.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: f7a1df65047fd8040dd40e9f7f057d6bfe6dea61
ms.sourcegitcommit: 4c97f5d73be7eb2da153a85183be4258b6b11ca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58290933"
---
# <a name="file-compression-in-xamarinios"></a>Сжатие файлов в Xamarin.iOS

Приложения Xamarin, предназначенных для iOS 9.0 или macOS 10.11 (и выше) можно использовать _Framework сжатия_ для сжатия (кодирование) и распаковки (декодирования) данных. Xamarin.iOS предоставляет этой платформы следуя Stream API. Сжатие framework позволяет разработчикам взаимодействовать с compress и распаковать данные, как если бы они обычных потоков без необходимости использовать обратные вызовы и делегаты.

Сжатие framework обеспечивает поддержку следующих алгоритмов:

* LZ4
* Необработанные LZ4
* Lzfse
* Lzma
* Zlib

Использование сжатия framework позволяет разработчикам выполнять операции сжатия без сторонних библиотек или пакеты NuGet. Это уменьшает внешние зависимости и гарантирует операции сжатия, которые будут поддерживаться на всех платформах, (при условии, что они соответствуют минимальным требованиям операционной системы).

## <a name="general-file-decompression"></a>Распаковка файла общие

Инфраструктура сжатие использует API потока в Xamarin.iOS и Xamarin.Mac. Этот API означает, что для сжатия данных, разработчик может использовать обычные шаблоны, используемые в других API-интерфейсов ввода-ВЫВОДА в .NET. Следующий пример показано, как распаковать данные с помощью платформы сжатия, аналогичный API, в `System.IO.Compression.DeflateStream` API:

```csharp
// sample zlib data
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompression stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

`CompressionStream` Реализует `IDisposable` интерфейс, как другой `System.IO.Streams`, поэтому разработчики должны гарантировать, что ресурсы освобождаются, если они больше не нужны.

## <a name="general-file-compression"></a>Сжатие общих файлов

Сжатие API также позволяет разработчикам сжатие данных и тот же API. Данные могут быть сжаты с помощью одного из алгоритмов из предоставленного набора в `CompressionAlgorithm` перечислителя.

```csharp
// sample method that copies the data from the source stream to the destination stream
static void CopyStream (Stream src, Stream dest)
{
    byte[] array = new byte[1024];
    int bytes_read;
    bytes_read = src.Read (array, 0, 1024);
    while (bytes_read != 0) {
        dest.Write (array, 0, bytes_read);
        bytes_read = src.Read (array, 0, 1024);
    }
}

static void CompressExample ()
{
    // create some sample data to compress
    byte [] data = new byte[100000];
    for (int i = 0; i < 100000; i++) {
        data[i] = (byte) i;
    }

    using (var dataStream = new MemoryStream (data)) // stream that contains the data to compress
    using (var backing = new MemoryStream ()) // stream in which the compress data will be written
    using (var compressing = new CompressionStream (backing, CompressionMode.Compress, CompressionAlgorithm.Zlib, true))
    {
        // copy the data to the compressing stream
        CopyStream (dataStream, compressing);
        // at this point, the CompressionStream contains all the data from the dataStream but compressed
        // using the zlib algorithm
    }
```

## <a name="async-support"></a>Поддержка асинхронного выполнения

`CompressionStream` Поддерживает все асинхронные операции, поддерживаемые `System.IO.DeflateStream`, что означает, что разработчики могут использовать ключевое слово async выполнять сжатие и распаковку операции без блокирования потока пользовательского интерфейса.