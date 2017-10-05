---
title: "Clusterize MySQL s vyrovnáváním zatížení sad | Microsoft Docs"
description: "Nastavení pro zařízení s vyrovnáváním zatížení, vysokou dostupnost clusteru Linux MySQL vytvořené pomocí modelu nasazení classic na platformě Azure"
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 4eaf86c9ac3e4dc2b51b88383626eda774cab0e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-load-balanced-sets-to-clusterize-mysql-on-linux"></a><span data-ttu-id="f8271-103">Pomocí vyrovnávání zatížení sítě nastaví clusterize MySQL v systému Linux</span><span class="sxs-lookup"><span data-stu-id="f8271-103">Use load-balanced sets to clusterize MySQL on Linux</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f8271-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="f8271-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="f8271-105">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="f8271-105">This article covers using the classic deployment model.</span></span> <span data-ttu-id="f8271-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f8271-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="f8271-107">A [šablony Resource Manageru](https://azure.microsoft.com/documentation/templates/mysql-replication/) je k dispozici, pokud je potřeba nasadit MySQL cluster.</span><span class="sxs-lookup"><span data-stu-id="f8271-107">A [Resource Manager template](https://azure.microsoft.com/documentation/templates/mysql-replication/) is available if you need to deploy a MySQL cluster.</span></span>

<span data-ttu-id="f8271-108">V tomto článku jsou zde popsány a ukazuje různý přístup k dispozici pro nasazení vysokou dostupností služby v Microsoft Azure, prohlížení vysoké dostupnosti serveru MySQL jako základy systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="f8271-108">This article explores and illustrates the different approaches available to deploy highly available Linux-based services on Microsoft Azure, exploring MySQL Server high availability as a primer.</span></span> <span data-ttu-id="f8271-109">Video ilustrující tento přístup je k dispozici na [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span><span class="sxs-lookup"><span data-stu-id="f8271-109">A video illustrating this approach is available on [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span></span>

<span data-ttu-id="f8271-110">Nemůžeme se popisují shared nothing, dvěma uzly, jeden hlavní řešení vysoké dostupnosti databáze MySQL na základě DRBD, Corosync a kardiostimulátor.</span><span class="sxs-lookup"><span data-stu-id="f8271-110">We will outline a shared-nothing, two-node, single-master MySQL high availability solution based on DRBD, Corosync, and Pacemaker.</span></span> <span data-ttu-id="f8271-111">Databáze MySQL byla najednou spuštěna pouze jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="f8271-111">Only one node runs MySQL at a time.</span></span> <span data-ttu-id="f8271-112">Čtení a zápis z prostředku DRBD je také omezen na jenom jeden uzel v čase.</span><span class="sxs-lookup"><span data-stu-id="f8271-112">Reading and writing from the DRBD resource is also limited to only one node at a time.</span></span>

<span data-ttu-id="f8271-113">Není nutné pro VIP řešení jako LVS, protože budete používat sady Vyrovnávání zatížení sítě v Microsoft Azure k zajištění funkcí a koncový bod zjišťování kruhového dotazování, odebrání a úspěšné obnovení virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f8271-113">There's no need for a VIP solution like LVS, because you'll use load-balanced sets in Microsoft Azure to provide round-robin functionality and endpoint detection, removal, and graceful recovery of the VIP.</span></span> <span data-ttu-id="f8271-114">Virtuální IP adresa je globálně směrovatelné adresu IPv4 přiřazené službou Microsoft Azure při prvním vytvoření cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="f8271-114">The VIP is a globally routable IPv4 address assigned by Microsoft Azure when you first create the cloud service.</span></span>

<span data-ttu-id="f8271-115">Nejsou k dispozici jako virtuální počítač na další možné architektury pro databázi MySQL, včetně NBD clusteru, Percona, Galera a několik řešení middleware, včetně alespoň jeden [skladu virtuálních počítačů](http://vmdepot.msopentech.com).</span><span class="sxs-lookup"><span data-stu-id="f8271-115">There are other possible architectures for MySQL, including NBD Cluster, Percona, Galera, and several middleware solutions, including at least one available as a VM on [VM Depot](http://vmdepot.msopentech.com).</span></span> <span data-ttu-id="f8271-116">Dokud tato řešení můžete replikovat na jednosměrové a vícesměrové vysílání nebo všesměrové vysílání a nejsou spoléhají na sdílené úložiště nebo více síťových rozhraní, musí být scénáře snadno nasadit v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8271-116">As long as these solutions can replicate on unicast vs. multicast or broadcast and don't rely on shared storage or multiple network interfaces, the scenarios should be easy to deploy on Microsoft Azure.</span></span>

<span data-ttu-id="f8271-117">Tyto clustering architektury lze rozšířit o další produkty jako PostgreSQL a OpenLDAP podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f8271-117">These clustering architectures can be extended to other products like PostgreSQL and OpenLDAP in a similar fashion.</span></span> <span data-ttu-id="f8271-118">Například tento postup Vyrovnávání zatížení s nesdílená byla úspěšně testována s více hlavní OpenLDAP a můžete ho sledovat na našem blogu Channel 9.</span><span class="sxs-lookup"><span data-stu-id="f8271-118">For example, this load-balancing procedure with shared nothing was successfully tested with multi-master OpenLDAP, and you can watch it on our Channel 9 blog.</span></span>

## <a name="get-ready"></a><span data-ttu-id="f8271-119">Příprava</span><span class="sxs-lookup"><span data-stu-id="f8271-119">Get ready</span></span>
<span data-ttu-id="f8271-120">Budete potřebovat následující prostředky a dalo:</span><span class="sxs-lookup"><span data-stu-id="f8271-120">You need the following resources and abilities:</span></span>

  - <span data-ttu-id="f8271-121">Účet Microsoft Azure s platným předplatným, moci vytvořit aspoň dva virtuální počítače (XS byl použit v tomto příkladu)</span><span class="sxs-lookup"><span data-stu-id="f8271-121">A Microsoft Azure account with a valid subscription, able to create at least two VMs (XS was used in this example)</span></span>
  - <span data-ttu-id="f8271-122">Síť a podsíť</span><span class="sxs-lookup"><span data-stu-id="f8271-122">A network and a subnet</span></span>
  - <span data-ttu-id="f8271-123">Skupiny vztahů</span><span class="sxs-lookup"><span data-stu-id="f8271-123">An affinity group</span></span>
  - <span data-ttu-id="f8271-124">Nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8271-124">An availability set</span></span>
  - <span data-ttu-id="f8271-125">Možnost vytvoříte virtuální pevné disky ve stejné oblasti jako cloudová služba a připojte je k virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f8271-125">The ability to create VHDs in the same region as the cloud service and attach them to the Linux VMs</span></span>

### <a name="tested-environment"></a><span data-ttu-id="f8271-126">Otestované prostředí</span><span class="sxs-lookup"><span data-stu-id="f8271-126">Tested environment</span></span>
* <span data-ttu-id="f8271-127">Ubuntu 13.10</span><span class="sxs-lookup"><span data-stu-id="f8271-127">Ubuntu 13.10</span></span>
  * <span data-ttu-id="f8271-128">DRBD</span><span class="sxs-lookup"><span data-stu-id="f8271-128">DRBD</span></span>
  * <span data-ttu-id="f8271-129">Server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="f8271-129">MySQL Server</span></span>
  * <span data-ttu-id="f8271-130">Corosync a kardiostimulátor</span><span class="sxs-lookup"><span data-stu-id="f8271-130">Corosync and Pacemaker</span></span>

### <a name="affinity-group"></a><span data-ttu-id="f8271-131">Skupina vztahů</span><span class="sxs-lookup"><span data-stu-id="f8271-131">Affinity group</span></span>
<span data-ttu-id="f8271-132">Vytvořit skupinu vztahů pro řešení po přihlášení k portálu Azure classic, vyberte **nastavení**a vytváření skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="f8271-132">Create an affinity group for the solution by signing in to the Azure classic portal, selecting **Settings**, and creating an affinity group.</span></span> <span data-ttu-id="f8271-133">Přidělené prostředky vytvořit později přiřadí do této skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="f8271-133">Allocated resources created later will be assigned to this affinity group.</span></span>

### <a name="networks"></a><span data-ttu-id="f8271-134">Networks</span><span class="sxs-lookup"><span data-stu-id="f8271-134">Networks</span></span>
<span data-ttu-id="f8271-135">Se vytvoří nová síť a podsíť je vytvořit uvnitř sítě.</span><span class="sxs-lookup"><span data-stu-id="f8271-135">A new network is created, and a subnet is created inside the network.</span></span> <span data-ttu-id="f8271-136">Tento příklad používá 10.10.10.0/24 síť s podsítí pouze jeden /24 uvnitř.</span><span class="sxs-lookup"><span data-stu-id="f8271-136">This example uses a 10.10.10.0/24 network with only one /24 subnet inside.</span></span>

### <a name="virtual-machines"></a><span data-ttu-id="f8271-137">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="f8271-137">Virtual machines</span></span>
<span data-ttu-id="f8271-138">První virtuálního počítače s Ubuntu 13.10 je vytvořená pomocí image Galerie Endorsed Ubuntu a se nazývá `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="f8271-138">The first Ubuntu 13.10 VM is created by using an Endorsed Ubuntu Gallery image and is called `hadb01`.</span></span> <span data-ttu-id="f8271-139">Nová Cloudová služba je vytvořená ve proces se nazývá hadb.</span><span class="sxs-lookup"><span data-stu-id="f8271-139">A new cloud service is created in the process, called hadb.</span></span> <span data-ttu-id="f8271-140">Tento název je znázorněný v podobě sdílené, Vyrovnávání zatížení sítě, které služba bude mít po přidání více prostředků.</span><span class="sxs-lookup"><span data-stu-id="f8271-140">This name illustrates the shared, load-balanced nature that the service will have when more resources are added.</span></span> <span data-ttu-id="f8271-141">Vytvoření `hadb01` je bezproblémové a dokončených pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="f8271-141">The creation of `hadb01` is uneventful and completed by using the portal.</span></span> <span data-ttu-id="f8271-142">Koncový bod SSH se automaticky vytvoří a bude nová síť je vybrána.</span><span class="sxs-lookup"><span data-stu-id="f8271-142">An endpoint for SSH is automatically created, and the new network is selected.</span></span> <span data-ttu-id="f8271-143">Nyní můžete vytvořit sadu dostupnosti pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f8271-143">Now you can create an availability set for the VMs.</span></span>

<span data-ttu-id="f8271-144">Po vytvoření první virtuální počítač (technicky vytvoření cloudové služby) vytvořit druhý virtuální počítač, `hadb02`.</span><span class="sxs-lookup"><span data-stu-id="f8271-144">After the first VM is created (technically, when the cloud service is created), create the second VM, `hadb02`.</span></span> <span data-ttu-id="f8271-145">Pro druhý virtuální počítač, použijte virtuálního počítače s Ubuntu 13.10 z Galerie pomocí portálu, ale použít stávající cloudovou službu, `hadb.cloudapp.net`, místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="f8271-145">For the second VM, use Ubuntu 13.10 VM from the Gallery by using the portal, but use an existing cloud service, `hadb.cloudapp.net`, instead of creating a new one.</span></span> <span data-ttu-id="f8271-146">Měla by být automaticky vybrána sadu sítě a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="f8271-146">The network and availability set should be automatically selected.</span></span> <span data-ttu-id="f8271-147">Koncový bod SSH bude vytvořen, příliš.</span><span class="sxs-lookup"><span data-stu-id="f8271-147">An SSH endpoint will be created, too.</span></span>

<span data-ttu-id="f8271-148">Po vytvoření oba virtuální počítače, poznamenejte si portu SSH pro `hadb01` (port TCP 22) a `hadb02` (automaticky přiřazené Azure).</span><span class="sxs-lookup"><span data-stu-id="f8271-148">After both VMs have been created, take note of the SSH port for `hadb01` (TCP 22) and `hadb02` (automatically assigned by Azure).</span></span>

### <a name="attached-storage"></a><span data-ttu-id="f8271-149">Připojené úložiště</span><span class="sxs-lookup"><span data-stu-id="f8271-149">Attached storage</span></span>
<span data-ttu-id="f8271-150">Nový disk Připojte oba virtuální počítače a vytvořit 5 GB disky v procesu.</span><span class="sxs-lookup"><span data-stu-id="f8271-150">Attach a new disk to both VMs and create 5-GB disks in the process.</span></span> <span data-ttu-id="f8271-151">Disky jsou hostované v kontejneru VHD použijte pro disky hlavní operační systém.</span><span class="sxs-lookup"><span data-stu-id="f8271-151">The disks are hosted in the VHD container in use for your main operating system disks.</span></span> <span data-ttu-id="f8271-152">Po disky jsou vytvořeny a připojené, není nutné restartovat Linux, protože nové zařízení se zobrazí jádra.</span><span class="sxs-lookup"><span data-stu-id="f8271-152">After disks are created and attached, there is no need to restart Linux because the kernel will see the new device.</span></span> <span data-ttu-id="f8271-153">Toto zařízení je většinou `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="f8271-153">This device is usually `/dev/sdc`.</span></span> <span data-ttu-id="f8271-154">Zkontrolujte `dmesg` pro výstup.</span><span class="sxs-lookup"><span data-stu-id="f8271-154">Check `dmesg` for the output.</span></span>

<span data-ttu-id="f8271-155">Na každý virtuální počítač vytvořit oddíl pomocí `cfdisk` (primární, Linux oddíl) a zápisu v nové tabulce oddílu.</span><span class="sxs-lookup"><span data-stu-id="f8271-155">On each VM, create a partition by using `cfdisk` (primary, Linux partition) and write the new partition table.</span></span> <span data-ttu-id="f8271-156">Nevytvářejte systém souborů na tento oddíl.</span><span class="sxs-lookup"><span data-stu-id="f8271-156">Do not create a file system on this partition.</span></span>

## <a name="set-up-the-cluster"></a><span data-ttu-id="f8271-157">Nastavení clusteru</span><span class="sxs-lookup"><span data-stu-id="f8271-157">Set up the cluster</span></span>
<span data-ttu-id="f8271-158">Použijte k instalaci Corosync, kardiostimulátor a DRBD oba virtuální počítače Ubuntu byt č.</span><span class="sxs-lookup"><span data-stu-id="f8271-158">Use APT to install Corosync, Pacemaker, and DRBD on both Ubuntu VMs.</span></span> <span data-ttu-id="f8271-159">Uděláte to tak s `apt-get`, spusťte následující kód:</span><span class="sxs-lookup"><span data-stu-id="f8271-159">To do so with `apt-get`, run the following code:</span></span>

    sudo apt-get install corosync pacemaker drbd8-utils.

<span data-ttu-id="f8271-160">V tuto chvíli neinstalujte MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-160">Do not install MySQL at this time.</span></span> <span data-ttu-id="f8271-161">Debian a Ubuntu instalační skripty inicializuje datový adresář MySQL na `/var/lib/mysql`, ale protože adresáři bude nahrazena systémem souborů DRBD, budete muset nainstalovat MySQL později.</span><span class="sxs-lookup"><span data-stu-id="f8271-161">Debian and Ubuntu installation scripts will initialize a MySQL data directory on `/var/lib/mysql`, but because the directory will be superseded by a DRBD file system, you need to install MySQL later.</span></span>

<span data-ttu-id="f8271-162">Ověření (pomocí `/sbin/ifconfig`) že jsou oba virtuální počítače pomocí adresami v podsíti 10.10.10.0/24 a že se navzájem ping podle názvu.</span><span class="sxs-lookup"><span data-stu-id="f8271-162">Verify (by using `/sbin/ifconfig`) that both VMs are using addresses in the 10.10.10.0/24 subnet and that they can ping each other by name.</span></span> <span data-ttu-id="f8271-163">Můžete také použít `ssh-keygen` a `ssh-copy-id` a ujistěte se, oba virtuální počítače mohou komunikovat pomocí protokolu SSH bez nutnosti heslo.</span><span class="sxs-lookup"><span data-stu-id="f8271-163">You can also use `ssh-keygen` and `ssh-copy-id` to make sure both VMs can communicate via SSH without requiring a password.</span></span>

### <a name="set-up-drbd"></a><span data-ttu-id="f8271-164">Nastavit DRBD</span><span class="sxs-lookup"><span data-stu-id="f8271-164">Set up DRBD</span></span>
<span data-ttu-id="f8271-165">Vytvořit DRBD prostředek, který používá základní `/dev/sdc1` oddíl, který chcete vytvořit `/dev/drbd1` prostředků, kterou můžete naformátovat pomocí ext3 a používá primární i sekundární uzly.</span><span class="sxs-lookup"><span data-stu-id="f8271-165">Create a DRBD resource that uses the underlying `/dev/sdc1` partition to produce a `/dev/drbd1` resource that can be formatted by using ext3 and used in both primary and secondary nodes.</span></span>

1. <span data-ttu-id="f8271-166">Otevřete `/etc/drbd.d/r0.res` a zkopírujte následující definici prostředků na oba virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="f8271-166">Open `/etc/drbd.d/r0.res` and copy the following resource definition on both VMs:</span></span>

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. <span data-ttu-id="f8271-167">Inicializace prostředku pomocí `drbdadm` na oba virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="f8271-167">Initialize the resource by using `drbdadm` on both VMs:</span></span>

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. <span data-ttu-id="f8271-168">Na primárním virtuálním počítači (`hadb01`), vynutit vlastnictví (primární) DRBD prostředku:</span><span class="sxs-lookup"><span data-stu-id="f8271-168">On the primary VM (`hadb01`), force ownership (primary) of the DRBD resource:</span></span>

        sudo drbdadm primary --force r0

<span data-ttu-id="f8271-169">Pokud si projdete obsah nebo proc/drbd (`sudo cat /proc/drbd`) na oba virtuální počítače, měli byste vidět `Primary/Secondary` na `hadb01` a `Secondary/Primary` na `hadb02`a konzistentní s řešením v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="f8271-169">If you examine the contents of /proc/drbd (`sudo cat /proc/drbd`) on both VMs, you should see `Primary/Secondary` on `hadb01` and `Secondary/Primary` on `hadb02`, consistent with the solution at this point.</span></span> <span data-ttu-id="f8271-170">5 GB disk se synchronizují přes síť 10.10.10.0/24 zdarma pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="f8271-170">The 5-GB disk is synchronized over the 10.10.10.0/24 network at no charge to customers.</span></span>

<span data-ttu-id="f8271-171">Po synchronizaci disk můžete vytvořit systém souborů na `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="f8271-171">After the disk is synchronized, you can create the file system on `hadb01`.</span></span> <span data-ttu-id="f8271-172">Pro účely testování jsme použili ext2, ale následující kód vytvoří systém souborů ext3:</span><span class="sxs-lookup"><span data-stu-id="f8271-172">For testing purposes, we used ext2, but the following code will create an ext3 file system:</span></span>

    mkfs.ext3 /dev/drbd1

### <a name="mount-the-drbd-resource"></a><span data-ttu-id="f8271-173">Připojte prostředek DRBD</span><span class="sxs-lookup"><span data-stu-id="f8271-173">Mount the DRBD resource</span></span>
<span data-ttu-id="f8271-174">Nyní jste připraveni připojit prostředky DRBD na `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="f8271-174">You're now ready to mount the DRBD resources on `hadb01`.</span></span> <span data-ttu-id="f8271-175">Použití debian a odvozené konfigurace `/var/lib/mysql` jako adresář data na MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-175">Debian and derivatives use `/var/lib/mysql` as MySQL's data directory.</span></span> <span data-ttu-id="f8271-176">Protože jste nenainstalovali MySQL, vytvořit adresář a připojte DRBD prostředků.</span><span class="sxs-lookup"><span data-stu-id="f8271-176">Because you haven't installed MySQL, create the directory and mount the DRBD resource.</span></span> <span data-ttu-id="f8271-177">Chcete-li provést tuto možnost, spusťte následující kód na `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="f8271-177">To perform this option, run the following code on `hadb01`:</span></span>

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a><span data-ttu-id="f8271-178">Nastavit MySQL</span><span class="sxs-lookup"><span data-stu-id="f8271-178">Set up MySQL</span></span>
<span data-ttu-id="f8271-179">Nyní jste připraveni k instalaci databáze MySQL na `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="f8271-179">Now you're ready to install MySQL on `hadb01`:</span></span>

    sudo apt-get install mysql-server

<span data-ttu-id="f8271-180">Pro `hadb02`, máte dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="f8271-180">For `hadb02`, you have two options.</span></span> <span data-ttu-id="f8271-181">Můžete nainstalovat mysql-server, který bude vytvářet /var/lib/mysql, vyplnit nový adresář dat a pak odeberte obsah.</span><span class="sxs-lookup"><span data-stu-id="f8271-181">You can install mysql-server, which will create /var/lib/mysql, fill it with a new data directory, and then remove the contents.</span></span> <span data-ttu-id="f8271-182">Chcete-li provést tuto možnost, spusťte následující kód na `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="f8271-182">To perform this option, run the following code on `hadb02`:</span></span>

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

<span data-ttu-id="f8271-183">Druhou možností je převzetí služeb při selhání `hadb02` a pak nainstalujte mysql-server existuje.</span><span class="sxs-lookup"><span data-stu-id="f8271-183">The second option is to failover to `hadb02` and then install mysql-server there.</span></span> <span data-ttu-id="f8271-184">Skripty instalace si všimněte existující instalaci a nebude touch.</span><span class="sxs-lookup"><span data-stu-id="f8271-184">Installation scripts will notice the existing installation and won't touch it.</span></span>

<span data-ttu-id="f8271-185">Spusťte následující kód `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="f8271-185">Run the following code on `hadb01`:</span></span>

    sudo drbdadm secondary –force r0

<span data-ttu-id="f8271-186">Spusťte následující kód `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="f8271-186">Run the following code on `hadb02`:</span></span>

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

<span data-ttu-id="f8271-187">Pokud nemáte v úmyslu převzetí služeb při selhání DRBD nyní, první možností je jednodušší, i když pravděpodobně méně elegantní.</span><span class="sxs-lookup"><span data-stu-id="f8271-187">If you don't plan to failover DRBD now, the first option is easier although arguably less elegant.</span></span> <span data-ttu-id="f8271-188">Po nastavit tuto možnost, můžete začít pracovat ve vaší databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-188">After you set this up, you can start working on your MySQL database.</span></span> <span data-ttu-id="f8271-189">Spusťte následující kód `hadb02` (nebo libovolného jeden ze serverů je aktivní, podle DRBD):</span><span class="sxs-lookup"><span data-stu-id="f8271-189">Run the following code on `hadb02` (or whichever one of the servers is active, according to DRBD):</span></span>

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

> [!WARNING]
> <span data-ttu-id="f8271-190">Tento poslední příkaz efektivně zakáže ověřování pro kořenového uživatele v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="f8271-190">This last statement effectively disables authentication for the root user in this table.</span></span> <span data-ttu-id="f8271-191">To by měl být nahrazen produkční úrovni udělit příkazy a je jen pro ilustraci zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="f8271-191">This should be replaced by your production-grade GRANT statements and is included only for illustrative purposes.</span></span>

<span data-ttu-id="f8271-192">Pokud chcete, aby dotazy z mimo virtuálních počítačů (což je účel tohoto průvodce), musíte také povolit sítě pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-192">If you want to make queries from outside the VMs (which is the purpose of this guide), you also need to enable networking for MySQL.</span></span> <span data-ttu-id="f8271-193">Na oba virtuální počítače, otevřete `/etc/mysql/my.cnf` a přejděte na `bind-address`.</span><span class="sxs-lookup"><span data-stu-id="f8271-193">On both VMs, open `/etc/mysql/my.cnf` and go to `bind-address`.</span></span> <span data-ttu-id="f8271-194">Změňte adresu z adresy 127.0.0.1 na hodnotu 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="f8271-194">Change the address from 127.0.0.1 to 0.0.0.0.</span></span> <span data-ttu-id="f8271-195">Po uložení souboru, vydávání `sudo service mysql restart` na váš aktuální primární.</span><span class="sxs-lookup"><span data-stu-id="f8271-195">After saving the file, issue a `sudo service mysql restart` on your current primary.</span></span>

### <a name="create-the-mysql-load-balanced-set"></a><span data-ttu-id="f8271-196">Vytvořit sadu MySQL Vyrovnávání zatížení sítě</span><span class="sxs-lookup"><span data-stu-id="f8271-196">Create the MySQL load-balanced set</span></span>
<span data-ttu-id="f8271-197">Přejděte zpět na portálu, přejděte na `hadb01`a zvolte **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="f8271-197">Go back to the portal, go to `hadb01`, and choose **Endpoints**.</span></span> <span data-ttu-id="f8271-198">Pokud chcete vytvořit koncový bod, zvolte MySQL (TCP 3306) z rozevíracího seznamu a vyberte **s vyrovnáváním zatížení vytvořit nové**.</span><span class="sxs-lookup"><span data-stu-id="f8271-198">To create an endpoint, choose MySQL (TCP 3306) from the drop-down list and select **Create new load balanced set**.</span></span> <span data-ttu-id="f8271-199">Název koncového bodu Vyrovnávání zatížení sítě `lb-mysql`.</span><span class="sxs-lookup"><span data-stu-id="f8271-199">Name the load-balanced endpoint `lb-mysql`.</span></span> <span data-ttu-id="f8271-200">Nastavit **čas** na 5 sekund, minimální.</span><span class="sxs-lookup"><span data-stu-id="f8271-200">Set **Time** to 5 seconds, minimum.</span></span>

<span data-ttu-id="f8271-201">Po vytvoření koncového bodu, přejděte na `hadb02`, zvolte **koncové body**a vytvořit koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f8271-201">After you create the endpoint, go to `hadb02`, choose **Endpoints**, and create an endpoint.</span></span> <span data-ttu-id="f8271-202">Zvolte `lb-mysql`a pak z rozevíracího seznamu vyberte MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-202">Choose `lb-mysql`, and then select MySQL from the drop-down list.</span></span> <span data-ttu-id="f8271-203">Můžete také použít rozhraní příkazového řádku Azure pro tento krok.</span><span class="sxs-lookup"><span data-stu-id="f8271-203">You can also use the Azure CLI for this step.</span></span>

<span data-ttu-id="f8271-204">Nyní máte všechny potřebné pro ruční operaci clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8271-204">You now have everything you need for manual operation of the cluster.</span></span>

### <a name="test-the-load-balanced-set"></a><span data-ttu-id="f8271-205">Testovací sady vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="f8271-205">Test the load-balanced set</span></span>
<span data-ttu-id="f8271-206">Testy můžete provést z mimo počítač, pomocí libovolného klienta, MySQL, nebo pomocí některých aplikací, jako je phpMyAdmin spuštěna jako web Azure.</span><span class="sxs-lookup"><span data-stu-id="f8271-206">Tests can be performed from an outside machine by using any MySQL client, or by using certain applications, like phpMyAdmin running as an Azure website.</span></span> <span data-ttu-id="f8271-207">V takovém případě použít nástroj příkazového řádku na MySQL na jiného pole Linux:</span><span class="sxs-lookup"><span data-stu-id="f8271-207">In this case, you used MySQL's command-line tool on another Linux box:</span></span>

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a><span data-ttu-id="f8271-208">Ručně přebírání služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="f8271-208">Manually failing over</span></span>
<span data-ttu-id="f8271-209">Převzetí služeb při selhání můžete simulovat vypínání databáze MySQL, přepnutí na DRBD primární a znovu spustit MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-209">You can simulate failovers by shutting down MySQL, switching DRBD's primary, and starting MySQL again.</span></span>

<span data-ttu-id="f8271-210">K provedení této úlohy, spusťte následující kód na hadb01:</span><span class="sxs-lookup"><span data-stu-id="f8271-210">To perform this task, run the following code on hadb01:</span></span>

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

<span data-ttu-id="f8271-211">Pak klikněte na hadb02:</span><span class="sxs-lookup"><span data-stu-id="f8271-211">Then, on hadb02:</span></span>

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

<span data-ttu-id="f8271-212">Po selhání ručně, můžete opakovat vzdálený dotaz a měli perfektně fungovat.</span><span class="sxs-lookup"><span data-stu-id="f8271-212">After you fail over manually, you can repeat your remote query and it should work perfectly.</span></span>

## <a name="set-up-corosync"></a><span data-ttu-id="f8271-213">Nastavit Corosync</span><span class="sxs-lookup"><span data-stu-id="f8271-213">Set up Corosync</span></span>
<span data-ttu-id="f8271-214">Corosync je základní clusteru infrastrukturu potřebnou pro kardiostimulátor pracovat.</span><span class="sxs-lookup"><span data-stu-id="f8271-214">Corosync is the underlying cluster infrastructure required for Pacemaker to work.</span></span> <span data-ttu-id="f8271-215">Pro zjišťování prezenčního signálu (a další metody jako Ultramonkey) je Corosync rozdělení funkce CRM, když kardiostimulátor zůstane více podobná prezenčního signálu ve funkcích.</span><span class="sxs-lookup"><span data-stu-id="f8271-215">For Heartbeat (and other methodologies like Ultramonkey), Corosync is a split of the CRM functionalities, while Pacemaker remains more similar to Heartbeat in functionality.</span></span>

<span data-ttu-id="f8271-216">Hlavní omezení pro Corosync v Azure je Corosync upřednostní vícesměrového vysílání přes všesměrové vysílání přes komunikace jednosměrového vysílání, že Microsoft Azure sítě podporuje pouze jednosměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="f8271-216">The main constraint for Corosync on Azure is that Corosync prefers multicast over broadcast over unicast communications, but Microsoft Azure networking only supports unicast.</span></span>

<span data-ttu-id="f8271-217">Naštěstí Corosync má pracovní režim jednosměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="f8271-217">Fortunately, Corosync has a working unicast mode.</span></span> <span data-ttu-id="f8271-218">Pouze skutečné omezení je, protože všechny uzly nejsou komunikaci mezi sebou, je třeba definovat uzly do konfiguračních souborů, včetně jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f8271-218">The only real constraint is that because all nodes are not communicating among themselves, you need to define the nodes in your configuration files, including their IP addresses.</span></span> <span data-ttu-id="f8271-219">Můžeme použít soubory příklad Corosync pro jednosměrového vysílání a změňte vazby adresu, uzel seznamy a protokolování adresáře (Ubuntu používá `/var/log/corosync` při použití souborů v příkladu `/var/log/cluster`) a povolit nástroje kvora.</span><span class="sxs-lookup"><span data-stu-id="f8271-219">We can use the Corosync example files for Unicast and change bind address, node lists, and logging directories (Ubuntu uses `/var/log/corosync` while the example files use `/var/log/cluster`), and enable quorum tools.</span></span>

> [!NOTE]
> <span data-ttu-id="f8271-220">Použijte následující `transport: udpu` směrnice a ručně definované IP adresy pro oba uzly.</span><span class="sxs-lookup"><span data-stu-id="f8271-220">Use the following `transport: udpu` directive and the manually defined IP addresses for both nodes.</span></span>

<span data-ttu-id="f8271-221">Spusťte následující kód `/etc/corosync/corosync.conf` pro oba uzly:</span><span class="sxs-lookup"><span data-stu-id="f8271-221">Run the following code on `/etc/corosync/corosync.conf` for both nodes:</span></span>

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

<span data-ttu-id="f8271-222">Zkopírujte tento konfigurační soubor na oba virtuální počítače a spustit Corosync v obou uzlů:</span><span class="sxs-lookup"><span data-stu-id="f8271-222">Copy this configuration file on both VMs and start Corosync in both nodes:</span></span>

    sudo service start corosync

<span data-ttu-id="f8271-223">Krátce po spuštění služby, by se mělo vytvořit cluster v aktuální prstenec a by měl být vytvářen kvora.</span><span class="sxs-lookup"><span data-stu-id="f8271-223">Shortly after starting the service, the cluster should be established in the current ring, and quorum should be constituted.</span></span> <span data-ttu-id="f8271-224">Tato funkce jsme můžete zkontrolovat kontrolou protokolů nebo spuštěním následující kód:</span><span class="sxs-lookup"><span data-stu-id="f8271-224">We can check this functionality by reviewing logs or by running the following code:</span></span>

    sudo corosync-quorumtool –l

<span data-ttu-id="f8271-225">Zobrazí se výstup podobný na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="f8271-225">You will see output similar to the following image:</span></span>

![corosync quorumtool - l ukázkový výstup](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a><span data-ttu-id="f8271-227">Nastavit kardiostimulátor</span><span class="sxs-lookup"><span data-stu-id="f8271-227">Set up Pacemaker</span></span>
<span data-ttu-id="f8271-228">Kardiostimulátor používá k monitorování pro prostředky, zadejte, kdy základní barvy přejděte a přepněte tyto prostředky do sekundární databáze clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8271-228">Pacemaker uses the cluster to monitor for resources, define when primaries go down, and switch those resources to secondaries.</span></span> <span data-ttu-id="f8271-229">Prostředky lze definovat ze sady skriptů, k dispozici nebo z LSB skripty (init jako), mezi další možnosti.</span><span class="sxs-lookup"><span data-stu-id="f8271-229">Resources can be defined from a set of available scripts or from LSB (init-like) scripts, among other choices.</span></span>

<span data-ttu-id="f8271-230">Chceme kardiostimulátor na "vlastní" prostředek DRBD, přípojného bodu a službu MySQL.</span><span class="sxs-lookup"><span data-stu-id="f8271-230">We want Pacemaker to "own" the DRBD resource, the mount point, and the MySQL service.</span></span> <span data-ttu-id="f8271-231">Pokud kardiostimulátor můžete zapnout a vypnout DRBD, připojit a odpojit a pak spusťte a ukončete MySQL ve správném pořadí když s primární se stane něco chybný, instalace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="f8271-231">If Pacemaker can turn on and off DRBD, mount and unmount it, and then start and stop MySQL in the right order when something bad happens with the primary, setup is complete.</span></span>

<span data-ttu-id="f8271-232">Při první instalaci kardiostimulátor, musí být dostatečně jednoduchá něco jako konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="f8271-232">When you first install Pacemaker, your configuration should be simple enough, something like:</span></span>

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. <span data-ttu-id="f8271-233">Zkontrolujte konfiguraci spuštěním `sudo crm configure show`.</span><span class="sxs-lookup"><span data-stu-id="f8271-233">Check the configuration by running `sudo crm configure show`.</span></span>
2. <span data-ttu-id="f8271-234">Pak vytvořte soubor (například `/tmp/cluster.conf`) se v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="f8271-234">Then create a file (like `/tmp/cluster.conf`) with the following resources:</span></span>

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. <span data-ttu-id="f8271-235">Načtení souboru do konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f8271-235">Load the file into the configuration.</span></span> <span data-ttu-id="f8271-236">Stačí to udělat v jednom uzlu.</span><span class="sxs-lookup"><span data-stu-id="f8271-236">You only need to do this in one node.</span></span>

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. <span data-ttu-id="f8271-237">Ujistěte se, že kardiostimulátor začíná na spouštění v obou uzlů:</span><span class="sxs-lookup"><span data-stu-id="f8271-237">Make sure that Pacemaker starts at boot in both nodes:</span></span>

        sudo update-rc.d pacemaker defaults

5. <span data-ttu-id="f8271-238">Pomocí `sudo crm_mon –L`, ověřte, že jeden z uzlů se stal hlavní pro cluster a běží všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f8271-238">By using `sudo crm_mon –L`, verify that one of your nodes has become the master for the cluster and is running all the resources.</span></span> <span data-ttu-id="f8271-239">Zkontrolujte, zda jsou spuštěny prostředky můžete připojit a ps.</span><span class="sxs-lookup"><span data-stu-id="f8271-239">You can use mount and ps to check that the resources are running.</span></span>

<span data-ttu-id="f8271-240">Následující snímek obrazovky ukazuje `crm_mon` s jedním uzlem zastavena (ukončení tak, že vyberete kombinaci kláves Ctrl + C):</span><span class="sxs-lookup"><span data-stu-id="f8271-240">The following screenshot shows `crm_mon` with one node stopped (exit by selecting Ctrl+C):</span></span>

![uzel crm_mon zastavena](./media/mysql-cluster/image002.png)

<span data-ttu-id="f8271-242">Tento snímek obrazovky ukazuje uzly, jeden z nich a jeden podřízený:</span><span class="sxs-lookup"><span data-stu-id="f8271-242">This screenshot shows both nodes, one master and one slave:</span></span>

![podřízený provozní hlavní crm_mon](./media/mysql-cluster/image003.png)

## <a name="testing"></a><span data-ttu-id="f8271-244">Testování</span><span class="sxs-lookup"><span data-stu-id="f8271-244">Testing</span></span>
<span data-ttu-id="f8271-245">Jste připraveni simulaci automatické převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f8271-245">You're ready for an automatic failover simulation.</span></span> <span data-ttu-id="f8271-246">Existují dva způsoby, jak to udělat: a nepodmíněných.</span><span class="sxs-lookup"><span data-stu-id="f8271-246">There are two ways to do this: soft and hard.</span></span>

<span data-ttu-id="f8271-247">Logicky způsob využívá funkce vypnutí clusteru: ``crm_standby -U `uname -n` -v on``.</span><span class="sxs-lookup"><span data-stu-id="f8271-247">The soft way uses the cluster's shutdown function: ``crm_standby -U `uname -n` -v on``.</span></span> <span data-ttu-id="f8271-248">Pokud používáte na hlavním serveru tím, že podřízená má.</span><span class="sxs-lookup"><span data-stu-id="f8271-248">If you use this on the master, the slave takes over.</span></span> <span data-ttu-id="f8271-249">Nezapomeňte nastavit zpět na vypnuto.</span><span class="sxs-lookup"><span data-stu-id="f8271-249">Remember to set this back to off.</span></span> <span data-ttu-id="f8271-250">Pokud to neuděláte, crm_mon zobrazí jeden uzel do úsporného režimu.</span><span class="sxs-lookup"><span data-stu-id="f8271-250">If you don't, crm_mon will show one node on standby.</span></span>

<span data-ttu-id="f8271-251">Pevné způsob, jakým se vypíná primárního virtuálního počítače (hadb01) prostřednictvím portálu nebo změnou runlevel ve virtuálním počítači (to znamená, zastavení, vypnutí).</span><span class="sxs-lookup"><span data-stu-id="f8271-251">The hard way is shutting down the primary VM (hadb01) via the portal or by changing the runlevel on the VM (that is, halt, shutdown).</span></span> <span data-ttu-id="f8271-252">To pomáhá Corosync a kardiostimulátor podle signalizace, která je hlavním směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="f8271-252">This helps Corosync and Pacemaker by signaling that the master's going down.</span></span> <span data-ttu-id="f8271-253">Toto můžete otestovat (vhodný pro údržbu), ale můžete taky přinutit, tento scénář zmrazené virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f8271-253">You can test this (useful for maintenance windows), but you can also force the scenario by freezing the VM.</span></span>

## <a name="stonith"></a><span data-ttu-id="f8271-254">STONITH</span><span class="sxs-lookup"><span data-stu-id="f8271-254">STONITH</span></span>
<span data-ttu-id="f8271-255">Musí být možné vydat vypnutí virtuálního počítače prostřednictvím rozhraní příkazového řádku Azure místo STONITH skript, který řídí fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="f8271-255">It should be possible to issue a VM shutdown via the Azure CLI in lieu of a STONITH script that controls a physical device.</span></span> <span data-ttu-id="f8271-256">Můžete použít `/usr/lib/stonith/plugins/external/ssh` jako základní a povolit STONITH v konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8271-256">You can use `/usr/lib/stonith/plugins/external/ssh` as a base and enable STONITH in the cluster's configuration.</span></span> <span data-ttu-id="f8271-257">Rozhraní příkazového řádku Azure by měly být globálně nainstalovány a publikovat nastavení a profil by měly být načteny pro uživatele clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8271-257">Azure CLI should be globally installed, and the publish settings and profile should be loaded for the cluster's user.</span></span>

<span data-ttu-id="f8271-258">Ukázkový kód pro prostředek je k dispozici na [Githubu](https://github.com/bureado/aztonith).</span><span class="sxs-lookup"><span data-stu-id="f8271-258">Sample code for the resource is available on [GitHub](https://github.com/bureado/aztonith).</span></span> <span data-ttu-id="f8271-259">Změnit konfiguraci clusteru přidáním následujícího `sudo crm configure`:</span><span class="sxs-lookup"><span data-stu-id="f8271-259">Change the cluster's configuration by adding the following to `sudo crm configure`:</span></span>

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> <span data-ttu-id="f8271-260">Skript neprovede nahoru/dolů kontroly.</span><span class="sxs-lookup"><span data-stu-id="f8271-260">The script doesn't perform up/down checks.</span></span> <span data-ttu-id="f8271-261">Původní prostředků SSH měl 15 příkaz ping kontroluje, ale čas obnovení pro virtuální počítač Azure může být další proměnná.</span><span class="sxs-lookup"><span data-stu-id="f8271-261">The original SSH resource had 15 ping checks, but recovery time for an Azure VM might be more variable.</span></span>

## <a name="limitations"></a><span data-ttu-id="f8271-262">Omezení</span><span class="sxs-lookup"><span data-stu-id="f8271-262">Limitations</span></span>
<span data-ttu-id="f8271-263">Platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="f8271-263">The following limitations apply:</span></span>

* <span data-ttu-id="f8271-264">Skript linbit DRBD prostředku, který spravuje DRBD jako prostředek v kardiostimulátor používá `drbdadm down` při vypnutí uzlu, i když uzlu se právě děje pohotovostní režim.</span><span class="sxs-lookup"><span data-stu-id="f8271-264">The linbit DRBD resource script that manages DRBD as a resource in Pacemaker uses `drbdadm down` when shutting down a node, even if the node is just going on standby.</span></span> <span data-ttu-id="f8271-265">Toto není ideální vzhledem k tomu, že podřízená nebude možné synchronizace DRBD prostředků při hlavní získá zápisy.</span><span class="sxs-lookup"><span data-stu-id="f8271-265">This is not ideal because the slave will not be synchronizing the DRBD resource while the master gets writes.</span></span> <span data-ttu-id="f8271-266">Pokud je hlavní server neselže zdvořile, že podřízená může trvat přes starší stav systému souborů.</span><span class="sxs-lookup"><span data-stu-id="f8271-266">If the master does not fail graciously, the slave can take over an older file system state.</span></span> <span data-ttu-id="f8271-267">Existují dva způsoby potenciální toto řešení:</span><span class="sxs-lookup"><span data-stu-id="f8271-267">There are two potential ways of solving this:</span></span>
  * <span data-ttu-id="f8271-268">Vynucení `drbdadm up r0` ve všech uzlech clusteru prostřednictvím místní sledovací zařízení (ne clusterized)</span><span class="sxs-lookup"><span data-stu-id="f8271-268">Enforcing a `drbdadm up r0` in all cluster nodes via a local (not clusterized) watchdog</span></span>
  * <span data-ttu-id="f8271-269">Úpravy linbit DRBD skript, a ověřte, zda `down` není volán`/usr/lib/ocf/resource.d/linbit/drbd`</span><span class="sxs-lookup"><span data-stu-id="f8271-269">Editing the linbit DRBD script, making sure that `down` is not called in `/usr/lib/ocf/resource.d/linbit/drbd`</span></span>
* <span data-ttu-id="f8271-270">Nástroje pro vyrovnávání zatížení potřeba alespoň pět sekund reagovat, takže aplikace by měla být clustery a být větší toleranci vůči časový limit.</span><span class="sxs-lookup"><span data-stu-id="f8271-270">The load balancer needs at least five seconds to respond, so applications should be cluster-aware and be more tolerant of timeout.</span></span> <span data-ttu-id="f8271-271">Jiné architektury, jako v aplikaci fronty a middlewares dotaz, může také pomoct.</span><span class="sxs-lookup"><span data-stu-id="f8271-271">Other architectures, like in-app queues and query middlewares, can also help.</span></span>
* <span data-ttu-id="f8271-272">Ladění MySQL je nutné zajistit, že zápis se provádí spravovat tempem a mezipaměti jsou vyprazdňuje na disk se často chcete-li minimalizovat ztrátu paměti.</span><span class="sxs-lookup"><span data-stu-id="f8271-272">MySQL tuning is necessary to ensure that writing is done at a manageable pace and caches are flushed to disk as frequently as possible to minimize memory loss.</span></span>
* <span data-ttu-id="f8271-273">Zápis výkonu je závislý na virtuální počítač propojení ve virtuálním přepínači, protože to je používáno DRBD k replikaci zařízení.</span><span class="sxs-lookup"><span data-stu-id="f8271-273">Write performance is dependent in VM interconnect in the virtual switch because this is the mechanism used by DRBD to replicate the device.</span></span>
