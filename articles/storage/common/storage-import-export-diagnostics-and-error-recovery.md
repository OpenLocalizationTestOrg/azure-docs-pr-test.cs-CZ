---
title: "obnovení aaaDiagnostics a chyby pro úlohy Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak služba tooenable podrobné protokolování pro Microsoft Azure Import/Export úloh."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 48164279e7904c78fed802aa3cff66e589c3f12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnostiky a zotavení po chybě pro úlohy Azure Import/Export
Pro každou jednotku zpracovat vytvoří hello služba Azure Import/Export protokol chyb v hello přidruženého účtu úložiště. Můžete také povolit podrobné protokolování tak nastavení hello `LogLevel` vlastnost příliš`Verbose` při volání metody hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace.

 Ve výchozím nastavení, protokoly zapisují tooa kontejner s názvem `waimportexport`. Můžete zadat jiný název nastavení hello `DiagnosticsPath` vlastnost při volání metody hello `Put Job` nebo `Update Job Properties` operace. Hello protokoly se ukládají jako objekty BLOB bloku s hello následující zásady vytváření názvů: `waies/jobname_driveid_timestamp_logtype.xml`.

 Můžete načíst hello URI hello protokoly pro úlohu pomocí volání hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci. Hello identifikátor URI pro hello podrobného protokolování se vrátí v hello `VerboseLogUri` vlastnosti pro každou jednotku, zatímco hello identifikátor URI pro protokol chyb hello je vrácený v hello `ErrorLogUri` vlastnost.

Můžete použít hello protokolování dat tooidentify hello následující problémy.

## <a name="drive-errors"></a>Jednotka chyby

Hello následující položky jsou klasifikovány jako chyby jednotky:

-   Soubor manifestu chyby v přístupu ke službám nebo čtení hello

-   Nesprávný klíče nástroje BitLocker

-   Jednotky pro čtení a zápis chyby

## <a name="blob-errors"></a>Objekt BLOB chyby

Hello následující položky jsou klasifikovány jako chyby objektů blob:

-   Objekt blob nesprávnou či konfliktní nebo názvy

-   Chybějící soubory

-   Nebyl nalezen objekt BLOB

-   Zkrácený soubory (hello soubory na disku hello jsou menší, než je zadáno v manifestu hello)

-   Poškozený soubor obsahu (pro úlohy importu, zjistila se neshoda MD5 kontrolního součtu)

-   Soubory metadat a vlastnost poškozená blob (zjistila se neshoda MD5 kontrolního součtu)

-   Nesprávné schéma pro vlastnosti objektů blob hello a soubory metadat

Můžou nastat případy, kdy některé části úlohu import nebo export není úspěšně dokončil, zatímco hello celkové stále dokončení úlohy. V takovém případě můžete buď odeslání nebo stažení hello chybí částí hello dat přes síť, nebo můžete vytvořit nové data hello tootransfer úlohy. V tématu hello [Azure Import/Export nástroj odkaz](storage-import-export-tool-how-to-v1.md) toolearn jak toorepair hello dat přes síť.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
