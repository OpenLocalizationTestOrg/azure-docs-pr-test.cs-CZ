---
title: "aaaTroubleshoot HBase pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se práce s HBase a Azure HDInsight."
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
ms.openlocfilehash: 5210184f8ea95628952a95df8c98f5b98e37c53e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a><span data-ttu-id="5358e-103">Řešení potíží s HBase pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5358e-103">Troubleshoot HBase by using Azure HDInsight</span></span>

<span data-ttu-id="5358e-104">Další informace o hello nejčastější problémy a jejich řešení při práci s Apache HBase datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="5358e-104">Learn about hello top issues and their resolutions when working with Apache HBase payloads in Apache Ambari.</span></span>

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a><span data-ttu-id="5358e-105">Jak spouštět sestavy příkaz hbck s několika nepřiřazené oblastí</span><span class="sxs-lookup"><span data-stu-id="5358e-105">How do I run hbck command reports with multiple unassigned regions</span></span>

<span data-ttu-id="5358e-106">Běžné chybová zpráva, že může dojít, když spustíte hello `hbase hbck` příkaz je "více oblastí se nepřiřazené nebo díry v řetězu hello oblastí."</span><span class="sxs-lookup"><span data-stu-id="5358e-106">A common error message that you might see when you run hello `hbase hbck` command is "multiple regions being unassigned or holes in hello chain of regions."</span></span>

<span data-ttu-id="5358e-107">V hello HBase hlavního uživatelského rozhraní se zobrazí hello počet oblastí, které jsou nevyvážené napříč všemi servery oblast.</span><span class="sxs-lookup"><span data-stu-id="5358e-107">In hello HBase Master UI, you can see hello number of regions that are unbalanced across all region servers.</span></span> <span data-ttu-id="5358e-108">Potom můžete spustit `hbase hbck` příkaz toosee díry v řetězu oblast hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-108">Then, you can run `hbase hbck` command toosee holes in hello region chain.</span></span>

<span data-ttu-id="5358e-109">Díry může být způsobeno hello offline oblastech, tak přiřazení hello oprava nejdřív.</span><span class="sxs-lookup"><span data-stu-id="5358e-109">Holes might be caused by hello offline regions, so fix hello assignments first.</span></span> 

<span data-ttu-id="5358e-110">toobring hello nepřiřazené oblasti back tooa normálním stavu, dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5358e-110">toobring hello unassigned regions back tooa normal state, complete hello following steps:</span></span>

1. <span data-ttu-id="5358e-111">Přihlaste se toohello clusteru HDInsight HBase pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="5358e-111">Sign in toohello HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="5358e-112">tooconnect s hello ZooKeeper prostředí, spusťte hello `hbase zkcli` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5358e-112">tooconnect with hello ZooKeeper shell, run hello `hbase zkcli` command.</span></span>
3. <span data-ttu-id="5358e-113">Spustit hello `rmr /hbase/regions-in-transition` příkaz nebo hello `rmr /hbase-unsecure/regions-in-transition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5358e-113">Run hello `rmr /hbase/regions-in-transition` command or hello `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="5358e-114">tooexit z hello `hbase zkcli` prostředí shell, použijte hello `exit` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5358e-114">tooexit from hello `hbase zkcli` shell, use hello `exit` command.</span></span>
5. <span data-ttu-id="5358e-115">Otevřete hello Apache Ambari uživatelského rozhraní a potom restartujte službu Active HBase hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-115">Open hello Apache Ambari UI, and then restart hello Active HBase Master service.</span></span>
6. <span data-ttu-id="5358e-116">Spustit hello `hbase hbck` příkaz znovu (bez jakékoli možnosti).</span><span class="sxs-lookup"><span data-stu-id="5358e-116">Run hello `hbase hbck` command again (without any options).</span></span> <span data-ttu-id="5358e-117">Zkontrolujte výstup hello tooensure tento příkaz, které jsou přiřazeny všechny oblasti.</span><span class="sxs-lookup"><span data-stu-id="5358e-117">Check hello output of this command tooensure that all regions are being assigned.</span></span>


## <span data-ttu-id="5358e-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Jak se při použití příkazů hbck oblasti přiřazení opravte problémy vypršení časového limitu</span><span class="sxs-lookup"><span data-stu-id="5358e-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>How do I fix timeout issues when using hbck commands for region assignments</span></span>

### <a name="issue"></a><span data-ttu-id="5358e-119">Problém</span><span class="sxs-lookup"><span data-stu-id="5358e-119">Issue</span></span>

<span data-ttu-id="5358e-120">Možnou příčinou problémů vypršení časového limitu při použití hello `hbck` příkaz může být, že několik oblasti jsou ve hello "v přechodném stavu" stavu po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="5358e-120">A potential cause for timeout issues when you use hello `hbck` command might be that several regions are in hello "in transition" state for a long time.</span></span> <span data-ttu-id="5358e-121">Najdete v těchto oblastech jako offline v hello HBase hlavního uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5358e-121">You can see those regions as offline in hello HBase Master UI.</span></span> <span data-ttu-id="5358e-122">Vzhledem k tomu, že vysoký počet oblastí se pokoušíte tootransition, hlavní HBase může časový limit a být nelze toobring těmito oblastmi zpět do režimu online.</span><span class="sxs-lookup"><span data-stu-id="5358e-122">Because a high number of regions are attempting tootransition, HBase Master might timeout and be unable toobring those regions back online.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="5358e-123">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-123">Resolution steps</span></span>

1. <span data-ttu-id="5358e-124">Přihlaste se toohello clusteru HDInsight HBase pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="5358e-124">Sign in toohello HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="5358e-125">tooconnect s hello ZooKeeper prostředí, spusťte hello `hbase zkcli` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5358e-125">tooconnect with hello ZooKeeper shell, run hello `hbase zkcli` command.</span></span>
3. <span data-ttu-id="5358e-126">Spustit hello `rmr /hbase/regions-in-transition` nebo hello `rmr /hbase-unsecure/regions-in-transition` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5358e-126">Run hello `rmr /hbase/regions-in-transition` or hello `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="5358e-127">tooexit hello `hbase zkcli` prostředí shell, použijte hello `exit` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5358e-127">tooexit hello `hbase zkcli` shell, use hello `exit` command.</span></span>
5. <span data-ttu-id="5358e-128">V hello uživatelského rozhraní Ambari restartujte službu Active HBase hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-128">In hello Ambari UI, restart hello Active HBase Master service.</span></span>
6. <span data-ttu-id="5358e-129">Spustit hello `hbase hbck -fixAssignments` příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="5358e-129">Run hello `hbase hbck -fixAssignments` command again.</span></span>

## <span data-ttu-id="5358e-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Jak I vynucení zakázání HDFS nouzovém režimu v clusteru</span><span class="sxs-lookup"><span data-stu-id="5358e-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>How do I force-disable HDFS safe mode in a cluster</span></span>

### <a name="issue"></a><span data-ttu-id="5358e-131">Problém</span><span class="sxs-lookup"><span data-stu-id="5358e-131">Issue</span></span>

<span data-ttu-id="5358e-132">Hello místní Hadoop Distributed File System (HDFS) se zasekla v nouzovém režimu v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-132">hello local Hadoop Distributed File System (HDFS) is stuck in safe mode on hello HDInsight cluster.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="5358e-133">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="5358e-133">Detailed description</span></span>

<span data-ttu-id="5358e-134">Tato chyba může být způsobena chybou, když spustíte následující příkaz HDFS hello:</span><span class="sxs-lookup"><span data-stu-id="5358e-134">This error might be caused by a failure when you run hello following HDFS command:</span></span>

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

<span data-ttu-id="5358e-135">Hello chyba, kterou můžete setkat při pokusu o toorun hello příkaz vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5358e-135">hello error you might see when you try toorun hello command looks like this:</span></span>

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" tooturn safe mode off.
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

### <a name="probable-cause"></a><span data-ttu-id="5358e-136">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="5358e-136">Probable cause</span></span>

<span data-ttu-id="5358e-137">Hello clusteru HDInsight byl škálovat dolů tooa jen několik uzlů.</span><span class="sxs-lookup"><span data-stu-id="5358e-137">hello HDInsight cluster has been scaled down tooa very few nodes.</span></span> <span data-ttu-id="5358e-138">Hello počet uzlů je menší než nebo zavřete toohello HDFS replikace faktor.</span><span class="sxs-lookup"><span data-stu-id="5358e-138">hello number of nodes is below or close toohello HDFS replication factor.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="5358e-139">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-139">Resolution steps</span></span> 

1. <span data-ttu-id="5358e-140">Získáte stav hello hello HDFS na hello HDInsight clusteru spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5358e-140">Get hello status of hello HDFS on hello HDInsight cluster by running hello following commands:</span></span>

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
2. <span data-ttu-id="5358e-141">Také můžete zkontrolovat integritu hello hello HDFS na hello HDInsight clusteru pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5358e-141">You also can check hello integrity of hello HDFS on hello HDInsight cluster by using hello following commands:</span></span>

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting toonamenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
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

   hello filesystem under path '/' is HEALTHY
   ```

3. <span data-ttu-id="5358e-142">Pokud zjistíte, že neexistují žádné chybějí, jsou poškozené nebo under-replikované bloky, nebo že tyto bloky můžete ignorovat, spusťte následující příkaz tootake hello název uzlu mimo nouzový režim hello:</span><span class="sxs-lookup"><span data-stu-id="5358e-142">If you determine that there are no missing, corrupt, or under-replicated blocks, or that those blocks can be ignored, run hello following command tootake hello name node out of safe mode:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a><span data-ttu-id="5358e-143">Jak opravit JDBC nebo SQLLine připojením k problémům s Apache Phoenix</span><span class="sxs-lookup"><span data-stu-id="5358e-143">How do I fix JDBC or SQLLine connectivity issues with Apache Phoenix</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="5358e-144">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-144">Resolution steps</span></span>

<span data-ttu-id="5358e-145">tooconnect s Phoenix, je nutné zadat IP adresu hello aktivní uzel ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="5358e-145">tooconnect with Phoenix, you must provide hello IP address of an active ZooKeeper node.</span></span> <span data-ttu-id="5358e-146">Ujistěte se, že hello ZooKeeper sqlline.py toowhich služba se pokouší tooconnect je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="5358e-146">Ensure that hello ZooKeeper service toowhich sqlline.py is trying tooconnect is up and running.</span></span>
1. <span data-ttu-id="5358e-147">Přihlaste se toohello clusteru HDInsight pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="5358e-147">Sign in toohello HDInsight cluster by using SSH.</span></span>
2. <span data-ttu-id="5358e-148">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5358e-148">Enter hello following command:</span></span>
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > <span data-ttu-id="5358e-149">Můžete získat IP adresu hello aktivní uzel ZooKeeper hello hello uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="5358e-149">You can get hello IP address of hello active ZooKeeper node from hello Ambari UI.</span></span> <span data-ttu-id="5358e-150">Přejděte příliš**HBase** > **rychlé odkazy** > **ZK\* (aktivní)** > **Zookeeper informace**.</span><span class="sxs-lookup"><span data-stu-id="5358e-150">Go too**HBase** > **Quick Links** > **ZK\* (Active)** > **Zookeeper Info**.</span></span> 

3. <span data-ttu-id="5358e-151">Pokud hello sqlline.py připojí tooPhoenix a nemá vypršení časového limitu, hello spusťte následující příkaz toovalidate hello dostupnosti a stavu Phoenix:</span><span class="sxs-lookup"><span data-stu-id="5358e-151">If hello sqlline.py connects tooPhoenix and does not timeout, run hello following command toovalidate hello availability and health of Phoenix:</span></span>

   ```apache
           !tables
           !quit
   ```      
4. <span data-ttu-id="5358e-152">Pokud tento příkaz lze použít, neexistuje žádný problém.</span><span class="sxs-lookup"><span data-stu-id="5358e-152">If this command works, there is no issue.</span></span> <span data-ttu-id="5358e-153">Hello IP adresa zadaná uživatelem hello můžou být nesprávné.</span><span class="sxs-lookup"><span data-stu-id="5358e-153">hello IP address provided by hello user might be incorrect.</span></span> <span data-ttu-id="5358e-154">Nicméně pokud hello příkaz pozastaví po delší dobu a potom zobrazí následující chyba hello, pokračujte toostep 5.</span><span class="sxs-lookup"><span data-stu-id="5358e-154">However, if hello command pauses for an extended time and then displays hello following error, continue toostep 5.</span></span>

   ```apache
           Error while connecting toosqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting toojdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. <span data-ttu-id="5358e-155">Spuštěním následujících příkazů z hlavního uzlu (hn0) hello toodiagnose hello podmínku hello Phoenix systému hello. Tabulka katalogu:</span><span class="sxs-lookup"><span data-stu-id="5358e-155">Run hello following commands from hello head node (hn0) toodiagnose hello condition of hello Phoenix SYSTEM.CATALOG table:</span></span>

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   <span data-ttu-id="5358e-156">příkaz Hello by měla vrátit následující chybě podobné toohello:</span><span class="sxs-lookup"><span data-stu-id="5358e-156">hello command should return an error similar toohello following:</span></span> 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. <span data-ttu-id="5358e-157">V hello uživatelského rozhraní Ambari proveďte následující kroky toorestart hello HMaster služby na všechny uzly ZooKeeper hello:</span><span class="sxs-lookup"><span data-stu-id="5358e-157">In hello Ambari UI, complete hello following steps toorestart hello HMaster service on all ZooKeeper nodes:</span></span>

    1. <span data-ttu-id="5358e-158">V hello **Souhrn** části hbase, přejděte příliš**HBase** > **Active HBase hlavní**.</span><span class="sxs-lookup"><span data-stu-id="5358e-158">In hello **Summary** section of HBase, go too**HBase** > **Active HBase Master**.</span></span> 
    2. <span data-ttu-id="5358e-159">V hello **součásti** část, restartujte službu HBase hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-159">In hello **Components** section, restart hello HBase Master service.</span></span>
    3. <span data-ttu-id="5358e-160">Tento postup opakujte pro všechny zbývající **pohotovostní HBase hlavní** služby.</span><span class="sxs-lookup"><span data-stu-id="5358e-160">Repeat these steps for all remaining **Standby HBase Master** services.</span></span> 

<span data-ttu-id="5358e-161">Může trvat až minut toofive hello HBase hlavní služby toostabilize a dokončit proces obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-161">It can take up toofive minutes for hello HBase Master service toostabilize and finish hello recovery process.</span></span> <span data-ttu-id="5358e-162">Po několika minutách opakujte hello sqlline.py příkazy tooconfirm této hello systému. Tabulka katalogu je zapnutý, a to může být dotazován.</span><span class="sxs-lookup"><span data-stu-id="5358e-162">After a few minutes, repeat hello sqlline.py commands tooconfirm that hello SYSTEM.CATALOG table is up, and that it can be queried.</span></span> 

<span data-ttu-id="5358e-163">Když hello systému. Tabulka katalogu je back toonormal, hello připojení problém tooPhoenix by měla být vyřešen automaticky.</span><span class="sxs-lookup"><span data-stu-id="5358e-163">When hello SYSTEM.CATALOG table is back toonormal, hello connectivity issue tooPhoenix should be automatically resolved.</span></span>


## <a name="what-causes-a-master-server-toofail-toostart"></a><span data-ttu-id="5358e-164">Co způsobí, že hlavní server toofail toostart</span><span class="sxs-lookup"><span data-stu-id="5358e-164">What causes a master server toofail toostart</span></span>

### <a name="error"></a><span data-ttu-id="5358e-165">Chyba</span><span class="sxs-lookup"><span data-stu-id="5358e-165">Error</span></span> 

<span data-ttu-id="5358e-166">Dojde k selhání atomic přejmenování.</span><span class="sxs-lookup"><span data-stu-id="5358e-166">An atomic renaming failure occurs.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="5358e-167">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="5358e-167">Detailed description</span></span>

<span data-ttu-id="5358e-168">Během procesu spuštění hello dokončení HMaster mnoho kroků inicializace.</span><span class="sxs-lookup"><span data-stu-id="5358e-168">During hello startup process, HMaster completes many initialization steps.</span></span> <span data-ttu-id="5358e-169">Patří mezi ně přesouvání dat od začátku hello (TMP) složka toohello data.</span><span class="sxs-lookup"><span data-stu-id="5358e-169">These include moving data from hello scratch (.tmp) folder toohello data folder.</span></span> <span data-ttu-id="5358e-170">HMaster hello předběžné protokoly (WALs) složky toosee také zabývá, pokud jsou všechny servery reagovat oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5358e-170">HMaster also looks at hello write-ahead logs (WALs) folder toosee if there are any unresponsive region servers, and so on.</span></span> 

<span data-ttu-id="5358e-171">Během spouštění HMaster nemá základní `list` příkazu u těchto složek.</span><span class="sxs-lookup"><span data-stu-id="5358e-171">During startup, HMaster does a basic `list` command on these folders.</span></span> <span data-ttu-id="5358e-172">Pokud kdykoli, uvidí HMaster neočekávaný soubor v žádném z těchto složek, vyvolá výjimku a nespustí.</span><span class="sxs-lookup"><span data-stu-id="5358e-172">If at any time, HMaster sees an unexpected file in any of these folders, it throws an exception and doesn't start.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="5358e-173">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="5358e-173">Probable cause</span></span>

<span data-ttu-id="5358e-174">V protokolech serveru hello oblasti opakujte časová osa hello tooidentify hello vytvoření souboru a zjistit, zda se, že proces havárií kolem hello čas hello soubor byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5358e-174">In hello region server logs, try tooidentify hello timeline of hello file creation, and then see if there was a process crash around hello time hello file was created.</span></span> <span data-ttu-id="5358e-175">(Obraťte se na podporu tooassist HBase je to.) Tento pomůže nám poskytují robustnější mechanismy, takže se můžete vyhnout stiskne to chyb a ujistěte se proces řádné vypnutí systému.</span><span class="sxs-lookup"><span data-stu-id="5358e-175">(Contact HBase support tooassist you in doing this.) This helps us provide more robust mechanisms, so that you can avoid hitting this bug, and ensure graceful process shutdowns.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="5358e-176">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-176">Resolution steps</span></span>

<span data-ttu-id="5358e-177">Zkontrolujte hello zásobník volání a zkuste to toodetermine složku, ke které může být příčinou problému hello (například se může být hello WALs nebo hello TMP složky).</span><span class="sxs-lookup"><span data-stu-id="5358e-177">Check hello call stack and try toodetermine which folder might be causing hello problem (for instance, it might be hello WALs folder or hello .tmp folder).</span></span> <span data-ttu-id="5358e-178">V Průzkumníku cloudu, nebo pomocí příkazů HDFS, opakujte toolocate hello problém souboru.</span><span class="sxs-lookup"><span data-stu-id="5358e-178">Then, in Cloud Explorer or by using HDFS commands, try toolocate hello problem file.</span></span> <span data-ttu-id="5358e-179">Obvykle se jedná \*-renamePending.json souboru.</span><span class="sxs-lookup"><span data-stu-id="5358e-179">Usually, this is a \*-renamePending.json file.</span></span> <span data-ttu-id="5358e-180">(hello \*-renamePending.json soubor je soubor deníku, který byl použit tooimplement hello atomické přejmenování operace v ovladači WASB hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-180">(hello \*-renamePending.json file is a journal file that's used tooimplement hello atomic rename operation in hello WASB driver.</span></span> <span data-ttu-id="5358e-181">Náležitý toobugs v této implementaci těchto souborů může být zbyly poté, co mimoprocesových atd.) Vynutit odstranění tohoto souboru v Průzkumníku cloudu nebo pomocí příkazů HDFS.</span><span class="sxs-lookup"><span data-stu-id="5358e-181">Due toobugs in this implementation, these files can be left over after process crashes, and so on.) Force-delete this file either in Cloud Explorer or by using HDFS commands.</span></span> 

<span data-ttu-id="5358e-182">V některých případech mohou existovat i dočasný soubor s názvem něco podobného jako *$$$. $$$* v tomto umístění.</span><span class="sxs-lookup"><span data-stu-id="5358e-182">Sometimes, there might also be a temporary file named something like *$$$.$$$* at this location.</span></span> <span data-ttu-id="5358e-183">Máte toouse HDFS `ls` příkaz toosee tento soubor, nelze zjistit hello soubor v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="5358e-183">You have toouse HDFS `ls` command toosee this file; you cannot see hello file in Cloud Explorer.</span></span> <span data-ttu-id="5358e-184">toodelete tento soubor, použijte hello HDFS příkaz `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span><span class="sxs-lookup"><span data-stu-id="5358e-184">toodelete this file, use hello HDFS command `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span></span>  

<span data-ttu-id="5358e-185">Po spuštění těchto příkazů, HMaster by měl spustit okamžitě.</span><span class="sxs-lookup"><span data-stu-id="5358e-185">After you've run these commands, HMaster should start immediately.</span></span> 

### <a name="error"></a><span data-ttu-id="5358e-186">Chyba</span><span class="sxs-lookup"><span data-stu-id="5358e-186">Error</span></span>

<span data-ttu-id="5358e-187">Žádná adresa serveru, je uvedena ve *hbase: meta* pro oblast xxx.</span><span class="sxs-lookup"><span data-stu-id="5358e-187">No server address is listed in *hbase: meta* for region xxx.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="5358e-188">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="5358e-188">Detailed description</span></span>

<span data-ttu-id="5358e-189">Může se zobrazit zpráva na váš cluster Linux, která určuje, že hello *hbase: meta* tabulka není online.</span><span class="sxs-lookup"><span data-stu-id="5358e-189">You might see a message on your Linux cluster that indicates that hello *hbase: meta* table is not online.</span></span> <span data-ttu-id="5358e-190">Spuštění `hbck` může zobrazit zprávu, že "hbase: meta tabulky replicaId 0 nebyl nalezen v libovolné oblasti."</span><span class="sxs-lookup"><span data-stu-id="5358e-190">Running `hbck` might report that "hbase: meta table replicaId 0 is not found on any region."</span></span> <span data-ttu-id="5358e-191">Hello problém může být, že HMaster nebylo možné inicializovat po restartování HBase.</span><span class="sxs-lookup"><span data-stu-id="5358e-191">hello problem might be that HMaster could not initialize after you restarted HBase.</span></span> <span data-ttu-id="5358e-192">V protokolech HMaster hello, zpráva se může zobrazit hello: "žádná adresa serveru uvedené v hbase: meta pro oblast hbase: zálohování \<název oblasti\>".</span><span class="sxs-lookup"><span data-stu-id="5358e-192">In hello HMaster logs, you might see hello message: "No server address listed in hbase: meta for region hbase: backup \<region name\>".</span></span>  

### <a name="resolution-steps"></a><span data-ttu-id="5358e-193">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-193">Resolution steps</span></span>

1. <span data-ttu-id="5358e-194">V hello prostředí HBase zadejte následující příkazy (skutečné hodnoty pro změnu podle vhodnosti) hello:</span><span class="sxs-lookup"><span data-stu-id="5358e-194">In hello HBase shell, enter hello following commands (change actual values as applicable):</span></span>  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. <span data-ttu-id="5358e-195">Odstranit hello *hbase: obor názvů* položku.</span><span class="sxs-lookup"><span data-stu-id="5358e-195">Delete hello *hbase: namespace* entry.</span></span> <span data-ttu-id="5358e-196">Tato položka může být hello stejné chybě, která je hlášena, když hello *hbase: obor názvů* tabulka je skenován.</span><span class="sxs-lookup"><span data-stu-id="5358e-196">This entry might be hello same error that's being reported when hello *hbase: namespace* table is scanned.</span></span>

3. <span data-ttu-id="5358e-197">toobring až HBase v běžícím stavu, v hello uživatelského rozhraní Ambari, restartujte službu Active HMaster hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-197">toobring up HBase in a running state, in hello Ambari UI, restart hello Active HMaster service.</span></span>  

4. <span data-ttu-id="5358e-198">V hello prostředí HBase, toobring až všechny tabulky v režimu offline, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5358e-198">In hello HBase shell, toobring up all offline tables, run hello following command:</span></span>

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a><span data-ttu-id="5358e-199">Další čtení</span><span class="sxs-lookup"><span data-stu-id="5358e-199">Additional reading</span></span>

[<span data-ttu-id="5358e-200">Tabulky HBase nelze tooprocess hello</span><span class="sxs-lookup"><span data-stu-id="5358e-200">Unable tooprocess hello HBase table</span></span>](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a><span data-ttu-id="5358e-201">Chyba</span><span class="sxs-lookup"><span data-stu-id="5358e-201">Error</span></span>

<span data-ttu-id="5358e-202">HMaster časového limitu s podobnou too"java.io.IOException závažné výjimce: Čekání na obor názvů tabulky toobe přiřazené 300000ms Timedout."</span><span class="sxs-lookup"><span data-stu-id="5358e-202">HMaster times out with a fatal exception similar too"java.io.IOException: Timedout 300000ms waiting for namespace table toobe assigned."</span></span>

### <a name="detailed-description"></a><span data-ttu-id="5358e-203">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="5358e-203">Detailed description</span></span>

<span data-ttu-id="5358e-204">Tomuto problému může dojít, pokud máte mnoho tabulek a oblasti, které nebyly byly vyprázdněny, při restartování vaší HMaster služeb.</span><span class="sxs-lookup"><span data-stu-id="5358e-204">You might experience this issue if you have many tables and regions that have not been flushed when you restart your HMaster services.</span></span> <span data-ttu-id="5358e-205">Restartování může selhat a uvidíte hello předcházející chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="5358e-205">Restart might fail, and you'll see hello preceding error message.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="5358e-206">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="5358e-206">Probable cause</span></span>

<span data-ttu-id="5358e-207">Jde o známý problém s hello HMaster služby.</span><span class="sxs-lookup"><span data-stu-id="5358e-207">This is a known issue with hello HMaster service.</span></span> <span data-ttu-id="5358e-208">Spuštění úlohy obecné clusteru může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="5358e-208">General cluster startup tasks can take a long time.</span></span> <span data-ttu-id="5358e-209">HMaster, protože tabulka obor názvů hello ještě není přiřazená vypne.</span><span class="sxs-lookup"><span data-stu-id="5358e-209">HMaster shuts down because hello namespace table isn’t yet assigned.</span></span> <span data-ttu-id="5358e-210">K tomu dochází jenom v situacích, ve kterém velké množství dat, unflushed existuje a není dostatečná vypršení časového limitu pěti minut.</span><span class="sxs-lookup"><span data-stu-id="5358e-210">This occurs only in scenarios in which large amount of unflushed data exists, and a timeout of five minutes is not sufficient.</span></span>
  
### <a name="resolution-steps"></a><span data-ttu-id="5358e-211">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-211">Resolution steps</span></span>

1. <span data-ttu-id="5358e-212">Hello uživatelského rozhraní Ambari, přejděte v příliš**HBase** > **konfigurací**.</span><span class="sxs-lookup"><span data-stu-id="5358e-212">In hello Ambari UI, go too**HBase** > **Configs**.</span></span> <span data-ttu-id="5358e-213">V souboru hello vlastní hbase-site.xml přidejte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="5358e-213">In hello custom hbase-site.xml file, add hello following setting:</span></span> 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. <span data-ttu-id="5358e-214">Restartujte služby hello požadované (HMaster a případně dalších HBase služby).</span><span class="sxs-lookup"><span data-stu-id="5358e-214">Restart hello required services (HMaster, and possibly other HBase services).</span></span>  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a><span data-ttu-id="5358e-215">Co způsobí restartování selhání na serveru oblast</span><span class="sxs-lookup"><span data-stu-id="5358e-215">What causes a restart failure on a region server</span></span>

### <a name="issue"></a><span data-ttu-id="5358e-216">Problém</span><span class="sxs-lookup"><span data-stu-id="5358e-216">Issue</span></span>

<span data-ttu-id="5358e-217">Selhání restartování v oblasti serveru může být zabránit následující osvědčené postupy.</span><span class="sxs-lookup"><span data-stu-id="5358e-217">A restart failure on a region server might be prevented by following best practices.</span></span> <span data-ttu-id="5358e-218">Doporučujeme, abyste pozastavit aktivity vytížen, při plánování toorestart HBase oblast servery.</span><span class="sxs-lookup"><span data-stu-id="5358e-218">We recommend that you pause heavy workload activity when you are planning toorestart HBase region servers.</span></span> <span data-ttu-id="5358e-219">Pokud aplikace stále tooconnect se servery pro oblast, když probíhá vypnutí, bude operace restartování serveru oblast hello pomalejší o několik minut.</span><span class="sxs-lookup"><span data-stu-id="5358e-219">If an application continues tooconnect with region servers when shutdown is in progress, hello region server restart operation will be slower by several minutes.</span></span> <span data-ttu-id="5358e-220">Je také vhodné toofirst, který je vyprázdnění že všech hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="5358e-220">Also, it's a good idea toofirst flush all hello tables.</span></span> <span data-ttu-id="5358e-221">Pro odkaz na postupy tooflush tabulky, viz [HDInsight HBase: jak tooimprove hello HBase cluster restartovat čas vyprázdnění tabulky](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span><span class="sxs-lookup"><span data-stu-id="5358e-221">For a reference on how tooflush tables, see [HDInsight HBase: How tooimprove hello HBase cluster restart time by flushing tables](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span></span>

<span data-ttu-id="5358e-222">Pokud spustíte hello operace restartování serverů oblast HBase z hello uživatelského rozhraní Ambari, zobrazí okamžitě hello oblast servery byl vypnut, že nemáte hned restartovat.</span><span class="sxs-lookup"><span data-stu-id="5358e-222">If you initiate hello restart operation on HBase region servers from hello Ambari UI, you immediately see that hello region servers went down, but they don't restart right away.</span></span> 

<span data-ttu-id="5358e-223">Tady je se přitom děje pozadí hello:</span><span class="sxs-lookup"><span data-stu-id="5358e-223">Here's what's happening behind hello scenes:</span></span> 

1. <span data-ttu-id="5358e-224">Hello Ambari agent odesílá serveru oblast toohello žádost o zastavení.</span><span class="sxs-lookup"><span data-stu-id="5358e-224">hello Ambari agent sends a stop request toohello region server.</span></span>
2. <span data-ttu-id="5358e-225">Hello Ambari počká 30 sekund pro hello oblast serveru tooshut dolů řádně.</span><span class="sxs-lookup"><span data-stu-id="5358e-225">hello Ambari agent waits for 30 seconds for hello region server tooshut down gracefully.</span></span> 
3. <span data-ttu-id="5358e-226">Pokud vaše aplikace pokračuje tooconnect s hello oblast serverem, hello server nebude vypnout okamžitě.</span><span class="sxs-lookup"><span data-stu-id="5358e-226">If your application continues tooconnect with hello region server, hello server won't shut down immediately.</span></span> <span data-ttu-id="5358e-227">předtím, než dojde k vypnutí platnost vyprší časový limit 30 sekund Hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-227">hello 30-second timeout expires before shutdown occurs.</span></span> 
4. <span data-ttu-id="5358e-228">Po 30 sekund, hello Ambari agent odesílá force-kill (`kill -9`) příkaz toohello oblast serveru.</span><span class="sxs-lookup"><span data-stu-id="5358e-228">After 30 seconds, hello Ambari agent sends a force-kill (`kill -9`) command toohello region server.</span></span> <span data-ttu-id="5358e-229">Můžete to vidět v protokolu agenta ambari hello (v hello /var nebo protokolu nebo adresář hello příslušných pracovního uzlu):</span><span class="sxs-lookup"><span data-stu-id="5358e-229">You can see this in hello ambari-agent log (in hello /var/log/ directory of hello respective worker node):</span></span>

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
<span data-ttu-id="5358e-230">Z důvodu vypnutí náhlému hello nemusí být vydání hello portu přidružené k procesu hello, i když je zastavena proces serveru oblast hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-230">Because of hello abrupt shutdown, hello port associated with hello process might not be released, even though hello region server process is stopped.</span></span> <span data-ttu-id="5358e-231">Tato situace může vést tooan AddressBindException během spouštění serveru hello oblast, jak je znázorněno v následujícím protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="5358e-231">This situation can lead tooan AddressBindException when hello region server is starting, as shown in hello following logs.</span></span> <span data-ttu-id="5358e-232">Můžete to ověřit v oblasti server.log hello v adresáři /var/log/hbase hello na hello pracovní uzly, kde server hello oblast selže toostart.</span><span class="sxs-lookup"><span data-stu-id="5358e-232">You can verify this in hello region-server.log in hello /var/log/hbase directory on hello worker nodes where hello region server fails toostart.</span></span> 

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
        
   Caused by: java.net.BindException: Problem binding too/10.2.0.4:16020 : Address already in use
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

### <a name="resolution-steps"></a><span data-ttu-id="5358e-233">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="5358e-233">Resolution steps</span></span>

1. <span data-ttu-id="5358e-234">Před spuštěním restartování, zkuste tooreduce hello zatížení hello HBase oblast serverů.</span><span class="sxs-lookup"><span data-stu-id="5358e-234">Try tooreduce hello load on hello HBase region servers before you initiate a restart.</span></span> 
2. <span data-ttu-id="5358e-235">Můžete taky (Pokud kroku 1 vám nepomohly), zkuste toomanually restartování oblasti, které servery na hello uzlů pracovního procesu pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5358e-235">Alternatively (if step 1 doesn't help), try toomanually restart region servers on hello worker nodes by using hello following commands:</span></span>

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

