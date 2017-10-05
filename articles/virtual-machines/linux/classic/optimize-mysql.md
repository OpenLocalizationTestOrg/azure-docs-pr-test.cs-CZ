---
title: "Optimalizace výkonu databáze MySQL na systému Linux | Microsoft Docs"
description: "Informace o optimalizaci MySQL spuštěna na virtuálním počítači Azure (VM) s Linuxem."
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
ms.openlocfilehash: 8f2ec884fa98e989448ac11675e71f39aa21fa7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a><span data-ttu-id="2540a-103">Optimalizace výkonu databáze MySQL na virtuálních počítačích Azure Linux</span><span class="sxs-lookup"><span data-stu-id="2540a-103">Optimize MySQL Performance on Azure Linux VMs</span></span>
<span data-ttu-id="2540a-104">Existuje celá řada faktorů, které ovlivňují výkon databáze MySQL na Azure, jak v výběr virtuální hardwarové a softwarové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2540a-104">There are many factors that affect MySQL performance on Azure, both in virtual hardware selection and software configuration.</span></span> <span data-ttu-id="2540a-105">Tento článek se zaměřuje na optimalizace výkonu úložiště, systému a konfigurace databáze.</span><span class="sxs-lookup"><span data-stu-id="2540a-105">This article focuses on optimizing performance through storage, system, and database configurations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2540a-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="2540a-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="2540a-107">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="2540a-107">This article covers using the classic deployment model.</span></span> <span data-ttu-id="2540a-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2540a-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="2540a-109">Informace o optimalizace virtuálního počítače s Linuxem pomocí modelu Resource Manager najdete v tématu [optimalizovat virtuálním počítačům s Linuxem v Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2540a-109">For information about Linux VM optimizations with the Resource Manager model, see [Optimize your Linux VM on Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="utilize-raid-on-an-azure-virtual-machine"></a><span data-ttu-id="2540a-110">Využívat RAID na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="2540a-110">Utilize RAID on an Azure virtual machine</span></span>
<span data-ttu-id="2540a-111">Úložiště je klíčovým faktorem, který ovlivňuje výkon databáze v prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="2540a-111">Storage is the key factor that affects database performance in cloud environments.</span></span> <span data-ttu-id="2540a-112">Porovnání na jeden disk, RAID zajistí rychlejší přístup prostřednictvím souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2540a-112">Compared to a single disk, RAID can provide faster access via concurrency.</span></span> <span data-ttu-id="2540a-113">Další informace najdete v tématu [standardní RAID úrovně](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span><span class="sxs-lookup"><span data-stu-id="2540a-113">For more information, see [Standard RAID levels](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span></span>   

<span data-ttu-id="2540a-114">Propustnost vstupu/výstupu disku a vstupně-výstupních operací dobu odezvy v Azure je možné zlepšit prostřednictvím RAID.</span><span class="sxs-lookup"><span data-stu-id="2540a-114">Disk I/O throughput and I/O response time in Azure can be improved through RAID.</span></span> <span data-ttu-id="2540a-115">Naše testy testovacího prostředí zobrazit, může být dvojitá propustnost vstupu/výstupu disku a vstupně-výstupních operací odezvu může snížit půl v průměru při je dvojnásobný počet disků RAID (ze dvou na čtyři, čtyř do osmi atd.).</span><span class="sxs-lookup"><span data-stu-id="2540a-115">Our lab tests show that disk I/O throughput can be doubled and I/O response time can be reduced by half on average when the number of RAID disks is doubled (from two to four, four to eight, etc.).</span></span> <span data-ttu-id="2540a-116">V tématu [příloha A](#AppendixA) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2540a-116">See [Appendix A](#AppendixA) for details.</span></span>  

<span data-ttu-id="2540a-117">Kromě diskových operací zlepšuje výkon MySQL když zvýšíte úroveň pole RAID.</span><span class="sxs-lookup"><span data-stu-id="2540a-117">In addition to disk I/O, MySQL performance improves when you increase the RAID level.</span></span>  <span data-ttu-id="2540a-118">V tématu [příloha B](#AppendixB) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2540a-118">See [Appendix B](#AppendixB) for details.</span></span>  

<span data-ttu-id="2540a-119">Můžete také zvážit velikost bloku.</span><span class="sxs-lookup"><span data-stu-id="2540a-119">You might also want to consider the chunk size.</span></span> <span data-ttu-id="2540a-120">Obecně platí když máte větší velikost bloku, získáte nižší nároky, hlavně pro velké zápisy.</span><span class="sxs-lookup"><span data-stu-id="2540a-120">In general, when you have a larger chunk size, you get lower overhead, especially for large writes.</span></span> <span data-ttu-id="2540a-121">Ale pokud velikost bloku je příliš velký, může přidat další režie, které zabraňují využívat výhod RAID.</span><span class="sxs-lookup"><span data-stu-id="2540a-121">However, when the chunk size is too large, it might add additional overhead that prevents you from taking advantage of RAID.</span></span> <span data-ttu-id="2540a-122">Aktuální výchozí velikost je 512 KB, který je ověřené být optimální pro nejobecnější provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="2540a-122">The current default size is 512 KB, which is proven to be optimal for most general production environments.</span></span> <span data-ttu-id="2540a-123">V tématu [příloha C](#AppendixC) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2540a-123">See [Appendix C](#AppendixC) for details.</span></span>   

<span data-ttu-id="2540a-124">Existují omezení na tom, kolik disků můžete přidat pro typy jiný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2540a-124">There are limits on how many disks you can add for different virtual machine types.</span></span> <span data-ttu-id="2540a-125">Tato omezení jsou podrobně popsané na [velikosti virtuálního počítače a cloudové služby pro Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="2540a-125">These limits are detailed in [Virtual machine and cloud service sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span> <span data-ttu-id="2540a-126">I když můžete nastavit RAID s méně disky, budete potřebovat čtyři připojené datových disků RAID příkladu v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2540a-126">You will need four attached data disks to follow the RAID example in this article, although you can choose to set up RAID with fewer disks.</span></span>  

<span data-ttu-id="2540a-127">Tento článek předpokládá jste již vytvořili virtuální počítač s Linuxem a MYSQL nainstalován a nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="2540a-127">This article assumes you have already created a Linux virtual machine and have MYSQL installed and configured.</span></span> <span data-ttu-id="2540a-128">Další informace o Začínáme najdete v části Jak nainstalovat MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="2540a-128">For more information on getting started, see How to install MySQL on Azure.</span></span>  

### <a name="set-up-raid-on-azure"></a><span data-ttu-id="2540a-129">Nastavení diskového pole RAID na Azure</span><span class="sxs-lookup"><span data-stu-id="2540a-129">Set up RAID on Azure</span></span>
<span data-ttu-id="2540a-130">Následující kroky ukazují, jak vytvořit RAID na platformě Azure pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2540a-130">The following steps show how to create RAID on Azure by using the Azure portal.</span></span> <span data-ttu-id="2540a-131">Můžete také nastavit tak RAID pomocí skriptů prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2540a-131">You can also set up RAID by using Windows PowerShell scripts.</span></span>
<span data-ttu-id="2540a-132">V tomto příkladu nakonfigurujeme RAID 0 s čtyři disky.</span><span class="sxs-lookup"><span data-stu-id="2540a-132">In this example, we will configure RAID 0 with four disks.</span></span>  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a><span data-ttu-id="2540a-133">Přidat datový disk k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="2540a-133">Add a data disk to your virtual machine</span></span>
<span data-ttu-id="2540a-134">Na portálu Azure přejděte do řídicího panelu a vyberte virtuální počítač, do které chcete přidat datový disk.</span><span class="sxs-lookup"><span data-stu-id="2540a-134">In the Azure portal, go to the dashboard and select the virtual machine to which you want to add a data disk.</span></span> <span data-ttu-id="2540a-135">V tomto příkladu je virtuální počítač mysqlnode1.</span><span class="sxs-lookup"><span data-stu-id="2540a-135">In this example, the virtual machine is mysqlnode1.</span></span>  

<!--![Virtual machines][1]-->

<span data-ttu-id="2540a-136">Klikněte na tlačítko **disky** a pak klikněte na **připojit nový**.</span><span class="sxs-lookup"><span data-stu-id="2540a-136">Click **Disks** and then click **Attach New**.</span></span>

![Virtuální počítače přidejte disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

<span data-ttu-id="2540a-138">Vytvoření nového disku 500 GB.</span><span class="sxs-lookup"><span data-stu-id="2540a-138">Create a new 500 GB disk.</span></span> <span data-ttu-id="2540a-139">Ujistěte se, že **předvoleb mezipaměti hostitele** je nastaven na **žádné**.</span><span class="sxs-lookup"><span data-stu-id="2540a-139">Make sure that **Host Cache Preference** is set to **None**.</span></span>  <span data-ttu-id="2540a-140">Až budete hotovi, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2540a-140">When you're finished, click **OK**.</span></span>

![Připojte prázdný disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


<span data-ttu-id="2540a-142">Tento postup přidá jeden prázdný disk do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2540a-142">This adds one empty disk into your virtual machine.</span></span> <span data-ttu-id="2540a-143">Opakujte tento krok tři vícekrát, aby měli čtyři datových disků RAID.</span><span class="sxs-lookup"><span data-stu-id="2540a-143">Repeat this step three more times so that you have four data disks for RAID.</span></span>  

<span data-ttu-id="2540a-144">Přidání jednotky ve virtuálním počítači zobrazíte prohlížení protokolů zpráv jádra.</span><span class="sxs-lookup"><span data-stu-id="2540a-144">You can see the added drives in the virtual machine by looking at the kernel message log.</span></span> <span data-ttu-id="2540a-145">Například je vidět na Ubuntu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-145">For example, to see this on Ubuntu, use the following command:</span></span>  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a><span data-ttu-id="2540a-146">Vytvoření RAID pomocí dalších disků.</span><span class="sxs-lookup"><span data-stu-id="2540a-146">Create RAID with the additional disks</span></span>
<span data-ttu-id="2540a-147">Následující kroky popisují postup [konfigurace softwaru diskového pole RAID v systému Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2540a-147">The following steps describe how to [configure software RAID on Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="2540a-148">Pokud používáte systém souborů XFS, provést následující kroky po vytvoření RAID.</span><span class="sxs-lookup"><span data-stu-id="2540a-148">If you are using the XFS file system, execute the following steps after you have created RAID.</span></span>
>
>

<span data-ttu-id="2540a-149">K instalaci XFS na Debian a Ubuntu a Linux máta, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-149">To install XFS on Debian, Ubuntu, or Linux Mint, use the following command:</span></span>  

    apt-get -y install xfsprogs  

<span data-ttu-id="2540a-150">Nainstalovat XFS Fedora, CentOS nebo RHEL, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-150">To install XFS on Fedora, CentOS, or RHEL, use the following command:</span></span>  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a><span data-ttu-id="2540a-151">Nastavit novou cestu úložiště</span><span class="sxs-lookup"><span data-stu-id="2540a-151">Set up a new storage path</span></span>
<span data-ttu-id="2540a-152">Nastavit nové cesty úložiště, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-152">Use the following command to set up a new storage path:</span></span>  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a><span data-ttu-id="2540a-153">Zkopírujte původní data na novou cestu úložiště</span><span class="sxs-lookup"><span data-stu-id="2540a-153">Copy the original data to the new storage path</span></span>
<span data-ttu-id="2540a-154">Ke zkopírování dat na novou cestu úložiště použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-154">Use the following command to copy data to the new storage path:</span></span>  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a><span data-ttu-id="2540a-155">Upravit oprávnění, můžete přístup MySQL (čtení a zápisu) datový disk</span><span class="sxs-lookup"><span data-stu-id="2540a-155">Modify permissions so MySQL can access (read and write) the data disk</span></span>
<span data-ttu-id="2540a-156">Použijte následující příkaz pro úpravu oprávnění:</span><span class="sxs-lookup"><span data-stu-id="2540a-156">Use the following command to modify permissions:</span></span>  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a><span data-ttu-id="2540a-157">Upravit algoritmus plánování diskové vstupně-výstupních operací</span><span class="sxs-lookup"><span data-stu-id="2540a-157">Adjust the disk I/O scheduling algorithm</span></span>
<span data-ttu-id="2540a-158">Linux implementuje čtyři typy vstupně-výstupních operací plánování algoritmů:</span><span class="sxs-lookup"><span data-stu-id="2540a-158">Linux implements four types of I/O scheduling algorithms:</span></span>  

* <span data-ttu-id="2540a-159">Nedojde k žádné akci algoritmus (ne operace)</span><span class="sxs-lookup"><span data-stu-id="2540a-159">NOOP algorithm (No Operation)</span></span>
* <span data-ttu-id="2540a-160">Algoritmus konečného termínu (termín)</span><span class="sxs-lookup"><span data-stu-id="2540a-160">Deadline algorithm (Deadline)</span></span>
* <span data-ttu-id="2540a-161">Úplně správného algoritmu front zpráv (CFQ)</span><span class="sxs-lookup"><span data-stu-id="2540a-161">Completely fair queuing algorithm (CFQ)</span></span>
* <span data-ttu-id="2540a-162">Nároky období algoritmus (Anticipatory)</span><span class="sxs-lookup"><span data-stu-id="2540a-162">Budget period algorithm (Anticipatory)</span></span>  

<span data-ttu-id="2540a-163">Můžete vybrat jiný plánovače vstupně-výstupních operací v různých scénářích za účelem optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="2540a-163">You can select different I/O schedulers under different scenarios to optimize performance.</span></span> <span data-ttu-id="2540a-164">V prostředí s úplně náhodný přístup není velký rozdíl mezi algoritmy CFQ a konečný termín pro výkon.</span><span class="sxs-lookup"><span data-stu-id="2540a-164">In a completely random access environment, there is not a significant difference between the CFQ and Deadline algorithms for performance.</span></span> <span data-ttu-id="2540a-165">Doporučujeme nastavit prostředí databáze MySQL na konečný termín pro stabilitu.</span><span class="sxs-lookup"><span data-stu-id="2540a-165">We recommend that you set the MySQL database environment to Deadline for stability.</span></span> <span data-ttu-id="2540a-166">Pokud existuje mnoho sekvenčních vstupně-výstupních operací, CFQ může snížit výkon vstupně-výstupní operace disku.</span><span class="sxs-lookup"><span data-stu-id="2540a-166">If there is a lot of sequential I/O, CFQ might reduce disk I/O performance.</span></span>   

<span data-ttu-id="2540a-167">Pro SSD a dalších zařízení nedojde k žádné akci nebo konečný termín můžete dosáhnout lepší výkon než výchozí plánovače.</span><span class="sxs-lookup"><span data-stu-id="2540a-167">For SSD and other equipment, NOOP or Deadline can achieve better performance than the default scheduler.</span></span>   

<span data-ttu-id="2540a-168">Před jádra 2.5 výchozí algoritmus plánování vstupně-výstupních operací je konečný termín.</span><span class="sxs-lookup"><span data-stu-id="2540a-168">Prior to the kernel 2.5, the default I/O scheduling algorithm is Deadline.</span></span> <span data-ttu-id="2540a-169">Počínaje jádra 2.6.18, CFQ stala výchozí algoritmus plánování vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="2540a-169">Starting with the kernel 2.6.18, CFQ became the default I/O scheduling algorithm.</span></span>  <span data-ttu-id="2540a-170">Můžete určit toto nastavení při spuštění jádra nebo dynamicky upravit toto nastavení při spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="2540a-170">You can specify this setting at kernel boot time or dynamically modify this setting when the system is running.</span></span>  

<span data-ttu-id="2540a-171">Následující příklad ukazuje, jak zkontrolovat a nastavit výchozí plánovač na řady Debian distribuční algoritmus nedojde k žádné akci.</span><span class="sxs-lookup"><span data-stu-id="2540a-171">The following example demonstrates how to check and set the default scheduler to the NOOP algorithm in the Debian distribution family.</span></span>  

### <a name="view-the-current-io-scheduler"></a><span data-ttu-id="2540a-172">Zobrazení aktuálního plánovače vstupně-výstupních operací</span><span class="sxs-lookup"><span data-stu-id="2540a-172">View the current I/O scheduler</span></span>
<span data-ttu-id="2540a-173">Chcete-li zobrazit Plánovač spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-173">To view the scheduler run the following command:</span></span>  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

<span data-ttu-id="2540a-174">Zobrazí se následující výstup, který označuje aktuálního plánovače:</span><span class="sxs-lookup"><span data-stu-id="2540a-174">You will see following output, which indicates the current scheduler:</span></span>  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a><span data-ttu-id="2540a-175">Změňte aktuální zařízení (/ dev/sda) plánování algoritmu vstupně-výstupních operací</span><span class="sxs-lookup"><span data-stu-id="2540a-175">Change the current device (/dev/sda) of the I/O scheduling algorithm</span></span>
<span data-ttu-id="2540a-176">Spusťte následující příkazy a změňte aktuální zařízení:</span><span class="sxs-lookup"><span data-stu-id="2540a-176">Run the following commands to change the current device:</span></span>  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> <span data-ttu-id="2540a-177">Nastavení to samostatně/dev/sda není užitečné.</span><span class="sxs-lookup"><span data-stu-id="2540a-177">Setting this for /dev/sda alone is not useful.</span></span> <span data-ttu-id="2540a-178">Je nutné ji nastavit na všech discích data kde je umístěna databáze.</span><span class="sxs-lookup"><span data-stu-id="2540a-178">It must be set on all data disks where the database resides.</span></span>  
>
>

<span data-ttu-id="2540a-179">Měli byste vidět následující výstup, oznamující, že tento grub.cfg byla znovu sestavena úspěšně a že plánovač výchozí se aktualizovalo a nedojde k žádné akci:</span><span class="sxs-lookup"><span data-stu-id="2540a-179">You should see the following output, indicating that grub.cfg has been rebuilt successfully and that the default scheduler has been updated to NOOP:</span></span>  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

<span data-ttu-id="2540a-180">Pro řadu distribuční Red Hat potřebujete jenom následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2540a-180">For the Red Hat distribution family, you need only the following command:</span></span>

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a><span data-ttu-id="2540a-181">Konfigurace nastavení operace systému souborů</span><span class="sxs-lookup"><span data-stu-id="2540a-181">Configure system file operations settings</span></span>
<span data-ttu-id="2540a-182">Jeden osvědčeným postupem je zakázat *atime* funkce protokolování v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="2540a-182">One best practice is to disable the *atime* logging feature on the file system.</span></span> <span data-ttu-id="2540a-183">Atime je čas posledního přístupu k souboru.</span><span class="sxs-lookup"><span data-stu-id="2540a-183">Atime is the last file access time.</span></span> <span data-ttu-id="2540a-184">Vždy, když je přístup k souboru, systém souborů zaznamenává časové razítko v protokolu.</span><span class="sxs-lookup"><span data-stu-id="2540a-184">Whenever a file is accessed, the file system records the timestamp in the log.</span></span> <span data-ttu-id="2540a-185">Tyto informace se ale zřídka používá.</span><span class="sxs-lookup"><span data-stu-id="2540a-185">However, this information is rarely used.</span></span> <span data-ttu-id="2540a-186">Ji můžete vypnout, pokud tomu tak není, které se sníží celkový čas přístup k disku.</span><span class="sxs-lookup"><span data-stu-id="2540a-186">You can disable it if you don't need it, which will reduce overall disk access time.</span></span>  

<span data-ttu-id="2540a-187">Zakázat atime protokolování, budete muset upravit soubor system configuration soubor/etc / fstab a přidat **noatime** možnost.</span><span class="sxs-lookup"><span data-stu-id="2540a-187">To disable atime logging, you need to modify the file system configuration file /etc/ fstab and add the **noatime** option.</span></span>  

<span data-ttu-id="2540a-188">Můžete třeba upravte soubor /etc/fstab vim přidáním noatime, jak znázorňuje následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="2540a-188">For example, edit the vim /etc/fstab file, adding the noatime as shown in the following sample:</span></span>  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

<span data-ttu-id="2540a-189">Potom se znovu připojte systém souborů pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2540a-189">Then, remount the file system with the following command:</span></span>  

    mount -o remount /RAID0

<span data-ttu-id="2540a-190">Otestujte upravené výsledek.</span><span class="sxs-lookup"><span data-stu-id="2540a-190">Test the modified result.</span></span> <span data-ttu-id="2540a-191">Když upravíte testovací soubor, čas přístupu se neaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="2540a-191">When you modify the test file, the access time is not updated.</span></span> <span data-ttu-id="2540a-192">Následující příklady ukazují, jak kód vypadá před a po změnách.</span><span class="sxs-lookup"><span data-stu-id="2540a-192">The following examples show what the code looks like before and after modification.</span></span>

<span data-ttu-id="2540a-193">Před:</span><span class="sxs-lookup"><span data-stu-id="2540a-193">Before:</span></span>        

![Před úpravou přístupu kódu][5]

<span data-ttu-id="2540a-195">Po:</span><span class="sxs-lookup"><span data-stu-id="2540a-195">After:</span></span>

![Po změnách přístupu kódu][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a><span data-ttu-id="2540a-197">Zvýšit maximální počet popisovačů systému pro vysokou souběžnosti</span><span class="sxs-lookup"><span data-stu-id="2540a-197">Increase the maximum number of system handles for high concurrency</span></span>
<span data-ttu-id="2540a-198">MySQL je vysoká souběžnosti databáze.</span><span class="sxs-lookup"><span data-stu-id="2540a-198">MySQL is a high concurrency database.</span></span> <span data-ttu-id="2540a-199">Výchozí počet souběžných obslužné rutiny je 1024 pro Linux, což není vždy dostatečná.</span><span class="sxs-lookup"><span data-stu-id="2540a-199">The default number of concurrent handles is 1024 for Linux, which is not always sufficient.</span></span> <span data-ttu-id="2540a-200">Pomocí následujících kroků zvýšit maximální souběžných popisovačů systému pro podporu vysoké souběžnosti MySQL.</span><span class="sxs-lookup"><span data-stu-id="2540a-200">Use the following steps to increase the maximum concurrent handles of the system to support high concurrency of MySQL.</span></span>

### <a name="modify-the-limitsconf-file"></a><span data-ttu-id="2540a-201">Upravte soubor limits.conf</span><span class="sxs-lookup"><span data-stu-id="2540a-201">Modify the limits.conf file</span></span>
<span data-ttu-id="2540a-202">Pokud chcete zvýšit maximální povolené souběžných obslužných rutin, přidejte následující čtyři řádky v souboru /etc/security/limits.conf.</span><span class="sxs-lookup"><span data-stu-id="2540a-202">To increase the maximum allowed concurrent handles, add the following four lines in the /etc/security/limits.conf file.</span></span> <span data-ttu-id="2540a-203">Všimněte si, že 65536 je maximální počet, který podporuje systém.</span><span class="sxs-lookup"><span data-stu-id="2540a-203">Note that 65536 is the maximum number that the system can support.</span></span>   

    * <span data-ttu-id="2540a-204">logicky nofile 65536</span><span class="sxs-lookup"><span data-stu-id="2540a-204">soft nofile 65536</span></span>
    * <span data-ttu-id="2540a-205">pevné nofile 65536</span><span class="sxs-lookup"><span data-stu-id="2540a-205">hard nofile 65536</span></span>
    * <span data-ttu-id="2540a-206">logicky nproc 65536</span><span class="sxs-lookup"><span data-stu-id="2540a-206">soft nproc 65536</span></span>
    * <span data-ttu-id="2540a-207">pevné nproc 65536</span><span class="sxs-lookup"><span data-stu-id="2540a-207">hard nproc 65536</span></span>

### <a name="update-the-system-for-the-new-limits"></a><span data-ttu-id="2540a-208">Aktualizujte systém na nový limity</span><span class="sxs-lookup"><span data-stu-id="2540a-208">Update the system for the new limits</span></span>
<span data-ttu-id="2540a-209">Chcete-li aktualizovat systém, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2540a-209">To update the system, run the following commands:</span></span>  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a><span data-ttu-id="2540a-210">Ujistěte se, že omezení jsou aktualizovány při spuštění</span><span class="sxs-lookup"><span data-stu-id="2540a-210">Ensure that the limits are updated at boot time</span></span>
<span data-ttu-id="2540a-211">Vložte následující příkazy spuštění souboru /etc/rc.local tak projeví při spuštění.</span><span class="sxs-lookup"><span data-stu-id="2540a-211">Put the following startup commands in the /etc/rc.local file so it will take effect at boot time.</span></span>  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a><span data-ttu-id="2540a-212">Optimalizace databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="2540a-212">MySQL database optimization</span></span>
<span data-ttu-id="2540a-213">Ke konfiguraci databáze MySQL na Azure, můžete použít stejné strategie optimalizace výkonu, který používáte v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="2540a-213">To configure MySQL on Azure, you can use the same performance-tuning strategy you use on an on-premises machine.</span></span>  

<span data-ttu-id="2540a-214">Hlavní pravidla optimalizace vstupně-výstupní operace jsou:</span><span class="sxs-lookup"><span data-stu-id="2540a-214">The main I/O optimization rules are:</span></span>   

* <span data-ttu-id="2540a-215">Zvětšete velikost mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="2540a-215">Increase the cache size.</span></span>
* <span data-ttu-id="2540a-216">Snížení doby odezvy vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="2540a-216">Reduce I/O response time.</span></span>  

<span data-ttu-id="2540a-217">Chcete-li optimalizovat nastavení serveru MySQL, můžete aktualizovat my.cnf souboru, který je výchozí konfigurační soubor pro server a klientských počítačů.</span><span class="sxs-lookup"><span data-stu-id="2540a-217">To optimize MySQL server settings, you can update the my.cnf file, which is the default configuration file for both server and client computers.</span></span>  

<span data-ttu-id="2540a-218">Hlavní faktory, které ovlivňují výkon MySQL jsou následující položky konfigurace:</span><span class="sxs-lookup"><span data-stu-id="2540a-218">The following configuration items are the main factors that affect MySQL performance:</span></span>  

* <span data-ttu-id="2540a-219">**innodb_buffer_pool_size**: fondu vyrovnávací paměti obsahuje data ve vyrovnávací paměti a index.</span><span class="sxs-lookup"><span data-stu-id="2540a-219">**innodb_buffer_pool_size**: The buffer pool contains buffered data and the index.</span></span> <span data-ttu-id="2540a-220">To je obvykle nastavena na 70 procent fyzické paměti.</span><span class="sxs-lookup"><span data-stu-id="2540a-220">This is usually set to 70 percent of physical memory.</span></span>
* <span data-ttu-id="2540a-221">**innodb_log_file_size**: Toto je velikost protokolu operaci znovu.</span><span class="sxs-lookup"><span data-stu-id="2540a-221">**innodb_log_file_size**: This is the redo log size.</span></span> <span data-ttu-id="2540a-222">Opakování protokoly se použít k zajištění, že operace zápisu jsou rychlé, spolehlivé a použitelná pro obnovení po chybě.</span><span class="sxs-lookup"><span data-stu-id="2540a-222">You use redo logs to ensure that write operations are fast, reliable, and recoverable after a crash.</span></span> <span data-ttu-id="2540a-223">Je nastavena na 512 MB, který vám poskytne dostatek místa pro protokolování operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="2540a-223">This is set to 512 MB, which will give you plenty of space for logging write operations.</span></span>
* <span data-ttu-id="2540a-224">**max_connections**: v některých případech aplikace neukončujte připojení správně.</span><span class="sxs-lookup"><span data-stu-id="2540a-224">**max_connections**: Sometimes applications do not close connections properly.</span></span> <span data-ttu-id="2540a-225">Větší hodnotu získáte víc času recyklace nečinný připojení serveru.</span><span class="sxs-lookup"><span data-stu-id="2540a-225">A larger value will give the server more time to recycle idled connections.</span></span> <span data-ttu-id="2540a-226">Maximální počet připojení je 10 000, ale maximální Doporučená hodnota je 5 000.</span><span class="sxs-lookup"><span data-stu-id="2540a-226">The maximum number of connections is 10,000, but the recommended maximum is 5,000.</span></span>
* <span data-ttu-id="2540a-227">**Innodb_file_per_table**: Toto nastavení povolí nebo zakáže schopnost InnoDB ukládání tabulek v samostatné soubory.</span><span class="sxs-lookup"><span data-stu-id="2540a-227">**Innodb_file_per_table**: This setting enables or disables the ability of InnoDB to store tables in separate files.</span></span> <span data-ttu-id="2540a-228">Zapněte možnost zajistit, že několik operací pokročilé správy může být použitá efektivně.</span><span class="sxs-lookup"><span data-stu-id="2540a-228">Turn on the option to ensure that several advanced administration operations can be applied efficiently.</span></span> <span data-ttu-id="2540a-229">Z výkonu hlediska může urychlit přenos místo tabulky a optimalizace výkonu správy zbytků.</span><span class="sxs-lookup"><span data-stu-id="2540a-229">From a performance point of view, it can speed up the table space transmission and optimize the debris management performance.</span></span> <span data-ttu-id="2540a-230">Doporučené nastavení pro tuto možnost je ON.</span><span class="sxs-lookup"><span data-stu-id="2540a-230">The recommended setting for this option is ON.</span></span></br></br>
<span data-ttu-id="2540a-231">Z databáze MySQL 5.6 výchozí nastavení je ON, takže není vyžadována žádná akce.</span><span class="sxs-lookup"><span data-stu-id="2540a-231">From MySQL 5.6, the default setting is ON, so no action is required.</span></span> <span data-ttu-id="2540a-232">U starších verzí je ve výchozím nastavení VYPNUTÝ.</span><span class="sxs-lookup"><span data-stu-id="2540a-232">For earlier versions, the default setting is OFF.</span></span> <span data-ttu-id="2540a-233">Nastavení by měl změnit před načtením dat, protože to ovlivňuje pouze nově vytvořené tabulky.</span><span class="sxs-lookup"><span data-stu-id="2540a-233">The setting should be changed before data is loaded, because only newly created tables are affected.</span></span>
* <span data-ttu-id="2540a-234">**innodb_flush_log_at_trx_commit**: výchozí hodnota je 1, spolu s rozsahem nastaven na hodnotu 0 ~ 2.</span><span class="sxs-lookup"><span data-stu-id="2540a-234">**innodb_flush_log_at_trx_commit**: The default value is 1, with the scope set to 0~2.</span></span> <span data-ttu-id="2540a-235">Výchozí hodnota je nejvhodnější možnost pro samostatnou databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="2540a-235">The default value is the most suitable option for standalone MySQL DB.</span></span> <span data-ttu-id="2540a-236">Nastavení 2 umožňuje většinu integritu dat a je vhodný pro hlavní server v clusteru MySQL.</span><span class="sxs-lookup"><span data-stu-id="2540a-236">The setting of 2 enables the most data integrity and is suitable for Master in MySQL Cluster.</span></span> <span data-ttu-id="2540a-237">Nastavení 0 umožňuje ztrátě dat, která může mít vliv na spolehlivost (v některých případech s lepším výkonem) a je vhodný pro podřízený v clusteru MySQL.</span><span class="sxs-lookup"><span data-stu-id="2540a-237">The setting of 0 allows data loss, which can affect reliability (in some cases with better performance), and is suitable for Slave in MySQL Cluster.</span></span>
* <span data-ttu-id="2540a-238">**Innodb_log_buffer_size**: umožňuje vyrovnávací paměť protokolu transakcí spustit bez nutnosti před potvrzení transakce jsou zapsány disku v protokolu.</span><span class="sxs-lookup"><span data-stu-id="2540a-238">**Innodb_log_buffer_size**: The log buffer allows transactions to run without having to flush the log to disk before the transactions commit.</span></span> <span data-ttu-id="2540a-239">Pokud je binární rozsáhlý objekt nebo textové pole, mezipaměti využijí rychle a aktivuje se často diskové vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="2540a-239">However, if there is large binary object or text field, the cache will be consumed quickly and frequent disk I/O will be triggered.</span></span> <span data-ttu-id="2540a-240">Pokud proměnné stavu Innodb_log_waits není lépe zvětšete velikost vyrovnávací paměti je 0.</span><span class="sxs-lookup"><span data-stu-id="2540a-240">It is better increase the buffer size if Innodb_log_waits state variable is not 0.</span></span>
* <span data-ttu-id="2540a-241">**query_cache_size**: nejlepší možnost je zakázat od samého počátku.</span><span class="sxs-lookup"><span data-stu-id="2540a-241">**query_cache_size**: The best option is to disable it from the outset.</span></span> <span data-ttu-id="2540a-242">Nastavte query_cache_size na hodnotu 0 (Toto je výchozí nastavení v MySQL 5.6) a použít jiné metody pro urychlení dotazů.</span><span class="sxs-lookup"><span data-stu-id="2540a-242">Set query_cache_size to 0 (this is the default setting in MySQL 5.6) and use other methods to speed up queries.</span></span>  

<span data-ttu-id="2540a-243">V tématu [Dodatek D](#AppendixD) porovnání před a po optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="2540a-243">See [Appendix D](#AppendixD) for a comparison of performance before and after the optimization.</span></span>

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a><span data-ttu-id="2540a-244">Zapnout protokol pomalé dotazu MySQL pro analýzu kritická místa výkonu</span><span class="sxs-lookup"><span data-stu-id="2540a-244">Turn on the MySQL slow query log for analyzing the performance bottleneck</span></span>
<span data-ttu-id="2540a-245">Protokol dotazu pomalé MySQL můžete identifikovat pomalé dotazů pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="2540a-245">The MySQL slow query log can help you identify the slow queries for MySQL.</span></span> <span data-ttu-id="2540a-246">Když povolíte protokol pomalé dotazu MySQL, můžete použít nástroje MySQL jako **mysqldumpslow** identifikovat kritická místa výkonu.</span><span class="sxs-lookup"><span data-stu-id="2540a-246">After enabling the MySQL slow query log, you can use MySQL tools like **mysqldumpslow** to identify the performance bottleneck.</span></span>  

<span data-ttu-id="2540a-247">Ve výchozím nastavení to není povoleno.</span><span class="sxs-lookup"><span data-stu-id="2540a-247">By default, this is not enabled.</span></span> <span data-ttu-id="2540a-248">Zapnutí protokol pomalé dotazu může využívat některé prostředky procesoru.</span><span class="sxs-lookup"><span data-stu-id="2540a-248">Turning on the slow query log might consume some CPU resources.</span></span> <span data-ttu-id="2540a-249">Doporučujeme, abyste povolili to dočasně pro řešení potíží s kritické body.</span><span class="sxs-lookup"><span data-stu-id="2540a-249">We recommend that you enable this temporarily for troubleshooting performance bottlenecks.</span></span> <span data-ttu-id="2540a-250">Chcete-li na protokol pomalé dotazu:</span><span class="sxs-lookup"><span data-stu-id="2540a-250">To turn on the slow query log:</span></span>

1. <span data-ttu-id="2540a-251">Upravte soubor my.cnf přidáním následující řádky na konec:</span><span class="sxs-lookup"><span data-stu-id="2540a-251">Modify the my.cnf file by adding the following lines to the end:</span></span>

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. <span data-ttu-id="2540a-252">Restartujte server, MySQL.</span><span class="sxs-lookup"><span data-stu-id="2540a-252">Restart the MySQL server.</span></span>

        service  mysql  restart

3. <span data-ttu-id="2540a-253">Zkontrolujte, zda nastavení trvá vliv pomocí **zobrazit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="2540a-253">Check whether the setting is taking effect by using the **show** command.</span></span>

![ON zpomalit protokol dotazu][7]   

![Výsledky zpomalit protokol dotazu][8]

<span data-ttu-id="2540a-256">V tomto příkladu vidíte, že byla zapnuta funkce pomalé dotazu.</span><span class="sxs-lookup"><span data-stu-id="2540a-256">In this example, you can see that the slow query feature has been turned on.</span></span> <span data-ttu-id="2540a-257">Pak můžete použít **mysqldumpslow** nástroj zjistit kritická místa výkonu a optimalizace výkonu, jako je například přidávání indexy.</span><span class="sxs-lookup"><span data-stu-id="2540a-257">You can then use the **mysqldumpslow** tool to determine performance bottlenecks and optimize performance, such as adding indexes.</span></span>

## <a name="appendices"></a><span data-ttu-id="2540a-258">Přílohy</span><span class="sxs-lookup"><span data-stu-id="2540a-258">Appendices</span></span>
<span data-ttu-id="2540a-259">Následuje ukázková výkonu testovací data vytvořeného v cílové testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2540a-259">The following are sample performance test data produced in a targeted lab environment.</span></span> <span data-ttu-id="2540a-260">Poskytují obecné na trend data výkonu s jinou ladění přístupy výkonu.</span><span class="sxs-lookup"><span data-stu-id="2540a-260">They provide general background on the performance data trend with different performance tuning approaches.</span></span> <span data-ttu-id="2540a-261">Výsledky se můžou lišit v rámci různých verzí prostředí nebo produktu.</span><span class="sxs-lookup"><span data-stu-id="2540a-261">The results might vary under different environment or product versions.</span></span>

### <span data-ttu-id="2540a-262"><a name="AppendixA"></a>Příloha A</span><span class="sxs-lookup"><span data-stu-id="2540a-262"><a name="AppendixA"></a>Appendix A</span></span>  
<span data-ttu-id="2540a-263">**Výkon disku (IOPS) s různými úrovněmi diskového pole RAID**</span><span class="sxs-lookup"><span data-stu-id="2540a-263">**Disk performance (IOPS) with different RAID levels**</span></span>

![Disk IOPS s různými úrovněmi diskového pole RAID][9]

<span data-ttu-id="2540a-265">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="2540a-265">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> <span data-ttu-id="2540a-266">Zatížení tento test používá 64 vláken, dostat horní limit počtu RAID.</span><span class="sxs-lookup"><span data-stu-id="2540a-266">The workload of this test uses 64 threads, trying to reach the upper limit of RAID.</span></span>
>
>

### <span data-ttu-id="2540a-267"><a name="AppendixB"></a>Dodatek B</span><span class="sxs-lookup"><span data-stu-id="2540a-267"><a name="AppendixB"></a>Appendix B</span></span>  
<span data-ttu-id="2540a-268">**Porovnání výkonu (propustnost) MySQL s různými úrovněmi diskového pole RAID** </span><span class="sxs-lookup"><span data-stu-id="2540a-268">**MySQL performance (throughput) comparison with different RAID levels** </span></span>  
<span data-ttu-id="2540a-269">(Systém souborů XFS)</span><span class="sxs-lookup"><span data-stu-id="2540a-269">(XFS file system)</span></span>

![Porovnání výkonu MySQL s různými úrovněmi diskového pole RAID][10]  
![Porovnání výkonu MySQL s různými úrovněmi diskového pole RAID][11]

<span data-ttu-id="2540a-272">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="2540a-272">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

<span data-ttu-id="2540a-273">**Porovnání výkonu (OLTP) MySQL s různými úrovněmi diskového pole RAID**</span><span class="sxs-lookup"><span data-stu-id="2540a-273">**MySQL performance (OLTP) comparison with different RAID levels**</span></span>  
<span data-ttu-id="2540a-274">![Porovnání výkonu (OLTP) MySQL s různými úrovněmi diskového pole RAID][12]</span><span class="sxs-lookup"><span data-stu-id="2540a-274">![MySQL performance (OLTP) comparison with different RAID levels][12]</span></span>

<span data-ttu-id="2540a-275">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="2540a-275">**Test commands**</span></span>

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <span data-ttu-id="2540a-276"><a name="AppendixC"></a>Příloha C</span><span class="sxs-lookup"><span data-stu-id="2540a-276"><a name="AppendixC"></a>Appendix C</span></span>   
<span data-ttu-id="2540a-277">**Porovnání výkonu (IOPS) disku pro různé bloku velikosti**</span><span class="sxs-lookup"><span data-stu-id="2540a-277">**Disk performance (IOPS) comparison for different chunk sizes**</span></span>  
<span data-ttu-id="2540a-278">(Systém souborů XFS)</span><span class="sxs-lookup"><span data-stu-id="2540a-278">(XFS file system)</span></span>

![][13]

<span data-ttu-id="2540a-279">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="2540a-279">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

<span data-ttu-id="2540a-280">Velikosti souborů použít pro toto testování 30 GB 1 GB, v uvedeném pořadí a s RAID 0 (4 disky) XFS systému souborů.</span><span class="sxs-lookup"><span data-stu-id="2540a-280">The file sizes used for this testing are 30 GB and 1 GB, respectively, with RAID 0 (4 disks) XFS file system.</span></span>

### <span data-ttu-id="2540a-281"><a name="AppendixD"></a>Dodatek D</span><span class="sxs-lookup"><span data-stu-id="2540a-281"><a name="AppendixD"></a>Appendix D</span></span>  
<span data-ttu-id="2540a-282">**Porovnání výkonu (propustnost) MySQL před a po optimalizace**</span><span class="sxs-lookup"><span data-stu-id="2540a-282">**MySQL performance (throughput) comparison before and after optimization**</span></span>  
<span data-ttu-id="2540a-283">(Systém souborů XFS)</span><span class="sxs-lookup"><span data-stu-id="2540a-283">(XFS File System)</span></span>

![Porovnání výkonu (propustnost) MySQL před a po optimalizace][14]

<span data-ttu-id="2540a-285">**Test příkazy**</span><span class="sxs-lookup"><span data-stu-id="2540a-285">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

<span data-ttu-id="2540a-286">**Nastavení konfigurace pro výchozí a optimalizace vypadá takto:**</span><span class="sxs-lookup"><span data-stu-id="2540a-286">**The configuration setting for default and optimization is as follows:**</span></span>

| <span data-ttu-id="2540a-287">Parametry</span><span class="sxs-lookup"><span data-stu-id="2540a-287">Parameters</span></span> | <span data-ttu-id="2540a-288">Výchozí</span><span class="sxs-lookup"><span data-stu-id="2540a-288">Default</span></span> | <span data-ttu-id="2540a-289">Optimalizace</span><span class="sxs-lookup"><span data-stu-id="2540a-289">Optimization</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2540a-290">**innodb_buffer_pool_size**</span><span class="sxs-lookup"><span data-stu-id="2540a-290">**innodb_buffer_pool_size**</span></span> |<span data-ttu-id="2540a-291">Žádný</span><span class="sxs-lookup"><span data-stu-id="2540a-291">None</span></span> |<span data-ttu-id="2540a-292">7 GB</span><span class="sxs-lookup"><span data-stu-id="2540a-292">7 GB</span></span> |
| <span data-ttu-id="2540a-293">**innodb_log_file_size**</span><span class="sxs-lookup"><span data-stu-id="2540a-293">**innodb_log_file_size**</span></span> |<span data-ttu-id="2540a-294">5 MB.</span><span class="sxs-lookup"><span data-stu-id="2540a-294">5 MB</span></span> |<span data-ttu-id="2540a-295">512 MB</span><span class="sxs-lookup"><span data-stu-id="2540a-295">512 MB</span></span> |
| <span data-ttu-id="2540a-296">**max_connections**</span><span class="sxs-lookup"><span data-stu-id="2540a-296">**max_connections**</span></span> |<span data-ttu-id="2540a-297">100</span><span class="sxs-lookup"><span data-stu-id="2540a-297">100</span></span> |<span data-ttu-id="2540a-298">5000</span><span class="sxs-lookup"><span data-stu-id="2540a-298">5000</span></span> |
| <span data-ttu-id="2540a-299">**innodb_file_per_table**</span><span class="sxs-lookup"><span data-stu-id="2540a-299">**innodb_file_per_table**</span></span> |<span data-ttu-id="2540a-300">0</span><span class="sxs-lookup"><span data-stu-id="2540a-300">0</span></span> |<span data-ttu-id="2540a-301">1</span><span class="sxs-lookup"><span data-stu-id="2540a-301">1</span></span> |
| <span data-ttu-id="2540a-302">**innodb_flush_log_at_trx_commit**</span><span class="sxs-lookup"><span data-stu-id="2540a-302">**innodb_flush_log_at_trx_commit**</span></span> |<span data-ttu-id="2540a-303">1</span><span class="sxs-lookup"><span data-stu-id="2540a-303">1</span></span> |<span data-ttu-id="2540a-304">2</span><span class="sxs-lookup"><span data-stu-id="2540a-304">2</span></span> |
| <span data-ttu-id="2540a-305">**innodb_log_buffer_size**</span><span class="sxs-lookup"><span data-stu-id="2540a-305">**innodb_log_buffer_size**</span></span> |<span data-ttu-id="2540a-306">8 MB</span><span class="sxs-lookup"><span data-stu-id="2540a-306">8 MB</span></span> |<span data-ttu-id="2540a-307">128 MB</span><span class="sxs-lookup"><span data-stu-id="2540a-307">128 MB</span></span> |
| <span data-ttu-id="2540a-308">**query_cache_size**</span><span class="sxs-lookup"><span data-stu-id="2540a-308">**query_cache_size**</span></span> |<span data-ttu-id="2540a-309">16 MB</span><span class="sxs-lookup"><span data-stu-id="2540a-309">16 MB</span></span> |<span data-ttu-id="2540a-310">0</span><span class="sxs-lookup"><span data-stu-id="2540a-310">0</span></span> |

<span data-ttu-id="2540a-311">Další podrobné [parametry konfigurace optimalizace](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), naleznete [oficiální pokyny MySQL](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span><span class="sxs-lookup"><span data-stu-id="2540a-311">For more detailed [optimization configuration parameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), refer to the [MySQL official instructions](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span></span>  

  <span data-ttu-id="2540a-312">**Testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="2540a-312">**Test environment**</span></span>  

| <span data-ttu-id="2540a-313">Hardware</span><span class="sxs-lookup"><span data-stu-id="2540a-313">Hardware</span></span> | <span data-ttu-id="2540a-314">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="2540a-314">Details</span></span> |
| --- | --- |
| <span data-ttu-id="2540a-315">Procesor</span><span class="sxs-lookup"><span data-stu-id="2540a-315">CPU</span></span> |<span data-ttu-id="2540a-316">AMD Opteron(tm) procesoru 4171 HE / 4 jádra</span><span class="sxs-lookup"><span data-stu-id="2540a-316">AMD Opteron(tm) Processor 4171 HE/4 cores</span></span> |
| <span data-ttu-id="2540a-317">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="2540a-317">Memory</span></span> |<span data-ttu-id="2540a-318">14 GB</span><span class="sxs-lookup"><span data-stu-id="2540a-318">14 GB</span></span> |
| <span data-ttu-id="2540a-319">Disk</span><span class="sxs-lookup"><span data-stu-id="2540a-319">Disk</span></span> |<span data-ttu-id="2540a-320">10 GB místa na disku</span><span class="sxs-lookup"><span data-stu-id="2540a-320">10 GB/disk</span></span> |
| <span data-ttu-id="2540a-321">Operační systém</span><span class="sxs-lookup"><span data-stu-id="2540a-321">OS</span></span> |<span data-ttu-id="2540a-322">Ubuntu 14.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="2540a-322">Ubuntu 14.04.1 LTS</span></span> |

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

