---
title: "aaaReplicate virtuální počítače Hyper-V ve VMM tooa sekundární lokality pomocí prostředí PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Popisuje, jak toodeploy Azure Site Recovery tooorchestrate replikace, převzetí služeb při selhání a obnovení virtuálních počítačů technologie Hyper-V v nástroji VMM cloudů sekundární lokalita VMM tooa pomocí prostředí PowerShell (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: a769dcc68d66c18b9dc47539071f4d0e0f1db70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-powershell-resource-manager"></a><span data-ttu-id="2bb92-103">Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM cloudy tooa sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="2bb92-103">Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site using PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2bb92-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2bb92-104">Azure Portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="2bb92-105">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="2bb92-105">Classic Portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="2bb92-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2bb92-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="2bb92-107">Vítejte tooAzure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="2bb92-107">Welcome tooAzure Site Recovery!</span></span> <span data-ttu-id="2bb92-108">Tento článek použijte, pokud chcete, aby tooreplicate místní virtuálních počítačů technologie Hyper-V spravovaných v System Center Virtual Machine Manager (VMM) cloudy tooa sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="2bb92-108">Use this article if you want tooreplicate on-premises Hyper-V  virtual machines managed in System Center Virtual Machine Manager (VMM) clouds tooa secondary site.</span></span>

<span data-ttu-id="2bb92-109">Tento článek ukazuje, jak toouse prostředí PowerShell tooautomate běžné úlohy je nutné tooperform při nastavování virtuální počítače Azure Site Recovery tooreplicate Hyper-V v cloudech VMM tooSystem Center System Center VMM cloudy v sekundární lokalitě.</span><span class="sxs-lookup"><span data-stu-id="2bb92-109">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooSystem Center VMM clouds in secondary site.</span></span>

<span data-ttu-id="2bb92-110">Hello článek obsahuje požadavky pro hello scénář a ukazuje</span><span class="sxs-lookup"><span data-stu-id="2bb92-110">hello article includes prerequisites for hello scenario, and shows you</span></span>

* <span data-ttu-id="2bb92-111">Jak tooset do trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="2bb92-111">How tooset up a Recovery Services Vault</span></span>
* <span data-ttu-id="2bb92-112">Nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello zdrojovém a cílovém serveru VMM hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-112">Install hello Azure Site Recovery Provider on hello source VMM server and hello target VMM server</span></span>
* <span data-ttu-id="2bb92-113">Zaregistrujte hello servery VMM v trezoru hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-113">Register hello VMM server(s) in hello vault</span></span>
* <span data-ttu-id="2bb92-114">Nakonfigurujte zásady replikace pro hello cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="2bb92-114">Configure replication policy for hello VMM Cloud.</span></span> <span data-ttu-id="2bb92-115">nastavení replikace Hello v zásadách hello bude použité tooall chráněné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="2bb92-115">hello replication settings in hello policy will be applied tooall protected virtual machines</span></span>
* <span data-ttu-id="2bb92-116">Povolení ochrany pro virtuální počítače hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-116">Enable protection for hello virtual machines.</span></span>
* <span data-ttu-id="2bb92-117">Testovací převzetí služeb při selhání hello virtuálních počítačů jednotlivě nebo jako součást toomake plán obnovení, která je opravdu že všechno funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="2bb92-117">Test hello failover of VMs individually or as part of a recovery plan toomake sure everything is working as expected.</span></span>
* <span data-ttu-id="2bb92-118">Proveďte plánované nebo neplánované převzetí služeb při selhání virtuálních počítačů jednotlivě nebo jako součást toomake plán obnovení, která je opravdu že všechno funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="2bb92-118">Perform a planned or an unplanned failover of VMs individually or as part of a recovery plan toomake sure everything is working as expected.</span></span>

<span data-ttu-id="2bb92-119">Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="2bb92-119">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="2bb92-120">Azure má dva různé [modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a klasický model.</span><span class="sxs-lookup"><span data-stu-id="2bb92-120">Azure has two different [deployment models](../azure-resource-manager/resource-manager-deployment-model.md) for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="2bb92-121">Azure má také dva portály – portál Azure classic, která podporuje model nasazení classic hello hello a hello portál Azure s podporou pro oba modely nasazení.</span><span class="sxs-lookup"><span data-stu-id="2bb92-121">Azure also has two portals – hello Azure classic portal that supports hello classic deployment model, and hello Azure portal with support for both deployment models.</span></span> <span data-ttu-id="2bb92-122">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-122">This article covers hello Resource Manager deployment model.</span></span>
>
>

## <a name="on-premises-prerequisites"></a><span data-ttu-id="2bb92-123">Místní požadavky</span><span class="sxs-lookup"><span data-stu-id="2bb92-123">On-premises prerequisites</span></span>
<span data-ttu-id="2bb92-124">Tady je co budete potřebovat v hello primární a sekundární místní lokality toodeploy tento scénář:</span><span class="sxs-lookup"><span data-stu-id="2bb92-124">Here's what you'll need in hello primary and secondary on-premises sites toodeploy this scenario:</span></span>

| <span data-ttu-id="2bb92-125">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="2bb92-125">**Prerequisites**</span></span> | <span data-ttu-id="2bb92-126">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="2bb92-126">**Details**</span></span> |
| --- | --- |
| <span data-ttu-id="2bb92-127">**VMM**</span><span class="sxs-lookup"><span data-stu-id="2bb92-127">**VMM**</span></span> |<span data-ttu-id="2bb92-128">Doporučujeme že nasadit server VMM v primární lokalitě hello a server VMM v sekundární lokalitě hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-128">We recommend you deploy a VMM server in hello primary site and a VMM server in hello secondary site.</span></span><br/><br/> <span data-ttu-id="2bb92-129">Můžete také [replikace mezi cloudy na jednom serveru VMM](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span><span class="sxs-lookup"><span data-stu-id="2bb92-129">You can also [replicate between clouds on a single VMM server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span></span> <span data-ttu-id="2bb92-130">toodo to budete potřebovat aspoň dva cloudy, které jsou nakonfigurované na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-130">toodo this you'll need at least two clouds configured on hello VMM server.</span></span><br/><br/> <span data-ttu-id="2bb92-131">Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-131">VMM servers should be running at least System Center 2012 SP1 with hello latest updates.</span></span><br/><br/> <span data-ttu-id="2bb92-132">Každý server VMM musí mít na jednom nebo více cloudů nakonfigurované a všechny cloudy musí mít nastaven profil hello kapacity technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="2bb92-132">Each VMM server must have at one or more clouds configured and all clouds must have hello Hyper-V Capacity profile set.</span></span> <br/><br/><span data-ttu-id="2bb92-133">Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.</span><span class="sxs-lookup"><span data-stu-id="2bb92-133">Clouds must contain one or more VMM host groups.</span></span><br/><br/><span data-ttu-id="2bb92-134">Další informace o nastavení cloudů VMM v [konfigurace hello VMM cloudové infrastruktury](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [návod: vytvoření privátních cloudů pomocí System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bb92-134">Learn more about setting up VMM clouds in [Configuring hello VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), and [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span></span><br/><br/> <span data-ttu-id="2bb92-135">Servery VMM by měl mít přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2bb92-135">VMM servers should have internet access.</span></span> |
| <span data-ttu-id="2bb92-136">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="2bb92-136">**Hyper-V**</span></span> |<span data-ttu-id="2bb92-137">Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí hello technologie Hyper-V a mít hello nainstalované nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2bb92-137">Hyper-V servers must be running at least Windows Server 2012 with hello Hyper-V role and have hello latest updates installed.</span></span><br/><br/> <span data-ttu-id="2bb92-138">Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2bb92-138">A Hyper-V server should contain one or more VMs.</span></span><br/><br/>  <span data-ttu-id="2bb92-139">Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v hello primárních a sekundárních cloudech VMM.</span><span class="sxs-lookup"><span data-stu-id="2bb92-139">Hyper-V host servers should be located in host groups in hello primary and secondary VMM clouds.</span></span><br/><br/> <span data-ttu-id="2bb92-140">Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 R2 nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)</span><span class="sxs-lookup"><span data-stu-id="2bb92-140">If you're running Hyper-V in a cluster on Windows Server 2012 R2 you should install [update 2961977](https://support.microsoft.com/kb/2961977)</span></span><br/><br/> <span data-ttu-id="2bb92-141">Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 Poznámka že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru.</span><span class="sxs-lookup"><span data-stu-id="2bb92-141">If you're running Hyper-V in a cluster on Windows Server 2012 note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="2bb92-142">Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně.</span><span class="sxs-lookup"><span data-stu-id="2bb92-142">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="2bb92-143">[Přečtěte si další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bb92-143">[Read more](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span></span> |
| <span data-ttu-id="2bb92-144">**Poskytovatel**</span><span class="sxs-lookup"><span data-stu-id="2bb92-144">**Provider**</span></span> |<span data-ttu-id="2bb92-145">Během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery hello na serverech VMM.</span><span class="sxs-lookup"><span data-stu-id="2bb92-145">During Site Recovery deployment you install hello Azure Site Recovery Provider on VMM servers.</span></span> <span data-ttu-id="2bb92-146">Hello zprostředkovatele Site Recovery komunikuje přes protokol HTTPS 443 tooorchestrate replikace.</span><span class="sxs-lookup"><span data-stu-id="2bb92-146">hello Provider communicates with Site Recovery over HTTPS 443 tooorchestrate replication.</span></span> <span data-ttu-id="2bb92-147">Dojde k replikaci dat mezi hello primární a sekundární servery technologie Hyper-V přes hello LAN nebo připojení k síti VPN.</span><span class="sxs-lookup"><span data-stu-id="2bb92-147">Data replication occurs between hello primary and secondary Hyper-V servers over hello LAN or a VPN connection.</span></span><br/><br/> <span data-ttu-id="2bb92-148">Hello zprostředkovatel, který běží na serveru VMM hello potřebuje přístup k adresám URL toothese: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="2bb92-148">hello Provider running on hello VMM server needs access toothese URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.</span></span><br/><br/> <span data-ttu-id="2bb92-149">Kromě toho povolit bránu firewall komunikaci z toohello servery VMM hello [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a povolit protokol HTTPS (443) hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-149">In addition allow firewall communication from hello VMM servers toohello [Azure datacenter IP ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) and allow hello HTTPS (443) protocol.</span></span> |

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="2bb92-150">Požadavky na mapování sítě</span><span class="sxs-lookup"><span data-stu-id="2bb92-150">Network mapping prerequisites</span></span>
<span data-ttu-id="2bb92-151">Mapování mapuje sítě mezi sítěmi virtuálních počítačů nástroje VMM na hello primární a sekundární servery VMM pro:</span><span class="sxs-lookup"><span data-stu-id="2bb92-151">Network mapping maps between VMM VM networks on hello primary and secondary VMM servers to:</span></span>

* <span data-ttu-id="2bb92-152">Optimálně umístíte virtuální počítače repliky na sekundární hostitelů Hyper-V po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="2bb92-152">Optimally place replica VMs on secondary Hyper-V hosts after failover.</span></span>
* <span data-ttu-id="2bb92-153">Připojte virtuální počítače repliky, tooappropriate sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2bb92-153">Connect replica VMs tooappropriate VM networks.</span></span>
* <span data-ttu-id="2bb92-154">Pokud nenakonfigurujete sítě mapování repliky virtuální počítače nebudou připojené tooany sítě po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="2bb92-154">If you don't configure network mapping replica VMs won't be connected tooany network after failover.</span></span>
* <span data-ttu-id="2bb92-155">Pokud chcete, aby tooset sítě mapování během obnovení lokality tady je nasazení co budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="2bb92-155">If you want tooset up network mapping during Site Recovery deployment here's what you'll need:</span></span>

  * <span data-ttu-id="2bb92-156">Zajistěte, aby virtuální počítače na zdrojovém hello hostitelský server Hyper-V byla připojené tooa sítě virtuálních počítačů nástroje VMM.</span><span class="sxs-lookup"><span data-stu-id="2bb92-156">Make sure that VMs on hello source Hyper-V host server are connected tooa VMM VM network.</span></span> <span data-ttu-id="2bb92-157">Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="2bb92-157">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
  * <span data-ttu-id="2bb92-158">Ověřte, zda text hello sekundární cloudu, který budete používat pro obnovení má odpovídající síť virtuálních počítačů konfigurovanou.</span><span class="sxs-lookup"><span data-stu-id="2bb92-158">Verify that hello secondary cloud that you'll use for recovery has a corresponding VM network configured.</span></span> <span data-ttu-id="2bb92-159">Tato síť virtuálních počítačů musí být propojené tooa logické sítě, který je spojen s hello sekundární cloudu.</span><span class="sxs-lookup"><span data-stu-id="2bb92-159">That VM network should be linked tooa logical network that's associated with hello secondary cloud.</span></span>

<span data-ttu-id="2bb92-160">Další informace o konfiguraci sítě VMM v hello níže články</span><span class="sxs-lookup"><span data-stu-id="2bb92-160">Learn more about configuring VMM networks in hello below articles</span></span>

* [<span data-ttu-id="2bb92-161">Jak tooconfigure logické sítě v nástroji VMM</span><span class="sxs-lookup"><span data-stu-id="2bb92-161">How tooconfigure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="2bb92-162">Jak tooconfigure virtuálních počítačů sítí a bran v nástroji VMM</span><span class="sxs-lookup"><span data-stu-id="2bb92-162">How tooconfigure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)

<span data-ttu-id="2bb92-163">[Další informace](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) o tom, jak funguje mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="2bb92-163">[Learn more](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) about how network mapping works.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="2bb92-164">Požadavky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2bb92-164">PowerShell prerequisites</span></span>
<span data-ttu-id="2bb92-165">Ujistěte se, že máte připravené toogo prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2bb92-165">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="2bb92-166">Pokud už používáte prostředí PowerShell, budete potřebovat tooupgrade tooversion 0.8.10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2bb92-166">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="2bb92-167">Informace o nastavení prostředí PowerShell najdete v tématu hello [Průvodce tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="2bb92-167">For information about setting up PowerShell, see hello [Guide tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="2bb92-168">Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny hello dostupných rutin služby hello [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2bb92-168">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="2bb92-169">toolearn o tipy, které vám pomohou při používání hello rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části hello [Průvodce Začínáme s rutinami Azure tooget](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="2bb92-169">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see hello [Guide tooget Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="2bb92-170">Krok 1: Nastavení odběru hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-170">Step 1: Set hello subscription</span></span>
1. <span data-ttu-id="2bb92-171">Z prostředí Azure powershell, tooyour přihlašovací účet Azure: pomocí následující rutiny hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-171">From Azure powershell, login tooyour Azure account: using hello following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="2bb92-172">Získejte seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="2bb92-172">Get a list of your subscriptions.</span></span> <span data-ttu-id="2bb92-173">Pro každé z předplatných hello to taky zobrazí seznam hello subscriptionIDs.</span><span class="sxs-lookup"><span data-stu-id="2bb92-173">This will also list hello subscriptionIDs for each of hello subscriptions.</span></span> <span data-ttu-id="2bb92-174">Poznamenejte si ID předplatného hello hello předplatného, ve kterém chcete trezor služeb zotavení toocreate hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-174">Note down hello subscriptionID of hello subscription in which you wish toocreate hello recovery services vault</span></span>    

        Get-AzureRmSubscription
3. <span data-ttu-id="2bb92-175">Nastavit hello předplatné, ve které hello trezoru služeb zotavení je toobe vytvořené zmínit ID předplatného hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-175">Set hello subscription in which hello recovery services vault is toobe created by mentioning hello subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="2bb92-176">Krok 2 – Vytvoření trezoru Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="2bb92-176">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="2bb92-177">Vytvořte skupinu prostředků Azure Resource Manager, pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="2bb92-177">Create an Azure Resource Manager resource group if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="2bb92-178">Vytvořit nový trezor služeb zotavení a uložte hello vytvořený objekt trezoru automatické obnovení systému v proměnné (použijí později).</span><span class="sxs-lookup"><span data-stu-id="2bb92-178">Create a new Recovery Services vault and save hello created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="2bb92-179">Můžete také načíst hello automatické obnovení systému trezoru post vytvoření objektu pomocí rutiny Get-AzureRMRecoveryServicesVault hello:-</span><span class="sxs-lookup"><span data-stu-id="2bb92-179">You can also retrieve hello ASR vault object post creation using hello Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="2bb92-180">Krok 3: Nastavte kontext hello trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="2bb92-180">Step 3: Set hello Recovery Services Vault context</span></span>
1. <span data-ttu-id="2bb92-181">Pokud máte již vytvořili trezor, spusťte hello níže trezor hello tooget příkaz.</span><span class="sxs-lookup"><span data-stu-id="2bb92-181">If you have a vault already created, run hello below command tooget hello vault.</span></span>

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. <span data-ttu-id="2bb92-182">Nastavit kontext hello trezoru spuštěním hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="2bb92-182">Set hello vault context by running hello below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="2bb92-183">Krok 4: Instalace hello zprostředkovatele Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="2bb92-183">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="2bb92-184">Na počítači VMM hello vytvořte adresář spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2bb92-184">On hello VMM machine, create a directory by running hello following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="2bb92-185">Extrahujte soubory hello pomocí zprostředkovatele hello stáhnout tak, že spustíte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-185">Extract hello files using hello downloaded provider by running hello following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="2bb92-186">Nainstalujte zprostředkovatele hello pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2bb92-186">Install hello provider using hello following commands:</span></span>

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   <span data-ttu-id="2bb92-187">Počkejte toofinish instalace hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-187">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="2bb92-188">Hello server zaregistrujte v trezoru hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2bb92-188">Register hello server in hello vault using hello following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a><span data-ttu-id="2bb92-189">Krok 5: Vytvořte a přidružte zásadu replikace</span><span class="sxs-lookup"><span data-stu-id="2bb92-189">Step 5: Create and associate a replication policy</span></span>
1. <span data-ttu-id="2bb92-190">Vytvořte zásadu replikace 2012 R2 Hyper-V tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2bb92-190">Create a Hyper-V 2012 R2 replication policy by running hello following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify hello number of hours tooretain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify hello frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify hello port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > <span data-ttu-id="2bb92-191">Hello cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server (jak je uvedeno v hello požadavky technologie Hyper-V), ale zásady replikace hello je konkrétní verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="2bb92-191">hello VMM cloud can contain Hyper-V hosts running different versions of Windows Server (as mentioned in hello Hyper-V prerequisites), but hello replication policy is OS version specific.</span></span> <span data-ttu-id="2bb92-192">Pokud máte různých hostitelích, které jsou spuštěné v různých verzí operačních systémů, vytvořte samostatné replikace zásady pro každý typ verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="2bb92-192">If you have different hosts running on different operating system versions, then create separate replication policies for each type of OS version.</span></span> <span data-ttu-id="2bb92-193">Pro např: Pokud máte pět hostitelů, které se spuštěným operačním systémem Windows 2012 serverů a tři na Windows Server 2012 R2, vytvořte dvě zásady replikace – jednu pro každý typ verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="2bb92-193">For eg: If you have five hosts running on Windows Servers 2012 and three on Windows Server 2012 R2, create two replication polices – one for each type of operating system versions.</span></span>

1. <span data-ttu-id="2bb92-194">Spustit hello následující příkazy a získáte hello primární ochranu kontejneru (primární Cloud VMM) a obnovení kontejneru ochrany (obnovení cloudu VMM):</span><span class="sxs-lookup"><span data-stu-id="2bb92-194">Get hello primary protection container (primary VMM Cloud) and recovery protection container (recovery VMM Cloud) by running hello following commands:</span></span>

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. <span data-ttu-id="2bb92-195">Načtení hello zásady, které jste vytvořili v kroku 1 pomocí hello popisný název zásady hello</span><span class="sxs-lookup"><span data-stu-id="2bb92-195">Retrieve hello policy you created in step 1 using hello friendly name of hello policy</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="2bb92-196">Přidružení hello hello kontejneru ochrany (VMM Cloud) začněte zásady replikace hello:</span><span class="sxs-lookup"><span data-stu-id="2bb92-196">Start hello association of hello protection container (VMM Cloud) with hello replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. <span data-ttu-id="2bb92-197">Počkejte toocomplete úlohy přidružení zásad hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-197">Wait for hello policy association job toocomplete.</span></span> <span data-ttu-id="2bb92-198">Můžete se podívat, pokud hello úlohy pomocí hello následující fragment kódu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2bb92-198">You can check if hello job has completed using hello following PowerShell snippet.</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   <span data-ttu-id="2bb92-199">Po dokončení zpracování úlohy hello spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2bb92-199">After hello job has finished processing, run hello following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="2bb92-200">dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="2bb92-200">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-6-configure-network-mapping"></a><span data-ttu-id="2bb92-201">Krok 6: Konfigurace mapování sítě</span><span class="sxs-lookup"><span data-stu-id="2bb92-201">Step 6: Configure network mapping</span></span>
1. <span data-ttu-id="2bb92-202">První příkaz Hello získá servery pro aktuální trezoru Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-202">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="2bb92-203">příkaz Hello ukládá v proměnné pole hello $Servers hello servery Microsoft Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="2bb92-203">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="2bb92-204">Hello níže příkazy získat hello síti pro obnovení lokality pro hello zdrojový server VMM a hello cílovém serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="2bb92-204">hello below commands get hello site recovery network for hello source VMM server and hello target VMM server.</span></span>

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > <span data-ttu-id="2bb92-205">Hello zdrojový server VMM můžete hello první z nich nebo hello druhý jeden v hello servery pole.</span><span class="sxs-lookup"><span data-stu-id="2bb92-205">hello source VMM server can be hello first one or hello second one in hello servers array.</span></span> <span data-ttu-id="2bb92-206">Zkontrolujte hello názvy serverů VMM hello a odpovídajícím způsobem získat hello sítě</span><span class="sxs-lookup"><span data-stu-id="2bb92-206">Check hello names of hello VMM servers and get hello networks appropriately</span></span>


1. <span data-ttu-id="2bb92-207">Hello konečné rutina vytvoří mapování mezi primární a sítí obnovení hello hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-207">hello final cmdlet creates a mapping between hello primary network and hello recovery network.</span></span> <span data-ttu-id="2bb92-208">rutiny Hello určuje hello primární síť jako první prvek hello $PrimaryNetworks a hello obnovení sítě jako první prvek $RecoveryNetworks hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-208">hello cmdlet specifies hello primary network as hello first element of $PrimaryNetworks and hello recovery network as hello first element of $RecoveryNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a><span data-ttu-id="2bb92-209">Krok 7: Konfigurace mapování úložiště</span><span class="sxs-lookup"><span data-stu-id="2bb92-209">Step 7: Configure storage mapping</span></span>
1. <span data-ttu-id="2bb92-210">Hello níže příkaz získá hello seznam klasifikací úložiště do $storageclassifications proměnné.</span><span class="sxs-lookup"><span data-stu-id="2bb92-210">hello below command gets hello list of storage classifications into $storageclassifications variable.</span></span>

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. <span data-ttu-id="2bb92-211">Hello níže příkazy získat hello zdroj klasifikace do proměnné $SourceClassificaion a klasifikaci cíl do $TargetClassification proměnné.</span><span class="sxs-lookup"><span data-stu-id="2bb92-211">hello below commands get hello source classification into $SourceClassificaion variable and target classification into $TargetClassification variable.</span></span>

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > <span data-ttu-id="2bb92-212">klasifikace Hello zdrojem a cílem může být libovolný prvek v poli hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-212">hello source and target classifications can be any element in hello array.</span></span> <span data-ttu-id="2bb92-213">Naleznete toohello výstup hello níže příkaz toofigure hello index zdrojové a cílové klasifikace v poli $storageclassifications.</span><span class="sxs-lookup"><span data-stu-id="2bb92-213">Refer toohello output of hello below command toofigure hello index of source and target classifications in $storageclassifications array.</span></span>

    > <span data-ttu-id="2bb92-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object - vlastnost FriendlyName, Id | Format-Table</span><span class="sxs-lookup"><span data-stu-id="2bb92-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table</span></span>


1. <span data-ttu-id="2bb92-215">Hello níže rutina vytvoří mapování mezi hello zdroj klasifikaci a klasifikaci cíl hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-215">hello below cmdlet creates a mapping between hello source classification and hello target classification.</span></span>

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a><span data-ttu-id="2bb92-216">Krok 8: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="2bb92-216">Step 8: Enable protection for virtual machines</span></span>
<span data-ttu-id="2bb92-217">Po hello serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-217">After hello servers, clouds and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span>

1. <span data-ttu-id="2bb92-218">Ochrana tooenable, spusťte následující příkaz tooget hello ochrany kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="2bb92-218">tooenable protection, run hello following command tooget hello protection container:</span></span>

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. <span data-ttu-id="2bb92-219">Entita ochrany hello (VM) získáte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2bb92-219">Get hello protection entity (VM) by running hello following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. <span data-ttu-id="2bb92-220">Povolení replikace pro virtuální počítač hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2bb92-220">Enable replication for hello VM by running hello following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a><span data-ttu-id="2bb92-221">Otestujte nasazení</span><span class="sxs-lookup"><span data-stu-id="2bb92-221">Test your deployment</span></span>
<span data-ttu-id="2bb92-222">Plánování nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro hello tootest.</span><span class="sxs-lookup"><span data-stu-id="2bb92-222">tootest your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for hello plan.</span></span> <span data-ttu-id="2bb92-223">Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.</span><span class="sxs-lookup"><span data-stu-id="2bb92-223">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb92-224">Můžete vytvořit plán obnovení pro aplikaci na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2bb92-224">You can create a recovery plan for your application in Azure portal.</span></span>
>
>

<span data-ttu-id="2bb92-225">dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="2bb92-225">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="2bb92-226">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="2bb92-226">Run a test failover</span></span>
1. <span data-ttu-id="2bb92-227">Spustit hello níže rutiny tooget hello virtuálních počítačů sítě toowhich chcete tootest převzetí služeb při selhání pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2bb92-227">Run hello below cmdlets tooget hello VM network toowhich you want tootest failover your VMs to.</span></span>

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. <span data-ttu-id="2bb92-228">Proveďte testovací převzetí služeb virtuálního počítače pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="2bb92-228">Perform a test failover of a VM by doing hello following:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. <span data-ttu-id="2bb92-229">Proveďte testovací převzetí služeb při selhání plánu obnovení pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="2bb92-229">Perform a test failover of a recovery plan by doing hello following:</span></span>

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a><span data-ttu-id="2bb92-230">Spusťte plánované převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="2bb92-230">Run a planned failover</span></span>
1. <span data-ttu-id="2bb92-231">Proveďte plánované převzetí služeb při selhání virtuálního počítače pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="2bb92-231">Perform a planned failover of a VM by doing hello following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. <span data-ttu-id="2bb92-232">Proveďte plánované převzetí služeb při selhání plánu obnovení pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="2bb92-232">Perform a planned failover of a recovery plan by doing hello following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="2bb92-233">Spustit neplánované převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="2bb92-233">Run an unplanned failover</span></span>
1. <span data-ttu-id="2bb92-234">Proveďte neplánované převzetí služeb při selhání virtuálního počítače pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="2bb92-234">Perform an unplanned failover of a VM by doing hello following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

<span data-ttu-id="2bb92-235">2. proveďte neplánované převzetí služeb při selhání plánu obnovení pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="2bb92-235">2.Perform an unplanned failover of a recovery plan by doing hello following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <span data-ttu-id="2bb92-236"><a name=monitor></a>Sledování aktivity</span><span class="sxs-lookup"><span data-stu-id="2bb92-236"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="2bb92-237">Použijte následující příkazy toomonitor hello aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-237">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="2bb92-238">Všimněte si, že máte toowait mezi úlohy pro zpracování toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="2bb92-238">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="2bb92-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bb92-239">Next steps</span></span>
<span data-ttu-id="2bb92-240">[Další informace](/powershell/module/azurerm.recoveryservices.backup/#recovery) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2bb92-240">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
