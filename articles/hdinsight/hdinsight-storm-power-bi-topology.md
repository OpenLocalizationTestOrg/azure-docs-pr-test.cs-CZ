---
title: "Použít Apache Storm s Power BI - Azure HDInsight | Microsoft Docs"
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
ms.openlocfilehash: 36487c0c34e5a4bb955bbc15c8c96b9e838aeb44
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-power-bi-to-visualize-data-from-an-apache-storm-topology"></a>Pomocí Power BI vizualizovat data od topologie Apache Storm

Power BI umožňuje vizuálně zobrazit data jako sestavy. Tento dokument obsahuje příklady použití Apache Storm v HDInsight pro generování dat pro Power BI.

> [!NOTE]
> Kroky v tomto dokumentu se spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio. Zkompilovaný projekt můžete odeslat ke clusteru HDInsight se systémem Linux. Pouze clustery se systémem Linux vytvořené po 10/28/2016 podporu SCP.NET topologie.
>
> Pokud chcete používat topologie C# s clusterem se systémem Linux, aktualizujte balíček Microsoft.SCP.Net.SDK NuGet používaný v projektu na verzi 0.10.0.6 a vyšší. Verze balíčku se zároveň musí shodovat s hlavní verzí Stormu nainstalovanou ve službě HDInsight. Například Storm ve službě HDInsight verze 3.3 a 3.4 používá Storm 0.10.x, zatímco HDInsight 3.5 používá Storm 1.0.x.
>
> Topologie jazyka C# v clusterech založených na Linuxu musí používat technologii .NET 4.5. a pro spuštění v clusteru HDInsight musí používat Mono. Většina věcí fungovat. Nicméně byste měli zkontrolovat [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.
>
> Verzi Javy tohoto projektu, který funguje s HDInsight se systémem Linux nebo systému Windows, najdete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="prerequisites"></a>Požadavky

* Uživatele služby Azure Active Directory s [Power BI](https://powerbi.com) přístup.
* Cluster služby HDInsight. Další informace najdete v tématu [Začínáme se Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

  > [!IMPORTANT]
  > HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (jedna z následujících verzí)

  * Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)
  * Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * Visual Studio 2017 (všechny edice)

* Nástroje HDInsight pro Visual Studio: najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o informace o instalaci.

## <a name="how-it-works"></a>Jak to funguje

Tento příklad obsahuje topologie C# Storm, který náhodně generuje data protokolu Internetové informační služby (IIS). Tato data pak zapsána do databáze SQL a z ní se používá ke generování sestav ve službě Power BI.

Následující soubory implementovat hlavní funkce tohoto příkladu:

* **SqlAzureBolt.cs**: zapisuje informace o vytvořil v topologii Storm k databázi SQL.
* **IISLogsTable.sql**: příkazy jazyka Transact-SQL sloužící ke generování data uložená v databázi.

> [!WARNING]
> Vytvoření tabulky v databázi SQL před zahájením topologii v clusteru HDInsight.

## <a name="download-the-example"></a>Stáhněte si příklad

Stažení [HDInsight C# Storm Power BI příklad](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi). Se stáhne, buď rozvětvení nebo klonování pomocí [git](http://git-scm.com/), nebo použijte **Stáhnout** odkaz ke stažení .zip archivu.

## <a name="create-a-database"></a>Vytvoření databáze

1. Chcete-li vytvořit databázi, použijte postup v [kurz k SQL Database](../sql-database/sql-database-get-started.md) dokumentu.

2. Připojit k databázi pomocí kroků v [připojit k SQL Database pomocí sady Visual Studio](../sql-database/sql-database-connect-query.md) dokumentu.

3. V Průzkumníku objektů, klikněte pravým tlačítkem na databázi a vyberte **nový dotaz**. Umožňuje vložit obsah **IISLogsTable.sql** souboru součástí staženého projektu do okna dotazu a potom pomocí kombinace kláves Ctrl + Shift + E provést dotaz. Měli byste obdržet zprávu, že tyto příkazy úspěšně provedly.

## <a name="configure-the-sample"></a>Ukázka konfigurace

1. Z [portál Azure](https://portal.azure.com), vyberte svou databázi SQL. Z **Essentials** část SQL databáze okně vyberte **zobrazit databázové připojovací řetězce**. Ze seznamu, který se zobrazí, zkopírujte **ADO.NET (ověřování systému SQL)** informace.

2. Otevřete ukázku v sadě Visual Studio. Z **Průzkumníku řešení**, otevřete **App.config** souboru a pak vyhledejte následující položky:

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    Nahraďte **TOBEFILLED ##** hodnotu s připojovací řetězec databáze zkopírovali v předchozím kroku. Nahraďte **{vaší\_uživatelské jméno}** a **{vaší\_heslo}** pomocí uživatelského jména a hesla pro databázi.

3. Uložte a zavřete soubory.

## <a name="deploy-the-sample"></a>Ukázka nasazení

1. Z **Průzkumníku řešení**, klikněte pravým tlačítkem myši **StormToSQL** projektu a vyberte **odeslání do Storm v HDInsight**. Vyberte cluster HDInsight z **Storm Cluster** dialogu rozevíracího seznamu.

   > [!NOTE]
   > To může trvat několik sekund **Storm Cluster** rozevírací k naplnění s názvy serverů.
   >
   > Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure. Pokud máte více než jedno předplatné, přihlaste se k ta, která obsahuje váš cluster Storm v HDInsight.

2. Při odeslání topologii __topologie prohlížeč__ se zobrazí. Pokud chcete zobrazit tato topologie, vyberte položku SqlAzureWriterTopology ze seznamu.

    ![Topologie, topologie vybrané](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    Můžete v tomto zobrazení můžete zobrazit informace o topologii nebo dvakrát klikněte na položku (například SqlAzureBolt) Chcete-li zobrazit informace specifické pro komponentu v topologii.

3. Poté, co má topologii spustili několik minut, vraťte se do okna dotazu SQL, které jste použili k vytvoření databáze. Nahraďte existující příkazy následující dotaz:

        select * from iislogs;

    Pomocí kombinace kláves Ctrl + Shift + E spustit dotaz a by měl zobrazit výsledky podobná následující data:

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    Tato data byla zapsána od topologie Storm.

## <a name="create-a-report"></a>Vytvoření sestavy

1. Připojení k [konektor Azure SQL Database](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) pro Power BI. 

2. V rámci **databáze**, vyberte **získat**.

3. Vyberte **Azure SQL Database**a potom vyberte **Connect**.

    > [!NOTE]
    > Můžete být vyzváni, abyste stáhnout Power BI Desktop pokračujte. Pokud ano, pro připojení použijte následující kroky:
    >
    > 1. Otevřít Power BI Desktop a vyberte __načíst Data__.
    > Vyberte 2 __Azure__a potom __Azure SQL database__.

4. Zadejte informace pro připojení k vaší databázi SQL Azure. Tyto informace můžete najít navštivte stránky [portál Azure](https://portal.azure.com) a vyberete databázi SQL.

   > [!NOTE]
   > Interval obnovování a vlastní filtry lze také nastavit pomocí **povolit rozšířené možnosti** z dialogového okna připojení.

5. Po připojení, zobrazí se nová datová sada se stejným názvem jako databázi, ke kterému jste připojení. Vyberte datovou sadu zahájíte navrhování sestavy.

6. Z **pole**, rozbalte **IISLOGS** položku. Pokud chcete vytvořit sestavu obsahující seznam stonky identifikátor URI, zaškrtněte políčko **modul URISTEM**.

    ![Vytvoření sestavy](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. V dalším kroku přetáhněte **metoda** do sestavy. Sestavy aktualizací k zobrazení seznamu stonky a odpovídající metodu HTTP použitou pro požadavek HTTP.

    ![Přidání dat – metoda](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. Z **vizualizace** sloupce, vyberte **pole** ikonu a potom vyberte na šipku dolů vedle **metoda** v **hodnoty** části. Chcete-li zobrazit počet kolikrát přistupoval identifikátor URI, vyberte **počet**.

    ![Změna na počet metody](./media/hdinsight-storm-power-bi-topology/count.png)

9. Potom vyberte **skládaný sloupcový graf** změnit, jak se zobrazují informace.

    ![Změna na skládaný graf](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. Chcete-li uložit sestavu, vyberte **Uložit** a zadejte název pro sestavu.

## <a name="stop-the-topology"></a>Zastavení topologie

Topologie i nadále spustit, dokud nejde zastavit ani odstranit Storm v clusteru HDInsight. K zastavení topologie, proveďte následující kroky:

1. V sadě Visual Studio vraťte do prohlížeče topologie a vyberte topologii.

2. Vyberte **Kill** tlačítko zastavení topologie.

    ![Příkaz kill tlačítko na topologii souhrn](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste zjistili, jak odesílat data do databáze SQL z topologie Storm a pak vizualizovat data pomocí Power BI. Informace o tom, jak pracovat s jinými Azure technologiemi používat Storm v HDInsight najdete v následujícím dokumentu:

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)
