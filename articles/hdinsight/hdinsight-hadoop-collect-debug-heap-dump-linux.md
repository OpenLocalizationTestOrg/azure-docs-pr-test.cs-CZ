---
title: "Povolit výpisů paměti haldy pro služby Hadoop v HDInsight - Azure | Microsoft Docs"
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
ms.openlocfilehash: 59942e989d622c2486edf181d76e13344c71e6f9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a><span data-ttu-id="a8d82-103">Povolit výpisů paměti haldy pro služby Hadoop v HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="a8d82-103">Enable heap dumps for Hadoop services on Linux-based HDInsight</span></span>

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="a8d82-104">Výpisy haldy obsahovat snímek paměti aplikace, včetně hodnot proměnných v době, kdy byla vytvořena výpis.</span><span class="sxs-lookup"><span data-stu-id="a8d82-104">Heap dumps contain a snapshot of the application's memory, including the values of variables at the time the dump was created.</span></span> <span data-ttu-id="a8d82-105">Takže se hodí pro diagnostiku problémů, ke kterým dochází za běhu.</span><span class="sxs-lookup"><span data-stu-id="a8d82-105">So they are useful for diagnosing problems that occur at run-time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8d82-106">Kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a8d82-106">The steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="a8d82-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a8d82-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a8d82-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a8d82-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="a8d82-109"><a name="whichServices"></a>Služby</span><span class="sxs-lookup"><span data-stu-id="a8d82-109"><a name="whichServices"></a>Services</span></span>

<span data-ttu-id="a8d82-110">Můžete povolit výpisů paměti haldy pro následující služby:</span><span class="sxs-lookup"><span data-stu-id="a8d82-110">You can enable heap dumps for the following services:</span></span>

* <span data-ttu-id="a8d82-111">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="a8d82-111">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="a8d82-112">**Hive** -hiveserver2, metaúložiště, derbyserver</span><span class="sxs-lookup"><span data-stu-id="a8d82-112">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="a8d82-113">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="a8d82-113">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="a8d82-114">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="a8d82-114">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="a8d82-115">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="a8d82-115">**hdfs** - datanode, secondarynamenode, namenode</span></span>

<span data-ttu-id="a8d82-116">Můžete také povolit výpisů paměti haldy mapy a snížit procesy spuštěné prostřednictvím HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a8d82-116">You can also enable heap dumps for the map and reduce processes ran by HDInsight.</span></span>

## <span data-ttu-id="a8d82-117"><a name="configuration"></a>Principy haldy výpis konfigurace</span><span class="sxs-lookup"><span data-stu-id="a8d82-117"><a name="configuration"></a>Understanding heap dump configuration</span></span>

<span data-ttu-id="a8d82-118">Výpisy haldy jsou povolené předáním možnosti (někdy označuje jako požádá, nebo parametry) na JVM při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="a8d82-118">Heap dumps are enabled by passing options (sometimes known as opts, or parameters) to the JVM when a service is started.</span></span> <span data-ttu-id="a8d82-119">Pro většinu služby Hadoop můžete upravit skript prostředí používá ke spuštění služby předávání tyto možnosti.</span><span class="sxs-lookup"><span data-stu-id="a8d82-119">For most Hadoop services, you can modify the shell script used to start the service to pass these options.</span></span>

<span data-ttu-id="a8d82-120">Do každého skriptu, je exportu pro  **\* \_OPTS**, který obsahuje možnosti, které předaný systém JVM.</span><span class="sxs-lookup"><span data-stu-id="a8d82-120">In each script, there is an export for **\*\_OPTS**, which contains the options passed to the JVM.</span></span> <span data-ttu-id="a8d82-121">Například v **hadoop env.sh** skriptu, řádek, který začíná `export HADOOP_NAMENODE_OPTS=` obsahuje možnosti pro službu NameNode.</span><span class="sxs-lookup"><span data-stu-id="a8d82-121">For example, in the **hadoop-env.sh** script, the line that begins with `export HADOOP_NAMENODE_OPTS=` contains the options for the NameNode service.</span></span>

<span data-ttu-id="a8d82-122">Mapování a snížit procesy jsou mírně liší, jsou tyto operace podřízený proces služby MapReduce.</span><span class="sxs-lookup"><span data-stu-id="a8d82-122">Map and reduce processes are slightly different, as these operations are a child process of the MapReduce service.</span></span> <span data-ttu-id="a8d82-123">Každý mapování, nebo snižte proces běží v kontejneru podřízené a existují dvě položky, které obsahují možnosti JVM.</span><span class="sxs-lookup"><span data-stu-id="a8d82-123">Each map or reduce process runs in a child container, and there are two entries that contain the JVM options.</span></span> <span data-ttu-id="a8d82-124">Obě obsažené v **mapred-site.xml**:</span><span class="sxs-lookup"><span data-stu-id="a8d82-124">Both contained in **mapred-site.xml**:</span></span>

* <span data-ttu-id="a8d82-125">**mapreduce.Admin.map.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="a8d82-125">**mapreduce.admin.map.child.java.opts**</span></span>
* <span data-ttu-id="a8d82-126">**mapreduce.Admin.reduce.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="a8d82-126">**mapreduce.admin.reduce.child.java.opts**</span></span>

> [!NOTE]
> <span data-ttu-id="a8d82-127">Doporučujeme pomocí nástroje Ambari k úpravě skripty a mapred-site.xml nastavení, jako popisovač Ambari replikace změn mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a8d82-127">We recommend using Ambari to modify both the scripts and mapred-site.xml settings, as Ambari handle replicating changes across nodes in the cluster.</span></span> <span data-ttu-id="a8d82-128">Najdete v článku [pomocí Ambari](#using-ambari) konkrétní postup.</span><span class="sxs-lookup"><span data-stu-id="a8d82-128">See the [Using Ambari](#using-ambari) section for specific steps.</span></span>

### <a name="enable-heap-dumps"></a><span data-ttu-id="a8d82-129">Povolení výpisů paměti haldy</span><span class="sxs-lookup"><span data-stu-id="a8d82-129">Enable heap dumps</span></span>

<span data-ttu-id="a8d82-130">Tato možnost umožňuje haldy výpisy, když dojde OutOfMemoryError:</span><span class="sxs-lookup"><span data-stu-id="a8d82-130">The following option enables heap dumps when an OutOfMemoryError occurs:</span></span>

    -XX:+HeapDumpOnOutOfMemoryError

<span data-ttu-id="a8d82-131"> **+**  Označuje, že tato možnost je povolená.</span><span class="sxs-lookup"><span data-stu-id="a8d82-131">The **+** indicates that this option is enabled.</span></span> <span data-ttu-id="a8d82-132">Výchozí hodnota je zakázána.</span><span class="sxs-lookup"><span data-stu-id="a8d82-132">The default is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="a8d82-133">Jako souborů výpisu paměti může být velký haldy výpisy nejsou povolené pro služby Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a8d82-133">Heap dumps are not enabled for Hadoop services on HDInsight by default, as the dump files can be large.</span></span> <span data-ttu-id="a8d82-134">Pokud povolíte je pro řešení potíží, nezapomeňte po reprodukovat problému a shromáždění souborů výpisu paměti je zakázat.</span><span class="sxs-lookup"><span data-stu-id="a8d82-134">If you do enable them for troubleshooting, remember to disable them once you have reproduced the problem and gathered the dump files.</span></span>

### <a name="dump-location"></a><span data-ttu-id="a8d82-135">Výpis umístění</span><span class="sxs-lookup"><span data-stu-id="a8d82-135">Dump location</span></span>

<span data-ttu-id="a8d82-136">Výchozí umístění pro soubor výpisu je aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="a8d82-136">The default location for the dump file is the current working directory.</span></span> <span data-ttu-id="a8d82-137">Můžete řídit tam, kde je soubor uložený pomocí následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="a8d82-137">You can control where the file is stored using the following option:</span></span>

    -XX:HeapDumpPath=/path

<span data-ttu-id="a8d82-138">Například pomocí `-XX:HeapDumpPath=/tmp` způsobí, že výpisy má být uložen v adresáři TMP.</span><span class="sxs-lookup"><span data-stu-id="a8d82-138">For example, using `-XX:HeapDumpPath=/tmp` causes the dumps to be stored in the /tmp directory.</span></span>

### <a name="scripts"></a><span data-ttu-id="a8d82-139">Skripty</span><span class="sxs-lookup"><span data-stu-id="a8d82-139">Scripts</span></span>

<span data-ttu-id="a8d82-140">Můžete také spustit skript při **OutOfMemoryError** dojde.</span><span class="sxs-lookup"><span data-stu-id="a8d82-140">You can also trigger a script when an **OutOfMemoryError** occurs.</span></span> <span data-ttu-id="a8d82-141">Například spouštěcí oznámení, abyste věděli, že došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="a8d82-141">For example, triggering a notification so you know that the error has occurred.</span></span> <span data-ttu-id="a8d82-142">Použijte možnost pro spuštění skriptu na __OutOfMemoryError__:</span><span class="sxs-lookup"><span data-stu-id="a8d82-142">Use the following option to trigger a script on an __OutOfMemoryError__:</span></span>

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> <span data-ttu-id="a8d82-143">Vzhledem k tomu, že Hadoop je distribuovaný systém, musí být umístěny všech skriptů používá na všech uzlech v clusteru, který spouští službu na.</span><span class="sxs-lookup"><span data-stu-id="a8d82-143">Since Hadoop is a distributed system, any script used must be placed on all nodes in the cluster that the service runs on.</span></span>
> 
> <span data-ttu-id="a8d82-144">Skript musí také být v umístění, které je přístupný pro účet služby spouští jako a zadejte oprávnění ke spouštění.</span><span class="sxs-lookup"><span data-stu-id="a8d82-144">The script must also be in a location that is accessible by the account the service runs as, and must provide execute permissions.</span></span> <span data-ttu-id="a8d82-145">Například můžete chtít uložit skripty v `/usr/local/bin` a používat `chmod go+rx /usr/local/bin/filename.sh` oprávnění ke spouštění a udělte pro čtení.</span><span class="sxs-lookup"><span data-stu-id="a8d82-145">For example, you may wish to store scripts in `/usr/local/bin` and use `chmod go+rx /usr/local/bin/filename.sh` to grant read and execute permissions.</span></span>

## <a name="using-ambari"></a><span data-ttu-id="a8d82-146">Pomocí nástroje Ambari</span><span class="sxs-lookup"><span data-stu-id="a8d82-146">Using Ambari</span></span>

<span data-ttu-id="a8d82-147">Pokud chcete upravit konfiguraci pro službu, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8d82-147">To modify the configuration for a service, use the following steps:</span></span>

1. <span data-ttu-id="a8d82-148">Otevřete webovému uživatelskému rozhraní Ambari pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="a8d82-148">Open the Ambari web UI for your cluster.</span></span> <span data-ttu-id="a8d82-149">Adresa URL je https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="a8d82-149">The URL is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span>

    <span data-ttu-id="a8d82-150">Po zobrazení výzvy ověřování k webu pomocí názvu účtu HTTP (výchozí: správce) a heslo pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="a8d82-150">When prompted, authenticate to the site using the HTTP account name (default: admin) and password for your cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a8d82-151">Můžete být vyzváni podruhé pomocí Ambari pro uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="a8d82-151">You may be prompted a second time by Ambari for the user name and password.</span></span> <span data-ttu-id="a8d82-152">Pokud ano, zadejte stejný název účtu a heslo</span><span class="sxs-lookup"><span data-stu-id="a8d82-152">If so, enter the same account name and password</span></span>

2. <span data-ttu-id="a8d82-153">Pomocí seznamu na levé straně vyberte oblast služby, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="a8d82-153">Using the list of on the left, select the service area you want to modify.</span></span> <span data-ttu-id="a8d82-154">Například **HDFS**.</span><span class="sxs-lookup"><span data-stu-id="a8d82-154">For example, **HDFS**.</span></span> <span data-ttu-id="a8d82-155">V oblasti center, vyberte **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="a8d82-155">In the center area, select the **Configs** tab.</span></span>

    ![Obrázek Ambari web s vybranou kartou HDFS konfigurací](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. <span data-ttu-id="a8d82-157">Pomocí **filtru...**  položku, zadejte **požádá**.</span><span class="sxs-lookup"><span data-stu-id="a8d82-157">Using the **Filter...** entry, enter **opts**.</span></span> <span data-ttu-id="a8d82-158">Zobrazí se pouze položky obsahující tento text.</span><span class="sxs-lookup"><span data-stu-id="a8d82-158">Only items containing this text are displayed.</span></span>

    ![Filtrovaný seznam](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. <span data-ttu-id="a8d82-160">Najít  **\* \_OPTS** záznam pro službu, kterou chcete povolit výpisů paměti haldy pro a přidání možnosti, které chcete povolit.</span><span class="sxs-lookup"><span data-stu-id="a8d82-160">Find the **\*\_OPTS** entry for the service you want to enable heap dumps for, and add the options you wish to enable.</span></span> <span data-ttu-id="a8d82-161">Na následujícím obrázku, byly přidány `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` k **HADOOP\_NAMENODE\_OPTS** položku:</span><span class="sxs-lookup"><span data-stu-id="a8d82-161">In the following image, I've added `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` to the **HADOOP\_NAMENODE\_OPTS** entry:</span></span>

    ![HADOOP_NAMENODE_OPTS s - XX: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > <span data-ttu-id="a8d82-163">Při povolení haldy výpisy mapy nebo snižte podřízeného procesu, vyhledejte pole s názvem **mapreduce.admin.map.child.java.opts** a **mapreduce.admin.reduce.child.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="a8d82-163">When enabling heap dumps for the map or reduce child process, look for the fields named **mapreduce.admin.map.child.java.opts** and **mapreduce.admin.reduce.child.java.opts**.</span></span>

    <span data-ttu-id="a8d82-164">Použití **Uložit** tlačítko a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="a8d82-164">Use the **Save** button to save the changes.</span></span> <span data-ttu-id="a8d82-165">Můžete zadat krátký poznámka popisující změny.</span><span class="sxs-lookup"><span data-stu-id="a8d82-165">You can enter a short note describing the changes.</span></span>

5. <span data-ttu-id="a8d82-166">Po použití změny **je vyžadován Restart** zobrazí ikona vedle jednu nebo více služeb.</span><span class="sxs-lookup"><span data-stu-id="a8d82-166">Once the changes have been applied, the **Restart required** icon appears beside one or more services.</span></span>

    ![Restartujte vyžaduje ikonu a restartujte tlačítko](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. <span data-ttu-id="a8d82-168">Vyberte každá služba, která vyžaduje restartování a pomocí **služby akce** tlačítko pro **zapnout v režimu údržby**.</span><span class="sxs-lookup"><span data-stu-id="a8d82-168">Select each service that needs a restart, and use the **Service Actions** button to **Turn On Maintenance Mode**.</span></span> <span data-ttu-id="a8d82-169">Režim údržby zabrání výstrahy generován ze služby, pokud byste ji restartovat.</span><span class="sxs-lookup"><span data-stu-id="a8d82-169">Maintenance mode prevents alerts from being generated from the service when you restart it.</span></span>

    ![Zapnout nabídky režim údržby](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. <span data-ttu-id="a8d82-171">Po povolení režimu údržby pomocí **restartujte** tlačítko pro službu, kterou chcete **restartujte všechny uskutečněn**</span><span class="sxs-lookup"><span data-stu-id="a8d82-171">Once you have enabled maintenance mode, use the **Restart** button for the service to **Restart All Effected**</span></span>

    ![Restartujte všechny ovlivněné položky](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > <span data-ttu-id="a8d82-173">položky pro **restartujte** tlačítko se může lišit pro jiné služby.</span><span class="sxs-lookup"><span data-stu-id="a8d82-173">the entries for the **Restart** button may be different for other services.</span></span>

8. <span data-ttu-id="a8d82-174">Až po restartování služby, pomocí **služby akce** tlačítko pro **zapnout vypnout režimu údržby**.</span><span class="sxs-lookup"><span data-stu-id="a8d82-174">Once the services have been restarted, use the **Service Actions** button to **Turn Off Maintenance Mode**.</span></span> <span data-ttu-id="a8d82-175">Tato Ambari obnovení monitorování pro výstrahy pro službu.</span><span class="sxs-lookup"><span data-stu-id="a8d82-175">This Ambari to resume monitoring for alerts for the service.</span></span>

