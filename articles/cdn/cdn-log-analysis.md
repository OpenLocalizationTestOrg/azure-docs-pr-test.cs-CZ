---
title: "V protokolu analýzy Azure CDN | Microsoft Docs"
description: "Zákazníka můžete povolit analýzy protokolů pro Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 03ff74ae4e40d3f2279caaf4f73e9b4aac6a2ebb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a>Diagnostické protokoly pro Azure CDN

Když povolíte CDN pro vaši aplikaci, bude pravděpodobně chtít monitorovat využití CDN, zkontrolujte stav vaší doručení a potenciální potíže. Azure CDN poskytuje tyto možnosti se [CDN základní analýza](cdn-analyze-usage-patterns.md) a [diagnostických protokolů](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)

## <a name="cdn-core-analytics"></a>Základní analýza CDN
Jako aktuální uživatel Azure CDN Verizon standard nebo premium profilu jste už moct zobrazovat základní analýza na doplňkovém portálu, který je přístupný prostřednictvím možnosti "Manage" z portálu Azure. 

## <a name="azure-diagnostic-logs"></a>Azure diagnostických protokolů

Azure pomocí této nové funkce, můžete nyní zobrazit analýzu základní a uložit je do jedné nebo více cílů, včetně:

 - Účet služby Azure Storage
 - Azure Event Hubs
 - [Úložiště analýzy protokolů OMS](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 Tato funkce je k dispozici pro všechny koncové body CDN patřící do Verizon (Standard a Premium) a profilů CDN Akamai (Standard).

Diagnostické protokoly umožňují, aby můžete využívat požadovaným způsobem exportu metriky základní informace o využití z koncový bod CDN do různých zdrojů. Například můžete provést následující typy dat exportu:

- Export dat do úložiště objektů blob, exportovat do souboru CSV a generovat grafy v aplikaci excel.
- Exportovat data do centra událostí a korelovat s daty z jiné služby azure.
- Exportovat data do protokolu analýzy a zobrazení dat ve vlastní pracovní prostor OMS

Následující obrázek znázorňuje typické přehled CDN základní analýza data.

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Obrázek 1 – základní analýza CDN zobrazení*

Následující návod projde schéma analytická data základní kroky při povolení funkce a jejich předání do různých umístění a spotřebě z těchto cílů.

## <a name="enable-logging-with-azure-portal"></a>Povolit protokolování pomocí portálu Azure

> [!NOTE]
> Diagnostické protokoly jsou zapnuté **vypnout** ve výchozím nastavení. 

Použijte následující postup povolení protokolování s CDN základní analýza:

Přihlaste se k webu [Azure Portal](http://portal.azure.com). Pokud ještě nemáte CDN povolené pro pracovní postup [povolení Azure CDN](cdn-create-new-endpoint.md) než budete pokračovat.

1. Na portálu, přejděte na **profil CDN**.
2. Vyberte profil CDN a pak vyberte koncového bodu CDN, který chcete povolit **protokolů diagnostiky**.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Přejděte na **protokolů diagnostiky** okno pod **monitorování** část, pak změňte na stavu **na**.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Povolit protokolování s Azure Storage
    
Používání Azure Storage k ukládání protokolů, vyberte **archivu do účtu úložiště**, vyberte dní uchovávání a klikněte na tlačítko **CoreAnalytics** pod **protokolu**.

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*Obrázek 2 – protokolování s Azure Storage*

### <a name="logging-with-oms-log-analytics"></a>Protokolování s OMS analýzy protokolů

OMS Log Analytics k ukládání protokolů, postupujte podle těchto kroků:

1. Z **protokolů diagnostiky** okno pod **monitorování**, vyberte **odeslat k analýze protokolů** z 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Konfigurace protokolování analýzy protokolů kliknutím na konfigurovat. Tím přejdete do dialogového okna, kde můžete vybrat předchozí pracovního prostoru nebo vytvořte novou.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Klikněte na tlačítko **vytvořit nový pracovní prostor**.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/07_Create-new.png)

4. Dále musíte vybrat nový název pracovního prostoru, stávající předplatné, skupinu prostředků (nová nebo stávající), umístění a cenovou úroveň. Máte možnost Připnutí na řídicí panel tuto konfiguraci. Klikněte na tlačítko OK k dokončení konfigurace.

    Dále byste měli vidět pracovního prostoru s názvy skupiny pracovním prostorem OMS a prostředků. Názvy musí být jedinečný a použít pouze písmena, číslice a pomlčky. Nejsou povoleny mezery a podtržítka. 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. Zobrazí další krátkou zprávu oznamující, že váš prostor byl vytvořen a vrátíte se na obrazovce konfigurace protokolování. Můžete potvrdit, název pracovního prostoru analýzy protokolů.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    Jakmile jste nastavili konfigurace analýzy protokolů, zkontrolujte, zda že můžete taky zaškrtnout políčko CoreAnalytics pro protokolování CDN.

6. Pokud všechno, co je titulků, klikněte na tlačítko **Uložit** tlačítka v horní části dialogového okna konfigurace.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/10_Save-me.png)

    **Uložit** tlačítko již není aktivní a nyní je tlačítko Zapnout nebo vypnout v, ale blue místo fialová.

7. Pokud chcete zobrazit nové pracovní prostor OMS, přejděte na portálu Azure řídicí panel, klikněte na název pracovního prostoru analýzy protokolů. Dále uvidíte pracovního prostoru (ujistěte se, zda je na levé straně zvýrazněný pracovním prostorem OMS). Klikněte na dlaždici portálu OMS zobrazíte pracovní prostor v úložišti OMS. 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Úložiště OMS je nyní připraven k protokolovat data. Aby bylo možné využívat data, musíte použít [OMS řešení](#consuming-oms-log-analytics-data), zahrnuté později v tomto článku.

Další informace o protokolu data zpoždění [zde](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Povolení protokolování v prostředí PowerShell

Dole je příklad o tom, jak povolit a získat diagnostické protokoly prostřednictvím rutin prostředí PowerShell Azure.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Povolení diagnostických protokolů v účtu úložiště

Nejdřív se přihlaste a vyberte předplatné:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


K povolení diagnostických protokolů v účtu úložiště použijte tento příkaz:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
K povolení protokolů diagnostiky v pracovním prostoru OMS použijte tento příkaz:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Použití protokolů diagnostiky ze služby Azure Storage
Tato část popisuje schéma základní analýza CDN, jak tyto jsou uspořádány v rámci účtu úložiště Azure a poskytuje ukázkový kód pro stažení protokolů v souboru CSV.

### <a name="using-microsoft-azure-storage-explorer"></a>Pomocí Průzkumníka úložiště Microsoft Azure
Než se dostanete k základní analytická data z účtu úložiště Azure, musíte nejprve nástroj pro přístup k obsahu v účtu úložiště. Na trhu jsou k dispozici několik nástrojů, i když je ten, který doporučujeme Microsoft Azure Storage Explorer. Si můžete stáhnout nástroj z [zde](http://storageexplorer.com/). Po stažení a instalaci softwaru, nakonfigurujte ji používat stejný účet úložiště Azure, který byl nakonfigurovaný jako cíl do protokolů diagnostiky CDN.

1.  Otevřete **Microsoft Azure Storage Explorer**
2.  Najděte účet úložiště
3.  Přejděte na **"Kontejnery objektů Blob"** uzel v rámci toto úložiště účtu a rozbalte uzel
4.  Vyberte kontejner s názvem **"insights-logs-coreanalytics"** a poklikejte na něj
5.  Zobrazit výsledky nahoru v pravém podokně počínaje první úroveň, který vypadá podobně jako **"resourceId ="**. Pokračujte kliknutím na úplně, dokud naleznete v souboru **PT1H.json**. Viz následující poznámka vysvětlení cesty.
6.  Každý objekt blob **PT1H.json** představuje analýzy protokolů pro jednu hodinu pro konkrétní koncový bod CDN nebo jeho vlastní doménu.
7.  Schéma obsah tohoto souboru JSON je popsaný v části schéma základní analýzy protokolů


> [!NOTE]
> **Formát cesty objektu BLOB**
> 
> Základní analýza protokolů jsou generovány, každou hodinu. Všechna data pro jednu hodinu jsou shromážděných a uložených do jediného objektu Blob Azure jako datové části JSON. Cesta k této objektů Blob v Azure se zobrazí, jako kdyby je hierarchická struktura. Toto je vzhledem k tomu, že nástroj Průzkumník úložišť interpretuje '/' za oddělovač adresářů a zobrazuje hierarchii ke zvýšení pohodlí. Ve skutečnosti celé cesty právě představuje název objektu blob. Tento název objektu blob postupuje podle následující konvence   
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Popis polí:**

|hodnota|Popis|
|-------|---------|
|ID předplatného    |ID předplatného Azure. Toto je ve formátu Guid.|
|Prostředek |Název skupiny název skupiny prostředků, do které patří prostředky CDN.|
|Název profilu |Název profilu CDN|
|Název koncového bodu |Název koncového bodu CDN|
|Rok|  reprezentace 4 číslice roku například 2017|
|Měsíc| 2 číslice reprezentace číslo měsíce. 01 = leden... 12 = prosinec|
|Den|   2 číslice reprezentace den v měsíci|
|PT1H.JSON| Skutečný soubor JSON se uloží analytická data|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Export základní analytická Data do souboru CSV

Chcete-li snadný přístup k základní analýza, poskytujeme ukázkový kód pro nějaký nástroj, který umožňuje stahování souborů JSON do nestrukturované textový soubor s oddělovači souboru ve formátu, který lze snadno vytvářet grafy nebo jiných agregací.

Zde je, jak můžete použít nástroj:

1.  Po kliknutí na odkaz githubu: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  Stáhněte si kód
3.  Postupujte podle pokynů pro kompilaci a konfigurace
4.  Spusťte nástroj
5.  Výsledný soubor CSV zobrazuje analytická data v jednoduchých ploché hierarchii.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Použití protokolů diagnostiky z úložiště analýzy protokolů OMS
Analýzy protokolů je služba v Operations Management Suite (OMS), který monitoruje své cloudové a místní prostředí k udržování své dostupnosti a výkonu. Shromažďuje data generovaná prostředky ve vašem cloudovém a místním prostředí a také data z dalších nástrojů pro monitorování a poskytuje analýzy napříč zdroji. 

Chcete-li použít analýzy protokolů, je nutné [povolit protokolování](#enable-logging-with-azure-storage) do úložiště analýzy protokolů Azure OMS, které je popsané výše v tomto článku.

### <a name="using-the-oms-repository"></a>Použití úložiště OMS

 Následující diagram znázorňuje architekturu vstupy a výstupy úložiště:

![Úložiště analýzy protokolů OMS](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Obrázek 3 - úložiště analýzy protokolů*

Data můžete zobrazit v mnoha různými způsoby pomocí řešení pro správu. Můžete získat řešení pro správu z [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Řešení pro správu můžete nainstalovat z Azure marketplace kliknutím **ho získat** odkaz na konci každé řešení.

### <a name="adding-an-oms-cdn-management-solution"></a>Přidávání do řešení pro správu OMS CDN

Použijte následující postup přidání řešení pro správu:

1.   Pokud jste tak již neučinili, přihlaste se k portálu Azure pomocí svého předplatného Azure a přejděte na řídicí panel.
    ![Řídicí panel Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. V **nový** okno pod **Marketplace**, vyberte **monitorování + správu**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. V **monitorování + správu** okně klikněte na tlačítko **zobrazit všechny**.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/15_See-all.png)

4.  Vyhledejte CDN do vyhledávacího pole.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Vyberte **Azure CDN základní analýza**. 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  Po kliknutí na **vytvořit**, zobrazí se výzva k vytvoření nové pracovní prostor OMS nebo použijte existující. 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Vyberte pracovní prostor, který jste vytvořili před. Pak je potřeba přidat účet automation.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Na následující obrazovce se zobrazí formulář účet automation, který je nutné vyplnit. 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/20_Automation.png)

9. Po vytvoření účtu automation, jste připraveni přidat řešení. Klikněte na tlačítko **Vytvořit**.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/21_Ready.png)

10. Řešení teď přidaná do pracovního prostoru. Přejděte zpět na portálu Azure řídicího panelu.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/22_Dashboard.png)

    Klikněte na pracovní prostor analýzy protokolů, které jste vytvořili pro přejděte do pracovního prostoru. 

11. Klikněte **portálu OMS** dlaždice zobrazíte nové řešení na portálu OMS.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/23_workspace.png)

12. Na portálu OMS by teď měl vypadat jako následující obrazovka:

    ![Zobrazit všechno](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Klikněte na jednu z dlaždice zobrazíte několik zobrazení na vaše data.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/25_Interior-view.png)

    Můžete se posunete doleva nebo doprava zobrazíte další dlaždice představující jednotlivé zobrazení do data. 

    Kliknutím na jedno ze dlaždice získáte další informace o data.

     ![Zobrazit všechno](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Nabídky a cenové úrovně

Můžete zobrazit nabídky a cenové úrovně pro řešení pro správu OMS [zde](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Přizpůsobení zobrazení

Zobrazení lze přizpůsobit do vašich dat pomocí **Návrhář zobrazení**. Přejděte do pracovního prostoru OMS a začnete navrhovat kliknutím **Návrhář zobrazení** dlaždici.

![Návrhář zobrazení](./media/cdn-diagnostics-log/27_Designer.png)

Můžete přetáhnout a vyřadit typy grafů zleva a zadejte podrobnosti dat, který chcete analyzovat na levé straně.

![Návrhář zobrazení](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Zpoždění data protokolu

Verizon protokolu data zpoždění | Akamai protokolu data zpoždění
--- | ---
Data protokolu Verizon je zpožděno 1 hodinu a trvat až 2 hodin zahájíte zobrazování po dokončení šíření koncový bod. | Data protokolu Akamai je 24 hodin, zpoždění a trvá až 2 hodin start, zobrazování, pokud byla vytvořena více než 24 hodinami. Pokud byl nedávno vytvořen, může trvat až 25 hodin pro protokoly spuštění, které jsou uvedeny.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Typy protokolů diagnostiky pro CDN základní analýza

Nabízíme aktuálně pouze základní analýza protokoly, které obsahují metriky ukazující statistiky odpovědi HTTP a odchozí statistiky, jak je vidět z CDN POP nebo okrajů.

### <a name="core-analytics-metrics-details"></a>Základní analýza metriky podrobnosti
Následující tabulka uvádí seznam metriky, které jsou k dispozici v základní Analýza protokolů. Ne všechny metriky jsou k dispozici od všech poskytovatelů, i když tyto rozdíly jsou minimální. V následující tabulce také ukazuje, pokud daná metrika je k dispozici od zprostředkovatele. Upozorňujeme, že jsou k dispozici pro jenom ty koncové body CDN mající přenosy na těchto metriky.


|Metrika                     | Popis   | Verizon  | Akamai 
|---------------------------|---------------|---|---|
| RequestCountTotal         |Celkový počet přístupů požadavek během tohoto období| Ano  |Ano   |
| RequestCountHttpStatus2xx |Počet všech požadavků, které 2xx kód HTTP (např. 200, 202)              | Ano  |Ano   |
| RequestCountHttpStatus3xx | Počet všech požadavků, které 3xx kód HTTP (např. 300, 302)              | Ano  |Ano   |
| RequestCountHttpStatus4xx |Počet všech požadavků, které 4xx kód HTTP (např. 400, 404)               | Ano   |Ano   |
| RequestCountHttpStatus5xx | Počet všech požadavků, jejichž výsledkem kód HTTP 5xx (např. 500, 504)              | Ano  |Ano   |
| RequestCountHttpStatusOthers |  Počet všechny ostatní kódy HTTP (mimo 2xx 5xx) | Ano  |Ano   |
| RequestCountHttpStatus200 | Počet všech požadavků, jejichž výsledkem 200 kód odpovědi HTTP              |Ne   |Ano   |
| RequestCountHttpStatus206 | Počet všech požadavků, jejichž výsledkem 206 kód odpovědi HTTP              |Ne   |Ano   |
| RequestCountHttpStatus302 | Počet všech požadavků, jejichž výsledkem 302 kód odpovědi HTTP              |Ne   |Ano   |
| RequestCountHttpStatus304 |  Počet všech požadavků, jejichž výsledkem 304 kód odpovědi HTTP             |Ne   |Ano   |
| RequestCountHttpStatus404 | Počet všech požadavků, jejichž výsledkem kódu odpovědi HTTP 404              |Ne   |Ano   |
| RequestCountCacheHit |Počet všech požadavků, jejichž výsledkem požadavků mezipaměti. To znamená, že zpracování asset přímo z POP do klienta.               | Ano  |Ne   |
| RequestCountCacheMiss | Počet všech požadavků, která byla vygenerována v neúspěšnému přístupu do mezipaměti. To znamená asset nebyla nalezena na serveru POP nejbližší klientovi a proto byla načtena z tohoto počátku.              |Ano   | Ne  |
| RequestCountCacheNoCache | Počet všech požadavků na prostředek, které nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele na hranici.              |Ano   | Ne  |
| RequestCountCacheUncacheable | Počet všech požadavků na prostředky, které nebudou moci ukládat do mezipaměti asset Cache-Control a Expires hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klient HTTP                |Ano   |Ne   |
| RequestCountCacheOthers | Počet všech požadavků stavem mezipaměti, které nejsou pokryty výše.              |Ano   | Ne  |
| EgressTotal | Odchozí přenosy dat v GB              |Ano   |Ano   |
| EgressHttpStatus2xx | Odchozí datové přenosy * pro odpovědi s stavové kódy HTTP 2xx v GB            |Ano   |Ne   |
| EgressHttpStatus3xx | Přenos odchozích dat pro odpovědi s stavové kódy HTTP 3xx v GB              |Ano   |Ne   |
| EgressHttpStatus4xx | Přenos odchozích dat pro odpovědi s stavové kódy HTTP 4xx v GB               |Ano   | Ne  |
| EgressHttpStatus5xx | Přenos odchozích dat pro odpovědi s stavové kódy HTTP 5xx v GB               |Ano   |  Ne |
| EgressHttpStatusOthers | Odchozí přenosy dat pro odpovědi s ostatních stavových kódech HTTP v GB                |Ano   |Ne   |
| EgressCacheHit |  Odchozí přenosy dat pro odpovědi, které byly doručeny přímo z mezipaměti CDN na CDN POP nebo okrajů  |Ano   |  Ne |
| EgressCacheMiss | Odchozí přenosy dat pro odpovědi, které nebyly na nejbližší server POP najít a načíst ze zdrojového serveru              |Ano   |  Ne |
| EgressCacheNoCache | Přenos odchozích dat pro prostředky, které nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele na hranici.                |Ano   |Ne   |
| EgressCacheUncacheable | Odchozí datové přenosy pro prostředky, které nebudou moci ukládat do mezipaměti asset Cache-Control nebo Expires hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klient HTTP                    |Ano   | Ne  |
| EgressCacheOthers |  Odchozí datové přenosy s dalšími scénáři mezipaměti.             |Ano   | Ne  |

* Odchozí přenosy dat odkazuje na provoz klientovi předána ze serverů CDN POP.


### <a name="schema-of-the-core-analytics-logs"></a>Schéma základní analýzy protokolů 

Všechny protokoly se ukládají ve formátu JSON a každá položka má následující pole řetězce níže schématu:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Kde 'čas' představuje čas zahájení hodinu hranic, pro který je hlášen statistik. Pokud metriky není podporována zprostředkovatelem CDN, místo hodnotu double nebo celé číslo, bude mít hodnotu null. Tato hodnota null ukazuje na nepřítomnost metriky a tento proces se liší od hodnotu 0. Všimněte si také, že bude jednu sadu tyto metriky v každé doméně nakonfigurovaný v koncovém bodě.

Příklad vlastnosti níže:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Další zdroje

* [Azure diagnostické protokoly](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Základní analýza prostřednictvím doplňkovém portálu Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Analýzy protokolů Azure OMS](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [Analýza protokolů Azure rozhraní REST API](https://docs.microsoft.com/en-us/rest/api/loganalytics)







