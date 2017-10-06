---
title: zdroje dat tooannotate aaaHow | Microsoft Docs
description: "Jak tooarticle zvýraznění jak tooannotate datových prostředcích v Azure Data Catalog, včetně popisné názvy, značky, popisy a odborníky."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 1d1ef34e3f1ef73cdc65129209d938abe1e36c01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooannotate-data-sources"></a>Jak tooannotate zdroje dat
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Jinými slovy Data Catalog se točí kolem osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím tooget větší hodnotu ze své stávající data. Při registraci zdroje dat pomocí katalogu Data Catalog je zkopírovat a indexované službou hello jeho metadata, ale hello scénáře není ukončení existuje. Data Catalog umožňuje uživatelům tooprovide vlastní popisných metadat – například značky a popisy – toosupplement hello metadat extrahovaných ze zdroje dat hello a zdroje dat hello toomake nerozumí toomore lidem.

## <a name="annotation-and-crowdsourcing"></a>Poznámky a crowdsourcingu
Každý má k vyjádření. A toto je dobré.
Data Catalog rozpozná, mají různé uživatele různých perspektiv v datových zdrojů organizace a že každý z těchto perspektiv mohou být cenné. Vezměte v úvahu hello následující scénář:

* Správce systému Hello zná hello smlouvu o úrovni služeb pro hello servery nebo služby tohoto zdroje dat. hello hostitele.
* Správce databáze Hello zná hello zálohování plán pro každou databázi, a povolených zpracování ETL windows hello.
* vlastníka systému Hello zná hello proces pro zdroj dat toohello přístup toorequest uživatele.
* Hello data steward neví, jak hello prostředky a atributy ve zdroji dat hello mapování toohello enterprise datového modelu.
* Hello analytika vědět, jak se používají hello data v kontextu hello hello obchodní procesy, které mu podporuje.

Každý z těchto perspektiv se hodí v situaci, a Data Catalog používá toometadata crowdsourcingový přístup, který umožňuje každý jeden toobe zachytit a použít tooprovide úplný přehled o registrované datové zdroje. Pomocí portálu hello katalogu Data Catalog, můžete každý uživatel přidat nebo upravit své vlastní poznámky, aniž by byly možné tooview poznámky od jiných uživatelů.

## <a name="different-types-of-annotations"></a>Různé typy poznámky
Data katalogu hello podporuje následující typy poznámky:

| Poznámky | Poznámky |
| --- | --- |
| Popisný název |Popisné názvy můžete zadat na úrovni asset hello dat, srozumitelnější toomake hello datových prostředků. Popisné názvy jsou velmi užitečné při hello základní název objektu je toousers jako nesrozumitelné, zkrácené nebo v opačném případě nemá význam. |
| Popis |Popisy můžete zadat v hello datový prostředek a atribut nebo úrovně sloupce. Popisy jsou krátké textové poznámky, které popisují hello uživatele perspektivou na datový prostředek hello nebo jeho použití. |
| Značky (značky uživatele) |Značky lze zadat v hello datový prostředek a atribut nebo úrovně sloupce. Značky uživatele jsou uživatelem definované popisky, které se dají použít toocategorize datových prostředků nebo atributy. |
| Značky (Glosář značky) |Značky lze zadat v hello datový prostředek a atribut nebo úrovně sloupce. Glosář značky jsou definované centrálně slovníku pojmů, které se dají použít toocategorize datových prostředků nebo atributů s použitím běžné obchodní taxonomii. Další informace najdete v části [jak tooset až hello obchodní Glosář řízeným přidáváním značek](data-catalog-how-to-business-glossary.md) |
| Odborníci |Odborníci můžete zadat na úrovni asset hello data. Specialisté identifikovat uživatele nebo skupiny s odbornými perspektivami na hello dat a může sloužit jako body kontaktu pro uživatele, kteří je zjistili hello zaregistrovaných zdrojů dat a máte otázky, které nenajdete odpovědi v existujících poznámek hello. |
| Požádat o přístup |Žádost o přístup k informacím můžete zadat na úrovni asset hello data. Tyto informace jsou pro uživatele, kteří zjistit zdroje dat, ještě nemají oprávnění tooaccess. Uživatele můžete zadat e-mailovou adresu hello hello uživatele nebo skupiny, která uděluje přístup, hello adresa URL procesu hello nástroj tento přístup uživatelé třeba toogain nebo můžete zadat samotný proces hello jako text. |
| Dokumentace |Dokumentace můžete zadat na úrovni asset hello data. Asset dokumentace je formátovaným textem informace, které může obsahovat odkazy a bitové kopie a která může poskytnout všechny informace není vyjádřené prostřednictvím popisy a značky. |

## <a name="annotating-multiple-assets"></a>Zadávání poznámek k více prostředků
Když vyberete více datových prostředků portálu hello katalogu Data Catalog, uživatelé může opatřit poznámkami všechny vybrané prostředky v rámci jedné operace. Poznámky použije tooall vybrané prostředky, takže je easy tooselect a zajistit konzistentní popis a sady značek a odborníky u prostředků související data.

> [!NOTE]
> Značky a odborníky se dá zadat i při registraci datové prostředky pomocí katalogu Data Catalog dat hello zdroje nástroj pro registraci.
>
>

Když vyberete více tabulek nebo zobrazení, jenom na sloupce, že všechny vybrané data, která mají společné prostředky se zobrazí portálu hello katalogu Data Catalog. To umožňuje uživatelům tooprovide značky a popisy pro všechny sloupce s hello stejný název pro všechny vybrané prostředky.

## <a name="annotations-and-discovery"></a>Poznámky a zjišťování
Stejně jako hello je přidali metadata extrahovaná ze zdroje dat hello během registrace indexu vyhledávání toohello katalogu Data Catalog, uživatelem zadané metadata jsou také indexována. To znamená, že nejen udělat poznámky usnadňují uživatelům toounderstand hello data, která se zjistit, poznámky také usnadňují uživatelům toodiscover hello opatřeny poznámkami datové prostředky vyhledáváním pomocí hello podmínky, které toothem smysl.

## <a name="summary"></a>Souhrn
Registrace zdroje dat pomocí katalogu Data Catalog umožňuje dat zjistitelný zkopírováním strukturální a popisný metadat ze zdroje dat hello do hello katalogu služby. Po registraci zdroje dat, uživatelé poskytnout jednodušší toodiscover toomake poznámky a pochopit z portálu hello katalogu Data Catalog.

## <a name="see-also"></a>Viz také
* [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurz podrobné informace o tooannotate datové zdroje.
