---
title: "aaaCreate virtuálního počítače systému SQL Server v prostředí Azure PowerShell (klasické) | Microsoft Docs"
description: "Obsahuje kroky a skriptů prostředí PowerShell pro vytvoření virtuálního počítače Azure s obrázky Galerie virtuálního počítače systému SQL Server. Toto téma používá režim hello nasazení classic."
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
ms.openlocfilehash: b14d5d9bc192fa0a21126395ee20ffd06b3bf47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a><span data-ttu-id="59194-104">Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="59194-104">Provision a SQL Server virtual machine using Azure PowerShell (Classic)</span></span>

<span data-ttu-id="59194-105">Tento článek popisuje kroky pro jak hello toocreate virtuálního počítače systému SQL Server v Azure pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59194-105">This article provides steps for how toocreate a SQL Server virtual machine in Azure by using hello PowerShell cmdlets.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="59194-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="59194-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="59194-107">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="59194-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="59194-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="59194-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="59194-109">Hello Resource Manager verzi tohoto tématu, naleznete v části [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="59194-109">For hello Resource Manager version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span></span>

### <a name="install-and-configure-powershell"></a><span data-ttu-id="59194-110">Instalace a konfigurace prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="59194-110">Install and configure PowerShell:</span></span>
1. <span data-ttu-id="59194-111">Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="59194-111">If you do not have an Azure account, visit [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
2. <span data-ttu-id="59194-112">[Stáhněte a nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="59194-112">[Download and install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>
3. <span data-ttu-id="59194-113">Spusťte prostředí Windows PowerShell a připojte ho tooyour předplatného Azure s hello **Add-AzureAccount** příkaz.</span><span class="sxs-lookup"><span data-stu-id="59194-113">Start Windows PowerShell, and connect it tooyour Azure subscription with hello **Add-AzureAccount** command.</span></span>

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a><span data-ttu-id="59194-114">Určení cílových oblast Azure</span><span class="sxs-lookup"><span data-stu-id="59194-114">Determine your target Azure region</span></span>

<span data-ttu-id="59194-115">Váš virtuální počítač SQL Server bude hostován v rámci cloudové služby, který se nachází v určité oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="59194-115">Your SQL Server Virtual Machine will be hosted in a cloud service that resides a specific Azure region.</span></span> <span data-ttu-id="59194-116">Hello pomůžou tyto kroky můžete toodetermine oblast, účtu úložiště a Cloudová služba, která se použije pro hello zbytek hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="59194-116">hello following steps help you toodetermine your region, storage account, and cloud service that will be used for hello rest of hello tutorial.</span></span>

1. <span data-ttu-id="59194-117">Určení hello datové centrum, které chcete toouse toohost virtuální počítač s SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="59194-117">Determine hello data center that you want toouse toohost your SQL Server VM.</span></span> <span data-ttu-id="59194-118">Následující příkaz prostředí PowerShell Hello zobrazí seznam názvů dostupné oblasti.</span><span class="sxs-lookup"><span data-stu-id="59194-118">hello following PowerShell command displays a list of available region names.</span></span>

   ```powershell
   (Get-AzureLocation).Name
   ```

2. <span data-ttu-id="59194-119">Jakmile jste zformulovali upřednostňované umístění, nastavení proměnné s názvem **$dcLocation** toothat oblast.</span><span class="sxs-lookup"><span data-stu-id="59194-119">Once you've identified your preferred location, set a variable named **$dcLocation** toothat region.</span></span> <span data-ttu-id="59194-120">Například hello následující příkaz nastaví hello oblast příliš "East US":</span><span class="sxs-lookup"><span data-stu-id="59194-120">For example, hello following command sets hello region too"East US":</span></span>

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a><span data-ttu-id="59194-121">Nastavit vaše předplatné a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="59194-121">Set your subscription and storage account</span></span>

1. <span data-ttu-id="59194-122">Určení hello předplatné Azure, které budete používat pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59194-122">Determine hello Azure subscription you will use for hello new virtual machine.</span></span>

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. <span data-ttu-id="59194-123">Přiřadit vašeho cílového předplatného Azure toohello **$subscr** proměnné.</span><span class="sxs-lookup"><span data-stu-id="59194-123">Assign your target Azure subscription toohello **$subscr** variable.</span></span> <span data-ttu-id="59194-124">Potom nastavením jako vaším aktuálním předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="59194-124">Then set this as your current Azure subscription.</span></span>

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. <span data-ttu-id="59194-125">Zkontrolujte pro existující účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="59194-125">Then check for existing storage accounts.</span></span> <span data-ttu-id="59194-126">Hello následující skript zobrazí všechny účty úložiště, které existují ve vaší vybrané oblasti:</span><span class="sxs-lookup"><span data-stu-id="59194-126">hello following script displays all storage accounts that exist in your chosen region:</span></span>

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > <span data-ttu-id="59194-127">Pokud budete potřebovat nový účet úložiště, nejprve vytvořte název účtu úložiště všechna malá příkazem hello New-AzureStorageAccount jako hello následující ukázka:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span><span class="sxs-lookup"><span data-stu-id="59194-127">If you require a new storage account, first create an all-lower-case storage account name with hello New-AzureStorageAccount command as in hello following example: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span></span>

4. <span data-ttu-id="59194-128">Přiřadit hello cílové úložiště účet název toohello **$staccount**.</span><span class="sxs-lookup"><span data-stu-id="59194-128">Assign hello target storage account name toohello **$staccount**.</span></span> <span data-ttu-id="59194-129">Potom pomocí **Set-AzureSubscription** tooset hello předplatného a aktuální účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="59194-129">Then use **Set-AzureSubscription** tooset hello subscription and current storage account.</span></span>

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a><span data-ttu-id="59194-130">Vyberte bitovou kopii virtuálního počítače systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="59194-130">Select a SQL Server virtual machine image</span></span>

1. <span data-ttu-id="59194-131">Zjistěte hello seznam dostupných imagí SQL serveru virtuálních počítačů z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="59194-131">Find out hello list of available SQL Server virtual machines images from hello gallery.</span></span> <span data-ttu-id="59194-132">Tyto Image jsou vybavené **ImageFamily** vlastnost, která začíná "SQL".</span><span class="sxs-lookup"><span data-stu-id="59194-132">These images all have an **ImageFamily** property that starts with "SQL".</span></span> <span data-ttu-id="59194-133">Hello následující dotaz zobrazí hello image rodiny k dispozici tooyou s předinstalovaným systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="59194-133">hello following query displays hello image family available tooyou that have SQL Server preinstalled.</span></span>

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. <span data-ttu-id="59194-134">Pokud zjistíte, rodina bitové kopie virtuálního počítače hello, může být více bitových kopií publikované v této rodině.</span><span class="sxs-lookup"><span data-stu-id="59194-134">When you find hello  virtual machine image family, there could be multiple published images in this family.</span></span> <span data-ttu-id="59194-135">Použití hello následující skript toofind hello nejnovější publikované název virtuálního počítače bitové kopie pro vybranou image rodinu (například **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2**):</span><span class="sxs-lookup"><span data-stu-id="59194-135">Use hello following script toofind hello latest published virtual machine image name for your selected image family (such as **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2**):</span></span>

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-hello-virtual-machine"></a><span data-ttu-id="59194-136">Vytvoření virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="59194-136">Create hello virtual machine</span></span>

<span data-ttu-id="59194-137">Nakonec vytvořte hello virtuálního počítače pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="59194-137">Finally, create hello virtual machine with PowerShell:</span></span>

1. <span data-ttu-id="59194-138">Vytvoření cloudu toohost služby hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59194-138">Create a cloud service toohost hello new VM.</span></span> <span data-ttu-id="59194-139">Všimněte si, že je také možné toouse stávající cloudovou službu místo.</span><span class="sxs-lookup"><span data-stu-id="59194-139">Note that it is also possible toouse an existing cloud service instead.</span></span> <span data-ttu-id="59194-140">Vytvoření nové proměnné **$svcname** s hello krátký název hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="59194-140">Create a new variable **$svcname** with hello short name of hello cloud service.</span></span>

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. <span data-ttu-id="59194-141">Zadejte název virtuálního počítače hello a velikost.</span><span class="sxs-lookup"><span data-stu-id="59194-141">Specify hello virtual machine name and a size.</span></span> <span data-ttu-id="59194-142">Další informace o velikosti virtuálních počítačů najdete v tématu [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59194-142">For more information about virtual machine sizes, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see hello link toohello other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. <span data-ttu-id="59194-143">Zadejte účet místního správce hello a heslo.</span><span class="sxs-lookup"><span data-stu-id="59194-143">Specify hello local administrator account and password.</span></span>

   ```powershell
   $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. <span data-ttu-id="59194-144">Spusťte následující skript toocreate hello virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="59194-144">Run hello following script toocreate hello virtual machine.</span></span>

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> <span data-ttu-id="59194-145">Další vysvětlení a možnosti konfigurace najdete v tématu hello **sestavení vaší sadu příkazů** kapitoly [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59194-145">For additional explanation and configuration options, see hello **Build your command set** section in [Use Azure PowerShell toocreate and preconfigure Windows-based Virtual Machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="example-powershell-script"></a><span data-ttu-id="59194-146">Ukázkový skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="59194-146">Example PowerShell script</span></span>

<span data-ttu-id="59194-147">Hello následující skript představuje příklad dokončení skriptu, který vytvoří **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2** virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59194-147">hello following script provides an example of a complete script that creates a **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2** virtual machine.</span></span> <span data-ttu-id="59194-148">Pokud chcete použít tento skript, je nutné přizpůsobit hello počáteční proměnných vycházejících z hello předchozí kroky v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="59194-148">If you use this script, you must customize hello initial variables based on hello previous steps in this topic.</span></span>

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set hello current subscription and storage account
# Comment out hello New-AzureStorageAccount line if hello account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select hello most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create hello new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create hello VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create hello SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a><span data-ttu-id="59194-149">Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="59194-149">Connect with remote desktop</span></span>

1. <span data-ttu-id="59194-150">Vytvoření souborů RDP hello toolaunch složky dokumentu hello aktuálního uživatele instalace toocomplete tyto virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="59194-150">Create hello RDP files in hello current user's document folder toolaunch these virtual machines toocomplete setup:</span></span>

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. <span data-ttu-id="59194-151">Hello adresáře dokumenty spusťte soubor RDP hello.</span><span class="sxs-lookup"><span data-stu-id="59194-151">In hello documents directory, launch hello RDP file.</span></span> <span data-ttu-id="59194-152">Spojte se s hello správce uživatelské jméno a heslo zadané dříve (například pokud vaše uživatelské jméno je VMAdmin, zadejte "\VMAdmin" jako uživatel hello a zadejte heslo hello).</span><span class="sxs-lookup"><span data-stu-id="59194-152">Connect with hello administrator user name and password provided earlier (for example, if your user name was VMAdmin, specify "\VMAdmin" as hello user and provide hello password).</span></span>

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-hello-configuration-of-hello-sql-server-machine-for-remote-access"></a><span data-ttu-id="59194-153">Dokončení hello konfiguraci hello počítače serveru SQL Server pro vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="59194-153">Complete hello configuration of hello SQL Server Machine for remote access</span></span>

<span data-ttu-id="59194-154">Po přihlášení toohello počítači pomocí vzdálené plochy se konfigurace systému SQL Server podle pokynů hello v [kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="59194-154">After logging on toohello machine with remote desktop, configure SQL Server based on hello instructions in [Steps for configuring SQL Server connectivity in an Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59194-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59194-155">Next steps</span></span>

<span data-ttu-id="59194-156">Můžete najít další pokyny pro zřizování virtuálních počítačů pomocí prostředí PowerShell v hello [virtuální počítače dokumentaci](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59194-156">You can find additional instructions for provisioning virtual machines with PowerShell in hello [virtual machines documentation](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="59194-157">V mnoha případech je dalším krokem hello toomigrate vaší databáze toothis nový virtuální počítač SQL Server.</span><span class="sxs-lookup"><span data-stu-id="59194-157">In many cases, hello next step is toomigrate your databases toothis new SQL Server VM.</span></span> <span data-ttu-id="59194-158">Pokyny pro migraci databáze najdete v tématu [migrace databáze tooSQL Server na virtuálním počítači Azure](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59194-158">For database migration guidance, see [Migrating a Database tooSQL Server on an Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="59194-159">Pokud byste chtěli také pomocí hello Azure portálu toocreate virtuálních počítačích SQL, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="59194-159">If you're also interested in using hello Azure portal toocreate SQL Virtual Machines, see [Provisioning a SQL Server Virtual Machine on Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="59194-160">Všimněte si, že hello kurzu, můžete prostřednictvím portálu hello vytvoří virtuální počítače pomocí nevystavíte slabé stránky zabezpečení hello doporučené modelu Resource Manager, místo klasického modelu hello použitým v tomto tématu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59194-160">Note that hello tutorial that walks you through hello portal creates VMs using hello recommended Resource Manager model, rather than hello classic model used in this PowerShell topic.</span></span>

<span data-ttu-id="59194-161">Kromě toho toothese prostředky, doporučujeme, abyste si prošli [další témata související s toorunning systému SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59194-161">In addition toothese resources, we recommend that you review [other topics related toorunning SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
