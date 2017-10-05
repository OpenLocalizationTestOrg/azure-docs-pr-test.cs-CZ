---
title: "Svisle škálování sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Postup svisle škálování virtuálního počítače v reakci na monitorování výstrahy s Azure Automation."
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
ms.openlocfilehash: 9159a5a9041864fe06785829121233379c46bb03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a><span data-ttu-id="0c77c-103">Svislé škálování s sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0c77c-103">Vertical autoscale with Virtual Machine Scale sets</span></span>
<span data-ttu-id="0c77c-104">Tento článek popisuje postup svisle škálování Azure [sady škálování virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/) s nebo bez reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="0c77c-104">This article describes how to vertically scale Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) with or without reprovisioning.</span></span> <span data-ttu-id="0c77c-105">Svislé škálování virtuálních počítačů, které nejsou v sadách škálování, najdete v části [svisle škálování virtuální počítač Azure s Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c77c-105">For vertical scaling of VMs which are not in scale sets, refer to [Vertically scale Azure virtual machine with Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="0c77c-106">Svislé škálování, také známé jako *škálovat* a *snižovat*, znamená zvýšením nebo snížením velikosti virtuálních počítačů (VM) v reakci na zatížení.</span><span class="sxs-lookup"><span data-stu-id="0c77c-106">Vertical scaling, also known as *scale up* and *scale down*, means increasing or decreasing virtual machine (VM) sizes in response to a workload.</span></span> <span data-ttu-id="0c77c-107">Výsledky porovnejte s [vodorovné škálování](virtual-machine-scale-sets-autoscale-overview.md), také nazývaný jako *škálovat* a *škálování v*, kde je počet virtuálních počítačů změnit v závislosti na zatížení.</span><span class="sxs-lookup"><span data-stu-id="0c77c-107">Compare this with [horizontal scaling](virtual-machine-scale-sets-autoscale-overview.md), also referred to as *scale out* and *scale in*, where the number of VMs is altered depending on the workload.</span></span>

<span data-ttu-id="0c77c-108">Reprovisioning znamená odebrat existující virtuální počítač a jeho nahrazení novou.</span><span class="sxs-lookup"><span data-stu-id="0c77c-108">Reprovisioning means removing an existing VM and replacing it with a new one.</span></span> <span data-ttu-id="0c77c-109">Když zvětšit nebo zmenšit velikost virtuální počítače ve Škálovací sadě virtuálních počítačů, v některých případech budete chtít změnit velikost existujících virtuálních počítačů a zachovat data, zatímco v ostatních případech je třeba nasadit nové virtuální počítače nové velikosti.</span><span class="sxs-lookup"><span data-stu-id="0c77c-109">When you increase or decrease the size of VMs in a VM Scale Set, in some cases you want to resize existing VMs and retain your data, while in other cases you need to deploy new VMs of the new size.</span></span> <span data-ttu-id="0c77c-110">Tento dokument popisuje obou případech.</span><span class="sxs-lookup"><span data-stu-id="0c77c-110">This document covers both cases.</span></span>

<span data-ttu-id="0c77c-111">Svislé škálování může být užitečné, když:</span><span class="sxs-lookup"><span data-stu-id="0c77c-111">Vertical scaling can be useful when:</span></span>

* <span data-ttu-id="0c77c-112">Služba založený na virtuální počítače je v části využívat (třeba na víkendů).</span><span class="sxs-lookup"><span data-stu-id="0c77c-112">A service built on virtual machines is under-utilized (for example at weekends).</span></span> <span data-ttu-id="0c77c-113">Zmenšení velikosti virtuálního počítače může snížit náklady na měsíční.</span><span class="sxs-lookup"><span data-stu-id="0c77c-113">Reducing the VM size can reduce monthly costs.</span></span>
* <span data-ttu-id="0c77c-114">Zvýšit velikost virtuálního počítače, aby se zvládl větší vyžádání bez vytvoření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c77c-114">Increasing VM size to cope with larger demand without creating additional VMs.</span></span>

<span data-ttu-id="0c77c-115">Nastavením svislé škálování být spouštěná na základě metriky na základě výstrahy ze sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c77c-115">You can set up vertical scaling to be triggered based on metric based alerts from your VM Scale Set.</span></span> <span data-ttu-id="0c77c-116">Když se aktivuje výstraha aktivuje webhook, jehož této aktivační události, které se nastavit sadu runbook, které můžete škálovat vaše škálování nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="0c77c-116">When the alert is activated it fires a webhook that triggers a runbook which can scale your scale set up or down.</span></span> <span data-ttu-id="0c77c-117">Svislé škálování lze nastavit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="0c77c-117">Vertical scaling can be configured by following these steps:</span></span>

1. <span data-ttu-id="0c77c-118">Vytvořte účet Azure Automation se schopností spustit jako.</span><span class="sxs-lookup"><span data-stu-id="0c77c-118">Create an Azure Automation account with run-as capability.</span></span>
2. <span data-ttu-id="0c77c-119">Importování sad runbook vertikální Škálováním Azure Automation pro škálovatelné sady virtuálních počítačů do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="0c77c-119">Import Azure Automation Vertical Scale runbooks for VM Scale Sets into your subscription.</span></span>
3. <span data-ttu-id="0c77c-120">Přidejte webhook, jehož do runbooku.</span><span class="sxs-lookup"><span data-stu-id="0c77c-120">Add a webhook to your runbook.</span></span>
4. <span data-ttu-id="0c77c-121">Přidáte oznámení na vaše Škálovací sady virtuálního počítače pomocí webhooku oznámení.</span><span class="sxs-lookup"><span data-stu-id="0c77c-121">Add an alert to your VM Scale Set using a webhook notification.</span></span>

> [!NOTE]
> <span data-ttu-id="0c77c-122">Svislé automatické škálování se v určitém rozsahu velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c77c-122">Vertical autoscaling can only take place within certain ranges of VM sizes.</span></span> <span data-ttu-id="0c77c-123">Porovnání specifikace velikosti jednotlivých před rozhodnutím o škálování na další (vyšší počet vždy neindikuje větší velikost virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="0c77c-123">Compare the specifications of each size before deciding to scale from one to another (higher number does not always indicate bigger VM size).</span></span> <span data-ttu-id="0c77c-124">Je možné škálovat mezi následující páry velikosti:</span><span class="sxs-lookup"><span data-stu-id="0c77c-124">You can choose to scale between the following pairs of sizes:</span></span>
> 
> | <span data-ttu-id="0c77c-125">Škálování pár velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0c77c-125">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="0c77c-126">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="0c77c-126">Standard_A0</span></span> |<span data-ttu-id="0c77c-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="0c77c-127">Standard_A11</span></span> |
> | <span data-ttu-id="0c77c-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="0c77c-128">Standard_D1</span></span> |<span data-ttu-id="0c77c-129">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="0c77c-129">Standard_D14</span></span> |
> | <span data-ttu-id="0c77c-130">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="0c77c-130">Standard_DS1</span></span> |<span data-ttu-id="0c77c-131">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="0c77c-131">Standard_DS14</span></span> |
> | <span data-ttu-id="0c77c-132">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="0c77c-132">Standard_D1v2</span></span> |<span data-ttu-id="0c77c-133">Standard_D15v2</span><span class="sxs-lookup"><span data-stu-id="0c77c-133">Standard_D15v2</span></span> |
> | <span data-ttu-id="0c77c-134">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="0c77c-134">Standard_G1</span></span> |<span data-ttu-id="0c77c-135">Na úrovni Standard_G5</span><span class="sxs-lookup"><span data-stu-id="0c77c-135">Standard_G5</span></span> |
> | <span data-ttu-id="0c77c-136">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="0c77c-136">Standard_GS1</span></span> |<span data-ttu-id="0c77c-137">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="0c77c-137">Standard_GS5</span></span> |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a><span data-ttu-id="0c77c-138">Vytvořit účet Azure Automation s možností Spustit jako</span><span class="sxs-lookup"><span data-stu-id="0c77c-138">Create an Azure Automation Account with run-as capability</span></span>
<span data-ttu-id="0c77c-139">První věc, které musíte udělat, je vytvořit účet Azure Automation, který bude hostitelem sady runbook lze škálovat instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c77c-139">The first thing you need to do is create an Azure Automation account that will host the runbooks used to scale the VM Scale Set instances.</span></span> <span data-ttu-id="0c77c-140">Nedávno [Azure Automation](https://azure.microsoft.com/services/automation/) zavedená funkci "Spustit jako účet", která zajišťuje nastavení se objekt služby pro automatické spouštění sady runbook jménem uživatele velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="0c77c-140">Recently [Azure Automation](https://azure.microsoft.com/services/automation/) introduced the "Run As account" feature which makes setting up the Service Principal for automatically running the runbooks on a user's behalf very easy.</span></span> <span data-ttu-id="0c77c-141">Další informace najdete v článku níže:</span><span class="sxs-lookup"><span data-stu-id="0c77c-141">You can read more about this in the article below:</span></span>

* [<span data-ttu-id="0c77c-142">Ověření runbooků pomocí účtu Spustit v Azure jako</span><span class="sxs-lookup"><span data-stu-id="0c77c-142">Authenticate Runbooks with Azure Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="0c77c-143">Importování sad runbook Azure Automation vertikální Škálováním do předplatného</span><span class="sxs-lookup"><span data-stu-id="0c77c-143">Import Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="0c77c-144">Sady runbook potřeby svisle škálovat vaše škálovatelné sady virtuálních počítačů jsou již publikován v galerii Azure Automation Runbook.</span><span class="sxs-lookup"><span data-stu-id="0c77c-144">The runbooks needed to vertically scale your VM Scale Sets are already published in the Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="0c77c-145">Chcete-li importovat je do vašeho předplatného postupujte podle kroků v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="0c77c-145">To import them into your subscription follow the steps in this article:</span></span>

* [<span data-ttu-id="0c77c-146">Galerie Runbooků a modulů pro Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="0c77c-146">Runbook and module galleries for Azure Automation</span></span>](../automation/automation-runbook-gallery.md)

<span data-ttu-id="0c77c-147">Vyberte možnost Procházet galerii v nabídce sady Runbook:</span><span class="sxs-lookup"><span data-stu-id="0c77c-147">Choose the Browse Gallery option from the Runbooks menu:</span></span>

![Sady Runbook k importu][runbooks]

<span data-ttu-id="0c77c-149">Sady runbook, které potřebují k importu se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="0c77c-149">The runbooks that need to be imported are shown.</span></span> <span data-ttu-id="0c77c-150">Vyberte sadu runbook založené na tom, zda chcete svislé škálování s nebo bez reprovisioning:</span><span class="sxs-lookup"><span data-stu-id="0c77c-150">Select the runbook based on whether you want vertical scaling with or without reprovisioning:</span></span>

![Galerie Runbooků][gallery]

## <a name="add-a-webhook-to-your-runbook"></a><span data-ttu-id="0c77c-152">Přidat webhook, jehož do runbooku</span><span class="sxs-lookup"><span data-stu-id="0c77c-152">Add a webhook to your runbook</span></span>
<span data-ttu-id="0c77c-153">Jakmile importujete sady runbook, které budete muset přidat webhook, jehož do sady runbook, mohou být aktivovány výstrahy ze sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c77c-153">Once you've imported the runbooks you'll need to add a webhook to the runbook so it can be triggered by an alert from a VM Scale Set.</span></span> <span data-ttu-id="0c77c-154">Podrobnosti o vytváření webhooku pro své sadě Runbook jsou popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="0c77c-154">The details of creating a webhook for your Runbook are described in this article:</span></span>

* [<span data-ttu-id="0c77c-155">Webhooky Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="0c77c-155">Azure Automation webhooks</span></span>](../automation/automation-webhooks.md)

> [!NOTE]
> <span data-ttu-id="0c77c-156">Ujistěte se, že zkopírujete webhooku URI před jeho zavřením dialogu webhooku, jak je budete potřebovat v další části.</span><span class="sxs-lookup"><span data-stu-id="0c77c-156">Make sure you copy the webhook URI before closing the webhook dialog as you will need this in the next section.</span></span>
> 
> 

## <a name="add-an-alert-to-your-vm-scale-set"></a><span data-ttu-id="0c77c-157">Přidat výstrahy do sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0c77c-157">Add an alert to your VM Scale Set</span></span>
<span data-ttu-id="0c77c-158">Níže je skript prostředí PowerShell, který ukazuje, jak přidat výstrahy do sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c77c-158">Below is a PowerShell script which shows how to add an alert to a VM Scale Set.</span></span> <span data-ttu-id="0c77c-159">Najdete v následujícím článku se získat název metriky do aktivovat upozornění na: [běžné metriky automatického škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="0c77c-159">Refer to the following article to get the name of the metric to fire the alert on: [Azure Monitor autoscaling common metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span>

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
> <span data-ttu-id="0c77c-160">Doporučujeme nakonfigurovat přiměřené časové okno k výstraze, aby se zabránilo spouštění svislé škálování a všechny přidružené přerušení služby, příliš často.</span><span class="sxs-lookup"><span data-stu-id="0c77c-160">It is recommended to configure a reasonable time window for the alert in order to avoid triggering vertical scaling, and any associated service interruption, too often.</span></span> <span data-ttu-id="0c77c-161">Vezměte v úvahu okno minimálně 20-30 minut nebo déle.</span><span class="sxs-lookup"><span data-stu-id="0c77c-161">Consider a window of least 20-30 minutes or more.</span></span> <span data-ttu-id="0c77c-162">Vezměte v úvahu horizontální škálování, pokud je potřeba se zabránilo přerušení.</span><span class="sxs-lookup"><span data-stu-id="0c77c-162">Consider horizontal scaling if you need to avoid any interruption.</span></span>
> 
> 

<span data-ttu-id="0c77c-163">Další informace o tom, jak vytvářet výstrahy, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="0c77c-163">For more information on how to create alerts refer to the following articles:</span></span>

* [<span data-ttu-id="0c77c-164">Ukázek Azure PowerShell monitorování rychlý start</span><span class="sxs-lookup"><span data-stu-id="0c77c-164">Azure Monitor PowerShell quick start samples</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [<span data-ttu-id="0c77c-165">Ukázek Azure rychlý start monitorování napříč platformami rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0c77c-165">Azure Monitor Cross-platform CLI quick start samples</span></span>](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a><span data-ttu-id="0c77c-166">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0c77c-166">Summary</span></span>
<span data-ttu-id="0c77c-167">Tento článek vám ukázal, jednoduché příklady svislé škálování.</span><span class="sxs-lookup"><span data-stu-id="0c77c-167">This article showed simple vertical scaling examples.</span></span> <span data-ttu-id="0c77c-168">Pomocí těchto stavební bloky – účet Automation, sady runbook, webhooků, výstrahy – můžete připojit mnoho různých událostí s vlastní sadu akcí.</span><span class="sxs-lookup"><span data-stu-id="0c77c-168">With these building blocks - Automation account, runbooks, webhooks, alerts - you can connect a rich variety of events with a customized set of actions.</span></span>

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
