---
title: "aaaCreate exportu úlohy pro Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak toocreate exportu úlohy pro hello služby Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 4a10b42cc86dbf3bcea3a515bc065e2259228ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-export-job-for-hello-azure-importexport-service"></a>Vytvoření úlohy exportu pro hello služba Azure Import/Export
Vytvoření úlohy exportu pro službu Microsoft Azure Import/Export hello pomocí hello REST API zahrnuje hello následující kroky:

-   Výběr hello objekty BLOB tooexport.

-   Získání přesouvání umístění.

-   Vytvoření úlohy exportu hello.

-   Přesouvání vaší prázdný jednotky tooMicrosoft prostřednictvím podporovaných poskytovatel služby.

-   Probíhá aktualizace informací o balíčku hello úloha exportu hello.

-   Přijetí hello jednotky zpět od společnosti Microsoft.

 V tématu [pomocí hello Windows Azure Import/Export služby tooTransfer Data tooBlob úložiště](storage-import-export-service.md) přehled hello importu/exportu služby a kurz, který ukazuje, jak toouse hello [portál Azure](https://portal.azure.com/) toocreate a spravovat import a export úloh.

## <a name="selecting-blobs-tooexport"></a>Výběr tooexport objektů BLOB
 toocreate úlohy exportu, budete potřebovat tooprovide seznam objektů BLOB, který má tooexport z vašeho účtu úložiště. Existuje několik způsobů tooselect objekty BLOB toobe export:

-   Můžete vytvořit tooselect cesta relativní objektů blob a jediného objektu blob a všechny jeho snímků.

-   Můžete vytvořit tooselect cesta relativní blob jediného objektu blob s výjimkou jeho snímků.

-   Můžete použít cestu k relativní objektů blob a tooselect čas snímku jeden snímek.

-   Předpona tooselect objektů blob můžete použít všechny objekty BLOB a snímky s hello zadané předpony.

-   Můžete exportovat všechny objekty BLOB a snímky v účtu úložiště hello.

 Další informace o zadání objekty BLOB tooexport, najdete v části hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.

## <a name="obtaining-your-shipping-location"></a>Získání vaši polohu přesouvání
Před vytvořením úlohy exportu, budete potřebovat tooobtain přenosů název umístění a adresy podle volání hello [získat umístění](https://portal.azure.com) nebo [umístění seznamu](/rest/api/storageimportexport/listlocations) operaci. `List Locations`Vrátí seznam umístění a jejich poštovní adresy. Můžete vybrat umístění z hello vrátil seznam a dodávat vaše adresa toothat pevné disky. Můžete taky hello `Get Location` operace tooobtain hello přímo přesouvání adresu konkrétního umístění.

Postupujte podle kroků hello tooobtain hello přenosů umístění:

-   Určení názvu hello hello umístění účtu úložiště. Tuto hodnotu najdete v části hello **umístění** na účet úložiště hello **řídicí panel** v klasickém hello portálu nebo předmětem dotazu pro pomocí hello service management operace rozhraní API [získat Vlastnosti účtu úložiště](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Načíst hello umístění, které jsou k dispozici tooprocess tento účet úložiště tak, že volání hello `Get Location` operaci.

-   Pokud hello `AlternateLocations` vlastnost hello umístění obsahuje hello umístění sám sebe a potom je v pořádku toouse toto umístění. Jinak volání hello `Get Location` operaci zopakovat s hello alternativního umístění. Hello původního umístění může být dočasně ukončeny kvůli údržbě.

## <a name="creating-hello-export-job"></a>Vytvoření úlohy exportu hello
 Úloha exportu toocreate hello, volání hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci. Budete potřebovat tooprovide hello následující informace:

-   Název úlohy hello.

-   název účtu úložiště Hello.

-   Hello přesouvání název umístění, získaných v předchozím kroku hello.

-   Typ úlohy (exportovat).

-   zpětná adresa Hello kde hello jednotky by měly být odeslány po dokončení úlohy exportu hello.

-   seznam objektů BLOB (nebo objekt blob předpony) toobe Hello exportovat.

## <a name="shipping-your-drives"></a>Přesouvání jednotky
 V dalším kroku použít hello nástroj Azure Import/Export toodetermine hello počet jednotek, je nutné toosend, založené na objekty BLOB hello výběru toobe exportovali a hello velikost disku. V tématu hello [Azure Import/Export nástroj odkaz](storage-import-export-tool-how-to-v1.md) podrobnosti.

 Balíček hello jednotky v jednom balíčku a dodávat je toohello adres získaných v hello dříve krok. Všimněte si hello sledování Číslo vašeho balíčku hello další krok.

> [!NOTE]
>  Je nutné dodat jednotky prostřednictvím podporovaných poskytovatel služby, která bude poskytovat sledování Číslo pro svůj balíček.

## <a name="updating-hello-export-job-with-your-package-information"></a>Aktualizace úlohy exportu hello s informací o balíčku
 Až budete mít vaše číslo sledování, volání hello [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace tooupdated hello poskytovatel název a číslo pro úlohu hello sledování. Volitelně můžete zadat počet hello jednotky, hello zpáteční adresu a také hello přesouvání datum.

## <a name="receiving-hello-package"></a>Přijetí balíčku hello
 Po zpracování vaše úloha exportu, jednotky, bude vrácen tooyou s šifrovaná data. Pro každou hello jednotek tím volání hello můžete načíst klíč nástroje BitLocker hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci. Potom můžete odemknout jednotku hello pomocí klíče hello. Soubor manifestu Hello jednotku na každém disku obsahuje hello seznam souborů na jednotce hello také hello původní objekt blob adresu pro každý soubor.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
