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
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a><span data-ttu-id="c6c2d-103">Zpracovávat a analyzovat dokumenty JSON používání Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6c2d-103">Process and analyze JSON documents using Hive in HDInsight</span></span>

<span data-ttu-id="c6c2d-104">Zjistěte, jak zpracovávat a analyzovat soubory JSON používání Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-104">Learn how to process and analyze JSON files using Hive in HDInsight.</span></span> <span data-ttu-id="c6c2d-105">V tomto kurzu se používá následující dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-105">The following JSON document is used in the tutorial:</span></span>

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

<span data-ttu-id="c6c2d-106">Soubor můžete najít v wasb://processjson@hditutorialdata.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-106">The file can be found at wasb://processjson@hditutorialdata.blob.core.windows.net/.</span></span> <span data-ttu-id="c6c2d-107">Další informace o používání úložiště objektů Azure Blob s HDInsight naleznete v tématu [použití HDFS kompatibilní úložiště Azure Blob s Hadoop v HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c6c2d-107">For more information on using Azure Blob storage with HDInsight, see [Use HDFS-compatible Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="c6c2d-108">Zkopírujte soubor do kontejneru výchozí clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-108">You can copy the file to the default container of your cluster.</span></span>

<span data-ttu-id="c6c2d-109">V tomto kurzu použijete konzolu Hive.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-109">In this tutorial, you use the Hive console.</span></span>  <span data-ttu-id="c6c2d-110">Postup otevření konzoly nástroje Hive naleznete v tématu [používání Hive s Hadoop v HDInsight pomocí vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="c6c2d-110">For instructions of opening the Hive console, see [Use Hive with Hadoop on HDInsight with Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md).</span></span>

## <a name="flatten-json-documents"></a><span data-ttu-id="c6c2d-111">Vyrovnání dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="c6c2d-111">Flatten JSON documents</span></span>
<span data-ttu-id="c6c2d-112">Metody uvedené v následující části vyžadují dokumentu JSON na jednom řádku.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-112">The methods listed in the next section require the JSON document in a single row.</span></span> <span data-ttu-id="c6c2d-113">Proto musí vyrovnání dokumentu JSON na řetězec.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-113">So you must flatten the JSON document to a string.</span></span> <span data-ttu-id="c6c2d-114">Pokud už je průmětu dokumentu JSON, můžete tento krok přeskočit a analýza JSON dat, přejděte přímo k další části.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-114">If your JSON document is already flattened, you can skip this step and go straight to the next section on Analyzing JSON data.</span></span>

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

<span data-ttu-id="c6c2d-115">Soubor raw JSON se nachází v  **wasb://processjson@hditutorialdata.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="c6c2d-115">The raw JSON file is located at **wasb://processjson@hditutorialdata.blob.core.windows.net/**.</span></span> <span data-ttu-id="c6c2d-116">*StudentsRaw* Hive tabulky body k nezpracované před sloučením dokumentu JSON.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-116">The *StudentsRaw* Hive table points to the raw unflattened JSON document.</span></span>

<span data-ttu-id="c6c2d-117">*StudentsOneLine* tabulku Hive ukládá data do HDInsight výchozí systém souborů v části */json/studenty/* cesta.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-117">The *StudentsOneLine* Hive table stores the data in the HDInsight default file system under the */json/students/* path.</span></span>

<span data-ttu-id="c6c2d-118">Příkaz INSERT naplní StudentOneLine tabulku s plochou data JSON.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-118">The INSERT statement populates the StudentOneLine table with the flattened JSON data.</span></span>

<span data-ttu-id="c6c2d-119">Příkaz SELECT se vrátit pouze jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-119">The SELECT statement shall only return one row.</span></span>

<span data-ttu-id="c6c2d-120">Toto je výstup příkazu SELECT:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-120">Here is the output of the SELECT statement:</span></span>

![Vyrovnání dokumentu JSON.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a><span data-ttu-id="c6c2d-122">Analýza dokumentů JSON v Hive</span><span class="sxs-lookup"><span data-stu-id="c6c2d-122">Analyze JSON documents in Hive</span></span>
<span data-ttu-id="c6c2d-123">Hive poskytuje tři různé mechanismy pro spouštění dotazů na dokumenty JSON:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-123">Hive provides three different mechanisms to run queries on JSON documents:</span></span>

* <span data-ttu-id="c6c2d-124">Použijte GET\_JSON\_OBJEKT UDF (uživatelsky definované funkce)</span><span class="sxs-lookup"><span data-stu-id="c6c2d-124">use the GET\_JSON\_OBJECT UDF (User-defined function)</span></span>
* <span data-ttu-id="c6c2d-125">Použití JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="c6c2d-125">use the JSON_TUPLE UDF</span></span>
* <span data-ttu-id="c6c2d-126">Použít vlastní SerDe</span><span class="sxs-lookup"><span data-stu-id="c6c2d-126">use custom SerDe</span></span>
* <span data-ttu-id="c6c2d-127">zapsat že UDF pomocí Python nebo další jazyky, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-127">write you own UDF using Python or other languages.</span></span> <span data-ttu-id="c6c2d-128">V tématu [v tomto článku] [ hdinsight-python] na spuštění vlastního kódu Python s Hive.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-128">See [this article][hdinsight-python] on running your own Python code with Hive.</span></span>

### <a name="use-the-getjsonobject-udf"></a><span data-ttu-id="c6c2d-129">Použijte GET\_JSON_OBJECT UDF</span><span class="sxs-lookup"><span data-stu-id="c6c2d-129">Use the GET\_JSON_OBJECT UDF</span></span>
<span data-ttu-id="c6c2d-130">Hive poskytuje integrované UDF názvem [získat objekt json](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), které můžete provádět JSON dotazování za běhu.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-130">Hive provides a built-in UDF called [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), which can perform JSON querying during run time.</span></span> <span data-ttu-id="c6c2d-131">Tato metoda přebírá dva argumenty – název tabulky a název metody, který má sloučeném dokumentu JSON a pole JSON, který potřebujete analyzovat.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-131">This method takes two arguments – the table name and method name, which has the flattened JSON document and the JSON field that needs to be parsed.</span></span> <span data-ttu-id="c6c2d-132">Podívejme se na příklad zobrazíte fungování této UDF.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-132">Let’s look at an example to see how this UDF works.</span></span>

<span data-ttu-id="c6c2d-133">Křestní jméno a příjmení pro každý studenty</span><span class="sxs-lookup"><span data-stu-id="c6c2d-133">Get the first name and last name for each student</span></span>

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

<span data-ttu-id="c6c2d-134">Toto je výstup při spuštění tohoto dotazu v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-134">Here is the output when running this query in console window.</span></span>

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

<span data-ttu-id="c6c2d-136">Existuje několik omezení get-json_object UDF.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-136">There are a few limitations of the get-json_object UDF.</span></span>

* <span data-ttu-id="c6c2d-137">Protože každé pole v dotazu vyžaduje reparsing dotaz, ovlivňuje výkon.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-137">Because each field in the query requires reparsing the query, it affects the performance.</span></span>
* <span data-ttu-id="c6c2d-138">ZÍSKAT\_JSON_OBJECT() vrátí řetězcovou reprezentaci pole.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-138">GET\_JSON_OBJECT() returns the string representation of an array.</span></span> <span data-ttu-id="c6c2d-139">Chcete-li převést toto pole k poli Hive, budete muset použít regulární výrazy k nahrazení hranaté závorky ' [' a ']' a také zavolat rozdělení získat pole.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-139">To convert this array to a Hive array, you have to use regular expressions to replace the square brackets ‘[‘ and ‘]’ and then also call split to get the array.</span></span>

<span data-ttu-id="c6c2d-140">To je proto doporučuje Hive wiki pomocí json_tuple.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-140">This is why the Hive wiki recommends using json_tuple.</span></span>  

### <a name="use-the-jsontuple-udf"></a><span data-ttu-id="c6c2d-141">Použití JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="c6c2d-141">Use the JSON_TUPLE UDF</span></span>
<span data-ttu-id="c6c2d-142">Jiné UDF poskytované Hive se nazývá [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), která provede lepší, než [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span><span class="sxs-lookup"><span data-stu-id="c6c2d-142">Another UDF provided by Hive is called [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), which performs better than [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span></span> <span data-ttu-id="c6c2d-143">Tato metoda přebírá sadu klíčů a řetězec formátu JSON a vrátí hodnot pomocí jednu funkci řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-143">This method takes a set of keys and a JSON string, and returns a tuple of values using one function.</span></span> <span data-ttu-id="c6c2d-144">Následující dotaz vrátí student id a úrovni z dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-144">The following query returns the student id and the grade from the JSON document:</span></span>

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

<span data-ttu-id="c6c2d-145">Výstup skriptu v konzole nástroje Hive:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-145">The output of this script in the Hive console:</span></span>

![json_tuple UDF][image-hdi-hivejson-jsontuple]

<span data-ttu-id="c6c2d-147">JSON\_používá řazené kolekce členů [pomoci odhalit laterální zobrazení](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntaxi Hive, která umožňuje json\_řazené kolekce členů a vytvořte virtuální tabulku použitím funkce UDT na každém řádku původní tabulky.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-147">JSON\_TUPLE uses the [lateral view](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax in Hive, which allows json\_tuple to create a virtual table by applying the UDT function to each row of the original table.</span></span>  <span data-ttu-id="c6c2d-148">Komplexní JSONs být příliš nepraktické kvůli opakované použití LATERÁLNÍ zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-148">Complex JSONs become too unwieldy because of the repeated use of LATERAL VIEW.</span></span> <span data-ttu-id="c6c2d-149">Kromě toho JSON_TUPLE nemůže zpracovat vnořené JSONs.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-149">Furthermore, JSON_TUPLE cannot handle nested JSONs.</span></span>

### <a name="use-custom-serde"></a><span data-ttu-id="c6c2d-150">Použít vlastní SerDe</span><span class="sxs-lookup"><span data-stu-id="c6c2d-150">Use custom SerDe</span></span>
<span data-ttu-id="c6c2d-151">SerDe je nejlepší volbou pro analýzu vnořených dokumentů JSON, ale umožňuje vám umožňuje definovat schématu JSON a použití schématu analyzovat dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-151">SerDe is the best choice for parsing nested JSON documents, it allows you to define the JSON schema, and use the schema to parse the documents.</span></span> <span data-ttu-id="c6c2d-152">V tomto kurzu použijete, jednu oblíbenější SerDe, která byla vytvořena pomocí [Roberto Congiu](https://github.com/rcongiu).</span><span class="sxs-lookup"><span data-stu-id="c6c2d-152">In this tutorial, you use one of the more popular SerDe that has been developed by [Roberto Congiu](https://github.com/rcongiu).</span></span>

<span data-ttu-id="c6c2d-153">**Použití vlastní SerDe:**</span><span class="sxs-lookup"><span data-stu-id="c6c2d-153">**To use the custom SerDe:**</span></span>

1. <span data-ttu-id="c6c2d-154">Nainstalujte [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span><span class="sxs-lookup"><span data-stu-id="c6c2d-154">Install [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span></span> <span data-ttu-id="c6c2d-155">Vyberte verzi Windows X64 sadu JDK, pokud se chystáte použít nasazení systému Windows služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6c2d-155">Choose the Windows X64 version of the JDK if you are going to be using the Windows deployment of HDInsight</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="c6c2d-156">JDK 1.8 nefunguje s Tento SerDe.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-156">JDK 1.8 doesn't work with this SerDe.</span></span>
   > 
   > 
   
    <span data-ttu-id="c6c2d-157">Po dokončení instalace, přidejte novou proměnnou prostředí uživatele:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-157">After the installation is completed, add a new user environment variable:</span></span>
   
   1. <span data-ttu-id="c6c2d-158">Otevřete **zobrazení Upřesnit nastavení systému** na obrazovce Windows.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-158">Open **View advanced system settings** from the Windows screen.</span></span>
   2. <span data-ttu-id="c6c2d-159">Klikněte na tlačítko **proměnné prostředí**.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-159">Click **Environment Variables**.</span></span>  
   3. <span data-ttu-id="c6c2d-160">Přidejte nový **JAVA_HOME** proměnnou prostředí ukazovat na **C:\Program Files\Java\jdk1.7.0_55** nebo všude, kde je nainstalována vaší JDK.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-160">Add a new **JAVA_HOME** environment variable is pointing to **C:\Program Files\Java\jdk1.7.0_55** or wherever your JDK is installed.</span></span>
      
      ![Nastavení konfigurace správné hodnoty pro JDK][image-hdi-hivejson-jdk]
2. <span data-ttu-id="c6c2d-162">Nainstalujte [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span><span class="sxs-lookup"><span data-stu-id="c6c2d-162">Install [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span></span>
   
    <span data-ttu-id="c6c2d-163">Přidat do složky bin cestu přechodem na ovládací prvek Panel--> Upravit proměnné systému pro účet proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-163">Add the bin folder to your path by going to Control Panel-->Edit the System Variables for your account Environment variables.</span></span> <span data-ttu-id="c6c2d-164">Následující snímek obrazovky ukazuje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-164">The following screenshot shows you how to do this.</span></span>
   
    ![Nastavení Maven][image-hdi-hivejson-maven]
3. <span data-ttu-id="c6c2d-166">Klonování projekt z [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) webu github.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-166">Clone the project from [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site.</span></span> <span data-ttu-id="c6c2d-167">To provedete kliknutím na tlačítko "Stáhnout Zip", jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-167">You can do this by clicking on the “Download Zip” button as shown in the following screenshot.</span></span>
   
    ![Klonování projektu][image-hdi-hivejson-serde]

<span data-ttu-id="c6c2d-169">4: přejděte do složky, kde jste si stáhli tento balíček a potom na typ "balíček mvn".</span><span class="sxs-lookup"><span data-stu-id="c6c2d-169">4: Go to the folder where you have downloaded this package and then type “mvn package”.</span></span> <span data-ttu-id="c6c2d-170">To by měl vytvořit potřebné jar soubory, které pak můžete zkopírovat přes do clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-170">This should create the necessary jar files that you can then copy over to the cluster.</span></span>

<span data-ttu-id="c6c2d-171">5: přejděte do cílové složky v kořenové složce, kam jste stáhli balíček.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-171">5: Go to the target folder under the root folder where you downloaded the package.</span></span> <span data-ttu-id="c6c2d-172">Nahrajte soubor json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar do hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-172">Upload the json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar file to head-node of your cluster.</span></span> <span data-ttu-id="c6c2d-173">Je obvykle umístíte ve složce binární hive: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin nebo něco podobného.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-173">I usually put it under the hive binary folder: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin or something similar.</span></span>

<span data-ttu-id="c6c2d-174">6: hive řádek, zadejte "Přidat jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar".</span><span class="sxs-lookup"><span data-stu-id="c6c2d-174">6: In the hive prompt, type “add jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar”.</span></span> <span data-ttu-id="c6c2d-175">Vzhledem k tomu, že v mém případě jar nachází ve složce C:\apps\dist\hive-0.13.x\bin, bylo možné přímo přidat jar s názvem, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-175">Since in my case, the jar is in the C:\apps\dist\hive-0.13.x\bin folder, I can directly add the jar with the name as shown:</span></span>

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Přidání JAR do projektu][image-hdi-hivejson-addjar]

<span data-ttu-id="c6c2d-177">Teď můžete je připravený k použití SerDe ke spouštění dotazů na dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-177">Now, you are ready to use the SerDe to run queries against the JSON document.</span></span>

<span data-ttu-id="c6c2d-178">Následující příkaz vytvoří tabulku se definované schéma:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-178">The following statement creates a table with a defined schema:</span></span>

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

<span data-ttu-id="c6c2d-179">K zobrazení seznamu křestní jméno a příjmení student</span><span class="sxs-lookup"><span data-stu-id="c6c2d-179">To list the first name and last name of the student</span></span>

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

<span data-ttu-id="c6c2d-180">Zde je výsledek z konzoly nástroje Hive.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-180">Here is the result from the Hive console.</span></span>

![Dotaz SerDe 1][image-hdi-hivejson-serde_query1]

<span data-ttu-id="c6c2d-182">Chcete-li vypočítat součet skóre dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="c6c2d-182">To calculate the sum of scores of the JSON document</span></span>

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

<span data-ttu-id="c6c2d-183">Předchozí dotaz používá [laterální zobrazení Rozbalit](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF rozbalte pole skóre, takže může být sčítají.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-183">The preceding query uses [lateral view explode](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF to expand the array of scores so that they can be summed.</span></span>

<span data-ttu-id="c6c2d-184">Toto je výstup z konzoly nástroje Hive.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-184">Here is the output from the Hive console.</span></span>

![Dotaz SerDe 2][image-hdi-hivejson-serde_query2]

<span data-ttu-id="c6c2d-186">K vyhledání, která předměty dané student má vypočítat skóre více než 80 body:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-186">To find which subjects a given student has scored more than 80 points:</span></span>

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

<span data-ttu-id="c6c2d-187">Předchozí dotaz vrátí pole Hive na rozdíl od get\_json\_objekt, který vrátí řetězec.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-187">The preceding query returns a Hive array unlike get\_json\_object, which returns a string.</span></span>

![Dotaz SerDe 3][image-hdi-hivejson-serde_query3]

<span data-ttu-id="c6c2d-189">Pokud chcete skil nesprávný formát JSON, pak jak je vysvětleno v [stránce wikiwebu](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) z této SerDe můžete dosáhnout, a to zadáním následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-189">If you want to skil malformed JSON, then as explained in the [wiki page](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) of this SerDe you can achieve that by typing the following code:</span></span>  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a><span data-ttu-id="c6c2d-190">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c6c2d-190">Summary</span></span>
<span data-ttu-id="c6c2d-191">Na závěr typ operátor JSON v Hive, který zvolíte, závisí na váš scénář.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-191">In conclusion, the type of JSON operator in Hive that you choose depends on your scenario.</span></span> <span data-ttu-id="c6c2d-192">Pokud máte jednoduchý dokumentu JSON a máte pouze jedno pole pro vyhledávání na – můžete použít get Hive UDF\_json\_objektu.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-192">If you have a simple JSON document and you only have one field to look up on – you can choose to use the Hive UDF get\_json\_object.</span></span> <span data-ttu-id="c6c2d-193">Pokud máte více než jeden klíč k vyhledání, můžete použít json_tuple.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-193">If you have more than one key to look up on, then you can use json_tuple.</span></span> <span data-ttu-id="c6c2d-194">Pokud máte vnořené dokumentu, měli byste použít JSON SerDe.</span><span class="sxs-lookup"><span data-stu-id="c6c2d-194">If you have a nested document, then you should use the JSON SerDe.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6c2d-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6c2d-195">Next steps</span></span>

<span data-ttu-id="c6c2d-196">Další související články naleznete v části</span><span class="sxs-lookup"><span data-stu-id="c6c2d-196">For other related articles, see</span></span>

* [<span data-ttu-id="c6c2d-197">Použití Hive a HiveQL s Hadoop v HDInsight k analýze ukázkového souboru Apache log4j</span><span class="sxs-lookup"><span data-stu-id="c6c2d-197">Use Hive and HiveQL with Hadoop in HDInsight to analyze a sample Apache log4j file</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="c6c2d-198">Analýza dat zpoždění letu pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6c2d-198">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="c6c2d-199">Analýza dat Twitteru pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6c2d-199">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="c6c2d-200">Spustit úlohu Hadoop pomocí Azure Cosmos DB a HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6c2d-200">Run a Hadoop job using Azure Cosmos DB and HDInsight</span></span>](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

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
