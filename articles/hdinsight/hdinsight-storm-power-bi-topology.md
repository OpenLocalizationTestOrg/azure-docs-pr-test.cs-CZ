---
title: aaaUse Apache Storm s Power BI - Azure HDInsight | Microsoft Docs
description: "Vytvoření sestavy Power BI pomocí dat z topologie C# spuštěná na clusteru serveru Apache Storm v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 36fe3b9c-5232-4464-8d75-95403b6da7a1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/31/2017
ms.author: larryfr
ms.openlocfilehash: 194cd8220bd60475af1d64a110e4b23ef92e1bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-toovisualize-data-from-an-apache-storm-topology"></a>Použít data toovisualize Power BI od topologie Apache Storm

Power BI umožňuje toovisually zobrazí data jako sestavy. Tento dokument obsahuje příklad toouse Apache Storm v HDInsight toogenerate data pro Power BI.

> [!NOTE]
> Hello kroky v tomto dokumentu spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio. zkompilovaný projekt Hello může být odeslaná tooa clusteru HDInsight se systémem Linux. Pouze clustery se systémem Linux vytvořené po 10/28/2016 podporu SCP.NET topologie.
>
> toouse topologie C# s clusterem systémem Linux, hello aktualizace balíčku Microsoft.SCP.Net.SDK NuGet použité ve vašem projektu tooversion 0.10.0.6 nebo vyšší. Hello verzi balíčku hello se taky musí shodovat hello hlavní verzi Storm v HDInsight nainstalována. Například Storm ve službě HDInsight verze 3.3 a 3.4 používá Storm 0.10.x, zatímco HDInsight 3.5 používá Storm 1.0.x.
>
> C# topologií v clusterech se systémem Linux musí používat rozhraní .NET 4.5 a používat Mono toorun na clusteru HDInsight hello. Většina věcí fungovat. Nicméně byste měli zkontrolovat hello [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.
>
> Verzi Javy tohoto projektu, který funguje s HDInsight se systémem Linux nebo systému Windows, najdete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="prerequisites"></a>Požadavky

* Uživatele služby Azure Active Directory s [Power BI](https://powerbi.com) přístup.
* Cluster služby HDInsight. Další informace najdete v tématu [Začínáme se Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (jeden hello následující verze)

  * Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)
  * Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * Visual Studio 2017 (všechny edice)

* Hello nástroje HDInsight pro Visual Studio: najdete v části [začít používat hello nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o informace o instalaci.

## <a name="how-it-works"></a>Jak to funguje

Tento příklad obsahuje topologie C# Storm, který náhodně generuje data protokolu Internetové informační služby (IIS). Tato data se pak zapíše tooa SQL Database a zde je použité toogenerate sestavy v Power BI.

Hello následující soubory implementace hello hlavní funkce tohoto příkladu:

* **SqlAzureBolt.cs**: zapisuje informace o vytvořil v tooSQL topologie Storm hello databáze.
* **IISLogsTable.sql**: hello Transact-SQL příkazy používají toogenerate hello databázi, která hello data jsou uložena v.

> [!WARNING]
> Před spuštěním hello topologie clusteru HDInsight vytvořte hello tabulky v databázi SQL.

## <a name="download-hello-example"></a>Stáhnout hello – ukázka

Stáhnout hello [HDInsight C# Storm Power BI příklad](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi). toodownload, buď rozvětvení nebo klonování pomocí [git](http://git-scm.com/), nebo použijte hello **Stáhnout** odkaz toodownload .zip hello archivu.

## <a name="create-a-database"></a>Vytvoření databáze

1. toocreate databázi, použijte postup hello v hello [kurz k SQL Database](../sql-database/sql-database-get-started.md) dokumentu.

2. Připojit databáze toohello pomocí následující hello kroky hello [připojit tooa SQL Database pomocí sady Visual Studio](../sql-database/sql-database-connect-query.md) dokumentu.

3. V Průzkumníku objektů, klikněte pravým tlačítkem na hello databáze a vyberte **nový dotaz**. Vložte obsah hello hello **IISLogsTable.sql** zahrnutý v hello stáhli projektu do okna dotazu hello a potom pomocí kombinace kláves Ctrl + Shift + E tooexecute hello dotazu. Měli byste obdržet zprávu, která hello příkazy byla úspěšně dokončena.

## <a name="configure-hello-sample"></a>Konfigurace ukázka hello

1. Z hello [portál Azure](https://portal.azure.com), vyberte svou databázi SQL. Z hello **Essentials** části hello SQL databáze okna, vyberte **zobrazit databázové připojovací řetězce**. Hello seznamu, které se zobrazí, zkopírujte hello **ADO.NET (ověřování systému SQL)** informace.

2. Ukázka hello otevřete v sadě Visual Studio. Z **Průzkumníku řešení**, otevřete hello **App.config** souboru a pak vyhledejte hello následující položky:

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    Nahraďte hello **TOBEFILLED ##** hodnotu s připojovací řetězec databáze hello zkopírovali v předchozím kroku hello. Nahraďte **{vaší\_uživatelské jméno}** a **{vaší\_heslo}** s hello uživatelské jméno a heslo pro databázi hello.

3. Uložte a zavřete soubory hello.

## <a name="deploy-hello-sample"></a>Nasazení ukázkové hello

1. Z **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **StormToSQL** projektu a vyberte **odeslání tooStorm v HDInsight**. Vyberte hello clusteru HDInsight z hello **Storm Cluster** dialogu rozevíracího seznamu.

   > [!NOTE]
   > To může trvat několik sekund, než hello **Storm Cluster** toopopulate rozevírací seznam s názvy serverů.
   >
   > Pokud se zobrazí výzva, zadejte hello přihlašovací údaje pro vaše předplatné Azure. Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.

2. Při odeslání hello topologie hello __topologie prohlížeč__ se zobrazí. tooview tento topologie, vyberte hello SqlAzureWriterTopology položky ze seznamu hello.

    ![Hello topologie, topologie hello vybrané](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    Můžete použít tyto informace zobrazit toosee na topologii hello nebo dvakrát klikněte na položku (například hello SqlAzureBolt) toosee informace tooa konkrétní součást v topologii hello.

3. Po hello má topologie spustili několik minut, okno dotazu SQL návratový toohello jste použili toocreate hello databáze. Nahraďte existující příkazy hello hello následující dotaz:

        select * from iislogs;

    Pomocí kombinace kláves Ctrl + Shift + E tooexecute hello dotazu a mělo by se zobrazit výsledky podobné toohello následující data:

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    Tato data byla zapsána od topologie Storm hello.

## <a name="create-a-report"></a>Vytvoření sestavy

1. Připojit toohello [konektor Azure SQL Database](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) pro Power BI. 

2. V rámci **databáze**, vyberte **získat**.

3. Vyberte **Azure SQL Database**a potom vyberte **Connect**.

    > [!NOTE]
    > Můžete být vyzváni, toocontinue Power BI Desktop toodownload hello. Pokud ano, použijte následující postup tooconnect hello:
    >
    > 1. Otevřít Power BI Desktop a vyberte __načíst Data__.
    > Vyberte 2 __Azure__a potom __Azure SQL database__.

4. Zadejte hello informace tooconnect tooyour Azure SQL Database. Tyto informace můžete najít návštěvou hello [portál Azure](https://portal.azure.com) a vyberete databázi SQL.

   > [!NOTE]
   > Interval obnovování hello a vlastní filtry lze také nastavit pomocí **povolit rozšířené možnosti** ze hello připojit dialogové okno.

5. Po připojení, zobrazí se nová datová sada s hello stejný název jako databáze hello je připojen k. Vyberte toobegin datovou sadu hello navrhování sestavy.

6. Z **pole**, rozbalte položku hello **IISLOGS** položku. toocreate sestavy, seznamy hello URI vyplývá, zaškrtněte políčko hello pro **modul URISTEM**.

    ![Vytvoření sestavy](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. V dalším kroku přetáhněte **metoda** toohello sestavy. vyplývá Hello sestavy aktualizace toolist hello a hello odpovídající metoda HTTP pro hello požadavek HTTP.

    ![Přidání dat hello – metoda](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. Z hello **vizualizace** sloupce, vyberte hello **pole** ikonu a potom vyberte hello šipka dolů vedle příliš**metoda** v hello **hodnoty**části. Vyberte toodisplay přistupoval počet jak často identifikátoru URI **počet**.

    ![Změna počtu tooa metod](./media/hdinsight-storm-power-bi-topology/count.png)

9. Potom vyberte hello **skládaný sloupcový graf** toochange, jak se zobrazují informace hello.

    ![Změna tooa skládaný graf](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. toosave hello sestavu, vyberte **Uložit** a zadejte název sestavy hello.

## <a name="stop-hello-topology"></a>Zastavení topologie hello

topologie Hello pokračuje toorun, dokud nejde zastavit ani odstranit hello Storm v clusteru HDInsight. toostop hello topologie, proveďte následující kroky hello:

1. V sadě Visual Studio vraťte toohello topologie prohlížeč a vyberte topologii hello.

2. Vyberte hello **Kill** tlačítko toostop hello topologie.

    ![Příkaz kill tlačítko na topologii hello souhrn](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste zjistili, jak toosend dat z tooSQL topologie Storm databáze, pak vizualizovat data hello pomocí Power BI. Informace o tom toowork s jinými Azure technologiemi používat Storm v HDInsight, najdete v části hello následujícím dokumentu:

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)
