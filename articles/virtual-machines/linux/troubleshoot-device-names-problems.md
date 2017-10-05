---
title: "Názvy zařízení virtuálního počítače s Linuxem se změní v Azure | Microsoft Docs"
description: "Vysvětluje důvod, proč se změní názvy zařízení a poskytují řešení tohoto problému."
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
ms.openlocfilehash: 789f4580901a22dc3aaae9599c7205c76f268403
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a>Řešení potíží: Názvy zařízení virtuálního počítače s Linuxem se změnilo

Tento článek vysvětluje, proč se po restartování systému Linux virtuálního počítače (VM), nebo znovu připojte disky změnit názvy zařízení. Také poskytuje řešení tohoto problému.

## <a name="symptom"></a>Příznaky

Následující problémy se můžete setkat, když běží virtuální počítače s Linuxem v Microsoft Azure.

- Virtuální počítač se nepodaří spustit po restartování.

- Pokud jsou datové disky odpojit a znovu připojit, se mění názvy zařízení pro disky.

- Aplikace nebo skriptu, který odkazuje na disk pomocí názvu zařízení se nezdaří. Zjistíte, že se změnil název zařízení disku.

## <a name="cause"></a>Příčina

Zařízení cesty v systému Linux nemusí být konzistentní napříč restartování. Název zařízení se skládá z hlavní (písmeno) a menší čísla.  Pokud ovladač zařízení úložiště Linux zjistí nové zařízení, přiřadí čísla hlavní a podverze zařízení k němu z rozsahu k dispozici. Při odebrání zařízení se znovu později použije uvolněno čísla zařízení.

K problému dochází, protože zařízení kontrolu v systému Linux naplánované subsystémem SCSI probíhá asynchronně. Pojmenování cesta konečné zařízení se může lišit mezi restarty. 

## <a name="solution"></a>Řešení

Chcete-li vyřešit tento problém, použijte trvalé názvy. Existují čtyři metody do trvalé pojmenování – prostřednictvím filesystem štítek, uuid, podle id a cestu. Doporučujeme popisek systému souborů a UUID metody pro virtuální počítače Linux Azure. 

Většina distribuce taky zadat buď **nofail** nebo **nobootwait** fstab možnosti. Tyto možnosti Povolit systému spustit i v případě, že na disku se nepodaří připojit při spuštění. Další informace o těchto parametrů jsou uvedeny v dokumentaci distribuce. Další informace o tom, jak nakonfigurovat virtuální počítač s Linuxem používat UUID, když přidáte datový disk najdete v tématu [připojit k virtuálního počítače s Linuxem připojit nový disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

Pokud na virtuálním počítači je nainstalován agent nástroje Azure Linux, používá pravidla Udev vytvořit sadu symbolické odkazy v části **/dev/disk/azure**. Tato pravidla Udev lze použít aplikace a skripty k identifikaci disky jsou připojené k virtuálnímu počítači, jejich typu a logickou jednotku LUN.

## <a name="more-information"></a>Další informace

### <a name="identify-disk-luns"></a>Identifikovat logické diskové jednotky

Aplikace můžete použít logické jednotky LUN najít všechny připojené disky a vytváření symbolické odkazy. Azure Linux agent teď obsahuje udev pravidla, která nastavit symbolické odkazy z logické jednotky do zařízení, následujícím způsobem:

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
                                 

Informace o logických jednotkách můžete také načíst z hosta Linux pomocí lsscsi nebo podobné nástroj následujícím způsobem.

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Tato informace o logických jednotkách hosta lze s metadaty předplatné Azure pro určení umístění v úložišti Azure virtuálního pevného disku, která ukládá data oddílu. Například použijte rozhraní příkazového řádku az:

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

Soubor skriptu nebo aplikace může číst výstup blkid nebo podobné zdroje informací a vytvořit symbolické odkazy v **/dev** pro použití. Výstup bude zobrazovat identifikátory UUID všech disků připojených k virtuálnímu počítači a soubor zařízení, ke kterému jsou přidružené:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Příkaz waagent udev pravidla vytvořit sadu symbolické odkazy v části **/dev/disk/azure**:


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


Tyto informace můžete použít aplikaci identifikovat spouštěcí disk zařízení a prostředků (dočasné) disku. V Azure, by měla aplikace označují **/dev/disk/azure/root-part1** nebo **/dev/disk/azure-resource-part1** ke zjištění těchto oddílů.

Pokud existují další oddíly ze seznamu blkid, jsou umístěny na datový disk. Aplikace můžete udržovat identifikátor UUID pro tyto oddíly a použijte cestu jako zjištění názvu zařízení v době běhu níže:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a>Získat nejnovější pravidla úložiště Azure

Nejnovější pravidla úložiště Azure spuštěním následujících příkazů:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


Další informace najdete v následujících článcích:

- [Ubuntu: Pomocí UUID](https://help.ubuntu.com/community/UsingUUID)

- [Red Hat: Trvalé pojmenování](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [Linux: Co identifikátory UUID využití.](https://www.linux.com/news/what-uuids-can-do-you)

- [Proces Udev: Úvod do správy zařízení v nástroji moderní systému Linux](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

