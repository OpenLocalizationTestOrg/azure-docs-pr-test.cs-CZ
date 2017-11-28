---
title: "aaaTroubleshoot Windows virtuální počítač aktivace problémy v Azure | Microsoft Docs"
description: "Poskytuje hello řešení potíží s kroky pro řešení potíží s aktivace systému Windows virtuálního počítače v Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 4ebdac66166e80dcd68cd9e2931b30a29ac01eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a><span data-ttu-id="39d0f-103">Řešení problémů aktivace virtuálního počítače Azure Windows</span><span class="sxs-lookup"><span data-stu-id="39d0f-103">Troubleshoot Azure Windows virtual machine activation problems</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="39d0f-104">Pokud máte potíže při aktivaci Azure Windows virtuální počítač (VM), který je vytvořený z vlastní image, můžete hello informací uvedených v potíže dokumentu tootroubleshoot hello.</span><span class="sxs-lookup"><span data-stu-id="39d0f-104">If you have trouble when activating Azure Windows virtual machine (VM) that is created from a custom image, you can use hello information provided in this document tootroubleshoot hello issue.</span></span> 

## <a name="symptom"></a><span data-ttu-id="39d0f-105">Příznaky</span><span class="sxs-lookup"><span data-stu-id="39d0f-105">Symptom</span></span>

<span data-ttu-id="39d0f-106">Při pokusu o tooactivate virtuálního počítače Windows Azure, se zobrazí chybová zpráva vypadá přibližně hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="39d0f-106">When you try tooactivate an Azure Windows VM, you receive an error message resembles hello following sample:</span></span>

<span data-ttu-id="39d0f-107">**Chyba: 0xC004F074 hello LicensingService softwaru oznámila, že hello počítač nepodařilo aktivovat. Klíč ManagementService (KMS) není možné kontaktovat. Naleznete v protokolu událostí aplikace hello Další informace.**</span><span class="sxs-lookup"><span data-stu-id="39d0f-107">**Error: 0xC004F074 hello Software LicensingService reported that hello computer could not be activated. No Key ManagementService (KMS) could be contacted. Please see hello Application Event Log for additional information.**</span></span>

## <a name="cause"></a><span data-ttu-id="39d0f-108">Příčina</span><span class="sxs-lookup"><span data-stu-id="39d0f-108">Cause</span></span>
<span data-ttu-id="39d0f-109">Obecně platí problémů s aktivací virtuálního počítače Azure dojít, pokud není nakonfiguroval hello virtuální počítač s Windows pomocí hello příslušný instalační klíč klienta služby správy KLÍČŮ nebo virtuální počítač s Windows hello má toohello problém připojením služby správy KLÍČŮ Azure (kms.core.windows.net, port 1668).</span><span class="sxs-lookup"><span data-stu-id="39d0f-109">Generally, Azure VM activation issues occur if hello Windows VM is not configured by using hello appropriate KMS client setup key, or hello Windows VM has a connectivity problem toohello Azure KMS service (kms.core.windows.net, port 1668).</span></span> 

## <a name="solution"></a><span data-ttu-id="39d0f-110">Řešení</span><span class="sxs-lookup"><span data-stu-id="39d0f-110">Solution</span></span>

>[!NOTE]
><span data-ttu-id="39d0f-111">Pokud používáte síť site-to-site VPN a vynuceného tunelování, najdete v části [Azure použijte vlastní směruje tooenable aktivace služby správy KLÍČŮ se vynucené tunelování](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span><span class="sxs-lookup"><span data-stu-id="39d0f-111">If you are using a site-to-site VPN and forced tunneling, see [Use Azure custom routes tooenable KMS activation with forced tunneling](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span></span> 
>
><span data-ttu-id="39d0f-112">Pokud používáte ExpressRoute a budete mít výchozí trasa publikování naleznete v tématu [virtuální počítač Azure může převzetí služeb při selhání tooactivate ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span><span class="sxs-lookup"><span data-stu-id="39d0f-112">If you are using ExpressRoute and you have a default route published, see [Azure VM may fail tooactivate over ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span></span>

### <a name="step-1-configure-hello-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a><span data-ttu-id="39d0f-113">Krok 1 konfigurovat hello vhodné služby správy KLÍČŮ klientský Instalační klíč (pro Windows Server 2016 a Windows Server 2012 R2)</span><span class="sxs-lookup"><span data-stu-id="39d0f-113">Step 1 Configure hello appropriate KMS client setup key (for Windows Server 2016 and Windows Server 2012 R2)</span></span>

<span data-ttu-id="39d0f-114">Pro hello virtuální počítač, který je vytvořený z vlastní image systému Windows Server 2016 nebo Windows Server 2012 R2 je nutné nakonfigurovat hello vhodné služby správy KLÍČŮ Instalační klíč klienta pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="39d0f-114">For hello VM that is created from a custom image of Windows Server 2016 or Windows Server 2012 R2, you must configure hello appropriate KMS client setup key for hello VM.</span></span>

<span data-ttu-id="39d0f-115">Tento krok se nevztahuje tooWindows 2012 nebo Windows 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="39d0f-115">This step does not apply tooWindows 2012 or Windows 2008 R2.</span></span> <span data-ttu-id="39d0f-116">Používá funkci hello automatizace aktivace virtuálního počítače (AVMA), která je podporován pouze Windows Server 2016 nebo Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="39d0f-116">It uses hello Automation Virtual Machine Activation (AVMA) feature, which is supported only by Windows Server 2016 and Windows Server 2012 R2.</span></span>

1. <span data-ttu-id="39d0f-117">Spustit **slmgr.vbs/dlv** na příkazovém řádku se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="39d0f-117">Run **slmgr.vbs /dlv** at an elevated command prompt.</span></span> <span data-ttu-id="39d0f-118">Zkontrolujte hello popisnou hodnotu ve výstupu hello a pak stanovit, zda byl vytvořen z prodejní (prodejní kanál) nebo médium licenci svazek (VOLUME_KMSCLIENT):</span><span class="sxs-lookup"><span data-stu-id="39d0f-118">Check hello Description value in hello output, and then determine whether it was created from retail (RETAIL channel) or volume (VOLUME_KMSCLIENT) license media:</span></span>
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. <span data-ttu-id="39d0f-119">Pokud **slmgr.vbs/dlv** zobrazí prodejní kanál, spusťte následující příkazy tooset hello hello [Instalační klíč klienta služby správy KLÍČŮ](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) hello verzi systému Windows Server používá a vynutit tooretry aktivace:</span><span class="sxs-lookup"><span data-stu-id="39d0f-119">If **slmgr.vbs /dlv** shows RETAIL channel, run hello following commands tooset hello [KMS client setup key](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) for hello version of Windows Server being used, and force it tooretry activation:</span></span> 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    <span data-ttu-id="39d0f-120">Například pro Windows Server 2016 Datacenter, spustili byste následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="39d0f-120">For example, for Windows Server 2016 Datacenter, you would run hello following command:</span></span>

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-hello-connectivity-between-hello-vm-and-azure-kms-service"></a><span data-ttu-id="39d0f-121">Krok 2 Ověřte, zda text hello připojení mezi hello virtuálního počítače a služby správy KLÍČŮ Azure</span><span class="sxs-lookup"><span data-stu-id="39d0f-121">Step 2 Verify hello connectivity between hello VM and Azure KMS service</span></span>

1. <span data-ttu-id="39d0f-122">Stažení a extrakci hello [Pspingu](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) nástroj tooa místní složky v hello virtuální počítač, který nemusí aktivovat.</span><span class="sxs-lookup"><span data-stu-id="39d0f-122">Download and extract hello [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) tool tooa local folder in hello VM that does not activate.</span></span> 

2. <span data-ttu-id="39d0f-123">Přejděte tooStart, vyhledávání v prostředí Windows PowerShell, klikněte pravým tlačítkem na prostředí Windows PowerShell a potom vyberte spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="39d0f-123">Go tooStart, search on Windows PowerShell, right-click Windows PowerShell, and then select Run as administrator.</span></span>

3. <span data-ttu-id="39d0f-124">Zajistěte, aby tento hello je virtuální počítač nakonfigurovaný toouse hello správný server služby správy KLÍČŮ Azure.</span><span class="sxs-lookup"><span data-stu-id="39d0f-124">Make sure that hello VM is configured toouse hello correct Azure KMS server.</span></span> <span data-ttu-id="39d0f-125">toodo to spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="39d0f-125">toodo this, run the following command:</span></span>
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    <span data-ttu-id="39d0f-126">příkaz Hello by měla vrátit: název počítače služby správy klíčů tookms.core.windows.net:1688 úspěšně nastaven.</span><span class="sxs-lookup"><span data-stu-id="39d0f-126">hello command should return: Key Management Service machine name set tookms.core.windows.net:1688 successfully.</span></span>

4. <span data-ttu-id="39d0f-127">Ověřte pomocí Pspingu, že máte připojení k serveru služby správy KLÍČŮ toohello.</span><span class="sxs-lookup"><span data-stu-id="39d0f-127">Verify by using Psping that you have connectivity toohello KMS server.</span></span> <span data-ttu-id="39d0f-128">Přepínač toohello složky, které jste extrahovali hello Pstools.zip stahování a pak spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="39d0f-128">Switch toohello folder where you extracted hello Pstools.zip download, and then run hello following:</span></span>
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  <span data-ttu-id="39d0f-129">Ujistěte se, že se zobrazí v hello sekundu poslední řádek výstupu hello,: odeslané = 4, přijaté = 4, Ztraceno = 0 (0 % ztráty).</span><span class="sxs-lookup"><span data-stu-id="39d0f-129">In hello second-to-last line of hello output, make sure that you see: Sent = 4, Received = 4, Lost = 0 (0% loss).</span></span>

  <span data-ttu-id="39d0f-130">Pokud ztráty je větší než 0 (nula), nemá připojení k serveru služby správy KLÍČŮ toohello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="39d0f-130">If Lost is greater than 0 (zero), hello VM does not have connectivity toohello KMS server.</span></span> <span data-ttu-id="39d0f-131">V této situaci pokud hello virtuálního počítače ve virtuální síti a má vlastní server DNS určena, ujistěte se, že server DNS je schopný tooresolve kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="39d0f-131">In this situation, if hello VM is in a virtual network and has a custom DNS server specified, you must make sure that DNS server is able tooresolve kms.core.windows.net.</span></span> <span data-ttu-id="39d0f-132">Nebo změňte hello DNS server tooone kms.core.windows.net přeložit.</span><span class="sxs-lookup"><span data-stu-id="39d0f-132">Or, change hello DNS server tooone that does resolve kms.core.windows.net.</span></span>

  <span data-ttu-id="39d0f-133">Všimněte si, že pokud odeberete všechny servery DNS z virtuální sítě, virtuální počítače používat interní DNS služby Azure.</span><span class="sxs-lookup"><span data-stu-id="39d0f-133">Notice that if you remove all DNS servers from a virtual network, VMs use Azure’s internal DNS service.</span></span> <span data-ttu-id="39d0f-134">Tato služba může vyřešit kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="39d0f-134">This service can resolve kms.core.windows.net.</span></span>
  
<span data-ttu-id="39d0f-135">Ověřte také, že aby brána firewall hello hosta nebyla nakonfigurována tak, aby by blokovat pokusy o aktivaci.</span><span class="sxs-lookup"><span data-stu-id="39d0f-135">Also verify that hello guest firewall has not been configured in a manner that would block activation attempts.</span></span>

5. <span data-ttu-id="39d0f-136">Po ověření, tookms.core.windows.net úspěšné připojení, spusťte následující příkaz v tomto řádku se zvýšenými oprávněními prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="39d0f-136">After you verify successful connectivity tookms.core.windows.net, run hello following command at that elevated Windows PowerShell prompt.</span></span> <span data-ttu-id="39d0f-137">Tento příkaz se pokusí aktivace vícekrát.</span><span class="sxs-lookup"><span data-stu-id="39d0f-137">This command tries activation multiple times.</span></span>

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

<span data-ttu-id="39d0f-138">Úspěšné aktivaci vrátí informace, které se podobá následující hello:</span><span class="sxs-lookup"><span data-stu-id="39d0f-138">A successful activation returns information that resembles hello following:</span></span>

<span data-ttu-id="39d0f-139">**Aktivace Windows(R) ServerDatacenter edition (12345678-1234-1234-1234-12345678)... Produkt úspěšně aktivoval.**</span><span class="sxs-lookup"><span data-stu-id="39d0f-139">**Activating Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) … Product activated successfully.**</span></span>

## <a name="faq"></a><span data-ttu-id="39d0f-140">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="39d0f-140">FAQ</span></span> 

### <a name="i-created-hello-windows-server-2016-from-azure-marketplace-do-i-need-tooconfigure-kms-key-for-activating-hello-windows-server-2016"></a><span data-ttu-id="39d0f-141">Vytvořený hello systému Windows Server 2016 z webu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="39d0f-141">I created hello Windows Server 2016 from Azure Marketplace.</span></span> <span data-ttu-id="39d0f-142">Potřebuji tooconfigure klíč služby správy KLÍČŮ pro aktivaci hello systému Windows Server 2016?</span><span class="sxs-lookup"><span data-stu-id="39d0f-142">Do I need tooconfigure KMS key for activating hello Windows Server 2016?</span></span> 
 
<span data-ttu-id="39d0f-143">Ne.</span><span class="sxs-lookup"><span data-stu-id="39d0f-143">No.</span></span> <span data-ttu-id="39d0f-144">bitová kopie Hello v Azure Marketplace je hello vhodné služby správy KLÍČŮ Instalační klíč klienta už nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="39d0f-144">hello image in Azure Marketplace has hello appropriate KMS client setup key already configured.</span></span> 

### <a name="does-windows-activation-work-hello-same-way-regardless-if-hello-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a><span data-ttu-id="39d0f-145">Pracovní aktivace Windows hello stejný způsobem, bez ohledu na to pokud hello virtuální počítač používá Azure hybridní použití zvýhodnění (HUB) nebo ne?</span><span class="sxs-lookup"><span data-stu-id="39d0f-145">Does Windows activation work hello same way regardless if hello VM is using Azure Hybrid Use Benefit (HUB) or not?</span></span> 
 
<span data-ttu-id="39d0f-146">Ano.</span><span class="sxs-lookup"><span data-stu-id="39d0f-146">Yes.</span></span> 
 
### <a name="what-happens-if-windows-activation-period-expires"></a><span data-ttu-id="39d0f-147">Co se stane, když vyprší platnost období aktivace systému Windows?</span><span class="sxs-lookup"><span data-stu-id="39d0f-147">What happens if Windows activation period expires?</span></span> 
 
<span data-ttu-id="39d0f-148">Když hello poskytnutá lhůta vyprší a není ještě aktivována Windows, Windows Server 2008 R2 a novějších verzích Windows zobrazí další oznámení o aktivaci.</span><span class="sxs-lookup"><span data-stu-id="39d0f-148">When hello grace period has expired and Windows is still not activated, Windows Server 2008 R2 and later versions of Windows will show additional notifications about activating.</span></span> <span data-ttu-id="39d0f-149">tapeta plochy Hello zůstane černé a Windows Update bude instalovat, zabezpečení a jenom důležité aktualizace, ale není nepovinný aktualizace.</span><span class="sxs-lookup"><span data-stu-id="39d0f-149">hello desktop wallpaper remains black, and Windows Update will install security and critical updates only, but not optional updates.</span></span> <span data-ttu-id="39d0f-150">Zobrazit oznámení hello oddíl na konci hello hello [licenční podmínky](http://technet.microsoft.com/en-us/library/ff793403.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="39d0f-150">See  hello Notifications section at hello bottom of hello [Licensing Conditions](http://technet.microsoft.com/en-us/library/ff793403.aspx) page.</span></span>   

## <a name="need-help-contact-support"></a><span data-ttu-id="39d0f-151">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="39d0f-151">Need help?</span></span> <span data-ttu-id="39d0f-152">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="39d0f-152">Contact support.</span></span>
<span data-ttu-id="39d0f-153">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="39d0f-153">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
