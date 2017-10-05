---
title: "Pomocí přesměrování v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak konfigurovat a používat přesměrování v Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 2c8c867f-4907-4f2e-9ccd-2eb82bb5b837
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: b5a65d129225fde46e3b090bc3cd9427989005ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a><span data-ttu-id="2c01b-103">Pomocí přesměrování v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="2c01b-103">Using redirection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2c01b-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="2c01b-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="2c01b-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="2c01b-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="2c01b-106">Přesměrování zařízení umožňuje uživatelům interakci s vzdálené aplikace pomocí zařízení připojených k jejich místním počítači, telefon nebo tablet.</span><span class="sxs-lookup"><span data-stu-id="2c01b-106">Device redirection lets your users interact with remote apps using the devices attached to their local computer, phone, or tablet.</span></span> <span data-ttu-id="2c01b-107">Například pokud jste zadali Skype pomocí Azure Remoteappu, uživatel musí fotoaparát nainstalovaný na počítači pro práci s Skype.</span><span class="sxs-lookup"><span data-stu-id="2c01b-107">For example, if you have provided Skype through Azure RemoteApp, your user needs the camera installed on their PC to work with Skype.</span></span> <span data-ttu-id="2c01b-108">To platí také pro tiskárny, řečníci, monitorování a periferních zařízení připojená k rozsahu portu USB.</span><span class="sxs-lookup"><span data-stu-id="2c01b-108">This is also true for printers, speakers, monitors, and a range of USB-connected peripherals.</span></span>

<span data-ttu-id="2c01b-109">Vzdálené aplikace RemoteApp využívá protokol RDP (Remote Desktop) a RemoteFX zadejte přesměrování.</span><span class="sxs-lookup"><span data-stu-id="2c01b-109">RemoteApp leverages the Remote Desktop Protocol (RDP) and RemoteFX to provide redirection.</span></span>

## <a name="what-redirection-is-enabled-by-default"></a><span data-ttu-id="2c01b-110">Ve výchozím nastavení je povoleno jaké přesměrování?</span><span class="sxs-lookup"><span data-stu-id="2c01b-110">What redirection is enabled by default?</span></span>
<span data-ttu-id="2c01b-111">Když používáte vzdálené aplikace RemoteApp, jsou ve výchozím nastavení povolené následující přesměrování.</span><span class="sxs-lookup"><span data-stu-id="2c01b-111">When you use RemoteApp, the following redirections are enabled by default.</span></span> <span data-ttu-id="2c01b-112">Informace v závorkách zobrazit nastavení protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="2c01b-112">The information in parentheses show the RDP setting.</span></span>

* <span data-ttu-id="2c01b-113">Přehrávání zvuků v místním počítači (**hrát na tomto počítači**).</span><span class="sxs-lookup"><span data-stu-id="2c01b-113">Play sounds on the local computer (**Play on this computer**).</span></span> <span data-ttu-id="2c01b-114">(audiomode:i:0)</span><span class="sxs-lookup"><span data-stu-id="2c01b-114">(audiomode:i:0)</span></span>
* <span data-ttu-id="2c01b-115">Zaznamenání zvuku z místního počítače a odeslat do vzdáleného počítače (**záznamů z tohoto počítače**).</span><span class="sxs-lookup"><span data-stu-id="2c01b-115">Capture audio from the local computer and send to the remote computer (**Record from this computer**).</span></span> <span data-ttu-id="2c01b-116">(audiocapturemode:i:1)</span><span class="sxs-lookup"><span data-stu-id="2c01b-116">(audiocapturemode:i:1)</span></span>
* <span data-ttu-id="2c01b-117">Používat místní tiskárny (redirectprinters:i:1)</span><span class="sxs-lookup"><span data-stu-id="2c01b-117">Print to local printers (redirectprinters:i:1)</span></span>
* <span data-ttu-id="2c01b-118">Porty COM (redirectcomports:i:1)</span><span class="sxs-lookup"><span data-stu-id="2c01b-118">COM ports (redirectcomports:i:1)</span></span>
* <span data-ttu-id="2c01b-119">Zařízení čipovou kartou (redirectsmartcards:i:1)</span><span class="sxs-lookup"><span data-stu-id="2c01b-119">Smart card device (redirectsmartcards:i:1)</span></span>
* <span data-ttu-id="2c01b-120">Schránka (možnost zkopírujte a vložte) (redirectclipboard:i:1)</span><span class="sxs-lookup"><span data-stu-id="2c01b-120">Clipboard (ability to copy and paste) (redirectclipboard:i:1)</span></span>
* <span data-ttu-id="2c01b-121">Vymazat typ písma vyhlazování (povolit vyhlazení písma: i:1)</span><span class="sxs-lookup"><span data-stu-id="2c01b-121">Clear type font smoothing (allow font smoothing:i:1)</span></span>
* <span data-ttu-id="2c01b-122">Přesměrování všechna podporovaná zařízení Plug and Play.</span><span class="sxs-lookup"><span data-stu-id="2c01b-122">Redirect all supported Plug and Play devices.</span></span> <span data-ttu-id="2c01b-123">(devicestoredirect:s: *)</span><span class="sxs-lookup"><span data-stu-id="2c01b-123">(devicestoredirect:s:*)</span></span>

## <a name="what-other-redirection-is-available"></a><span data-ttu-id="2c01b-124">Jaké další přesměrování je k dispozici?</span><span class="sxs-lookup"><span data-stu-id="2c01b-124">What other redirection is available?</span></span>
<span data-ttu-id="2c01b-125">Ve výchozím nastavení jsou zakázány dvě možnosti přesměrování:</span><span class="sxs-lookup"><span data-stu-id="2c01b-125">Two redirection options are disabled by default:</span></span>

* <span data-ttu-id="2c01b-126">Přesměrování jednotky (mapování jednotky): stát mapované jednotky v této relaci vzdálené jednotky místního počítače.</span><span class="sxs-lookup"><span data-stu-id="2c01b-126">Drive redirection (drive mapping): Your local computer's drives become mapped drives in the remote session.</span></span> <span data-ttu-id="2c01b-127">Díky tomu můžete uložit nebo otevřít soubory z místní jednotky během práce ve vzdálené relaci.</span><span class="sxs-lookup"><span data-stu-id="2c01b-127">This lets you save or open files from your local drives while you work in the remote session.</span></span>
* <span data-ttu-id="2c01b-128">Přesměrování USB: můžete použít zařízení USB připojených do místního počítače v této relaci vzdálené.</span><span class="sxs-lookup"><span data-stu-id="2c01b-128">USB redirection: You can use the USB devices attached to your local computer within the remote session.</span></span>

## <a name="change-your-redirection-settings-in-remoteapp"></a><span data-ttu-id="2c01b-129">Změňte nastavení přesměrování v Remoteappu</span><span class="sxs-lookup"><span data-stu-id="2c01b-129">Change your redirection settings in RemoteApp</span></span>
<span data-ttu-id="2c01b-130">Nastavení přesměrování zařízení pro kolekci můžete změnit pomocí Microsoft Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="2c01b-130">You can change the device redirection settings for a collection by using the Microsoft Azure PowerShell with SDK.</span></span> <span data-ttu-id="2c01b-131">Po instalaci nové prostředí PowerShell a sady SDK se ho napřed nakonfigurovat ke správě svého předplatného, jak je popsáno v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2c01b-131">After you install the new PowerShell and SDK, first configure it to manage your subscription as described in [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="2c01b-132">Nastavit vlastní vlastnosti protokolu RDP pak použijte příkaz podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="2c01b-132">Then use a command similar to the following to set the custom RDP properties:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="2c01b-133">(Všimněte si, že  *\`n*  se používá jako oddělovač mezi jednotlivé vlastnosti.)</span><span class="sxs-lookup"><span data-stu-id="2c01b-133">(Note that *\`n* is used as a delimiter between individual properties.)</span></span>

<span data-ttu-id="2c01b-134">Chcete-li získat seznam, které jsou nakonfigurované jaké vlastní vlastnosti protokolu RDP, spusťte následující rutinu.</span><span class="sxs-lookup"><span data-stu-id="2c01b-134">To get a list of what custom RDP properties are configured, run the following cmdlet.</span></span> <span data-ttu-id="2c01b-135">Všimněte si, jestli se zobrazují pouze vlastní vlastnosti jako výstup výsledků a ne výchozí vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="2c01b-135">Note that only custom properties are shown as output results and not the default properties:</span></span>  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

<span data-ttu-id="2c01b-136">Pokud nastavíte vlastní vlastnosti je třeba zadat všechny vlastní vlastnosti pokaždé, když; v opačném případě vrátí nastavení na zakázáno.</span><span class="sxs-lookup"><span data-stu-id="2c01b-136">When you set custom properties you must specify all custom properties each time; otherwise the setting reverts to disabled.</span></span>   

### <a name="common-examples"></a><span data-ttu-id="2c01b-137">Běžné příklady</span><span class="sxs-lookup"><span data-stu-id="2c01b-137">Common examples</span></span>
<span data-ttu-id="2c01b-138">Chcete-li povolit přesměrování jednotky použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2c01b-138">Use the following cmdlet to enable drive redirection:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

<span data-ttu-id="2c01b-139">Tuto rutinu použijte k povolení USB a jednotku přesměrování:</span><span class="sxs-lookup"><span data-stu-id="2c01b-139">Use this cmdlet to enable both USB and Drive redirection:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="2c01b-140">Chcete-li zakázat sdílení schránky, použijte tuto rutinu:</span><span class="sxs-lookup"><span data-stu-id="2c01b-140">Use this cmdlet to disable clipboard sharing:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> <span data-ttu-id="2c01b-141">Nezapomeňte úplně Odhlásit všechny uživatele v kolekci (a ne jenom odpojte je) před testovacího změnu.</span><span class="sxs-lookup"><span data-stu-id="2c01b-141">Be sure to completely log off all users in the collection (and not just disconnect them) before you test the change.</span></span> <span data-ttu-id="2c01b-142">Aby se uživatelé jsou zcela odhlášením, přejděte na **relací** v kolekci v portálu Azure a odhlásit všechny uživatele, kterým se odpojí nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2c01b-142">To ensure users are completely logged off, go to the **Sessions** tab in the collection in the Azure portal and log off any users who are disconnected or signed in.</span></span> <span data-ttu-id="2c01b-143">Někdy může trvat několik sekund místní jednotky, které se zobrazí v Průzkumníku v rámci relace.</span><span class="sxs-lookup"><span data-stu-id="2c01b-143">Sometimes it can take several seconds for the local drives to show in Explorer within the session.</span></span>
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a><span data-ttu-id="2c01b-144">Změňte nastavení přesměrování USB na vašeho klienta Windows</span><span class="sxs-lookup"><span data-stu-id="2c01b-144">Change USB redirection settings on your Windows client</span></span>
<span data-ttu-id="2c01b-145">Pokud chcete používat přesměrování USB v počítači, který se připojuje k vzdálené aplikace RemoteApp, existují 2 akce, které je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="2c01b-145">If you want to use USB redirection on a computer that connects to RemoteApp, there are 2 actions that need to happen.</span></span> <span data-ttu-id="2c01b-146">1 - Správce musí povolit přesměrování USB na úrovni kolekce pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c01b-146">1 - Your administrator needs to enable USB redirection at the collection level by using Azure PowerShell.</span></span> <span data-ttu-id="2c01b-147">2 – u jednotlivých zařízení, ve které chcete používat přesměrování USB budete muset povolit zásady skupiny, které povolí ho.</span><span class="sxs-lookup"><span data-stu-id="2c01b-147">2 - On each device where you want to use USB redirection, you need to enable a group policy that permits it.</span></span> <span data-ttu-id="2c01b-148">Tento krok bude muset být provedeno pro každý uživatel, který chce používat přesměrování USB.</span><span class="sxs-lookup"><span data-stu-id="2c01b-148">This step will need to be done for each user that wants to use USB redirection.</span></span>

> [!NOTE]
> <span data-ttu-id="2c01b-149">Přesměrování USB s Azure Remoteappem je podporována pouze pro počítače se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="2c01b-149">USB redirection with Azure RemoteApp is only supported for Windows computers.</span></span>
> 
> 

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a><span data-ttu-id="2c01b-150">Povolit přesměrování USB pro kolekci vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="2c01b-150">Enable USB redirection for the RemoteApp collection</span></span>
<span data-ttu-id="2c01b-151">Chcete povolit přesměrování USB na úrovni kolekce použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="2c01b-151">Use the following cmdlet to enable USB redirection at the collection level:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a><span data-ttu-id="2c01b-152">Povolit přesměrování USB pro klientský počítač</span><span class="sxs-lookup"><span data-stu-id="2c01b-152">Enable USB redirection for the client computer</span></span>
<span data-ttu-id="2c01b-153">Konfigurace nastavení přesměrování USB v počítači:</span><span class="sxs-lookup"><span data-stu-id="2c01b-153">To configure USB redirection settings on your computer:</span></span>

1. <span data-ttu-id="2c01b-154">Otevřete Editor zásad skupiny místní (GPEDIT. MSC).</span><span class="sxs-lookup"><span data-stu-id="2c01b-154">Open the Local Group Policy Editor (GPEDIT.MSC).</span></span> <span data-ttu-id="2c01b-155">(Spuštění gpedit.msc z příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="2c01b-155">(Run gpedit.msc from a command prompt.)</span></span>
2. <span data-ttu-id="2c01b-156">Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-156">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
3. <span data-ttu-id="2c01b-157">Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-157">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
4. <span data-ttu-id="2c01b-158">Vyberte **povoleno**a potom vyberte **správců a uživatelů v přístupová práva přesměrování USB RemoteFX**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-158">Select **Enabled**, and then select **Administrators and Users in the RemoteFX USB Redirection Access Rights**.</span></span>
5. <span data-ttu-id="2c01b-159">Otevřete příkazový řádek s oprávněním správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2c01b-159">Open a command prompt with administrative permissions, and run the following command:</span></span>
   
        gpupdate /force
6. <span data-ttu-id="2c01b-160">Restartujte počítač.</span><span class="sxs-lookup"><span data-stu-id="2c01b-160">Restart the computer.</span></span>

<span data-ttu-id="2c01b-161">Můžete taky nástroje pro správu zásad skupiny k vytvoření a použití zásad přesměrování USB pro všechny počítače ve vaší doméně:</span><span class="sxs-lookup"><span data-stu-id="2c01b-161">You can also use the Group Policy Management tool to create and apply the USB redirection policy for all computers in your domain:</span></span>

1. <span data-ttu-id="2c01b-162">Přihlaste se k řadiči domény jako správce domény.</span><span class="sxs-lookup"><span data-stu-id="2c01b-162">Log into the domain controller as the domain administrator.</span></span>
2. <span data-ttu-id="2c01b-163">Otevřete konzolu pro správu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="2c01b-163">Open the Group Policy Management Console.</span></span> <span data-ttu-id="2c01b-164">(Klikněte na tlačítko **Start > Nástroje pro správu > Správa zásad skupiny**.)</span><span class="sxs-lookup"><span data-stu-id="2c01b-164">(Click **Start > Administrative Tools > Group Policy Management**.)</span></span>
3. <span data-ttu-id="2c01b-165">Přejděte na doménu nebo organizační jednotku, pro který chcete vytvořit zásadu.</span><span class="sxs-lookup"><span data-stu-id="2c01b-165">Navigate to the domain or organizational unit for which you want to create the policy.</span></span>
4. <span data-ttu-id="2c01b-166">Klikněte pravým tlačítkem na **výchozí zásady domény**a potom klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-166">Right-click **Default Domain Policy**, and then click **Edit**.</span></span>
5. <span data-ttu-id="2c01b-167">Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-167">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
6. <span data-ttu-id="2c01b-168">Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-168">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
7. <span data-ttu-id="2c01b-169">Vyberte **povoleno**a potom vyberte **správců a uživatelů v přístupová práva přesměrování USB RemoteFX**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-169">Select **Enabled**, and then select **Administrators and Users in the RemoteFX USB Redirection Access Rights**.</span></span>
8. <span data-ttu-id="2c01b-170">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c01b-170">Click **OK**.</span></span>  

