---
title: "Integrovat Azure Automation Visual Stuido Team Services zdrojového kódu | Microsoft Docs"
description: "Scénář vás provede procesem nastavení integrace s účet Azure Automation a zdrojového kódu Visual Stuido Team Services."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: "služby VSTS, Azure powershell zdrojového kódu automatizace"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 01f9c01c9e04e02dbb548b68cf99684ba6ddd57e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a><span data-ttu-id="ea434-104">Azure Automation scénář – integrace ovládacích prvků zdrojového automatizace s Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="ea434-104">Azure Automation scenario - Automation source control integration with Visual Studio Team Services</span></span>

<span data-ttu-id="ea434-105">V tomto scénáři máte projekt Visual Studio Team Services, který používáte ke správě Azure Automation runbook nebo konfigurace DSC ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="ea434-105">In this scenario, you have a Visual Studio Team Services project that you are using to manage Azure Automation runbooks or DSC configurations under source control.</span></span>
<span data-ttu-id="ea434-106">Tento článek popisuje postup pro integraci služby VSTS prostředí Azure Automation, aby průběžnou integraci se stane, pro každý vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="ea434-106">This article describes how to integrate VSTS with your Azure Automation environment so that continuous integration happens for each check-in.</span></span>

## <a name="getting-the-scenario"></a><span data-ttu-id="ea434-107">Získání scénáře</span><span class="sxs-lookup"><span data-stu-id="ea434-107">Getting the scenario</span></span>

<span data-ttu-id="ea434-108">Tento scénář se skládá ze dvou Powershellové runbooky, které můžete importovat přímo z [Galerie Runbooků](automation-runbook-gallery.md) v portálu Azure nebo stažení [Galerie prostředí PowerShell](https://www.powershellgallery.com).</span><span class="sxs-lookup"><span data-stu-id="ea434-108">This scenario consists of two PowerShell runbooks that you can import directly from the [Runbook Gallery](automation-runbook-gallery.md) in the Azure portal or download from the [PowerShell Gallery](https://www.powershellgallery.com).</span></span>

### <a name="runbooks"></a><span data-ttu-id="ea434-109">Runbooky</span><span class="sxs-lookup"><span data-stu-id="ea434-109">Runbooks</span></span>

<span data-ttu-id="ea434-110">Runbook</span><span class="sxs-lookup"><span data-stu-id="ea434-110">Runbook</span></span> | <span data-ttu-id="ea434-111">Popis</span><span class="sxs-lookup"><span data-stu-id="ea434-111">Description</span></span>| 
--------|------------|
<span data-ttu-id="ea434-112">Služby synchronizace VSTS</span><span class="sxs-lookup"><span data-stu-id="ea434-112">Sync-VSTS</span></span> | <span data-ttu-id="ea434-113">Import sady runbook nebo konfigurace z služby VSTS zdrojového kódu, pokud se provádí vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="ea434-113">Import runbooks or configurations from VSTS source control when a check-in is done.</span></span> <span data-ttu-id="ea434-114">Je-li spustit ručně, bude import a publikovat všechny sady runbook nebo konfigurace do účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ea434-114">If run manually, it will import and publish all runbooks or configurations into the Automation account.</span></span>| 
<span data-ttu-id="ea434-115">Synchronizace VSTSGit</span><span class="sxs-lookup"><span data-stu-id="ea434-115">Sync-VSTSGit</span></span> | <span data-ttu-id="ea434-116">Import sady runbook nebo konfigurace ze služby VSTS ve správě zdrojového Git Pokud se provádí vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="ea434-116">Import runbooks or configurations from VSTS under Git source control when a check-in is done.</span></span> <span data-ttu-id="ea434-117">Je-li spustit ručně, bude import a publikovat všechny sady runbook nebo konfigurace do účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ea434-117">If run manually, it will import and publish all runbooks or configurations into the Automation account.</span></span>|

### <a name="variables"></a><span data-ttu-id="ea434-118">Proměnné</span><span class="sxs-lookup"><span data-stu-id="ea434-118">Variables</span></span>

<span data-ttu-id="ea434-119">Proměnná</span><span class="sxs-lookup"><span data-stu-id="ea434-119">Variable</span></span> | <span data-ttu-id="ea434-120">Popis</span><span class="sxs-lookup"><span data-stu-id="ea434-120">Description</span></span>|
-----------|------------|
<span data-ttu-id="ea434-121">VSToken</span><span class="sxs-lookup"><span data-stu-id="ea434-121">VSToken</span></span> | <span data-ttu-id="ea434-122">Zabezpečte variabilní prostředek, které vytvoříte, který obsahuje osobní přístupový token služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="ea434-122">Secure variable asset you will create that contains the VSTS personal access token.</span></span> <span data-ttu-id="ea434-123">Zjistěte, jak vytvořit osobní přístupový token služby VSTS na [stránka ověřování služby VSTS](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview).</span><span class="sxs-lookup"><span data-stu-id="ea434-123">You can learn how to create a VSTS personal access token on the [VSTS authentication page](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview).</span></span> 
## <a name="installing-and-configuring-this-scenario"></a><span data-ttu-id="ea434-124">Instalace a konfigurace tohoto scénáře</span><span class="sxs-lookup"><span data-stu-id="ea434-124">Installing and configuring this scenario</span></span>

<span data-ttu-id="ea434-125">Vytvoření [osobní přístupový token](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) v služby VSTS, který budete používat k synchronizaci sady runbook nebo konfigurace do vašeho účtu automation.</span><span class="sxs-lookup"><span data-stu-id="ea434-125">Create a [personal access token](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) in VSTS that you will use to sync the runbooks or configurations into your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

<span data-ttu-id="ea434-126">Vytvoření [zabezpečené proměnná](automation-variables.md) ve vašem účtu automation k uchování osobní přístupový token, aby mohli ověřit služby VSTS a synchronizace sady runbook nebo konfigurace do účtu Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="ea434-126">Create a [secure variable](automation-variables.md) in your automation account to hold the personal access token so that the runbook can authenticate to VSTS and sync the runbooks or configurations into the Automation account.</span></span> <span data-ttu-id="ea434-127">Tato VSToken můžete pojmenovat.</span><span class="sxs-lookup"><span data-stu-id="ea434-127">You can name this VSToken.</span></span> 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

<span data-ttu-id="ea434-128">Umožňuje importujte sady runbook, která se synchronizují sady runbook nebo konfigurace do účtu automation.</span><span class="sxs-lookup"><span data-stu-id="ea434-128">Import the runbook that will sync your runbooks or configurations into the automation account.</span></span> <span data-ttu-id="ea434-129">Můžete použít [ukázkové sady runbook služby VSTS](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) nebo [VSTS s Git ukázkové sady runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) z PowerShellGallery.com podle toho, pokud používáte služby VSTS zdroje služby VSTS s Gitem nebo ovládací prvek a nasazení do vašeho účtu automation.</span><span class="sxs-lookup"><span data-stu-id="ea434-129">You can use the [VSTS sample runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) or the [VSTS with Git sample runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) from the PowerShellGallery.com depending on if you use VSTS source control or VSTS with Git and deploy to your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

<span data-ttu-id="ea434-130">Teď můžete [publikování](automation-creating-importing-runbook.md#publishing-a-runbook) této sady runbook, takže si můžete vytvořit webhooku.</span><span class="sxs-lookup"><span data-stu-id="ea434-130">You can now [publish](automation-creating-importing-runbook.md#publishing-a-runbook) this runbook so you can create a webhook.</span></span> 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

<span data-ttu-id="ea434-131">Vytvoření [webhooku](automation-webhooks.md) pro tento runbook služby synchronizace VSTS a vyplňte parametry, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="ea434-131">Create a [webhook](automation-webhooks.md) for this Sync-VSTS runbook and fill in the parameters as shown below.</span></span> <span data-ttu-id="ea434-132">Ujistěte se, že zkopírujete url webhooku se nenačetla, jako je nutné pro služby háku v služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="ea434-132">Make sure you copy the webhook url as you will need it for a service hook in VSTS.</span></span> <span data-ttu-id="ea434-133">VSAccessTokenVariableName je název (VSToken) zabezpečené proměnné, kterou jste vytvořili dříve, aby udržení osobní přístupový token.</span><span class="sxs-lookup"><span data-stu-id="ea434-133">The VSAccessTokenVariableName is the name (VSToken) of the secure variable that you created earlier to hold the personal access token.</span></span> 

<span data-ttu-id="ea434-134">Integrace s služby VSTS (synchronizace-VSTS.ps1) bude trvat následující parametry.</span><span class="sxs-lookup"><span data-stu-id="ea434-134">Integrating with VSTS (Sync-VSTS.ps1) will take the following parameters.</span></span>
### <a name="sync-vsts-parameters"></a><span data-ttu-id="ea434-135">Služby synchronizace VSTS parametry</span><span class="sxs-lookup"><span data-stu-id="ea434-135">Sync-VSTS Parameters</span></span>

<span data-ttu-id="ea434-136">Parametr</span><span class="sxs-lookup"><span data-stu-id="ea434-136">Parameter</span></span> | <span data-ttu-id="ea434-137">Popis</span><span class="sxs-lookup"><span data-stu-id="ea434-137">Description</span></span>| 
--------|------------|
<span data-ttu-id="ea434-138">WebhookData</span><span class="sxs-lookup"><span data-stu-id="ea434-138">WebhookData</span></span> | <span data-ttu-id="ea434-139">To bude obsahovat informace o vrácení se změnami z hák služby VSTS služby.</span><span class="sxs-lookup"><span data-stu-id="ea434-139">This will contain the checkin information sent from the VSTS service hook.</span></span> <span data-ttu-id="ea434-140">Tento parametr by měl ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="ea434-140">You should leave this parameter blank.</span></span>| 
<span data-ttu-id="ea434-141">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ea434-141">ResourceGroup</span></span> | <span data-ttu-id="ea434-142">Toto je název skupiny prostředků, zda má účet automation v.</span><span class="sxs-lookup"><span data-stu-id="ea434-142">This is the name of the resource group that the automation account is in.</span></span>|
<span data-ttu-id="ea434-143">AutomationAccountName</span><span class="sxs-lookup"><span data-stu-id="ea434-143">AutomationAccountName</span></span> | <span data-ttu-id="ea434-144">Název účtu automation, který bude synchronizovat s služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="ea434-144">The name of the automation account that will sync with VSTS.</span></span>|
<span data-ttu-id="ea434-145">VSFolder</span><span class="sxs-lookup"><span data-stu-id="ea434-145">VSFolder</span></span> | <span data-ttu-id="ea434-146">Název složky v služby VSTS existuje sady runbook a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ea434-146">The name of the folder in VSTS where the runbooks and configurations exist.</span></span>|
<span data-ttu-id="ea434-147">VSAccount</span><span class="sxs-lookup"><span data-stu-id="ea434-147">VSAccount</span></span> | <span data-ttu-id="ea434-148">Název účtu, Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="ea434-148">The name of the Visual Studio Team Services account.</span></span>| 
<span data-ttu-id="ea434-149">VSAccessTokenVariableName</span><span class="sxs-lookup"><span data-stu-id="ea434-149">VSAccessTokenVariableName</span></span> | <span data-ttu-id="ea434-150">Název proměnné zabezpečené (VSToken), která uchovává osobní přístupový token služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="ea434-150">The name of the secure variable (VSToken) that holds the VSTS personal access token.</span></span>| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

<span data-ttu-id="ea434-151">Pokud používáte služby VSTS s GITEM (synchronizace-VSTSGit.ps1) bude trvat následující parametry.</span><span class="sxs-lookup"><span data-stu-id="ea434-151">If you are using VSTS with GIT (Sync-VSTSGit.ps1) it will take the following parameters.</span></span>

<span data-ttu-id="ea434-152">Parametr</span><span class="sxs-lookup"><span data-stu-id="ea434-152">Parameter</span></span> | <span data-ttu-id="ea434-153">Popis</span><span class="sxs-lookup"><span data-stu-id="ea434-153">Description</span></span>|
--------|------------|
<span data-ttu-id="ea434-154">WebhookData</span><span class="sxs-lookup"><span data-stu-id="ea434-154">WebhookData</span></span> | <span data-ttu-id="ea434-155">To bude obsahovat informace o vrácení se změnami z hák služby VSTS služby.</span><span class="sxs-lookup"><span data-stu-id="ea434-155">This will contain the checkin information sent from the VSTS service hook.</span></span> <span data-ttu-id="ea434-156">Tento parametr by měl ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="ea434-156">You should leave this parameter blank.</span></span>| <span data-ttu-id="ea434-157">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ea434-157">ResourceGroup</span></span> | <span data-ttu-id="ea434-158">Tento název skupiny prostředků, zda má účet automation v.</span><span class="sxs-lookup"><span data-stu-id="ea434-158">This the name of the resource group that the automation account is in.</span></span>|
<span data-ttu-id="ea434-159">AutomationAccountName</span><span class="sxs-lookup"><span data-stu-id="ea434-159">AutomationAccountName</span></span> | <span data-ttu-id="ea434-160">Název účtu automation, který bude synchronizovat s služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="ea434-160">The name of the automation account that will sync with VSTS.</span></span>|
<span data-ttu-id="ea434-161">VSAccount</span><span class="sxs-lookup"><span data-stu-id="ea434-161">VSAccount</span></span> | <span data-ttu-id="ea434-162">Název účtu, Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="ea434-162">The name of the Visual Studio Team Services account.</span></span>|
<span data-ttu-id="ea434-163">VSProject</span><span class="sxs-lookup"><span data-stu-id="ea434-163">VSProject</span></span> | <span data-ttu-id="ea434-164">Název projektu v služby VSTS existuje sady runbook a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ea434-164">The name of the project in VSTS where the runbooks and configurations exist.</span></span>|
<span data-ttu-id="ea434-165">GitRepo</span><span class="sxs-lookup"><span data-stu-id="ea434-165">GitRepo</span></span> | <span data-ttu-id="ea434-166">Název úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="ea434-166">The name of the Git repository.</span></span>|
<span data-ttu-id="ea434-167">GitBranch</span><span class="sxs-lookup"><span data-stu-id="ea434-167">GitBranch</span></span> | <span data-ttu-id="ea434-168">Název větve v úložišti služby VSTS Git.</span><span class="sxs-lookup"><span data-stu-id="ea434-168">The name of the branch in VSTS Git repository.</span></span>|
<span data-ttu-id="ea434-169">Složka</span><span class="sxs-lookup"><span data-stu-id="ea434-169">Folder</span></span> | <span data-ttu-id="ea434-170">Název složky v Git služby VSTS větev.</span><span class="sxs-lookup"><span data-stu-id="ea434-170">The name of the folder in VSTS Git branch.</span></span>|
<span data-ttu-id="ea434-171">VSAccessTokenVariableName</span><span class="sxs-lookup"><span data-stu-id="ea434-171">VSAccessTokenVariableName</span></span> | <span data-ttu-id="ea434-172">Název proměnné zabezpečené (VSToken), která uchovává osobní přístupový token služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="ea434-172">The name of the secure variable (VSToken) that holds the VSTS personal access token.</span></span>|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

<span data-ttu-id="ea434-173">Vytvoření služby háku v služby VSTS pro vrácení se změnami do složky, která aktivuje tento webhook na změnami kódu.</span><span class="sxs-lookup"><span data-stu-id="ea434-173">Create a service hook in VSTS for check-ins to the folder that triggers this webhook on code check-in.</span></span> <span data-ttu-id="ea434-174">Vyberte nástrojů Web Hooks jako službu, kterou chcete integrovat při vytváření nové předplatné.</span><span class="sxs-lookup"><span data-stu-id="ea434-174">Select Web Hooks as the service to integrate with when you create a new subscription.</span></span> <span data-ttu-id="ea434-175">Další informace o službách v [dokumentace služby VSTS služeb Jenkins](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).</span><span class="sxs-lookup"><span data-stu-id="ea434-175">You can learn more about service hooks on [VSTS Service Hooks documentation](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).</span></span>
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

<span data-ttu-id="ea434-176">Teď by měl být schopen provést všechny vrácení se změnami runbooky a konfiguraci do služby VSTS a automaticky se tyto synchronizace 'd do vašeho účtu automation.</span><span class="sxs-lookup"><span data-stu-id="ea434-176">You should now be able to do all check-ins of your runbooks and configurations into VSTS and have these automatically sync’d into your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

<span data-ttu-id="ea434-177">Pokud ručně spuštění této sady runbook bez se aktivuje pomocí služby VSTS parametr webhookdata můžete nechat prázdné a ve složce služby VSTS zadané akce provede úplnou synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="ea434-177">If you run this runbook manually without being triggered by VSTS, you can leave the webhookdata parameter empty and it will do a full sync from the VSTS folder specified.</span></span>

<span data-ttu-id="ea434-178">Pokud chcete odinstalovat tento scénář, odeberte hák služby ze služby VSTS, odstraňte sadu runbook a proměnná VSToken.</span><span class="sxs-lookup"><span data-stu-id="ea434-178">If you wish to uninstall the scenario, remove the service hook from VSTS, delete the runbook, and the VSToken variable.</span></span>
