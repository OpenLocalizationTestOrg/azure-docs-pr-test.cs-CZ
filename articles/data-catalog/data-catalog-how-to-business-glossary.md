---
title: "aaaSet až hello obchodní Glosář pro upraveny označování v Azure Data Catalog | Microsoft Docs"
description: "Jak tooarticle zvýraznění hello obchodní Glosář v Azure Data Catalog pro definování a používání běžné tootag slovník business registrovaných datových prostředků."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: c9adf663bd08ac3c0c7b5d3551e6af409fe69ebc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-business-glossary-for-governed-tagging"></a>Nastavit obchodní Glosář hello pro řídí označování
## <a name="introduction"></a>Úvod
Azure Data Catalog umožňuje zdroj dat zjišťování, takže lze snadno vyhledat a pochopit zdroje dat hello potřebovat tooperform analýzy a rozhodnutí. Tyto možnosti zajistěte největších dopad hello při můžete najít a pochopit hello nejširší řadu dostupných zdrojů dat.

Je označování jeden katalogu Data Catalog funkce, která podporuje větší přehled o datové prostředky. Pomocí označování, můžete přidružit klíčová slova prostředek nebo sloupec, který zase umožňuje snazší asset hello toodiscover prostřednictvím vyhledávání a procházení. Označování také pomáhá další snadno pochopit hello kontextu a záměr hello asset.

Ale označování může v některých případech způsobit problémy své vlastní. Mezi příklady problémy, které můžou vést k označování patří:

* Hello použití zkratky na některých prostředků a rozšířená text na ostatní. Tato nekonzistence omezuje hello zjišťování prostředků, i když hello záměr byl tootag hello prostředky s hello stejnou značku.
* Potenciální rozdíly v význam, a to v závislosti na kontextu. Například značku názvem *výnosy* na Zákazník může znamenat datové sady, obrat podle zákazníka, ale hello stejnou značku na čtvrtletně prodeje datová sada může znamenat čtvrtletně výnosy hello společnosti.  

toohelp řeší tyto a další podobné problémy, katalogu Data Catalog zahrnuje obchodní Glosář.

Pomocí hello katalogu Data Catalog obchodní glosář, můžete v organizaci dokumentů klíče firemní podmínky a jejich definice toocreate běžné obchodní termínů. Tato zásady správného řízení umožňuje konzistenci využití dat mezi hello organizace. Po termín, který je definován v hello obchodní glosář, může být přiřazena tooa datový prostředek v katalogu hello. Tento přístup *řídí označování*, je stejný přístup jako označování hello.

## <a name="glossary-availability-and-privileges"></a>Glosář dostupnosti a oprávnění
obchodní Glosář Hello je k dispozici pouze v hello Azure Data Catalog, Standard Edition. Hello bezplatná edice katalogu Data Catalog nezahrnuje Glosář a nenabízí možnosti pro označování upraveny.

Dostanete hello obchodní Glosář prostřednictvím hello **Glosář** možnost v navigační nabídce portál hello katalogu Data Catalog.  

![Přístup k hello obchodní Glosář](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Data katalogu a členové hello glosář, role Správci mohou vytvářet, upravovat a odstraňovat slovníku pojmů v obchodní Glosář hello. Všichni uživatelé katalogu Data Catalog můžete zobrazit definice období hello a značky prostředky pomocí slovníku pojmů.

![Přidání nové období Glosář](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Vytváření slovníku pojmů
Data katalogu správců a správců Glosář kliknutím hello můžete vytvořit slovníku pojmů **nové období** tlačítko. Každému termínu glosář obsahuje hello následující pole:

* Obchodní definice pro termín hello
* Popis, který zachycuje hello určené použití nebo obchodní pravidla pro hello asset nebo sloupce
* Seznam účastníkům, kteří znají hello nejvíce o hello termín
* Hello nadřazený termín, který definuje hello hierarchie, ve které hello je organizovaná termín

## <a name="glossary-term-hierarchies"></a>Glosář termínů hierarchie
Pomocí hello katalogu Data Catalog obchodní glosář, organizace může popsat jeho slovník business jako hierarchie podmínek a může vytvořit klasifikace podmínky, která lépe představuje jeho obchodní taxonomii.

Termín, který musí být jedinečný na dané úrovni hierarchie. Duplicitní názvy nejsou povoleny. Neexistuje žádné omezení toohello počet úrovní v hierarchii, ale hierarchie je často srozumitelnější když existují tři úrovně nebo méně.

použití Hello hierarchií v hello obchodní Glosář je volitelné. Výstupu hello nadřazené termín pole prázdné, pokud slovníku pojmů vytvoří dvojrozměrném (jiných než hierarchických) seznam termínů v glosáři hello.  

## <a name="tagging-assets-with-glossary-terms"></a>Označování prostředky pomocí slovníku pojmů
Po definování slovníku pojmů v rámci katalogu hello hello činnost označování prostředků se optimalizované toosearch hello Glosář jako uživatel zadá značku. Hello katalogu Data Catalog portálu se zobrazí seznam odpovídající toochoose podmínky Glosář z. Pokud hello uživatel vybere ze seznamu hello Glosář termín, termín hello přidán toohello asset jako tag (také nazývané značku glosář). Hello uživatele můžete také zvolit toocreate novou značku zadáním termín, který se nenachází v hello Glosář (také nazývané značku uživatele).

![Datový prostředek označené značku jednoho uživatele a dvě značky Glosář](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Značky uživatele jsou hello pouze typ značky, které jsou podporovány v hello bezplatná edice katalogu Data Catalog.
>
>

### <a name="hover-behavior-on-tags"></a>Chování při přechodu na značky
Data Catalog portálu hello jsou dva typy hello značek vizuálně odlišné a existuje jiný hover chování. Po přesunutí ukazatele myši značku uživatele, zobrazí se text hello značky a hello uživatele nebo uživatelů, kteří přidání značka hello. Po přesunutí ukazatele myši značku glosář, uvidíte také hello Definice hello Glosář termínů a odkaz tooopen hello obchodní Glosář tooview hello úplné definice podmínek hello.

### <a name="search-filters-for-tags"></a>Filtry hledání pro značky
Glosář značky a značky uživatele jsou obě vyhledávat a můžete je použít jako filtry v hledání.

## <a name="summary"></a>Souhrn
Pomocí hello obchodní Glosář v Azure Data Catalog a hello řídí označování příznaky umožňuje můžete identifikovat, spravovat a zjistili datové prostředky konzistentním způsobem. Hello obchodní Glosář můžete zvýšit úroveň learning hello firmy termínů členové organizace. Glosář Hello také podporuje zaznamenávání smysluplný metadata, která zjednodušuje asset zjišťování a pochopení.

## <a name="next-steps"></a>Další kroky
* [Dokumentace k REST API pro operace obchodní Glosář](https://msdn.microsoft.com/library/mt708855.aspx)
