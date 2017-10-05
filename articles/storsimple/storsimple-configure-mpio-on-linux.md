---
title: "Konfigurace funkce MPIO na hostiteli systému StorSimple Linux | Microsoft Docs"
description: "Konfigurace funkce MPIO na StorSimple připojené k Linux hostitele se systémem CentOS 6.6"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: add539351066f9ff94febeebfd5334773b360e8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a><span data-ttu-id="87035-103">Konfigurace funkce MPIO na StorSimple hostitele se systémem CentOS</span><span class="sxs-lookup"><span data-stu-id="87035-103">Configure MPIO on a StorSimple host running CentOS</span></span>
<span data-ttu-id="87035-104">Tento článek vysvětluje kroky nutné ke konfiguraci více cest vstupně-výstupní operace (MPIO) na serveru hostitele Centos 6.6.</span><span class="sxs-lookup"><span data-stu-id="87035-104">This article explains the steps required to configure Multipathing IO (MPIO) on your Centos 6.6 host server.</span></span> <span data-ttu-id="87035-105">Hostitelský server je připojená k zařízení s Microsoft Azure StorSimple pro vysokou dostupnost prostřednictvím iniciátory iSCSI.</span><span class="sxs-lookup"><span data-stu-id="87035-105">The host server is connected to your Microsoft Azure StorSimple device for high availability via iSCSI initiators.</span></span> <span data-ttu-id="87035-106">Je podrobně popisuje automatické zjišťování vícenásobný zařízení a konkrétní nastavení pouze pro svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-106">It describes in detail the automatic discovery of multipath devices and the specific setup only for StorSimple volumes.</span></span>

<span data-ttu-id="87035-107">Tento postup se vztahuje na všechny modely řadu zařízení StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="87035-107">This procedure is applicable to all the models of StorSimple 8000 series devices.</span></span>

> [!NOTE]
> <span data-ttu-id="87035-108">Tento postup nelze použít pro virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-108">This procedure cannot be used for a StorSimple virtual device.</span></span> <span data-ttu-id="87035-109">Další informace najdete v tématu Jak nakonfigurovat servery hostitele pro virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="87035-109">For more information, see how to configure host servers for your virtual device.</span></span>
> 
> 

## <a name="about-multipathing"></a><span data-ttu-id="87035-110">O vytváření více cest</span><span class="sxs-lookup"><span data-stu-id="87035-110">About multipathing</span></span>
<span data-ttu-id="87035-111">Tato funkce více cest můžete konfigurovat více vstupně-výstupních cest mezi hostitelským serverem a zařízením úložiště.</span><span class="sxs-lookup"><span data-stu-id="87035-111">The multipathing feature allows you to configure multiple I/O paths between a host server and a storage device.</span></span> <span data-ttu-id="87035-112">Tyto cesty vstupně-výstupní operace jsou fyzické připojení SAN, která může zahrnovat samostatné kabely, přepínače, síťových rozhraní a řadiče.</span><span class="sxs-lookup"><span data-stu-id="87035-112">These I/O paths are physical SAN connections that can include separate cables, switches, network interfaces, and controllers.</span></span> <span data-ttu-id="87035-113">Více cest agreguje vstupně-výstupních cest ke konfiguraci nové zařízení, která souvisí s všechny agregované cesty.</span><span class="sxs-lookup"><span data-stu-id="87035-113">Multipathing aggregates the I/O paths, to configure a new device that is associated with all of the aggregated paths.</span></span>

<span data-ttu-id="87035-114">Účelem více cest je dvojí:</span><span class="sxs-lookup"><span data-stu-id="87035-114">The purpose of multipathing is two-fold:</span></span>

* <span data-ttu-id="87035-115">**Vysoká dostupnost**: poskytuje alternativní cestu, pokud se nezdaří libovolný element vstupně-výstupní cestu (například kabel, přepínače, síťové nebo řadič).</span><span class="sxs-lookup"><span data-stu-id="87035-115">**High availability**: It provides an alternate path if any element of the I/O path (such as a cable, switch, network interface, or controller) fails.</span></span>
* <span data-ttu-id="87035-116">**Vyrovnávání zatížení**: v závislosti na konfiguraci zařízení úložiště, můžete zvýšit výkon zjišťování zatížením na vstupně-výstupních cest a dynamicky vyrovnává těchto zatížení.</span><span class="sxs-lookup"><span data-stu-id="87035-116">**Load balancing**: Depending on the configuration of your storage device, it can improve the performance by detecting loads on the I/O paths and dynamically rebalancing those loads.</span></span>

### <a name="about-multipathing-components"></a><span data-ttu-id="87035-117">O komponentách více cest</span><span class="sxs-lookup"><span data-stu-id="87035-117">About multipathing components</span></span>
<span data-ttu-id="87035-118">Více cest v systému Linux se skládá z jádra a komponenty uživatelského prostoru jako v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="87035-118">Multipathing in Linux consists of kernel components and user-space components as tabulated below.</span></span>

* <span data-ttu-id="87035-119">**Jádra**: hlavní součást je *zařízení mapper* , bude přesměrována vstupně-výstupních operací a podporuje převzetí služeb při selhání pro cesty a cesty skupiny.</span><span class="sxs-lookup"><span data-stu-id="87035-119">**Kernel**: The main component is the *device-mapper* that reroutes I/O and supports failover for paths and path groups.</span></span>

* <span data-ttu-id="87035-120">**Uživatel místo**: Jedná se o *multipath nástroje* , správě zařízení multipathed instruující vícenásobný modul Mapovač zařízení, co dělat.</span><span class="sxs-lookup"><span data-stu-id="87035-120">**User-space**: These are *multipath-tools* that manage multipathed devices by instructing the device-mapper multipath module what to do.</span></span> <span data-ttu-id="87035-121">Nástroje obsahovat:</span><span class="sxs-lookup"><span data-stu-id="87035-121">The tools consist of:</span></span>
   
   * <span data-ttu-id="87035-122">**Funkce Multipath**: uvádí a konfiguruje multipathed zařízení.</span><span class="sxs-lookup"><span data-stu-id="87035-122">**Multipath**: lists and configures multipathed devices.</span></span>
   * <span data-ttu-id="87035-123">**Multipathd**: démon, která se spustí multipath a monitoruje cesty.</span><span class="sxs-lookup"><span data-stu-id="87035-123">**Multipathd**: daemon that executes multipath and monitors the paths.</span></span>
   * <span data-ttu-id="87035-124">**Název Devmap**: poskytuje smysluplný název zařízení pro proces udev pro devmaps.</span><span class="sxs-lookup"><span data-stu-id="87035-124">**Devmap-name**: provides a meaningful device-name to udev for devmaps.</span></span>
   * <span data-ttu-id="87035-125">**Kpartx**: mapuje lineární devmaps zařízení oddíly, aby rozdělený vícenásobný mapy.</span><span class="sxs-lookup"><span data-stu-id="87035-125">**Kpartx**: maps linear devmaps to device partitions to make multipath maps partitionable.</span></span>
   * <span data-ttu-id="87035-126">**Multipath.conf**: konfigurační soubor pro více cest démon procesu, který se používá k přepsání předdefinovaných konfigurace tabulky.</span><span class="sxs-lookup"><span data-stu-id="87035-126">**Multipath.conf**: configuration file for multipath daemon that is used to overwrite the built-in configuration table.</span></span>

### <a name="about-the-multipathconf-configuration-file"></a><span data-ttu-id="87035-127">O konfiguračního souboru multipath.conf</span><span class="sxs-lookup"><span data-stu-id="87035-127">About the multipath.conf configuration file</span></span>
<span data-ttu-id="87035-128">Konfigurační soubor `/etc/multipath.conf` umožňuje mnoho funkcí více cest uživatelsky konfigurovatelného.</span><span class="sxs-lookup"><span data-stu-id="87035-128">The configuration file `/etc/multipath.conf` makes many of the multipathing features user-configurable.</span></span> <span data-ttu-id="87035-129">`multipath` Příkaz a démon jádra `multipathd` použijte informace v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="87035-129">The `multipath` command and the kernel daemon `multipathd` use information found in this file.</span></span> <span data-ttu-id="87035-130">Soubor je konzultaci pouze při konfiguraci funkce multipath zařízení.</span><span class="sxs-lookup"><span data-stu-id="87035-130">The file is consulted only during the configuration of the multipath devices.</span></span> <span data-ttu-id="87035-131">Ujistěte se, že jsou všechny změny provedené před spuštěním `multipath` příkaz.</span><span class="sxs-lookup"><span data-stu-id="87035-131">Make sure that all changes are made before you run the `multipath` command.</span></span> <span data-ttu-id="87035-132">Pokud upravíte soubor později, musíte zastavit a spustit multipathd znovu pro změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="87035-132">If you modify the file afterwards, you will need to stop and start multipathd again for the changes to take effect.</span></span>

<span data-ttu-id="87035-133">Multipath.conf má pět částí:</span><span class="sxs-lookup"><span data-stu-id="87035-133">The multipath.conf has five sections:</span></span>

- <span data-ttu-id="87035-134">**Výchozí nastavení na úrovni systému** *(výchozí)*: můžete přepsat výchozí nastavení na úrovni systému.</span><span class="sxs-lookup"><span data-stu-id="87035-134">**System level defaults** *(defaults)*: You can override system level defaults.</span></span>
- <span data-ttu-id="87035-135">**Zakázáno zařízení** *(černý list)*: můžete zadat seznam zařízení, které by neměly být řízené zařízení mapper.</span><span class="sxs-lookup"><span data-stu-id="87035-135">**Blacklisted devices** *(blacklist)*: You can specify the list of devices that should not be controlled by device-mapper.</span></span>
- <span data-ttu-id="87035-136">**Blokovaných výjimky** *(blacklist_exceptions)*: můžete identifikovat konkrétní zařízení jsou považovány za vícenásobný zařízení i v případě, že uvedené v blacklist.</span><span class="sxs-lookup"><span data-stu-id="87035-136">**Blacklist exceptions** *(blacklist_exceptions)*: You can identify specific devices to be treated as multipath devices even if listed in the blacklist.</span></span>
- <span data-ttu-id="87035-137">**Konkrétní nastavení řadiče úložiště** *(zařízení)*: můžete zadat nastavení konfigurace, které budou použity na zařízení, které mají dodavatele a informace o produktu.</span><span class="sxs-lookup"><span data-stu-id="87035-137">**Storage controller specific settings** *(devices)*: You can specify configuration settings that will be applied to devices that have Vendor and Product information.</span></span>
- <span data-ttu-id="87035-138">**Nastavení pro konkrétní zařízení** *(multipaths)*: v této části můžete doladit nastavení konfigurace pro jednotlivé logické jednotky.</span><span class="sxs-lookup"><span data-stu-id="87035-138">**Device specific settings** *(multipaths)*: You can use this section to fine-tune the configuration settings for individual LUNs.</span></span>

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a><span data-ttu-id="87035-139">Konfigurace více cest na StorSimple připojený k hostiteli systému Linux</span><span class="sxs-lookup"><span data-stu-id="87035-139">Configure multipathing on StorSimple connected to Linux host</span></span>
<span data-ttu-id="87035-140">Zařízení StorSimple připojený k hostiteli systému Linux lze nakonfigurovat pro vysokou dostupnost a vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="87035-140">A StorSimple device connected to a Linux host can be configured for high availability and load balancing.</span></span> <span data-ttu-id="87035-141">Například pokud má hostitel Linux dvě rozhraní, které jsou připojené k síti SAN a zařízení má dvě rozhraní, které jsou připojené k síti SAN, tak, aby tato rozhraní jsou ve stejné podsíti, pak bude 4 cesty k dispozici.</span><span class="sxs-lookup"><span data-stu-id="87035-141">For example, if the Linux host has two interfaces connected to the SAN and the device has two interfaces connected to the SAN such that these interfaces are on the same subnet, then there will be 4 paths available.</span></span> <span data-ttu-id="87035-142">Ale pokud každé rozhraní DATA na zařízení a hostitele rozhraní jsou v jiné podsíti protokolu IP (a ne směrovatelné), pak pouze 2 cesty nebudou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="87035-142">However, if each DATA interface on the device and host interface are on a different IP subnet (and not routable), then only 2 paths will be available.</span></span> <span data-ttu-id="87035-143">Můžete nakonfigurovat více cest automaticky zjistit všechny cesty k dispozici, zvolte algoritmu Vyrovnávání zatížení u těchto cest, použít specifické nastavení pro svazky jen StorSimple a potom povolit a ověřit více cest.</span><span class="sxs-lookup"><span data-stu-id="87035-143">You can configure multipathing to automatically discover all the available paths, choose a load-balancing algorithm for those paths, apply specific configuration settings for StorSimple-only volumes, and then enable and verify multipathing.</span></span>

<span data-ttu-id="87035-144">Následující postup popisuje, jak nakonfigurovat více cest při připojení zařízení StorSimple se dvěma síťovými rozhraními na hostitele se dvěma síťovými rozhraními.</span><span class="sxs-lookup"><span data-stu-id="87035-144">The following procedure describes how to configure multipathing when a StorSimple device with two network interfaces is connected to a host with two network interfaces.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87035-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87035-145">Prerequisites</span></span>
<span data-ttu-id="87035-146">Tato část podrobně popisuje požadavky konfigurace pro CentOS server a zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-146">This section details the configuration prerequisites for CentOS server and your StorSimple device.</span></span>

### <a name="on-centos-host"></a><span data-ttu-id="87035-147">Na hostiteli CentOS</span><span class="sxs-lookup"><span data-stu-id="87035-147">On CentOS host</span></span>
1. <span data-ttu-id="87035-148">Ujistěte se, že má váš hostitel CentOS 2 síťových rozhraní, které jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="87035-148">Make sure that your CentOS host has 2 network interfaces enabled.</span></span> <span data-ttu-id="87035-149">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-149">Type:</span></span>
   
    `ifconfig`
   
    <span data-ttu-id="87035-150">Následující příklad ukazuje výstup, pokud dva síťové rozhraní (`eth0` a `eth1`) jsou k dispozici na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="87035-150">The following example shows the output when two network interfaces (`eth0` and `eth1`) are present on the host.</span></span>
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. <span data-ttu-id="87035-151">Nainstalujte *iSCSI. iniciátoru utils* na vašem serveru CentOS.</span><span class="sxs-lookup"><span data-stu-id="87035-151">Install *iSCSI-initiator-utils* on your CentOS server.</span></span> <span data-ttu-id="87035-152">Proveďte následující kroky k instalaci *iSCSI. iniciátoru utils*.</span><span class="sxs-lookup"><span data-stu-id="87035-152">Perform the following steps to install *iSCSI-initiator-utils*.</span></span>
   
   1. <span data-ttu-id="87035-153">Přihlaste se jako `root` do svého CentOS hostitele.</span><span class="sxs-lookup"><span data-stu-id="87035-153">Log on as `root` into your CentOS host.</span></span>
   2. <span data-ttu-id="87035-154">Nainstalujte *iSCSI. iniciátoru utils*.</span><span class="sxs-lookup"><span data-stu-id="87035-154">Install the *iSCSI-initiator-utils*.</span></span> <span data-ttu-id="87035-155">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-155">Type:</span></span>
      
       `yum install iscsi-initiator-utils`
   3. <span data-ttu-id="87035-156">Po *iSCSI. iniciátoru utils* je úspěšně nainstalována, spusťte službu iSCSI.</span><span class="sxs-lookup"><span data-stu-id="87035-156">After the *iSCSI-Initiator-utils* is successfully installed, start the iSCSI service.</span></span> <span data-ttu-id="87035-157">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-157">Type:</span></span>
      
       `service iscsid start`
      
       <span data-ttu-id="87035-158">V případech `iscsid` ve skutečnosti se nemusí spustit a `--force` možnost může být potřeba.</span><span class="sxs-lookup"><span data-stu-id="87035-158">On occasions, `iscsid` may not actually start and the `--force` option may be needed</span></span>
   4. <span data-ttu-id="87035-159">K zajištění, že vaše iniciátor iSCSI je povolena při spuštění, použijte `chkconfig` příkaz, který povolí službu.</span><span class="sxs-lookup"><span data-stu-id="87035-159">To ensure that your iSCSI initiator is enabled during boot time, use the `chkconfig` command to enable the service.</span></span>
      
       `chkconfig iscsi on`
   5. <span data-ttu-id="87035-160">Pokud chcete ověřit, aby byla správně nastavit, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="87035-160">To verify that that it was properly setup, run the command:</span></span>
      
       `chkconfig --list | grep iscsi`
      
       <span data-ttu-id="87035-161">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="87035-161">A sample output is shown below.</span></span>
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       <span data-ttu-id="87035-162">Z výše uvedeném příkladu vidíte, že prostředí iSCSI bude spouštět na spuštění spuštění úrovně 2, 3, 4 a 5.</span><span class="sxs-lookup"><span data-stu-id="87035-162">From the above example, you can see that your iSCSI environment will run on boot time on run levels 2, 3, 4, and 5.</span></span>
3. <span data-ttu-id="87035-163">Nainstalujte *zařízení. mapper multipath*.</span><span class="sxs-lookup"><span data-stu-id="87035-163">Install *device-mapper-multipath*.</span></span> <span data-ttu-id="87035-164">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-164">Type:</span></span>
   
    `yum install device-mapper-multipath`
   
    <span data-ttu-id="87035-165">Spustí se instalace.</span><span class="sxs-lookup"><span data-stu-id="87035-165">The installation will start.</span></span> <span data-ttu-id="87035-166">Typ **Y** pokračovat po zobrazení výzvy k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="87035-166">Type **Y** to continue when prompted for confirmation.</span></span>

### <a name="on-storsimple-device"></a><span data-ttu-id="87035-167">Na zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="87035-167">On StorSimple device</span></span>
<span data-ttu-id="87035-168">Zařízení StorSimple musí mít:</span><span class="sxs-lookup"><span data-stu-id="87035-168">Your StorSimple device should have:</span></span>

* <span data-ttu-id="87035-169">Minimálně dvě rozhraní povolená pro iSCSI.</span><span class="sxs-lookup"><span data-stu-id="87035-169">A minimum of two interfaces enabled for iSCSI.</span></span> <span data-ttu-id="87035-170">Pokud chcete ověřit, zda jsou dvě rozhraní iSCSI povolený v zařízení StorSimple, proveďte následující kroky na portálu Azure classic pro zařízení StorSimple:</span><span class="sxs-lookup"><span data-stu-id="87035-170">To verify that two interfaces are iSCSI-enabled on your StorSimple device, perform the following steps in the Azure classic portal for your StorSimple device:</span></span>
  
  1. <span data-ttu-id="87035-171">Přihlaste se ke klasickému portálu pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-171">Log into the classic portal for your StorSimple device.</span></span>
  2. <span data-ttu-id="87035-172">Vybrat služby StorSimple Manager, klikněte na tlačítko **zařízení** a zvolte konkrétní zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-172">Select your StorSimple Manager service, click **Devices** and choose the specific StorSimple device.</span></span> <span data-ttu-id="87035-173">Klikněte na tlačítko **konfigurace** a ověřte nastavení síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="87035-173">Click **Configure** and verify the network interface settings.</span></span> <span data-ttu-id="87035-174">Snímek obrazovky s dvě rozhraní iSCSI povolený sítě jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="87035-174">A screenshot with two iSCSI-enabled network interfaces is shown below.</span></span> <span data-ttu-id="87035-175">Sem DATA 2 a DATA 3, oba 10 GbE je povoleno rozhraní iSCSI.</span><span class="sxs-lookup"><span data-stu-id="87035-175">Here DATA 2 and DATA 3, both 10 GbE interfaces are enabled for iSCSI.</span></span>
     
      ![Konfigurace funkce MPIO StorsSimple DATA 2](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![Funkce MPIO StorSimple dat 3 konfigurace](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      <span data-ttu-id="87035-178">V **konfigurace** stránky</span><span class="sxs-lookup"><span data-stu-id="87035-178">In the **Configure** page</span></span>
     
     1. <span data-ttu-id="87035-179">Zajistěte, aby obě síťová rozhraní iSCSI povolený.</span><span class="sxs-lookup"><span data-stu-id="87035-179">Ensure that both network interfaces are iSCSI-enabled.</span></span> <span data-ttu-id="87035-180">**ISCSI povoleno** pole musí být nastavena na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="87035-180">The **iSCSI enabled** field should be set to **Yes**.</span></span>
     2. <span data-ttu-id="87035-181">Zkontrolujte, zda síťových rozhraní má stejnou rychlostí, jak by měla být 1 GbE nebo 10 GbE.</span><span class="sxs-lookup"><span data-stu-id="87035-181">Ensure that the network interfaces have the same speed, both should be 1 GbE or 10 GbE.</span></span>
     3. <span data-ttu-id="87035-182">Poznámka: adresy IPv4 rozhraní iSCSI povolený a uložit pro pozdější použití na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="87035-182">Note the IPv4 addresses of the iSCSI-enabled interfaces and save for later use on the host.</span></span>
* <span data-ttu-id="87035-183">Na rozhraní iSCSI v zařízení StorSimple by měl být dostupný ze serveru, CentOS.</span><span class="sxs-lookup"><span data-stu-id="87035-183">The iSCSI interfaces on your StorSimple device should be reachable from the CentOS server.</span></span>
      <span data-ttu-id="87035-184">Chcete-li to ověřit, zadejte IP adresy vašich rozhraní iSCSI povolený sítě StorSimple na hostitelském serveru.</span><span class="sxs-lookup"><span data-stu-id="87035-184">To verify this, you need to provide the IP addresses of your StorSimple iSCSI-enabled network interfaces on your host server.</span></span> <span data-ttu-id="87035-185">Příkazy používají a odpovídající výstup s DATA2 (10.126.162.25) a DATA3 (10.126.162.26) jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="87035-185">The commands used and the corresponding output with DATA2 (10.126.162.25) and DATA3 (10.126.162.26) is shown below:</span></span>
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a><span data-ttu-id="87035-186">Konfigurace hardwaru</span><span class="sxs-lookup"><span data-stu-id="87035-186">Hardware configuration</span></span>
<span data-ttu-id="87035-187">Doporučujeme, abyste připojení dvě síťová rozhraní iSCSI, na samostatné cesty pro redundanci.</span><span class="sxs-lookup"><span data-stu-id="87035-187">We recommend that you connect the two iSCSI network interfaces on separate paths for redundancy.</span></span> <span data-ttu-id="87035-188">Následující obrázek znázorňuje doporučené hardwarové konfigurace pro vysokou dostupnost a vyrovnávaní zatížení více cest pro váš CentOS server a zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-188">The figure below shows the recommended hardware configuration for high availability and load-balancing multipathing for your CentOS server and StorSimple device.</span></span>

![Hardwarová konfigurace funkce MPIO pro zařízení StorSimple na hostitele platformy Linux](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

<span data-ttu-id="87035-190">Jak je vidět na předchozím obrázku:</span><span class="sxs-lookup"><span data-stu-id="87035-190">As shown in the preceding figure:</span></span>

* <span data-ttu-id="87035-191">Zařízení StorSimple je v konfiguraci aktivní pasivní s dva řadiče.</span><span class="sxs-lookup"><span data-stu-id="87035-191">Your StorSimple device is in an active-passive configuration with two controllers.</span></span>
* <span data-ttu-id="87035-192">Dva přepínače sítě SAN jsou připojené k vašim řadičům zařízení.</span><span class="sxs-lookup"><span data-stu-id="87035-192">Two SAN switches are connected to your device controllers.</span></span>
* <span data-ttu-id="87035-193">Dva iniciátory iSCSI jsou povolené v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-193">Two iSCSI initiators are enabled on your StorSimple device.</span></span>
* <span data-ttu-id="87035-194">Na hostiteli CentOS jsou povolené dvě síťová rozhraní.</span><span class="sxs-lookup"><span data-stu-id="87035-194">Two network interfaces are enabled on your CentOS host.</span></span>

<span data-ttu-id="87035-195">Výše uvedené konfigurace předá 4 samostatné cesty mezi vaším zařízením a hostitele, pokud jsou data hostitelů a rozhraní směrovatelné.</span><span class="sxs-lookup"><span data-stu-id="87035-195">The above configuration will yield 4 separate paths between your device and the host if the host and data interfaces are routable.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="87035-196">Doporučujeme vám, že nemíchat 1 GbE a 10 GbE síťová rozhraní pro vytváření více cest.</span><span class="sxs-lookup"><span data-stu-id="87035-196">We recommend that you do not mix 1 GbE and 10 GbE network interfaces for multipathing.</span></span> <span data-ttu-id="87035-197">Při použití dvou síťových rozhraní, jak rozhraní musí být identické typu.</span><span class="sxs-lookup"><span data-stu-id="87035-197">When using two network interfaces, both the interfaces should be the identical type.</span></span>
> * <span data-ttu-id="87035-198">V zařízení StorSimple, DATA0, DATA1, DATA4 a DATA5 jsou 1 GbE rozhraní zatímco DATA2 a DATA3 10 GbE síťových rozhraní. |</span><span class="sxs-lookup"><span data-stu-id="87035-198">On your StorSimple device, DATA0, DATA1, DATA4 and DATA5 are 1 GbE interfaces whereas DATA2 and DATA3 are 10 GbE network interfaces.|</span></span>
> 
> 

## <a name="configuration-steps"></a><span data-ttu-id="87035-199">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="87035-199">Configuration steps</span></span>
<span data-ttu-id="87035-200">Postup konfigurace pro více cest zahrnuje konfigurace k dispozici cesty pro automatické zjišťování, zadání algoritmus Vyrovnávání zatížení, pokud chcete použít, povolení více cest a nakonec ověření konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87035-200">The configuration steps for multipathing involve configuring the available paths for automatic discovery, specifying the load-balancing algorithm to use, enabling multipathing and finally verifying the configuration.</span></span> <span data-ttu-id="87035-201">Každý z těchto kroků je podrobněji v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="87035-201">Each of these steps is discussed in detail in the following sections.</span></span>

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a><span data-ttu-id="87035-202">Krok 1: Konfigurace používání více cest pro automatické zjišťování</span><span class="sxs-lookup"><span data-stu-id="87035-202">Step 1: Configure multipathing for automatic discovery</span></span>
<span data-ttu-id="87035-203">Zařízení podporující funkci multipath může automaticky zjistit a nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="87035-203">The multipath-supported devices can be automatically discovered and configured.</span></span>

1. <span data-ttu-id="87035-204">Inicializace `/etc/multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="87035-204">Initialize `/etc/multipath.conf` file.</span></span> <span data-ttu-id="87035-205">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-205">Type:</span></span>
   
     `mpathconf --enable`
   
    <span data-ttu-id="87035-206">Výše uvedený příkaz vytvoří `sample/etc/multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="87035-206">The above command will create a `sample/etc/multipath.conf` file.</span></span>
2. <span data-ttu-id="87035-207">Vícenásobný službu spusťte.</span><span class="sxs-lookup"><span data-stu-id="87035-207">Start multipath service.</span></span> <span data-ttu-id="87035-208">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-208">Type:</span></span>
   
    `service multipathd start`
   
    <span data-ttu-id="87035-209">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="87035-209">You will see the following output:</span></span>
   
    `Starting multipathd daemon:`
3. <span data-ttu-id="87035-210">Povolte automatické zjišťování multipaths.</span><span class="sxs-lookup"><span data-stu-id="87035-210">Enable automatic discovery of multipaths.</span></span> <span data-ttu-id="87035-211">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-211">Type:</span></span>
   
    `mpathconf --find_multipaths y`
   
    <span data-ttu-id="87035-212">To slouží k úpravě části výchozí nastavení vaší `multipath.conf` jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="87035-212">This will modify the defaults section of your `multipath.conf` as shown below:</span></span>
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a><span data-ttu-id="87035-213">Krok 2: Konfigurace používání více cest pro svazky zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="87035-213">Step 2: Configure multipathing for StorSimple volumes</span></span>
<span data-ttu-id="87035-214">Ve výchozím nastavení všechna zařízení jsou černé uvedené v souboru multipath.conf a bude možné obejít.</span><span class="sxs-lookup"><span data-stu-id="87035-214">By default, all devices are black listed in the multipath.conf file and will be bypassed.</span></span> <span data-ttu-id="87035-215">Musíte vytvořit zakázaných výjimky, které umožňují více cest pro svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-215">You will need to create blacklist exceptions to allow multipathing for volumes from StorSimple devices.</span></span>

1. <span data-ttu-id="87035-216">Upravit `/etc/mulitpath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="87035-216">Edit the `/etc/mulitpath.conf` file.</span></span> <span data-ttu-id="87035-217">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-217">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="87035-218">Vyhledejte v souboru multipath.conf blacklist_exceptions oddíl.</span><span class="sxs-lookup"><span data-stu-id="87035-218">Locate the blacklist_exceptions section in the multipath.conf file.</span></span> <span data-ttu-id="87035-219">Zařízení StorSimple musí být uveden jako zakázaných výjimka v této části.</span><span class="sxs-lookup"><span data-stu-id="87035-219">Your StorSimple device needs to be listed as a blacklist exception in this section.</span></span> <span data-ttu-id="87035-220">Zrušením komentáře u příslušné řádky v tomto souboru upravit podle následujícího obrázku (použijte pouze konkrétní model zařízení, které používáte):</span><span class="sxs-lookup"><span data-stu-id="87035-220">You can uncomment relevant lines in this file to modify it as shown below (use only the specific model of the device you are using):</span></span>
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a><span data-ttu-id="87035-221">Krok 3: Konfigurace pomocí kruhového dotazování více cest</span><span class="sxs-lookup"><span data-stu-id="87035-221">Step 3: Configure round-robin multipathing</span></span>
<span data-ttu-id="87035-222">Tento algoritmus Vyrovnávání zatížení používá všechny dostupné multipaths k aktivnímu řadiči vyrovnáváním, kruhové dotazování způsobem.</span><span class="sxs-lookup"><span data-stu-id="87035-222">This load-balancing algorithm uses all the available multipaths to the active controller in a balanced, round-robin fashion.</span></span>

1. <span data-ttu-id="87035-223">Upravit `/etc/multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="87035-223">Edit the `/etc/multipath.conf` file.</span></span> <span data-ttu-id="87035-224">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-224">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="87035-225">V části `defaults` nastavte `path_grouping_policy` k `multibus`.</span><span class="sxs-lookup"><span data-stu-id="87035-225">Under the `defaults` section, set the `path_grouping_policy` to `multibus`.</span></span> <span data-ttu-id="87035-226">`path_grouping_policy` Určuje výchozí cestu seskupování zásadu použít neurčené multipaths.</span><span class="sxs-lookup"><span data-stu-id="87035-226">The `path_grouping_policy` specifies the default path grouping policy to apply to unspecified multipaths.</span></span> <span data-ttu-id="87035-227">V části výchozí hodnoty bude vypadat, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="87035-227">The defaults section will look as shown below.</span></span>
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> <span data-ttu-id="87035-228">Nejběžnější hodnoty `path_grouping_policy` zahrnují:</span><span class="sxs-lookup"><span data-stu-id="87035-228">The most common values of `path_grouping_policy` include:</span></span>
> 
> * <span data-ttu-id="87035-229">převzetí služeb při selhání = 1 cesty na skupinu s prioritou</span><span class="sxs-lookup"><span data-stu-id="87035-229">failover = 1 path per priority group</span></span>
> * <span data-ttu-id="87035-230">multibus = všechny platné cesty ve skupině s prioritou 1</span><span class="sxs-lookup"><span data-stu-id="87035-230">multibus = all valid paths in 1 priority group</span></span>
> 
> 

### <a name="step-4-enable-multipathing"></a><span data-ttu-id="87035-231">Krok 4: Povolení používání více cest</span><span class="sxs-lookup"><span data-stu-id="87035-231">Step 4: Enable multipathing</span></span>
1. <span data-ttu-id="87035-232">Restartujte `multipathd` démona.</span><span class="sxs-lookup"><span data-stu-id="87035-232">Restart the `multipathd` daemon.</span></span> <span data-ttu-id="87035-233">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-233">Type:</span></span>
   
    `service multipathd restart`
2. <span data-ttu-id="87035-234">Výstup bude, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="87035-234">The output will be as shown below:</span></span>
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a><span data-ttu-id="87035-235">Krok 5: Ověření více cest</span><span class="sxs-lookup"><span data-stu-id="87035-235">Step 5: Verify multipathing</span></span>
1. <span data-ttu-id="87035-236">Nejdříve se ujistěte, že iSCSI se naváže připojení zařízení StorSimple následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="87035-236">First make sure that iSCSI connection is established with the StorSimple device as follows:</span></span>
   
   <span data-ttu-id="87035-237">a.</span><span class="sxs-lookup"><span data-stu-id="87035-237">a.</span></span> <span data-ttu-id="87035-238">Zjistit zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-238">Discover your StorSimple device.</span></span> <span data-ttu-id="87035-239">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-239">Type:</span></span>
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>
    ```
    
    <span data-ttu-id="87035-240">Výstup, pokud je IP adresa pro DATA0 10.126.162.25 a otevřít port 3260 v zařízení StorSimple pro přenosy iSCSI odchozí je, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="87035-240">The output when IP address for DATA0 is 10.126.162.25 and port 3260 is opened on the StorSimple device for outbound iSCSI traffic is as shown below:</span></span>
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    <span data-ttu-id="87035-241">Zkopírujte IQN zařízení StorSimple `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, z předchozí výstupu.</span><span class="sxs-lookup"><span data-stu-id="87035-241">Copy the IQN of your StorSimple device, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, from the preceding output.</span></span>

   <span data-ttu-id="87035-242">b.</span><span class="sxs-lookup"><span data-stu-id="87035-242">b.</span></span> <span data-ttu-id="87035-243">Připojte k zařízení pomocí cíl IQN.</span><span class="sxs-lookup"><span data-stu-id="87035-243">Connect to the device using target IQN.</span></span> <span data-ttu-id="87035-244">Zařízení StorSimple je zde cíle iSCSI.</span><span class="sxs-lookup"><span data-stu-id="87035-244">The StorSimple device is the iSCSI target here.</span></span> <span data-ttu-id="87035-245">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-245">Type:</span></span>

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    <span data-ttu-id="87035-246">Následující příklad ukazuje výstup s cílem IQN systému `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span><span class="sxs-lookup"><span data-stu-id="87035-246">The following example shows output with a target IQN of `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span></span> <span data-ttu-id="87035-247">Výstup označuje, že jste úspěšně připojení dvě iSCSI povolený síťovým rozhraním na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="87035-247">The output indicates that you have successfully connected to the two iSCSI-enabled network interfaces on your device.</span></span>

    ```
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    <span data-ttu-id="87035-248">Pokud se zobrazí pouze jednoho hostitele rozhraní a dvě cesty sem, budete muset povolit rozhraní na hostiteli pro iSCSI.</span><span class="sxs-lookup"><span data-stu-id="87035-248">If you see only one host interface and two paths here, then you need to enable both the interfaces on host for iSCSI.</span></span> <span data-ttu-id="87035-249">Můžete provést [podrobné pokyny v dokumentaci k systému Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span><span class="sxs-lookup"><span data-stu-id="87035-249">You can follow the [detailed instructions in Linux documentation](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span></span>

2. <span data-ttu-id="87035-250">Svazek má přístup k serveru CentOS ze zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-250">A volume is exposed to the CentOS server from the StorSimple device.</span></span> <span data-ttu-id="87035-251">Další informace najdete v tématu [krok 6: vytvoření svazku](storsimple-deployment-walkthrough.md#step-6-create-a-volume) prostřednictvím portálu Azure classic na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-251">For more information, see [Step 6: Create a volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via the Azure classic portal on your StorSimple device.</span></span>

3. <span data-ttu-id="87035-252">Ověření cesty k dispozici.</span><span class="sxs-lookup"><span data-stu-id="87035-252">Verify the available paths.</span></span> <span data-ttu-id="87035-253">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="87035-253">Type:</span></span>

      ```
      multipath –l
      ```

      <span data-ttu-id="87035-254">Následující příklad ukazuje výstup pro dvě síťová rozhraní na zařízení StorSimple připojené k jeden hostitel síťové rozhraní se dvě cesty k dispozici.</span><span class="sxs-lookup"><span data-stu-id="87035-254">The following example shows the output for two network interfaces on a StorSimple device connected to a single host network interface with two available paths.</span></span>

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        The following example shows the output for two network interfaces on a StorSimple device connected to two host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After the paths are configured, refer to the specific instructions on your host operating system (Centos 6.6) to mount and format this volume.

## <a name="troubleshoot-multipathing"></a><span data-ttu-id="87035-255">Řešení potíží s více cest</span><span class="sxs-lookup"><span data-stu-id="87035-255">Troubleshoot multipathing</span></span>
<span data-ttu-id="87035-256">Tato část obsahuje některé užitečné tipy, pokud narazíte na potíže během konfigurace více cest.</span><span class="sxs-lookup"><span data-stu-id="87035-256">This section provides some helpful tips if you run into any issues during multipathing configuration.</span></span>

<span data-ttu-id="87035-257">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="87035-257">Q.</span></span> <span data-ttu-id="87035-258">Nevidím změny v `multipath.conf` souboru neprojeví.</span><span class="sxs-lookup"><span data-stu-id="87035-258">I do not see the changes in `multipath.conf` file taking effect.</span></span>

<span data-ttu-id="87035-259">A.</span><span class="sxs-lookup"><span data-stu-id="87035-259">A.</span></span> <span data-ttu-id="87035-260">Pokud jste provedli všechny změny `multipath.conf` soubor, budete muset restartovat službu více cest.</span><span class="sxs-lookup"><span data-stu-id="87035-260">If you have made any changes to the `multipath.conf` file, you will need to restart the multipathing service.</span></span> <span data-ttu-id="87035-261">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="87035-261">Type the following command:</span></span>

    service multipathd restart

<span data-ttu-id="87035-262">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="87035-262">Q.</span></span> <span data-ttu-id="87035-263">Je povoleno dvě síťová rozhraní v zařízení StorSimple a dvě síťová rozhraní na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="87035-263">I have enabled two network interfaces on the StorSimple device and two network interfaces on the host.</span></span> <span data-ttu-id="87035-264">Při zobrazení seznamu cest k dispozici, vidím pouze dvě cesty.</span><span class="sxs-lookup"><span data-stu-id="87035-264">When I list the available paths, I see only two paths.</span></span> <span data-ttu-id="87035-265">Očekávání zobrazíte čtyři dostupné cesty.</span><span class="sxs-lookup"><span data-stu-id="87035-265">I expected to see four available paths.</span></span>

<span data-ttu-id="87035-266">A.</span><span class="sxs-lookup"><span data-stu-id="87035-266">A.</span></span> <span data-ttu-id="87035-267">Ujistěte se, že tyto dvě cesty jsou ve stejné podsíti a směrovat.</span><span class="sxs-lookup"><span data-stu-id="87035-267">Make sure that the two paths are on the same subnet and routable.</span></span> <span data-ttu-id="87035-268">Pokud rozhraní sítě jsou v jiné sítě VLAN a není směrovatelné, zobrazí se pouze dvě cesty.</span><span class="sxs-lookup"><span data-stu-id="87035-268">If the network interfaces are on different vLANs and not routable, you will see only two paths.</span></span> <span data-ttu-id="87035-269">Chcete-li to ověřit je a ujistěte se, že může kontaktovat rozhraní hostitele ze síťového rozhraní v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-269">One way to verify this is to make sure that you can reach both the host interfaces from a network interface on the StorSimple device.</span></span> <span data-ttu-id="87035-270">Budete muset [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) jako toto ověření provést pouze prostřednictvím podpory relace.</span><span class="sxs-lookup"><span data-stu-id="87035-270">You will need to [contact Microsoft Support](storsimple-contact-microsoft-support.md) as this verification can only be done via a support session.</span></span>

<span data-ttu-id="87035-271">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="87035-271">Q.</span></span> <span data-ttu-id="87035-272">Při zobrazení seznamu cest k dispozici, nezobrazí žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="87035-272">When I list available paths, I do not see any output.</span></span>

<span data-ttu-id="87035-273">A.</span><span class="sxs-lookup"><span data-stu-id="87035-273">A.</span></span> <span data-ttu-id="87035-274">Obvykle není zobrazuje všechny cesty multipathed naznačuje problém s démon více cest a bude s největší pravděpodobností, jakýkoli problém s zde spočívá v `multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="87035-274">Typically, not seeing any multipathed paths suggests a problem with the multipathing daemon, and it’s most likely that any problem here lies in the `multipath.conf` file.</span></span>

<span data-ttu-id="87035-275">Toto nastavení by také být vhodné kontrola, zda se ve skutečnosti zobrazí některé disky po připojení k cíli, neboť žádná odpověď z vícecestného výpisů může taky znamenat, že nemáte žádné disky.</span><span class="sxs-lookup"><span data-stu-id="87035-275">It would also be worth checking that you can actually see some disks after connecting to the target, as no response from the multipath listings could also mean you don’t have any disks.</span></span>

* <span data-ttu-id="87035-276">Jestliže chcete prohledat sběrnice SCSI, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="87035-276">Use the following command to rescan the SCSI bus:</span></span>
  
    <span data-ttu-id="87035-277">`$ rescan-scsi-bus.sh `(součást balíčku sg3_utils)</span><span class="sxs-lookup"><span data-stu-id="87035-277">`$ rescan-scsi-bus.sh `(part of sg3_utils package)</span></span>
* <span data-ttu-id="87035-278">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="87035-278">Type the following commands:</span></span>
  
    `$ dmesg | grep sd*`
     
     <span data-ttu-id="87035-279">Nebo</span><span class="sxs-lookup"><span data-stu-id="87035-279">Or</span></span>
  
    `$ fdisk –l`
  
    <span data-ttu-id="87035-280">Tyto vrátí podrobnosti o nedávno přidané disky.</span><span class="sxs-lookup"><span data-stu-id="87035-280">These will return details of recently added disks.</span></span>
* <span data-ttu-id="87035-281">K určení, zda se jedná o disk StorSimple, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="87035-281">To determine whether it is a StorSimple disk, use the following commands:</span></span>
  
    `cat /sys/block/<DISK>/device/model`
  
    <span data-ttu-id="87035-282">Tato možnost vrátí řetězec, který bude zjistit, jestli je StorSimple disk.</span><span class="sxs-lookup"><span data-stu-id="87035-282">This will return a string, which will determine if it’s a StorSimple disk.</span></span>

<span data-ttu-id="87035-283">Méně pravděpodobné, ale možná příčina A mohou být také zastaralé iscsid pid.</span><span class="sxs-lookup"><span data-stu-id="87035-283">A less likely but possible cause could also be stale iscsid pid.</span></span> <span data-ttu-id="87035-284">Použijte následující příkaz k odhlášení z relace iSCSI:</span><span class="sxs-lookup"><span data-stu-id="87035-284">Use the following command to log off from the iSCSI sessions:</span></span>

    iscsiadm -m node --logout -p <Target_IP>

<span data-ttu-id="87035-285">Opakujte tento příkaz pro všechna rozhraní propojená síť na cíli iSCSI, která je zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="87035-285">Repeat this command for all the connected network interfaces on the iSCSI target, which is your StorSimple device.</span></span> <span data-ttu-id="87035-286">Jakmile jste se odhlásili z všechny relace iSCSI, použijte pokud chcete znovu vytvořit relace iSCSI cíle iSCSI IQN.</span><span class="sxs-lookup"><span data-stu-id="87035-286">Once you have logged off from all the iSCSI sessions, use the iSCSI target IQN to reestablish the iSCSI session.</span></span> <span data-ttu-id="87035-287">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="87035-287">Type the following command:</span></span>

    iscsiadm -m node --login -T <TARGET_IQN>


<span data-ttu-id="87035-288">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="87035-288">Q.</span></span> <span data-ttu-id="87035-289">Nejste si jisti, pokud zařízení je seznam povolených adres.</span><span class="sxs-lookup"><span data-stu-id="87035-289">I am not sure if my device is whitelisted.</span></span>

<span data-ttu-id="87035-290">A.</span><span class="sxs-lookup"><span data-stu-id="87035-290">A.</span></span> <span data-ttu-id="87035-291">Pokud chcete ověřit, jestli vaše zařízení je seznam povolených adres, použijte následující řešení potíží interaktivní příkaz:</span><span class="sxs-lookup"><span data-stu-id="87035-291">To verify whether your device is whitelisted, use the following troubleshooting interactive command:</span></span>

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


<span data-ttu-id="87035-292">Další informace, přejděte na [použít řešení potíží s interaktivního příkazu pro více cest](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span><span class="sxs-lookup"><span data-stu-id="87035-292">For more information, go to [use troubleshooting interactive command for multipathing](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span></span>

## <a name="list-of-useful-commands"></a><span data-ttu-id="87035-293">Seznam užitečné příkazy</span><span class="sxs-lookup"><span data-stu-id="87035-293">List of useful commands</span></span>
| <span data-ttu-id="87035-294">Typ</span><span class="sxs-lookup"><span data-stu-id="87035-294">Type</span></span> | <span data-ttu-id="87035-295">Příkaz</span><span class="sxs-lookup"><span data-stu-id="87035-295">Command</span></span> | <span data-ttu-id="87035-296">Popis</span><span class="sxs-lookup"><span data-stu-id="87035-296">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87035-297">**iSCSI**</span><span class="sxs-lookup"><span data-stu-id="87035-297">**iSCSI**</span></span> |`service iscsid start` |<span data-ttu-id="87035-298">Spuštění služby iSCSI</span><span class="sxs-lookup"><span data-stu-id="87035-298">Start iSCSI service</span></span> |
| &nbsp; |`service iscsid stop` |<span data-ttu-id="87035-299">Zastavit službu iSCSI</span><span class="sxs-lookup"><span data-stu-id="87035-299">Stop iSCSI service</span></span> |
| &nbsp; |`service iscsid restart` |<span data-ttu-id="87035-300">Restartujte službu iSCSI</span><span class="sxs-lookup"><span data-stu-id="87035-300">Restart iSCSI service</span></span> |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |<span data-ttu-id="87035-301">Zjišťování dostupných cílů na zadanou adresu</span><span class="sxs-lookup"><span data-stu-id="87035-301">Discover available targets on the specified address</span></span> |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |<span data-ttu-id="87035-302">Přihlaste se k cíli iSCSI</span><span class="sxs-lookup"><span data-stu-id="87035-302">Log in to the iSCSI target</span></span> |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |<span data-ttu-id="87035-303">Odhlaste se z cíle iSCSI</span><span class="sxs-lookup"><span data-stu-id="87035-303">Log out from the iSCSI target</span></span> |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |<span data-ttu-id="87035-304">Tisk – název iniciátoru iSCSI</span><span class="sxs-lookup"><span data-stu-id="87035-304">Print iSCSI initiator name</span></span> |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |<span data-ttu-id="87035-305">Zkontrolujte stav relace iSCSI a svazek zjištěných na hostiteli</span><span class="sxs-lookup"><span data-stu-id="87035-305">Check the state of the iSCSI session and volume discovered on the host</span></span> |
| &nbsp; |`iscsi –m session` |<span data-ttu-id="87035-306">Zobrazí všechny relace iSCSI navázat mezi hostitelem a zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="87035-306">Shows all the iSCSI sessions established between the host and the StorSimple device</span></span> |
|  | | |
| <span data-ttu-id="87035-307">**Více cest**</span><span class="sxs-lookup"><span data-stu-id="87035-307">**Multipathing**</span></span> |`service multipathd start` |<span data-ttu-id="87035-308">Démon spuštění více cest</span><span class="sxs-lookup"><span data-stu-id="87035-308">Start multipath daemon</span></span> |
| &nbsp; |`service multipathd stop` |<span data-ttu-id="87035-309">Zastavit vícenásobný démon</span><span class="sxs-lookup"><span data-stu-id="87035-309">Stop multipath daemon</span></span> |
| &nbsp; |`service multipathd restart` |<span data-ttu-id="87035-310">Restartujte vícenásobný démon</span><span class="sxs-lookup"><span data-stu-id="87035-310">Restart multipath daemon</span></span> |
| &nbsp; |`chkconfig multipathd on` </br> <span data-ttu-id="87035-311">NEBO</span><span class="sxs-lookup"><span data-stu-id="87035-311">OR</span></span> </br> `mpathconf –with_chkconfig y` |<span data-ttu-id="87035-312">Povolení funkce multipath démon spustit při spuštění</span><span class="sxs-lookup"><span data-stu-id="87035-312">Enable multipath daemon to start at boot time</span></span> |
| &nbsp; |`multipathd –k` |<span data-ttu-id="87035-313">Spusťte interaktivní konzolu pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="87035-313">Start the interactive console for troubleshooting</span></span> |
| &nbsp; |`multipath –l` |<span data-ttu-id="87035-314">Připojení více cest seznamu a zařízení</span><span class="sxs-lookup"><span data-stu-id="87035-314">List multipath connections and devices</span></span> |
| &nbsp; |`mpathconf --enable` |<span data-ttu-id="87035-315">Vytvořte soubor ukázka mulitpath.conf v`/etc/mulitpath.conf`</span><span class="sxs-lookup"><span data-stu-id="87035-315">Create a sample mulitpath.conf file in `/etc/mulitpath.conf`</span></span> |
|  | | |

## <a name="next-steps"></a><span data-ttu-id="87035-316">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87035-316">Next steps</span></span>
<span data-ttu-id="87035-317">Jak na hostiteli systému Linux jsou konfigurace funkce MPIO, může také muset naleznete v následujících dokumentech CentoS 6.6:</span><span class="sxs-lookup"><span data-stu-id="87035-317">As you are configuring MPIO on Linux host, you may also need to refer to the following CentoS 6.6 documents:</span></span>

* [<span data-ttu-id="87035-318">Nastavení funkce MPIO na CentOS</span><span class="sxs-lookup"><span data-stu-id="87035-318">Setting up MPIO on CentOS</span></span>](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [<span data-ttu-id="87035-319">Průvodce školení Linux</span><span class="sxs-lookup"><span data-stu-id="87035-319">Linux Training Guide</span></span>](http://linux-training.be/files/books/LinuxAdm.pdf)

