---
title: "aaaCorrelate události v čase pomocí nástrojů Storm a HBase v HDInsight"
description: "Zjistěte, jak toocorrelate události, které přicházejí v různou dobu pomocí nástrojů Storm a HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: f915eef77bbda5b02bfd02ad0b5a4923ff2f4f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a>Korelovat události, které přicházejí v různou dobu pomocí nástrojů Storm a HBase

Pomocí úložiště trvalé dat s Apache Storm mohou korelovat datové položky, které přicházejí v různých časech. Například propojení události přihlášení a odhlášení pro toocalculate relace uživatele, jak dlouho hello relace už bylo.

V tomto dokumentu, zjistíte, jak toocreate základní topologie C# Storm, který sleduje události přihlášení a odhlášení pro relace uživatelů a vypočítá dobu trvání relace hello hello. topologie Hello používá HBase jako úložiště dat trvalé. HBase můžete taky tooperform batch dotazy na další statistiky tooproduce hello historická data. Například kolik uživatelských relací byly spuštěna, nebo skončila v konkrétním časovém období.

## <a name="prerequisites"></a>Požadavky

* Visual Studio a hello nástroje HDInsight pro Visual Studio. Další informace najdete v tématu [začít používat hello nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Apache Storm v HDInsight clusteru (založené na Windows).

  > [!WARNING]
  > Když SCP.NET topologie jsou podporované u clusterů Storm se systémem Linux vytvořené po 10/28/2016, hello HBase SDK pro .NET balíčku k dispozici od 10/28/2016 nebude správně fungovat na HDInsight se systémem Linux

* Apache HBase v clusteru HDInsight (Linux nebo na základě Windows).

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Java](https://java.com) 1.7 nebo vyšší na vašem vývojovém prostředí. Java je použité toopackage hello topologie, pokud je odeslaná toohello clusteru HDInsight.

  * Hello **JAVA_HOME** prostředí proměnné musí bodu toohello adresář, který obsahuje Java.
  * Hello **%JAVA_HOME%/bin** adresář se musí nacházet v cestě hello

## <a name="architecture"></a>Architektura

![Diagram toku dat hello prostřednictvím hello topologie](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

Obecný identifikátor korelace událostí vyžaduje pro zdroj události hello. For example ID uživatele, ID relace nebo další část dat, je (a) jedinečný a b) zahrnuli do všech dat odesílaných tooStorm. Tento příklad používá toorepresent hodnota GUID ID relace.

V tomto příkladu se skládá ze dvou clustery HDInsight:

* HBase: úložiště dat pro historických dat
* Storm: používá tooingest příchozích dat

Hello data je náhodně generované topologie Storm hello a se skládá z hello následující položky:

* ID relace: identifikátor GUID, který jednoznačně identifikuje každou relaci
* Událost: počáteční nebo koncové události. V tomto příkladu počáteční dochází vždy konce
* Čas: čas hello hello událostí.

Tato data jsou zpracovány a uložená v HBase.

### <a name="storm-topology"></a>Topologie Storm

Když se spustí relaci, **spustit** událost přijme hello topologie a protokolovat tooHBase. Když **END** události, hello topologie načte hello **spustit** událostí a vypočítá hello čas mezi událostmi hello dva. To **doba trvání** hodnoty jsou pak uloženy v HBase společně s hello **END** informací o události.

> [!IMPORTANT]
> Zatímco tato topologie znázorňuje hello základní vzor, produkční řešení potřebovat tootake návrhu pro hello následující scénáře:
>
> * Příchozích mimo pořadí událostí
> * Duplicitní události
> * Vyřazené události

Ukázková topologie Hello se skládá z hello následující součásti:

* Session.cs: simuluje vytvořením ID náhodných relace, počáteční čas a jak dlouho hello relace vydrží uživatelské relace.

* Spout.cs: Vytvoří 100 relací, vysílá události spuštění, čeká hello náhodných vypršení časového limitu pro každou relaci a potom vydá KONCOVÁ událost. Potom recykluje skončila relací toogenerate nové.

* HBaseLookupBolt.cs: používá toolook ID relace hello relace informací z HBase. Při zpracování KONCOVÁ událost najde hello odpovídající počáteční událost a vypočítá dobu trvání relace hello hello.

* HBaseBolt.cs: Ukládá informace o HBase.

* TypeHelper.cs: Pomáhá s převod typů při čtení / zápis tooHBase.

### <a name="hbase-schema"></a>Schéma HBase

V HBase hello data jsou uložena v tabulce s hello následující schéma nebo nastavení:

* Klíč řádku: hello relace ID slouží jako klíč hello řádků v této tabulce.

* Rodin sloupců: název rodiny hello je 'CR'. Jsou uložené v této rodině sloupce:

  * událost: počáteční nebo KONCOVÝ.

  * čas: hello čas v milisekundách, po jejímž hello událostí došlo k chybě.

  * Doba trvání: hello délka mezi počáteční a koncové události.

* VERZE: hello 'CR' rodiny nastavena tooretain 5 verzích každý řádek.

  > [!NOTE]
  > Verze jsou protokolu předchozí hodnot uložených pro klíč specifickým řádkem. Ve výchozím nastavení HBase pouze vrací hodnotu hello hello nejnovější verze řádku. V takovém případě hello stejný řádek se používá pro každou verzi řádek je identifikována hodnotu časového razítka hello všechny události (START a END.). Verze poskytují Historický přehled události evidované pro konkrétní ID.

## <a name="download-hello-project"></a>Stáhněte si projekt hello

Ukázkový projekt Hello si můžete stáhnout z [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).

Tento soubor ke stažení obsahuje hello následující projektů C#:

* CorrelationTopology: Topologie C# Storm, který náhodně vysílá počáteční a koncové události pro relace uživatelů. Každý relace trvá od 1 do 5 minut.

* SessionInfo: C# konzolovou aplikaci, která vytvoří tabulku HBase hello a poskytuje příklad dotazy tooreturn informace o datech uložených relací.

## <a name="create-hello-table"></a>Vytvoření tabulky hello

1. Otevřete hello **SessionInfo** projektu v sadě Visual Studio.

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **SessionInfo** projektu a vyberte **vlastnosti**.

    ![Snímek obrazovky nabídky s vlastnostmi vybrané](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. Vyberte **nastavení**, pak sada hello následující hodnoty:

   * HBaseClusterURL: hello URL tooyour HBase clusteru. Například https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: hello správce/HTTP uživatelský účet pro váš cluster

   * HBaseClusterPassword: hello heslo pro uživatelský účet správce nebo HTTP hello

   * HBaseTableName: název hello toouse tabulky hello tento příklad

   * HBaseTableColumnFamily: název řady sloupce hello

     ![Obrázek dialogového okna nastavení](./media/hdinsight-storm-correlation-topology/settings.png)

4. Spuštění hello řešení. Po zobrazení výzvy vyberte hello "c" klíče toocreate hello tabulku v clusteru HBase.

## <a name="build-and-deploy-hello-storm-topology"></a>Vytváření a nasazování topologie Storm hello

1. Otevřete hello **CorrelationTopology** řešení v sadě Visual Studio.

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **CorrelationTopology** projektu a vyberte možnost Vlastnosti.

3. V okně vlastností hello vyberte **nastavení** a zadejte hodnoty konfigurace hello pro tento projekt. Hello první 5 jsou hello používá stejné hodnoty hello **SessionInfo** projektu:

   * HBaseClusterURL: hello URL tooyour HBase clusteru. Například https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: hello správce/HTTP uživatelský účet pro váš cluster.

   * HBaseClusterPassword: hello heslo pro uživatelský účet správce nebo HTTP hello.

   * HBaseTableName: název hello hello toouse tabulky se v tomto příkladu. Tato hodnota je hello stejný název tabulky v rámci projektu SessionInfo hello.

   * HBaseTableColumnFamily: název řady sloupce hello. Tato hodnota je hello stejný název rodiny sloupec v rámci projektu SessionInfo hello.

   > [!IMPORTANT]
   > Neměňte hello HBaseTableColumnNames, jako výchozí hodnoty hello jsou názvy hello používá **SessionInfo** tooretrieve data.

4. Uložit hello vlastnosti a potom sestavení projektu hello.

5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**. Pokud se zobrazí výzva, zadejte přihlašovací údaje hello vašeho předplatného Azure.

   ![Obrázek hello odeslání toostorm položky nabídky](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. V hello **odeslání topologie** dialogové okno, chcete toodeploy Tato topologie do clusteru Storm vyberte hello.

   > [!NOTE]
   > Hello poprvé odeslání topologii, může trvat několik sekund tooretrieve hello název clusterům HDInsight.

7. Jakmile hello topologie už nahrávat a Odeslaná toohello clusteru, hello **zobrazení topologie Storm** otevře a zobrazí hello spuštěná topologie. toorefresh hello data, vyberte hello **CorrelationTopology** a použijte tlačítko Aktualizovat hello v hello top pravé části stránky hello.

   ![Obrázek zobrazení topologie hello](./media/hdinsight-storm-correlation-topology/topologyview.png)

   Po zahájení generování dat hello topologie hello hodnota v hello **Emitted** sloupec přírůstcích.

   > [!NOTE]
   > Pokud hello **zobrazení topologie Storm** neobsahuje automaticky otevře, použijte hello následující tooopen kroky:
   >
   > 1. V **Průzkumníku řešení**, rozbalte položku **Azure**a potom rozbalte **HDInsight**.
   > 2. Klikněte pravým tlačítkem na cluster Storm hello, který hello topologie je spuštěný a pak vyberte **topologie Storm zobrazení**

## <a name="query-hello-data"></a>Dotaz na data hello

Jakmile data byla vygenerované, použijte následující kroky tooquery hello data hello.

1. Vrátí toohello **SessionInfo** projektu. Pokud není spuštěná, spusťte novou instanci.

2. Po zobrazení výzvy vyberte **s** toosearch pro spuštění událost. Jsou výzvami tooenter spuštění a ukončení toodefine čas časové rozmezí - se vrátí jenom události mezi těmito časy.

    Použijte následující hello formátu při zadávání hello spuštění a ukončení: hh: mm a "" m"nebo 'pm'. Například 23:20:00.

    tooreturn protokolují události, použijte čas zahájení z před hello nasazená topologie Storm a koncový čas z nyní. Hello vracených dat obsahuje položky podobné toohello následující text:

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

Hledání koncové události funguje hello stejné jako počáteční události. Ale koncové události jsou generovány, náhodně rozmezí 1 až 5 minut po události spuštění hello. Tootry může mít několik čas rozsahy toofind hello koncové události. KONCOVÉ události obsahovat také hello trvání relace hello - hello rozdíl mezi čas události hello počáteční a KONCOVÝ čas události. Tady je příklad dat pro koncové události:

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> Hello časové hodnoty, které zadáte, jsou v místním čase, je čas hello vrácená z dotazu hello ve standardu UTC.

## <a name="stop-hello-topology"></a>Zastavení topologie hello

Pokud jste připravené toostop hello topologie, vrátí toohello **CorrelationTopology** projektu v sadě Visual Studio. V hello **zobrazení topologie Storm**, vyberte hello topologie a potom pomocí hello **Kill** tlačítka v horní části hello hello topologie zobrazení.

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

Další příklady Storm naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).
