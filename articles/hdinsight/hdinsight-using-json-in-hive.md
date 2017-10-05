---
title: "Analýza a tvorba dokumenty JSON proces se Hive v HDInsight | Microsoft Docs"
description: "Naučte se používat dokumentů JSON a analyzujte je pomocí Hive v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2017
ms.author: jgao
ms.openlocfilehash: bd136afebeceb0cd9c24cfc5f15601caa80a755e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Zpracovávat a analyzovat dokumenty JSON používání Hive v HDInsight

Zjistěte, jak zpracovávat a analyzovat soubory JSON používání Hive v HDInsight. V tomto kurzu se používá následující dokumentu JSON:

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Soubor můžete najít v wasb://processjson@hditutorialdata.blob.core.windows.net/. Další informace o používání úložiště objektů Azure Blob s HDInsight naleznete v tématu [použití HDFS kompatibilní úložiště Azure Blob s Hadoop v HDInsight](hdinsight-hadoop-use-blob-storage.md). Zkopírujte soubor do kontejneru výchozí clusteru.

V tomto kurzu použijete konzolu Hive.  Postup otevření konzoly nástroje Hive naleznete v tématu [používání Hive s Hadoop v HDInsight pomocí vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>Vyrovnání dokumentů JSON
Metody uvedené v následující části vyžadují dokumentu JSON na jednom řádku. Proto musí vyrovnání dokumentu JSON na řetězec. Pokud už je průmětu dokumentu JSON, můžete tento krok přeskočit a analýza JSON dat, přejděte přímo k další části.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Soubor raw JSON se nachází v  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . *StudentsRaw* Hive tabulky body k nezpracované před sloučením dokumentu JSON.

*StudentsOneLine* tabulku Hive ukládá data do HDInsight výchozí systém souborů v části */json/studenty/* cesta.

Příkaz INSERT naplní StudentOneLine tabulku s plochou data JSON.

Příkaz SELECT se vrátit pouze jeden řádek.

Toto je výstup příkazu SELECT:

![Vyrovnání dokumentu JSON.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>Analýza dokumentů JSON v Hive
Hive poskytuje tři různé mechanismy pro spouštění dotazů na dokumenty JSON:

* Použijte GET\_JSON\_OBJEKT UDF (uživatelsky definované funkce)
* Použití JSON_TUPLE UDF
* Použít vlastní SerDe
* zapsat že UDF pomocí Python nebo další jazyky, které vlastníte. V tématu [v tomto článku] [ hdinsight-python] na spuštění vlastního kódu Python s Hive.

### <a name="use-the-getjsonobject-udf"></a>Použijte GET\_JSON_OBJECT UDF
Hive poskytuje integrované UDF názvem [získat objekt json](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), které můžete provádět JSON dotazování za běhu. Tato metoda přebírá dva argumenty – název tabulky a název metody, který má sloučeném dokumentu JSON a pole JSON, který potřebujete analyzovat. Podívejme se na příklad zobrazíte fungování této UDF.

Křestní jméno a příjmení pro každý studenty

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Toto je výstup při spuštění tohoto dotazu v okně konzoly.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Existuje několik omezení get-json_object UDF.

* Protože každé pole v dotazu vyžaduje reparsing dotaz, ovlivňuje výkon.
* ZÍSKAT\_JSON_OBJECT() vrátí řetězcovou reprezentaci pole. Chcete-li převést toto pole k poli Hive, budete muset použít regulární výrazy k nahrazení hranaté závorky ' [' a ']' a také zavolat rozdělení získat pole.

To je proto doporučuje Hive wiki pomocí json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Použití JSON_TUPLE UDF
Jiné UDF poskytované Hive se nazývá [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), která provede lepší, než [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Tato metoda přebírá sadu klíčů a řetězec formátu JSON a vrátí hodnot pomocí jednu funkci řazené kolekce členů. Následující dotaz vrátí student id a úrovni z dokumentu JSON:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Výstup skriptu v konzole nástroje Hive:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_používá řazené kolekce členů [pomoci odhalit laterální zobrazení](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntaxi Hive, která umožňuje json\_řazené kolekce členů a vytvořte virtuální tabulku použitím funkce UDT na každém řádku původní tabulky.  Komplexní JSONs být příliš nepraktické kvůli opakované použití LATERÁLNÍ zobrazení. Kromě toho JSON_TUPLE nemůže zpracovat vnořené JSONs.

### <a name="use-custom-serde"></a>Použít vlastní SerDe
SerDe je nejlepší volbou pro analýzu vnořených dokumentů JSON, ale umožňuje vám umožňuje definovat schématu JSON a použití schématu analyzovat dokumenty. V tomto kurzu použijete, jednu oblíbenější SerDe, která byla vytvořena pomocí [Roberto Congiu](https://github.com/rcongiu).

**Použití vlastní SerDe:**

1. Nainstalujte [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Vyberte verzi Windows X64 sadu JDK, pokud se chystáte použít nasazení systému Windows služby HDInsight
   
   > [!WARNING]
   > JDK 1.8 nefunguje s Tento SerDe.
   > 
   > 
   
    Po dokončení instalace, přidejte novou proměnnou prostředí uživatele:
   
   1. Otevřete **zobrazení Upřesnit nastavení systému** na obrazovce Windows.
   2. Klikněte na tlačítko **proměnné prostředí**.  
   3. Přidejte nový **JAVA_HOME** proměnnou prostředí ukazovat na **C:\Program Files\Java\jdk1.7.0_55** nebo všude, kde je nainstalována vaší JDK.
      
      ![Nastavení konfigurace správné hodnoty pro JDK][image-hdi-hivejson-jdk]
2. Nainstalujte [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)
   
    Přidat do složky bin cestu přechodem na ovládací prvek Panel--> Upravit proměnné systému pro účet proměnných prostředí. Následující snímek obrazovky ukazuje, jak to udělat.
   
    ![Nastavení Maven][image-hdi-hivejson-maven]
3. Klonování projekt z [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) webu github. To provedete kliknutím na tlačítko "Stáhnout Zip", jak je znázorněno na následujícím snímku obrazovky.
   
    ![Klonování projektu][image-hdi-hivejson-serde]

4: přejděte do složky, kde jste si stáhli tento balíček a potom na typ "balíček mvn". To by měl vytvořit potřebné jar soubory, které pak můžete zkopírovat přes do clusteru.

5: přejděte do cílové složky v kořenové složce, kam jste stáhli balíček. Nahrajte soubor json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar do hlavního uzlu clusteru. Je obvykle umístíte ve složce binární hive: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin nebo něco podobného.

6: hive řádek, zadejte "Přidat jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Vzhledem k tomu, že v mém případě jar nachází ve složce C:\apps\dist\hive-0.13.x\bin, bylo možné přímo přidat jar s názvem, jak je znázorněno:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Přidání JAR do projektu][image-hdi-hivejson-addjar]

Teď můžete je připravený k použití SerDe ke spouštění dotazů na dokument JSON.

Následující příkaz vytvoří tabulku se definované schéma:

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

K zobrazení seznamu křestní jméno a příjmení student

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Zde je výsledek z konzoly nástroje Hive.

![Dotaz SerDe 1][image-hdi-hivejson-serde_query1]

Chcete-li vypočítat součet skóre dokumentu JSON

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Předchozí dotaz používá [laterální zobrazení Rozbalit](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF rozbalte pole skóre, takže může být sčítají.

Toto je výstup z konzoly nástroje Hive.

![Dotaz SerDe 2][image-hdi-hivejson-serde_query2]

K vyhledání, která předměty dané student má vypočítat skóre více než 80 body:

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

Předchozí dotaz vrátí pole Hive na rozdíl od get\_json\_objekt, který vrátí řetězec.

![Dotaz SerDe 3][image-hdi-hivejson-serde_query3]

Pokud chcete skil nesprávný formát JSON, pak jak je vysvětleno v [stránce wikiwebu](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) z této SerDe můžete dosáhnout, a to zadáním následujícího kódu:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a>Souhrn
Na závěr typ operátor JSON v Hive, který zvolíte, závisí na váš scénář. Pokud máte jednoduchý dokumentu JSON a máte pouze jedno pole pro vyhledávání na – můžete použít get Hive UDF\_json\_objektu. Pokud máte více než jeden klíč k vyhledání, můžete použít json_tuple. Pokud máte vnořené dokumentu, měli byste použít JSON SerDe.

## <a name="next-steps"></a>Další kroky

Další související články naleznete v části

* [Použití Hive a HiveQL s Hadoop v HDInsight k analýze ukázkového souboru Apache log4j](hdinsight-use-hive.md)
* [Analýza dat zpoždění letu pomocí Hive v HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Analýza dat Twitteru pomocí Hive v HDInsight](hdinsight-analyze-twitter-data.md)
* [Spustit úlohu Hadoop pomocí Azure Cosmos DB a HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
