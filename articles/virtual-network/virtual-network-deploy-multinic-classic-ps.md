---
title: "aaaCreate virtuální počítač (klasický) s více síťovými kartami - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač (klasický) s více síťovými kartami pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90c967929bb418042c3fb7079e0f69246faac53c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a>Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí prostředí PowerShell

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) tooeach vaše virtuálních počítačů. Několik síťových adaptérů povolit oddělení typů přenosů mezi síťové adaptéry. Například jedna síťová karta může komunikovat s hello Internetu, zatímco jiné komunikuje jenom s interním prostředkům není připojen toohello Internetu. Hello možnost tooseparate síťový provoz na několik síťových adaptérů je vyžadována pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak tooperform tyto kroky, pomocí hello [modelu nasazení Resource Manager](virtual-network-deploy-multinic-arm-ps.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.

## <a name="prerequisites"></a>Požadavky

Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář. toocreate těchto prostředků, dokončení hello kroky, které následují. Vytvoření virtuální sítě pomocí následujících kroků hello v hello [vytvořit virtuální síť](virtual-networks-create-vnet-classic-netcfg-ps.md) článku.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a>Vytvořit hello back-end virtuální počítače
Hello virtuálních počítačů v back-end závisí na vytvoření hello hello následující prostředky:

* **Back-end podsítě**. Hello databázové servery budou součástí samostatnou podsíť, toosegregate provoz. níže uvedený skript Hello očekává, že tuto podsíť tooexist ve virtuální síti s názvem *WTestVnet*.
* **Účet úložiště pro datové disky**. Pro lepší výkon použije hello datových disků na serverech databáze hello technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium. Ujistěte se, zda text hello umístění Azure nasazujete toosupport storage úrovně premium.
* **Skupina dostupnosti**. Všechny databázové servery budou přidány sady dostupnosti. jeden tooa, tooensure alespoň jeden z virtuálních počítačů hello je v provozu během údržby.

### <a name="step-1---start-your-script"></a>Krok 1 – spustit skript
Si můžete stáhnout hello úplné použít skript prostředí PowerShell [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Postupujte podle kroků hello toochange hello skriptu toowork ve vašem prostředí.

1. Změnit hello hodnoty proměnných hello níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. Změna hodnoty hello hello proměnných níže na základě hodnot hello chcete toouse pro vaše nasazení back-end.

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače
Je nutné toocreate nová Cloudová služba a úložiště účtu pro hello datových disků pro všechny virtuální počítače. Budete také potřebovat toospecify bitovou kopii a účet místního správce pro hello virtuálních počítačů. dokončení těchto prostředků, toocreate hello následující kroky:

1. Vytvořte novou cloudovou službu.

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. Vytvořte nový účet úložiště premium.

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. Účet úložiště hello sadu vytvořili výše jako hello aktuální účet úložiště pro vaše předplatné.

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. Vyberte bitovou kopii pro hello virtuálních počítačů.

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. Nastavte přihlašovací údaje účtu místního správce hello.

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a>Krok 3 – vytvoření virtuálních počítačů
Musíte toouse toocreate smyčky, jak velký počet virtuálních počítačů a vytvořit hello potřebné síťové karty a virtuální počítače v rámci hello smyčky. toocreate hello síťové karty a virtuální počítače, spusťte hello následující kroky.

1. Spuštění `for` smyčky toorepeat hello příkazy toocreate virtuální počítač a dva síťové adaptéry s funkcí jako mnohokrát podle potřeby podle hello hodnotu hello `$numberOfVMs` proměnné.

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Vytvoření `VMConfig` objekt hello obrázku, velikost a sadu dostupnosti pro hello virtuálních počítačů.

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. Hello zřídit virtuální počítač jako virtuální počítač s Windows.

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. Nastavit výchozí hello síťový adaptér a přiřaďte mu statickou IP adresu.

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. Přidejte druhý síťový adaptér pro každý virtuální počítač.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. Vytvořte toodata disků pro každý virtuální počítač.

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. Vytvořte každý virtuální počítač a end hello smyčky.

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-hello-script"></a>Krok 4 – spustit skript hello
Teď, když jste stáhli a změnit hello skriptu na základě potřeb, o mu skriptu toocreate hello back-end databáze virtuálních počítačů s více síťovými kartami.

1. Uložte skript a spusťte jej z hello **prostředí PowerShell** příkazového řádku, nebo **prostředí PowerShell ISE**. Zobrazí se výstup hello počáteční, a jak je uvedeno níže.

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. Vyplňte hello informace potřebné v řádku hello přihlašovací údaje a klikněte na **OK**. výstup Hello níže se vrátí.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
