---
title: "Zobrazení Ambari toowork aaaUse s Hive v HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello zobrazení Hive z vaší webové prohlížeče toosubmit Hive dotazy. Hello zobrazení Hive je součástí hello poskytnuté webové uživatelské rozhraní Ambari k vašemu clusteru HDInsight se systémem Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: f9a77b652e70d34a0ff9165fbb8c2e16d3401ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-hive-view-with-hadoop-in-hdinsight"></a>Použít hello zobrazení Hive se systémem Hadoop v HDInsight

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Zjistěte, jak toorun Hive dotazuje pomocí zobrazení Ambari Hive. Ambari je správy a monitorování nástroj s clustery HDInsight se systémem Linux. Jedna z funkcí hello poskytované prostřednictvím Ambari je webového uživatelského rozhraní, které můžou být použité toorun dotazů Hive.

> [!NOTE]
> Ambari má mnoho možností, které nejsou popsané v tomto dokumentu. Další informace najdete v tématu [Správa clusterů HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Požadavky

* Cluster HDInsight se systémem Linux. Informace týkající se vytvoření clusteru najdete v tématu [Začínáme s HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-hello-hive-view"></a>Otevření zobrazení Hive hello

Zobrazení Ambari můžete z portálu Azure; hello Vyberte HDInsight cluster a pak vyberte **zobrazení Ambari** z hello **rychlé odkazy** části.

![Rychlé odkazy části portálu hello](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

Ze seznamu hello zobrazení, vyberte hello __zobrazení Hive__.

![Hello vybrané zobrazení Hive](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Při přístupu k Ambari, jste výzvami tooauthenticate toohello lokality. Zadejte Dobrý den, správce (výchozí `admin`) účet jméno a heslo použité při vytváření hello clusteru.

Měli byste vidět stránky podobné toohello následující bitové kopie:

![Obrázek hello listu dotazu pro zobrazení Hive hello](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="hivequery"></a>Spuštění dotazu

toorun hive dotaz, použít následující kroky ze zobrazení Hive hello hello.

1. Z hello __dotazu__ kartě, vložte následující příkazy HiveQL do listu hello hello:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Tyto příkazy provádět hello následující akce:

   * `DROP TABLE`-Odstraní se hello tabulky a hello datový soubor, v případě, že hello tabulka již existuje.

   * `CREATE EXTERNAL TABLE`-Vytvoří novou tabulku "externí" v Hive.
   Externí tabulky uložit jenom hello Definice tabulky Hive. Hello data je ponechán v hello původního umístění.

   * `ROW FORMAT`-Hello data formátování. V takovém případě hello pole v každém protokolu jsou oddělené mezerou.

   * `STORED AS TEXTFILE LOCATION`-Hello je ukládat data a že je uloženo jako text.

   * `SELECT`-Vybere počet všech řádků, kde t4 sloupec obsahuje hodnotu hello [Chyba].

     > [!NOTE]
     > Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje. Například automatické nahrání dat je proces, nebo jiná operace MapReduce. Vyřazení externí tabulku nemá *není* odstranit hello data, jenom hello definici tabulky.

    > [!IMPORTANT]
    > Nechte hello __databáze__ výběr v __výchozí__. Hello příklady v tomto dokumentu používají hello výchozí databázi, která je zahrnutá v HDInsight.

2. toostart hello dotaz, použít hello **Execute** tlačítko pod hello listu. Změní oranžová a hello text se změní příliš**Zastavit**.

3. Po dokončení dotazu hello hello **výsledky** karta zobrazuje výsledky hello hello operace. Následující text Hello je hello výsledek dotazu hello:

        sev       cnt
        [ERROR]   3

    Hello **protokoly** karta může být použité tooview hello protokolování informací o vytvořených hello úlohou.

   > [!TIP]
   > Hello **výsledky uložit** dialogové okno rozevíracího seznamu ve hello horní pravé hello **výsledky zpracování dotazu** části vám umožní toodownload nebo uložení výsledků.

4. Vyberte hello první čtyři řádky tohoto dotazu, pak vyberte **Execute**. Všimněte si, že po dokončení úlohy hello neexistují žádné výsledky. Pomocí hello **Execute** tlačítko, pokud je součástí hello dotazu je vybrána pouze spustí hello vybrané příkazy. V takovém případě hello výběr nezahrnuli hello poslední příkaz, který načte řádky z tabulky hello. Pokud vyberte právě daného řádku a pomocí **Execute**, měli byste vidět hello očekávané výsledky.

5. tooadd listu, použijte hello **nový list** tlačítko dole hello hello **Editor dotazů**. V novém listu hello zadejte následující příkazy HiveQL hello:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Tyto příkazy provádět hello následující akce:

   * **Vytvoření tabulka není v případě existuje** -vytvoří tabulku, pokud ještě neexistuje. Od hello **externí** – klíčové slovo nepoužívá, k vytvoření interní tabulky. Interní tabulku je uložena v datovém skladu Hive hello a je zcela spravuje Hive. Na rozdíl od externích tabulek se odstranit interní tabulku odstraní hello základní data.

   * **ULOŽENÉ ORC AS** – ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC). ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.

   * **PŘEPSAT INSERT... Vyberte** -vybere řádky z hello **log4jLogs** tabulku, která obsahují `[ERROR]`, a poté vloží hello data do hello **errorLogs** tabulky.

     Použití hello **Execute** tlačítko toorun tento dotaz. Hello **výsledky** karta neobsahuje žádné informace, pokud hello dotaz vrátí nulový počet řádků. Stav Hello by měl zobrazit jako **úspěšné** po dokončení dotazu hello.

### <a name="visual-explain"></a>Vysvětlují Visual

toodisplay vizualizaci hello plán dotazu, vyberte hello **Visual vysvětlují** kartě níže hello listu.

Hello **Visual vysvětlují** hello dotazu může být užitečné pro porozumění hello toku složitých dotazů. Textové ekvivalent v tomto zobrazení můžete zobrazit pomocí hello **vysvětlit** tlačítka na hello editoru dotazů.

### <a name="tez-ui"></a>Tez uživatelského rozhraní

toodisplay hello Tez uživatelského rozhraní pro hello dotaz, vyberte hello **Tez** kartě níže hello listu.

> [!IMPORTANT]
> Tez je nepoužívá tooresolve všechny dotazy. Mnoho dotazů, bez použití Tez lze vyřešit. 

Pokud Tez použité tooresolve hello dotazu, zobrazí se hello řízené Acyklické grafu (DAG). Pokud chcete tooview hello DAG pro dotazy, které jste spustili v posledních hello nebo ladění hello Tez procesu, použijte hello [Tez zobrazení](hdinsight-debug-ambari-tez-view.md) místo.

## <a name="view-job-history"></a>Zobrazení historie úlohy

Hello __úlohy__ karta se zobrazuje historie dotazů Hive.

![Obrázek hello historie úlohy](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Databázové tabulky

Můžete použít hello __tabulky__ kartě toowork u tabulek v rámci databáze Hive.

![Obrázek kartu tabulky hello](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Uložené dotazy

Z karty hello dotazu můžete volitelně uložit dotazy. Po uložení, můžete opakovaně použít dotaz hello z hello __uložené dotazy__ kartě.

![Obrázek karty uložené dotazy](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Uživatelem definované funkce

Hive lze také rozšířit pomocí uživatelem definovaných funkcí (UDF). Uživatelem definovanou FUNKCI umožňuje tooimplement funkce nebo logiky, která není snadno modelován v HiveQL.

Hello UDF karta hello horní části hello zobrazení Hive můžete toodeclare a uložte sadu UDF. Tyto funkce lze použít s hello **Editor dotazů**.

![Obrázek karty UDF](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

Po přidání UDF toohello zobrazení Hive **vložit UDF** dole hello hello se zobrazí tlačítko **Editor dotazů**. Výběrem této položky se zobrazí rozevírací seznam hello UDF definované v hello zobrazení Hive. Výběr UDF přidá HiveQL příkazy tooyour dotazu tooenable hello UDF.

Například, pokud jste definovali uživatelem definovanou FUNKCI hello následující vlastnosti:

* Název prostředku: myudfs

* Cesta prostředku: /myudfs.jar

* Název UDF: myawesomeudf

* Název třídy UDF: com.myudfs.Awesome

Pomocí hello **vložit UDF** tlačítko zobrazí položka s názvem **myudfs**, s jinou rozevíracího seznamu pro každý UDF definované pro tento prostředek. V takovém případě **myawesomeudf**. Výběrem této položky přidáte hello následující toohello začátku hello dotazu:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Potom můžete hello UDF v dotazu. Například, `SELECT myawesomeudf(name) FROM people;`.

Další informace o používání funkce UDF s Hive v HDInsight naleznete v tématu hello následující dokumenty:

* [Python pomocí Hive a Pig v HDInsight](hdinsight-python.md)
* [Jak tooadd vlastní tooHDInsight Hive UDF](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Nastavení Hive

Nastavení může být použité toochange různá nastavení Hive. Například změna hello spouštěcímu modulu pro Hive z tooMapReduce Tez (hello výchozí nastavení).

## <a id="nextsteps"></a>Další kroky

Obecné informace o Hive v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
