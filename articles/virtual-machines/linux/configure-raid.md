---
title: "aaaConfigure softwaru diskového pole RAID na virtuálním počítači se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak toouse mdadm tooconfigure RAID v systému Linux v Azure."
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: f3cb2786-bda6-4d2c-9aaf-2db80f490feb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.openlocfilehash: f06e2679d953faf88ffee9991226cdb3cc1cbdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-software-raid-on-linux"></a>Konfigurace softwarového pole RAID v Linuxu
Jedná se o běžné software toouse scénář RAID na virtuální počítače s Linuxem v Azure toopresent dat více připojené disky jako jedno zařízení RAID. Obvykle to být použité tooimprove výkonu a povolit pro zlepšila propustnost porovnání toousing právě jeden disk.

## <a name="attaching-data-disks"></a>Připojení datových disků
Minimálně dva prázdné datové disky jsou potřebné tooconfigure zařízení RAID.  Hello primární důvod pro vytváření diskového pole RAID zařízení je tooimprove výkon disku vstupně-výstupní operace.  V závislosti na vašich potřebách vstupně-výstupní operace, můžete vybrat tooattach disky, které jsou uložené v našich standardní úložiště s až too500 vstupně-výstupní operace/ps na disk nebo naše storage úrovně Premium s až too5000 vstupně-výstupní operace/ps na disk. Tento článek nezabývá podrobnosti o tom, tooprovision a připojte datové disky tooa Linux virtuálního počítače.  Najdete v článku Microsoft Azure hello [připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro podrobné pokyny, jak tooattach prázdný datový disk tooa Linux virtuálního počítače na platformě Azure.

## <a name="install-hello-mdadm-utility"></a>Nainstalujte nástroj mdadm hello
* **Ubuntu**
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* **CentOS & Oracle Linux**
```bash
sudo yum install mdadm
```

* **SLES a openSUSE**
```bash  
zypper install mdadm
```

## <a name="create-hello-disk-partitions"></a>Vytvoření oddílů disku hello
V tomto příkladu vytvoříme na /dev/sdc oddíl jediný disk. Hello nový diskový oddíl bude volána /dev/sdc1.

1. Spustit `fdisk` toobegin vytváření oddílů

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide toowrite them.
    After that, of course, hello previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off hello mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. Stiskněte klávesu n na výzvy toocreate hello  **n** ové oddílu:

    ```bash
    Command (m for help): n
    ```

3. Stiskněte klávesu toocreate "p" **p**rimární oddílu:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. Stiskněte číslo oddílu '1' tooselect 1:

    ```bash
    Partition number (1-4): 1
    ```

5. Vyberte hello výchozí bod hello nový oddíl, nebo klikněte na tlačítko `<enter>` tooaccept hello výchozí tooplace hello oddílu na začátku hello hello volné místo na jednotce hello:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. Vyberte velikost hello hello oddílu, například typ '+10G' toocreate oddíl 10 GB. Také můžete stisknout klávesu `<enter>` vytvořte jeden oddíl, který zahrnuje celou jednotku hello:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. V dalším kroku změnit hello ID a **t**typ oddílu hello z výchozí hello ID '83' tooID (Linux), fd' (Linux raid automaticky):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L toolist codes): fd
    ```

8. Nakonec zápisu hello oddílu tabulky toohello jednotky a ukončete fdisk:

    ```bash   
    Command (m for help): w
    hello partition table has been altered!
    ```

## <a name="create-hello-raid-array"></a>Vytvořit hello typu
1. Následující příklad se "stripe" (úroveň pole RAID 0) tři oddíly nachází na tři samostatné datové disky (sdc1, sdd1, sde1) Hello.  Po spuštění tohoto příkazu nové zařízení RAID názvem **/dev/md127** je vytvořena. Všimněte si, že pokud tyto datové disky jsme dříve součástí jiného nefunkční pole RAID může být nutné tooadd hello `--force` parametr toohello `mdadm` příkaz:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. Vytvořit systém souborů hello na nové zařízení RAID hello
   
    a. **CentOS, Oracle Linux, SLES 12, openSUSE a Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    b. **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    c. **SLES 11** – povolit boot.md a vytvořit mdadm.conf

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > Po provedení těchto změn na systémy SUSE může být nutný restart. Tento krok je *není* na SLES 12 vyžaduje.
   > 
   > 

## <a name="add-hello-new-file-system-tooetcfstab"></a>Přidat hello nový soubor systému příliš/atd/fstab
> [!IMPORTANT]
> Nesprávně úprav hello /etc/fstab souboru může mít za následek nelze spustit systém. Pokud si jisti, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor. Dále je doporučeno, jestli je vytvořená záloha souboru /etc/fstab hello před úpravou.

1. Vytvořte hello potřeby přípojný bod pro nový systém souborů, například:

    ```bash
    sudo mkdir /data
    ```
2. Při úpravě /etc/fstab, hello **UUID** by měla být použité tooreference hello systému, nikoli hello zařízení název souboru.  Použití hello `blkid` nástroj toodetermine hello UUID pro hello nový systém souborů:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. V textovém editoru otevřete /etc/fstab a přidejte položku pro hello nový systém souborů, například:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Nebo na **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Potom uložte a zavřete /etc/fstab.

4. Test této hello etc/správnost zadání fstab:

    ```bash  
    sudo mount -a
    ```

    Pokud tento příkaz výsledkem chybovou zprávu, Zkontrolujte prosím hello syntaxe v souboru /etc/fstab hello.
   
    Následně spusťte hello `mount` připojení k systému souborů hello tooensure příkaz:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. (Volitelné) Bezporuchový spouštěcí parametry
   
    **Konfigurace fstab**
   
    Velkém množství distribucí obsahovat buď hello `nobootwait` nebo `nofail` připojit parametry, které mohou být přidány toohello/etc/fstab souboru. Tyto parametry umožňují selhání při připojení příslušného systému souborů a povolit tooboot toocontinue systému Linux hello, i když je systém souborů nelze tooproperly připojení hello RAID. Další informace o těchto parametrů naleznete v dokumentaci tooyour distribuce.
   
    Příklad (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Parametry spuštění systému Linux**
   
    V přidání toohello výše parametry, hello jádra parametr "`bootdegraded=true`" Povolit hello systému tooboot i v případě hello RAID je považována za poškozený nebo ke snížení, například pokud datová jednotka je neúmyslně odebere z hello virtuálního počítače. Ve výchozím nastavení může také výsledkem-spouštěcího systému.
   
    Naleznete v dokumentaci tooyour distribuční na tom, jak tooproperly upravit parametry jádra. Například ve velkém množství distribucí (CentOS, Oracle Linux SLES 11.) tyto parametry mohou být přidány ručně toohello "`/boot/grub/menu.lst`" soubor.  Na Ubuntu tento parametr je možné přidat toohello `GRUB_CMDLINE_LINUX_DEFAULT` proměnné na "/ etc/výchozí/grub".


## <a name="trimunmap-support"></a>Podpora uvolnění dočasné paměti nebo UNMAP
Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello. Tyto operace jsou užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány. Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.

> [!NOTE]
> RAID nemusí zadávání příkazů zahození, velikost bloku hello hello pole je nastaven tooless než výchozí hello (512 KB). Důvodem je, že zrušit mapování hello členitosti na hello hostitele je také 512KB. Pokud jste změnili velikost bloku hello pole prostřednictvím na mdadm `--chunk=` parametr a potom zrušit mapování/TRIM požadavky může být ignorována hello jádra.

Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem. Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:

- Použití hello `discard` připojit možnost v `/etc/fstab`, například:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- V některých případech hello `discard` možnost může mít vliv na výkon. Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:

    **Ubuntu**

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    **RHEL nebo CentOS**
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
