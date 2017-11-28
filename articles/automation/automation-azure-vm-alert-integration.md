---
title: "AAA\"Opravit výstrahy virtuálního počítače Azure pomocí Runbooků Automation | Microsoft Docs\""
description: "Tento článek ukazuje, jak výstrahy toointegrate virtuální počítač Azure s Azure Automation runbook a automatické odstranění problémů"
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
ms.openlocfilehash: c226368a5c4c51fbfb331f4b97f7f2f239e701c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a><span data-ttu-id="f93ce-103">Scénář automatizace Azure – opravit virtuální počítač Azure výstrahy</span><span class="sxs-lookup"><span data-stu-id="f93ce-103">Azure Automation scenario - remediate Azure VM alerts</span></span>
<span data-ttu-id="f93ce-104">Azure Automation a virtuální počítače Azure vydali novou funkci, umožní vám tooconfigure runbooků služeb automatizace toorun virtuálního počítače (VM) výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f93ce-104">Azure Automation and Azure Virtual Machines have released a new feature allowing you tooconfigure Virtual Machine (VM) alerts toorun Automation runbooks.</span></span> <span data-ttu-id="f93ce-105">Díky této nové funkci můžete tooautomatically nápravy standardní ve výstrahách tooVM odpovědi, jako je restartování nebo hello zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f93ce-105">This new capability allows you tooautomatically perform standard remediation in response tooVM alerts, like restarting or stopping hello VM.</span></span>

<span data-ttu-id="f93ce-106">Dříve, při vytváření virtuálních počítačů pravidlo výstrahy bylo možné příliš[zadejte webhook automatizace](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook v pořadí toorun hello runbook vždy, když se aktivuje výstraha hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-106">Previously, during VM alert rule creation you were able too[specify an Automation webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook in order toorun hello runbook whenever hello alert triggered.</span></span> <span data-ttu-id="f93ce-107">Ale to je nutné toodo hello práci při vytváření sady runbook hello, vytvoření webhooku hello hello sady runbook a pak kopírování a vkládání hello webhooku během vytváření pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f93ce-107">However, this required you toodo hello work of creating hello runbook, creating hello webhook for hello runbook, and then copying and pasting hello webhook during alert rule creation.</span></span> <span data-ttu-id="f93ce-108">V této nové verzi není hello proces je mnohem jednodušší, protože sady runbook můžete přímo vybrat ze seznamu během vytváření pravidlo výstrahy, a můžete zvolit účet Automation, který bude spuštění sady runbook hello nebo snadno vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="f93ce-108">With this new release, hello process is much easier because you can directly choose a runbook from a list during alert rule creation, and you can choose an Automation account which will run hello runbook or easily create an account.</span></span>

<span data-ttu-id="f93ce-109">V tomto článku jsme se ukazují, jak je snadné tooset až výstrahu virtuální počítač Azure a nakonfigurovat toorun automatizace sady runbook vždy, když se aktivuje výstraha hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-109">In this article, we will show you how easy it is tooset up an Azure VM alert and configure an Automation runbook toorun whenever hello alert triggers.</span></span> <span data-ttu-id="f93ce-110">Příklad scénáře zahrnují restartování virtuálního počítače, pokud využití paměti hello překročí některé prahovou hodnotu z důvodu tooan aplikace na hello virtuálních počítačů s nevrácenou pamětí nebo zastavení virtuálního počítače, jestliže uživatelského času procesoru hello byl nižší než 1 % pro poslední hodinu a není používán.</span><span class="sxs-lookup"><span data-stu-id="f93ce-110">Example scenarios include restarting a VM when hello memory usage exceeds some threshold due tooan application on hello VM with a memory leak, or stopping a VM when hello CPU user time has been below 1% for past hour and is not in use.</span></span> <span data-ttu-id="f93ce-111">Dále vysvětlíme, jak hello automatizovat vytvoření objektu služby ve vašem Automation účet zjednodušuje použití hello sad runbook v Azure výstrahy nápravy.</span><span class="sxs-lookup"><span data-stu-id="f93ce-111">We’ll also explain how hello automated creation of a service principal in your Automation account simplifies hello use of runbooks in Azure alert remediation.</span></span>

## <a name="create-an-alert-on-a-vm"></a><span data-ttu-id="f93ce-112">Vytvořit výstrahu na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="f93ce-112">Create an alert on a VM</span></span>
<span data-ttu-id="f93ce-113">Proveďte následující kroky tooconfigure výstrahy toolaunch sady runbook při jeho prahová hodnota byla splněna hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-113">Perform hello following steps tooconfigure an alert toolaunch a runbook when its threshold has been met.</span></span>

> [!NOTE]
> <span data-ttu-id="f93ce-114">V této verzi jsme podporují jenom V2 virtuální počítače a podporu pro classic, který bude brzy přidat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f93ce-114">With this release, we only support V2 virtual machines and support for classic VMs will be added soon.</span></span>  
> 
> 

1. <span data-ttu-id="f93ce-115">Přihlaste se toohello portál Azure a klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="f93ce-115">Log in toohello Azure portal and click **Virtual Machines**.</span></span>  
2. <span data-ttu-id="f93ce-116">Vyberte jednu z vašich virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f93ce-116">Select one of your virtual machines.</span></span>  <span data-ttu-id="f93ce-117">Hello okno řídicího panelu virtuální počítač se zobrazí a hello **nastavení** okno tooits vpravo.</span><span class="sxs-lookup"><span data-stu-id="f93ce-117">hello virtual machine dashboard blade will appear and hello **Settings** blade tooits right.</span></span>  
3. <span data-ttu-id="f93ce-118">Z hello **nastavení** okno, v části hello vyberte části monitorování **výstrah pravidla**.</span><span class="sxs-lookup"><span data-stu-id="f93ce-118">From hello **Settings** blade, under hello Monitoring section select **Alert rules**.</span></span>
4. <span data-ttu-id="f93ce-119">Na hello **výstrah pravidla** okně klikněte na tlačítko **přidat upozornění**.</span><span class="sxs-lookup"><span data-stu-id="f93ce-119">On hello **Alert rules** blade, click **Add alert**.</span></span>

<span data-ttu-id="f93ce-120">Otevře se hello **přidání pravidla výstrahy** okno, kde můžete nakonfigurovat podmínky hello hello výstrahy a volit z jednoho nebo všech z těchto možností: Odeslat toosomeone e-mailu, použít systém výstrah tooanother webhooku tooforward hello, nebo Spusťte runbook služby automatizace v odpovědi pokus o tooremediate hello problém.</span><span class="sxs-lookup"><span data-stu-id="f93ce-120">This opens up hello **Add an alert rule** blade, where you can configure hello conditions for hello alert and choose among one or all of these options: send email toosomeone, use a webhook tooforward hello alert tooanother system, and/or run an Automation runbook in response attempt tooremediate hello issue.</span></span>

## <a name="configure-a-runbook"></a><span data-ttu-id="f93ce-121">Konfigurace sady runbook</span><span class="sxs-lookup"><span data-stu-id="f93ce-121">Configure a runbook</span></span>
<span data-ttu-id="f93ce-122">Vyberte tooconfigure runbook toorun při splnění prahové hodnoty pro výstrahu virtuálních počítačů hello **sady Automation Runbook**.</span><span class="sxs-lookup"><span data-stu-id="f93ce-122">tooconfigure a runbook toorun when hello VM alert threshold is met, select **Automation Runbook**.</span></span> <span data-ttu-id="f93ce-123">V hello **konfigurace runbook** okno, můžete vybrat hello runbook toorun a hello hello runbook toorun účet Automation v.</span><span class="sxs-lookup"><span data-stu-id="f93ce-123">In hello **Configure runbook** blade, you can select hello runbook toorun and hello Automation account toorun hello runbook in.</span></span>

![Nakonfigurujte runbook služby automatizace a vytvořte nový účet Automation.](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> <span data-ttu-id="f93ce-125">Pro tuto verzi můžete zvolit ze tří sady runbook, které nabízí služba hello – restartování virtuálního počítače, zastavte virtuální počítač nebo odeberte virtuální počítač (odstranit).</span><span class="sxs-lookup"><span data-stu-id="f93ce-125">For this release you can choose from three runbooks that hello service provides – Restart VM, Stop VM, or Remove VM (delete it).</span></span>  <span data-ttu-id="f93ce-126">Hello možnost tooselect jiné sady runbook nebo jeden z vlastní sady runbook bude k dispozici v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="f93ce-126">hello ability tooselect other runbooks or one of your own runbooks will be available in a future release.</span></span>
> 
> 

![Sady Runbook toochoose z](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

<span data-ttu-id="f93ce-128">Po výběru hello tři dostupné sady runbook hello **účet Automation** rozevíracím seznamu se zobrazí a vyberete automation bude runbook hello účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="f93ce-128">After you select one of hello three available runbooks, hello **Automation account** drop-down list appears and you can select an automation account hello runbook will run as.</span></span> <span data-ttu-id="f93ce-129">Runbooky potřebují toorun v kontextu hello [účet Automation](automation-security-overview.md) se ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="f93ce-129">Runbooks need toorun in hello context of an [Automation account](automation-security-overview.md) that is in your Azure subscription.</span></span> <span data-ttu-id="f93ce-130">Můžete vybrat účet Automation, zda jste již vytvořili, nebo máte vytvořený pro vás nový účet automatizace.</span><span class="sxs-lookup"><span data-stu-id="f93ce-130">You can select an Automation account that you already created, or you can have a new Automation account created for you.</span></span>

<span data-ttu-id="f93ce-131">Hello sady runbook, které jsou k dispozici ověřit tooAzure používání objektu služby.</span><span class="sxs-lookup"><span data-stu-id="f93ce-131">hello runbooks that are provided authenticate tooAzure using a service principal.</span></span> <span data-ttu-id="f93ce-132">Pokud si zvolíte toorun hello runbook v jednom z vaší existující účty Automation, automaticky vytvoříme hello služby hlavní za vás.</span><span class="sxs-lookup"><span data-stu-id="f93ce-132">If you choose toorun hello runbook in one of your existing Automation accounts, we will automatically create hello service principal for you.</span></span> <span data-ttu-id="f93ce-133">Pokud si zvolíte toocreate nového účtu Automation, pak automaticky vytvoříme účet hello a hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="f93ce-133">If you choose toocreate a new Automation account, then we will automatically create hello account and hello service principal.</span></span> <span data-ttu-id="f93ce-134">V obou případech dva prostředky bude vytvořen také v hello účet Automation – certifikát asset s názvem **AzureRunAsCertificate** a připojení asset s názvem **AzureRunAsConnection**.</span><span class="sxs-lookup"><span data-stu-id="f93ce-134">In both cases, two assets will also be created in hello Automation account – a certificate asset named **AzureRunAsCertificate** and a connection asset named **AzureRunAsConnection**.</span></span> <span data-ttu-id="f93ce-135">sady runbook Hello použije **AzureRunAsConnection** tooauthenticate s Azure v pořadí tooperform hello správu akci vůči hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f93ce-135">hello runbooks will use **AzureRunAsConnection** tooauthenticate with Azure in order tooperform hello management action against hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="f93ce-136">Hello instanční objekt se vytvoří v oboru hello předplatného a je přiřazena role Přispěvatel hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-136">hello service principal is created in hello subscription scope and is assigned hello Contributor role.</span></span> <span data-ttu-id="f93ce-137">Tato role je nutná pro hello účet toohave oprávnění toorun automatizace sady runbook toomanage virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="f93ce-137">This role is required in order for hello account toohave permission toorun Automation runbooks toomanage Azure VMs.</span></span>  <span data-ttu-id="f93ce-138">Vytvoření Hello účtu Automaton nebo instančního objektu je jednorázové události.</span><span class="sxs-lookup"><span data-stu-id="f93ce-138">hello creation of an Automaton account and/or service principal is a one-time event.</span></span> <span data-ttu-id="f93ce-139">Po jejich vytvoření, můžete tento účet toorun runbooky pro jiné výstrahy virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="f93ce-139">Once they are created, you can use that account toorun runbooks for other Azure VM alerts.</span></span>
> 
> 

<span data-ttu-id="f93ce-140">Když kliknete na tlačítko **OK** hello výstraha je nakonfigurován, a pokud jste vybrali možnost toocreate hello nového účtu Automation, vytvoří se společně s hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="f93ce-140">When you click **OK** hello alert is configured and if you selected hello option toocreate a new Automation account, it is created along with hello service principal.</span></span>  <span data-ttu-id="f93ce-141">Může to trvat několik sekund toocomplete.</span><span class="sxs-lookup"><span data-stu-id="f93ce-141">This can take a few seconds toocomplete.</span></span>  

![Konfigurace sady Runbook](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

<span data-ttu-id="f93ce-143">Po dokončení konfigurace hello uvidíte hello název sady runbook hello se zobrazí v hello **přidání pravidla výstrahy** okno.</span><span class="sxs-lookup"><span data-stu-id="f93ce-143">After hello configuration is completed you will see hello name of hello runbook appear in hello **Add an alert rule** blade.</span></span>

![Runbook nakonfigurovat](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

<span data-ttu-id="f93ce-145">Klikněte na tlačítko **OK** v hello **přidání pravidla výstrahy** okno a hello pravidlo výstrahy se vytvoří a aktivovat, pokud je hello virtuální počítač ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="f93ce-145">Click **OK** in hello **Add an alert rule** blade and hello alert rule will be created and activate if hello virtual machine is in a running state.</span></span>

### <a name="enable-or-disable-a-runbook"></a><span data-ttu-id="f93ce-146">Povolit nebo zakázat sady runbook</span><span class="sxs-lookup"><span data-stu-id="f93ce-146">Enable or disable a runbook</span></span>
<span data-ttu-id="f93ce-147">Pokud je runbook nakonfigurovaný pro výstrahy, ji můžete vypnout bez odebrání konfigurace nástroje runbook hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-147">If you have a runbook configured for an alert, you can disable it without removing hello runbook configuration.</span></span> <span data-ttu-id="f93ce-148">To vám umožní tookeep hello výstraha spuštěná a možná testování hello pravidla výstrah a později znovu povolit hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f93ce-148">This allows you tookeep hello alert running and perhaps test some of hello alert rules and then later re-enable hello runbook.</span></span>

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a><span data-ttu-id="f93ce-149">Vytvoření sady runbook, který funguje s Azure výstrahy</span><span class="sxs-lookup"><span data-stu-id="f93ce-149">Create a runbook that works with an Azure alert</span></span>
<span data-ttu-id="f93ce-150">Když zvolíte sady runbook v rámci Azure pravidla výstrahy, hello runbook musí hello toomanage výstrahy se data, která je předána tooit toohave logiku v ní.</span><span class="sxs-lookup"><span data-stu-id="f93ce-150">When you choose a runbook as part of an Azure alert rule, hello runbook needs toohave logic in it toomanage hello alert data that is passed tooit.</span></span>  <span data-ttu-id="f93ce-151">Pokud sada runbook je nakonfigurován v pravidlo výstrahy, webhook, jehož je vytvořena pro sadu runbook hello; Tento webhook je pak použít toostart hello runbook každé čas hello výstrahy aktivační události.</span><span class="sxs-lookup"><span data-stu-id="f93ce-151">When a runbook is configured in an alert rule, a webhook is created for hello runbook; that webhook is then used toostart hello runbook each time hello alert triggers.</span></span>  <span data-ttu-id="f93ce-152">Hello vlastní volání toostart hello runbook je adresu URL webhooku toohello požadavek HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f93ce-152">hello actual call toostart hello runbook is an HTTP POST request toohello webhook URL.</span></span> <span data-ttu-id="f93ce-153">Hello text požadavku POST hello obsahuje objekt uvedena ve správném formátu JSON, který obsahuje užitečné vlastnosti toohello související výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f93ce-153">hello body of hello POST request contains a JSON-formated object that contains useful properties related toohello alert.</span></span>  <span data-ttu-id="f93ce-154">Jak vidíte níže, dat výstrah hello obsahuje podrobnosti, třeba ID předplatného, název skupiny prostředků, resourceName a typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="f93ce-154">As you can see below, hello alert data contains details like subscriptionID, resourceGroupName, resourceName, and resourceType.</span></span>

### <a name="example-of-alert-data"></a><span data-ttu-id="f93ce-155">Příklad dat výstrah</span><span class="sxs-lookup"><span data-stu-id="f93ce-155">Example of Alert data</span></span>
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

<span data-ttu-id="f93ce-156">Přijetí hello webhook služby automatizace hello HTTP POST extrahuje hello dat výstrah a předává je toohello runbook ve vstupní parametr runbooku WebhookData hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-156">When hello Automation webhook service receives hello HTTP POST it extracts hello alert data and passes it toohello runbook in hello WebhookData runbook input parameter.</span></span>  <span data-ttu-id="f93ce-157">Níže je ukázkové sady runbook, který ukazuje, jak toouse hello WebhookData parametr a extrahování dat výstrah hello a použít ho toomanage hello prostředků Azure, který aktivoval výstrahu hello.</span><span class="sxs-lookup"><span data-stu-id="f93ce-157">Below is a sample runbook that shows how toouse hello WebhookData parameter and extract hello alert data and use it toomanage hello Azure resource that triggered hello alert.</span></span>

### <a name="example-runbook"></a><span data-ttu-id="f93ce-158">Ukázkový runbook</span><span class="sxs-lookup"><span data-stu-id="f93ce-158">Example runbook</span></span>
```
#  This runbook will restart an ARM (V2) VM in response tooan Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get hello data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that hello alert status is 'Activated' (alert condition went from false tootrue)
    # and not 'Resolved' (alert condition went from true toofalse)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get hello info needed tooidentify hello VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is hello expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate tooAzure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in hello Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart hello VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # hello alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant toobe started from an Azure alert only."
}
```

## <a name="summary"></a><span data-ttu-id="f93ce-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f93ce-159">Summary</span></span>
<span data-ttu-id="f93ce-160">Když konfigurujete výstrahu na virtuální počítač Azure, teď máte tooeasily hello možnost konfigurace Automation runbook tooautomatically provést akci nápravy, která při hello výstraha aktivuje.</span><span class="sxs-lookup"><span data-stu-id="f93ce-160">When you configure an alert on an Azure VM, you now have hello ability tooeasily configure an Automation runbook tooautomatically perform remediation action when hello alert triggers.</span></span> <span data-ttu-id="f93ce-161">Pro tuto verzi můžete si vybrat z toorestart sady runbook, zastavení nebo odstranit virtuální počítač v závislosti na vašem scénáři výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f93ce-161">For this release, you can choose from runbooks toorestart, stop, or delete a VM depending on your alert scenario.</span></span> <span data-ttu-id="f93ce-162">Toto je pouze hello začátku povolení scénáře, kde můžete řídit akce hello (oznámení, řešení potíží s nápravy), které se mají provést automaticky, když se aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="f93ce-162">This is just hello beginning of enabling scenarios where you control hello actions (notification, troubleshooting, remediation) that will be taken automatically when an alert triggers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f93ce-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f93ce-163">Next Steps</span></span>
* <span data-ttu-id="f93ce-164">tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="f93ce-164">tooget started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span>
* <span data-ttu-id="f93ce-165">tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="f93ce-165">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="f93ce-166">toolearn Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)</span><span class="sxs-lookup"><span data-stu-id="f93ce-166">toolearn more about runbook types, their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md)</span></span>

