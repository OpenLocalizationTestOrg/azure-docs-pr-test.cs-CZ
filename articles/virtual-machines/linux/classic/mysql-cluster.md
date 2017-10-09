---
title: "aaaClusterize MySQL s vyrovnáváním zatížení sad | Microsoft Docs"
description: "Nastavení pro zařízení s vyrovnáváním zatížení, vysokou dostupnost clusteru Linux MySQL vytvořené pomocí modelu nasazení classic hello v Azure"
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
ms.openlocfilehash: 1829fd877c4b0ed177b23a8e3404dbb3db746561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-load-balanced-sets-tooclusterize-mysql-on-linux"></a><span data-ttu-id="8a5bd-103">Použití vyrovnávání zatížení sítě nastaví tooclusterize MySQL v systému Linux</span><span class="sxs-lookup"><span data-stu-id="8a5bd-103">Use load-balanced sets tooclusterize MySQL on Linux</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8a5bd-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="8a5bd-105">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="8a5bd-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="8a5bd-107">A [šablony Resource Manageru](https://azure.microsoft.com/documentation/templates/mysql-replication/) je k dispozici, pokud potřebujete toodeploy MySQL cluster.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-107">A [Resource Manager template](https://azure.microsoft.com/documentation/templates/mysql-replication/) is available if you need toodeploy a MySQL cluster.</span></span>

<span data-ttu-id="8a5bd-108">V tomto článku jsou zde popsány a ukazuje hello různý přístup k dispozici toodeploy systémem Linux služeb s vysokou dostupností v Microsoft Azure, prohlížení vysoké dostupnosti serveru MySQL jako základy.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-108">This article explores and illustrates hello different approaches available toodeploy highly available Linux-based services on Microsoft Azure, exploring MySQL Server high availability as a primer.</span></span> <span data-ttu-id="8a5bd-109">Video ilustrující tento přístup je k dispozici na [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span><span class="sxs-lookup"><span data-stu-id="8a5bd-109">A video illustrating this approach is available on [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span></span>

<span data-ttu-id="8a5bd-110">Nemůžeme se popisují shared nothing, dvěma uzly, jeden hlavní řešení vysoké dostupnosti databáze MySQL na základě DRBD, Corosync a kardiostimulátor.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-110">We will outline a shared-nothing, two-node, single-master MySQL high availability solution based on DRBD, Corosync, and Pacemaker.</span></span> <span data-ttu-id="8a5bd-111">Databáze MySQL byla najednou spuštěna pouze jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-111">Only one node runs MySQL at a time.</span></span> <span data-ttu-id="8a5bd-112">Čtení a zápis z hello DRBD prostředků je také omezena tooonly jednoho uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-112">Reading and writing from hello DRBD resource is also limited tooonly one node at a time.</span></span>

<span data-ttu-id="8a5bd-113">Není nutné pro VIP řešení jako LVS, protože budete používat sady Vyrovnávání zatížení sítě v tooprovide kruhového dotazování funkce a koncový bod zjišťování, odebrání a úspěšné obnovení hello VIP Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-113">There's no need for a VIP solution like LVS, because you'll use load-balanced sets in Microsoft Azure tooprovide round-robin functionality and endpoint detection, removal, and graceful recovery of hello VIP.</span></span> <span data-ttu-id="8a5bd-114">Hello VIP je globálně směrovatelné adresu IPv4 přiřazené službou Microsoft Azure při prvním vytváření hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-114">hello VIP is a globally routable IPv4 address assigned by Microsoft Azure when you first create hello cloud service.</span></span>

<span data-ttu-id="8a5bd-115">Nejsou k dispozici jako virtuální počítač na další možné architektury pro databázi MySQL, včetně NBD clusteru, Percona, Galera a několik řešení middleware, včetně alespoň jeden [skladu virtuálních počítačů](http://vmdepot.msopentech.com).</span><span class="sxs-lookup"><span data-stu-id="8a5bd-115">There are other possible architectures for MySQL, including NBD Cluster, Percona, Galera, and several middleware solutions, including at least one available as a VM on [VM Depot](http://vmdepot.msopentech.com).</span></span> <span data-ttu-id="8a5bd-116">Dokud tato řešení můžete replikovat na jednosměrové a vícesměrové vysílání nebo všesměrové vysílání a nejsou spoléhají na sdílené úložiště nebo více síťových rozhraní, hello scénáře by měl být snadno toodeploy v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-116">As long as these solutions can replicate on unicast vs. multicast or broadcast and don't rely on shared storage or multiple network interfaces, hello scenarios should be easy toodeploy on Microsoft Azure.</span></span>

<span data-ttu-id="8a5bd-117">Tyto clustering architektury lze rozšířit produkty tooother jako PostgreSQL a OpenLDAP podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-117">These clustering architectures can be extended tooother products like PostgreSQL and OpenLDAP in a similar fashion.</span></span> <span data-ttu-id="8a5bd-118">Například tento postup Vyrovnávání zatížení s nesdílená byla úspěšně testována s více hlavní OpenLDAP a můžete ho sledovat na našem blogu Channel 9.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-118">For example, this load-balancing procedure with shared nothing was successfully tested with multi-master OpenLDAP, and you can watch it on our Channel 9 blog.</span></span>

## <a name="get-ready"></a><span data-ttu-id="8a5bd-119">Příprava</span><span class="sxs-lookup"><span data-stu-id="8a5bd-119">Get ready</span></span>
<span data-ttu-id="8a5bd-120">Budete potřebovat následující hello prostředky a dalo:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-120">You need hello following resources and abilities:</span></span>

  - <span data-ttu-id="8a5bd-121">Účet Microsoft Azure s platným předplatným, možné toocreate aspoň dva virtuální počítače (XS byl použit v tomto příkladu)</span><span class="sxs-lookup"><span data-stu-id="8a5bd-121">A Microsoft Azure account with a valid subscription, able toocreate at least two VMs (XS was used in this example)</span></span>
  - <span data-ttu-id="8a5bd-122">Síť a podsíť</span><span class="sxs-lookup"><span data-stu-id="8a5bd-122">A network and a subnet</span></span>
  - <span data-ttu-id="8a5bd-123">Skupiny vztahů</span><span class="sxs-lookup"><span data-stu-id="8a5bd-123">An affinity group</span></span>
  - <span data-ttu-id="8a5bd-124">Nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="8a5bd-124">An availability set</span></span>
  - <span data-ttu-id="8a5bd-125">Hello možnost toocreate virtuální pevné disky ve stejné oblasti jako cloudová služba hello hello a připojte je toohello virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="8a5bd-125">hello ability toocreate VHDs in hello same region as hello cloud service and attach them toohello Linux VMs</span></span>

### <a name="tested-environment"></a><span data-ttu-id="8a5bd-126">Otestované prostředí</span><span class="sxs-lookup"><span data-stu-id="8a5bd-126">Tested environment</span></span>
* <span data-ttu-id="8a5bd-127">Ubuntu 13.10</span><span class="sxs-lookup"><span data-stu-id="8a5bd-127">Ubuntu 13.10</span></span>
  * <span data-ttu-id="8a5bd-128">DRBD</span><span class="sxs-lookup"><span data-stu-id="8a5bd-128">DRBD</span></span>
  * <span data-ttu-id="8a5bd-129">Server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="8a5bd-129">MySQL Server</span></span>
  * <span data-ttu-id="8a5bd-130">Corosync a kardiostimulátor</span><span class="sxs-lookup"><span data-stu-id="8a5bd-130">Corosync and Pacemaker</span></span>

### <a name="affinity-group"></a><span data-ttu-id="8a5bd-131">Skupina vztahů</span><span class="sxs-lookup"><span data-stu-id="8a5bd-131">Affinity group</span></span>
<span data-ttu-id="8a5bd-132">Vytvořit skupinu vztahů pro řešení hello přihlášením toohello portál Azure classic, vyberte **nastavení**a vytváření skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-132">Create an affinity group for hello solution by signing in toohello Azure classic portal, selecting **Settings**, and creating an affinity group.</span></span> <span data-ttu-id="8a5bd-133">Skupina vztahů toothis bude přiřazen přidělené prostředky vytvořit později.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-133">Allocated resources created later will be assigned toothis affinity group.</span></span>

### <a name="networks"></a><span data-ttu-id="8a5bd-134">Networks</span><span class="sxs-lookup"><span data-stu-id="8a5bd-134">Networks</span></span>
<span data-ttu-id="8a5bd-135">Se vytvoří nová síť a podsíť je vytvořit uvnitř sítě hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-135">A new network is created, and a subnet is created inside hello network.</span></span> <span data-ttu-id="8a5bd-136">Tento příklad používá 10.10.10.0/24 síť s podsítí pouze jeden /24 uvnitř.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-136">This example uses a 10.10.10.0/24 network with only one /24 subnet inside.</span></span>

### <a name="virtual-machines"></a><span data-ttu-id="8a5bd-137">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8a5bd-137">Virtual machines</span></span>
<span data-ttu-id="8a5bd-138">Hello první virtuální počítač 13.10 Ubuntu je vytvořená pomocí image Galerie Endorsed Ubuntu a se nazývá `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-138">hello first Ubuntu 13.10 VM is created by using an Endorsed Ubuntu Gallery image and is called `hadb01`.</span></span> <span data-ttu-id="8a5bd-139">Nová Cloudová služba je vytvořená ve hello proces se nazývá hadb.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-139">A new cloud service is created in hello process, called hadb.</span></span> <span data-ttu-id="8a5bd-140">Tento název znázorňuje hello sdílené, povaze Vyrovnávání zatížení, která hello služba bude mít, když se přidají další prostředky.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-140">This name illustrates hello shared, load-balanced nature that hello service will have when more resources are added.</span></span> <span data-ttu-id="8a5bd-141">Hello vytvoření `hadb01` je bezproblémové a dokončených pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-141">hello creation of `hadb01` is uneventful and completed by using hello portal.</span></span> <span data-ttu-id="8a5bd-142">Koncový bod SSH je automaticky vytvořen a hello novou síť je vybrána.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-142">An endpoint for SSH is automatically created, and hello new network is selected.</span></span> <span data-ttu-id="8a5bd-143">Nyní můžete vytvořit sadu dostupnosti pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-143">Now you can create an availability set for hello VMs.</span></span>

<span data-ttu-id="8a5bd-144">Po hello první virtuální počítač je vytvořen (technicky při hello cloudové služby), vytvořte druhý virtuální počítač, hello `hadb02`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-144">After hello first VM is created (technically, when hello cloud service is created), create hello second VM, `hadb02`.</span></span> <span data-ttu-id="8a5bd-145">Pro hello druhé virtuálních počítačů, použít virtuálního počítače s Ubuntu 13.10 z hello Galerie pomocí hello portálu, ale použít stávající cloudovou službu, `hadb.cloudapp.net`, místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-145">For hello second VM, use Ubuntu 13.10 VM from hello Gallery by using hello portal, but use an existing cloud service, `hadb.cloudapp.net`, instead of creating a new one.</span></span> <span data-ttu-id="8a5bd-146">měla by být automaticky vybrána Hello sítě a dostupnost sady.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-146">hello network and availability set should be automatically selected.</span></span> <span data-ttu-id="8a5bd-147">Koncový bod SSH bude vytvořen, příliš.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-147">An SSH endpoint will be created, too.</span></span>

<span data-ttu-id="8a5bd-148">Po vytvoření oba virtuální počítače, poznamenejte si hello portu SSH pro `hadb01` (port TCP 22) a `hadb02` (automaticky přiřazené Azure).</span><span class="sxs-lookup"><span data-stu-id="8a5bd-148">After both VMs have been created, take note of hello SSH port for `hadb01` (TCP 22) and `hadb02` (automatically assigned by Azure).</span></span>

### <a name="attached-storage"></a><span data-ttu-id="8a5bd-149">Připojené úložiště</span><span class="sxs-lookup"><span data-stu-id="8a5bd-149">Attached storage</span></span>
<span data-ttu-id="8a5bd-150">Připojte nový disk tooboth virtuálních počítačů a vytvořit 5 GB disky v procesu hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-150">Attach a new disk tooboth VMs and create 5-GB disks in hello process.</span></span> <span data-ttu-id="8a5bd-151">Hello disky jsou hostované v kontejneru VHD hello používá pro disky hlavní operační systém.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-151">hello disks are hosted in hello VHD container in use for your main operating system disks.</span></span> <span data-ttu-id="8a5bd-152">Po vytvoření a připojené disky nejsou bez nutnosti toorestart Linux, protože hello jádra uvidí hello nové zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-152">After disks are created and attached, there is no need toorestart Linux because hello kernel will see hello new device.</span></span> <span data-ttu-id="8a5bd-153">Toto zařízení je většinou `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-153">This device is usually `/dev/sdc`.</span></span> <span data-ttu-id="8a5bd-154">Zkontrolujte `dmesg` pro výstup hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-154">Check `dmesg` for hello output.</span></span>

<span data-ttu-id="8a5bd-155">Na každý virtuální počítač vytvořit oddíl pomocí `cfdisk` (primární, Linux oddíl) a zápis hello nový oddíl tabulky.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-155">On each VM, create a partition by using `cfdisk` (primary, Linux partition) and write hello new partition table.</span></span> <span data-ttu-id="8a5bd-156">Nevytvářejte systém souborů na tento oddíl.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-156">Do not create a file system on this partition.</span></span>

## <a name="set-up-hello-cluster"></a><span data-ttu-id="8a5bd-157">Nastavení clusteru hello</span><span class="sxs-lookup"><span data-stu-id="8a5bd-157">Set up hello cluster</span></span>
<span data-ttu-id="8a5bd-158">Použijte VÝSTIŽNÝ tooinstall Corosync, kardiostimulátor a DRBD na oba Ubuntu virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-158">Use APT tooinstall Corosync, Pacemaker, and DRBD on both Ubuntu VMs.</span></span> <span data-ttu-id="8a5bd-159">toodo Ano s `apt-get`spusťte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-159">toodo so with `apt-get`, run hello following code:</span></span>

    sudo apt-get install corosync pacemaker drbd8-utils.

<span data-ttu-id="8a5bd-160">V tuto chvíli neinstalujte MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-160">Do not install MySQL at this time.</span></span> <span data-ttu-id="8a5bd-161">Debian a Ubuntu instalační skripty inicializuje datový adresář MySQL na `/var/lib/mysql`, ale protože hello adresář bude nahrazena systémem souborů DRBD, budete potřebovat později tooinstall MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-161">Debian and Ubuntu installation scripts will initialize a MySQL data directory on `/var/lib/mysql`, but because hello directory will be superseded by a DRBD file system, you need tooinstall MySQL later.</span></span>

<span data-ttu-id="8a5bd-162">Ověření (pomocí `/sbin/ifconfig`) že jsou oba virtuální počítače pomocí adresy v podsíti 10.10.10.0/24 hello a že se navzájem ping podle názvu.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-162">Verify (by using `/sbin/ifconfig`) that both VMs are using addresses in hello 10.10.10.0/24 subnet and that they can ping each other by name.</span></span> <span data-ttu-id="8a5bd-163">Můžete také použít `ssh-keygen` a `ssh-copy-id` toomake, že oba virtuální počítače mohou komunikovat pomocí protokolu SSH bez nutnosti heslo.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-163">You can also use `ssh-keygen` and `ssh-copy-id` toomake sure both VMs can communicate via SSH without requiring a password.</span></span>

### <a name="set-up-drbd"></a><span data-ttu-id="8a5bd-164">Nastavit DRBD</span><span class="sxs-lookup"><span data-stu-id="8a5bd-164">Set up DRBD</span></span>
<span data-ttu-id="8a5bd-165">Vytvořte prostředek DRBD používající základní hello `/dev/sdc1` oddílu tooproduce `/dev/drbd1` prostředků, kterou můžete naformátovat pomocí ext3 a používá primární i sekundární uzly.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-165">Create a DRBD resource that uses hello underlying `/dev/sdc1` partition tooproduce a `/dev/drbd1` resource that can be formatted by using ext3 and used in both primary and secondary nodes.</span></span>

1. <span data-ttu-id="8a5bd-166">Otevřete `/etc/drbd.d/r0.res` a kopírování hello následující definice prostředků na oba virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-166">Open `/etc/drbd.d/r0.res` and copy hello following resource definition on both VMs:</span></span>

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

2. <span data-ttu-id="8a5bd-167">Inicializace hello prostředků pomocí `drbdadm` na oba virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-167">Initialize hello resource by using `drbdadm` on both VMs:</span></span>

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. <span data-ttu-id="8a5bd-168">Na hello primárního virtuálního počítače (`hadb01`), vynutit vlastnictví (primární) hello DRBD prostředku:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-168">On hello primary VM (`hadb01`), force ownership (primary) of hello DRBD resource:</span></span>

        sudo drbdadm primary --force r0

<span data-ttu-id="8a5bd-169">Pokud si projdete hello obsah nebo proc/drbd (`sudo cat /proc/drbd`) na oba virtuální počítače, měli byste vidět `Primary/Secondary` na `hadb01` a `Secondary/Primary` na `hadb02`a konzistentní s hello řešení v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-169">If you examine hello contents of /proc/drbd (`sudo cat /proc/drbd`) on both VMs, you should see `Primary/Secondary` on `hadb01` and `Secondary/Primary` on `hadb02`, consistent with hello solution at this point.</span></span> <span data-ttu-id="8a5bd-170">Hello 5-GB místa na disku se synchronizují přes síť 10.10.10.0/24 hello v žádné toocustomers zdarma.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-170">hello 5-GB disk is synchronized over hello 10.10.10.0/24 network at no charge toocustomers.</span></span>

<span data-ttu-id="8a5bd-171">Po synchronizaci hello disku můžete vytvořit systém souborů hello na `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-171">After hello disk is synchronized, you can create hello file system on `hadb01`.</span></span> <span data-ttu-id="8a5bd-172">Pro účely testování jsme použili ext2, ale hello následující kód vytvoří systém souborů ext3:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-172">For testing purposes, we used ext2, but hello following code will create an ext3 file system:</span></span>

    mkfs.ext3 /dev/drbd1

### <a name="mount-hello-drbd-resource"></a><span data-ttu-id="8a5bd-173">Připojte prostředek DRBD hello</span><span class="sxs-lookup"><span data-stu-id="8a5bd-173">Mount hello DRBD resource</span></span>
<span data-ttu-id="8a5bd-174">Jste nyní připraven toomount hello DRBD prostředky na `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-174">You're now ready toomount hello DRBD resources on `hadb01`.</span></span> <span data-ttu-id="8a5bd-175">Použití debian a odvozené konfigurace `/var/lib/mysql` jako adresář data na MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-175">Debian and derivatives use `/var/lib/mysql` as MySQL's data directory.</span></span> <span data-ttu-id="8a5bd-176">Protože jste nenainstalovali MySQL, vytvořit adresář hello a přípojných hello DRBD prostředků.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-176">Because you haven't installed MySQL, create hello directory and mount hello DRBD resource.</span></span> <span data-ttu-id="8a5bd-177">tooperform tuto možnost, spusťte následující kód hello `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-177">tooperform this option, run hello following code on `hadb01`:</span></span>

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a><span data-ttu-id="8a5bd-178">Nastavit MySQL</span><span class="sxs-lookup"><span data-stu-id="8a5bd-178">Set up MySQL</span></span>
<span data-ttu-id="8a5bd-179">Nyní jste připravené tooinstall MySQL na `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-179">Now you're ready tooinstall MySQL on `hadb01`:</span></span>

    sudo apt-get install mysql-server

<span data-ttu-id="8a5bd-180">Pro `hadb02`, máte dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-180">For `hadb02`, you have two options.</span></span> <span data-ttu-id="8a5bd-181">Můžete nainstalovat mysql-server, který bude vytvářet /var/lib/mysql, vyplnit nový adresář dat a pak odeberte hello obsah.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-181">You can install mysql-server, which will create /var/lib/mysql, fill it with a new data directory, and then remove hello contents.</span></span> <span data-ttu-id="8a5bd-182">tooperform tuto možnost, spusťte následující kód hello `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-182">tooperform this option, run hello following code on `hadb02`:</span></span>

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

<span data-ttu-id="8a5bd-183">Druhá možnost Hello je toofailover příliš`hadb02` a pak nainstalujte mysql-server existuje.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-183">hello second option is toofailover too`hadb02` and then install mysql-server there.</span></span> <span data-ttu-id="8a5bd-184">Skripty instalace si všimněte hello existující instalaci a nebude touch.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-184">Installation scripts will notice hello existing installation and won't touch it.</span></span>

<span data-ttu-id="8a5bd-185">Spuštění hello následující kód na `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-185">Run hello following code on `hadb01`:</span></span>

    sudo drbdadm secondary –force r0

<span data-ttu-id="8a5bd-186">Spuštění hello následující kód na `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-186">Run hello following code on `hadb02`:</span></span>

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

<span data-ttu-id="8a5bd-187">Pokud neplánujete toofailover DRBD nyní, první možnost hello je jednodušší, i když pravděpodobně méně elegantní.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-187">If you don't plan toofailover DRBD now, hello first option is easier although arguably less elegant.</span></span> <span data-ttu-id="8a5bd-188">Po nastavit tuto možnost, můžete začít pracovat ve vaší databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-188">After you set this up, you can start working on your MySQL database.</span></span> <span data-ttu-id="8a5bd-189">Spuštění hello následující kód na `hadb02` (nebo libovolného jeden z hello serverů je aktivní, podle tooDRBD):</span><span class="sxs-lookup"><span data-stu-id="8a5bd-189">Run hello following code on `hadb02` (or whichever one of hello servers is active, according tooDRBD):</span></span>

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* tooroot;

> [!WARNING]
> <span data-ttu-id="8a5bd-190">Tento poslední příkaz efektivně zakáže ověřování pro uživatele root hello v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-190">This last statement effectively disables authentication for hello root user in this table.</span></span> <span data-ttu-id="8a5bd-191">To by měl být nahrazen produkční úrovni udělit příkazy a je jen pro ilustraci zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-191">This should be replaced by your production-grade GRANT statements and is included only for illustrative purposes.</span></span>

<span data-ttu-id="8a5bd-192">Pokud chcete, aby dotazy toomake z virtuálních počítačů mimo hello (což je hello účel tohoto průvodce), musíte taky tooenable sítě pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-192">If you want toomake queries from outside hello VMs (which is hello purpose of this guide), you also need tooenable networking for MySQL.</span></span> <span data-ttu-id="8a5bd-193">Na oba virtuální počítače, otevřete `/etc/mysql/my.cnf` a přejděte příliš`bind-address`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-193">On both VMs, open `/etc/mysql/my.cnf` and go too`bind-address`.</span></span> <span data-ttu-id="8a5bd-194">Změna adresy hello z adresy 127.0.0.1 too0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-194">Change hello address from 127.0.0.1 too0.0.0.0.</span></span> <span data-ttu-id="8a5bd-195">Po uložení souboru hello, vydávání `sudo service mysql restart` na váš aktuální primární.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-195">After saving hello file, issue a `sudo service mysql restart` on your current primary.</span></span>

### <a name="create-hello-mysql-load-balanced-set"></a><span data-ttu-id="8a5bd-196">Vytvoření sady s vyrovnáváním zatížení MySQL hello</span><span class="sxs-lookup"><span data-stu-id="8a5bd-196">Create hello MySQL load-balanced set</span></span>
<span data-ttu-id="8a5bd-197">Přejděte zpět toohello portálu, přejděte příliš`hadb01`a zvolte **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-197">Go back toohello portal, go too`hadb01`, and choose **Endpoints**.</span></span> <span data-ttu-id="8a5bd-198">toocreate na koncový bod, MySQL (TCP 3306) vybírat hello rozevíracího seznamu a vyberte **s vyrovnáváním zatížení vytvořit nové**.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-198">toocreate an endpoint, choose MySQL (TCP 3306) from hello drop-down list and select **Create new load balanced set**.</span></span> <span data-ttu-id="8a5bd-199">Koncový bod Vyrovnávání zatížení na název hello `lb-mysql`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-199">Name hello load-balanced endpoint `lb-mysql`.</span></span> <span data-ttu-id="8a5bd-200">Nastavit **čas** too5 sekund, minimální.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-200">Set **Time** too5 seconds, minimum.</span></span>

<span data-ttu-id="8a5bd-201">Po vytvoření koncového bodu hello přejděte příliš`hadb02`, zvolte **koncové body**a vytvořit koncový bod.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-201">After you create hello endpoint, go too`hadb02`, choose **Endpoints**, and create an endpoint.</span></span> <span data-ttu-id="8a5bd-202">Zvolte `lb-mysql`a potom vyberte z rozevíracího seznamu hello MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-202">Choose `lb-mysql`, and then select MySQL from hello drop-down list.</span></span> <span data-ttu-id="8a5bd-203">Můžete taky hello rozhraní příkazového řádku Azure pro tento krok.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-203">You can also use hello Azure CLI for this step.</span></span>

<span data-ttu-id="8a5bd-204">Nyní máte všechny potřebné pro ruční operaci hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-204">You now have everything you need for manual operation of hello cluster.</span></span>

### <a name="test-hello-load-balanced-set"></a><span data-ttu-id="8a5bd-205">Testovací sady hello vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="8a5bd-205">Test hello load-balanced set</span></span>
<span data-ttu-id="8a5bd-206">Testy můžete provést z mimo počítač, pomocí libovolného klienta, MySQL, nebo pomocí některých aplikací, jako je phpMyAdmin spuštěna jako web Azure.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-206">Tests can be performed from an outside machine by using any MySQL client, or by using certain applications, like phpMyAdmin running as an Azure website.</span></span> <span data-ttu-id="8a5bd-207">V takovém případě použít nástroj příkazového řádku na MySQL na jiného pole Linux:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-207">In this case, you used MySQL's command-line tool on another Linux box:</span></span>

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a><span data-ttu-id="8a5bd-208">Ručně přebírání služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="8a5bd-208">Manually failing over</span></span>
<span data-ttu-id="8a5bd-209">Převzetí služeb při selhání můžete simulovat vypínání databáze MySQL, přepnutí na DRBD primární a znovu spustit MySQL.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-209">You can simulate failovers by shutting down MySQL, switching DRBD's primary, and starting MySQL again.</span></span>

<span data-ttu-id="8a5bd-210">tooperform této úlohy, spusťte následující kód na hadb01 hello:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-210">tooperform this task, run hello following code on hadb01:</span></span>

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

<span data-ttu-id="8a5bd-211">Pak klikněte na hadb02:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-211">Then, on hadb02:</span></span>

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

<span data-ttu-id="8a5bd-212">Po selhání ručně, můžete opakovat vzdálený dotaz a měli perfektně fungovat.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-212">After you fail over manually, you can repeat your remote query and it should work perfectly.</span></span>

## <a name="set-up-corosync"></a><span data-ttu-id="8a5bd-213">Nastavit Corosync</span><span class="sxs-lookup"><span data-stu-id="8a5bd-213">Set up Corosync</span></span>
<span data-ttu-id="8a5bd-214">Corosync je základní infrastruktura clusteru hello požadované pro kardiostimulátor toowork.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-214">Corosync is hello underlying cluster infrastructure required for Pacemaker toowork.</span></span> <span data-ttu-id="8a5bd-215">Pro zjišťování prezenčního signálu (a další metody jako Ultramonkey) je Corosync rozdělení hello CRM funkce, když kardiostimulátor zůstane více podobné tooHeartbeat funkcí.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-215">For Heartbeat (and other methodologies like Ultramonkey), Corosync is a split of hello CRM functionalities, while Pacemaker remains more similar tooHeartbeat in functionality.</span></span>

<span data-ttu-id="8a5bd-216">Hello hlavní omezení pro Corosync v Azure je Corosync upřednostní vícesměrového vysílání přes všesměrové vysílání přes komunikace jednosměrového vysílání, že Microsoft Azure sítě podporuje pouze jednosměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-216">hello main constraint for Corosync on Azure is that Corosync prefers multicast over broadcast over unicast communications, but Microsoft Azure networking only supports unicast.</span></span>

<span data-ttu-id="8a5bd-217">Naštěstí Corosync má pracovní režim jednosměrového vysílání.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-217">Fortunately, Corosync has a working unicast mode.</span></span> <span data-ttu-id="8a5bd-218">pouze skutečné omezení Hello je, že vzhledem k tomu, že všechny uzly nejsou komunikaci mezi sebou, toodefine hello uzly v konfiguračních souborech, včetně jejich IP adresy.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-218">hello only real constraint is that because all nodes are not communicating among themselves, you need toodefine hello nodes in your configuration files, including their IP addresses.</span></span> <span data-ttu-id="8a5bd-219">Můžeme použít hello Corosync příklad soubory pro jednosměrového vysílání a změňte vazby adresu, uzel seznamy a protokolování adresáře (Ubuntu používá `/var/log/corosync` při hello například souborů použijte `/var/log/cluster`) a povolit nástroje kvora.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-219">We can use hello Corosync example files for Unicast and change bind address, node lists, and logging directories (Ubuntu uses `/var/log/corosync` while hello example files use `/var/log/cluster`), and enable quorum tools.</span></span>

> [!NOTE]
> <span data-ttu-id="8a5bd-220">Použijte hello `transport: udpu` směrnice a hello ručně definovaná IP adresy pro oba uzly.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-220">Use hello following `transport: udpu` directive and hello manually defined IP addresses for both nodes.</span></span>

<span data-ttu-id="8a5bd-221">Spuštění hello následující kód na `/etc/corosync/corosync.conf` pro oba uzly:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-221">Run hello following code on `/etc/corosync/corosync.conf` for both nodes:</span></span>

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

<span data-ttu-id="8a5bd-222">Zkopírujte tento konfigurační soubor na oba virtuální počítače a spustit Corosync v obou uzlů:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-222">Copy this configuration file on both VMs and start Corosync in both nodes:</span></span>

    sudo service start corosync

<span data-ttu-id="8a5bd-223">Krátce po spuštění služby hello, by se mělo vytvořit hello cluster v aktuální prstenec hello a by měl být vytvářen kvora.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-223">Shortly after starting hello service, hello cluster should be established in hello current ring, and quorum should be constituted.</span></span> <span data-ttu-id="8a5bd-224">Tato funkce jsme můžete zkontrolovat kontrolou protokolů nebo spuštěním hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-224">We can check this functionality by reviewing logs or by running hello following code:</span></span>

    sudo corosync-quorumtool –l

<span data-ttu-id="8a5bd-225">Zobrazí se výstup podobný toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-225">You will see output similar toohello following image:</span></span>

![corosync quorumtool - l ukázkový výstup](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a><span data-ttu-id="8a5bd-227">Nastavit kardiostimulátor</span><span class="sxs-lookup"><span data-stu-id="8a5bd-227">Set up Pacemaker</span></span>
<span data-ttu-id="8a5bd-228">Používá kardiostimulátor hello toomonitor clusteru pro prostředky, definují, kdy základní barvy přejděte a přepínače toosecondaries tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-228">Pacemaker uses hello cluster toomonitor for resources, define when primaries go down, and switch those resources toosecondaries.</span></span> <span data-ttu-id="8a5bd-229">Prostředky lze definovat ze sady skriptů, k dispozici nebo z LSB skripty (init jako), mezi další možnosti.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-229">Resources can be defined from a set of available scripts or from LSB (init-like) scripts, among other choices.</span></span>

<span data-ttu-id="8a5bd-230">Chceme kardiostimulátor prostředek DRBD příliš "vlastní" hello, hello přípojného bodu a službu MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-230">We want Pacemaker too"own" hello DRBD resource, hello mount point, and hello MySQL service.</span></span> <span data-ttu-id="8a5bd-231">Pokud kardiostimulátor můžete zapnout a vypnout DRBD, připojit a odpojit a pak spustit a zastavit MySQL v hello správné pořadí při něco chybný se stane s hello primární, instalace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-231">If Pacemaker can turn on and off DRBD, mount and unmount it, and then start and stop MySQL in hello right order when something bad happens with hello primary, setup is complete.</span></span>

<span data-ttu-id="8a5bd-232">Při první instalaci kardiostimulátor, musí být dostatečně jednoduchá něco jako konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-232">When you first install Pacemaker, your configuration should be simple enough, something like:</span></span>

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. <span data-ttu-id="8a5bd-233">Zkontrolujte konfiguraci hello spuštěním `sudo crm configure show`.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-233">Check hello configuration by running `sudo crm configure show`.</span></span>
2. <span data-ttu-id="8a5bd-234">Pak vytvořte soubor (například `/tmp/cluster.conf`) s hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-234">Then create a file (like `/tmp/cluster.conf`) with hello following resources:</span></span>

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

3. <span data-ttu-id="8a5bd-235">Načtení souboru hello do konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-235">Load hello file into hello configuration.</span></span> <span data-ttu-id="8a5bd-236">Potřebujete jenom toodo to v jednom uzlu.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-236">You only need toodo this in one node.</span></span>

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. <span data-ttu-id="8a5bd-237">Ujistěte se, že kardiostimulátor začíná na spouštění v obou uzlů:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-237">Make sure that Pacemaker starts at boot in both nodes:</span></span>

        sudo update-rc.d pacemaker defaults

5. <span data-ttu-id="8a5bd-238">Pomocí `sudo crm_mon –L`, ověřte, že jeden z uzlů se stal hello hlavní hello clusteru a běží všechny prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-238">By using `sudo crm_mon –L`, verify that one of your nodes has become hello master for hello cluster and is running all hello resources.</span></span> <span data-ttu-id="8a5bd-239">Můžete vytvořit připojení a ps toocheck spuštěným hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-239">You can use mount and ps toocheck that hello resources are running.</span></span>

<span data-ttu-id="8a5bd-240">Následující snímek obrazovky ukazuje Hello `crm_mon` s jedním uzlem zastavena (ukončení tak, že vyberete kombinaci kláves Ctrl + C):</span><span class="sxs-lookup"><span data-stu-id="8a5bd-240">hello following screenshot shows `crm_mon` with one node stopped (exit by selecting Ctrl+C):</span></span>

![uzel crm_mon zastavena](./media/mysql-cluster/image002.png)

<span data-ttu-id="8a5bd-242">Tento snímek obrazovky ukazuje uzly, jeden z nich a jeden podřízený:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-242">This screenshot shows both nodes, one master and one slave:</span></span>

![podřízený provozní hlavní crm_mon](./media/mysql-cluster/image003.png)

## <a name="testing"></a><span data-ttu-id="8a5bd-244">Testování</span><span class="sxs-lookup"><span data-stu-id="8a5bd-244">Testing</span></span>
<span data-ttu-id="8a5bd-245">Jste připraveni simulaci automatické převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-245">You're ready for an automatic failover simulation.</span></span> <span data-ttu-id="8a5bd-246">Existují dva způsoby toodo to: a nepodmíněných.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-246">There are two ways toodo this: soft and hard.</span></span>

<span data-ttu-id="8a5bd-247">Hello logicky způsob využívá funkce vypnutí hello cluster: ``crm_standby -U `uname -n` -v on``.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-247">hello soft way uses hello cluster's shutdown function: ``crm_standby -U `uname -n` -v on``.</span></span> <span data-ttu-id="8a5bd-248">Pokud použijete toto na hlavní server hello, podřízený hello má.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-248">If you use this on hello master, hello slave takes over.</span></span> <span data-ttu-id="8a5bd-249">Mějte na paměti tooset tento back toooff.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-249">Remember tooset this back toooff.</span></span> <span data-ttu-id="8a5bd-250">Pokud to neuděláte, crm_mon zobrazí jeden uzel do úsporného režimu.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-250">If you don't, crm_mon will show one node on standby.</span></span>

<span data-ttu-id="8a5bd-251">Hello pevný způsob, jak se vypíná dolů hello primárního virtuálního počítače (hadb01) přes portál hello nebo změnou hello runlevel na hello virtuálního počítače (tj, zastavení, vypnutí).</span><span class="sxs-lookup"><span data-stu-id="8a5bd-251">hello hard way is shutting down hello primary VM (hadb01) via hello portal or by changing hello runlevel on hello VM (that is, halt, shutdown).</span></span> <span data-ttu-id="8a5bd-252">To pomáhá Corosync a kardiostimulátor podle signalizace danou hello předlohu probíhající dolů.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-252">This helps Corosync and Pacemaker by signaling that hello master's going down.</span></span> <span data-ttu-id="8a5bd-253">Toto můžete otestovat (vhodný pro údržbu), ale můžete vynutit hello scénář zmrazené hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-253">You can test this (useful for maintenance windows), but you can also force hello scenario by freezing hello VM.</span></span>

## <a name="stonith"></a><span data-ttu-id="8a5bd-254">STONITH</span><span class="sxs-lookup"><span data-stu-id="8a5bd-254">STONITH</span></span>
<span data-ttu-id="8a5bd-255">By mělo být možné tooissue vypnutí virtuálního počítače prostřednictvím rozhraní příkazového řádku Azure hello místo STONITH skript, který řídí fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-255">It should be possible tooissue a VM shutdown via hello Azure CLI in lieu of a STONITH script that controls a physical device.</span></span> <span data-ttu-id="8a5bd-256">Můžete použít `/usr/lib/stonith/plugins/external/ssh` jako základní a povolit STONITH v konfiguraci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-256">You can use `/usr/lib/stonith/plugins/external/ssh` as a base and enable STONITH in hello cluster's configuration.</span></span> <span data-ttu-id="8a5bd-257">Rozhraní příkazového řádku Azure by měly být globálně nainstalovány a hello nastavení publikování a by měly být načteny profilu pro uživatele hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-257">Azure CLI should be globally installed, and hello publish settings and profile should be loaded for hello cluster's user.</span></span>

<span data-ttu-id="8a5bd-258">Ukázkový kód pro prostředek hello je k dispozici na [Githubu](https://github.com/bureado/aztonith).</span><span class="sxs-lookup"><span data-stu-id="8a5bd-258">Sample code for hello resource is available on [GitHub](https://github.com/bureado/aztonith).</span></span> <span data-ttu-id="8a5bd-259">Změnit konfiguraci clusteru hello přidáním hello následující příliš`sudo crm configure`:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-259">Change hello cluster's configuration by adding hello following too`sudo crm configure`:</span></span>

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> <span data-ttu-id="8a5bd-260">skript Hello neprovede nahoru/dolů kontroly.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-260">hello script doesn't perform up/down checks.</span></span> <span data-ttu-id="8a5bd-261">původní prostředků SSH Hello měl 15 příkaz ping kontroluje, ale čas obnovení pro virtuální počítač Azure může být další proměnná.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-261">hello original SSH resource had 15 ping checks, but recovery time for an Azure VM might be more variable.</span></span>

## <a name="limitations"></a><span data-ttu-id="8a5bd-262">Omezení</span><span class="sxs-lookup"><span data-stu-id="8a5bd-262">Limitations</span></span>
<span data-ttu-id="8a5bd-263">použít Hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-263">hello following limitations apply:</span></span>

* <span data-ttu-id="8a5bd-264">Hello linbit DRBD prostředků skript, který spravuje DRBD jako prostředek v kardiostimulátor používá `drbdadm down` při vypnutí uzlu, i v případě, že uzel hello se právě děje pohotovostní režim.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-264">hello linbit DRBD resource script that manages DRBD as a resource in Pacemaker uses `drbdadm down` when shutting down a node, even if hello node is just going on standby.</span></span> <span data-ttu-id="8a5bd-265">Toto není ideální protože hello podřízený nebude možné synchronizaci hello DRBD prostředků při hello hlavní získá zápisy.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-265">This is not ideal because hello slave will not be synchronizing hello DRBD resource while hello master gets writes.</span></span> <span data-ttu-id="8a5bd-266">Pokud hlavní hello neselže zdvořile, podřízený hello může trvat přes starší stav systému souborů.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-266">If hello master does not fail graciously, hello slave can take over an older file system state.</span></span> <span data-ttu-id="8a5bd-267">Existují dva způsoby potenciální toto řešení:</span><span class="sxs-lookup"><span data-stu-id="8a5bd-267">There are two potential ways of solving this:</span></span>
  * <span data-ttu-id="8a5bd-268">Vynucení `drbdadm up r0` ve všech uzlech clusteru prostřednictvím místní sledovací zařízení (ne clusterized)</span><span class="sxs-lookup"><span data-stu-id="8a5bd-268">Enforcing a `drbdadm up r0` in all cluster nodes via a local (not clusterized) watchdog</span></span>
  * <span data-ttu-id="8a5bd-269">Úpravy hello linbit DRBD skript, a ověřte, zda `down` není volán`/usr/lib/ocf/resource.d/linbit/drbd`</span><span class="sxs-lookup"><span data-stu-id="8a5bd-269">Editing hello linbit DRBD script, making sure that `down` is not called in `/usr/lib/ocf/resource.d/linbit/drbd`</span></span>
* <span data-ttu-id="8a5bd-270">Nástroj pro vyrovnávání zatížení Hello potřebuje toorespond alespoň pět sekund, takže aplikace by měla být clustery a být větší toleranci vůči časový limit.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-270">hello load balancer needs at least five seconds toorespond, so applications should be cluster-aware and be more tolerant of timeout.</span></span> <span data-ttu-id="8a5bd-271">Jiné architektury, jako v aplikaci fronty a middlewares dotaz, může také pomoct.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-271">Other architectures, like in-app queues and query middlewares, can also help.</span></span>
* <span data-ttu-id="8a5bd-272">Ladění MySQL je nezbytné tooensure, které se provádí zápis spravovat tempem a mezipaměti jsou vyprázdněn toodisk často toominimize možné ztrátě paměti.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-272">MySQL tuning is necessary tooensure that writing is done at a manageable pace and caches are flushed toodisk as frequently as possible toominimize memory loss.</span></span>
* <span data-ttu-id="8a5bd-273">Zápis výkonu je závislý na virtuální počítač propojení ve virtuálním přepínači hello, protože se jedná hello mechanismus používaný DRBD tooreplicate hello zařízením.</span><span class="sxs-lookup"><span data-stu-id="8a5bd-273">Write performance is dependent in VM interconnect in hello virtual switch because this is hello mechanism used by DRBD tooreplicate hello device.</span></span>
