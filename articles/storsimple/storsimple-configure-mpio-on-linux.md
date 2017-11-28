---
title: aaaConfigure funkci MPIO na hostitele platformy StorSimple Linux | Microsoft Docs
description: "Konfigurace funkce MPIO na StorSimple připojené tooa Linux hostitele se systémem CentOS 6.6"
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
ms.openlocfilehash: d9f7e02903243494c909313fb2c33ac690764274
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a><span data-ttu-id="c4962-103">Konfigurace funkce MPIO na StorSimple hostitele se systémem CentOS</span><span class="sxs-lookup"><span data-stu-id="c4962-103">Configure MPIO on a StorSimple host running CentOS</span></span>
<span data-ttu-id="c4962-104">Tento článek vysvětluje hello kroky požadované tooconfigure více cest vstupně-výstupní operace (MPIO) na serveru hostitele Centos 6.6.</span><span class="sxs-lookup"><span data-stu-id="c4962-104">This article explains hello steps required tooconfigure Multipathing IO (MPIO) on your Centos 6.6 host server.</span></span> <span data-ttu-id="c4962-105">hostitelský server Hello je zařízení připojené tooyour Microsoft Azure StorSimple pro vysokou dostupnost prostřednictvím iniciátory iSCSI.</span><span class="sxs-lookup"><span data-stu-id="c4962-105">hello host server is connected tooyour Microsoft Azure StorSimple device for high availability via iSCSI initiators.</span></span> <span data-ttu-id="c4962-106">Popisuje podrobnosti hello automatického zjišťování vícenásobný zařízení a hello konkrétní nastavení pouze pro svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-106">It describes in detail hello automatic discovery of multipath devices and hello specific setup only for StorSimple volumes.</span></span>

<span data-ttu-id="c4962-107">Tento postup je použít tooall hello modely řadu zařízení StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="c4962-107">This procedure is applicable tooall hello models of StorSimple 8000 series devices.</span></span>

> [!NOTE]
> <span data-ttu-id="c4962-108">Tento postup nelze použít pro virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-108">This procedure cannot be used for a StorSimple virtual device.</span></span> <span data-ttu-id="c4962-109">Další informace najdete v tématu Jak tooconfigure hostitelské servery pro vaše virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4962-109">For more information, see how tooconfigure host servers for your virtual device.</span></span>
> 
> 

## <a name="about-multipathing"></a><span data-ttu-id="c4962-110">O vytváření více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-110">About multipathing</span></span>
<span data-ttu-id="c4962-111">Funkce vytváření více cest Hello vám umožní tooconfigure více vstupně-výstupních cest mezi hostitelským serverem a zařízením úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4962-111">hello multipathing feature allows you tooconfigure multiple I/O paths between a host server and a storage device.</span></span> <span data-ttu-id="c4962-112">Tyto cesty vstupně-výstupní operace jsou fyzické připojení SAN, která může zahrnovat samostatné kabely, přepínače, síťových rozhraní a řadiče.</span><span class="sxs-lookup"><span data-stu-id="c4962-112">These I/O paths are physical SAN connections that can include separate cables, switches, network interfaces, and controllers.</span></span> <span data-ttu-id="c4962-113">Více cest agreguje hello vstupně-výstupních cest tooconfigure nové zařízení, která souvisí s všechny cesty hello agregovat.</span><span class="sxs-lookup"><span data-stu-id="c4962-113">Multipathing aggregates hello I/O paths, tooconfigure a new device that is associated with all of hello aggregated paths.</span></span>

<span data-ttu-id="c4962-114">účelem Hello více cest je dvojí:</span><span class="sxs-lookup"><span data-stu-id="c4962-114">hello purpose of multipathing is two-fold:</span></span>

* <span data-ttu-id="c4962-115">**Vysoká dostupnost**: poskytuje alternativní cestu, pokud se nezdaří libovolný element hello vstupně-výstupní cestu (například kabel, přepínače, síťové nebo řadič).</span><span class="sxs-lookup"><span data-stu-id="c4962-115">**High availability**: It provides an alternate path if any element of hello I/O path (such as a cable, switch, network interface, or controller) fails.</span></span>
* <span data-ttu-id="c4962-116">**Vyrovnávání zatížení**: v závislosti na konfiguraci hello zařízení úložiště, se může zlepšit výkon hello zjišťuje zatížením na cesty hello vstupně-výstupních operací a dynamicky vyrovnává těchto zatížení.</span><span class="sxs-lookup"><span data-stu-id="c4962-116">**Load balancing**: Depending on hello configuration of your storage device, it can improve hello performance by detecting loads on hello I/O paths and dynamically rebalancing those loads.</span></span>

### <a name="about-multipathing-components"></a><span data-ttu-id="c4962-117">O komponentách více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-117">About multipathing components</span></span>
<span data-ttu-id="c4962-118">Více cest v systému Linux se skládá z jádra a komponenty uživatelského prostoru jako v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c4962-118">Multipathing in Linux consists of kernel components and user-space components as tabulated below.</span></span>

* <span data-ttu-id="c4962-119">**Jádra**: součást hlavní hello je hello *zařízení mapper* , bude přesměrována vstupně-výstupních operací a podporuje převzetí služeb při selhání pro cesty a cesty skupiny.</span><span class="sxs-lookup"><span data-stu-id="c4962-119">**Kernel**: hello main component is hello *device-mapper* that reroutes I/O and supports failover for paths and path groups.</span></span>

* <span data-ttu-id="c4962-120">**Uživatel místo**: Jedná se o *multipath nástroje* , správě multipathed zařízení instruující hello zařízení mapper vícenásobný modulu co toodo.</span><span class="sxs-lookup"><span data-stu-id="c4962-120">**User-space**: These are *multipath-tools* that manage multipathed devices by instructing hello device-mapper multipath module what toodo.</span></span> <span data-ttu-id="c4962-121">Nástroje pro Hello obsahovat:</span><span class="sxs-lookup"><span data-stu-id="c4962-121">hello tools consist of:</span></span>
   
   * <span data-ttu-id="c4962-122">**Funkce Multipath**: uvádí a konfiguruje multipathed zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4962-122">**Multipath**: lists and configures multipathed devices.</span></span>
   * <span data-ttu-id="c4962-123">**Multipathd**: démon procesu, který spouští funkci multipath a monitorování hello cesty.</span><span class="sxs-lookup"><span data-stu-id="c4962-123">**Multipathd**: daemon that executes multipath and monitors hello paths.</span></span>
   * <span data-ttu-id="c4962-124">**Název Devmap**: poskytuje smysluplný název zařízení tooudev pro devmaps.</span><span class="sxs-lookup"><span data-stu-id="c4962-124">**Devmap-name**: provides a meaningful device-name tooudev for devmaps.</span></span>
   * <span data-ttu-id="c4962-125">**Kpartx**: mapuje lineární devmaps toodevice oddíly toomake vícenásobný mapy rozdělený.</span><span class="sxs-lookup"><span data-stu-id="c4962-125">**Kpartx**: maps linear devmaps toodevice partitions toomake multipath maps partitionable.</span></span>
   * <span data-ttu-id="c4962-126">**Multipath.conf**: konfigurační soubor pro více cest démon, který je použité toooverwrite hello předdefinované konfigurace tabulky.</span><span class="sxs-lookup"><span data-stu-id="c4962-126">**Multipath.conf**: configuration file for multipath daemon that is used toooverwrite hello built-in configuration table.</span></span>

### <a name="about-hello-multipathconf-configuration-file"></a><span data-ttu-id="c4962-127">O hello multipath.conf konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="c4962-127">About hello multipath.conf configuration file</span></span>
<span data-ttu-id="c4962-128">konfigurační soubor Hello `/etc/multipath.conf` provede mnoho hello více cest funkce uživatelsky konfigurovatelného.</span><span class="sxs-lookup"><span data-stu-id="c4962-128">hello configuration file `/etc/multipath.conf` makes many of hello multipathing features user-configurable.</span></span> <span data-ttu-id="c4962-129">Hello `multipath` příkaz a hello démon jádra `multipathd` použijte informace v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="c4962-129">hello `multipath` command and hello kernel daemon `multipathd` use information found in this file.</span></span> <span data-ttu-id="c4962-130">soubor Hello je konzultaci pouze během konfigurace hello hello vícenásobný zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4962-130">hello file is consulted only during hello configuration of hello multipath devices.</span></span> <span data-ttu-id="c4962-131">Ujistěte se, že jsou všechny změny provedené před spuštěním hello `multipath` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4962-131">Make sure that all changes are made before you run hello `multipath` command.</span></span> <span data-ttu-id="c4962-132">Pokud upravíte soubor hello můžete později, bude nutné toostop a spusťte multipathd znovu pro hello změny tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="c4962-132">If you modify hello file afterwards, you will need toostop and start multipathd again for hello changes tootake effect.</span></span>

<span data-ttu-id="c4962-133">Hello multipath.conf má pět částí:</span><span class="sxs-lookup"><span data-stu-id="c4962-133">hello multipath.conf has five sections:</span></span>

- <span data-ttu-id="c4962-134">**Výchozí nastavení na úrovni systému** *(výchozí)*: můžete přepsat výchozí nastavení na úrovni systému.</span><span class="sxs-lookup"><span data-stu-id="c4962-134">**System level defaults** *(defaults)*: You can override system level defaults.</span></span>
- <span data-ttu-id="c4962-135">**Zakázáno zařízení** *(černý list)*: můžete zadat hello seznam zařízení, které by neměly být řízené zařízení mapper.</span><span class="sxs-lookup"><span data-stu-id="c4962-135">**Blacklisted devices** *(blacklist)*: You can specify hello list of devices that should not be controlled by device-mapper.</span></span>
- <span data-ttu-id="c4962-136">**Blokovaných výjimky** *(blacklist_exceptions)*: můžete identifikovat považovat za zařízení s funkcí multipath i v případě, že uvedené v hello zakázaných toobe konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4962-136">**Blacklist exceptions** *(blacklist_exceptions)*: You can identify specific devices toobe treated as multipath devices even if listed in hello blacklist.</span></span>
- <span data-ttu-id="c4962-137">**Konkrétní nastavení řadiče úložiště** *(zařízení)*: můžete zadat nastavení konfigurace, které budou použité toodevices, které mají dodavatele a informace o produktu.</span><span class="sxs-lookup"><span data-stu-id="c4962-137">**Storage controller specific settings** *(devices)*: You can specify configuration settings that will be applied toodevices that have Vendor and Product information.</span></span>
- <span data-ttu-id="c4962-138">**Nastavení pro konkrétní zařízení** *(multipaths)*: nastavení konfigurace hello toofine tune této části můžete použít pro jednotlivé logické jednotky.</span><span class="sxs-lookup"><span data-stu-id="c4962-138">**Device specific settings** *(multipaths)*: You can use this section toofine-tune hello configuration settings for individual LUNs.</span></span>

## <a name="configure-multipathing-on-storsimple-connected-toolinux-host"></a><span data-ttu-id="c4962-139">Na hostiteli StorSimple připojené tooLinux nakonfigurovat více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-139">Configure multipathing on StorSimple connected tooLinux host</span></span>
<span data-ttu-id="c4962-140">Hostitele platformy Linux tooa připojeno zařízení StorSimple lze nakonfigurovat pro vysokou dostupnost a vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c4962-140">A StorSimple device connected tooa Linux host can be configured for high availability and load balancing.</span></span> <span data-ttu-id="c4962-141">Například pokud hello Linux hostitele má dvě rozhraní zařízení sítě SAN a hello připojené toohello má dvě rozhraní připojené toohello sítě SAN, které tato rozhraní jsou ve stejné podsíti hello, pak bude 4 cesty k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c4962-141">For example, if hello Linux host has two interfaces connected toohello SAN and hello device has two interfaces connected toohello SAN such that these interfaces are on hello same subnet, then there will be 4 paths available.</span></span> <span data-ttu-id="c4962-142">Ale pokud každé rozhraní DATA na zařízení a hostitele rozhraní hello jsou v jiné podsíti protokolu IP (a ne směrovatelné), pak pouze 2 cesty nebudou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c4962-142">However, if each DATA interface on hello device and host interface are on a different IP subnet (and not routable), then only 2 paths will be available.</span></span> <span data-ttu-id="c4962-143">Můžete nakonfigurovat více cest tooautomatically zjistit všechny cesty k dispozici hello, zvolte algoritmu Vyrovnávání zatížení u těchto cest, použít specifické nastavení pro svazky jen StorSimple a pak povolte a ověřte více cest.</span><span class="sxs-lookup"><span data-stu-id="c4962-143">You can configure multipathing tooautomatically discover all hello available paths, choose a load-balancing algorithm for those paths, apply specific configuration settings for StorSimple-only volumes, and then enable and verify multipathing.</span></span>

<span data-ttu-id="c4962-144">Hello následující postup popisuje, jak tooconfigure více cest, když zařízení StorSimple se dvěma síťovými rozhraními připojené tooa hostitele se dvěma síťovými rozhraními.</span><span class="sxs-lookup"><span data-stu-id="c4962-144">hello following procedure describes how tooconfigure multipathing when a StorSimple device with two network interfaces is connected tooa host with two network interfaces.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4962-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4962-145">Prerequisites</span></span>
<span data-ttu-id="c4962-146">Tato část podrobně hello požadavky konfigurace pro CentOS server a zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-146">This section details hello configuration prerequisites for CentOS server and your StorSimple device.</span></span>

### <a name="on-centos-host"></a><span data-ttu-id="c4962-147">Na hostiteli CentOS</span><span class="sxs-lookup"><span data-stu-id="c4962-147">On CentOS host</span></span>
1. <span data-ttu-id="c4962-148">Ujistěte se, že má váš hostitel CentOS 2 síťových rozhraní, které jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="c4962-148">Make sure that your CentOS host has 2 network interfaces enabled.</span></span> <span data-ttu-id="c4962-149">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-149">Type:</span></span>
   
    `ifconfig`
   
    <span data-ttu-id="c4962-150">Hello následující příklad ukazuje výstup hello při dva síťové rozhraní (`eth0` a `eth1`) jsou k dispozici na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-150">hello following example shows hello output when two network interfaces (`eth0` and `eth1`) are present on hello host.</span></span>
   
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
2. <span data-ttu-id="c4962-151">Nainstalujte *iSCSI. iniciátoru utils* na vašem serveru CentOS.</span><span class="sxs-lookup"><span data-stu-id="c4962-151">Install *iSCSI-initiator-utils* on your CentOS server.</span></span> <span data-ttu-id="c4962-152">Proveďte následující kroky tooinstall hello *iSCSI. iniciátoru utils*.</span><span class="sxs-lookup"><span data-stu-id="c4962-152">Perform hello following steps tooinstall *iSCSI-initiator-utils*.</span></span>
   
   1. <span data-ttu-id="c4962-153">Přihlaste se jako `root` do svého CentOS hostitele.</span><span class="sxs-lookup"><span data-stu-id="c4962-153">Log on as `root` into your CentOS host.</span></span>
   2. <span data-ttu-id="c4962-154">Nainstalujte hello *iSCSI. iniciátoru utils*.</span><span class="sxs-lookup"><span data-stu-id="c4962-154">Install hello *iSCSI-initiator-utils*.</span></span> <span data-ttu-id="c4962-155">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-155">Type:</span></span>
      
       `yum install iscsi-initiator-utils`
   3. <span data-ttu-id="c4962-156">Po hello *iSCSI. iniciátoru utils* je úspěšně nainstalován, spuštění služby iSCSI hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-156">After hello *iSCSI-Initiator-utils* is successfully installed, start hello iSCSI service.</span></span> <span data-ttu-id="c4962-157">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-157">Type:</span></span>
      
       `service iscsid start`
      
       <span data-ttu-id="c4962-158">V případech `iscsid` může ve skutečnosti neumožňují spuštění a hello `--force` možnost může být potřeba.</span><span class="sxs-lookup"><span data-stu-id="c4962-158">On occasions, `iscsid` may not actually start and hello `--force` option may be needed</span></span>
   4. <span data-ttu-id="c4962-159">tooensure, že vaše iniciátor iSCSI je povolena při spuštění, použijte hello `chkconfig` příkaz tooenable hello služby.</span><span class="sxs-lookup"><span data-stu-id="c4962-159">tooensure that your iSCSI initiator is enabled during boot time, use hello `chkconfig` command tooenable hello service.</span></span>
      
       `chkconfig iscsi on`
   5. <span data-ttu-id="c4962-160">tooverify, které bylo správně nastavit, spusťte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-160">tooverify that that it was properly setup, run hello command:</span></span>
      
       `chkconfig --list | grep iscsi`
      
       <span data-ttu-id="c4962-161">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="c4962-161">A sample output is shown below.</span></span>
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       <span data-ttu-id="c4962-162">Z hello výše příklad uvidíte, že vaše prostředí iSCSI bude spouštět na spuštění spuštění úrovně 2, 3, 4 a 5.</span><span class="sxs-lookup"><span data-stu-id="c4962-162">From hello above example, you can see that your iSCSI environment will run on boot time on run levels 2, 3, 4, and 5.</span></span>
3. <span data-ttu-id="c4962-163">Nainstalujte *zařízení. mapper multipath*.</span><span class="sxs-lookup"><span data-stu-id="c4962-163">Install *device-mapper-multipath*.</span></span> <span data-ttu-id="c4962-164">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-164">Type:</span></span>
   
    `yum install device-mapper-multipath`
   
    <span data-ttu-id="c4962-165">Hello instalace začne.</span><span class="sxs-lookup"><span data-stu-id="c4962-165">hello installation will start.</span></span> <span data-ttu-id="c4962-166">Typ **Y** toocontinue po zobrazení výzvy k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="c4962-166">Type **Y** toocontinue when prompted for confirmation.</span></span>

### <a name="on-storsimple-device"></a><span data-ttu-id="c4962-167">Na zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="c4962-167">On StorSimple device</span></span>
<span data-ttu-id="c4962-168">Zařízení StorSimple musí mít:</span><span class="sxs-lookup"><span data-stu-id="c4962-168">Your StorSimple device should have:</span></span>

* <span data-ttu-id="c4962-169">Minimálně dvě rozhraní povolená pro iSCSI.</span><span class="sxs-lookup"><span data-stu-id="c4962-169">A minimum of two interfaces enabled for iSCSI.</span></span> <span data-ttu-id="c4962-170">tooverify, že jsou dvě rozhraní iSCSI povolený v zařízení StorSimple, proveďte následující kroky v hello klasický portál Azure pro zařízení StorSimple hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-170">tooverify that two interfaces are iSCSI-enabled on your StorSimple device, perform hello following steps in hello Azure classic portal for your StorSimple device:</span></span>
  
  1. <span data-ttu-id="c4962-171">Přihlaste se k portálu classic hello pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-171">Log into hello classic portal for your StorSimple device.</span></span>
  2. <span data-ttu-id="c4962-172">Vybrat služby StorSimple Manager, klikněte na tlačítko **zařízení** a zvolte konkrétní zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-172">Select your StorSimple Manager service, click **Devices** and choose hello specific StorSimple device.</span></span> <span data-ttu-id="c4962-173">Klikněte na tlačítko **konfigurace** a ověřte nastavení síťového rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-173">Click **Configure** and verify hello network interface settings.</span></span> <span data-ttu-id="c4962-174">Snímek obrazovky s dvě rozhraní iSCSI povolený sítě jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="c4962-174">A screenshot with two iSCSI-enabled network interfaces is shown below.</span></span> <span data-ttu-id="c4962-175">Sem DATA 2 a DATA 3, oba 10 GbE je povoleno rozhraní iSCSI.</span><span class="sxs-lookup"><span data-stu-id="c4962-175">Here DATA 2 and DATA 3, both 10 GbE interfaces are enabled for iSCSI.</span></span>
     
      ![Konfigurace funkce MPIO StorsSimple DATA 2](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![Funkce MPIO StorSimple dat 3 konfigurace](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      <span data-ttu-id="c4962-178">V hello **konfigurace** stránky</span><span class="sxs-lookup"><span data-stu-id="c4962-178">In hello **Configure** page</span></span>
     
     1. <span data-ttu-id="c4962-179">Zajistěte, aby obě síťová rozhraní iSCSI povolený.</span><span class="sxs-lookup"><span data-stu-id="c4962-179">Ensure that both network interfaces are iSCSI-enabled.</span></span> <span data-ttu-id="c4962-180">Hello **iSCSI povoleno** pole musí být nastavené příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="c4962-180">hello **iSCSI enabled** field should be set too**Yes**.</span></span>
     2. <span data-ttu-id="c4962-181">Zkontrolujte, zda text hello síťových rozhraní hello stejnou rychlostí, jak by měla být 1 GbE nebo 10 GbE.</span><span class="sxs-lookup"><span data-stu-id="c4962-181">Ensure that hello network interfaces have hello same speed, both should be 1 GbE or 10 GbE.</span></span>
     3. <span data-ttu-id="c4962-182">Poznamenejte si hello IPv4 adresy rozhraní iSCSI povolený hello a uložit pro pozdější použití na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-182">Note hello IPv4 addresses of hello iSCSI-enabled interfaces and save for later use on hello host.</span></span>
* <span data-ttu-id="c4962-183">rozhraní iSCSI Hello zařízení StorSimple by měly být dostupné z hello CentOS serveru.</span><span class="sxs-lookup"><span data-stu-id="c4962-183">hello iSCSI interfaces on your StorSimple device should be reachable from hello CentOS server.</span></span>
      <span data-ttu-id="c4962-184">tooverify, je nutné tooprovide hello IP adresy vašich rozhraní iSCSI povolený sítě StorSimple na hostitelském serveru.</span><span class="sxs-lookup"><span data-stu-id="c4962-184">tooverify this, you need tooprovide hello IP addresses of your StorSimple iSCSI-enabled network interfaces on your host server.</span></span> <span data-ttu-id="c4962-185">Hello příkazy používané a hello odpovídající výstup s DATA2 (10.126.162.25) a DATA3 (10.126.162.26) jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="c4962-185">hello commands used and hello corresponding output with DATA2 (10.126.162.25) and DATA3 (10.126.162.26) is shown below:</span></span>
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a><span data-ttu-id="c4962-186">Konfigurace hardwaru</span><span class="sxs-lookup"><span data-stu-id="c4962-186">Hardware configuration</span></span>
<span data-ttu-id="c4962-187">Doporučujeme vám, že připojíte hello dvě iSCSI síťová rozhraní na samostatné cesty pro redundanci.</span><span class="sxs-lookup"><span data-stu-id="c4962-187">We recommend that you connect hello two iSCSI network interfaces on separate paths for redundancy.</span></span> <span data-ttu-id="c4962-188">Hello obrázek níže znázorňuje hello doporučené hardwarové konfigurace pro vysokou dostupnost a vyrovnávaní zatížení více cest pro váš CentOS server a zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-188">hello figure below shows hello recommended hardware configuration for high availability and load-balancing multipathing for your CentOS server and StorSimple device.</span></span>

![Hardwarová konfigurace funkce MPIO pro hostitele tooLinux StorSimple](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

<span data-ttu-id="c4962-190">Jak je znázorněno v předchozích obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-190">As shown in hello preceding figure:</span></span>

* <span data-ttu-id="c4962-191">Zařízení StorSimple je v konfiguraci aktivní pasivní s dva řadiče.</span><span class="sxs-lookup"><span data-stu-id="c4962-191">Your StorSimple device is in an active-passive configuration with two controllers.</span></span>
* <span data-ttu-id="c4962-192">Dva přepínače sítě SAN jsou připojené tooyour řadiče zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4962-192">Two SAN switches are connected tooyour device controllers.</span></span>
* <span data-ttu-id="c4962-193">Dva iniciátory iSCSI jsou povolené v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-193">Two iSCSI initiators are enabled on your StorSimple device.</span></span>
* <span data-ttu-id="c4962-194">Na hostiteli CentOS jsou povolené dvě síťová rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c4962-194">Two network interfaces are enabled on your CentOS host.</span></span>

<span data-ttu-id="c4962-195">Hello výše konfigurace předá 4 samostatné cest mezi hostiteli zařízení a hello, pokud jsou směrovatelné hello data hostitelů a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c4962-195">hello above configuration will yield 4 separate paths between your device and hello host if hello host and data interfaces are routable.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="c4962-196">Doporučujeme vám, že nemíchat 1 GbE a 10 GbE síťová rozhraní pro vytváření více cest.</span><span class="sxs-lookup"><span data-stu-id="c4962-196">We recommend that you do not mix 1 GbE and 10 GbE network interfaces for multipathing.</span></span> <span data-ttu-id="c4962-197">Při použití dvou síťových rozhraní, musí být obě rozhraní hello hello identické typu.</span><span class="sxs-lookup"><span data-stu-id="c4962-197">When using two network interfaces, both hello interfaces should be hello identical type.</span></span>
> * <span data-ttu-id="c4962-198">V zařízení StorSimple, DATA0, DATA1, DATA4 a DATA5 jsou 1 GbE rozhraní zatímco DATA2 a DATA3 10 GbE síťových rozhraní. |</span><span class="sxs-lookup"><span data-stu-id="c4962-198">On your StorSimple device, DATA0, DATA1, DATA4 and DATA5 are 1 GbE interfaces whereas DATA2 and DATA3 are 10 GbE network interfaces.|</span></span>
> 
> 

## <a name="configuration-steps"></a><span data-ttu-id="c4962-199">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="c4962-199">Configuration steps</span></span>
<span data-ttu-id="c4962-200">Hello kroky konfigurace pro více cest zahrnovat konfigurace hello k dispozici cesty pro automatické zjišťování, zadání hello Vyrovnávání zatížení algoritmus toouse, povolení více cest a nakonec ověření konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-200">hello configuration steps for multipathing involve configuring hello available paths for automatic discovery, specifying hello load-balancing algorithm toouse, enabling multipathing and finally verifying hello configuration.</span></span> <span data-ttu-id="c4962-201">Každý z těchto kroků je podrobněji v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-201">Each of these steps is discussed in detail in hello following sections.</span></span>

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a><span data-ttu-id="c4962-202">Krok 1: Konfigurace používání více cest pro automatické zjišťování</span><span class="sxs-lookup"><span data-stu-id="c4962-202">Step 1: Configure multipathing for automatic discovery</span></span>
<span data-ttu-id="c4962-203">zařízení podporující funkci multipath Hello lze automaticky zjistit a nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="c4962-203">hello multipath-supported devices can be automatically discovered and configured.</span></span>

1. <span data-ttu-id="c4962-204">Inicializace `/etc/multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="c4962-204">Initialize `/etc/multipath.conf` file.</span></span> <span data-ttu-id="c4962-205">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-205">Type:</span></span>
   
     `mpathconf --enable`
   
    <span data-ttu-id="c4962-206">Hello výše příkaz vytvoří `sample/etc/multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="c4962-206">hello above command will create a `sample/etc/multipath.conf` file.</span></span>
2. <span data-ttu-id="c4962-207">Vícenásobný službu spusťte.</span><span class="sxs-lookup"><span data-stu-id="c4962-207">Start multipath service.</span></span> <span data-ttu-id="c4962-208">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-208">Type:</span></span>
   
    `service multipathd start`
   
    <span data-ttu-id="c4962-209">Zobrazí se následující výstup hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-209">You will see hello following output:</span></span>
   
    `Starting multipathd daemon:`
3. <span data-ttu-id="c4962-210">Povolte automatické zjišťování multipaths.</span><span class="sxs-lookup"><span data-stu-id="c4962-210">Enable automatic discovery of multipaths.</span></span> <span data-ttu-id="c4962-211">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-211">Type:</span></span>
   
    `mpathconf --find_multipaths y`
   
    <span data-ttu-id="c4962-212">To slouží k úpravě hello výchozí část vaší `multipath.conf` jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="c4962-212">This will modify hello defaults section of your `multipath.conf` as shown below:</span></span>
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a><span data-ttu-id="c4962-213">Krok 2: Konfigurace používání více cest pro svazky zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="c4962-213">Step 2: Configure multipathing for StorSimple volumes</span></span>
<span data-ttu-id="c4962-214">Ve výchozím nastavení všechna zařízení jsou černé uvedené v souboru multipath.conf hello a bude možné obejít.</span><span class="sxs-lookup"><span data-stu-id="c4962-214">By default, all devices are black listed in hello multipath.conf file and will be bypassed.</span></span> <span data-ttu-id="c4962-215">Pro svazky zařízení StorSimple budete potřebovat více cest tooallow toocreate blacklist výjimky.</span><span class="sxs-lookup"><span data-stu-id="c4962-215">You will need toocreate blacklist exceptions tooallow multipathing for volumes from StorSimple devices.</span></span>

1. <span data-ttu-id="c4962-216">Upravit hello `/etc/mulitpath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="c4962-216">Edit hello `/etc/mulitpath.conf` file.</span></span> <span data-ttu-id="c4962-217">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-217">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="c4962-218">Vyhledejte v souboru multipath.conf hello hello blacklist_exceptions oddíl.</span><span class="sxs-lookup"><span data-stu-id="c4962-218">Locate hello blacklist_exceptions section in hello multipath.conf file.</span></span> <span data-ttu-id="c4962-219">Zařízení StorSimple musí toobe uveden jako zakázaných výjimka v této části.</span><span class="sxs-lookup"><span data-stu-id="c4962-219">Your StorSimple device needs toobe listed as a blacklist exception in this section.</span></span> <span data-ttu-id="c4962-220">Můžete Odkomentujte, relevantní řádků tento soubor toomodify ho takto (použijte pouze hello konkrétní model hello zařízení, který používáte):</span><span class="sxs-lookup"><span data-stu-id="c4962-220">You can uncomment relevant lines in this file toomodify it as shown below (use only hello specific model of hello device you are using):</span></span>
   
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

### <a name="step-3-configure-round-robin-multipathing"></a><span data-ttu-id="c4962-221">Krok 3: Konfigurace pomocí kruhového dotazování více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-221">Step 3: Configure round-robin multipathing</span></span>
<span data-ttu-id="c4962-222">Tento algoritmus Vyrovnávání zatížení používá všechny aktivního řadiče k dispozici multipaths toohello hello vyrovnáváním, kruhové dotazování způsobem.</span><span class="sxs-lookup"><span data-stu-id="c4962-222">This load-balancing algorithm uses all hello available multipaths toohello active controller in a balanced, round-robin fashion.</span></span>

1. <span data-ttu-id="c4962-223">Upravit hello `/etc/multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="c4962-223">Edit hello `/etc/multipath.conf` file.</span></span> <span data-ttu-id="c4962-224">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-224">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="c4962-225">V části hello `defaults` část, sada hello `path_grouping_policy` příliš`multibus`.</span><span class="sxs-lookup"><span data-stu-id="c4962-225">Under hello `defaults` section, set hello `path_grouping_policy` too`multibus`.</span></span> <span data-ttu-id="c4962-226">Hello `path_grouping_policy` určuje hello výchozí cestu seskupení zásad tooapply toounspecified multipaths.</span><span class="sxs-lookup"><span data-stu-id="c4962-226">hello `path_grouping_policy` specifies hello default path grouping policy tooapply toounspecified multipaths.</span></span> <span data-ttu-id="c4962-227">část výchozí hodnoty Hello bude vypadat, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c4962-227">hello defaults section will look as shown below.</span></span>
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> <span data-ttu-id="c4962-228">Hello nejběžnější hodnoty `path_grouping_policy` zahrnují:</span><span class="sxs-lookup"><span data-stu-id="c4962-228">hello most common values of `path_grouping_policy` include:</span></span>
> 
> * <span data-ttu-id="c4962-229">převzetí služeb při selhání = 1 cesty na skupinu s prioritou</span><span class="sxs-lookup"><span data-stu-id="c4962-229">failover = 1 path per priority group</span></span>
> * <span data-ttu-id="c4962-230">multibus = všechny platné cesty ve skupině s prioritou 1</span><span class="sxs-lookup"><span data-stu-id="c4962-230">multibus = all valid paths in 1 priority group</span></span>
> 
> 

### <a name="step-4-enable-multipathing"></a><span data-ttu-id="c4962-231">Krok 4: Povolení používání více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-231">Step 4: Enable multipathing</span></span>
1. <span data-ttu-id="c4962-232">Restartujte hello `multipathd` démona.</span><span class="sxs-lookup"><span data-stu-id="c4962-232">Restart hello `multipathd` daemon.</span></span> <span data-ttu-id="c4962-233">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-233">Type:</span></span>
   
    `service multipathd restart`
2. <span data-ttu-id="c4962-234">výstup Hello bude, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="c4962-234">hello output will be as shown below:</span></span>
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a><span data-ttu-id="c4962-235">Krok 5: Ověření více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-235">Step 5: Verify multipathing</span></span>
1. <span data-ttu-id="c4962-236">Nejdříve se ujistěte, že iSCSI se naváže připojení zařízení StorSimple hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c4962-236">First make sure that iSCSI connection is established with hello StorSimple device as follows:</span></span>
   
   <span data-ttu-id="c4962-237">a.</span><span class="sxs-lookup"><span data-stu-id="c4962-237">a.</span></span> <span data-ttu-id="c4962-238">Zjistit zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-238">Discover your StorSimple device.</span></span> <span data-ttu-id="c4962-239">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-239">Type:</span></span>
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on hello device>:<iSCSI port on StorSimple device>
    ```
    
    <span data-ttu-id="c4962-240">výstup Hello, pokud je IP adresa pro DATA0 10.126.162.25 a otevřít port 3260 v zařízení StorSimple hello pro přenosy iSCSI odchozí je, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="c4962-240">hello output when IP address for DATA0 is 10.126.162.25 and port 3260 is opened on hello StorSimple device for outbound iSCSI traffic is as shown below:</span></span>
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    <span data-ttu-id="c4962-241">Kopírování hello IQN zařízení StorSimple `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, z předchozích výstup hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-241">Copy hello IQN of your StorSimple device, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, from hello preceding output.</span></span>

   <span data-ttu-id="c4962-242">b.</span><span class="sxs-lookup"><span data-stu-id="c4962-242">b.</span></span> <span data-ttu-id="c4962-243">Připojte zařízení toohello pomocí cíl IQN.</span><span class="sxs-lookup"><span data-stu-id="c4962-243">Connect toohello device using target IQN.</span></span> <span data-ttu-id="c4962-244">zařízení StorSimple Hello je služby iSCSI target hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-244">hello StorSimple device is hello iSCSI target here.</span></span> <span data-ttu-id="c4962-245">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-245">Type:</span></span>

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    <span data-ttu-id="c4962-246">Hello následující příklad ukazuje výstup s cílem IQN systému `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span><span class="sxs-lookup"><span data-stu-id="c4962-246">hello following example shows output with a target IQN of `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span></span> <span data-ttu-id="c4962-247">výstup Hello označuje, že jste úspěšně připojení dvě rozhraní iSCSI povolený sítě toohello na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4962-247">hello output indicates that you have successfully connected toohello two iSCSI-enabled network interfaces on your device.</span></span>

    ```
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    <span data-ttu-id="c4962-248">Pokud se zobrazí pouze jednoho hostitele rozhraní a dvě cesty sem, budete potřebovat tooenable obě hello rozhraní na hostiteli pro iSCSI.</span><span class="sxs-lookup"><span data-stu-id="c4962-248">If you see only one host interface and two paths here, then you need tooenable both hello interfaces on host for iSCSI.</span></span> <span data-ttu-id="c4962-249">Můžete postupovat podle hello [podrobné pokyny v dokumentaci k systému Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span><span class="sxs-lookup"><span data-stu-id="c4962-249">You can follow hello [detailed instructions in Linux documentation](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span></span>

2. <span data-ttu-id="c4962-250">Svazek je zveřejněné toohello CentOS server ze zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-250">A volume is exposed toohello CentOS server from hello StorSimple device.</span></span> <span data-ttu-id="c4962-251">Další informace najdete v tématu [krok 6: vytvoření svazku](storsimple-deployment-walkthrough.md#step-6-create-a-volume) prostřednictvím hello portál Azure classic na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-251">For more information, see [Step 6: Create a volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via hello Azure classic portal on your StorSimple device.</span></span>

3. <span data-ttu-id="c4962-252">Ověření cesty k dispozici hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-252">Verify hello available paths.</span></span> <span data-ttu-id="c4962-253">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c4962-253">Type:</span></span>

      ```
      multipath –l
      ```

      <span data-ttu-id="c4962-254">Hello následující příklad ukazuje výstup hello dvě síťová rozhraní na StorSimple zařízení připojených tooa jeden hostitel síťové rozhraní se dvě cesty k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c4962-254">hello following example shows hello output for two network interfaces on a StorSimple device connected tooa single host network interface with two available paths.</span></span>

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        hello following example shows hello output for two network interfaces on a StorSimple device connected tootwo host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After hello paths are configured, refer toohello specific instructions on your host operating system (Centos 6.6) toomount and format this volume.

## <a name="troubleshoot-multipathing"></a><span data-ttu-id="c4962-255">Řešení potíží s více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-255">Troubleshoot multipathing</span></span>
<span data-ttu-id="c4962-256">Tato část obsahuje některé užitečné tipy, pokud narazíte na potíže během konfigurace více cest.</span><span class="sxs-lookup"><span data-stu-id="c4962-256">This section provides some helpful tips if you run into any issues during multipathing configuration.</span></span>

<span data-ttu-id="c4962-257">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="c4962-257">Q.</span></span> <span data-ttu-id="c4962-258">Nevidím hello změny v `multipath.conf` souboru neprojeví.</span><span class="sxs-lookup"><span data-stu-id="c4962-258">I do not see hello changes in `multipath.conf` file taking effect.</span></span>

<span data-ttu-id="c4962-259">A.</span><span class="sxs-lookup"><span data-stu-id="c4962-259">A.</span></span> <span data-ttu-id="c4962-260">Pokud jste provedli jakékoli změny toohello `multipath.conf` soubor, budete potřebovat toorestart hello více cest služby.</span><span class="sxs-lookup"><span data-stu-id="c4962-260">If you have made any changes toohello `multipath.conf` file, you will need toorestart hello multipathing service.</span></span> <span data-ttu-id="c4962-261">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c4962-261">Type hello following command:</span></span>

    service multipathd restart

<span data-ttu-id="c4962-262">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="c4962-262">Q.</span></span> <span data-ttu-id="c4962-263">Je povoleno dvě síťová rozhraní na zařízení StorSimple hello a dvě síťová rozhraní na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-263">I have enabled two network interfaces on hello StorSimple device and two network interfaces on hello host.</span></span> <span data-ttu-id="c4962-264">Při zobrazení seznamu cest k dispozici hello, vidím pouze dvě cesty.</span><span class="sxs-lookup"><span data-stu-id="c4962-264">When I list hello available paths, I see only two paths.</span></span> <span data-ttu-id="c4962-265">Očekávání toosee čtyři dostupné cesty.</span><span class="sxs-lookup"><span data-stu-id="c4962-265">I expected toosee four available paths.</span></span>

<span data-ttu-id="c4962-266">A.</span><span class="sxs-lookup"><span data-stu-id="c4962-266">A.</span></span> <span data-ttu-id="c4962-267">Ujistěte se, že jsou hello dvě cesty na hello stejné podsíti a směrovatelné.</span><span class="sxs-lookup"><span data-stu-id="c4962-267">Make sure that hello two paths are on hello same subnet and routable.</span></span> <span data-ttu-id="c4962-268">Pokud hello síťových rozhraní jsou na různých sítí VLAN a není směrovatelné, zobrazí se pouze dvě cesty.</span><span class="sxs-lookup"><span data-stu-id="c4962-268">If hello network interfaces are on different vLANs and not routable, you will see only two paths.</span></span> <span data-ttu-id="c4962-269">Jedním ze způsobů tooverify jde toomake jistotu, že se lze připojit i rozhraní hello hostitele ze síťového rozhraní na zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-269">One way tooverify this is toomake sure that you can reach both hello host interfaces from a network interface on hello StorSimple device.</span></span> <span data-ttu-id="c4962-270">Budete potřebovat příliš[kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) jako toto ověření provést pouze prostřednictvím podpory relace.</span><span class="sxs-lookup"><span data-stu-id="c4962-270">You will need too[contact Microsoft Support](storsimple-contact-microsoft-support.md) as this verification can only be done via a support session.</span></span>

<span data-ttu-id="c4962-271">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="c4962-271">Q.</span></span> <span data-ttu-id="c4962-272">Při zobrazení seznamu cest k dispozici, nezobrazí žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="c4962-272">When I list available paths, I do not see any output.</span></span>

<span data-ttu-id="c4962-273">A.</span><span class="sxs-lookup"><span data-stu-id="c4962-273">A.</span></span> <span data-ttu-id="c4962-274">Obvykle není zobrazuje všechny cesty multipathed naznačuje problém s více cest démon hello a bude s největší pravděpodobností, jakýkoli problém s zde spočívá v hello `multipath.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="c4962-274">Typically, not seeing any multipathed paths suggests a problem with hello multipathing daemon, and it’s most likely that any problem here lies in hello `multipath.conf` file.</span></span>

<span data-ttu-id="c4962-275">Toto nastavení by také být vhodné kontrola, zda se ve skutečnosti zobrazí některé disky po připojení toohello cíl, jako žádná odpověď z vícecestného výpisů hello může také znamená, že nemáte žádné disky.</span><span class="sxs-lookup"><span data-stu-id="c4962-275">It would also be worth checking that you can actually see some disks after connecting toohello target, as no response from hello multipath listings could also mean you don’t have any disks.</span></span>

* <span data-ttu-id="c4962-276">Použijte následující příkaz toorescan hello ke sběrnici SCSI hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-276">Use hello following command toorescan hello SCSI bus:</span></span>
  
    <span data-ttu-id="c4962-277">`$ rescan-scsi-bus.sh `(součást balíčku sg3_utils)</span><span class="sxs-lookup"><span data-stu-id="c4962-277">`$ rescan-scsi-bus.sh `(part of sg3_utils package)</span></span>
* <span data-ttu-id="c4962-278">Zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="c4962-278">Type hello following commands:</span></span>
  
    `$ dmesg | grep sd*`
     
     <span data-ttu-id="c4962-279">Nebo</span><span class="sxs-lookup"><span data-stu-id="c4962-279">Or</span></span>
  
    `$ fdisk –l`
  
    <span data-ttu-id="c4962-280">Tyto vrátí podrobnosti o nedávno přidané disky.</span><span class="sxs-lookup"><span data-stu-id="c4962-280">These will return details of recently added disks.</span></span>
* <span data-ttu-id="c4962-281">toodetermine toho, jestli je StorSimple disk, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="c4962-281">toodetermine whether it is a StorSimple disk, use hello following commands:</span></span>
  
    `cat /sys/block/<DISK>/device/model`
  
    <span data-ttu-id="c4962-282">Tato možnost vrátí řetězec, který bude zjistit, jestli je StorSimple disk.</span><span class="sxs-lookup"><span data-stu-id="c4962-282">This will return a string, which will determine if it’s a StorSimple disk.</span></span>

<span data-ttu-id="c4962-283">Méně pravděpodobné, ale možná příčina A mohou být také zastaralé iscsid pid.</span><span class="sxs-lookup"><span data-stu-id="c4962-283">A less likely but possible cause could also be stale iscsid pid.</span></span> <span data-ttu-id="c4962-284">Použijte následující příkaz toolog vypnout z relace iSCSI hello hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-284">Use hello following command toolog off from hello iSCSI sessions:</span></span>

    iscsiadm -m node --logout -p <Target_IP>

<span data-ttu-id="c4962-285">Tento příkaz opakujte u všech síťových rozhraní hello připojení na cíli iSCSI hello, což je zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c4962-285">Repeat this command for all hello connected network interfaces on hello iSCSI target, which is your StorSimple device.</span></span> <span data-ttu-id="c4962-286">Jakmile jste se odhlásili z všechny relace iSCSI hello, použijte hello iSCSI target relace iSCSI IQN tooreestablish hello.</span><span class="sxs-lookup"><span data-stu-id="c4962-286">Once you have logged off from all hello iSCSI sessions, use hello iSCSI target IQN tooreestablish hello iSCSI session.</span></span> <span data-ttu-id="c4962-287">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c4962-287">Type hello following command:</span></span>

    iscsiadm -m node --login -T <TARGET_IQN>


<span data-ttu-id="c4962-288">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="c4962-288">Q.</span></span> <span data-ttu-id="c4962-289">Nejste si jisti, pokud zařízení je seznam povolených adres.</span><span class="sxs-lookup"><span data-stu-id="c4962-289">I am not sure if my device is whitelisted.</span></span>

<span data-ttu-id="c4962-290">A.</span><span class="sxs-lookup"><span data-stu-id="c4962-290">A.</span></span> <span data-ttu-id="c4962-291">tooverify jestli zařízení je seznam povolených adres, použijte následující řešení potíží interaktivního příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="c4962-291">tooverify whether your device is whitelisted, use hello following troubleshooting interactive command:</span></span>

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


<span data-ttu-id="c4962-292">Další informace, přejděte příliš[použít řešení potíží s interaktivního příkazu pro více cest](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span><span class="sxs-lookup"><span data-stu-id="c4962-292">For more information, go too[use troubleshooting interactive command for multipathing](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span></span>

## <a name="list-of-useful-commands"></a><span data-ttu-id="c4962-293">Seznam užitečné příkazy</span><span class="sxs-lookup"><span data-stu-id="c4962-293">List of useful commands</span></span>
| <span data-ttu-id="c4962-294">Typ</span><span class="sxs-lookup"><span data-stu-id="c4962-294">Type</span></span> | <span data-ttu-id="c4962-295">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c4962-295">Command</span></span> | <span data-ttu-id="c4962-296">Popis</span><span class="sxs-lookup"><span data-stu-id="c4962-296">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c4962-297">**iSCSI**</span><span class="sxs-lookup"><span data-stu-id="c4962-297">**iSCSI**</span></span> |`service iscsid start` |<span data-ttu-id="c4962-298">Spuštění služby iSCSI</span><span class="sxs-lookup"><span data-stu-id="c4962-298">Start iSCSI service</span></span> |
| &nbsp; |`service iscsid stop` |<span data-ttu-id="c4962-299">Zastavit službu iSCSI</span><span class="sxs-lookup"><span data-stu-id="c4962-299">Stop iSCSI service</span></span> |
| &nbsp; |`service iscsid restart` |<span data-ttu-id="c4962-300">Restartujte službu iSCSI</span><span class="sxs-lookup"><span data-stu-id="c4962-300">Restart iSCSI service</span></span> |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |<span data-ttu-id="c4962-301">Zjišťování dostupných cílů na hello zadaná adresa</span><span class="sxs-lookup"><span data-stu-id="c4962-301">Discover available targets on hello specified address</span></span> |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |<span data-ttu-id="c4962-302">Přihlaste se toohello cíle iSCSI</span><span class="sxs-lookup"><span data-stu-id="c4962-302">Log in toohello iSCSI target</span></span> |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |<span data-ttu-id="c4962-303">Odhlaste se z cíle iSCSI hello</span><span class="sxs-lookup"><span data-stu-id="c4962-303">Log out from hello iSCSI target</span></span> |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |<span data-ttu-id="c4962-304">Tisk – název iniciátoru iSCSI</span><span class="sxs-lookup"><span data-stu-id="c4962-304">Print iSCSI initiator name</span></span> |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |<span data-ttu-id="c4962-305">Zkontrolujte stav hello relace iSCSI hello a svazek zjištěných na hostiteli hello</span><span class="sxs-lookup"><span data-stu-id="c4962-305">Check hello state of hello iSCSI session and volume discovered on hello host</span></span> |
| &nbsp; |`iscsi –m session` |<span data-ttu-id="c4962-306">Zobrazí všechny relace iSCSI hello navázat mezi hostiteli hello a hello zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="c4962-306">Shows all hello iSCSI sessions established between hello host and hello StorSimple device</span></span> |
|  | | |
| <span data-ttu-id="c4962-307">**Více cest**</span><span class="sxs-lookup"><span data-stu-id="c4962-307">**Multipathing**</span></span> |`service multipathd start` |<span data-ttu-id="c4962-308">Démon spuštění více cest</span><span class="sxs-lookup"><span data-stu-id="c4962-308">Start multipath daemon</span></span> |
| &nbsp; |`service multipathd stop` |<span data-ttu-id="c4962-309">Zastavit vícenásobný démon</span><span class="sxs-lookup"><span data-stu-id="c4962-309">Stop multipath daemon</span></span> |
| &nbsp; |`service multipathd restart` |<span data-ttu-id="c4962-310">Restartujte vícenásobný démon</span><span class="sxs-lookup"><span data-stu-id="c4962-310">Restart multipath daemon</span></span> |
| &nbsp; |`chkconfig multipathd on` </br> <span data-ttu-id="c4962-311">NEBO</span><span class="sxs-lookup"><span data-stu-id="c4962-311">OR</span></span> </br> `mpathconf –with_chkconfig y` |<span data-ttu-id="c4962-312">Povolení funkce multipath démon toostart při spuštění</span><span class="sxs-lookup"><span data-stu-id="c4962-312">Enable multipath daemon toostart at boot time</span></span> |
| &nbsp; |`multipathd –k` |<span data-ttu-id="c4962-313">Spuštění hello interaktivní konzoly pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c4962-313">Start hello interactive console for troubleshooting</span></span> |
| &nbsp; |`multipath –l` |<span data-ttu-id="c4962-314">Připojení více cest seznamu a zařízení</span><span class="sxs-lookup"><span data-stu-id="c4962-314">List multipath connections and devices</span></span> |
| &nbsp; |`mpathconf --enable` |<span data-ttu-id="c4962-315">Vytvořte soubor ukázka mulitpath.conf v`/etc/mulitpath.conf`</span><span class="sxs-lookup"><span data-stu-id="c4962-315">Create a sample mulitpath.conf file in `/etc/mulitpath.conf`</span></span> |
|  | | |

## <a name="next-steps"></a><span data-ttu-id="c4962-316">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4962-316">Next steps</span></span>
<span data-ttu-id="c4962-317">Jak na hostiteli systému Linux jsou konfigurace funkce MPIO, budete pravděpodobně potřebovat toorefer toohello následující dokumenty CentoS 6.6:</span><span class="sxs-lookup"><span data-stu-id="c4962-317">As you are configuring MPIO on Linux host, you may also need toorefer toohello following CentoS 6.6 documents:</span></span>

* [<span data-ttu-id="c4962-318">Nastavení funkce MPIO na CentOS</span><span class="sxs-lookup"><span data-stu-id="c4962-318">Setting up MPIO on CentOS</span></span>](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [<span data-ttu-id="c4962-319">Průvodce školení Linux</span><span class="sxs-lookup"><span data-stu-id="c4962-319">Linux Training Guide</span></span>](http://linux-training.be/files/books/LinuxAdm.pdf)

