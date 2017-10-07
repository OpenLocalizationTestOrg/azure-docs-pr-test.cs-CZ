---
title: "aaaUse a řešení potíží s virtuálních počítačů s hello Azure CLI 1.0 Linux | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot, které vystavuje virtuálního počítače s Linuxem pomocí připojování hello operačního systému disku tooa obnovení virtuálního počítače Azure CLI 1.0"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 398f681d1149299d444fcfdab20737315db02855
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-cli-10"></a>Řešení potíží s virtuálního počítače s Linuxem připojením hello operačního systému disku tooa obnovení virtuálních počítačů pomocí Azure CLI 1.0 hello
Pokud systém Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe. Běžným příkladem by neplatná položka v `/etc/fstab` , který brání hello virtuálních počítačů je možné tooboot úspěšně. Tento článek podrobnosti jak toouse hello Azure CLI 1.0 tooconnect vaše virtuálního pevného disku virtuálního počítače s Linuxem toofix tooanother všechny chyby a potom ho znovu vytvořit původní virtuální počítač.


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#recovery-process-overview) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="recovery-process-overview"></a>Přehled procesu obnovení
řešení potíží s procesem Hello vypadá takto:

1. Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.
2. Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Linuxem pro účely odstraňování potíží.
3. Připojte toohello řešení potíží s virtuálních počítačů. Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.
4. Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.
5. Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.

Ujistěte se, že máte [hello nejnovější Azure CLI 1.0](../../cli-install-nodejs.md) nainstalován a přihlášení a pomocí režimu Resource Manager:

```azurecli
azure config mode arm
```

V hello následujících příkladech nahraďte vlastními hodnotami názvy parametrů. Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.


## <a name="determine-boot-issues"></a>Určení spouštěcí problémy
Zkontrolujte toodetermine sériové výstup hello proč virtuálního počítače není možné tooboot správně. Běžným příkladem jsou neplatná položka v `/etc/fstab`, nebo hello základní virtuální pevný disk se odstranil nebo přesunul.

Hello následující příklad načte sériové výstup hello ze hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

Zkontrolujte toodetermine sériové výstup hello proč hello virtuálního počítače selhává tooboot. Pokud sériové výstup hello neposkytuje jakoukoli indikaci toho, může být nutné tooreview souborů protokolů v `/var/log` až budete mít hello virtuální pevný disk připojený tooa řešení potíží s virtuálních počítačů.


## <a name="view-existing-virtual-hard-disk-details"></a>Zobrazení podrobností existující virtuální pevný disk
Než můžete připojit vaše tooanother virtuální pevný disk virtuálního počítače, je třeba název hello tooidentify hello virtuálního pevného disku (VHD). 

Hello následující příklad načte informace o hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

Vyhledejte `Vhd URI` v hello výstup hello předcházející příkaz. Hello následující zkrácený příklad výstupu zobrazuje následující stavy hello `Vhd URI` na poslední řádek hello:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myVM"
+ Looking up hello NIC "myNic"
+ Looking up hello public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a>Odstraňte existující virtuální počítač
Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky. Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace. Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC). Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů. I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače. Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.

první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe. Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště. Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.

Následující příklad odstranění Hello hello virtuálního počítače s názvem `myVM` z hello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače. Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Připojit existující virtuální pevný disk tooanother virtuálních počítačů
Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží. Připojte hello existující virtuální pevný disk toothis řešení potíží s toobrowse virtuálních počítačů a upravit obsah hello disku. Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu. Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.

Když připojíte hello existující virtuální pevný disk, zadejte hello URL toohello disk získaných v předchozím hello `azure vm show` příkaz. Hello následující příklad připojí existující virtuální pevný disk toohello řešení potíží s virtuálním počítačem s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a>Připojte disk připojená data hello

> [!NOTE]
> Hello následující příklady podrobnosti hello kroky na virtuálního počítače s Ubuntu. Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, hello umístění souborů protokolu a `mount` příkazy se můžou mírně lišit. Naleznete v dokumentaci toohello pro vaše konkrétní distro hello příslušné změny v příkazy.

1. Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour SSH. Pokud tento disk je hello první datový disk připojený tooyour řešení potíží virtuální počítač, hello disk pravděpodobně připojený příliš`/dev/sdc`. Použití `dmseg` tooview připojenými disky:

    ```bash
    dmesg | grep SCSI
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    V předchozím příkladu hello, je disk hello operačního systému na `/dev/sda` a hello dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`. Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.

2. Vytvořte adresář toomount existující virtuální pevný disk. Hello následující příklad vytvoří adresář s názvem `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Pokud máte více oddílů na existující virtuální pevný disk, připojte hello požadované oddílu. Hello následující příklad připojí hello na první primární oddíl `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Osvědčeným postupem je, že toomount datové disky na virtuálních počítačích v Azure pomocí hello identifikátor UUID (UUID) hello virtuálního pevného disku. Pro tento krátký odstraňování potíží není nutné připojování hello virtuální pevný disk pomocí hello UUID. Ale při normálním používání úpravy `/etc/fstab` toomount virtuálních pevných disků pomocí název zařízení, nikoli UUID může způsobit hello tooboot toofail virtuálních počítačů.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Vyřešte problémy na původní virtuální pevný disk
S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby. Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Odpojte Image a odpojit původní virtuální pevný disk
Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit hello existující virtuální pevný disk z virtuálního počítače řešení potíží. Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.

1. Z relace tooyour SSH hello řešení potíží virtuální počítač odpojte hello existující virtuální pevný disk. Nejprve změňte mimo hello nadřazený adresář pro přípojného bodu:

    ```bash
    cd /
    ```

    Nyní odpojte hello existující virtuální pevný disk. Hello následující příklad odpojí hello zařízení na `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nyní odpojte hello virtuálního pevného disku z hello virtuálních počítačů. Ukončete tooyour relace SSH hello řešení potíží s virtuálních počítačů. V hello rozhraní příkazového řádku Azure první hello seznamu připojený datové disky tooyour řešení potíží s virtuálních počítačů. Hello následující příklad zobrazí hello datových disků připojených toohello virtuálního počítače s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    Poznámka: hello `Lun` hodnotu pro existující virtuální pevný disk. Hello výstupu příkazu následující příklad ukazuje hello existujícího virtuálního disku připojená na logické jednotce 0:

    ```azurecli
    info:    Executing command vm disk list
    + Looking up hello VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    Odpojit hello datový disk od virtuálního počítače pomocí hello použít `Lun` hodnotu:

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a>Vytvoření virtuálního počítače z původního pevného disku
použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). Šablona JSON skutečné Hello je na hello následující odkaz:

- https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON

Šablona Hello nasadí virtuální počítač do existující virtuální síť pomocí hello adresu URL VHD z hello dříve příkaz. Hello následující příklad nasadí hello šablony toohello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

Odpověď hello vyzve k zadání hello šablony, jako je například název virtuálního počítače (`myDeployedVM` hello v následujícím příkladu), typ operačního systému (`Linux`) a velikost virtuálního počítače (`Standard_DS1_v2`). Hello `osDiskVhdUri` hello je stejný jako použil při připojení hello existující virtuální pevný disk toohello řešení potíží s virtuálních počítačů. Příklad výstupu příkazu hello a vyzve vypadá takto:

```azurecli
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment toocomplete
+
```


## <a name="re-enable-boot-diagnostics"></a>Opětovné povolení Diagnostika spouštění

Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené. Hello následující příklad povolí rozšíření diagnostiky hello na hello virtuálního počítače s názvem `myDeployedVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [řešení SSH připojení tooan virtuálního počítače Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
