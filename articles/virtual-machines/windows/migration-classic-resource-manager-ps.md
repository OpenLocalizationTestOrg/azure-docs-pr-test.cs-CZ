---
title: "aaaMigrate tooResource Manager pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede hello migrace podporované platformy IaaS prostředků, jako jsou virtuální počítače (VM), virtuální sítě (virtuální sítě) a účty úložiště z classic tooAzure Resource Manager (ARM) pomocí příkazů prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: b5b2987da292f1c241be71a354b0c2e1a96a07c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-powershell"></a>Migraci prostředky infrastruktury z classic tooAzure správce prostředků pomocí Azure PowerShell
Tyto kroky vám ukážou, jak toouse prostředí Azure PowerShell příkazy toomigrate infrastruktury jako služby (IaaS) prostředky z modelu nasazení classic hello, toohello modelu nasazení Azure Resource Manager.

Pokud chcete, můžete také migrovat prostředky pomocí hello [rozhraní příkazového řádku Azure (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* Pro informace o podporovaných scénářů migrace, viz [platformy podporované migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md).
* Podrobné pokyny a migrace návod najdete v tématu [technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).
* [Běžné chyby při migraci](migration-classic-resource-manager-errors.md)

<br>
Tady je pořadí tooidentify hello vývojový diagram, ve kterém kroky nutné toobe provést během procesu migrace

![Snímek obrazovky, který zobrazuje kroky migrace hello](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a>Krok 1: Plánování migrace
Tady je několik osvědčených postupů, které doporučujeme jak vyhodnotit migrace prostředky infrastruktury z classic tooResource Manager:

* Pročtěte hello [podporované a nepodporované funkce a konfigurace](migration-classic-resource-manager-overview.md). Pokud máte virtuální počítače, které používají nepodporované konfigurace nebo funkce, doporučujeme počkejte hello konfigurace nebo funkce podpory toobe oznámeno. Případně pokud ji vyhovuje vašim potřebám, odeberte tuto funkci nebo přesunout mimo tuto tooenable migraci konfigurace.
* Pokud máte automatizované skripty, které jsou dnes nasazení infrastruktury a aplikace, zkuste toocreate podobné nastavení testu pomocí těchto skriptů pro migraci. Alternativně můžete nastavit ukázkové prostředí pomocí hello portálu Azure.

> [!IMPORTANT]
> Application Gateway nejsou aktuálně podporovány pro migraci z classic tooResource správce. Před spuštěním síť Příprava operace toomove hello odebrat toomigrate klasickou virtuální síť s aplikační brány, brány hello. Po dokončení migrace hello znovu hello brány ve službě Správce prostředků Azure.
>
>Připojení tooExpressRoute okruhů v jiné předplatné brány ExpressRoute se nedají automaticky migrovat. V takových případech odeberte hello brány ExpressRoute, migrovat hello virtuální síť a znovu hello brány. Najdete v tématu [migrovat ExpressRoute okruhy a přidružené virtuální sítě z modelu nasazení Resource Manager classic toohello hello](../../expressroute/expressroute-migration-classic-resource-manager.md) Další informace.
>
>

## <a name="step-2-install-hello-latest-version-of-azure-powershell"></a>Krok 2: Instalace hello nejnovější verzi prostředí Azure PowerShell
Existují dvě hlavní možnosti tooinstall prostředí Azure PowerShell: [Galerie prostředí PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) nebo [instalačního programu webové platformy (WebPI)](http://aka.ms/webpi-azps). WebPI obdrží měsíčních aktualizací. Galerie prostředí PowerShell získává aktualizace trvale. Tento článek je založená na prostředí Azure PowerShell verze 2.1.0.

Pokyny k instalaci naleznete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-hello-subscription-in-azure-portal"></a>Krok 3: Ujistěte se, že jste správce pro předplatné hello na portálu Azure
tooperform této migrace, je třeba přidat jako spolusprávce pro předplatné hello v hello [portál Azure](https://portal.azure.com).

1. Přihlaste se k hello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello vyberte **předplatné**. Pokud ho nevidíte, vyberte **další služby**.
3. Najít položku odpovídající předplatné hello, potom si prohlédněte hello **Moje ROLE** pole. Pro správce a společné hello hodnota by měla být _správce účtu_.

Pokud si nejste možné tooadd společné správce, pak obraťte se na služby správce nebo spolusprávce pro tooget předplatné hello sami přidat.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>Krok 4: Nastavte předplatné a zaregistrujte se pro migraci
Nejprve spusťte příkazovém řádku prostředí PowerShell. Pro migraci, je nutné tooset prostředí pro obě classic a Resource Manager.

Přihlaste se tooyour účet pro hello modelu Resource Manager.

```powershell
    Login-AzureRmAccount
```

Získáte dostupných předplatných hello pomocí hello následující příkaz:

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

Nastavte vašeho předplatného Azure pro hello aktuální relaci. Tento příklad sady hello výchozí název odběru příliš**Moje předplatné Azure**. Nahraďte název odběru příklad hello vlastní.

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Registrace je jednorázové krok, ale musíte to provést jednou před pokusem o migraci. Bez registrace, najdete v části hello následující chybová zpráva:
>
> *Struktura BadRequest: Předplatné není zaregistrované pro migraci.*
>
>

Zaregistrovat u zprostředkovatele prostředků migrace hello pomocí hello následující příkaz:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Počkejte pět minut, než toofinish registrace hello. Stav hello hello schválení můžete zkontrolovat pomocí hello následující příkaz:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Ujistěte se, že je RegistrationState `Registered` než budete pokračovat.

Nyní přihlašovací účet tooyour pro hello klasického modelu.

```powershell
    Add-AzureAccount
```

Získáte dostupných předplatných hello pomocí hello následující příkaz:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Nastavte vašeho předplatného Azure pro hello aktuální relaci. Tento příklad nastaví hello výchozí předplatné příliš**Moje předplatné Azure**. Nahraďte název odběru příklad hello vlastní.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a>Krok 5: Zkontrolujte, zda že máte dostatečný počet jader virtuálního počítače Azure Resource Manager v hello oblast Azure vaše aktuální nasazení nebo virtuální sítě
Můžete použít hello následující prostředí PowerShell příkaz toocheck hello aktuální počet jader, které máte ve službě Správce prostředků Azure. toolearn Další informace o základní kvóty, najdete v části [omezení a hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

Tento příklad zkontroluje dostupnost hello ve hello **západní USA** oblast. Nahraďte název oblasti příklad hello vlastní.

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-toomigrate-your-iaas-resources"></a>Krok 6: Spuštění příkazů toomigrate vašich prostředků IaaS
> [!NOTE]
> Všechny operace hello postupu popsaného tady jsou idempotent. Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme, abyste se znovu pokusíte hello připravit, zrušení nebo potvrzení operace. Platforma Hello potom pokusí hello akci znovu.
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Krok 6.1: Možnost 1 - migraci virtuálních počítačů v rámci cloudové služby (ne ve virtuální síti)
Získání seznamu hello cloudových služeb pomocí hello následující příkaz a potom vyberte hello cloudové služby, které chcete toomigrate. Pokud jsou hello virtuálních počítačů v rámci hello cloudové služby ve virtuální síti nebo mají role web nebo worker, hello příkaz vrátí chybovou zprávu.

```powershell
    Get-AzureService | ft Servicename
```

Získáte název nasazení hello hello cloudové služby. V tomto příkladu je název služby hello **Moje služba**. Nahraďte název službu příkladu hello svůj vlastní název služby.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Připravte hello virtuálních počítačů v rámci hello cloudové služby pro migraci. Máte dvě možnosti toochoose z.

* **Možnost 1. Migrovat hello virtuální počítače tooa platformy vytvořit virtuální síť**

    Nejprve ověřte, jestli je možné migrovat hello cloudové služby pomocí hello následující příkazy:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Hello předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace. Pokud je ověření úspěšné, pak můžete přesunout na toohello **Příprava** kroku:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Možnost 2. Migrovat existující virtuální síť tooan v modelu nasazení Resource Manager hello**

    Tento příklad sady hello název skupiny prostředků příliš**myResourceGroup**, hello název virtuální sítě příliš**myVirtualNetwork** a hello název podsítě příliš**mySubNet**. Nahraďte názvy hello v příkladu hello hello názvy vlastních prostředků.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Nejprve ověřte, jestli je možné migrovat hello virtuální sítě pomocí hello následující příkaz:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Hello předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace. Pokud je ověření úspěšné, pak můžete pokračovat hello následující přípravný krok:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Po úspěšné hello Příprava operace s některou z předchozích možností hello dotaz na stav migrace hello hello virtuálních počítačů. Ujistěte se, že jsou v hello `Prepared` stavu.

Tento příklad sady hello název virtuálního počítače příliš**Můjvp**. Příklad názvu hello nahraďte název virtuálního počítače.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

Zkontrolujte konfiguraci hello hello připravenou prostředky pomocí prostředí PowerShell nebo hello portálu Azure. Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte hello následující příkaz:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>Krok 6.1: Možnost 2 - migrovat virtuální počítače ve virtuální síti

toomigrate virtuální počítače ve virtuální síti, můžete migrovat hello virtuální síť. Hello virtuální počítače migrovat automaticky pomocí hello virtuální sítě. Vybrat hello virtuální sítě, které chcete toomigrate.
> [!NOTE]
> [Migrovat jeden virtuální počítač classic](migrate-single-classic-to-resource-manager.md) po vytvoření nového virtuálního počítače správce prostředků se spravovaná diskům použít (hello virtuálního pevného disku operačního systému a datových) souborů hello virtuálního počítače.
<br>

> [!NOTE]
> název virtuální sítě Hello se může lišit od informace zobrazené v hello nového portálu. Hello nový portál Azure zobrazí název hello jako `[vnet-name]` ale hello skutečné virtuální síť s názvem je typu `Group [resource-group-name] [vnet-name]`. Před migrací, vyhledávání hello skutečné virtuální síť s názvem pomocí příkazu hello `Get-AzureVnetSite | Select -Property Name` nebo ji zobrazit v hello starý portál Azure. 

Tento příklad sady hello název virtuální sítě příliš**myVnet**. Nahraďte název virtuální sítě příklad hello vlastní.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Pokud hello virtuální síť obsahuje webové nebo rolí pracovního procesu nebo virtuálních počítačů s nepodporované konfigurace, zobrazí chybovou zprávu ověření.
>
>

Nejprve ověřte, jestli virtuální sítě hello lze migrovat pomocí hello následující příkaz:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Hello předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace. Pokud je ověření úspěšné, pak můžete pokračovat hello následující přípravný krok:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Zkontrolujte konfiguraci hello hello připravenou virtuálních počítačů pomocí Azure PowerShell nebo hello portálu Azure. Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte hello následující příkaz:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a>Krok 6.2 migrací účtu úložiště
Po dokončení migrace hello virtuálních počítačů, doporučujeme že migraci účtů úložiště hello.

Před migrací hello účet úložiště, proveďte předcházející kontrol požadovaných součástí:

* **Migrace klasické virtuální počítače, jejichž disky jsou uložené v účtu úložiště hello**

    Předcházející příkaz vrátí RoleName a DiskName vlastnosti všech hello classic disků virtuálních počítačů v účtu úložiště hello. RoleName je název hello toowhich hello virtuálního počítače, který je připojen na disk. Pokud předcházející příkaz vrátí disky pak se ujistěte, že virtuální počítače toowhich tyto disky připojené jsou migrovány před migrací hello účet úložiště.
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* **Odstranit odpojit classic disky virtuálních počítačů uložených v účtu storage hello**

    Najít odpojit classic disky virtuálních počítačů v úložišti hello účet pomocí následující příkaz:

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    Pokud se výše příkaz vrátí disky pak odstraňte tyto disky pomocí následující příkaz:

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* **Odstranit Image virtuálních počítačů uložených v účtu storage hello**

    Předcházející příkaz vrátí všechny Image virtuálních počítačů hello disk operačního systému uložené v účtu úložiště hello.
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     Předcházející příkaz vrátí všechny Image virtuálních počítačů hello s datovými disky uložené v účtu úložiště hello.
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    Odstraňte všechny Image virtuálních počítačů hello vrácený výše příkazy předchozím příkazem:
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

Každý účet úložiště pro migraci ověřte pomocí hello následující příkaz. V tomto příkladu je název účtu úložiště hello **Můj_účet_úložiště**. Příklad názvu hello nahraďte hello název účtu úložiště.

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

Dalším krokem je tooPrepare hello účet úložiště pro migraci

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Zkontrolujte konfiguraci hello hello připravenou účet úložiště pomocí prostředí Azure PowerShell nebo hello portálu Azure. Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte hello následující příkaz:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Další kroky
* [Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Pomocí rozhraní příkazového řádku toomigrate IaaS prostředky z classic tooAzure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Komunita nástroje asistence s migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Běžné chyby při migraci](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
