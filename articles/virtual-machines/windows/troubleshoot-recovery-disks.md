---
title: "aaaUse a řešení potíží s virtuálních počítačů v prostředí Azure PowerShell systému Windows | Microsoft Docs"
description: "Zjistěte, jak problémy tootroubleshoot virtuální počítač s Windows v Azure připojením obnovení tooa disku hello operačního systému virtuálního počítače pomocí prostředí Azure PowerShell"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 7a6a76f64824fe5d06dc4286cb1d87ab8bb794e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-azure-powershell"></a>Řešení potíží s virtuální počítač s Windows připojením obnovení tooa disku hello operačního systému virtuálního počítače pomocí prostředí Azure PowerShell
Pokud Windows virtuálního počítače (VM) v prostředí Azure dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe. Běžným příkladem bude aplikaci, která selhala aktualizace, která brání hello virtuální počítač schopný tooboot úspěšně. Tento článek podrobnosti o tom, jak tooconnect toouse prostředí Azure PowerShell vaše virtuální pevný disk tooanother virtuální počítač s Windows toofix všechny chyby, potom je znovu vytvořit původní virtuální počítač.


## <a name="recovery-process-overview"></a>Přehled procesu obnovení
řešení potíží s procesem Hello vypadá takto:

1. Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.
2. Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Windows pro účely odstraňování potíží.
3. Připojte toohello řešení potíží s virtuálních počítačů. Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.
4. Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.
5. Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.

Ujistěte se, že máte [hello nejnovější prostředí Azure PowerShell](/powershell/azure/overview) nainstalován a přihlášení tooyour předplatného:

```powershell
Login-AzureRMAccount
```

V hello následujících příkladech nahraďte vlastními hodnotami názvy parametrů. Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.


## <a name="determine-boot-issues"></a>Určení spouštěcí problémy
Zobrazí se snímek virtuálního počítače v Azure toohelp potíží při spouštění systému. Tento snímek obrazovky může pomoct identifikovat, proč tooboot selže virtuálního počítače. Hello následující příklad načte hello – snímek obrazovky ze hello virtuální počítač s Windows s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Zkontrolujte toodetermine – snímek obrazovky hello proč hello virtuálního počítače selhává tooboot. Poznámka: všechny specifické chybové zprávy nebo kódy chyb, které jsou dispozici.


## <a name="view-existing-virtual-hard-disk-details"></a>Zobrazení podrobností existující virtuální pevný disk
Než můžete připojit vaše tooanother virtuální pevný disk virtuálního počítače, je třeba název hello tooidentify hello virtuálního pevného disku (VHD).

Hello následující příklad načte informace o hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Vyhledejte `Vhd URI` v rámci hello `StorageProfile` části z výstupu hello hello předcházející příkaz. Hello následující zkrácený příklad výstupu zobrazuje následující stavy hello `Vhd URI` hello konci hello blok kódu:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Odstraňte existující virtuální počítač
Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky. Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace. Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC). Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů. I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače. Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.

první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe. Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště. Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.

Následující příklad odstranění Hello hello virtuálního počítače s názvem `myVM` z hello skupinu prostředků s názvem `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače. Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Připojit existující virtuální pevný disk tooanother virtuálních počítačů
Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží. Připojte hello existující virtuální pevný disk toothis řešení potíží s toobrowse virtuálních počítačů a upravit obsah hello disku. Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu. Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.

Když připojíte hello existující virtuální pevný disk, zadejte hello URL toohello disk získaných v předchozím hello `Get-AzureRmVM` příkaz. Hello následující příklad připojí existující virtuální pevný disk toohello řešení potíží s virtuálním počítačem s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Přidání disku vyžaduje toospecify hello velikost disku hello. Jak jsme připojit stávající disk, hello `-DiskSizeInGB` je zadán jako `$null`. Tato hodnota zajistí hello datový disk je správně připojena a bez hello potřebovat toodetermine hello true velikost datového disku.


## <a name="mount-hello-attached-data-disk"></a>Připojte disk připojená data hello

1. Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour protokolu RDP. Hello následující příklad soubory ke stažení souboru připojení hello RDP pro virtuální počítač s názvem hello `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`a stáhne je příliš`C:\Users\ops\Documents`"

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Hello datový disk je automaticky zjistil a připojené. Zobrazte seznam hello podle písmene jednotky hello toodetermine připojené svazky následujícím způsobem:

    ```powershell
    Get-Disk
    ```

    Hello následující příklad výstupu zobrazuje hello virtuální pevný disk připojený disk **2**. (Můžete také použít `Get-Volume` písmeno jednotky tooview hello):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Vyřešte problémy na původní virtuální pevný disk
S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby. Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Odpojte Image a odpojit původní virtuální pevný disk
Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit hello existující virtuální pevný disk z virtuálního počítače řešení potíží. Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.

1. Z v rámci relace RDP, odpojte hello datový disk na váš virtuální počítač pro obnovení. Je třeba číslo disku hello z hello předchozí `Get-Disk` rutiny. Poté použijte `Set-Disk` tooset hello jako v režimu offline disku:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Potvrďte hello disk je teď nastavená jako offline pomocí `Get-Disk` znovu. Hello následující příklad výstupu zobrazuje hello disk je teď nastavená jako offline:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Ukončete relaci protokolu RDP. Z relace prostředí Azure PowerShell odeberte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Vytvoření virtuálního počítače z původního pevného disku
použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Šablona JSON skutečné Hello je na hello následující odkaz:

- https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD-Existing-vnet/azuredeploy.JSON

Šablona Hello nasadí virtuální počítač do existující virtuální síť pomocí hello adresu URL VHD z hello dříve příkaz. Hello následující příklad nasadí hello šablony toohello skupinu prostředků s názvem `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Odpověď hello vyzve k zadání šablony hello například název virtuálního počítače, typ operačního systému a velikost virtuálního počítače. Hello `osDiskVhdUri` hello je stejný jako použil při připojení hello existující virtuální pevný disk toohello řešení potíží s virtuálních počítačů.


## <a name="re-enable-boot-diagnostics"></a>Opětovné povolení Diagnostika spouštění

Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené. Hello následující příklad povolí rozšíření diagnostiky hello na hello virtuálního počítače s názvem `myVMDeployed` v hello skupinu prostředků s názvem `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [tooan řešení potíží s RDP připojení virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuálním počítači Windows](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
