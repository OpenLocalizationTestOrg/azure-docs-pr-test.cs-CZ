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
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a>Povolit výpisů paměti haldy pro služby Hadoop v HDInsight se systémem Linux

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Výpisy haldy obsahovat snímek paměti hello aplikace, včetně hello hodnoty proměnných v hello čas vytvoření výpisu hello. Takže se hodí pro diagnostiku problémů, ke kterým dochází za běhu.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whichServices"></a>Služby

Můžete povolit výpisů paměti haldy pro hello následující služby:

* **hcatalog** -tempelton
* **Hive** -hiveserver2, metaúložiště, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

Můžete také povolit výpisů paměti haldy pro mapu hello a snížit procesy spuštěné prostřednictvím HDInsight.

## <a name="configuration"></a>Principy haldy výpis konfigurace

Výpisy haldy jsou povolené předáním možnosti (někdy označuje jako požádá, nebo parametry) toohello JVM při spuštění služby. Pro většinu služby Hadoop můžete upravit hello prostředí skriptu použít toostart hello služby toopass tyto možnosti.

Do každého skriptu, je exportu pro  **\* \_OPTS**, který obsahuje možnosti hello předán toohello JVM. Například v hello **hadoop env.sh** skriptu, hello řádek, který začíná `export HADOOP_NAMENODE_OPTS=` obsahuje hello možnosti hello NameNode služby.

Mapování a snížit procesy se mírně liší, tyto operace jsou podřízený proces systému hello MapReduce služby. Každý mapování, nebo snižte proces běží v kontejneru podřízené a existují dvě položky, které obsahují hello JVM možnosti. Obě obsažené v **mapred-site.xml**:

* **mapreduce.Admin.map.child.Java.opts**
* **mapreduce.Admin.reduce.child.Java.opts**

> [!NOTE]
> Doporučujeme pomocí nástroje Ambari toomodify obou hello skripty a mapred-site.xml nastavení, jako Ambari zpracování replikace změn mezi uzly v clusteru hello. V tématu hello [pomocí Ambari](#using-ambari) konkrétní postup.

### <a name="enable-heap-dumps"></a>Povolení výpisů paměti haldy

Hello následující možnost umožňuje výpisy heap když dojde OutOfMemoryError:

    -XX:+HeapDumpOnOutOfMemoryError

Hello  **+**  označuje, že tato možnost je povolená. Výchozí Hello je zakázána.

> [!WARNING]
> Jako hello souborů výpisu paměti může být velký haldy výpisy nejsou povolené pro služby Hadoop v HDInsight. Pokud povolíte je pro řešení potíží, mějte na paměti toodisable je po opakuje se hello problému a získaná hello souborů výpisu paměti.

### <a name="dump-location"></a>Výpis umístění

Hello výchozí umístění souboru s výpisem hello je hello aktuální pracovní adresář. Můžete řídit hello kde je uložený soubor pomocí hello následující možnost:

    -XX:HeapDumpPath=/path

Například pomocí `-XX:HeapDumpPath=/tmp` způsobí, že toobe výpisy hello uložené v adresáři TMP hello.

### <a name="scripts"></a>Skripty

Můžete také spustit skript při **OutOfMemoryError** dojde. Například spouštěcí oznámení, abyste věděli, že chyba hello došlo k chybě. Použití hello následující možnost tootrigger skript __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> Vzhledem k tomu, že Hadoop je distribuovaný systém, musí být umístěny všech skriptů používá na všech uzlech v clusteru hello, který běží služba hello na.
> 
> skript Hello musí také být v umístění, která je přístupná pomocí nástroje hello účet hello service běží jako a musí poskytnout oprávnění ke spouštění. Například můžete toostore skripty v `/usr/local/bin` a používat `chmod go+rx /usr/local/bin/filename.sh` toogrant číst a spouštět oprávnění.

## <a name="using-ambari"></a>Pomocí nástroje Ambari

Konfigurace hello toomodify služby hello použijte následující kroky:

1. Otevřete hello Ambari webového uživatelského rozhraní pro váš cluster. Adresa URL Hello je https://YOURCLUSTERNAME.azurehdinsight.net.

    Po zobrazení výzvy ověřování toohello lokality pomocí názvu účtu hello HTTP (výchozí: správce) a heslo pro váš cluster.

   > [!NOTE]
   > Můžete být vyzváni podruhé pomocí Ambari pro hello uživatelské jméno a heslo. Pokud ano, zadejte text hello stejný název účtu a heslo

2. Pomocí seznamu hello na levé straně hello vyberte oblasti služby hello chcete toomodify. Například **HDFS**. V oblasti hello center, vyberte hello **konfigurací** kartě.

    ![Obrázek Ambari web s vybranou kartou HDFS konfigurací](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Pomocí hello **filtru...**  položku, zadejte **požádá**. Zobrazí se pouze položky obsahující tento text.

    ![Filtrovaný seznam](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Najde hello  **\* \_OPTS** položka služby hello výpisů paměti haldy tooenable pro a přidejte hello možnosti, které chcete tooenable. V hello následující bitové kopie, byly přidány `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** položku:

    ![HADOOP_NAMENODE_OPTS s - XX: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > Při povolení výpisů paměti haldy pro hello mapy nebo snižte podřízeného procesu, vyhledejte hello pole s názvem **mapreduce.admin.map.child.java.opts** a **mapreduce.admin.reduce.child.java.opts**.

    Použití hello **Uložit** tlačítko toosave hello změny. Můžete zadat krátký poznámka popisující hello změny.

5. Jakmile provedli změny hello hello **je vyžadován Restart** zobrazí ikona vedle jednu nebo více služeb.

    ![Restartujte vyžaduje ikonu a restartujte tlačítko](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Vyberte každá služba, která vyžaduje restartování a použijte hello **služby akce** tlačítko příliš**zapnout v režimu údržby**. Režim údržby zabrání výstrahy generován ze služby hello při restartování.

    ![Zapnout nabídky režim údržby](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. Po povolení režimu údržby použít hello **restartujte** tlačítko pro službu hello příliš**restartujte všechny uskutečněn**

    ![Restartujte všechny ovlivněné položky](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > Hello položky pro hello **restartujte** tlačítko se může lišit pro jiné služby.

8. Po spuštění hello služeb, použijte hello **služby akce** tlačítko příliš**zapnout vypnout režimu údržby**. Tato Ambari tooresume monitorování výstrah pro službu hello.

