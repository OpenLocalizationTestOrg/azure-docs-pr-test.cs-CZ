---
title: "Konfigurace skupiny dostupnosti Always On na virtuální počítač Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento kurz používá prostředky, které byly vytvořeny s modelem nasazení classic. Použít PowerShell k vytvoření skupiny dostupnosti Always On v Azure."
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
ms.openlocfilehash: b99cf767fb931d3f7fe14fcbe7990126244613ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a><span data-ttu-id="c1881-104">Konfigurace skupiny dostupnosti Always On na virtuální počítač Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1881-104">Configure the Always On availability group on an Azure VM with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c1881-105">Klasické: uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c1881-105">Classic: UI</span></span>](../classic/portal-sql-alwayson-availability-groups.md)
> * <span data-ttu-id="c1881-106">[Klasické: prostředí PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span><span class="sxs-lookup"><span data-stu-id="c1881-106">[Classic: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span></span><br/>

<span data-ttu-id="c1881-107">Než začnete, vezměte v úvahu, že je nyní možné dokončit tuto úlohu v modelu Azure resource manager.</span><span class="sxs-lookup"><span data-stu-id="c1881-107">Before you begin, consider that you can now complete this task in Azure resource manager model.</span></span> <span data-ttu-id="c1881-108">Doporučujeme, abyste model nástroje Správce prostředků Azure pro nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="c1881-108">We recommend Azure resource manager model for new deployments.</span></span> <span data-ttu-id="c1881-109">V tématu [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c1881-109">See [SQL Server Always On availability groups on Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c1881-110">Doporučujeme vám, že většina nových nasazení používala model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c1881-110">We recommend that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="c1881-111">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c1881-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c1881-112">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="c1881-112">This article covers using the classic deployment model.</span></span>

<span data-ttu-id="c1881-113">Virtuální počítače Azure (VM) může pomoct správci databází za účelem snížení nákladů systému SQL Server vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="c1881-113">Azure virtual machines (VMs) can help database administrators to lower the cost of a high-availability SQL Server system.</span></span> <span data-ttu-id="c1881-114">V tomto kurzu se dozvíte, jak implementovat skupinu dostupnosti pomocí SQL serveru Always On klient server v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="c1881-114">This tutorial shows you how to implement an availability group by using SQL Server Always On end-to-end inside an Azure environment.</span></span> <span data-ttu-id="c1881-115">Na konci tohoto kurzu vaše řešení SQL serveru Always On v Azure budou tvořeny následující prvky:</span><span class="sxs-lookup"><span data-stu-id="c1881-115">At the end of the tutorial, your SQL Server Always On solution in Azure will consist of the following elements:</span></span>

* <span data-ttu-id="c1881-116">Virtuální síť, která obsahuje více podsítí, včetně front-end a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="c1881-116">A virtual network that contains multiple subnets, including a front-end and a back-end subnet.</span></span>
* <span data-ttu-id="c1881-117">Řadič domény s doménou služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1881-117">A domain controller with an Active Directory domain.</span></span>
* <span data-ttu-id="c1881-118">Dva SQL serveru virtuálních počítačů, které jsou nasazeny do podsítě back-end a připojený k doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1881-118">Two SQL Server VMs that are deployed to the back-end subnet and joined to the Active Directory domain.</span></span>
* <span data-ttu-id="c1881-119">Tři uzly clusteru převzetí služeb při selhání systému Windows s modelem kvora Většina uzlů.</span><span class="sxs-lookup"><span data-stu-id="c1881-119">A three-node Windows failover cluster with the Node Majority quorum model.</span></span>
* <span data-ttu-id="c1881-120">Skupina dostupnosti databáze dostupnosti s dvěma replik se synchronním potvrzováním.</span><span class="sxs-lookup"><span data-stu-id="c1881-120">An availability group with two synchronous-commit replicas of an availability database.</span></span>

<span data-ttu-id="c1881-121">Tento scénář je vhodný pro jeho jednoduchost v Azure, ne pro jeho nákladová efektivnost nebo dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="c1881-121">This scenario is a good choice for its simplicity on Azure, not for its cost-effectiveness or other factors.</span></span> <span data-ttu-id="c1881-122">Například můžete minimalizovat počet virtuálních počítačů pro skupinu dostupnosti dvě repliky uložit na výpočetní hodiny v Azure pomocí řadiče domény jako sdílenou složku kvora v clusteru s podporou převzetí služeb při selhání dvěma uzly.</span><span class="sxs-lookup"><span data-stu-id="c1881-122">For example, you can minimize the number of VMs for a two-replica availability group to save on compute hours in Azure by using the domain controller as the quorum file share witness in a two-node failover cluster.</span></span> <span data-ttu-id="c1881-123">Tato metoda snižuje počet virtuálních počítačů pomocí jedné z výše uvedených konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c1881-123">This method reduces the VM count by one from the above configuration.</span></span>

<span data-ttu-id="c1881-124">Tento kurz je určený tak, aby zobrazovalo kroky, které je potřeba nastavit řešení popsané výše, bez vypracování na podrobnosti o jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="c1881-124">This tutorial is intended to show you the steps that are required to set up the described solution above, without elaborating on the details of each step.</span></span> <span data-ttu-id="c1881-125">Proto místo kroky konfigurace grafickým uživatelským rozhraním, používá prostředí PowerShell na vás provede rychle každý krok.</span><span class="sxs-lookup"><span data-stu-id="c1881-125">Therefore, instead of providing the GUI configuration steps, it uses PowerShell scripting to take you quickly through each step.</span></span> <span data-ttu-id="c1881-126">Tento kurz předpokládá následující:</span><span class="sxs-lookup"><span data-stu-id="c1881-126">This tutorial assumes the following:</span></span>

* <span data-ttu-id="c1881-127">Už máte účet Azure s předplatným virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c1881-127">You already have an Azure account with the virtual machine subscription.</span></span>
* <span data-ttu-id="c1881-128">Jste nainstalovali [rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c1881-128">You've installed the [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>
* <span data-ttu-id="c1881-129">Už máte plnou Principy Always On skupin dostupnosti pro místní řešení.</span><span class="sxs-lookup"><span data-stu-id="c1881-129">You already have a solid understanding of Always On availability groups for on-premises solutions.</span></span> <span data-ttu-id="c1881-130">Další informace najdete v tématu [Always On skupiny dostupnosti (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1881-130">For more information, see [Always On availability groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span></span>

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a><span data-ttu-id="c1881-131">Připojení k předplatnému Azure a vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c1881-131">Connect to your Azure subscription and create the virtual network</span></span>
1. <span data-ttu-id="c1881-132">V okně prostředí PowerShell v místním počítači naimportujte modul Azure, stáhnout nastavení publikování do vašeho počítače a připojení relace prostředí PowerShell k předplatnému Azure importováním stažené nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="c1881-132">In a PowerShell window on your local computer, import the Azure module, download the publishing settings file to your machine, and connect your PowerShell session to your Azure subscription by importing the downloaded publishing settings.</span></span>

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    <span data-ttu-id="c1881-133">**Get-AzurePublishSettingsFile** příkaz automaticky vygeneruje certifikát pro správu s Azure a stáhne do počítače.</span><span class="sxs-lookup"><span data-stu-id="c1881-133">The **Get-AzurePublishSettingsFile** command automatically generates a management certificate with Azure and downloads it to your machine.</span></span> <span data-ttu-id="c1881-134">V prohlížeči se otevře automaticky a se zobrazí výzva k zadání přihlašovacích údajů účtu Microsoft pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c1881-134">A browser is automatically opened, and you're prompted to enter the Microsoft account credentials for your Azure subscription.</span></span> <span data-ttu-id="c1881-135">Stažené **.publishsettings** soubor obsahuje všechny informace, které potřebujete ke správě vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c1881-135">The downloaded **.publishsettings** file contains all the information that you need to manage your Azure subscription.</span></span> <span data-ttu-id="c1881-136">Po uložení tohoto souboru do místního adresáře, ho importovat pomocí **Import AzurePublishSettingsFile** příkaz.</span><span class="sxs-lookup"><span data-stu-id="c1881-136">After saving this file to a local directory, import it by using the **Import-AzurePublishSettingsFile** command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c1881-137">Soubor .publishsettings obsahuje vaše pověření (nekódovaných), které se používají ke správě vašich předplatných Azure a služby.</span><span class="sxs-lookup"><span data-stu-id="c1881-137">The .publishsettings file contains your credentials (unencoded) that are used to administer your Azure subscriptions and services.</span></span> <span data-ttu-id="c1881-138">Nejlepším způsobem zabezpečení pro tento soubor je dočasně uložit mimo adresáře zdroje (například ve složce Libraries\Documents) a poté jej odstraňte po dokončení importu.</span><span class="sxs-lookup"><span data-stu-id="c1881-138">The security best practice for this file is to store it temporarily outside your source directories (for example, in the Libraries\Documents folder), and then delete it after the import has finished.</span></span> <span data-ttu-id="c1881-139">Uživatel se zlými úmysly, který získá přístup k souboru .publishsettings můžete upravit, vytvoření a odstranění služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="c1881-139">A malicious user who gains access to the .publishsettings file can edit, create, and delete your Azure services.</span></span>

2. <span data-ttu-id="c1881-140">Definujte řady proměnných, které budete používat k vytvoření vaší cloudové infrastruktury IT.</span><span class="sxs-lookup"><span data-stu-id="c1881-140">Define a series of variables that you'll use to create your cloud IT infrastructure.</span></span>

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

    <span data-ttu-id="c1881-141">Věnujte pozornost následujícím zajistit, že vaše příkazy bude úspěšné později:</span><span class="sxs-lookup"><span data-stu-id="c1881-141">Pay attention to the following to ensure that your commands will succeed later:</span></span>

   * <span data-ttu-id="c1881-142">Proměnné **$storageAccountName** a **$dcServiceName** musí být jedinečný, protože se používá k identifikaci cloudu účet a cloudu server úložiště, v uvedeném pořadí, v síti Internet.</span><span class="sxs-lookup"><span data-stu-id="c1881-142">Variables **$storageAccountName** and **$dcServiceName** must be unique because they're used to identify your cloud storage account and cloud server, respectively, on the Internet.</span></span>
   * <span data-ttu-id="c1881-143">Názvy, které zadáte pro proměnné **$affinityGroupName** a **$virtualNetworkName** jsou nakonfigurované v dokumentu konfigurace virtuální sítě, které budete používat později.</span><span class="sxs-lookup"><span data-stu-id="c1881-143">The names that you specify for variables **$affinityGroupName** and **$virtualNetworkName** are configured in the virtual network configuration document that you'll use later.</span></span>
   * <span data-ttu-id="c1881-144">**$sqlImageName** určuje aktualizované název image virtuálního počítače, který obsahuje SQL Server 2012 Service Pack 1 Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="c1881-144">**$sqlImageName** specifies the updated name of the VM image that contains SQL Server 2012 Service Pack 1 Enterprise Edition.</span></span>
   * <span data-ttu-id="c1881-145">Pro jednoduchost **Contoso! 000** je stejné heslo, které se používá napříč celou kurzu.</span><span class="sxs-lookup"><span data-stu-id="c1881-145">For simplicity, **Contoso!000** is the same password that's used throughout the entire tutorial.</span></span>

3. <span data-ttu-id="c1881-146">Vytvořte skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="c1881-146">Create an affinity group.</span></span>

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. <span data-ttu-id="c1881-147">Vytvoření virtuální sítě pomocí importu konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="c1881-147">Create a virtual network by importing a configuration file.</span></span>

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    <span data-ttu-id="c1881-148">Konfigurační soubor obsahuje následující dokument XML.</span><span class="sxs-lookup"><span data-stu-id="c1881-148">The configuration file contains the following XML document.</span></span> <span data-ttu-id="c1881-149">Stručně řečeno, určuje virtuální sítě s názvem **ContosoNET** ve skupině vztahů názvem **ContosoAG**.</span><span class="sxs-lookup"><span data-stu-id="c1881-149">In brief, it specifies a virtual network called **ContosoNET** in the affinity group called **ContosoAG**.</span></span> <span data-ttu-id="c1881-150">Má adresní prostor **10.10.0.0/16** a má dvě podsítě, **10.10.1.0/24** a **10.10.2.0/24**, které jsou v uvedeném pořadí front podsíť a zpět podsítě.</span><span class="sxs-lookup"><span data-stu-id="c1881-150">It has the address space **10.10.0.0/16** and has two subnets, **10.10.1.0/24** and **10.10.2.0/24**, which are the front subnet and back subnet, respectively.</span></span> <span data-ttu-id="c1881-151">Front podsíť je, kde můžete umístit klientské aplikace, třeba Microsoft SharePoint.</span><span class="sxs-lookup"><span data-stu-id="c1881-151">The front subnet is where you can place client applications such as Microsoft SharePoint.</span></span> <span data-ttu-id="c1881-152">Back podsíť je, kde budete umístit virtuální počítače serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="c1881-152">The back subnet is where you'll place the SQL Server VMs.</span></span> <span data-ttu-id="c1881-153">Pokud změníte **$affinityGroupName** a **$virtualNetworkName** proměnné starší, musíte změnit taky odpovídající názvy níže.</span><span class="sxs-lookup"><span data-stu-id="c1881-153">If you change the **$affinityGroupName** and **$virtualNetworkName** variables earlier, you must also change the corresponding names below.</span></span>

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

5. <span data-ttu-id="c1881-154">Vytvořte účet úložiště, který je spojen s skupinu vztahů, že jste vytvořili a nastavte ji jako aktuální účet úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="c1881-154">Create a storage account that's associated with the affinity group that you created, and set it as the current storage account in your subscription.</span></span>

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. <span data-ttu-id="c1881-155">Vytvořte server řadiče domény v nové cloudové služby a dostupnost sady.</span><span class="sxs-lookup"><span data-stu-id="c1881-155">Create the domain controller server in the new cloud service and availability set.</span></span>

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

    <span data-ttu-id="c1881-156">Tyto příkazy vytvoření kanálu provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="c1881-156">These piped commands do the following things:</span></span>

   * <span data-ttu-id="c1881-157">**Nové AzureVMConfig** vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c1881-157">**New-AzureVMConfig** creates a VM configuration.</span></span>
   * <span data-ttu-id="c1881-158">**Přidat AzureProvisioningConfig** obsahuje parametry konfigurace samostatný server systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c1881-158">**Add-AzureProvisioningConfig** gives the configuration parameters of a standalone Windows server.</span></span>
   * <span data-ttu-id="c1881-159">**Přidat AzureDataDisk** přidá datový disk, který použijete pro ukládání dat služby Active Directory, s možností ukládání do mezipaměti, nastavena na hodnotu None.</span><span class="sxs-lookup"><span data-stu-id="c1881-159">**Add-AzureDataDisk** adds the data disk that you'll use for storing Active Directory data, with the caching option set to None.</span></span>
   * <span data-ttu-id="c1881-160">**Nový-AzureVM** vytvoří novou cloudovou službu a vytvoří nový virtuální počítač Azure v rámci nové cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c1881-160">**New-AzureVM** creates a new cloud service and creates the new Azure VM in the new cloud service.</span></span>

7. <span data-ttu-id="c1881-161">Počkejte kompletní zřízení nového virtuálního počítače a stáhněte si soubor vzdálené plochy do pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="c1881-161">Wait for the new VM to be fully provisioned, and download the remote desktop file to your working directory.</span></span> <span data-ttu-id="c1881-162">Vzhledem k tomu, že nový virtuální počítač Azure trvá příliš dlouho a zajišťují, `while` smyčky nadále dotazování nový virtuální počítač, dokud je připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="c1881-162">Because the new Azure VM takes a long time to provision, the `while` loop continues to poll the new VM until it's ready for use.</span></span>

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

<span data-ttu-id="c1881-163">Nyní je úspěšně zřízený server řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="c1881-163">The domain controller server is now successfully provisioned.</span></span> <span data-ttu-id="c1881-164">Dále nakonfigurujete domény služby Active Directory na tomto serveru řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="c1881-164">Next, you'll configure the Active Directory domain on this domain controller server.</span></span> <span data-ttu-id="c1881-165">Ponechte okno prostředí PowerShell otevřené v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="c1881-165">Leave the PowerShell window open on your local computer.</span></span> <span data-ttu-id="c1881-166">Budete ho později používali k vytváření dva virtuální počítače SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="c1881-166">You'll use it again later to create the two SQL Server VMs.</span></span>

## <a name="configure-the-domain-controller"></a><span data-ttu-id="c1881-167">Konfigurace řadiče domény</span><span class="sxs-lookup"><span data-stu-id="c1881-167">Configure the domain controller</span></span>
1. <span data-ttu-id="c1881-168">Připojení k serveru řadiče domény spuštěním souboru vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c1881-168">Connect to the domain controller server by launching the remote desktop file.</span></span> <span data-ttu-id="c1881-169">Uživatelské jméno AzureAdmin a heslo správce počítače **Contoso! 000**, který jste zadali při vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c1881-169">Use the machine administrator’s username AzureAdmin and password **Contoso!000**, which you specified when you created the new VM.</span></span>
2. <span data-ttu-id="c1881-170">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="c1881-170">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="c1881-171">Spusťte následující **DCPROMO. EXE** příkaz nastavit **corp.contoso.com** domény s datové adresáře na disku M.</span><span class="sxs-lookup"><span data-stu-id="c1881-171">Run the following **DCPROMO.EXE** command to set up the **corp.contoso.com** domain, with the data directories on drive M.</span></span>

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

    <span data-ttu-id="c1881-172">Po dokončení příkazu virtuální počítač se automaticky restartuje.</span><span class="sxs-lookup"><span data-stu-id="c1881-172">After the command finishes, the VM restarts automatically.</span></span>

4. <span data-ttu-id="c1881-173">Znova se připojte k serveru řadiče domény spuštěním souboru vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c1881-173">Connect to the domain controller server again by launching the remote desktop file.</span></span> <span data-ttu-id="c1881-174">Tentokrát, přihlaste se jako **CORP\Administrator**.</span><span class="sxs-lookup"><span data-stu-id="c1881-174">This time, sign in as **CORP\Administrator**.</span></span>
5. <span data-ttu-id="c1881-175">Otevřete okno prostředí PowerShell v režimu správce a importujte modul Active Directory PowerShell pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c1881-175">Open a PowerShell window in administrator mode, and import the Active Directory PowerShell module by using the following command:</span></span>

        Import-Module ActiveDirectory

6. <span data-ttu-id="c1881-176">Spusťte následující příkazy pro přidání tři uživatelů k doméně.</span><span class="sxs-lookup"><span data-stu-id="c1881-176">Run the following commands to add three users to the domain.</span></span>

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

    <span data-ttu-id="c1881-177">**CORP\Install** slouží ke konfiguraci všechno související s instancí služby SQL Server, převzetí služeb při selhání clusteru a skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-177">**CORP\Install** is used to configure everything related to the SQL Server service instances, the failover cluster, and the availability group.</span></span> <span data-ttu-id="c1881-178">**CORP\SQLSvc1** a **CORP\SQLSvc2** jsou použity jako účty služby SQL Server pro dva virtuální počítače SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="c1881-178">**CORP\SQLSvc1** and **CORP\SQLSvc2** are used as the SQL Server service accounts for the two SQL Server VMs.</span></span>
7. <span data-ttu-id="c1881-179">Potom spusťte následující příkazy umožnit **CORP\Install** oprávnění k vytváření počítačových objektů v doméně.</span><span class="sxs-lookup"><span data-stu-id="c1881-179">Next, run the following commands to give **CORP\Install** the permissions to create computer objects in the domain.</span></span>

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    <span data-ttu-id="c1881-180">Identifikátor GUID uveden výše je identifikátor GUID pro objekt typu počítače.</span><span class="sxs-lookup"><span data-stu-id="c1881-180">The GUID specified above is the GUID for the computer object type.</span></span> <span data-ttu-id="c1881-181">**CORP\Install** musí účet **čtení všech vlastností** a **vytvářet objekty počítačů** oprávnění k vytvoření Active přímé objekty pro převzetí služeb při selhání clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1881-181">The **CORP\Install** account needs the **Read All Properties** and **Create Computer Objects** permission to create the Active Direct objects for the failover cluster.</span></span> <span data-ttu-id="c1881-182">**Čtení všech vlastností** oprávnění je již CORP\Install ve výchozím nastavení přiděleno, takže není nutné ji explicitně udělit.</span><span class="sxs-lookup"><span data-stu-id="c1881-182">The **Read All Properties** permission is already given to CORP\Install by default, so you don't need to grant it explicitly.</span></span> <span data-ttu-id="c1881-183">Další informace o oprávnění, které jsou potřebné k vytvoření clusteru převzetí služeb při selhání najdete v tématu [převzetí služeb při selhání clusteru podrobný průvodce: Konfigurace účty ve službě Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1881-183">For more information on permissions that are needed to create the failover cluster, see [Failover Cluster Step-by-Step Guide: Configuring Accounts in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span></span>

    <span data-ttu-id="c1881-184">Teď, když jste dokončili konfigurace služby Active Directory a uživatelských objektů, vytvoříte dva virtuální počítače serveru SQL a připojovat je k této doméně.</span><span class="sxs-lookup"><span data-stu-id="c1881-184">Now that you've finished configuring Active Directory and the user objects, you'll create two SQL Server VMs and join them to this domain.</span></span>

## <a name="create-the-sql-server-vms"></a><span data-ttu-id="c1881-185">Vytvoření virtuálních počítačů serveru SQL</span><span class="sxs-lookup"><span data-stu-id="c1881-185">Create the SQL Server VMs</span></span>
1. <span data-ttu-id="c1881-186">Nadále používat okno prostředí PowerShell, který je otevřený v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="c1881-186">Continue to use the PowerShell window that's open on your local computer.</span></span> <span data-ttu-id="c1881-187">Definujte následující další proměnné:</span><span class="sxs-lookup"><span data-stu-id="c1881-187">Define the following additional variables:</span></span>

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

    <span data-ttu-id="c1881-188">IP adresa **10.10.0.4** je obvykle přiřazena první virtuální počítač, který vytvoříte v **10.10.0.0/16** podsíť virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="c1881-188">The IP address **10.10.0.4** is typically assigned to the first VM that you create in the **10.10.0.0/16** subnet of your Azure virtual network.</span></span> <span data-ttu-id="c1881-189">Ověřte, že jde o adresu serveru řadiče domény tak, že spustíte **IPCONFIG**.</span><span class="sxs-lookup"><span data-stu-id="c1881-189">You should verify that this is the address of your domain controller server by running **IPCONFIG**.</span></span>
2. <span data-ttu-id="c1881-190">Spusťte následující přesměruje příkazů pro vytvoření první virtuální počítač v clusteru převzetí služeb při selhání, s názvem **ContosoQuorum**:</span><span class="sxs-lookup"><span data-stu-id="c1881-190">Run the following piped commands to create the first VM in the failover cluster, named **ContosoQuorum**:</span></span>

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

    <span data-ttu-id="c1881-191">Vezměte na vědomí následující týkající se výše uvedeného příkazu:</span><span class="sxs-lookup"><span data-stu-id="c1881-191">Note the following regarding the command above:</span></span>

   * <span data-ttu-id="c1881-192">**Nové AzureVMConfig** vytvoří konfigurace virtuálního počítače s názvem sadu požadovanou dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-192">**New-AzureVMConfig** creates a VM configuration with the desired availability set name.</span></span> <span data-ttu-id="c1881-193">Se stejným názvem sady dostupnosti bude vytvořen dalších virtuálních počítačů, tak, aby se připojit do stejné skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-193">The subsequent VMs will be created with the same availability set name so that they're joined to the same availability set.</span></span>
   * <span data-ttu-id="c1881-194">**Přidat AzureProvisioningConfig** připojí virtuální počítač k doméně služby Active Directory, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c1881-194">**Add-AzureProvisioningConfig** joins the VM to the Active Directory domain that you created.</span></span>
   * <span data-ttu-id="c1881-195">**Set-AzureSubnet** umístí virtuální počítač v back podsíti.</span><span class="sxs-lookup"><span data-stu-id="c1881-195">**Set-AzureSubnet** places the VM in the back subnet.</span></span>
   * <span data-ttu-id="c1881-196">**Nový-AzureVM** vytvoří novou cloudovou službu a vytvoří nový virtuální počítač Azure v rámci nové cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c1881-196">**New-AzureVM** creates a new cloud service and creates the new Azure VM in the new cloud service.</span></span> <span data-ttu-id="c1881-197">**DnsSettings** parametr určuje, zda má server DNS pro servery v cloudové službě novou IP adresu **10.10.0.4**.</span><span class="sxs-lookup"><span data-stu-id="c1881-197">The **DnsSettings** parameter specifies that the DNS server for the servers in the new cloud service has the IP address **10.10.0.4**.</span></span> <span data-ttu-id="c1881-198">Toto je IP adresa serveru řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="c1881-198">This is the IP address of the domain controller server.</span></span> <span data-ttu-id="c1881-199">Tento parametr je potřebná k povolení nové virtuální počítače v rámci cloudové služby pro připojení k doméně služby Active Directory úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c1881-199">This parameter is needed to enable the new VMs in the cloud service to join to the Active Directory domain successfully.</span></span> <span data-ttu-id="c1881-200">Tento parametr musíte ručně nastavit nastavením IPv4 v virtuálního počítače používat server řadiče domény jako primární server DNS po zřízení virtuálního počítače a potom připojí virtuální počítač k doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1881-200">Without this parameter, you must manually set the IPv4 settings in your VM to use the domain controller server as the primary DNS server after the VM is provisioned, and then join the VM to the Active Directory domain.</span></span>
3. <span data-ttu-id="c1881-201">Spusťte následující přesměruje příkazů pro vytvoření virtuálních počítačů serveru SQL s názvem **ContosoSQL1** a **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="c1881-201">Run the following piped commands to create the SQL Server VMs, named **ContosoSQL1** and **ContosoSQL2**.</span></span>

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

    <span data-ttu-id="c1881-202">Vezměte na vědomí následující týkající se výše uvedené příkazy:</span><span class="sxs-lookup"><span data-stu-id="c1881-202">Note the following regarding the commands above:</span></span>

   * <span data-ttu-id="c1881-203">**Nové AzureVMConfig** používá stejný název sady dostupnosti jako server řadiče domény a bitovou kopii systému SQL Server 2012 Service Pack 1 Enterprise Edition v galerii virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1881-203">**New-AzureVMConfig** uses the same availability set name as the domain controller server, and uses the SQL Server 2012 Service Pack 1 Enterprise Edition image in the virtual machine gallery.</span></span> <span data-ttu-id="c1881-204">Také nastaví disku operačního systému na čtení ukládání do mezipaměti pouze (bez ukládání do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="c1881-204">It also sets the operating system disk to read-caching only (no write caching).</span></span> <span data-ttu-id="c1881-205">Doporučujeme migraci soubory databáze do samostatné datový disk, který připojíte k virtuálnímu počítači a konfigurace bez pro čtení a ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c1881-205">We recommend that you migrate the database files to a separate data disk that you attach to the VM, and configure it with no read or write caching.</span></span> <span data-ttu-id="c1881-206">Skoro je však k odebrání ukládání do mezipaměti na disku operačního systému, protože nelze odebrat, přečtěte si ukládání do mezipaměti na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c1881-206">However, the next best thing is to remove write caching on the operating system disk because you can't remove read caching on the operating system disk.</span></span>
   * <span data-ttu-id="c1881-207">**Přidat AzureProvisioningConfig** připojí virtuální počítač k doméně služby Active Directory, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c1881-207">**Add-AzureProvisioningConfig** joins the VM to the Active Directory domain that you created.</span></span>
   * <span data-ttu-id="c1881-208">**Set-AzureSubnet** umístí virtuální počítač v back podsíti.</span><span class="sxs-lookup"><span data-stu-id="c1881-208">**Set-AzureSubnet** places the VM in the back subnet.</span></span>
   * <span data-ttu-id="c1881-209">**Přidat AzureEndpoint** přidá přístup koncových bodů, aby klientské aplikace přístup tyto instance služby SQL Server na Internetu.</span><span class="sxs-lookup"><span data-stu-id="c1881-209">**Add-AzureEndpoint** adds access endpoints so that client applications can access these SQL Server services instances on the Internet.</span></span> <span data-ttu-id="c1881-210">K ContosoSQL1 a ContosoSQL2 mají jiné porty.</span><span class="sxs-lookup"><span data-stu-id="c1881-210">Different ports are given to ContosoSQL1 and ContosoSQL2.</span></span>
   * <span data-ttu-id="c1881-211">**Nový-AzureVM** vytvoří nový virtuální počítač SQL Server v rámci stejné cloudové služby jako ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="c1881-211">**New-AzureVM** creates the new SQL Server VM in the same cloud service as ContosoQuorum.</span></span> <span data-ttu-id="c1881-212">Virtuální počítače musí být ve stejné cloudové služby, pokud chcete, aby se ve stejné sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-212">You must place the VMs in the same cloud service if you want them to be in the same availability set.</span></span>
4. <span data-ttu-id="c1881-213">Počkejte, než pro každý virtuální počítač plně zřídit a pro každý virtuální počítač ke stažení souboru jeho vzdálené plochy do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="c1881-213">Wait for each VM to be fully provisioned and for each VM to download its remote desktop file to your working directory.</span></span> <span data-ttu-id="c1881-214">`for` Smyčky procházení tři nové virtuální počítače a spouští příkazy uvnitř nejvyšší úrovně složené závorky pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="c1881-214">The `for` loop cycles through the three new VMs and executes the commands inside the top-level curly brackets for each of them.</span></span>

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
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

    <span data-ttu-id="c1881-215">Virtuální počítače serveru SQL je nyní opatřen a spuštěná, ale instalované se systémem SQL Server s výchozími možnostmi.</span><span class="sxs-lookup"><span data-stu-id="c1881-215">The SQL Server VMs are now provisioned and running, but they're installed with SQL Server with default options.</span></span>

## <a name="initialize-the-failover-cluster-vms"></a><span data-ttu-id="c1881-216">Inicializovat převzetí služeb při selhání clusteru virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c1881-216">Initialize the failover cluster VMs</span></span>
<span data-ttu-id="c1881-217">V této části musíte upravit tři servery, které budete používat v clusteru převzetí služeb při selhání a instalace systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1881-217">In this section, you need to modify the three servers that you'll use in the failover cluster and the SQL Server installation.</span></span> <span data-ttu-id="c1881-218">Zejména:</span><span class="sxs-lookup"><span data-stu-id="c1881-218">Specifically:</span></span>

* <span data-ttu-id="c1881-219">Všechny servery: je potřeba nainstalovat **Clustering převzetí služeb při selhání** funkce.</span><span class="sxs-lookup"><span data-stu-id="c1881-219">All servers: You need to install the **Failover Clustering** feature.</span></span>
* <span data-ttu-id="c1881-220">Všechny servery: je nutné přidat **CORP\Install** jako počítač **správce**.</span><span class="sxs-lookup"><span data-stu-id="c1881-220">All servers: You need to add **CORP\Install** as the machine **administrator**.</span></span>
* <span data-ttu-id="c1881-221">ContosoSQL1 a ContosoSQL2 pouze: je nutné přidat **CORP\Install** jako **sysadmin** role v výchozí databáze.</span><span class="sxs-lookup"><span data-stu-id="c1881-221">ContosoSQL1 and ContosoSQL2 only: You need to add **CORP\Install** as a **sysadmin** role in the default database.</span></span>
* <span data-ttu-id="c1881-222">ContosoSQL1 a ContosoSQL2 pouze: je nutné přidat **NT AUTHORITY\System** jako u přihlášení s následujícími oprávněními:</span><span class="sxs-lookup"><span data-stu-id="c1881-222">ContosoSQL1 and ContosoSQL2 only: You need to add **NT AUTHORITY\System** as a sign-in with the following permissions:</span></span>

  * <span data-ttu-id="c1881-223">Příkaz ALTER žádnou skupinu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c1881-223">Alter any availability group</span></span>
  * <span data-ttu-id="c1881-224">Připojení SQL</span><span class="sxs-lookup"><span data-stu-id="c1881-224">Connect SQL</span></span>
  * <span data-ttu-id="c1881-225">Zobrazení stavu serveru</span><span class="sxs-lookup"><span data-stu-id="c1881-225">View server state</span></span>
* <span data-ttu-id="c1881-226">ContosoSQL1 a ContosoSQL2 pouze: **TCP** na virtuální počítač SQL Server je již povolen protokol.</span><span class="sxs-lookup"><span data-stu-id="c1881-226">ContosoSQL1 and ContosoSQL2 only: The **TCP** protocol is already enabled on the SQL Server VM.</span></span> <span data-ttu-id="c1881-227">Však stále musíte otevřít bránu firewall pro vzdálený přístup systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1881-227">However, you still need to open the firewall for remote access of SQL Server.</span></span>

<span data-ttu-id="c1881-228">Teď jste připravení začít.</span><span class="sxs-lookup"><span data-stu-id="c1881-228">Now, you're ready to start.</span></span> <span data-ttu-id="c1881-229">Počínaje **ContosoQuorum**, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c1881-229">Beginning with **ContosoQuorum**, follow the steps below:</span></span>

1. <span data-ttu-id="c1881-230">Připojení k **ContosoQuorum** spuštěním soubory vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c1881-230">Connect to **ContosoQuorum** by launching the remote desktop files.</span></span> <span data-ttu-id="c1881-231">Použijte uživatelské jméno správce počítače **AzureAdmin** a heslo **Contoso! 000**, který jste zadali při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1881-231">Use the machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created the VMs.</span></span>
2. <span data-ttu-id="c1881-232">Ověřte, že počítače byl úspěšně připojen k **corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="c1881-232">Verify that the computers have been successfully joined to **corp.contoso.com**.</span></span>
3. <span data-ttu-id="c1881-233">Počkejte na dokončení spuštěných úloh automatizované inicializace před pokračováním instalace systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1881-233">Wait for the SQL Server installation to finish running the automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="c1881-234">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="c1881-234">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="c1881-235">Instalace funkce Clustering převzetí služeb při selhání systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c1881-235">Install the Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="c1881-236">Přidat **CORP\Install** jako místní správce.</span><span class="sxs-lookup"><span data-stu-id="c1881-236">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="c1881-237">Odhlásit z ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="c1881-237">Sign out of ContosoQuorum.</span></span> <span data-ttu-id="c1881-238">Jste hotovi s tímto serverem teď.</span><span class="sxs-lookup"><span data-stu-id="c1881-238">You're done with this server now.</span></span>

        logoff.exe

<span data-ttu-id="c1881-239">V dalším kroku inicializovat **ContosoSQL1** a **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="c1881-239">Next, initialize **ContosoSQL1** and **ContosoSQL2**.</span></span> <span data-ttu-id="c1881-240">Postupujte podle následujících kroků, které jsou stejné pro oba virtuální počítače serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="c1881-240">Follow the steps below, which are identical for both SQL Server VMs.</span></span>

1. <span data-ttu-id="c1881-241">Připojte k dva virtuální počítače SQL serveru spuštěním soubory vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c1881-241">Connect to the two SQL Server VMs by launching the remote desktop files.</span></span> <span data-ttu-id="c1881-242">Použijte uživatelské jméno správce počítače **AzureAdmin** a heslo **Contoso! 000**, který jste zadali při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1881-242">Use the machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created the VMs.</span></span>
2. <span data-ttu-id="c1881-243">Ověřte, že počítače byl úspěšně připojen k **corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="c1881-243">Verify that the computers have been successfully joined to **corp.contoso.com**.</span></span>
3. <span data-ttu-id="c1881-244">Počkejte na dokončení spuštěných úloh automatizované inicializace před pokračováním instalace systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1881-244">Wait for the SQL Server installation to finish running the automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="c1881-245">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="c1881-245">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="c1881-246">Instalace funkce Clustering převzetí služeb při selhání systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c1881-246">Install the Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="c1881-247">Přidat **CORP\Install** jako místní správce.</span><span class="sxs-lookup"><span data-stu-id="c1881-247">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="c1881-248">Importujte poskytovatele prostředí PowerShell serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="c1881-248">Import the SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. <span data-ttu-id="c1881-249">Přidat **CORP\Install** jako roli správce systému pro výchozí instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="c1881-249">Add **CORP\Install** as the sysadmin role for the default SQL Server instance.</span></span>

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. <span data-ttu-id="c1881-250">Přidat **NT AUTHORITY\System** jako Přihlaste se pomocí tří oprávnění popsané výše.</span><span class="sxs-lookup"><span data-stu-id="c1881-250">Add **NT AUTHORITY\System** as a sign-in with the three permissions described above.</span></span>

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. <span data-ttu-id="c1881-251">Otevření brány firewall pro vzdálený přístup systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1881-251">Open the firewall for remote access of SQL Server.</span></span>

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. <span data-ttu-id="c1881-252">Odhlásit z oba virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c1881-252">Sign out of both VMs.</span></span>

         logoff.exe

<span data-ttu-id="c1881-253">Nakonec budete připraveni ke konfiguraci skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-253">Finally, you're ready to configure the availability group.</span></span> <span data-ttu-id="c1881-254">Zprostředkovatel SQL Server prostředí PowerShell budete používat k provádění všech činností, které na **ContosoSQL1**.</span><span class="sxs-lookup"><span data-stu-id="c1881-254">You'll use the SQL Server PowerShell Provider to perform all of the work on **ContosoSQL1**.</span></span>

## <a name="configure-the-availability-group"></a><span data-ttu-id="c1881-255">Nakonfigurujte skupinu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c1881-255">Configure the availability group</span></span>
1. <span data-ttu-id="c1881-256">Připojení k **ContosoSQL1** znovu spuštěním soubory vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c1881-256">Connect to **ContosoSQL1** again by launching the remote desktop files.</span></span> <span data-ttu-id="c1881-257">Místo přihlášení pomocí účtu počítače, přihlaste se pomocí **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="c1881-257">Instead of signing in by using the machine account, sign in by using **CORP\Install**.</span></span>
2. <span data-ttu-id="c1881-258">Otevřete okno prostředí PowerShell v režimu správce.</span><span class="sxs-lookup"><span data-stu-id="c1881-258">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="c1881-259">Definujte následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="c1881-259">Define the following variables:</span></span>

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
4. <span data-ttu-id="c1881-260">Importujte poskytovatele prostředí PowerShell serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="c1881-260">Import the SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. <span data-ttu-id="c1881-261">Změňte účet služby SQL Server pro ContosoSQL1 na CORP\SQLSvc1.</span><span class="sxs-lookup"><span data-stu-id="c1881-261">Change the SQL Server service account for ContosoSQL1 to CORP\SQLSvc1.</span></span>

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. <span data-ttu-id="c1881-262">Změňte účet služby SQL Server pro ContosoSQL2 na CORP\SQLSvc2.</span><span class="sxs-lookup"><span data-stu-id="c1881-262">Change the SQL Server service account for ContosoSQL2 to CORP\SQLSvc2.</span></span>

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. <span data-ttu-id="c1881-263">Stáhněte si **CreateAzureFailoverCluster.ps1** z [vytvořit Cluster převzetí služeb při selhání pro skupiny dostupnosti Always On ve virtuálním počítači Azure](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) do místní pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="c1881-263">Download **CreateAzureFailoverCluster.ps1** from [Create Failover Cluster for Always On Availability Groups in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) to the local working directory.</span></span> <span data-ttu-id="c1881-264">Tento skript budete používat k vytváření clusteru s podporou převzetí služeb při selhání funkční.</span><span class="sxs-lookup"><span data-stu-id="c1881-264">You'll use this script to help you create a functional failover cluster.</span></span> <span data-ttu-id="c1881-265">Důležité informace o clusteringu Windows převzetí služeb při selhání jak komunikuje s síť Azure, najdete v části [vysoké dostupnosti a zotavení po havárii pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1881-265">For important information on how Windows Failover Clustering interacts with the Azure network, see [High availability and disaster recovery for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>
8. <span data-ttu-id="c1881-266">Změňte pracovní adresář a vytvoření clusteru převzetí služeb při selhání pomocí staženého skriptu.</span><span class="sxs-lookup"><span data-stu-id="c1881-266">Change to your working directory and create the failover cluster with the downloaded script.</span></span>

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. <span data-ttu-id="c1881-267">Povolte Always On skupiny dostupnosti pro výchozí instance systému SQL Server na **ContosoSQL1** a **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="c1881-267">Enable Always On availability groups for the default SQL Server instances on **ContosoSQL1** and **ContosoSQL2**.</span></span>

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
10. <span data-ttu-id="c1881-268">Vytvořte adresář zálohy a udělit oprávnění účtům služby SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1881-268">Create a backup directory and grant permissions for the SQL Server service accounts.</span></span> <span data-ttu-id="c1881-269">Příprava databáze dostupnosti na sekundární repliku použijete tento adresář.</span><span class="sxs-lookup"><span data-stu-id="c1881-269">You'll use this directory to prepare the availability database on the secondary replica.</span></span>

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. <span data-ttu-id="c1881-270">Vytvořit databázi na **ContosoSQL1** názvem **MyDB1**, provést úplnou zálohu a zálohu protokolu a obnoví je na **ContosoSQL2** s **WITH NORECOVERY**  možnost.</span><span class="sxs-lookup"><span data-stu-id="c1881-270">Create a database on **ContosoSQL1** called **MyDB1**, take both a full backup and a log backup, and restore them on **ContosoSQL2** with the **WITH NORECOVERY** option.</span></span>

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. <span data-ttu-id="c1881-271">Vytvořte dostupnost skupiny koncových bodů na virtuálních počítačích SQL Server a nastavit oprávnění u koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="c1881-271">Create the availability group endpoints on the SQL Server VMs and set the proper permissions on the endpoints.</span></span>

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
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2
13. <span data-ttu-id="c1881-272">Vytvoření repliky dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-272">Create the availability replicas.</span></span>

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
14. <span data-ttu-id="c1881-273">Nakonec vytvořte skupinu dostupnosti a připojení sekundární repliky ke skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c1881-273">Finally, create the availability group and join the secondary replica to the availability group.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c1881-274">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1881-274">Next steps</span></span>
<span data-ttu-id="c1881-275">Jste teď úspěšně implementovali SQL serveru Always On tak, že vytvoříte skupinu dostupnosti v Azure.</span><span class="sxs-lookup"><span data-stu-id="c1881-275">You've now successfully implemented SQL Server Always On by creating an availability group in Azure.</span></span> <span data-ttu-id="c1881-276">Ke konfiguraci naslouchacího procesu pro tuto skupinu dostupnosti, najdete v části [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="c1881-276">To configure a listener for this availability group, see [Configure an ILB listener for Always On availability groups in Azure](../classic/ps-sql-int-listener.md).</span></span>

<span data-ttu-id="c1881-277">Další informace o používání systému SQL Server v Azure najdete v tématu [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c1881-277">For other information about using SQL Server in Azure, see [SQL Server on Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
