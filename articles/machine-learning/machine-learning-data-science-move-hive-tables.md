---
title: "aaaCreate tabulek Hive a načtení dat z Azure Blob Storage | Microsoft Docs"
description: "Vytváření tabulek Hive a načtení dat do tabulky toohive objektu blob"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 09622972bcac31c2971858393a8340f24e4b7390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Vytváření tabulek Hive a načtení dat z Azure Blob Storage
Toto téma představuje obecné dotazů Hive, které vytváření tabulek Hive a načtení dat z Azure blob storage. Některé pokyny jsou tu taky o dělení tabulek Hive a o používání výkon formátování tooimprove dotazů hello optimalizované řádek sloupcovém (ORC).

To **nabídky** odkazy tootopics, které popisují, jak tooingest data do cílové prostředí, kde můžete ukládat a zpracovávat během hello data hello tým datové vědy procesu (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* Vytvořit účet úložiště Azure. Pokud budete potřebovat pokyny, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).
* Zřizuje přizpůsobené clusteru Hadoop s hello služba HDInsight.  Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilou analýzu](machine-learning-data-science-customize-hadoop-cluster.md).
* Clusteru toohello povoleno vzdáleného přístupu, přihlášení a otevřít konzolu příkazového řádku hello Hadoop. Pokud budete potřebovat pokyny, najdete v části [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-tooazure-blob-storage"></a>Nahrání dat tooAzure objektu blob úložiště
Pokud jste vytvořili virtuální počítač Azure podle pokynů hello součástí [nastavení virtuálního počítače Azure pro pokročilou analýzu](machine-learning-data-science-setup-virtual-machine.md), tento soubor skriptu by musela být stažené toohello *C:\\ Uživatelé\\\<uživatelské jméno\>\\dokumenty\\datové vědy skripty* v hello virtuálního počítače. Tyto dotazy Hive vyžadují jenom zařaďte vlastní schéma dat a konfigurace úložiště objektů blob v Azure v toobe odpovídající pole hello připravené k odeslání.

Předpokládáme, že data hello tabulek Hive jsou v **nekomprimované** tabulkovém formátu, a aby byla hello data nahrané toohello výchozí (nebo další tooan) kontejneru účtu úložiště hello používá hello Hadoop cluster.

Pokud chcete, aby toopractice na hello **NYC taxíkem cestě Data**, budete muset:

* **Stáhněte si** hello 24 [NYC taxíkem cestě Data](http://www.andresmh.com/nyctaxitrips) soubory (12 cestě soubory a soubory 12 tarif),
* **Rozbalte** všechny soubory do souborů .csv a potom
* **Nahrát** je toohello výchozí (nebo odpovídajícího kontejneru) hello účtu úložiště Azure, který byl vytvořen postup hello uvedených v hello [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy proces a technologie ](machine-learning-data-science-customize-hadoop-cluster.md) tématu. Hello proces tooupload hello .csv soubory toohello výchozí kontejner hello účtu úložiště najdete v tomto [stránky](machine-learning-data-science-process-hive-walkthrough.md#upload).

## <a name="submit"></a>Jak toosubmit dotazů Hive
Dotazy Hive můžete odeslat pomocí:

1. [Odesílání dotazů Hive prostřednictvím Hadoop příkazového řádku v headnode clusteru Hadoop](#headnode)
2. [Odesílání dotazů Hive s hello Hive Editor](#hive-editor)
3. [Odesílání dotazů Hive pomocí příkazů prostředí PowerShell Azure](#ps)

Dotazy Hive jsou podobné jazyku SQL. Pokud jste obeznámeni s SQL, může se stát hello [Hive pro SQL uživatelé cheaty list](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) užitečné.

Při odesílání dotazů Hive, můžete také ovládat hello cílové hello výstupu z dotazů Hive, zda byl na hello obrazovky nebo tooa místního souboru na hello hlavního uzlu nebo tooan objektů blob v Azure.

### <a name="headnode"></a> 1. Odesílání dotazů Hive prostřednictvím Hadoop příkazového řádku v headnode clusteru Hadoop
Pokud hello Hive dotaz je složité, odesílá se, že jej přímo do hello hlavního uzlu v clusteru Hadoop hello obvykle vede zapnout toofaster kolem než odesílání se v editoru Hive nebo Azure PowerShell skripty.

Přihlášení toohello hlavního uzlu v clusteru Hadoop hello, otevřete na ploše hello hlavního uzlu hello hello Hadoop příkazového řádku a zadejte příkaz `cd %hive_home%\bin`.

Máte tři způsoby toosubmit Hive dotazy v hello Hadoop příkazového řádku:

* přímo
* pomocí souborů .hql
* s hello Hive konzoly příkazu

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Odesílání dotazů Hive přímo v Hadoop příkazového řádku.
Můžete spustit příkaz jako `hive -e "<your hive query>;` toosubmit jednoduchých dotazů Hive přímo v Hadoop příkazového řádku. Tady je příklad, kde hello red pole jsou podrobněji popsány dále hello příkaz, který odešle dotaz Hive hello a hello zelené pole jsou podrobněji popsány dále hello výstup hello dotaz Hive.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Odesílání dotazů Hive v souborech .hql
Pokud dotaz Hive hello je složitější a má více řádků, není praktické úpravy dotazů v příkazovém řádku nebo konzoly příkazu Hive. Alternativou je toouse textového editoru v hello hlavního uzlu v clusteru hello Hadoop toosave hello dotazů na Hive v souboru .hql do místního adresáře hello hlavního uzlu. Potom pomocí hello jde odeslat dotaz Hive hello v souboru .hql hello `-f` argument následujícím způsobem:

    hive -f "<path toohello .hql file>"

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

**Potlačit tisk obrazovky stav průběhu dotazů Hive**

Ve výchozím nastavení po odeslání dotazu Hive v systému Hadoop příkazového řádku hello průběh úlohy mapy nebo snižte hello se vytiskne na obrazovce. toosuppress hello obrazovky tiskových průběh úlohy mapy nebo snižte hello, můžete použít argument `-S` ("S velkými písmeny) v hello příkazový řádek následujícím způsobem:

    hive -S -f "<path toohello .hql file>"
.    Hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Odesílání dotazů Hive v konzole příkaz Hive.
Hello Hive příkaz konzoly můžete zadat také nejprve spuštěním příkazu `hive` v Hadoop příkazového řádku a pak odesílání dotazů Hive v konzole příkaz Hive. Tady je příklad. V tomto příkladu hello dvě červená pole zvýraznění hello příkazy používané tooenter hello Hive konzoly příkazu a hello dotaz Hive odeslání v konzole příkaz Hive v uvedeném pořadí. pole Hello zelená označuje hello výstup hello dotaz Hive.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

předchozí příklady Hello přímo výstup výsledků dotazu Hive hello na obrazovce. Je také možné zapsat místního souboru tooa hello výstup hello hlavního uzlu nebo tooan objektů blob v Azure. Pak můžete použít jiné nástroje toofurther analyzovat výstup hello dotazů Hive.

**Výstupu podregistru dotazu výsledky tooa místního souboru.**
toooutput Hive dotaz výsledky tooa místního adresáře v hello hlavního uzlu, máte dotaz Hive hello toosubmit v hello Hadoop příkazový řádek následujícím způsobem:

    hive -e "<hive query>" > <local path in hello head node>

V následujícím příkladu hello, výstup hello dotazu Hive je zapsán do souboru `hivequeryoutput.txt` v adresáři `C:\apps\temp`.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Výstupu podregistru dotazu výsledky tooan objektů blob v Azure**

Můžete také výstup hello Hive dotaz výsledky tooan objektů blob Azure v rámci clusteru Hadoop hello hello výchozí kontejner. dotaz Hive Hello to vypadá takto:

    insert overwrite directory wasb:///<directory within hello default container> <select clause from ...>

V následujícím příkladu hello, je zapsán výstup hello dotaz Hive tooa blob directory `queryoutputdir` v rámci clusteru Hadoop hello hello výchozí kontejner. Zde potřebujete jenom název adresáře hello tooprovide, bez hello název objektu blob. Pokud například zadáte názvy adresáře a objektů blob, je vyvolána chyba `wasb:///queryoutputdir/queryoutput.txt`.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Pokud otevřete hello výchozí kontejner hello clusteru Hadoop pomocí Průzkumníka úložiště Azure, uvidíte výstup hello dotaz Hive hello, jak ukazuje následující obrázek hello. Můžete použít objekt blob hello filtru (zvýrazněné podle červeným rámečkem) tooonly načtení hello s zadaná písmena v názvech.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Odesílání dotazů Hive s hello Hive Editor
Můžete použít také hello dotazu konzoly (Hive Editor) tak, že zadáte adresu URL ve formátu hello *https://&#60; Název clusteru Hadoop >.azurehdinsight.net/Home/HiveEditor* do webového prohlížeče. Musí být zaznamenána najdete v tématu hello této konzoly a proto je třeba zde pověření clusteru Hadoop.

### <a name="ps"></a> 3. Odesílání dotazů Hive pomocí příkazů prostředí PowerShell Azure
Můžete také použít dotazy Hive toosubmit prostředí PowerShell. Pokyny najdete v tématu [úlohy odeslání Hive pomocí prostředí PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Vytvoření databáze Hive a tabulky
Hello dotazů Hive jsou sdílené v hello [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) a z ní si můžete stáhnout.

Zde je hello dotaz Hive, který vytvoří tabulku Hive.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Tady je popis hello hello pole, které je třeba tooplug v a další konfigurace:

* **& č. 60; název databáze >**: název hello hello databáze, které chcete toocreate. Pokud chcete toouse hello výchozí databázi, hello dotazu *vytvoření databáze...*  lze vynechat.
* **& č. 60; název tabulky >**: hello název hello tabulky, které chcete toocreate v rámci hello zadaná databáze. Pokud chcete toouse hello výchozí databázi, se hello tabulky můžete přímo odkazuje *& č. 60; název tabulky >* bez & č. 60; název databáze >.
* **& č. 60; oddělovač polí >**: hello oddělovač, který vymezuje pole v hello dat souboru toobe nahrán toohello tabulku Hive.
* **& č. 60; oddělovací čáry >**: hello oddělovač, který vymezuje řádků v datovém souboru hello.
* **& č. 60; umístění úložiště >**: hello data o umístění toosave hello úložiště Azure tabulek Hive. Pokud nezadáte *umístění & č. 60; umístění úložiště >*, hello databáze a hello tabulek jsou uložené v *hive neboskladu/* adresář v kontejneru výchozí hello hello Hive clusteru ve výchozím nastavení. Pokud chcete umístění úložiště hello toospecify, má umístění úložiště hello toobe v rámci hello výchozí kontejner hello databáze a tabulky. Toto umístění se označuje jako umístění relativní toohello výchozí kontejner hello clusteru ve formátu hello toobe *' wasb: / / / & č. 60; adresář 1 > / "* nebo *' wasb: / / / & č. 60; adresář 1 > / & č. 60; adresář 2 > / "*atd. Po provedení dotazu hello jsou vytvořeny hello relativní adresáře v rámci hello výchozí kontejner.
* **TBLPROPERTIES("Skip.Header.line.Count"="1")**: Pokud hello datový soubor obsahuje řádek záhlaví, máte tooadd tuto vlastnost **na konci hello** z hello *vytvořit tabulku* dotazu. Řádek záhlaví hello, jinak je načten jako tabulku záznamů toohello. Pokud hello datový soubor nemá řádek záhlaví, můžete tuto konfiguraci vynechat v dotazu hello.

## <a name="load-data"></a>Načíst data tabulky tooHive
Zde je hello dotaz Hive, který načte data do tabulky Hive.

    LOAD DATA INPATH '<path tooblob data>' INTO TABLE <database name>.<table name>;

* **& č. 60; data tooblob cesty >**: Pokud hello objektu blob souboru toobe nahrán toohello tabulku Hive v hello výchozí kontejner hello clusteru HDInsight Hadoop, hello *& č. 60; data tooblob cesty >* musí být ve formátu hello *' wasb: / / / & č. 60; adresář v tomto kontejneru > / & č. 60; název souboru objektů blob >'*. soubor Hello blob může být také v další kontejner hello clusteru HDInsight Hadoop. V takovém případě *& č. 60; data tooblob cesty >* musí být ve formátu hello *' wasb: / / & č. 60; název kontejneru > @& č. 60; název účtu úložiště >.blob.core.windows.net/ & č. 60; název souboru objektů blob >'*.

  > [!NOTE]
  > Hello tabulka tooHive toobe nahrát data objektů blob má toobe v hello výchozí nebo další kontejner hello účtu úložiště pro hello Hadoop cluster. V opačném hello *načítání dat* nesouhlasících, že nelze přístup k datům hello se dotaz nezdaří.
  >
  >

## <a name="partition-orc"></a>Rozšířené témata: tabulka a úložiště Hive data ve formátu ORC na oddíly
Pokud hello data velká, vytváření oddílů tabulky hello je výhodné pro dotazy, které stačí tooscan několik oddílů tabulky hello. Například je přiměřené toopartition data protokolu hello webové stránky podle data.

V přidání toopartitioning Hive tabulky, je také užitečné toostore hello Hive data ve formátu hello optimalizované řádek sloupcovém (ORC). Další informace o ORC formátování naleznete v tématu <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">a souborů ORC pomocí zvyšuje výkon při čtení, zápisu a zpracování dat Hive</a>.

### <a name="partitioned-table"></a>Dělenou tabulku
Zde je hello dotaz Hive, který vytvoří dělenou tabulku a načte data do ní.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Při dotazování dělené tabulky, se doporučuje tooadd hello oddílu podmínky v hello **začátku** z hello `where` klauzule jako to zvyšuje účinnost hello výrazně vyhledávání.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Ukládání dat Hive ve formátu ORC
Nelze načíst přímo dat z úložiště objektů blob do tabulek Hive, které je uložený ve formátu ORC hello. Tady jsou kroky hello tooHive tabulek uložených ve formátu ORC objekty tohoto hello potřebujete tootake tooload dat z Azure BLOB.

Vytvoření externí tabulky **ULOŽENÝ textový soubor AS** a načtení dat z tabulky toohello úložiště objektů blob.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<table name>;

Vytvoření interní tabulky s hello stejného schématu jako hello externí tabulky v kroku 1, s hello stejné oddělovač polí a uložte hello Hive data ve formátu ORC hello.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Vyberte data z externí tabulky hello v kroku 1 a vložit do tabulky ORC hello

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Pokud tabulka textový soubor hello *& č. 60; název databáze >. & č. 60; název tabulky externí textový soubor >* má oddíly, v KROKU 3 hello `SELECT * FROM <database name>.<external textfile table name>` příkaz vybere hello oddílu proměnné pole v hello vrácený datové sady. Vkládání do hello *& č. 60; název databáze >. & č. 60; název tabulky ORC >* selže od *& č. 60; název databáze >. & č. 60; název tabulky ORC >* nemá hello jako proměnnou oddílu pole ve schématu tabulky hello. V takovém případě musíte toospecifically vyberte hello pole toobe vložit příliš*& č. 60; název databáze >. & č. 60; název tabulky ORC >* následujícím způsobem:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Je bezpečné toodrop hello *& č. 60; název tabulky externí textový soubor >* při použití hello následující dotaz po všechna data byla vložena do *& č. 60; název databáze >. & č. 60; název tabulky ORC >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Až projdete tímto postupem, měli byste mít tabulku s daty v připravené toouse formátu hello ORC.  
