---
title: aaaHow toodiscover zdroje dat v Azure Data Catalog | Microsoft Docs
description: "Tento článek popisuje, jak toodiscover registrovaných datových prostředků s Azure Data Catalog, včetně vyhledávání a filtrování a pomocí hello dosáhl zvýraznění možnosti portálu Azure Data Catalog hello."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 624834b8895dd50c8931c9d3e6f8dc217927c617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodiscover-data-sources-in-azure-data-catalog"></a>Jak zdroje dat toodiscover v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog je plně spravovaná Cloudová služba, která slouží jako systém registrace a zjišťování datových zdrojů organizace. Jinými slovy Data Catalog umožňuje uživatelům zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu ze své stávající data. Po registraci zdroje dat pomocí katalogu Data Catalog jeho metadata je indexovaný službou hello, takže můžete snadno vyhledat toodiscover hello dat, které potřebujete.

## <a name="searching-and-filtering"></a>Vyhledávání a filtrování
Funkce zjišťování v katalogu Data Catalog používá dva primární mechanismy: vyhledávání a filtrování.

Hledání je navrženou toobe intuitivní a efektivní. Ve výchozím nastavení jsou podmínky vyhledávání porovnání všechny vlastnosti v katalogu hello, včetně poznámek zadaný uživatelem.

Filtrování je určená toocomplement vyhledávání. Můžete vybrat konkrétní charakteristiky jako odborníky, typ zdroje dat, typ objektu a značky. Můžete zobrazit jenom odpovídající datových prostředků a omezit prostředky toomatching výsledky hledání.

Pomocí kombinace vyhledávání a filtrování, můžete rychle procházet hello zdroje dat, které byly zaregistrovány pomocí katalogu Data Catalog toodiscover hello datového zdroje, které potřebujete.

## <a name="search-syntax"></a>Syntaxe služby Search
Přestože hello výchozí textové vyhledávání je jednoduché a intuitivní, můžete také použít syntaxe vyhledávání katalogu Data Catalog pro větší kontrolu nad výsledky hledání hello. Hello podporuje vyhledávání katalogu dat následující techniky:

| Technika | Použití | Příklad |
| --- | --- | --- |
| Základní hledání |Základní vyhledávání, který používá jeden nebo více výrazů vyhledávání. Výsledky jsou všechny prostředky odpovídající libovolné vlastnosti s jednou nebo více podmínek hello zadán. |`sales data` |
| Zkoumání vlastnosti |Návratový pouze zdroje dat kde hello hledaný termín shoduje s hello určená vlastnost. |`name:finance` |
| Logické operátory |Rozšíří nebo zúží vyhledávání pomocí logických operací. |`finance NOT corporate` |
| Seskupování pomocí závorek |Pomocí závorek toogroup částí hello dotazu tooachieve logické izolace, zejména ve spojení s logickými operátory. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Operátory porovnání |Porovnávání jiné než rovnost použijte pro vlastnosti, které mají číselné a číselné datové typy. |`modifiedTime > "11/05/2014"` |

Další informace o hledání v katalogu dat najdete v tématu hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) článku.

## <a name="hit-highlighting"></a>Zvýrazňování položek
Při zobrazení výsledků vyhledávání žádný zobrazí vlastnosti, které odpovídají zadané hello hledaných termínů (například hello data asset název, popis a značky) jsou zvýrazněné toomake ho jednodušší tooidentify, proč daný datový prostředek vrátil zadaný hledaný.

> [!NOTE]
> tooturn vypnout podle zvýraznění, použijte hello **zvýrazněte** přepínač portálu hello katalogu Data Catalog.
>
>

Při zobrazení výsledků vyhledávání, nemusí být vždy zřejmé proč datový prostředek je součástí, dokonce i s přístupů zvýraznění povolena. Protože vyhledávají se všechny vlastnosti ve výchozím nastavení, může být z důvodu shoda na úrovni sloupce vlastnost vrácena datový prostředek. A protože více uživatelů může opatřit poznámkami registrované datové prostředky s vlastní značky a popisy, ne všechny metadata může zobrazit v seznamu hello výsledků hledání.

V zobrazení tile výchozí hello, každou dlaždici zobrazí ve výsledcích hledání hello zahrnuje **zobrazení hledaný termín shoduje** ikonu, tak, aby hello počet shod a jejich umístění a toojump toothem lze rychle zobrazit, pokud chcete.

 ![Zvýrazňování položek a odpovídá vyhledávání v portálu Azure Data Catalog hello](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Souhrn
Protože registraci zdroje dat se katalogu Data Catalog kopiemi strukturální a popisný metadata z hello datového zdroje toohello katalogu služby, hello zdroj dat bude jednodušší toodiscover a pochopit. Poté, co jste registrováni zdroj dat, můžete zjistit pomocí filtrování a vyhledávání z portálu hello katalogu Data Catalog.

## <a name="next-steps"></a>Další kroky
* Podrobné informace o toodiscover datových zdrojů, najdete v části [Začínáme s Azure Data Catalog](data-catalog-get-started.md).
