---
title: "aaaUse Tez uživatelského rozhraní s HDInsight se systémem Windows - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello uživatelského rozhraní Tez toodebug Tez úlohy na HDInsight HDInsight se systémem Windows."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 7ae21242ee1f8dc34a8501bed1ca995480885540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-tez-ui-toodebug-tez-jobs-on-windows-based-hdinsight"></a>Použít na HDInsight se systémem Windows hello úlohách Tez toodebug Tez uživatelského rozhraní
Hello Tez uživatelského rozhraní je webová stránka, kterou lze použít toounderstand a ladění úlohy, které používají Tez jako modul provádění hello v clusterech HDInsight se systémem Windows. Hello Tez uživatelského rozhraní umožňuje toovisualize hello úlohy graf připojených položek, přejdete na každou položku a načíst informace o protokolování a statistiky.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Požadavky
* Cluster HDInsight se systémem Windows. Pokyny týkající se vytvoření nového clusteru, najdete v části [začněte používat HDInsight se systémem Windows](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > Hello Tez uživatelského rozhraní je dostupná pouze na clustery HDInsight se systémem Windows, které jsou vytvořené po 8. únoru 2016.
  >
  >
* Klient vzdálené plochy se systémem Windows.

## <a name="understanding-tez"></a>Principy Tez
Tez je rozšiřitelná architektura pro zpracování dat v Hadoop, která poskytuje vyšší rychlosti než tradiční zpracování prostředí MapReduce. Pro clustery HDInsight se systémem Windows je volitelné motoru, kterou můžete povolit pro Hive pomocí hello jako součást dotazu Hive následující příkaz:

    set hive.execution.engine=tez;

Odeslaná tooTez po pracovní vytvoří směrované Acyklické grafu (DAG) popisující hello pořadí provádění akcí hello vyžadovanou hello úlohy. Jednotlivé akce se nazývají vrcholy a spouštět úsek hello celkové úlohy. skutečné provádění Hello popsaného Vrchol pracovní hello je volána úloha a mohou být distribuovány mezi několika uzly v clusteru hello.

### <a name="understanding-hello-tez-ui"></a>Principy hello Tez uživatelského rozhraní
Hello Tez uživatelského rozhraní je na webové stránce poskytuje informace o procesech, které jsou spuštěné, nebo byl již spuštěn pomocí Tez. Umožňuje tooview hello DAG generované Tez, jak je rozdělené mezi clustery, jako je množství paměti používané úlohy a vrcholy a informace o chybě čítače. Vám může nabídnout užitečné informace v hello následující scénáře:

* Monitorování dlouho běžící procesy, zobrazení hello průběh mapy a snížit úlohy.
* Analýza historické údaje o úspěšném nebo neúspěšném procesy toolearn, jak lze zlepšit zpracování nebo proč se nezdařilo.

## <a name="generate-a-dag"></a>Generovat DAG
Hello uživatelského rozhraní Tez bude obsahovat pouze data, pokud úlohu, která používá hello Tez modul běží v současné době nebo byl byla spuštěna v posledních hello. Jednoduché dotazů Hive obvykle lze přeložit bez použití Tez, ale složitější dotazy, které provádějí filtrování, seskupování, řazení, atd. spojení se obvykle vyžadují Tez.

Pomocí následujících kroků toorun dotaz Hive, který provede pomocí Tez hello.

1. Ve webovém prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je hello název clusteru HDInsight.
2. Z nabídky hello hello horní části stránky hello vyberte hello **Hive Editor**. Zobrazí se stránka s hello následující příklad dotazu.

        Select * from hivesampletable

    Vymazat hello příklad dotazu a nahraďte ji metodou následující hello.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Vyberte hello **odeslání** tlačítko. Hello **úlohy relace** oddíl hello dolní části stránky hello se zobrazí stav hello hello dotazu. Jednou hello změny stavu příliš**dokončeno**, vyberte hello **zobrazit podrobnosti** odkaz tooview hello výsledky. Hello **výstup úlohy** by měla být podobné toohello následující:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-hello-tez-ui"></a>Použití hello Tez uživatelského rozhraní
> [!NOTE]
> Hello Tez uživatelského rozhraní je dostupná pouze z plochy hello hello head uzlů clusteru, takže je nutné použít vzdálené plochy tooconnect toohello hlavních uzlech.
>
>

1. Z hello [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight. Hello horní části okna hello HDInsight, vyberte hello **vzdálené plochy** ikonu. Bude se zobrazovat vzdálené plochy okno hello

    ![Ikona vzdálené plochy](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. V okně hello vzdálené plochy, vyberte **Connect** hlavního uzlu clusteru tooconnect toohello. Pokud budete vyzváni, použijte hello clusteru připojení ke vzdálené ploše uživatelské jméno a heslo tooauthenticate hello.

    ![Ikona připojení vzdálené plochy](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Pokud jste nepovolili připojení vzdálené plochy, zadejte uživatelské jméno, heslo a datum vypršení platnosti a pak vyberte **povolit** tooenable vzdálené plochy. Jakmile je povoleno, použijte hello předchozí kroky tooconnect.
   >
   >
3. Po připojení otevřete Internet Explorer na hello vzdálené plochy, vyberte hello ozubené kolečko ikonu v hello pravém horním rohu stránky hello prohlížeče a potom vyberte **nastavení kompatibilního zobrazení**.
4. Hello dolní části **nastavení kompatibilního zobrazení**, zrušte hello zaškrtávací políčko pro **zobrazit intranetové servery v kompatibilního zobrazení** a **použití Microsoft kompatibility seznamy**, a pak vyberte **Zavřít**.
5. V aplikaci Internet Explorer procházet toohttp://headnodehost:8188/tezui / #/. Bude se zobrazovat hello Tez uživatelského rozhraní

    ![Tez uživatelského rozhraní](./media/hdinsight-debug-tez-ui/tezui.png)

    Až se načte hello Tez uživatelského rozhraní, zobrazí se, že seznam DAG, které jsou aktuálně spuštěné, nebo byla spuštěna v clusteru hello. Výchozí zobrazení Hello zahrnuje hello Dag název, Id, odesílatel, stav, čas spuštění, čas ukončení, doba trvání, ID aplikace a fronty. Pomocí ikony ozubené kolečko hello na hello napravo od stránku hello lze přidat více sloupců.

    Pokud máte pouze jednu položku, bude pro hello dotaz, který jste spustili v předchozí části hello. Pokud máte více položek, můžete vyhledat tak, že zadáte kritéria hledání v polích hello výše hello DAG a pak stiskněte tlačítko **Enter**.
6. Vyberte hello **Dag název** u hello nejnovější DAG položky. Tato akce zobrazí informace o hello DAG, jakož i hello možnost toodownload zip soubory JSON, které obsahují informace o hello DAG.

    ![Podrobnosti o DAG](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Výše hello **DAG podrobnosti** je několik odkazů, které se dají použít toodisplay informace o hello DAG.

   * **Čítače DAG** zobrazí informace o čítačích pro tento DAG.
   * **Grafické zobrazení** zobrazuje grafické reprezentace této DAG.
   * **Všechny vrcholy** zobrazí seznam hello vrcholy v této DAG.
   * **Všechny úlohy** zobrazí seznam hello úloh pro všechny vrcholy v této DAG.
   * **Všechny TaskAttempts** zobrazí informace o hello pokusů o zadání toorun úlohy pro tuto DAG.

     > [!NOTE]
     > Pokud se posunete zobrazení sloupce hello vrcholy, úlohy a TaskAttempts, Všimněte si, že existují odkazy tooview **čítače** a **zobrazení nebo stažení protokolů** pro každý řádek.
     >
     >

     Pokud došlo k selhání s úlohou hello, hello podrobnosti DAG se zobrazí stav se nezdařilo, spolu s odkazy tooinformation o neúspěšné úloze hello. Diagnostické informace se zobrazí pod hello DAG podrobnosti.
8. Vyberte **grafické zobrazení**. Zobrazí se grafické reprezentace hello DAG. Každý vrchol hello zobrazení toodisplay informace o tom můžete umístit hello myši.

    ![Grafické zobrazení](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Kliknutím na vrchol načte hello **vrchol podrobnosti** pro tuto položku. Klikněte na hello **mapy 1** vrchol toodisplay podrobnosti pro tuto položku. Vyberte **potvrdit** tooconfirm hello navigace.

    ![Vrchol podrobnosti](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Upozorňujeme ale, nyní se odkazy v horní části hello hello stránky, které jsou související toovertices a úloh.

    > [!NOTE]
    > Tato stránka také můžete dospět tak, že přejdete příliš**DAG podrobnosti**, vyberete **vrchol podrobnosti**a potom vyberete hello **mapy 1** vrchol.
    >
    >

    * **Vrchol čítače** zobrazí čítače informace pro tento vrchol.
    * **Úlohy** zobrazuje úlohy, které pro tento vrchol.
    * **Úloha pokusy o** zobrazí informace o úlohách toorun pokusů pro tento vrchol.
    * **Zdroje & jímky** zobrazí zdroje dat a pro tento vrchol jímky.

      > [!NOTE]
      > S předchozí nabídce hello při posouvání hello sloupec zobrazení pro úlohy, propojí pokusy o úloh a zdroje & Sinks__ toodisplay toomore informace pro každou položku.
      >
      >
11. Vyberte **úlohy**, a pak vyberte hello položku s názvem **00_000000**. Bude se zobrazovat **podrobnosti úlohy** pro tuto úlohu. Na této obrazovce můžete zobrazit **úloh čítače** a **úloh pokusy**.

    ![Podrobnosti úlohy](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili, jak zobrazit toouse hello Tez, další informace o [pomocí Hive v HDInsight](hdinsight-use-hive.md).

Další podrobné technické informace o Tez naleznete v tématu hello [Tez stránku v Hortonworks](http://hortonworks.com/hadoop/tez/).
