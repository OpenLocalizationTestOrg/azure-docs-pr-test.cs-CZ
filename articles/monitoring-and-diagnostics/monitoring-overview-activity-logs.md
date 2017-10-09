---
title: "aaaOverview hello protokol činnosti Azure | Microsoft Docs"
description: "Zjistěte, jaké hello je protokol činnosti Azure a jak můžete ji použít toounderstand událostí v rámci vašeho předplatného Azure."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: johnkem
ms.openlocfilehash: cba79b7f6dc0833ef588382e763761fd77d43307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-subscription-activity-with-hello-azure-activity-log"></a>Sledovat činnost předplatné s hello protokol činnosti Azure
Hello **protokol činnosti Azure** je protokol odběru, který poskytuje přehled o události na úrovni předplatného, k nimž došlo v Azure. To zahrnuje celou řadu dat z Azure Resource Manager tooupdates provozních dat na události stavu služby. Hello protokol aktivit se dřív označovala jako "Protokoly auditu" nebo "Provozní protokoly," od hello administrativní kategorie sestavy rovině řízení události pro vaše předplatné. Pomocí hello protokolu činnosti, můžete určit hello ", kdo a kdy se pro všechny zápisu operace (PUT, POST, DELETE) pořízené hello prostředky ve vašem předplatném. Můžete také chápou hello stav operace hello a další relevantní vlastnosti. operace čtení (GET) nebo operace pro prostředky, které používají hello Classic nezahrnuje Hello protokol aktivit nebo model "RDFE".

![Aktivita protokoly vs jiné typy protokolů ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Obrázek 1: Protokoly aktivity vs jiné typy protokolů

Hello protokol aktivit se liší od [diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md). Protokoly aktivity poskytují data týkající se operací hello na prostředku z hello mimo (hello "řízení rovinou"). Diagnostické protokoly jsou vygenerované prostředek a zadejte informace o operaci hello toho prostředku (hello "data rovinou").

Můžete načíst události z protokolu aktivit pomocí hello portál Azure, rozhraní příkazového řádku, rutiny prostředí PowerShell a rozhraní REST API Azure monitorování.


> [!WARNING]
> Hello protokol činnosti Azure je určen pro činnosti, které ve službě Správce prostředků Azure. Tato možnost nesleduje prostředky pomocí modelu Classic nebo RDFE hello. Některé typy prostředků Classic mají proxy poskytovatele prostředků v Azure Resource Manageru (například Microsoft.Network). Při práci s typem prostředku Classic prostřednictvím Správce Azure Resource Manager pomocí těchto poskytovatelů prostředků proxy serveru, se zobrazí v hello protokol aktivit hello operace. Při práci s typ prostředku Classic v portálu Classic hello nebo jinak mimo hello proxy Azure Resource Manager, vaše akce jenom zaznamenávají v hello protokolu operací. Hello protokolu operací je dostupné pouze v portálu Classic hello.
>
>

Následující video představení hello protokol aktivit hello zobrazení.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="categories-in-hello-activity-log"></a>Kategorie v hello protokol aktivit
Hello protokol aktivit obsahuje několik kategorií data. Úplné podrobnosti o hello schémat z těchto kategorií [najdete v článku](monitoring-activity-log-schema.md). Mezi ně patří:
* **Správce** – Tato kategorie obsahuje hello záznam všech vytvořit, operace aktualizace, odstranění a akce provést pomocí Správce prostředků. Příklady hello, které typy událostí, které zobrazí se v této kategorii "vytvořit virtuální počítač" a "odstranit skupinu zabezpečení sítě" každé akce uživatele nebo aplikace pomocí Správce prostředků je modelovaná jako operace na konkrétní typ prostředku. Pokud je typ operace hello zapsat, Delete nebo akce, záznamy hello hello start a úspěch nebo selhání této operace se zaznamenávají do administrativní kategorie hello. Administrativní kategorie Hello také zahrnuje všechny řízení přístupu na základě toorole změny v předplatném.
* **Stav služby** – Tato kategorie obsahuje záznam hello všechny služby stavu incidentů, kterým došlo v Azure. Příkladem hello typu události, které se zobrazí se v této kategorii je "SQL Azure v oblasti Východ USA dochází k výpadku." Události stavu služby existují ve pět variantách: je vyžadována akce, jíž obnovení, Incident, údržby, informace nebo zabezpečení a objeví jenom tehdy, pokud máte prostředku v hello předplatné, které by ovlivňovat hello událostí.
* **Výstrahy** – Tato kategorie obsahuje záznam hello všechny aktivací Azure výstrah. Příkladem hello typu události, který zobrazí se v této kategorii je "% využití procesoru na Můjvp přes 80 pro hello za posledních 5 minut." Výstrahy koncept mají různé systémy Azure – můžete definovat pravidla nějaká a přijímat oznámení v případě podmínky odpovídají daného pravidla. Pokaždé, když podporovaných Azure typu výstrahy, aktivuje,' nebo hello podmínky jsou splněny toogenerate oznámení, záznam hello aktivace je také poslat toothis kategorii hello protokol aktivit.
* **Škálování** – Tato kategorie obsahuje záznam hello jakékoli operace související toohello události modulu hello škálování podle nastavení automatického škálování, který jste definovali ve vašem předplatném. Příkladem hello typu události, které se zobrazí se v této kategorii je "Škálování rozšiřování škálování využívajících akce se nezdařila." Použití automatického škálování, můžete automaticky škálovat nebo škálování hello podle počtu instancí v podporovaných prostředků typu čas, den nebo zatížení (metriky) dat, na které se používá nastavení automatického škálování. Při splnění podmínek hello tooscale nahoru nebo dolů, hello spuštění a úspěšné nebo neúspěšné události se zaznamenávají v této kategorii.
* **Doporučení** – Tato kategorie obsahuje doporučení události z určité typy prostředků, jako jsou webové servery a servery SQL Server. Tyto události nabízet doporučení, jak toobetter využívat vaše prostředky. Pokud máte prostředky, které emitování doporučení pouze zobrazí události tohoto typu.
* **Zásady zabezpečení a stav prostředků** -tyto kategorie neobsahují žádné události; jsou vyhrazené pro budoucí použití.

## <a name="event-schema-per-category"></a>Schématu události podle kategorie
[Najdete v tomto článku toounderstand hello aktivity protokolu událostí schématu podle kategorie.](monitoring-activity-log-schema.md)

## <a name="what-you-can-do-with-hello-activity-log"></a>Co můžete dělat s hello protokol aktivit
Tady jsou některé hello věcí, které můžete provést hello protokol aktivit:

![Azure protokol aktivit](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Dotazování a zobrazit v hello **portál Azure**.
* [Vytvořte výstrahu na aktivity protokolu události.](monitoring-activity-log-alerts.md)
* [Streamovat ho tooan **centra událostí** ](monitoring-stream-activity-logs-event-hubs.md) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako je například PowerBI.
* Analyzovat v Power BI pomocí hello [ **balíček obsahu Power BI**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Uložit tooa **účet úložiště** pro archivaci nebo ruční kontroly](monitoring-archive-activity-log.md). Můžete zadat hello dobu uchování (ve dnech) pomocí hello **profil protokolu**.
* Dotaz je pomocí rutiny prostředí PowerShell, rozhraní příkazového řádku nebo REST API.

## <a name="query-hello-activity-log-in-hello-azure-portal"></a>Dotaz hello protokol aktivit v hello portálu Azure
V rámci hello portálu Azure můžete zobrazit aktivitu protokolu na několika místech:
* Hello **okno Protokol činnosti**, kterým můžete přistupovat vyhledáním hello protokol aktivit v části "Další služby" v levém navigačním podokně hello.
* Hello **monitorování okno**, která se zobrazí ve výchozím nastavení v levém navigačním podokně hello. Hello protokol aktivit je jeden oddíl tohoto okna Azure monitorování.
* Jakémukoli prostředku **okna prostředků**, například hello okno konfigurace pro virtuální počítač. Hello protokol aktivit se jeden z oddílů hello většina těchto oken prostředků a kliknete na něm automaticky vyfiltruje události hello toothose související toothat konkrétní prostředek.

V hello portálu Azure můžete filtrovat aktivity protokolu podle těchto polí:
* Časový interval - hello počáteční a koncový čas pro události.
* Kategorie – kategorie události hello, jak je popsáno výše.
* Předplatné – jeden nebo více názvů předplatného Azure.
* Skupiny prostředků – jeden nebo více prostředků skupiny v rámci těchto předplatných.
* Prostředek (název) - hello název konkrétní prostředek.
* Typ prostředku - hello typ prostředku, například Microsoft.Compute/virtualmachines.
* Operace název – název hello operace Azure Resource Manager, například Microsoft.SQL/servers/Write.
* Závažnost – hello úroveň závažnosti události hello (Informativní upozornění, chyby, kritické).
* Událost iniciovaná - hello volající nebo uživatele, který provedl operaci hello.
* Otevřete vyhledávání - Toto je již otevřete textového pole hledání, prohledáván všechna pole v všechny události pro tento řetězec.

Jakmile definujete sadu filtrů, můžete ho uložit jako dotaz, který je trvalé napříč relacemi, pokud byste někdy potřebovali tooperform hello stejný dotaz s tyto filtry znovu použity v budoucí hello. Můžete taky připnout dotaz tooyour řídicí panel Azure tooalways zachovat přehled na konkrétní události.

Kliknutím na tlačítko "Použijí" spustí dotaz a všechny odpovídající události. Kliknutím na libovolnou událost v seznamu hello ukazuje hello souhrn tuto událost a také hello úplné nezpracovaném formátu JSON této události.

Pro více síly, můžete kliknout na hello **hledání protokolů** ikonu, která zobrazuje data protokolu aktivit v hello [řešení Log Analytics aktivity protokolu analýzy](../log-analytics/log-analytics-activity.md). okno Protokol činnosti Hello nabízí prostředí základní filtru nebo přejděte na protokoly, ale umožňuje analýzy protokolů toopivot můžete dotazovat a vizualizovat data výkonnější způsoby.

## <a name="export-hello-activity-log-with-a-log-profile"></a>Hello export protokolu aktivit k profilu protokolu
A **profil protokolu** řídí, jak je export protokolu aktivit. Použití profilu protokolu, můžete nakonfigurovat:

* Kde by měly být odeslány hello protokol aktivit (účet úložiště nebo Event Hubs)
* Kategorie, které události (akce zápisu, odstranit,) by měly být odeslány. *význam Hello "kategorie" v profilech protokolu a aktivity protokolu událostí se liší. V hello profil protokolu "Kategorie" představuje typ operace hello (akce zápisu, odstranit,). V protokolu aktivit události představuje vlastnost "kategorie" hello hello zdroje nebo typu události (například správy, ServiceHealth, upozornění a další).*
* Které oblasti (umístění) mají být exportovány. Zkontrolujte že tooinclude "globální", mnoho událostí v hello protokolu aktivit jsou globální události.
* Jak dlouho hello protokol aktivit uchovávání v účtu úložiště.
    - Uchování nulový počet dnů znamená, že jsou protokoly v nekonečné smyčce. Hodnota hello, jinak hodnota může být libovolný počet dnů od 1 do 2147483647.
    - Pokud nejsou nastavené zásady uchovávání informací, ale ukládání protokolů v účtu úložiště je zakázaný (například pokud jenom jsou vybrané možnosti služby Event Hubs nebo OMS), zásady uchovávání informací hello nemají žádný vliv.
    - Zásady uchovávání informací jsou použité za den, tak při hello Konec dne (UTC), zaprotokoluje hello dni, který je teď nad rámec zásad uchovávání informací hello se odstraní. Například pokud jste měli zásady uchovávání informací jeden den, od začátku hello dnešní den hello hello protokoly z hello den před včerejšek by odstraněn.

Můžete použít účet úložiště nebo názvový prostor události rozbočovače, který není součástí hello stejnému předplatnému jako jeden emitování protokoly hello. Hello uživatel, který konfiguruje nastavení hello musí mít hello příslušné RBAC přístup tooboth předplatné.

Tato nastavení můžete nakonfigurovat přes hello "Export" možnost v okně hello protokol aktivit portálu hello. Je také možné nakonfigurovat prostřednictvím kódu programu [pomocí rozhraní REST API Azure monitorování hello](https://msdn.microsoft.com/library/azure/dn931927.aspx), rutiny prostředí PowerShell nebo rozhraní příkazového řádku. Předplatné může mít pouze jeden profil protokolu.

### <a name="configure-log-profiles-using-hello-azure-portal"></a>Konfigurace protokolu profilů pomocí hello portálu Azure
Můžete stream hello protokol aktivit tooan centra událostí nebo uložit je do účtu úložiště pomocí možnosti "Export" hello v hello portálu Azure.

1. Přejděte toohello **protokol aktivit** okno pomocí hello nabídky na levé straně hello portálu hello.

    ![Přejděte tooActivity protokolu portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klikněte na tlačítko hello **exportovat** tlačítko hello horní části okna hello.

    ![Tlačítko Exportovat portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. V zobrazeném okně hello můžete vybrat:  
  * oblasti, pro které byste chtěli tooexport události
  * Hello toowhich účet úložiště, které byste chtěli toosave události
  * Hello počet dní, které chcete tooretain tyto události v úložišti. Nastavení 0 dnů zachová hello protokoly navždy.
  * Hello Service Bus Namespace ve které chcete toobe centra událostí vytvořit pro streamování tyto události.

     ![Export protokolu aktivit okno](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klikněte na tlačítko **Uložit** toosave tato nastavení. nastavení Hello jsou okamžitě být použité tooyour předplatné.

### <a name="configure-log-profiles-using-hello-azure-powershell-cmdlets"></a>Konfigurace protokolu profilů pomocí rutin prostředí Azure PowerShell hello
#### <a name="get-existing-log-profile"></a>Získat stávající profil protokolu
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Přidat profil protokolu
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Name (Název) |Ano |Název vašeho profilu protokolu. |
| StorageAccountId |Ne |ID prostředku hello účet úložiště toowhich hello protokol aktivit má být uložen. |
| serviceBusRuleId |Ne |ID pravidla sběrnice služby pro chcete centra událostí toohave vytvořené v oboru názvů Service Bus hello. Je řetězec s Tento formát: `{service bus resource ID}/authorizationrules/{key name}`. |
| Umístění |Ano |Seznam oddělený čárkami oblastí, pro které byste chtěli toocollect aktivity protokolu události. |
| retentionInDays |Ano |Počet dní pro události, které by měl být zachován, od 1 do 2147483647. Hodnota nula ukládá protokoly hello po neomezenou dobu (navždy). |
| Kategorie |Ne |Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny. Možné hodnoty jsou zápisu, odstranění a akce. |

#### <a name="remove-a-log-profile"></a>Odebrat profil protokolu
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-hello-azure-cross-platform-cli"></a>Konfigurace protokolu profilů hello pomocí rozhraní příkazového řádku Azure a platformy
#### <a name="get-existing-log-profile"></a>Získat stávající profil protokolu
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Hello `name` vlastnost by měla být hello název vašeho profilu protokolu.

#### <a name="add-a-log-profile"></a>Přidat profil protokolu
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| jméno |Ano |Název vašeho profilu protokolu. |
| storageId |Ne |ID prostředku hello účet úložiště toowhich hello protokol aktivit má být uložen. |
| serviceBusRuleId |Ne |ID pravidla sběrnice služby pro chcete centra událostí toohave vytvořené v oboru názvů Service Bus hello. Je řetězec s Tento formát: `{service bus resource ID}/authorizationrules/{key name}`. |
| Umístění |Ano |Seznam oddělený čárkami oblastí, pro které byste chtěli toocollect aktivity protokolu události. |
| retentionInDays |Ano |Počet dní pro události, které by měl být zachován, od 1 do 2147483647. Hodnota nula ukládá protokoly hello po neomezenou dobu (navždy). |
| Kategorie |Ne |Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny. Možné hodnoty jsou zápisu, odstranění a akce. |

#### <a name="remove-a-log-profile"></a>Odebrat profil protokolu
```
azure insights logprofile delete --name my_log_profile
```

## <a name="next-steps"></a>Další kroky
* [Další informace o hello protokol aktivit (dříve protokoly auditu)](../azure-resource-manager/resource-group-audit.md)
* [Stream hello protokol činnosti Azure tooEvent rozbočovače](monitoring-stream-activity-logs-event-hubs.md)
