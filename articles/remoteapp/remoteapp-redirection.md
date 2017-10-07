---
title: "přesměrování aaaUsing v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure a používání přesměrování v Remoteappu"
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
ms.openlocfilehash: d5739a75cf606bd971268da67b2c5ff0fe5fe19b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a><span data-ttu-id="d1e19-103">Pomocí přesměrování v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="d1e19-103">Using redirection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d1e19-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="d1e19-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d1e19-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d1e19-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d1e19-106">Přesměrování zařízení umožňuje uživatelům interakci s vzdálené aplikace pomocí hello zařízení připojených tootheir místního počítače, telefon nebo tablet.</span><span class="sxs-lookup"><span data-stu-id="d1e19-106">Device redirection lets your users interact with remote apps using hello devices attached tootheir local computer, phone, or tablet.</span></span> <span data-ttu-id="d1e19-107">Například pokud jste zadali Skype pomocí Azure Remoteappu, uživatel musí hello fotoaparát nainstalovaný na jejich počítač toowork s Skype.</span><span class="sxs-lookup"><span data-stu-id="d1e19-107">For example, if you have provided Skype through Azure RemoteApp, your user needs hello camera installed on their PC toowork with Skype.</span></span> <span data-ttu-id="d1e19-108">To platí také pro tiskárny, řečníci, monitorování a periferních zařízení připojená k rozsahu portu USB.</span><span class="sxs-lookup"><span data-stu-id="d1e19-108">This is also true for printers, speakers, monitors, and a range of USB-connected peripherals.</span></span>

<span data-ttu-id="d1e19-109">Vzdálené aplikace RemoteApp využívá hello protokolu RDP (Remote Desktop) a RemoteFX tooprovide přesměrování.</span><span class="sxs-lookup"><span data-stu-id="d1e19-109">RemoteApp leverages hello Remote Desktop Protocol (RDP) and RemoteFX tooprovide redirection.</span></span>

## <a name="what-redirection-is-enabled-by-default"></a><span data-ttu-id="d1e19-110">Ve výchozím nastavení je povoleno jaké přesměrování?</span><span class="sxs-lookup"><span data-stu-id="d1e19-110">What redirection is enabled by default?</span></span>
<span data-ttu-id="d1e19-111">Když používáte vzdálené aplikace RemoteApp, jsou ve výchozím nastavení povolené hello následující přesměrování.</span><span class="sxs-lookup"><span data-stu-id="d1e19-111">When you use RemoteApp, hello following redirections are enabled by default.</span></span> <span data-ttu-id="d1e19-112">nastavení protokolu RDP hello zobrazovat informace Hello v závorkách.</span><span class="sxs-lookup"><span data-stu-id="d1e19-112">hello information in parentheses show hello RDP setting.</span></span>

* <span data-ttu-id="d1e19-113">Přehrávání zvuků na místním počítači hello (**hrát na tomto počítači**).</span><span class="sxs-lookup"><span data-stu-id="d1e19-113">Play sounds on hello local computer (**Play on this computer**).</span></span> <span data-ttu-id="d1e19-114">(audiomode:i:0)</span><span class="sxs-lookup"><span data-stu-id="d1e19-114">(audiomode:i:0)</span></span>
* <span data-ttu-id="d1e19-115">Zaznamenat zvuk z místního počítače hello a odeslání toohello vzdáleného počítače (**záznamů z tohoto počítače**).</span><span class="sxs-lookup"><span data-stu-id="d1e19-115">Capture audio from hello local computer and send toohello remote computer (**Record from this computer**).</span></span> <span data-ttu-id="d1e19-116">(audiocapturemode:i:1)</span><span class="sxs-lookup"><span data-stu-id="d1e19-116">(audiocapturemode:i:1)</span></span>
* <span data-ttu-id="d1e19-117">Tisk toolocal tiskárny (redirectprinters:i:1)</span><span class="sxs-lookup"><span data-stu-id="d1e19-117">Print toolocal printers (redirectprinters:i:1)</span></span>
* <span data-ttu-id="d1e19-118">Porty COM (redirectcomports:i:1)</span><span class="sxs-lookup"><span data-stu-id="d1e19-118">COM ports (redirectcomports:i:1)</span></span>
* <span data-ttu-id="d1e19-119">Zařízení čipovou kartou (redirectsmartcards:i:1)</span><span class="sxs-lookup"><span data-stu-id="d1e19-119">Smart card device (redirectsmartcards:i:1)</span></span>
* <span data-ttu-id="d1e19-120">Schránka (možnost toocopy a vložení) (redirectclipboard:i:1)</span><span class="sxs-lookup"><span data-stu-id="d1e19-120">Clipboard (ability toocopy and paste) (redirectclipboard:i:1)</span></span>
* <span data-ttu-id="d1e19-121">Vymazat typ písma vyhlazování (povolit vyhlazení písma: i:1)</span><span class="sxs-lookup"><span data-stu-id="d1e19-121">Clear type font smoothing (allow font smoothing:i:1)</span></span>
* <span data-ttu-id="d1e19-122">Přesměrování všechna podporovaná zařízení Plug and Play.</span><span class="sxs-lookup"><span data-stu-id="d1e19-122">Redirect all supported Plug and Play devices.</span></span> <span data-ttu-id="d1e19-123">(devicestoredirect:s: *)</span><span class="sxs-lookup"><span data-stu-id="d1e19-123">(devicestoredirect:s:*)</span></span>

## <a name="what-other-redirection-is-available"></a><span data-ttu-id="d1e19-124">Jaké další přesměrování je k dispozici?</span><span class="sxs-lookup"><span data-stu-id="d1e19-124">What other redirection is available?</span></span>
<span data-ttu-id="d1e19-125">Ve výchozím nastavení jsou zakázány dvě možnosti přesměrování:</span><span class="sxs-lookup"><span data-stu-id="d1e19-125">Two redirection options are disabled by default:</span></span>

* <span data-ttu-id="d1e19-126">Přesměrování jednotky (mapování jednotky): jednotky místního počítače stane mapované jednotky ve vzdálené relaci hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-126">Drive redirection (drive mapping): Your local computer's drives become mapped drives in hello remote session.</span></span> <span data-ttu-id="d1e19-127">Díky tomu můžete uložit nebo otevřít soubory z místní jednotky během práce ve vzdálené relaci hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-127">This lets you save or open files from your local drives while you work in hello remote session.</span></span>
* <span data-ttu-id="d1e19-128">Přesměrování USB: hello USB zařízení připojených tooyour místního počítače můžete použít v relaci vzdálené hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-128">USB redirection: You can use hello USB devices attached tooyour local computer within hello remote session.</span></span>

## <a name="change-your-redirection-settings-in-remoteapp"></a><span data-ttu-id="d1e19-129">Změňte nastavení přesměrování v Remoteappu</span><span class="sxs-lookup"><span data-stu-id="d1e19-129">Change your redirection settings in RemoteApp</span></span>
<span data-ttu-id="d1e19-130">Nastavení přesměrování hello zařízení pro kolekci můžete změnit pomocí hello Microsoft Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="d1e19-130">You can change hello device redirection settings for a collection by using hello Microsoft Azure PowerShell with SDK.</span></span> <span data-ttu-id="d1e19-131">Po instalaci hello nové prostředí PowerShell a sady SDK, ho napřed nakonfigurovat předplatné jako popsané v toomanage [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d1e19-131">After you install hello new PowerShell and SDK, first configure it toomanage your subscription as described in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="d1e19-132">Pak použijte příkaz podobné toohello následující tooset hello vlastní RDP vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d1e19-132">Then use a command similar toohello following tooset hello custom RDP properties:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="d1e19-133">(Všimněte si, že * \`n * se používá jako oddělovač mezi jednotlivé vlastnosti.)</span><span class="sxs-lookup"><span data-stu-id="d1e19-133">(Note that *\`n* is used as a delimiter between individual properties.)</span></span>

<span data-ttu-id="d1e19-134">tooget seznam nakonfigurovaných jaké vlastní vlastnosti protokolu RDP, spusťte následující rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-134">tooget a list of what custom RDP properties are configured, run hello following cmdlet.</span></span> <span data-ttu-id="d1e19-135">Upozorňujeme, že pouze vlastní vlastnosti jsou uvedeny jako výstup výsledky a není hello výchozí vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d1e19-135">Note that only custom properties are shown as output results and not hello default properties:</span></span>  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

<span data-ttu-id="d1e19-136">Pokud nastavíte vlastní vlastnosti je třeba zadat všechny vlastní vlastnosti pokaždé, když; v opačném případě hello nastavení vrací toodisabled.</span><span class="sxs-lookup"><span data-stu-id="d1e19-136">When you set custom properties you must specify all custom properties each time; otherwise hello setting reverts toodisabled.</span></span>   

### <a name="common-examples"></a><span data-ttu-id="d1e19-137">Běžné příklady</span><span class="sxs-lookup"><span data-stu-id="d1e19-137">Common examples</span></span>
<span data-ttu-id="d1e19-138">Použijte následující rutinu tooenable jednotky přesměrování hello:</span><span class="sxs-lookup"><span data-stu-id="d1e19-138">Use hello following cmdlet tooenable drive redirection:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

<span data-ttu-id="d1e19-139">Použijte tuto rutinu tooenable přesměrování USB a jednotky:</span><span class="sxs-lookup"><span data-stu-id="d1e19-139">Use this cmdlet tooenable both USB and Drive redirection:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="d1e19-140">Použijte tuto rutinu toodisable schránky sdílení:</span><span class="sxs-lookup"><span data-stu-id="d1e19-140">Use this cmdlet toodisable clipboard sharing:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> <span data-ttu-id="d1e19-141">Být jisti toocompletely odhlášení všechny uživatele v kolekci hello (a ne jenom odpojte je) před testovacího hello změnu.</span><span class="sxs-lookup"><span data-stu-id="d1e19-141">Be sure toocompletely log off all users in hello collection (and not just disconnect them) before you test hello change.</span></span> <span data-ttu-id="d1e19-142">tooensure uživatelé úplně odhlášeni, přejděte toohello **relací** v kolekci hello v hello portál Azure a odhlásit všechny uživatele, kterým se odpojí nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d1e19-142">tooensure users are completely logged off, go toohello **Sessions** tab in hello collection in hello Azure portal and log off any users who are disconnected or signed in.</span></span> <span data-ttu-id="d1e19-143">Někdy může trvat několik sekund, tooshow hello místní jednotky v Průzkumníku v rámci relace hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-143">Sometimes it can take several seconds for hello local drives tooshow in Explorer within hello session.</span></span>
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a><span data-ttu-id="d1e19-144">Změňte nastavení přesměrování USB na vašeho klienta Windows</span><span class="sxs-lookup"><span data-stu-id="d1e19-144">Change USB redirection settings on your Windows client</span></span>
<span data-ttu-id="d1e19-145">Pokud chcete toouse přesměrování USB v počítači, který se připojuje tooRemoteApp, existují 2 akce, které je třeba toohappen.</span><span class="sxs-lookup"><span data-stu-id="d1e19-145">If you want toouse USB redirection on a computer that connects tooRemoteApp, there are 2 actions that need toohappen.</span></span> <span data-ttu-id="d1e19-146">1 - Správce musí tooenable přesměrování USB na úrovni kolekce hello pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d1e19-146">1 - Your administrator needs tooenable USB redirection at hello collection level by using Azure PowerShell.</span></span> <span data-ttu-id="d1e19-147">2 – u jednotlivých zařízení, kam chcete toouse přesměrování USB je nutné tooenable zásad skupiny, která umožňuje ho.</span><span class="sxs-lookup"><span data-stu-id="d1e19-147">2 - On each device where you want toouse USB redirection, you need tooenable a group policy that permits it.</span></span> <span data-ttu-id="d1e19-148">Tento krok bude nutné provést pro každý uživatel, který chce přesměrování USB toouse toobe.</span><span class="sxs-lookup"><span data-stu-id="d1e19-148">This step will need toobe done for each user that wants toouse USB redirection.</span></span>

> [!NOTE]
> <span data-ttu-id="d1e19-149">Přesměrování USB s Azure Remoteappem je podporována pouze pro počítače se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="d1e19-149">USB redirection with Azure RemoteApp is only supported for Windows computers.</span></span>
> 
> 

### <a name="enable-usb-redirection-for-hello-remoteapp-collection"></a><span data-ttu-id="d1e19-150">Povolit přesměrování USB pro hello kolekce vzdálené aplikace RemoteApp</span><span class="sxs-lookup"><span data-stu-id="d1e19-150">Enable USB redirection for hello RemoteApp collection</span></span>
<span data-ttu-id="d1e19-151">Použijte následující rutinu tooenable USB přesměrování na úrovni kolekce hello hello:</span><span class="sxs-lookup"><span data-stu-id="d1e19-151">Use hello following cmdlet tooenable USB redirection at hello collection level:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-hello-client-computer"></a><span data-ttu-id="d1e19-152">Povolit přesměrování USB hello klientských počítačů</span><span class="sxs-lookup"><span data-stu-id="d1e19-152">Enable USB redirection for hello client computer</span></span>
<span data-ttu-id="d1e19-153">nastavení přesměrování USB tooconfigure ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="d1e19-153">tooconfigure USB redirection settings on your computer:</span></span>

1. <span data-ttu-id="d1e19-154">Otevřete hello místní Editor zásad skupiny (GPEDIT. MSC).</span><span class="sxs-lookup"><span data-stu-id="d1e19-154">Open hello Local Group Policy Editor (GPEDIT.MSC).</span></span> <span data-ttu-id="d1e19-155">(Spuštění gpedit.msc z příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="d1e19-155">(Run gpedit.msc from a command prompt.)</span></span>
2. <span data-ttu-id="d1e19-156">Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-156">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
3. <span data-ttu-id="d1e19-157">Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-157">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
4. <span data-ttu-id="d1e19-158">Vyberte **povoleno**a potom vyberte **správců a uživatelů v hello RemoteFX USB přesměrování přístupová práva**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-158">Select **Enabled**, and then select **Administrators and Users in hello RemoteFX USB Redirection Access Rights**.</span></span>
5. <span data-ttu-id="d1e19-159">Otevřete příkazový řádek s oprávněním správce a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d1e19-159">Open a command prompt with administrative permissions, and run hello following command:</span></span>
   
        gpupdate /force
6. <span data-ttu-id="d1e19-160">Restartujte počítač hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-160">Restart hello computer.</span></span>

<span data-ttu-id="d1e19-161">Můžete také použít toocreate nástroj pro správu zásad skupiny hello a použití zásad přesměrování hello USB pro všechny počítače ve vaší doméně:</span><span class="sxs-lookup"><span data-stu-id="d1e19-161">You can also use hello Group Policy Management tool toocreate and apply hello USB redirection policy for all computers in your domain:</span></span>

1. <span data-ttu-id="d1e19-162">Přihlaste se k řadiči domény hello jako správce domény hello.</span><span class="sxs-lookup"><span data-stu-id="d1e19-162">Log into hello domain controller as hello domain administrator.</span></span>
2. <span data-ttu-id="d1e19-163">Otevřete hello konzoly pro správu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="d1e19-163">Open hello Group Policy Management Console.</span></span> <span data-ttu-id="d1e19-164">(Klikněte na tlačítko **Start > Nástroje pro správu > Správa zásad skupiny**.)</span><span class="sxs-lookup"><span data-stu-id="d1e19-164">(Click **Start > Administrative Tools > Group Policy Management**.)</span></span>
3. <span data-ttu-id="d1e19-165">Přejděte toohello doménu nebo organizační jednotku, pro které chcete toocreate hello zásad.</span><span class="sxs-lookup"><span data-stu-id="d1e19-165">Navigate toohello domain or organizational unit for which you want toocreate hello policy.</span></span>
4. <span data-ttu-id="d1e19-166">Klikněte pravým tlačítkem na **výchozí zásady domény**a potom klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-166">Right-click **Default Domain Policy**, and then click **Edit**.</span></span>
5. <span data-ttu-id="d1e19-167">Otevřete **počítače složce Konfigurace počítače\Zásady\Šablony pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel připojení k ploše Client\RemoteFX USB zařízení přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-167">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
6. <span data-ttu-id="d1e19-168">Klikněte dvakrát na **povolit RDP přesměrování jiné podporovaná zařízení RemoteFX USB z tohoto počítače**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-168">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
7. <span data-ttu-id="d1e19-169">Vyberte **povoleno**a potom vyberte **správců a uživatelů v hello RemoteFX USB přesměrování přístupová práva**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-169">Select **Enabled**, and then select **Administrators and Users in hello RemoteFX USB Redirection Access Rights**.</span></span>
8. <span data-ttu-id="d1e19-170">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1e19-170">Click **OK**.</span></span>  

