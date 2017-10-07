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
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a>Řešení potíží s HBase pomocí Azure HDInsight

Další informace o hello nejčastější problémy a jejich řešení při práci s Apache HBase datové části v Apache Ambari.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Jak spouštět sestavy příkaz hbck s několika nepřiřazené oblastí

Běžné chybová zpráva, že může dojít, když spustíte hello `hbase hbck` příkaz je "více oblastí se nepřiřazené nebo díry v řetězu hello oblastí."

V hello HBase hlavního uživatelského rozhraní se zobrazí hello počet oblastí, které jsou nevyvážené napříč všemi servery oblast. Potom můžete spustit `hbase hbck` příkaz toosee díry v řetězu oblast hello.

Díry může být způsobeno hello offline oblastech, tak přiřazení hello oprava nejdřív. 

toobring hello nepřiřazené oblasti back tooa normálním stavu, dokončete hello následující kroky:

1. Přihlaste se toohello clusteru HDInsight HBase pomocí protokolu SSH.
2. tooconnect s hello ZooKeeper prostředí, spusťte hello `hbase zkcli` příkaz.
3. Spustit hello `rmr /hbase/regions-in-transition` příkaz nebo hello `rmr /hbase-unsecure/regions-in-transition` příkaz.
4. tooexit z hello `hbase zkcli` prostředí shell, použijte hello `exit` příkaz.
5. Otevřete hello Apache Ambari uživatelského rozhraní a potom restartujte službu Active HBase hlavní hello.
6. Spustit hello `hbase hbck` příkaz znovu (bez jakékoli možnosti). Zkontrolujte výstup hello tooensure tento příkaz, které jsou přiřazeny všechny oblasti.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Jak se při použití příkazů hbck oblasti přiřazení opravte problémy vypršení časového limitu

### <a name="issue"></a>Problém

Možnou příčinou problémů vypršení časového limitu při použití hello `hbck` příkaz může být, že několik oblasti jsou ve hello "v přechodném stavu" stavu po dlouhou dobu. Najdete v těchto oblastech jako offline v hello HBase hlavního uživatelského rozhraní. Vzhledem k tomu, že vysoký počet oblastí se pokoušíte tootransition, hlavní HBase může časový limit a být nelze toobring těmito oblastmi zpět do režimu online.

### <a name="resolution-steps"></a>Kroky řešení

1. Přihlaste se toohello clusteru HDInsight HBase pomocí protokolu SSH.
2. tooconnect s hello ZooKeeper prostředí, spusťte hello `hbase zkcli` příkaz.
3. Spustit hello `rmr /hbase/regions-in-transition` nebo hello `rmr /hbase-unsecure/regions-in-transition` příkaz.
4. tooexit hello `hbase zkcli` prostředí shell, použijte hello `exit` příkaz.
5. V hello uživatelského rozhraní Ambari restartujte službu Active HBase hlavní hello.
6. Spustit hello `hbase hbck -fixAssignments` příkaz znovu.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Jak I vynucení zakázání HDFS nouzovém režimu v clusteru

### <a name="issue"></a>Problém

Hello místní Hadoop Distributed File System (HDFS) se zasekla v nouzovém režimu v clusteru HDInsight hello.

### <a name="detailed-description"></a>Podrobný popis

Tato chyba může být způsobena chybou, když spustíte následující příkaz HDFS hello:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

Hello chyba, kterou můžete setkat při pokusu o toorun hello příkaz vypadá takto:

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

### <a name="probable-cause"></a>Pravděpodobná příčina

Hello clusteru HDInsight byl škálovat dolů tooa jen několik uzlů. Hello počet uzlů je menší než nebo zavřete toohello HDFS replikace faktor.

### <a name="resolution-steps"></a>Kroky řešení 

1. Získáte stav hello hello HDFS na hello HDInsight clusteru spuštěním hello následující příkazy:

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
2. Také můžete zkontrolovat integritu hello hello HDFS na hello HDInsight clusteru pomocí hello následující příkazy:

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

3. Pokud zjistíte, že neexistují žádné chybějí, jsou poškozené nebo under-replikované bloky, nebo že tyto bloky můžete ignorovat, spusťte následující příkaz tootake hello název uzlu mimo nouzový režim hello:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Jak opravit JDBC nebo SQLLine připojením k problémům s Apache Phoenix

### <a name="resolution-steps"></a>Kroky řešení

tooconnect s Phoenix, je nutné zadat IP adresu hello aktivní uzel ZooKeeper. Ujistěte se, že hello ZooKeeper sqlline.py toowhich služba se pokouší tooconnect je spuštěná.
1. Přihlaste se toohello clusteru HDInsight pomocí protokolu SSH.
2. Zadejte hello následující příkaz:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Můžete získat IP adresu hello aktivní uzel ZooKeeper hello hello uživatelského rozhraní Ambari. Přejděte příliš**HBase** > **rychlé odkazy** > **ZK\* (aktivní)** > **Zookeeper informace**. 

3. Pokud hello sqlline.py připojí tooPhoenix a nemá vypršení časového limitu, hello spusťte následující příkaz toovalidate hello dostupnosti a stavu Phoenix:

   ```apache
           !tables
           !quit
   ```      
4. Pokud tento příkaz lze použít, neexistuje žádný problém. Hello IP adresa zadaná uživatelem hello můžou být nesprávné. Nicméně pokud hello příkaz pozastaví po delší dobu a potom zobrazí následující chyba hello, pokračujte toostep 5.

   ```apache
           Error while connecting toosqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting toojdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Spuštěním následujících příkazů z hlavního uzlu (hn0) hello toodiagnose hello podmínku hello Phoenix systému hello. Tabulka katalogu:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   příkaz Hello by měla vrátit následující chybě podobné toohello: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. V hello uživatelského rozhraní Ambari proveďte následující kroky toorestart hello HMaster služby na všechny uzly ZooKeeper hello:

    1. V hello **Souhrn** části hbase, přejděte příliš**HBase** > **Active HBase hlavní**. 
    2. V hello **součásti** část, restartujte službu HBase hlavní hello.
    3. Tento postup opakujte pro všechny zbývající **pohotovostní HBase hlavní** služby. 

Může trvat až minut toofive hello HBase hlavní služby toostabilize a dokončit proces obnovení hello. Po několika minutách opakujte hello sqlline.py příkazy tooconfirm této hello systému. Tabulka katalogu je zapnutý, a to může být dotazován. 

Když hello systému. Tabulka katalogu je back toonormal, hello připojení problém tooPhoenix by měla být vyřešen automaticky.


## <a name="what-causes-a-master-server-toofail-toostart"></a>Co způsobí, že hlavní server toofail toostart

### <a name="error"></a>Chyba 

Dojde k selhání atomic přejmenování.

### <a name="detailed-description"></a>Podrobný popis

Během procesu spuštění hello dokončení HMaster mnoho kroků inicializace. Patří mezi ně přesouvání dat od začátku hello (TMP) složka toohello data. HMaster hello předběžné protokoly (WALs) složky toosee také zabývá, pokud jsou všechny servery reagovat oblasti a tak dále. 

Během spouštění HMaster nemá základní `list` příkazu u těchto složek. Pokud kdykoli, uvidí HMaster neočekávaný soubor v žádném z těchto složek, vyvolá výjimku a nespustí.  

### <a name="probable-cause"></a>Pravděpodobná příčina

V protokolech serveru hello oblasti opakujte časová osa hello tooidentify hello vytvoření souboru a zjistit, zda se, že proces havárií kolem hello čas hello soubor byl vytvořen. (Obraťte se na podporu tooassist HBase je to.) Tento pomůže nám poskytují robustnější mechanismy, takže se můžete vyhnout stiskne to chyb a ujistěte se proces řádné vypnutí systému.

### <a name="resolution-steps"></a>Kroky řešení

Zkontrolujte hello zásobník volání a zkuste to toodetermine složku, ke které může být příčinou problému hello (například se může být hello WALs nebo hello TMP složky). V Průzkumníku cloudu, nebo pomocí příkazů HDFS, opakujte toolocate hello problém souboru. Obvykle se jedná \*-renamePending.json souboru. (hello \*-renamePending.json soubor je soubor deníku, který byl použit tooimplement hello atomické přejmenování operace v ovladači WASB hello. Náležitý toobugs v této implementaci těchto souborů může být zbyly poté, co mimoprocesových atd.) Vynutit odstranění tohoto souboru v Průzkumníku cloudu nebo pomocí příkazů HDFS. 

V některých případech mohou existovat i dočasný soubor s názvem něco podobného jako *$$$. $$$* v tomto umístění. Máte toouse HDFS `ls` příkaz toosee tento soubor, nelze zjistit hello soubor v Průzkumníku cloudu. toodelete tento soubor, použijte hello HDFS příkaz `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Po spuštění těchto příkazů, HMaster by měl spustit okamžitě. 

### <a name="error"></a>Chyba

Žádná adresa serveru, je uvedena ve *hbase: meta* pro oblast xxx.

### <a name="detailed-description"></a>Podrobný popis

Může se zobrazit zpráva na váš cluster Linux, která určuje, že hello *hbase: meta* tabulka není online. Spuštění `hbck` může zobrazit zprávu, že "hbase: meta tabulky replicaId 0 nebyl nalezen v libovolné oblasti." Hello problém může být, že HMaster nebylo možné inicializovat po restartování HBase. V protokolech HMaster hello, zpráva se může zobrazit hello: "žádná adresa serveru uvedené v hbase: meta pro oblast hbase: zálohování \<název oblasti\>".  

### <a name="resolution-steps"></a>Kroky řešení

1. V hello prostředí HBase zadejte následující příkazy (skutečné hodnoty pro změnu podle vhodnosti) hello:  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Odstranit hello *hbase: obor názvů* položku. Tato položka může být hello stejné chybě, která je hlášena, když hello *hbase: obor názvů* tabulka je skenován.

3. toobring až HBase v běžícím stavu, v hello uživatelského rozhraní Ambari, restartujte službu Active HMaster hello.  

4. V hello prostředí HBase, toobring až všechny tabulky v režimu offline, spusťte následující příkaz hello:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Další čtení

[Tabulky HBase nelze tooprocess hello](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Chyba

HMaster časového limitu s podobnou too"java.io.IOException závažné výjimce: Čekání na obor názvů tabulky toobe přiřazené 300000ms Timedout."

### <a name="detailed-description"></a>Podrobný popis

Tomuto problému může dojít, pokud máte mnoho tabulek a oblasti, které nebyly byly vyprázdněny, při restartování vaší HMaster služeb. Restartování může selhat a uvidíte hello předcházející chybová zpráva.  

### <a name="probable-cause"></a>Pravděpodobná příčina

Jde o známý problém s hello HMaster služby. Spuštění úlohy obecné clusteru může trvat dlouhou dobu. HMaster, protože tabulka obor názvů hello ještě není přiřazená vypne. K tomu dochází jenom v situacích, ve kterém velké množství dat, unflushed existuje a není dostatečná vypršení časového limitu pěti minut.
  
### <a name="resolution-steps"></a>Kroky řešení

1. Hello uživatelského rozhraní Ambari, přejděte v příliš**HBase** > **konfigurací**. V souboru hello vlastní hbase-site.xml přidejte hello následující nastavení: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Restartujte služby hello požadované (HMaster a případně dalších HBase služby).  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Co způsobí restartování selhání na serveru oblast

### <a name="issue"></a>Problém

Selhání restartování v oblasti serveru může být zabránit následující osvědčené postupy. Doporučujeme, abyste pozastavit aktivity vytížen, při plánování toorestart HBase oblast servery. Pokud aplikace stále tooconnect se servery pro oblast, když probíhá vypnutí, bude operace restartování serveru oblast hello pomalejší o několik minut. Je také vhodné toofirst, který je vyprázdnění že všech hello tabulky. Pro odkaz na postupy tooflush tabulky, viz [HDInsight HBase: jak tooimprove hello HBase cluster restartovat čas vyprázdnění tabulky](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

Pokud spustíte hello operace restartování serverů oblast HBase z hello uživatelského rozhraní Ambari, zobrazí okamžitě hello oblast servery byl vypnut, že nemáte hned restartovat. 

Tady je se přitom děje pozadí hello: 

1. Hello Ambari agent odesílá serveru oblast toohello žádost o zastavení.
2. Hello Ambari počká 30 sekund pro hello oblast serveru tooshut dolů řádně. 
3. Pokud vaše aplikace pokračuje tooconnect s hello oblast serverem, hello server nebude vypnout okamžitě. předtím, než dojde k vypnutí platnost vyprší časový limit 30 sekund Hello. 
4. Po 30 sekund, hello Ambari agent odesílá force-kill (`kill -9`) příkaz toohello oblast serveru. Můžete to vidět v protokolu agenta ambari hello (v hello /var nebo protokolu nebo adresář hello příslušných pracovního uzlu):

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
Z důvodu vypnutí náhlému hello nemusí být vydání hello portu přidružené k procesu hello, i když je zastavena proces serveru oblast hello. Tato situace může vést tooan AddressBindException během spouštění serveru hello oblast, jak je znázorněno v následujícím protokoly hello. Můžete to ověřit v oblasti server.log hello v adresáři /var/log/hbase hello na hello pracovní uzly, kde server hello oblast selže toostart. 

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

### <a name="resolution-steps"></a>Kroky řešení

1. Před spuštěním restartování, zkuste tooreduce hello zatížení hello HBase oblast serverů. 
2. Můžete taky (Pokud kroku 1 vám nepomohly), zkuste toomanually restartování oblasti, které servery na hello uzlů pracovního procesu pomocí hello následující příkazy:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

