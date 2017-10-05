---
title: "Monitorování s řešení potíží s sledovací proces sítě Azure VPN Gateway | Microsoft Docs"
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
ms.openlocfilehash: 55ec52dd0d94a0347cc67a8785b89611da955111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a><span data-ttu-id="a0bb9-103">Sledování brány sítě VPN s řešení potíží s sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="a0bb9-103">Monitor VPN gateways with Network Watcher troubleshooting</span></span>

<span data-ttu-id="a0bb9-104">Získání hlubšímu porozumění na výkonu sítě je důležité poskytují spolehlivé služby zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-104">Gaining deep insights on your network performance is critical to provide reliable services to customers.</span></span> <span data-ttu-id="a0bb9-105">Proto je důležité ke zjišťování síťové podmínky výpadku rychle a podniknout kroky ke zmírnění stav výpadku.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-105">It is therefore critical to detect network outage conditions quickly and take corrective action to mitigate the outage condition.</span></span> <span data-ttu-id="a0bb9-106">Automatizace Azure umožňuje implementovat a spusťte úlohu programový způsobem prostřednictvím sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-106">Azure Automation enables you to implement and run a task in a programmatic fashion through runbooks.</span></span> <span data-ttu-id="a0bb9-107">Pomocí Azure Automation vytvoří ideální recepturách pro provádění sítě nepřetržitý a Proaktivní monitorování a výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-107">Using Azure Automation creates a perfect recipe for performing continuous and proactive network monitoring and alerting.</span></span>

## <a name="scenario"></a><span data-ttu-id="a0bb9-108">Scénář</span><span class="sxs-lookup"><span data-stu-id="a0bb9-108">Scenario</span></span>

<span data-ttu-id="a0bb9-109">Scénář na následujícím obrázku je víceúrovňových aplikace, se na místní připojení navázat pomocí brány sítě VPN a tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-109">The scenario in the following image is a multi-tiered application, with on premises connectivity established using a VPN Gateway and tunnel.</span></span> <span data-ttu-id="a0bb9-110">Zajištění, že služby VPN Gateway je zapnutý a spuštění je důležité pro výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-110">Ensuring the VPN Gateway is up and running is critical to the applications performance.</span></span>

<span data-ttu-id="a0bb9-111">Runbook se vytvoří pomocí skriptu zkontrolujte stav připojení tunelového propojení sítě VPN, pomocí rozhraní API řešení potíží s prostředků zkontrolujte stav tunelového připojení.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-111">A runbook is created with a script to check for connection status of the VPN tunnel, using the Resource Troubleshooting API to check for connection tunnel status.</span></span> <span data-ttu-id="a0bb9-112">Pokud stav není v pořádku, se správcům odešle aktivační procedury e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-112">If the status is not healthy, an email trigger is sent to administrators.</span></span>

![Příklad scénáře][scenario]

<span data-ttu-id="a0bb9-114">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="a0bb9-114">This scenario will:</span></span>

- <span data-ttu-id="a0bb9-115">Vytvoření volání sady runbook `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutiny, chcete-li vyřešit stav připojení</span><span class="sxs-lookup"><span data-stu-id="a0bb9-115">Create a runbook calling the `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet to troubleshoot connection status</span></span>
- <span data-ttu-id="a0bb9-116">Propojit plán s sady runbook</span><span class="sxs-lookup"><span data-stu-id="a0bb9-116">Link a schedule to the runbook</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a0bb9-117">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a0bb9-117">Before you begin</span></span>

<span data-ttu-id="a0bb9-118">Než začnete tento scénář, musíte mít následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="a0bb9-118">Before you start this scenario, you must have the following pre-requisites:</span></span>

- <span data-ttu-id="a0bb9-119">Účet Azure automation v Azure.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-119">An Azure automation account in Azure.</span></span> <span data-ttu-id="a0bb9-120">Zajistěte, aby účet automation nejnovější modulů a také obsahuje modul AzureRM.Network.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-120">Ensure that the automation account has the latest modules and also has the AzureRM.Network module.</span></span> <span data-ttu-id="a0bb9-121">Modul AzureRM.Network je k dispozici v galerii modulů, pokud je potřeba ho přidat do vašeho účtu automation.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-121">The AzureRM.Network module is available in the module gallery if you need to add it to your automation account.</span></span>
- <span data-ttu-id="a0bb9-122">Musíte mít sadu přihlašovacích údajů nakonfigurovat ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-122">You must have a set of credentials configure in Azure Automation.</span></span> <span data-ttu-id="a0bb9-123">Další informace na [zabezpečení Azure Automation.](../automation/automation-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a0bb9-123">Learn more at [Azure Automation security](../automation/automation-security-overview.md)</span></span>
- <span data-ttu-id="a0bb9-124">Platný server SMTP (Office 365, místní e-mailu nebo jiné) a přihlašovací údaje definované ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a0bb9-124">A valid SMTP server (Office 365, your on-premises email or another) and credentials defined in Azure Automation</span></span>
- <span data-ttu-id="a0bb9-125">Nakonfigurované virtuální bránu sítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-125">A configured Virtual Network Gateway in Azure.</span></span>
- <span data-ttu-id="a0bb9-126">Stávající účet úložiště s existující kontejner k ukládání protokolů v.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-126">An existing storage account with an existing container to store the logs in.</span></span>

> [!NOTE]
> <span data-ttu-id="a0bb9-127">Infrastruktury, které popsané v předchozí obrázek je pro účely obrázku a nejsou vytvořeny s kroky obsažené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-127">The infrastructure depicted in the preceding image is for illustration purposes and are not created with the steps contained in this article.</span></span>

### <a name="create-the-runbook"></a><span data-ttu-id="a0bb9-128">Vytvoření sady runbook</span><span class="sxs-lookup"><span data-stu-id="a0bb9-128">Create the runbook</span></span>

<span data-ttu-id="a0bb9-129">Prvním krokem k konfigurace v příkladu se k vytvoření sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-129">The first step to configuring the example is to create the runbook.</span></span> <span data-ttu-id="a0bb9-130">Tento příklad používá účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-130">This example uses a run-as account.</span></span> <span data-ttu-id="a0bb9-131">Další informace o účtech spustit jako najdete [ověření Runbooků pomocí účtu spustit v Azure jako](../automation/automation-sec-configure-azure-runas-account.md)</span><span class="sxs-lookup"><span data-stu-id="a0bb9-131">To learn about run-as accounts, visit [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)</span></span>

### <a name="step-1"></a><span data-ttu-id="a0bb9-132">Krok 1</span><span class="sxs-lookup"><span data-stu-id="a0bb9-132">Step 1</span></span>

<span data-ttu-id="a0bb9-133">Přejděte do Azure Automation v [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sady Runbook**</span><span class="sxs-lookup"><span data-stu-id="a0bb9-133">Navigate to Azure Automation in the [Azure portal](https://portal.azure.com) and click **Runbooks**</span></span>

![Přehled účtu Automation][1]

### <a name="step-2"></a><span data-ttu-id="a0bb9-135">Krok 2</span><span class="sxs-lookup"><span data-stu-id="a0bb9-135">Step 2</span></span>

<span data-ttu-id="a0bb9-136">Klikněte na tlačítko **přidat runbook** ke spuštění procesu vytvoření sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-136">Click **Add a runbook** to start the creation process of the runbook.</span></span>

![okno sady runbook][2]

### <a name="step-3"></a><span data-ttu-id="a0bb9-138">Krok 3</span><span class="sxs-lookup"><span data-stu-id="a0bb9-138">Step 3</span></span>

<span data-ttu-id="a0bb9-139">V části **rychle vytvořit**, klikněte na tlačítko **vytvořit nový runbook** k vytvoření sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-139">Under **Quick Create**, click **Create a new runbook** to create the runbook.</span></span>

![runbook okně Přidat][3]

### <a name="step-4"></a><span data-ttu-id="a0bb9-141">Krok 4</span><span class="sxs-lookup"><span data-stu-id="a0bb9-141">Step 4</span></span>

<span data-ttu-id="a0bb9-142">V tomto kroku jsme dejte runbooku název, v příkladu se označuje jako **Get-VPNGatewayStatus**.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-142">In this step, we give the runbook a name, in the example it is called **Get-VPNGatewayStatus**.</span></span> <span data-ttu-id="a0bb9-143">Je důležité si dejte runbooku popisný název a doporučené pod názvem, který následuje standardní standardy pro vytváření názvů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-143">It is important to give the runbook a descriptive name, and recommended giving it a name that follows standard PowerShell naming standards.</span></span> <span data-ttu-id="a0bb9-144">Typ sady runbook v tomto příkladu je **prostředí PowerShell**, ostatní možnosti patří grafický pracovního postupu Powershellu a pracovní postup grafické prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-144">The runbook type for this example is **PowerShell**, the other options are Graphical, PowerShell workflow, and Graphical PowerShell workflow.</span></span>

![okno sady runbook][4]

### <a name="step-5"></a><span data-ttu-id="a0bb9-146">Krok 5</span><span class="sxs-lookup"><span data-stu-id="a0bb9-146">Step 5</span></span>

<span data-ttu-id="a0bb9-147">Následující příklad kódu v tomto kroku, který je sada runbook vytvořena poskytuje všechny potřebné pro tento příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-147">In this step the runbook is created, the following code example provides all the code needed for the example.</span></span> <span data-ttu-id="a0bb9-148">Položky v kódu, které obsahují \<hodnota\> je nutné nahradit s hodnotami ze svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-148">The items in the code that contain \<value\> need to be replaced with the values from your subscription.</span></span>

<span data-ttu-id="a0bb9-149">Použít následující kód jako klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="a0bb9-149">Use the following code as click **Save**</span></span>

```PowerShell
# Set these variables to the proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<to email address>"
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

# Get the connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in to Azure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context to a specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView the logs at $($storagePath) to learn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -To $toEmail `
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

### <a name="step-6"></a><span data-ttu-id="a0bb9-150">Krok 6</span><span class="sxs-lookup"><span data-stu-id="a0bb9-150">Step 6</span></span>

<span data-ttu-id="a0bb9-151">Po uložení runbooku plánu musí být propojena k němu automatizovat spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-151">Once the runbook is saved, a schedule must be linked to it to automate the start of the runbook.</span></span> <span data-ttu-id="a0bb9-152">Chcete-li spustit proces, klikněte na tlačítko **plán**.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-152">To start the process, click **Schedule**.</span></span>

![Krok 6][6]

## <a name="link-a-schedule-to-the-runbook"></a><span data-ttu-id="a0bb9-154">Propojit plán s sady runbook</span><span class="sxs-lookup"><span data-stu-id="a0bb9-154">Link a schedule to the runbook</span></span>

<span data-ttu-id="a0bb9-155">Je nutné vytvořit nový plán.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-155">A new schedule must be created.</span></span> <span data-ttu-id="a0bb9-156">Klikněte na tlačítko **propojit plán s runbookem**.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-156">Click **Link a schedule to your runbook**.</span></span>

![Krok 7][7]

### <a name="step-1"></a><span data-ttu-id="a0bb9-158">Krok 1</span><span class="sxs-lookup"><span data-stu-id="a0bb9-158">Step 1</span></span>

<span data-ttu-id="a0bb9-159">Na **plán** okně klikněte na tlačítko **vytvoření nového plánu**</span><span class="sxs-lookup"><span data-stu-id="a0bb9-159">On the **Schedule** blade, click **Create a new schedule**</span></span>

![Krok 8][8]

### <a name="step-2"></a><span data-ttu-id="a0bb9-161">Krok 2</span><span class="sxs-lookup"><span data-stu-id="a0bb9-161">Step 2</span></span>

<span data-ttu-id="a0bb9-162">Na **nový plán** výplně okno se informace o plánu.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-162">On the **New Schedule** blade fill out the schedule information.</span></span> <span data-ttu-id="a0bb9-163">V následujícím seznamu jsou hodnoty, které lze nastavit:</span><span class="sxs-lookup"><span data-stu-id="a0bb9-163">The values that can be set are in the following list:</span></span>

- <span data-ttu-id="a0bb9-164">**Název** -popisný název plánu.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-164">**Name** - The friendly name of the schedule.</span></span>
- <span data-ttu-id="a0bb9-165">**Popis** – popis plánu.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-165">**Description** - A description of the schedule.</span></span>
- <span data-ttu-id="a0bb9-166">**Spustí** – tato hodnota je kombinací datum, čas a časové pásmo, které tvoří časový plán aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-166">**Starts** - This value is a combination of date, time, and time zone that make up the time the schedule triggers.</span></span>
- <span data-ttu-id="a0bb9-167">**Opakování** – tato hodnota určuje opakování plány.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-167">**Recurrence** - This value determines the schedules repetition.</span></span>  <span data-ttu-id="a0bb9-168">Platné hodnoty jsou **jednou** nebo **opakovaná**.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-168">Valid values are **Once** or **Recurring**.</span></span>
- <span data-ttu-id="a0bb9-169">**Opakovat každý** – interval opakování plánu v hodiny, dny, týdny nebo měsíce.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-169">**Recur every** - The recurrence interval of the schedule in hours, days, weeks, or months.</span></span>
- <span data-ttu-id="a0bb9-170">**Nastavit dobu platnosti** -hodnota určuje, pokud by měla vypršet podle plánu, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-170">**Set Expiration** - The value determines if the schedule should expire or not.</span></span> <span data-ttu-id="a0bb9-171">Může být nastaven na **Ano** nebo **ne**.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-171">Can be set to **Yes** or **No**.</span></span> <span data-ttu-id="a0bb9-172">Pokud ano jste vybrali, musíte zadat jsou platné datum a čas.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-172">A valid date and time are to be provided if yes is chosen.</span></span>

> [!NOTE]
> <span data-ttu-id="a0bb9-173">Pokud je potřeba mít sady runbook spustit častěji než každou hodinu, je nutné vytvořit více plánů v různých intervalech (to znamená, 15, 30, 45 minut po hodině)</span><span class="sxs-lookup"><span data-stu-id="a0bb9-173">If you need to have a runbook run more often than every hour, multiple schedules must be created at different intervals (that is, 15, 30, 45 minutes after the hour)</span></span>

![Krok 9][9]

### <a name="step-3"></a><span data-ttu-id="a0bb9-175">Krok 3</span><span class="sxs-lookup"><span data-stu-id="a0bb9-175">Step 3</span></span>

<span data-ttu-id="a0bb9-176">Kliknutím na Uložit o uložení plánu k sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="a0bb9-176">Click Save to save the schedule to the runbook.</span></span>

![Krok 10][10]

## <a name="next-steps"></a><span data-ttu-id="a0bb9-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0bb9-178">Next steps</span></span>

<span data-ttu-id="a0bb9-179">Nyní když máte představu o tom, jak integrovat řešení potíží s sledovací proces sítě s Azure Automation, zjistěte, jak aktivovat zachycení paketů na výstrahy virtuálních počítačů tím, navštivte stránky [vytvořit zaznamenání výstrahy spouštěná paketů s sledovací proces sítě Azure](network-watcher-alert-triggered-packet-capture.md).</span><span class="sxs-lookup"><span data-stu-id="a0bb9-179">Now that you have an understanding on how to integrate Network Watcher troubleshooting with Azure Automation, learn how to trigger packet captures on VM alerts by visiting [Create an alert triggered packet capture with Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).</span></span>

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
