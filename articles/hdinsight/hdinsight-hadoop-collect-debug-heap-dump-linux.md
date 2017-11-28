---
title: "pro služby Hadoop v HDInsight - Azure výpisy paměti haldy aaaEnable | Microsoft Docs"
description: "Povolte výpisů paměti haldy pro služby Hadoop z clusterů HDInsight se systémem Linux pro ladění a analýzu."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8f151adb-f687-41e4-aca0-82b551953725
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 49e30f26e1a83f19e068e9da253b5548caec70d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a><span data-ttu-id="ab95d-103">Povolit výpisů paměti haldy pro služby Hadoop v HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="ab95d-103">Enable heap dumps for Hadoop services on Linux-based HDInsight</span></span>

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="ab95d-104">Výpisy haldy obsahovat snímek paměti hello aplikace, včetně hello hodnoty proměnných v hello čas vytvoření výpisu hello.</span><span class="sxs-lookup"><span data-stu-id="ab95d-104">Heap dumps contain a snapshot of hello application's memory, including hello values of variables at hello time hello dump was created.</span></span> <span data-ttu-id="ab95d-105">Takže se hodí pro diagnostiku problémů, ke kterým dochází za běhu.</span><span class="sxs-lookup"><span data-stu-id="ab95d-105">So they are useful for diagnosing problems that occur at run-time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab95d-106">Hello kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ab95d-106">hello steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="ab95d-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ab95d-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab95d-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab95d-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="ab95d-109"><a name="whichServices"></a>Služby</span><span class="sxs-lookup"><span data-stu-id="ab95d-109"><a name="whichServices"></a>Services</span></span>

<span data-ttu-id="ab95d-110">Můžete povolit výpisů paměti haldy pro hello následující služby:</span><span class="sxs-lookup"><span data-stu-id="ab95d-110">You can enable heap dumps for hello following services:</span></span>

* <span data-ttu-id="ab95d-111">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="ab95d-111">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="ab95d-112">**Hive** -hiveserver2, metaúložiště, derbyserver</span><span class="sxs-lookup"><span data-stu-id="ab95d-112">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="ab95d-113">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="ab95d-113">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="ab95d-114">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="ab95d-114">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="ab95d-115">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="ab95d-115">**hdfs** - datanode, secondarynamenode, namenode</span></span>

<span data-ttu-id="ab95d-116">Můžete také povolit výpisů paměti haldy pro mapu hello a snížit procesy spuštěné prostřednictvím HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab95d-116">You can also enable heap dumps for hello map and reduce processes ran by HDInsight.</span></span>

## <span data-ttu-id="ab95d-117"><a name="configuration"></a>Principy haldy výpis konfigurace</span><span class="sxs-lookup"><span data-stu-id="ab95d-117"><a name="configuration"></a>Understanding heap dump configuration</span></span>

<span data-ttu-id="ab95d-118">Výpisy haldy jsou povolené předáním možnosti (někdy označuje jako požádá, nebo parametry) toohello JVM při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="ab95d-118">Heap dumps are enabled by passing options (sometimes known as opts, or parameters) toohello JVM when a service is started.</span></span> <span data-ttu-id="ab95d-119">Pro většinu služby Hadoop můžete upravit hello prostředí skriptu použít toostart hello služby toopass tyto možnosti.</span><span class="sxs-lookup"><span data-stu-id="ab95d-119">For most Hadoop services, you can modify hello shell script used toostart hello service toopass these options.</span></span>

<span data-ttu-id="ab95d-120">Do každého skriptu, je exportu pro  **\* \_OPTS**, který obsahuje možnosti hello předán toohello JVM.</span><span class="sxs-lookup"><span data-stu-id="ab95d-120">In each script, there is an export for **\*\_OPTS**, which contains hello options passed toohello JVM.</span></span> <span data-ttu-id="ab95d-121">Například v hello **hadoop env.sh** skriptu, hello řádek, který začíná `export HADOOP_NAMENODE_OPTS=` obsahuje hello možnosti hello NameNode služby.</span><span class="sxs-lookup"><span data-stu-id="ab95d-121">For example, in hello **hadoop-env.sh** script, hello line that begins with `export HADOOP_NAMENODE_OPTS=` contains hello options for hello NameNode service.</span></span>

<span data-ttu-id="ab95d-122">Mapování a snížit procesy se mírně liší, tyto operace jsou podřízený proces systému hello MapReduce služby.</span><span class="sxs-lookup"><span data-stu-id="ab95d-122">Map and reduce processes are slightly different, as these operations are a child process of hello MapReduce service.</span></span> <span data-ttu-id="ab95d-123">Každý mapování, nebo snižte proces běží v kontejneru podřízené a existují dvě položky, které obsahují hello JVM možnosti.</span><span class="sxs-lookup"><span data-stu-id="ab95d-123">Each map or reduce process runs in a child container, and there are two entries that contain hello JVM options.</span></span> <span data-ttu-id="ab95d-124">Obě obsažené v **mapred-site.xml**:</span><span class="sxs-lookup"><span data-stu-id="ab95d-124">Both contained in **mapred-site.xml**:</span></span>

* <span data-ttu-id="ab95d-125">**mapreduce.Admin.map.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="ab95d-125">**mapreduce.admin.map.child.java.opts**</span></span>
* <span data-ttu-id="ab95d-126">**mapreduce.Admin.reduce.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="ab95d-126">**mapreduce.admin.reduce.child.java.opts**</span></span>

> [!NOTE]
> <span data-ttu-id="ab95d-127">Doporučujeme pomocí nástroje Ambari toomodify obou hello skripty a mapred-site.xml nastavení, jako Ambari zpracování replikace změn mezi uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ab95d-127">We recommend using Ambari toomodify both hello scripts and mapred-site.xml settings, as Ambari handle replicating changes across nodes in hello cluster.</span></span> <span data-ttu-id="ab95d-128">V tématu hello [pomocí Ambari](#using-ambari) konkrétní postup.</span><span class="sxs-lookup"><span data-stu-id="ab95d-128">See hello [Using Ambari](#using-ambari) section for specific steps.</span></span>

### <a name="enable-heap-dumps"></a><span data-ttu-id="ab95d-129">Povolení výpisů paměti haldy</span><span class="sxs-lookup"><span data-stu-id="ab95d-129">Enable heap dumps</span></span>

<span data-ttu-id="ab95d-130">Hello následující možnost umožňuje výpisy heap když dojde OutOfMemoryError:</span><span class="sxs-lookup"><span data-stu-id="ab95d-130">hello following option enables heap dumps when an OutOfMemoryError occurs:</span></span>

    -XX:+HeapDumpOnOutOfMemoryError

<span data-ttu-id="ab95d-131">Hello  **+**  označuje, že tato možnost je povolená.</span><span class="sxs-lookup"><span data-stu-id="ab95d-131">hello **+** indicates that this option is enabled.</span></span> <span data-ttu-id="ab95d-132">Výchozí Hello je zakázána.</span><span class="sxs-lookup"><span data-stu-id="ab95d-132">hello default is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="ab95d-133">Jako hello souborů výpisu paměti může být velký haldy výpisy nejsou povolené pro služby Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab95d-133">Heap dumps are not enabled for Hadoop services on HDInsight by default, as hello dump files can be large.</span></span> <span data-ttu-id="ab95d-134">Pokud povolíte je pro řešení potíží, mějte na paměti toodisable je po opakuje se hello problému a získaná hello souborů výpisu paměti.</span><span class="sxs-lookup"><span data-stu-id="ab95d-134">If you do enable them for troubleshooting, remember toodisable them once you have reproduced hello problem and gathered hello dump files.</span></span>

### <a name="dump-location"></a><span data-ttu-id="ab95d-135">Výpis umístění</span><span class="sxs-lookup"><span data-stu-id="ab95d-135">Dump location</span></span>

<span data-ttu-id="ab95d-136">Hello výchozí umístění souboru s výpisem hello je hello aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="ab95d-136">hello default location for hello dump file is hello current working directory.</span></span> <span data-ttu-id="ab95d-137">Můžete řídit hello kde je uložený soubor pomocí hello následující možnost:</span><span class="sxs-lookup"><span data-stu-id="ab95d-137">You can control where hello file is stored using hello following option:</span></span>

    -XX:HeapDumpPath=/path

<span data-ttu-id="ab95d-138">Například pomocí `-XX:HeapDumpPath=/tmp` způsobí, že toobe výpisy hello uložené v adresáři TMP hello.</span><span class="sxs-lookup"><span data-stu-id="ab95d-138">For example, using `-XX:HeapDumpPath=/tmp` causes hello dumps toobe stored in hello /tmp directory.</span></span>

### <a name="scripts"></a><span data-ttu-id="ab95d-139">Skripty</span><span class="sxs-lookup"><span data-stu-id="ab95d-139">Scripts</span></span>

<span data-ttu-id="ab95d-140">Můžete také spustit skript při **OutOfMemoryError** dojde.</span><span class="sxs-lookup"><span data-stu-id="ab95d-140">You can also trigger a script when an **OutOfMemoryError** occurs.</span></span> <span data-ttu-id="ab95d-141">Například spouštěcí oznámení, abyste věděli, že chyba hello došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="ab95d-141">For example, triggering a notification so you know that hello error has occurred.</span></span> <span data-ttu-id="ab95d-142">Použití hello následující možnost tootrigger skript __OutOfMemoryError__:</span><span class="sxs-lookup"><span data-stu-id="ab95d-142">Use hello following option tootrigger a script on an __OutOfMemoryError__:</span></span>

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> <span data-ttu-id="ab95d-143">Vzhledem k tomu, že Hadoop je distribuovaný systém, musí být umístěny všech skriptů používá na všech uzlech v clusteru hello, který běží služba hello na.</span><span class="sxs-lookup"><span data-stu-id="ab95d-143">Since Hadoop is a distributed system, any script used must be placed on all nodes in hello cluster that hello service runs on.</span></span>
> 
> <span data-ttu-id="ab95d-144">skript Hello musí také být v umístění, která je přístupná pomocí nástroje hello účet hello service běží jako a musí poskytnout oprávnění ke spouštění.</span><span class="sxs-lookup"><span data-stu-id="ab95d-144">hello script must also be in a location that is accessible by hello account hello service runs as, and must provide execute permissions.</span></span> <span data-ttu-id="ab95d-145">Například můžete toostore skripty v `/usr/local/bin` a používat `chmod go+rx /usr/local/bin/filename.sh` toogrant číst a spouštět oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ab95d-145">For example, you may wish toostore scripts in `/usr/local/bin` and use `chmod go+rx /usr/local/bin/filename.sh` toogrant read and execute permissions.</span></span>

## <a name="using-ambari"></a><span data-ttu-id="ab95d-146">Pomocí nástroje Ambari</span><span class="sxs-lookup"><span data-stu-id="ab95d-146">Using Ambari</span></span>

<span data-ttu-id="ab95d-147">Konfigurace hello toomodify služby hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ab95d-147">toomodify hello configuration for a service, use hello following steps:</span></span>

1. <span data-ttu-id="ab95d-148">Otevřete hello Ambari webového uživatelského rozhraní pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="ab95d-148">Open hello Ambari web UI for your cluster.</span></span> <span data-ttu-id="ab95d-149">Adresa URL Hello je https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="ab95d-149">hello URL is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span>

    <span data-ttu-id="ab95d-150">Po zobrazení výzvy ověřování toohello lokality pomocí názvu účtu hello HTTP (výchozí: správce) a heslo pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="ab95d-150">When prompted, authenticate toohello site using hello HTTP account name (default: admin) and password for your cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ab95d-151">Můžete být vyzváni podruhé pomocí Ambari pro hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="ab95d-151">You may be prompted a second time by Ambari for hello user name and password.</span></span> <span data-ttu-id="ab95d-152">Pokud ano, zadejte text hello stejný název účtu a heslo</span><span class="sxs-lookup"><span data-stu-id="ab95d-152">If so, enter hello same account name and password</span></span>

2. <span data-ttu-id="ab95d-153">Pomocí seznamu hello na levé straně hello vyberte oblasti služby hello chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="ab95d-153">Using hello list of on hello left, select hello service area you want toomodify.</span></span> <span data-ttu-id="ab95d-154">Například **HDFS**.</span><span class="sxs-lookup"><span data-stu-id="ab95d-154">For example, **HDFS**.</span></span> <span data-ttu-id="ab95d-155">V oblasti hello center, vyberte hello **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="ab95d-155">In hello center area, select hello **Configs** tab.</span></span>

    ![Obrázek Ambari web s vybranou kartou HDFS konfigurací](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. <span data-ttu-id="ab95d-157">Pomocí hello **filtru...**  položku, zadejte **požádá**.</span><span class="sxs-lookup"><span data-stu-id="ab95d-157">Using hello **Filter...** entry, enter **opts**.</span></span> <span data-ttu-id="ab95d-158">Zobrazí se pouze položky obsahující tento text.</span><span class="sxs-lookup"><span data-stu-id="ab95d-158">Only items containing this text are displayed.</span></span>

    ![Filtrovaný seznam](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. <span data-ttu-id="ab95d-160">Najde hello  **\* \_OPTS** položka služby hello výpisů paměti haldy tooenable pro a přidejte hello možnosti, které chcete tooenable.</span><span class="sxs-lookup"><span data-stu-id="ab95d-160">Find hello **\*\_OPTS** entry for hello service you want tooenable heap dumps for, and add hello options you wish tooenable.</span></span> <span data-ttu-id="ab95d-161">V hello následující bitové kopie, byly přidány `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** položku:</span><span class="sxs-lookup"><span data-stu-id="ab95d-161">In hello following image, I've added `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** entry:</span></span>

    ![HADOOP_NAMENODE_OPTS s - XX: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > <span data-ttu-id="ab95d-163">Při povolení výpisů paměti haldy pro hello mapy nebo snižte podřízeného procesu, vyhledejte hello pole s názvem **mapreduce.admin.map.child.java.opts** a **mapreduce.admin.reduce.child.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="ab95d-163">When enabling heap dumps for hello map or reduce child process, look for hello fields named **mapreduce.admin.map.child.java.opts** and **mapreduce.admin.reduce.child.java.opts**.</span></span>

    <span data-ttu-id="ab95d-164">Použití hello **Uložit** tlačítko toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="ab95d-164">Use hello **Save** button toosave hello changes.</span></span> <span data-ttu-id="ab95d-165">Můžete zadat krátký poznámka popisující hello změny.</span><span class="sxs-lookup"><span data-stu-id="ab95d-165">You can enter a short note describing hello changes.</span></span>

5. <span data-ttu-id="ab95d-166">Jakmile provedli změny hello hello **je vyžadován Restart** zobrazí ikona vedle jednu nebo více služeb.</span><span class="sxs-lookup"><span data-stu-id="ab95d-166">Once hello changes have been applied, hello **Restart required** icon appears beside one or more services.</span></span>

    ![Restartujte vyžaduje ikonu a restartujte tlačítko](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. <span data-ttu-id="ab95d-168">Vyberte každá služba, která vyžaduje restartování a použijte hello **služby akce** tlačítko příliš**zapnout v režimu údržby**.</span><span class="sxs-lookup"><span data-stu-id="ab95d-168">Select each service that needs a restart, and use hello **Service Actions** button too**Turn On Maintenance Mode**.</span></span> <span data-ttu-id="ab95d-169">Režim údržby zabrání výstrahy generován ze služby hello při restartování.</span><span class="sxs-lookup"><span data-stu-id="ab95d-169">Maintenance mode prevents alerts from being generated from hello service when you restart it.</span></span>

    ![Zapnout nabídky režim údržby](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. <span data-ttu-id="ab95d-171">Po povolení režimu údržby použít hello **restartujte** tlačítko pro službu hello příliš**restartujte všechny uskutečněn**</span><span class="sxs-lookup"><span data-stu-id="ab95d-171">Once you have enabled maintenance mode, use hello **Restart** button for hello service too**Restart All Effected**</span></span>

    ![Restartujte všechny ovlivněné položky](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > <span data-ttu-id="ab95d-173">Hello položky pro hello **restartujte** tlačítko se může lišit pro jiné služby.</span><span class="sxs-lookup"><span data-stu-id="ab95d-173">hello entries for hello **Restart** button may be different for other services.</span></span>

8. <span data-ttu-id="ab95d-174">Po spuštění hello služeb, použijte hello **služby akce** tlačítko příliš**zapnout vypnout režimu údržby**.</span><span class="sxs-lookup"><span data-stu-id="ab95d-174">Once hello services have been restarted, use hello **Service Actions** button too**Turn Off Maintenance Mode**.</span></span> <span data-ttu-id="ab95d-175">Tato Ambari tooresume monitorování výstrah pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="ab95d-175">This Ambari tooresume monitoring for alerts for hello service.</span></span>

