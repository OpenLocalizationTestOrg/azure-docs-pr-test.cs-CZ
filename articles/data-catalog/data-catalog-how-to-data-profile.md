---
title: aaaHow tooData zdroje dat.
description: "Jak tooarticle zvýraznění jak toouse dat profilů toounderstand zdroje dat a jak profily tooinclude úrovni tabulky a sloupce dat při registraci zdroje dat v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 12c9f38501cdaee903d0dcbbdd0b82395f35a187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-profile-data-sources"></a>Zdroje dat profilů dat
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Jinými slovy **Azure Data Catalog** všechny o pomoc osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím tooget další hodnoty ze své stávající data. Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou hello, ale není hello scénáře ukončení existuje.

Hello **dat profilace** funkce **Azure Data Catalog** prozkoumá hello data z podporovaných zdrojů dat v katalogu a shromažďuje statistické údaje a informace o těchto datech. Je snadno tooinclude profil datových prostředků. Když si zaregistrujete datový prostředek, zvolte **zahrnují Data profilu** v nástroj registrace zdroje dat hello.

## <a name="what-is-data-profiling"></a>Co je profilace dat
Data profilování prozkoumá hello dat ve zdroji dat hello registrované a shromažďuje statistické údaje a informace o těchto datech. Během zjišťování zdroje dat ve statistikách vám pomohou určit hello vhodnosti hello data toosolve obchodního problému.

<!-- In [How toodiscover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How tooinclude a data profile when registering a data source](#howto). -->

Hello následující zdroje dat podporují profilace dat:

* SQL Server (včetně databáze SQL Azure a Azure SQL Data Warehouse) tabulek a zobrazení
* Oracle tabulek a zobrazení
* Teradata tabulek a zobrazení
* Tabulek Hive

Včetně profily dat při registraci datových prostředků pomáhá uživatelům odpovězte otázky o zdrojích dat, včetně:

* Můžete je možné použít toosolve Moje obchodního problému?
* Hello dat odpovídat tooparticular standardy nebo vzory?
* Jaké jsou některé hello anomálií hello zdroje dat?
* Jaké jsou možné problémy integrace tato data do Moje aplikace?

> [!NOTE]
> Můžete také přidat dokumentaci tooan asset toodescribe tom, jak může být data integrovaná do aplikace. V tématu [jak zdroje dat toodocument](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-tooinclude-a-data-profile-when-registering-a-data-source"></a>Jak tooinclude dat profilu při registraci zdroje dat
Je snadno tooinclude profil datového zdroje. Při registraci zdroje dat, v hello **toobe objekty zaregistrován** panelu hello registrace zdroje dat nástroje, zvolte **zahrnují Data profilu**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

toolearn informace o tom, jak zjistit, tooregister datových zdrojů, [jak zdroje dat tooregister](data-catalog-how-to-register.md) a [Začínáme s Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtrování datových prostředků, které zahrnují data profily
toodiscover datových prostředků, které zahrnují data profilu, můžete zahrnout `has:tableDataProfiles` nebo `has:columnsDataProfiles` jako jeden z vašich podmínek vyhledávání.

> [!NOTE]
> Výběr **zahrnují Data profilu** v datech hello zahrnuje nástroj registrace zdroje tabulky a informace o profilu úrovni sloupce. Však hello API katalogu Data Catalog umožňuje toobe prostředky dat registrovány jen jednu sadu zahrnuty informace o profilu.
>
>

## <a name="viewing-data-profile-information"></a>Informace o profilu zobrazení dat
Jakmile zjistíte, zdroj dat vhodný k profilu, můžete zobrazit podrobnosti hello data profilu. tooview hello dat profilu, vyberte datový prostředek a zvolte **Data profilu** v okně portálu hello katalogu Data Catalog.

![](media/data-catalog-data-profile/data-catalog-view.png)

Data profilu v **Azure Data Catalog** zobrazuje tabulky a sloupce profil informace, včetně:

### <a name="object-data-profile"></a>Objekt dat profilu
* Počet řádků
* Velikost tabulky
* Poslední aktualizace hello objektu

### <a name="column-data-profile"></a>Sloupec dat profilu
* Datový typ sloupce
* Počet jedinečných hodnot
* Počet řádků s hodnotami NULL
* Minimum, maximum, průměr a standardní odchylce pro hodnoty ve sloupcích

## <a name="summary"></a>Souhrn
Data profilování poskytuje statistiky a informace o registrované datové prostředky toohelp určit hello vhodnosti hello data toosolve obchodních problémů. Zadávání poznámek a dokumentace zdroje dat, data profily můžete uživatelům lépe pochopili, vaše data.

## <a name="see-also"></a>Viz také
* [Jak tooregister zdroje dat](data-catalog-how-to-register.md)
* [Začínáme s Azure Data Catalogem](data-catalog-get-started.md)
