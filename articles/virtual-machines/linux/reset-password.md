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
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a><span data-ttu-id="fa993-103">Jak obnovit heslo místního Linux na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="fa993-103">How to reset local Linux password on Azure VMs</span></span>

<span data-ttu-id="fa993-104">Tento článek představuje několik metod k resetování hesel místní systém Linux virtuálního počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="fa993-104">This article introduces several methods to reset local Linux Virtual Machine (VM) passwords.</span></span> <span data-ttu-id="fa993-105">Pokud vypršela platnost uživatelského účtu nebo jenom chcete vytvořit nový účet, můžete použít následující metody vytvořit nový účet místního správce a znovu získat přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="fa993-105">If the user account is expired or you just want to create a new account, you can use the following methods to create a new local admin account and re-gain access to the VM.</span></span>

## <a name="symptoms"></a><span data-ttu-id="fa993-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="fa993-106">Symptoms</span></span>

<span data-ttu-id="fa993-107">Se nemůže přihlásit k virtuálnímu počítači a obdržíte zprávu, která označuje, že je nesprávné heslo, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="fa993-107">You can't log in to the VM, and you receive a message that indicates that the password that you used is incorrect.</span></span> <span data-ttu-id="fa993-108">Kromě toho nelze použít VMAgent k resetování hesla na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fa993-108">Additionally, you can't use VMAgent to reset your password on the Azure Portal.</span></span> 

## <a name="manual-password-reset-procedure"></a><span data-ttu-id="fa993-109">Ruční postup vytvoření nového hesla</span><span class="sxs-lookup"><span data-stu-id="fa993-109">Manual Password Reset procedure</span></span>

1.  <span data-ttu-id="fa993-110">Odstraňte virtuální počítač a zachovat připojených disků.</span><span class="sxs-lookup"><span data-stu-id="fa993-110">Delete the VM and keep the attached disks.</span></span>

2.  <span data-ttu-id="fa993-111">Připojte jednotku operačního systému jako datový disk k jiné dočasné virtuálních počítačů ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="fa993-111">Attach the OS Drive as a data disk to another temporal VM in the same location.</span></span>

3.  <span data-ttu-id="fa993-112">Pomocí následujícího příkazu SSH na dočasný virtuální počítač stane nadtypem uživatele.</span><span class="sxs-lookup"><span data-stu-id="fa993-112">Run the following SSH command on the temporal VM to become a super-user.</span></span>


    ~~~~
    sudo su
    ~~~~

4.  <span data-ttu-id="fa993-113">Spustit **fdisk -l** nebo v protokolech systému najít nově připojený disk.</span><span class="sxs-lookup"><span data-stu-id="fa993-113">Run **fdisk -l** or look at system logs to find the newly attached disk.</span></span> <span data-ttu-id="fa993-114">Vyhledejte název jednotky připojit.</span><span class="sxs-lookup"><span data-stu-id="fa993-114">Locate the drive name to mount.</span></span> <span data-ttu-id="fa993-115">Pak na dočasný virtuální počítač, zkontrolujte soubor protokolu relevantní.</span><span class="sxs-lookup"><span data-stu-id="fa993-115">Then on the temporal VM, look in the relevant log file.</span></span>

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    <span data-ttu-id="fa993-116">Následuje příklad výstupu příkazu grep:</span><span class="sxs-lookup"><span data-stu-id="fa993-116">The following is example output of the grep command:</span></span>

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  <span data-ttu-id="fa993-117">Vytvořit bod připojení s názvem **tempmount**.</span><span class="sxs-lookup"><span data-stu-id="fa993-117">Create a mount point called **tempmount**.</span></span>

    ~~~~
    mkdir /tempmount
    ~~~~

6.  <span data-ttu-id="fa993-118">Připojte disk operačního systému na přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="fa993-118">Mount the OS disk on the mount point.</span></span> <span data-ttu-id="fa993-119">Obvykle potřebujete připojit sdc1 nebo sdc2.</span><span class="sxs-lookup"><span data-stu-id="fa993-119">You usually need to mount sdc1 or sdc2.</span></span> <span data-ttu-id="fa993-120">To bude záviset na hostování oddílu v adresáři/etc z disku porušený počítače.</span><span class="sxs-lookup"><span data-stu-id="fa993-120">This will depend on the hosting partition in /etc directory from the broken machine disk.</span></span>

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  <span data-ttu-id="fa993-121">Proveďte zálohu před provedením jakýchkoli změn:</span><span class="sxs-lookup"><span data-stu-id="fa993-121">Perform a backup before making any changes:</span></span>

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  <span data-ttu-id="fa993-122">Resetovat heslo uživatele, které budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="fa993-122">Reset the user’s password that you need:</span></span>

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  <span data-ttu-id="fa993-123">Přesuňte změněné soubory do správného umístění na disku porušený počítače.</span><span class="sxs-lookup"><span data-stu-id="fa993-123">Move the modified files to the correct location on the broken machine's disk.</span></span>

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back to the root and unmount the disk.

    ~~~~
    <span data-ttu-id="fa993-124">CD / umount /tempmount</span><span class="sxs-lookup"><span data-stu-id="fa993-124">cd / umount /tempmount</span></span>
    ~~~~

11. Detach the disk from the management portal.

12. Recreate the VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk to another Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How to delete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)