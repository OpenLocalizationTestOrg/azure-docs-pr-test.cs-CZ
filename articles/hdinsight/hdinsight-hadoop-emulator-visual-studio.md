---
title: "nástroje aaaData Lake pro Visual Studio s Hortonworks karanténě - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse hello nástroje Azure Data Lake pro Visual Studio s izolovaného prostoru Hortonworks hello spuštěné v místní virtuální počítač. Pomocí těchto nástrojů můžete vytvořit a spustit úlohy Hive a Pig na hello izolovaného prostoru a zobrazit výstup úlohy a historie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 7a3406b28d87d953e9b5be387faa86268f47b935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-data-lake-tools-for-visual-studio-with-hello-hortonworks-sandbox"></a>Použít hello nástroje Azure Data Lake pro Visual Studio s hello Hortonworks karanténě

Azure Data Lake obsahuje nástroje pro práci s obecné clusterů systému Hadoop. Tento dokument obsahuje kroky hello potřeby hello nástrojů Data Lake hello toouse s Hortonworks karanténě běží na místním virtuálním počítači.

Pomocí hello Hortonworks karanténě vám umožní toowork s Hadoop místně na vašem vývojovém prostředí. Poté, co jste si vytvořili řešení a chcete toodeploy ji ve velkém měřítku, potom můžete přesunout tooan clusteru HDInsight.

## <a name="prerequisites"></a>Požadavky

* Hello Hortonworks karanténě, běží na virtuálním počítači na vašem vývojovém prostředí. Tento dokument byl zapsán a testovány s izolovaného prostoru hello spuštěné v Oracle VirtualBox. Informace o nastavení izolovaného prostoru hello najdete v tématu hello [začít pracovat s Hortonworks karanténě hello.](hdinsight-hadoop-emulator-get-started.md) dokument.

* Visual Studio 2013, Visual Studio 2015 nebo Visual Studio 2017 (všechny edice).

* Hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 nebo novější.

* Hello [nástroje Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-hello-sandbox"></a>Konfigurace hesel pro izolovaný prostor hello

Ujistěte se, že hello Hortonworks karanténě běží. Potom postupujte podle kroků hello v hello [Začínáme v hello Hortonworks karanténě](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentu. Tyto kroky konfigurace hello heslo pro hello SSH `root` účtu a hello Ambari `admin` účtu. Tato hesla se používají při připojení izolovaného prostoru toohello ze sady Visual Studio.

## <a name="connect-hello-tools-toohello-sandbox"></a>Připojit hello nástroje toohello izolovaného prostoru

1. Otevřete Visual Studio, vyberte **zobrazení**a potom vyberte **Průzkumníka serveru**.

2. Z **Průzkumníka serveru**, klikněte pravým tlačítkem na hello **HDInsight** položku a potom vyberte **připojit tooHDInsight emulátoru**.

    ![Snímek obrazovky z Průzkumníku serveru s Connect tooHDInsight emulátoru zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. Z hello **připojit tooHDInsight emulátoru** dialogovém okně zadejte hello heslo, které jste nakonfigurovali pro Ambari.

    ![Snímek obrazovky dialogového okna, se zvýrazněnou textového pole hesla](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Vyberte **Další** toocontinue.

4. Použití hello **heslo** pole tooenter hello heslo jste nakonfigurovali pro hello `root` účtu. Ostatní pole na výchozí hodnotu hello zůstanou hello.

    ![Snímek obrazovky dialogového okna, se zvýrazněnou textového pole hesla](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Vyberte **Další** toocontinue.

5. Počkejte, než pro ověření služby toofinish hello. V některých případech může ověření nezdaří a vyzvat vás tooupdate hello konfigurace. Pokud ověření selže, vyberte **aktualizace**a počkat na hello konfigurace a ověření pro služby toofinish hello.

    ![Snímek obrazovky dialogového okna, pomocí tlačítka Aktualizovat zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > proces aktualizace Hello používá hello toomodify Ambari toowhat Hortonworks karanténě konfigurace je očekáváno hello nástrojů Data Lake pro Visual Studio.

6. Po dokončení ověření vyberte **Dokončit** toocomplete konfigurace.
    ![Snímek obrazovky dialogového okna, s tlačítkem Dokončit](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > V závislosti na rychlosti hello vývojového prostředí a hello množství paměti přidělené toohello virtuálního počítače může trvat několik minut tooconfigure a ověřit hello služby.

Po následujícím postupem, nyní máte **místní cluster HDInsight** položky v Průzkumníku serveru pod hello **HDInsight** části.

## <a name="write-a-hive-query"></a>Zápis dotazů Hive

Hive poskytuje podobné jazyku SQL dotazovací jazyk (HiveQL) pro práci s strukturovaná data. Pomocí následujících kroků toolearn jak toorun na vyžádání dotazuje na místní cluster hello hello.

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem na položku hello pro hello místní cluster, který jste přidali dříve a pak vyberte **napsat dotaz Hive**.

    ![Snímek obrazovky z Průzkumníku serveru při zápisu zvýrazněná dotaz Hive](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Zobrazí se nové okno dotazu. Zde můžete rychle zapisování a odesílání dotazů toohello místní cluster.

2. V novém okně dotazu hello zadejte hello následující příkaz:

        select count(*) from sample_08;

    toorun hello dotaz, vyberte **odeslání** hello horní části okna hello. Nechte hello ostatní hodnoty (**Batch** a název serveru) v hello výchozí hodnoty.

    ![Snímek obrazovky okna dotazu s tlačítkem odeslání hello](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Můžete také použít hello rozevírací nabídce vedle příliš**odeslání** tooselect **Upřesnit**. Rozšířené možnosti povolit další možnosti tooprovide při odesílání úlohy hello.

    ![Dialogové okno snímek obrazovky odeslat skript](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. Po odeslání hello dotazu, zobrazí se stav úlohy hello. Stav úlohy Hello zobrazí informace o hello úlohy, jako je zpracován Hadoop. **Stav úlohy** poskytuje hello stav úlohy hello. Stav Hello je pravidelně aktualizována, nebo můžete použít hello obnovit ikonu toorefresh hello stav ručně.

    ![Snímek obrazovky zobrazení úloh dialogové okno, se zvýrazněnou stav úlohy](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Po hello **stav úlohy** změní příliš**dokončeno**, zobrazí se přesměruje Acyklické grafu (DAG). Tento diagram znázorňuje hello provádění cestu, která byla dáno Tez při zpracování dotazu Hive hello. Tez je hello výchozí spouštěcí modul pro Hive na místní cluster hello.

    > [!NOTE]
    > Tez je také výchozí hello, pokud používáte clustery HDInsight se systémem Linux. Není výchozí hello na HDInsight se systémem Windows. toouse ho existuje, je nutné přidat hello řádku `set hive.execution.engine = tez;` toohello začátku dotaz Hive.

    Použití hello **výstup úlohy** odkaz tooview hello výstup. V takovém případě je 823 hello počet řádků v tabulce sample_08 hello. Diagnostické informace o úloze hello můžete zobrazit pomocí hello **protokol úlohy** a **stáhnout protokolu YARN** odkazy.

4. Můžete také spouštět úlohy Hive interaktivně změnou hello **Batch** pole příliš**interaktivní**. Potom vyberte **Execute**.

    ![Snímek obrazovky interaktivní i pro spouštění tlačítka zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Interaktivní dotaz datové proudy hello výstup protokolu vygenerovaných během zpracování toohello **HiveServer2 výstup** okno.

    > [!NOTE]
    > Hello informace je hello stejné, která je dostupná na hello **protokol úlohy** odkaz po dokončení úlohy.

    ![Snímek obrazovky výstupu protokolu](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Vytvoření projektu Hive

Můžete také vytvořit projekt, který obsahuje několik skriptů Hive. Projekt použijte, pokud mají související skripty nebo má toostore skripty v systému správy verzí.

1. V sadě Visual Studio, vyberte **soubor**, **nový**a potom **projektu**.

2. Ze seznamu hello projektů, rozbalte položku **šablony**, rozbalte položku **Azure Data Lake**a potom vyberte **HIVE (HDInsight)**. Hello seznam šablon, vyberte **Hive ukázka**. Zadejte název a umístění a potom vyberte **OK**.

    ![Zvýrazněná okno snímek obrazovky nový projekt, s Azure Data Lake, HIVE, Hive ukázka a OK](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

Hello **Hive ukázka** projekt obsahuje dva skripty **WebLogAnalysis.hql** a **SensorDataAnalysis.hql**. Můžete odeslat tyto skripty pomocí hello stejné **odeslání** tlačítko hello horní části okna hello.

## <a name="create-a-pig-project"></a>Vytvořit projekt Pig

Zatímco Hive poskytuje podobné jazyku SQL jazyk pro práci s strukturovaných dat, Pig funguje tak, že provádění transformací na data. Pig poskytuje jazyk (Pig Latin), který vám umožní toodevelop kanálu transformací. toouse Pig s hello místní cluster, postupujte takto:

1. Otevřete Visual Studio a vyberte **soubor**, **nový**a potom **projektu**. Ze seznamu hello projektů, rozbalte položku **šablony**, rozbalte položku **Azure Data Lake**a potom vyberte **Pig (HDInsight)**. Hello seznam šablon, vyberte **Pig aplikace**. Zadejte název, umístění a potom vyberte **OK**.

    ![Zvýrazněná okno snímek obrazovky nový projekt, s Azure Data Lake, Pig, Pig aplikace a OK](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. Zadejte následující text jako hello obsah hello hello **script.pig** soubor, který byl vytvořen tento projekt.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Při Pig používá jiný jazyk než Hive, jak spouštět úlohy hello je konzistentní mezi oba jazyky, prostřednictvím hello **odeslání** tlačítko. Výběr hello rozevíracího seznamu vedle položky **odeslání** zobrazí dialogové okno rozšířených pro Pig.

    ![Dialogové okno snímek obrazovky odeslat skript](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. Stav úlohy Hello a výstup se také zobrazí, hello stejné jako dotaz Hive.

    ![Snímek obrazovky dokončené úlohy Pig](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>Zobrazit úlohy

Nástroje data Lake také umožní tooeasily zobrazit informace o úlohách, které byly spuštěny v systému Hadoop. Pomocí následujících kroků toosee hello úlohy, které byly spuštěny na místní cluster hello hello.

1. Z **Průzkumníka serveru**, klikněte pravým tlačítkem na místní cluster hello a pak vyberte **zobrazit úlohy**. Zobrazí se seznam úloh, které byly odeslaná toohello clusteru.

    ![Snímek obrazovky Server Explorer, se zvýrazněnou zobrazit úlohy](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. Hello seznam úloh vyberte jednu podrobnosti úlohy tooview hello.

    ![Snímek obrazovky úlohy prohlížeče, s jedním z úlohy hello zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    zobrazené informace jsou Hello je podobné toowhat, které se zobrazí po spuštění dotazu Hive nebo Pig, včetně informací odkazy tooview hello výstup a protokolu.

3. Můžete také upravit a znovu odeslat úlohu hello z tohoto umístění.

## <a name="view-hive-databases"></a>Zobrazení Hive databáze

1. V **Průzkumníka serveru**, rozbalte položku hello **místní cluster HDInsight** položku a potom rozbalte **databáze Hive**. Hello **výchozí** a **xademo** databáze na místní cluster hello jsou zobrazeny. Rozšíření databáze zobrazuje hello tabulky v rámci hello databáze.

    ![Snímek obrazovky z Průzkumníka serveru s databází rozšířit](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Rozbalení tabulku zobrazí hello sloupce pro tuto tabulku. data hello tooquickly zobrazení, klikněte pravým tlačítkem na tabulku a vyberte **zobrazit prvních 100 řádků**.

    ![Snímek obrazovky z Průzkumníku serveru s Tabulka rozbalit a zobrazit prvních 100 řádků vybrané](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Vlastnosti databáze a tabulky

Můžete zobrazit vlastnosti hello databázi nebo tabulku. Výběr **vlastnosti** zobrazuje podrobnosti o hello vybrané položky v okně Vlastnosti hello. Například najdete hello informace zobrazené v hello následující snímek obrazovky:

![Snímek obrazovky Vlastnosti – okno](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Vytvoření tabulky

toocreate tabulku, klikněte pravým tlačítkem na databázi a potom vyberte **Create Table**.

![Snímek obrazovky Server Explorer, se zvýrazněnou Create Table](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Potom můžete vytvořit hello tabulky pomocí formuláře. Dole hello hello následující snímek obrazovky, uvidíte hello nezpracovaná HiveQL, který je použité toocreate hello tabulky.

![Snímek obrazovky formuláře hello používá toocreate tabulku](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Další kroky

* [Learning hello LAN Dobrý den Hortonworks karanténě](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop kurz – Začínáme s HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
