---
title: " Opravit virtuální počítač Azure výstrah pomocí Runbooků Automation | Microsoft Docs"
description: "Tento článek ukazuje, jak integrovat Azure Automation runbook virtuální počítač Azure výstrahy a automatické odstranění problémů"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2016
ms.author: csand;magoedte
ms.openlocfilehash: 738959b8e1ee5da989bb996d1ce8148cbf912781
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a><span data-ttu-id="5a914-103">Scénář automatizace Azure – opravit virtuální počítač Azure výstrahy</span><span class="sxs-lookup"><span data-stu-id="5a914-103">Azure Automation scenario - remediate Azure VM alerts</span></span>
<span data-ttu-id="5a914-104">Azure Automation a virtuální počítače Azure byla vydána nová funkce umožňuje nakonfigurovat výstrahy virtuálního počítače (VM) ke spuštění sady Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="5a914-104">Azure Automation and Azure Virtual Machines have released a new feature allowing you to configure Virtual Machine (VM) alerts to run Automation runbooks.</span></span> <span data-ttu-id="5a914-105">Tato nová funkce umožňuje automaticky provádět standardní nápravy v reakci na výstrahy virtuálních počítačů, jako je restartování nebo zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5a914-105">This new capability allows you to automatically perform standard remediation in response to VM alerts, like restarting or stopping the VM.</span></span>

<span data-ttu-id="5a914-106">Při vytváření virtuálních počítačů pravidlo výstrahy bylo dřív možné [zadejte webhook automatizace](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) k sadě runbook ke spuštění sady runbook vždy, když se výstraha.</span><span class="sxs-lookup"><span data-stu-id="5a914-106">Previously, during VM alert rule creation you were able to [specify an Automation webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) to a runbook in order to run the runbook whenever the alert triggered.</span></span> <span data-ttu-id="5a914-107">Ale to vyžaduje k práci při vytváření sady runbook, vytváření webhooku pro sadu runbook a pak kopírování a vkládání webhook při vytváření pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="5a914-107">However, this required you to do the work of creating the runbook, creating the webhook for the runbook, and then copying and pasting the webhook during alert rule creation.</span></span> <span data-ttu-id="5a914-108">V této verzi nové proces je mnohem jednodušší, protože sady runbook můžete přímo vybrat ze seznamu během vytváření pravidlo výstrahy, a můžete zvolit účet Automation, který bude spouštět sadu runbook nebo snadno vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="5a914-108">With this new release, the process is much easier because you can directly choose a runbook from a list during alert rule creation, and you can choose an Automation account which will run the runbook or easily create an account.</span></span>

<span data-ttu-id="5a914-109">V tomto článku zjistíte, jak je snadné vytvořit výstrahu virtuální počítač Azure a nakonfigurovat runbook služby automatizace ke spuštění, kdykoli se aktivuje výstraha.</span><span class="sxs-lookup"><span data-stu-id="5a914-109">In this article, we will show you how easy it is to set up an Azure VM alert and configure an Automation runbook to run whenever the alert triggers.</span></span> <span data-ttu-id="5a914-110">Příklad scénáře zahrnují restartování virtuálního počítače, pokud využití paměti překročí některé prahovou hodnotu z důvodu aplikace ve virtuálním počítači s nevrácenou pamětí nebo zastavení virtuálního počítače při uživatelského času procesoru bylo nižší než 1 % po poslední hodinu a není používán.</span><span class="sxs-lookup"><span data-stu-id="5a914-110">Example scenarios include restarting a VM when the memory usage exceeds some threshold due to an application on the VM with a memory leak, or stopping a VM when the CPU user time has been below 1% for past hour and is not in use.</span></span> <span data-ttu-id="5a914-111">Vysvětlíme taky, jak automatické vytvoření objektu v účtu Automation služby zjednodušuje použití sad runbook v Azure výstrahy nápravu.</span><span class="sxs-lookup"><span data-stu-id="5a914-111">We’ll also explain how the automated creation of a service principal in your Automation account simplifies the use of runbooks in Azure alert remediation.</span></span>

## <a name="create-an-alert-on-a-vm"></a><span data-ttu-id="5a914-112">Vytvořit výstrahu na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="5a914-112">Create an alert on a VM</span></span>
<span data-ttu-id="5a914-113">Proveďte následující postup pro konfiguraci výstrahu pro spuštění sady runbook při jeho prahová hodnota byla splněna.</span><span class="sxs-lookup"><span data-stu-id="5a914-113">Perform the following steps to configure an alert to launch a runbook when its threshold has been met.</span></span>

> [!NOTE]
> <span data-ttu-id="5a914-114">V této verzi jsme podporují jenom V2 virtuální počítače a podporu pro classic, který bude brzy přidat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5a914-114">With this release, we only support V2 virtual machines and support for classic VMs will be added soon.</span></span>  
> 
> 

1. <span data-ttu-id="5a914-115">Přihlaste se k portálu Azure a klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="5a914-115">Log in to the Azure portal and click **Virtual Machines**.</span></span>  
2. <span data-ttu-id="5a914-116">Vyberte jednu z vašich virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a914-116">Select one of your virtual machines.</span></span>  <span data-ttu-id="5a914-117">Zobrazí se okno řídicího panelu virtuální počítač a **nastavení** okno záhlavím vpravo.</span><span class="sxs-lookup"><span data-stu-id="5a914-117">The virtual machine dashboard blade will appear and the **Settings** blade to its right.</span></span>  
3. <span data-ttu-id="5a914-118">Z **nastavení** okno, v části vyberte části monitorování **výstrah pravidla**.</span><span class="sxs-lookup"><span data-stu-id="5a914-118">From the **Settings** blade, under the Monitoring section select **Alert rules**.</span></span>
4. <span data-ttu-id="5a914-119">Na **výstrah pravidla** okně klikněte na tlačítko **přidat upozornění**.</span><span class="sxs-lookup"><span data-stu-id="5a914-119">On the **Alert rules** blade, click **Add alert**.</span></span>

<span data-ttu-id="5a914-120">Otevře **přidání pravidla výstrahy** okno, kde můžete konfigurovat podmínky pro výstrahu a zvolit si takový jeden nebo všechny tyto možnosti: odeslání e-mailu, použijte webhook, jehož a předat výstrahy do jiného systému, popř. spuštěn Sada Automation runbook v odpovědi se pokusí opravit tento problém.</span><span class="sxs-lookup"><span data-stu-id="5a914-120">This opens up the **Add an alert rule** blade, where you can configure the conditions for the alert and choose among one or all of these options: send email to someone, use a webhook to forward the alert to another system, and/or run an Automation runbook in response attempt to remediate the issue.</span></span>

## <a name="configure-a-runbook"></a><span data-ttu-id="5a914-121">Konfigurace sady runbook</span><span class="sxs-lookup"><span data-stu-id="5a914-121">Configure a runbook</span></span>
<span data-ttu-id="5a914-122">Konfigurace sady runbook spouštět, když je dosaženo prahové hodnoty pro výstrahu virtuálního počítače, vyberte **sady Automation Runbook**.</span><span class="sxs-lookup"><span data-stu-id="5a914-122">To configure a runbook to run when the VM alert threshold is met, select **Automation Runbook**.</span></span> <span data-ttu-id="5a914-123">V **konfigurace runbook** okno, můžete vybrat spuštění sady runbook a účet Automation ke spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="5a914-123">In the **Configure runbook** blade, you can select the runbook to run and the Automation account to run the runbook in.</span></span>

![Nakonfigurujte runbook služby automatizace a vytvořte nový účet Automation.](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> <span data-ttu-id="5a914-125">Pro tuto verzi můžete zvolit ze tří sady runbook, které poskytuje služba – restartování virtuálního počítače, zastavte virtuální počítač nebo odeberte virtuální počítač (odstranit).</span><span class="sxs-lookup"><span data-stu-id="5a914-125">For this release you can choose from three runbooks that the service provides – Restart VM, Stop VM, or Remove VM (delete it).</span></span>  <span data-ttu-id="5a914-126">Umožňuje vybrat jiné sady runbook nebo jeden z vlastní sady runbook bude k dispozici v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="5a914-126">The ability to select other runbooks or one of your own runbooks will be available in a future release.</span></span>
> 
> 

![Vybírejte z těchto možností sady Runbook](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

<span data-ttu-id="5a914-128">Po výběru tři dostupné sady runbook, **účet Automation** rozevíracím seznamu se zobrazí a můžete vybrat účet automation runbook se spustit jako.</span><span class="sxs-lookup"><span data-stu-id="5a914-128">After you select one of the three available runbooks, the **Automation account** drop-down list appears and you can select an automation account the runbook will run as.</span></span> <span data-ttu-id="5a914-129">Runbooky potřebují spustit v kontextu [účet Automation](automation-security-overview.md) se ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="5a914-129">Runbooks need to run in the context of an [Automation account](automation-security-overview.md) that is in your Azure subscription.</span></span> <span data-ttu-id="5a914-130">Můžete vybrat účet Automation, zda jste již vytvořili, nebo máte vytvořený pro vás nový účet automatizace.</span><span class="sxs-lookup"><span data-stu-id="5a914-130">You can select an Automation account that you already created, or you can have a new Automation account created for you.</span></span>

<span data-ttu-id="5a914-131">Sady runbook, které jsou k dispozici ověřování v Azure pomocí objektu služby.</span><span class="sxs-lookup"><span data-stu-id="5a914-131">The runbooks that are provided authenticate to Azure using a service principal.</span></span> <span data-ttu-id="5a914-132">Pokud zvolíte možnost spuštění sady runbook v jednom z vaší existující účty Automation, jsme automaticky vytvoří služba hlavní za vás.</span><span class="sxs-lookup"><span data-stu-id="5a914-132">If you choose to run the runbook in one of your existing Automation accounts, we will automatically create the service principal for you.</span></span> <span data-ttu-id="5a914-133">Pokud se rozhodnete vytvořit nový účet Automation, pak automaticky vytvoříme účet a objekt služby.</span><span class="sxs-lookup"><span data-stu-id="5a914-133">If you choose to create a new Automation account, then we will automatically create the account and the service principal.</span></span> <span data-ttu-id="5a914-134">V obou případech dva prostředky bude vytvořen také v účtu Automation – certifikát asset s názvem **AzureRunAsCertificate** a připojení asset s názvem **AzureRunAsConnection**.</span><span class="sxs-lookup"><span data-stu-id="5a914-134">In both cases, two assets will also be created in the Automation account – a certificate asset named **AzureRunAsCertificate** and a connection asset named **AzureRunAsConnection**.</span></span> <span data-ttu-id="5a914-135">Sady runbook použije **AzureRunAsConnection** k ověření pomocí Azure, abyste mohli provádět akce správy u virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5a914-135">The runbooks will use **AzureRunAsConnection** to authenticate with Azure in order to perform the management action against the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="5a914-136">Objekt služby se vytvoří v oboru předplatného a má přiřazenou roli Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="5a914-136">The service principal is created in the subscription scope and is assigned the Contributor role.</span></span> <span data-ttu-id="5a914-137">Tato role se vyžaduje, aby pro účet, který chcete mít oprávnění ke spouštění runbooků služeb automatizace spravovat virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="5a914-137">This role is required in order for the account to have permission to run Automation runbooks to manage Azure VMs.</span></span>  <span data-ttu-id="5a914-138">Vytvoření Automaton účet nebo instančního objektu je jednorázové událost.</span><span class="sxs-lookup"><span data-stu-id="5a914-138">The creation of an Automaton account and/or service principal is a one-time event.</span></span> <span data-ttu-id="5a914-139">Po vytvoření, můžete tento účet ke spuštění sady runbook pro jiné výstrahy virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="5a914-139">Once they are created, you can use that account to run runbooks for other Azure VM alerts.</span></span>
> 
> 

<span data-ttu-id="5a914-140">Když kliknete na tlačítko **OK** výstrahy je nakonfigurován, a pokud jste vybrali možnost vytvořit nový účet Automation, vytvoří se společně s objektu služby.</span><span class="sxs-lookup"><span data-stu-id="5a914-140">When you click **OK** the alert is configured and if you selected the option to create a new Automation account, it is created along with the service principal.</span></span>  <span data-ttu-id="5a914-141">Může to trvat několik sekund.</span><span class="sxs-lookup"><span data-stu-id="5a914-141">This can take a few seconds to complete.</span></span>  

![Konfigurace sady Runbook](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

<span data-ttu-id="5a914-143">Po dokončení konfigurace se zobrazí název sady runbook se zobrazí v **přidání pravidla výstrahy** okno.</span><span class="sxs-lookup"><span data-stu-id="5a914-143">After the configuration is completed you will see the name of the runbook appear in the **Add an alert rule** blade.</span></span>

![Runbook nakonfigurovat](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

<span data-ttu-id="5a914-145">Klikněte na tlačítko **OK** v **přidání pravidla výstrahy** okno a pravidlo výstrahy se vytvoří a aktivovat, pokud je virtuální počítač ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="5a914-145">Click **OK** in the **Add an alert rule** blade and the alert rule will be created and activate if the virtual machine is in a running state.</span></span>

### <a name="enable-or-disable-a-runbook"></a><span data-ttu-id="5a914-146">Povolit nebo zakázat sady runbook</span><span class="sxs-lookup"><span data-stu-id="5a914-146">Enable or disable a runbook</span></span>
<span data-ttu-id="5a914-147">Pokud je runbook nakonfigurovaný pro výstrahy, ji můžete vypnout bez odebrání konfigurace sady runbook.</span><span class="sxs-lookup"><span data-stu-id="5a914-147">If you have a runbook configured for an alert, you can disable it without removing the runbook configuration.</span></span> <span data-ttu-id="5a914-148">Můžete ponechat oznámení spuštění a případně testování pravidla výstrah a později znovu povolit sady runbook.</span><span class="sxs-lookup"><span data-stu-id="5a914-148">This allows you to keep the alert running and perhaps test some of the alert rules and then later re-enable the runbook.</span></span>

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a><span data-ttu-id="5a914-149">Vytvoření sady runbook, který funguje s Azure výstrahy</span><span class="sxs-lookup"><span data-stu-id="5a914-149">Create a runbook that works with an Azure alert</span></span>
<span data-ttu-id="5a914-150">Když zvolíte sady runbook v rámci Azure pravidla výstrahy, sada runbook musí mít logiku v ho ke správě dat výstrah, který je předán.</span><span class="sxs-lookup"><span data-stu-id="5a914-150">When you choose a runbook as part of an Azure alert rule, the runbook needs to have logic in it to manage the alert data that is passed to it.</span></span>  <span data-ttu-id="5a914-151">Pokud sady runbook je nakonfigurovaná v pravidlo výstrahy, vytvoří se webhook, jehož pro sadu runbook; Tento webhooku se pak používá ke spuštění sady runbook pokaždé, když se aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="5a914-151">When a runbook is configured in an alert rule, a webhook is created for the runbook; that webhook is then used to start the runbook each time the alert triggers.</span></span>  <span data-ttu-id="5a914-152">Vlastní volání pro spuštění sady runbook je požadavek HTTP POST na adresu URL webhooku.</span><span class="sxs-lookup"><span data-stu-id="5a914-152">The actual call to start the runbook is an HTTP POST request to the webhook URL.</span></span> <span data-ttu-id="5a914-153">Text požadavku POST obsahuje objekt uvedena ve správném formátu JSON, který obsahuje užitečné vlastnosti související výstrahy.</span><span class="sxs-lookup"><span data-stu-id="5a914-153">The body of the POST request contains a JSON-formated object that contains useful properties related to the alert.</span></span>  <span data-ttu-id="5a914-154">Jak vidíte níže, data výstrah obsahuje podrobnosti, třeba ID předplatného, název skupiny prostředků, resourceName a typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="5a914-154">As you can see below, the alert data contains details like subscriptionID, resourceGroupName, resourceName, and resourceType.</span></span>

### <a name="example-of-alert-data"></a><span data-ttu-id="5a914-155">Příklad dat výstrah</span><span class="sxs-lookup"><span data-stu-id="5a914-155">Example of Alert data</span></span>
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

<span data-ttu-id="5a914-156">Když služba webhook automatizace obdrží HTTP POST extrahuje data výstrah a předává je pro sadu runbook v WebhookData vstupní parametr runbooku.</span><span class="sxs-lookup"><span data-stu-id="5a914-156">When the Automation webhook service receives the HTTP POST it extracts the alert data and passes it to the runbook in the WebhookData runbook input parameter.</span></span>  <span data-ttu-id="5a914-157">Níže je ukázkové sady runbook, který ukazuje, jak pomocí parametru WebhookData a extrahování dat výstrah a použít ho ke správě prostředků Azure, který aktivoval výstrahu.</span><span class="sxs-lookup"><span data-stu-id="5a914-157">Below is a sample runbook that shows how to use the WebhookData parameter and extract the alert data and use it to manage the Azure resource that triggered the alert.</span></span>

### <a name="example-runbook"></a><span data-ttu-id="5a914-158">Ukázkový runbook</span><span class="sxs-lookup"><span data-stu-id="5a914-158">Example runbook</span></span>
```
#  This runbook will restart an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a><span data-ttu-id="5a914-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5a914-159">Summary</span></span>
<span data-ttu-id="5a914-160">Když konfigurujete výstrahu na virtuální počítač Azure, nyní máte možnost snadno konfigurovat automaticky provádět akci nápravy, která aktivuje výstrahu, když runbook služby automatizace.</span><span class="sxs-lookup"><span data-stu-id="5a914-160">When you configure an alert on an Azure VM, you now have the ability to easily configure an Automation runbook to automatically perform remediation action when the alert triggers.</span></span> <span data-ttu-id="5a914-161">Pro tuto verzi můžete ze sady runbook se znovu spustit, zastavit nebo odstranění virtuálního počítače v závislosti na vašem scénáři výstrahy.</span><span class="sxs-lookup"><span data-stu-id="5a914-161">For this release, you can choose from runbooks to restart, stop, or delete a VM depending on your alert scenario.</span></span> <span data-ttu-id="5a914-162">Toto je právě začátku povolení scénáře, kde můžete řídit akce (oznámení, řešení potíží s nápravy), které se mají provést automaticky, když se aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="5a914-162">This is just the beginning of enabling scenarios where you control the actions (notification, troubleshooting, remediation) that will be taken automatically when an alert triggers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a914-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a914-163">Next Steps</span></span>
* <span data-ttu-id="5a914-164">První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="5a914-164">To get started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span>
* <span data-ttu-id="5a914-165">První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).</span><span class="sxs-lookup"><span data-stu-id="5a914-165">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="5a914-166">Další informace o typech runbooků, jejich výhodách a omezeních najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).</span><span class="sxs-lookup"><span data-stu-id="5a914-166">To learn more about runbook types, their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md)</span></span>

