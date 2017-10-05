---
title: "Přidat do plánů obnovení portálu classic runbooky služby Azure automation | Microsoft Docs"
description: "Tento článek popisuje, jak Azure Site Recovery teď můžete rozšířit plány obnovení pro dokončení složité úlohy během obnovení do Azure pomocí Azure Automation."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0a248e7c3f39a35ac10dc6ac64e5cef7d152e033
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans-in-the-classic-portal"></a><span data-ttu-id="17ced-103">Přidat do plánů obnovení portálu classic runbooky služby Azure automation</span><span class="sxs-lookup"><span data-stu-id="17ced-103">Add Azure automation runbooks to recovery plans in the classic portal</span></span>
<span data-ttu-id="17ced-104">Tento kurz popisuje, jak Azure Site Recovery integruje se službou Azure Automation zajistí možnosti rozšíření do plánů obnovení.</span><span class="sxs-lookup"><span data-stu-id="17ced-104">This tutorial describes how Azure Site Recovery integrates with Azure Automation to provide extensibility to recovery plans.</span></span> <span data-ttu-id="17ced-105">Plány obnovení můžete orchestraci obnovení virtuální počítače chráněné pomocí Azure Site Recovery pro replikace do sekundární cloudu a replikaci do Azure scénáře.</span><span class="sxs-lookup"><span data-stu-id="17ced-105">Recovery plans can orchestrate recovery of your virtual machines protected using Azure Site Recovery for both replication to secondary cloud and replication to Azure scenarios.</span></span> <span data-ttu-id="17ced-106">Také pomoci při obnovení **přesné**, **repeatable**, a **automatizované**.</span><span class="sxs-lookup"><span data-stu-id="17ced-106">They also help in making the recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="17ced-107">Pokud jsou selhání virtuálních počítačů do Azure, integraci s Azure Automation rozšiřuje plány obnovy a nabízí možnost spuštění sady runbook, což umožňuje efektivní automatizace úloh.</span><span class="sxs-lookup"><span data-stu-id="17ced-107">If you are failing over your virtual machines to Azure, integration with Azure Automation extends the recovery plans and gives you capability to execute runbooks, thus allowing powerful automation tasks.</span></span>

<span data-ttu-id="17ced-108">Pokud jste ještě se dozvěděli o službě Azure Automation ještě, zaregistrujte si [sem](https://azure.microsoft.com/services/automation/) a stáhnout jejich ukázkové skripty [zde](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="17ced-108">If you have not heard about Azure Automation yet, sign up [here](https://azure.microsoft.com/services/automation/) and download their sample scripts [here](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="17ced-109">Další informace o [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) a orchestraci obnovení do Azure pomocí plánů obnovení [zde](https://azure.microsoft.com/blog/?p=166264).</span><span class="sxs-lookup"><span data-stu-id="17ced-109">Read more about [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) and how to orchestrate recovery to Azure using recovery plans [here](https://azure.microsoft.com/blog/?p=166264).</span></span>

<span data-ttu-id="17ced-110">V tomto kurzu krátké se podíváme na tom, jak můžete integrovat Azure Automation runbook do plánů obnovení.</span><span class="sxs-lookup"><span data-stu-id="17ced-110">In this short tutorial, we will look at how you can integrate Azure Automation runbooks into recovery plans.</span></span> <span data-ttu-id="17ced-111">Nemůžeme se automatizovat jednoduché úkolů, které dříve vyžadují ruční zásah a zjistit, jak převést více krok obnovení do akce obnovení jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="17ced-111">We will automate simple tasks that earlier required manual intervention and see how to convert a multi step recovery into a single-click recovery action.</span></span> <span data-ttu-id="17ced-112">Podíváme se také na řešení k jednoduchého skriptu, pokud se nepovede.</span><span class="sxs-lookup"><span data-stu-id="17ced-112">We will also look at how you can troubleshoot a simple script if it goes wrong.</span></span>

## <a name="protect-the-application-to-azure"></a><span data-ttu-id="17ced-113">Chránit aplikaci do Azure</span><span class="sxs-lookup"><span data-stu-id="17ced-113">Protect the application to Azure</span></span>
<span data-ttu-id="17ced-114">Dejte nám začněte jednoduché aplikace, která obsahuje dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="17ced-114">Let us begin with a simple application consisting of two virtual machines.</span></span> <span data-ttu-id="17ced-115">Zde máme aplikace HRweb ze společnosti Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="17ced-115">Here, we have a HRweb application of Fabrikam.</span></span> <span data-ttu-id="17ced-116">Společnost Fabrikam. HRweb front-end a back-Fabrikam-Hrweb end jsou dva virtuální počítače chráněné na Azure pomocí Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="17ced-116">Fabrikam-HRweb-frontend and Fabrikam-Hrweb-backend are the two virtual machines protected to Azure using Azure Site Recovery.</span></span> <span data-ttu-id="17ced-117">K ochraně virtuálních počítačů pomocí Azure Site Recovery, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="17ced-117">To protect the virtual machines using Azure Site Recovery, follow the steps below.</span></span>

1. <span data-ttu-id="17ced-118">Povolení ochrany pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="17ced-118">Enable protection for your virtual machines.</span></span>
2. <span data-ttu-id="17ced-119">Zajistěte, aby virtuální počítače dokončit počáteční replikaci a jsou replikace.</span><span class="sxs-lookup"><span data-stu-id="17ced-119">Ensure that the virtual machines have completed initial replication and are replicating.</span></span>
3. <span data-ttu-id="17ced-120">Počkejte na dokončení počáteční replikace a stavu replikace uvádí chráněné.</span><span class="sxs-lookup"><span data-stu-id="17ced-120">Wait till the initial replication completes and the Replication status says Protected.</span></span>

## ![](media/site-recovery-runbook-automation/01.png)
<span data-ttu-id="17ced-121">V tomto kurzu vytvoříme plán obnovení pro aplikace Fabrikam HRweb převzetí služeb při selhání aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="17ced-121">In this tutorial, we will create a recovery plan for the Fabrikam HRweb application to failover the application to Azure.</span></span> <span data-ttu-id="17ced-122">Pak budeme ji integrovat s sadu runbook, která vytvoří koncový bod na neúspěšný přes virtuální počítač Azure k obsluze webových stránek na portu 80.</span><span class="sxs-lookup"><span data-stu-id="17ced-122">Then we will integrate it with a runbook that will create an endpoint on the failed over Azure virtual machine to serve web pages at port 80.</span></span>

<span data-ttu-id="17ced-123">Nejdříve vytvoříme plán obnovení pro naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17ced-123">First, let's create a recovery plan for our application.</span></span>

## <a name="create-the-recovery-plan"></a><span data-ttu-id="17ced-124">Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="17ced-124">Create the recovery plan</span></span>
<span data-ttu-id="17ced-125">Chcete-li obnovit aplikaci do Azure, vytvořte plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="17ced-125">To recover the application to Azure, you need to create a recovery plan.</span></span>
<span data-ttu-id="17ced-126">Pomocí plán obnovení, že které lze nastavit pořadí obnovení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="17ced-126">Using a recovery plan you can specify the order of recovery of the virtual machines.</span></span> <span data-ttu-id="17ced-127">Virtuální počítač ve skupině 1 umístit bude obnovit a spusťte první a postupujte virtuálního počítače ve skupině 2.</span><span class="sxs-lookup"><span data-stu-id="17ced-127">The virtual machine placed in group 1 will recover and start first, and then the virtual machine in group 2 will follow.</span></span>

<span data-ttu-id="17ced-128">Vytvořte do plánu obnovení, který může vypadat níže.</span><span class="sxs-lookup"><span data-stu-id="17ced-128">Create a Recovery Plan that looks like below.</span></span>

![](media/site-recovery-runbook-automation/12.png)

<span data-ttu-id="17ced-129">Další informace o plánech obnovení, přečtěte si dokumentaci [sem](https://msdn.microsoft.com/library/azure/dn788799.aspx "zde").</span><span class="sxs-lookup"><span data-stu-id="17ced-129">To read more about recovery plans, read documentation [here](https://msdn.microsoft.com/library/azure/dn788799.aspx "here").</span></span>

<span data-ttu-id="17ced-130">V dalším kroku vytvoříme nezbytné artefakty ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="17ced-130">Next, let's create the necessary artifacts in Azure Automation.</span></span>

## <a name="create-the-automation-account-and-its-assets"></a><span data-ttu-id="17ced-131">Vytvoření účtu automation a její prostředky</span><span class="sxs-lookup"><span data-stu-id="17ced-131">Create the automation account and its assets</span></span>
<span data-ttu-id="17ced-132">Potřebujete účet Azure Automation pro vytváření runbooků.</span><span class="sxs-lookup"><span data-stu-id="17ced-132">You need an Azure Automation account to create runbooks.</span></span> <span data-ttu-id="17ced-133">Pokud již účet nemáte, přejděte na kartu Azure Automation označený jako ![](media/site-recovery-runbook-automation/02.png)a vytvořit nový účet.</span><span class="sxs-lookup"><span data-stu-id="17ced-133">If you do not already have an account, navigate to Azure Automation tab denoted by ![](media/site-recovery-runbook-automation/02.png)and create a new account.</span></span>

1. <span data-ttu-id="17ced-134">Pojmenujte účet můžete identifikovat se.</span><span class="sxs-lookup"><span data-stu-id="17ced-134">Give the account a name to identify with.</span></span>
2. <span data-ttu-id="17ced-135">Zadejte geografické oblasti, ve které chcete umístit účet.</span><span class="sxs-lookup"><span data-stu-id="17ced-135">Specify a geographical region where you want to place the account.</span></span>

<span data-ttu-id="17ced-136">Doporučujeme umístit účet ve stejné oblasti jako trezor automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="17ced-136">It is recommended to place the account in the same region as the ASR vault.</span></span>

![](media/site-recovery-runbook-automation/03.png)

<span data-ttu-id="17ced-137">Dále vytvořte následující prostředky v účtu.</span><span class="sxs-lookup"><span data-stu-id="17ced-137">Next, create the following assets in the Account.</span></span>

### <a name="add-a-subscription-name-as-asset"></a><span data-ttu-id="17ced-138">Přidejte název odběru jako prostředek</span><span class="sxs-lookup"><span data-stu-id="17ced-138">Add a subscription name as asset</span></span>
1. <span data-ttu-id="17ced-139">Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v Azure Automation prostředky a vyberte k![](media/site-recovery-runbook-automation/05.png)</span><span class="sxs-lookup"><span data-stu-id="17ced-139">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in the Azure Automation Assets and select to ![](media/site-recovery-runbook-automation/05.png)</span></span>
2. <span data-ttu-id="17ced-140">Vyberte typ proměnné jako **řetězec**</span><span class="sxs-lookup"><span data-stu-id="17ced-140">Select the variable type as **String**</span></span>
3. <span data-ttu-id="17ced-141">Zadejte název proměnné jako **AzureSubscriptionName**</span><span class="sxs-lookup"><span data-stu-id="17ced-141">Specify variable name as **AzureSubscriptionName**</span></span>

   ![](media/site-recovery-runbook-automation/06.png)
4. <span data-ttu-id="17ced-142">Jako hodnotu proměnné, zadejte skutečný název předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="17ced-142">Specify your actual Azure Subscription name as the variable value.</span></span>

   ![](media/site-recovery-runbook-automation/07_1.png)

<span data-ttu-id="17ced-143">Můžete určit název vaše předplatné na stránce nastavení svého účtu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="17ced-143">You can identify the name of your subscription from the settings page of your account on the Azure portal.</span></span>

### <a name="add-an-azure-login-credential-as-asset"></a><span data-ttu-id="17ced-144">Přidat jako asset přihlašovacích údajů přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="17ced-144">Add an Azure login credential as asset</span></span>
<span data-ttu-id="17ced-145">Automatizace Azure používá pro připojení k předplatnému Azure PowerShell a funguje na artefakty existuje.</span><span class="sxs-lookup"><span data-stu-id="17ced-145">Azure Automation uses Azure PowerShell to connect to the subscription and operates on the artifacts there.</span></span> <span data-ttu-id="17ced-146">V takovém případě budete muset ověřit pomocí účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="17ced-146">For this, you need to authenticate using your Microsoft account or a work or school account.</span></span>
<span data-ttu-id="17ced-147">Přihlašovací údaje účtu můžete uložit ve prostředek má být bezpečně používána sady runbook.</span><span class="sxs-lookup"><span data-stu-id="17ced-147">You can store the account credentials in an asset to be used securely by the runbook.</span></span>

1. <span data-ttu-id="17ced-148">Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v Azure Automation prostředky a vyberte![](media/site-recovery-runbook-automation/09.png)</span><span class="sxs-lookup"><span data-stu-id="17ced-148">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in the Azure Automation Assets and select ![](media/site-recovery-runbook-automation/09.png)</span></span>
2. <span data-ttu-id="17ced-149">Vyberte typ přihlašovacích údajů jako **přihlašovací údaje Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="17ced-149">Select the Credential type as **Windows PowerShell Credential**</span></span>
3. <span data-ttu-id="17ced-150">Zadejte název **AzureCredential**</span><span class="sxs-lookup"><span data-stu-id="17ced-150">Specify the name as **AzureCredential**</span></span>

   ![](media/site-recovery-runbook-automation/10.png)
4. <span data-ttu-id="17ced-151">Zadejte uživatelské jméno a heslo k přihlášení s.</span><span class="sxs-lookup"><span data-stu-id="17ced-151">Specify the username and password to sign-in with.</span></span>

<span data-ttu-id="17ced-152">Obě tato nastavení jsou nyní k dispozici v vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="17ced-152">Now both these settings are available in your assets.</span></span>

![](media/site-recovery-runbook-automation/11.png)

<span data-ttu-id="17ced-153">Další informace o tom, jak se připojit k předplatnému pomocí prostředí PowerShell jsou uvedeny [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="17ced-153">More information about how to connect to your subscription via PowerShell is given [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="17ced-154">V dalším kroku vytvoříte sady runbook ve službě Azure Automation, které můžete přidat koncový bod pro front-endu virtuální počítač po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="17ced-154">Next, you will create a runbook in Azure Automation that can add an endpoint for the front-end virtual machine after failover.</span></span>

## <a name="azure-automation-context"></a><span data-ttu-id="17ced-155">Kontext služby Azure automation</span><span class="sxs-lookup"><span data-stu-id="17ced-155">Azure automation context</span></span>
<span data-ttu-id="17ced-156">Automatické obnovení systému předá kontextové proměnné do runbooku usnadňuje psaní skriptů deterministický.</span><span class="sxs-lookup"><span data-stu-id="17ced-156">ASR passes a context variable to the runbook to help you write deterministic scripts.</span></span> <span data-ttu-id="17ced-157">Jeden může uvádějí, že jsou předvídatelný názvy cloudové služby a virtuální počítač, ale se stane, že není vždy případ vyplývající z určité scénáře, jako je ta, kde název název virtuálního počítače může změnily kvůli nepodporované znaky v Azure.</span><span class="sxs-lookup"><span data-stu-id="17ced-157">One could argue that the names of the Cloud Service and the Virtual Machine are predictable, but happens that it is not always the case owing to certain scenarios such as the one where the name of the virtual machine name might have changed due to unsupported characters in Azure.</span></span> <span data-ttu-id="17ced-158">Proto se předá tyto informace do plánu obnovení automatické obnovení systému jako součást *kontextu*.</span><span class="sxs-lookup"><span data-stu-id="17ced-158">Hence this information is passed to the ASR recovery plan as part of the *context*.</span></span>

<span data-ttu-id="17ced-159">Níže je příklad, jak vypadá kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="17ced-159">Below is an example of how the context variable looks.</span></span>

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


<span data-ttu-id="17ced-160">Následující tabulka obsahuje název a popis pro každou proměnnou v kontextu.</span><span class="sxs-lookup"><span data-stu-id="17ced-160">The table below contains name and description for each variable in the context.</span></span>

| <span data-ttu-id="17ced-161">**Název proměnné**</span><span class="sxs-lookup"><span data-stu-id="17ced-161">**Variable name**</span></span> | <span data-ttu-id="17ced-162">**Popis**</span><span class="sxs-lookup"><span data-stu-id="17ced-162">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="17ced-163">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="17ced-163">RecoveryPlanName</span></span> |<span data-ttu-id="17ced-164">Název plánu spuštěn.</span><span class="sxs-lookup"><span data-stu-id="17ced-164">Name of plan being run.</span></span> <span data-ttu-id="17ced-165">Umožňuje provést akci na základě názvu pomocí stejného skriptu</span><span class="sxs-lookup"><span data-stu-id="17ced-165">Helps you take action based on name using the same script</span></span> |
| <span data-ttu-id="17ced-166">FailoverType</span><span class="sxs-lookup"><span data-stu-id="17ced-166">FailoverType</span></span> |<span data-ttu-id="17ced-167">Určuje, zda je převzetí služeb při selhání otestovat, plánovaná nebo neplánovaná.</span><span class="sxs-lookup"><span data-stu-id="17ced-167">Specifies whether the failover is test, planned, or unplanned.</span></span> |
| <span data-ttu-id="17ced-168">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="17ced-168">FailoverDirection</span></span> |<span data-ttu-id="17ced-169">Určete, zda je primární nebo sekundární obnovení</span><span class="sxs-lookup"><span data-stu-id="17ced-169">Specify whether recovery is to primary or secondary</span></span> |
| <span data-ttu-id="17ced-170">GroupID</span><span class="sxs-lookup"><span data-stu-id="17ced-170">GroupID</span></span> |<span data-ttu-id="17ced-171">Identifikovat číslo skupiny v rámci plánu obnovení, když běží plánu</span><span class="sxs-lookup"><span data-stu-id="17ced-171">Identify the group number within the recovery plan when the plan is running</span></span> |
| <span data-ttu-id="17ced-172">VmMap</span><span class="sxs-lookup"><span data-stu-id="17ced-172">VmMap</span></span> |<span data-ttu-id="17ced-173">Pole všech virtuálních počítačů ve skupině</span><span class="sxs-lookup"><span data-stu-id="17ced-173">Array of all the virtual machines in the group</span></span> |
| <span data-ttu-id="17ced-174">Klíč VMMap</span><span class="sxs-lookup"><span data-stu-id="17ced-174">VMMap key</span></span> |<span data-ttu-id="17ced-175">Jedinečný klíč (GUID) pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="17ced-175">Unique key (GUID) for each VM.</span></span> <span data-ttu-id="17ced-176">Případně je stejný jako Identifikátor VMM virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17ced-176">It's the same as the VMM ID of the virtual machine where applicable.</span></span> |
| <span data-ttu-id="17ced-177">RoleName</span><span class="sxs-lookup"><span data-stu-id="17ced-177">RoleName</span></span> |<span data-ttu-id="17ced-178">Název virtuálního počítače Azure, který obnovuje</span><span class="sxs-lookup"><span data-stu-id="17ced-178">Name of the Azure VM that's being recovered</span></span> |
| <span data-ttu-id="17ced-179">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="17ced-179">CloudServiceName</span></span> |<span data-ttu-id="17ced-180">Azure název cloudové služby, pod kterou je vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="17ced-180">Azure Cloud Service name under which the virtual machine is created.</span></span> |

<span data-ttu-id="17ced-181">K identifikaci VmMap klíče v kontextu, můžete také přejít na stránku vlastností virtuálního počítače v nástroji automatické obnovení systému a podívejte se na vlastnost identifikátor GUID virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17ced-181">To identify the VmMap Key in the context you could also go to the VM properties page in ASR and look at the VM GUID property.</span></span>

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a><span data-ttu-id="17ced-182">Autor runbook služby Automation.</span><span class="sxs-lookup"><span data-stu-id="17ced-182">Author an Automation runbook</span></span>
<span data-ttu-id="17ced-183">Teď vytvoříte runbook otevřít port 80 na front-endu virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="17ced-183">Now create the runbook to open port 80 on the front-end virtual machine.</span></span>

1. <span data-ttu-id="17ced-184">Vytvořit novou sadu runbook v účtu Azure Automation s názvem **OpenPort80**</span><span class="sxs-lookup"><span data-stu-id="17ced-184">Create a new runbook in the Azure Automation account with the name **OpenPort80**</span></span>

   ![](media/site-recovery-runbook-automation/14.png)
2. <span data-ttu-id="17ced-185">Přejděte do zobrazení Autor sady runbook a zadejte režimu konceptu.</span><span class="sxs-lookup"><span data-stu-id="17ced-185">Navigate to the Author view of the runbook and enter the draft mode.</span></span>
3. <span data-ttu-id="17ced-186">Nejdřív zadejte proměnnou použít jako kontext plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="17ced-186">First specify the variable to use as the recovery plan context</span></span>

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. <span data-ttu-id="17ced-187">Další připojení k předplatnému pomocí přihlašovacích údajů a předplatné názvu</span><span class="sxs-lookup"><span data-stu-id="17ced-187">Next connect to the subscription using the credential and subscription name</span></span>

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect to Azure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   <span data-ttu-id="17ced-188">Všimněte si, že používáte Azure prostředky – **AzureCredential** a **AzureSubscriptionName** sem.</span><span class="sxs-lookup"><span data-stu-id="17ced-188">Note that you use the Azure assets – **AzureCredential** and **AzureSubscriptionName** here.</span></span>
5. <span data-ttu-id="17ced-189">Teď určete koncový bod podrobnosti a identifikátor GUID virtuálního počítače, pro kterou chcete vystavit koncový bod.</span><span class="sxs-lookup"><span data-stu-id="17ced-189">Now specify the endpoint details and the GUID of the virtual machine for which you want to expose the endpoint.</span></span> <span data-ttu-id="17ced-190">V tomto případě front-endu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17ced-190">In this case the front-end virtual machine.</span></span>

   ```
       # Specify the parameters to be used by the script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   <span data-ttu-id="17ced-191">Určuje protokol koncového bodu Azure, místní port ve virtuálním počítači a jeho namapované veřejný port.</span><span class="sxs-lookup"><span data-stu-id="17ced-191">This specifies the Azure endpoint protocol, local port on the VM and its mapped public port.</span></span> <span data-ttu-id="17ced-192">Tyto proměnné jsou parametry, které vyžadují Azure příkazy, které přidat koncové body k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="17ced-192">These variables are parameters     required by the Azure commands that add endpoints to VMs.</span></span> <span data-ttu-id="17ced-193">VMGUID obsahuje identifikátor GUID virtuálního počítače, které potřebujete pracovat.</span><span class="sxs-lookup"><span data-stu-id="17ced-193">The VMGUID holds the GUID of the virtual machine you need to operate on.</span></span>
6. <span data-ttu-id="17ced-194">Skript se teď extrahování kontext pro daný identifikátor GUID virtuálního počítače a vytvořit koncový bod na virtuálním počítači, který se odkazuje.</span><span class="sxs-lookup"><span data-stu-id="17ced-194">The script will now extract the context for the given VM GUID and create an endpoint on the virtual machine referenced by it.</span></span>

   ```
       #Read the VM GUID from the context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. <span data-ttu-id="17ced-195">Po dokončení, stiskněte tlačítko Publikovat ![](media/site-recovery-runbook-automation/20.png) umožňující váš skript k dispozici pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="17ced-195">Once this is complete, hit Publish ![](media/site-recovery-runbook-automation/20.png) to allow your script to be available for execution.</span></span>

<span data-ttu-id="17ced-196">Dále je pro vaši informaci uveden dokončení skriptu</span><span class="sxs-lookup"><span data-stu-id="17ced-196">The complete script is given below for your reference</span></span>

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a><span data-ttu-id="17ced-197">Přidejte skript do plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="17ced-197">Add the script to the recovery plan</span></span>
<span data-ttu-id="17ced-198">Až tento skript je připraven, můžete ho přidat do plánu obnovení, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="17ced-198">Once the script is ready, you can add it to the recovery plan that you created earlier.</span></span>

1. <span data-ttu-id="17ced-199">V plánu obnovení, kterou jste vytvořili vyberte skript přidáte tak po skupiny 2.</span><span class="sxs-lookup"><span data-stu-id="17ced-199">In the recovery plan you created, choose to add a script after the group 2.</span></span> ![](media/site-recovery-runbook-automation/15.png)
2. <span data-ttu-id="17ced-200">Zadejte název skriptu.</span><span class="sxs-lookup"><span data-stu-id="17ced-200">Specify a script name.</span></span> <span data-ttu-id="17ced-201">Toto je právě popisný název pro tento skript pro zobrazení v rámci plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="17ced-201">This is just a friendly name for this script for showing within the Recovery plan.</span></span>
3. <span data-ttu-id="17ced-202">V převzetí služeb při selhání do Azure skriptu – vyberte název účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="17ced-202">In the failover to Azure script – Select the Azure Automation Account name.</span></span>
4. <span data-ttu-id="17ced-203">V sadách Runbook Azure vyberte sadu runbook, vámi vytvořený.</span><span class="sxs-lookup"><span data-stu-id="17ced-203">In the Azure Runbooks, select the runbook you authored.</span></span>

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a><span data-ttu-id="17ced-204">Primární straně skripty</span><span class="sxs-lookup"><span data-stu-id="17ced-204">Primary side scripts</span></span>
<span data-ttu-id="17ced-205">Když jsou prováděny převzetí služeb při selhání do Azure, můžete také spustit skripty primární straně.</span><span class="sxs-lookup"><span data-stu-id="17ced-205">When you are executing a failover to Azure, you can also choose to execute primary side scripts.</span></span> <span data-ttu-id="17ced-206">Tyto skripty se spustí na serveru VMM během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="17ced-206">These scripts will run on the VMM server during failover.</span></span>
<span data-ttu-id="17ced-207">Primární straně skriptů jsou k dispozici pouze pro před vypnutím pouze a post vypnutí fázích.</span><span class="sxs-lookup"><span data-stu-id="17ced-207">Primary side scripts are only available only for pre-shutdown and post shutdown stages.</span></span> <span data-ttu-id="17ced-208">Toto je očekávané být obvykle není k dispozici při havárii narazilo primární lokality.</span><span class="sxs-lookup"><span data-stu-id="17ced-208">This is because we expect the primary site to be typically unavailable when a disaster strikes.</span></span>
<span data-ttu-id="17ced-209">Během neplánované převzetí služeb při selhání pouze v případě, že můžete vyjádřit výslovný souhlas pro operace primární lokality, pokusí se spustit skripty primární straně.</span><span class="sxs-lookup"><span data-stu-id="17ced-209">During an unplanned failover, only if you opt in for primary site operations, it will attempt to run the primary side scripts.</span></span> <span data-ttu-id="17ced-210">Pokud nejsou dosažitelné nebo vypršel časový limit, bude pokračovat převzetí služeb při selhání obnovení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="17ced-210">If they are not reachable or timeout, the failover will continue to recover the virtual machines.</span></span>
<span data-ttu-id="17ced-211">Primární straně skripty jsou zrušení dostupnou pro servery VMware nebo fyzických/Hyper-v bez VMM chráněné na Azure - při můžete převzetí služeb při selhání do Azure.</span><span class="sxs-lookup"><span data-stu-id="17ced-211">Primary side scripts are un-available for VMware/Physical/Hyper-v Sites without VMM protected to Azure - while you failover to Azure.</span></span>
<span data-ttu-id="17ced-212">Ale když můžete navrácení služeb po obnovení z Azure na primární straně skripty (sady Runbook) na místě slouží pro všechny cíle kromě VMware.</span><span class="sxs-lookup"><span data-stu-id="17ced-212">However, when you failback from Azure to on-premises, primary side scripts (Runbooks) can be used for all targets except VMware.</span></span>

## <a name="test-the-recovery-plan"></a><span data-ttu-id="17ced-213">Testovací plán obnovení</span><span class="sxs-lookup"><span data-stu-id="17ced-213">Test the recovery plan</span></span>
<span data-ttu-id="17ced-214">Po přidání sady runbook k plánu, můžete zahájit testovací převzetí služeb a pozorování v akci.</span><span class="sxs-lookup"><span data-stu-id="17ced-214">Once you have added the runbook to the plan you can initiate a test failover and see it in action.</span></span> <span data-ttu-id="17ced-215">Doporučujeme vždy spustit převzetí služeb při selhání pro testování vaší aplikace a plán obnovení tak, ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="17ced-215">It is always recommended to run a test failover to test your application and the recovery plan to ensure that there are no errors.</span></span>

1. <span data-ttu-id="17ced-216">Vyberte plán obnovení a spustit testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="17ced-216">Select the recovery plan and initiate a test failover.</span></span>
2. <span data-ttu-id="17ced-217">Během provádění plánu můžete zjistit, zda sada runbook má provést, nebo nikoli prostřednictvím jeho stav.</span><span class="sxs-lookup"><span data-stu-id="17ced-217">During the plan execution, you can see whether the runbook has executed or not via its status.</span></span>

   ![](media/site-recovery-runbook-automation/17.png)
3. <span data-ttu-id="17ced-218">Stav provádění podrobné runbook můžete zobrazit také na stránce úlohy automatizace Azure pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="17ced-218">You can also see the detailed runbook execution status on the Azure Automation jobs page for the runbook.</span></span>

   ![](media/site-recovery-runbook-automation/18.png)
4. <span data-ttu-id="17ced-219">Po dokončení převzetí kromě výsledku spuštění runbooku, uvidíte, zda je úspěšné provedení nebo není na stránce virtuální počítač Azure a prohlížení koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="17ced-219">After the failover completes, apart from the runbook execution result, you can see whether the execution is successful or not by visiting the Azure virtual machine page and looking at the endpoints.</span></span>

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a><span data-ttu-id="17ced-220">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="17ced-220">Sample scripts</span></span>
<span data-ttu-id="17ced-221">Když jsme projít automatizaci jeden běžně používat úkolu přidávání koncového bodu pro virtuální počítač Azure v tomto kurzu, můžete to udělat několika další výkonné automatizované úlohy pomocí služby Azure automation.</span><span class="sxs-lookup"><span data-stu-id="17ced-221">While we walked through automating one commonly used task of adding an endpoint to an Azure virtual machine in this tutorial, you could do a number of other powerful automation tasks using Azure automation.</span></span> <span data-ttu-id="17ced-222">Microsoft a její komunitě Azure Automation poskytují ukázkové sady runbook, které vám pomůžou začít vytvářet vlastní řešení a sady runbook nástroj, který můžete použít jako stavební bloky pro větší úlohy automatizace.</span><span class="sxs-lookup"><span data-stu-id="17ced-222">Microsoft and the Azure Automation community provide sample runbooks which can help you get started creating your own solutions, and utility runbooks, which you can use as building blocks for larger automation tasks.</span></span> <span data-ttu-id="17ced-223">Začít používat je z galerie a vytvářet plány obnovení výkonné jedním kliknutím pro vaše aplikace pomocí Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="17ced-223">Start using them from the gallery and build  powerful one-click recovery plans for your applications using Azure Site Recovery.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17ced-224">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="17ced-224">Additional Resources</span></span>
[<span data-ttu-id="17ced-225">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="17ced-225">Azure Automation Overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Přehled služby Azure Automation")

[<span data-ttu-id="17ced-226">Ukázkové skripty pro automatizaci Azure</span><span class="sxs-lookup"><span data-stu-id="17ced-226">Sample Azure Automation Scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty služby Azure Automation")
