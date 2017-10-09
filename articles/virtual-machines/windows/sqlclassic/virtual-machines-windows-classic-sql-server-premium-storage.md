---
title: aaaUse Azure Premium Storage s SQL serverem | Microsoft Docs
description: "Tento článek používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a poskytuje pokyny k používání Azure Premium Storage s SQL Server běžící na virtuálních počítačích Azure."
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 393ea2020b39ea686302ae632e1049935c24af00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a><span data-ttu-id="26764-103">Použití Azure Premium Storage s SQL Serverem na virtuálních počítačích</span><span class="sxs-lookup"><span data-stu-id="26764-103">Use Azure Premium Storage with SQL Server on Virtual Machines</span></span>
## <a name="overview"></a><span data-ttu-id="26764-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="26764-104">Overview</span></span>
<span data-ttu-id="26764-105">[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) je hello nové generace úložiště, které zajišťuje nízkou latencí a vysokou propustnost vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="26764-105">[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) is hello next generation of storage that provides low latency and high throughput IO.</span></span> <span data-ttu-id="26764-106">Je nejvhodnější pro klíče vstupně-výstupní operace náročné úlohy, jako je SQL Server na IaaS [virtuální počítače](https://azure.microsoft.com/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="26764-106">It works best for key IO intensive workloads, such as SQL Server on IaaS [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="26764-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="26764-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="26764-108">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="26764-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="26764-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="26764-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="26764-110">Tento článek obsahuje plánování a pokyny k migraci virtuálního počítače se systémem SQL Server toouse Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-110">This article provides planning and guidance for migrating a Virtual Machine running SQL Server toouse Premium Storage.</span></span> <span data-ttu-id="26764-111">To zahrnuje infrastrukturu Azure (sítě, úložiště) a hostovaný virtuální počítač s Windows kroky.</span><span class="sxs-lookup"><span data-stu-id="26764-111">This includes Azure infrastructure (networking, storage) and guest Windows VM steps.</span></span> <span data-ttu-id="26764-112">Příklad Hello v hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) ukazuje migraci za úplné komplexní end tooend jak toomove větší virtuální počítače tootake výhod vylepšené místní úložiště SSD pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="26764-112">hello example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) shows a full comprehensive end tooend migration of how toomove larger VMs tootake advantage of improved local SSD storage with PowerShell.</span></span>

<span data-ttu-id="26764-113">Je důležité toounderstand hello začátku do konce proces využitím Azure Premium Storage s SQL serverem na virtuální počítače IAAS.</span><span class="sxs-lookup"><span data-stu-id="26764-113">It is important toounderstand hello end-to-end process of utilizing Azure Premium Storage with SQL Server on IAAS VMs.</span></span> <span data-ttu-id="26764-114">To zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="26764-114">This includes:</span></span>

* <span data-ttu-id="26764-115">Identifikace hello požadavky toouse Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-115">Identification of hello prerequisites toouse Premium Storage.</span></span>
* <span data-ttu-id="26764-116">Příklady nasazení systému SQL Server na IaaS tooPremium úložiště pro nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="26764-116">Examples of deploying SQL Server on IaaS tooPremium Storage for new deployments.</span></span>
* <span data-ttu-id="26764-117">Příklady migraci existujícího nasazení samostatných serverů a nasazení pomocí SQL skupin dostupnosti Always On.</span><span class="sxs-lookup"><span data-stu-id="26764-117">Examples of migrating existing deployments, both stand-alone servers and deployments using SQL Always On Availability Groups.</span></span>
* <span data-ttu-id="26764-118">Migrace možných přístupů.</span><span class="sxs-lookup"><span data-stu-id="26764-118">Possible migration approaches.</span></span>
* <span data-ttu-id="26764-119">Příklad úplného začátku do konce zobrazující Azure, Windows a SQL Server kroky pro migraci hello stávající implementaci Always On.</span><span class="sxs-lookup"><span data-stu-id="26764-119">Full end-to-end example showing Azure, Windows, and SQL Server steps for hello migration of an existing Always On implementation.</span></span>

<span data-ttu-id="26764-120">Další informace pozadí na serveru SQL Server v Azure Virtual Machines najdete v tématu [systému SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="26764-120">For more background information on SQL Server in Azure Virtual Machines, see [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

<span data-ttu-id="26764-121">**Autor:** ADAM Sol **odborní recenzenti:** Leoš Carlos Vargas sleďů, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span><span class="sxs-lookup"><span data-stu-id="26764-121">**Author:** Daniel Sol **Technical Reviewers:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span></span>

## <a name="prerequisites-for-premium-storage"></a><span data-ttu-id="26764-122">Předpoklady pro Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="26764-122">Prerequisites for Premium Storage</span></span>
<span data-ttu-id="26764-123">Existuje několik předpokladů pro použití služby Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-123">There are several prerequisites for using Premium Storage.</span></span>

### <a name="machine-size"></a><span data-ttu-id="26764-124">Velikost počítače</span><span class="sxs-lookup"><span data-stu-id="26764-124">Machine size</span></span>
<span data-ttu-id="26764-125">Pro použití služby Premium Storage budete potřebovat toouse DS řady virtuální počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="26764-125">For using Premium Storage you will need toouse DS series Virtual Machines (VM).</span></span> <span data-ttu-id="26764-126">Pokud jste nepoužili řady DS počítače v cloudové službě před, musíte odstranit hello existující virtuální počítač, zachovat hello připojené disky a pak vytvořte novou cloudovou službu před opětovným vytvořením hello virtuální počítač jako velikost role DS *.</span><span class="sxs-lookup"><span data-stu-id="26764-126">If you have not used DS Series machines in your cloud service before, you must delete hello existing VM, keep hello attached disks, and then create a new cloud service before recreating hello VM as DS* role size.</span></span> <span data-ttu-id="26764-127">Další informace o velikosti virtuálních počítačů najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="26764-127">For more information on Virtual Machine sizes, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="cloud-services"></a><span data-ttu-id="26764-128">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="26764-128">Cloud services</span></span>
<span data-ttu-id="26764-129">Při jejich vytváření v novou cloudovou službu, můžete použít pouze virtuální počítače DS * Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-129">You can only use DS* VMs with Premium Storage when they are created in a new cloud service.</span></span> <span data-ttu-id="26764-130">Pokud používáte SQL serveru Always On v Azure, bude odkazovat hello vždy na naslouchací proces adresu toohello Azure interní nebo externí IP nástroje pro vyrovnávání zatížení, která je přidružená ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="26764-130">If you are using SQL Server Always On in Azure, hello Always On Listener will refer toohello Azure Internal or External Load Balancer IP address that is associated with a cloud service.</span></span> <span data-ttu-id="26764-131">Tento článek se zaměřuje na toomigrate při zachování dostupnosti v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="26764-131">This article focuses on how toomigrate while maintaining availability in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="26764-132">Musí být první virtuální počítač, který je nasazený toohello hello řady DS * novou Cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="26764-132">A DS* Series must be hello first VM that is deployed toohello new Cloud Service.</span></span>
>
>

### <a name="regional-vnets"></a><span data-ttu-id="26764-133">Virtuální místní sítě</span><span class="sxs-lookup"><span data-stu-id="26764-133">Regional VNETS</span></span>
<span data-ttu-id="26764-134">Pro virtuální počítače DS * je nutné nakonfigurovat hello virtuální síť (VNET) hostování vaší místní toobe virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="26764-134">For DS* VMs you must configure hello Virtual Network (VNET) hosting your VMs toobe regional.</span></span> <span data-ttu-id="26764-135">Tento "rozšiřuje" hello virtuální sítě je tooallow hello větší virtuální počítače toobe zřízené v jiné clustery a povolit komunikaci mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="26764-135">This “widens” hello VNET is tooallow hello larger VMs toobe provisioned in other clusters and allow communication between them.</span></span> <span data-ttu-id="26764-136">V hello následující snímek obrazovky hello zvýrazněná umístění ukazuje regionální virtuální sítě, zatímco první výsledek hello ukazuje "úzké" virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="26764-136">In hello following screenshot, hello highlighted Location shows regional VNETs, whereas hello first result shows a “narrow” VNET.</span></span>

![RegionalVNET][1]

<span data-ttu-id="26764-138">Můžete zvýšit tooa toomigrate lístek podpory Microsoft oblastní virtuální síť, Microsoft bude změňte, pak toocomplete hello migrace tooregional virtuálních sítí, změňte vlastnost hello AffinityGroup v konfiguraci sítě hello.</span><span class="sxs-lookup"><span data-stu-id="26764-138">You can raise a Microsoft support ticket toomigrate tooa regional VNET, Microsoft will make a change, then toocomplete hello migration tooregional VNETs, change hello property AffinityGroup in hello network configuration.</span></span> <span data-ttu-id="26764-139">Nejprve vyexportovat hello konfigurace sítě v prostředí PowerShell a potom můžete nahradit hello **AffinityGroup** vlastnost hello **VirtualNetworkSite** element s **umístění** Vlastnost.</span><span class="sxs-lookup"><span data-stu-id="26764-139">First export hello Network Configuration in PowerShell, and then replace hello **AffinityGroup** property in hello **VirtualNetworkSite** element with a **Location** property.</span></span> <span data-ttu-id="26764-140">Zadejte `Location = XXXX` kde `XXXX` je oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="26764-140">Specify `Location = XXXX` where `XXXX` is an Azure region.</span></span> <span data-ttu-id="26764-141">Pak importujte hello novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="26764-141">Then import hello new configuration.</span></span>

<span data-ttu-id="26764-142">Například zvažování hello následující konfiguraci virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="26764-142">For example, considering hello following VNET configuration:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

<span data-ttu-id="26764-143">toomove tento tooa oblastní virtuální síť v oblasti západní Evropa, změnit toohello konfigurace hello následující:</span><span class="sxs-lookup"><span data-stu-id="26764-143">toomove this tooa regional VNET in West Europe, change hello configuration toohello following:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a><span data-ttu-id="26764-144">Účty úložiště</span><span class="sxs-lookup"><span data-stu-id="26764-144">Storage accounts</span></span>
<span data-ttu-id="26764-145">Budete potřebovat toocreate nový účet úložiště, který je nakonfigurován pro Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-145">You will need toocreate a new storage account that is configured for Premium Storage.</span></span> <span data-ttu-id="26764-146">Všimněte si, že použití hello úložiště Premium Storage je nastavený na účet úložiště hello, nikoli na jednotlivé virtuální pevné disky, ale při použití Virtuálního řady DS * z účtů Premium a Standard Storage můžete připojit virtuální pevný disk na.</span><span class="sxs-lookup"><span data-stu-id="26764-146">Note that hello use of Premium Storage is set at hello storage account, not on individual VHDs, however when using a DS* Series VM you can attach VHD’s from Premium and Standard Storage accounts.</span></span> <span data-ttu-id="26764-147">To může zvažte, pokud nechcete, aby tooplace hello virtuálního pevného disku operačního systému na toohello účet služby Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-147">You may consider this if you do not want tooplace hello OS VHD on toohello Premium Storage account.</span></span>

<span data-ttu-id="26764-148">Následující Hello **New-AzureStorageAccountPowerShell** s hello "Premium_LRS" **typ** vytvoří účet úložiště Premium:</span><span class="sxs-lookup"><span data-stu-id="26764-148">hello following **New-AzureStorageAccountPowerShell** command with hello "Premium_LRS" **Type** creates a Premium Storage Account:</span></span>

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a><span data-ttu-id="26764-149">Nastavení mezipaměti virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="26764-149">VHDs Cache Settings</span></span>
<span data-ttu-id="26764-150">Hello hlavní rozdíl mezi vytváření disků, které jsou součástí účtu úložiště Premium je nastavení mezipaměti hello disku.</span><span class="sxs-lookup"><span data-stu-id="26764-150">hello main difference between creating disks that are part of a Premium Storage account is hello disk cache setting.</span></span> <span data-ttu-id="26764-151">Pro svazek dat systému SQL Server disků se doporučuje použít '**ukládání do mezipaměti pro čtení**'.</span><span class="sxs-lookup"><span data-stu-id="26764-151">For SQL Server Data volume disks it is recommended that you use ‘**Read Caching**’.</span></span> <span data-ttu-id="26764-152">Pro svazky protokolu transakcí, hello disku mezipaměti nastavení by mělo být příliš '**žádné**'.</span><span class="sxs-lookup"><span data-stu-id="26764-152">For Transaction log volumes, hello disk cache setting should be set too‘**None**’.</span></span> <span data-ttu-id="26764-153">To se liší od hello doporučení pro účty úložiště Standard Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-153">This is different from hello recommendations for Standard Storage accounts.</span></span>

<span data-ttu-id="26764-154">Jakmile byly připojeny hello virtuální pevné disky, nelze změnit nastavení mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="26764-154">Once hello VHDs have been attached, hello cache setting cannot be altered.</span></span> <span data-ttu-id="26764-155">By třeba toodetach a připojte hello virtuální pevný disk s nastavením aktualizované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="26764-155">You would need toodetach and reattach hello VHD with an updated cache setting.</span></span>

### <a name="windows-storage-spaces"></a><span data-ttu-id="26764-156">Prostory úložiště ve Windows</span><span class="sxs-lookup"><span data-stu-id="26764-156">Windows storage spaces</span></span>
<span data-ttu-id="26764-157">Můžete použít [prostory úložiště ve Windows](https://technet.microsoft.com/library/hh831739.aspx) jako u předchozí standardního úložiště, bude možné toomigrate virtuální počítač, který je již využívá prostory úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-157">You can use [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) as you did with previous Standard Storage, this will allow you toomigrate a VM that is already utilizing Storage Spaces.</span></span> <span data-ttu-id="26764-158">Příklad Hello v [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (krok 9 a předat dál) ukazuje hello tooextract kódu prostředí Powershell a import virtuálního počítače s více připojených virtuálních pevných disků.</span><span class="sxs-lookup"><span data-stu-id="26764-158">hello example in [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (step 9 and forward) demonstrates hello Powershell code tooextract and import a VM with multiple attached VHDs.</span></span>

<span data-ttu-id="26764-159">Fondy úložiště používaly s propustností tooenhance účet úložiště Standard Azure a snižování latence.</span><span class="sxs-lookup"><span data-stu-id="26764-159">Storage Pools were used with Standard Azure storage account tooenhance throughput and reduce latency.</span></span> <span data-ttu-id="26764-160">Je možné, hodnota při testování fondy úložiště Storage úrovně Premium pro nová nasazení, ale zvyšují zvýšenou složitostí při instalaci úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-160">You might find value in testing Storage Pools with Premium Storage for new deployments, but they do add additional complexity with storage setup.</span></span>

#### <a name="how-toofind-which-azure-virtual-disks-map-toostorage-pools"></a><span data-ttu-id="26764-161">Jak toofind které virtuální disky Azure mapování toostorage fondy</span><span class="sxs-lookup"><span data-stu-id="26764-161">How toofind which Azure Virtual Disks map toostorage pools</span></span>
<span data-ttu-id="26764-162">Jako existují různé mezipaměti nastavení doporučení pro virtuální pevné disky připojené, můžete se rozhodnout toocopy hello virtuální pevné disky tooa účet služby Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-162">As there are different cache setting recommendations for attached VHDs, you might decide toocopy hello VHDs tooa Premium Storage account.</span></span> <span data-ttu-id="26764-163">Pokud jste je připojte toohello novou DS řadu virtuálních počítačů, však může být zapotřebí nastavení mezipaměti tooalter hello.</span><span class="sxs-lookup"><span data-stu-id="26764-163">However, when you reattach them toohello new DS series VM, you might need tooalter hello cache settings.</span></span> <span data-ttu-id="26764-164">Je jednodušší tooapply hello Storage úrovně Premium doporučené nastavení mezipaměti, pokud máte samostatný virtuální pevné disky pro hello dat SQL soubory a protokolu souborů (spíše než jeden virtuální pevný disk obsahující oba).</span><span class="sxs-lookup"><span data-stu-id="26764-164">It is simpler tooapply hello Premium Storage recommended cache settings when you have separate VHDs for hello SQL Data files and log files (rather than a single VHD that contains both).</span></span>

> [!NOTE]
> <span data-ttu-id="26764-165">Pokud máte soubory protokolu a data systému SQL Server na hello stejný svazek, hello ukládání do mezipaměti možnost, kterou zvolíte, závisí na hello vzory přístupu k vstupně-výstupní operace pro zatížení databáze.</span><span class="sxs-lookup"><span data-stu-id="26764-165">If you have SQL Server data and log files on hello same volume, hello caching option you choose depends on hello IO access patterns for your database workloads.</span></span> <span data-ttu-id="26764-166">Pouze testování můžete ukazují, které možnost ukládání do mezipaměti je nejvhodnější pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="26764-166">Only testing can demonstrate which caching option is best for this scenario.</span></span>
>
>

<span data-ttu-id="26764-167">Pokud používáte prostory úložiště systému Windows, které se skládají z více virtuálních pevných disků, budete potřebovat toolook na vaše původní tooidentify skripty, které připojit virtuální pevné disky jsou však v jaké konkrétní fondu, takže pak můžete nastavit nastavení mezipaměti hello odpovídajícím způsobem pro každý disk.</span><span class="sxs-lookup"><span data-stu-id="26764-167">However, if you are using Windows Storage Spaces which are made up of multiple VHDs you will need toolook at your original scripts tooidentify which attached VHDs are in what specific pool, so you can then set hello cache settings accordingly for each disk.</span></span>

<span data-ttu-id="26764-168">Pokud nemáte původní skript k dispozici tooshow jste, které virtuální pevné disky mapování toohello fondu úložiště, můžete použít následující kroky toodetermine hello disku/úložiště fondu mapování hello.</span><span class="sxs-lookup"><span data-stu-id="26764-168">If you do not have original script available tooshow you which VHDs map toohello storage pool, you can use hello following steps toodetermine hello disk/storage pool mapping.</span></span>

<span data-ttu-id="26764-169">Pro každý disk použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="26764-169">For each disk, use hello following steps:</span></span>

1. <span data-ttu-id="26764-170">Získání seznamu disky připojené tooVM s hello **Get-AzureVM** příkaz:</span><span class="sxs-lookup"><span data-stu-id="26764-170">Get list of disks attached tooVM with hello **Get-AzureVM** command:</span></span>

    <span data-ttu-id="26764-171">Get-AzureVM - ServiceName <servicename> -název <vmname> | Get-AzureDataDisk</span><span class="sxs-lookup"><span data-stu-id="26764-171">Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk</span></span>
2. <span data-ttu-id="26764-172">Poznámka: hello Diskname a logické jednotky.</span><span class="sxs-lookup"><span data-stu-id="26764-172">Note hello Diskname and LUN.</span></span>

    ![DisknameAndLUN][2]
3. <span data-ttu-id="26764-174">Vzdálená plocha do hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="26764-174">Remote desktop into hello VM.</span></span> <span data-ttu-id="26764-175">Potom přejděte příliš**Správa počítače** | **Správce zařízení** | **diskové jednotky**.</span><span class="sxs-lookup"><span data-stu-id="26764-175">Then go too**Computer Management** | **Device Manager** | **Disk Drives**.</span></span> <span data-ttu-id="26764-176">Podívejte se na vlastnosti hello jednotlivých hello Microsoft virtuálních disků</span><span class="sxs-lookup"><span data-stu-id="26764-176">Look at hello properties of each of hello ‘Microsoft Virtual Disks’</span></span>

    ![VirtualDiskProperties][3]
4. <span data-ttu-id="26764-178">Zde Hello číslo logické jednotky je referenční toohello číslo logické jednotky, které můžete zadat při připojování toohello hello virtuálního pevného disku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="26764-178">hello LUN number here is a reference toohello LUN number you specify when attaching hello VHD toohello VM.</span></span>
5. <span data-ttu-id="26764-179">Hello Microsoft virtuální Disk najdete toohello **podrobnosti** kartě, pak v hello **vlastnost** seznamu, přejděte příliš**klíč ovladače**.</span><span class="sxs-lookup"><span data-stu-id="26764-179">For hello ‘Microsoft Virtual Disk’ go toohello **Details** tab, then in hello **Property** list, go too**Driver Key**.</span></span> <span data-ttu-id="26764-180">V hello **hodnotu**, Poznámka hello **posun**, což je 0002 v hello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="26764-180">In hello **Value**, note hello **Offset**, which is 0002 in hello following screenshot.</span></span> <span data-ttu-id="26764-181">Hello 0002 označuje hello PhysicalDisk2 hello odkazy fondu úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-181">hello 0002 denotes hello PhysicalDisk2 that hello storage pool references.</span></span>

    ![VirtualDiskPropertyDetails][4]
6. <span data-ttu-id="26764-183">Pro každý fond úložiště výpisu se hello přidružené disky:</span><span class="sxs-lookup"><span data-stu-id="26764-183">For each storage pool, dump out hello associated disks:</span></span>

    <span data-ttu-id="26764-184">Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk</span><span class="sxs-lookup"><span data-stu-id="26764-184">Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk</span></span>

    ![GetStoragePool][5]

<span data-ttu-id="26764-186">Nyní můžete pomocí této informace tooassociate připojit virtuální pevné disky tooPhysical disků ve fondu úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-186">Now you can use this information tooassociate attached VHDs tooPhysical Disks in Storage Pools.</span></span>

<span data-ttu-id="26764-187">Jakmile jsou namapovány disky tooPhysical virtuální pevné disky ve fondech úložiště, můžete odpojit a zkopírujte je přes tooa účet služby Premium Storage, připojte je s mezipamětí správné hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="26764-187">Once you have mapped VHDs tooPhysical Disks in Storage Pools you can then detach and copy them over tooa Premium Storage account, then attach them with hello correct cache setting.</span></span> <span data-ttu-id="26764-188">Podrobnosti najdete příklad hello v hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), kroky 8 až 12.</span><span class="sxs-lookup"><span data-stu-id="26764-188">Please see hello example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steps 8 through 12.</span></span> <span data-ttu-id="26764-189">Tyto kroky ukazují, jak tooextract soubor virtuálního pevného disku virtuálního počítače připojeného disku konfigurace tooa CSV kopírování hello virtuální pevné disky, změnit nastavení mezipaměti konfigurace disku hello a nakonec znovu nasaďte hello virtuálního počítače jako řady DS virtuálních počítačů s všechny hello připojenými disky.</span><span class="sxs-lookup"><span data-stu-id="26764-189">These steps show how tooextract a VM-attached VHD disk configuration tooa CSV file, copy hello VHDs, alter hello disk configuration cache settings, and finally redeploy hello VM as a DS series VM with all hello attached disks.</span></span>

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a><span data-ttu-id="26764-190">Šířka pásma úložiště virtuálních počítačů a propustnost úložiště virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="26764-190">VM storage bandwidth and VHD storage throughput</span></span>
<span data-ttu-id="26764-191">Hello množství výkon úložiště závisí na hello zadaná velikost virtuálního počítače DS * a hello velikosti virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="26764-191">hello amount of storage performance depends on hello DS* VM size specified and hello VHD sizes.</span></span> <span data-ttu-id="26764-192">virtuální počítače Hello mají různé příspěvky pro hello počet virtuálních pevných disků, které lze připojit a hello maximální šířka pásma se podporují (MB/s).</span><span class="sxs-lookup"><span data-stu-id="26764-192">hello VMs have different allowances for hello number of VHDs that can be attached and hello maximum bandwidth they will support (MB/s).</span></span> <span data-ttu-id="26764-193">Čísla hello konkrétní šířky pásma, najdete v části [virtuálního počítače a Cloud velikost služeb pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="26764-193">For hello specific bandwidth numbers, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="26764-194">Při větší velikosti disků jsou dosáhnout vyšší IOPS.</span><span class="sxs-lookup"><span data-stu-id="26764-194">Increased IOPS are achieved with larger disk sizes.</span></span> <span data-ttu-id="26764-195">Byste měli zvážit při úvahách o vaši cestu migrace.</span><span class="sxs-lookup"><span data-stu-id="26764-195">You should consider this when you think about your migration path.</span></span> <span data-ttu-id="26764-196">Podrobnosti najdete [IOPS a typech disků najdete v části hello tabulky](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="26764-196">For details, [see hello table for IOPS and Disk Types](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span></span>

<span data-ttu-id="26764-197">Nakonec zvažte, že virtuální počítače mají jiný disk maximální šířek pásma, které se podporují pro všechny disky připojené.</span><span class="sxs-lookup"><span data-stu-id="26764-197">Finally, consider that VMs have different maximum disk bandwidths they will support for all disks attached.</span></span> <span data-ttu-id="26764-198">Vysoké zátěži může saturate hello disku maximální šířka pásma pro velikost role tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="26764-198">Under high load, you could saturate hello maximum disk bandwidth available for that VM role size.</span></span> <span data-ttu-id="26764-199">Například Standard_DS14 bude podporovat až too512MB/s; Proto může s tři disky P30 saturate hello disku šířka pásma hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="26764-199">For example a Standard_DS14 will support up too512MB/s; therefore, with three P30 disks you could saturate hello disk bandwidth of hello VM.</span></span> <span data-ttu-id="26764-200">Ale v tomto příkladu může být překročen limit hello propustnost v závislosti na hello směs ke čtení a zápisu IOs.</span><span class="sxs-lookup"><span data-stu-id="26764-200">But in this example, hello throughput limit could be exceeded depending on hello mix of read and write IOs.</span></span>

## <a name="new-deployments"></a><span data-ttu-id="26764-201">Nová nasazení</span><span class="sxs-lookup"><span data-stu-id="26764-201">New deployments</span></span>
<span data-ttu-id="26764-202">Hello následujících dvou částech ukazují, jak můžete nasadit virtuální počítače serveru SQL tooPremium úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-202">hello next two sections demonstrate how you can deploy SQL Server VMs tooPremium Storage.</span></span> <span data-ttu-id="26764-203">Jak je uvedeno nahoře, není nutné nutně tooplace hello disk operačního systému do úložiště úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-203">As mentioned before, you do not necessarily need tooplace hello OS disk onto Premium storage.</span></span> <span data-ttu-id="26764-204">Můžete zvolit, toodo to pokud jste se záměrné tooplace žádné zatížení s intenzivním vstupně-výstupních operací na hello virtuálního pevného disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="26764-204">You might choose toodo this if you are intending tooplace any intensive IO workloads on hello OS VHD.</span></span>

<span data-ttu-id="26764-205">Hello první příklad ukazuje, využívá existující obrázky z Galerie Azure.</span><span class="sxs-lookup"><span data-stu-id="26764-205">hello first example demonstrates utilizing existing Azure Gallery Images.</span></span> <span data-ttu-id="26764-206">Hello druhý příklad ukazuje, jak toouse vlastní virtuální bitové kopie, kterou máte ve stávající účet úložiště Standard storage.</span><span class="sxs-lookup"><span data-stu-id="26764-206">hello second example shows how toouse a custom VM image that you have in an existing Standard storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="26764-207">Tyto příklady předpokládejme, že jste již vytvořili oblastní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="26764-207">These examples assume that you have already created a Regional VNET.</span></span>
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a><span data-ttu-id="26764-208">Vytvořit nový virtuální počítač s Storage úrovně Premium s bitovou kopií Galerie</span><span class="sxs-lookup"><span data-stu-id="26764-208">Create a new VM with Premium Storage with Gallery Image</span></span>
<span data-ttu-id="26764-209">Následující příklad Hello ukazuje, jak tooplace hello virtuálního pevného disku operačního systému do úložiště úrovně premium a připojte virtuální pevné disky úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-209">hello example below shows how tooplace hello OS VHD onto premium storage and attach Premium Storage VHDs.</span></span> <span data-ttu-id="26764-210">Můžete však také umístit hello disk operačního systému v standardní účet úložiště a pak připojte virtuální pevné disky, které jsou umístěny v účtu Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-210">However, you can also place hello OS disk in a Standard Storage account and then attach VHDs that reside in a Premium Storage account.</span></span> <span data-ttu-id="26764-211">Je ukázán oba scénáře.</span><span class="sxs-lookup"><span data-stu-id="26764-211">Both scenarios are demonstrated.</span></span>

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a><span data-ttu-id="26764-212">Krok 1: Vytvoření účtu služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="26764-212">Step 1: Create a Premium Storage Account</span></span>
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a><span data-ttu-id="26764-213">Krok 2: Vytvořte novou Cloudovou službu</span><span class="sxs-lookup"><span data-stu-id="26764-213">Step 2: Create a New Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a><span data-ttu-id="26764-214">Krok 3: Rezervovat VIP cloudové služby (volitelné)</span><span class="sxs-lookup"><span data-stu-id="26764-214">Step 3: Reserve a Cloud Service VIP (Optional)</span></span>
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a><span data-ttu-id="26764-215">Krok 4: Vytvořte kontejner virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="26764-215">Step 4: Create a VM Container</span></span>
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a><span data-ttu-id="26764-216">Krok 5: Umístění virtuálního pevného disku operačního systému na Standard nebo Premium Storage</span><span class="sxs-lookup"><span data-stu-id="26764-216">Step 5: Placing OS VHD on Standard or Premium Storage</span></span>
    #NOTE: Set up subscription and default storage account which will be used tooplace hello OS VHD in

    #If you want tooplace hello OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted tooplace hello OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a><span data-ttu-id="26764-217">Krok 6: Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="26764-217">Step 6: Create VM</span></span>
    #Get list of available SQL Server Images from hello Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember toochange tooDS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks tooVM Config
    #Note hello size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising hello Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-toouse-premium-storage-with-a-custom-image"></a><span data-ttu-id="26764-218">Vytvoření nového virtuálního počítače toouse Premium Storage pomocí vlastní image</span><span class="sxs-lookup"><span data-stu-id="26764-218">Create a new VM toouse Premium Storage with a custom image</span></span>
<span data-ttu-id="26764-219">Tento scénář předvádí, kdy máte existující přizpůsobené bitové kopie, které jsou umístěny ve standardní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-219">This scenario demonstrates where you have existing customized images that reside in a Standard Storage account.</span></span> <span data-ttu-id="26764-220">Jak je uvedeno, pokud chcete tooplace hello virtuálního pevného disku operačního systému na Storage úrovně Premium, budete potřebovat toocopy hello bitovou kopii, která existuje v hello standardní účet úložiště a je před použitím přenos tooa Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-220">As mentioned if you want tooplace hello OS VHD on Premium Storage you will need toocopy hello image that exists in hello Standard Storage account and transfer them tooa Premium Storage before it can be used.</span></span> <span data-ttu-id="26764-221">Pokud máte bitovou kopii na místě, můžete také použít tuto metodu toocopy přímo toohello účet služby Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-221">If you have an image on-premises, you can also use this method toocopy that directly toohello Premium Storage account.</span></span>

#### <a name="step-1-create-storage-account"></a><span data-ttu-id="26764-222">Krok 1: Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="26764-222">Step 1: Create Storage Account</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a><span data-ttu-id="26764-223">Krok 2 Vytvoření cloudové služby</span><span class="sxs-lookup"><span data-stu-id="26764-223">Step 2 Create Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a><span data-ttu-id="26764-224">Krok 3: Použití existující bitová kopie</span><span class="sxs-lookup"><span data-stu-id="26764-224">Step 3: Use existing image</span></span>
<span data-ttu-id="26764-225">Můžete použít stávající image.</span><span class="sxs-lookup"><span data-stu-id="26764-225">You can use an existing image.</span></span> <span data-ttu-id="26764-226">Nebo můžete [trvat image existujícího počítače](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="26764-226">Or, you can [take an image of an existing machine](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="26764-227">Poznámka: hello počítači, na kterém je obrázek nemá toobe DS * počítače.</span><span class="sxs-lookup"><span data-stu-id="26764-227">Note hello machine you image does not have toobe DS* machine.</span></span> <span data-ttu-id="26764-228">Jakmile máte hello image, hello následující kroky zobrazit jak toocopy ho toohello účet úložiště Premium se hello **Start-AzureStorageBlobCopy** prostředí PowerShell..</span><span class="sxs-lookup"><span data-stu-id="26764-228">Once you have hello image, hello following steps show how toocopy it toohello Premium Storage account with hello **Start-AzureStorageBlobCopy** PowerShell commandlet.</span></span>

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for hello storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a><span data-ttu-id="26764-229">Krok 4: Kopírování objektu Blob mezi účty úložiště</span><span class="sxs-lookup"><span data-stu-id="26764-229">Step 4: Copy Blob between Storage Accounts</span></span>
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a><span data-ttu-id="26764-230">Krok 5: Pravidelné kontroly stavu kopie:</span><span class="sxs-lookup"><span data-stu-id="26764-230">Step 5: Regularly check copy status:</span></span>
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-tooazure-disk-repository-in-subscription"></a><span data-ttu-id="26764-231">Krok 6: Přidejte bitové kopie disku tooAzure úložiště v předplatném</span><span class="sxs-lookup"><span data-stu-id="26764-231">Step 6: Add Image disk tooAzure disk Repository in Subscription</span></span>
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> <span data-ttu-id="26764-232">Můžete zjistit, že i když jako úspěšné sestavy stavu hello, může stále dostanete zapůjčení chyba disku.</span><span class="sxs-lookup"><span data-stu-id="26764-232">You may find that even though hello status reports as success, you could still get a disk lease error.</span></span> <span data-ttu-id="26764-233">V takovém případě Počkejte přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="26764-233">In this case, wait about 10 minutes.</span></span>
>
>

#### <a name="step-7--build-hello-vm"></a><span data-ttu-id="26764-234">Krok 7: Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="26764-234">Step 7:  Build hello VM</span></span>
<span data-ttu-id="26764-235">Tady jsou sestavování hello virtuálního počítače z image a připojení dvě disků VHD úložiště Premium:</span><span class="sxs-lookup"><span data-stu-id="26764-235">Here you are building hello VM from your image and attaching two Premium Storage VHDs:</span></span>

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need toobe a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use tooDS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a><span data-ttu-id="26764-236">Existující nasazení, které nepoužívají skupin dostupnosti Always On</span><span class="sxs-lookup"><span data-stu-id="26764-236">Existing deployments that do not use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="26764-237">Existující nasazení, nejprve najdete v části hello [požadavky](#prerequisites-for-premium-storage) části tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="26764-237">For existing deployments, first see hello [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="26764-238">Existují různé aspekty pro nasazení systému SQL Server, které nepoužívají skupin dostupnosti Always On a ty, které provádějí.</span><span class="sxs-lookup"><span data-stu-id="26764-238">There are different considerations for SQL Server deployments that do not use Always On Availability Groups and those that do.</span></span> <span data-ttu-id="26764-239">Pokud nepoužíváte Always On a máte existující samostatný systém SQL Server, můžete upgradovat tooPremium úložiště pomocí nového účtu služby a úložiště cloudu.</span><span class="sxs-lookup"><span data-stu-id="26764-239">If you are not using Always On and have an existing standalone SQL Server, you can upgrade tooPremium Storage by using a new cloud service and storage account.</span></span> <span data-ttu-id="26764-240">Vezměte v úvahu hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="26764-240">Consider hello following options:</span></span>

* <span data-ttu-id="26764-241">**Vytvoření nového virtuálního počítače SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="26764-241">**Create a new SQL Server VM**.</span></span> <span data-ttu-id="26764-242">Můžete vytvořit nový virtuální počítač SQL Server, který používá účet úložiště Premium, jak je uvedeno v nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="26764-242">You can create a new SQL Server VM that uses a Premium Storage account, as documented in New Deployments.</span></span> <span data-ttu-id="26764-243">Potom zálohování a obnovení konfigurace a uživatele databáze SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26764-243">Then backup and restore your SQL Server configuration and user databases.</span></span> <span data-ttu-id="26764-244">Hello aplikace bude nutné aktualizovat toobe tooreference hello nový Server SQL, pokud je právě přístupu interně nebo externě.</span><span class="sxs-lookup"><span data-stu-id="26764-244">hello application will need toobe updated tooreference hello new SQL Server if it is being accessed internally or externally.</span></span> <span data-ttu-id="26764-245">Potřebovali byste toocopy všechny objekty 'z databáze, jako když jste dělali migrace systému SQL Server (SxS) vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="26764-245">You would need toocopy all ‘out of db’ objects as if you were doing a Side by Side (SxS) SQL Server migration.</span></span> <span data-ttu-id="26764-246">To zahrnuje objekty, jako je například přihlášení, certifikáty a propojené servery.</span><span class="sxs-lookup"><span data-stu-id="26764-246">This includes objects such as logins, certificates, and linked servers.</span></span>
* <span data-ttu-id="26764-247">**Migrovat existující virtuální počítač serveru SQL**.</span><span class="sxs-lookup"><span data-stu-id="26764-247">**Migrate an existing SQL Server VM**.</span></span> <span data-ttu-id="26764-248">To bude vyžadovat převedení hello virtuální počítač SQL Server do režimu offline a pak ho přenosu tooa novou cloudovou službu, která zahrnuje kopírování všech jeho připojené virtuální pevné disky toohello účet služby Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-248">This will require taking hello SQL Server VM offline, then transferring it tooa new cloud service, which includes copying all of its attached VHDs toohello Premium Storage account.</span></span> <span data-ttu-id="26764-249">Při přechodu do režimu online hello virtuálních počítačů, bude aplikace hello odkazovat hello název serveru hostitele jako před.</span><span class="sxs-lookup"><span data-stu-id="26764-249">When hello VM comes online, hello application will reference hello server host name as before.</span></span> <span data-ttu-id="26764-250">Upozorňujeme, že velikost hello stávající disk hello ovlivní hello výkonové charakteristiky.</span><span class="sxs-lookup"><span data-stu-id="26764-250">Be aware that hello size of hello existing disk will affect hello performance characteristics.</span></span> <span data-ttu-id="26764-251">Například 400 GB místa na disku získá zaokrouhlit tooa P20.</span><span class="sxs-lookup"><span data-stu-id="26764-251">For example, a 400 GB disk gets rounded up tooa P20.</span></span> <span data-ttu-id="26764-252">Pokud víte, že výkon disku nevyžadují, může znovu vytvořte virtuální počítač jako virtuální počítač řady DS hello a připojte virtuální pevné disky úložiště Premium hello specifikace velikosti a výkonu, kterou požadujete.</span><span class="sxs-lookup"><span data-stu-id="26764-252">If you know that you do not require that disk performance, then you could recreate hello VM as a DS Series VM, and attach Premium Storage VHDs of hello size/performance specification you require.</span></span> <span data-ttu-id="26764-253">Pak můžete odpojit a připojte hello soubory databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="26764-253">Then you could detach and reattach hello SQL DB files.</span></span>

> [!NOTE]
> <span data-ttu-id="26764-254">Při kopírování disků VHD hello byste měli vědět o velikosti hello, v závislosti na velikosti hello bude znamenat, jaký typ disku úložiště Premium se spadají do, tato hodnota určuje specifikace výkon disku.</span><span class="sxs-lookup"><span data-stu-id="26764-254">When copying hello VHD disks you should be aware of hello size, depending on hello size will mean what Premium Storage Disk type they fall into, this determines disk performance specification.</span></span> <span data-ttu-id="26764-255">Velikost Azure se zaokrouhlí nahoru toohello nejbližší disku, takže pokud máte 400 GB místa na disku, to bude zaokrouhlený nahoru tooa P20.</span><span class="sxs-lookup"><span data-stu-id="26764-255">Azure will round up toohello nearest disk size, so if you have a 400GB disk, this will be rounded up tooa P20.</span></span> <span data-ttu-id="26764-256">V závislosti na vaší stávající vstupně-výstupní požadavky hello virtuálního pevného disku operačního systému nemusí potřebovat toomigrate tento tooa účet služby Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="26764-256">Depending on your existing IO requirements of hello OS VHD, you might not need toomigrate this tooa Premium Storage account.</span></span>
>
>

<span data-ttu-id="26764-257">Pokud je externě přístup k systému SQL Server, se změní hello cloudové služby virtuálních IP adres.</span><span class="sxs-lookup"><span data-stu-id="26764-257">If your SQL Server is accessed externally, then hello cloud service VIP will change.</span></span> <span data-ttu-id="26764-258">Bude také nutné tooupdate koncové body, seznamy řízení přístupu a DNS nastavení.</span><span class="sxs-lookup"><span data-stu-id="26764-258">You will also have tooupdate end points, ACLs, and DNS settings.</span></span>

## <a name="existing-deployments-that-use-always-on-availability-groups"></a><span data-ttu-id="26764-259">Existující nasazení, které používají skupiny dostupnosti Always On</span><span class="sxs-lookup"><span data-stu-id="26764-259">Existing deployments that use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="26764-260">Existující nasazení, nejprve najdete v části hello [požadavky](#prerequisites-for-premium-storage) části tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="26764-260">For existing deployments, first see hello [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="26764-261">Nejprve v této části se podíváme na jak Always On komunikuje s sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="26764-261">Initially in this section we will look at how Always On interacts with Azure Networking.</span></span> <span data-ttu-id="26764-262">Jsme se potom rozčlenit v tootwo scénáře migrace: migrace, kde musí dosáhnout minimální dobou výpadku a migrace, kde lze tolerovat výpadky.</span><span class="sxs-lookup"><span data-stu-id="26764-262">We will then break down migrations in tootwo scenarios: migrations where some downtime can be tolerated and migrations where you must achieve minimal downtime.</span></span>

<span data-ttu-id="26764-263">Použít místní SQL Server skupin dostupnosti Always On naslouchací proces místní které zaregistruje virtuální název DNS společně s IP adresu, která jsou sdílena mezi jeden nebo více serverů SQL.</span><span class="sxs-lookup"><span data-stu-id="26764-263">On-premises SQL Server Always On Availability Groups use a Listener on-premises that registers a virtual DNS name along with an IP address that is shared between one or more SQL Servers.</span></span> <span data-ttu-id="26764-264">Pokud se klienti připojují směrování v hello naslouchací proces IP toohello primární systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26764-264">When clients connect they are routed through hello listener IP toohello Primary SQL Server.</span></span> <span data-ttu-id="26764-265">Toto je hello serveru, který je vlastníkem hello prostředek vždy na IP v daném čase.</span><span class="sxs-lookup"><span data-stu-id="26764-265">This is hello server that owns hello Always On IP resource at that time.</span></span>

![DeploymentsUseAlways na][6]

<span data-ttu-id="26764-267">Ve službě Microsoft Azure může mít pouze jednu adresu přiřazenou tooa IP síťový adaptér na hello virtuálních počítačů, takže v pořadí tooachieve hello stejnou úroveň abstrakce jako místní, Azure využívá hello IP adresu, která je přiřazena toohello interní/externí služby Vyrovnávání zatížení (ILB/ELB).</span><span class="sxs-lookup"><span data-stu-id="26764-267">In Microsoft Azure you can have only one IP address assigned tooa NIC on hello VM, so in order tooachieve hello same layer of abstraction as on-premises, Azure utilizes hello IP address that is assigned toohello Internal/External Load Balancers (ILB/ELB).</span></span> <span data-ttu-id="26764-268">Zdroj Hello IP, jež jsou sdílena mezi servery hello je nastaven toohello stejnou IP Adresou jako hello ILB/ELB.</span><span class="sxs-lookup"><span data-stu-id="26764-268">hello IP resource that is shared between hello servers is set toohello same IP as hello ILB/ELB.</span></span> <span data-ttu-id="26764-269">To je publikován v hello DNS a přenosy klientů je předána hello ILB/ELB toohello primární SQL serveru repliky.</span><span class="sxs-lookup"><span data-stu-id="26764-269">This is published in hello DNS, and client traffic is passed through hello ILB/ELB toohello Primary SQL Server replica.</span></span> <span data-ttu-id="26764-270">Hello ILB/ELB ví, což SQL Server je primární, protože používá sondy tooprobe hello vždy na IP prostředků.</span><span class="sxs-lookup"><span data-stu-id="26764-270">hello ILB/ELB knows which SQL Server is primary since it uses probes tooprobe hello Always On IP resource.</span></span> <span data-ttu-id="26764-271">V předchozím příkladu hello ho sondy každý uzel, který má koncový bod odkazuje hello ELB/ILB, podle toho, která odpovídá je hello primární systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26764-271">In hello previous example, it probes each node that has an endpoint referenced by hello ELB/ILB, whichever responds is hello Primary SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="26764-272">Hello ILB a ELB obě přiřazené tooa konkrétní cloudové služby Azure, proto žádné migrace do cloudu v Azure s největší pravděpodobností znamená, že hello, které změní IP nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="26764-272">hello ILB and ELB are both assigned tooa particular Azure cloud service, therefore any cloud migration in Azure will most likely mean that hello Load Balancer IP will change.</span></span>
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a><span data-ttu-id="26764-273">Migrace vždy na nasazení, která můžete povolit výpadky</span><span class="sxs-lookup"><span data-stu-id="26764-273">Migrating Always On deployments that can allow some downtime</span></span>
<span data-ttu-id="26764-274">Existují dvě strategie toomigrate Always On nasazení, které umožňují výpadky:</span><span class="sxs-lookup"><span data-stu-id="26764-274">There are two strategies toomigrate Always On deployments that allow for some downtime:</span></span>

1. <span data-ttu-id="26764-275">**Přidat další tooan sekundární repliky existující vždy na clusteru**</span><span class="sxs-lookup"><span data-stu-id="26764-275">**Add more secondary replicas tooan existing Always On Cluster**</span></span>
2. <span data-ttu-id="26764-276">**Migrace tooa nového vždy na clusteru**</span><span class="sxs-lookup"><span data-stu-id="26764-276">**Migrate tooa new Always On Cluster**</span></span>

#### <a name="1-add-more-secondary-replicas-tooan-existing-always-on-cluster"></a><span data-ttu-id="26764-277">1. Přidat další tooan sekundární repliky existující vždy na Cluster</span><span class="sxs-lookup"><span data-stu-id="26764-277">1. Add more Secondary Replicas tooan Existing Always On Cluster</span></span>
<span data-ttu-id="26764-278">Jednou z možných strategií je tooadd více sekundárních toohello vždy na skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="26764-278">One strategy is tooadd more secondaries toohello Always On Availability Group.</span></span> <span data-ttu-id="26764-279">Potřebujete tooadd tyto do novou cloudovou službu a aktualizovat hello naslouchací proces s hello nových IP nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="26764-279">You need tooadd these into a new cloud service and update hello listener with hello new load balancer IP.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="26764-280">Body výpadek:</span><span class="sxs-lookup"><span data-stu-id="26764-280">Points of downtime:</span></span>
* <span data-ttu-id="26764-281">Ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="26764-281">Cluster Validation.</span></span>
* <span data-ttu-id="26764-282">Testování vždy na převzetí služeb při selhání pro novou sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="26764-282">Testing Always On failovers for New Secondaries.</span></span>

<span data-ttu-id="26764-283">Pokud používáte fondy úložišť systému Windows v rámci hello virtuálních počítačů pro vyšší propustnost vstupně-výstupní operace, pak tyto režimu budou během úplného ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="26764-283">If you are using Windows Storage Pools within hello VM for higher IO throughput, then these will be taken offline during a Full Cluster Validation.</span></span> <span data-ttu-id="26764-284">Při přidání uzlů toohello clusteru je potřeba Hello ověřovací test.</span><span class="sxs-lookup"><span data-stu-id="26764-284">hello validation test is required when you add nodes toohello cluster.</span></span> <span data-ttu-id="26764-285">Doba trvání testu hello toorun Hello se může měnit, měli byste otestovat to ve vaší reprezentativního testovacího prostředí tooget Přibližná doba o tom, jak dlouho to bude trvat.</span><span class="sxs-lookup"><span data-stu-id="26764-285">hello time it takes toorun hello test can vary, so you should test this in your representative test environment tooget an approximate time of how long this will take.</span></span>

<span data-ttu-id="26764-286">By měl zřídit čas, kde můžete provádět ruční převzetí služeb při selhání a chaos testování v hello nově přidali uzly tooensure vždy na vysokou dostupnost funkcí podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="26764-286">You should provision time where you can perform manual failover and chaos testing on hello newly added nodes tooensure Always On High Availability functions as expected.</span></span>

![DeploymentUseAlways On2][7]

> [!NOTE]
> <span data-ttu-id="26764-288">Všechny instance systému SQL Server, kdy se používá fondy úložiště hello před spuštěním technologie hello ověření by se měla zastavit.</span><span class="sxs-lookup"><span data-stu-id="26764-288">You should stop all instances of SQL Server where hello Storage Pools are used before hello Validation runs.</span></span>
>
> ##### <a name="high-level-steps"></a><span data-ttu-id="26764-289">Postup vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="26764-289">High-level steps</span></span>
>

1. <span data-ttu-id="26764-290">Vytvoření dvou nových serverů SQL v novou cloudovou službu s připojeným úložištěm Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-290">Create two new SQL Servers in new cloud service with attached Premium Storage.</span></span>
2. <span data-ttu-id="26764-291">Zkopírujte přes úplné zálohování a obnovení pomocí **NORECOVERY**.</span><span class="sxs-lookup"><span data-stu-id="26764-291">Copy over FULL backups and restore with **NORECOVERY**.</span></span>
3. <span data-ttu-id="26764-292">Zkopírujte přes "mimo uživatelem DB" závislé objekty, jako je například přihlášení atd.</span><span class="sxs-lookup"><span data-stu-id="26764-292">Copy over ‘out of user DB’ dependent objects, such as logins etc.</span></span>
4. <span data-ttu-id="26764-293">Vytvořit nový nové vyrovnávání pro interní zatížení (ILB) nebo použijte externí zatížení vyrovnávání (ELB) a potom nastavit koncové body skupinu s vyrovnáváním zatížení v obou uzlech nového.</span><span class="sxs-lookup"><span data-stu-id="26764-293">Create new a new Internal Load Balancer (ILB) or use an External Load Balancer (ELB), and then set up Load Balanced Endpoints on both new nodes.</span></span>

   > [!NOTE]
   > <span data-ttu-id="26764-294">Zkontrolujte, jestli že všechny uzly mají hello správné konfigurace koncového bodu, než budete pokračovat</span><span class="sxs-lookup"><span data-stu-id="26764-294">Check all Nodes have hello correct Endpoint configuration before you continue</span></span>
   >
   >
5. <span data-ttu-id="26764-295">Zastavte toohello přístup uživatele nebo aplikace SQL Server (Pokud používáte fondy úložišť).</span><span class="sxs-lookup"><span data-stu-id="26764-295">Stop User/Application Access toohello SQL Server (if using Storage Pools).</span></span>
6. <span data-ttu-id="26764-296">Zastavte služby stroje SQL serveru na všech uzlech (Pokud používáte fondy úložišť).</span><span class="sxs-lookup"><span data-stu-id="26764-296">Stop SQL Server Engine Services on All Nodes (if using Storage Pools).</span></span>
7. <span data-ttu-id="26764-297">Přidejte nové uzly toocluster a spuštění úplného ověření.</span><span class="sxs-lookup"><span data-stu-id="26764-297">Add new Nodes toocluster and run full validation.</span></span>
8. <span data-ttu-id="26764-298">Po úspěšném ověření spusťte všechny služby SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26764-298">Once Validation is successful, start all SQL Server Services.</span></span>
9. <span data-ttu-id="26764-299">Protokoly transakcí zálohování a obnovení uživatelských databází.</span><span class="sxs-lookup"><span data-stu-id="26764-299">Backup Transaction logs, and restore user databases.</span></span>
10. <span data-ttu-id="26764-300">Přidání nových uzlů do hello vždy na skupiny dostupnosti a umístěte replikace do **Synchronous**.</span><span class="sxs-lookup"><span data-stu-id="26764-300">Add new nodes into hello Always On Availability Group and place replication into **Synchronous**.</span></span>
11. <span data-ttu-id="26764-301">Přidat hello IP adresu prostředek hello nové cloudové služby ILB/ELB pomocí prostředí PowerShell pro Always On podle příkladu více lokalit hello hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="26764-301">Add hello IP address resource of hello new Cloud Service ILB/ELB through PowerShell for Always On based on hello Multi-site example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span> <span data-ttu-id="26764-302">V systému vytváření clusterů systému Windows, nastavte hello **Možní vlastníci** z hello **IP adresu** prostředků toohello nové uzly starý.</span><span class="sxs-lookup"><span data-stu-id="26764-302">In Windows clustering, set hello **Possible owners** of hello **IP Address** resource toohello new nodes old.</span></span> <span data-ttu-id="26764-303">V části hello 'Přidání prostředek IP adresy ve stejné podsíti, s hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="26764-303">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
12. <span data-ttu-id="26764-304">Převzetí služeb při selhání tooone hello nové uzly.</span><span class="sxs-lookup"><span data-stu-id="26764-304">Failover tooone of hello new nodes.</span></span>
13. <span data-ttu-id="26764-305">Proveďte hello nové uzly partnery automatické převzetí služeb při selhání a testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-305">Make hello new nodes Auto Failover Partners and test failovers.</span></span>
14. <span data-ttu-id="26764-306">Odeberte původní uzly ze skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="26764-306">Remove original nodes from Availability Group.</span></span>

##### <a name="advantages"></a><span data-ttu-id="26764-307">Výhody</span><span class="sxs-lookup"><span data-stu-id="26764-307">Advantages</span></span>
* <span data-ttu-id="26764-308">Nové servery SQL Server může být testována (SQL Server a aplikace) před přidáním tooAlways na.</span><span class="sxs-lookup"><span data-stu-id="26764-308">New SQL Servers can be tested (SQL Server and Application) before they are added tooAlways On.</span></span>
* <span data-ttu-id="26764-309">Můžete změnit velikost virtuálního počítače hello a přizpůsobit přesných požadavcích tooyour hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-309">You can change hello VM size and customize hello storage tooyour exact requirements.</span></span> <span data-ttu-id="26764-310">Nicméně, je výhodné tookeep všechny cesty k souborům SQL hello hello stejné.</span><span class="sxs-lookup"><span data-stu-id="26764-310">However, it would be beneficial tookeep all hello SQL file paths hello same.</span></span>
* <span data-ttu-id="26764-311">Můžete ovládat, když jsou spuštěny hello přenos hello DB zálohy toohello sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="26764-311">You can control when hello transfer of hello DB backups toohello Secondary Replicas are started.</span></span> <span data-ttu-id="26764-312">Tím se liší od použití Azure **Start-AzureStorageBlobCopy** příkaz toocopy virtuální pevné disky, protože se jedná o asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="26764-312">This differs from using Azure **Start-AzureStorageBlobCopy** commandlet toocopy VHDs, because that is an asynchronous copy.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="26764-313">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="26764-313">Disadvantages</span></span>
* <span data-ttu-id="26764-314">Při použití fondů úložišť systému Windows, je během hello úplného ověření clusteru pro hello nové další uzly clusteru výpadku.</span><span class="sxs-lookup"><span data-stu-id="26764-314">When using Windows Storage Pools, there is Cluster downtime during hello Full Cluster Validation for hello new additional nodes.</span></span>
* <span data-ttu-id="26764-315">V závislosti na hello verze serveru SQL a hello existující počet sekundárních replikách, pravděpodobně není být schopný tooadd více sekundárních replik bez odebrání stávající sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="26764-315">Depending on hello SQL Server Version and hello existing number of secondary replicas, you might not be able tooadd more secondary replicas without removing existing secondaries.</span></span>
* <span data-ttu-id="26764-316">Může dojít při nastavování hello sekundárních dlouhá doba přenosu dat SQL.</span><span class="sxs-lookup"><span data-stu-id="26764-316">There could be long SQL data transfer time while setting up hello secondaries.</span></span>
* <span data-ttu-id="26764-317">Není další náklady během migrace, když máte nové počítače, které běží paralelně.</span><span class="sxs-lookup"><span data-stu-id="26764-317">There is additional cost during migration while you have new machines running in parallel.</span></span>

#### <a name="2-migrate-tooa-new-always-on-cluster"></a><span data-ttu-id="26764-318">2. Migrace tooa nového vždy na clusteru</span><span class="sxs-lookup"><span data-stu-id="26764-318">2. Migrate tooa new Always On Cluster</span></span>
<span data-ttu-id="26764-319">Další strategie je toocreate zcela nový vždy na Cluster s uzly, které zcela nový novou cloudovou službu, a potom přesměrování hello klientům toouse ho.</span><span class="sxs-lookup"><span data-stu-id="26764-319">Another strategy is toocreate a brand new Always On Cluster with brand new nodes in new cloud service and then redirect hello clients toouse it.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="26764-320">Body výpadku</span><span class="sxs-lookup"><span data-stu-id="26764-320">Points of downtime</span></span>
<span data-ttu-id="26764-321">Při přenosu aplikace a uživatelé toohello nové Always On naslouchací proces dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="26764-321">There is downtime when you transfer applications and users toohello new Always On listener.</span></span> <span data-ttu-id="26764-322">výpadek Hello závisí na:</span><span class="sxs-lookup"><span data-stu-id="26764-322">hello downtime depends on:</span></span>

* <span data-ttu-id="26764-323">Hello doba toorestore konečné transakce protokolu zálohování toodatabases na nové servery.</span><span class="sxs-lookup"><span data-stu-id="26764-323">hello time taken toorestore final transaction log backups toodatabases on new servers.</span></span>
* <span data-ttu-id="26764-324">Hello doba tooupdate klienta aplikace toouse nové Always On naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="26764-324">hello time taken tooupdate client applications toouse new Always On listener.</span></span>

##### <a name="advantages"></a><span data-ttu-id="26764-325">Výhody</span><span class="sxs-lookup"><span data-stu-id="26764-325">Advantages</span></span>
* <span data-ttu-id="26764-326">Můžete otestovat hello skutečné produkční prostředí, SQL Server, a změny sestavení operačního systému.</span><span class="sxs-lookup"><span data-stu-id="26764-326">You can test hello actual production environment, SQL Server, and OS build changes.</span></span>
* <span data-ttu-id="26764-327">Máte hello možnost toocustomize hello úložiště a toopotentially zmenšit velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="26764-327">You have hello option toocustomize hello storage and toopotentially reduce size of VM.</span></span> <span data-ttu-id="26764-328">To může mít za následek snížení nákladů.</span><span class="sxs-lookup"><span data-stu-id="26764-328">This could result in cost reduction.</span></span>
* <span data-ttu-id="26764-329">Během tohoto procesu můžete aktualizovat sestavení systému SQL Server nebo verze.</span><span class="sxs-lookup"><span data-stu-id="26764-329">You can update your SQL Server build or version during this process.</span></span> <span data-ttu-id="26764-330">Rovněž lze upgradovat hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="26764-330">You can also upgrade hello Operating System.</span></span>
* <span data-ttu-id="26764-331">Hello předchozí vždy na clusteru může fungovat jako cíl plnou vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="26764-331">hello previous Always On Cluster can act as a solid rollback target.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="26764-332">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="26764-332">Disadvantages</span></span>
* <span data-ttu-id="26764-333">Název DNS hello toochange hello naslouchacího procesu budete potřebovat, pokud chcete, aby obě Always On clustery se systémem současně.</span><span class="sxs-lookup"><span data-stu-id="26764-333">You need toochange hello DNS name of hello listener if you want both Always On clusters running simultaneously.</span></span> <span data-ttu-id="26764-334">Tento postup přidá správy režii během migrace hello jako řetězců aplikace klienta musí odrážet hello nový název naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="26764-334">This adds administration overhead during hello migration as client application strings must reflect hello new Listener name.</span></span>
* <span data-ttu-id="26764-335">Je nutné implementovat synchronizační mechanismus mezi dvěma tookeep prostředí hello je jako zavřete jako možné toominimize hello konečná synchronizace požadavky před migrací.</span><span class="sxs-lookup"><span data-stu-id="26764-335">You must implement a synchronization mechanism between hello two environments tookeep them as close as possible toominimize hello final synchronization requirements before migration.</span></span>
* <span data-ttu-id="26764-336">Během migrace se se přidá náklady na době, kdy máte hello nové prostředí spuštěna.</span><span class="sxs-lookup"><span data-stu-id="26764-336">There is added cost during migration while you have hello new environment running.</span></span>

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a><span data-ttu-id="26764-337">Migrace vždy na nasazení pro minimální dobou výpadku</span><span class="sxs-lookup"><span data-stu-id="26764-337">Migrating Always On Deployments for minimal downtime</span></span>
<span data-ttu-id="26764-338">Pro minimální dobou výpadku existují dvě strategie migrace nasazení Always On:</span><span class="sxs-lookup"><span data-stu-id="26764-338">There are two strategies for migrating Always On deployments for minimal downtime:</span></span>

1. <span data-ttu-id="26764-339">**Využívat stávající sekundární: jedné lokalitě**</span><span class="sxs-lookup"><span data-stu-id="26764-339">**Utilize an Existing Secondary: Single-Site**</span></span>
2. <span data-ttu-id="26764-340">**Využívat stávající sekundární následující: více lokalit**</span><span class="sxs-lookup"><span data-stu-id="26764-340">**Utilize Existing Secondary Replica(s): Multi-Site**</span></span>

#### <a name="1-utilize-an-existing-secondary-single-site"></a><span data-ttu-id="26764-341">1. Využívat stávající sekundární: jedné lokalitě</span><span class="sxs-lookup"><span data-stu-id="26764-341">1. Utilize an existing secondary: Single-Site</span></span>
<span data-ttu-id="26764-342">Jednou z možných strategií pro minimální dobou výpadku je tootake ve stávající sekundární cloudu a odebere ji z hello aktuální cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="26764-342">One strategy for minimal downtime is tootake an existing cloud secondary and remove it from hello current cloud service.</span></span> <span data-ttu-id="26764-343">Pak zkopírujte hello virtuální pevné disky toohello nový účet úložiště Premium a vytvořte hello virtuálních počítačů v hello novou cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="26764-343">Then copy hello VHDs toohello new Premium Storage account, and create hello VM in hello new cloud service.</span></span> <span data-ttu-id="26764-344">Aktualizujte hello naslouchací proces ve clustering a převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-344">Then update hello listener in clustering and failover.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="26764-345">Body výpadku</span><span class="sxs-lookup"><span data-stu-id="26764-345">Points of downtime</span></span>
* <span data-ttu-id="26764-346">Při aktualizaci hello poslední uzel s koncovým bodem s vyrovnáváním zatížení se hello dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="26764-346">There is downtime when you update hello final node with hello Load Balanced endpoint.</span></span>
* <span data-ttu-id="26764-347">Vaše opětovné připojení klienta může být zpoždění v závislosti na konfiguraci klienta a DNS.</span><span class="sxs-lookup"><span data-stu-id="26764-347">Your client reconnection might be delayed depending on your client/DNS configuration.</span></span>
* <span data-ttu-id="26764-348">Pokud se rozhodnete tootake hello vždy na clusteru skupiny offline tooswap out hello IP adresy je další výpadku.</span><span class="sxs-lookup"><span data-stu-id="26764-348">There is additional downtime if you choose tootake hello Always On Cluster group offline tooswap out hello IP addresses.</span></span> <span data-ttu-id="26764-349">To se můžete vyhnout pomocí nebo závislostí a možní vlastníci pro hello přidat prostředek IP adresy.</span><span class="sxs-lookup"><span data-stu-id="26764-349">You can avoid this by using an OR dependency and Possible Owners for hello added IP Address resource.</span></span> <span data-ttu-id="26764-350">V části hello 'Přidání prostředek IP adresy ve stejné podsíti, s hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="26764-350">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>

> [!NOTE]
> <span data-ttu-id="26764-351">Pokud chcete hello toopartake přidané uzlu v jako vždy na převzetí služeb při selhání Partner, je potřeba tooadd koncový bod Azure s odkaz toohello, skupinu s vyrovnáváním zatížení nastavit.</span><span class="sxs-lookup"><span data-stu-id="26764-351">When you want hello added node toopartake in as an Always On Failover Partner, you need tooadd an Azure Endpoint with a reference toohello Load Balanced Set.</span></span> <span data-ttu-id="26764-352">Když spustíte hello **přidat AzureEndpoint** příkaz toodo se aktuální tooremain připojení otevřené, ale nové naslouchání toohello připojení nebude možné toobe navázat, dokud nástroj pro vyrovnávání zatížení hello byl aktualizován.</span><span class="sxs-lookup"><span data-stu-id="26764-352">When you run hello **Add-AzureEndpoint** command toodo this, current connections tooremain open, but new connections toohello listener will not be able toobe established until hello load balancer has updated.</span></span> <span data-ttu-id="26764-353">Při testování šlo zaznamenané toolast 90-120seconds, by měla být testována.</span><span class="sxs-lookup"><span data-stu-id="26764-353">In testing this was seen toolast 90-120seconds, this should be tested.</span></span>
>
>

##### <a name="advantages"></a><span data-ttu-id="26764-354">Výhody</span><span class="sxs-lookup"><span data-stu-id="26764-354">Advantages</span></span>
* <span data-ttu-id="26764-355">Žádné další náklady vzniklé během migrace.</span><span class="sxs-lookup"><span data-stu-id="26764-355">No extra cost incurred during migration.</span></span>
* <span data-ttu-id="26764-356">1: 1 migrace.</span><span class="sxs-lookup"><span data-stu-id="26764-356">A one-to-one migration.</span></span>
* <span data-ttu-id="26764-357">Menší složitost.</span><span class="sxs-lookup"><span data-stu-id="26764-357">Reduced complexity.</span></span>
* <span data-ttu-id="26764-358">Umožňuje vyšší IOPS z SKU úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="26764-358">Allows for increased IOPS from Premium Storage SKUs.</span></span> <span data-ttu-id="26764-359">Pokud hello disky odpojit od hello virtuální počítač a zkopírovat toohello novou cloudovou službu 3. stran nástroj může být velikost virtuálního pevného disku hello použité tooincrease, která poskytuje vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="26764-359">When hello disks are detached from hello VM and copied toohello new cloud service, a 3rd party tool can be used tooincrease hello VHD size, which provides higher throughputs.</span></span> <span data-ttu-id="26764-360">Zvýšení velikosti virtuálního pevného disku, najdete [diskuzi na fóru](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span><span class="sxs-lookup"><span data-stu-id="26764-360">For increasing VHD sizes, see this [forum discussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="26764-361">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="26764-361">Disadvantages</span></span>
* <span data-ttu-id="26764-362">Při migraci dojde k dočasné ztrátě HA a zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="26764-362">There is a temporary loss of HA and DR during migration.</span></span>
* <span data-ttu-id="26764-363">Protože se jedná o migraci 1:1, budete mít toouse minimální velikost virtuálního počítače, která bude podporovat počtu virtuální pevné disky, takže nemusí být možné toodownsize virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="26764-363">As this is a 1:1 migration, you will have toouse a minimum VM size that will support your number of VHDs, so you might not be able toodownsize your VMs.</span></span>
* <span data-ttu-id="26764-364">Tento scénář využije hello Azure **Start-AzureStorageBlobCopy** příkaz, který je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="26764-364">This scenario would use hello Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="26764-365">Neexistuje žádná smlouva SLA na dokončení kopírování.</span><span class="sxs-lookup"><span data-stu-id="26764-365">There is no SLA on copy completion.</span></span> <span data-ttu-id="26764-366">čas Hello kopií hello se liší, když to závisí na čekání ve frontě, bude také záviset na hello množství dat tootransfer.</span><span class="sxs-lookup"><span data-stu-id="26764-366">hello time of hello copies varies, while this depends on wait in queue it will also depend on hello amount of data tootransfer.</span></span> <span data-ttu-id="26764-367">Čas kopie Hello zvyšuje, pokud přenos hello budete tooanother Azure datového centra, který podporuje službu Premium Storage v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="26764-367">hello copy time increases if hello transfer is going tooanother Azure data center that supports Premium Storage in another region.</span></span> <span data-ttu-id="26764-368">Pokud máte právě 2 uzly, vezměte v úvahu možné snížení rizika v případě, že kopírování hello trvá déle než při testování.</span><span class="sxs-lookup"><span data-stu-id="26764-368">If you just have 2 nodes, consider a possible mitigation in case hello copy takes longer than in testing.</span></span> <span data-ttu-id="26764-369">To může zahrnovat následující návrhy hello.</span><span class="sxs-lookup"><span data-stu-id="26764-369">This could include hello following ideas.</span></span>
  * <span data-ttu-id="26764-370">Přidáte dočasný uzel systému SQL Server 3. pro HA před migrací hello odsouhlaseného výpadku.</span><span class="sxs-lookup"><span data-stu-id="26764-370">Add a temporary 3rd SQL Server node for HA before hello migration with agreed downtime.</span></span>
  * <span data-ttu-id="26764-371">Spusťte migraci hello mimo Azure plánované údržby.</span><span class="sxs-lookup"><span data-stu-id="26764-371">Run hello migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="26764-372">Zkontrolujte, zda že jste správně nakonfigurovali vaší kvorum clusteru.</span><span class="sxs-lookup"><span data-stu-id="26764-372">Ensure you have configured your cluster quorum correctly.</span></span>  

##### <a name="high-level-steps"></a><span data-ttu-id="26764-373">Postup vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="26764-373">High-level steps</span></span>
<span data-ttu-id="26764-374">Tento dokument nepředvádí v příkladu tooend dokončení end, ale hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) poskytuje podrobné informace, které mohou být využity tooperform to.</span><span class="sxs-lookup"><span data-stu-id="26764-374">This document does not demonstrate a complete end tooend example, however hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) provides details that can be leveraged tooperform this.</span></span>

![MinimalDowntime][8]

* <span data-ttu-id="26764-376">Konfigurace disku shromáždění a odebrat hello uzlu (neodstraňujte připojené virtuální pevné disky).</span><span class="sxs-lookup"><span data-stu-id="26764-376">Gather disk configuration, and remove hello node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="26764-377">Vytvořit účet úložiště Premium a zkopírovat virtuální pevné disky z hello standardní účet úložiště</span><span class="sxs-lookup"><span data-stu-id="26764-377">Create a Premium Storage account and copy VHDs from hello Standard Storage account</span></span>
* <span data-ttu-id="26764-378">Vytvořte novou cloudovou službu a znovu nasaďte hello SQL2 virtuálních počítačů v rámci této cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="26764-378">Create new cloud service and redeploy hello SQL2 VM in that cloud service.</span></span> <span data-ttu-id="26764-379">Vytvoření hello virtuálních počítačů pomocí hello zkopíruje původní virtuální pevný disk operačního systému a připojení hello zkopírují virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="26764-379">Create hello VM using hello copied original OS VHD and attaching hello copied VHDs.</span></span>
* <span data-ttu-id="26764-380">Konfigurace ILB / ELB a přidat koncové body.</span><span class="sxs-lookup"><span data-stu-id="26764-380">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="26764-381">Buď aktualizujte naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="26764-381">Update Listener by either:</span></span>
  * <span data-ttu-id="26764-382">Pořízení hello vždy na skupiny offline a aktualizuje hello vždy na naslouchací proces s novou ILB / ELB IP adres.</span><span class="sxs-lookup"><span data-stu-id="26764-382">Taking hello Always On Group offline and updating hello Always On Listener with new ILB / ELB IP address.</span></span>
  * <span data-ttu-id="26764-383">Nebo přidání hello IP adresu prostředku z nové cloudové služby ILB/ELB pomocí prostředí PowerShell do clusteringu Windows.</span><span class="sxs-lookup"><span data-stu-id="26764-383">Or adding hello IP address resource of new Cloud Service ILB/ELB through PowerShell into Windows clustering.</span></span> <span data-ttu-id="26764-384">Sada hello Možní vlastníci hello IP adresu prostředku toohello pak migrovat uzlu SQL2 a nastavit jako závislosti nebo v hello název sítě.</span><span class="sxs-lookup"><span data-stu-id="26764-384">Then set hello Possible owners of hello IP Address resource toohello migrated node, SQL2, and set this as OR dependency in hello Network Name.</span></span> <span data-ttu-id="26764-385">V části hello 'Přidání prostředek IP adresy ve stejné podsíti, s hello [příloha](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="26764-385">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
* <span data-ttu-id="26764-386">Zkontrolujte klienty toohello konfigurace/šíření DNS.</span><span class="sxs-lookup"><span data-stu-id="26764-386">Check DNS configuration/propogation toohello clients.</span></span>
* <span data-ttu-id="26764-387">Migrujte virtuální počítač SQL1 a projít kroky 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="26764-387">Migrate SQL1 VM, and go through steps 2 – 4.</span></span>
* <span data-ttu-id="26764-388">Pokud používáte 5ii kroky, přidejte počítač SQL1 jako možného vlastníka pro hello přidat prostředek IP adresy</span><span class="sxs-lookup"><span data-stu-id="26764-388">If using steps 5ii, then add SQL1 as a Possible Owner for hello added IP Address Resource</span></span>
* <span data-ttu-id="26764-389">Testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-389">Test failovers.</span></span>

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a><span data-ttu-id="26764-390">2. Využívat stávající sekundární následující: Multi-Site</span><span class="sxs-lookup"><span data-stu-id="26764-390">2. Utilize existing secondary replica(s): Multi-Site</span></span>
<span data-ttu-id="26764-391">Pokud máte uzly ve více než jeden datové centrum Azure (DC) nebo pokud máte hybridní prostředí, můžete použít konfigurace vždy na tento výpadek toominimize prostředí.</span><span class="sxs-lookup"><span data-stu-id="26764-391">If you have nodes in more than one Azure datacenter (DC) or if you have a hybrid environment, then you can use an Always On configuration in this environment toominimize downtime.</span></span>

<span data-ttu-id="26764-392">Hello přístup je toochange hello Always On synchronizace tooSynchronous hello místní nebo sekundární řadič domény Azure a převzetí služeb při selhání přes toothat systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26764-392">hello approach is toochange hello Always On synchronization tooSynchronous for hello on-premises or secondary Azure DC, and then failover over toothat SQL Server.</span></span> <span data-ttu-id="26764-393">Pak zkopírujte hello virtuální pevné disky tooa účet služby Premium Storage a znovu nasadit počítač hello do novou cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="26764-393">Then copy hello VHDs tooa Premium Storage account, and redeploy hello machine into a new cloud service.</span></span> <span data-ttu-id="26764-394">Aktualizujte hello naslouchací proces a pak navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="26764-394">Update hello listener, and then fail back.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="26764-395">Body výpadku</span><span class="sxs-lookup"><span data-stu-id="26764-395">Points of downtime</span></span>
<span data-ttu-id="26764-396">výpadek Hello se skládá z hello čas toofailover toohello alternativní řadič domény a zpět.</span><span class="sxs-lookup"><span data-stu-id="26764-396">hello downtime consists of hello time toofailover toohello alternative DC and back.</span></span> <span data-ttu-id="26764-397">Také závisí na konfiguraci klienta a DNS a může být zpoždění vaší opětovné připojení klienta.</span><span class="sxs-lookup"><span data-stu-id="26764-397">It also depends on your client/DNS configuration and your client reconnection may be delayed.</span></span>
<span data-ttu-id="26764-398">Vezměte v úvahu následující ukázka hybridní Always On konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="26764-398">Consider hello following example of a hybrid Always On configuration:</span></span>

![MultiSite1][9]

##### <a name="advantages"></a><span data-ttu-id="26764-400">Výhody</span><span class="sxs-lookup"><span data-stu-id="26764-400">Advantages</span></span>
* <span data-ttu-id="26764-401">Můžete využít stávající infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="26764-401">You can utilize existing infrastructure.</span></span>
* <span data-ttu-id="26764-402">Nejprve máte hello možnost toopre upgrade hello úložiště Azure na hello řadič domény Azure zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="26764-402">You have hello option toopre-upgrade hello Azure storage on hello DR Azure DC first.</span></span>
* <span data-ttu-id="26764-403">můžete třeba překonfigurovat Hello úložiště Azure pro zotavení po Havárii řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="26764-403">hello DR Azure DC storage can be reconfigured.</span></span>
* <span data-ttu-id="26764-404">Během migrace, s výjimkou testovací převzetí služeb při selhání není k dispozici minimálně dvě převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-404">There is a minimum of two failovers during migration, excluding test failovers.</span></span>
* <span data-ttu-id="26764-405">Není nutné toomove dat systému SQL Server pomocí zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="26764-405">You do not need toomove SQL Server data with backup and restore.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="26764-406">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="26764-406">Disadvantages</span></span>
* <span data-ttu-id="26764-407">V závislosti na klienta tooSQL přístup k serveru může být zvýší latence při systém SQL Server běží v aplikaci toohello alternativní řadič domény.</span><span class="sxs-lookup"><span data-stu-id="26764-407">Depending on client access tooSQL Server, there might be increased latency when SQL Server is running in an alternative DC toohello application.</span></span>
* <span data-ttu-id="26764-408">čas Hello kopírování virtuálních pevných disků tooPremium úložiště může trvat dlouho.</span><span class="sxs-lookup"><span data-stu-id="26764-408">hello copy time of VHDs tooPremium storage could be long.</span></span> <span data-ttu-id="26764-409">Může to mít vliv na rozhodnutí o tom, jestli tookeep hello uzlu v hello skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="26764-409">This might affect your decision on whether tookeep hello node in hello Availability Group.</span></span> <span data-ttu-id="26764-410">Zvažte proto pro při protokolu náročné pracovním zatížením běží během migrace hello je nutné, protože hello primárním uzlu, který bude mít tookeep hello nereplikovaných transakce v protokolu transakcí.</span><span class="sxs-lookup"><span data-stu-id="26764-410">Consider this for when log intensive work loads are running during hello migration is required, since hello Primary node will have tookeep hello unreplicated transactions in its transaction log.</span></span> <span data-ttu-id="26764-411">Proto to může výrazně zvýší.</span><span class="sxs-lookup"><span data-stu-id="26764-411">Therefore this could grow significantly.</span></span>
* <span data-ttu-id="26764-412">Tento scénář využije hello Azure **Start-AzureStorageBlobCopy** příkaz, který je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="26764-412">This scenario would use hello Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="26764-413">Neexistuje žádná smlouva SLA na dokončení.</span><span class="sxs-lookup"><span data-stu-id="26764-413">There is no SLA on completion.</span></span> <span data-ttu-id="26764-414">Hello čas kopií hello se liší, když to závisí na čekání ve frontě, bude také záviset na hello množství dat tootransfer.</span><span class="sxs-lookup"><span data-stu-id="26764-414">hello time of hello copies varies, while this depends on wait in queue, it will also depend on hello amount of data tootransfer.</span></span> <span data-ttu-id="26764-415">Proto máte jenom jeden uzel v 2. datové centrum, můžete provést kroky zmírňující rizika v případě, že kopírování hello trvá déle než při testování.</span><span class="sxs-lookup"><span data-stu-id="26764-415">Therefore you just have one node in your 2nd data center, you should take mitigation steps in case hello copy takes longer than in testing.</span></span> <span data-ttu-id="26764-416">To může zahrnovat následující návrhy hello.</span><span class="sxs-lookup"><span data-stu-id="26764-416">This could include hello following ideas.</span></span>
  * <span data-ttu-id="26764-417">Přidáte dočasný uzel SQL 2. pro HA před migrací hello odsouhlaseného výpadku.</span><span class="sxs-lookup"><span data-stu-id="26764-417">Add a temporary 2nd SQL node for HA before hello migration with agreed downtime.</span></span>
  * <span data-ttu-id="26764-418">Spusťte migraci hello mimo Azure plánované údržby.</span><span class="sxs-lookup"><span data-stu-id="26764-418">Run hello migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="26764-419">Zkontrolujte, zda že jste správně nakonfigurovali vaší kvorum clusteru.</span><span class="sxs-lookup"><span data-stu-id="26764-419">Ensure you have configured your cluster quorum correctly.</span></span>

<span data-ttu-id="26764-420">Tento scénář předpokládá, že můžete mít zdokumentovaný instalaci vaší a vědět, jak je v pořadí toomake změny pro disk optimální nastavení mezipaměti namapované hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-420">This scenario assumes that you have documented your install and know how hello storage is mapped in order toomake changes for optimal disk cache settings.</span></span>

##### <a name="high-level-steps"></a><span data-ttu-id="26764-421">Postup vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="26764-421">High-level steps</span></span>
![Multisite2][10]

* <span data-ttu-id="26764-423">Ujistěte se, hello místní / alternativní řadič domény Azure hello primární Server SQL a nastavte jej hello jiné automatické převzetí služeb při selhání partnera (AFP).</span><span class="sxs-lookup"><span data-stu-id="26764-423">Make hello on-premises / alternate Azure DC hello SQL Server Primary, and make it hello other Auto Failover Partner (AFP).</span></span>
* <span data-ttu-id="26764-424">Shromážděte informace o konfiguraci disku z SQL2 a odebrat hello uzlu (neodstraňujte připojené virtuální pevné disky).</span><span class="sxs-lookup"><span data-stu-id="26764-424">Gather disk configuration information from SQL2, and remove hello node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="26764-425">Vytvořit účet úložiště Premium a zkopírovat virtuální pevné disky z hello standardní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="26764-425">Create a Premium Storage account and copy VHDs from hello Standard Storage account.</span></span>
* <span data-ttu-id="26764-426">Vytvořte novou cloudovou službu a vytvořte hello SQL2 virtuálních počítačů s jeho úložiště prémií disky připojené.</span><span class="sxs-lookup"><span data-stu-id="26764-426">Create a new cloud service and create hello SQL2 VM with its Premiums Storage disks attached.</span></span>
* <span data-ttu-id="26764-427">Konfigurace ILB / ELB a přidat koncové body.</span><span class="sxs-lookup"><span data-stu-id="26764-427">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="26764-428">Aktualizace hello vždy na naslouchací proces s novou ILB / ELB IP adresu a testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-428">Update hello Always On Listener with new ILB / ELB IP address and test failover.</span></span>
* <span data-ttu-id="26764-429">Zkontrolujte konfiguraci DNS hello.</span><span class="sxs-lookup"><span data-stu-id="26764-429">Check hello DNS configuration.</span></span>
* <span data-ttu-id="26764-430">Změnit hello AFP tooSQL2 migrovat SQL1 a projít kroky 2 až 5.</span><span class="sxs-lookup"><span data-stu-id="26764-430">Change hello AFP tooSQL2, and then migrate SQL1 and go through steps 2 – 5.</span></span>
* <span data-ttu-id="26764-431">Testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-431">Test failovers.</span></span>
* <span data-ttu-id="26764-432">Přepnout zpět tooSQL1 hello AFP a SQL2</span><span class="sxs-lookup"><span data-stu-id="26764-432">Switch hello AFP back tooSQL1 and SQL2</span></span>

## <a name="appendix-migrating-a-multisite-always-on-cluster-toopremium-storage"></a><span data-ttu-id="26764-433">Dodatek: Migrace tooPremium nasazení ve více lokalitách vždy na clusteru úložiště</span><span class="sxs-lookup"><span data-stu-id="26764-433">Appendix: Migrating a Multisite Always On Cluster tooPremium Storage</span></span>
<span data-ttu-id="26764-434">Hello zbývající část tohoto tématu poskytuje podrobný příklad převodu více lokalit Always On tooPremium úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="26764-434">hello remainder of this topic provides a detailed example of converting a multi-site Always On cluster tooPremium storage.</span></span> <span data-ttu-id="26764-435">Převede také hello naslouchací proces pomocí k externí nástroje pro vyrovnávání (ELB) tooan interní služby load pro vyrovnávání zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="26764-435">It also converts hello Listener from using an external load balancer (ELB) tooan internal load balancer (ILB).</span></span>

### <a name="environment"></a><span data-ttu-id="26764-436">Prostředí</span><span class="sxs-lookup"><span data-stu-id="26764-436">Environment</span></span>
* <span data-ttu-id="26764-437">Windows 2k 12 / SQL 2k 12</span><span class="sxs-lookup"><span data-stu-id="26764-437">Windows 2k12 / SQL 2k12</span></span>
* <span data-ttu-id="26764-438">1 DB soubory na SP</span><span class="sxs-lookup"><span data-stu-id="26764-438">1 DB Files on SP</span></span>
* <span data-ttu-id="26764-439">2 x fondy úložiště na každý uzel</span><span class="sxs-lookup"><span data-stu-id="26764-439">2 x Storage Pools per Node</span></span>

![Appendix1][11]

### <a name="vm"></a><span data-ttu-id="26764-441">VIRTUÁLNÍ POČÍTAČ:</span><span class="sxs-lookup"><span data-stu-id="26764-441">VM:</span></span>
<span data-ttu-id="26764-442">V tomto příkladu přidáme toodemonstrate přechod ze ELB tooILB.</span><span class="sxs-lookup"><span data-stu-id="26764-442">In this example we are going toodemonstrate moving from an ELB tooILB.</span></span> <span data-ttu-id="26764-443">Režim Manageout nebylo k dispozici před ILB, takže to ukazuje, jak hello tooswitch toothis během migrace.</span><span class="sxs-lookup"><span data-stu-id="26764-443">ELB was available before ILB, so this shows how tooswitch toothis during hello migration.</span></span>

![Appendix2][12]

### <a name="pre-steps-connect-toosubscription"></a><span data-ttu-id="26764-445">Předběžné kroky: Připojit tooSubscription</span><span class="sxs-lookup"><span data-stu-id="26764-445">Pre Steps: Connect tooSubscription</span></span>
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a><span data-ttu-id="26764-446">Krok 1: Vytvořte nový účet úložiště a cloudové služby</span><span class="sxs-lookup"><span data-stu-id="26764-446">Step 1: Create New Storage Account and Cloud Service</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where hello vm toomigrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-hello-permitted-failures-on-resources-optional"></a><span data-ttu-id="26764-447">Krok 2: Zvýšení hello povolená selhání na prostředky<Optional></span><span class="sxs-lookup"><span data-stu-id="26764-447">Step 2: Increase hello permitted failures on resources <Optional></span></span>
<span data-ttu-id="26764-448">Na některé prostředky, které patří tooyour vždy na skupiny dostupnosti neexistují omezení na tom, kolik chyby, ke kterým může dojít v době, kdy hello Clusterová služba se pokusí skupiny prostředků toorestart hello.</span><span class="sxs-lookup"><span data-stu-id="26764-448">On certain resources that belong tooyour Always On Availability Group there are limits on how many failures that can occur in a period, where hello cluster service will attempt toorestart hello resource group.</span></span> <span data-ttu-id="26764-449">Doporučuje se, že to zvýšíte a přitom jste se s návodem tento postup, vzhledem k tomu, že pokud to neuděláte ruční převzetí služeb při selhání a aktivační události převzetí služeb při selhání pomocí vypínání počítačů můžete získat zavřít toothis limit.</span><span class="sxs-lookup"><span data-stu-id="26764-449">It is recommended you increase this whilst you are walking through this procedure, since if you don’t manually failover and trigger failovers by shutting down machines you can get close toothis limit.</span></span>

<span data-ttu-id="26764-450">Ho by být vhodné toodouble hello selhání příspěvek, toodo to ve Správci clusteru převzetí služeb při selhání, přejděte toohello vlastnosti hello Always On skupiny zdrojů:</span><span class="sxs-lookup"><span data-stu-id="26764-450">It would be prudent toodouble hello failure allowance, toodo this in Failover Cluster Manager, go toohello Properties of hello Always On resource group:</span></span>

![Appendix3][13]

<span data-ttu-id="26764-452">Změňte hello too6 maximální počet selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-452">Change hello Maximum Failures too6.</span></span>

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a><span data-ttu-id="26764-453">Krok 3: Přidání IP adresu prostředku pro skupinu clusteru<Optional></span><span class="sxs-lookup"><span data-stu-id="26764-453">Step 3: Addition IP Address resource for Cluster Group <Optional></span></span>
<span data-ttu-id="26764-454">Pokud máte pouze jednu IP adresu pro hello skupina clusteru a toto je zarovnaný toohello cloudu podsíť, mějte na paměti, pokud jste omylem převést do režimu offline všechny uzly clusteru v cloudu hello síť pak prostředek hello IP clusteru a název sítě s clustery nebude možné toocome online.</span><span class="sxs-lookup"><span data-stu-id="26764-454">If you have only one IP address for hello Cluster Group and this is aligned toohello cloud subnet, beware, if you accidentally take offline all cluster nodes in hello cloud on that network then hello Cluster IP resource and Cluster Network Name will not be able toocome online.</span></span> <span data-ttu-id="26764-455">Události tohoto, které zabrání v hello aktualizuje tooother prostředky clusteru.</span><span class="sxs-lookup"><span data-stu-id="26764-455">In hello event of this it will prevent updates tooother cluster resources.</span></span>

#### <a name="step-4-dns-configuration"></a><span data-ttu-id="26764-456">Krok 4: Konfigurace serveru DNS</span><span class="sxs-lookup"><span data-stu-id="26764-456">Step 4: DNS configuration</span></span>
<span data-ttu-id="26764-457">tooimplement s hladkým přechodem závisí na tom, jak se DNS využité a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="26764-457">tooimplement a smooth transition depends on how DNS is being utilized and updated.</span></span>
<span data-ttu-id="26764-458">Když Always On je nainstalován, vytvoří skupinu prostředků clusteru systému Windows, pokud otevřete Správce clusteru převzetí služeb při selhání, uvidíte, že minimálně bude mít tři zdroje, hello dva, které hello dokumentu odkazuje tooare:</span><span class="sxs-lookup"><span data-stu-id="26764-458">When Always On is installed, it creates a Windows Cluster Resource group, if you open Failover Cluster Manager, you will see that at a minimum it will have three resources, hello two that hello document refers tooare:</span></span>

* <span data-ttu-id="26764-459">Název virtuální sítě (VNN) – Toto je hello DNS název tohoto klienta připojit toowhen podmínka mimo tooconnect tooSQL servery prostřednictvím Always On.</span><span class="sxs-lookup"><span data-stu-id="26764-459">Virtual Network Name (VNN) – This is hello DNS name that client connect toowhen wanting tooconnect tooSQL Servers via Always On.</span></span>
* <span data-ttu-id="26764-460">Prostředek IP adresy – je hello IP adresu, kterou přidružené hello VNN, můžete mít více než jednu a v konfiguraci ve více lokalitách bude mít za lokality a podsítě IP adresu.</span><span class="sxs-lookup"><span data-stu-id="26764-460">IP Address Resource – This is hello IP address that associated with hello VNN, you can have more than one, and in a multisite configuration you will have an IP address per site/subnet.</span></span>

<span data-ttu-id="26764-461">Při připojování tooSQL Server hello ovladač klienta systému SQL Server bude načítat záznamy DNS hello přidružený naslouchací proces hello a zkuste tooconnect tooeach Always On přidružené IP adresu, níže probereme některé faktory, které mohou mít vliv na to.</span><span class="sxs-lookup"><span data-stu-id="26764-461">When connecting tooSQL Server, hello SQL Server Client driver will retrieve hello DNS records associated with hello listener and try tooconnect tooeach Always On associated IP address, below we discuss some factors that can influence this.</span></span>

<span data-ttu-id="26764-462">Hello počet souběžných záznamy DNS, které jsou přidruženy název naslouchacího procesu hello závisí nejen na hello počet IP adresám, ale hello ' RegisterAllIpProviders'setting clusteringu pro převzetí služeb při selhání pro hello Always ON VNN prostředků.</span><span class="sxs-lookup"><span data-stu-id="26764-462">hello number of concurrent DNS records that are associated with hello listener name depends not only on hello number of IP addresses associated, but hello ‘RegisterAllIpProviders’setting in Failover Clustering for hello Always ON VNN resource.</span></span>

<span data-ttu-id="26764-463">Když nasadíte Always On v Azure existují různé kroky toocreate hello naslouchací proces a IP adresy, máte toomanually konfigurace too1 "RegisterAllIpProviders" hello, toto je jiný tooan místní vždy na nasazení, kde je již nastaven too1.</span><span class="sxs-lookup"><span data-stu-id="26764-463">When you deploy Always On in Azure there are different steps toocreate hello Listener and IP Addresses, you have toomanually configure hello ‘RegisterAllIpProviders’ too1, this is different tooan on-premises Always On deployment where it is already set too1.</span></span>

<span data-ttu-id="26764-464">Pokud 'RegisterAllIpProviders' 0, zobrazí se jenom jeden záznam DNS ve službě DNS přidružené hello naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="26764-464">If ‘RegisterAllIpProviders’ is 0, then you will only see one DNS record in DNS associated with hello Listener:</span></span>

![Appendix4][14]

<span data-ttu-id="26764-466">Je-li 'RegisterAllIpProviders' 1:</span><span class="sxs-lookup"><span data-stu-id="26764-466">If ‘RegisterAllIpProviders’ is 1:</span></span>

![Appendix5][15]

<span data-ttu-id="26764-468">Následující kód Hello bude dump out hello VNN nastavení a nastavení za vás, prosím Poznámka pro hello změnit efekt tootake budete potřebovat tootake hello VNN offline a zapněte ji zpět do režimu online, tento pořízení hello naslouchací proces offline což způsobilo klientských připojení k přerušení.</span><span class="sxs-lookup"><span data-stu-id="26764-468">hello code below will dump out hello VNN settings and set it for you, please note, for hello change tootake effect you will need tootake hello VNN offline and turn it back online, this taking hello Listener offline causing client connectivity disruption.</span></span>

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

<span data-ttu-id="26764-469">V pozdější fázi migrace budete potřebovat tooupdate hello Always On naslouchací proces s aktualizovanou adresou IP, která bude odkazovat na službu Vyrovnávání zatížení, to bude zahrnovat odebrání prostředků IP adresy a přidání.</span><span class="sxs-lookup"><span data-stu-id="26764-469">In a later migration step you will need tooupdate hello Always On listener with an updated IP address that will reference a load balancer, this will involve an IP Address resource removal and addition.</span></span> <span data-ttu-id="26764-470">Po aktualizaci hello IP je třeba tooensure hello novou IP adresu byl aktualizován v zóně DNS a že hello klientům aktualizují své místní mezipaměti DNS.</span><span class="sxs-lookup"><span data-stu-id="26764-470">After hello IP update, you need tooensure hello new IP address has been updated in DNS Zone and that hello clients are updating their local DNS cache.</span></span>

<span data-ttu-id="26764-471">Pokud vaši klienti nacházejí v různých síťových segmentu a odkazovat na jiný server DNS, je třeba tooconsider, co se stane o přenos zóny DNS v průběhu migrace hello jako hello aplikace znovu připojit čas budou omezené. pomocí alespoň hello čas přenosu zóny všechny nové IP adres pro naslouchání hello.</span><span class="sxs-lookup"><span data-stu-id="26764-471">If your clients reside in a different network segment and reference a different DNS server, you need tooconsider what happens about DNS Zone Transfer during hello migration, as hello application reconnect time will be constrained by at least hello Zone Transfer Time of any new IP addresses for hello listener.</span></span> <span data-ttu-id="26764-472">Pokud jste v části času omezení tady, by měl zabývat a otestovat vynucení přenos zóny přírůstkové s týmům Windows a taky hello DNS tooa záznamu hostitele nižší Time (tooLive TTL), takže klienti hello aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="26764-472">If you are under time constraint here, you should discuss and test forcing an incremental zone transfer with your Windows teams, and also put hello DNS host record tooa lower Time tooLive (TTL), so hello clients update.</span></span> <span data-ttu-id="26764-473">Další informace najdete v tématu [přírůstkové přenosy zóny](https://technet.microsoft.com/library/cc958973.aspx) a [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span><span class="sxs-lookup"><span data-stu-id="26764-473">For more information, see [Incremental Zone Transfers](https://technet.microsoft.com/library/cc958973.aspx) and [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span></span>

<span data-ttu-id="26764-474">Ve výchozím nastavení hello TTL pro DNS záznam, který je přidružen hello naslouchací proces ve Always On v Azure je 1 200 sekund.</span><span class="sxs-lookup"><span data-stu-id="26764-474">By default hello TTL for DNS Record that is associated with hello Listener in Always On in Azure is 1200 seconds.</span></span> <span data-ttu-id="26764-475">Můžete tooreduce to pokud jste v rámci omezení čas během migrace tooensure hello klientům aktualizace jejich DNS s hello aktualizovat IP adres pro naslouchání hello.</span><span class="sxs-lookup"><span data-stu-id="26764-475">You may wish tooreduce this if you are under time constraint during your migration tooensure hello clients update their DNS with hello updated IP address for hello listener.</span></span> <span data-ttu-id="26764-476">Můžete zobrazit a upravit konfiguraci hello vypsání out hello konfiguraci hello VNN:</span><span class="sxs-lookup"><span data-stu-id="26764-476">You can see and modify hello configuration by dumping out hello configuration of hello VNN:</span></span>

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

<span data-ttu-id="26764-477">Poznámka: hello nižší hello 'HostRecordTTL', dojde k vyšší množství přenosy DNS.</span><span class="sxs-lookup"><span data-stu-id="26764-477">Please note, hello lower hello ‘HostRecordTTL’, a higher amount of DNS traffic will occur.</span></span>

##### <a name="client-application-settings"></a><span data-ttu-id="26764-478">Nastavení aplikace klienta</span><span class="sxs-lookup"><span data-stu-id="26764-478">Client application settings</span></span>
<span data-ttu-id="26764-479">Pokud vaše aplikace podporuje klient SQL hello .net 4.5 SQLClient a pak můžete použít "MULTISUBNETFAILOVER = TRUE" – klíčové slovo, tato možnost se doporučuje toobe použít jako během převzetí služeb při selhání umožňuje rychlejší připojení tooSQL vždy na skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="26764-479">If your SQL client application supports hello .Net 4.5 SQLClient, then you can use ‘MULTISUBNETFAILOVER=TRUE’ keyword, this is recommended toobe applied as it allows for faster connection tooSQL Always On Availability Group during failover.</span></span> <span data-ttu-id="26764-480">Vytvoří výčet prostřednictvím všechny IP adresy přidružené k hello Always On naslouchací proces paralelně a provede agresivnější rychlost opakování připojení TCP při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-480">It enumerates through all IP addresses associated with hello Always On listener in parallel, and performs a more aggressive TCP connection retry speed during a failover.</span></span>

<span data-ttu-id="26764-481">Další informace týkající se nastavení hello výše, najdete v tématu [MultiSubnetFailover – klíčové slovo a související funkce](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span><span class="sxs-lookup"><span data-stu-id="26764-481">For more information regarding hello settings above, please see [MultiSubnetFailover Keyword and Associated Features](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span></span> <span data-ttu-id="26764-482">Viz také [SqlClient podporu pro vysokou dostupnost a zotavení po havárii](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="26764-482">Also see [SqlClient Support for High Availability, Disaster Recovery](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span></span>

#### <a name="step-5-cluster-quorum-settings"></a><span data-ttu-id="26764-483">Krok 5: Nastavení kvora clusteru</span><span class="sxs-lookup"><span data-stu-id="26764-483">Step 5: Cluster quorum settings</span></span>
<span data-ttu-id="26764-484">Jak budete toobe vyřazení alespoň jeden Server SQL dolů v čase, by měl hello nastavení kvora clusteru, upravit, pokud pomocí souboru sdílené složky s kopií clusteru (FSW) s uzly, 2, doporučujeme nastavit Většina uzlů tooallow hello kvora a využívat dynamické hlasování, a to je tooallow pro stálého tooremain jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="26764-484">As you are going toobe taking out at least one SQL Server down at a time, you should modify hello cluster quorum setting, if using File Share Witness (FSW) with 2 nodes, you should set hello quorum tooallow node majority and utilize dynamic voting, and this is tooallow for a single node tooremain standing.</span></span>

    Set-ClusterQuorum -NodeMajority  

<span data-ttu-id="26764-485">Další informace o správě a nastavení kvora clusteru hello, najdete v tématu [konfigurace a správa kvora v clusteru převzetí služeb při selhání ve Windows serveru 2012 hello](https://technet.microsoft.com/library/jj612870.aspx).</span><span class="sxs-lookup"><span data-stu-id="26764-485">For more information on managing and configuring hello cluster quorum, please see [Configure and Manage hello Quorum in a Windows Server 2012 Failover Cluster](https://technet.microsoft.com/library/jj612870.aspx).</span></span>

#### <a name="step-6-extract-existing-endpoints-and-acls"></a><span data-ttu-id="26764-486">Krok 6: Extrahování stávající koncové body a seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="26764-486">Step 6: Extract Existing Endpoints and ACLs</span></span>
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for hello Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

<span data-ttu-id="26764-487">Uložte tyto tooa textového souboru.</span><span class="sxs-lookup"><span data-stu-id="26764-487">Save these tooa text file.</span></span>

#### <a name="step-7-change-failover-partners-and-replication-modes"></a><span data-ttu-id="26764-488">Krok 7: Změnit režimy replikace a převzetí služeb při selhání partnery</span><span class="sxs-lookup"><span data-stu-id="26764-488">Step 7: Change Failover Partners and Replication Modes</span></span>
<span data-ttu-id="26764-489">Pokud máte více než 2 servery SQL Server, by se měl změnit hello převzetí služeb při selhání jinou sekundární v jiné řadiče domény nebo místní too'Synchronous' se bude automatické převzetí služeb při selhání partnera (AFP), to je tak spravovat HA a přitom provádíte změny.</span><span class="sxs-lookup"><span data-stu-id="26764-489">If you have more than 2 SQL Servers, you should change hello failover of another secondary in another DC or on-premises too‘Synchronous’ and make it an Automatic Failover Partner (AFP), this is so you maintain HA whilst you are making changes.</span></span> <span data-ttu-id="26764-490">Můžete to provést prostřednictvím TSQL systému změnit, když aplikace SSMS:</span><span class="sxs-lookup"><span data-stu-id="26764-490">You can do this through TSQL of modify though SSMS:</span></span>

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a><span data-ttu-id="26764-492">Krok 8: Odeberte sekundární virtuální počítač z cloudové služby</span><span class="sxs-lookup"><span data-stu-id="26764-492">Step 8: Remove Secondary VM from cloud service</span></span>
<span data-ttu-id="26764-493">Byste měli počítat toomigrate sekundární uzel cloudu první, pokud je aktuálně primární, by měl spustíte ruční převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="26764-493">You should be planning toomigrate a cloud secondary node first, if this is currently primary, you should initiate a manual failover.</span></span>

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns hello disks associated with hello VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="26764-494">Krok 9: Změňte disku ukládání do mezipaměti nastavení v souboru CSV a uložit</span><span class="sxs-lookup"><span data-stu-id="26764-494">Step 9: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="26764-495">Pro datové svazky tyto měli nastavit tooREADONLY.</span><span class="sxs-lookup"><span data-stu-id="26764-495">For data volumes these should be set tooREADONLY.</span></span>

<span data-ttu-id="26764-496">Pro svazky protokolu tyto měli nastavit tooNONE.</span><span class="sxs-lookup"><span data-stu-id="26764-496">For TLOG volumes these should be set tooNONE.</span></span>

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a><span data-ttu-id="26764-498">Krok 10: Kopírování virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="26764-498">Step 10: Copy VHDS</span></span>
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



<span data-ttu-id="26764-499">Můžete zkontrolovat stav kopírování hello hello virtuální pevné disky toohello účet služby Premium Storage:</span><span class="sxs-lookup"><span data-stu-id="26764-499">You can check hello copy status of hello VHDs toohello Premium Storage account:</span></span>

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

<span data-ttu-id="26764-501">Počkejte, dokud všechny tyto se zaznamenávají jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="26764-501">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="26764-502">Informace pro jednotlivé objekty BLOB:</span><span class="sxs-lookup"><span data-stu-id="26764-502">For information for individual blobs:</span></span>

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a><span data-ttu-id="26764-503">Krok 11: Registrace operačního systému disku</span><span class="sxs-lookup"><span data-stu-id="26764-503">Step 11: Register OS disk</span></span>
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a><span data-ttu-id="26764-504">Krok 12: Importujte sekundární do novou cloudovou službu</span><span class="sxs-lookup"><span data-stu-id="26764-504">Step 12: Import secondary into new cloud service</span></span>
<span data-ttu-id="26764-505">Kód Hello pod také používá hello přidána možnost sem můžete můžete importovat hello počítače a použít hello retainable VIP.</span><span class="sxs-lookup"><span data-stu-id="26764-505">hello code below also uses hello added option here you can import hello machine and use hello retainable VIP.</span></span>

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember toochange tooXIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks tooa VM during a deploy tooa new cloud service and new storage account is different from just attaching VHDs toojust a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="26764-506">Krok 13: Vytvoření ILB na nové cloudové Svc zatížit vyrovnáváním koncových bodů a seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="26764-506">Step 13: Create ILB on New Cloud Svc, Add Load Balanced Endpoints and ACLs</span></span>
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a><span data-ttu-id="26764-507">Krok 14: Aktualizace vždy na</span><span class="sxs-lookup"><span data-stu-id="26764-507">Step 14: Update Always On</span></span>
    #Code toobe executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # hello azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency tooListener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

<span data-ttu-id="26764-509">Nyní odeberte hello staré Cloudová služba IP adresu.</span><span class="sxs-lookup"><span data-stu-id="26764-509">Now remove hello old cloud service IP Address.</span></span>

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a><span data-ttu-id="26764-511">Krok 15: Kontrola aktualizací DNS</span><span class="sxs-lookup"><span data-stu-id="26764-511">Step 15: DNS update check</span></span>
<span data-ttu-id="26764-512">Nyní zkontrolujte servery DNS na vaše klientské sítě systému SQL Server a ujistěte se, že clustering přidala hello záznam navíc hostitele pro hello přidat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="26764-512">You should now check DNS Servers on your SQL Server client networks and make sure that clustering has added hello extra host record for hello added IP address.</span></span> <span data-ttu-id="26764-513">Pokud nebyly aktualizovány tyto servery DNS, zvažte vynucení přenos zóny DNS a zkontrolujte, zda hello klientům v podsíti, že jsou možné tooresolve tooboth vždy na IP adresy, to je proto není nutné toowait pro automatické replikaci DNS.</span><span class="sxs-lookup"><span data-stu-id="26764-513">If those DNS servers have not updated, consider forcing a DNS Zone transfer and ensure that hello clients in there subnet are able tooresolve tooboth Always On IP Addresses, this is so you do not need toowait for automatic DNS replication.</span></span>

#### <a name="step-16-reconfigure-always-on"></a><span data-ttu-id="26764-514">Krok 16: Překonfigurujte vždy na</span><span class="sxs-lookup"><span data-stu-id="26764-514">Step 16: Reconfigure Always On</span></span>
<span data-ttu-id="26764-515">V tomto okamžiku je počkat hello sekundární tento uzel, který byl migrované toofully Opětovná synchronizace s hello místní uzel a přepněte uzel toosynchronous replikace a nastavit jej jako hello AFP.</span><span class="sxs-lookup"><span data-stu-id="26764-515">At this point you wait for hello secondary that node that was migrated toofully resynchronize with hello on-premises node and switch toosynchronous replication node and make it hello AFP.</span></span>  

#### <a name="step-17-migrate-second-node"></a><span data-ttu-id="26764-516">Krok 17: Migrace druhého uzlu</span><span class="sxs-lookup"><span data-stu-id="26764-516">Step 17: Migrate second node</span></span>
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="26764-517">Krok 18: Změňte disku ukládání do mezipaměti nastavení v souboru CSV a uložit</span><span class="sxs-lookup"><span data-stu-id="26764-517">Step 18: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="26764-518">Pro datové svazky tyto měli nastavit tooREADONLY.</span><span class="sxs-lookup"><span data-stu-id="26764-518">For data volumes these should be set tooREADONLY.</span></span>

<span data-ttu-id="26764-519">Pro svazky protokolu tyto měli nastavit tooNONE.</span><span class="sxs-lookup"><span data-stu-id="26764-519">For TLOG volumes these should be set tooNONE.</span></span>

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a><span data-ttu-id="26764-521">Krok 19: Vytvoření nového účtu nezávislé úložiště pro sekundární uzel</span><span class="sxs-lookup"><span data-stu-id="26764-521">Step 19: Create New Independent Storage Account for Secondary Node</span></span>
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset hello storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a><span data-ttu-id="26764-522">Krok 20: Kopírování virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="26764-522">Step 20: Copy VHDS</span></span>
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


<span data-ttu-id="26764-523">Pro všechny virtuální pevné disky můžete zkontrolovat stav kopie virtuálního pevného disku hello: příkazu ForEach ($disk v $diskobjects) {$lun = $disk. Logická jednotka $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Jmenovka_disku $diskName = $disk. DiskName</span><span class="sxs-lookup"><span data-stu-id="26764-523">You can check hello VHD copy status for all VHDs: ForEach ($disk in $diskobjects) { $lun = $disk.Lun $vhdname = $disk.vhdname $cacheoption = $disk.HostCaching $disklabel = $disk.DiskLabel $diskName = $disk.DiskName</span></span>

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

<span data-ttu-id="26764-525">Počkejte, dokud všechny tyto se zaznamenávají jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="26764-525">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="26764-526">Informace pro jednotlivé objekty BLOB:</span><span class="sxs-lookup"><span data-stu-id="26764-526">For information for individual blobs:</span></span>

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a><span data-ttu-id="26764-527">Krok 21: Registrace operačního systému disku</span><span class="sxs-lookup"><span data-stu-id="26764-527">Step 21: Register OS disk</span></span>
    #change storage account toohello new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join tooexisting Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different toojust a straight cloud service change
    #note if you do not have a disk label hello command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="26764-528">Krok 22: Přidejte zatížení vyrovnáváním koncových bodů a seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="26764-528">Step 22: Add Load Balanced Endpoints and ACLs</span></span>
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in hello Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a><span data-ttu-id="26764-529">Krok 23: Test převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="26764-529">Step 23: Test failover</span></span>
<span data-ttu-id="26764-530">Teď by měla umožňují hello migrované uzlu synchronizovat s hello místní vždy na uzlu, umístěte režim replikace toosynchronous a počkejte, dokud je synchronizován.</span><span class="sxs-lookup"><span data-stu-id="26764-530">You should now let hello migrated node synchronize with hello on-premises Always On node, place it in toosynchronous replication mode and wait until it is synchronized.</span></span> <span data-ttu-id="26764-531">Následně převzetí služeb při selhání z prvního uzlu v místní toohello migrován, což je hello AFP.</span><span class="sxs-lookup"><span data-stu-id="26764-531">Then failover from on-prem toohello first node migrated, which is hello AFP.</span></span> <span data-ttu-id="26764-532">Jakmile který pracoval, změna hello poslední migrace AFP toohello uzlu.</span><span class="sxs-lookup"><span data-stu-id="26764-532">Once that has worked, change hello last migrated node toohello AFP.</span></span>

<span data-ttu-id="26764-533">Doporučujeme testovací převzetí služeb při selhání mezi všemi uzly a spustit v případě chaos testy, které tooensure převzetí služeb při selhání fungovat jako očekáváno a včas manor.</span><span class="sxs-lookup"><span data-stu-id="26764-533">You should test failovers between all nodes and run though chaos tests tooensure failovers work as expected and in a timely manor.</span></span>

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a><span data-ttu-id="26764-534">Krok 24: Vrátit zpět nastavení kvora clusteru nebo DNS TTL nebo Pntrs převzetí služeb při selhání nebo nastavení synchronizace</span><span class="sxs-lookup"><span data-stu-id="26764-534">Step 24: Put back cluster quorum settings / DNS TTL / Failover Pntrs / Sync Settings</span></span>
##### <a name="adding-ip-address-resource-on-same-subnet"></a><span data-ttu-id="26764-535">Přidání prostředku IP adresy ve stejné podsíti</span><span class="sxs-lookup"><span data-stu-id="26764-535">Adding IP Address Resource on Same Subnet</span></span>
<span data-ttu-id="26764-536">Pokud máte pouze 2 servery SQL a chcete toomigrate je tooa nová Cloudová služba, ale chcete tookeep je na hello stejné podsíti, můžete vyhnout trvá naslouchací proces hello offline toodelete hello vždy původní na IP adresu a přidat hello novou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="26764-536">If you have only 2 SQL Servers and want toomigrate them tooa new cloud service, but want tookeep them on hello same subnet, you can avoid taking hello listener offline toodelete hello original Always On IP Address and add hello New IP Address.</span></span> <span data-ttu-id="26764-537">Pokud provádíte migraci hello virtuální počítače tooanother podsíť nebudete potřebovat toodo to jak bude síť další clusteru, který bude odkazovat na této podsíti.</span><span class="sxs-lookup"><span data-stu-id="26764-537">If you are migrating hello VMs tooanother subnet you will not need toodo this as there will be an additional cluster network that will reference that subnet.</span></span>

<span data-ttu-id="26764-538">Jakmile budete mít zapínají hello migrovat sekundární a přidán v hello novou IP adresu prostředku pro hello novou cloudovou službu, než existující primární hello převzetí služeb při selhání, můžete provést tyto kroky v rámci hello Správce clusteru převzetí služeb při selhání:</span><span class="sxs-lookup"><span data-stu-id="26764-538">Once you have brought up hello migrated secondary and added in hello new IP Address resource for hello new cloud service before failover hello existing Primary, you should take these steps within hello Cluster Failover Manager:</span></span>

<span data-ttu-id="26764-539">tooadd IP adresu, najdete v části hello [příloha](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), krok 14.</span><span class="sxs-lookup"><span data-stu-id="26764-539">tooadd in IP Address, see hello [Appendix](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), step 14.</span></span>

1. <span data-ttu-id="26764-540">Pro hello aktuální prostředek IP adresu, změnit hello možných vlastníků too'Existing primárního serveru SQL', v níže 'dansqlams4' hello příkladu:</span><span class="sxs-lookup"><span data-stu-id="26764-540">For hello current IP Address resource, change hello possible owner too‘Existing Primary SQL Server’, in hello example below, ‘dansqlams4’:</span></span>

    ![Appendix13][23]
2. <span data-ttu-id="26764-542">Pro hello nový prostředek IP adresu, změnit hello možných vlastníků too'Migrated sekundárního serveru SQL', v níže 'dansqlams5' hello příkladu:</span><span class="sxs-lookup"><span data-stu-id="26764-542">For hello new IP Address resource, change hello possible owner too‘Migrated secondary SQL Server’, in hello example below, ‘dansqlams5’:</span></span>

    ![Appendix14][24]
3. <span data-ttu-id="26764-544">Toto nastavení můžete převzetí služeb při selhání, a při migraci poslední uzel hello Možní vlastníci hello by měl být upraven tak, že uzel je přidán jako možného vlastníka:</span><span class="sxs-lookup"><span data-stu-id="26764-544">Once this is set you can failover, and when hello last node is migrated hello Possible Owners should be edited so that node is added as a Possible Owner:</span></span>

    ![Appendix15][25]

## <a name="additional-resources"></a><span data-ttu-id="26764-546">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="26764-546">Additional resources</span></span>
* [<span data-ttu-id="26764-547">Azure Premium Storage</span><span class="sxs-lookup"><span data-stu-id="26764-547">Azure Premium Storage</span></span>](../../../storage/common/storage-premium-storage.md)
* [<span data-ttu-id="26764-548">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="26764-548">Virtual Machines</span></span>](https://azure.microsoft.com/services/virtual-machines/)
* [<span data-ttu-id="26764-549">Systému SQL Server na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="26764-549">SQL Server in Azure Virtual Machines</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
