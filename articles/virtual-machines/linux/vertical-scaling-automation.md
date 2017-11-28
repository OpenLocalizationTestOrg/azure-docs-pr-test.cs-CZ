---
title: "škálování aaaVertically virtuální počítač Azure s Azure Automation | Microsoft Docs"
description: "Jak toovertically škálování virtuální počítač s Linuxem v odpovědi toomonitoring výstrahy s Azure Automation."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee4c1c33a588bd907d107f1828380a8afdaa725e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a><span data-ttu-id="d06fc-103">Svisle škálování virtuálního počítače Azure Linux s Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="d06fc-103">Vertically scale Azure Linux virtual machine with Azure Automation</span></span>
<span data-ttu-id="d06fc-104">Svislé škálování je proces hello zvýšením nebo snížením hello prostředky počítače v odpovědi toohello zatížení.</span><span class="sxs-lookup"><span data-stu-id="d06fc-104">Vertical scaling is hello process of increasing or decreasing hello resources of a machine in response toohello workload.</span></span> <span data-ttu-id="d06fc-105">V Azure to můžete udělat tak, že změníte velikost hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d06fc-105">In Azure this can be accomplished by changing hello size of hello Virtual Machine.</span></span> <span data-ttu-id="d06fc-106">To může pomoct v hello následující scénáře</span><span class="sxs-lookup"><span data-stu-id="d06fc-106">This can help in hello following scenarios</span></span>

* <span data-ttu-id="d06fc-107">Pokud hello virtuálního počítače není často používají, můžete změnit velikost ho dolů tooa menší velikost tooreduce náklady na měsíční</span><span class="sxs-lookup"><span data-stu-id="d06fc-107">If hello Virtual Machine is not being used frequently, you can resize it down tooa smaller size tooreduce your monthly costs</span></span>
* <span data-ttu-id="d06fc-108">Pokud hello virtuálního počítače je očekávaný zátěž ve špičce, můžete se změněnou velikostí tooa větší velikost tooincrease své kapacity</span><span class="sxs-lookup"><span data-stu-id="d06fc-108">If hello Virtual Machine is seeing a peak load, it can be resized tooa larger size tooincrease its capacity</span></span>

<span data-ttu-id="d06fc-109">Hello outline pro tooaccomplish kroky hello jedná jako níže</span><span class="sxs-lookup"><span data-stu-id="d06fc-109">hello outline for hello steps tooaccomplish this is as below</span></span>

1. <span data-ttu-id="d06fc-110">Instalační program Azure Automation tooaccess virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="d06fc-110">Setup Azure Automation tooaccess your Virtual Machines</span></span>
2. <span data-ttu-id="d06fc-111">Importování sad runbook hello Azure Automation vertikální Škálováním do předplatného</span><span class="sxs-lookup"><span data-stu-id="d06fc-111">Import hello Azure Automation Vertical Scale runbooks into your subscription</span></span>
3. <span data-ttu-id="d06fc-112">Přidat runbook tooyour webhooku</span><span class="sxs-lookup"><span data-stu-id="d06fc-112">Add a webhook tooyour runbook</span></span>
4. <span data-ttu-id="d06fc-113">Přidat výstrahy tooyour virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d06fc-113">Add an alert tooyour Virtual Machine</span></span>

> [!NOTE]
> <span data-ttu-id="d06fc-114">Z důvodu hello velikost hello první virtuální počítač, velikosti hello je možné rozšířit, bude možná omezen z důvodu toohello dostupnost hello ostatní formáty v clusteru hello aktuální virtuální počítač je nasazena v.</span><span class="sxs-lookup"><span data-stu-id="d06fc-114">Because of hello size of hello first Virtual Machine, hello sizes it can be scaled to, may be limited due toohello availability of hello other sizes in hello cluster current Virtual Machine is deployed in.</span></span> <span data-ttu-id="d06fc-115">V hello publikovaných runbooků služeb automatizace používané v tomto článku jsme postará o tento případ a škálovat jenom v rámci hello níže páry velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d06fc-115">In hello published automation runbooks used in this article we take care of this case and only scale within hello below VM size pairs.</span></span> <span data-ttu-id="d06fc-116">To znamená, že virtuální počítač Standard_D1v2 není najednou škálovat tooStandard_G5 nebo škálovat dolů tooBasic_A0.</span><span class="sxs-lookup"><span data-stu-id="d06fc-116">This means that a Standard_D1v2 Virtual Machine will not suddenly be scaled up tooStandard_G5 or scaled down tooBasic_A0.</span></span>
> 
> | <span data-ttu-id="d06fc-117">Škálování pár velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d06fc-117">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="d06fc-118">Basic_A0</span><span class="sxs-lookup"><span data-stu-id="d06fc-118">Basic_A0</span></span> |<span data-ttu-id="d06fc-119">Basic_A4</span><span class="sxs-lookup"><span data-stu-id="d06fc-119">Basic_A4</span></span> |
> | <span data-ttu-id="d06fc-120">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="d06fc-120">Standard_A0</span></span> |<span data-ttu-id="d06fc-121">Standard_A4</span><span class="sxs-lookup"><span data-stu-id="d06fc-121">Standard_A4</span></span> |
> | <span data-ttu-id="d06fc-122">Standard_A5</span><span class="sxs-lookup"><span data-stu-id="d06fc-122">Standard_A5</span></span> |<span data-ttu-id="d06fc-123">Standard_A7</span><span class="sxs-lookup"><span data-stu-id="d06fc-123">Standard_A7</span></span> |
> | <span data-ttu-id="d06fc-124">Standard_A8</span><span class="sxs-lookup"><span data-stu-id="d06fc-124">Standard_A8</span></span> |<span data-ttu-id="d06fc-125">Standard_A9</span><span class="sxs-lookup"><span data-stu-id="d06fc-125">Standard_A9</span></span> |
> | <span data-ttu-id="d06fc-126">Standard_A10</span><span class="sxs-lookup"><span data-stu-id="d06fc-126">Standard_A10</span></span> |<span data-ttu-id="d06fc-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="d06fc-127">Standard_A11</span></span> |
> | <span data-ttu-id="d06fc-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="d06fc-128">Standard_D1</span></span> |<span data-ttu-id="d06fc-129">Standard_D4</span><span class="sxs-lookup"><span data-stu-id="d06fc-129">Standard_D4</span></span> |
> | <span data-ttu-id="d06fc-130">Standard_D11</span><span class="sxs-lookup"><span data-stu-id="d06fc-130">Standard_D11</span></span> |<span data-ttu-id="d06fc-131">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="d06fc-131">Standard_D14</span></span> |
> | <span data-ttu-id="d06fc-132">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="d06fc-132">Standard_DS1</span></span> |<span data-ttu-id="d06fc-133">Standard_DS4</span><span class="sxs-lookup"><span data-stu-id="d06fc-133">Standard_DS4</span></span> |
> | <span data-ttu-id="d06fc-134">Standard_DS11</span><span class="sxs-lookup"><span data-stu-id="d06fc-134">Standard_DS11</span></span> |<span data-ttu-id="d06fc-135">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="d06fc-135">Standard_DS14</span></span> |
> | <span data-ttu-id="d06fc-136">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="d06fc-136">Standard_D1v2</span></span> |<span data-ttu-id="d06fc-137">Standard_D5v2</span><span class="sxs-lookup"><span data-stu-id="d06fc-137">Standard_D5v2</span></span> |
> | <span data-ttu-id="d06fc-138">Standard_D11v2</span><span class="sxs-lookup"><span data-stu-id="d06fc-138">Standard_D11v2</span></span> |<span data-ttu-id="d06fc-139">Standard_D14v2</span><span class="sxs-lookup"><span data-stu-id="d06fc-139">Standard_D14v2</span></span> |
> | <span data-ttu-id="d06fc-140">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="d06fc-140">Standard_G1</span></span> |<span data-ttu-id="d06fc-141">Na úrovni Standard_G5</span><span class="sxs-lookup"><span data-stu-id="d06fc-141">Standard_G5</span></span> |
> | <span data-ttu-id="d06fc-142">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="d06fc-142">Standard_GS1</span></span> |<span data-ttu-id="d06fc-143">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="d06fc-143">Standard_GS5</span></span> |
> 
> 

## <a name="setup-azure-automation-tooaccess-your-virtual-machines"></a><span data-ttu-id="d06fc-144">Instalační program Azure Automation tooaccess virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="d06fc-144">Setup Azure Automation tooaccess your Virtual Machines</span></span>
<span data-ttu-id="d06fc-145">První věc Hello potřebujete toodo je, vytvořte účet Azure Automation, který bude hostitelem hello sady runbook používá tooscale hello Škálovací sady virtuálního počítače instancí.</span><span class="sxs-lookup"><span data-stu-id="d06fc-145">hello first thing you need toodo is create an Azure Automation account that will host hello runbooks used tooscale hello VM Scale Set instances.</span></span> <span data-ttu-id="d06fc-146">Služba Automation hello nedávno zavedl "Účet Spustit jako" funkci hello, která usnadňuje nastavení hello instanční objekt automaticky spouští sady runbook hello jménem hello uživatele velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="d06fc-146">Recently hello Automation service introduced hello "Run As account" feature which makes setting up hello Service Principal for automatically running hello runbooks on hello user's behalf very easy.</span></span> <span data-ttu-id="d06fc-147">Další informace najdete v článku hello níže:</span><span class="sxs-lookup"><span data-stu-id="d06fc-147">You can read more about this in hello article below:</span></span>

* [<span data-ttu-id="d06fc-148">Ověření runbooků pomocí účtu Spustit v Azure jako</span><span class="sxs-lookup"><span data-stu-id="d06fc-148">Authenticate Runbooks with Azure Run As account</span></span>](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-hello-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="d06fc-149">Importování sad runbook hello Azure Automation vertikální Škálováním do předplatného</span><span class="sxs-lookup"><span data-stu-id="d06fc-149">Import hello Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="d06fc-150">Hello sady runbook, které jsou potřebné pro svisle škálování virtuálního počítače jsou již publikován v hello Galerie Runbooků automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="d06fc-150">hello runbooks that are needed for Vertically Scaling your Virtual Machine are already published in hello Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="d06fc-151">Budete potřebovat tooimport je do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="d06fc-151">You will need tooimport them into your subscription.</span></span> <span data-ttu-id="d06fc-152">Dozvíte, jak hello tooimport sady runbook ve čtení tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="d06fc-152">You can learn how tooimport runbooks by reading hello following article.</span></span>

* [<span data-ttu-id="d06fc-153">Galerie Runbooků a modulů pro Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="d06fc-153">Runbook and module galleries for Azure Automation</span></span>](../../automation/automation-runbook-gallery.md)

<span data-ttu-id="d06fc-154">v hello obrázek níže jsou uvedeny Hello sady runbook, které je třeba importovat toobe</span><span class="sxs-lookup"><span data-stu-id="d06fc-154">hello runbooks that need toobe imported are shown in hello image below</span></span>

![Importování sad runbook](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-tooyour-runbook"></a><span data-ttu-id="d06fc-156">Přidat runbook tooyour webhooku</span><span class="sxs-lookup"><span data-stu-id="d06fc-156">Add a webhook tooyour runbook</span></span>
<span data-ttu-id="d06fc-157">Jakmile importujete sady runbook hello budete potřebovat tooadd webhooku toohello runbook, mohou být aktivovány výstrahu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d06fc-157">Once you've imported hello runbooks you'll need tooadd a webhook toohello runbook so it can be triggered by an alert from a Virtual Machine.</span></span> <span data-ttu-id="d06fc-158">Hello podrobnosti o vytváření webhooku pro vaše sada Runbook můžete přečíst zde</span><span class="sxs-lookup"><span data-stu-id="d06fc-158">hello details of creating a webhook for your Runbook can be read here</span></span>

* [<span data-ttu-id="d06fc-159">Webhooky Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="d06fc-159">Azure Automation webhooks</span></span>](../../automation/automation-webhooks.md)

<span data-ttu-id="d06fc-160">Ujistěte se, že zkopírujete hello webhooku před jeho zavřením hello webhooku dialogu, jak je budete potřebovat v další části hello.</span><span class="sxs-lookup"><span data-stu-id="d06fc-160">Make sure you copy hello webhook before closing hello webhook dialog as you will need this in hello next section.</span></span>

## <a name="add-an-alert-tooyour-virtual-machine"></a><span data-ttu-id="d06fc-161">Přidat výstrahy tooyour virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d06fc-161">Add an alert tooyour Virtual Machine</span></span>
1. <span data-ttu-id="d06fc-162">Vyberte nastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d06fc-162">Select Virtual Machine settings</span></span>
2. <span data-ttu-id="d06fc-163">Vyberte "Pravidla výstrah"</span><span class="sxs-lookup"><span data-stu-id="d06fc-163">Select "Alert rules"</span></span>
3. <span data-ttu-id="d06fc-164">Vyberte možnost "Přidat upozornění"</span><span class="sxs-lookup"><span data-stu-id="d06fc-164">Select "Add alert"</span></span>
4. <span data-ttu-id="d06fc-165">Vyberte výstrahu hello metriky toofire na</span><span class="sxs-lookup"><span data-stu-id="d06fc-165">Select a metric toofire hello alert on</span></span>
5. <span data-ttu-id="d06fc-166">Vyberte podmínku, která v případě splnění bude způsobit výstrahy toofire hello</span><span class="sxs-lookup"><span data-stu-id="d06fc-166">Select a condition, which when fulfilled will cause hello alert toofire</span></span>
6. <span data-ttu-id="d06fc-167">Prahová hodnota pro podmínku hello vyberte v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="d06fc-167">Select a threshold for hello condition in Step 5.</span></span> <span data-ttu-id="d06fc-168">toobe splněny</span><span class="sxs-lookup"><span data-stu-id="d06fc-168">toobe fulfilled</span></span>
7. <span data-ttu-id="d06fc-169">Vyberte období, přes které hello službu monitorování zkontrolujte hello podmínku a prahovou hodnotu v kroky 5 a 6</span><span class="sxs-lookup"><span data-stu-id="d06fc-169">Select a period over which hello monitoring service will check for hello condition and threshold in Steps 5 & 6</span></span>
8. <span data-ttu-id="d06fc-170">Vložte hello webhooku, které jste zkopírovali z předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="d06fc-170">Paste in hello webhook you copied from hello previous section.</span></span>

![Přidat výstrahy tooVirtual 1 počítač](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Přidat výstrahy tooVirtual Machine 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

