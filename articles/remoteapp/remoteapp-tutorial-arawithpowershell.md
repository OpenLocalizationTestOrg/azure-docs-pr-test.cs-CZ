---
title: "Pomocí rutin prostředí PowerShell s Azure Remoteappem | Microsoft Docs"
description: "Další informace o použití rutin prostředí Windows PowerShell v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 699b20a4dadd4ecaff57e2cc80355fe545360663
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a><span data-ttu-id="004b6-103">Pomocí rutin prostředí Windows PowerShell s Azure Remoteappem</span><span class="sxs-lookup"><span data-stu-id="004b6-103">Use Windows PowerShell cmdlets with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="004b6-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="004b6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="004b6-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="004b6-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

 <span data-ttu-id="004b6-106">Rutiny prostředí Azure RemoteApp PowerShell můžete použít pro správu a údržbu kolekce.</span><span class="sxs-lookup"><span data-stu-id="004b6-106">You can use the Azure RemoteApp PowerShell cmdlets to administer and maintain your collections.</span></span> <span data-ttu-id="004b6-107">Abyste mohli začít, použijte následující informace.</span><span class="sxs-lookup"><span data-stu-id="004b6-107">Use the following information to get started.</span></span>

## <a name="get-the-cmdlets"></a><span data-ttu-id="004b6-108">Získat rutiny</span><span class="sxs-lookup"><span data-stu-id="004b6-108">Get the cmdlets</span></span>
- - -
<span data-ttu-id="004b6-109">Nejdřív stáhnout rutin prostředí Azure Powershell [zde](http://go.microsoft.com/?linkid=9811175), rutiny vzdálené aplikace RemoteApp jsou zahrnuty v ní.</span><span class="sxs-lookup"><span data-stu-id="004b6-109">First download the Azure Powershell cmdlets [here](http://go.microsoft.com/?linkid=9811175), the RemoteApp cmdlets are included in it.</span></span> 

<span data-ttu-id="004b6-110">Podívejte se [nápovědu rutiny Azure Remoteappu](/powershell/module/azure?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="004b6-110">Check out the [Azure RemoteApp cmdlet help](/powershell/module/azure?view=azuresmps-3.7.0).</span></span>

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a><span data-ttu-id="004b6-111">Konfigurace rutiny na používání vašeho předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="004b6-111">Configure Azure cmdlets to use your subscription</span></span>
- - -
<span data-ttu-id="004b6-112">Postupujte podle [Tato příručka](/powershell/azure/overview) abyste je mohli používat rutiny pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="004b6-112">Follow [this guide](/powershell/azure/overview) so you can use the cmdlets against your Azure subscription.</span></span>

<span data-ttu-id="004b6-113">Abyste mohli rychle začít, můžete použít tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="004b6-113">You can use these steps to get started quickly:</span></span>

1. <span data-ttu-id="004b6-114">Stáhněte a nainstalujte [rutin prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175).</span><span class="sxs-lookup"><span data-stu-id="004b6-114">Download and install the [Azure PowerShell cmdlets](http://go.microsoft.com/?linkid=9811175).</span></span>
2. <span data-ttu-id="004b6-115">Spusťte Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="004b6-115">Launch Microsoft Azure PowerShell.</span></span>
3. <span data-ttu-id="004b6-116">Spustit **Add-AzureAccount** k ověření vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="004b6-116">Run **Add-AzureAccount** to authenticate to your Azure subscription.</span></span> <span data-ttu-id="004b6-117">Po zobrazení výzvy zadejte stejné uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="004b6-117">When prompted, enter the same user name and password that you use to sign in to Azure portal.</span></span>  
4. <span data-ttu-id="004b6-118">Spustit **Get-AzureSubscription** seznam předplatná spojená s vaším uživatelským účtem.</span><span class="sxs-lookup"><span data-stu-id="004b6-118">Run **Get-AzureSubscription** to list the subscriptions associated with your user account.</span></span> 
5. <span data-ttu-id="004b6-119">Spustit **Select-AzureSubscription - Název_předplatného &lt;název odběru&gt;**  nebo **Select-AzureSubscription - SubscriptionId &lt;ID předplatného&gt;**  pro určení předplatného, používat.</span><span class="sxs-lookup"><span data-stu-id="004b6-119">Run **Select-AzureSubscription -SubscriptionName &lt;subscription name&gt;** or **Select-AzureSubscription -SubscriptionId &lt;subscription ID&gt;** to specify the subscription to use.</span></span>

<span data-ttu-id="004b6-120">Blahopřejeme, konzolu prostředí Azure PowerShell je konfigurována a připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="004b6-120">Congratulations, your Azure PowerShell console is configured and ready to use.</span></span> <span data-ttu-id="004b6-121">Uvědomte si, že budete muset repeate kroky 2 až 5 při každém spuštění konzoly Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="004b6-121">Be aware that you'll need to repeate steps 2 through 5 each time you start the the Azure PowerShell console.</span></span>  


## <a name="list-all-collections"></a><span data-ttu-id="004b6-122">Zobrazí seznam všech kolekcí</span><span class="sxs-lookup"><span data-stu-id="004b6-122">List all collections</span></span>
- - -
     <span data-ttu-id="004b6-123">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="004b6-123">Get-AzureRemoteAppCollection</span></span>

## <a name="delete-a-collection"></a><span data-ttu-id="004b6-124">Odstranit kolekci</span><span class="sxs-lookup"><span data-stu-id="004b6-124">Delete a collection</span></span>
- - -
    <span data-ttu-id="004b6-125">Odebrat AzureRemoteAppCollection<enter collection name></span><span class="sxs-lookup"><span data-stu-id="004b6-125">Remove-AzureRemoteAppCollection <enter collection name></span></span>

<span data-ttu-id="004b6-126">Příklad: `Remove-AzureRemoteAppCollection ContosoProduction`.</span><span class="sxs-lookup"><span data-stu-id="004b6-126">Example:  `Remove-AzureRemoteAppCollection ContosoProduction`.</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="004b6-127">Vytvoření cloudové kolekce</span><span class="sxs-lookup"><span data-stu-id="004b6-127">Create a cloud collection</span></span>
- - -
<span data-ttu-id="004b6-128">Je jednoduchý, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="004b6-128">It's simple, run the following command:</span></span>

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

<span data-ttu-id="004b6-129">Výše uvedený příkaz automaticky publikuje aplikace Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, aplikace Visio a Word).</span><span class="sxs-lookup"><span data-stu-id="004b6-129">The above command automatically publishes Microsoft Office 365 applications (Excel, OneNote, Outlook, PowerPoint, Visio and Word).</span></span>

<span data-ttu-id="004b6-130">Vytvoření kolekce může trvat 30 minut nebo déle.</span><span class="sxs-lookup"><span data-stu-id="004b6-130">Collection creation can take 30 minutes or longer to complete.</span></span> <span data-ttu-id="004b6-131">Proto tento příkaz vrátí ID pro sledování, který můžete použít takto:</span><span class="sxs-lookup"><span data-stu-id="004b6-131">Therefore, this command returns a tracking ID that you can use as follows:</span></span>

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

<span data-ttu-id="004b6-132">Po dokončení kolekce, můžete přidat uživatele do kolekce pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="004b6-132">After the collection is done, you can add users to the collection with the following command:</span></span>

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

<span data-ttu-id="004b6-133">A jste hotovi!</span><span class="sxs-lookup"><span data-stu-id="004b6-133">And you're done!</span></span> <span data-ttu-id="004b6-134">Tento uživatel musí být schopný se připojit k aplikaci pomocí Azure RemoteApp klienta najít [zde](https://www.remoteapp.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="004b6-134">That user should be able to connect to the application using the Azure RemoteApp client found [here](https://www.remoteapp.windowsazure.com/).</span></span>

## <a name="available-cmdlets"></a><span data-ttu-id="004b6-135">Dostupné rutiny</span><span class="sxs-lookup"><span data-stu-id="004b6-135">Available cmdlets</span></span>
<span data-ttu-id="004b6-136">Obsahuje velké množství jinými příkazy, které máme, dokumentace pro ně budou přicházet krátce:</span><span class="sxs-lookup"><span data-stu-id="004b6-136">There are lots of other commands that we have, the documentation for them will be coming shortly:</span></span>

<span data-ttu-id="004b6-137">Základní rutiny kolekce vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="004b6-137">Basic RemoteApp Collection cmdlets:</span></span> 

* <span data-ttu-id="004b6-138">New-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="004b6-138">New-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="004b6-139">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="004b6-139">Get-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="004b6-140">Set-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="004b6-140">Set-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="004b6-141">Update-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="004b6-141">Update-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="004b6-142">Remove-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="004b6-142">Remove-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="004b6-143">Add-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="004b6-143">Add-AzureRemoteAppUser</span></span>
* <span data-ttu-id="004b6-144">Get-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="004b6-144">Get-AzureRemoteAppUser</span></span>
* <span data-ttu-id="004b6-145">Remove-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="004b6-145">Remove-AzureRemoteAppUser</span></span>
* <span data-ttu-id="004b6-146">Get-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="004b6-146">Get-AzureRemoteAppSession</span></span>
* <span data-ttu-id="004b6-147">Disconnect-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="004b6-147">Disconnect-AzureRemoteAppSession</span></span>
* <span data-ttu-id="004b6-148">Invoke-AzureRemoteAppSessionLogoff</span><span class="sxs-lookup"><span data-stu-id="004b6-148">Invoke-AzureRemoteAppSessionLogoff</span></span>
* <span data-ttu-id="004b6-149">Send-AzureRemoteAppSessionMessage</span><span class="sxs-lookup"><span data-stu-id="004b6-149">Send-AzureRemoteAppSessionMessage</span></span>
* <span data-ttu-id="004b6-150">Get-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="004b6-150">Get-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="004b6-151">Get-AzureRemoteAppStartMenuProgram</span><span class="sxs-lookup"><span data-stu-id="004b6-151">Get-AzureRemoteAppStartMenuProgram</span></span>
* <span data-ttu-id="004b6-152">Publish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="004b6-152">Publish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="004b6-153">Unpublish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="004b6-153">Unpublish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="004b6-154">Get-AzureRemoteAppCollectionUsageDetails</span><span class="sxs-lookup"><span data-stu-id="004b6-154">Get-AzureRemoteAppCollectionUsageDetails</span></span>
* <span data-ttu-id="004b6-155">Get-AzureRemoteAppCollectionUsageSummary</span><span class="sxs-lookup"><span data-stu-id="004b6-155">Get-AzureRemoteAppCollectionUsageSummary</span></span>
* <span data-ttu-id="004b6-156">Get-AzureRemoteAppPlan</span><span class="sxs-lookup"><span data-stu-id="004b6-156">Get-AzureRemoteAppPlan</span></span>

<span data-ttu-id="004b6-157">Rutiny virtuální sítě vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="004b6-157">RemoteApp virtual network cmdlets:</span></span>

* <span data-ttu-id="004b6-158">New-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="004b6-158">New-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="004b6-159">Get-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="004b6-159">Get-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="004b6-160">Set-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="004b6-160">Set-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="004b6-161">Remove-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="004b6-161">Remove-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="004b6-162">Get-AzureRemoteAppVpnDevice</span><span class="sxs-lookup"><span data-stu-id="004b6-162">Get-AzureRemoteAppVpnDevice</span></span>
* <span data-ttu-id="004b6-163">Get – AzureRemoteAppVpnDeviceConfigScript</span><span class="sxs-lookup"><span data-stu-id="004b6-163">Get-- AzureRemoteAppVpnDeviceConfigScript</span></span>
* <span data-ttu-id="004b6-164">Reset-AzureRemoteAppVpnSharedKey</span><span class="sxs-lookup"><span data-stu-id="004b6-164">Reset-AzureRemoteAppVpnSharedKey</span></span>

<span data-ttu-id="004b6-165">Rutiny image šablony vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="004b6-165">RemoteApp template image cmdlets:</span></span>

* <span data-ttu-id="004b6-166">New-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="004b6-166">New-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="004b6-167">Get-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="004b6-167">Get-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="004b6-168">Rename-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="004b6-168">Rename-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="004b6-169">Remove-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="004b6-169">Remove-AzureRemoteAppTemplateImage</span></span>

<span data-ttu-id="004b6-170">Ostatní rutiny vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="004b6-170">Other RemoteApp cmdlets:</span></span>

* <span data-ttu-id="004b6-171">Get-AzureRemoteAppLocation</span><span class="sxs-lookup"><span data-stu-id="004b6-171">Get-AzureRemoteAppLocation</span></span>
* <span data-ttu-id="004b6-172">Get-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="004b6-172">Get-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="004b6-173">Set-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="004b6-173">Set-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="004b6-174">Get-AzureRemoteAppOperationResult</span><span class="sxs-lookup"><span data-stu-id="004b6-174">Get-AzureRemoteAppOperationResult</span></span>

