---
title: "aaaConnect tooAzure HDInsight pomocí nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooinstall a použití nástrojů Data Lake pro Visual Studio tooconnect tooHadoop clusterů v Azure HDInsight a spouštět dotazy Hive."
keywords: hadoop tools, hive query, visual studio, visual studio hadoop
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: ff5819a64bebe5f4ab3cf763ce6c45c81aa34b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>Připojit tooAzure HDInsight a spouštět dotazy Hive pomocí nástrojů Data Lake pro Visual Studio

Zjistěte, jak toouse nástrojů Data Lake pro Visual Studio tooconnect tooHadoop clusterů v [Azure HDInsight](hdinsight-hadoop-introduction.md) a odesílání dotazů Hive. Další informace o používání HDInsight naleznete v tématu [Úvod tooHDInsight](hdinsight-hadoop-introduction.md) a [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md). Další informace o připojování clusteru Storm tooa najdete v tématu [vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Nástroje data Lake pro Visual Studio může být použité tooaccess Data Lake Analytics a HDInsight.  Hello informace o nástrojů Data Lake najdete v tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Požadavky**

toocomplete tohoto kurzu a používání hello nástroje Data Lake v sadě Visual Studio, budete potřebovat následující hello:

* Cluster Azure HDInsight: jeden, viz toocreate [začněte používat HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md)
* Pracovní stanice s hello následující software:
  
  * Windows 10, Windows 8.1, Windows 8 nebo Windows 7.
  * Visual Studio 2013/2015/2017.
    
    > [!NOTE]
    > V současné době hello nástrojů Data Lake pro Visual Studio pouze dodávají s hello anglickou verzi.
    > 
    > 

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalace nástrojů Data Lake Tools pro Visual Studio

Data Lake Tools se pro sadu Visual Studio 2017 instalují ve výchozím nastavení. Pro starší verze, můžete nainstalovat pomocí hello [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/). Hello musíte zvolit ten, který se shoduje s vaší verzí sady Visual Studio. Pokud nemáte nainstalovanou sadu Visual Studio, můžete nainstalovat hello nejnovější Visual Studio Community a Azure SDK pomocí hello [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/):

![Nástroje data Lake pro Visual Studio webové platformy. ] (./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Tooinstall instalačního programu webové platformy pomocí nástrojů Data Lake pro Visual Studio")

## <a name="connect-tooazure-subscriptions"></a>Připojit tooAzure odběrů
Nástroje data Lake pro Visual Studio můžete clustery HDInsight tooyour tooconnect, provádění některých operací základní správy a spouštění dotazů Hive.

> [!NOTE]
> Informace o připojování tooa obecnému clusteru Hadoop naleznete v tématu [zapisování a odesílání dotazů Hive pomocí sady Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

**tooconnect tooyour předplatného Azure**

1. Otevřete sadu Visual Studio.
2. Z hello **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru** okno Průzkumníka serveru tooopen hello.
3. Rozbalte položku **Azure** a pak rozbalte **HDInsight**.
   
   > [!NOTE]
   > Všimněte si hello **seznam úloh HDInsight** by se mělo otevřít okno. Pokud ho nevidíte, klikněte na tlačítko **ostatní okna** z hello **zobrazení** nabídce a pak klikněte na tlačítko **okno seznam úloh HDInsight**.  
   > 
   > 
4. Zadejte své přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **Přihlásit**. Používá se pouze požadované Pokud nikdy připojíte toohello předplatného Azure ze sady Visual Studio na této pracovní stanici.
5. V Průzkumníku serveru se zobrazí seznam stávajících clusterů HDInsight. Pokud nemáte žádné clustery, můžete vytvořit jeden pomocí hello portálu Azure, Azure PowerShell nebo hello HDInsight SDK. Další informace najdete v tématu [Vytvoření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Seznam clusterů v Průzkumníku serveru pro Data Lake pro Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Průzkumník serveru pro Data Lake pro Visual Studio")
6. Rozbalte cluster služby HDInsight. Zobrazí se **Databáze Hive**, výchozí účet úložiště, propojené účty úložiště a **protokol služby Hadoop**. Můžete dále rozšířit hello entity.

Po připojení tooyour předplatné Azure, budete moct toodo hello následující:

**tooconnect toohello portálu Azure ze sady Visual Studio**

* Z Průzkumníka serveru rozbalte **Azure** > **HDInsight**, klikněte pravým tlačítkem na cluster HDInsight a pak klikněte na tlačítko **Spravovat cluster webu Azure Portal**.

**tooask otázky a napsat svůj názor ze sady Visual Studio**

* Z hello **nástroje** nabídky, klikněte na tlačítko **HDInsight**a potom klikněte na **fórum MSDN** tooask otázky, nebo klikněte na tlačítko **odeslat zpětnou vazbu**.

## <a name="navigate-hello-linked-resources"></a>Přejděte hello propojené prostředky
V Průzkumníku serveru můžete zobrazit hello výchozí účet úložiště a všechny propojené účty úložiště. Pokud rozbalíte hello výchozí účet úložiště, uvidíte hello kontejnery na účtu úložiště hello. jsou označeny Hello výchozí účet úložiště a hello výchozí kontejner. Můžete můžete také kliknout pravým tlačítkem myši hello kontejnery tooview hello obsah.

![Seznam propojených prostředků v Průzkumníku serveru pro Data Lake pro Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Seznam propojených prostředků")

Po otevření kontejner, můžete použít následující tooupload tlačítka, odstranění a objekty BLOB stažení hello:

![Operace s objekty blob v Průzkumníku serveru pro Data Lake Tools pro Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Nahrání, odstranění a stažení objektů blob")

## <a name="run-a-hive-query"></a>Spuštění dotazu Hive
[Apache Hive](http://hive.apache.org) je infrastruktura datového skladu postavená na Hadoop pro poskytování souhrnů dat, dotazů a analýz. Data Lake Tools pro Visual Studio podporují spouštění dotazů Hive ze sady Visual Studio. Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight](hdinsight-use-hive.md).

Je časově náročná tootest skriptu Hive proti clusteru služby HDInsight. Může to trvat několik minut nebo déle. Nástroje data Lake pro Visual Studio je schopen ověřování skriptu místně bez připojování tooa živému clusteru.

Nástroje data Lake pro Visual Studio také umožňuje uživatelům toosee co je uvnitř hello úlohy Hive díky shromažďování a zpřístupnění protokolů YARN hello určitých úloh Hive.

### <a name="view-hello-hivesampletable"></a>Zobrazení hello **hivesampletable**
Všechny clustery HDInsight se vzorovou tabulkou Hive se nazývají *hivesampletable*. Použijeme tooshow této tabulky můžete způsobu toolist tabulek Hive, Zobrazit schémata tabulek hello a seznam řádků hello v hello Hive tabulky.

**toolist tabulek Hive a zobrazení schématu tabulek Hive**

1. Z **Průzkumníka serveru**, rozbalte položku **Azure** > **HDInsight** > zvoleného clusteru hello > **databáze Hive**  >  **Výchozí** > **hivesampletable** schématu tabulky toosee hello.
2. Klikněte pravým tlačítkem na **hivesampletable**a potom klikněte na **zobrazit prvních 100 řádků** toolist hello řádků. Jedná se o ekvivalent toorunning hello následující dotaz Hive pomocí ovladače Hive ODBC:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Počet řádků hello můžete přizpůsobit.
   
   ![Data Lake Tools: dotaz schématu HDinsight Hive sady Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Výsledky dotazu Hive")

### <a name="create-hive-tables"></a>Vytváření tabulek Hive
Můžete použít grafické uživatelské rozhraní toocreate hello tabulku Hive nebo dotazů Hive. Informace o použití dotazů Hive naleznete v tématu [Spouštění dotazů Hive](#run.queries).

**toocreate tabulku Hive**

1. V nabídce **Průzkumníku serveru** rozbalte položku **Azure** > **HDInsight clustery** cluster služby HDInsight > **Databáze Hive**, klikněte pravým tlačítkem myši na položku **výchozí** a klikněte na tlačítko **Vytvořit tabulku**.
2. Nakonfigurujte hello tabulky.
3. Klikněte na tlačítko **Create Table** toosubmit hello úlohy toocreate hello novou tabulku Hive.
   
    ![Data Lake Tools: nástroje HDInsight sady Visual Studio vytvoří tabulku Hive](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Vytvoření tabulky Hive")

### <a name="run.queries"></a>Ověřování a spouštění dotazů Hive
Existují dva způsoby, jak toocreate a spouštět dotazy Hive:

* Vytváření dotazů ad-hoc
* Vytvoření aplikace Hive

**toocreate, ověřování a spouštění dotazů ad-hoc**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **clustery HDInsight**.
2. Klikněte pravým tlačítkem na hello clusteru, kam chcete toorun hello dotazu a pak klikněte na tlačítko **napsat dotaz Hive**.
3. Zadejte dotazy Hive hello. Editor Hive hello oznámení podporuje technologii IntelliSense. Nástroje data Lake pro Visual Studio podporuje načítání hello vzdálených metadat při úpravách skriptu Hive. Například když zadáte "SELECT * FROM", hello IntelliSense zobrazí seznam všech hello navrhovaných tabulek. Pokud je zadán název tabulky, názvy sloupců hello jsou uvedeny podle hello IntelliSense. nástroje Hello téměř všechny příkazy Hive DML, poddotazech, podpory a hello integrované funkce UDF.
   
    ![Data Lake Tools: IntelliSense pro nástroje HDInsight sady Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Data Lake Tools: IntelliSense pro nástroje HDInsight sady Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Navrhne pouze hello metadata clusterů hello vybranou v panelu nástrojů HDInsight.
   > 
   > 
4. (Volitelné): klikněte na tlačítko **ověření skriptu** chyby syntaxe skriptu toocheck hello.
   
    ![Data Lake Tools: místní ověřování Data Lake Tools pro Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Ověření skriptu")
5. Klikněte na tlačítko **Odeslat** nebo **Odeslat (rozšířené)**. S hello rozšířené možnosti pro odeslání, budete konfigurovat **název úlohy**, **argumenty**, **další konfigurace**, a **stavový adresář**hello skriptu:
   
    ![Dotaz Hive v HDInsight Hadoop](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Odeslání dotazů")
   
    Po odeslání úlohy hello se zobrazí **Souhrn úlohy Hive** okno.
   
    ![Souhrn dotazu Hive v HDInsight Hadoop](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Souhrn úlohy Hive")
6. Použití hello **aktualizovat** tlačítko tooupdate hello stav dokud změny stavu úlohy hello příliš**dokončeno**.
7. Klikněte na tlačítko hello odkazy v následující hello toosee dolní hello: **dotaz úlohy**, **výstup úlohy**, **protokol úlohy**, nebo **protokolu Yarn**.

**toocreate a spuštění řešení Hive**

1. Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
2. Vyberte **HDInsight** hello levém podokně vyberte **aplikace Hive** v prostředním podokně hello, zadejte vlastnosti hello a pak klikněte na tlačítko **OK**.
   
    ![Data Lake Tools: nový projekt Hive v HDInsight sady Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Vytvoření aplikací Hive v sadě Visual Studio")
3. Z **Průzkumníku řešení**, dvakrát klikněte na **Script.hql** tooopen ho.
4. skript Hive hello toovalidate, můžete kliknout na hello **ověření skriptu** tlačítko, nebo klikněte pravým tlačítkem na skript hello v editoru Hive hello a pak klikněte na tlačítko **ověření skriptu** hello místní nabídce.

### <a name="view-hive-jobs"></a>Zobrazení úloh Hive
Můžete zobrazit dotazy úlohy, výstup úlohy, protokoly úlohy a protokoly Yarn pro úlohy Hive. Další informace najdete v tématu hello předchozím snímku obrazovky.

Hello nejnovější verzi nástroje hello umožňuje toosee co je uvnitř vaší úlohy Hive díky shromažďování a zpřístupnění protokolů YARN. Protokol YARN vám může pomoci prozkoumat problémy s výkonem. Další informace o tom, jak služba HDInsight shromažďuje protokoly YARN, najdete v tématu [Programový přístup k protokolům aplikací v prostředí HDInsight](hdinsight-hadoop-access-yarn-app-logs.md).

**tooview úloh Hive**

1. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte **HDInsight**.
2. Klikněte pravým tlačítkem cluster HDInsight a potom klikněte na tlačítko **Zobrazit úlohy**. Zobrazí seznam hello Hive úlohy, které byly spuštěny v clusteru hello.
3. Klikněte na úlohu v hello úlohy seznamu tooselect ho a pak použijte hello **Souhrn úlohy Hive** okno tooopen **dotaz úlohy**, **výstup úlohy**, **protokol úlohy**, nebo **protokolu Yarn**.
   
    ![Data Lake Tools: zobrazení úloh Hive v HDInsight Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Zobrazení úloh Hive")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Rychlejší cesta ke spouštění Hive prostřednictvím HiveServer2
> [!NOTE]
> Tato funkce funguje pouze na verzi clusteru HDInsight 3.2 a novější.
> 
> 

Nástroje Data Lake Hello používá toosubmit úloh Hive prostřednictvím [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (také známé jako Templeton). Trvalo dlouhou dobu tooreturn podrobnosti úlohy a informace o chybě.
V pořadí toosolve Hive tento problém s výkonem, hello nástrojů Data Lake provádí úlohy přímo v hello clusteru prostřednictvím HiveServer2, tak, aby obešly RDP/SSH.
Přidání toobetter výkonu uživatelé můžete také zobrazit Hive na grafech Tez a hello podrobnosti úlohy.

U clusteru HDInsight verze 3.2 nebo vyšší můžete vidět tlačítko **Spustit prostřednictvím HiveServer2**:

![Data Lake Visual Studio Tools spouští úlohy prostřednictvím HiveServer2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Spouštění dotazů Hive pomocí HiveServer2")

A můžete najdete v části hello protokoly vysílané datovým proudem ve v reálném čase a zobrazit grafy úloh hello, pokud hello dotaz Hive spouští v Tez.

![Rychlá cesta spuštění Hive v Data Lake Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Zobrazení grafů úloh")

**Rozdíl mezi spouštěním dotazů prostřednictvím HiveServer2 a odesíláním dotazů prostřednictvím WebHCat**

I když má zpracování dotazů prostřednictvím HiveServer2 mnoho výkonnostních výhod, má několik omezení. Některé hello omezení nejsou vhodná pro produkční účely. Hello následující tabulka uvádí hello rozdíly:

|  | Spouštění prostřednictvím HiveServer2 | Odesílání prostřednictvím WebHCat |
| --- | --- | --- |
| Provádění dotazů |Eliminuje hello režijní náklady v WebHCat (které spustí úlohu MapReduce s názvem "TempletonControllerJob"). |Dokud je dotaz proveden prostřednictvím WebHCat, spustí WebHCat úlohu MapReduce, která zavádí další latence. |
| Zpětné protokolování datovými proudy |Téměř v reálném čase. |Hello protokoly spouštění úlohy jsou k dispozici jenom v případě, že dokončení úlohy hello. |
| Zobrazení historie úlohy |Pokud je dotaz proveden prostřednictvím HiveServer2, nezachová se jeho historie úloh (protokol úloh, výstup úloh). aplikace Hello lze zobrazit v uživatelském rozhraní YARN s omezenými informacemi. |Pokud je dotaz proveden prostřednictvím WebHCat, jeho historie úloh (protokol úlohy, výstup úlohy) je zachována a lze ji zobrazit pomocí aplikace Visual Studio/HDInsight SDK/PowerShell. |
| Zavření okna |Spouštění prostřednictvím HiveServer2 je "synchronní" způsob, takže je nutné zachovat hello windows otevřete; Pokud jsou uzavřeny hello windows budou zrušeny hello při provádění dotazu. |Odesílání prostřednictvím WebHCat je "asynchronní" tak, abyste mohli odeslat hello dotazů prostřednictvím WebHCat a zavřete Visual Studio. Můžete se vrátit a zobrazit výsledky hello kdykoli. |

### <a name="tez-hive-job-performance-graph"></a>Graf výkonu úlohy Tez Hive
Hello nástrojů Data Lake podporují zobrazení grafů výkonu pro hello úlohy Hive spuštěné prostřednictvím modulu Tez hello. Informace o povolení Tez najdete v tématu [Používání Hive ve službě HDInsight](hdinsight-use-hive.md). Po odeslání hello podregistru úlohy v sadě Visual Studio, Visual Studio se dozvíte po dokončení úlohy hello grafu.  Může být nutné tooclick hello **aktualizovat** tlačítko tooget hello nejnovější stav úlohy.

> [!NOTE]
> Tato funkce je dostupná pouze pro verze clusteru HDInsight nad 3.2.4.593 a může fungovat pouze pro dokončené úlohy (pokud jste odeslali úlohu prostřednictvím WebHCat; tento grafu se zobrazí při spuštění dotazu prostřednictvím HiveServer2). Tento postup funguje pro clustery založené na Windows i Linuxu.
> 
> 

![graf výkonu hadoop hive tez](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Stav úlohy")

dotaz lepší pochopení vašeho Hive toohelp, nástroje hello přidat zobrazení operátora Hive hello v této verzi. Stačí toodouble, klikněte na vrcholy grafu úlohy hello hello a zobrazí se všechny hello operátory uvnitř vrcholu hello. Můžete také najet na konkrétní operátor tooview další podrobnosti tohoto operátoru.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Zobrazení spouštění úlohy pro Hive na úlohách Tez
Hello zobrazení spouštění úlohy pro Hive na úlohách Tez lze použít tooget strukturovaných a vizualizovaných informací pro úlohy Hive a získání dalších podrobností úlohy. Pokud jsou některé problémy s výkonem, můžete použít hello zobrazení tooget další podrobnosti. Například jak každý úkol funguje a hello podrobné informace o jednotlivých úlohách (čtení a zápis dat, plán/počáteční/koncový čas atd.), tak, aby mohli vyladit úlohu konfigurace nebo architekturu systému na základě hello vizualizována informací.

![Zobrazení spouštění úlohy v nástrojích Data Lake pro Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Zobrazení spuštění úlohy")

## <a name="run-pig-scripts"></a>Spouštění skriptů Pig
Nástroje data Lake pro Visual Studio podporují vytváření a odesílání clustery tooHDInsight skriptů Pig. Uživatelé vytvořit projekt Pig ze šablony a pak odeslat hello skriptu tooHDInsight clustery.

## <a name="feedbacks--known-issues"></a>Názory a známé problémy
* Aktuálně jsou výsledky HiveServer2 zobrazeny čistě textovým způsobem, což není ideální. Pracujeme na řešení.
* Pokud hello výsledky začínají s hodnotami NULL, momentálně se nezobrazí výsledky hello. Jsme tento problém opravili a pokud se v tento problém blokuje myslíte, že volné toodrop nám e-mail nebo kontaktujte tým podpory.
* v závislosti na nastavení místní oblast uživatele je zakódován Hello HQL skript vytvořený pomocí sady Visual Studio. Se nemusí správně spustit, pokud uživatel nahraje skript toocluster hello jako binární.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak tooconnect tooHDInsight clusterů ze sady Visual Studio, hello Data Lake (HDInsight) pomocí nástroje pro balíček a jak toorun dotazů Hive. Další informace naleznete v tématu:

* [Použití Hadoop Hive ve službě HDInsight](hdinsight-use-hive.md)
* [Začínáme používat Hadoop ve službě HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Odesílání úloh Hadoop do služby HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Analýza dat Twitteru pomocí softwaru Hadoop ve službě HDInsight](hdinsight-analyze-twitter-data.md)

