---
title: "pracovní postupy aaaUse Hadoop Oozie v HDInsight se systémem Linux | Microsoft Docs"
description: "Použijte Hadoop Oozie v HDInsight se systémem Linux. Zjistěte, jak toodefine pracovním postupu Oozie a odešlete úlohu Oozie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: cb5682837543312621e3424b7a9341b5d2a00bf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-on-linux-based-hdinsight"></a>Pomocí Oozie Hadoop toodefine a spuštění workflowu v HDInsight se systémem Linux

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Zjistěte, jak toouse Apache Oozie s Hadoop v HDInsight. Apache Oozie je pracovní postup nebo koordinaci systém, který spravuje úloh Hadoop. Oozie je integrována hello zásobníku Hadoop a podporuje hello následující úlohy:

* Apache MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

Oozie může být také použít tooschedule úlohy, které jsou specifické tooa systém, jako jsou programy v jazyce Java nebo skripty prostředí

> [!NOTE]
> Další možností pro definování pracovních postupů v prostředí HDInsight je Azure Data Factory. toolearn Další informace o Azure Data Factory najdete v části [použijte Pig a Hive pomocí služby Data Factory][azure-data-factory-pig-hive].

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

## <a name="prerequisites"></a>Požadavky

* **Cluster služby HDInsight**: najdete v části [Začínáme s prostředím HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)

  > [!IMPORTANT]
  > Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="example-workflow"></a>Příklad pracovního postupu

Hello pracovního postupu v tomto dokumentu obsahuje dvě akce. Akce jsou definice pro úlohy, jako je například spuštění Hive, Sqoop, MapReduce nebo jiný proces:

![Diagram pracovního postupu][img-workflow-diagram]

1. Akce Hive spouští skript HiveQL tooextract záznamů z hello **hivesampletable** součástí HDInsight. Každý řádek dat popisuje návštěvu z určité mobilní zařízení. Formát záznamu Hello se zobrazí podobné toohello následující text:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Hello skriptu Hive v tomto dokumentu počítá hello celkový počet návštěv pro každou platformu (třeba na Android nebo iPhone) a ukládá hello počty tooa novou tabulku Hive.

    Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].

2. Akce Sqoop exportuje obsah hello hello nové Hive tooa tabulku v databázi Azure SQL. Další informace o Sqoop najdete v tématu [Sqoop pomocí Hadoop v prostředí HDInsight][hdinsight-use-sqoop].

> [!NOTE]
> Podporované verze Oozie v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight][hdinsight-versions].

## <a name="create-hello-working-directory"></a>Vytvoření hello pracovní adresář

Oozie očekává prostředky potřebné pro úlohy toobe uložené v hello stejný adresář. Tento příklad používá **wasb: / / / kurzy/useoozie**. Tento adresář a hello datový adresář, který obsahuje novou tabulku Hive hello vytvořené tento pracovní postup, použijte následující příkaz toocreate hello:

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> Hello `-p` parametr způsobí, že všechny adresáře v toobe cesta hello vytvořili. Hello **data** adresář je použité toohold dat používá hello **useooziewf.hql** skriptu.

Taky spusťte následující příkaz, který zajistí, že Oozie může zosobnit váš uživatelský účet při spuštění úlohy Hive a Sqoop hello. Nahraďte **uživatelské jméno** s vaše přihlašovací jméno:

```
sudo adduser USERNAME users
```

> [!NOTE]
> Můžete ignorovat chyby tohoto uživatele hello je již členem hello `users` skupiny.

## <a name="add-a-database-driver"></a>Přidat ovladač databáze

Vzhledem k tomu, že tento pracovní postup používá Sqoop tooexport data tooSQL databáze, je nutné zadat, že kopii ovladač JDBC hello používá tootalk tooSQL databáze. Použití hello následující příkaz toocopy ho toohello pracovní adresář:

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

Pokud pracovní postup používá jiné prostředky, jako je například jar obsahující aplikaci MapReduce, potřebovali byste tooadd také tyto prostředky.

## <a name="define-hello-hive-query"></a>Definování dotazu Hive hello

Pomocí následujících kroků toocreate HiveQL skript, který definuje dotaz, který se používá v pracovním postupu Oozie později v tomto dokumentu hello.

1. Připojte toohello clusteru pomocí protokolu SSH. Hello následujícího příkazu je příklad použití hello `ssh` příkaz. Nahraďte __uživatelské jméno__ hello uživatele SSH pro hello cluster. Nahraďte __CLUSTERNAME__ s názvem hello hello clusteru HDInsight.

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Z hello připojení SSH použijte následující příkaz toocreate soubor hello:

    ```
    nano useooziewf.hql
    ```

3. Jakmile se otevře hello nano editor, použijte následující dotaz jako hello obsah souboru hello hello:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Existují dvě proměnné používané ve skriptu hello:

    * **${hiveTableName}**: obsahuje název hello toobe tabulky hello vytvořen

    * **${hiveDataFolder}**: obsahuje hello umístění toostore hello datové soubory pro tabulku hello

    Soubor definice pracovního postupu Hello (workflow.xml v tomto kurzu) předává tyto hodnoty toothis skript HiveQL v době běhu

4. tooexit hello editor, stiskněte kombinaci kláves Ctrl-X. Po zobrazení výzvy vyberte **Y** toosave hello souboru, potom použijte **Enter** toouse hello **useooziewf.hql** název souboru.

5. Použití hello následující příkazy toocopy **useooziewf.hql** příliš**wasb:///tutorials/useoozie/useooziewf.hql**:

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Tyto příkazy ukládání hello **useooziewf.hql** souboru na hello HDFS kompatibilní úložiště pro hello cluster.

## <a name="define-hello-workflow"></a>Definice pracovního postupu hello

Definice Oozie pracovní postupy jsou zapsány ve hPDL (XML proces Definition Language). Hello použijte následující postup toodefine hello pracovního postupu:

1. Použijte následující příkaz toocreate hello a upravit nový soubor:

    ```
    nano workflow.xml
    ```

2. Jakmile se otevře hello nano editor, zadejte následující XML jako obsah souboru hello hello:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
        </action>
        <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
        </action>
        <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

    Existují dvě akce definované v pracovním postupu hello:

   * **RunHiveScript**: Tato akce je hello spuštění akce a spouští hello **useooziewf.hql** skript podregistru

   * **RunSqoopExport**: Tato akce exportuje hello data vytvořená z tooSQL skriptu Hive hello databáze pomocí Sqoop. Tato akce je spuštěna pouze pokud hello **RunHiveScript** akce je úspěšné.

     pracovní postup Hello má několik položek, jako například `${jobTracker}`. Tyto položky jsou nahrazovány hodnoty, které můžete použít v definici úlohy hello. Vytvoří se definice úlohy Hello později v tomto dokumentu.

     Také Poznámka hello `<archive>sqljdbc4.jar</arcive>` položku v hello Sqoop části. Tato položka dá pokyn, Oozie toomake archivu k dispozici pro Sqoop po spuštění této akce.

3. Použijte Ctrl-X, pak **Y** a **Enter** toosave hello souboru.

4. Použití hello následující příkaz toocopy hello **workflow.xml** souboru příliš**/tutorials/useoozie/workflow.xml**:

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-hello-database"></a>Vytvoření databáze hello

toocreate Azure SQL Database, postupujte podle kroků hello v hello [vytvoření databáze SQL](../sql-database/sql-database-get-started.md) dokumentu. Při vytváření hello databáze, použijte `oozietest` jako název databáze hello. Také si poznamenejte název hello hello databázového serveru.

### <a name="create-hello-table"></a>Vytvoření tabulky hello

> [!NOTE]
> Existuje mnoho způsobů tooconnect tooSQL databáze toocreate tabulku. Následující postup použijte Hello [FreeTDS](http://www.freetds.org/) z clusteru HDInsight hello.


1. Použijte následující příkaz tooinstall FreeTDS na clusteru HDInsight hello hello:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Po instalaci FreeTDS, použijte následující příkaz tooconnect toohello databáze SQL serveru, kterou jste vytvořili dříve hello:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    Zobrazí se výstup podobný toohello následující text:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toooozietest
        1>

3. V hello `1>` výzva, zadejte hello následující řádky:

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    Když hello `GO` příkaz je zadán, jsou vyhodnocovány hello předchozí příkazy. Tyto příkazy vytvořit tabulku s názvem **mobiledata** používané v pracovním postupu hello.

    Použití hello následující tooverify, který hello tabulka byla vytvořena:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    Zobrazí výstup podobný toohello následující text:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. Zadejte `exit` v hello `1>` výzvu tooexit hello tsql nástroj.

## <a name="create-hello-job-definition"></a>Vytvořit definici úlohy hello

definice úlohy Hello popisuje, kde toofind hello workflow.xml. Také popisuje, kde toofind další soubory, které používá pracovní postup hello (například useooziewf.hql.) Také definuje hello hodnoty pro vlastnosti používaných v rámci pracovního postupu hello a související soubory.

1. Použijte následující příkaz tooget hello úplná adresa hello výchozí úložiště hello. Tato adresa se používá v konfiguračním souboru hello za chvíli:

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Tento příkaz vrátí informace podobné toohello následující XML:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > Pokud hello HDInsight cluster používá jako hello výchozí úložiště Azure Storage, hello `<value>` obsah elementu začínat `wasb://`. Pokud je použita Azure Data Lake Store, začne s `adl://`.

    Uložit obsah hello hello `<value>` element, protože se používá v dalších krocích hello.

2. Použijte následující příkaz tooget plně kvalifikovaný název domény clusteru headnode hello hello. Tyto informace se používají pro hello JobTracker adresu pro hello cluster:

    ```
    hostname -f
    ```

    Tento příkaz vrátí informace podobné toohello následující text:

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    Hello port používaný pro hello JobTracker je 8050, takže je hello toouse úplná adresa pro hello JobTracker `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.

3. Použijte následující toocreate hello Oozie úlohy definice konfigurace hello:

    ```
    nano job.xml
    ```

4. Jakmile se otevře hello nano editor, použijte následující XML jako hello obsah souboru hello hello:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>JOBTRACKERADDRESS</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * Nahraďte všechny výskyty  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  s hodnotou hello jste obdrželi dříve pro výchozí úložiště.

     > [!WARNING]
     > Pokud se cesta hello `wasb` cestu, musíte použít úplnou cestu hello. Není Zkraťte ho toojust `wasb:///`.

   * Nahraďte **JOBTRACKERADDRESS** s hello JobTracker/ResourceManager adresu jste obdrželi dříve.
   * Nahraďte **jméno** s vaše přihlašovací jméno pro hello HDInsight cluster.
   * Nahraďte **serverName**, **adminLogin**, a **adminPassword** s hello informace o vaší databázi SQL Azure.

     Většina hello informace v tomto souboru je použité toopopulate hello hodnoty používané v hello workflow.xml nebo ooziewf.hql soubory (např. ${nameNode}.)

     > [!NOTE]
     > Hello **oozie.wf.application.path** položka udává kde toofind hello workflow.xml soubor, který obsahuje pracovní postup hello spuštěné prostřednictvím této úlohy.

5. Použijte Ctrl-X, pak **Y** a **Enter** toosave hello souboru.

## <a name="submit-and-manage-hello-job"></a>Odesílat a spravovat úlohy hello

Hello následující kroky použijte hello Oozie příkaz toosubmit a správa pracovních postupů Oozie v clusteru hello. příkaz Oozie Hello je popisný rozhraní přes hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]
> Při použití příkazu hello Oozie, je nutné použít hello plně kvalifikovaný název domény pro hello HDInsight headnode. Tento plně kvalifikovaný název domény je k dispozici pouze z clusteru hello nebo pokud hello clusteru je na virtuální síť Azure, z jiných počítačů v hello stejné síti.


1. Použijte následující tooobtain hello URL toohello Oozie služby hello:

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Tento příkaz vrátí informace podobné toohello následující XML:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    Hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` část je hello toouse adresu URL s hello Oozie příkaz.

2. Použití hello následující toocreate proměnné prostředí pro adresu URL hello, takže není nutné tootype pro každý příkaz:

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    Nahraďte adresu URL hello hello jeden, který jste dostali dříve.
3. Použijte následující úlohy hello toosubmit hello:

    ```
    oozie job -config job.xml -submit
    ```

    Tento příkaz načte informace o úlohách hello z **job.xml** a odešle ji tooOozie, ale nespustí se nepodporuje.

    Po dokončení příkazu hello by měl vrátit hello ID úlohy hello. Například, `0000005-150622124850154-oozie-oozi-W`. Toto ID je použité toomanage hello úlohy.

4. Zobrazit stav hello hello úlohy pomocí hello následující příkaz:

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > Nahraďte `<JOBID>` s hello ID vrácené v předchozím kroku hello.

    Tento příkaz vrátí informace podobné toohello následující text:

    ```
    Job ID : 0000005-150622124850154-oozie-oozi-W
    ------------------------------------------------------------------------------------------------------------------------------------
    Workflow Name : useooziewf
    App Path      : wasb:///tutorials/useoozie
    Status        : PREP
    Run           : 0
    User          : USERNAME
    Group         : -
    Created       : 2015-06-22 15:06 GMT
    Started       : -
    Last Modified : 2015-06-22 15:06 GMT
    Ended         : -
    CoordAction ID: -
    ------------------------------------------------------------------------------------------------------------------------------------
    ```

    Tato úloha je ve stavu `PREP`. Tento stav indikuje tuto úlohu hello byla vytvořena, ale není spuštěna.

5. Použijte následující příkaz toostart hello úlohy hello:

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > Nahraďte `<JOBID>` s hello ID vrácené dříve.

    Pokud zaškrtnete hello stav po tento příkaz, je v běžícím stavu a pro hello akce v rámci úlohy hello se vrátí informace.

6. Po úspěšném dokončení úkolů hello můžete ověřit, že hello data byla vygenerována a exportovali toohello tabulka databáze SQL pomocí hello následující příkazy:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    V hello `1>` výzva, zadejte hello následující dotaz:

    ```
    SELECT * FROM mobiledata
    GO
    ```

    vrácené informace Hello je podobné toohello následující text:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Další informace o hello Oozie příkaz najdete v tématu [nástroj příkazového řádku Oozie](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API

Hello Oozie REST API vám umožní toobuild vlastní nástroje, které pracují s Oozie. Hello následují HDInsight konkrétní informace o používání hello Oozie REST API:

* **Identifikátor URI**: hello REST API je přístupná z clusteru mimo hello`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Ověřování**: ověření toohello rozhraní API pomocí účet clusteru HTTP hello (správce) a hesla. Například:

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Další informace o používání hello Oozie REST API najdete v tématu [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie webového uživatelského rozhraní

Hello Oozie webového uživatelského rozhraní poskytuje webové pohled na stav hello Oozie úloh na clusteru hello. Hello webového uživatelského rozhraní umožňuje tooview hello následující informace:

* Stav úlohy
* Definice úlohy
* Konfigurace
* Graf hello akcí v úloze hello
* Protokoly pro úlohu hello

Můžete také zobrazit podrobnosti pro akce v rámci úlohy.

tooaccess hello Oozie webového uživatelského rozhraní, použijte hello následující kroky:

1. Vytvoření clusteru HDInsight toohello tunelového propojení SSH. Informace najdete v tématu hello [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.

2. Po vytvoření tunelu, otevřete ve webovém prohlížeči webovému uživatelskému rozhraní Ambari hello. Hello identifikátor URI pro lokalitu Ambari hello je **https://CLUSTERNAME.azurehdinsight.net**. Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight se systémem Linux.

3. Hello levé straně stránky hello, vyberte **Oozie**, pak **rychlé odkazy**a v neposlední řadě **Oozie webového uživatelského rozhraní**.

    ![Obrázek nabídky hello](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. výchozí hodnoty toodisplaying Oozie webového uživatelského rozhraní Hello spuštěné úlohy pracovního postupu. Vyberte všechny úlohy pracovního postupu, toosee **všechny úlohy**.

    ![Zobrazí všechny úlohy](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Vyberte úlohy tooview Další informace o úloze hello.

    ![Informace o úloze](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Z karty hello informace o úloze můžete zobrazit informace o základní úlohy a hello jednotlivé akce v rámci úlohy hello. V horní části hello, které můžete zobrazit pomocí karty hello hello definice úlohy, úlohy konfigurace přístup hello protokol úlohy nebo zobrazení směrované Acyklické grafu (DAG) hello úlohy.

   * **Protokol úlohy**: Vyberte hello **GetLogs** tlačítko tooget všechny protokoly pro úlohu hello, nebo použijte hello **zadejte vyhledávací filtr** pole toofilter protokoly

       ![Protokol úlohy](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **JobDAG**: hello DAG je grafické zobrazení cesty k datům hello prováděné prostřednictvím hello workflowu

       ![Úloha DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Vyberte jednu z akcí hello z hello **informace o úloze** kartě vyvoláte informace pro akce hello. Vyberte například hello **RunHiveScript** akce.

    ![Informace o akci](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Zobrazí podrobnosti o hello akce, například odkaz toohello **adresa URL konzoly**. Tento odkaz může být použité tooview JobTracker informace pro úlohu hello.

## <a name="scheduling-jobs"></a>Plánování úloh

Koordinátor Hello umožňuje toospecify začátek, konec a výskyt frekvence pro úlohy. toodefine plán pro pracovní postup hello, hello použijte následující kroky:

1. Použití hello následující toocreate soubor s názvem **coordinator.xml**:

    ```
    nano coordinator.xml
    ```

    Použijte následující XML jako hello obsah souboru hello hello:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]
    > Hello `${...}` proměnné jsou nahrazena hodnotami v definici úlohy hello za běhu. Hello proměnnými jsou:
    >
    > * `${coordFrequency}`: Doba mezi spuštěním instancí hello úlohy.
    > ** `${coordStart}`: Úloha hello počáteční čas.
    > * `${coordEnd}`: času ukončení úlohy hello.
    > * `${coordTimezone}`: Koordinátor úlohy jsou v pevné časové pásmo s žádné letní čas (obvykle vyjádřený pomocí UTC). Toto časové pásmo se označuje jako hello "Oozie zpracování časové pásmo."
    > * `${wfPath}`: hello workflow.xml toohello cesta.

2. toosave hello soubor, použijte kombinaci kláves Ctrl-X **Y**, a **Enter**.

3. Použijte následující příkaz toocopy hello souboru toohello pracovní adresář pro tuto úlohu hello:

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Použití hello následující toomodify hello **job.xml** souboru:

    ```
    nano job.xml
    ```

    Ujistěte se, hello následující změny:

   * tooinstruct oozie toorun hello coordinator souboru místo hello pracovního postupu, změna `<name>oozie.wf.application.path</name>` příliš`<name>oozie.coord.application.path</name>`.

   * tooset hello `workflowPath` proměnné používané hello coordinator, přidejte následující XML hello:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Nahraďte hello `wasb://mycontainer@mystorageaccount.blob.core.windows` textu s hodnotou hello používá v jiné položky v souboru job.xml hello.

   * spuštění hello toodefine, end a četnost pro koordinátora hello, přidejte následující XML hello:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       Tyto hodnoty nastaveny hello počáteční čas too12: 00 PM na 10 může 2017 hello koncový čas tooMay 12, 2017. Hello interval pro spuštění tuto úlohu denně. frekvence Hello je v minutách, takže 24 hodin x 60 minut = 1 440 minut. Nakonec časové pásmo hello nastavena tooUTC.

5. Použijte Ctrl-X, pak **Y** a **Enter** toosave hello souboru.

6. toorun hello úloha hello použijte následující příkaz:

    ```
    oozie job -config job.xml -run
    ```

    Tento příkaz odešle a spustí úlohu hello.

7. Pokud navštívíte hello Oozie webového uživatelského rozhraní a vyberte hello **koordinátor úlohy** kartě uvidíte informace podobné toohello následující bitové kopie:

    ![Karta úlohy Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Hello **další Materialization** záznam obsahuje hello příštím hello spuštění úloh.

8. Podobně jako toohello starší úlohy pracovního postupu, výběr položek úlohy hello v hello webového uživatelského rozhraní zobrazí informace o úloze hello:

    ![Informace o úloze Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > Tento image se zobrazí pouze úspěšně spustí hello úlohy, ne z individuálních akce v rámci pracovního postupu hello naplánované. toosee, vyberte jednu z hello **akce** položky.

    ![Informace o akci](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Řešení potíží

Hello Oozie uživatelského rozhraní umožňuje tooview Oozie protokoly. Obsahuje taky odkazy tooJobTracker protokoly pro úlohy MapReduce spustí pracovní postup hello. musí být Hello vzor pro řešení potíží:

1. Zobrazit úlohy hello v Oozie webového uživatelského rozhraní.

2. Pokud dojde k chybě nebo pro určité akce se nezdařilo, vyberte hello toosee akce, pokud hello **chybová zpráva** pole poskytuje další informace o selhání hello.

3. Pokud je k dispozici, použijte hello adresu URL z hello akce tooview další podrobnosti (třeba protokoly JobTracker) pro akci hello.

Hello následují konkrétní chyby, které se můžete setkat, a jak tooresolve je.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Nelze inicializovat clusteru

**Příznaky**: hello změny stavu úlohy příliš**POZASTAVENO**. Zobrazí podrobnosti pro úlohu hello hello RunHiveScript stav jako **START_MANUAL**. Výběr akce hello zobrazí hello následující chybová zpráva:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Příčina**: hello WASB adresy použité v hello **job.xml** soubor neobsahuje kontejner úložiště hello nebo název účtu úložiště. musí být ve formátu adresa WASB Hello `wasb://containername@storageaccountname.blob.core.windows.net`.

**Řešení**: Změna adresy WASB hello používá hello úloha.

### <a name="ja002-oozie-is-not-allowed-tooimpersonate-ltuser"></a>JA002: Oozie není povolen tooimpersonate &lt;uživatele >

**Příznaky**: hello změny stavu úlohy příliš**POZASTAVENO**. Zobrazí podrobnosti pro úlohu hello hello RunHiveScript stav jako **START_MANUAL**. Výběr akce hello ukazuje hello následující chybová zpráva:

    JA002: User: oozie is not allowed tooimpersonate <USER>

**Příčina**: nastavení aktuální oprávnění neumožňují Oozie tooimpersonate hello zadaný uživatelský účet.

**Řešení**: Oozie v hello je povolená uživatelé tooimpersonate **uživatelé** skupiny. Použití hello `groups USERNAME` toosee hello skupiny, které hello uživatelský účet je členem skupiny. Pokud není uživatel hello členem hello **uživatelé** skupiny, použijte následující příkaz tooadd hello uživatele toohello skupiny hello:

    sudo adduser USERNAME users

> [!NOTE]
> Může trvat několik minut, než HDInsight rozpozná, že tento uživatel hello přidala toohello skupiny.

### <a name="launcher-error-sqoop"></a>Spouštěč chyby (Sqoop)

**Příznaky**: hello změny stavu úlohy příliš**KILLED**. Zobrazí podrobnosti pro úlohu hello hello RunSqoopExport stav jako **chyba**. Výběr akce hello ukazuje hello následující chybová zpráva:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Příčina**: Sqoop je nelze tooload hello ovladač vyžaduje tooaccess hello databáze.

**Řešení**: při použití Sqoop z úlohu Oozie, je nutné zahrnout hello ovladač databáze s hello jiné prostředky (třeba hello workflow.xml) používá hello úloha. Také odkazovat hello archivu obsahující hello ovladač databáze z hello `<sqoop>...</sqoop>` části hello workflow.xml.

Například pro úlohu hello v tomto dokumentu použijete hello následující kroky:

1. Zkopírujte adresář /tutorials/useoozie hello sqljdbc4.1.jar souboru toohello:

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. Upravit hello workflow.xml tooadd hello následující XML na nový řádek výše `</sqoop>`:

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili jak toodefine pracovním postupu Oozie a jak toorun úlohu Oozie. toolearn Další informace o práci s HDInsight, najdete v části hello následující články:

* [Použijte založené na čase Oozie Coordinator s HDInsight][hdinsight-oozie-coordinator-time]
* [Nahrání dat pro úlohy Hadoop v HDInsight][hdinsight-upload-data]
* [Použití nástroje Sqoop se systémem Hadoop v HDInsight][hdinsight-use-sqoop]
* [Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]
* [Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]
* [Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
