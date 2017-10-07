---
title: aaaRegister zdroje dat v Azure Data Catalog | Microsoft Docs
description: "Tento článek popisuje, jak extrahovat tooregister zdroje dat v Azure Data Catalog, včetně pole metadat hello během registrace."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: efc8a852ddc9fb4bbacc7b0280477bd47814936f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Registrace zdrojů dat v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog je plně spravovaná Cloudová služba, která slouží jako systém registrace a zjišťování datových zdrojů organizace. Jinými slovy Data Catalog umožňuje uživatelům zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu ze své stávající data. první krok toomaking zdroj dat Hello zjistitelný prostřednictvím katalogu Data Catalog je tooregister zdroje dat.

## <a name="register-data-sources"></a>Registrace zdrojů dat
Registrace je proces hello extrakce dat ze zdroje dat hello a kopírování tohoto data toohello služby Data Catalog. Hello data zůstanou kde se nyní nachází, a zůstane pod dohledem hello hello správců a zásady hello aktuálním systému.

tooregister zdroje dat, hello následující:
1. Na portálu Azure Data Catalog hello spusťte nástroj registrace zdroje dat katalogu Data Catalog hello. 
2. Přihlaste pomocí svého pracovního nebo školního účtu s hello stejné přihlašovací údaje služby Azure Active Directory použít toosign toohello portálu.
3. Vyberte zdroj dat hello chcete tooregister.

Další podrobné informace najdete v tématu hello [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurzu.

Poté, co jste registrováni hello zdroj dat, katalog hello sleduje jeho umístění a indexy jeho metadata. Uživatele můžete vyhledávat, procházet a zjišťovat zdroje dat hello a potom pomocí jeho umístění tooconnect tooit pomocí aplikace hello nebo nástroj podle svého výběru.

## <a name="supported-data-sources"></a>Podporované zdroje dat
Seznam aktuálně podporované zdroje dat, naleznete v části [DSR katalogu dat](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Strukturální metadata
Při registraci zdroje dat, nástroj pro registraci hello extrahuje informace o struktuře hello hello objektů, které vyberete. Tyto informace jsou odkazované tooas strukturální metadata.

Pro všechny objekty zahrnuje tato strukturální metadata umístění hello objektu, tak, aby uživatelé, kteří zjistit hello dat můžete použít tento objekt toohello tooconnect informace v nástrojích klienta hello podle svého výběru. Další strukturální metadata obsahuje název objektu a typu a zadejte název atributu/sloupce a data.

## <a name="descriptive-metadata"></a>Popisná metadata
Kromě toho toohello základní strukturální metadata, která je extrahována ze zdroje dat hello, nástroj registrace zdroje dat hello extrahuje popisných metadat. Pro SQL Server Analysis Services a SQL Server Reporting Services je tato metadata převzat ze vlastnosti popisu hello vystavené těchto služeb. Pro systém SQL Server, hodnoty poskytnuté pomocí hello ms\_popis rozšířené vlastnosti je rozbalena. Pro databáze Oracle hello zdroj dat registrace nástroj extrahuje hello KOMENTÁŘE sloupec z hello všechny\_KARTĚ\_zobrazení KOMENTÁŘE.

V přidání toohello popisný metadatech, které je extrahována ze zdroje dat hello mohou uživatelé zadávat popisných metadat pomocí nástroj registrace zdroje dat hello. Uživatele můžete přidat značky a identifikují odborníky pro objekty hello registrována. Zkopírovat všechny, které je tato popisná metadata služby Data Catalog toohello společně s hello strukturální metadata.

## <a name="include-previews"></a>Zahrnout verze Preview
Ve výchozím nastavení je pouze metadata extrahovány ze zdroje dat a zkopírovaný toohello služby Data Catalog, ale které zdroj dat je často snazší když zobrazujete ukázku hello data, která obsahuje.

Pomocí nástroje registrace hello katalogu Data Catalog zdroje dat, můžete zahrnout náhled snímku hello data v každé tabulce a zobrazení, která je zaregistrovaná. Pokud si zvolíte tooinclude náhledy během registrace, nástroj pro registraci hello zahrnuje až too20 záznamů z každé tabulky a zobrazení. Tento snímek se pak zkopíruje toohello katalogu společně s metadata hello strukturální a popisný.

> [!NOTE]
> Široké tabulky s velkým počtem sloupců může mít méně než 20 záznamů, které jsou zahrnuty v jejich náhled.
>
>

## <a name="include-data-profiles"></a>Zahrnují data profily
Stejně jako verze Preview včetně může poskytnout cenné kontext pro uživatele, kteří vyhledávat zdroje dat v katalogu Data Catalog, včetně dat profilu může být snazší zdroje dat toounderstand zjištěny.

Pomocí nástroje registrace hello katalogu Data Catalog zdroje dat, můžete zahrnout profil data pro jednotlivé tabulky a zobrazení, která je zaregistrovaná. Pokud si zvolíte tooinclude profil dat během registrace, nástroj pro registraci hello zahrnuje souhrnné statistiky o hello dat v každém tabulky a zobrazení, včetně:

* Hello počtu řádků a velikost dat hello v objektu hello.
* Hello datum poslední aktualizace hello hello dat a schématu objekt hello.
* Hello počet záznamů hodnotu null a odlišné hodnoty pro sloupce.
* Hello hodnoty pro sloupce minimum, maximum, průměr a směrodatné odchylky.

Tyto statistické údaje jsou pak zkopíruje toohello katalogu společně s metadata hello strukturální a popisný.

> [!NOTE]
> Text a datum sloupce nezahrnují statistiku průměr či směrodatná odchylka v jejich data profilu.
>
>

## <a name="update-registrations"></a>Aktualizovat registrace
Registrace zdroje dat je zjistitelný v katalogu Data Catalog při použití hello metadata a volitelné preview extrahovat během registrace. Pokud zdroj dat hello musí toobe aktualizovat v katalogu hello (například pokud došlo ke změně schématu hello objektu, původně vyloučené tabulky by měly být zahrnuty, nebo chcete tooupdate hello data, která je součástí verze Preview hello), nástroj registrace zdroje dat hello opětovně lze spustit.

Opakováním registrace zdroj dat již zaregistrován provede operaci sloučení "upsert": stávající objekty jsou aktualizovány a vytvoření nových objektů. Veškerá metadata, která poskytuje uživatelům prostřednictvím portálu katalogu Data Catalog hello zůstanou zachovány.

## <a name="summary"></a>Souhrn
Protože kopíruje strukturální a popisný metadata z katalogu služby toohello zdroje dat, registrace zdroje dat hello v katalogu Data Catalog umožňuje snazší toodiscover hello dat a pochopit. Po registraci zdroje dat hello opatřit poznámkami a spravovat zjistit pomocí portálu hello katalogu Data Catalog.

## <a name="next-steps"></a>Další kroky
Další informace o registraci zdrojů dat naleznete v tématu hello [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurzu.
