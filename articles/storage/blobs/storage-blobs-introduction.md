---
title: "aaaIntroduction tooAzure úložiště objektů Blob | Microsoft Docs"
description: "Úvod tooAzure úložiště objektů Blob"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: robinsh
ms.openlocfilehash: 3431f826ae51d42dbced084ee60f9ff70a8168d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooblob-storage"></a>Úvod tooBlob úložiště

Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS. Data tooexpose úložiště objektů Blob můžete použít veřejně toohello svět nebo data aplikací toostore soukromě.

Mezi běžná použití služby Blob Storage patří:

* Slouží obrázků nebo dokumentů přímo tooa prohlížeče
* ukládání souborů pro distribuovaný přístup
* streamování videa a zvuku
* ukládání dat pro zálohování a obnovování, zotavení po havárii a pro archivaci
* ukládání dat, které bude analyzovat místní nebo v Azure hostovaná služba

## <a name="blob-service-concepts"></a>Koncepty služby Blob service

Hello služby objektů Blob obsahuje hello následující součásti:

![Architektura objektu blob](./media/storage-blobs-introduction/blob1.png)

* **Účet úložiště:** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Tento účet úložiště může být **účet úložiště pro obecné účely** nebo **účtem služby Blob storage** který se specializuje na ukládání objektů BLOB. Další informace najdete v tématu [Účty Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Kontejner:** Kontejner zajišťuje seskupení sady objektů blob. Všechny objekty blob musí být v kontejneru. Účet může obsahovat neomezený počet kontejnerů. Kontejner můžete pojmout neomezený počet objektů blob. Všimněte si, že hello název kontejneru musí být malá písmena.

* **Objekt blob:** Soubor libovolného typu a velikosti. Úložiště Azure Storage nabízí tři typy objektů blob – objekty blob bloků, doplňovací objekty blob a objekty blob stránek.
  
    *Objekty blob bloků* jsou ideální pro ukládání textových nebo binárních souborů, například dokumentů a multimediálních souborů. *Doplňovací objekty BLOB* jsou podobné objektům BLOB tooblock v tom, že je tvoří bloky, ale jsou optimalizované pro doplňovací operace, takže se hodí pro scénáře protokolování. Objekt blob jeden blok může obsahovat až too50 000 bloků až too100 MB, každý na celkovou velikost mírně větší, než 4.75 TB (100 MB × 50 000). Jeden doplňovací objekt blob může obsahovat až too50 000 bloků až too4 MB, každý na celkovou velikost něco větší než 195 GB (4 MB × 50 000).
  
    *Objekty BLOB stránek* může být až velikost too1 TB a jsou efektivnější pro časté čtení/zápisu operace. Virtuální počítače Azure používá objekty BLOB stránek jako disky s operačním systémem a daty.
  
    Podrobnosti o vytváření názvů kontejnerů a objektů blob najdete v článku [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

## <a name="next-steps"></a>Další kroky

* [Vytvoření účtu úložiště](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Začínáme s úložištěm Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md)