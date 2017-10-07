---
title: "aaaConfigure hello skupiny dostupnosti Always On na virtuální počítač Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento kurz používá prostředky, které byly vytvořené pomocí modelu nasazení classic hello. Použijte PowerShell toocreate skupiny dostupnosti Always On v Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d4a27e203b2ff299adebec2b010c03422459b3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-always-on-availability-group-on-an-azure-vm-with-powershell"></a><span data-ttu-id="15843-104">Konfigurace skupiny dostupnosti Always On hello na virtuální počítač Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="15843-104">Configure hello Always On availability group on an Azure VM with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="15843-105">Klasické: uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="15843-105">Classic: UI</span></span>](../classic/portal-sql-alwayson-availability-groups.md)
> * <span data-ttu-id="15843-106">[Klasické: prostředí PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span><span class="sxs-lookup"><span data-stu-id="15843-106">[Classic: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span></span><br/>

<span data-ttu-id="15843-107">Než začnete, vezměte v úvahu, že je nyní možné dokončit tuto úlohu v modelu Azure resource manager.</span><span class="sxs-lookup"><span data-stu-id="15843-107">Before you begin, consider that you can now complete this task in Azure resource manager model.</span></span> <span data-ttu-id="15843-108">Doporučujeme, abyste model nástroje Správce prostředků Azure pro nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="15843-108">We recommend Azure resource manager model for new deployments.</span></span> <span data-ttu-id="15843-109">V tématu [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="15843-109">See [SQL Server Always On availability groups on Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15843-110">Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="15843-110">We recommend that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="15843-111">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="15843-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="15843-112">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="15843-112">This article covers using hello classic deployment model.</span></span>

<span data-ttu-id="15843-113">Virtuální počítače Azure (VM) může pomoct náklady na správci toolower hello databáze systému SQL Server vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="15843-113">Azure virtual machines (VMs) can help database administrators toolower hello cost of a high-availability SQL Server system.</span></span> <span data-ttu-id="15843-114">Tento kurz ukazuje, jak tooimplement dostupnosti skupiny pomocí SQL serveru Always On klient server v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="15843-114">This tutorial shows you how tooimplement an availability group by using SQL Server Always On end-to-end inside an Azure environment.</span></span> <span data-ttu-id="15843-115">Na konci hello hello kurzu vaše řešení SQL serveru Always On v Azure budou tvořeny hello následující prvky:</span><span class="sxs-lookup"><span data-stu-id="15843-115">At hello end of hello tutorial, your SQL Server Always On solution in Azure will consist of hello following elements:</span></span>

* <span data-ttu-id="15843-116">Virtuální síť, která obsahuje více podsítí, včetně front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="15843-116">A virtual network that contains multiple subnets, including a front-end and a back-end subnet.</span></span>
* <span data-ttu-id="15843-117">Řadič domény s doménou služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="15843-117">A domain controller with an Active Directory domain.</span></span>
* <span data-ttu-id="15843-118">Dva SQL serveru virtuálních počítačů, které jsou nasazené toohello back-end podsíť a toohello připojené k doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="15843-118">Two SQL Server VMs that are deployed toohello back-end subnet and joined toohello Active Directory domain.</span></span>
* <span data-ttu-id="15843-119">Tři uzly clusteru převzetí služeb při selhání systému Windows s modelem kvora Většina uzlů hello.</span><span class="sxs-lookup"><span data-stu-id="15843-119">A three-node Windows failover cluster with hello Node Majority quorum model.</span></span>
* <span data-ttu-id="15843-120">Skupina dostupnosti databáze dostupnosti s dvěma replik se synchronním potvrzováním.</span><span class="sxs-lookup"><span data-stu-id="15843-120">An availability group with two synchronous-commit replicas of an availability database.</span></span>

<span data-ttu-id="15843-121">Tento scénář je vhodný pro jeho jednoduchost v Azure, ne pro jeho nákladová efektivnost nebo dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="15843-121">This scenario is a good choice for its simplicity on Azure, not for its cost-effectiveness or other factors.</span></span> <span data-ttu-id="15843-122">Například můžete minimalizovat hello počet virtuálních počítačů pro toosave dvě repliky dostupnosti skupiny na výpočetní hodiny v Azure pomocí řadiče domény hello jako určující sdílená složka souboru hello kvora v clusteru s podporou převzetí služeb při selhání dvěma uzly.</span><span class="sxs-lookup"><span data-stu-id="15843-122">For example, you can minimize hello number of VMs for a two-replica availability group toosave on compute hours in Azure by using hello domain controller as hello quorum file share witness in a two-node failover cluster.</span></span> <span data-ttu-id="15843-123">Tato metoda snižuje počet hello virtuálních počítačů pomocí jedné z hello výše konfigurace.</span><span class="sxs-lookup"><span data-stu-id="15843-123">This method reduces hello VM count by one from hello above configuration.</span></span>

<span data-ttu-id="15843-124">Tento kurz je určen, že tooshow hello kroky, které jsou požadované tooset až hello popsané výše, řešení bez vypracování na hello podrobnosti o jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="15843-124">This tutorial is intended tooshow you hello steps that are required tooset up hello described solution above, without elaborating on hello details of each step.</span></span> <span data-ttu-id="15843-125">Proto místo Pokud kroků konfigurace hello grafickým uživatelským rozhraním, používá prostředí PowerShell skriptování tootake můžete rychle přes každý krok.</span><span class="sxs-lookup"><span data-stu-id="15843-125">Therefore, instead of providing hello GUI configuration steps, it uses PowerShell scripting tootake you quickly through each step.</span></span> <span data-ttu-id="15843-126">Tento kurz předpokládá hello následující:</span><span class="sxs-lookup"><span data-stu-id="15843-126">This tutorial assumes hello following:</span></span>

* <span data-ttu-id="15843-127">Už máte účet Azure s předplatným hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="15843-127">You already have an Azure account with hello virtual machine subscription.</span></span>
* <span data-ttu-id="15843-128">Jste nainstalovali hello [rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="15843-128">You've installed hello [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>
* <span data-ttu-id="15843-129">Už máte plnou Principy Always On skupin dostupnosti pro místní řešení.</span><span class="sxs-lookup"><span data-stu-id="15843-129">You already have a solid understanding of Always On availability groups for on-premises solutions.</span></span> <span data-ttu-id="15843-130">Další informace najdete v tématu [Always On skupiny dostupnosti (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="15843-130">For more information, see [Always On availability groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span></span>

## <a name="connect-tooyour-azure-subscription-and-create-hello-virtual-network"></a><span data-ttu-id="15843-131">Připojení tooyour předplatného Azure a vytvořit virtuální síť hello</span><span class="sxs-lookup"><span data-stu-id="15843-131">Connect tooyour Azure subscription and create hello virtual network</span></span>
1. <span data-ttu-id="15843-132">V okně prostředí PowerShell v místním počítači importovat hello Azure modulu, stáhněte hello počítač tooyour soubor nastavení publikování a připojit vaše tooyour relace prostředí PowerShell předplatného Azure importováním hello stáhnout nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="15843-132">In a PowerShell window on your local computer, import hello Azure module, download hello publishing settings file tooyour machine, and connect your PowerShell session tooyour Azure subscription by importing hello downloaded publishing settings.</span></span>

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    <span data-ttu-id="15843-133">Hello **Get-AzurePublishSettingsFile** příkaz automaticky vygeneruje certifikát pro správu s Azure a stáhne je tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="15843-133">hello **Get-AzurePublishSettingsFile** command automatically generates a management certificate with Azure and downloads it tooyour machine.</span></span> <span data-ttu-id="15843-134">Prohlížeč se automaticky otevře a vy budete výzvami tooenter přihlašovacích údajů účtu Microsoft hello vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="15843-134">A browser is automatically opened, and you're prompted tooenter hello Microsoft account credentials for your Azure subscription.</span></span> <span data-ttu-id="15843-135">Hello Stáhnout **.publishsettings** soubor obsahuje všechny informace hello, je nutné toomanage vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="15843-135">hello downloaded **.publishsettings** file contains all hello information that you need toomanage your Azure subscription.</span></span> <span data-ttu-id="15843-136">Po uložení tento soubor tooa místní adresář, ho importovat pomocí hello **Import AzurePublishSettingsFile** příkaz.</span><span class="sxs-lookup"><span data-stu-id="15843-136">After saving this file tooa local directory, import it by using hello **Import-AzurePublishSettingsFile** command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="15843-137">soubor .publishsettings Hello obsahuje vaše pověření (nekódovaných), které jsou používané tooadminister vaše předplatná Azure a služby.</span><span class="sxs-lookup"><span data-stu-id="15843-137">hello .publishsettings file contains your credentials (unencoded) that are used tooadminister your Azure subscriptions and services.</span></span> <span data-ttu-id="15843-138">Hello osvědčený postup zabezpečení pro tento soubor je toostore ho dočasně mimo adresáře zdroje (například ve složce Libraries\Documents hello) a poté jej odstraňte po dokončení importu hello.</span><span class="sxs-lookup"><span data-stu-id="15843-138">hello security best practice for this file is toostore it temporarily outside your source directories (for example, in hello Libraries\Documents folder), and then delete it after hello import has finished.</span></span> <span data-ttu-id="15843-139">Uživatel se zlými úmysly, který získá přístup k souboru .publishsettings toohello můžete upravit, vytvoření a odstranění služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="15843-139">A malicious user who gains access toohello .publishsettings file can edit, create, and delete your Azure services.</span></span>

2. <span data-ttu-id="15843-140">Definujte řady proměnných, které použijete toocreate vaší cloudové infrastruktury IT.</span><span class="sxs-lookup"><span data-stu-id="15843-140">Define a series of variables that you'll use toocreate your cloud IT infrastructure.</span></span>

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    <span data-ttu-id="15843-141">Věnovat pozornost toohello následující tooensure, který bude později úspěšné příkazech:</span><span class="sxs-lookup"><span data-stu-id="15843-141">Pay attention toohello following tooensure that your commands will succeed later:</span></span>

   * <span data-ttu-id="15843-142">Proměnné **$storageAccountName** a **$dcServiceName** musí být jedinečný, protože jsou použít tooidentify účet cloudového úložiště a cloudovými servery však v uvedeném pořadí, v hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="15843-142">Variables **$storageAccountName** and **$dcServiceName** must be unique because they're used tooidentify your cloud storage account and cloud server, respectively, on hello Internet.</span></span>
   * <span data-ttu-id="15843-143">Hello názvy, které zadáte pro proměnné **$affinityGroupName** a **$virtualNetworkName** konfigurované v dokumentu konfigurace virtuální sítě hello, které budete používat později.</span><span class="sxs-lookup"><span data-stu-id="15843-143">hello names that you specify for variables **$affinityGroupName** and **$virtualNetworkName** are configured in hello virtual network configuration document that you'll use later.</span></span>
   * <span data-ttu-id="15843-144">**$sqlImageName** určuje hello aktualizovat název hello image virtuálního počítače, který obsahuje SQL Server 2012 Service Pack 1 Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="15843-144">**$sqlImageName** specifies hello updated name of hello VM image that contains SQL Server 2012 Service Pack 1 Enterprise Edition.</span></span>
   * <span data-ttu-id="15843-145">Pro jednoduchost **Contoso! 000** je hello stejné heslo, které se používá napříč celou kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="15843-145">For simplicity, **Contoso!000** is hello same password that's used throughout hello entire tutorial.</span></span>

3. <span data-ttu-id="15843-146">Vytvořte skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="15843-146">Create an affinity group.</span></span>

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. <span data-ttu-id="15843-147">Vytvoření virtuální sítě pomocí importu konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="15843-147">Create a virtual network by importing a configuration file.</span></span>

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    <span data-ttu-id="15843-148">Hello konfigurační soubor obsahuje hello následující dokument XML.</span><span class="sxs-lookup"><span data-stu-id="15843-148">hello configuration file contains hello following XML document.</span></span> <span data-ttu-id="15843-149">Stručně řečeno, určuje virtuální sítě s názvem **ContosoNET** ve skupině vztahů hello názvem **ContosoAG**.</span><span class="sxs-lookup"><span data-stu-id="15843-149">In brief, it specifies a virtual network called **ContosoNET** in hello affinity group called **ContosoAG**.</span></span> <span data-ttu-id="15843-150">Má hello adresní prostor **10.10.0.0/16** a má dvě podsítě, **10.10.1.0/24** a **10.10.2.0/24**, které jsou v uvedeném pořadí hello podsítě front a zpět podsíť.</span><span class="sxs-lookup"><span data-stu-id="15843-150">It has hello address space **10.10.0.0/16** and has two subnets, **10.10.1.0/24** and **10.10.2.0/24**, which are hello front subnet and back subnet, respectively.</span></span> <span data-ttu-id="15843-151">Hello front podsíť je, kde můžete umístit klientské aplikace, třeba Microsoft SharePoint.</span><span class="sxs-lookup"><span data-stu-id="15843-151">hello front subnet is where you can place client applications such as Microsoft SharePoint.</span></span> <span data-ttu-id="15843-152">je back podsíť Hello budete umístění virtuálních počítačů hello SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="15843-152">hello back subnet is where you'll place hello SQL Server VMs.</span></span> <span data-ttu-id="15843-153">Pokud změníte hello **$affinityGroupName** a **$virtualNetworkName** proměnné starší, musíte změnit taky hello odpovídající názvy níže.</span><span class="sxs-lookup"><span data-stu-id="15843-153">If you change hello **$affinityGroupName** and **$virtualNetworkName** variables earlier, you must also change hello corresponding names below.</span></span>

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. <span data-ttu-id="15843-154">Vytvořte účet úložiště, který je spojen s hello skupinu vztahů, že jste vytvořili a nastavte ji jako hello aktuální účet úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="15843-154">Create a storage account that's associated with hello affinity group that you created, and set it as hello current storage account in your subscription.</span></span>

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. <span data-ttu-id="15843-155">Vytvořte server řadiče domény hello sady hello nové cloudové služby a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="15843-155">Create hello domain controller server in hello new cloud service and availability set.</span></span>

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    <span data-ttu-id="15843-156">Tyto příkazy vytvoření kanálu hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="15843-156">These piped commands do hello following things:</span></span>

   * <span data-ttu-id="15843-157">**Nové AzureVMConfig** vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="15843-157">**New-AzureVMConfig** creates a VM configuration.</span></span>
   * <span data-ttu-id="15843-158">**Přidat AzureProvisioningConfig** poskytuje hello konfigurační parametry samostatný server systému Windows.</span><span class="sxs-lookup"><span data-stu-id="15843-158">**Add-AzureProvisioningConfig** gives hello configuration parameters of a standalone Windows server.</span></span>
   * <span data-ttu-id="15843-159">**Přidat AzureDataDisk** přidá hello datový disk, které budete používat pro ukládání dat služby Active Directory s hello tooNone sadu možnost ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="15843-159">**Add-AzureDataDisk** adds hello data disk that you'll use for storing Active Directory data, with hello caching option set tooNone.</span></span>
   * <span data-ttu-id="15843-160">**Nový-AzureVM** vytvoří novou cloudovou službu a vytvoří hello nového virtuálního počítače Azure v hello novou cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="15843-160">**New-AzureVM** creates a new cloud service and creates hello new Azure VM in hello new cloud service.</span></span>

7. <span data-ttu-id="15843-161">Počkejte hello nový virtuální počítač toobe plně zřízený a stáhnout hello souboru vzdálené plochy tooyour pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="15843-161">Wait for hello new VM toobe fully provisioned, and download hello remote desktop file tooyour working directory.</span></span> <span data-ttu-id="15843-162">Vzhledem k tomu hello nový virtuální počítač Azure používá tooprovision dlouhou dobu, hello `while` smyčky pokračuje toopoll hello nový virtuální počítač, dokud je připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="15843-162">Because hello new Azure VM takes a long time tooprovision, hello `while` loop continues toopoll hello new VM until it's ready for use.</span></span>

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

<span data-ttu-id="15843-163">Nyní je úspěšně zřízen Hello server řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="15843-163">hello domain controller server is now successfully provisioned.</span></span> <span data-ttu-id="15843-164">Dále nakonfigurujete hello domény služby Active Directory na tomto serveru řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="15843-164">Next, you'll configure hello Active Directory domain on this domain controller server.</span></span> <span data-ttu-id="15843-165">Nechte okno prostředí PowerShell hello otevřené v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="15843-165">Leave hello PowerShell window open on your local computer.</span></span> <span data-ttu-id="15843-166">Použijete jej znovu novější toocreate hello dva virtuální počítače SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="15843-166">You'll use it again later toocreate hello two SQL Server VMs.</span></span>

## <a name="configure-hello-domain-controller"></a><span data-ttu-id="15843-167">Konfigurace řadiče domény hello</span><span class="sxs-lookup"><span data-stu-id="15843-167">Configure hello domain controller</span></span>
1. <span data-ttu-id="15843-168">Připojte server řadiče domény toohello spuštěním souboru vzdálené plochy hello.</span><span class="sxs-lookup"><span data-stu-id="15843-168">Connect toohello domain controller server by launching hello remote desktop file.</span></span> <span data-ttu-id="15843-169">Uživatelské jméno AzureAdmin a heslo správce počítače hello **Contoso! 000**, který jste zadali při vytvoření hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="15843-169">Use hello machine administrator’s username AzureAdmin and password **Contoso!000**, which you specified when you created hello new VM.</span></span>
2. <span data-ttu-id="15843-170">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="15843-170">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="15843-171">Spusťte následující hello **DCPROMO. EXE** tooset příkaz až hello **corp.contoso.com** domény s hello datové adresáře na disku M.</span><span class="sxs-lookup"><span data-stu-id="15843-171">Run hello following **DCPROMO.EXE** command tooset up hello **corp.contoso.com** domain, with hello data directories on drive M.</span></span>

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    <span data-ttu-id="15843-172">Po dokončení příkazu hello hello virtuální počítač se automaticky restartuje.</span><span class="sxs-lookup"><span data-stu-id="15843-172">After hello command finishes, hello VM restarts automatically.</span></span>

4. <span data-ttu-id="15843-173">Znovu připojte server řadiče domény toohello spuštěním souboru vzdálené plochy hello.</span><span class="sxs-lookup"><span data-stu-id="15843-173">Connect toohello domain controller server again by launching hello remote desktop file.</span></span> <span data-ttu-id="15843-174">Tentokrát, přihlaste se jako **CORP\Administrator**.</span><span class="sxs-lookup"><span data-stu-id="15843-174">This time, sign in as **CORP\Administrator**.</span></span>
5. <span data-ttu-id="15843-175">Otevřete okno prostředí PowerShell v režimu správce a importujte modul Active Directory PowerShell hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15843-175">Open a PowerShell window in administrator mode, and import hello Active Directory PowerShell module by using hello following command:</span></span>

        Import-Module ActiveDirectory

6. <span data-ttu-id="15843-176">Spusťte následující příkazy tooadd tři uživatelé toohello domény hello.</span><span class="sxs-lookup"><span data-stu-id="15843-176">Run hello following commands tooadd three users toohello domain.</span></span>

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    <span data-ttu-id="15843-177">**CORP\Install** je použité tooconfigure všechno související toohello instance služby SQL Server, hello převzetí služeb při selhání clusteru a skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="15843-177">**CORP\Install** is used tooconfigure everything related toohello SQL Server service instances, hello failover cluster, and hello availability group.</span></span> <span data-ttu-id="15843-178">**CORP\SQLSvc1** a **CORP\SQLSvc2** jsou použity jako účty služby SQL Server hello pro hello dva virtuální počítače serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="15843-178">**CORP\SQLSvc1** and **CORP\SQLSvc2** are used as hello SQL Server service accounts for hello two SQL Server VMs.</span></span>
7. <span data-ttu-id="15843-179">Hello další, spusťte následující příkazy toogive **CORP\Install** hello oprávnění toocreate počítačových objektů v doméně hello.</span><span class="sxs-lookup"><span data-stu-id="15843-179">Next, run hello following commands toogive **CORP\Install** hello permissions toocreate computer objects in hello domain.</span></span>

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    <span data-ttu-id="15843-180">Hello výše zadaný identifikátor GUID je hello identifikátor GUID pro typ objektu počítače hello.</span><span class="sxs-lookup"><span data-stu-id="15843-180">hello GUID specified above is hello GUID for hello computer object type.</span></span> <span data-ttu-id="15843-181">Hello **CORP\Install** účet potřebuje hello **čtení všech vlastností** a **vytvářet objekty počítačů** oprávnění toocreate hello Active přímé objekty hello převzetí služeb při selhání cluster.</span><span class="sxs-lookup"><span data-stu-id="15843-181">hello **CORP\Install** account needs hello **Read All Properties** and **Create Computer Objects** permission toocreate hello Active Direct objects for hello failover cluster.</span></span> <span data-ttu-id="15843-182">Hello **čtení všech vlastností** oprávnění je již přidělena tooCORP\Install ve výchozím nastavení, takže není nutné toogrant jej explicitně.</span><span class="sxs-lookup"><span data-stu-id="15843-182">hello **Read All Properties** permission is already given tooCORP\Install by default, so you don't need toogrant it explicitly.</span></span> <span data-ttu-id="15843-183">Další informace o oprávnění, které jsou potřeba toocreate hello převzetí služeb při selhání clusteru, najdete v části [převzetí služeb při selhání clusteru podrobný průvodce: Konfigurace účty ve službě Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="15843-183">For more information on permissions that are needed toocreate hello failover cluster, see [Failover Cluster Step-by-Step Guide: Configuring Accounts in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span></span>

    <span data-ttu-id="15843-184">Teď, když jste dokončili konfigurace služby Active Directory a hello uživatelské objekty, vytvoříte dva virtuální počítače serveru SQL a připojovat je toothis domény.</span><span class="sxs-lookup"><span data-stu-id="15843-184">Now that you've finished configuring Active Directory and hello user objects, you'll create two SQL Server VMs and join them toothis domain.</span></span>

## <a name="create-hello-sql-server-vms"></a><span data-ttu-id="15843-185">Vytvoření virtuálních počítačů hello SQL serveru</span><span class="sxs-lookup"><span data-stu-id="15843-185">Create hello SQL Server VMs</span></span>
1. <span data-ttu-id="15843-186">Pokračujte v okně prostředí PowerShell hello toouse, které je otevřený v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="15843-186">Continue toouse hello PowerShell window that's open on your local computer.</span></span> <span data-ttu-id="15843-187">Definujte hello následující další proměnné:</span><span class="sxs-lookup"><span data-stu-id="15843-187">Define hello following additional variables:</span></span>

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    <span data-ttu-id="15843-188">Hello IP adresu **10.10.0.4** je obvykle přiřazena toohello první virtuální počítač, který vytvoříte v hello **10.10.0.0/16** podsíť virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="15843-188">hello IP address **10.10.0.4** is typically assigned toohello first VM that you create in hello **10.10.0.0/16** subnet of your Azure virtual network.</span></span> <span data-ttu-id="15843-189">By měl ověřit, zda je hello adresu serveru řadiče domény tak, že spustíte **IPCONFIG**.</span><span class="sxs-lookup"><span data-stu-id="15843-189">You should verify that this is hello address of your domain controller server by running **IPCONFIG**.</span></span>
2. <span data-ttu-id="15843-190">S názvem virtuální počítač v clusteru převzetí služeb při selhání hello spuštění hello po vytvoření kanálu příkazy toocreate hello nejprve **ContosoQuorum**:</span><span class="sxs-lookup"><span data-stu-id="15843-190">Run hello following piped commands toocreate hello first VM in hello failover cluster, named **ContosoQuorum**:</span></span>

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    <span data-ttu-id="15843-191">Vezměte na vědomí následující hello týkající se nahoře na příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="15843-191">Note hello following regarding hello command above:</span></span>

   * <span data-ttu-id="15843-192">**Nové AzureVMConfig** vytvoří konfigurace virtuálního počítače se název sady dostupnosti požadované hello.</span><span class="sxs-lookup"><span data-stu-id="15843-192">**New-AzureVMConfig** creates a VM configuration with hello desired availability set name.</span></span> <span data-ttu-id="15843-193">Hello dalších virtuálních počítačů bude vytvořena s hello stejný název sady dostupnosti tak, aby se v připojené k toohello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="15843-193">hello subsequent VMs will be created with hello same availability set name so that they're joined toohello same availability set.</span></span>
   * <span data-ttu-id="15843-194">**Přidat AzureProvisioningConfig** spojení hello domény služby Active Directory toohello virtuálního počítače, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="15843-194">**Add-AzureProvisioningConfig** joins hello VM toohello Active Directory domain that you created.</span></span>
   * <span data-ttu-id="15843-195">**Set-AzureSubnet** místech hello virtuálního počítače v podsíti back hello.</span><span class="sxs-lookup"><span data-stu-id="15843-195">**Set-AzureSubnet** places hello VM in hello back subnet.</span></span>
   * <span data-ttu-id="15843-196">**Nový-AzureVM** vytvoří novou cloudovou službu a vytvoří hello nového virtuálního počítače Azure v hello novou cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="15843-196">**New-AzureVM** creates a new cloud service and creates hello new Azure VM in hello new cloud service.</span></span> <span data-ttu-id="15843-197">Hello **DnsSettings** parametr určuje tento server DNS hello pro hello servery v hello nové cloudové služby má hello IP adresu **10.10.0.4**.</span><span class="sxs-lookup"><span data-stu-id="15843-197">hello **DnsSettings** parameter specifies that hello DNS server for hello servers in hello new cloud service has hello IP address **10.10.0.4**.</span></span> <span data-ttu-id="15843-198">Toto je IP adresa hello hello server řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="15843-198">This is hello IP address of hello domain controller server.</span></span> <span data-ttu-id="15843-199">Tento parametr je potřeba tooenable hello nové virtuální počítače v doméně služby Active Directory hello cloudové služby toojoin toohello úspěšně.</span><span class="sxs-lookup"><span data-stu-id="15843-199">This parameter is needed tooenable hello new VMs in hello cloud service toojoin toohello Active Directory domain successfully.</span></span> <span data-ttu-id="15843-200">Bez tohoto parametru je nutné ručně nastavit hello nastavením IPv4 na serveru řadiče domény hello toouse virtuálních počítačů jako primární server DNS hello po hello virtuálního počítače je zřízený a pak připojit k doméně služby Active Directory toohello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="15843-200">Without this parameter, you must manually set hello IPv4 settings in your VM toouse hello domain controller server as hello primary DNS server after hello VM is provisioned, and then join hello VM toohello Active Directory domain.</span></span>
3. <span data-ttu-id="15843-201">Po vytvoření kanálu spuštění hello příkazy hello toocreate virtuálním počítačům systému SQL Server, s názvem **ContosoSQL1** a **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="15843-201">Run hello following piped commands toocreate hello SQL Server VMs, named **ContosoSQL1** and **ContosoSQL2**.</span></span>

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    <span data-ttu-id="15843-202">Vezměte na vědomí následující hello týkající se příkazy hello výše:</span><span class="sxs-lookup"><span data-stu-id="15843-202">Note hello following regarding hello commands above:</span></span>

   * <span data-ttu-id="15843-203">**Nové AzureVMConfig** hello používá stejný název sady dostupnosti jako server řadiče domény hello a hello používá SQL Server 2012 Service Pack 1 Enterprise Edition obrázek v galerii virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="15843-203">**New-AzureVMConfig** uses hello same availability set name as hello domain controller server, and uses hello SQL Server 2012 Service Pack 1 Enterprise Edition image in hello virtual machine gallery.</span></span> <span data-ttu-id="15843-204">Nastaví taky hello operačního systému disku tooread ukládání do mezipaměti pouze (bez ukládání do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="15843-204">It also sets hello operating system disk tooread-caching only (no write caching).</span></span> <span data-ttu-id="15843-205">Doporučujeme vám, že migrujete hello databáze soubory tooa samostatné datový disk připojit toohello virtuální počítač a konfigurovat žádné čtení nebo ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="15843-205">We recommend that you migrate hello database files tooa separate data disk that you attach toohello VM, and configure it with no read or write caching.</span></span> <span data-ttu-id="15843-206">Hello skoro stejné je však, že tooremove ukládání do mezipaměti na disku operačního systému hello vzhledem k tomu, že nemůžete odebrat mezipaměti pro čtení na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="15843-206">However, hello next best thing is tooremove write caching on hello operating system disk because you can't remove read caching on hello operating system disk.</span></span>
   * <span data-ttu-id="15843-207">**Přidat AzureProvisioningConfig** spojení hello domény služby Active Directory toohello virtuálního počítače, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="15843-207">**Add-AzureProvisioningConfig** joins hello VM toohello Active Directory domain that you created.</span></span>
   * <span data-ttu-id="15843-208">**Set-AzureSubnet** místech hello virtuálního počítače v podsíti back hello.</span><span class="sxs-lookup"><span data-stu-id="15843-208">**Set-AzureSubnet** places hello VM in hello back subnet.</span></span>
   * <span data-ttu-id="15843-209">**Přidat AzureEndpoint** přidá přístup koncových bodů, aby tyto instance služby SQL Server na hello Internet přístup klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="15843-209">**Add-AzureEndpoint** adds access endpoints so that client applications can access these SQL Server services instances on hello Internet.</span></span> <span data-ttu-id="15843-210">Jiné porty jsou uvedeny tooContosoSQL1 a ContosoSQL2.</span><span class="sxs-lookup"><span data-stu-id="15843-210">Different ports are given tooContosoSQL1 and ContosoSQL2.</span></span>
   * <span data-ttu-id="15843-211">**Nový-AzureVM** vytvoří hello nový virtuální počítač SQL Server v hello stejné cloudové služby jako ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="15843-211">**New-AzureVM** creates hello new SQL Server VM in hello same cloud service as ContosoQuorum.</span></span> <span data-ttu-id="15843-212">Virtuální počítače hello musí být ve stejné cloudové služby pokud je chcete toobe v hello hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="15843-212">You must place hello VMs in hello same cloud service if you want them toobe in hello same availability set.</span></span>
4. <span data-ttu-id="15843-213">Počkejte, než pro každý virtuální počítač toobe plně zřízený a pro každý virtuální počítač toodownload jeho souboru vzdálené plochy tooyour pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="15843-213">Wait for each VM toobe fully provisioned and for each VM toodownload its remote desktop file tooyour working directory.</span></span> <span data-ttu-id="15843-214">Hello `for` smyček procházení hello tři nové virtuální počítače a spouští příkazy hello uvnitř hello nejvyšší úrovně složené závorky pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="15843-214">hello `for` loop cycles through hello three new VMs and executes hello commands inside hello top-level curly brackets for each of them.</span></span>

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until hello VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    <span data-ttu-id="15843-215">Hello virtuálních počítačů serveru SQL je nyní opatřen a spuštěná, ale instalované se systémem SQL Server s výchozími možnostmi.</span><span class="sxs-lookup"><span data-stu-id="15843-215">hello SQL Server VMs are now provisioned and running, but they're installed with SQL Server with default options.</span></span>

## <a name="initialize-hello-failover-cluster-vms"></a><span data-ttu-id="15843-216">Inicializace hello převzetí služeb při selhání clusteru virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="15843-216">Initialize hello failover cluster VMs</span></span>
<span data-ttu-id="15843-217">V této části je nutné hello toomodify tři se servery, které budete používat v clusteru převzetí služeb při selhání hello a instalace systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="15843-217">In this section, you need toomodify hello three servers that you'll use in hello failover cluster and hello SQL Server installation.</span></span> <span data-ttu-id="15843-218">Zejména:</span><span class="sxs-lookup"><span data-stu-id="15843-218">Specifically:</span></span>

* <span data-ttu-id="15843-219">Všechny servery: budete potřebovat tooinstall hello **Clustering převzetí služeb při selhání** funkce.</span><span class="sxs-lookup"><span data-stu-id="15843-219">All servers: You need tooinstall hello **Failover Clustering** feature.</span></span>
* <span data-ttu-id="15843-220">Všechny servery: budete potřebovat tooadd **CORP\Install** jako počítač hello **správce**.</span><span class="sxs-lookup"><span data-stu-id="15843-220">All servers: You need tooadd **CORP\Install** as hello machine **administrator**.</span></span>
* <span data-ttu-id="15843-221">ContosoSQL1 a ContosoSQL2 pouze: budete potřebovat tooadd **CORP\Install** jako **sysadmin** role v hello výchozí databáze.</span><span class="sxs-lookup"><span data-stu-id="15843-221">ContosoSQL1 and ContosoSQL2 only: You need tooadd **CORP\Install** as a **sysadmin** role in hello default database.</span></span>
* <span data-ttu-id="15843-222">ContosoSQL1 a ContosoSQL2 pouze: budete potřebovat tooadd **NT AUTHORITY\System** jako Přihlaste se pomocí hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="15843-222">ContosoSQL1 and ContosoSQL2 only: You need tooadd **NT AUTHORITY\System** as a sign-in with hello following permissions:</span></span>

  * <span data-ttu-id="15843-223">Příkaz ALTER žádnou skupinu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="15843-223">Alter any availability group</span></span>
  * <span data-ttu-id="15843-224">Připojení SQL</span><span class="sxs-lookup"><span data-stu-id="15843-224">Connect SQL</span></span>
  * <span data-ttu-id="15843-225">Zobrazení stavu serveru</span><span class="sxs-lookup"><span data-stu-id="15843-225">View server state</span></span>
* <span data-ttu-id="15843-226">ContosoSQL1 a ContosoSQL2 pouze: hello **TCP** na hello virtuální počítač SQL Server je již povolen protokol.</span><span class="sxs-lookup"><span data-stu-id="15843-226">ContosoSQL1 and ContosoSQL2 only: hello **TCP** protocol is already enabled on hello SQL Server VM.</span></span> <span data-ttu-id="15843-227">Stále však tooopen hello brány firewall pro vzdálený přístup systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15843-227">However, you still need tooopen hello firewall for remote access of SQL Server.</span></span>

<span data-ttu-id="15843-228">Nyní jste toostart připraven.</span><span class="sxs-lookup"><span data-stu-id="15843-228">Now, you're ready toostart.</span></span> <span data-ttu-id="15843-229">Počínaje **ContosoQuorum**, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="15843-229">Beginning with **ContosoQuorum**, follow hello steps below:</span></span>

1. <span data-ttu-id="15843-230">Připojit příliš**ContosoQuorum** spuštěním hello soubory vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="15843-230">Connect too**ContosoQuorum** by launching hello remote desktop files.</span></span> <span data-ttu-id="15843-231">Použijte uživatelské jméno správce počítače hello **AzureAdmin** a heslo **Contoso! 000**, který jste zadali při vytváření virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="15843-231">Use hello machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created hello VMs.</span></span>
2. <span data-ttu-id="15843-232">Ověřte, zda text hello počítače mít byla úspěšně připojeni příliš**corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="15843-232">Verify that hello computers have been successfully joined too**corp.contoso.com**.</span></span>
3. <span data-ttu-id="15843-233">Počkejte toofinish instalace systému SQL Server hello systémem hello automatizované úlohy inicializace než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="15843-233">Wait for hello SQL Server installation toofinish running hello automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="15843-234">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="15843-234">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="15843-235">Nainstalujte funkci Clustering převzetí služeb při selhání Windows hello.</span><span class="sxs-lookup"><span data-stu-id="15843-235">Install hello Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="15843-236">Přidat **CORP\Install** jako místní správce.</span><span class="sxs-lookup"><span data-stu-id="15843-236">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="15843-237">Odhlásit z ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="15843-237">Sign out of ContosoQuorum.</span></span> <span data-ttu-id="15843-238">Jste hotovi s tímto serverem teď.</span><span class="sxs-lookup"><span data-stu-id="15843-238">You're done with this server now.</span></span>

        logoff.exe

<span data-ttu-id="15843-239">V dalším kroku inicializovat **ContosoSQL1** a **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="15843-239">Next, initialize **ContosoSQL1** and **ContosoSQL2**.</span></span> <span data-ttu-id="15843-240">Postupujte podle hello kroky, které jsou stejné pro oba virtuální počítače serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="15843-240">Follow hello steps below, which are identical for both SQL Server VMs.</span></span>

1. <span data-ttu-id="15843-241">Připojte virtuální počítače serveru SQL toohello dva spuštěním hello soubory vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="15843-241">Connect toohello two SQL Server VMs by launching hello remote desktop files.</span></span> <span data-ttu-id="15843-242">Použijte uživatelské jméno správce počítače hello **AzureAdmin** a heslo **Contoso! 000**, který jste zadali při vytváření virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="15843-242">Use hello machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created hello VMs.</span></span>
2. <span data-ttu-id="15843-243">Ověřte, zda text hello počítače mít byla úspěšně připojeni příliš**corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="15843-243">Verify that hello computers have been successfully joined too**corp.contoso.com**.</span></span>
3. <span data-ttu-id="15843-244">Počkejte toofinish instalace systému SQL Server hello systémem hello automatizované úlohy inicializace než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="15843-244">Wait for hello SQL Server installation toofinish running hello automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="15843-245">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="15843-245">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="15843-246">Nainstalujte funkci Clustering převzetí služeb při selhání Windows hello.</span><span class="sxs-lookup"><span data-stu-id="15843-246">Install hello Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="15843-247">Přidat **CORP\Install** jako místní správce.</span><span class="sxs-lookup"><span data-stu-id="15843-247">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="15843-248">Importujte hello poskytovatele prostředí PowerShell pro Server SQL.</span><span class="sxs-lookup"><span data-stu-id="15843-248">Import hello SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. <span data-ttu-id="15843-249">Přidat **CORP\Install** jako role sysadmin hello hello výchozí instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="15843-249">Add **CORP\Install** as hello sysadmin role for hello default SQL Server instance.</span></span>

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. <span data-ttu-id="15843-250">Přidat **NT AUTHORITY\System** jako u přihlášení s oprávněními hello tři popsané výše.</span><span class="sxs-lookup"><span data-stu-id="15843-250">Add **NT AUTHORITY\System** as a sign-in with hello three permissions described above.</span></span>

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. <span data-ttu-id="15843-251">Otevřete hello brány firewall pro vzdálený přístup systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15843-251">Open hello firewall for remote access of SQL Server.</span></span>

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. <span data-ttu-id="15843-252">Odhlásit z oba virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="15843-252">Sign out of both VMs.</span></span>

         logoff.exe

<span data-ttu-id="15843-253">Nakonec jste skupiny dostupnosti připravené tooconfigure hello.</span><span class="sxs-lookup"><span data-stu-id="15843-253">Finally, you're ready tooconfigure hello availability group.</span></span> <span data-ttu-id="15843-254">Budete používat hello poskytovatele prostředí PowerShell pro Server SQL tooperform všechny hello pracovat na **ContosoSQL1**.</span><span class="sxs-lookup"><span data-stu-id="15843-254">You'll use hello SQL Server PowerShell Provider tooperform all of hello work on **ContosoSQL1**.</span></span>

## <a name="configure-hello-availability-group"></a><span data-ttu-id="15843-255">Konfigurace skupiny dostupnosti hello</span><span class="sxs-lookup"><span data-stu-id="15843-255">Configure hello availability group</span></span>
1. <span data-ttu-id="15843-256">Připojit příliš**ContosoSQL1** znovu spuštěním hello soubory vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="15843-256">Connect too**ContosoSQL1** again by launching hello remote desktop files.</span></span> <span data-ttu-id="15843-257">Místo přihlášení pomocí účtu počítače hello, přihlaste se pomocí **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="15843-257">Instead of signing in by using hello machine account, sign in by using **CORP\Install**.</span></span>
2. <span data-ttu-id="15843-258">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="15843-258">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="15843-259">Definujte hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="15843-259">Define hello following variables:</span></span>

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. <span data-ttu-id="15843-260">Importujte hello poskytovatele prostředí PowerShell pro Server SQL.</span><span class="sxs-lookup"><span data-stu-id="15843-260">Import hello SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. <span data-ttu-id="15843-261">Změňte účet služby SQL Server hello ContosoSQL1 tooCORP\SQLSvc1.</span><span class="sxs-lookup"><span data-stu-id="15843-261">Change hello SQL Server service account for ContosoSQL1 tooCORP\SQLSvc1.</span></span>

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. <span data-ttu-id="15843-262">Změňte účet služby SQL Server hello ContosoSQL2 tooCORP\SQLSvc2.</span><span class="sxs-lookup"><span data-stu-id="15843-262">Change hello SQL Server service account for ContosoSQL2 tooCORP\SQLSvc2.</span></span>

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. <span data-ttu-id="15843-263">Stáhněte si **CreateAzureFailoverCluster.ps1** z [vytvořit Cluster převzetí služeb při selhání pro skupiny dostupnosti Always On ve virtuálním počítači Azure](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello místní pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="15843-263">Download **CreateAzureFailoverCluster.ps1** from [Create Failover Cluster for Always On Availability Groups in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello local working directory.</span></span> <span data-ttu-id="15843-264">Tento skript toohelp vytvoření clusteru s podporou převzetí služeb při selhání funkční budete používat.</span><span class="sxs-lookup"><span data-stu-id="15843-264">You'll use this script toohelp you create a functional failover cluster.</span></span> <span data-ttu-id="15843-265">Důležité informace o clusteringu Windows převzetí služeb při selhání jak komunikuje s hello Azure sítě najdete v tématu [vysoké dostupnosti a zotavení po havárii pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="15843-265">For important information on how Windows Failover Clustering interacts with hello Azure network, see [High availability and disaster recovery for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>
8. <span data-ttu-id="15843-266">Změňte tooyour pracovní adresář a vytvoření clusteru převzetí služeb při selhání hello pomocí skriptu hello stáhli.</span><span class="sxs-lookup"><span data-stu-id="15843-266">Change tooyour working directory and create hello failover cluster with hello downloaded script.</span></span>

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. <span data-ttu-id="15843-267">Povolte Always On skupiny dostupnosti pro instance systému SQL Server hello výchozí na **ContosoSQL1** a **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="15843-267">Enable Always On availability groups for hello default SQL Server instances on **ContosoSQL1** and **ContosoSQL2**.</span></span>

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. <span data-ttu-id="15843-268">Vytvořte adresář zálohy a udělit oprávnění účtům služby SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="15843-268">Create a backup directory and grant permissions for hello SQL Server service accounts.</span></span> <span data-ttu-id="15843-269">Tuto databázi dostupnosti hello tooprepare directory budete používat v sekundární replice hello.</span><span class="sxs-lookup"><span data-stu-id="15843-269">You'll use this directory tooprepare hello availability database on hello secondary replica.</span></span>

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. <span data-ttu-id="15843-270">Vytvořit databázi na **ContosoSQL1** názvem **MyDB1**, provést úplnou zálohu a zálohu protokolu a obnoví je na **ContosoSQL2** s hello **WITH NORECOVERY** možnost.</span><span class="sxs-lookup"><span data-stu-id="15843-270">Create a database on **ContosoSQL1** called **MyDB1**, take both a full backup and a log backup, and restore them on **ContosoSQL2** with hello **WITH NORECOVERY** option.</span></span>

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. <span data-ttu-id="15843-271">Vytvoření hello dostupnosti skupiny koncových bodů na hello virtuálním počítačům systému SQL Server a nastavte u koncových bodů hello hello příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="15843-271">Create hello availability group endpoints on hello SQL Server VMs and set hello proper permissions on hello endpoints.</span></span>

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct1]" -ServerInstance $server2
13. <span data-ttu-id="15843-272">Vytvořte hello replik dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="15843-272">Create hello availability replicas.</span></span>

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. <span data-ttu-id="15843-273">Nakonec vytvořte hello skupiny dostupnosti a skupinu dostupnosti toohello sekundární repliky hello spojení.</span><span class="sxs-lookup"><span data-stu-id="15843-273">Finally, create hello availability group and join hello secondary replica toohello availability group.</span></span>

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a><span data-ttu-id="15843-274">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15843-274">Next steps</span></span>
<span data-ttu-id="15843-275">Jste teď úspěšně implementovali SQL serveru Always On tak, že vytvoříte skupinu dostupnosti v Azure.</span><span class="sxs-lookup"><span data-stu-id="15843-275">You've now successfully implemented SQL Server Always On by creating an availability group in Azure.</span></span> <span data-ttu-id="15843-276">tooconfigure naslouchací proces pro tuto skupinu dostupnosti, najdete v části [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="15843-276">tooconfigure a listener for this availability group, see [Configure an ILB listener for Always On availability groups in Azure](../classic/ps-sql-int-listener.md).</span></span>

<span data-ttu-id="15843-277">Další informace o používání systému SQL Server v Azure najdete v tématu [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="15843-277">For other information about using SQL Server in Azure, see [SQL Server on Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
