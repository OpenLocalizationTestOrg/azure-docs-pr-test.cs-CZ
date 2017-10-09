---
title: "sady škálování virtuálního počítače Azure škálování aaaVertically | Microsoft Docs"
description: "Jak toovertically škálování virtuálního počítače v odpovědi toomonitoring výstrahy s Azure Automation."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: madhana
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: guybo
ms.openlocfilehash: 1cc35a805b6a5742252a89c21588ca451ff547a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Svislé škálování s sady škálování virtuálního počítače
Tento článek popisuje, jak toovertically škálování Azure [sady škálování virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/) s nebo bez reprovisioning. Svislé škálování virtuálních počítačů, které nejsou v sadách škálování, najdete v části příliš[svisle škálování virtuální počítač Azure s Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Svislé škálování, také známé jako *škálovat* a *snižovat*, znamená zvýšením nebo snížením velikosti virtuálních počítačů (VM) v odpovědi tooa zatížení. Výsledky porovnejte s [vodorovné škálování](virtual-machine-scale-sets-autoscale-overview.md), nazývaná také jen tooas *škálovat* a *škálování v*, kde je v závislosti na zatížení hello ovlivněny hello počet virtuálních počítačů.

Reprovisioning znamená odebrat existující virtuální počítač a jeho nahrazení novou. Když zvětšit nebo zmenšit velikost hello virtuální počítače ve Škálovací sadě virtuálních počítačů, v některých případech je chcete tooresize existující virtuální počítače a zachovat data, zatímco v ostatních případech musíte toodeploy nové virtuální počítače nové velikosti hello. Tento dokument popisuje obou případech.

Svislé škálování může být užitečné, když:

* Služba založený na virtuální počítače je v části využívat (třeba na víkendů). Zmenšení velikosti virtuálního počítače hello může snížit náklady na měsíční.
* Zvýšení toocope velikost virtuálního počítače s větší vyžádání bez vytvoření dalších virtuálních počítačů.

Můžete nastavit vertical škálování toobe aktivaci na základě metriky na základě výstrah ze sady škálování virtuálního počítače. Když je aktivován hello výstraha aktivuje webhook, jehož této aktivační události, které se nastavit sadu runbook, které můžete škálovat vaše škálování nahoru nebo dolů. Svislé škálování lze nastavit pomocí následujících kroků:

1. Vytvořte účet Azure Automation se schopností spustit jako.
2. Importování sad runbook vertikální Škálováním Azure Automation pro škálovatelné sady virtuálních počítačů do vašeho předplatného.
3. Přidáte runbook tooyour webhooku.
4. Přidáte výstrahy tooyour Škálovací sady virtuálního počítače pomocí webhooku oznámení.

> [!NOTE]
> Svislé automatické škálování se v určitém rozsahu velikostí virtuálních počítačů. Porovnání hello specifikace velikosti jednotlivých před rozhodnutím tooscale z jednoho tooanother (vyšší počet vždy neindikuje větší velikost virtuálního počítače). Můžete zvolit tooscale mezi hello následující páry velikosti:
> 
> | Škálování pár velikosti virtuálních počítačů |  |
> | --- | --- |
> | Standard_A0 |Standard_A11 |
> | Standard_D1 |Standard_D14 |
> | Standard_DS1 |Standard_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standard_G1 |Na úrovni Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Vytvořit účet Azure Automation s možností Spustit jako
První věc Hello potřebujete toodo je, vytvořte účet Azure Automation, který bude hostitelem hello sady runbook používá tooscale hello Škálovací sady virtuálního počítače instancí. Nedávno [Azure Automation](https://azure.microsoft.com/services/automation/) zavedená "Účet Spustit jako" funkci hello, která usnadňuje nastavení hello instanční objekt automaticky spouští sady runbook hello jménem uživatele velmi snadné. Další informace najdete v článku hello níže:

* [Ověření runbooků pomocí účtu Spustit v Azure jako](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importování sad runbook Azure Automation vertikální Škálováním do předplatného
sady runbook Hello potřeba toovertically měřítka, které vaše škálovatelné sady virtuálních počítačů jsou již publikován v hello Galerie Runbooků automatizace Azure. tooimport je do vašeho předplatného použijte hello kroky v tomto článku:

* [Galerie Runbooků a modulů pro Azure Automation.](../automation/automation-runbook-gallery.md)

Vyberte možnost Procházet galerii hello nabídce hello sady Runbook:

![Importovat toobe sady Runbook][runbooks]

Hello sady runbook, které je třeba importovat toobe se zobrazí. Vyberte sadu runbook hello založené na tom, zda chcete svislé škálování s nebo bez reprovisioning:

![Galerie Runbooků][gallery]

## <a name="add-a-webhook-tooyour-runbook"></a>Přidat runbook tooyour webhooku
Jakmile importujete sady runbook hello budete potřebovat tooadd webhooku toohello runbook, mohou být aktivovány výstrahy ze sady škálování virtuálního počítače. Hello podrobnosti o vytváření webhooku pro své sadě Runbook jsou popsané v tomto článku:

* [Webhooky Azure Automation.](../automation/automation-webhooks.md)

> [!NOTE]
> Ujistěte se, že zkopírujete hello webhooku URI před jeho zavřením hello webhooku dialogu, jak je budete potřebovat v další části hello.
> 
> 

## <a name="add-an-alert-tooyour-vm-scale-set"></a>Přidat výstrahy tooyour Škálovací sady virtuálního počítače
Níže je skript prostředí PowerShell, který ukazuje způsob tooadd výstrahy tooa Škálovací sady virtuálního počítače. Odkazovat toohello následující název hello tooget článku hello metriky toofire hello výstrahy: [běžné metriky automatického škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> Je doporučeno tooconfigure přiměřené časový interval pro upozornění hello tooavoid pořadí spouštění svislé škálování a všech přidružených přerušení služby, příliš často. Vezměte v úvahu okno minimálně 20-30 minut nebo déle. Vezměte v úvahu horizontální škálování, pokud potřebujete tooavoid žádné přerušení.
> 
> 

Další informace o tom, jak toocreate výstrahy naleznete toohello následující články:

* [Ukázek Azure PowerShell monitorování rychlý start](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Ukázek Azure rychlý start monitorování napříč platformami rozhraní příkazového řádku](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Souhrn
Tento článek vám ukázal, jednoduché příklady svislé škálování. Pomocí těchto stavební bloky – účet Automation, sady runbook, webhooků, výstrahy – můžete připojit mnoho různých událostí s vlastní sadu akcí.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
