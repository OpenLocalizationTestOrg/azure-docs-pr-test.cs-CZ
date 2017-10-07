---
title: "aaaAnalyze a proces JSON dokumentů s Hive v HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse JSON dokumentů a analyzovat jejich používání Hive v HDInsight."
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
ms.openlocfilehash: b4b20172e8553f91a446615dc52f2ea2ef24cd04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Zpracovávat a analyzovat dokumenty JSON používání Hive v HDInsight

Zjistěte, jak tooprocess a analyzovat soubory JSON používání Hive v HDInsight. Následující dokument JSON Hello se používá v kurzu hello:

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

Hello soubor se nachází v wasb://processjson@hditutorialdata.blob.core.windows.net/. Další informace o používání úložiště objektů Azure Blob s HDInsight naleznete v tématu [použití HDFS kompatibilní úložiště Azure Blob s Hadoop v HDInsight](hdinsight-hadoop-use-blob-storage.md). Můžete zkopírovat hello souboru toohello výchozí kontejner clusteru.

V tomto kurzu použijete konzolu hello Hive.  Postup otevření konzoly hello Hive naleznete v tématu [používání Hive s Hadoop v HDInsight pomocí vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>Vyrovnání dokumentů JSON
Hello metody uvedené v další části hello vyžadují hello dokumentu JSON na jednom řádku. Proto musí vyrovnání hello JSON dokumentu tooa řetězec. Pokud už je průmětu dokumentu JSON, můžete tento krok přeskočit a přímých toohello další části, přejděte na data analýza JSON.

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

Hello soubor raw JSON je umístěn v  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . Hello *StudentsRaw* dokumentu JSON nezpracovaná před sloučením toohello body tabulku Hive.

Hello *StudentsOneLine* tabulku Hive ukládá hello data v hello HDInsight výchozí systém souborů v části hello */json/studenty/* cesta.

příkaz INSERT Hello naplní hello StudentOneLine tabulku s daty JSON hello průmětu.

příkaz SELECT Hello musí vrátit pouze jeden řádek.

Toto je výstup hello příkazu SELECT hello:

![Vyrovnání hello dokumentu JSON.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>Analýza dokumentů JSON v Hive
Hive nabízí tři různé mechanismy toorun dotazy na dokumenty JSON:

* použít hello GET\_JSON\_OBJEKT UDF (uživatelsky definované funkce)
* použít hello JSON_TUPLE UDF
* Použít vlastní SerDe
* zapsat že UDF pomocí Python nebo další jazyky, které vlastníte. V tématu [v tomto článku] [ hdinsight-python] na spuštění vlastního kódu Python s Hive.

### <a name="use-hello-getjsonobject-udf"></a>Použití hello GET\_JSON_OBJECT UDF
Hive poskytuje integrované UDF názvem [získat objekt json](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), které můžete provádět JSON dotazování za běhu. Tato metoda přebírá dva argumenty – hello název tabulky a název metody, která má hello plochou JSON a hello dokumentů JSON pole, které potřebuje toobe analyzovat. Podívejme se na příklad toosee fungování této UDF.

Získat hello křestní jméno a příjmení pro každý studenty

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Toto je hello výstup při spuštění tohoto dotazu v okně konzoly.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Existuje několik omezení hello get-json_object UDF.

* Protože každé pole v dotazu hello vyžaduje reparsing hello dotazu, ovlivňuje výkon hello.
* ZÍSKAT\_JSON_OBJECT() vrátí řetězcovou reprezentaci hello pole. tooconvert toto pole tooa Hive pole, máte tooreplace regulární výrazy toouse hello odmocnina závorky ' [' a ']' a potom taky volání rozdělit tooget hello pole.

Z tohoto důvodu hello Hive wiki doporučuje použít json_tuple.  

### <a name="use-hello-jsontuple-udf"></a>Použití hello JSON_TUPLE UDF
Jiné UDF poskytované Hive se nazývá [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), která provede lepší, než [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Tato metoda přebírá sadu klíčů a řetězec formátu JSON a vrátí hodnot pomocí jednu funkci řazené kolekce členů. Hello následující dotaz vrátí hello student id a úrovni hello z dokumentu JSON hello:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

výstup Hello skriptu v konzole hello Hive:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_řazené kolekce členů používá hello [pomoci odhalit laterální zobrazení](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntaxi Hive, která umožňuje json\_toocreate řazené kolekce členů virtuální tabulku použitím hello UDT funkce tooeach řádku původní tabulky hello.  Komplexní JSONs být příliš nepraktické kvůli hello opakované použití LATERÁLNÍ zobrazení. Kromě toho JSON_TUPLE nemůže zpracovat vnořené JSONs.

### <a name="use-custom-serde"></a>Použít vlastní SerDe
SerDe je nejlepší volbou hello k analýze vnořených dokumentů JSON, ale umožňuje vám schématu JSON hello toodefine a použití hello schématu tooparse hello dokumentů. V tomto kurzu použijete, jednu hello oblíbenější SerDe, která byla vytvořena pomocí [Roberto Congiu](https://github.com/rcongiu).

**toouse hello vlastní SerDe:**

1. Nainstalujte [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Vyberte verzi Windows X64 hello hello JDK, pokud chcete toobe pomocí nasazení systému Windows hello služby HDInsight
   
   > [!WARNING]
   > JDK 1.8 nefunguje s Tento SerDe.
   > 
   > 
   
    Po dokončení instalace hello přidáte novou proměnnou prostředí uživatele:
   
   1. Otevřete **zobrazení Upřesnit nastavení systému** z obrazovky Windows hello.
   2. Klikněte na tlačítko **proměnné prostředí**.  
   3. Přidejte nový **JAVA_HOME** příliš ukazovat proměnnou prostředí**C:\Program Files\Java\jdk1.7.0_55** nebo všude, kde je nainstalována vaší JDK.
      
      ![Nastavení konfigurace správné hodnoty pro JDK][image-hdi-hivejson-jdk]
2. Nainstalujte [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)
   
    Přidejte cestu ke složce tooyour hello bin přechodem tooControl panely--> Upravit hello systémové proměnné pro váš účet proměnné prostředí. Hello následující snímek obrazovky ukazuje, jak toodo to.
   
    ![Nastavení Maven][image-hdi-hivejson-maven]
3. Klon hello projekt z [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) webu github. To provedete kliknutím na tlačítko "Stáhnout Zip" hello, jak ukazuje následující snímek obrazovky hello.
   
    ![Klonování hello projektu][image-hdi-hivejson-serde]

4: přejděte toohello složku, kam jste si stáhli tento balíček a potom na typ "balíček mvn". To by měl vytvořit hello jar potřebné soubory, které můžete zkopírovat přes toohello clusteru.

5: přejděte toohello cílové složky v kořenové složce hello, kam jste stáhli balíček hello. Nahrajte hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar soubor toohead uzlu clusteru. Je obvykle umístíte binární složce hello hive: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin nebo něco podobného.

6: hello hive řádek, zadejte "Přidat jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Vzhledem k tomu, že v mém případě hello jar je ve složce C:\apps\dist\hive-0.13.x\bin hello, bylo možné přidat přímo hello jar s názvem hello znázorněné:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Přidání JAR tooyour projektu][image-hdi-hivejson-addjar]

Teď můžete je připraven toouse hello SerDe toorun dotazy na dokument JSON hello.

Hello následující příkaz vytvoří tabulku se definované schéma:

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

toolist hello křestní jméno a příjmení hello student

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Zde je výsledek hello z konzoly hello Hive.

![Dotaz SerDe 1][image-hdi-hivejson-serde_query1]

Součet hello toocalculate skóre hello dokumentu JSON

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Hello předcházející dotaz používá [laterální zobrazení Rozbalit](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello pole skóre, takže může být sčítají.

Toto je výstup hello z konzoly hello Hive.

![Dotaz SerDe 2][image-hdi-hivejson-serde_query2]

toofind, který předměty dané student má vypočítat skóre více než 80 body:

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

Hello předchozí dotaz vrátí pole Hive na rozdíl od get\_json\_objekt, který vrátí řetězec.

![Dotaz SerDe 3][image-hdi-hivejson-serde_query3]

Pokud chcete, aby tooskil nesprávný formát JSON, jak bylo vysvětleno v hello [stránce wikiwebu](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) z této SerDe můžete toho dosáhnout zadáním hello následující kód:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a>Souhrn
Na závěr hello typ operátoru JSON v Hive, který zvolíte, závisí na váš scénář. Pokud máte jednoduchý dokumentu JSON a máte jenom jeden toolook pole – můžete toouse hello Hive UDF get\_json\_objektu. Pokud máte více než jeden klíč toolook, můžete použít json_tuple. Pokud máte vnořené dokumentu, měli byste použít hello JSON SerDe.

## <a name="next-steps"></a>Další kroky

Další související články naleznete v části

* [Použití Hive a HiveQL s Hadoop v HDInsight tooanalyze ukázkového souboru Apache log4j](hdinsight-use-hive.md)
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
