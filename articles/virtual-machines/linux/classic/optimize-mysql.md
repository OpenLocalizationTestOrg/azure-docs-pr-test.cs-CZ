---
title: "aaaOptimize MySQL výkonu v systému Linux | Microsoft Docs"
description: "Zjistěte, jak toooptimize MySQL spuštěna na virtuálním počítači Azure (VM) s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 9e6458723233721e06f30b9de33635d403eefcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a><span data-ttu-id="74a77-103">Optimalizace výkonu databáze MySQL na virtuálních počítačích Azure Linux</span><span class="sxs-lookup"><span data-stu-id="74a77-103">Optimize MySQL Performance on Azure Linux VMs</span></span>
<span data-ttu-id="74a77-104">Existuje celá řada faktorů, které ovlivňují výkon databáze MySQL na Azure, jak v výběr virtuální hardwarové a softwarové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="74a77-104">There are many factors that affect MySQL performance on Azure, both in virtual hardware selection and software configuration.</span></span> <span data-ttu-id="74a77-105">Tento článek se zaměřuje na optimalizace výkonu úložiště, systému a konfigurace databáze.</span><span class="sxs-lookup"><span data-stu-id="74a77-105">This article focuses on optimizing performance through storage, system, and database configurations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="74a77-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="74a77-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="74a77-107">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-107">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="74a77-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="74a77-109">Informace o optimalizace virtuálního počítače s Linuxem pomocí modelu Resource Manager hello najdete v tématu [optimalizovat virtuálním počítačům s Linuxem v Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="74a77-109">For information about Linux VM optimizations with hello Resource Manager model, see [Optimize your Linux VM on Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="utilize-raid-on-an-azure-virtual-machine"></a><span data-ttu-id="74a77-110">Využívat RAID na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="74a77-110">Utilize RAID on an Azure virtual machine</span></span>
<span data-ttu-id="74a77-111">Úložiště je hello klíčovým faktorem, který ovlivňuje výkon databáze v prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="74a77-111">Storage is hello key factor that affects database performance in cloud environments.</span></span> <span data-ttu-id="74a77-112">Porovnání tooa jeden disk, RAID zajistí rychlejší přístup prostřednictvím souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="74a77-112">Compared tooa single disk, RAID can provide faster access via concurrency.</span></span> <span data-ttu-id="74a77-113">Další informace najdete v tématu [standardní RAID úrovně](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span><span class="sxs-lookup"><span data-stu-id="74a77-113">For more information, see [Standard RAID levels](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span></span>   

<span data-ttu-id="74a77-114">Propustnost vstupu/výstupu disku a vstupně-výstupních operací dobu odezvy v Azure je možné zlepšit prostřednictvím RAID.</span><span class="sxs-lookup"><span data-stu-id="74a77-114">Disk I/O throughput and I/O response time in Azure can be improved through RAID.</span></span> <span data-ttu-id="74a77-115">Naše testy testovacího prostředí zobrazit, může být dvojitá propustnost vstupu/výstupu disku a doby odezvy vstupně-výstupních operací může snížit o polovinu v průměru při (z toofour dva, čtyři tooeight atd.) se zdvojnásobí hello počet disků RAID.</span><span class="sxs-lookup"><span data-stu-id="74a77-115">Our lab tests show that disk I/O throughput can be doubled and I/O response time can be reduced by half on average when hello number of RAID disks is doubled (from two toofour, four tooeight, etc.).</span></span> <span data-ttu-id="74a77-116">V tématu [příloha A](#AppendixA) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="74a77-116">See [Appendix A](#AppendixA) for details.</span></span>  

<span data-ttu-id="74a77-117">Kromě toho toodisk vstupně-výstupních operací, MySQL výkonu zvyšuje, když zvýšíte úroveň pole RAID hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-117">In addition toodisk I/O, MySQL performance improves when you increase hello RAID level.</span></span>  <span data-ttu-id="74a77-118">V tématu [příloha B](#AppendixB) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="74a77-118">See [Appendix B](#AppendixB) for details.</span></span>  

<span data-ttu-id="74a77-119">Také můžete chtít velikost bloku tooconsider hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-119">You might also want tooconsider hello chunk size.</span></span> <span data-ttu-id="74a77-120">Obecně platí když máte větší velikost bloku, získáte nižší nároky, hlavně pro velké zápisy.</span><span class="sxs-lookup"><span data-stu-id="74a77-120">In general, when you have a larger chunk size, you get lower overhead, especially for large writes.</span></span> <span data-ttu-id="74a77-121">Ale když hello velikost deduplikačního bloku dat je příliš velký, může přidat další režie, které zabraňují využívat výhod RAID.</span><span class="sxs-lookup"><span data-stu-id="74a77-121">However, when hello chunk size is too large, it might add additional overhead that prevents you from taking advantage of RAID.</span></span> <span data-ttu-id="74a77-122">Hello aktuální výchozí velikost je 512 KB, který je ověřené toobe optimální pro nejobecnější provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="74a77-122">hello current default size is 512 KB, which is proven toobe optimal for most general production environments.</span></span> <span data-ttu-id="74a77-123">V tématu [příloha C](#AppendixC) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="74a77-123">See [Appendix C](#AppendixC) for details.</span></span>   

<span data-ttu-id="74a77-124">Existují omezení na tom, kolik disků můžete přidat pro typy jiný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="74a77-124">There are limits on how many disks you can add for different virtual machine types.</span></span> <span data-ttu-id="74a77-125">Tato omezení jsou podrobně popsané na [velikosti virtuálního počítače a cloudové služby pro Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="74a77-125">These limits are detailed in [Virtual machine and cloud service sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span> <span data-ttu-id="74a77-126">I když můžete tooset až RAID s méně disky, budete potřebovat čtyři připojené disky toofollow hello RAID příklad dat v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="74a77-126">You will need four attached data disks toofollow hello RAID example in this article, although you can choose tooset up RAID with fewer disks.</span></span>  

<span data-ttu-id="74a77-127">Tento článek předpokládá jste již vytvořili virtuální počítač s Linuxem a MYSQL nainstalován a nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="74a77-127">This article assumes you have already created a Linux virtual machine and have MYSQL installed and configured.</span></span> <span data-ttu-id="74a77-128">Další informace o začátcích najdete v tématu Jak tooinstall MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="74a77-128">For more information on getting started, see How tooinstall MySQL on Azure.</span></span>  

### <a name="set-up-raid-on-azure"></a><span data-ttu-id="74a77-129">Nastavení diskového pole RAID na Azure</span><span class="sxs-lookup"><span data-stu-id="74a77-129">Set up RAID on Azure</span></span>
<span data-ttu-id="74a77-130">Hello následující kroky ukazují, jak toocreate RAID na platformě Azure pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="74a77-130">hello following steps show how toocreate RAID on Azure by using hello Azure portal.</span></span> <span data-ttu-id="74a77-131">Můžete také nastavit tak RAID pomocí skriptů prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="74a77-131">You can also set up RAID by using Windows PowerShell scripts.</span></span>
<span data-ttu-id="74a77-132">V tomto příkladu nakonfigurujeme RAID 0 s čtyři disky.</span><span class="sxs-lookup"><span data-stu-id="74a77-132">In this example, we will configure RAID 0 with four disks.</span></span>  

#### <a name="add-a-data-disk-tooyour-virtual-machine"></a><span data-ttu-id="74a77-133">Přidat datový disk tooyour virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="74a77-133">Add a data disk tooyour virtual machine</span></span>
<span data-ttu-id="74a77-134">V hello portálu Azure přejděte toohello řídicího panelu a vyberte toowhich hello virtuálního počítače chcete tooadd datový disk.</span><span class="sxs-lookup"><span data-stu-id="74a77-134">In hello Azure portal, go toohello dashboard and select hello virtual machine toowhich you want tooadd a data disk.</span></span> <span data-ttu-id="74a77-135">V tomto příkladu je virtuální počítač hello mysqlnode1.</span><span class="sxs-lookup"><span data-stu-id="74a77-135">In this example, hello virtual machine is mysqlnode1.</span></span>  

<!--![Virtual machines][1]-->

<span data-ttu-id="74a77-136">Klikněte na tlačítko **disky** a pak klikněte na **připojit nový**.</span><span class="sxs-lookup"><span data-stu-id="74a77-136">Click **Disks** and then click **Attach New**.</span></span>

![Virtuální počítače přidejte disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

<span data-ttu-id="74a77-138">Vytvoření nového disku 500 GB.</span><span class="sxs-lookup"><span data-stu-id="74a77-138">Create a new 500 GB disk.</span></span> <span data-ttu-id="74a77-139">Ujistěte se, že **předvoleb mezipaměti hostitele** je nastaven příliš**žádné**.</span><span class="sxs-lookup"><span data-stu-id="74a77-139">Make sure that **Host Cache Preference** is set too**None**.</span></span>  <span data-ttu-id="74a77-140">Až budete hotovi, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="74a77-140">When you're finished, click **OK**.</span></span>

![Připojte prázdný disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


<span data-ttu-id="74a77-142">Tento postup přidá jeden prázdný disk do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="74a77-142">This adds one empty disk into your virtual machine.</span></span> <span data-ttu-id="74a77-143">Opakujte tento krok tři vícekrát, aby měli čtyři datových disků RAID.</span><span class="sxs-lookup"><span data-stu-id="74a77-143">Repeat this step three more times so that you have four data disks for RAID.</span></span>  

<span data-ttu-id="74a77-144">Můžete zobrazit hello přidat jednotky ve virtuálním počítači hello prohlížením hello jádra zprávu protokolu.</span><span class="sxs-lookup"><span data-stu-id="74a77-144">You can see hello added drives in hello virtual machine by looking at hello kernel message log.</span></span> <span data-ttu-id="74a77-145">Například toosee to na Ubuntu hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a77-145">For example, toosee this on Ubuntu, use hello following command:</span></span>  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-hello-additional-disks"></a><span data-ttu-id="74a77-146">Vytvoření RAID s hello dalších disků.</span><span class="sxs-lookup"><span data-stu-id="74a77-146">Create RAID with hello additional disks</span></span>
<span data-ttu-id="74a77-147">Hello následující kroky popisují, jak příliš[konfigurace softwaru diskového pole RAID v systému Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="74a77-147">hello following steps describe how too[configure software RAID on Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="74a77-148">Pokud používáte systém souborů XFS hello, provést následující kroky po vytvoření RAID hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-148">If you are using hello XFS file system, execute hello following steps after you have created RAID.</span></span>
>
>

<span data-ttu-id="74a77-149">tooinstall XFS Debian a Ubuntu, máta Linux hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a77-149">tooinstall XFS on Debian, Ubuntu, or Linux Mint, use hello following command:</span></span>  

    apt-get -y install xfsprogs  

<span data-ttu-id="74a77-150">tooinstall XFS Fedora, CentOS nebo RHEL, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a77-150">tooinstall XFS on Fedora, CentOS, or RHEL, use hello following command:</span></span>  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a><span data-ttu-id="74a77-151">Nastavit novou cestu úložiště</span><span class="sxs-lookup"><span data-stu-id="74a77-151">Set up a new storage path</span></span>
<span data-ttu-id="74a77-152">Použijte následující příkaz tooset si novou cestu úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-152">Use hello following command tooset up a new storage path:</span></span>  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-hello-original-data-toohello-new-storage-path"></a><span data-ttu-id="74a77-153">Zkopírujte hello původní data toohello novou cestu k úložišti</span><span class="sxs-lookup"><span data-stu-id="74a77-153">Copy hello original data toohello new storage path</span></span>
<span data-ttu-id="74a77-154">Použijte následující příkaz toocopy toohello nové úložiště cestu k datům hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-154">Use hello following command toocopy data toohello new storage path:</span></span>  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-hello-data-disk"></a><span data-ttu-id="74a77-155">Upravit oprávnění, můžete přístup MySQL (čtení a zápisu) hello datový disk</span><span class="sxs-lookup"><span data-stu-id="74a77-155">Modify permissions so MySQL can access (read and write) hello data disk</span></span>
<span data-ttu-id="74a77-156">Použijte následující příkaz toomodify oprávnění hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-156">Use hello following command toomodify permissions:</span></span>  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-hello-disk-io-scheduling-algorithm"></a><span data-ttu-id="74a77-157">Upravit v/v disku hello plánování algoritmus</span><span class="sxs-lookup"><span data-stu-id="74a77-157">Adjust hello disk I/O scheduling algorithm</span></span>
<span data-ttu-id="74a77-158">Linux implementuje čtyři typy vstupně-výstupních operací plánování algoritmů:</span><span class="sxs-lookup"><span data-stu-id="74a77-158">Linux implements four types of I/O scheduling algorithms:</span></span>  

* <span data-ttu-id="74a77-159">Nedojde k žádné akci algoritmus (ne operace)</span><span class="sxs-lookup"><span data-stu-id="74a77-159">NOOP algorithm (No Operation)</span></span>
* <span data-ttu-id="74a77-160">Algoritmus konečného termínu (termín)</span><span class="sxs-lookup"><span data-stu-id="74a77-160">Deadline algorithm (Deadline)</span></span>
* <span data-ttu-id="74a77-161">Úplně správného algoritmu front zpráv (CFQ)</span><span class="sxs-lookup"><span data-stu-id="74a77-161">Completely fair queuing algorithm (CFQ)</span></span>
* <span data-ttu-id="74a77-162">Nároky období algoritmus (Anticipatory)</span><span class="sxs-lookup"><span data-stu-id="74a77-162">Budget period algorithm (Anticipatory)</span></span>  

<span data-ttu-id="74a77-163">Můžete vybrat jiný plánovače vstupně-výstupních operací v různých scénářích toooptimize výkonu.</span><span class="sxs-lookup"><span data-stu-id="74a77-163">You can select different I/O schedulers under different scenarios toooptimize performance.</span></span> <span data-ttu-id="74a77-164">V prostředí s úplně náhodný přístup není velký rozdíl mezi hello CFQ a algoritmy konečný termín pro výkon.</span><span class="sxs-lookup"><span data-stu-id="74a77-164">In a completely random access environment, there is not a significant difference between hello CFQ and Deadline algorithms for performance.</span></span> <span data-ttu-id="74a77-165">Doporučujeme, abyste nastavili hello MySQL database prostředí tooDeadline pro stabilitu.</span><span class="sxs-lookup"><span data-stu-id="74a77-165">We recommend that you set hello MySQL database environment tooDeadline for stability.</span></span> <span data-ttu-id="74a77-166">Pokud existuje mnoho sekvenčních vstupně-výstupních operací, CFQ může snížit výkon vstupně-výstupní operace disku.</span><span class="sxs-lookup"><span data-stu-id="74a77-166">If there is a lot of sequential I/O, CFQ might reduce disk I/O performance.</span></span>   

<span data-ttu-id="74a77-167">Pro SSD a dalších zařízení nedojde k žádné akci nebo konečný termín můžete dosáhnout lepší výkon než Plánovač výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-167">For SSD and other equipment, NOOP or Deadline can achieve better performance than hello default scheduler.</span></span>   

<span data-ttu-id="74a77-168">Předchozí toohello jádra 2.5, vstupně-výstupní výchozí hello plánování algoritmus je konečný termín.</span><span class="sxs-lookup"><span data-stu-id="74a77-168">Prior toohello kernel 2.5, hello default I/O scheduling algorithm is Deadline.</span></span> <span data-ttu-id="74a77-169">Počínaje hello jádra 2.6.18, CFQ stala hello výchozí algoritmus plánování vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="74a77-169">Starting with hello kernel 2.6.18, CFQ became hello default I/O scheduling algorithm.</span></span>  <span data-ttu-id="74a77-170">Můžete určit toto nastavení při spuštění jádra nebo dynamicky toto nastavení změnit, když je spuštěn systém hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-170">You can specify this setting at kernel boot time or dynamically modify this setting when hello system is running.</span></span>  

<span data-ttu-id="74a77-171">Hello následující příklad ukazuje, jak toocheck a nastavte hello výchozí plánovač toohello nedojde k žádné akci algoritmus řady Debian distribuční hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-171">hello following example demonstrates how toocheck and set hello default scheduler toohello NOOP algorithm in hello Debian distribution family.</span></span>  

### <a name="view-hello-current-io-scheduler"></a><span data-ttu-id="74a77-172">Zobrazení hello aktuálního vstupně-výstupních operací plánovače</span><span class="sxs-lookup"><span data-stu-id="74a77-172">View hello current I/O scheduler</span></span>
<span data-ttu-id="74a77-173">tooview hello hello scheduler spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a77-173">tooview hello scheduler run hello following command:</span></span>  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

<span data-ttu-id="74a77-174">Zobrazí se následující výstup, který označuje aktuálního plánovače hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-174">You will see following output, which indicates hello current scheduler:</span></span>  

    noop [deadline] cfq


### <a name="change-hello-current-device-devsda-of-hello-io-scheduling-algorithm"></a><span data-ttu-id="74a77-175">Změnit aktuální zařízení hello (/ dev/sda) plánování algoritmu hello vstupně-výstupních operací</span><span class="sxs-lookup"><span data-stu-id="74a77-175">Change hello current device (/dev/sda) of hello I/O scheduling algorithm</span></span>
<span data-ttu-id="74a77-176">Spusťte následující příkazy toochange hello aktuální zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-176">Run hello following commands toochange hello current device:</span></span>  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> <span data-ttu-id="74a77-177">Nastavení to samostatně/dev/sda není užitečné.</span><span class="sxs-lookup"><span data-stu-id="74a77-177">Setting this for /dev/sda alone is not useful.</span></span> <span data-ttu-id="74a77-178">Je nutné ji nastavit na všech discích data níž se nachází databáze hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-178">It must be set on all data disks where hello database resides.</span></span>  
>
>

<span data-ttu-id="74a77-179">Měli byste vidět hello následující výstup, označující, že grub.cfg byla znovu sestavena úspěšně a že scheduler výchozí hello byl aktualizovaný tooNOOP:</span><span class="sxs-lookup"><span data-stu-id="74a77-179">You should see hello following output, indicating that grub.cfg has been rebuilt successfully and that hello default scheduler has been updated tooNOOP:</span></span>  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

<span data-ttu-id="74a77-180">Pro hello Red Hat distribuční rodiny třeba jenom hello, následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a77-180">For hello Red Hat distribution family, you need only hello following command:</span></span>

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a><span data-ttu-id="74a77-181">Konfigurace nastavení operace systému souborů</span><span class="sxs-lookup"><span data-stu-id="74a77-181">Configure system file operations settings</span></span>
<span data-ttu-id="74a77-182">Jeden osvědčeným postupem je toodisable hello *atime* funkce protokolování v systému souborů hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-182">One best practice is toodisable hello *atime* logging feature on hello file system.</span></span> <span data-ttu-id="74a77-183">Atime je hello čas posledního přístupu souboru.</span><span class="sxs-lookup"><span data-stu-id="74a77-183">Atime is hello last file access time.</span></span> <span data-ttu-id="74a77-184">Vždy, když je přístup k souboru, záznamů systému souboru hello hello časové razítko v protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-184">Whenever a file is accessed, hello file system records hello timestamp in hello log.</span></span> <span data-ttu-id="74a77-185">Tyto informace se ale zřídka používá.</span><span class="sxs-lookup"><span data-stu-id="74a77-185">However, this information is rarely used.</span></span> <span data-ttu-id="74a77-186">Ji můžete vypnout, pokud tomu tak není, které se sníží celkový čas přístup k disku.</span><span class="sxs-lookup"><span data-stu-id="74a77-186">You can disable it if you don't need it, which will reduce overall disk access time.</span></span>  

<span data-ttu-id="74a77-187">toodisable atime protokolování, můžete potřebovat toomodify hello souboru systému konfigurační soubor/etc / fstab a přidat hello **noatime** možnost.</span><span class="sxs-lookup"><span data-stu-id="74a77-187">toodisable atime logging, you need toomodify hello file system configuration file /etc/ fstab and add hello **noatime** option.</span></span>  

<span data-ttu-id="74a77-188">Můžete třeba upravte soubor /etc/fstab hello vim přidáním hello noatime, jak je znázorněno v následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-188">For example, edit hello vim /etc/fstab file, adding hello noatime as shown in hello following sample:</span></span>  

    # CLOUD_IMG: This file was created/modified by hello Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add hello “noatime” option below toodisable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

<span data-ttu-id="74a77-189">Znovu připojte hello systém souborů s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74a77-189">Then, remount hello file system with hello following command:</span></span>  

    mount -o remount /RAID0

<span data-ttu-id="74a77-190">Test hello upravit výsledek.</span><span class="sxs-lookup"><span data-stu-id="74a77-190">Test hello modified result.</span></span> <span data-ttu-id="74a77-191">Když upravíte hello testovací soubor, čas přístupu hello se neaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="74a77-191">When you modify hello test file, hello access time is not updated.</span></span> <span data-ttu-id="74a77-192">Dobrý den, následující příklady zobrazují, jaký kód hello vypadá před a po změnách.</span><span class="sxs-lookup"><span data-stu-id="74a77-192">hello following examples show what hello code looks like before and after modification.</span></span>

<span data-ttu-id="74a77-193">Před:</span><span class="sxs-lookup"><span data-stu-id="74a77-193">Before:</span></span>        

![Před úpravou přístupu kódu][5]

<span data-ttu-id="74a77-195">Po:</span><span class="sxs-lookup"><span data-stu-id="74a77-195">After:</span></span>

![Po změnách přístupu kódu][6]

## <a name="increase-hello-maximum-number-of-system-handles-for-high-concurrency"></a><span data-ttu-id="74a77-197">Zvýšit maximální počet popisovačů systému pro vysokou souběžnosti hello</span><span class="sxs-lookup"><span data-stu-id="74a77-197">Increase hello maximum number of system handles for high concurrency</span></span>
<span data-ttu-id="74a77-198">MySQL je vysoká souběžnosti databáze.</span><span class="sxs-lookup"><span data-stu-id="74a77-198">MySQL is a high concurrency database.</span></span> <span data-ttu-id="74a77-199">Hello výchozí počet souběžných obslužných rutin je 1024 pro Linux, který není vždy dostatečná.</span><span class="sxs-lookup"><span data-stu-id="74a77-199">hello default number of concurrent handles is 1024 for Linux, which is not always sufficient.</span></span> <span data-ttu-id="74a77-200">Pomocí následujících kroků tooincrease hello maximální souběžných popisovačů systému hello systému toosupport vysoké souběžnosti z databáze MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-200">Use hello following steps tooincrease hello maximum concurrent handles of hello system toosupport high concurrency of MySQL.</span></span>

### <a name="modify-hello-limitsconf-file"></a><span data-ttu-id="74a77-201">Upravte soubor limits.conf hello</span><span class="sxs-lookup"><span data-stu-id="74a77-201">Modify hello limits.conf file</span></span>
<span data-ttu-id="74a77-202">tooincrease hello maximální povolené souběžných obslužných rutin, přidejte následující čtyři řádků v souboru /etc/security/limits.conf hello hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-202">tooincrease hello maximum allowed concurrent handles, add hello following four lines in hello /etc/security/limits.conf file.</span></span> <span data-ttu-id="74a77-203">Všimněte si, že je 65536 hello maximální počet, který podporuje hello systému.</span><span class="sxs-lookup"><span data-stu-id="74a77-203">Note that 65536 is hello maximum number that hello system can support.</span></span>   

    * <span data-ttu-id="74a77-204">logicky nofile 65536</span><span class="sxs-lookup"><span data-stu-id="74a77-204">soft nofile 65536</span></span>
    * <span data-ttu-id="74a77-205">pevné nofile 65536</span><span class="sxs-lookup"><span data-stu-id="74a77-205">hard nofile 65536</span></span>
    * <span data-ttu-id="74a77-206">logicky nproc 65536</span><span class="sxs-lookup"><span data-stu-id="74a77-206">soft nproc 65536</span></span>
    * <span data-ttu-id="74a77-207">pevné nproc 65536</span><span class="sxs-lookup"><span data-stu-id="74a77-207">hard nproc 65536</span></span>

### <a name="update-hello-system-for-hello-new-limits"></a><span data-ttu-id="74a77-208">Aktualizovat hello systém nové omezení hello</span><span class="sxs-lookup"><span data-stu-id="74a77-208">Update hello system for hello new limits</span></span>
<span data-ttu-id="74a77-209">tooupdate hello systému, spustit hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="74a77-209">tooupdate hello system, run hello following commands:</span></span>  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-hello-limits-are-updated-at-boot-time"></a><span data-ttu-id="74a77-210">Zajistěte, aby se při spuštění aktualizovala hello omezení</span><span class="sxs-lookup"><span data-stu-id="74a77-210">Ensure that hello limits are updated at boot time</span></span>
<span data-ttu-id="74a77-211">Uveďte hello následující příkazy spuštění v souboru /etc/rc.local hello tak projeví při spuštění.</span><span class="sxs-lookup"><span data-stu-id="74a77-211">Put hello following startup commands in hello /etc/rc.local file so it will take effect at boot time.</span></span>  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a><span data-ttu-id="74a77-212">Optimalizace databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="74a77-212">MySQL database optimization</span></span>
<span data-ttu-id="74a77-213">tooconfigure MySQL v Azure, můžete použít hello stejné strategie optimalizace výkonu můžete použít na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="74a77-213">tooconfigure MySQL on Azure, you can use hello same performance-tuning strategy you use on an on-premises machine.</span></span>  

<span data-ttu-id="74a77-214">Hello hlavní vstupně-výstupních operací optimalizace pravidel jsou:</span><span class="sxs-lookup"><span data-stu-id="74a77-214">hello main I/O optimization rules are:</span></span>   

* <span data-ttu-id="74a77-215">Zvětšete velikost mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-215">Increase hello cache size.</span></span>
* <span data-ttu-id="74a77-216">Snížení doby odezvy vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="74a77-216">Reduce I/O response time.</span></span>  

<span data-ttu-id="74a77-217">nastavení serveru toooptimize MySQL, můžete aktualizovat hello my.cnf souboru, který je hello výchozí konfigurační soubor pro server a klientských počítačů.</span><span class="sxs-lookup"><span data-stu-id="74a77-217">toooptimize MySQL server settings, you can update hello my.cnf file, which is hello default configuration file for both server and client computers.</span></span>  

<span data-ttu-id="74a77-218">Hello následující položky konfigurace jsou hello hlavní faktory, které ovlivňují výkon MySQL:</span><span class="sxs-lookup"><span data-stu-id="74a77-218">hello following configuration items are hello main factors that affect MySQL performance:</span></span>  

* <span data-ttu-id="74a77-219">**innodb_buffer_pool_size**: fondu vyrovnávací paměti hello obsahuje data ve vyrovnávací paměti a hello index.</span><span class="sxs-lookup"><span data-stu-id="74a77-219">**innodb_buffer_pool_size**: hello buffer pool contains buffered data and hello index.</span></span> <span data-ttu-id="74a77-220">Obvykle je nastavena v procentech too70 fyzické paměti.</span><span class="sxs-lookup"><span data-stu-id="74a77-220">This is usually set too70 percent of physical memory.</span></span>
* <span data-ttu-id="74a77-221">**innodb_log_file_size**: Toto je velikost protokolu hello operaci znovu.</span><span class="sxs-lookup"><span data-stu-id="74a77-221">**innodb_log_file_size**: This is hello redo log size.</span></span> <span data-ttu-id="74a77-222">Můžete použít tooensure protokoly opakování operace zápisu jsou rychlé, spolehlivé a použitelná pro obnovení po chybě.</span><span class="sxs-lookup"><span data-stu-id="74a77-222">You use redo logs tooensure that write operations are fast, reliable, and recoverable after a crash.</span></span> <span data-ttu-id="74a77-223">Toto nastavení too512 MB, který vám poskytne dostatek místa pro protokolování operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="74a77-223">This is set too512 MB, which will give you plenty of space for logging write operations.</span></span>
* <span data-ttu-id="74a77-224">**max_connections**: v některých případech aplikace neukončujte připojení správně.</span><span class="sxs-lookup"><span data-stu-id="74a77-224">**max_connections**: Sometimes applications do not close connections properly.</span></span> <span data-ttu-id="74a77-225">Větší hodnotu získáte hello server déle toorecycle nečinný připojení.</span><span class="sxs-lookup"><span data-stu-id="74a77-225">A larger value will give hello server more time toorecycle idled connections.</span></span> <span data-ttu-id="74a77-226">Hello maximální počet připojení je 10 000, ale doporučuje hello, že maximální počet je 5 000.</span><span class="sxs-lookup"><span data-stu-id="74a77-226">hello maximum number of connections is 10,000, but hello recommended maximum is 5,000.</span></span>
* <span data-ttu-id="74a77-227">**Innodb_file_per_table**: Toto nastavení povolí nebo zakáže možnost hello InnoDB toostore tabulek v samostatné soubory.</span><span class="sxs-lookup"><span data-stu-id="74a77-227">**Innodb_file_per_table**: This setting enables or disables hello ability of InnoDB toostore tables in separate files.</span></span> <span data-ttu-id="74a77-228">Zapněte tooensure hello možnost, že několik operací pokročilé správy může být použitá efektivně.</span><span class="sxs-lookup"><span data-stu-id="74a77-228">Turn on hello option tooensure that several advanced administration operations can be applied efficiently.</span></span> <span data-ttu-id="74a77-229">Z výkonu hlediska může urychlit přenos místo tabulky hello a optimalizace výkonu správy zbytků hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-229">From a performance point of view, it can speed up hello table space transmission and optimize hello debris management performance.</span></span> <span data-ttu-id="74a77-230">Hello doporučená nastavení pro tuto možnost je ON.</span><span class="sxs-lookup"><span data-stu-id="74a77-230">hello recommended setting for this option is ON.</span></span></br></br>
<span data-ttu-id="74a77-231">Z databáze MySQL 5.6 hello výchozí nastavení je ON, takže není vyžadována žádná akce.</span><span class="sxs-lookup"><span data-stu-id="74a77-231">From MySQL 5.6, hello default setting is ON, so no action is required.</span></span> <span data-ttu-id="74a77-232">U starších verzí hello výchozí nastavení je VYPNUTÝ.</span><span class="sxs-lookup"><span data-stu-id="74a77-232">For earlier versions, hello default setting is OFF.</span></span> <span data-ttu-id="74a77-233">Hello parametr změnit před načtením dat, protože to ovlivňuje pouze nově vytvořené tabulky.</span><span class="sxs-lookup"><span data-stu-id="74a77-233">hello setting should be changed before data is loaded, because only newly created tables are affected.</span></span>
* <span data-ttu-id="74a77-234">**innodb_flush_log_at_trx_commit**: hello výchozí hodnota je 1, s hello nastavte obor too0 ~ 2.</span><span class="sxs-lookup"><span data-stu-id="74a77-234">**innodb_flush_log_at_trx_commit**: hello default value is 1, with hello scope set too0~2.</span></span> <span data-ttu-id="74a77-235">Hello výchozí hodnota je hello nejvíce vhodnou možností pro samostatné databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="74a77-235">hello default value is hello most suitable option for standalone MySQL DB.</span></span> <span data-ttu-id="74a77-236">nastavení Hello 2 umožňuje hello většina integritu dat a je vhodný pro hlavní server v clusteru MySQL.</span><span class="sxs-lookup"><span data-stu-id="74a77-236">hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL Cluster.</span></span> <span data-ttu-id="74a77-237">nastavení Hello 0 umožňuje ztrátě dat, která může mít vliv na spolehlivost (v některých případech s lepším výkonem) a je vhodný pro podřízený v clusteru MySQL.</span><span class="sxs-lookup"><span data-stu-id="74a77-237">hello setting of 0 allows data loss, which can affect reliability (in some cases with better performance), and is suitable for Slave in MySQL Cluster.</span></span>
* <span data-ttu-id="74a77-238">**Innodb_log_buffer_size**: hello protokolu vyrovnávací paměti umožňuje transakce toorun bez nutnosti tooflush hello protokolu toodisk před potvrzení transakce hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-238">**Innodb_log_buffer_size**: hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit.</span></span> <span data-ttu-id="74a77-239">Pokud je binární rozsáhlý objekt nebo textové pole, mezipaměti hello využijí rychle a aktivuje se často diskové vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="74a77-239">However, if there is large binary object or text field, hello cache will be consumed quickly and frequent disk I/O will be triggered.</span></span> <span data-ttu-id="74a77-240">Je lépe zvýšit velikost vyrovnávací paměti hello, pokud není Innodb_log_waits proměnné stavu 0.</span><span class="sxs-lookup"><span data-stu-id="74a77-240">It is better increase hello buffer size if Innodb_log_waits state variable is not 0.</span></span>
* <span data-ttu-id="74a77-241">**query_cache_size**: hello nejlepší možnost je toodisable z hello outset.</span><span class="sxs-lookup"><span data-stu-id="74a77-241">**query_cache_size**: hello best option is toodisable it from hello outset.</span></span> <span data-ttu-id="74a77-242">Nastavit query_cache_size too0 (Toto je výchozí nastavení hello v MySQL 5.6) a použít jiné metody toospeed zpracování dotazů.</span><span class="sxs-lookup"><span data-stu-id="74a77-242">Set query_cache_size too0 (this is hello default setting in MySQL 5.6) and use other methods toospeed up queries.</span></span>  

<span data-ttu-id="74a77-243">V tématu [Dodatek D](#AppendixD) porovnání před a po hello optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="74a77-243">See [Appendix D](#AppendixD) for a comparison of performance before and after hello optimization.</span></span>

## <a name="turn-on-hello-mysql-slow-query-log-for-analyzing-hello-performance-bottleneck"></a><span data-ttu-id="74a77-244">Zapnout protokol pomalé dotazu hello MySQL pro analýzu hello přetížení</span><span class="sxs-lookup"><span data-stu-id="74a77-244">Turn on hello MySQL slow query log for analyzing hello performance bottleneck</span></span>
<span data-ttu-id="74a77-245">protokol pomalé dotazu MySQL Hello můžete identifikovat hello pomalé dotazů pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="74a77-245">hello MySQL slow query log can help you identify hello slow queries for MySQL.</span></span> <span data-ttu-id="74a77-246">Když povolíte protokol pomalé dotazu hello MySQL, můžete použít nástroje MySQL jako **mysqldumpslow** tooidentify hello přetížení.</span><span class="sxs-lookup"><span data-stu-id="74a77-246">After enabling hello MySQL slow query log, you can use MySQL tools like **mysqldumpslow** tooidentify hello performance bottleneck.</span></span>  

<span data-ttu-id="74a77-247">Ve výchozím nastavení to není povoleno.</span><span class="sxs-lookup"><span data-stu-id="74a77-247">By default, this is not enabled.</span></span> <span data-ttu-id="74a77-248">Zapnutí protokol pomalé dotazu hello můžou využívat některé prostředky procesoru.</span><span class="sxs-lookup"><span data-stu-id="74a77-248">Turning on hello slow query log might consume some CPU resources.</span></span> <span data-ttu-id="74a77-249">Doporučujeme, abyste povolili to dočasně pro řešení potíží s kritické body.</span><span class="sxs-lookup"><span data-stu-id="74a77-249">We recommend that you enable this temporarily for troubleshooting performance bottlenecks.</span></span> <span data-ttu-id="74a77-250">tooturn na protokol hello pomalé dotazu:</span><span class="sxs-lookup"><span data-stu-id="74a77-250">tooturn on hello slow query log:</span></span>

1. <span data-ttu-id="74a77-251">Upravte soubor my.cnf hello přidáním následující řádky toohello end hello:</span><span class="sxs-lookup"><span data-stu-id="74a77-251">Modify hello my.cnf file by adding hello following lines toohello end:</span></span>

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. <span data-ttu-id="74a77-252">Restartujte server, MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="74a77-252">Restart hello MySQL server.</span></span>

        service  mysql  restart

3. <span data-ttu-id="74a77-253">Zkontrolujte, zda text hello nastavení trvá vliv pomocí hello **zobrazit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="74a77-253">Check whether hello setting is taking effect by using hello **show** command.</span></span>

![ON zpomalit protokol dotazu][7]   

![Výsledky zpomalit protokol dotazu][8]

<span data-ttu-id="74a77-256">V tomto příkladu vidíte, že tato funkce pomalé dotazu hello je zapnutý.</span><span class="sxs-lookup"><span data-stu-id="74a77-256">In this example, you can see that hello slow query feature has been turned on.</span></span> <span data-ttu-id="74a77-257">Pak můžete použít hello **mysqldumpslow** nástroj kritické body toodetermine a optimalizace výkonu, jako je například přidávání indexy.</span><span class="sxs-lookup"><span data-stu-id="74a77-257">You can then use hello **mysqldumpslow** tool toodetermine performance bottlenecks and optimize performance, such as adding indexes.</span></span>

## <a name="appendices"></a><span data-ttu-id="74a77-258">Přílohy</span><span class="sxs-lookup"><span data-stu-id="74a77-258">Appendices</span></span>
<span data-ttu-id="74a77-259">Hello následují ukázková výkonu testovací data vytvořeného v cílové testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="74a77-259">hello following are sample performance test data produced in a targeted lab environment.</span></span> <span data-ttu-id="74a77-260">Poskytují obecné na trend data výkonu hello s jinou ladění přístupy výkonu.</span><span class="sxs-lookup"><span data-stu-id="74a77-260">They provide general background on hello performance data trend with different performance tuning approaches.</span></span> <span data-ttu-id="74a77-261">výsledky Hello se mohou lišit v rámci různých verzí prostředí nebo produktu.</span><span class="sxs-lookup"><span data-stu-id="74a77-261">hello results might vary under different environment or product versions.</span></span>

### <span data-ttu-id="74a77-262"><a name="AppendixA"></a>Příloha A</span><span class="sxs-lookup"><span data-stu-id="74a77-262"><a name="AppendixA"></a>Appendix A</span></span>  
<span data-ttu-id="74a77-263">**Výkon disku (IOPS) s různými úrovněmi diskového pole RAID**</span><span class="sxs-lookup"><span data-stu-id="74a77-263">**Disk performance (IOPS) with different RAID levels**</span></span>

![Disk IOPS s různými úrovněmi diskového pole RAID][9]

<span data-ttu-id="74a77-265">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="74a77-265">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> <span data-ttu-id="74a77-266">Hello úlohy, které obsahují tento test používá 64 vláken, pokusu o tooreach hello horní limit počtu RAID.</span><span class="sxs-lookup"><span data-stu-id="74a77-266">hello workload of this test uses 64 threads, trying tooreach hello upper limit of RAID.</span></span>
>
>

### <span data-ttu-id="74a77-267"><a name="AppendixB"></a>Dodatek B</span><span class="sxs-lookup"><span data-stu-id="74a77-267"><a name="AppendixB"></a>Appendix B</span></span>  
<span data-ttu-id="74a77-268">**Porovnání výkonu (propustnost) MySQL s různými úrovněmi diskového pole RAID** </span><span class="sxs-lookup"><span data-stu-id="74a77-268">**MySQL performance (throughput) comparison with different RAID levels** </span></span>  
<span data-ttu-id="74a77-269">(Systém souborů XFS)</span><span class="sxs-lookup"><span data-stu-id="74a77-269">(XFS file system)</span></span>

![Porovnání výkonu MySQL s různými úrovněmi diskového pole RAID][10]  
![Porovnání výkonu MySQL s různými úrovněmi diskového pole RAID][11]

<span data-ttu-id="74a77-272">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="74a77-272">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

<span data-ttu-id="74a77-273">**Porovnání výkonu (OLTP) MySQL s různými úrovněmi diskového pole RAID**</span><span class="sxs-lookup"><span data-stu-id="74a77-273">**MySQL performance (OLTP) comparison with different RAID levels**</span></span>  
<span data-ttu-id="74a77-274">![Porovnání výkonu (OLTP) MySQL s různými úrovněmi diskového pole RAID][12]</span><span class="sxs-lookup"><span data-stu-id="74a77-274">![MySQL performance (OLTP) comparison with different RAID levels][12]</span></span>

<span data-ttu-id="74a77-275">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="74a77-275">**Test commands**</span></span>

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <span data-ttu-id="74a77-276"><a name="AppendixC"></a>Příloha C</span><span class="sxs-lookup"><span data-stu-id="74a77-276"><a name="AppendixC"></a>Appendix C</span></span>   
<span data-ttu-id="74a77-277">**Porovnání výkonu (IOPS) disku pro různé bloku velikosti**</span><span class="sxs-lookup"><span data-stu-id="74a77-277">**Disk performance (IOPS) comparison for different chunk sizes**</span></span>  
<span data-ttu-id="74a77-278">(Systém souborů XFS)</span><span class="sxs-lookup"><span data-stu-id="74a77-278">(XFS file system)</span></span>

![][13]

<span data-ttu-id="74a77-279">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="74a77-279">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

<span data-ttu-id="74a77-280">použít pro toto testování velikosti souborů Hello 30 GB 1 GB, v uvedeném pořadí a s RAID 0 (4 disky) XFS systému souborů.</span><span class="sxs-lookup"><span data-stu-id="74a77-280">hello file sizes used for this testing are 30 GB and 1 GB, respectively, with RAID 0 (4 disks) XFS file system.</span></span>

### <span data-ttu-id="74a77-281"><a name="AppendixD"></a>Dodatek D</span><span class="sxs-lookup"><span data-stu-id="74a77-281"><a name="AppendixD"></a>Appendix D</span></span>  
<span data-ttu-id="74a77-282">**Porovnání výkonu (propustnost) MySQL před a po optimalizace**</span><span class="sxs-lookup"><span data-stu-id="74a77-282">**MySQL performance (throughput) comparison before and after optimization**</span></span>  
<span data-ttu-id="74a77-283">(Systém souborů XFS)</span><span class="sxs-lookup"><span data-stu-id="74a77-283">(XFS File System)</span></span>

![Porovnání výkonu (propustnost) MySQL před a po optimalizace][14]

<span data-ttu-id="74a77-285">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="74a77-285">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

<span data-ttu-id="74a77-286">**nastavení konfigurace Hello výchozí a optimalizace vypadá takto:**</span><span class="sxs-lookup"><span data-stu-id="74a77-286">**hello configuration setting for default and optimization is as follows:**</span></span>

| <span data-ttu-id="74a77-287">Parametry</span><span class="sxs-lookup"><span data-stu-id="74a77-287">Parameters</span></span> | <span data-ttu-id="74a77-288">Výchozí</span><span class="sxs-lookup"><span data-stu-id="74a77-288">Default</span></span> | <span data-ttu-id="74a77-289">Optimalizace</span><span class="sxs-lookup"><span data-stu-id="74a77-289">Optimization</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74a77-290">**innodb_buffer_pool_size**</span><span class="sxs-lookup"><span data-stu-id="74a77-290">**innodb_buffer_pool_size**</span></span> |<span data-ttu-id="74a77-291">Žádný</span><span class="sxs-lookup"><span data-stu-id="74a77-291">None</span></span> |<span data-ttu-id="74a77-292">7 GB</span><span class="sxs-lookup"><span data-stu-id="74a77-292">7 GB</span></span> |
| <span data-ttu-id="74a77-293">**innodb_log_file_size**</span><span class="sxs-lookup"><span data-stu-id="74a77-293">**innodb_log_file_size**</span></span> |<span data-ttu-id="74a77-294">5 MB.</span><span class="sxs-lookup"><span data-stu-id="74a77-294">5 MB</span></span> |<span data-ttu-id="74a77-295">512 MB</span><span class="sxs-lookup"><span data-stu-id="74a77-295">512 MB</span></span> |
| <span data-ttu-id="74a77-296">**max_connections**</span><span class="sxs-lookup"><span data-stu-id="74a77-296">**max_connections**</span></span> |<span data-ttu-id="74a77-297">100</span><span class="sxs-lookup"><span data-stu-id="74a77-297">100</span></span> |<span data-ttu-id="74a77-298">5000</span><span class="sxs-lookup"><span data-stu-id="74a77-298">5000</span></span> |
| <span data-ttu-id="74a77-299">**innodb_file_per_table**</span><span class="sxs-lookup"><span data-stu-id="74a77-299">**innodb_file_per_table**</span></span> |<span data-ttu-id="74a77-300">0</span><span class="sxs-lookup"><span data-stu-id="74a77-300">0</span></span> |<span data-ttu-id="74a77-301">1</span><span class="sxs-lookup"><span data-stu-id="74a77-301">1</span></span> |
| <span data-ttu-id="74a77-302">**innodb_flush_log_at_trx_commit**</span><span class="sxs-lookup"><span data-stu-id="74a77-302">**innodb_flush_log_at_trx_commit**</span></span> |<span data-ttu-id="74a77-303">1</span><span class="sxs-lookup"><span data-stu-id="74a77-303">1</span></span> |<span data-ttu-id="74a77-304">2</span><span class="sxs-lookup"><span data-stu-id="74a77-304">2</span></span> |
| <span data-ttu-id="74a77-305">**innodb_log_buffer_size**</span><span class="sxs-lookup"><span data-stu-id="74a77-305">**innodb_log_buffer_size**</span></span> |<span data-ttu-id="74a77-306">8 MB</span><span class="sxs-lookup"><span data-stu-id="74a77-306">8 MB</span></span> |<span data-ttu-id="74a77-307">128 MB</span><span class="sxs-lookup"><span data-stu-id="74a77-307">128 MB</span></span> |
| <span data-ttu-id="74a77-308">**query_cache_size**</span><span class="sxs-lookup"><span data-stu-id="74a77-308">**query_cache_size**</span></span> |<span data-ttu-id="74a77-309">16 MB</span><span class="sxs-lookup"><span data-stu-id="74a77-309">16 MB</span></span> |<span data-ttu-id="74a77-310">0</span><span class="sxs-lookup"><span data-stu-id="74a77-310">0</span></span> |

<span data-ttu-id="74a77-311">Další podrobné [parametry konfigurace optimalizace](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), najdete v toohello [MySQL oficiální pokyny](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span><span class="sxs-lookup"><span data-stu-id="74a77-311">For more detailed [optimization configuration parameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), refer toohello [MySQL official instructions](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span></span>  

  <span data-ttu-id="74a77-312">**Testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="74a77-312">**Test environment**</span></span>  

| <span data-ttu-id="74a77-313">Hardware</span><span class="sxs-lookup"><span data-stu-id="74a77-313">Hardware</span></span> | <span data-ttu-id="74a77-314">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="74a77-314">Details</span></span> |
| --- | --- |
| <span data-ttu-id="74a77-315">Procesor</span><span class="sxs-lookup"><span data-stu-id="74a77-315">CPU</span></span> |<span data-ttu-id="74a77-316">AMD Opteron(tm) procesoru 4171 HE / 4 jádra</span><span class="sxs-lookup"><span data-stu-id="74a77-316">AMD Opteron(tm) Processor 4171 HE/4 cores</span></span> |
| <span data-ttu-id="74a77-317">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="74a77-317">Memory</span></span> |<span data-ttu-id="74a77-318">14 GB</span><span class="sxs-lookup"><span data-stu-id="74a77-318">14 GB</span></span> |
| <span data-ttu-id="74a77-319">Disk</span><span class="sxs-lookup"><span data-stu-id="74a77-319">Disk</span></span> |<span data-ttu-id="74a77-320">10 GB místa na disku</span><span class="sxs-lookup"><span data-stu-id="74a77-320">10 GB/disk</span></span> |
| <span data-ttu-id="74a77-321">Operační systém</span><span class="sxs-lookup"><span data-stu-id="74a77-321">OS</span></span> |<span data-ttu-id="74a77-322">Ubuntu 14.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="74a77-322">Ubuntu 14.04.1 LTS</span></span> |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

