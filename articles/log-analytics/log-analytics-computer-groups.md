---
title: "aaaComputer skupin v analýzy protokolů protokolu hledání | Microsoft Docs"
description: "Skupiny počítačů v analýzy protokolů umožňují tooscope protokolu hledání tooa konkrétní sadu počítačů.  Tento článek popisuje hello toocreate skupiny počítačů a jak se toouse je v protokolu vyhledejte můžete použít různé metody."
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
ms.openlocfilehash: 7dafea9829e541f5582a1d855fafb82aa4d94430
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Skupiny počítačů v analýzy protokolů protokolu hledání

>[!NOTE]
> Tento článek popisuje hello použití skupiny počítačů pomocí hello aktuální Anayltics protokolu dotazu jazyka.    Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak skupiny počítačů fungují jinak.  Poznámky jsou uvedené v tomto článku s jinou syntaxi hello a chování pro nové dotazovací jazyk hello.  


Skupiny počítačů v analýzy protokolů umožňují tooscope [protokolu hledání](log-analytics-log-searches.md) tooa konkrétní sadu počítačů.  Každé skupiny se naplní se počítači buď pomocí dotaz, který definujete nebo importem skupiny z různých zdrojů.  Když hello skupina je obsažena v hledání protokolů, hello výsledky jsou omezené toorecords, která odpovídají hello počítačů ve skupině hello.

## <a name="creating-a-computer-group"></a>Vytvoření skupiny počítačů
Můžete vytvořit skupinu počítačů v analýzy protokolů pomocí metod hello v hello následující tabulka.  Podrobnosti o jednotlivých metod jsou uvedeny v následujících částech hello. 

| Metoda | Popis |
|:--- |:--- |
| Prohledávání protokolů |Vyhledávání protokolu, který vrátí seznam počítačů, vytvořte a uložte výsledky hello jako skupinu počítačů. |
| Rozhraní API pro prohledávání protokolů |Použití hello rozhraní API pro vyhledávání protokolu tooprogrammatically vytvořit skupinu počítačů, na základě hello výsledků hledání protokolů. |
| Active Directory |Automaticky kontrolu členství ve skupině hello všech počítačů agenta, které jsou členy domény služby Active Directory a vytvořte skupinu ve analýzy protokolů pro každou skupinu zabezpečení. |
| SLUŽBY WSUS |Automatické prohledávání WSUS servery nebo klienty pro cílové skupiny a vytvořte skupinu ve analýzy protokolů pro každý. |

### <a name="log-search"></a>Prohledávání protokolů
Skupiny počítačů, které jsou vytvořené z hledání protokolů obsahovat všechny počítače hello vrácených dotazem vyhledávání, které definujete.  Tento dotaz je spustit při každém hello skupiny počítačů se používá, aby se projeví jakékoli změny provedené od hello skupina byla vytvořena.

Použijte následující postup toocreate skupinu počítačů z protokolu hledání hello.

1. [Vytvoření vyhledávání protokolu](log-analytics-log-searches.md) který vrátí seznam počítačů.  Hello hledání musí vracet odlišnou sadu počítačů pomocí něčeho jako jazyk **Distinct Computer** nebo **měření count() počítačem** v dotazu hello.  
2. Klikněte na tlačítko hello **Uložit** tlačítko hello horní části úvodní obrazovka.
3. Vyberte **Ano** příliš**uložit tento dotaz jako skupinu počítačů**.
4. Zadejte **název** a **kategorie** pro skupinu hello.  Pokud hello hledání se stejným názvem a kategorie již existuje, pak nebudete výzvami toooverwrite ho.  Můžete mít více hledání s hello stejný název v různých kategorií. 

Následují příklady hledání, které můžete uložit jako skupinu počítačů.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md) pak hello následující změny jsou učiněna toohello postup toocreate nová skupina počítačů.
>  
> - Hello toocreate dotazů musí obsahovat skupinu počítačů `distinct Computer`.  Tady je příklad dotazu toocreate skupinu počítačů.<br>`Heartbeat | where Computer contains "srv" `
> - Když vytvoříte novou skupinu počítačů, musíte zadat alias v přidání toohello názvu.  Můžete použít hello alias při použití skupiny počítačů hello v dotazu, jak je popsáno níže.  

### <a name="log-search-api"></a>Hledání protokolů rozhraní API
Skupiny počítačů, které jsou vytvořené pomocí rozhraní API pro vyhledávání protokolu jsou hello hello stejné jako rozšířeného hledání protokolů hledání.

Podrobnosti týkající se vytvoření skupiny počítačů pomocí hello rozhraní API pro vyhledávání protokolu najdete v tématu [skupiny počítačů v protokolu analýzy protokolů hledání REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Když konfigurujete analýzy protokolů členství ve skupině služby Active Directory tooimport, analyzuje hello skupiny členství ve všech počítačích připojených k doméně s agentem OMS hello.  Skupina počítačů se vytvoří v analýzy protokolů pro každou skupinu zabezpečení ve službě Active Directory a každý počítač přidán odpovídající toohello skupiny zabezpečení, které jsou členy skupiny počítačů toohello.  Členství v této se průběžně aktualizuje každé 4 hodiny.  

Konfigurace skupin zabezpečení služby Active Directory tooimport analýzy protokolů z hello **skupiny počítačů** nabídky v analýzy protokolů **nastavení**.  Vyberte **automatizace** a potom **členství ve skupině služby Active Directory Import z počítačů**.  Není nutná žádná další konfigurace.

![Skupiny počítačů ze služby Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Když skupiny mají být naimportována, hello nabídky seznamy hello počet počítačů s členství ve skupině zjistil a hello počet skupin importovat.  Kliknutím na některou z těchto odkazů tooreturn hello **ComputerGroup** záznamy s těmito informacemi.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Při konfiguraci analýzy protokolů tooimport WSUS členství ve skupinách, analyzuje hello cílení na členství ve skupině všechny počítače s agentem OMS hello.  Pokud používáte klienta cílení na jakýkoli počítač, který je připojený tooOMS a je součástí všech WSUS cílení na skupiny jeho členství ve skupinách importoval tooLog Analytics. Pokud používáte serverové cílení hello agenta OMS by měl být nainstalován na hello WSUS serveru, aby toobe informace o členství ve skupině hello importovat tooOMS.  Členství v této se průběžně aktualizuje každé 4 hodiny. 

Konfigurace skupin zabezpečení služby Active Directory tooimport analýzy protokolů z hello **skupiny počítačů** nabídky v analýzy protokolů **nastavení**.  Vyberte **služby Active Directory** a potom **členství ve skupině služby Active Directory Import z počítačů**.  Není nutná žádná další konfigurace.

![Skupiny počítačů ze služby Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Když skupiny mají být naimportována, hello nabídky seznamy hello počet počítačů s členství ve skupině zjistil a hello počet skupin importovat.  Kliknutím na některou z těchto odkazů tooreturn hello **ComputerGroup** záznamy s těmito informacemi.

## <a name="managing-computer-groups"></a>Správa skupin počítačů
Můžete zobrazit skupiny počítačů, které byly vytvořeny z protokolu hledání nebo hello rozhraní API pro vyhledávání protokolu z hello **skupiny počítačů** nabídky v analýzy protokolů **nastavení**.  Klikněte na tlačítko hello **x** v hello **odebrat** skupinu počítačů hello toodelete sloupce.  Klikněte na tlačítko hello **zobrazit členy** ikony pro skupiny toorun hello skupině protokolu vyhledávání, který vrátí její členy. 

![Skupiny uložené počítačů](media/log-analytics-computer-groups/configure-saved.png)

toomodify hello skupiny, vytvořte novou skupinu s hello stejné **kategorie** a **název** toooverwrite hello původní skupiny.

## <a name="using-a-computer-group-in-a-log-search"></a>Použití skupiny počítačů do protokolu hledání
Můžete použít následující syntaxi toorefer tooa skupinu počítačů do protokolu hledání hello.  Zadání hello **kategorie** je volitelné a vyžaduje, pokud máte skupiny počítačů s hello stejný název v různých kategorií. 

    $ComputerGroups[Category: Name]

Při spuštění vyhledávání se nejprve překládají hello členy všech skupin počítačů, které jsou součástí hello vyhledávání.  Pokud skupina hello je založená na protokolu vyhledávání, pak toto hledání běží před provedením hledání nejvyšší úrovně protokolů hello tooreturn hello členy skupiny hello.

Skupiny počítačů se obvykle používá s hello **IN** klauzuli v hledání protokolů hello jako hello následující ukázka:

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), potom použijte skupinu počítačů v dotazu tak, že považuje jeho alias jako funkce jako hello následující ukázka:
> 
>  `UpdateSummary | where Computer IN (MyComputerGroup)`

## <a name="computer-group-records"></a>Záznamů skupiny počítače
V úložišti hello OMS pro každý členství ve skupině počítačů vytvořené pomocí služby Active Directory nebo služby WSUS se vytvoří záznam.  Tyto záznamy mají typ **ComputerGroup** a mít hello vlastnosti v hello následující tabulka.  Záznamy nejsou vytvořeny pro skupiny počítačů, které jsou založené na protokolu hledání.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Počítač |Název hello členského počítače. |
| Skupina |Název skupiny hello. |
| GroupFullName |Úplná cesta toohello skupina včetně hello zdroje a název zdroje. |
| GroupSource |Zdroj této skupině se shromážděných z. <br><br>Active Directory<br>SLUŽBY WSUS<br>WSUSClientTargeting |
| GroupSourceName |Název zdroje hello, který hello skupiny nebyla shromážděna z.  U služby Active Directory je to název domény hello. |
| ManagementGroupName |Název skupiny pro správu hello SCOM agenty.  Pro jiné agenty jde AOI -\<ID pracovního prostoru\> |
| TimeGenerated |Skupina počítačů hello datum a čas byl vytvořen nebo aktualizován. |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.  

