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
ms.openlocfilehash: 3827e32186c5f034d9bb6fc502dc26708b52a00a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-linux-password-on-azure-vms"></a><span data-ttu-id="94655-103">Jak tooreset místní heslo Linux na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="94655-103">How tooreset local Linux password on Azure VMs</span></span>

<span data-ttu-id="94655-104">Tento článek představuje několik metod tooreset místní systém Linux virtuálního počítače (VM) hesla.</span><span class="sxs-lookup"><span data-stu-id="94655-104">This article introduces several methods tooreset local Linux Virtual Machine (VM) passwords.</span></span> <span data-ttu-id="94655-105">Pokud vypršela platnost hello uživatelský účet nebo jenom chcete toocreate nový účet, můžete použít následující metody toocreate nový účet místního správce hello a znovu získat přístup k toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="94655-105">If hello user account is expired or you just want toocreate a new account, you can use hello following methods toocreate a new local admin account and re-gain access toohello VM.</span></span>

## <a name="symptoms"></a><span data-ttu-id="94655-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="94655-106">Symptoms</span></span>

<span data-ttu-id="94655-107">Se nemůže přihlásit toohello virtuálních počítačů a obdržíte zprávu, která označuje, že toto hello heslo, které jste použili není správné.</span><span class="sxs-lookup"><span data-stu-id="94655-107">You can't log in toohello VM, and you receive a message that indicates that hello password that you used is incorrect.</span></span> <span data-ttu-id="94655-108">Kromě toho nelze použít VMAgent tooreset heslo na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="94655-108">Additionally, you can't use VMAgent tooreset your password on hello Azure Portal.</span></span> 

## <a name="manual-password-reset-procedure"></a><span data-ttu-id="94655-109">Ruční postup vytvoření nového hesla</span><span class="sxs-lookup"><span data-stu-id="94655-109">Manual Password Reset procedure</span></span>

1.  <span data-ttu-id="94655-110">Odstraňte hello virtuálních počítačů a zachovat hello připojené disky.</span><span class="sxs-lookup"><span data-stu-id="94655-110">Delete hello VM and keep hello attached disks.</span></span>

2.  <span data-ttu-id="94655-111">Připojte hello jednotce operačního systému jako tooanother disku data dočasný virtuální počítač v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="94655-111">Attach hello OS Drive as a data disk tooanother temporal VM in hello same location.</span></span>

3.  <span data-ttu-id="94655-112">Spusťte následující příkazu SSH na hello dočasné virtuálních počítačů toobecome hello nadtypem uživatele.</span><span class="sxs-lookup"><span data-stu-id="94655-112">Run hello following SSH command on hello temporal VM toobecome a super-user.</span></span>


    ~~~~
    sudo su
    ~~~~

4.  <span data-ttu-id="94655-113">Spustit **fdisk -l** nebo pohled na systém protokoly toofind hello nově připojený disk.</span><span class="sxs-lookup"><span data-stu-id="94655-113">Run **fdisk -l** or look at system logs toofind hello newly attached disk.</span></span> <span data-ttu-id="94655-114">Vyhledejte toomount název jednotky hello.</span><span class="sxs-lookup"><span data-stu-id="94655-114">Locate hello drive name toomount.</span></span> <span data-ttu-id="94655-115">Potom na hello dočasný virtuální počítač, naleznete v příslušných hello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="94655-115">Then on hello temporal VM, look in hello relevant log file.</span></span>

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    <span data-ttu-id="94655-116">Hello následuje příklad výstupu příkazu grep hello:</span><span class="sxs-lookup"><span data-stu-id="94655-116">hello following is example output of hello grep command:</span></span>

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  <span data-ttu-id="94655-117">Vytvořit bod připojení s názvem **tempmount**.</span><span class="sxs-lookup"><span data-stu-id="94655-117">Create a mount point called **tempmount**.</span></span>

    ~~~~
    mkdir /tempmount
    ~~~~

6.  <span data-ttu-id="94655-118">Připojte disk hello operačního systému na hello přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="94655-118">Mount hello OS disk on hello mount point.</span></span> <span data-ttu-id="94655-119">Obvykle potřebujete toomount sdc1 nebo sdc2.</span><span class="sxs-lookup"><span data-stu-id="94655-119">You usually need toomount sdc1 or sdc2.</span></span> <span data-ttu-id="94655-120">To bude záviset na hello hostování oddílu v adresáři/etc z disku porušený počítače hello.</span><span class="sxs-lookup"><span data-stu-id="94655-120">This will depend on hello hosting partition in /etc directory from hello broken machine disk.</span></span>

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  <span data-ttu-id="94655-121">Proveďte zálohu před provedením jakýchkoli změn:</span><span class="sxs-lookup"><span data-stu-id="94655-121">Perform a backup before making any changes:</span></span>

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  <span data-ttu-id="94655-122">Resetování hesla hello uživatele, který budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="94655-122">Reset hello user’s password that you need:</span></span>

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  <span data-ttu-id="94655-123">Přesunutí hello upravit soubory toohello správném umístění na hello poškozený disk počítače.</span><span class="sxs-lookup"><span data-stu-id="94655-123">Move hello modified files toohello correct location on hello broken machine's disk.</span></span>

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back toohello root and unmount hello disk.

    ~~~~
    <span data-ttu-id="94655-124">CD / umount /tempmount</span><span class="sxs-lookup"><span data-stu-id="94655-124">cd / umount /tempmount</span></span>
    ~~~~

11. Detach hello disk from hello management portal.

12. Recreate hello VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk tooanother Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How toodelete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
