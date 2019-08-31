---
title: Сжатие файлов в Xamarin. iOS
description: В этом документе описывается работа с API либкомпрессион в Xamarin. iOS. В нем рассматривается деуровневое, Deflate и различные поддерживаемые алгоритмы.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: bcc63aa4e1926f5502d571bf47c83b0c8ea7e429
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199519"
---
# <a name="file-compression-in-xamarinios"></a>Сжатие файлов в Xamarin. iOS

Приложения Xamarin, предназначенные для iOS 9,0 или macOS 10,11 (и более поздние версии), могут использовать _платформу сжатия_ для сжатия (кодирования) и распаковки (расшифровки) данных. Xamarin. iOS предоставляет эту платформу после API потока. Платформа сжатия позволяет разработчикам взаимодействовать с сжатыми и распакованными данными, как если бы они были обычными потоками без необходимости использовать обратные вызовы или делегаты.

Платформа сжатия обеспечивает поддержку следующих алгоритмов:

* LZ4
* LZ4 RAW
* лзфсе
* лзма
* Zlib

Использование платформы сжатия позволяет разработчикам выполнять операции сжатия без каких-либо сторонних библиотек или NuGet. Это сокращает количество внешних зависимостей и гарантирует, что операции сжатия будут поддерживаться на всех платформах (при условии, что они соответствуют минимальным требованиям к ОС).

## <a name="general-file-decompression"></a>Общее распаковка файлов

Платформа сжатия использует API-интерфейс потока в Xamarin. iOS и Xamarin. Mac. Этот API означает, что для сжатия данных разработчик может использовать обычные шаблоны, используемые в других API-интерфейсах ввода-вывода в .NET. В следующем примере показано, как распаковать данные с помощью платформы сжатия, похожей на API, найденный в `System.IO.Compression.DeflateStream` API:

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

Класс реализует интерфейс, как и другие `System.IO.Streams`, поэтому разработчики должны обеспечить освобождение ресурсов, когда они больше не требуются. `IDisposable` `CompressionStream`

## <a name="general-file-compression"></a>Общее сжатие файлов

API сжатия также позволяет разработчикам сжимать данные после того же API. Данные можно сжимать с помощью одного из указанных алгоритмов, `CompressionAlgorithm` указанных в перечислителе.

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

Поддерживает все асинхронные операции, поддерживаемые `System.IO.DeflateStream`. Это означает, что разработчики могут использовать ключевое слово async для выполнения операций сжатия и распаковки без блокировки потока пользовательского интерфейса. `CompressionStream`
