---
title: "názvy zařízení aaaLinux virtuálního počítače se změní v Azure | Microsoft Docs"
description: "Vysvětluje hello proč názvy zařízení se změní a nabízí řešení tohoto problému."
services: virtual-machines-linux
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: 
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 4d3a5853d61edd2c8e8b85ab69e5ed3b3bc00bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a>Řešení potíží: Názvy zařízení virtuálního počítače s Linuxem se změnilo

Hello článek vysvětluje, proč se po restartování systému Linux virtuálního počítače (VM), nebo znovu připojte hello disky změnit názvy zařízení. Poskytuje také hello řešení tohoto problému.

## <a name="symptom"></a>Příznaky

Může dojít k následujícím problémům, když běží virtuální počítače s Linuxem v Microsoft Azure hello.

- Hello virtuálního počítače selže tooboot po restartu.

- Pokud datových disků jsou odpojit a znovu připojit, se mění hello názvy zařízení pro disky.

- Aplikace nebo skriptu, který odkazuje na disk pomocí názvu zařízení se nezdaří. Zjistíte, že hello se změnil název zařízení hello disku.

## <a name="cause"></a>Příčina

Zařízení cesty v systému Linux se nezaručuje, že toobe konzistentní napříč restartování. Název zařízení se skládá z hlavní (písmeno) a menší čísla.  Pokud ovladač zařízení úložiště Linux hello zjistí nové zařízení, přiřadí tooit čísla hlavní a podverze zařízení z rozsahu hello k dispozici. Při odebrání zařízení jsou čísla zařízení hello uvolněné toobe znovu později.

Hello k problému dochází kvůli hello zařízení kontrolu v systému Linux naplánované subsystémem SCSI hello probíhá asynchronně. pojmenování cesta Hello konečné zařízení se může lišit mezi restarty. 

## <a name="solution"></a>Řešení

tooresolve tento problém používat trvalé názvy. Existují čtyři metody toopersistent pojmenování - prostřednictvím systému souborů štítek, uuid, id a cestu. Doporučujeme hello filesystem popisek a UUID metody pro virtuální počítače Linux Azure. 

Většina distribuce taky zadat buď hello **nofail** nebo **nobootwait** fstab možnosti. Tyto možnosti Povolit tooboot systému i v případě selhání disku hello toomount při spuštění. Další informace o těchto parametrů jsou uvedeny v dokumentaci distribuční hello. Další informace o tom, jak tooconfigure virtuálního počítače s Linuxem toouse UUID, když přidáte datový disk, najdete v části [připojit toohello virtuálního počítače s Linuxem toomount hello nový disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

Když hello Azure Linux agent nainstalovaný na virtuálním počítači, používá Udev pravidla tooconstruct sadu symbolické odkazy v části **/dev/disk/azure**. Tato pravidla Udev lze použít aplikace a skripty tooidentify disky připojené toohello virtuálních počítačů, jejich typu a hello logické jednotky.

## <a name="more-information"></a>Další informace

### <a name="identify-disk-luns"></a>Identifikovat logické diskové jednotky

Aplikace můžete použít toofind logických jednotek, všechny hello připojené disky a vytváření symbolické odkazy. Hello Azure Linux agent teď obsahuje udev pravidla, která nastavit symbolické odkazy z zařízeních toohello logickou jednotku LUN, následujícím způsobem:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 

Informace o logických jednotkách můžete také načíst z hosta hello Linux pomocí lsscsi nebo podobné nástroj následujícím způsobem.

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Tato informace o logických jednotkách hosta lze použít s předplatného Azure metadata tooidentify hello umístění v úložišti Azure hello virtuálního pevného disku, která ukládá data oddílu hello. Například použijte hello az rozhraní příkazového řádku:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                             
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Zjišťovat identifikátory UUID systému souborů pomocí blkid

Soubor skriptu nebo aplikace může číst výstup hello blkid nebo podobné zdroje informací a vytvořit symbolické odkazy v **/dev** pro použití. výstup Hello zobrazí hello identifikátory UUID všech disků připojených toohello virtuálních počítačů a hello zařízení souboru toowhich jsou přidružené:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Hello příkaz waagent udev pravidla vytvořit sadu symbolické odkazy v části **/dev/disk/azure**:


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


Tyto informace můžete použít aplikace Hello identifikovat hello spouštěcí disk zařízení a prostředků (dočasné) disku hello. V Azure, aplikace by měla odkazovat příliš**/dev/disk/azure/root-part1** nebo **/dev/disk/azure-resource-part1** toodiscover tyto oddíly.

Pokud existují další oddíly hello blkid seznamu, jsou umístěny na datový disk. Aplikace můžete udržovat hello UUID pro tyto oddíly a použijte cestu jako hello níže název zařízení hello toodiscover za běhu:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-hello-latest-azure-storage-rules"></a>Získat nejnovější pravidla úložiště Azure hello

toohello nejnovější pravidla úložiště Azure, spuštěním následujících příkazů:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


Další informace najdete v tématu hello následující články:

- [Ubuntu: Pomocí UUID](https://help.ubuntu.com/community/UsingUUID)

- [Red Hat: Trvalé pojmenování](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [Linux: Co identifikátory UUID využití.](https://www.linux.com/news/what-uuids-can-do-you)

- [Proces Udev: Úvod tooDevice správy v moderní systému Linux](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

