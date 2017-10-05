---
title: "Skupiny počítačů v analýzy protokolů protokolu hledání | Microsoft Docs"
description: "Skupiny počítačů v analýzy protokolů umožňují oboru vyhledávání protokolu na konkrétní sadu počítačů.  Tento článek popisuje různé metody, které můžete použít k vytvoření skupiny počítačů a jejich použití v hledání protokolů."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: a2ddc932343d54963a378ee27dc962a790326b2a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Skupiny počítačů v analýzy protokolů protokolu hledání

>[!NOTE]
> Tento článek popisuje použití skupiny počítačů pomocí aktuální dotaz jazyka Anayltics protokolu.    Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak skupiny počítačů fungují jinak.  Poznámky jsou uvedené v tomto článku s jinou syntaxi a chování pro nové dotazovací jazyk.  


Skupiny počítačů v analýzy protokolů umožňují oboru [protokolu hledání](log-analytics-log-searches.md) na konkrétní sadu počítačů.  Každé skupiny se naplní se počítači buď pomocí dotaz, který definujete nebo importem skupiny z různých zdrojů.  Pokud skupina je obsažena v protokolu vyhledávání, výsledky jsou omezeny na záznamy, které odpovídají počítače ve skupině.

## <a name="creating-a-computer-group"></a>Vytvoření skupiny počítačů
Můžete vytvořit skupinu počítačů v analýzy protokolů pomocí libovolné metody v následující tabulce.  Podrobnosti o jednotlivých metod jsou uvedeny v následujících částech. 

| Metoda | Popis |
|:--- |:--- |
| Prohledávání protokolů |Vyhledávání protokolu, který vrátí seznam počítačů, vytvořte a uložte výsledky jako skupinu počítačů. |
| Rozhraní API pro prohledávání protokolů |Pomocí rozhraní API pro vyhledávání protokolu prostřednictvím kódu programu vytvořit skupinu počítačů, na základě výsledků hledání protokolů. |
| Active Directory |Automaticky kontrolu členství ve skupině všechny počítače agenta, které jsou členy domény služby Active Directory a vytvořte skupinu ve analýzy protokolů pro každou skupinu zabezpečení. |
| SLUŽBY WSUS |Automatické prohledávání WSUS servery nebo klienty pro cílové skupiny a vytvořte skupinu ve analýzy protokolů pro každý. |

### <a name="log-search"></a>Prohledávání protokolů
Skupiny počítačů, které jsou vytvořené z hledání protokolů obsahovat všechny počítače, vrácený vyhledávací dotaz, který určíte.  Tento dotaz běží pokaždé, když se používá skupina počítačů, tak, aby se projeví všechny změny, protože byla skupina vytvořena.

Vytvořit skupinu počítačů z hledání protokolů použijte následující postup.

1. [Vytvoření vyhledávání protokolu](log-analytics-log-searches.md) který vrátí seznam počítačů.  Hledání musí vracet odlišnou sadu počítačů pomocí něčeho jako jazyk **Distinct Computer** nebo **měření count() počítačem** v dotazu.  
2. Klikněte **Uložit** tlačítka v horní části obrazovky.
3. Vyberte **Ano** k **uložit tento dotaz jako skupinu počítačů**.
4. Zadejte **název** a **kategorie** pro skupinu.  Pokud hledání se stejným názvem a kategorie již existuje, pak se výzva k jeho přepsání.  Může mít několik hledání se stejným názvem v různých kategorií. 

Následují příklady hledání, které můžete uložit jako skupinu počítačů.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md) pak jsou provedeny následující změny postup k vytvoření nové skupiny počítačů.
>  
> - Dotaz na vytvořit skupinu počítačů musí obsahovat `distinct Computer`.  Tady je příklad dotazu na vytvořit skupinu počítačů.<br>`Heartbeat | where Computer contains "srv" `
> - Když vytvoříte novou skupinu počítačů, musíte zadat alias kromě názvu.  Můžete použít alias při použití skupiny počítačů v dotazu, jak je popsáno níže.  

### <a name="log-search-api"></a>Hledání protokolů rozhraní API
Skupiny počítačů, které jsou vytvořené pomocí rozhraní API pro vyhledávání protokolu jsou stejné jako rozšířeného hledání protokolů hledání.

Podrobné informace o vytváření skupiny počítačů pomocí rozhraní API pro vyhledávání protokolu najdete v tématu [skupiny počítačů v protokolu analýzy protokolů hledání REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Když konfigurujete analýzy protokolů pro import členství ve skupině služby Active Directory, analyzuje členství ve skupině všechny počítače připojené k doméně s agentem OMS.  Skupina počítačů se vytvoří v analýzy protokolů pro každou skupinu zabezpečení ve službě Active Directory a každý počítač se přidá do odpovídající skupiny zabezpečení, které jsou členy skupin počítačů.  Členství v této se průběžně aktualizuje každé 4 hodiny.  

Konfigurace analýzy protokolů k importu skupin zabezpečení služby Active Directory z **skupiny počítačů** nabídky v analýzy protokolů **nastavení**.  Vyberte **automatizace** a potom **členství ve skupině služby Active Directory Import z počítačů**.  Není nutná žádná další konfigurace.

![Skupiny počítačů ze služby Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Importu skupin v nabídce uvádí počet počítačů s zjistil členství ve skupině a počet skupin importovat.  Kliknutím na některou z těchto odkazů se vrátíte **ComputerGroup** záznamy s těmito informacemi.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Když konfigurujete analýzy protokolů pro import členství ve skupině WSUS, analyzuje zaměřená na členství ve skupině všechny počítače s agentem OMS.  Pokud používáte klienta cílení na skupiny cílení na jakýkoli počítač, který je připojený k OMS a je součástí všech WSUS má jeho členství ve skupinách importovat k analýze protokolů. Pokud používáte serverové cílení OMS agenta by měly být nainstalovány na serveru WSUS, aby se informace o členství ve skupině k importu OMS.  Členství v této se průběžně aktualizuje každé 4 hodiny. 

Konfigurace analýzy protokolů k importu skupin zabezpečení služby Active Directory z **skupiny počítačů** nabídky v analýzy protokolů **nastavení**.  Vyberte **služby Active Directory** a potom **členství ve skupině služby Active Directory Import z počítačů**.  Není nutná žádná další konfigurace.

![Skupiny počítačů ze služby Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Importu skupin v nabídce uvádí počet počítačů s zjistil členství ve skupině a počet skupin importovat.  Kliknutím na některou z těchto odkazů se vrátíte **ComputerGroup** záznamy s těmito informacemi.

## <a name="managing-computer-groups"></a>Správa skupin počítačů
Můžete zobrazit skupiny počítačů, které byly vytvořeny z vyhledávání protokolu nebo rozhraní API pro vyhledávání protokolu z **skupiny počítačů** nabídky v analýzy protokolů **nastavení**.  Klikněte **x** v **odebrat** sloupec odstranit skupinu počítačů.  Klikněte **zobrazit členy** ikona pro skupinu pro spuštění vyhledávání protokolu skupiny, který vrátí její členy. 

![Skupiny uložené počítačů](media/log-analytics-computer-groups/configure-saved.png)

Chcete-li upravit skupiny, vytvořte novou skupinu se stejným **kategorie** a **název** přepsat původní skupinu.

## <a name="using-a-computer-group-in-a-log-search"></a>Použití skupiny počítačů do protokolu hledání
Použijte následující syntaxi odkazovat na skupinu počítačů do protokolů hledání.  Určení **kategorie** je volitelné a vyžaduje, pokud máte skupiny počítačů se stejným názvem v různých kategorií. 

    $ComputerGroups[Category: Name]

Při spuštění vyhledávání, jsou členy všech skupin počítačů, které jsou součástí hledání nejprve vyřešit.  Pokud skupině je založená na protokolu vyhledávání, toto hledání běží před provedením hledání nejvyšší úrovně protokolů vrátit členy skupiny.

Skupiny počítačů se obvykle používají se **IN** klauzuli v hledání protokolů jako v následujícím příkladu:

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), potom použijte skupinu počítačů v dotazu tak, že považuje jeho alias jako funkce jako v následujícím příkladu:
> 
>  `UpdateSummary | where Computer IN (MyComputerGroup)`

## <a name="computer-group-records"></a>Záznamů skupiny počítače
V úložišti OMS pro každý členství ve skupině počítačů vytvořené pomocí služby Active Directory nebo služby WSUS se vytvoří záznam.  Tyto záznamy mají typ **ComputerGroup** a mít vlastnosti v následující tabulce.  Záznamy nejsou vytvořeny pro skupiny počítačů, které jsou založené na protokolu hledání.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Počítač |Název členského počítače. |
| Skupina |Název skupiny. |
| GroupFullName |Úplná cesta ke skupině, včetně zdroje a název zdroje. |
| GroupSource |Zdroj této skupině se shromážděných z. <br><br>Active Directory<br>SLUŽBY WSUS<br>WSUSClientTargeting |
| GroupSourceName |Název zdroje, který skupině nebyla shromážděna z.  U služby Active Directory je to název domény. |
| ManagementGroupName |Název skupiny pro správu agentů SCOM.  Pro jiné agenty jde AOI -\<ID pracovního prostoru\> |
| TimeGenerated |Datum a čas vytvoření nebo aktualizovat skupiny počítačů. |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.  

