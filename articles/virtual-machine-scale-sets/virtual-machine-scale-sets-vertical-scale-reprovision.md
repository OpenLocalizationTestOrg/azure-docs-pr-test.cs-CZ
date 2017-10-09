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
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a><span data-ttu-id="b17b0-103">Svislé škálování s sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b17b0-103">Vertical autoscale with Virtual Machine Scale sets</span></span>
<span data-ttu-id="b17b0-104">Tento článek popisuje, jak toovertically škálování Azure [sady škálování virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/) s nebo bez reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="b17b0-104">This article describes how toovertically scale Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) with or without reprovisioning.</span></span> <span data-ttu-id="b17b0-105">Svislé škálování virtuálních počítačů, které nejsou v sadách škálování, najdete v části příliš[svisle škálování virtuální počítač Azure s Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b17b0-105">For vertical scaling of VMs which are not in scale sets, refer too[Vertically scale Azure virtual machine with Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="b17b0-106">Svislé škálování, také známé jako *škálovat* a *snižovat*, znamená zvýšením nebo snížením velikosti virtuálních počítačů (VM) v odpovědi tooa zatížení.</span><span class="sxs-lookup"><span data-stu-id="b17b0-106">Vertical scaling, also known as *scale up* and *scale down*, means increasing or decreasing virtual machine (VM) sizes in response tooa workload.</span></span> <span data-ttu-id="b17b0-107">Výsledky porovnejte s [vodorovné škálování](virtual-machine-scale-sets-autoscale-overview.md), nazývaná také jen tooas *škálovat* a *škálování v*, kde je v závislosti na zatížení hello ovlivněny hello počet virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b17b0-107">Compare this with [horizontal scaling](virtual-machine-scale-sets-autoscale-overview.md), also referred tooas *scale out* and *scale in*, where hello number of VMs is altered depending on hello workload.</span></span>

<span data-ttu-id="b17b0-108">Reprovisioning znamená odebrat existující virtuální počítač a jeho nahrazení novou.</span><span class="sxs-lookup"><span data-stu-id="b17b0-108">Reprovisioning means removing an existing VM and replacing it with a new one.</span></span> <span data-ttu-id="b17b0-109">Když zvětšit nebo zmenšit velikost hello virtuální počítače ve Škálovací sadě virtuálních počítačů, v některých případech je chcete tooresize existující virtuální počítače a zachovat data, zatímco v ostatních případech musíte toodeploy nové virtuální počítače nové velikosti hello.</span><span class="sxs-lookup"><span data-stu-id="b17b0-109">When you increase or decrease hello size of VMs in a VM Scale Set, in some cases you want tooresize existing VMs and retain your data, while in other cases you need toodeploy new VMs of hello new size.</span></span> <span data-ttu-id="b17b0-110">Tento dokument popisuje obou případech.</span><span class="sxs-lookup"><span data-stu-id="b17b0-110">This document covers both cases.</span></span>

<span data-ttu-id="b17b0-111">Svislé škálování může být užitečné, když:</span><span class="sxs-lookup"><span data-stu-id="b17b0-111">Vertical scaling can be useful when:</span></span>

* <span data-ttu-id="b17b0-112">Služba založený na virtuální počítače je v části využívat (třeba na víkendů).</span><span class="sxs-lookup"><span data-stu-id="b17b0-112">A service built on virtual machines is under-utilized (for example at weekends).</span></span> <span data-ttu-id="b17b0-113">Zmenšení velikosti virtuálního počítače hello může snížit náklady na měsíční.</span><span class="sxs-lookup"><span data-stu-id="b17b0-113">Reducing hello VM size can reduce monthly costs.</span></span>
* <span data-ttu-id="b17b0-114">Zvýšení toocope velikost virtuálního počítače s větší vyžádání bez vytvoření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b17b0-114">Increasing VM size toocope with larger demand without creating additional VMs.</span></span>

<span data-ttu-id="b17b0-115">Můžete nastavit vertical škálování toobe aktivaci na základě metriky na základě výstrah ze sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b17b0-115">You can set up vertical scaling toobe triggered based on metric based alerts from your VM Scale Set.</span></span> <span data-ttu-id="b17b0-116">Když je aktivován hello výstraha aktivuje webhook, jehož této aktivační události, které se nastavit sadu runbook, které můžete škálovat vaše škálování nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="b17b0-116">When hello alert is activated it fires a webhook that triggers a runbook which can scale your scale set up or down.</span></span> <span data-ttu-id="b17b0-117">Svislé škálování lze nastavit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b17b0-117">Vertical scaling can be configured by following these steps:</span></span>

1. <span data-ttu-id="b17b0-118">Vytvořte účet Azure Automation se schopností spustit jako.</span><span class="sxs-lookup"><span data-stu-id="b17b0-118">Create an Azure Automation account with run-as capability.</span></span>
2. <span data-ttu-id="b17b0-119">Importování sad runbook vertikální Škálováním Azure Automation pro škálovatelné sady virtuálních počítačů do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="b17b0-119">Import Azure Automation Vertical Scale runbooks for VM Scale Sets into your subscription.</span></span>
3. <span data-ttu-id="b17b0-120">Přidáte runbook tooyour webhooku.</span><span class="sxs-lookup"><span data-stu-id="b17b0-120">Add a webhook tooyour runbook.</span></span>
4. <span data-ttu-id="b17b0-121">Přidáte výstrahy tooyour Škálovací sady virtuálního počítače pomocí webhooku oznámení.</span><span class="sxs-lookup"><span data-stu-id="b17b0-121">Add an alert tooyour VM Scale Set using a webhook notification.</span></span>

> [!NOTE]
> <span data-ttu-id="b17b0-122">Svislé automatické škálování se v určitém rozsahu velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b17b0-122">Vertical autoscaling can only take place within certain ranges of VM sizes.</span></span> <span data-ttu-id="b17b0-123">Porovnání hello specifikace velikosti jednotlivých před rozhodnutím tooscale z jednoho tooanother (vyšší počet vždy neindikuje větší velikost virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="b17b0-123">Compare hello specifications of each size before deciding tooscale from one tooanother (higher number does not always indicate bigger VM size).</span></span> <span data-ttu-id="b17b0-124">Můžete zvolit tooscale mezi hello následující páry velikosti:</span><span class="sxs-lookup"><span data-stu-id="b17b0-124">You can choose tooscale between hello following pairs of sizes:</span></span>
> 
> | <span data-ttu-id="b17b0-125">Škálování pár velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b17b0-125">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="b17b0-126">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="b17b0-126">Standard_A0</span></span> |<span data-ttu-id="b17b0-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="b17b0-127">Standard_A11</span></span> |
> | <span data-ttu-id="b17b0-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="b17b0-128">Standard_D1</span></span> |<span data-ttu-id="b17b0-129">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="b17b0-129">Standard_D14</span></span> |
> | <span data-ttu-id="b17b0-130">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="b17b0-130">Standard_DS1</span></span> |<span data-ttu-id="b17b0-131">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="b17b0-131">Standard_DS14</span></span> |
> | <span data-ttu-id="b17b0-132">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="b17b0-132">Standard_D1v2</span></span> |<span data-ttu-id="b17b0-133">Standard_D15v2</span><span class="sxs-lookup"><span data-stu-id="b17b0-133">Standard_D15v2</span></span> |
> | <span data-ttu-id="b17b0-134">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="b17b0-134">Standard_G1</span></span> |<span data-ttu-id="b17b0-135">Na úrovni Standard_G5</span><span class="sxs-lookup"><span data-stu-id="b17b0-135">Standard_G5</span></span> |
> | <span data-ttu-id="b17b0-136">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="b17b0-136">Standard_GS1</span></span> |<span data-ttu-id="b17b0-137">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="b17b0-137">Standard_GS5</span></span> |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a><span data-ttu-id="b17b0-138">Vytvořit účet Azure Automation s možností Spustit jako</span><span class="sxs-lookup"><span data-stu-id="b17b0-138">Create an Azure Automation Account with run-as capability</span></span>
<span data-ttu-id="b17b0-139">První věc Hello potřebujete toodo je, vytvořte účet Azure Automation, který bude hostitelem hello sady runbook používá tooscale hello Škálovací sady virtuálního počítače instancí.</span><span class="sxs-lookup"><span data-stu-id="b17b0-139">hello first thing you need toodo is create an Azure Automation account that will host hello runbooks used tooscale hello VM Scale Set instances.</span></span> <span data-ttu-id="b17b0-140">Nedávno [Azure Automation](https://azure.microsoft.com/services/automation/) zavedená "Účet Spustit jako" funkci hello, která usnadňuje nastavení hello instanční objekt automaticky spouští sady runbook hello jménem uživatele velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="b17b0-140">Recently [Azure Automation](https://azure.microsoft.com/services/automation/) introduced hello "Run As account" feature which makes setting up hello Service Principal for automatically running hello runbooks on a user's behalf very easy.</span></span> <span data-ttu-id="b17b0-141">Další informace najdete v článku hello níže:</span><span class="sxs-lookup"><span data-stu-id="b17b0-141">You can read more about this in hello article below:</span></span>

* [<span data-ttu-id="b17b0-142">Ověření runbooků pomocí účtu Spustit v Azure jako</span><span class="sxs-lookup"><span data-stu-id="b17b0-142">Authenticate Runbooks with Azure Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="b17b0-143">Importování sad runbook Azure Automation vertikální Škálováním do předplatného</span><span class="sxs-lookup"><span data-stu-id="b17b0-143">Import Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="b17b0-144">sady runbook Hello potřeba toovertically měřítka, které vaše škálovatelné sady virtuálních počítačů jsou již publikován v hello Galerie Runbooků automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="b17b0-144">hello runbooks needed toovertically scale your VM Scale Sets are already published in hello Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="b17b0-145">tooimport je do vašeho předplatného použijte hello kroky v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="b17b0-145">tooimport them into your subscription follow hello steps in this article:</span></span>

* [<span data-ttu-id="b17b0-146">Galerie Runbooků a modulů pro Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b17b0-146">Runbook and module galleries for Azure Automation</span></span>](../automation/automation-runbook-gallery.md)

<span data-ttu-id="b17b0-147">Vyberte možnost Procházet galerii hello nabídce hello sady Runbook:</span><span class="sxs-lookup"><span data-stu-id="b17b0-147">Choose hello Browse Gallery option from hello Runbooks menu:</span></span>

![Importovat toobe sady Runbook][runbooks]

<span data-ttu-id="b17b0-149">Hello sady runbook, které je třeba importovat toobe se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b17b0-149">hello runbooks that need toobe imported are shown.</span></span> <span data-ttu-id="b17b0-150">Vyberte sadu runbook hello založené na tom, zda chcete svislé škálování s nebo bez reprovisioning:</span><span class="sxs-lookup"><span data-stu-id="b17b0-150">Select hello runbook based on whether you want vertical scaling with or without reprovisioning:</span></span>

![Galerie Runbooků][gallery]

## <a name="add-a-webhook-tooyour-runbook"></a><span data-ttu-id="b17b0-152">Přidat runbook tooyour webhooku</span><span class="sxs-lookup"><span data-stu-id="b17b0-152">Add a webhook tooyour runbook</span></span>
<span data-ttu-id="b17b0-153">Jakmile importujete sady runbook hello budete potřebovat tooadd webhooku toohello runbook, mohou být aktivovány výstrahy ze sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b17b0-153">Once you've imported hello runbooks you'll need tooadd a webhook toohello runbook so it can be triggered by an alert from a VM Scale Set.</span></span> <span data-ttu-id="b17b0-154">Hello podrobnosti o vytváření webhooku pro své sadě Runbook jsou popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="b17b0-154">hello details of creating a webhook for your Runbook are described in this article:</span></span>

* [<span data-ttu-id="b17b0-155">Webhooky Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b17b0-155">Azure Automation webhooks</span></span>](../automation/automation-webhooks.md)

> [!NOTE]
> <span data-ttu-id="b17b0-156">Ujistěte se, že zkopírujete hello webhooku URI před jeho zavřením hello webhooku dialogu, jak je budete potřebovat v další části hello.</span><span class="sxs-lookup"><span data-stu-id="b17b0-156">Make sure you copy hello webhook URI before closing hello webhook dialog as you will need this in hello next section.</span></span>
> 
> 

## <a name="add-an-alert-tooyour-vm-scale-set"></a><span data-ttu-id="b17b0-157">Přidat výstrahy tooyour Škálovací sady virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b17b0-157">Add an alert tooyour VM Scale Set</span></span>
<span data-ttu-id="b17b0-158">Níže je skript prostředí PowerShell, který ukazuje způsob tooadd výstrahy tooa Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b17b0-158">Below is a PowerShell script which shows how tooadd an alert tooa VM Scale Set.</span></span> <span data-ttu-id="b17b0-159">Odkazovat toohello následující název hello tooget článku hello metriky toofire hello výstrahy: [běžné metriky automatického škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b17b0-159">Refer toohello following article tooget hello name of hello metric toofire hello alert on: [Azure Monitor autoscaling common metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span>

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
> <span data-ttu-id="b17b0-160">Je doporučeno tooconfigure přiměřené časový interval pro upozornění hello tooavoid pořadí spouštění svislé škálování a všech přidružených přerušení služby, příliš často.</span><span class="sxs-lookup"><span data-stu-id="b17b0-160">It is recommended tooconfigure a reasonable time window for hello alert in order tooavoid triggering vertical scaling, and any associated service interruption, too often.</span></span> <span data-ttu-id="b17b0-161">Vezměte v úvahu okno minimálně 20-30 minut nebo déle.</span><span class="sxs-lookup"><span data-stu-id="b17b0-161">Consider a window of least 20-30 minutes or more.</span></span> <span data-ttu-id="b17b0-162">Vezměte v úvahu horizontální škálování, pokud potřebujete tooavoid žádné přerušení.</span><span class="sxs-lookup"><span data-stu-id="b17b0-162">Consider horizontal scaling if you need tooavoid any interruption.</span></span>
> 
> 

<span data-ttu-id="b17b0-163">Další informace o tom, jak toocreate výstrahy naleznete toohello následující články:</span><span class="sxs-lookup"><span data-stu-id="b17b0-163">For more information on how toocreate alerts refer toohello following articles:</span></span>

* [<span data-ttu-id="b17b0-164">Ukázek Azure PowerShell monitorování rychlý start</span><span class="sxs-lookup"><span data-stu-id="b17b0-164">Azure Monitor PowerShell quick start samples</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [<span data-ttu-id="b17b0-165">Ukázek Azure rychlý start monitorování napříč platformami rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b17b0-165">Azure Monitor Cross-platform CLI quick start samples</span></span>](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a><span data-ttu-id="b17b0-166">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b17b0-166">Summary</span></span>
<span data-ttu-id="b17b0-167">Tento článek vám ukázal, jednoduché příklady svislé škálování.</span><span class="sxs-lookup"><span data-stu-id="b17b0-167">This article showed simple vertical scaling examples.</span></span> <span data-ttu-id="b17b0-168">Pomocí těchto stavební bloky – účet Automation, sady runbook, webhooků, výstrahy – můžete připojit mnoho různých událostí s vlastní sadu akcí.</span><span class="sxs-lookup"><span data-stu-id="b17b0-168">With these building blocks - Automation account, runbooks, webhooks, alerts - you can connect a rich variety of events with a customized set of actions.</span></span>

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
