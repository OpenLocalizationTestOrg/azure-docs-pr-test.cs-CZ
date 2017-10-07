---
title: "aaaCreate zobrazení dat tooanalyze v OMS Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů vám umožní toocreate vlastní zobrazení, která se zobrazí na portálu OMS a Azure hello a obsahovat různé vizualizace dat v úložišti OMS hello. Tento článek obsahuje přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40b4bfef50d70e4479b6cae16abfa8ec33d1a2f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-view-designer-toocreate-custom-views-in-log-analytics"></a>Použití vlastních zobrazení toocreate Návrhář zobrazení v analýzy protokolů
Hello Návrhář zobrazení v [analýzy protokolů](log-analytics-overview.md) vám umožní toocreate vlastních zobrazení v konzole OMS hello obsahující různé vizualizace dat v úložišti OMS hello. Tento článek obsahuje přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Odkaz na dlaždici](log-analytics-view-designer-tiles.md) -odkaz hello nastavení pro každou hello dlaždice dostupné toouse do vlastních zobrazení.
* [Odkaz na část vizualizace](log-analytics-view-designer-parts.md) -odkaz hello nastavení pro každou hello dlaždice dostupné toouse do vlastních zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná v hello [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován hello prostoru bude automtically převést.

## <a name="concepts"></a>Koncepty
Zobrazení vytvořená pomocí hello Návrhář zobrazení obsahovat elementy s hello v hello následující tabulka.

| Část | Popis |
|:--- |:--- |
| Dlaždice |Zobrazí na hlavním řídicím přehled protokolu Analytics hello.  Zahrnuje visual shrnutí hello informace obsažené v hello vlastní zobrazení.  Různé typy dlaždici poskytovat různé vizualizace záznamů v úložišti OMS hello.  Klikněte na hello dlaždice tooopen hello vlastní zobrazení. |
| Vlastní zobrazení |Zobrazí, když hello uživatel klikne na hello dlaždice.  Obsahuje jednu nebo více částí vizualizace. |
| Vizualizace částí |Vizualizace dat v úložišti OMS hello založené na jeden nebo více [protokolu hledání](log-analytics-log-searches.md).  Většina částí bude obsahovat hlavičku, která poskytuje na nejvyšší úrovni vizualizaci a seznam výsledků nejvyšší hello.  Typy jinou částí poskytují různé vizualizace záznamů v úložišti OMS hello.  Klikněte na elementů v rámci tooperform hello vyhledávání protokolu poskytuje podrobné záznamy. |

![Zobrazit přehled návrháře](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-tooyour-workspace"></a>Přidání prostoru tooyour Návrhář zobrazení
Při zobrazení návrhu je ve verzi preview, musíte jej přidat tooyour prostoru výběrem **funkce verze Preview** v hello **nastavení** části portálu OMS hello.

![Povolit náhled](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Vytvoření a úprava zobrazení
### <a name="create-a-new-view"></a>Vytvoření nového zobrazení
Otevřít nové zobrazení v hello **Návrhář zobrazení** kliknutím na hello Návrhář zobrazení dlaždici v hlavním řídicím OMS hello.

![Dlaždice Návrhář zobrazení](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Upravit stávající zobrazení
tooedit existujícímu zobrazení v hello zobrazení návrhu zobrazení hello otevřete kliknutím na jeho dlaždici v hlavním řídicím OMS hello.  Pak klikněte na tlačítko hello **upravit** tlačítko zobrazení hello tooopen v hello Návrhář zobrazení.

![Upravit zobrazení](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonování existující zobrazení
Pokud jste klonovat zobrazení, vytvoří nové zobrazení a otevře ji v hello Návrhář zobrazení.  nové zobrazení Hello bude mít stejný název jako původní s koncem připojením toohello "Kopie" jej hello hello.  tooclone zobrazení, otevřete hello existující zobrazení kliknutím na jeho dlaždici v hlavním řídicím OMS hello.  Pak klikněte na tlačítko hello **klon** tlačítko zobrazení hello tooopen v hello Návrhář zobrazení.

![Clone – zobrazení](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Odstraňte existující zobrazení
toodelete stávající zobrazení, zobrazení hello otevřete kliknutím na jeho dlaždici v hlavním řídicím OMS hello.  Pak klikněte na tlačítko hello **upravit** tlačítko zobrazení hello tooopen v hello Návrhář zobrazení a klikněte na tlačítko **odstranit zobrazení**.

![Odstranit zobrazení](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Export existující zobrazení
Můžete exportovat soubor JSON tooa zobrazení, který lze importovat do jiného prostoru nebo lze použít v [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).  tooexport stávající zobrazení, zobrazení hello otevřete kliknutím na jeho dlaždici v hlavním řídicím OMS hello.  Pak klikněte na tlačítko hello **exportovat** tlačítko toocreate soubor ve složce pro stahování hello prohlížeče.  Hello název souboru hello bude hello název zobrazení hello s příponou hello *omsview*.

![Export zobrazení](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importovat stávající zobrazení
Můžete importovat *omsview* soubor, který jste exportovali z jiné skupiny pro správu.  tooimport stávající zobrazení, nejprve vytvořte nové zobrazení.  Pak klikněte na tlačítko hello **Import** tlačítkem a vyberte hello *omsview* souboru.  Konfigurace Hello v souboru hello se zkopírují do stávajícího zobrazení hello.

![Export zobrazení](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Práce s Návrhář zobrazení
Hello Návrhář zobrazení má tři podokna.  Hello **návrhu** podokně představuje hello vlastní zobrazení.  Když přidáte dlaždice a částí z hello **řízení** podokně toohello **návrhu** podokně jsou přidány toohello zobrazení.  Hello **vlastnosti** podokně se zobrazí vlastnosti hello hello dlaždice nebo vybraných součástí.

![Návrhář zobrazení](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurace zobrazení dlaždice
Vlastní zobrazení může mít pouze jedna dlaždice.  Vyberte hello **dlaždici** ve hello **řízení** aktuální hello tooview podokně dlaždice, nebo vyberte alternativní šablonu.  Hello **vlastnosti** podokně se zobrazí vlastnosti hello pro aktuální dlaždici hello.  Konfigurovat vlastnosti dlaždice hello podle toohello podrobné informace v hello [odkaz na dlaždici](log-analytics-view-designer-tiles.md) a klikněte na tlačítko **použít** toosave změny.

### <a name="configure-visualization-parts"></a>Konfigurace vizualizace částí
Zobrazení může obsahovat libovolný počet částí vizualizace.  Vyberte hello **zobrazení** kartu a pak na vizualizaci součástí tooadd toohello zobrazení.  Hello **vlastnosti** podokně se zobrazí vlastnosti hello hello vybraná část.  Konfigurace zobrazení hello vlastnosti podle toohello podrobné informace v hello [vizualizace část odkaz](log-analytics-view-designer-parts.md) a klikněte na tlačítko **použít** toosave změny.

### <a name="delete-a-visualization-part"></a>Odstraňte část vizualizace
Součástí vizualizace můžete odebrat ze zobrazení hello klepnutím hello **X** tlačítko v horním pravém rohu hello části hello.

### <a name="rearrange-visualization-parts"></a>Změna uspořádání částí vizualizace
Zobrazení mít pouze jeden řádek částí vizualizace.  Změna uspořádání existující částí v zobrazení kliknutím a přetažením je tooa nové umístění.

## <a name="next-steps"></a>Další kroky
* Přidat [dlaždice](log-analytics-view-designer-tiles.md) tooyour vlastní zobrazení.
* Přidat [vizualizace částí](log-analytics-view-designer-parts.md) tooyour vlastní zobrazení.
