---
title: "Vytvořte výstrahy pro služby Azure - portálu Azure | Microsoft Docs"
description: "Aktivační událost e-mailů, oznámení, weby adresy URL (webhooky), nebo volat automatizace při splnění zadané podmínky."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 3e09c145d35665ec1c2467b60f06191ac51a5c16
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Vytvoření metriky výstrah v monitorování Azure pro služby Azure – portál Azure
> [!div class="op_single_selector"]
> * [Azure Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Rozhraní příkazového řádku](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak nastavit Azure metriky výstrah pomocí portálu Azure. 

Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.

* **Metriky hodnoty** -výstrahy aktivuje, když hodnota zadané metriky překračuje prahovou hodnotu přiřadíte v obou směrech. To znamená, aktivuje obě při nejprve je splněna podmínka, a pak později, pokud podmínka je už plněny.    
* **Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události. Další informace o [aktivity protokolu výstrahy](monitoring-activity-log-alerts.md).

Můžete nakonfigurovat metriky výstrahu při aktivaci, proveďte následující:

* odesílat oznámení e-mailu Správce služeb a spolusprávci
* Odeslat e-mail na další e-mailů, které zadáte.
* Volat webhook, jehož
* Spusťte provádění runbook služby Azure (pouze z portálu Azure)

> [!NOTE]
> Monitorování Azure nyní podporuje téměř v reálném čase metrika výstrahy ve verzi public preview. Tyto akce skupiny použít. Další informace o [téměř v reálném čase metrika výstrahy](monitoring-near-real-time-metric-alerts.md).
>
>

Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah

* [portál Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [rozhraní příkazového řádku (CLI)](insights-alerts-command-line-interface.md)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Vytvořit pravidlo výstrahy na metrika pomocí portálu Azure
1. V [portál](https://portal.azure.com/), vyhledejte prostředků, které vás zajímají monitorování a vyberte ho.

2. Vyberte **výstrahy** nebo **výstrah pravidla** části monitorování. Text a ikona se mohou mírně lišit pro různé prostředky.  

    ![Monitorování](./media/insights-alerts-portal/AlertRulesButton.png)

3. Vyberte **přidat upozornění** příkazů a vyplňte příslušná pole.

    ![Přidání oznámení](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.

5. Vyberte **metrika** chcete monitorovat, a potom vyberte **podmínku** a **prahová hodnota** hodnoty pro metriku. Taky **období** času, který metriky pravidlo je nutné splnit před výstrahy aktivačních událostí. Tak například pokud používáte období "za posledních 5 minut" a upozornění hledá procesoru vyšší než 80 %, výstrahy se aktivuje při procesoru bylo konzistentně výše 80 % 5 minut. Jakmile dojde k první aktivační událost, se znovu spustí, když procesoru zůstává nižší než 80 % 5 minut. Metriky měření využití procesoru nastane každou minutu.

6. Zkontrolujte **e-mailu vlastníky...**  Pokud chcete, aby správci a spolusprávci se má při aktivuje výstrahu.

7. Pokud chcete další e-mailů příjmu oznámení, když se aktivuje výstraha, přidejte je do **email(s) další správce** pole. Oddělte více e-mailů středníky -  *email@contoso.com;email2@contoso.com*

8. Umístit do platný identifikátor URI, **Webhooku** pole, pokud chcete, aby je volána, když se aktivuje výstrahu.

9. Pokud používáte Azure Automation, můžete vybrat Runbook a spustit, když se aktivuje výstrahu.

10. Vyberte **OK** po dokončení vytvoření výstrahy.   

Během několika minut výstraha je aktivní a jak se popisuje výš se aktivuje.

## <a name="managing-your-alerts"></a>Správa upozornění
Po vytvoření výstrahy, můžete ji vybrat a:

* Zobrazte graf zobrazující prahovou hodnotu metriky a skutečnými hodnotami z předchozí den.
* Upravit nebo odstranit.
* **Zakázat** nebo **povolit** ji, pokud chcete dočasně zastavit nebo obnovit příjem oznámení pro výstrahy.

## <a name="next-steps"></a>Další kroky
* [Získat přehled o Azure monitorování](monitoring-overview.md) včetně typy informací, můžete sledovat a shromažďovat.
* Další informace o nové [téměř v reálném čase metriky výstrahy (preview)](monitoring-near-real-time-metric-alerts.md)
* Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).
* Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).
* Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Získat [přehled diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) a shromažďovat podrobné metriky vysoká frekvence vaší služby.
* Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.
