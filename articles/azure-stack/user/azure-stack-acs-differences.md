---
title: "Zásobník úložiště Azure: Rozdíly a aspekty"
description: "Porozumějte rozdílům mezi zásobník úložiště Azure a Azure Storage, společně s aspekty nasazení Azure zásobníku."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/08/2017
ms.author: jeffgilb
ms.openlocfilehash: 1dc099fa234e217b682c88f2214fe271c916eec2
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Zásobník úložiště Azure: Rozdíly a aspekty

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Azure zásobníku úložiště je sada cloudových služeb úložiště v Microsoft Azure zásobníku. Úložiště Azure zásobníku obsahuje objekt blob, tabulky, fronty a funkce správy účet s Azure konzistentní sémantiku.

Tento článek shrnuje známé rozdíly zásobník úložiště Azure z úložiště Azure. Shrnuje také další důležité informace, třeba vzít v úvahu při nasazování Azure zásobníku. Další informace o nejvýraznějších rozdílů mezi zásobník Azure a Azure, najdete v článku [klíčové aspekty](azure-stack-considerations.md) tématu.

## <a name="cheat-sheet-storage-differences"></a>Tahák: rozdíly úložiště

| Funkce | Azure (globální) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloudové sdílené složky SMB podporované|Není dosud podporován.
|Šifrování služby Azure Storage pro neaktivní uložená data|šifrování AES 256 bitů|Není dosud podporován.
|Typ účtu úložiště|Účty úložiště Blob pro obecné účely a Azure|Pro obecné účely pouze
|Možnosti replikace|Místně redundantní úložiště, geograficky redundantní úložiště, geograficky redundantní úložiště s přístupem pro čtení a zónově redundantní úložiště|Místně redundantní úložiště
|Storage úrovně Premium|Plně podporovány.|Může být zřízen, ale žádné omezení výkonu nebo záruk
|Managed Disks|Premium a standard podporováno|Není dosud podporován.
|Název objektu BLOB|1 024 znaků (2 048 bajtů)|880 znaků (1,760 bajty)
|Maximální velikost objektu blob bloku|4.75 TB (100 MB × 50 000 bloků)|50 000 × 4 MB (poli 195 GB)
|Kopie snímků objekt blob stránky|Zálohování nespravované virtuální počítač disky Azure připojené k spuštění virtuálního počítače podporované|Není dosud podporován.
|Kopie přírůstkový snímek objekt blob stránky|Premium a objekty BLOB stránky standardní Azure podporován|Není dosud podporován.
|Maximální velikost stránky objektu blob|8 TB|1 TB
|Velikost stránky objektu blob stránky|512 bajtů|4 KB
|Velikost klíče tabulky klíč oddílu a řádku|1 024 znaků (2 048 bajtů)|400 znaků (800 bajtů)

### <a name="metrics"></a>Metriky
Existují také některé rozdíly mezi metriky úložiště:
* Data transakce v úložiště metriky nerozlišuje interních nebo externích šířku pásma sítě.
* Data transakce v úložiště metriky nezahrnuje virtuálnímu počítači přístup k připojené disky.

## <a name="api-version"></a>Verze rozhraní API
S Azure zásobníku úložiště jsou podporovány následující verze:

* Data služby Azure Storage: [verze 2015-04-05 rozhraní REST API](https://docs.microsoft.com/rest/api/storageservices/Version-2015-04-05?redirectedfrom=MSDN)
* Správa služby Azure Storage: [2015-05-01-preview, 2015-06-15 a 2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN) 

## <a name="next-steps"></a>Další kroky

* [Začínáme s Azure Storage zásobníku nástroje pro vývoj](azure-stack-storage-dev.md)
* [Úvod do Azure zásobník úložiště](azure-stack-storage-overview.md)

