---
title: "brány sítě VPN aaaMonitor při řešení problémů sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak diagnostikovat místní připojení Azure Automation a sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a607d0c862ea1be63c687717f0c5dc137db58a43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a><span data-ttu-id="6b061-103">Sledování brány sítě VPN s řešení potíží s sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="6b061-103">Monitor VPN gateways with Network Watcher troubleshooting</span></span>

<span data-ttu-id="6b061-104">Získání hlubšímu porozumění na výkonu sítě je důležité tooprovide toocustomers spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="6b061-104">Gaining deep insights on your network performance is critical tooprovide reliable services toocustomers.</span></span> <span data-ttu-id="6b061-105">Je proto důležité toodetect síťové podmínky výpadku rychle a trvat stav výpadku hello toomitigate opravné akce.</span><span class="sxs-lookup"><span data-stu-id="6b061-105">It is therefore critical toodetect network outage conditions quickly and take corrective action toomitigate hello outage condition.</span></span> <span data-ttu-id="6b061-106">Automatizace Azure umožňuje tooimplement a spusťte úlohu programový způsobem prostřednictvím sady runbook.</span><span class="sxs-lookup"><span data-stu-id="6b061-106">Azure Automation enables you tooimplement and run a task in a programmatic fashion through runbooks.</span></span> <span data-ttu-id="6b061-107">Pomocí Azure Automation vytvoří ideální recepturách pro provádění sítě nepřetržitý a Proaktivní monitorování a výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6b061-107">Using Azure Automation creates a perfect recipe for performing continuous and proactive network monitoring and alerting.</span></span>

## <a name="scenario"></a><span data-ttu-id="6b061-108">Scénář</span><span class="sxs-lookup"><span data-stu-id="6b061-108">Scenario</span></span>

<span data-ttu-id="6b061-109">scénář Hello v hello následující obrázek je víceúrovňových aplikace, s na připojení místní navázat pomocí brány sítě VPN a tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="6b061-109">hello scenario in hello following image is a multi-tiered application, with on premises connectivity established using a VPN Gateway and tunnel.</span></span> <span data-ttu-id="6b061-110">Zajištění hello VPN Gateway je zapnutý a spuštění je důležité toohello výkonu aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b061-110">Ensuring hello VPN Gateway is up and running is critical toohello applications performance.</span></span>

<span data-ttu-id="6b061-111">Runbook se vytvoří s toocheck skript pro stav připojení hello tunelového připojení sítě VPN, pomocí toocheck hello prostředků řešení potíží s rozhraní API pro stav připojení tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="6b061-111">A runbook is created with a script toocheck for connection status of hello VPN tunnel, using hello Resource Troubleshooting API toocheck for connection tunnel status.</span></span> <span data-ttu-id="6b061-112">Pokud hello stav není v pořádku, odešle aktivační procedury e-mailu tooadministrators.</span><span class="sxs-lookup"><span data-stu-id="6b061-112">If hello status is not healthy, an email trigger is sent tooadministrators.</span></span>

![Příklad scénáře][scenario]

<span data-ttu-id="6b061-114">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="6b061-114">This scenario will:</span></span>

- <span data-ttu-id="6b061-115">Vytváření volání hello runbook `Start-AzureRmNetworkWatcherResourceTroubleshooting` stav připojení tootroubleshoot rutiny</span><span class="sxs-lookup"><span data-stu-id="6b061-115">Create a runbook calling hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet tootroubleshoot connection status</span></span>
- <span data-ttu-id="6b061-116">Propojit plán toohello runbooku</span><span class="sxs-lookup"><span data-stu-id="6b061-116">Link a schedule toohello runbook</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6b061-117">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6b061-117">Before you begin</span></span>

<span data-ttu-id="6b061-118">Než začnete tento scénář, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="6b061-118">Before you start this scenario, you must have hello following pre-requisites:</span></span>

- <span data-ttu-id="6b061-119">Účet Azure automation v Azure.</span><span class="sxs-lookup"><span data-stu-id="6b061-119">An Azure automation account in Azure.</span></span> <span data-ttu-id="6b061-120">Zajistěte, aby účet automation hello hello nejnovější modulů a také obsahuje modul AzureRM.Network hello.</span><span class="sxs-lookup"><span data-stu-id="6b061-120">Ensure that hello automation account has hello latest modules and also has hello AzureRM.Network module.</span></span> <span data-ttu-id="6b061-121">Hello AzureRM.Network modulu je k dispozici v galerii modulů hello tooadd potřebujete-li ji tooyour účet automation.</span><span class="sxs-lookup"><span data-stu-id="6b061-121">hello AzureRM.Network module is available in hello module gallery if you need tooadd it tooyour automation account.</span></span>
- <span data-ttu-id="6b061-122">Musíte mít sadu přihlašovacích údajů nakonfigurovat ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="6b061-122">You must have a set of credentials configure in Azure Automation.</span></span> <span data-ttu-id="6b061-123">Další informace na [zabezpečení Azure Automation.](../automation/automation-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="6b061-123">Learn more at [Azure Automation security](../automation/automation-security-overview.md)</span></span>
- <span data-ttu-id="6b061-124">Platný server SMTP (Office 365, místní e-mailu nebo jiné) a přihlašovací údaje definované ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="6b061-124">A valid SMTP server (Office 365, your on-premises email or another) and credentials defined in Azure Automation</span></span>
- <span data-ttu-id="6b061-125">Nakonfigurované virtuální bránu sítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="6b061-125">A configured Virtual Network Gateway in Azure.</span></span>
- <span data-ttu-id="6b061-126">Přihlásí se existující toostore hello kontejneru stávající účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6b061-126">An existing storage account with an existing container toostore hello logs in.</span></span>

> [!NOTE]
> <span data-ttu-id="6b061-127">Hello infrastruktury znázorněný v hello předcházející bitové kopie je pro účely obrázku a nejsou vytvořeny s hello kroky obsažené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6b061-127">hello infrastructure depicted in hello preceding image is for illustration purposes and are not created with hello steps contained in this article.</span></span>

### <a name="create-hello-runbook"></a><span data-ttu-id="6b061-128">Vytvoření sady runbook hello</span><span class="sxs-lookup"><span data-stu-id="6b061-128">Create hello runbook</span></span>

<span data-ttu-id="6b061-129">Příklad hello tooconfiguring první krok Hello je toocreate hello runbook.</span><span class="sxs-lookup"><span data-stu-id="6b061-129">hello first step tooconfiguring hello example is toocreate hello runbook.</span></span> <span data-ttu-id="6b061-130">Tento příklad používá účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="6b061-130">This example uses a run-as account.</span></span> <span data-ttu-id="6b061-131">toolearn informace o účtech spustit jako, navštivte [ověření Runbooků pomocí účtu spustit v Azure jako](../automation/automation-sec-configure-azure-runas-account.md)</span><span class="sxs-lookup"><span data-stu-id="6b061-131">toolearn about run-as accounts, visit [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)</span></span>

### <a name="step-1"></a><span data-ttu-id="6b061-132">Krok 1</span><span class="sxs-lookup"><span data-stu-id="6b061-132">Step 1</span></span>

<span data-ttu-id="6b061-133">Přejděte tooAzure automatizace v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sady Runbook**</span><span class="sxs-lookup"><span data-stu-id="6b061-133">Navigate tooAzure Automation in hello [Azure portal](https://portal.azure.com) and click **Runbooks**</span></span>

![Přehled účtu Automation][1]

### <a name="step-2"></a><span data-ttu-id="6b061-135">Krok 2</span><span class="sxs-lookup"><span data-stu-id="6b061-135">Step 2</span></span>

<span data-ttu-id="6b061-136">Klikněte na tlačítko **přidat runbook** toostart hello proces vytvoření sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="6b061-136">Click **Add a runbook** toostart hello creation process of hello runbook.</span></span>

![okno sady runbook][2]

### <a name="step-3"></a><span data-ttu-id="6b061-138">Krok 3</span><span class="sxs-lookup"><span data-stu-id="6b061-138">Step 3</span></span>

<span data-ttu-id="6b061-139">V části **rychle vytvořit**, klikněte na tlačítko **vytvořit nový runbook** toocreate hello runbook.</span><span class="sxs-lookup"><span data-stu-id="6b061-139">Under **Quick Create**, click **Create a new runbook** toocreate hello runbook.</span></span>

![runbook okně Přidat][3]

### <a name="step-4"></a><span data-ttu-id="6b061-141">Krok 4</span><span class="sxs-lookup"><span data-stu-id="6b061-141">Step 4</span></span>

<span data-ttu-id="6b061-142">V tomto kroku jsme pojmenujte hello runbook, v příkladu hello se označuje jako **Get-VPNGatewayStatus**.</span><span class="sxs-lookup"><span data-stu-id="6b061-142">In this step, we give hello runbook a name, in hello example it is called **Get-VPNGatewayStatus**.</span></span> <span data-ttu-id="6b061-143">Je důležité toogive hello runbook popisný název a doporučuje pod názvem, který následuje standardní standardy pro vytváření názvů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6b061-143">It is important toogive hello runbook a descriptive name, and recommended giving it a name that follows standard PowerShell naming standards.</span></span> <span data-ttu-id="6b061-144">Typ runbooku Hello v tomto příkladu je **prostředí PowerShell**, hello ostatní možnosti patří grafický pracovního postupu Powershellu a pracovní postup grafické prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6b061-144">hello runbook type for this example is **PowerShell**, hello other options are Graphical, PowerShell workflow, and Graphical PowerShell workflow.</span></span>

![okno sady runbook][4]

### <a name="step-5"></a><span data-ttu-id="6b061-146">Krok 5</span><span class="sxs-lookup"><span data-stu-id="6b061-146">Step 5</span></span>

<span data-ttu-id="6b061-147">V tomto kroku hello sady runbook se vytvoří, hello následující ukázka kódu obsahuje že všechny hello kód potřebný například hello.</span><span class="sxs-lookup"><span data-stu-id="6b061-147">In this step hello runbook is created, hello following code example provides all hello code needed for hello example.</span></span> <span data-ttu-id="6b061-148">Hello položky v hello kódu, které obsahují \<hodnotu\> potřebovat toobe nahradit hello hodnotami ze svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="6b061-148">hello items in hello code that contain \<value\> need toobe replaced with hello values from your subscription.</span></span>

<span data-ttu-id="6b061-149">Použití hello následující kód jako klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="6b061-149">Use hello following code as click **Save**</span></span>

```PowerShell
# Set these variables toohello proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<tooemail address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get hello connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in tooAzure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context tooa specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView hello logs at $($storagePath) toolearn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -too$toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a><span data-ttu-id="6b061-150">Krok 6</span><span class="sxs-lookup"><span data-stu-id="6b061-150">Step 6</span></span>

<span data-ttu-id="6b061-151">Po uložení hello runbook plánu musí být propojená tooit tooautomate hello spuštění sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="6b061-151">Once hello runbook is saved, a schedule must be linked tooit tooautomate hello start of hello runbook.</span></span> <span data-ttu-id="6b061-152">proces hello toostart, klikněte na tlačítko **plán**.</span><span class="sxs-lookup"><span data-stu-id="6b061-152">toostart hello process, click **Schedule**.</span></span>

![Krok 6][6]

## <a name="link-a-schedule-toohello-runbook"></a><span data-ttu-id="6b061-154">Propojit plán toohello runbooku</span><span class="sxs-lookup"><span data-stu-id="6b061-154">Link a schedule toohello runbook</span></span>

<span data-ttu-id="6b061-155">Je nutné vytvořit nový plán.</span><span class="sxs-lookup"><span data-stu-id="6b061-155">A new schedule must be created.</span></span> <span data-ttu-id="6b061-156">Klikněte na tlačítko **propojit plán tooyour runbook**.</span><span class="sxs-lookup"><span data-stu-id="6b061-156">Click **Link a schedule tooyour runbook**.</span></span>

![Krok 7][7]

### <a name="step-1"></a><span data-ttu-id="6b061-158">Krok 1</span><span class="sxs-lookup"><span data-stu-id="6b061-158">Step 1</span></span>

<span data-ttu-id="6b061-159">Na hello **plán** okně klikněte na tlačítko **vytvoření nového plánu**</span><span class="sxs-lookup"><span data-stu-id="6b061-159">On hello **Schedule** blade, click **Create a new schedule**</span></span>

![Krok 8][8]

### <a name="step-2"></a><span data-ttu-id="6b061-161">Krok 2</span><span class="sxs-lookup"><span data-stu-id="6b061-161">Step 2</span></span>

<span data-ttu-id="6b061-162">Na hello **nový plán** výplně okno se informace o plánu hello.</span><span class="sxs-lookup"><span data-stu-id="6b061-162">On hello **New Schedule** blade fill out hello schedule information.</span></span> <span data-ttu-id="6b061-163">v následujícím seznamu hello jsou Hello hodnoty, které lze nastavit:</span><span class="sxs-lookup"><span data-stu-id="6b061-163">hello values that can be set are in hello following list:</span></span>

- <span data-ttu-id="6b061-164">**Název** -hello popisný název plánu hello.</span><span class="sxs-lookup"><span data-stu-id="6b061-164">**Name** - hello friendly name of hello schedule.</span></span>
- <span data-ttu-id="6b061-165">**Popis** – popis hello plánu.</span><span class="sxs-lookup"><span data-stu-id="6b061-165">**Description** - A description of hello schedule.</span></span>
- <span data-ttu-id="6b061-166">**Spustí** – tato hodnota je kombinací datum, čas a časové pásmo, které tvoří hello čas hello plán aktivační události.</span><span class="sxs-lookup"><span data-stu-id="6b061-166">**Starts** - This value is a combination of date, time, and time zone that make up hello time hello schedule triggers.</span></span>
- <span data-ttu-id="6b061-167">**Opakování** – tato hodnota určuje hello plány opakování.</span><span class="sxs-lookup"><span data-stu-id="6b061-167">**Recurrence** - This value determines hello schedules repetition.</span></span>  <span data-ttu-id="6b061-168">Platné hodnoty jsou **jednou** nebo **opakovaná**.</span><span class="sxs-lookup"><span data-stu-id="6b061-168">Valid values are **Once** or **Recurring**.</span></span>
- <span data-ttu-id="6b061-169">**Opakovat každý** – interval opakování hello hello plánu v hodiny, dny, týdny nebo měsíce.</span><span class="sxs-lookup"><span data-stu-id="6b061-169">**Recur every** - hello recurrence interval of hello schedule in hours, days, weeks, or months.</span></span>
- <span data-ttu-id="6b061-170">**Nastavit dobu platnosti** -hello hodnota určuje, pokud by měla vypršet hello plánu, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6b061-170">**Set Expiration** - hello value determines if hello schedule should expire or not.</span></span> <span data-ttu-id="6b061-171">Můžete nastavit příliš**Ano** nebo **ne**.</span><span class="sxs-lookup"><span data-stu-id="6b061-171">Can be set too**Yes** or **No**.</span></span> <span data-ttu-id="6b061-172">Platné datum a čas se toobe zadán, pokud ano je vybrán.</span><span class="sxs-lookup"><span data-stu-id="6b061-172">A valid date and time are toobe provided if yes is chosen.</span></span>

> [!NOTE]
> <span data-ttu-id="6b061-173">Pokud potřebujete toohave sady runbook spustit častěji než každou hodinu, je nutné vytvořit více plánů v různých intervalech (to znamená, 15, 30, 45 minut po hodině hello)</span><span class="sxs-lookup"><span data-stu-id="6b061-173">If you need toohave a runbook run more often than every hour, multiple schedules must be created at different intervals (that is, 15, 30, 45 minutes after hello hour)</span></span>

![Krok 9][9]

### <a name="step-3"></a><span data-ttu-id="6b061-175">Krok 3</span><span class="sxs-lookup"><span data-stu-id="6b061-175">Step 3</span></span>

<span data-ttu-id="6b061-176">Klikněte na Uložit toosave hello plán toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="6b061-176">Click Save toosave hello schedule toohello runbook.</span></span>

![Krok 10][10]

## <a name="next-steps"></a><span data-ttu-id="6b061-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b061-178">Next steps</span></span>

<span data-ttu-id="6b061-179">Teď, když máte představu o tom, toointegrate sledovací proces sítě řešení potíží s Azure Automation, zjistěte, jak zaznamená tootrigger paketů na virtuální počítač výstrahy navštivte stránky [vytvořit zaznamenání výstrahy spouštěná paketů s sledovací proces sítě Azure](network-watcher-alert-triggered-packet-capture.md).</span><span class="sxs-lookup"><span data-stu-id="6b061-179">Now that you have an understanding on how toointegrate Network Watcher troubleshooting with Azure Automation, learn how tootrigger packet captures on VM alerts by visiting [Create an alert triggered packet capture with Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).</span></span>

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
