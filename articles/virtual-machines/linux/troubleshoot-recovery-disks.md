---
title: "Použijte řešení potíží s virtuálního počítače pomocí Azure CLI 2.0 Linux | Microsoft Docs"
description: "Naučte se virtuální počítač s Linuxem potíží s připojením k obnovení virtuálního počítače pomocí Azure CLI 2.0 disk operačního systému"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 7a28accce1bd328b2b486b588c44d91b03e42122
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli-20"></a>Odstranění virtuálního počítače s Linuxem pomocí disk operačního systému se připojuje k obnovení virtuálního počítače pomocí Azure CLI 2.0
Pokud Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, musíte provést na virtuálním pevném disku, sám sebe pro řešení potíží. Běžným příkladem by neplatná položka v `/etc/fstab` , který brání virtuálního počítače se úspěšně spustil. Tento článek popisuje, jak pomocí Azure CLI 2.0 připojit virtuální pevný disk na jiný virtuální počítač s Linuxem opravte případné chyby a pak znovu vytvořte původní virtuální počítač. K provedení těchto kroků můžete také využít [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="recovery-process-overview"></a>Přehled procesu obnovení
Proces řešení potíží je následující:

1. Odstraňte virtuální počítač, na zjištění problémy, zachovat virtuální pevné disky.
2. Připojte a připojit virtuální pevný disk na jiný virtuální počítač s Linuxem pro účely odstraňování potíží.
3. Připojení k virtuálnímu počítači pro řešení potíží. Úpravy souborů nebo spustit žádné nástroje na opravte problémy v původní virtuální pevný disk.
4. Odpojení virtuálního pevného disku od virtuálního počítače pro řešení potíží.
5. Vytvoření virtuálního počítače pomocí původní virtuální pevný disk.

Pokud chcete provést následující kroky pro řešení, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).

V následujících příkladech nahraďte názvy parametrů s vlastními hodnotami. Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.


## <a name="determine-boot-issues"></a>Určení spouštěcí problémy
Prohlédněte si výstup sériové určit, proč váš virtuální počítač není možné správně spustit. Běžným příkladem jsou neplatná položka v `/etc/fstab`, nebo základní virtuální pevný disk se odstranil nebo přesunul.

Získání protokolů spouštěcí s [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](/cli/azure/vm/boot-diagnostics#get-boot-log). Následující příklad načte sériové výstup z virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

Zkontrolujte sériové výstup chcete-li zjistit, proč se nedaří spustit virtuální počítač. Pokud sériové výstup neposkytuje jakoukoli indikaci toho, budete muset zkontrolujte soubory protokolu ve `/var/log` až budete mít virtuální pevný disk připojený k řešení potíží virtuální počítač.


## <a name="view-existing-virtual-hard-disk-details"></a>Zobrazení podrobností existující virtuální pevný disk
Předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk (VHD), musíte určit identifikátor URI disk operačního systému. 

Zobrazit informace o virtuální počítač s [az virtuálních počítačů zobrazit](/cli/azure/vm#show). Použití `--query` příznak k extrakci identifikátor URI pro disk operačního systému. Následující příklad získá informace o disku pro virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

Identifikátor URI je podobná **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.

## <a name="delete-existing-vm"></a>Odstraňte existující virtuální počítač
Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky. Virtuální pevný disk je, kde jsou uloženy samotného operačního systému, aplikace a konfigurace. Virtuální počítač je jenom metadata, která definuje velikosti či umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC). Každý virtuální pevný disk má zapůjčení přiřazen při připojen k virtuálnímu počítači. Přestože datové disky je možné připojovat a odpojovat dokonce i za běhu virtuálního počítače, disk s operačním systémem není možné odpojit, dokud se neodstraní prostředek virtuálního počítače. Zapůjčení i nadále i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated přidružení disk operačního systému virtuálního počítače.

Prvním krokem k obnovení virtuálního počítače je odstranit samotné prostředků virtuálního počítače. Když odstraníte virtuální počítač, virtuální pevné disky zůstanou ve vašem účtu úložiště. Po odstranění virtuálního počítače připojit virtuální pevný disk k jiným virtuálním Počítačem vyřešit chyby.

Odstraňte virtuální počítač s [odstranit virtuální počítač az](/cli/azure/vm#delete). Následující příklad odstraní virtuální počítač s názvem `myVM` ze skupiny prostředků s názvem `myResourceGroup`:

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

Počkejte, dokud je virtuální počítač dokončí odstraňování před připojit virtuální pevný disk k jiným virtuálním Počítačem. Zapůjčení na virtuální pevný disk, který přidruží k němu virtuální počítač je nutné uvolnit předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Připojit existující virtuální pevný disk k jiným virtuálním Počítačem
Pro několika dalších krocích použijete jiný počítač pro účely odstraňování potíží. Existující virtuální pevný disk se připojit k řešení potíží VM na Procházet a upravovat obsah na disk. Tento proces umožňuje opravte všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů protokolu, např. Vyberte nebo vytvořte jiným virtuálním Počítačem používat pro účely odstraňování potíží.

Připojit existující virtuální pevný disk s [nespravované virtuální počítač az-disk připojit](/cli/azure/vm/unmanaged-disk#attach). Když připojíte existující virtuální pevný disk, zadejte identifikátor URI na disk získaných v předchozím `az vm show` příkaz. Následující příklad připojí k řešení potíží virtuální počítač s názvem existujícího virtuálního pevného disku `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a>Připojit disk připojená data

> [!NOTE]
> Následující příklady jsou upřesněny kroky na virtuálního počítače s Ubuntu. Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, umístění souborů protokolu a `mount` příkazy se můžou mírně lišit. Naleznete v dokumentaci k vaší konkrétní distro pro příslušné změny v příkazech.

1. SSH k řešení potíží virtuální počítač pomocí příslušných přihlašovacích údajů. Pokud tento disk je první datový disk připojen k řešení potíží virtuální počítač, disk je pravděpodobně připojen k `/dev/sdc`. Použití `dmseg` zobrazíte připojené disky:

    ```bash
    dmesg | grep SCSI
    ```

    Výstup se podobá následujícímu příkladu:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    V předchozím příkladu je disk operačního systému na `/dev/sda` a dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`. Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.

2. Vytvořte adresář připojit existující virtuální pevný disk. Následující příklad vytvoří adresář s názvem `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Pokud máte více oddílů na existující virtuální pevný disk, připojte požadovaný oddíl. Následující příklad připojí na první primární oddíl `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Osvědčeným postupem je připojit datové disky na virtuální počítače v Azure pomocí identifikátor (UUID) virtuálního pevného disku. Tento krátký odstraňování potíží připojení virtuálního pevného disku pomocí identifikátoru UUID není nezbytné. Ale při normálním používání úpravy `/etc/fstab` připojit virtuální pevné disky použití název zařízení, nikoli UUID může způsobit selhání spuštění virtuálního počítače.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Vyřešte problémy na původní virtuální pevný disk
S existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby. Jakmile vyřešíte problémy, pokračujte následujícími kroky.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Odpojte Image a odpojit původní virtuální pevný disk
Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit existující virtuální pevný disk z virtuálního počítače řešení potíží. Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání zapůjčení virtuální pevný disk se připojuje k řešení potíží virtuální počítač.

1. Z relace SSH k řešení potíží virtuální počítač odpojte existující virtuální pevný disk. Nejprve změňte mimo nadřazený adresář pro přípojného bodu:

    ```bash
    cd /
    ```

    Nyní odpojte existující virtuální pevný disk. Následující příklad odpojí zařízení na `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nyní Odpojte virtuální pevný disk z virtuálního počítače. Ukončení relace SSH k řešení potíží virtuálního počítače. Seznam připojených datových disků k řešení potíží virtuální počítač s [seznamu nespravované disku virtuálního počítače az](/cli/azure/vm/unmanaged-disk#list). Následující příklad vypíše datových disků připojených k virtuálnímu počítači s názvem `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    Poznamenejte si název pro existující virtuální pevný disk. Například název disku s identifikátor URI **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** je **myVHD**. 

    Odpojit datový disk od virtuálního počítače [nespravované virtuální počítač az-disk odpojit](/cli/azure/vm/unmanaged-disk#detach). Následující příklad umožňuje odpojit disk s názvem `myVHD` z virtuálního počítače s názvem `myVMRecovery` v `myResourceGroup` skupiny prostředků:

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a>Vytvoření virtuálního počítače z původního pevného disku
Chcete-li vytvořit virtuální počítač z původní virtuální pevný disk, použijte [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). Skutečné šablona JSON je na následující odkaz:

- https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON

Šablona nasadí virtuálního počítače pomocí identifikátoru URI virtuálního pevného disku z dřívějších příkazu. Nasazení šablony s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create). Zadejte identifikátor URI na vaše původní virtuální pevný disk a pak zadejte typ operačního systému, velikost virtuálního počítače a název virtuálního počítače takto:

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a>Opětovné povolení Diagnostika spouštění
Při vytváření virtuálního počítače z existujícího virtuálního pevného disku, nemusí být Diagnostika spouštění automaticky povolené. Povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](/cli/azure/vm/boot-diagnostics#enable). Následující příklad povolí diagnostiky rozšíření ve virtuálním počítači s názvem `myDeployedVM` ve skupině prostředků s názvem `myResourceGroup`:

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením k virtuálnímu počítači, přečtěte si téma [řešení SSH připojení k virtuální počítač Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).