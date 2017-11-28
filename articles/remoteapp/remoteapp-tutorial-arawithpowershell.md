---
title: "aaaUse rutiny prostředí PowerShell s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak toouse rutiny prostředí Windows PowerShell v Azure Remoteappu."
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
ms.openlocfilehash: a09cae2093e2c3a4a2ed673b5d148a22ceb935f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a><span data-ttu-id="02248-103">Pomocí rutin prostředí Windows PowerShell s Azure Remoteappem</span><span class="sxs-lookup"><span data-stu-id="02248-103">Use Windows PowerShell cmdlets with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="02248-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="02248-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="02248-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="02248-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

 <span data-ttu-id="02248-106">Můžete použít tooadminister rutin prostředí Azure RemoteApp PowerShell hello a spravovat kolekce.</span><span class="sxs-lookup"><span data-stu-id="02248-106">You can use hello Azure RemoteApp PowerShell cmdlets tooadminister and maintain your collections.</span></span> <span data-ttu-id="02248-107">Použijte následující informace tooget spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="02248-107">Use hello following information tooget started.</span></span>

## <a name="get-hello-cmdlets"></a><span data-ttu-id="02248-108">Získat rutiny hello</span><span class="sxs-lookup"><span data-stu-id="02248-108">Get hello cmdlets</span></span>
- - -
<span data-ttu-id="02248-109">Nejdřív stáhnout rutin prostředí Azure Powershell hello [zde](http://go.microsoft.com/?linkid=9811175), rutiny hello vzdálené aplikace RemoteApp jsou zahrnuty v ní.</span><span class="sxs-lookup"><span data-stu-id="02248-109">First download hello Azure Powershell cmdlets [here](http://go.microsoft.com/?linkid=9811175), hello RemoteApp cmdlets are included in it.</span></span> 

<span data-ttu-id="02248-110">Podívejte se na hello [nápovědu rutiny Azure Remoteappu](/powershell/module/azure?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="02248-110">Check out hello [Azure RemoteApp cmdlet help](/powershell/module/azure?view=azuresmps-3.7.0).</span></span>

## <a name="configure-azure-cmdlets-toouse-your-subscription"></a><span data-ttu-id="02248-111">Konfigurovat rutiny Azure toouse předplatné</span><span class="sxs-lookup"><span data-stu-id="02248-111">Configure Azure cmdlets toouse your subscription</span></span>
- - -
<span data-ttu-id="02248-112">Postupujte podle [Tato příručka](/powershell/azure/overview) abyste je mohli používat rutiny hello u vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="02248-112">Follow [this guide](/powershell/azure/overview) so you can use hello cmdlets against your Azure subscription.</span></span>

<span data-ttu-id="02248-113">Můžete provádět tyto kroky tooget rychle začít:</span><span class="sxs-lookup"><span data-stu-id="02248-113">You can use these steps tooget started quickly:</span></span>

1. <span data-ttu-id="02248-114">Stáhněte a nainstalujte hello [rutin prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175).</span><span class="sxs-lookup"><span data-stu-id="02248-114">Download and install hello [Azure PowerShell cmdlets](http://go.microsoft.com/?linkid=9811175).</span></span>
2. <span data-ttu-id="02248-115">Spusťte Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="02248-115">Launch Microsoft Azure PowerShell.</span></span>
3. <span data-ttu-id="02248-116">Spustit **Add-AzureAccount** tooauthenticate tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="02248-116">Run **Add-AzureAccount** tooauthenticate tooyour Azure subscription.</span></span> <span data-ttu-id="02248-117">Po zobrazení výzvy zadejte hello stejné uživatelské jméno a heslo používané toosign tooAzure portálu.</span><span class="sxs-lookup"><span data-stu-id="02248-117">When prompted, enter hello same user name and password that you use toosign in tooAzure portal.</span></span>  
4. <span data-ttu-id="02248-118">Spustit **Get-AzureSubscription** toolist hello předplatná spojená s vaším uživatelským účtem.</span><span class="sxs-lookup"><span data-stu-id="02248-118">Run **Get-AzureSubscription** toolist hello subscriptions associated with your user account.</span></span> 
5. <span data-ttu-id="02248-119">Spustit **Select-AzureSubscription - Název_předplatného &lt;název odběru&gt;**  nebo **Select-AzureSubscription - SubscriptionId &lt;ID předplatného&gt;**  toospecify hello předplatné toouse.</span><span class="sxs-lookup"><span data-stu-id="02248-119">Run **Select-AzureSubscription -SubscriptionName &lt;subscription name&gt;** or **Select-AzureSubscription -SubscriptionId &lt;subscription ID&gt;** toospecify hello subscription toouse.</span></span>

<span data-ttu-id="02248-120">Blahopřejeme, je konfigurována a připravena toouse konzola prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="02248-120">Congratulations, your Azure PowerShell console is configured and ready toouse.</span></span> <span data-ttu-id="02248-121">Mějte na paměti, že potřebujete toorepeate kroky 2 až 5 při každém spuštění konzoly Azure PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="02248-121">Be aware that you'll need toorepeate steps 2 through 5 each time you start hello hello Azure PowerShell console.</span></span>  


## <a name="list-all-collections"></a><span data-ttu-id="02248-122">Zobrazí seznam všech kolekcí</span><span class="sxs-lookup"><span data-stu-id="02248-122">List all collections</span></span>
- - -
     <span data-ttu-id="02248-123">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="02248-123">Get-AzureRemoteAppCollection</span></span>

## <a name="delete-a-collection"></a><span data-ttu-id="02248-124">Odstranit kolekci</span><span class="sxs-lookup"><span data-stu-id="02248-124">Delete a collection</span></span>
- - -
    <span data-ttu-id="02248-125">Odebrat AzureRemoteAppCollection<enter collection name></span><span class="sxs-lookup"><span data-stu-id="02248-125">Remove-AzureRemoteAppCollection <enter collection name></span></span>

<span data-ttu-id="02248-126">Příklad: `Remove-AzureRemoteAppCollection ContosoProduction`.</span><span class="sxs-lookup"><span data-stu-id="02248-126">Example:  `Remove-AzureRemoteAppCollection ContosoProduction`.</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="02248-127">Vytvoření cloudové kolekce</span><span class="sxs-lookup"><span data-stu-id="02248-127">Create a cloud collection</span></span>
- - -
<span data-ttu-id="02248-128">Je jednoduchý, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="02248-128">It's simple, run hello following command:</span></span>

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

<span data-ttu-id="02248-129">Hello výše příkaz automaticky publikuje aplikace Microsoft Office 365 (Excel, OneNote, Outlook, PowerPoint, aplikace Visio a Word).</span><span class="sxs-lookup"><span data-stu-id="02248-129">hello above command automatically publishes Microsoft Office 365 applications (Excel, OneNote, Outlook, PowerPoint, Visio and Word).</span></span>

<span data-ttu-id="02248-130">Vytvoření kolekce může trvat 30 minut nebo déle toocomplete.</span><span class="sxs-lookup"><span data-stu-id="02248-130">Collection creation can take 30 minutes or longer toocomplete.</span></span> <span data-ttu-id="02248-131">Proto tento příkaz vrátí ID pro sledování, který můžete použít takto:</span><span class="sxs-lookup"><span data-stu-id="02248-131">Therefore, this command returns a tracking ID that you can use as follows:</span></span>

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

<span data-ttu-id="02248-132">Po dokončení hello kolekce, můžete přidat uživatele toohello kolekce s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="02248-132">After hello collection is done, you can add users toohello collection with hello following command:</span></span>

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

<span data-ttu-id="02248-133">A jste hotovi!</span><span class="sxs-lookup"><span data-stu-id="02248-133">And you're done!</span></span> <span data-ttu-id="02248-134">Tento uživatel by měl být schopný tooconnect toohello aplikací pomocí klienta aplikace Azure RemoteApp hello najít [zde](https://www.remoteapp.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="02248-134">That user should be able tooconnect toohello application using hello Azure RemoteApp client found [here](https://www.remoteapp.windowsazure.com/).</span></span>

## <a name="available-cmdlets"></a><span data-ttu-id="02248-135">Dostupné rutiny</span><span class="sxs-lookup"><span data-stu-id="02248-135">Available cmdlets</span></span>
<span data-ttu-id="02248-136">Obsahuje velké množství jinými příkazy, které máme, hello dokumentace pro ně budou přicházet krátce:</span><span class="sxs-lookup"><span data-stu-id="02248-136">There are lots of other commands that we have, hello documentation for them will be coming shortly:</span></span>

<span data-ttu-id="02248-137">Základní rutiny kolekce vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="02248-137">Basic RemoteApp Collection cmdlets:</span></span> 

* <span data-ttu-id="02248-138">New-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="02248-138">New-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="02248-139">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="02248-139">Get-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="02248-140">Set-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="02248-140">Set-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="02248-141">Update-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="02248-141">Update-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="02248-142">Remove-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="02248-142">Remove-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="02248-143">Add-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="02248-143">Add-AzureRemoteAppUser</span></span>
* <span data-ttu-id="02248-144">Get-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="02248-144">Get-AzureRemoteAppUser</span></span>
* <span data-ttu-id="02248-145">Remove-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="02248-145">Remove-AzureRemoteAppUser</span></span>
* <span data-ttu-id="02248-146">Get-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="02248-146">Get-AzureRemoteAppSession</span></span>
* <span data-ttu-id="02248-147">Disconnect-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="02248-147">Disconnect-AzureRemoteAppSession</span></span>
* <span data-ttu-id="02248-148">Invoke-AzureRemoteAppSessionLogoff</span><span class="sxs-lookup"><span data-stu-id="02248-148">Invoke-AzureRemoteAppSessionLogoff</span></span>
* <span data-ttu-id="02248-149">Send-AzureRemoteAppSessionMessage</span><span class="sxs-lookup"><span data-stu-id="02248-149">Send-AzureRemoteAppSessionMessage</span></span>
* <span data-ttu-id="02248-150">Get-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="02248-150">Get-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="02248-151">Get-AzureRemoteAppStartMenuProgram</span><span class="sxs-lookup"><span data-stu-id="02248-151">Get-AzureRemoteAppStartMenuProgram</span></span>
* <span data-ttu-id="02248-152">Publish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="02248-152">Publish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="02248-153">Unpublish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="02248-153">Unpublish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="02248-154">Get-AzureRemoteAppCollectionUsageDetails</span><span class="sxs-lookup"><span data-stu-id="02248-154">Get-AzureRemoteAppCollectionUsageDetails</span></span>
* <span data-ttu-id="02248-155">Get-AzureRemoteAppCollectionUsageSummary</span><span class="sxs-lookup"><span data-stu-id="02248-155">Get-AzureRemoteAppCollectionUsageSummary</span></span>
* <span data-ttu-id="02248-156">Get-AzureRemoteAppPlan</span><span class="sxs-lookup"><span data-stu-id="02248-156">Get-AzureRemoteAppPlan</span></span>

<span data-ttu-id="02248-157">Rutiny virtuální sítě vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="02248-157">RemoteApp virtual network cmdlets:</span></span>

* <span data-ttu-id="02248-158">New-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="02248-158">New-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="02248-159">Get-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="02248-159">Get-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="02248-160">Set-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="02248-160">Set-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="02248-161">Remove-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="02248-161">Remove-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="02248-162">Get-AzureRemoteAppVpnDevice</span><span class="sxs-lookup"><span data-stu-id="02248-162">Get-AzureRemoteAppVpnDevice</span></span>
* <span data-ttu-id="02248-163">Get – AzureRemoteAppVpnDeviceConfigScript</span><span class="sxs-lookup"><span data-stu-id="02248-163">Get-- AzureRemoteAppVpnDeviceConfigScript</span></span>
* <span data-ttu-id="02248-164">Reset-AzureRemoteAppVpnSharedKey</span><span class="sxs-lookup"><span data-stu-id="02248-164">Reset-AzureRemoteAppVpnSharedKey</span></span>

<span data-ttu-id="02248-165">Rutiny image šablony vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="02248-165">RemoteApp template image cmdlets:</span></span>

* <span data-ttu-id="02248-166">New-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="02248-166">New-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="02248-167">Get-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="02248-167">Get-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="02248-168">Rename-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="02248-168">Rename-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="02248-169">Remove-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="02248-169">Remove-AzureRemoteAppTemplateImage</span></span>

<span data-ttu-id="02248-170">Ostatní rutiny vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="02248-170">Other RemoteApp cmdlets:</span></span>

* <span data-ttu-id="02248-171">Get-AzureRemoteAppLocation</span><span class="sxs-lookup"><span data-stu-id="02248-171">Get-AzureRemoteAppLocation</span></span>
* <span data-ttu-id="02248-172">Get-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="02248-172">Get-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="02248-173">Set-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="02248-173">Set-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="02248-174">Get-AzureRemoteAppOperationResult</span><span class="sxs-lookup"><span data-stu-id="02248-174">Get-AzureRemoteAppOperationResult</span></span>

