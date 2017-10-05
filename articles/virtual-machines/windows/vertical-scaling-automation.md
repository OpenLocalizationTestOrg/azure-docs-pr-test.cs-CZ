---
title: "Svisle škálování virtuální počítače s Windows pomocí Azure Automation | Microsoft Docs"
description: "Svisle škálování virtuálního počítače s Windows v reakci na monitorování výstrahy s Azure Automation."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ea5169c1a95f00e78ae3f5f177812466eb7a0deb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a><span data-ttu-id="1df6e-103">Svisle škálování virtuálních počítačů Windows s Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1df6e-103">Vertically scale Windows VMs with Azure Automation</span></span>

<span data-ttu-id="1df6e-104">Svislé škálování je proces zvýšením nebo snížením prostředky počítače v reakci na zatížení.</span><span class="sxs-lookup"><span data-stu-id="1df6e-104">Vertical scaling is the process of increasing or decreasing the resources of a machine in response to the workload.</span></span> <span data-ttu-id="1df6e-105">V Azure to můžete provést změnou velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1df6e-105">In Azure this can be accomplished by changing the size of the Virtual Machine.</span></span> <span data-ttu-id="1df6e-106">To může pomoct v těchto scénářích</span><span class="sxs-lookup"><span data-stu-id="1df6e-106">This can help in the following scenarios</span></span>

* <span data-ttu-id="1df6e-107">Pokud virtuální počítač není často používají, můžete jeho velikost a menší velikosti, abyste snížili náklady na měsíční</span><span class="sxs-lookup"><span data-stu-id="1df6e-107">If the Virtual Machine is not being used frequently, you can resize it down to a smaller size to reduce your monthly costs</span></span>
* <span data-ttu-id="1df6e-108">Pokud virtuální počítač se zobrazuje zátěž ve špičce, můžete změnit na větší velikost ke zvýšení jeho kapacity</span><span class="sxs-lookup"><span data-stu-id="1df6e-108">If the Virtual Machine is seeing a peak load, it can be resized to a larger size to increase its capacity</span></span>

<span data-ttu-id="1df6e-109">Osnova pokyny k tomu je jako níže</span><span class="sxs-lookup"><span data-stu-id="1df6e-109">The outline for the steps to accomplish this is as below</span></span>

1. <span data-ttu-id="1df6e-110">Instalační program přístup k virtuálním počítačům Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1df6e-110">Setup Azure Automation to access your Virtual Machines</span></span>
2. <span data-ttu-id="1df6e-111">Import sady runbook Azure Automation vertikální Škálováním do předplatného</span><span class="sxs-lookup"><span data-stu-id="1df6e-111">Import the Azure Automation Vertical Scale runbooks into your subscription</span></span>
3. <span data-ttu-id="1df6e-112">Přidat webhook, jehož do runbooku</span><span class="sxs-lookup"><span data-stu-id="1df6e-112">Add a webhook to your runbook</span></span>
4. <span data-ttu-id="1df6e-113">Přidání oznámení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="1df6e-113">Add an alert to your Virtual Machine</span></span>

> [!NOTE]
> <span data-ttu-id="1df6e-114">Vzhledem k velikosti první virtuální počítač, velikosti, které je možné rozšířit, může být omezen z důvodu dostupnosti v clusteru velikosti aktuální virtuální počítač je nasazena v.</span><span class="sxs-lookup"><span data-stu-id="1df6e-114">Because of the size of the first Virtual Machine, the sizes it can be scaled to, may be limited due to the availability of the other sizes in the cluster current Virtual Machine is deployed in.</span></span> <span data-ttu-id="1df6e-115">V sadách runbook publikované automatizace používané v tomto článku jsme postará o tento případ a škálovat jenom v rámci níže páry velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1df6e-115">In the published automation runbooks used in this article we take care of this case and only scale within the below VM size pairs.</span></span> <span data-ttu-id="1df6e-116">To znamená, že Standard_D1v2 virtuální počítač není najednou rozšířit tak, aby na úrovni Standard_G5 nebo škálovat na Basic_A0.</span><span class="sxs-lookup"><span data-stu-id="1df6e-116">This means that a Standard_D1v2 Virtual Machine will not suddenly be scaled up to Standard_G5 or scaled down to Basic_A0.</span></span>
> 
> | <span data-ttu-id="1df6e-117">Škálování pár velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1df6e-117">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="1df6e-118">Basic_A0</span><span class="sxs-lookup"><span data-stu-id="1df6e-118">Basic_A0</span></span> |<span data-ttu-id="1df6e-119">Basic_A4</span><span class="sxs-lookup"><span data-stu-id="1df6e-119">Basic_A4</span></span> |
> | <span data-ttu-id="1df6e-120">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="1df6e-120">Standard_A0</span></span> |<span data-ttu-id="1df6e-121">Standard_A4</span><span class="sxs-lookup"><span data-stu-id="1df6e-121">Standard_A4</span></span> |
> | <span data-ttu-id="1df6e-122">Standard_A5</span><span class="sxs-lookup"><span data-stu-id="1df6e-122">Standard_A5</span></span> |<span data-ttu-id="1df6e-123">Standard_A7</span><span class="sxs-lookup"><span data-stu-id="1df6e-123">Standard_A7</span></span> |
> | <span data-ttu-id="1df6e-124">Standard_A8</span><span class="sxs-lookup"><span data-stu-id="1df6e-124">Standard_A8</span></span> |<span data-ttu-id="1df6e-125">Standard_A9</span><span class="sxs-lookup"><span data-stu-id="1df6e-125">Standard_A9</span></span> |
> | <span data-ttu-id="1df6e-126">Standard_A10</span><span class="sxs-lookup"><span data-stu-id="1df6e-126">Standard_A10</span></span> |<span data-ttu-id="1df6e-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="1df6e-127">Standard_A11</span></span> |
> | <span data-ttu-id="1df6e-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="1df6e-128">Standard_D1</span></span> |<span data-ttu-id="1df6e-129">Standard_D4</span><span class="sxs-lookup"><span data-stu-id="1df6e-129">Standard_D4</span></span> |
> | <span data-ttu-id="1df6e-130">Standard_D11</span><span class="sxs-lookup"><span data-stu-id="1df6e-130">Standard_D11</span></span> |<span data-ttu-id="1df6e-131">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="1df6e-131">Standard_D14</span></span> |
> | <span data-ttu-id="1df6e-132">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="1df6e-132">Standard_DS1</span></span> |<span data-ttu-id="1df6e-133">Standard_DS4</span><span class="sxs-lookup"><span data-stu-id="1df6e-133">Standard_DS4</span></span> |
> | <span data-ttu-id="1df6e-134">Standard_DS11</span><span class="sxs-lookup"><span data-stu-id="1df6e-134">Standard_DS11</span></span> |<span data-ttu-id="1df6e-135">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="1df6e-135">Standard_DS14</span></span> |
> | <span data-ttu-id="1df6e-136">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="1df6e-136">Standard_D1v2</span></span> |<span data-ttu-id="1df6e-137">Standard_D5v2</span><span class="sxs-lookup"><span data-stu-id="1df6e-137">Standard_D5v2</span></span> |
> | <span data-ttu-id="1df6e-138">Standard_D11v2</span><span class="sxs-lookup"><span data-stu-id="1df6e-138">Standard_D11v2</span></span> |<span data-ttu-id="1df6e-139">Standard_D14v2</span><span class="sxs-lookup"><span data-stu-id="1df6e-139">Standard_D14v2</span></span> |
> | <span data-ttu-id="1df6e-140">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="1df6e-140">Standard_G1</span></span> |<span data-ttu-id="1df6e-141">Na úrovni Standard_G5</span><span class="sxs-lookup"><span data-stu-id="1df6e-141">Standard_G5</span></span> |
> | <span data-ttu-id="1df6e-142">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="1df6e-142">Standard_GS1</span></span> |<span data-ttu-id="1df6e-143">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="1df6e-143">Standard_GS5</span></span> |
> 
> 

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a><span data-ttu-id="1df6e-144">Instalační program přístup k virtuálním počítačům Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1df6e-144">Setup Azure Automation to access your Virtual Machines</span></span>
<span data-ttu-id="1df6e-145">První věc, které musíte udělat, je vytvořit účet Azure Automation, který bude hostitelem sady runbook používá škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1df6e-145">The first thing you need to do is create an Azure Automation account that will host the runbooks used to scale a Virtual Machine.</span></span> <span data-ttu-id="1df6e-146">Služba Automation se nedávno zavedl funkci "Spustit jako účet", která zajišťuje nastavení se objekt služby pro automatické spouštění sady runbook jménem uživatele velmi snadné.</span><span class="sxs-lookup"><span data-stu-id="1df6e-146">Recently the Automation service introduced the "Run As account" feature which makes setting up the Service Principal for automatically running the runbooks on the user's behalf very easy.</span></span> <span data-ttu-id="1df6e-147">Další informace najdete v článku níže:</span><span class="sxs-lookup"><span data-stu-id="1df6e-147">You can read more about this in the article below:</span></span>

* [<span data-ttu-id="1df6e-148">Ověření runbooků pomocí účtu Spustit v Azure jako</span><span class="sxs-lookup"><span data-stu-id="1df6e-148">Authenticate Runbooks with Azure Run As account</span></span>](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="1df6e-149">Import sady runbook Azure Automation vertikální Škálováním do předplatného</span><span class="sxs-lookup"><span data-stu-id="1df6e-149">Import the Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="1df6e-150">Sady runbook, které jsou potřebné pro svisle škálování virtuálního počítače jsou již publikován v galerii Azure Automation Runbook.</span><span class="sxs-lookup"><span data-stu-id="1df6e-150">The runbooks that are needed for Vertically Scaling your Virtual Machine are already published in the Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="1df6e-151">Musíte je importovat do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="1df6e-151">You will need to import them into your subscription.</span></span> <span data-ttu-id="1df6e-152">Můžete postup importování sad runbook načtením v následujícím článku.</span><span class="sxs-lookup"><span data-stu-id="1df6e-152">You can learn how to import runbooks by reading the following article.</span></span>

* [<span data-ttu-id="1df6e-153">Galerie Runbooků a modulů pro Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1df6e-153">Runbook and module galleries for Azure Automation</span></span>](../../automation/automation-runbook-gallery.md)

<span data-ttu-id="1df6e-154">Na obrázku níže jsou uvedeny sady runbook, které potřebují k importu</span><span class="sxs-lookup"><span data-stu-id="1df6e-154">The runbooks that need to be imported are shown in the image below</span></span>

![Importování sad runbook](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a><span data-ttu-id="1df6e-156">Přidat webhook, jehož do runbooku</span><span class="sxs-lookup"><span data-stu-id="1df6e-156">Add a webhook to your runbook</span></span>
<span data-ttu-id="1df6e-157">Jakmile importujete sady runbook, které budete muset přidat webhook, jehož do sady runbook, mohou být aktivovány výstrahu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1df6e-157">Once you've imported the runbooks you'll need to add a webhook to the runbook so it can be triggered by an alert from a Virtual Machine.</span></span> <span data-ttu-id="1df6e-158">Podrobnosti o vytváření webhooku pro vaše sada Runbook můžete přečíst zde</span><span class="sxs-lookup"><span data-stu-id="1df6e-158">The details of creating a webhook for your Runbook can be read here</span></span>

* [<span data-ttu-id="1df6e-159">Webhooky Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1df6e-159">Azure Automation webhooks</span></span>](../../automation/automation-webhooks.md)

<span data-ttu-id="1df6e-160">Ujistěte se, že zkopírujete webhooku před jeho zavřením dialogu webhooku, jak je budete potřebovat v další části.</span><span class="sxs-lookup"><span data-stu-id="1df6e-160">Make sure you copy the webhook before closing the webhook dialog as you will need this in the next section.</span></span>

## <a name="add-an-alert-to-your-virtual-machine"></a><span data-ttu-id="1df6e-161">Přidání oznámení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="1df6e-161">Add an alert to your Virtual Machine</span></span>
1. <span data-ttu-id="1df6e-162">Vyberte nastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1df6e-162">Select Virtual Machine settings</span></span>
2. <span data-ttu-id="1df6e-163">Vyberte "Pravidla výstrah"</span><span class="sxs-lookup"><span data-stu-id="1df6e-163">Select "Alert rules"</span></span>
3. <span data-ttu-id="1df6e-164">Vyberte možnost "Přidat upozornění"</span><span class="sxs-lookup"><span data-stu-id="1df6e-164">Select "Add alert"</span></span>
4. <span data-ttu-id="1df6e-165">Vyberte metriku chcete aktivovat upozornění na</span><span class="sxs-lookup"><span data-stu-id="1df6e-165">Select a metric to fire the alert on</span></span>
5. <span data-ttu-id="1df6e-166">Vyberte podmínku, která v případě splnění bude způsobit výstrahu, kterou chcete aktivovat</span><span class="sxs-lookup"><span data-stu-id="1df6e-166">Select a condition, which when fulfilled will cause the alert to fire</span></span>
6. <span data-ttu-id="1df6e-167">Prahová hodnota pro podmínku vyberte v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="1df6e-167">Select a threshold for the condition in Step 5.</span></span> <span data-ttu-id="1df6e-168">musí být splněny</span><span class="sxs-lookup"><span data-stu-id="1df6e-168">to be fulfilled</span></span>
7. <span data-ttu-id="1df6e-169">Vyberte dobu, za které bude služba monitorování zkontrolujte pro podmínky a prahovou hodnotu v kroky 5 a 6</span><span class="sxs-lookup"><span data-stu-id="1df6e-169">Select a period over which the monitoring service will check for the condition and threshold in Steps 5 & 6</span></span>
8. <span data-ttu-id="1df6e-170">Vložte webhook, které jste zkopírovali z předchozí části.</span><span class="sxs-lookup"><span data-stu-id="1df6e-170">Paste in the webhook you copied from the previous section.</span></span>

![Přidání upozornění k virtuálnímu počítači 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Přidání upozornění k virtuálnímu počítači 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

