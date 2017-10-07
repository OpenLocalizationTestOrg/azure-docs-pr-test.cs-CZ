---
title: "aaaAttach tooa disku virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak tooattach datový disk tooa virtuálního počítače s Linuxem pomocí nasazení Classic hello modelu a inicializovat hello disk tak, aby byl připravený k použití"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: c76d8479ac2b522d2b6df658cd28f242473f30ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-virtual-machine"></a>Jak tooAttach datový Disk tooa Linux virtuálního počítače
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. V tématu Jak příliš[připojit datový disk pomocí modelu nasazení Resource Manager hello](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Můžete připojit prázdné disky a disky, které obsahují data tooyour virtuálních počítačích Azure. Oba typy disků jsou soubory VHD, které jsou umístěny v účtu úložiště Azure. Přidávání jakéhokoli počítače Linux tooa disku po připojit hello disk můžete potřebovat tooinitialize a naformátujte ho tak, aby byl připravený k použití. Tento článek údaje prázdný disků i disků již obsahující data tooyour i virtuálních počítačů, jak toothen inicializace a formátování nový disk se připojuje.

> [!NOTE]
> Je nejlepší postup toouse, jeden nebo více disků toostore oddělení dat virtuálního počítače. Když vytvoříte virtuální počítač Azure, je disk operačního systému a dočasný disk. **Nepoužívejte hello dočasným diskovým toostore trvalá data.** Jak hello název napovídá, obsahuje pouze dočasné úložiště. Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.
> dočasným diskovým Hello je obvykle spravuje hello Azure Linux Agent a automaticky připojit příliš**/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image). Na hello druhé straně, datový disk může být pojmenován podle hello Linux jádra něco podobného jako `/dev/sdc`, a je třeba toopartition, formátování a připojte tento prostředek. V tématu hello [Azure Linux Agent uživatelská příručka] [ Agent] podrobnosti.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Inicializace nový datový disk v systému Linux
1. SSH tooyour virtuálních počítačů. Další informace najdete v tématu [jak toolog na tooa virtuální počítač se systémem Linux][Logon].
2. Dále pro hello datového disku tooinitialize potřebovat identifikátor zařízení toofind hello. Existují dva způsoby toodo který:
   
    a) Grep pro zařízení SCSI v hello protokoly, například hello následující příkaz:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Pro poslední Ubuntu distribuce, pravděpodobně bude třeba toouse `sudo grep SCSI /var/log/syslog` protože protokolování příliš`/var/log/messages` může ve výchozím nastavení zakázané.
   
    Můžete najít identifikátor hello hello poslední datový disk, který se přidal ve hello zprávy, které jsou zobrazeny.
   
    ![Získávání zpráv disku hello](./media/attach-disk/scsidisklog.png)
   
    NEBO
   
    b) hello použijte `lsscsi` příkaz toofind se id zařízení hello. `lsscsi` může být instalován buď `yum install lsscsi` (na Red Hat na základě distribuce) nebo `apt-get install lsscsi` (na Debian na základě distribuce). Můžete najít disk hello hledáte podle jeho *lun* nebo **číslo logické jednotky**. Například hello *lun* pro připojené disky hello je snadno vidět z `azure vm disk list <virtual-machine>` jako:

    ```azurecli
    azure vm disk list myVM
    ```

    výstup Hello je podobné toohello následující:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Porovnat tato data se výstup hello `lsscsi` pro hello stejná ukázková virtuálního počítače:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    Poslední číslo hello řazené kolekce členů v jednotlivých řádcích Hello je hello *lun*. V tématu `man lsscsi` Další informace.
3. Hello řádku zadejte následující příkaz toocreate hello zařízení:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. Po zobrazení výzvy zadejte  **n**  toocreate oddílu.

    ![Vytvoření zařízení](./media/attach-disk/fdisknewpartition.png)

5. Po zobrazení výzvy zadejte **p** toomake hello oddílu hello primární oddíl. Typ **1** toomake hello prvního oddílu a poté zadejte zadejte tooaccept hello výchozí hodnota pro cylindr hello. Na některé systémy může zobrazit výchozí hodnoty hello hello první a poslední sektory, místo hello cylindr hello. Tooaccept můžete tyto výchozí hodnoty.

    ![Vytvořit oddíl](./media/attach-disk/fdisknewpartdetails.png)


6. Typ **p** toosee hello podrobnosti o hello disk, který je rozdělena na oddíly.

    ![Informace o disku seznamu](./media/attach-disk/fdiskpartitiondetails.png)


7. Typ **w** toowrite hello nastavení pro hello disk.

    ![Zápis hello změny na disku](./media/attach-disk/fdiskwritedisk.png)

8. Nyní můžete vytvořit systém souborů hello na hello nový oddíl. Připojit ID číslo toohello zařízení hello oddílu (v hello následující ukázka `/dev/sdc1`). Hello následující příklad vytvoří ext4 oddíl na /dev/sdc1:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Vytvořit systém souborů](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > Systémy SuSE Linux Enterprise 11 pro systémy souborů ext4 podporují pouze oprávnění jen pro čtení. Pro tyto systémy se doporučuje tooformat hello nový systém souborů jako ext3, nikoli ext4.

9. Ujistěte se, adresář toomount hello nový systém souborů, následujícím způsobem:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Nakonec můžete připojit hello jednotku, následujícím způsobem:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    Hello datový disk je nyní připraven toouse jako **/datadrive**.
   
    ![Vytvoření disku hello hello directory a připojení](./media/attach-disk/mkdirandmount.png)

11. Přidejte hello nového disku příliš/atd/fstab:
   
    znovu tooensure hello jednotky je připojeny automaticky po restartování počítače, které musí být přidán toohello /etc/fstab souboru. Kromě toho důrazně doporučujeme tuto hello UUID (univerzálně jedinečný identifikátor) se používá v /etc/fstab toorefer toohello disku, nikoli jen název zařízení hello (tj. /dev/sdc1). Pomocí hello UUID zabraňuje hello nesprávnou disku se připojené tooa zadané umístění, pokud hello OS zjistí chybu disk během spuštění a všechny zbývající datových disků, pak se ty přiřazené ID zařízení. toofind hello UUID hello nový disk, můžete použít hello **blkid** nástroj:
   
    ```bash
    sudo -i blkid
    ```
   
    výstup Hello vypadá podobně jako toohello následující ukázka:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Nesprávně úpravy hello **/etc/fstab** soubor může mít za následek nelze spustit systém. Pokud si jisti, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor. Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab hello před úpravou.

    Dále otevřete hello **/etc/fstab** soubor v textovém editoru:

    ```bash
    sudo vi /etc/fstab
    ```

    V tomto příkladu používáme hello UUID hodnotu pro hello nové **/dev/sdc1** zařízení, který byl vytvořen v předchozích krocích hello a přípojný bod hello **/datadrive**. Přidejte následující řádek toohello konec hello hello **/etc/fstab** souboru:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Nebo na systémy založené na systému SuSE Linux může být nutné toouse mírně odlišný formát:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > Hello `nofail` možnost zajistí, že hello virtuální počítač spustí i v případě systému souborů hello je poškozený nebo hello disk neexistuje při spuštění. Bez této možnosti se můžete setkat chování jak je popsáno v [nelze SSH tooLinux virtuálních počítačů z důvodu chyby tooFSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Nyní můžete otestovat, zda text hello připojení k systému souborů správně odpojování a pak opakovanému připojení hello systému souborů, tj. pomocí hello příklad přípojného bodu `/datadrive` vytvořené v hello dříve kroky:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Pokud hello `mount` příkaz vytvořil chybu, zkontrolovat, zda text hello/etc/fstab soubor správnou syntaxi. Pokud budou vytvořeny další datové jednotky nebo oddíly, zadejte je do/etc/fstab také samostatně.

    Vytvořte jednotku hello zapisovatelné pomocí tohoto příkazu:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Následně odebrat datový disk bez úprav fstab by mohlo způsobit tooboot toofail hello virtuálních počítačů. Pokud je to běžné v situaci, většina distribuce zadejte buď hello `nofail` nebo `nobootwait` fstab možnosti, které umožňují tooboot systému i v případě selhání disku hello toomount při spuštění. Další informace o těchto parametrů naleznete v dokumentaci vaší distribuce.

### <a name="trimunmap-support-for-linux-in-azure"></a>Podpora uvolnění dočasné paměti nebo UNMAP pro Linux v Azure
Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello. Tyto operace jsou užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány. Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.

Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem. Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:

* Použití hello `discard` připojit možnost v `/etc/fstab`, například:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* V některých případech hello `discard` možnost může mít vliv na výkon. Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL nebo CentOS**
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Další kroky
Další informace o používání virtuálním počítačům s Linuxem v hello následující články:

* [Jak toolog na tooa virtuální počítač se systémem Linux][Logon]
* [Jak toodetach disk z virtuálního počítače systému Linux](detach-disk.md)
* [Pomocí modelu nasazení Classic hello hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Konfigurace RAID na virtuální počítač s Linuxem v Azure](../configure-raid.md)
* [Konfigurace LVM na virtuální počítač s Linuxem v Azure](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
