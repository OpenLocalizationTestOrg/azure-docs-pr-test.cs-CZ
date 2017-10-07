---
title: "aaaCreate výstrahy pro služby Azure - napříč platformami rozhraní příkazového řádku | Microsoft Docs"
description: "Při splnění zadané podmínky hello aktivaci e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: e53701e5377a415038a69fbd32f1e5fc5fe99be9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a>Vytvoření metriky výstrah v monitorování Azure pro služby Azure - napříč platformami rozhraní příkazového řádku
> [!div class="op_single_selector"]
> * [Azure Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Rozhraní příkazového řádku](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak tooset si Azure metriky výstrah pomocí hello napříč platformami rozhraní příkazového řádku (CLI).

> [!NOTE]
> Azure monitorování je hello nový název pro co byla volána "Statistika Azure" až 25 září 2016. Ale hello obory názvů a proto níže uvedené příkazy hello stále obsahovat hello "insights".
>
>

Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.

* **Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech. To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.    
* **Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události. Další informace o protokolu upozornění toolearn [, klikněte sem](monitoring-activity-log-alerts.md)

Můžete nakonfigurovat následující hello metriky toodo výstrahy, když ji spustí:

* odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci
* odesílání e-mailů tooadditional e-mailů, které zadáte.
* Volat webhook, jehož
* Spusťte provádění runbook služby Azure (pouze z portálu Azure v tuto chvíli hello)

Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [rozhraní příkazového řádku (CLI)](insights-alerts-command-line-interface.md)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Vždy dostanete nápovědy pro příkazy zadáním příkazu a - pomoci na konci hello. Například:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-hello-cli"></a>Vytvořit pravidla výstrah pomocí hello rozhraní příkazového řádku
1. Proveďte hello požadavky a tooAzure přihlášení. V tématu [ukázky rozhraní příkazového řádku Azure monitorování](insights-cli-samples.md). Stručně řečeno nainstalujte hello rozhraní příkazového řádku a spuštění těchto příkazů. Jejich získat jste přihlášeni, jaké předplatné a připravit se příkazy Azure monitorování toorun zobrazit.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. toolist existující pravidla na skupinu prostředků, použijte následující formulář hello **statistiky azure výstrahy pravidlo seznamu** *[možnosti] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. toocreate pravidlo, budete potřebovat toohave několik důležitých informací nejdřív.
  * Hello **ID prostředku** pro prostředek hello chcete tooset výstrahu pro
  * Hello **definice metrik** k dispozici pro tento prostředek

     Jedním ze způsobů tooget hello ID prostředku je toouse hello portálu Azure. Za předpokladu, že již není vytvořen hello prostředků, vyberte ho v portálu hello. V dalším okně hello vyberte *vlastnosti* pod hello *nastavení* části. Hello *ID prostředku* je pole v dalším okně hello. Další možností je toouse hello [Průzkumníka prostředků Azure](https://resources.azure.com/).

     Je třeba id prostředků pro webovou aplikaci

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     tooget seznam hello k dostupným metrikám a jednotky pro tyto metriky pro hello předchozí příklad prostředků, hello použijte následující příkaz rozhraní příkazového řádku:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* je hello členitost hello k dispozici měření (1-minutách). Pomocí různých členitostí nabízí různé možnosti metriky.
4. toocreate základě metrika pravidlo výstrahy, použijte příkaz hello následující formulář:

    **metriky sadu pravidel výstrahy statistiky Azure** *[možnosti] &lt;ruleName&gt; &lt;umístění&gt; &lt;resourceGroup&gt; &lt;velikost_okna &gt; &lt;operátor&gt; &lt;prahová hodnota&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt; timeAggregationOperator&gt;*

    Následující příklad sady si výstrahy na prostředku webu Hello. Hello výstrahy aktivuje vždy, když obdrží veškerou komunikaci konzistentně 5 minut a opakujte při přijetí žádný provoz 5 minut.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. toocreate webhooku nebo odesílání e-mailu při aktivaci se metriky výstrahy, se nejprve vytvořit hello e-mailu nebo webhooky. Vytvořit pravidlo hello okamžitě později. Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí hello rozhraní příkazového řádku.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Můžete ověřit, zda byly upozornění správně vytvořeny prohlížením jednotlivých pravidel.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. pravidla toodelete příkazem hello formuláře:

    **Odstranit pravidlo výstrahy statistiky** [možnosti] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Tyto příkazy odstranit pravidla hello dříve vytvořené v tomto článku.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Další kroky
* [Získat přehled o Azure monitorování](monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.
* Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).
* Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).
* Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Získat [přehled shromažďování diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md) toocollect podrobné vysoká frekvence metriky pro vaši službu.
* Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
