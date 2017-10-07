---
title: "aaaConfigure LVM na virtuálním počítači se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak tooconfigure LVM v systému Linux v Azure."
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 8daf792d87c6bb3d91a2eddcd01cfab34fd28cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfigurace LVM na virtuální počítač s Linuxem v Azure
Tento dokument popisuje jak tooconfigure Manager logické svazek (LVM) ve virtuálním počítači Azure. I když je to vhodné tooconfigure LVM na každý disk připojen toohello virtuálního počítače, ve výchozím nastavení většina cloudové Image nebude mít LVM nakonfigurované na hello disk operačního systému. To je tooprevent problémy se skupinami duplicitní svazku, pokud hello disk s operačním systémem je někdy připojena tooanother virtuální počítač hello stejné distribuce a typ, tj. během na scénář zotavení. Proto se doporučuje jenom toouse LVM na hello datových disků.

## <a name="linear-vs-striped-logical-volumes"></a>Lineární oproti logické prokládané svazky
LVM lze použít toocombine počet fyzických disků do do jednoho svazku úložiště. Ve výchozím nastavení LVM obvykle vytvoří lineární logické svazcích, což znamená, že hello fyzického úložiště je zřetězen společně. V takovém případě operace čtení a zápisu obvykle pouze odešle tooa jediný disk. Naproti tomu můžeme také prokládané svazky lze vytvářet logické kde čtení a zápisu jsou distribuované toomultiple disky patřící do skupiny svazku hello (tj. podobné tooRAID0). Z důvodů výkonu, pravděpodobně budete chtít toostripe logické svazků tak, aby čtení a zápisy využívat všechny disky připojené data.

Tento dokument se popisují, jak toocombine několik datové disky do jednoho svazku skupiny a pak vytvořte Prokládané logické svazku. Hello kroky jsou poněkud zobecněný toowork s většina distribuce. Ve většině případů hello nástroje a pracovní postupy pro správu LVM v Azure nejsou zásadně liší od jiných prostředích. Obvyklým způsobem také najdete dodavatele Linux dokumentace a osvědčené postupy pro použití s konkrétní distribuční LVM.

## <a name="attaching-data-disks"></a>Připojení datových disků
Obvykle jednu bude chcete toostart dvě nebo více disků prázdný data při použití LVM. V závislosti na vašich potřebách vstupně-výstupní operace, můžete vybrat tooattach disky, které jsou uložené v našich standardní úložiště s až too500 vstupně-výstupní operace/ps na disk nebo naše storage úrovně Premium s až too5000 vstupně-výstupní operace/ps na disk. Tento článek nepřejde do podrobnosti o tom, tooprovision a připojte datové disky tooa Linux virtuálního počítače. Naleznete v článku Microsoft Azure hello [připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro podrobné pokyny, jak tooattach prázdný datový disk tooa Linux virtuálního počítače na platformě Azure.

## <a name="install-hello-lvm-utilities"></a>Nainstalujte nástroje LVM hello
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS a Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 a openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    Na SLES11 je nutné upravit také `/etc/sysconfig/lvm` a nastavte `LVM_ACTIVATED_ON_DISCOVERED` příliš "Povolit":

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>Konfigurace LVM
V tomto průvodci budeme předpokládat připojeny tři datových disků, které budete označujeme tooas `/dev/sdc`, `/dev/sdd` a `/dev/sde`. Poznámka: tyto nesmí být vždy možné hello stejné názvy cest v virtuálního počítače. Můžete spustit '`sudo fdisk -l`' nebo podobné toolist příkaz dostupné disky.

1. Příprava fyzických svazků hello:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Vytvořte skupinu svazku. V tomto příkladu voláme hello svazku skupiny `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Vytvoření logické svazky hello. Hello příkaz níže jsme dojde k vytvoření jedné logické svazek nazvaný `data-lv01` toospan hello celý svazek skupiny, ale Všimněte si, je také vhodná toocreate více logických svazky ve skupině svazku hello.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Formátování svazku logické hello

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > S použitím SLES11 `-t ext3` místo ext4. SLES11 podporuje pouze systémy tooext4 oprávnění jen pro čtení.

## <a name="add-hello-new-file-system-tooetcfstab"></a>Přidat hello nový soubor systému příliš/atd/fstab
> [!IMPORTANT]
> Nesprávně úpravy hello `/etc/fstab` soubor může mít za následek nelze spustit systém. Pokud jistí, naleznete v dokumentaci toohello distribuční informace o tom, jak upravit tooproperly tento soubor. Je také doporučeno zálohu hello `/etc/fstab` soubor je vytvořen před úpravou.

1. Vytvořte hello potřeby přípojný bod pro nový systém souborů, například:

    ```bash  
    sudo mkdir /data
    ```

2. Vyhledejte cestu logické svazku hello

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Otevřete `/etc/fstab` v textovém editoru a přidejte položku pro hello nový systém souborů, například:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Potom uložte a zavřete `/etc/fstab`.

4. Testování této hello `/etc/fstab` správnost zadání:

    ```bash    
    sudo mount -a
    ```

    Pokud tento příkaz má za následek chybové zprávy zkontrolujte syntaxi hello hello `/etc/fstab` souboru.
   
    Následně spusťte hello `mount` připojení k systému souborů hello tooensure příkaz:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (Volitelné) Spouštěcí parametry bezporuchový v`/etc/fstab`
   
    Velkém množství distribucí obsahovat buď hello `nobootwait` nebo `nofail` připojit parametry, které mohou být přidány toohello `/etc/fstab` souboru. Tyto parametry umožňují selhání při připojení příslušného systému souborů a povolit tooboot toocontinue systému Linux hello, i když je systém souborů nelze tooproperly připojení hello RAID. Další informace o těchto parametrů naleznete v dokumentaci tooyour distribuční.
   
    Příklad (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>Podpora uvolnění dočasné paměti nebo UNMAP
Některé jádra Linux podporují toodiscard operace TRIM/UNMAP nepoužívané bloky na disku hello. Tyto operace jsou užitečné hlavně v tooinform standardní úložiště Azure, které odstraněné stránky již nejsou platné a může být vymazány. Zahození stránky můžete uložit náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.

Existují dva způsoby tooenable TRIM podporují ve virtuálním počítačům s Linuxem. Obvyklým způsobem podívejte se distribuční hello doporučenému přístupu:

- Použití hello `discard` připojit možnost v `/etc/fstab`, například:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- V některých případech hello `discard` možnost může mít vliv na výkon. Alternativně můžete spustit hello `fstrim` ručně příkaz z příkazového řádku hello, nebo ho přidat tooyour crontab toorun pravidelně:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL nebo CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
