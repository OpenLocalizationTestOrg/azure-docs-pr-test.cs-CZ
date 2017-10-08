---
title: "aaaManage Azure disky s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – Správa Azure disků s hello rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ad29cfbec5f9738f47705b19d0450c9a2f555492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-hello-azure-cli"></a>Správa Azure disků s hello rozhraní příkazového řádku Azure

Virtuální počítače Azure pomocí disků toostore hello virtuální počítače operačního systému, aplikace a data. Při vytváření virtuálního počítače je důležité toochoose velikost disku a příslušné toohello očekává úlohy konfigurace. Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů. Informace o:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků
> * Změna velikosti disků
> * Snímky disku


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Výchozí disky systému Azure

Když je vytvořen virtuální počítač Azure, jsou dva disky automaticky připojené toohello virtuálního počítače. 

**Disk s operačním systémem** – disků operačního systému může mít velikost až too1 terabajt a hello hostitelů virtuálních počítačů operačního systému. Hello OS disk označený */dev/sda* ve výchozím nastavení. ukládání do mezipaměti konfigurace disku operačního systému hello disku Hello je optimalizovaná pro výkon operačního systému. Z důvodu této konfiguraci hello disk s operačním systémem **neměli** hostitelem aplikace nebo data. Pro aplikace a data použijte datových disků, které jsou podrobně popsané dál v tomto článku. 

**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na hello stejného Azure hostitele jako hello virtuálních počítačů. Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování. Pokud hello virtuální počítač přesunutý tooa nového hostitele, je odebrat všechna data uložená na dočasném disku. Hello velikost dočasné disku hello je určen podle hello velikost virtuálního počítače. Dočasné disky jsou označeny */dev/sdb* a mít přípojný bod systému */mnt*.

### <a name="temporary-disk-sizes"></a>Velikosti dočasné disků

| Typ | Velikost virtuálního počítače | Maximální velikost dočasného disk (GB) |
|----|----|----|
| [Obecné účely](sizes-general.md) | A a řady D | 800 |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md) | Řady F | 800 |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md) | Série D a G | 6144 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md) | Řada L | 5630 |
| [GPU](sizes-gpu.md) | N řady | 1440 |
| [Vysoký výkon](sizes-hpc.md) | A a řady H | 2000 |

## <a name="azure-data-disks"></a>Azure datových disků

Pro instalaci aplikací a ukládání dat přidáním dalších datových disků. Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště. Každý datový disk má maximální kapacita 1 terabajt. velikost Hello hello virtuálního počítače určuje, kolik datových disků může být připojené tooa virtuálních počítačů. Pro každý základní virtuální počítač se dá připojit dvěma datovými disky. 

### <a name="max-data-disks-per-vm"></a>Maximální počet datových disků na virtuální počítač

| Typ | Velikost virtuálního počítače | Maximální počet datových disků na virtuální počítač |
|----|----|----|
| [Obecné účely](sizes-general.md) | A a řady D | 32 |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md) | Řady F | 32 |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md) | Série D a G | 64 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md) | Řada L | 64 |
| [GPU](sizes-gpu.md) | N řady | 48 |
| [Vysoký výkon](sizes-hpc.md) | A a řady H | 32 |

## <a name="vm-disk-types"></a>Typy disků virtuálních počítačů

Azure nabízí dva typy disku.

### <a name="standard-disk"></a>Disků na úrovni Standard

Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu. Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.

### <a name="premium-disk"></a>Premium disku

Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku. Ideální pro virtuální počítače se systémem produkční zatížení. Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series. Prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost hello hello disku určuje typ disku hello. Když vyberete, hodnota hello velikosti disku zaokrouhlit toohello další typ. Například pokud hello velikost disku je menší než 128 GB, typ disku hello je P10. Pokud je velikost disku hello 129 GB až 512 GB, velikost hello je P20. Nic více než 512 GB, velikost hello je P30.

### <a name="premium-disk-performance"></a>Výkon disku Premium

|Typ disku úložiště Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Velikost disku (round nahoru) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Maximum vstupně-výstupních operací za sekundu (IOPS) na disk | 500 | 2,300 | 5,000 |
Propustnost / disk | 100 MB/s | 150 MB/s | 200 MB/s |

Při hello výše tabulky identifikuje maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků. Pro instanci virtuálního počítače Standard_GS5 můžete dosáhnout maximálně 80 000 IOPS. Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](sizes.md).

## <a name="create-and-attach-disks"></a>Vytvořte a připojte disky

Datové disky můžete vytvořit a připojit v okamžiku vytvoření virtuálního počítače nebo tooan existující virtuální počítač.

### <a name="attach-disk-at-vm-creation"></a>Připojit disk při vytváření virtuálního počítače

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz. 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Vytvoření virtuálního počítače pomocí hello [vytvořit virtuální počítač az]( /cli/azure/vm#create) příkaz. Hello `--datadisk-sizes-gb` argument je použité toospecify, že další disk by měl vytvořit a připojit toohello virtuálního počítače. toocreate a připojit více než jeden disk, použijte mezerami oddělený seznam hodnot velikosti disku. V následujícím příkladu hello je virtuální počítač vytvořený s dvěma datovými disky, oba 128 GB. Protože hello velikosti disků jsou 128 GB, jsou obě tyto disky nakonfigurované jako P10s, které poskytují maximální 500 IOPS na disku.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-tooexisting-vm"></a>Připojte tooexisting disku virtuálního počítače

toocreate a připojit nový disk tooan existující virtuální počítač, použijte hello [připojit disk virtuálního počítače az](/cli/azure/vm/disk#attach) příkaz. Hello následující příklad vytvoří disk premium, 128 GB, velikost a připojí jej toohello, kterou virtuální počítač vytvořen v posledním kroku hello.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Příprava datových disků

Jakmile disk virtuálního počítače připojené toohello, už hello operačního systému musí toobe nakonfigurované toouse hello disku. Hello následující příklad ukazuje, jak toomanually konfigurace disku. Tento proces je také možné automatizovat pomocí inicializací cloudu, kterému se věnujeme v [novější kurzu](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Ruční konfigurace

Vytvoření připojení SSH s hello virtuálního počítače. Nahraďte hello příklad IP adresu s veřejnou IP adresu hello hello virtuálního počítače.

```azurecli-interactive 
ssh 52.174.34.95
```

Rozdělit disk na hello s `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Zapsat systému souborů toohello oddílu pomocí hello `mkfs` příkaz.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Připojte nový disk hello tak, aby se v hello operačního systému.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Hello disku se dá dostat teď hello *datadrive* přípojný bod, který lze ověřit spuštěním hello `df -h` příkaz. 

```bash
df -h
```

výstup Hello zobrazuje hello připojit na nový disk */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

tooensure, který hello jednotky je znovu připojeny po restartování systému, musí být přidané toohello */etc/fstab* souboru. toodo tedy získat hello UUID disku hello s hello `blkid` nástroj.

```bash
sudo -i blkid
```

výstup Hello zobrazí hello UUID disku hello `/dev/sdc1` v tomto případě.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Přidání řádku podobné toohello následující toohello */etc/fstab* souboru. Také Upozorňujeme, že zápis překážek můžete zakázat pomocí *bariéry = 0*, tato konfigurace může zlepšit výkon disku. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Teď, když hello disku byl nakonfigurován, zavřete relace SSH hello.

```bash
exit
```

## <a name="resize-vm-disk"></a>Změna velikosti disku virtuálního počítače

Po nasazený virtuální počítač, disk operačního systému hello nebo jakýchkoli připojených datových disků je možné zvýšit velikost. Zvýšení hello velikost disku je v případě nutnosti další úložný prostor nebo vyšší úroveň výkonu (P10, P20, P30). Všimněte si, že se disky není možné snížit velikost.

Před zvýšením velikosti disku, hello Id nebo název hello disku je potřeba. Použití hello [seznam disků az](/cli/azure/vm/disk#list) příkaz tooreturn všechny disky ve skupině prostředků. Poznamenejte si název hello disku, které chcete tooresize.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

také musí být deallocated Hello virtuálních počítačů. Použití hello [az OM deallocate]( /cli/azure/vm#deallocate) příkaz toostop a navrátit hello virtuálních počítačů.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Použití hello [aktualizace disku az](/cli/azure/vm/disk#update) příkaz tooresize hello disku. Tento příklad změní velikost disku s názvem *myDataDisk* too1 terabajt.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Po dokončení operace změny velikosti hello spusťte hello virtuálních počítačů.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Pokud jste ke změně velikosti hello operačního systému disku, je automaticky rozšířena hello oddílu. Pokud jste změnili datový disk, musí všechny aktuální oddíly toobe rozšířit hello virtuální počítače operačního systému.

## <a name="snapshot-azure-disks"></a>Snímek disky systému Azure

Pořízení snímku disku vytvoří čtení pouze, v okamžiku kopie disku hello. Azure snímky virtuálních počítačů jsou užitečné pro rychle ukládají hello stav virtuálního počítače, před prováděním změn konfigurace. V případě hello hello změny konfigurace prokázat toobe nežádoucí, stav virtuálního počítače může být obnovovány pomocí snímků hello. Virtuální počítač má více než jeden disk, je snímek prováděné každého disku nezávisle na hello ostatní. Za vyjádření zálohování konzistentní s aplikací, vezměte v úvahu zastavení hello virtuálních počítačů před přepnutím snímky disku. Můžete taky použít hello [služby Azure Backup](/azure/backup/), které umožňuje vám tooperform automatizované zálohování při hello je spuštěný virtuální počítač.

### <a name="create-snapshot"></a>Vytvořit snímek

Před vytvořením snímku disku virtuálního počítače, hello Id nebo názvem hello disku je potřeba. Použití hello [az virtuálních počítačů zobrazit](https://docs.microsoft.com/en-us/cli/azure/vm#show) id disku hello tooreturn příkaz. V tomto příkladu je id disku hello uložené v proměnné, aby se může použít později.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Teď, když máte hello id hello disku virtuálního počítače, hello následující příkaz vytvoří snímek hello disku.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Vytvoření disku ze snímku

Tento snímek pak může být převedena na disk, který lze použít toorecreate hello virtuálního počítače.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Virtuální počítač obnovit ze snímku

obnovení virtuálního počítače toodemonstrate, odstraňte hello existující virtuální počítač. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Vytvoření nového virtuálního počítače z disku hello snímku.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Připojte datový disk

Všechny disky dat potřebovat toobe znovu připojit toohello virtuálního počítače.

Nejprve najít název disku hello dat pomocí hello [seznam disků az](https://docs.microsoft.com/cli/azure/disk#list) příkaz. Tento příklad místech hello název disku hello do proměnné s názvem *datadisk*, která je použita v dalším kroku hello.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Použití hello [připojit disk virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/disk#attach) příkaz tooattach hello disku.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků
> * Změna velikosti disků
> * Snímky disku

Posunutí další kurz toolearn toohello o automatizaci konfigurace virtuálního počítače.

> [!div class="nextstepaction"]
> [Automatizace konfigurace virtuálních počítačů](./tutorial-automate-vm-deployment.md)
