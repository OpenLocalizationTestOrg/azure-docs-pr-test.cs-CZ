---
title: "aaaHow toowork se zdroji dat 'velkých objemů dat. | Microsoft Docs"
description: "Jak tooarticle zvýraznění vzory pro pomocí Azure Data Catalog, velké objemy dat, datových zdrojů, včetně Azure Blob Storage, Azure Data Lake a Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e478f71f26744975a7d7e1784b74bf50b424cf65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowork-with-big-data-sources-in-azure-data-catalog"></a>Jak zdroje toowork s velkým objemem dat v Azure Data Catalog
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Všechno se točí kolem osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím tooget větší hodnotu ze svých existujících zdrojů dat, včetně velkých objemů dat.

**Azure Data Catalog** podporuje hello registrace Azure Blog úložiště objektů BLOB a adresáře a také Hadoop HDFS souborů a adresářů. Hello částečně strukturovaných povaha tyto zdroje dat poskytuje flexibilitu. Ale tooget hello maximální hodnotu registraci pomocí **Azure Data Catalog**, uživatelé musíte zvážit, jak jsou uspořádány zdroje dat hello.

## <a name="directories-as-logical-data-sets"></a>Adresářů jako logické datové sady
Běžný vzor uspořádání zdroje velkých objemů dat je tootreat adresářů jako logické datové sady. Nejvyšší úrovně adresáře jsou použité toodefine datové sady, zatímco podsložky Definujte oddíly a hello soubory, které obsahují ukládání hello samotná data.

Příkladem tohoto vzoru může být:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

V tomto příkladu vehicle_maintenance_events a location_tracking_events představují logické datové sady. Každou z těchto složek obsahuje datové soubory, které jsou uspořádané podle rok a měsíc do podsložky. Každou z těchto složek mohou obsahovat stovky nebo tisíce souborů.

V tomto vzoru registraci jednotlivých souborů s **Azure Data Catalog** pravděpodobně nemá smysl. Místo toho zaregistrujte hello adresáře, které představují hello datových sad, které se smysluplný toohello uživatelů, kteří pracují s daty hello.

## <a name="reference-data-files"></a>Referenční datové soubory
Doplňkové vzor je toostore referenční datové sady jako jednotlivé soubory. Tyto sady dat může považovat za hello "malá" straně velkých objemů dat a jsou často podobné toodimensions ve model analytickými daty. Referenčních dat souborů obsahuje záznamy, které jsou používané tooprovide kontext pro hromadné hello hello souborů data uložená na jiném místě v úložišti velkých objemů dat hello.

Příkladem tohoto vzoru může být:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Při vědecký pracovník analytik nebo data pracuje s daty hello součástí větší struktury adresářů hello, hello data v těchto souborech odkaz může být použité tooprovide podrobnější informace pro entity, které jsou označuje tooonly podle názvu nebo ID v datech větší hello Sada.

V tomto vzoru, má smysl tooregister hello individuální referenční datové soubory s **Azure Data Catalog**. Každý soubor představuje datové sady a každé z nich můžou opatřeny poznámkami a zjištěné jednotlivě.

## <a name="alternate-patterns"></a>Alternativní vzory
Hello vzory popsané v předcházející části hello jsou právě dva možné úložiště velkých objemů dat může být uspořádaný, ale každý implementace se liší. Bez ohledu na to, jak jsou strukturovaná zdroje dat, při registraci zdroje velkých objemů dat s **Azure Data Catalog**, soustředit na registrace hello soubory a adresáře, které představují hello datových sad, které jsou tooothers hodnotu v rámci vaší organizace. Registrace všechny soubory a adresáře může zaplnit katalogu hello, takže je pro uživatele toofind těžší požadované.

## <a name="summary"></a>Souhrn
Registrace zdroje dat s **Azure Data Catalog** umožňuje jejich jednodušší toodiscover a pochopit. Registrace a zadávání poznámek k hello velkých objemů dat souborů a adresářů, které představují logické datové sady, může pomoci uživatelům najít a používat zdroje hello velkých objemů dat, které potřebují.
