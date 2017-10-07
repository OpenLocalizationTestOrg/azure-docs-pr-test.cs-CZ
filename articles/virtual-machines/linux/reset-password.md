---
title: "aaaHow tooreset místní Linux heslo na virtuálních počítačích Azure | Microsoft Docs"
description: "Zavést hello kroky tooreset hello místní Linux heslo pro virtuální počítač Azure"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: delhan
ms.openlocfilehash: b28a679a36bf93c6881633eefa03aef3cd33e804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-linux-password-on-azure-vms"></a>Jak tooreset místní heslo Linux na virtuálních počítačích Azure

Tento článek představuje několik metod tooreset místní systém Linux virtuálního počítače (VM) hesla. Pokud vypršela platnost hello uživatelský účet nebo jenom chcete toocreate nový účet, můžete použít následující metody toocreate nový účet místního správce hello a znovu získat přístup k toohello virtuálních počítačů.

## <a name="symptoms"></a>Příznaky

Se nemůže přihlásit toohello virtuálních počítačů a obdržíte zprávu, která označuje, že toto hello heslo, které jste použili není správné. Kromě toho nelze použít VMAgent tooreset heslo na hello portálu Azure. 

## <a name="manual-password-reset-procedure"></a>Ruční postup vytvoření nového hesla

1.  Odstraňte hello virtuálních počítačů a zachovat hello připojené disky.

2.  Připojte hello jednotce operačního systému jako tooanother disku data dočasný virtuální počítač v hello stejné umístění.

3.  Spusťte následující příkazu SSH na hello dočasné virtuálních počítačů toobecome hello nadtypem uživatele.


    ~~~~
    sudo su
    ~~~~

4.  Spustit **fdisk -l** nebo pohled na systém protokoly toofind hello nově připojený disk. Vyhledejte toomount název jednotky hello. Potom na hello dočasný virtuální počítač, naleznete v příslušných hello souboru protokolu.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    Hello následuje příklad výstupu příkazu grep hello:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Vytvořit bod připojení s názvem **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Připojte disk hello operačního systému na hello přípojného bodu. Obvykle potřebujete toomount sdc1 nebo sdc2. To bude záviset na hello hostování oddílu v adresáři/etc z disku porušený počítače hello.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Proveďte zálohu před provedením jakýchkoli změn:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Resetování hesla hello uživatele, který budete potřebovat:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Přesunutí hello upravit soubory toohello správném umístění na hello poškozený disk počítače.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back toohello root and unmount hello disk.

    ~~~~
    CD / umount /tempmount
    ~~~~

11. Detach hello disk from hello management portal.

12. Recreate hello VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk tooanother Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How toodelete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
