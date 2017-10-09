---
title: "aaaAdd služby Azure automation runbook toorecovery plány portálu classic hello | Microsoft Docs"
description: "Tento článek popisuje, jak Azure Site Recovery teď můžete pomocí Azure Automation toocomplete složité úlohy během obnovení tooAzure plány obnovení tooextend"
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
ms.openlocfilehash: 3bb7420911afbce289b656f28823b1923e8af0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans-in-hello-classic-portal"></a><span data-ttu-id="9de14-103">Přidat plány toorecovery sady runbook služby Azure automation na portálu classic hello</span><span class="sxs-lookup"><span data-stu-id="9de14-103">Add Azure automation runbooks toorecovery plans in hello classic portal</span></span>
<span data-ttu-id="9de14-104">Tento kurz popisuje, jak Azure Site Recovery integruje se službou Azure Automation tooprovide rozšiřitelnost toorecovery plány.</span><span class="sxs-lookup"><span data-stu-id="9de14-104">This tutorial describes how Azure Site Recovery integrates with Azure Automation tooprovide extensibility toorecovery plans.</span></span> <span data-ttu-id="9de14-105">Plány obnovení můžete orchestraci obnovení chránit pomocí Azure Site Recovery pro cloud toosecondary replikace a scénáře tooAzure replikace virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9de14-105">Recovery plans can orchestrate recovery of your virtual machines protected using Azure Site Recovery for both replication toosecondary cloud and replication tooAzure scenarios.</span></span> <span data-ttu-id="9de14-106">Také pomoci při obnovení hello **přesné**, **repeatable**, a **automatizované**.</span><span class="sxs-lookup"><span data-stu-id="9de14-106">They also help in making hello recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="9de14-107">Pokud jsou selhání tooAzure vaše virtuální počítače, integraci s Azure Automation rozšiřuje plány obnovení a poskytuje schopnost tooexecute sady runbook, což umožňuje efektivní automatizace úloh.</span><span class="sxs-lookup"><span data-stu-id="9de14-107">If you are failing over your virtual machines tooAzure, integration with Azure Automation extends the recovery plans and gives you capability tooexecute runbooks, thus allowing powerful automation tasks.</span></span>

<span data-ttu-id="9de14-108">Pokud jste ještě se dozvěděli o službě Azure Automation ještě, zaregistrujte si [sem](https://azure.microsoft.com/services/automation/) a stáhnout jejich ukázkové skripty [zde](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="9de14-108">If you have not heard about Azure Automation yet, sign up [here](https://azure.microsoft.com/services/automation/) and download their sample scripts [here](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="9de14-109">Další informace o [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) a způsob, jak tooAzure tooorchestrate obnovení pomocí obnovení plánuje [zde](https://azure.microsoft.com/blog/?p=166264).</span><span class="sxs-lookup"><span data-stu-id="9de14-109">Read more about [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) and how tooorchestrate recovery tooAzure using recovery plans [here](https://azure.microsoft.com/blog/?p=166264).</span></span>

<span data-ttu-id="9de14-110">V tomto kurzu krátké se podíváme na tom, jak můžete integrovat Azure Automation runbook do plánů obnovení.</span><span class="sxs-lookup"><span data-stu-id="9de14-110">In this short tutorial, we will look at how you can integrate Azure Automation runbooks into recovery plans.</span></span> <span data-ttu-id="9de14-111">Nemůžeme se automatizovat jednoduché úkolů, které dříve vyžadují ruční zásah a najdete v části Jak tooconvert více krok obnovení do akce obnovení jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="9de14-111">We will automate simple tasks that earlier required manual intervention and see how tooconvert a multi step recovery into a single-click recovery action.</span></span> <span data-ttu-id="9de14-112">Podíváme se také na řešení k jednoduchého skriptu, pokud se nepovede.</span><span class="sxs-lookup"><span data-stu-id="9de14-112">We will also look at how you can troubleshoot a simple script if it goes wrong.</span></span>

## <a name="protect-hello-application-tooazure"></a><span data-ttu-id="9de14-113">Chránit tooAzure aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9de14-113">Protect hello application tooAzure</span></span>
<span data-ttu-id="9de14-114">Dejte nám začněte jednoduché aplikace, která obsahuje dva virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9de14-114">Let us begin with a simple application consisting of two virtual machines.</span></span> <span data-ttu-id="9de14-115">Zde máme aplikace HRweb ze společnosti Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="9de14-115">Here, we have a HRweb application of Fabrikam.</span></span> <span data-ttu-id="9de14-116">Společnost Fabrikam. HRweb front-end a back-Fabrikam-Hrweb end jsou hello dva virtuální počítače chráněné tooAzure pomocí Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9de14-116">Fabrikam-HRweb-frontend and Fabrikam-Hrweb-backend are hello two virtual machines protected tooAzure using Azure Site Recovery.</span></span> <span data-ttu-id="9de14-117">tooprotect hello virtuálních počítačů pomocí Azure Site Recovery, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="9de14-117">tooprotect hello virtual machines using Azure Site Recovery, follow hello steps below.</span></span>

1. <span data-ttu-id="9de14-118">Povolení ochrany pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9de14-118">Enable protection for your virtual machines.</span></span>
2. <span data-ttu-id="9de14-119">Zajistěte, aby virtuální počítače hello dokončit počáteční replikaci a jsou replikace.</span><span class="sxs-lookup"><span data-stu-id="9de14-119">Ensure that hello virtual machines have completed initial replication and are replicating.</span></span>
3. <span data-ttu-id="9de14-120">Počkejte na dokončení počáteční replikace hello a hello stav replikace uvádí chráněné.</span><span class="sxs-lookup"><span data-stu-id="9de14-120">Wait till hello initial replication completes and hello Replication status says Protected.</span></span>

## ![](media/site-recovery-runbook-automation/01.png)
<span data-ttu-id="9de14-121">V tomto kurzu vytvoříme plán obnovení pro společnost Fabrikam HRweb aplikace toofailover hello aplikace tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="9de14-121">In this tutorial, we will create a recovery plan for hello Fabrikam HRweb application toofailover hello application tooAzure.</span></span> <span data-ttu-id="9de14-122">Potom jsme se integrovat do sady runbook, která vytvoří koncový bod na hello při selhání virtuálního počítače Azure tooserve webových stránek na portu 80.</span><span class="sxs-lookup"><span data-stu-id="9de14-122">Then we will integrate it with a runbook that will create an endpoint on hello failed over Azure virtual machine tooserve web pages at port 80.</span></span>

<span data-ttu-id="9de14-123">Nejdříve vytvoříme plán obnovení pro naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9de14-123">First, let's create a recovery plan for our application.</span></span>

## <a name="create-hello-recovery-plan"></a><span data-ttu-id="9de14-124">Vytvoření plánu obnovení hello</span><span class="sxs-lookup"><span data-stu-id="9de14-124">Create hello recovery plan</span></span>
<span data-ttu-id="9de14-125">tooAzure aplikace hello toorecover, musíte toocreate plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="9de14-125">toorecover hello application tooAzure, you need toocreate a recovery plan.</span></span>
<span data-ttu-id="9de14-126">Pomocí plán obnovení, že které lze nastavit pořadí hello obnovení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9de14-126">Using a recovery plan you can specify hello order of recovery of the virtual machines.</span></span> <span data-ttu-id="9de14-127">Hello virtuálního počítače umístěny do skupiny 1 se obnovit a spusťte první a postupujte hello virtuálního počítače ve skupině 2.</span><span class="sxs-lookup"><span data-stu-id="9de14-127">hello virtual machine placed in group 1 will recover and start first, and then hello virtual machine in group 2 will follow.</span></span>

<span data-ttu-id="9de14-128">Vytvořte do plánu obnovení, který může vypadat níže.</span><span class="sxs-lookup"><span data-stu-id="9de14-128">Create a Recovery Plan that looks like below.</span></span>

![](media/site-recovery-runbook-automation/12.png)

<span data-ttu-id="9de14-129">Další informace o plány obnovení, přečtěte si dokumentaci tooread [sem](https://msdn.microsoft.com/library/azure/dn788799.aspx "zde").</span><span class="sxs-lookup"><span data-stu-id="9de14-129">tooread more about recovery plans, read documentation [here](https://msdn.microsoft.com/library/azure/dn788799.aspx "here").</span></span>

<span data-ttu-id="9de14-130">V dalším kroku vytvoříme hello nezbytné artefakty ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9de14-130">Next, let's create hello necessary artifacts in Azure Automation.</span></span>

## <a name="create-hello-automation-account-and-its-assets"></a><span data-ttu-id="9de14-131">Vytvoření účtu automation hello a její prostředky</span><span class="sxs-lookup"><span data-stu-id="9de14-131">Create hello automation account and its assets</span></span>
<span data-ttu-id="9de14-132">Budete potřebovat runbooky toocreate účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9de14-132">You need an Azure Automation account toocreate runbooks.</span></span> <span data-ttu-id="9de14-133">Pokud již účet nemáte, přejděte karta automatizace tooAzure označený jako ![](media/site-recovery-runbook-automation/02.png)a vytvořit nový účet.</span><span class="sxs-lookup"><span data-stu-id="9de14-133">If you do not already have an account, navigate tooAzure Automation tab denoted by ![](media/site-recovery-runbook-automation/02.png)and create a new account.</span></span>

1. <span data-ttu-id="9de14-134">Dejte účtu hello tooidentify název s.</span><span class="sxs-lookup"><span data-stu-id="9de14-134">Give hello account a name tooidentify with.</span></span>
2. <span data-ttu-id="9de14-135">Zadejte geografické oblasti, kde chcete tooplace hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9de14-135">Specify a geographical region where you want tooplace hello account.</span></span>

<span data-ttu-id="9de14-136">Je doporučeno tooplace hello účet v hello stejné oblasti jako trezor hello automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="9de14-136">It is recommended tooplace hello account in hello same region as hello ASR vault.</span></span>

![](media/site-recovery-runbook-automation/03.png)

<span data-ttu-id="9de14-137">Dále vytvořte hello následující prostředky v hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9de14-137">Next, create hello following assets in hello Account.</span></span>

### <a name="add-a-subscription-name-as-asset"></a><span data-ttu-id="9de14-138">Přidejte název odběru jako prostředek</span><span class="sxs-lookup"><span data-stu-id="9de14-138">Add a subscription name as asset</span></span>
1. <span data-ttu-id="9de14-139">Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v hello prostředky Azure Automation a vyberte příliš![](media/site-recovery-runbook-automation/05.png)</span><span class="sxs-lookup"><span data-stu-id="9de14-139">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in hello Azure Automation Assets and select too![](media/site-recovery-runbook-automation/05.png)</span></span>
2. <span data-ttu-id="9de14-140">Vyberte typ proměnné hello jako **řetězec**</span><span class="sxs-lookup"><span data-stu-id="9de14-140">Select hello variable type as **String**</span></span>
3. <span data-ttu-id="9de14-141">Zadejte název proměnné jako **AzureSubscriptionName**</span><span class="sxs-lookup"><span data-stu-id="9de14-141">Specify variable name as **AzureSubscriptionName**</span></span>

   ![](media/site-recovery-runbook-automation/06.png)
4. <span data-ttu-id="9de14-142">Jako hodnotu proměnné hello zadejte skutečný název předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="9de14-142">Specify your actual Azure Subscription name as hello variable value.</span></span>

   ![](media/site-recovery-runbook-automation/07_1.png)

<span data-ttu-id="9de14-143">Můžete určit název hello předplatného ze stránky hello nastavení svého účtu na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9de14-143">You can identify hello name of your subscription from hello settings page of your account on hello Azure portal.</span></span>

### <a name="add-an-azure-login-credential-as-asset"></a><span data-ttu-id="9de14-144">Přidat jako asset přihlašovacích údajů přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="9de14-144">Add an Azure login credential as asset</span></span>
<span data-ttu-id="9de14-145">Automatizace Azure používá prostředí Azure PowerShell tooconnect toothe předplatného a funguje na hello artefakty existuje.</span><span class="sxs-lookup"><span data-stu-id="9de14-145">Azure Automation uses Azure PowerShell tooconnect toothe subscription and operates on hello artifacts there.</span></span> <span data-ttu-id="9de14-146">V takovém případě budete muset ověřit pomocí účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="9de14-146">For this, you need to authenticate using your Microsoft account or a work or school account.</span></span>
<span data-ttu-id="9de14-147">Přihlašovací údaje účtu hello můžete uložit ve toobe asset používá bezpečně hello runbook.</span><span class="sxs-lookup"><span data-stu-id="9de14-147">You can store hello account credentials in an asset toobe used securely by hello runbook.</span></span>

1. <span data-ttu-id="9de14-148">Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v hello prostředky Azure Automation a vyberte![](media/site-recovery-runbook-automation/09.png)</span><span class="sxs-lookup"><span data-stu-id="9de14-148">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in hello Azure Automation Assets and select ![](media/site-recovery-runbook-automation/09.png)</span></span>
2. <span data-ttu-id="9de14-149">Vyberte typ přihlašovacích údajů hello jako **přihlašovací údaje Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="9de14-149">Select hello Credential type as **Windows PowerShell Credential**</span></span>
3. <span data-ttu-id="9de14-150">Zadejte název hello jako **AzureCredential**</span><span class="sxs-lookup"><span data-stu-id="9de14-150">Specify hello name as **AzureCredential**</span></span>

   ![](media/site-recovery-runbook-automation/10.png)
4. <span data-ttu-id="9de14-151">Zadejte hello uživatelské jméno a heslo toosign v s.</span><span class="sxs-lookup"><span data-stu-id="9de14-151">Specify hello username and password toosign-in with.</span></span>

<span data-ttu-id="9de14-152">Obě tato nastavení jsou nyní k dispozici v vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="9de14-152">Now both these settings are available in your assets.</span></span>

![](media/site-recovery-runbook-automation/11.png)

<span data-ttu-id="9de14-153">Další informace o tom, jak je zadána tooconnect tooyour předplatné přes PowerShell [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9de14-153">More information about how tooconnect tooyour subscription via PowerShell is given [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="9de14-154">V dalším kroku vytvoříte sady runbook ve službě Azure Automation, které můžete přidat koncový bod pro hello front-end virtuální počítač po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9de14-154">Next, you will create a runbook in Azure Automation that can add an endpoint for hello front-end virtual machine after failover.</span></span>

## <a name="azure-automation-context"></a><span data-ttu-id="9de14-155">Kontext služby Azure automation</span><span class="sxs-lookup"><span data-stu-id="9de14-155">Azure automation context</span></span>
<span data-ttu-id="9de14-156">Automatické obnovení systému předá kontextu proměnné toohello runbook toohelp napíšete skripty deterministický.</span><span class="sxs-lookup"><span data-stu-id="9de14-156">ASR passes a context variable toohello runbook toohelp you write deterministic scripts.</span></span> <span data-ttu-id="9de14-157">Jeden může uvádějí, že jsou předvídatelný hello názvy hello cloudové služby a hello virtuálního počítače, ale se stane, že není vždy hello případ díky toocertain scénáře, jako je hello jeden kde byl změněn název hello hello virtuálního počítače z důvodu toounsupported znaků v Azure.</span><span class="sxs-lookup"><span data-stu-id="9de14-157">One could argue that hello names of hello Cloud Service and hello Virtual Machine are predictable, but happens that it is not always hello case owing toocertain scenarios such as hello one where hello name of hello virtual machine name might have changed due toounsupported characters in Azure.</span></span> <span data-ttu-id="9de14-158">Proto tyto informace se předá plán obnovení toohello automatické obnovení systému jako součást hello *kontextu*.</span><span class="sxs-lookup"><span data-stu-id="9de14-158">Hence this information is passed toohello ASR recovery plan as part of hello *context*.</span></span>

<span data-ttu-id="9de14-159">Níže je příklad, jak vypadá hello kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="9de14-159">Below is an example of how hello context variable looks.</span></span>

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


<span data-ttu-id="9de14-160">Následující tabulka Hello obsahuje název a popis pro každou proměnnou v kontextu hello.</span><span class="sxs-lookup"><span data-stu-id="9de14-160">hello table below contains name and description for each variable in hello context.</span></span>

| <span data-ttu-id="9de14-161">**Název proměnné**</span><span class="sxs-lookup"><span data-stu-id="9de14-161">**Variable name**</span></span> | <span data-ttu-id="9de14-162">**Popis**</span><span class="sxs-lookup"><span data-stu-id="9de14-162">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="9de14-163">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="9de14-163">RecoveryPlanName</span></span> |<span data-ttu-id="9de14-164">Název plánu spuštěn.</span><span class="sxs-lookup"><span data-stu-id="9de14-164">Name of plan being run.</span></span> <span data-ttu-id="9de14-165">Pomáhá provedení akce podle názvu pomocí hello stejný skriptu</span><span class="sxs-lookup"><span data-stu-id="9de14-165">Helps you take action based on name using hello same script</span></span> |
| <span data-ttu-id="9de14-166">FailoverType</span><span class="sxs-lookup"><span data-stu-id="9de14-166">FailoverType</span></span> |<span data-ttu-id="9de14-167">Určuje, zda je převzetí služeb při selhání hello otestovat, plánovaná nebo neplánovaná.</span><span class="sxs-lookup"><span data-stu-id="9de14-167">Specifies whether hello failover is test, planned, or unplanned.</span></span> |
| <span data-ttu-id="9de14-168">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="9de14-168">FailoverDirection</span></span> |<span data-ttu-id="9de14-169">Určete, zda je obnovení tooprimary nebo sekundární</span><span class="sxs-lookup"><span data-stu-id="9de14-169">Specify whether recovery is tooprimary or secondary</span></span> |
| <span data-ttu-id="9de14-170">GroupID</span><span class="sxs-lookup"><span data-stu-id="9de14-170">GroupID</span></span> |<span data-ttu-id="9de14-171">Když běží hello plán identifikovat hello číslo skupiny v rámci plánu obnovení hello</span><span class="sxs-lookup"><span data-stu-id="9de14-171">Identify hello group number within hello recovery plan when hello plan is running</span></span> |
| <span data-ttu-id="9de14-172">VmMap</span><span class="sxs-lookup"><span data-stu-id="9de14-172">VmMap</span></span> |<span data-ttu-id="9de14-173">Pole všech hello virtuálních počítačů ve skupině hello</span><span class="sxs-lookup"><span data-stu-id="9de14-173">Array of all hello virtual machines in hello group</span></span> |
| <span data-ttu-id="9de14-174">Klíč VMMap</span><span class="sxs-lookup"><span data-stu-id="9de14-174">VMMap key</span></span> |<span data-ttu-id="9de14-175">Jedinečný klíč (GUID) pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9de14-175">Unique key (GUID) for each VM.</span></span> <span data-ttu-id="9de14-176">Má hello stejné jako hello ID VMM hello virtuálního počítače, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="9de14-176">It's hello same as hello VMM ID of hello virtual machine where applicable.</span></span> |
| <span data-ttu-id="9de14-177">RoleName</span><span class="sxs-lookup"><span data-stu-id="9de14-177">RoleName</span></span> |<span data-ttu-id="9de14-178">Název virtuálního počítače Azure, který obnovuje hello</span><span class="sxs-lookup"><span data-stu-id="9de14-178">Name of hello Azure VM that's being recovered</span></span> |
| <span data-ttu-id="9de14-179">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="9de14-179">CloudServiceName</span></span> |<span data-ttu-id="9de14-180">Název Azure cloudové služby v rámci které hello při vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9de14-180">Azure Cloud Service name under which hello virtual machine is created.</span></span> |

<span data-ttu-id="9de14-181">tooidentify hello VmMap klíč v kontextu hello můžete také přejít toohello stránku vlastností virtuálního počítače v nástroji automatické obnovení systému a podívejte se na hello vlastnost identifikátor GUID virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9de14-181">tooidentify hello VmMap Key in hello context you could also go toohello VM properties page in ASR and look at hello VM GUID property.</span></span>

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a><span data-ttu-id="9de14-182">Autor runbook služby Automation.</span><span class="sxs-lookup"><span data-stu-id="9de14-182">Author an Automation runbook</span></span>
<span data-ttu-id="9de14-183">Teď vytvořte hello runbook tooopen port 80 hello front-end virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9de14-183">Now create hello runbook tooopen port 80 on hello front-end virtual machine.</span></span>

1. <span data-ttu-id="9de14-184">Vytvořit novou sadu runbook v hello účet Azure Automation s názvem hello **OpenPort80**</span><span class="sxs-lookup"><span data-stu-id="9de14-184">Create a new runbook in hello Azure Automation account with hello name **OpenPort80**</span></span>

   ![](media/site-recovery-runbook-automation/14.png)
2. <span data-ttu-id="9de14-185">Přejděte toohello zobrazení Autor sady runbook hello a zadejte hello režimu konceptu.</span><span class="sxs-lookup"><span data-stu-id="9de14-185">Navigate toohello Author view of hello runbook and enter hello draft mode.</span></span>
3. <span data-ttu-id="9de14-186">Nejprve zadejte proměnné toouse hello jako kontext plán obnovení hello</span><span class="sxs-lookup"><span data-stu-id="9de14-186">First specify hello variable toouse as hello recovery plan context</span></span>

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. <span data-ttu-id="9de14-187">Další připojení odběru toohello pomocí přihlašovacích údajů a předplatné názvu hello</span><span class="sxs-lookup"><span data-stu-id="9de14-187">Next connect toohello subscription using hello credential and subscription name</span></span>

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect tooAzure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   <span data-ttu-id="9de14-188">Všimněte si, že používáte hello Azure prostředky – **AzureCredential** a **AzureSubscriptionName** sem.</span><span class="sxs-lookup"><span data-stu-id="9de14-188">Note that you use hello Azure assets – **AzureCredential** and **AzureSubscriptionName** here.</span></span>
5. <span data-ttu-id="9de14-189">Nyní zadejte hello koncový bod podrobnosti a hello GUID hello virtuálního počítače, pro které chcete tooexpose hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9de14-189">Now specify hello endpoint details and hello GUID of hello virtual machine for which you want tooexpose hello endpoint.</span></span> <span data-ttu-id="9de14-190">V tomto případu hello front-end virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9de14-190">In this case hello front-end virtual machine.</span></span>

   ```
       # Specify hello parameters toobe used by hello script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   <span data-ttu-id="9de14-191">Určuje hello protokol koncového bodu Azure, místního portu na hello virtuálního počítače a její namapované veřejný port.</span><span class="sxs-lookup"><span data-stu-id="9de14-191">This specifies hello Azure endpoint protocol, local port on hello VM and its mapped public port.</span></span> <span data-ttu-id="9de14-192">Tyto proměnné jsou parametry vyžadované hello Azure příkazy, které přidat tooVMs koncové body.</span><span class="sxs-lookup"><span data-stu-id="9de14-192">These variables are parameters     required by hello Azure commands that add endpoints tooVMs.</span></span> <span data-ttu-id="9de14-193">Hello VMGUID obsahuje hello hello virtuálního počítače, které potřebujete toooperate na identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="9de14-193">hello VMGUID holds hello GUID of hello virtual machine you need toooperate on.</span></span>
6. <span data-ttu-id="9de14-194">skript Hello teď extrahovat hello kontext pro hello zadaný identifikátor GUID virtuálního počítače a na virtuálním počítači hello odkazuje ho vytvořit koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9de14-194">hello script will now extract hello context for hello given VM GUID and create an endpoint on hello virtual machine referenced by it.</span></span>

   ```
       #Read hello VM GUID from hello context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke hello necessary pipeline commands tooadd a Azure Endpoint tooa specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. <span data-ttu-id="9de14-195">Po dokončení, stiskněte tlačítko Publikovat ![](media/site-recovery-runbook-automation/20.png) tooallow váš skript toobe k dispozici pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="9de14-195">Once this is complete, hit Publish ![](media/site-recovery-runbook-automation/20.png) tooallow your script toobe available for execution.</span></span>

<span data-ttu-id="9de14-196">Dále je pro vaši informaci uveden Hello dokončení skriptu</span><span class="sxs-lookup"><span data-stu-id="9de14-196">hello complete script is given below for your reference</span></span>

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect tooAzure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify hello parameters toobe used by hello script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read hello VM GUID from hello context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke hello necessary pipeline commands tooadd an Azure Endpoint tooa specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-hello-script-toohello-recovery-plan"></a><span data-ttu-id="9de14-197">Přidat plán obnovení toohello hello skriptu</span><span class="sxs-lookup"><span data-stu-id="9de14-197">Add hello script toohello recovery plan</span></span>
<span data-ttu-id="9de14-198">Až hello skriptu je připraven, můžete ho přidat toohello plánu obnovení, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="9de14-198">Once hello script is ready, you can add it toohello recovery plan that you created earlier.</span></span>

1. <span data-ttu-id="9de14-199">V plánu obnovení hello, kterou jste vytvořili zvolte tooadd skriptu po skupiny 2.</span><span class="sxs-lookup"><span data-stu-id="9de14-199">In hello recovery plan you created, choose tooadd a script after the group 2.</span></span> ![](media/site-recovery-runbook-automation/15.png)
2. <span data-ttu-id="9de14-200">Zadejte název skriptu.</span><span class="sxs-lookup"><span data-stu-id="9de14-200">Specify a script name.</span></span> <span data-ttu-id="9de14-201">Toto je právě popisný název pro tento skript pro zobrazení v rámci plánu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="9de14-201">This is just a friendly name for this script for showing within hello Recovery plan.</span></span>
3. <span data-ttu-id="9de14-202">Ve skriptu tooAzure převzetí služeb při selhání hello – vyberte název hello účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9de14-202">In hello failover tooAzure script – Select hello Azure Automation Account name.</span></span>
4. <span data-ttu-id="9de14-203">V hello Runbooky Azure vyberte sadu runbook hello, vámi vytvořený.</span><span class="sxs-lookup"><span data-stu-id="9de14-203">In hello Azure Runbooks, select hello runbook you authored.</span></span>

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a><span data-ttu-id="9de14-204">Primární straně skripty</span><span class="sxs-lookup"><span data-stu-id="9de14-204">Primary side scripts</span></span>
<span data-ttu-id="9de14-205">Když jsou prováděny tooAzure převzetí služeb při selhání, můžete také tooexecute primární straně skripty.</span><span class="sxs-lookup"><span data-stu-id="9de14-205">When you are executing a failover tooAzure, you can also choose tooexecute primary side scripts.</span></span> <span data-ttu-id="9de14-206">Tyto skripty se spustí na serveru VMM hello během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9de14-206">These scripts will run on hello VMM server during failover.</span></span>
<span data-ttu-id="9de14-207">Primární straně skriptů jsou k dispozici pouze pro před vypnutím pouze a post vypnutí fázích.</span><span class="sxs-lookup"><span data-stu-id="9de14-207">Primary side scripts are only available only for pre-shutdown and post shutdown stages.</span></span> <span data-ttu-id="9de14-208">Toto je očekávané hello primární lokality toobe obvykle není k dispozici při havárii narazilo.</span><span class="sxs-lookup"><span data-stu-id="9de14-208">This is because we expect hello primary site toobe typically unavailable when a disaster strikes.</span></span>
<span data-ttu-id="9de14-209">Pouze v případě, že můžete vyjádřit výslovný souhlas pro operace primární lokality, je během neplánované převzetí služeb při selhání, se pokusí toorun hello primární straně skripty.</span><span class="sxs-lookup"><span data-stu-id="9de14-209">During an unplanned failover, only if you opt in for primary site operations, it will attempt toorun hello primary side scripts.</span></span> <span data-ttu-id="9de14-210">Pokud nejsou dosažitelné nebo vypršel časový limit, bude pokračovat hello převzetí služeb při selhání toorecover hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9de14-210">If they are not reachable or timeout, hello failover will continue toorecover hello virtual machines.</span></span>
<span data-ttu-id="9de14-211">Primární straně skriptů jsou zrušení k dispozici pro servery VMware nebo fyzických/Hyper-v bez VMM chráněné tooAzure - při převzetí služeb při selhání tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9de14-211">Primary side scripts are un-available for VMware/Physical/Hyper-v Sites without VMM protected tooAzure - while you failover tooAzure.</span></span>
<span data-ttu-id="9de14-212">Ale když můžete navrácení služeb po obnovení z Azure tooon místní, primární straně skripty (sady Runbook) lze pro všechny cíle kromě VMware.</span><span class="sxs-lookup"><span data-stu-id="9de14-212">However, when you failback from Azure tooon-premises, primary side scripts (Runbooks) can be used for all targets except VMware.</span></span>

## <a name="test-hello-recovery-plan"></a><span data-ttu-id="9de14-213">Testovací plán obnovení hello</span><span class="sxs-lookup"><span data-stu-id="9de14-213">Test hello recovery plan</span></span>
<span data-ttu-id="9de14-214">Po přidání hello runbook toohello plán můžete spustit testovací převzetí služeb a pozorování v akci.</span><span class="sxs-lookup"><span data-stu-id="9de14-214">Once you have added hello runbook toohello plan you can initiate a test failover and see it in action.</span></span> <span data-ttu-id="9de14-215">Vždycky je doporučeno toorun testovací převzetí služeb při selhání tootest vaší aplikace a hello tooensure plán obnovení, nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="9de14-215">It is always recommended toorun a test failover tootest your application and hello recovery plan tooensure that there are no errors.</span></span>

1. <span data-ttu-id="9de14-216">Vyberte plán obnovení hello a spustit testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="9de14-216">Select hello recovery plan and initiate a test failover.</span></span>
2. <span data-ttu-id="9de14-217">Během provádění hello plán můžete zjistit, zda provedlo hello runbook nebo nikoli prostřednictvím jeho stav.</span><span class="sxs-lookup"><span data-stu-id="9de14-217">During hello plan execution, you can see whether hello runbook has executed or not via its status.</span></span>

   ![](media/site-recovery-runbook-automation/17.png)
3. <span data-ttu-id="9de14-218">Můžete také zjistit hello podrobný stav spuštění sady runbook na stránce úloh Azure Automation hello hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="9de14-218">You can also see hello detailed runbook execution status on hello Azure Automation jobs page for hello runbook.</span></span>

   ![](media/site-recovery-runbook-automation/18.png)
4. <span data-ttu-id="9de14-219">Po dokončení převzetí služeb při selhání hello kromě hello výsledku spuštění runbooku, uvidíte, zda zpracování hello se úspěšně nebo ne návštěvou stránku hello virtuální počítač Azure a prohlížení hello koncové body.</span><span class="sxs-lookup"><span data-stu-id="9de14-219">After hello failover completes, apart from hello runbook execution result, you can see whether hello execution is successful or not by visiting hello Azure virtual machine page and looking at hello endpoints.</span></span>

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a><span data-ttu-id="9de14-220">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="9de14-220">Sample scripts</span></span>
<span data-ttu-id="9de14-221">Když jsme projít automatizaci jeden běžně používat úkolu přidávání tooan koncový bod virtuálního počítače Azure v tomto kurzu, můžete to udělat několika další výkonné automatizované úlohy pomocí služby Azure automation.</span><span class="sxs-lookup"><span data-stu-id="9de14-221">While we walked through automating one commonly used task of adding an endpoint tooan Azure virtual machine in this tutorial, you could do a number of other powerful automation tasks using Azure automation.</span></span> <span data-ttu-id="9de14-222">Společnost Microsoft a hello Azure Automation komunity poskytují ukázkové sady runbook, které vám pomůžou začít vytvářet vlastní řešení a sady runbook nástroj, který můžete použít jako stavební bloky pro větší úlohy automatizace.</span><span class="sxs-lookup"><span data-stu-id="9de14-222">Microsoft and hello Azure Automation community provide sample runbooks which can help you get started creating your own solutions, and utility runbooks, which you can use as building blocks for larger automation tasks.</span></span> <span data-ttu-id="9de14-223">Začít používat je z Galerie hello a vytvářet plány obnovení výkonné jedním kliknutím pro vaše aplikace pomocí Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9de14-223">Start using them from hello gallery and build  powerful one-click recovery plans for your applications using Azure Site Recovery.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9de14-224">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9de14-224">Additional Resources</span></span>
[<span data-ttu-id="9de14-225">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="9de14-225">Azure Automation Overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Přehled služby Azure Automation")

[<span data-ttu-id="9de14-226">Ukázkové skripty pro automatizaci Azure</span><span class="sxs-lookup"><span data-stu-id="9de14-226">Sample Azure Automation Scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty služby Azure Automation")
