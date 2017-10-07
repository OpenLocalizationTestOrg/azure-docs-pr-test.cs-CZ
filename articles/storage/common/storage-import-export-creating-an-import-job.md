---
title: "aaaCreate úlohy importu pro Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak toocreate importu pro hello služby Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: da974c33a3688bb5e2412c8bfcbeca704096c2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-import-job-for-hello-azure-importexport-service"></a>Vytvoření úlohy importu pro hello služba Azure Import/Export

Vytvoření úlohy importu pro službu Microsoft Azure Import/Export hello pomocí hello REST API zahrnuje hello následující kroky:

-   Příprava jednotky se hello nástroj Azure Import/Export.

-   Získání hello umístění toowhich tooship hello jednotky.

-   Vytvoření úlohy importu hello.

-   Přesouvání hello jednotky tooMicrosoft prostřednictvím podporovaných poskytovatel služby.

-   Probíhá aktualizace úlohy importu hello hello přesouvání podrobnosti.

 V tématu [pomocí hello Microsoft Azure Import/Export služby tooTransfer Data tooBlob úložiště](storage-import-export-service.md) přehled hello importu/exportu služby a kurz, který ukazuje, jak toouse hello [portál Azure](https://portal.azure.com/) toocreate a spravovat import a export úloh.

## <a name="preparing-drives-with-hello-azure-importexport-tool"></a>Příprava jednotky se hello nástroj Azure Import/Export

pro úlohy importu Hello jednotek tooprepare kroky jsou hello stejné jestli vytvoříte hello jobvia hello portálu nebo pomocí hello REST API.

Níže je stručný přehled přípravy jednotky. Odkazovat toohello [Azure Import ExportTool odkaz](storage-import-export-tool-how-to-v1.md) úplné pokyny. Můžete si stáhnout hello nástroj Azure Import/Export [zde](http://go.microsoft.com/fwlink/?LinkID=301900).

Příprava vašeho disku zahrnuje:

-   Identifikace toobe hello data importovat.

-   Identifikace hello cílové objekty BLOB v systému Windows Azure Storage.

-   Pomocí nástroje Azure Import/Export toocopy hello vaše data tooone nebo více pevných disků.

 Hello nástroj Azure Import/Export také vygeneruje soubor manifestu pro každou hello jednotek, jako je připraveno. Soubor manifestu obsahuje:

-   Výčet všech hello soubory určené pro odesílání a hello mapování z tooblobs tyto soubory.

-   Kontrolní součty segmentů hello každého souboru.

-   Informace o hello metadata a vlastnosti tooassociate s jednotlivých objektů blob.

-   Výpis tootake akce hello, pokud má objekt blob, který se nahrává hello stejný název jako existující objekt blob v kontejneru hello. Dostupné možnosti patří: a) souborem hello přepsat hello blob, b) zachovat existující objekt blob hello a skip nahrávání souboru hello, c) připojit příponu toohello tak, aby nedošlo ke konfliktu s ostatními soubory.

## <a name="obtaining-your-shipping-location"></a>Získání vaši polohu přesouvání

Před vytvořením úlohy importu, budete potřebovat tooobtain přenosů název umístění a adresy podle volání hello [umístění seznamu](/rest/api/storageimportexport/listlocations) operaci. `List Locations`Vrátí seznam umístění a jejich poštovní adresy. Můžete vybrat umístění z hello vrátil seznam a dodávat vaše adresa toothat pevné disky. Můžete taky hello `Get Location` operace tooobtain hello přímo přesouvání adresu konkrétního umístění.

 Postupujte podle kroků hello tooobtain hello přenosů umístění:

-   Určení názvu hello hello umístění účtu úložiště. Tuto hodnotu najdete v části hello **umístění** na účet úložiště hello **řídicí panel** v hello Azure portálu nebo předmětem dotazu pro pomocí hello service management operace rozhraní API [získat úložiště Účet vlastnosti](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Načíst hello umístění, které je k dispozici tooprocess tento účet úložiště tak, že volání hello `Get Location` operaci.

-   Pokud hello `AlternateLocations` vlastnost hello umístění obsahuje hello umístění sám sebe a potom je v pořádku toouse toto umístění. Jinak volání hello `Get Location` operaci zopakovat s hello alternativního umístění. Hello původního umístění může být dočasně ukončeny kvůli údržbě.

## <a name="creating-hello-import-job"></a>Vytvoření úlohy importu hello
Úloha importu toocreate hello, volání hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci. Budete potřebovat tooprovide hello následující informace:

-   Název úlohy hello.

-   název účtu úložiště Hello.

-   Hello přesouvání název umístění, získané z předchozího kroku hello.

-   Typ úlohy (Importovat).

-   zpětná adresa Hello kde hello jednotky by měly být odeslány po dokončení úlohy importu hello.

-   Hello seznam jednotek v úloze hello. Pro každou jednotku musí zahrnovat následující informace, který byl získán během kroku přípravy jednotky hello hello:

    -   jednotka Hello Id

    -   klíč nástroje BitLocker Hello

    -   relativní cesta souboru manifestu Hello na pevném disku hello

    -   Hello Base16 kódovaný hodnota hash MD5 souboru manifestu

## <a name="shipping-your-drives"></a>Přesouvání jednotky
Je nutné dodat vaši adresu toohello jednotky, který jste získali v předchozím kroku hello a je nutné zadat hello importu/exportu služby s hello sledování počtu hello balíčku.

> [!NOTE]
>  Je nutné dodat jednotky prostřednictvím podporovaných poskytovatel služby, která bude poskytovat sledování Číslo pro svůj balíček.

## <a name="updating-hello-import-job-with-your-shipping-information"></a>Aktualizace úlohy importu hello s informace o způsobu dodání
Až budete mít vaše číslo sledování, volání hello [vlastnosti úlohy aktualizace](/api/storageimportexport/jobs#Jobs_Update) hello tooupdate operace přesouvání poskytovatel název, číslo sledování hello hello úlohy a hello poskytovatel účet číslo návratový přesouvání. Volitelně můžete zadat počet hello jednotky a hello přesouvání i datum.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
