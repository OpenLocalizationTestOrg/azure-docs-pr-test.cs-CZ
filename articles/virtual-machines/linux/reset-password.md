---
title: "Jak obnovit heslo místního Linux na virtuálních počítačích Azure | Microsoft Docs"
description: "Zavést postup obnovit heslo místního Linux na virtuálním počítači Azure"
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
ms.openlocfilehash: bd48128a078821b7a4baa02d5d7ceecc6de99608
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Jak obnovit heslo místního Linux na virtuálních počítačích Azure

Tento článek představuje několik metod k resetování hesel místní systém Linux virtuálního počítače (VM). Pokud vypršela platnost uživatelského účtu nebo jenom chcete vytvořit nový účet, můžete použít následující metody vytvořit nový účet místního správce a znovu získat přístup k virtuálnímu počítači.

## <a name="symptoms"></a>Příznaky

Se nemůže přihlásit k virtuálnímu počítači a obdržíte zprávu, která označuje, že je nesprávné heslo, které jste použili. Kromě toho nelze použít VMAgent k resetování hesla na portálu Azure. 

## <a name="manual-password-reset-procedure"></a>Ruční postup vytvoření nového hesla

1.  Odstraňte virtuální počítač a zachovat připojených disků.

2.  Připojte jednotku operačního systému jako datový disk k jiné dočasné virtuálních počítačů ve stejném umístění.

3.  Pomocí následujícího příkazu SSH na dočasný virtuální počítač stane nadtypem uživatele.


    ~~~~
    sudo su
    ~~~~

4.  Spustit **fdisk -l** nebo v protokolech systému najít nově připojený disk. Vyhledejte název jednotky připojit. Pak na dočasný virtuální počítač, zkontrolujte soubor protokolu relevantní.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    Následuje příklad výstupu příkazu grep:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Vytvořit bod připojení s názvem **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Připojte disk operačního systému na přípojného bodu. Obvykle potřebujete připojit sdc1 nebo sdc2. To bude záviset na hostování oddílu v adresáři/etc z disku porušený počítače.

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

8.  Resetovat heslo uživatele, které budete potřebovat:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Přesuňte změněné soubory do správného umístění na disku porušený počítače.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back to the root and unmount the disk.

    ~~~~
    CD / umount /tempmount
    ~~~~

11. Detach the disk from the management portal.

12. Recreate the VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk to another Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How to delete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)