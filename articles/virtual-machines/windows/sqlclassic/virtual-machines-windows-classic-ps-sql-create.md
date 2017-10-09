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
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (klasické)

Tento článek popisuje kroky pro jak hello toocreate virtuálního počítače systému SQL Server v Azure pomocí rutin prostředí PowerShell.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Hello Resource Manager verzi tohoto tématu, naleznete v části [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Instalace a konfigurace prostředí PowerShell:
1. Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
2. [Stáhněte a nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview).
3. Spusťte prostředí Windows PowerShell a připojte ho tooyour předplatného Azure s hello **Add-AzureAccount** příkaz.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Určení cílových oblast Azure

Váš virtuální počítač SQL Server bude hostován v rámci cloudové služby, který se nachází v určité oblasti Azure. Hello pomůžou tyto kroky můžete toodetermine oblast, účtu úložiště a Cloudová služba, která se použije pro hello zbytek hello kurzu.

1. Určení hello datové centrum, které chcete toouse toohost virtuální počítač s SQL serverem. Následující příkaz prostředí PowerShell Hello zobrazí seznam názvů dostupné oblasti.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Jakmile jste zformulovali upřednostňované umístění, nastavení proměnné s názvem **$dcLocation** toothat oblast. Například hello následující příkaz nastaví hello oblast příliš "East US":

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Nastavit vaše předplatné a účet úložiště

1. Určení hello předplatné Azure, které budete používat pro hello nového virtuálního počítače.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Přiřadit vašeho cílového předplatného Azure toohello **$subscr** proměnné. Potom nastavením jako vaším aktuálním předplatným Azure.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Zkontrolujte pro existující účty úložiště. Hello následující skript zobrazí všechny účty úložiště, které existují ve vaší vybrané oblasti:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Pokud budete potřebovat nový účet úložiště, nejprve vytvořte název účtu úložiště všechna malá příkazem hello New-AzureStorageAccount jako hello následující ukázka:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Přiřadit hello cílové úložiště účet název toohello **$staccount**. Potom pomocí **Set-AzureSubscription** tooset hello předplatného a aktuální účet úložiště.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>Vyberte bitovou kopii virtuálního počítače systému SQL Server

1. Zjistěte hello seznam dostupných imagí SQL serveru virtuálních počítačů z Galerie hello. Tyto Image jsou vybavené **ImageFamily** vlastnost, která začíná "SQL". Hello následující dotaz zobrazí hello image rodiny k dispozici tooyou s předinstalovaným systému SQL Server.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Pokud zjistíte, rodina bitové kopie virtuálního počítače hello, může být více bitových kopií publikované v této rodině. Použití hello následující skript toofind hello nejnovější publikované název virtuálního počítače bitové kopie pro vybranou image rodinu (například **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-hello-virtual-machine"></a>Vytvoření virtuálního počítače hello

Nakonec vytvořte hello virtuálního počítače pomocí prostředí PowerShell:

1. Vytvoření cloudu toohost služby hello nového virtuálního počítače. Všimněte si, že je také možné toouse stávající cloudovou službu místo. Vytvoření nové proměnné **$svcname** s hello krátký název hello cloudové služby.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Zadejte název virtuálního počítače hello a velikost. Další informace o velikosti virtuálních počítačů najdete v tématu [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see hello link toohello other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Zadejte účet místního správce hello a heslo.

   ```powershell
   $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Spusťte následující skript toocreate hello virtuálního počítače hello.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> Další vysvětlení a možnosti konfigurace najdete v tématu hello **sestavení vaší sadu příkazů** kapitoly [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Ukázkový skript prostředí PowerShell

Hello následující skript představuje příklad dokončení skriptu, který vytvoří **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2** virtuálního počítače. Pokud chcete použít tento skript, je nutné přizpůsobit hello počáteční proměnných vycházejících z hello předchozí kroky v tomto tématu.

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

## <a name="connect-with-remote-desktop"></a>Připojit pomocí vzdálené plochy

1. Vytvoření souborů RDP hello toolaunch složky dokumentu hello aktuálního uživatele instalace toocomplete tyto virtuální počítače:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Hello adresáře dokumenty spusťte soubor RDP hello. Spojte se s hello správce uživatelské jméno a heslo zadané dříve (například pokud vaše uživatelské jméno je VMAdmin, zadejte "\VMAdmin" jako uživatel hello a zadejte heslo hello).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-hello-configuration-of-hello-sql-server-machine-for-remote-access"></a>Dokončení hello konfiguraci hello počítače serveru SQL Server pro vzdálený přístup

Po přihlášení toohello počítači pomocí vzdálené plochy se konfigurace systému SQL Server podle pokynů hello v [kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Další kroky

Můžete najít další pokyny pro zřizování virtuálních počítačů pomocí prostředí PowerShell v hello [virtuální počítače dokumentaci](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

V mnoha případech je dalším krokem hello toomigrate vaší databáze toothis nový virtuální počítač SQL Server. Pokyny pro migraci databáze najdete v tématu [migrace databáze tooSQL Server na virtuálním počítači Azure](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Pokud byste chtěli také pomocí hello Azure portálu toocreate virtuálních počítačích SQL, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md). Všimněte si, že hello kurzu, můžete prostřednictvím portálu hello vytvoří virtuální počítače pomocí nevystavíte slabé stránky zabezpečení hello doporučené modelu Resource Manager, místo klasického modelu hello použitým v tomto tématu prostředí PowerShell.

Kromě toho toothese prostředky, doporučujeme, abyste si prošli [další témata související s toorunning systému SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
