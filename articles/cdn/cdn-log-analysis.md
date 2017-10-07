---
title: "Analýza aaaLog Azure CDN | Microsoft Docs"
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
ms.openlocfilehash: 56e5a4fec46fd156cf38252732afb4522741d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a>Diagnostické protokoly pro Azure CDN

Když povolíte CDN pro vaši aplikaci, bude pravděpodobně chcete toomonitor hello CDN využití, zkontrolujte stav hello vaší doručení a potenciální potíže. Azure CDN poskytuje tyto možnosti se [CDN základní analýza](cdn-analyze-usage-patterns.md) a [diagnostických protokolů](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)

## <a name="cdn-core-analytics"></a>Základní analýza CDN
Jako aktuální uživatel Azure CDN Verizon standard nebo premium profilu jste už moct tooview základní analýza hello doplňkovém portálu přístupné přes hello "Manage" možnost hello portálu Azure. 

## <a name="azure-diagnostic-logs"></a>Azure diagnostických protokolů

Azure pomocí této nové funkce, můžete nyní zobrazit analýzu základní a uložit je do jedné nebo více cílů, včetně:

 - Účet služby Azure Storage
 - Azure Event Hubs
 - [Úložiště analýzy protokolů OMS](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 Tato funkce je k dispozici pro všechny koncové body CDN patřící tooVerizon (Standard a Premium) a profilů CDN Akamai (Standard).

Diagnostické protokoly povolení metrik tooexport základní informace o využití z vaší CDN koncový bod tooa řady zdrojů tak, aby můžete využívat požadovaným způsobem. Například můžete provést následující typy dat export hello:

- Exportovat data tooblob úložiště, exportovat tooCSV a generovat grafy v aplikaci excel.
- Exportovat data tooevent rozbočovače a korelovat s daty z jiné služby azure.
- Exportovat data analýzy a zobrazení toolog dat ve vlastní pracovní prostor OMS

Hello následující obrázek ukazuje typické CDN základní analýza zobrazení na data.

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Obrázek 1 – základní analýza CDN zobrazení*

Následující postup Hello projde hello schéma hello základní analytická data, kroky při povolení funkce hello a jejich předání toovarious cíle a spotřebě z těchto cílů.

## <a name="enable-logging-with-azure-portal"></a>Povolit protokolování pomocí portálu Azure

> [!NOTE]
> Hello diagnostické protokoly jsou zapnuté **vypnout** ve výchozím nastavení. 

Postupujte podle kroků hello tooenable protokolování s CDN základní analýza:

Přihlaste se toohello [portál Azure](http://portal.azure.com). Pokud ještě nemáte CDN povolené pro pracovní postup [povolení Azure CDN](cdn-create-new-endpoint.md) než budete pokračovat.

1. Hello portálu, přejděte příliš**profil CDN**.
2. Vyberte profil CDN a pak vyberte hello koncový bod CDN, které chcete tooenable **protokolů diagnostiky**.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Přejděte příliš**protokolů diagnostiky** okno pod **monitorování** část, pak změňte stav hello příliš**na**.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Povolit protokolování s Azure Storage
    
protokoly hello toostore toouse Azure Storage, vyberte **archivu účet úložiště tooa**, vyberte dní uchovávání dat a klikněte na **CoreAnalytics** pod **protokolu**.

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*Obrázek 2 – protokolování s Azure Storage*

### <a name="logging-with-oms-log-analytics"></a>Protokolování s OMS analýzy protokolů

toouse analýzy protokolů OMS toostore hello protokoly, postupujte takto:

1. Z hello **protokolů diagnostiky** okno pod **monitorování**, vyberte **odeslat tooLog Analytics** z 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Konfigurace protokolování analýzy protokolů hello kliknutím na konfigurovat. Tím přejdete tooa dialogové okno, kde můžete vybrat předchozí pracovního prostoru nebo vytvořte novou.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Klikněte na tlačítko **vytvořit nový pracovní prostor**.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/07_Create-new.png)

4. Dále musíte vybrat nový název pracovního prostoru, stávající předplatné, skupinu prostředků (nová nebo stávající), umístění a cenovou úroveň. Máte možnost hello Připnutí tento řídicí panel tooyour konfigurace. Klikněte na tlačítko OK toocomplete hello konfigurace.

    Dále byste měli vidět pracovního prostoru s názvy skupiny pracovním prostorem OMS a prostředků. Názvy musí být jedinečný a použít pouze písmena, číslice a pomlčky. Nejsou povoleny mezery a podtržítka. 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. Zobrazí další krátkou zprávu oznamující, že váš prostor byl vytvořen a vrátíte se tooyour protokolování konfigurační obrazovce. Můžete potvrdit, hello název pracovního prostoru analýzy protokolů.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    Jakmile nastavíte konfigurace analýzy protokolů hello, ujistěte se, že zaškrtnete políčko CoreAnalytics hello CDN protokolování.

6. Pokud všechno, co je tooyour libosti, klikněte na tlačítko hello **Uložit** tlačítka v horní části hello dialogové okno Konfigurace hello.

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/10_Save-me.png)

    Hello **Uložit** tlačítko již není aktivní a že hello na nebo vypnutí teď ON, ale blue místo fialová.

7. Pokud chcete toosee nový pracovní prostor OMS, přejděte tooyour portál Azure řídicí panel, klikněte na název hello pracovní prostor analýzy protokolů. Dále uvidíte pracovního prostoru (ujistěte se, že pracovní prostor OMS je označený na levé straně hello). Klikněte na hello portálu OMS dlaždice toosee pracovního prostoru v úložišti OMS hello. 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Úložiště OMS je nyní připraven toolog data. V pořadí tooconsume dat, je nutné použít [OMS řešení](#consuming-oms-log-analytics-data), zahrnuté později v tomto článku.

Další informace o protokolu data zpoždění [zde](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Povolení protokolování v prostředí PowerShell

Dole je příklad na tom, jak hello tooenable a získání diagnostických protokolů prostřednictvím rutin prostředí Azure PowerShell.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Povolení diagnostických protokolů v účtu úložiště

Nejdřív se přihlaste a vyberte předplatné:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


tooEnable diagnostických protokolů v účtu úložiště, použijte tento příkaz:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
tooEnable protokolů diagnostiky v pracovním prostoru OMS, použijte tento příkaz:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Použití protokolů diagnostiky ze služby Azure Storage
Tato část popisuje schéma hello hello CDN základní analýza, jak tyto jsou uspořádány v rámci účtu úložiště Azure a poskytuje ukázkový kód toodownload hello protokolů v souboru CSV tooa.

### <a name="using-microsoft-azure-storage-explorer"></a>Pomocí Průzkumníka úložiště Microsoft Azure
Než se dostanete k hello základní analytická data z hello účet úložiště Azure, je nutné nejprve nástroj tooaccess hello obsah v účtu úložiště. Hello trhu jsou k dispozici několik nástrojů, i když je hello ten, který doporučujeme hello Microsoft Azure Storage Explorer. Si můžete stáhnout z nástroj hello [zde](http://storageexplorer.com/). Po stažení a instalace softwaru hello konfigurace toouse hello stejný účet úložiště Azure, který byl nakonfigurovaný jako cílový toohello CDN diagnostické protokoly.

1.  Otevřete **Microsoft Azure Storage Explorer**
2.  Najděte účet úložiště hello
3.  Přejděte toohello **"Kontejnery objektů Blob"** uzel v rámci toto úložiště účtu a rozbalte uzel hello
4.  Vyberte hello kontejner s názvem **"insights-logs-coreanalytics"** a poklikejte na něj
5.  Zobrazit výsledky až na hello pravém podokně počínaje hello první úrovně, který vypadá podobně jako **"resourceId ="**. Pokračujte kliknutím na všechny hello způsobem, dokud neuvidíte hello souboru **PT1H.json**. V tématu hello následující poznámka vysvětlení hello cesty.
6.  Každý objekt blob **PT1H.json** představuje hello analýzy protokolů pro jednu hodinu pro konkrétní koncový bod CDN nebo jeho vlastní doménu.
7.  schéma Hello hello obsah tohoto souboru JSON je popsané v sekci hello schématu hello základní analýzy protokolů


> [!NOTE]
> **Formát cesty objektu BLOB**
> 
> Základní analýza protokolů jsou generovány, každou hodinu. Všechna data pro jednu hodinu jsou shromážděných a uložených do jediného objektu Blob Azure jako datové části JSON. Hello cesta toothis objektů Blob v Azure se zobrazí, jako kdyby je hierarchická struktura. Toto je vzhledem k tomu, že nástroj Průzkumník úložiště hello interpretuje '/' za oddělovač adresářů a zobrazuje hierarchii hello ke zvýšení pohodlí. Ve skutečnosti celou cestu hello právě představuje název objektu blob hello. Tento název objektu hello blob následuje hello následující zásady vytváření názvů 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Popis polí:**

|hodnota|description|
|-------|---------|
|ID předplatného    |ID hello předplatné Azure. Toto je ve formátu Guid hello.|
|Prostředek |Název skupiny prostředků hello prostředků skupiny toowhich hello CDN patří.|
|Název profilu |Název hello profil CDN|
|Název koncového bodu |Název hello koncový bod CDN|
|Rok|  reprezentace 4 číslice roku hello například 2017|
|Měsíc| 2 číslice reprezentace číslo měsíce hello. 01 = leden... 12 = prosinec|
|Den|   2 číslice reprezentace hello den v měsíci hello|
|PT1H.JSON| Skutečný soubor JSON se uloží hello analytická data|

### <a name="exporting-hello-core-analytics-data-tooa-csv-file"></a>Export hello základní analytická Data tooa souboru CSV

toomake it snadno tooaccess hello základní analýza poskytujeme ukázkový kód pro nějaký nástroj, který umožňuje stahování souborů hello JSON do souboru nestrukturované oddělený čárkami ve formátu, který lze použít tooeasily vytvořit grafy nebo jiných agregací.

Zde je, jak můžete použít nástroj hello:

1.  Po kliknutí hello githubu odkaz: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  Stáhněte si kód hello
3.  Postupujte podle pokynů toocompile a konfigurace
4.  Spusťte nástroj hello
5.  Výsledný soubor CSV ukazuje hello analytická data v jednoduchých ploché hierarchii.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Použití protokolů diagnostiky z úložiště analýzy protokolů OMS
Analýzy protokolů je služba v Operations Management Suite (OMS), která monitoruje vaše cloudové a místní prostředí toomaintain jejich dostupnost a výkon. Shromáždí data generována prostředky ve vašich cloudových a místních prostředích a z dalších monitorování tooprovide analysis nástroje napříč více zdrojů. 

toouse analýzy protokolů, musíte [povolit protokolování](#enable-logging-with-azure-storage) úložiště analýzy protokolů Azure OMS toohello, které je popsané výše v tomto článku.

### <a name="using-hello-oms-repository"></a>Pomocí hello OMS úložiště

 Následující diagram ukazuje hello architektura hello vstupy a výstupy úložiště hello Hello:

![Úložiště analýzy protokolů OMS](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Obrázek 3 - úložiště analýzy protokolů*

Hello data můžete zobrazit v mnoha různými způsoby pomocí řešení pro správu. Řešení pro správu můžete získat z hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Řešení pro správu můžete nainstalovat z Azure marketplace kliknutím hello **ho získat** odkaz dole hello v jednotlivých řešení.

### <a name="adding-an-oms-cdn-management-solution"></a>Přidávání do řešení pro správu OMS CDN

Postupujte podle těchto kroků tooadd řešení pro správu:

1.   Pokud jste tak již neučinili, přihlaste se toohello portálu Azure pomocí svého předplatného Azure a přejděte tooyour řídicího panelu.
    ![Řídicí panel Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. V hello **nový** okno pod **Marketplace**, vyberte **monitorování + správu**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. V hello **monitorování + správu** okně klikněte na tlačítko **zobrazit všechny**.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/15_See-all.png)

4.  Vyhledejte CDN hello vyhledávacího pole.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Vyberte **Azure CDN základní analýza**. 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  Po kliknutí na **vytvořit**, nebudete vyzváni toocreate nový pracovní prostor OMS nebo použijte existující. 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Vyberte pracovní prostor hello, kterou jste vytvořili před. Pak musíte tooadd účet automation.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Hello následující obrazovka ukazuje hello automatizace účet formuláře, které je nutné vyplnit. 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/20_Automation.png)

9. Po vytvoření účtu automation hello jste připravené tooadd řešení. Klikněte na tlačítko hello **vytvořit** tlačítko.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/21_Ready.png)

10. Řešení byl přidán tooyour prostoru. Vraťte se zpátky tooyour portál Azure řídicího panelu.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/22_Dashboard.png)

    Klikněte na pracovní prostor analýzy protokolů hello, které jste vytvořili toogo tooyour prostoru. 

11. Klikněte na tlačítko hello **portálu OMS** dlaždici toosee nové řešení na portálu OMS hello.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/23_workspace.png)

12. Na portálu OMS by teď měl vypadat jako hello následující obrazovka:

    ![Zobrazit všechno](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Klikněte na jednu z hello dlaždice toosee několik zobrazení na vaše data.

    ![Zobrazit všechno](./media/cdn-diagnostics-log/25_Interior-view.png)

    Se posunete doleva nebo další správné toosee dlaždice představující jednotlivé zobrazení na hello data. 

    Kliknutím na jedno hello dlaždic vám dává další podrobnosti o vaše data.

     ![Zobrazit všechno](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Nabídky a cenové úrovně

Můžete zobrazit nabídky a cenové úrovně pro řešení pro správu OMS [zde](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Přizpůsobení zobrazení

Můžete přizpůsobit zobrazení hello do vašich dat pomocí hello **Návrhář zobrazení**. Přejděte na pracovní prostor OMS tooyour a začnete navrhovat kliknutím hello **Návrhář zobrazení** dlaždici.

![Návrhář zobrazení](./media/cdn-diagnostics-log/27_Designer.png)

Můžete přetáhnout a vyřadit typy grafů zleva hello a vyplňte informace o datových hello chcete tooanalyze na zbývajících hello.

![Návrhář zobrazení](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Zpoždění data protokolu

Verizon protokolu data zpoždění | Akamai protokolu data zpoždění
--- | ---
Data protokolu Verizon je zpožděno 1 hodinu a trvat až toostart hodin too2 zobrazování po dokončení šíření koncový bod. | Data protokolu Akamai je 24 hodin, zpoždění a zabírají toostart hodin too2 zobrazování, pokud byla vytvořena více než 24 hodinami. Pokud byl nedávno vytvořen, může trvat až too25 hodin toostart hello protokoly, které jsou uvedeny.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Typy protokolů diagnostiky pro CDN základní analýza

Nabízíme aktuálně pouze základní analýza protokoly, které obsahují metriky ukazující statistiky odpovědi HTTP a odchozí statistiky, jak je vidět z hello CDN POP nebo okraje.

### <a name="core-analytics-metrics-details"></a>Základní analýza metriky podrobnosti
Hello následující tabulka obsahuje seznam metriky, které jsou k dispozici v hello základní Analýza protokolů. Ne všechny metriky jsou k dispozici od všech poskytovatelů, i když tyto rozdíly jsou minimální. Hello také následující tabulka ukazuje, pokud daná metrika je k dispozici od zprostředkovatele. Upozorňujeme, že jsou k dispozici pro jenom ty koncové body CDN mající přenosy na těchto hello metriky.


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
| RequestCountCacheHit |Počet všech požadavků, jejichž výsledkem požadavků mezipaměti. To znamená, že zpracování hello asset přímo z hello POP toohello klienta.               | Ano  |Ne   |
| RequestCountCacheMiss | Počet všech požadavků, která byla vygenerována v neúspěšnému přístupu do mezipaměti. To znamená hello asset nebyl nalezen na hello POP nejbližší toohello klienta a proto byla načtena z hello původu.              |Ano   | Ne  |
| RequestCountCacheNoCache | Počet všech požadavků tooan asset, který nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele tooa hranu hello.              |Ano   | Ne  |
| RequestCountCacheUncacheable | Počet všech požadavků tooassets, která nebudou moci ukládat do mezipaměti podle hello asset Cache-Control a vyprší platnost hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klientem hello HTTP                |Ano   |Ne   |
| RequestCountCacheOthers | Počet všech požadavků stavem mezipaměti, které nejsou pokryty výše.              |Ano   | Ne  |
| EgressTotal | Odchozí přenosy dat v GB              |Ano   |Ano   |
| EgressHttpStatus2xx | Odchozí datové přenosy * pro odpovědi s stavové kódy HTTP 2xx v GB            |Ano   |Ne   |
| EgressHttpStatus3xx | Přenos odchozích dat pro odpovědi s stavové kódy HTTP 3xx v GB              |Ano   |Ne   |
| EgressHttpStatus4xx | Přenos odchozích dat pro odpovědi s stavové kódy HTTP 4xx v GB               |Ano   | Ne  |
| EgressHttpStatus5xx | Přenos odchozích dat pro odpovědi s stavové kódy HTTP 5xx v GB               |Ano   |  Ne |
| EgressHttpStatusOthers | Odchozí přenosy dat pro odpovědi s ostatních stavových kódech HTTP v GB                |Ano   |Ne   |
| EgressCacheHit |  Odchozí přenos dat pro odpovědi, které byly doručeny přímo z mezipaměti CDN hello na hello CDN POP od okrajů  |Ano   |  Ne |
| EgressCacheMiss | Přenos odchozích dat pro odpovědi, které nebyly na hello nejbližší POP server najít a načíst ze serveru původu hello              |Ano   |  Ne |
| EgressCacheNoCache | Odchozí datové přenosy pro prostředky, které nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele tooa hranu hello.                |Ano   |Ne   |
| EgressCacheUncacheable | Odchozí datové přenosy pro prostředky, které nebudou moci ukládat do mezipaměti hello asset Cache-Control nebo Expires hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klientem hello HTTP                    |Ano   | Ne  |
| EgressCacheOthers |  Odchozí datové přenosy s dalšími scénáři mezipaměti.             |Ano   | Ne  |

* Odchozí přenosy dat odkazuje tootraffic doručit od CDN POP servery toohello klienta.


### <a name="schema-of-hello-core-analytics-logs"></a>Schéma hello základní analýzy protokolů 

Všechny protokoly se ukládají ve formátu JSON a každá položka má následující hello níže schématu polí s řetězcem:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of hello CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of hello domain for which hello statistics is reported>",
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

Kde hello čas představuje čas zahájení hello hello hodinu hranic, pro který je hlášen hello statistiky. Pokud metriky není podporována zprostředkovatelem CDN, místo hodnotu double nebo celé číslo, bude mít hodnotu null. Tato hodnota null znamená hello absenci metriky a tento proces se liší od hodnotu 0. Všimněte si také, že bude jednu sadu tyto metriky na doménu na koncový bod hello nakonfigurovaný.

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







