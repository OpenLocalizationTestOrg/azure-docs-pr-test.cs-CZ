---
title: "Řešení potíží s HBase pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi na časté otázky týkající se práce s HBase a Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 7/7/2017
ms.author: nitinver
ms.openlocfilehash: 15412c3853a2b8436c5e96034c9a92a2a1094662
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a><span data-ttu-id="16491-103">Řešení potíží s HBase pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="16491-103">Troubleshoot HBase by using Azure HDInsight</span></span>

<span data-ttu-id="16491-104">Další informace o hlavních problémů a jejich řešení při práci s Apache HBase datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="16491-104">Learn about the top issues and their resolutions when working with Apache HBase payloads in Apache Ambari.</span></span>

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a><span data-ttu-id="16491-105">Jak spouštět sestavy příkaz hbck s několika nepřiřazené oblastí</span><span class="sxs-lookup"><span data-stu-id="16491-105">How do I run hbck command reports with multiple unassigned regions</span></span>

<span data-ttu-id="16491-106">Běžné chybová zpráva, že může dojít při spuštění `hbase hbck` příkaz je "více oblastí se nepřiřazené nebo díry v řetězu oblastí."</span><span class="sxs-lookup"><span data-stu-id="16491-106">A common error message that you might see when you run the `hbase hbck` command is "multiple regions being unassigned or holes in the chain of regions."</span></span>

<span data-ttu-id="16491-107">V uživatelském rozhraní HBase hlavní uvidíte počet oblastí, které jsou nevyvážené napříč všemi servery oblast.</span><span class="sxs-lookup"><span data-stu-id="16491-107">In the HBase Master UI, you can see the number of regions that are unbalanced across all region servers.</span></span> <span data-ttu-id="16491-108">Potom můžete spustit `hbase hbck` příkazu zobrazte díry v řetězu oblast.</span><span class="sxs-lookup"><span data-stu-id="16491-108">Then, you can run `hbase hbck` command to see holes in the region chain.</span></span>

<span data-ttu-id="16491-109">Díry může být způsobeno offline oblasti, takže opravte nejprve přiřazení.</span><span class="sxs-lookup"><span data-stu-id="16491-109">Holes might be caused by the offline regions, so fix the assignments first.</span></span> 

<span data-ttu-id="16491-110">Aby nepřiřazené oblasti zpět k normálním stavu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="16491-110">To bring the unassigned regions back to a normal state, complete the following steps:</span></span>

1. <span data-ttu-id="16491-111">Přihlaste se ke clusteru HDInsight HBase pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="16491-111">Sign in to the HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="16491-112">Chcete-li připojit pomocí prostředí ZooKeeper, spusťte `hbase zkcli` příkaz.</span><span class="sxs-lookup"><span data-stu-id="16491-112">To connect with the ZooKeeper shell, run the `hbase zkcli` command.</span></span>
3. <span data-ttu-id="16491-113">Spustit `rmr /hbase/regions-in-transition` příkaz nebo `rmr /hbase-unsecure/regions-in-transition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="16491-113">Run the `rmr /hbase/regions-in-transition` command or the `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="16491-114">Chcete-li ukončit z `hbase zkcli` prostředí shell, použijte `exit` příkaz.</span><span class="sxs-lookup"><span data-stu-id="16491-114">To exit from the `hbase zkcli` shell, use the `exit` command.</span></span>
5. <span data-ttu-id="16491-115">Otevřete uživatelské rozhraní Apache Ambari a potom restartujte službu hlavní HBase aktivní.</span><span class="sxs-lookup"><span data-stu-id="16491-115">Open the Apache Ambari UI, and then restart the Active HBase Master service.</span></span>
6. <span data-ttu-id="16491-116">Spustit `hbase hbck` příkaz znovu (bez jakékoli možnosti).</span><span class="sxs-lookup"><span data-stu-id="16491-116">Run the `hbase hbck` command again (without any options).</span></span> <span data-ttu-id="16491-117">Zkontrolujte výstup tohoto příkazu do zajistit přiřazení všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="16491-117">Check the output of this command to ensure that all regions are being assigned.</span></span>


## <span data-ttu-id="16491-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Jak se při použití příkazů hbck oblasti přiřazení opravte problémy vypršení časového limitu</span><span class="sxs-lookup"><span data-stu-id="16491-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>How do I fix timeout issues when using hbck commands for region assignments</span></span>

### <a name="issue"></a><span data-ttu-id="16491-119">Problém</span><span class="sxs-lookup"><span data-stu-id="16491-119">Issue</span></span>

<span data-ttu-id="16491-120">Možnou příčinou problémů vypršení časového limitu při použití `hbck` příkaz může být, že několik oblasti jsou ve stavu "v přechodném stavu" po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="16491-120">A potential cause for timeout issues when you use the `hbck` command might be that several regions are in the "in transition" state for a long time.</span></span> <span data-ttu-id="16491-121">Jako offline v uživatelském rozhraní HBase hlavní najdete v těchto oblastech.</span><span class="sxs-lookup"><span data-stu-id="16491-121">You can see those regions as offline in the HBase Master UI.</span></span> <span data-ttu-id="16491-122">Vzhledem k tomu, že vysoký počet oblastí se pokouší přechodu, hlavní HBase může časový limit a být schopen převést těmito oblastmi zpět do režimu online.</span><span class="sxs-lookup"><span data-stu-id="16491-122">Because a high number of regions are attempting to transition, HBase Master might timeout and be unable to bring those regions back online.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="16491-123">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-123">Resolution steps</span></span>

1. <span data-ttu-id="16491-124">Přihlaste se ke clusteru HDInsight HBase pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="16491-124">Sign in to the HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="16491-125">Chcete-li připojit pomocí prostředí ZooKeeper, spusťte `hbase zkcli` příkaz.</span><span class="sxs-lookup"><span data-stu-id="16491-125">To connect with the ZooKeeper shell, run the `hbase zkcli` command.</span></span>
3. <span data-ttu-id="16491-126">Spustit `rmr /hbase/regions-in-transition` nebo `rmr /hbase-unsecure/regions-in-transition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="16491-126">Run the `rmr /hbase/regions-in-transition` or the `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="16491-127">Chcete-li ukončit `hbase zkcli` prostředí shell, použijte `exit` příkaz.</span><span class="sxs-lookup"><span data-stu-id="16491-127">To exit the `hbase zkcli` shell, use the `exit` command.</span></span>
5. <span data-ttu-id="16491-128">V uživatelském rozhraní Ambari restartujte službu hlavní HBase aktivní.</span><span class="sxs-lookup"><span data-stu-id="16491-128">In the Ambari UI, restart the Active HBase Master service.</span></span>
6. <span data-ttu-id="16491-129">Spustit `hbase hbck -fixAssignments` příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="16491-129">Run the `hbase hbck -fixAssignments` command again.</span></span>

## <span data-ttu-id="16491-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Jak I vynucení zakázání HDFS nouzovém režimu v clusteru</span><span class="sxs-lookup"><span data-stu-id="16491-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>How do I force-disable HDFS safe mode in a cluster</span></span>

### <a name="issue"></a><span data-ttu-id="16491-131">Problém</span><span class="sxs-lookup"><span data-stu-id="16491-131">Issue</span></span>

<span data-ttu-id="16491-132">Místní Hadoop Distributed File System (HDFS) se zasekla v nouzovém režimu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="16491-132">The local Hadoop Distributed File System (HDFS) is stuck in safe mode on the HDInsight cluster.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="16491-133">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="16491-133">Detailed description</span></span>

<span data-ttu-id="16491-134">Tato chyba může být způsobena chybou, když spustíte následující příkaz HDFS:</span><span class="sxs-lookup"><span data-stu-id="16491-134">This error might be caused by a failure when you run the following HDFS command:</span></span>

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

<span data-ttu-id="16491-135">Chyba, kterou můžete setkat při pokusu spustit příkaz vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="16491-135">The error you might see when you try to run the command looks like this:</span></span>

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a><span data-ttu-id="16491-136">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="16491-136">Probable cause</span></span>

<span data-ttu-id="16491-137">Změny velikosti clusteru HDInsight se dolů k velmi několika uzlů.</span><span class="sxs-lookup"><span data-stu-id="16491-137">The HDInsight cluster has been scaled down to a very few nodes.</span></span> <span data-ttu-id="16491-138">Počet uzlů je menší než nebo blízko Multi-Factor replikace HDFS.</span><span class="sxs-lookup"><span data-stu-id="16491-138">The number of nodes is below or close to the HDFS replication factor.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="16491-139">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-139">Resolution steps</span></span> 

1. <span data-ttu-id="16491-140">Získáte stav HDFS v clusteru HDInsight spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="16491-140">Get the status of the HDFS on the HDInsight cluster by running the following commands:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdiuser@hn0-spark2:~$ hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   Safe mode is ON
   Configured Capacity: 3372381241344 (3.07 TB)
   Present Capacity: 3138625077248 (2.85 TB)
   DFS Remaining: 3102710317056 (2.82 TB)
   DFS Used: 35914760192 (33.45 GB)
   DFS Used%: 1.14%
   Under replicated blocks: 0
   Blocks with corrupt replicas: 0
   Missing blocks: 0
   Missing blocks (with replication factor 1): 0

   -------------------------------------------------
   Live datanodes (8):

   Name: 10.0.0.17:30010 (10.0.0.17)
   Hostname: 10.0.0.17
   Decommission Status : Normal
   Configured Capacity: 421547655168 (392.60 GB)
   DFS Used: 5288128512 (4.92 GB)
   Non DFS Used: 29087272960 (27.09 GB)
   DFS Remaining: 387172253696 (360.58 GB)
   DFS Used%: 1.25%
   DFS Remaining%: 91.85%
   Configured Cache Capacity: 0 (0 B)
   Cache Used: 0 (0 B)
   Cache Remaining: 0 (0 B)
   Cache Used%: 100.00%
   Cache Remaining%: 0.00%
   Xceivers: 2
   Last contact: Wed Apr 05 16:22:00 UTC 2017
   ...

   ```
2. <span data-ttu-id="16491-141">Také můžete zkontrolovat integritu HDFS v clusteru HDInsight pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="16491-141">You also can check the integrity of the HDFS on the HDInsight cluster by using the following commands:</span></span>

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting to namenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
   FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
   ....................................................................................................

   ....................................................................................................
   ..................Status: HEALTHY
   Total size:    9330539472 B
   Total dirs:    37
   Total files:   2618
   Total symlinks:                0 (Files currently being written: 2)
   Total blocks (validated):      2535 (avg. block size 3680686 B)
   Minimally replicated blocks:   2535 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       0 (0.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     3.0
   Corrupt blocks:                0
   Missing replicas:              0 (0.0 %)
   Number of data-nodes:          8
   Number of racks:               1
   FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds

   The filesystem under path '/' is HEALTHY
   ```

3. <span data-ttu-id="16491-142">Pokud zjistíte, že neexistují žádné chybí, poškozený, nebo under-replikované bloky, nebo že tyto bloky můžete ignorovat, spusťte následující příkaz provést název uzlu mimo nouzový režim:</span><span class="sxs-lookup"><span data-stu-id="16491-142">If you determine that there are no missing, corrupt, or under-replicated blocks, or that those blocks can be ignored, run the following command to take the name node out of safe mode:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a><span data-ttu-id="16491-143">Jak opravit JDBC nebo SQLLine připojením k problémům s Apache Phoenix</span><span class="sxs-lookup"><span data-stu-id="16491-143">How do I fix JDBC or SQLLine connectivity issues with Apache Phoenix</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="16491-144">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-144">Resolution steps</span></span>

<span data-ttu-id="16491-145">Pro připojení k Phoenix, je nutné zadat IP adresu aktivní uzel ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="16491-145">To connect with Phoenix, you must provide the IP address of an active ZooKeeper node.</span></span> <span data-ttu-id="16491-146">Ujistěte se, že služba ZooKeeper sqlline.py, které se pokouší o připojení, není spuštěná.</span><span class="sxs-lookup"><span data-stu-id="16491-146">Ensure that the ZooKeeper service to which sqlline.py is trying to connect is up and running.</span></span>
1. <span data-ttu-id="16491-147">Přihlaste se ke clusteru HDInsight pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="16491-147">Sign in to the HDInsight cluster by using SSH.</span></span>
2. <span data-ttu-id="16491-148">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="16491-148">Enter the following command:</span></span>
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > <span data-ttu-id="16491-149">IP adresa aktivního uzlu ZooKeeper můžete získat z uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="16491-149">You can get the IP address of the active ZooKeeper node from the Ambari UI.</span></span> <span data-ttu-id="16491-150">Přejděte na **HBase** > **rychlé odkazy** > **ZK\* (aktivní)** > **Zookeeper informace**.</span><span class="sxs-lookup"><span data-stu-id="16491-150">Go to **HBase** > **Quick Links** > **ZK\* (Active)** > **Zookeeper Info**.</span></span> 

3. <span data-ttu-id="16491-151">Pokud sqlline.py připojí k Phoenix a nemá vypršení časového limitu, spusťte následující příkaz k ověření dostupnosti a stav Phoenix:</span><span class="sxs-lookup"><span data-stu-id="16491-151">If the sqlline.py connects to Phoenix and does not timeout, run the following command to validate the availability and health of Phoenix:</span></span>

   ```apache
           !tables
           !quit
   ```      
4. <span data-ttu-id="16491-152">Pokud tento příkaz lze použít, neexistuje žádný problém.</span><span class="sxs-lookup"><span data-stu-id="16491-152">If this command works, there is no issue.</span></span> <span data-ttu-id="16491-153">IP adresa zadaná uživatelem je pravděpodobně nesprávná.</span><span class="sxs-lookup"><span data-stu-id="16491-153">The IP address provided by the user might be incorrect.</span></span> <span data-ttu-id="16491-154">Ale pokud příkaz pozastaví po delší dobu a potom zobrazí chybová zpráva, přejděte ke kroku 5.</span><span class="sxs-lookup"><span data-stu-id="16491-154">However, if the command pauses for an extended time and then displays the following error, continue to step 5.</span></span>

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. <span data-ttu-id="16491-155">Spusťte následující příkazy z hlavního uzlu (hn0), který se při diagnostice stav Phoenix systému. Tabulka katalogu:</span><span class="sxs-lookup"><span data-stu-id="16491-155">Run the following commands from the head node (hn0) to diagnose the condition of the Phoenix SYSTEM.CATALOG table:</span></span>

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   <span data-ttu-id="16491-156">Příkaz by měla vrátit chybu podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="16491-156">The command should return an error similar to the following:</span></span> 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. <span data-ttu-id="16491-157">V uživatelském rozhraní Ambari proveďte následující kroky a restartujte službu HMaster na všechny uzly ZooKeeper:</span><span class="sxs-lookup"><span data-stu-id="16491-157">In the Ambari UI, complete the following steps to restart the HMaster service on all ZooKeeper nodes:</span></span>

    1. <span data-ttu-id="16491-158">V **Souhrn** části HBase, přejděte na **HBase** > **Active HBase hlavní**.</span><span class="sxs-lookup"><span data-stu-id="16491-158">In the **Summary** section of HBase, go to **HBase** > **Active HBase Master**.</span></span> 
    2. <span data-ttu-id="16491-159">V **součásti** část, restartujte službu hlavní HBase.</span><span class="sxs-lookup"><span data-stu-id="16491-159">In the **Components** section, restart the HBase Master service.</span></span>
    3. <span data-ttu-id="16491-160">Tento postup opakujte pro všechny zbývající **pohotovostní HBase hlavní** služby.</span><span class="sxs-lookup"><span data-stu-id="16491-160">Repeat these steps for all remaining **Standby HBase Master** services.</span></span> 

<span data-ttu-id="16491-161">To může trvat až pět minut, než služba HBase Master stabilizovat a dokončit proces obnovení.</span><span class="sxs-lookup"><span data-stu-id="16491-161">It can take up to five minutes for the HBase Master service to stabilize and finish the recovery process.</span></span> <span data-ttu-id="16491-162">Po několika minutách opakujte příkazy sqlline.py, ujistěte se, že v systému. Tabulka katalogu je zapnutý, a to může být dotazován.</span><span class="sxs-lookup"><span data-stu-id="16491-162">After a few minutes, repeat the sqlline.py commands to confirm that the SYSTEM.CATALOG table is up, and that it can be queried.</span></span> 

<span data-ttu-id="16491-163">Pokud v systému. Tabulka katalogu je zpět do normální, problém s připojením k Phoenix by měla být vyřešen automaticky.</span><span class="sxs-lookup"><span data-stu-id="16491-163">When the SYSTEM.CATALOG table is back to normal, the connectivity issue to Phoenix should be automatically resolved.</span></span>


## <a name="what-causes-a-master-server-to-fail-to-start"></a><span data-ttu-id="16491-164">Co způsobí, že hlavní server nezdaří spustit</span><span class="sxs-lookup"><span data-stu-id="16491-164">What causes a master server to fail to start</span></span>

### <a name="error"></a><span data-ttu-id="16491-165">Chyba</span><span class="sxs-lookup"><span data-stu-id="16491-165">Error</span></span> 

<span data-ttu-id="16491-166">Dojde k selhání atomic přejmenování.</span><span class="sxs-lookup"><span data-stu-id="16491-166">An atomic renaming failure occurs.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="16491-167">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="16491-167">Detailed description</span></span>

<span data-ttu-id="16491-168">Během procesu spuštění dokončení HMaster mnoho kroků inicializace.</span><span class="sxs-lookup"><span data-stu-id="16491-168">During the startup process, HMaster completes many initialization steps.</span></span> <span data-ttu-id="16491-169">Jedná se o přesouvání dat ve složce začátku (TMP) ke složce data.</span><span class="sxs-lookup"><span data-stu-id="16491-169">These include moving data from the scratch (.tmp) folder to the data folder.</span></span> <span data-ttu-id="16491-170">HMaster také zjistí složce předběžné protokolů (WALs), pokud jsou k dispozici žádné servery reagovat oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="16491-170">HMaster also looks at the write-ahead logs (WALs) folder to see if there are any unresponsive region servers, and so on.</span></span> 

<span data-ttu-id="16491-171">Během spouštění HMaster nemá základní `list` příkazu u těchto složek.</span><span class="sxs-lookup"><span data-stu-id="16491-171">During startup, HMaster does a basic `list` command on these folders.</span></span> <span data-ttu-id="16491-172">Pokud kdykoli, uvidí HMaster neočekávaný soubor v žádném z těchto složek, vyvolá výjimku a nespustí.</span><span class="sxs-lookup"><span data-stu-id="16491-172">If at any time, HMaster sees an unexpected file in any of these folders, it throws an exception and doesn't start.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="16491-173">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="16491-173">Probable cause</span></span>

<span data-ttu-id="16491-174">V protokolech serveru oblast pokuste se identifikovat časovou osu vytvoření souboru a zjistěte, pokud byl proces havárií v době, kdy byl vytvořen soubor.</span><span class="sxs-lookup"><span data-stu-id="16491-174">In the region server logs, try to identify the timeline of the file creation, and then see if there was a process crash around the time the file was created.</span></span> <span data-ttu-id="16491-175">(Obraťte se na podporu HBase vám pomůže to). Tento pomůže nám poskytují robustnější mechanismy, takže se můžete vyhnout stiskne to chyb a ujistěte se proces řádné vypnutí systému.</span><span class="sxs-lookup"><span data-stu-id="16491-175">(Contact HBase support to assist you in doing this.) This helps us provide more robust mechanisms, so that you can avoid hitting this bug, and ensure graceful process shutdowns.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="16491-176">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-176">Resolution steps</span></span>

<span data-ttu-id="16491-177">Zkontrolujte zásobník volání a určete složku, ke které může být příčinou problému (například se může být složce WALs nebo ve složce tmp).</span><span class="sxs-lookup"><span data-stu-id="16491-177">Check the call stack and try to determine which folder might be causing the problem (for instance, it might be the WALs folder or the .tmp folder).</span></span> <span data-ttu-id="16491-178">V Průzkumníku cloudu, nebo pomocí příkazů HDFS, poté zkuste najít soubor problém.</span><span class="sxs-lookup"><span data-stu-id="16491-178">Then, in Cloud Explorer or by using HDFS commands, try to locate the problem file.</span></span> <span data-ttu-id="16491-179">Obvykle se jedná \*-renamePending.json souboru.</span><span class="sxs-lookup"><span data-stu-id="16491-179">Usually, this is a \*-renamePending.json file.</span></span> <span data-ttu-id="16491-180">( \*-RenamePending.json soubor je soubor deníku, který se používá k implementaci operaci atomic přejmenování v ovladači WASB.</span><span class="sxs-lookup"><span data-stu-id="16491-180">(The \*-renamePending.json file is a journal file that's used to implement the atomic rename operation in the WASB driver.</span></span> <span data-ttu-id="16491-181">Z důvodu chyby v této implementaci těchto souborů může být zbyly poté, co mimoprocesových atd.) Vynutit odstranění tohoto souboru v Průzkumníku cloudu nebo pomocí příkazů HDFS.</span><span class="sxs-lookup"><span data-stu-id="16491-181">Due to bugs in this implementation, these files can be left over after process crashes, and so on.) Force-delete this file either in Cloud Explorer or by using HDFS commands.</span></span> 

<span data-ttu-id="16491-182">V některých případech mohou existovat i dočasný soubor s názvem něco podobného jako *$$$. $$$* v tomto umístění.</span><span class="sxs-lookup"><span data-stu-id="16491-182">Sometimes, there might also be a temporary file named something like *$$$.$$$* at this location.</span></span> <span data-ttu-id="16491-183">Budete muset použít HDFS `ls` příkaz, který má tento soubor; nelze naleznete v souboru v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="16491-183">You have to use HDFS `ls` command to see this file; you cannot see the file in Cloud Explorer.</span></span> <span data-ttu-id="16491-184">K odstranění tohoto souboru, použijte příkaz HDFS `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span><span class="sxs-lookup"><span data-stu-id="16491-184">To delete this file, use the HDFS command `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span></span>  

<span data-ttu-id="16491-185">Po spuštění těchto příkazů, HMaster by měl spustit okamžitě.</span><span class="sxs-lookup"><span data-stu-id="16491-185">After you've run these commands, HMaster should start immediately.</span></span> 

### <a name="error"></a><span data-ttu-id="16491-186">Chyba</span><span class="sxs-lookup"><span data-stu-id="16491-186">Error</span></span>

<span data-ttu-id="16491-187">Žádná adresa serveru, je uvedena ve *hbase: meta* pro oblast xxx.</span><span class="sxs-lookup"><span data-stu-id="16491-187">No server address is listed in *hbase: meta* for region xxx.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="16491-188">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="16491-188">Detailed description</span></span>

<span data-ttu-id="16491-189">Může se zobrazit zpráva na váš cluster Linux, která označuje, že *hbase: meta* tabulka není online.</span><span class="sxs-lookup"><span data-stu-id="16491-189">You might see a message on your Linux cluster that indicates that the *hbase: meta* table is not online.</span></span> <span data-ttu-id="16491-190">Spuštění `hbck` může zobrazit zprávu, že "hbase: meta tabulky replicaId 0 nebyl nalezen v libovolné oblasti."</span><span class="sxs-lookup"><span data-stu-id="16491-190">Running `hbck` might report that "hbase: meta table replicaId 0 is not found on any region."</span></span> <span data-ttu-id="16491-191">Tento problém může být, že HMaster nebylo možné inicializovat po restartování HBase.</span><span class="sxs-lookup"><span data-stu-id="16491-191">The problem might be that HMaster could not initialize after you restarted HBase.</span></span> <span data-ttu-id="16491-192">V protokolech HMaster může zobrazit zpráva: "žádná adresa serveru uvedené v hbase: meta pro oblast hbase: zálohování \<název oblasti\>".</span><span class="sxs-lookup"><span data-stu-id="16491-192">In the HMaster logs, you might see the message: "No server address listed in hbase: meta for region hbase: backup \<region name\>".</span></span>  

### <a name="resolution-steps"></a><span data-ttu-id="16491-193">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-193">Resolution steps</span></span>

1. <span data-ttu-id="16491-194">V prostředí HBase zadejte následující příkazy (skutečné hodnoty pro změnu podle vhodnosti):</span><span class="sxs-lookup"><span data-stu-id="16491-194">In the HBase shell, enter the following commands (change actual values as applicable):</span></span>  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. <span data-ttu-id="16491-195">Odstranit *hbase: obor názvů* položku.</span><span class="sxs-lookup"><span data-stu-id="16491-195">Delete the *hbase: namespace* entry.</span></span> <span data-ttu-id="16491-196">Tato položka může být ke stejné chybě, která se při hlášené *hbase: obor názvů* tabulka je skenován.</span><span class="sxs-lookup"><span data-stu-id="16491-196">This entry might be the same error that's being reported when the *hbase: namespace* table is scanned.</span></span>

3. <span data-ttu-id="16491-197">Restartujte službu Active HMaster zprovoznit HBase v běžícím stavu, v uživatelském rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="16491-197">To bring up HBase in a running state, in the Ambari UI, restart the Active HMaster service.</span></span>  

4. <span data-ttu-id="16491-198">V prostředí HBase se zprovoznit všechny tabulky v režimu offline, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="16491-198">In the HBase shell, to bring up all offline tables, run the following command:</span></span>

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a><span data-ttu-id="16491-199">Další čtení</span><span class="sxs-lookup"><span data-stu-id="16491-199">Additional reading</span></span>

[<span data-ttu-id="16491-200">Nelze zpracovat tabulku HBase</span><span class="sxs-lookup"><span data-stu-id="16491-200">Unable to process the HBase table</span></span>](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a><span data-ttu-id="16491-201">Chyba</span><span class="sxs-lookup"><span data-stu-id="16491-201">Error</span></span>

<span data-ttu-id="16491-202">HMaster časového limitu se podobně jako závažné výjimce "java.io.IOException: Timedout 300000ms čekání tabulky obor názvů pro přiřazení."</span><span class="sxs-lookup"><span data-stu-id="16491-202">HMaster times out with a fatal exception similar to "java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned."</span></span>

### <a name="detailed-description"></a><span data-ttu-id="16491-203">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="16491-203">Detailed description</span></span>

<span data-ttu-id="16491-204">Tomuto problému může dojít, pokud máte mnoho tabulek a oblasti, které nebyly byly vyprázdněny, při restartování vaší HMaster služeb.</span><span class="sxs-lookup"><span data-stu-id="16491-204">You might experience this issue if you have many tables and regions that have not been flushed when you restart your HMaster services.</span></span> <span data-ttu-id="16491-205">Restartování může selhat a uvidíte uvedená chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="16491-205">Restart might fail, and you'll see the preceding error message.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="16491-206">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="16491-206">Probable cause</span></span>

<span data-ttu-id="16491-207">Jde o známý problém se službou HMaster.</span><span class="sxs-lookup"><span data-stu-id="16491-207">This is a known issue with the HMaster service.</span></span> <span data-ttu-id="16491-208">Spuštění úlohy obecné clusteru může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="16491-208">General cluster startup tasks can take a long time.</span></span> <span data-ttu-id="16491-209">HMaster vypne vzhledem k tomu, že v tabulce oboru názvů není dosud přiřazen.</span><span class="sxs-lookup"><span data-stu-id="16491-209">HMaster shuts down because the namespace table isn’t yet assigned.</span></span> <span data-ttu-id="16491-210">K tomu dochází jenom v situacích, ve kterém velké množství dat, unflushed existuje a není dostatečná vypršení časového limitu pěti minut.</span><span class="sxs-lookup"><span data-stu-id="16491-210">This occurs only in scenarios in which large amount of unflushed data exists, and a timeout of five minutes is not sufficient.</span></span>
  
### <a name="resolution-steps"></a><span data-ttu-id="16491-211">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-211">Resolution steps</span></span>

1. <span data-ttu-id="16491-212">V uživatelském rozhraní Ambari, přejděte do **HBase** > **konfigurací**.</span><span class="sxs-lookup"><span data-stu-id="16491-212">In the Ambari UI, go to **HBase** > **Configs**.</span></span> <span data-ttu-id="16491-213">V souboru vlastní hbase-site.xml přidejte následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="16491-213">In the custom hbase-site.xml file, add the following setting:</span></span> 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. <span data-ttu-id="16491-214">Restartujte službu (HMaster a případně dalších služeb HBase).</span><span class="sxs-lookup"><span data-stu-id="16491-214">Restart the required services (HMaster, and possibly other HBase services).</span></span>  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a><span data-ttu-id="16491-215">Co způsobí restartování selhání na serveru oblast</span><span class="sxs-lookup"><span data-stu-id="16491-215">What causes a restart failure on a region server</span></span>

### <a name="issue"></a><span data-ttu-id="16491-216">Problém</span><span class="sxs-lookup"><span data-stu-id="16491-216">Issue</span></span>

<span data-ttu-id="16491-217">Selhání restartování v oblasti serveru může být zabránit následující osvědčené postupy.</span><span class="sxs-lookup"><span data-stu-id="16491-217">A restart failure on a region server might be prevented by following best practices.</span></span> <span data-ttu-id="16491-218">Doporučujeme, abyste pozastavit aktivity vytížen, pokud máte v úmyslu restartování serverů oblast HBase.</span><span class="sxs-lookup"><span data-stu-id="16491-218">We recommend that you pause heavy workload activity when you are planning to restart HBase region servers.</span></span> <span data-ttu-id="16491-219">Pokud k připojení se servery pro oblast, když probíhá vypnutí i aplikace, bude operace restartování serveru oblast pomalejší o několik minut.</span><span class="sxs-lookup"><span data-stu-id="16491-219">If an application continues to connect with region servers when shutdown is in progress, the region server restart operation will be slower by several minutes.</span></span> <span data-ttu-id="16491-220">Je také vhodné nejprve vyprázdnit všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="16491-220">Also, it's a good idea to first flush all the tables.</span></span> <span data-ttu-id="16491-221">Odkaz na tom, jak vyprázdnění tabulky, najdete v části [HDInsight HBase: jak ke zlepšení čas restartování clusteru HBase pomocí Vyčištění tabulky](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span><span class="sxs-lookup"><span data-stu-id="16491-221">For a reference on how to flush tables, see [HDInsight HBase: How to improve the HBase cluster restart time by flushing tables](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span></span>

<span data-ttu-id="16491-222">Pokud spustíte operace restartování serverů oblast HBase z rozhraní Ambari, zobrazí okamžitě servery oblast byl vypnut, že nemáte hned restartovat.</span><span class="sxs-lookup"><span data-stu-id="16491-222">If you initiate the restart operation on HBase region servers from the Ambari UI, you immediately see that the region servers went down, but they don't restart right away.</span></span> 

<span data-ttu-id="16491-223">Zde je, co se děje na pozadí:</span><span class="sxs-lookup"><span data-stu-id="16491-223">Here's what's happening behind the scenes:</span></span> 

1. <span data-ttu-id="16491-224">Ambari agent odešle žádost o zastavení serveru oblast.</span><span class="sxs-lookup"><span data-stu-id="16491-224">The Ambari agent sends a stop request to the region server.</span></span>
2. <span data-ttu-id="16491-225">Ambari agent čeká 30 sekund pro oblast server korektně vypnout.</span><span class="sxs-lookup"><span data-stu-id="16491-225">The Ambari agent waits for 30 seconds for the region server to shut down gracefully.</span></span> 
3. <span data-ttu-id="16491-226">Pokud vaše aplikace nadále připojit k serveru oblast, server nebude vypnout okamžitě.</span><span class="sxs-lookup"><span data-stu-id="16491-226">If your application continues to connect with the region server, the server won't shut down immediately.</span></span> <span data-ttu-id="16491-227">Předtím, než dojde k vypnutí vyprší časový limit 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="16491-227">The 30-second timeout expires before shutdown occurs.</span></span> 
4. <span data-ttu-id="16491-228">Po 30 sekund, Ambari agent odešle force-kill (`kill -9`) příkaz k serveru oblast.</span><span class="sxs-lookup"><span data-stu-id="16491-228">After 30 seconds, the Ambari agent sends a force-kill (`kill -9`) command to the region server.</span></span> <span data-ttu-id="16491-229">Můžete to vidět v protokolu agenta ambari (v /var nebo protokolu nebo adresář příslušných pracovního uzlu):</span><span class="sxs-lookup"><span data-stu-id="16491-229">You can see this in the ambari-agent log (in the /var/log/ directory of the respective worker node):</span></span>

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
<span data-ttu-id="16491-230">Z důvodu náhlému vypnutí nemusí být vydání port spojených s procesem, přestože proces serveru oblast je zastavena.</span><span class="sxs-lookup"><span data-stu-id="16491-230">Because of the abrupt shutdown, the port associated with the process might not be released, even though the region server process is stopped.</span></span> <span data-ttu-id="16491-231">Tato situace může vést k AddressBindException během spouštění serveru oblast, jak je znázorněno v následujících protokolech.</span><span class="sxs-lookup"><span data-stu-id="16491-231">This situation can lead to an AddressBindException when the region server is starting, as shown in the following logs.</span></span> <span data-ttu-id="16491-232">Můžete to ověřit v oblasti server.log v adresáři /var/log/hbase na pracovních uzlech, které oblasti serveru nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="16491-232">You can verify this in the region-server.log in the /var/log/hbase directory on the worker nodes where the region server fails to start.</span></span> 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a><span data-ttu-id="16491-233">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="16491-233">Resolution steps</span></span>

1. <span data-ttu-id="16491-234">Pokuste se snížit zatížení serverů oblast HBase před spuštěním restartování.</span><span class="sxs-lookup"><span data-stu-id="16491-234">Try to reduce the load on the HBase region servers before you initiate a restart.</span></span> 
2. <span data-ttu-id="16491-235">Můžete taky (Pokud kroku 1 vám nepomohly), pokuste se restartovat servery oblast na pracovních uzlech ručně pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="16491-235">Alternatively (if step 1 doesn't help), try to manually restart region servers on the worker nodes by using the following commands:</span></span>

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

