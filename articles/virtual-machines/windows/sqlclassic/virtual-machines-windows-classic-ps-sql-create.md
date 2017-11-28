---
title: "Vytvoření virtuálního počítače s SQL serverem v prostředí Azure PowerShell (klasické) | Microsoft Docs"
description: "Obsahuje kroky a skriptů prostředí PowerShell pro vytvoření virtuálního počítače Azure s obrázky Galerie virtuálního počítače systému SQL Server. Toto téma používá režim nasazení classic."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: c3bd4329e8a22ce8503d6593560d29c2a3135e83
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a><span data-ttu-id="b01d5-104">Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="b01d5-104">Provision a SQL Server virtual machine using Azure PowerShell (Classic)</span></span>

<span data-ttu-id="b01d5-105">Tento článek obsahuje kroky pro vytvoření virtuálního počítače s SQL serverem v Azure pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b01d5-105">This article provides steps for how to create a SQL Server virtual machine in Azure by using the PowerShell cmdlets.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="b01d5-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b01d5-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b01d5-107">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="b01d5-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="b01d5-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b01d5-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="b01d5-109">Resource Manager verzi tohoto tématu naleznete v části [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="b01d5-109">For the Resource Manager version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span></span>

### <a name="install-and-configure-powershell"></a><span data-ttu-id="b01d5-110">Instalace a konfigurace prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b01d5-110">Install and configure PowerShell:</span></span>
1. <span data-ttu-id="b01d5-111">Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b01d5-111">If you do not have an Azure account, visit [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
2. <span data-ttu-id="b01d5-112">[Stáhněte a nainstalujte nejnovější příkazy prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b01d5-112">[Download and install the latest Azure PowerShell commands](/powershell/azure/overview).</span></span>
3. <span data-ttu-id="b01d5-113">Spusťte prostředí Windows PowerShell a připojte ho k předplatnému Azure s **Add-AzureAccount** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b01d5-113">Start Windows PowerShell, and connect it to your Azure subscription with the **Add-AzureAccount** command.</span></span>

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a><span data-ttu-id="b01d5-114">Určení cílových oblast Azure</span><span class="sxs-lookup"><span data-stu-id="b01d5-114">Determine your target Azure region</span></span>

<span data-ttu-id="b01d5-115">Váš virtuální počítač SQL Server bude hostován v rámci cloudové služby, který se nachází v určité oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="b01d5-115">Your SQL Server Virtual Machine will be hosted in a cloud service that resides a specific Azure region.</span></span> <span data-ttu-id="b01d5-116">Následující postup vám pomůže určit oblasti, účet úložiště a cloudové služby, který se použije pro zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b01d5-116">The following steps help you to determine your region, storage account, and cloud service that will be used for the rest of the tutorial.</span></span>

1. <span data-ttu-id="b01d5-117">Určuje datové centrum, které chcete použít k hostování virtuální počítač s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="b01d5-117">Determine the data center that you want to use to host your SQL Server VM.</span></span> <span data-ttu-id="b01d5-118">Následující příkaz prostředí PowerShell zobrazí seznam názvů dostupné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b01d5-118">The following PowerShell command displays a list of available region names.</span></span>

   ```powershell
   (Get-AzureLocation).Name
   ```

2. <span data-ttu-id="b01d5-119">Jakmile jste zformulovali upřednostňované umístění, nastavení proměnné s názvem **$dcLocation** do této oblasti.</span><span class="sxs-lookup"><span data-stu-id="b01d5-119">Once you've identified your preferred location, set a variable named **$dcLocation** to that region.</span></span> <span data-ttu-id="b01d5-120">Následující příkaz třeba nastaví oblasti, kterou chcete "Východ USA":</span><span class="sxs-lookup"><span data-stu-id="b01d5-120">For example, the following command sets the region to "East US":</span></span>

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a><span data-ttu-id="b01d5-121">Nastavit vaše předplatné a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="b01d5-121">Set your subscription and storage account</span></span>

1. <span data-ttu-id="b01d5-122">Určete předplatné, které chcete použít pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b01d5-122">Determine the Azure subscription you will use for the new virtual machine.</span></span>

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. <span data-ttu-id="b01d5-123">Přiřadit cílových předplatného Azure k **$subscr** proměnné.</span><span class="sxs-lookup"><span data-stu-id="b01d5-123">Assign your target Azure subscription to the **$subscr** variable.</span></span> <span data-ttu-id="b01d5-124">Potom nastavením jako vaším aktuálním předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="b01d5-124">Then set this as your current Azure subscription.</span></span>

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. <span data-ttu-id="b01d5-125">Zkontrolujte pro existující účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="b01d5-125">Then check for existing storage accounts.</span></span> <span data-ttu-id="b01d5-126">Následující skript zobrazí všechny účty úložiště, které existují ve vaší vybrané oblasti:</span><span class="sxs-lookup"><span data-stu-id="b01d5-126">The following script displays all storage accounts that exist in your chosen region:</span></span>

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > <span data-ttu-id="b01d5-127">Pokud budete potřebovat nový účet úložiště, název účtu úložiště všechna malá nejprve vytvořte pomocí příkazu New-AzureStorageAccount jako v následujícím příkladu:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span><span class="sxs-lookup"><span data-stu-id="b01d5-127">If you require a new storage account, first create an all-lower-case storage account name with the New-AzureStorageAccount command as in the following example: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span></span>

4. <span data-ttu-id="b01d5-128">Přiřadit název cílového účtu úložiště k **$staccount**.</span><span class="sxs-lookup"><span data-stu-id="b01d5-128">Assign the target storage account name to the **$staccount**.</span></span> <span data-ttu-id="b01d5-129">Potom pomocí **Set-AzureSubscription** nastavte předplatné a aktuální účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b01d5-129">Then use **Set-AzureSubscription** to set the subscription and current storage account.</span></span>

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a><span data-ttu-id="b01d5-130">Vyberte bitovou kopii virtuálního počítače systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="b01d5-130">Select a SQL Server virtual machine image</span></span>

1. <span data-ttu-id="b01d5-131">Získat seznam dostupných imagí SQL serveru virtuálních počítačů z galerie.</span><span class="sxs-lookup"><span data-stu-id="b01d5-131">Find out the list of available SQL Server virtual machines images from the gallery.</span></span> <span data-ttu-id="b01d5-132">Tyto Image jsou vybavené **ImageFamily** vlastnost, která začíná "SQL".</span><span class="sxs-lookup"><span data-stu-id="b01d5-132">These images all have an **ImageFamily** property that starts with "SQL".</span></span> <span data-ttu-id="b01d5-133">Následující dotaz zobrazí pro předinstalována systému SQL Server, které vám k dispozici bitovou kopii rodiny.</span><span class="sxs-lookup"><span data-stu-id="b01d5-133">The following query displays the image family available to you that have SQL Server preinstalled.</span></span>

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. <span data-ttu-id="b01d5-134">Pokud zjistíte, rodina bitové kopie virtuálního počítače, může být více bitových kopií publikované v této rodině.</span><span class="sxs-lookup"><span data-stu-id="b01d5-134">When you find the  virtual machine image family, there could be multiple published images in this family.</span></span> <span data-ttu-id="b01d5-135">Pomocí následujícího skriptu najít nejnovější název bitové kopie publikované virtuálního počítače pro vybranou image rodinu (například **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2**):</span><span class="sxs-lookup"><span data-stu-id="b01d5-135">Use the following script to find the latest published virtual machine image name for your selected image family (such as **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2**):</span></span>

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-the-virtual-machine"></a><span data-ttu-id="b01d5-136">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b01d5-136">Create the virtual machine</span></span>

<span data-ttu-id="b01d5-137">Nakonec vytvořte virtuální počítač pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b01d5-137">Finally, create the virtual machine with PowerShell:</span></span>

1. <span data-ttu-id="b01d5-138">Vytvořte cloudovou službu k hostování nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b01d5-138">Create a cloud service to host the new VM.</span></span> <span data-ttu-id="b01d5-139">Všimněte si, že je také možné místo toho použít stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="b01d5-139">Note that it is also possible to use an existing cloud service instead.</span></span> <span data-ttu-id="b01d5-140">Vytvoření nové proměnné **$svcname** s krátký název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b01d5-140">Create a new variable **$svcname** with the short name of the cloud service.</span></span>

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. <span data-ttu-id="b01d5-141">Zadejte název virtuálního počítače a velikost.</span><span class="sxs-lookup"><span data-stu-id="b01d5-141">Specify the virtual machine name and a size.</span></span> <span data-ttu-id="b01d5-142">Další informace o velikosti virtuálních počítačů najdete v tématu [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b01d5-142">For more information about virtual machine sizes, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. <span data-ttu-id="b01d5-143">Zadejte účet místního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="b01d5-143">Specify the local administrator account and password.</span></span>

   ```powershell
   $cred=Get-Credential -Message "Type the name and password of the local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. <span data-ttu-id="b01d5-144">Spusťte následující skript pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b01d5-144">Run the following script to create the virtual machine.</span></span>

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> <span data-ttu-id="b01d5-145">Další vysvětlení a možnosti konfigurace najdete v tématu **sestavení vaší sadu příkazů** kapitoly [použití Azure PowerShell k vytvoření a nastavení virtuálních počítačích se systémem Windows](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b01d5-145">For additional explanation and configuration options, see the **Build your command set** section in [Use Azure PowerShell to create and preconfigure Windows-based Virtual Machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="example-powershell-script"></a><span data-ttu-id="b01d5-146">Ukázkový skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b01d5-146">Example PowerShell script</span></span>

<span data-ttu-id="b01d5-147">Následující skript představuje příklad dokončení skriptu, který vytvoří **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2** virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b01d5-147">The following script provides an example of a complete script that creates a **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2** virtual machine.</span></span> <span data-ttu-id="b01d5-148">Pokud chcete použít tento skript, je nutné přizpůsobit počáteční proměnné podle předchozích kroků v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b01d5-148">If you use this script, you must customize the initial variables based on the previous steps in this topic.</span></span>

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set the current subscription and storage account
# Comment out the New-AzureStorageAccount line if the account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select the most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create the new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create the VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create the SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a><span data-ttu-id="b01d5-149">Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="b01d5-149">Connect with remote desktop</span></span>

1. <span data-ttu-id="b01d5-150">Soubory protokolu RDP vytvořte ve složce dokumentu stávajícího uživatele ke spuštění těchto virtuálních počítačů k dokončení instalace:</span><span class="sxs-lookup"><span data-stu-id="b01d5-150">Create the RDP files in the current user's document folder to launch these virtual machines to complete setup:</span></span>

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. <span data-ttu-id="b01d5-151">V adresáři dokumenty spusťte soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="b01d5-151">In the documents directory, launch the RDP file.</span></span> <span data-ttu-id="b01d5-152">Spojte se s správce uživatelské jméno a heslo zadané dříve (například pokud vaše uživatelské jméno je VMAdmin, zadejte "\VMAdmin" jako uživatel a zadejte heslo).</span><span class="sxs-lookup"><span data-stu-id="b01d5-152">Connect with the administrator user name and password provided earlier (for example, if your user name was VMAdmin, specify "\VMAdmin" as the user and provide the password).</span></span>

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a><span data-ttu-id="b01d5-153">Dokončení konfigurace počítače systému SQL Server pro vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="b01d5-153">Complete the configuration of the SQL Server Machine for remote access</span></span>

<span data-ttu-id="b01d5-154">Po přihlášení k počítači pomocí vzdálené plochy, nakonfigurujte podle pokynů v systému SQL Server [kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="b01d5-154">After logging on to the machine with remote desktop, configure SQL Server based on the instructions in [Steps for configuring SQL Server connectivity in an Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b01d5-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b01d5-155">Next steps</span></span>

<span data-ttu-id="b01d5-156">Můžete najít další pokyny pro zřizování virtuálních počítačů pomocí prostředí PowerShell ve [virtuální počítače dokumentaci](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b01d5-156">You can find additional instructions for provisioning virtual machines with PowerShell in the [virtual machines documentation](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="b01d5-157">V mnoha případech je dalším krokem je migrace databáze do tohoto nového virtuálního počítače SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="b01d5-157">In many cases, the next step is to migrate your databases to this new SQL Server VM.</span></span> <span data-ttu-id="b01d5-158">Pokyny pro migraci databáze najdete v tématu [migrace databáze do systému SQL Server na virtuálním počítači Azure](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b01d5-158">For database migration guidance, see [Migrating a Database to SQL Server on an Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="b01d5-159">Pokud vás zajímá také pomocí portálu Azure vytvořit virtuální počítače se systémem SQL, najdete v článku [zřizování virtuálního počítače systému SQL Server na platformě Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="b01d5-159">If you're also interested in using the Azure portal to create SQL Virtual Machines, see [Provisioning a SQL Server Virtual Machine on Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="b01d5-160">Všimněte si, že kurz, který vás provede portálu vytvoří virtuální počítače pomocí modelu Resource Manager doporučené místo klasického modelu použitým v tomto tématu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b01d5-160">Note that the tutorial that walks you through the portal creates VMs using the recommended Resource Manager model, rather than the classic model used in this PowerShell topic.</span></span>

<span data-ttu-id="b01d5-161">Kromě těchto prostředků, doporučujeme, abyste si prošli [další témata související s SQL serverem v Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b01d5-161">In addition to these resources, we recommend that you review [other topics related to running SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
