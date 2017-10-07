---
title: "aaaCreate výstrahy pro služby Azure - portálu Azure | Microsoft Docs"
description: "Při splnění zadané podmínky hello aktivaci e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace."
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
ms.openlocfilehash: 78d862d25255cda9fdfe347329e908a471c39846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Vytvoření metriky výstrah v monitorování Azure pro služby Azure – portál Azure
> [!div class="op_single_selector"]
> * [Azure Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Rozhraní příkazového řádku](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak hello tooset si Azure metriky výstrah pomocí portálu Azure.   

Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.

* **Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech. To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.    
* **Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události. Další informace o protokolu upozornění toolearn [, klikněte sem](monitoring-activity-log-alerts.md)

Můžete nakonfigurovat následující hello metriky toodo výstrahy, když ji spustí:

* odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci
* odesílání e-mailů tooadditional e-mailů, které zadáte.
* Volat webhook, jehož
* Spusťte provádění runbook služby Azure (pouze z hello portál Azure)

Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [rozhraní příkazového řádku (CLI)](insights-alerts-command-line-interface.md)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a>Vytvořit pravidlo výstrahy na metrika s hello portálu Azure
1. V hello [portál](https://portal.azure.com/), vyhledejte hello prostředků, které vás zajímají monitorování a vyberte ho.

2. Vyberte **výstrahy** nebo **výstrah pravidla** pod hello části monitorování. Ikona a textu Hello se mohou mírně lišit pro různé prostředky.  

    ![Monitorování](./media/insights-alerts-portal/AlertRulesButton.png)

3. Vyberte hello **přidat upozornění** příkazů a vyplňte pole hello.

    ![Přidání oznámení](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.

5. Vyberte hello **metrika** toomonitor a pak vyberte **podmínku** a **prahová hodnota** hodnotu metriky hello. Také si zvolili hello **období** času, který hello metrika pravidlo je nutné splnit před hello výstrahy aktivační události. Tak například pokud používáte období hello "PT5M" a upozornění hledá procesoru vyšší než 80 %, hello výstraha se aktivuje při hello procesoru bylo konzistentně výše 80 % 5 minut. Jakmile dojde k první aktivační událost hello, se znovu spustí, když hello procesoru zůstává nižší než 80 % 5 minut. Hello procesoru měření dochází každých 1 minuta.   

6. Zkontrolujte **e-mailu vlastníky...**  Pokud chcete, aby správci a spolusprávci toobe e-mailem při hello výstrahy aktivuje.

7. Pokud chcete další e-mailů tooreceive oznámení, když hello výstrahy je aktivována, je přidat v hello **email(s) další správce** pole. Oddělte více e-mailů středníky -  *email@contoso.com;email2@contoso.com*

8. Umístit do platný identifikátor URI v hello **Webhooku** pole Pokud se má volat při hello výstrahy aktivuje.

9. Pokud používáte Azure Automation, můžete vybrat Runbook toobe, spustit, když se aktivuje výstraha hello.

10. Vyberte **OK** při done toocreate hello výstrahy.   

Během několika minut hello výstraha je aktivní a jak se popisuje výš se aktivuje.

## <a name="managing-your-alerts"></a>Správa upozornění
Po vytvoření výstrahy, můžete ji vybrat a:

* Zobrazte graf zobrazující hello metriky prahovou hodnotu a hello skutečnými hodnotami z hello předchozí den.
* Upravit nebo odstranit.
* **Zakázat** nebo **povolit** ho chcete-li zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy.

## <a name="next-steps"></a>Další kroky
* [Získat přehled o Azure monitorování](monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.
* Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).
* Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).
* Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Získat [přehled diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) a shromažďovat podrobné metriky vysoká frekvence vaší služby.
* Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
