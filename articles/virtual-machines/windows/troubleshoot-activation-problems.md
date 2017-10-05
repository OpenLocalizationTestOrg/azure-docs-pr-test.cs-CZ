---
title: "Řešení problémů aktivace systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Popisuje kroky poradce při potížích pro řešení potíží s aktivace systému Windows virtuálního počítače v Azure"
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
ms.openlocfilehash: 77ef396e0bf75fab56bda2e76516b481d018c5b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a><span data-ttu-id="023fb-103">Řešení problémů aktivace virtuálního počítače Azure Windows</span><span class="sxs-lookup"><span data-stu-id="023fb-103">Troubleshoot Azure Windows virtual machine activation problems</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="023fb-104">Pokud máte potíže při aktivaci Azure Windows virtuální počítač (VM), který je vytvořený z vlastní image, můžete k odstranění potíží informací uvedených v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="023fb-104">If you have trouble when activating Azure Windows virtual machine (VM) that is created from a custom image, you can use the information provided in this document to troubleshoot the issue.</span></span> 

## <a name="symptom"></a><span data-ttu-id="023fb-105">Příznaky</span><span class="sxs-lookup"><span data-stu-id="023fb-105">Symptom</span></span>

<span data-ttu-id="023fb-106">Když se pokusíte aktivovat virtuálního počítače Windows Azure, se zobrazí chybová zpráva vypadá přibližně následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="023fb-106">When you try to activate an Azure Windows VM, you receive an error message resembles the following sample:</span></span>

<span data-ttu-id="023fb-107">**Chyba: 0xC004F074, které LicensingService softwaru oznámila, že se počítač nepodařilo aktivovat. Klíč ManagementService (KMS) není možné kontaktovat. Najdete v protokolu událostí aplikací Další informace.**</span><span class="sxs-lookup"><span data-stu-id="023fb-107">**Error: 0xC004F074 The Software LicensingService reported that the computer could not be activated. No Key ManagementService (KMS) could be contacted. Please see the Application Event Log for additional information.**</span></span>

## <a name="cause"></a><span data-ttu-id="023fb-108">Příčina</span><span class="sxs-lookup"><span data-stu-id="023fb-108">Cause</span></span>
<span data-ttu-id="023fb-109">Obecně platí virtuální počítač Azure problémů s aktivací dojít, pokud virtuální počítač Windows není nakonfigurovaný pomocí příslušný instalační klíč klienta služby správy KLÍČŮ nebo virtuální počítač Windows má potíže s připojením ke službě Azure služby správy KLÍČŮ (kms.core.windows.net, port 1668).</span><span class="sxs-lookup"><span data-stu-id="023fb-109">Generally, Azure VM activation issues occur if the Windows VM is not configured by using the appropriate KMS client setup key, or the Windows VM has a connectivity problem to the Azure KMS service (kms.core.windows.net, port 1668).</span></span> 

## <a name="solution"></a><span data-ttu-id="023fb-110">Řešení</span><span class="sxs-lookup"><span data-stu-id="023fb-110">Solution</span></span>

>[!NOTE]
><span data-ttu-id="023fb-111">Pokud používáte síť site-to-site VPN a vynuceného tunelování, najdete v části [Azure použijte vlastní trasy umožňující služby správy KLÍČŮ aktivaci pomocí vynuceného tunelování](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span><span class="sxs-lookup"><span data-stu-id="023fb-111">If you are using a site-to-site VPN and forced tunneling, see [Use Azure custom routes to enable KMS activation with forced tunneling](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span></span> 
>
><span data-ttu-id="023fb-112">Pokud používáte ExpressRoute a budete mít výchozí trasa publikování naleznete v tématu [virtuálního počítače Azure se pravděpodobně nepodaří aktivovat přes ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span><span class="sxs-lookup"><span data-stu-id="023fb-112">If you are using ExpressRoute and you have a default route published, see [Azure VM may fail to activate over ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span></span>

### <a name="step-1-configure-the-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a><span data-ttu-id="023fb-113">Krok 1 konfigurace příslušný instalační klíč klienta služby správy KLÍČŮ (pro Windows Server 2016 a Windows Server 2012 R2)</span><span class="sxs-lookup"><span data-stu-id="023fb-113">Step 1 Configure the appropriate KMS client setup key (for Windows Server 2016 and Windows Server 2012 R2)</span></span>

<span data-ttu-id="023fb-114">Pro virtuální počítač, který je vytvořený z vlastní image systému Windows Server 2016 nebo Windows Server 2012 R2 je nutné nakonfigurovat příslušný instalační klíč klienta služby správy KLÍČŮ pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="023fb-114">For the VM that is created from a custom image of Windows Server 2016 or Windows Server 2012 R2, you must configure the appropriate KMS client setup key for the VM.</span></span>

<span data-ttu-id="023fb-115">Tento krok se nevztahuje na Windows 2012 nebo Windows 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="023fb-115">This step does not apply to Windows 2012 or Windows 2008 R2.</span></span> <span data-ttu-id="023fb-116">Používá funkci automatizace aktivace virtuálního počítače (AVMA), která je podporován pouze Windows Server 2016 nebo Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="023fb-116">It uses the Automation Virtual Machine Activation (AVMA) feature, which is supported only by Windows Server 2016 and Windows Server 2012 R2.</span></span>

1. <span data-ttu-id="023fb-117">Spustit **slmgr.vbs/dlv** na příkazovém řádku se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="023fb-117">Run **slmgr.vbs /dlv** at an elevated command prompt.</span></span> <span data-ttu-id="023fb-118">Zkontrolujte hodnotu popis ve výstupu a pak stanovit, zda byl vytvořen z prodejní (prodejní kanál) nebo médium licenci svazek (VOLUME_KMSCLIENT):</span><span class="sxs-lookup"><span data-stu-id="023fb-118">Check the Description value in the output, and then determine whether it was created from retail (RETAIL channel) or volume (VOLUME_KMSCLIENT) license media:</span></span>
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. <span data-ttu-id="023fb-119">Pokud **slmgr.vbs/dlv** zobrazí prodejní kanál, spusťte následující příkazy nastavit [Instalační klíč klienta služby správy KLÍČŮ](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) pro verzi systému Windows Server používá a vynutit, aby aktivaci opakujte:</span><span class="sxs-lookup"><span data-stu-id="023fb-119">If **slmgr.vbs /dlv** shows RETAIL channel, run the following commands to set the [KMS client setup key](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) for the version of Windows Server being used, and force it to retry activation:</span></span> 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    <span data-ttu-id="023fb-120">Například pro Windows Server 2016 Datacenter, spustili byste následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="023fb-120">For example, for Windows Server 2016 Datacenter, you would run the following command:</span></span>

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a><span data-ttu-id="023fb-121">Krok 2 Ověřte připojení mezi služby virtuálního počítače a služby správy KLÍČŮ Azure</span><span class="sxs-lookup"><span data-stu-id="023fb-121">Step 2 Verify the connectivity between the VM and Azure KMS service</span></span>

1. <span data-ttu-id="023fb-122">Stažení a extrakci [Pspingu](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) nástroj do místní složky ve virtuálním počítači, který nemusí aktivovat.</span><span class="sxs-lookup"><span data-stu-id="023fb-122">Download and extract the [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) tool to a local folder in the VM that does not activate.</span></span> 

2. <span data-ttu-id="023fb-123">Přejděte na spuštění, vyhledávání v prostředí Windows PowerShell, klikněte pravým tlačítkem na prostředí Windows PowerShell a potom vyberte spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="023fb-123">Go to Start, search on Windows PowerShell, right-click Windows PowerShell, and then select Run as administrator.</span></span>

3. <span data-ttu-id="023fb-124">Ujistěte se, že virtuální počítač je nakonfigurován pro použití správné serveru Azure služby správy KLÍČŮ.</span><span class="sxs-lookup"><span data-stu-id="023fb-124">Make sure that the VM is configured to use the correct Azure KMS server.</span></span> <span data-ttu-id="023fb-125">Chcete-li to provést, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="023fb-125">To do this, run the following command:</span></span>
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    <span data-ttu-id="023fb-126">Příkaz by měla vrátit: název služby správy klíčů počítače byl úspěšně nastaven na kms.core.windows.net:1688.</span><span class="sxs-lookup"><span data-stu-id="023fb-126">The command should return: Key Management Service machine name set to kms.core.windows.net:1688 successfully.</span></span>

4. <span data-ttu-id="023fb-127">Ověřte pomocí Pspingu, že jste připojeni k serveru služby správy KLÍČŮ.</span><span class="sxs-lookup"><span data-stu-id="023fb-127">Verify by using Psping that you have connectivity to the KMS server.</span></span> <span data-ttu-id="023fb-128">Přejděte do složky, které jste extrahovali Pstools.zip stahování a pak spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="023fb-128">Switch to the folder where you extracted the Pstools.zip download, and then run the following:</span></span>
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  <span data-ttu-id="023fb-129">Ujistěte se, že se zobrazí v sekundu poslední řádek výstupu,: odeslané = 4, přijaté = 4, Ztraceno = 0 (0 % ztráty).</span><span class="sxs-lookup"><span data-stu-id="023fb-129">In the second-to-last line of the output, make sure that you see: Sent = 4, Received = 4, Lost = 0 (0% loss).</span></span>

  <span data-ttu-id="023fb-130">Pokud ztráty je větší než 0 (nula), virtuální počítač nemá připojení k serveru služby správy KLÍČŮ.</span><span class="sxs-lookup"><span data-stu-id="023fb-130">If Lost is greater than 0 (zero), the VM does not have connectivity to the KMS server.</span></span> <span data-ttu-id="023fb-131">V této situaci pokud je virtuální počítač ve virtuální síti a má vlastní server DNS, ujistěte se, že server DNS je schopný přeložit kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="023fb-131">In this situation, if the VM is in a virtual network and has a custom DNS server specified, you must make sure that DNS server is able to resolve kms.core.windows.net.</span></span> <span data-ttu-id="023fb-132">Nebo zvolit, které řešení kms.core.windows.net serverem DNS.</span><span class="sxs-lookup"><span data-stu-id="023fb-132">Or, change the DNS server to one that does resolve kms.core.windows.net.</span></span>

  <span data-ttu-id="023fb-133">Všimněte si, že pokud odeberete všechny servery DNS z virtuální sítě, virtuální počítače používat interní DNS služby Azure.</span><span class="sxs-lookup"><span data-stu-id="023fb-133">Notice that if you remove all DNS servers from a virtual network, VMs use Azure’s internal DNS service.</span></span> <span data-ttu-id="023fb-134">Tato služba může vyřešit kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="023fb-134">This service can resolve kms.core.windows.net.</span></span>
  
<span data-ttu-id="023fb-135">Také ověřte, že brána firewall hosta nebyl nakonfigurován způsobem, který by blokovat pokusy o aktivaci.</span><span class="sxs-lookup"><span data-stu-id="023fb-135">Also verify that the guest firewall has not been configured in a manner that would block activation attempts.</span></span>

5. <span data-ttu-id="023fb-136">Po ověření úspěšné připojení k kms.core.windows.net, spusťte na tomto řádku prostředí Windows PowerShell se zvýšenými oprávněními následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="023fb-136">After you verify successful connectivity to kms.core.windows.net, run the following command at that elevated Windows PowerShell prompt.</span></span> <span data-ttu-id="023fb-137">Tento příkaz se pokusí aktivace vícekrát.</span><span class="sxs-lookup"><span data-stu-id="023fb-137">This command tries activation multiple times.</span></span>

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

<span data-ttu-id="023fb-138">Úspěšné aktivaci vrátí informace, které vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="023fb-138">A successful activation returns information that resembles the following:</span></span>

<span data-ttu-id="023fb-139">**Aktivace Windows(R) ServerDatacenter edition (12345678-1234-1234-1234-12345678)... Produkt úspěšně aktivoval.**</span><span class="sxs-lookup"><span data-stu-id="023fb-139">**Activating Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) … Product activated successfully.**</span></span>

## <a name="faq"></a><span data-ttu-id="023fb-140">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="023fb-140">FAQ</span></span> 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a><span data-ttu-id="023fb-141">Windows Server 2016 vytvořený z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="023fb-141">I created the Windows Server 2016 from Azure Marketplace.</span></span> <span data-ttu-id="023fb-142">Je potřeba nakonfigurovat klíč služby správy KLÍČŮ pro aktivaci Windows Server 2016?</span><span class="sxs-lookup"><span data-stu-id="023fb-142">Do I need to configure KMS key for activating the Windows Server 2016?</span></span> 
 
<span data-ttu-id="023fb-143">Ne.</span><span class="sxs-lookup"><span data-stu-id="023fb-143">No.</span></span> <span data-ttu-id="023fb-144">Bitovou kopii v Azure Marketplace obsahuje odpovídající klíč klienta instalace služby správy KLÍČŮ, kde je již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="023fb-144">The image in Azure Marketplace has the appropriate KMS client setup key already configured.</span></span> 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a><span data-ttu-id="023fb-145">Aktivace systému Windows funguje stejným způsobem jako bez ohledu na to pokud virtuální počítač používá Azure hybridní použití zvýhodnění (HUB) nebo ne?</span><span class="sxs-lookup"><span data-stu-id="023fb-145">Does Windows activation work the same way regardless if the VM is using Azure Hybrid Use Benefit (HUB) or not?</span></span> 
 
<span data-ttu-id="023fb-146">Ano.</span><span class="sxs-lookup"><span data-stu-id="023fb-146">Yes.</span></span> 
 
### <a name="what-happens-if-windows-activation-period-expires"></a><span data-ttu-id="023fb-147">Co se stane, když vyprší platnost období aktivace systému Windows?</span><span class="sxs-lookup"><span data-stu-id="023fb-147">What happens if Windows activation period expires?</span></span> 
 
<span data-ttu-id="023fb-148">Když poskytnutá lhůta vyprší a systému Windows není ještě aktivována, se zobrazí další oznámení o aktivaci služby Windows Server 2008 R2 a novějších verzích Windows.</span><span class="sxs-lookup"><span data-stu-id="023fb-148">When the grace period has expired and Windows is still not activated, Windows Server 2008 R2 and later versions of Windows will show additional notifications about activating.</span></span> <span data-ttu-id="023fb-149">Tapeta plochy zůstane černé a Windows Update bude instalovat, zabezpečení a jenom důležité aktualizace, ale není nepovinný aktualizace.</span><span class="sxs-lookup"><span data-stu-id="023fb-149">The desktop wallpaper remains black, and Windows Update will install security and critical updates only, but not optional updates.</span></span> <span data-ttu-id="023fb-150">Najdete v části oznámení v dolní části [licenční podmínky](http://technet.microsoft.com/en-us/library/ff793403.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="023fb-150">See  the Notifications section at the bottom of the [Licensing Conditions](http://technet.microsoft.com/en-us/library/ff793403.aspx) page.</span></span>   

## <a name="need-help-contact-support"></a><span data-ttu-id="023fb-151">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="023fb-151">Need help?</span></span> <span data-ttu-id="023fb-152">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="023fb-152">Contact support.</span></span>
<span data-ttu-id="023fb-153">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="023fb-153">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>