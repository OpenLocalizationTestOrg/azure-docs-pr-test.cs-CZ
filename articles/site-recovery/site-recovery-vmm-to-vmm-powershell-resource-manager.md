---
title: "Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM do sekundární lokality pomocí prostředí PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Popisuje, jak nasadit Azure Site Recovery k orchestraci replikace, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)"
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
ms.openlocfilehash: 5a6e00877b0a2b139d5322f610c1901ad76a710f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a><span data-ttu-id="51894-103">Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="51894-103">Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site using PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="51894-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="51894-104">Azure Portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="51894-105">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="51894-105">Classic Portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="51894-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="51894-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="51894-107">Vítejte v Azure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="51894-107">Welcome to Azure Site Recovery!</span></span> <span data-ttu-id="51894-108">Použití v tomto článku, pokud chcete replikovat virtuální počítače Hyper-V místní spravované v cloudech System Center Virtual Machine Manager (VMM) do sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="51894-108">Use this article if you want to replicate on-premises Hyper-V  virtual machines managed in System Center Virtual Machine Manager (VMM) clouds to a secondary site.</span></span>

<span data-ttu-id="51894-109">Tento článek ukazuje, jak pomocí prostředí PowerShell pro automatizaci běžných úkolů, které je třeba provést při nastavení Azure Site Recovery replikovat virtuální počítače Hyper-V v cloudech System Center VMM cloudech System Center VMM v sekundární lokalitě.</span><span class="sxs-lookup"><span data-stu-id="51894-109">This article shows you how to use PowerShell to automate common tasks you need to perform when you set up Azure Site Recovery to replicate Hyper-V virtual machines in System Center VMM clouds to System Center VMM clouds in secondary site.</span></span>

<span data-ttu-id="51894-110">Článek obsahuje předpoklady pro scénář a ukazuje</span><span class="sxs-lookup"><span data-stu-id="51894-110">The article includes prerequisites for the scenario, and shows you</span></span>

* <span data-ttu-id="51894-111">Jak nastavit trezor služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="51894-111">How to set up a Recovery Services Vault</span></span>
* <span data-ttu-id="51894-112">Nainstalujte zprostředkovatele Azure Site Recovery na zdrojovém serveru VMM a cílovém serveru VMM</span><span class="sxs-lookup"><span data-stu-id="51894-112">Install the Azure Site Recovery Provider on the source VMM server and the target VMM server</span></span>
* <span data-ttu-id="51894-113">Zaregistrujte servery VMM v trezoru</span><span class="sxs-lookup"><span data-stu-id="51894-113">Register the VMM server(s) in the vault</span></span>
* <span data-ttu-id="51894-114">Nakonfigurujte zásady replikace pro VMM Cloud.</span><span class="sxs-lookup"><span data-stu-id="51894-114">Configure replication policy for the VMM Cloud.</span></span> <span data-ttu-id="51894-115">Nastavení replikace na zásady se použijí na všechny chráněné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="51894-115">The replication settings in the policy will be applied to all protected virtual machines</span></span>
* <span data-ttu-id="51894-116">Povolení ochrany pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="51894-116">Enable protection for the virtual machines.</span></span>
* <span data-ttu-id="51894-117">Testovací převzetí služeb při selhání virtuálních počítačů jednotlivě nebo jako součást plánu obnovení a ujistěte se, zda že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="51894-117">Test the failover of VMs individually or as part of a recovery plan to make sure everything is working as expected.</span></span>
* <span data-ttu-id="51894-118">Proveďte plánované nebo neplánované převzetí služeb při selhání virtuálních počítačů jednotlivě nebo jako součást plánu obnovení a ujistěte se, zda že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="51894-118">Perform a planned or an unplanned failover of VMs individually or as part of a recovery plan to make sure everything is working as expected.</span></span>

<span data-ttu-id="51894-119">Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="51894-119">If you run into problems setting up this scenario, post your questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="51894-120">Azure má dva různé [modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a klasický model.</span><span class="sxs-lookup"><span data-stu-id="51894-120">Azure has two different [deployment models](../azure-resource-manager/resource-manager-deployment-model.md) for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="51894-121">Azure má také dva portály – portál Azure Classic, který podporuje klasický model nasazení, a portál Azure s podporou pro oba modely nasazení.</span><span class="sxs-lookup"><span data-stu-id="51894-121">Azure also has two portals – the Azure classic portal that supports the classic deployment model, and the Azure portal with support for both deployment models.</span></span> <span data-ttu-id="51894-122">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="51894-122">This article covers the Resource Manager deployment model.</span></span>
>
>

## <a name="on-premises-prerequisites"></a><span data-ttu-id="51894-123">Místní požadavky</span><span class="sxs-lookup"><span data-stu-id="51894-123">On-premises prerequisites</span></span>
<span data-ttu-id="51894-124">Tady je co budete muset v lokalitách primární a sekundární místní nasazení tohoto scénáře:</span><span class="sxs-lookup"><span data-stu-id="51894-124">Here's what you'll need in the primary and secondary on-premises sites to deploy this scenario:</span></span>

| <span data-ttu-id="51894-125">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="51894-125">**Prerequisites**</span></span> | <span data-ttu-id="51894-126">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="51894-126">**Details**</span></span> |
| --- | --- |
| <span data-ttu-id="51894-127">**VMM**</span><span class="sxs-lookup"><span data-stu-id="51894-127">**VMM**</span></span> |<span data-ttu-id="51894-128">Doporučujeme že nasadit server VMM v primární lokalitě a server VMM v sekundární lokalitě.</span><span class="sxs-lookup"><span data-stu-id="51894-128">We recommend you deploy a VMM server in the primary site and a VMM server in the secondary site.</span></span><br/><br/> <span data-ttu-id="51894-129">Můžete také [replikace mezi cloudy na jednom serveru VMM](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span><span class="sxs-lookup"><span data-stu-id="51894-129">You can also [replicate between clouds on a single VMM server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span></span> <span data-ttu-id="51894-130">K tomu budete potřebovat aspoň dva cloudy, které jsou nakonfigurované na serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="51894-130">To do this you'll need at least two clouds configured on the VMM server.</span></span><br/><br/> <span data-ttu-id="51894-131">Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi.</span><span class="sxs-lookup"><span data-stu-id="51894-131">VMM servers should be running at least System Center 2012 SP1 with the latest updates.</span></span><br/><br/> <span data-ttu-id="51894-132">Každý server VMM musí mít na jeden nebo více cloudy nakonfigurované a všechny cloudy musí mít sadu profilů kapacity technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="51894-132">Each VMM server must have at one or more clouds configured and all clouds must have the Hyper-V Capacity profile set.</span></span> <br/><br/><span data-ttu-id="51894-133">Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.</span><span class="sxs-lookup"><span data-stu-id="51894-133">Clouds must contain one or more VMM host groups.</span></span><br/><br/><span data-ttu-id="51894-134">Další informace o nastavení cloudů VMM v [konfigurace VMM cloudové infrastruktury](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [návod: vytvoření privátních cloudů pomocí System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span><span class="sxs-lookup"><span data-stu-id="51894-134">Learn more about setting up VMM clouds in [Configuring the VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), and [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span></span><br/><br/> <span data-ttu-id="51894-135">Servery VMM by měl mít přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="51894-135">VMM servers should have internet access.</span></span> |
| <span data-ttu-id="51894-136">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="51894-136">**Hyper-V**</span></span> |<span data-ttu-id="51894-137">Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí Hyper-V a mají nainstalované nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="51894-137">Hyper-V servers must be running at least Windows Server 2012 with the Hyper-V role and have the latest updates installed.</span></span><br/><br/> <span data-ttu-id="51894-138">Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51894-138">A Hyper-V server should contain one or more VMs.</span></span><br/><br/>  <span data-ttu-id="51894-139">Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v primárních a sekundárních cloudech VMM.</span><span class="sxs-lookup"><span data-stu-id="51894-139">Hyper-V host servers should be located in host groups in the primary and secondary VMM clouds.</span></span><br/><br/> <span data-ttu-id="51894-140">Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 R2 nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)</span><span class="sxs-lookup"><span data-stu-id="51894-140">If you're running Hyper-V in a cluster on Windows Server 2012 R2 you should install [update 2961977](https://support.microsoft.com/kb/2961977)</span></span><br/><br/> <span data-ttu-id="51894-141">Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 Poznámka že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru.</span><span class="sxs-lookup"><span data-stu-id="51894-141">If you're running Hyper-V in a cluster on Windows Server 2012 note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="51894-142">Budete ho muset nakonfigurovat ručně.</span><span class="sxs-lookup"><span data-stu-id="51894-142">You'll need to configure the cluster broker manually.</span></span> <span data-ttu-id="51894-143">[Další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span><span class="sxs-lookup"><span data-stu-id="51894-143">[Read more](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span></span> |
| <span data-ttu-id="51894-144">**Poskytovatel**</span><span class="sxs-lookup"><span data-stu-id="51894-144">**Provider**</span></span> |<span data-ttu-id="51894-145">Během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery na serverech VMM.</span><span class="sxs-lookup"><span data-stu-id="51894-145">During Site Recovery deployment you install the Azure Site Recovery Provider on VMM servers.</span></span> <span data-ttu-id="51894-146">Zprostředkovatel komunikováním se službou Site Recovery přes protokol HTTPS 443 pro orchestraci replikace.</span><span class="sxs-lookup"><span data-stu-id="51894-146">The Provider communicates with Site Recovery over HTTPS 443 to orchestrate replication.</span></span> <span data-ttu-id="51894-147">Dojde k replikaci dat mezi primární a sekundární servery technologie Hyper-V přes síť LAN nebo připojení k síti VPN.</span><span class="sxs-lookup"><span data-stu-id="51894-147">Data replication occurs between the primary and secondary Hyper-V servers over the LAN or a VPN connection.</span></span><br/><br/> <span data-ttu-id="51894-148">Zprostředkovatel, který běží na serveru VMM potřebuje přístup k tyto adresy URL: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="51894-148">The Provider running on the VMM server needs access to these URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.</span></span><br/><br/> <span data-ttu-id="51894-149">Kromě toho povolit bránu firewall komunikaci ze serverů VMM pro [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a povolit protokol HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="51894-149">In addition allow firewall communication from the VMM servers to the [Azure datacenter IP ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) and allow the HTTPS (443) protocol.</span></span> |

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="51894-150">Požadavky na mapování sítě</span><span class="sxs-lookup"><span data-stu-id="51894-150">Network mapping prerequisites</span></span>
<span data-ttu-id="51894-151">Mapování mapuje sítě mezi sítěmi virtuálních počítačů nástroje VMM na primární a sekundární servery VMM pro:</span><span class="sxs-lookup"><span data-stu-id="51894-151">Network mapping maps between VMM VM networks on the primary and secondary VMM servers to:</span></span>

* <span data-ttu-id="51894-152">Optimálně umístíte virtuální počítače repliky na sekundární hostitelů Hyper-V po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="51894-152">Optimally place replica VMs on secondary Hyper-V hosts after failover.</span></span>
* <span data-ttu-id="51894-153">Virtuální počítače repliky se připojte k příslušné sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51894-153">Connect replica VMs to appropriate VM networks.</span></span>
* <span data-ttu-id="51894-154">Pokud nenakonfigurujete sítě mapování repliky virtuálních počítačů se nepřipojí k žádné síti po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="51894-154">If you don't configure network mapping replica VMs won't be connected to any network after failover.</span></span>
* <span data-ttu-id="51894-155">Pokud chcete nastavit síť mapování během obnovení lokality v tomto poli je nasazení co budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="51894-155">If you want to set up network mapping during Site Recovery deployment here's what you'll need:</span></span>

  * <span data-ttu-id="51894-156">Zajistěte, aby virtuální počítače na zdrojovém hostitelském serveru Hyper-V byly připojené k síti virtuálních počítačů ve VMM.</span><span class="sxs-lookup"><span data-stu-id="51894-156">Make sure that VMs on the source Hyper-V host server are connected to a VMM VM network.</span></span> <span data-ttu-id="51894-157">Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.</span><span class="sxs-lookup"><span data-stu-id="51894-157">That network should be linked to a logical network that is associated with the cloud.</span></span>
  * <span data-ttu-id="51894-158">Ověřte, že sekundární cloudu, které budete používat pro obnovení má odpovídající síť virtuálních počítačů konfigurovanou.</span><span class="sxs-lookup"><span data-stu-id="51894-158">Verify that the secondary cloud that you'll use for recovery has a corresponding VM network configured.</span></span> <span data-ttu-id="51894-159">Tato síť virtuálních počítačů musí být propojena na logickou síť, která je přidružená sekundární cloudu.</span><span class="sxs-lookup"><span data-stu-id="51894-159">That VM network should be linked to a logical network that's associated with the secondary cloud.</span></span>

<span data-ttu-id="51894-160">Další informace o konfiguraci sítě VMM v následující články</span><span class="sxs-lookup"><span data-stu-id="51894-160">Learn more about configuring VMM networks in the below articles</span></span>

* [<span data-ttu-id="51894-161">Postup konfigurace logické sítě v nástroji VMM</span><span class="sxs-lookup"><span data-stu-id="51894-161">How to configure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="51894-162">Postup konfigurace sítě virtuálních počítačů a bran v nástroji VMM</span><span class="sxs-lookup"><span data-stu-id="51894-162">How to configure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)

<span data-ttu-id="51894-163">[Další informace](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) o tom, jak funguje mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="51894-163">[Learn more](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) about how network mapping works.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="51894-164">Požadavky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="51894-164">PowerShell prerequisites</span></span>
<span data-ttu-id="51894-165">Ujistěte se, že máte Azure PowerShell připravené na vynucování.</span><span class="sxs-lookup"><span data-stu-id="51894-165">Make sure you have Azure PowerShell ready to go.</span></span> <span data-ttu-id="51894-166">Pokud už používáte prostředí PowerShell, budete muset upgradovat na verzi 0.8.10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="51894-166">If you are already using PowerShell, you'll need to upgrade to version 0.8.10 or later.</span></span> <span data-ttu-id="51894-167">Informace o nastavení prostředí PowerShell najdete v tématu [Průvodce instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="51894-167">For information about setting up PowerShell, see the [Guide to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="51894-168">Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny dostupné rutiny služby [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="51894-168">Once you have set up and configured PowerShell, you can view all of the available cmdlets for the service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="51894-169">Další informace o tipy, které mohou pomoci pomocí rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v článku [Průvodce Začínáme s Azure rutiny](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="51894-169">To learn about tips that can help you use the cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see the [Guide to get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-the-subscription"></a><span data-ttu-id="51894-170">Krok 1: Nastavte předplatné</span><span class="sxs-lookup"><span data-stu-id="51894-170">Step 1: Set the subscription</span></span>
1. <span data-ttu-id="51894-171">Z prostředí Azure powershell, přihlášení k účtu Azure: pomocí následujících rutin</span><span class="sxs-lookup"><span data-stu-id="51894-171">From Azure powershell, login to your Azure account: using the following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="51894-172">Získejte seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="51894-172">Get a list of your subscriptions.</span></span> <span data-ttu-id="51894-173">SubscriptionIDs to taky zobrazí seznam všech odběrů.</span><span class="sxs-lookup"><span data-stu-id="51894-173">This will also list the subscriptionIDs for each of the subscriptions.</span></span> <span data-ttu-id="51894-174">Poznamenejte si ID předplatného předplatné, ve kterém chcete vytvořit trezor služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="51894-174">Note down the subscriptionID of the subscription in which you wish to create the recovery services vault</span></span>    

        Get-AzureRmSubscription
3. <span data-ttu-id="51894-175">Nastavte předplatné, ve kterém má být vytvořen třeba uvést ID předplatného trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="51894-175">Set the subscription in which the recovery services vault is to be created by mentioning the subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="51894-176">Krok 2 – Vytvoření trezoru Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="51894-176">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="51894-177">Vytvořte skupinu prostředků Azure Resource Manager, pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="51894-177">Create an Azure Resource Manager resource group if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="51894-178">Vytvořit nový trezor služeb zotavení a uložte vytvořený objekt trezoru automatické obnovení systému v proměnné (použijí později).</span><span class="sxs-lookup"><span data-stu-id="51894-178">Create a new Recovery Services vault and save the created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="51894-179">Můžete také načíst pomocí rutiny Get-AzureRMRecoveryServicesVault vytvoření post objektu automatické obnovení systému trezoru:-</span><span class="sxs-lookup"><span data-stu-id="51894-179">You can also retrieve the ASR vault object post creation using the Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a><span data-ttu-id="51894-180">Krok 3: Nastavte kontext trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="51894-180">Step 3: Set the Recovery Services Vault context</span></span>
1. <span data-ttu-id="51894-181">Pokud máte již vytvořili trezor, spusťte níže získat trezoru.</span><span class="sxs-lookup"><span data-stu-id="51894-181">If you have a vault already created, run the below command to get the vault.</span></span>

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. <span data-ttu-id="51894-182">Nastavit kontext trezoru spuštěním následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="51894-182">Set the vault context by running the below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a><span data-ttu-id="51894-183">Krok 4: Instalace nástroje Azure Site Recovery Provider</span><span class="sxs-lookup"><span data-stu-id="51894-183">Step 4: Install the Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="51894-184">Na počítači VMM vytvořte adresář tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51894-184">On the VMM machine, create a directory by running the following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="51894-185">Extrahujte soubory pomocí stažené poskytovatele tak, že spustíte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="51894-185">Extract the files using the downloaded provider by running the following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="51894-186">Nainstalujte poskytovatele pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="51894-186">Install the provider using the following commands:</span></span>

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

   <span data-ttu-id="51894-187">Počkejte na dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="51894-187">Wait for the installation to finish.</span></span>
4. <span data-ttu-id="51894-188">Zaregistrujte server v trezoru, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="51894-188">Register the server in the vault using the following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a><span data-ttu-id="51894-189">Krok 5: Vytvořte a přidružte zásadu replikace</span><span class="sxs-lookup"><span data-stu-id="51894-189">Step 5: Create and associate a replication policy</span></span>
1. <span data-ttu-id="51894-190">Vytvořte zásadu replikace 2012 R2 Hyper-V tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51894-190">Create a Hyper-V 2012 R2 replication policy by running the following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > <span data-ttu-id="51894-191">Cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server (jak je uvedeno v požadavky technologie Hyper-V), ale zásady replikace je konkrétní verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="51894-191">The VMM cloud can contain Hyper-V hosts running different versions of Windows Server (as mentioned in the Hyper-V prerequisites), but the replication policy is OS version specific.</span></span> <span data-ttu-id="51894-192">Pokud máte různých hostitelích, které jsou spuštěné v různých verzí operačních systémů, vytvořte samostatné replikace zásady pro každý typ verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="51894-192">If you have different hosts running on different operating system versions, then create separate replication policies for each type of OS version.</span></span> <span data-ttu-id="51894-193">Pro např: Pokud máte pět hostitelů, které se spuštěným operačním systémem Windows 2012 serverů a tři na Windows Server 2012 R2, vytvořte dvě zásady replikace – jednu pro každý typ verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="51894-193">For eg: If you have five hosts running on Windows Servers 2012 and three on Windows Server 2012 R2, create two replication polices – one for each type of operating system versions.</span></span>

1. <span data-ttu-id="51894-194">Získáte primární ochranu kontejneru (primární Cloud VMM) a obnovení kontejneru ochrany (obnovení cloudu VMM) tak, že spustíte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="51894-194">Get the primary protection container (primary VMM Cloud) and recovery protection container (recovery VMM Cloud) by running the following commands:</span></span>

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. <span data-ttu-id="51894-195">Načtení na zásadu, kterou jste vytvořili v kroku 1 pomocí popisný název zásady</span><span class="sxs-lookup"><span data-stu-id="51894-195">Retrieve the policy you created in step 1 using the friendly name of the policy</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="51894-196">Přidružení kontejneru ochrany (VMM Cloud) začněte zásady replikace:</span><span class="sxs-lookup"><span data-stu-id="51894-196">Start the association of the protection container (VMM Cloud) with the replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. <span data-ttu-id="51894-197">Počkejte na dokončení úlohy přidružení zásad.</span><span class="sxs-lookup"><span data-stu-id="51894-197">Wait for the policy association job to complete.</span></span> <span data-ttu-id="51894-198">Můžete se podívat, pokud byla dokončena úloha pomocí následující fragment kódu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="51894-198">You can check if the job has completed using the following PowerShell snippet.</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   <span data-ttu-id="51894-199">Po dokončení zpracování úlohy spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51894-199">After the job has finished processing, run the following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="51894-200">Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="51894-200">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-6-configure-network-mapping"></a><span data-ttu-id="51894-201">Krok 6: Konfigurace mapování sítě</span><span class="sxs-lookup"><span data-stu-id="51894-201">Step 6: Configure network mapping</span></span>
1. <span data-ttu-id="51894-202">První příkaz získá servery pro aktuální trezoru Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="51894-202">The first command gets servers for the current Azure Site Recovery vault.</span></span> <span data-ttu-id="51894-203">Příkaz ukládá v proměnné pole $Servers servery Microsoft Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="51894-203">The command stores the Microsoft Azure Site Recovery servers in the $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="51894-204">Níže příkazy získat síť site recovery pro server VMM zdrojovém a cílovém serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="51894-204">The below commands get the site recovery network for the source VMM server and the target VMM server.</span></span>

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > <span data-ttu-id="51894-205">Zdrojový server VMM může být první nebo druhý v poli servery.</span><span class="sxs-lookup"><span data-stu-id="51894-205">The source VMM server can be the first one or the second one in the servers array.</span></span> <span data-ttu-id="51894-206">Zkontrolujte názvy serverů VMM a získejte sítě správně</span><span class="sxs-lookup"><span data-stu-id="51894-206">Check the names of the VMM servers and get the networks appropriately</span></span>


1. <span data-ttu-id="51894-207">Poslední rutina vytvoří mapování mezi primární síť a síti pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="51894-207">The final cmdlet creates a mapping between the primary network and the recovery network.</span></span> <span data-ttu-id="51894-208">Rutina určuje primární síť jako první prvek $PrimaryNetworks a obnovení sítě jako první prvek $RecoveryNetworks.</span><span class="sxs-lookup"><span data-stu-id="51894-208">The cmdlet specifies the primary network as the first element of $PrimaryNetworks and the recovery network as the first element of $RecoveryNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a><span data-ttu-id="51894-209">Krok 7: Konfigurace mapování úložiště</span><span class="sxs-lookup"><span data-stu-id="51894-209">Step 7: Configure storage mapping</span></span>
1. <span data-ttu-id="51894-210">Níže příkaz získá seznam klasifikací úložiště do $storageclassifications proměnné.</span><span class="sxs-lookup"><span data-stu-id="51894-210">The below command gets the list of storage classifications into $storageclassifications variable.</span></span>

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. <span data-ttu-id="51894-211">Níže příkazy získali klasifikaci zdroje do proměnné a cíle klasifikace $SourceClassificaion do $TargetClassification proměnné.</span><span class="sxs-lookup"><span data-stu-id="51894-211">The below commands get the source classification into $SourceClassificaion variable and target classification into $TargetClassification variable.</span></span>

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > <span data-ttu-id="51894-212">Klasifikace zdrojem a cílem může být libovolný prvek v poli.</span><span class="sxs-lookup"><span data-stu-id="51894-212">The source and target classifications can be any element in the array.</span></span> <span data-ttu-id="51894-213">Odkazovat na výstup níže příkazu obrázek indexu zdrojové a cílové klasifikace v poli $storageclassifications.</span><span class="sxs-lookup"><span data-stu-id="51894-213">Refer to the output of the below command to figure the index of source and target classifications in $storageclassifications array.</span></span>

    > <span data-ttu-id="51894-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object - vlastnost FriendlyName, Id | Format-Table</span><span class="sxs-lookup"><span data-stu-id="51894-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table</span></span>


1. <span data-ttu-id="51894-215">Následující rutina vytvoří mapování mezi klasifikace zdroj a cíl klasifikace.</span><span class="sxs-lookup"><span data-stu-id="51894-215">The below cmdlet creates a mapping between the source classification and the target classification.</span></span>

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a><span data-ttu-id="51894-216">Krok 8: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="51894-216">Step 8: Enable protection for virtual machines</span></span>
<span data-ttu-id="51894-217">Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu.</span><span class="sxs-lookup"><span data-stu-id="51894-217">After the servers, clouds and networks are configured correctly, you can enable protection for virtual machines in the cloud.</span></span>

1. <span data-ttu-id="51894-218">Pokud chcete povolit ochranu, spusťte následující příkaz k získání kontejneru ochrany:</span><span class="sxs-lookup"><span data-stu-id="51894-218">To enable protection, run the following command to get the protection container:</span></span>

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. <span data-ttu-id="51894-219">Získáte entita ochrany (VM) tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51894-219">Get the protection entity (VM) by running the following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. <span data-ttu-id="51894-220">Povolení replikace pro virtuální počítač tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51894-220">Enable replication for the VM by running the following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a><span data-ttu-id="51894-221">Otestujte nasazení</span><span class="sxs-lookup"><span data-stu-id="51894-221">Test your deployment</span></span>
<span data-ttu-id="51894-222">Chcete nasazení otestovat můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro plán.</span><span class="sxs-lookup"><span data-stu-id="51894-222">To test your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for the plan.</span></span> <span data-ttu-id="51894-223">Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.</span><span class="sxs-lookup"><span data-stu-id="51894-223">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span>

> [!NOTE]
> <span data-ttu-id="51894-224">Můžete vytvořit plán obnovení pro aplikaci na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="51894-224">You can create a recovery plan for your application in Azure portal.</span></span>
>
>

<span data-ttu-id="51894-225">Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="51894-225">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="51894-226">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="51894-226">Run a test failover</span></span>
1. <span data-ttu-id="51894-227">Spustit níže rutiny získat virtuální počítače k síti virtuálních počítačů, do které chcete testovat převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="51894-227">Run the below cmdlets to get the VM network to which you want to test failover your VMs to.</span></span>

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. <span data-ttu-id="51894-228">Proveďte testovací převzetí služeb virtuálního počítače provedením následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="51894-228">Perform a test failover of a VM by doing the following:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. <span data-ttu-id="51894-229">Pomocí následujícího postupu proveďte testovací převzetí služeb při selhání plánu obnovení:</span><span class="sxs-lookup"><span data-stu-id="51894-229">Perform a test failover of a recovery plan by doing the following:</span></span>

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a><span data-ttu-id="51894-230">Spusťte plánované převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="51894-230">Run a planned failover</span></span>
1. <span data-ttu-id="51894-231">Proveďte plánované převzetí služeb při selhání virtuálního počítače provedením následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="51894-231">Perform a planned failover of a VM by doing the following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. <span data-ttu-id="51894-232">Proveďte plánované převzetí služeb při selhání plánu obnovení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="51894-232">Perform a planned failover of a recovery plan by doing the following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="51894-233">Spustit neplánované převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="51894-233">Run an unplanned failover</span></span>
1. <span data-ttu-id="51894-234">Pomocí následujícího postupu proveďte neplánované převzetí služeb při selhání virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="51894-234">Perform an unplanned failover of a VM by doing the following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

<span data-ttu-id="51894-235">2. proveďte pomocí následujícího postupu neplánované převzetí služeb při selhání plánu obnovení:</span><span class="sxs-lookup"><span data-stu-id="51894-235">2.Perform an unplanned failover of a recovery plan by doing the following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <span data-ttu-id="51894-236"><a name=monitor></a>Sledování aktivity</span><span class="sxs-lookup"><span data-stu-id="51894-236"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="51894-237">Použijte následující příkazy k monitorování aktivit.</span><span class="sxs-lookup"><span data-stu-id="51894-237">Use the following commands to monitor the activity.</span></span> <span data-ttu-id="51894-238">Všimněte si, že máte čekat mezi úlohy pro zpracování ukončíte.</span><span class="sxs-lookup"><span data-stu-id="51894-238">Note that you have to wait in between jobs for the processing to finish.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="51894-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51894-239">Next steps</span></span>
<span data-ttu-id="51894-240">[Další informace](/powershell/module/azurerm.recoveryservices.backup/#recovery) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="51894-240">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
