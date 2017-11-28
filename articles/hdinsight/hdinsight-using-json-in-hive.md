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
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a><span data-ttu-id="3e462-103">Zpracovávat a analyzovat dokumenty JSON používání Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e462-103">Process and analyze JSON documents using Hive in HDInsight</span></span>

<span data-ttu-id="3e462-104">Zjistěte, jak tooprocess a analyzovat soubory JSON používání Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e462-104">Learn how tooprocess and analyze JSON files using Hive in HDInsight.</span></span> <span data-ttu-id="3e462-105">Následující dokument JSON Hello se používá v kurzu hello:</span><span class="sxs-lookup"><span data-stu-id="3e462-105">hello following JSON document is used in hello tutorial:</span></span>

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

<span data-ttu-id="3e462-106">Hello soubor se nachází v wasb://processjson@hditutorialdata.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="3e462-106">hello file can be found at wasb://processjson@hditutorialdata.blob.core.windows.net/.</span></span> <span data-ttu-id="3e462-107">Další informace o používání úložiště objektů Azure Blob s HDInsight naleznete v tématu [použití HDFS kompatibilní úložiště Azure Blob s Hadoop v HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3e462-107">For more information on using Azure Blob storage with HDInsight, see [Use HDFS-compatible Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="3e462-108">Můžete zkopírovat hello souboru toohello výchozí kontejner clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e462-108">You can copy hello file toohello default container of your cluster.</span></span>

<span data-ttu-id="3e462-109">V tomto kurzu použijete konzolu hello Hive.</span><span class="sxs-lookup"><span data-stu-id="3e462-109">In this tutorial, you use hello Hive console.</span></span>  <span data-ttu-id="3e462-110">Postup otevření konzoly hello Hive naleznete v tématu [používání Hive s Hadoop v HDInsight pomocí vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="3e462-110">For instructions of opening hello Hive console, see [Use Hive with Hadoop on HDInsight with Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md).</span></span>

## <a name="flatten-json-documents"></a><span data-ttu-id="3e462-111">Vyrovnání dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="3e462-111">Flatten JSON documents</span></span>
<span data-ttu-id="3e462-112">Hello metody uvedené v další části hello vyžadují hello dokumentu JSON na jednom řádku.</span><span class="sxs-lookup"><span data-stu-id="3e462-112">hello methods listed in hello next section require hello JSON document in a single row.</span></span> <span data-ttu-id="3e462-113">Proto musí vyrovnání hello JSON dokumentu tooa řetězec.</span><span class="sxs-lookup"><span data-stu-id="3e462-113">So you must flatten hello JSON document tooa string.</span></span> <span data-ttu-id="3e462-114">Pokud už je průmětu dokumentu JSON, můžete tento krok přeskočit a přímých toohello další části, přejděte na data analýza JSON.</span><span class="sxs-lookup"><span data-stu-id="3e462-114">If your JSON document is already flattened, you can skip this step and go straight toohello next section on Analyzing JSON data.</span></span>

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

<span data-ttu-id="3e462-115">Hello soubor raw JSON je umístěn v  **wasb://processjson@hditutorialdata.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="3e462-115">hello raw JSON file is located at **wasb://processjson@hditutorialdata.blob.core.windows.net/**.</span></span> <span data-ttu-id="3e462-116">Hello *StudentsRaw* dokumentu JSON nezpracovaná před sloučením toohello body tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="3e462-116">hello *StudentsRaw* Hive table points toohello raw unflattened JSON document.</span></span>

<span data-ttu-id="3e462-117">Hello *StudentsOneLine* tabulku Hive ukládá hello data v hello HDInsight výchozí systém souborů v části hello */json/studenty/* cesta.</span><span class="sxs-lookup"><span data-stu-id="3e462-117">hello *StudentsOneLine* Hive table stores hello data in hello HDInsight default file system under hello */json/students/* path.</span></span>

<span data-ttu-id="3e462-118">příkaz INSERT Hello naplní hello StudentOneLine tabulku s daty JSON hello průmětu.</span><span class="sxs-lookup"><span data-stu-id="3e462-118">hello INSERT statement populates hello StudentOneLine table with hello flattened JSON data.</span></span>

<span data-ttu-id="3e462-119">příkaz SELECT Hello musí vrátit pouze jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="3e462-119">hello SELECT statement shall only return one row.</span></span>

<span data-ttu-id="3e462-120">Toto je výstup hello příkazu SELECT hello:</span><span class="sxs-lookup"><span data-stu-id="3e462-120">Here is hello output of hello SELECT statement:</span></span>

![Vyrovnání hello dokumentu JSON.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a><span data-ttu-id="3e462-122">Analýza dokumentů JSON v Hive</span><span class="sxs-lookup"><span data-stu-id="3e462-122">Analyze JSON documents in Hive</span></span>
<span data-ttu-id="3e462-123">Hive nabízí tři různé mechanismy toorun dotazy na dokumenty JSON:</span><span class="sxs-lookup"><span data-stu-id="3e462-123">Hive provides three different mechanisms toorun queries on JSON documents:</span></span>

* <span data-ttu-id="3e462-124">použít hello GET\_JSON\_OBJEKT UDF (uživatelsky definované funkce)</span><span class="sxs-lookup"><span data-stu-id="3e462-124">use hello GET\_JSON\_OBJECT UDF (User-defined function)</span></span>
* <span data-ttu-id="3e462-125">použít hello JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="3e462-125">use hello JSON_TUPLE UDF</span></span>
* <span data-ttu-id="3e462-126">Použít vlastní SerDe</span><span class="sxs-lookup"><span data-stu-id="3e462-126">use custom SerDe</span></span>
* <span data-ttu-id="3e462-127">zapsat že UDF pomocí Python nebo další jazyky, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="3e462-127">write you own UDF using Python or other languages.</span></span> <span data-ttu-id="3e462-128">V tématu [v tomto článku] [ hdinsight-python] na spuštění vlastního kódu Python s Hive.</span><span class="sxs-lookup"><span data-stu-id="3e462-128">See [this article][hdinsight-python] on running your own Python code with Hive.</span></span>

### <a name="use-hello-getjsonobject-udf"></a><span data-ttu-id="3e462-129">Použití hello GET\_JSON_OBJECT UDF</span><span class="sxs-lookup"><span data-stu-id="3e462-129">Use hello GET\_JSON_OBJECT UDF</span></span>
<span data-ttu-id="3e462-130">Hive poskytuje integrované UDF názvem [získat objekt json](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), které můžete provádět JSON dotazování za běhu.</span><span class="sxs-lookup"><span data-stu-id="3e462-130">Hive provides a built-in UDF called [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), which can perform JSON querying during run time.</span></span> <span data-ttu-id="3e462-131">Tato metoda přebírá dva argumenty – hello název tabulky a název metody, která má hello plochou JSON a hello dokumentů JSON pole, které potřebuje toobe analyzovat.</span><span class="sxs-lookup"><span data-stu-id="3e462-131">This method takes two arguments – hello table name and method name, which has hello flattened JSON document and hello JSON field that needs toobe parsed.</span></span> <span data-ttu-id="3e462-132">Podívejme se na příklad toosee fungování této UDF.</span><span class="sxs-lookup"><span data-stu-id="3e462-132">Let’s look at an example toosee how this UDF works.</span></span>

<span data-ttu-id="3e462-133">Získat hello křestní jméno a příjmení pro každý studenty</span><span class="sxs-lookup"><span data-stu-id="3e462-133">Get hello first name and last name for each student</span></span>

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

<span data-ttu-id="3e462-134">Toto je hello výstup při spuštění tohoto dotazu v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="3e462-134">Here is hello output when running this query in console window.</span></span>

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

<span data-ttu-id="3e462-136">Existuje několik omezení hello get-json_object UDF.</span><span class="sxs-lookup"><span data-stu-id="3e462-136">There are a few limitations of hello get-json_object UDF.</span></span>

* <span data-ttu-id="3e462-137">Protože každé pole v dotazu hello vyžaduje reparsing hello dotazu, ovlivňuje výkon hello.</span><span class="sxs-lookup"><span data-stu-id="3e462-137">Because each field in hello query requires reparsing hello query, it affects hello performance.</span></span>
* <span data-ttu-id="3e462-138">ZÍSKAT\_JSON_OBJECT() vrátí řetězcovou reprezentaci hello pole.</span><span class="sxs-lookup"><span data-stu-id="3e462-138">GET\_JSON_OBJECT() returns hello string representation of an array.</span></span> <span data-ttu-id="3e462-139">tooconvert toto pole tooa Hive pole, máte tooreplace regulární výrazy toouse hello odmocnina závorky ' [' a ']' a potom taky volání rozdělit tooget hello pole.</span><span class="sxs-lookup"><span data-stu-id="3e462-139">tooconvert this array tooa Hive array, you have toouse regular expressions tooreplace hello square brackets ‘[‘ and ‘]’ and then also call split tooget hello array.</span></span>

<span data-ttu-id="3e462-140">Z tohoto důvodu hello Hive wiki doporučuje použít json_tuple.</span><span class="sxs-lookup"><span data-stu-id="3e462-140">This is why hello Hive wiki recommends using json_tuple.</span></span>  

### <a name="use-hello-jsontuple-udf"></a><span data-ttu-id="3e462-141">Použití hello JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="3e462-141">Use hello JSON_TUPLE UDF</span></span>
<span data-ttu-id="3e462-142">Jiné UDF poskytované Hive se nazývá [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), která provede lepší, než [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span><span class="sxs-lookup"><span data-stu-id="3e462-142">Another UDF provided by Hive is called [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), which performs better than [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span></span> <span data-ttu-id="3e462-143">Tato metoda přebírá sadu klíčů a řetězec formátu JSON a vrátí hodnot pomocí jednu funkci řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="3e462-143">This method takes a set of keys and a JSON string, and returns a tuple of values using one function.</span></span> <span data-ttu-id="3e462-144">Hello následující dotaz vrátí hello student id a úrovni hello z dokumentu JSON hello:</span><span class="sxs-lookup"><span data-stu-id="3e462-144">hello following query returns hello student id and hello grade from hello JSON document:</span></span>

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

<span data-ttu-id="3e462-145">výstup Hello skriptu v konzole hello Hive:</span><span class="sxs-lookup"><span data-stu-id="3e462-145">hello output of this script in hello Hive console:</span></span>

![json_tuple UDF][image-hdi-hivejson-jsontuple]

<span data-ttu-id="3e462-147">JSON\_řazené kolekce členů používá hello [pomoci odhalit laterální zobrazení](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntaxi Hive, která umožňuje json\_toocreate řazené kolekce členů virtuální tabulku použitím hello UDT funkce tooeach řádku původní tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="3e462-147">JSON\_TUPLE uses hello [lateral view](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax in Hive, which allows json\_tuple toocreate a virtual table by applying hello UDT function tooeach row of hello original table.</span></span>  <span data-ttu-id="3e462-148">Komplexní JSONs být příliš nepraktické kvůli hello opakované použití LATERÁLNÍ zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3e462-148">Complex JSONs become too unwieldy because of hello repeated use of LATERAL VIEW.</span></span> <span data-ttu-id="3e462-149">Kromě toho JSON_TUPLE nemůže zpracovat vnořené JSONs.</span><span class="sxs-lookup"><span data-stu-id="3e462-149">Furthermore, JSON_TUPLE cannot handle nested JSONs.</span></span>

### <a name="use-custom-serde"></a><span data-ttu-id="3e462-150">Použít vlastní SerDe</span><span class="sxs-lookup"><span data-stu-id="3e462-150">Use custom SerDe</span></span>
<span data-ttu-id="3e462-151">SerDe je nejlepší volbou hello k analýze vnořených dokumentů JSON, ale umožňuje vám schématu JSON hello toodefine a použití hello schématu tooparse hello dokumentů.</span><span class="sxs-lookup"><span data-stu-id="3e462-151">SerDe is hello best choice for parsing nested JSON documents, it allows you toodefine hello JSON schema, and use hello schema tooparse hello documents.</span></span> <span data-ttu-id="3e462-152">V tomto kurzu použijete, jednu hello oblíbenější SerDe, která byla vytvořena pomocí [Roberto Congiu](https://github.com/rcongiu).</span><span class="sxs-lookup"><span data-stu-id="3e462-152">In this tutorial, you use one of hello more popular SerDe that has been developed by [Roberto Congiu](https://github.com/rcongiu).</span></span>

<span data-ttu-id="3e462-153">**toouse hello vlastní SerDe:**</span><span class="sxs-lookup"><span data-stu-id="3e462-153">**toouse hello custom SerDe:**</span></span>

1. <span data-ttu-id="3e462-154">Nainstalujte [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span><span class="sxs-lookup"><span data-stu-id="3e462-154">Install [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span></span> <span data-ttu-id="3e462-155">Vyberte verzi Windows X64 hello hello JDK, pokud chcete toobe pomocí nasazení systému Windows hello služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e462-155">Choose hello Windows X64 version of hello JDK if you are going toobe using hello Windows deployment of HDInsight</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="3e462-156">JDK 1.8 nefunguje s Tento SerDe.</span><span class="sxs-lookup"><span data-stu-id="3e462-156">JDK 1.8 doesn't work with this SerDe.</span></span>
   > 
   > 
   
    <span data-ttu-id="3e462-157">Po dokončení instalace hello přidáte novou proměnnou prostředí uživatele:</span><span class="sxs-lookup"><span data-stu-id="3e462-157">After hello installation is completed, add a new user environment variable:</span></span>
   
   1. <span data-ttu-id="3e462-158">Otevřete **zobrazení Upřesnit nastavení systému** z obrazovky Windows hello.</span><span class="sxs-lookup"><span data-stu-id="3e462-158">Open **View advanced system settings** from hello Windows screen.</span></span>
   2. <span data-ttu-id="3e462-159">Klikněte na tlačítko **proměnné prostředí**.</span><span class="sxs-lookup"><span data-stu-id="3e462-159">Click **Environment Variables**.</span></span>  
   3. <span data-ttu-id="3e462-160">Přidejte nový **JAVA_HOME** příliš ukazovat proměnnou prostředí**C:\Program Files\Java\jdk1.7.0_55** nebo všude, kde je nainstalována vaší JDK.</span><span class="sxs-lookup"><span data-stu-id="3e462-160">Add a new **JAVA_HOME** environment variable is pointing too**C:\Program Files\Java\jdk1.7.0_55** or wherever your JDK is installed.</span></span>
      
      ![Nastavení konfigurace správné hodnoty pro JDK][image-hdi-hivejson-jdk]
2. <span data-ttu-id="3e462-162">Nainstalujte [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span><span class="sxs-lookup"><span data-stu-id="3e462-162">Install [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span></span>
   
    <span data-ttu-id="3e462-163">Přidejte cestu ke složce tooyour hello bin přechodem tooControl panely--> Upravit hello systémové proměnné pro váš účet proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e462-163">Add hello bin folder tooyour path by going tooControl Panel-->Edit hello System Variables for your account Environment variables.</span></span> <span data-ttu-id="3e462-164">Hello následující snímek obrazovky ukazuje, jak toodo to.</span><span class="sxs-lookup"><span data-stu-id="3e462-164">hello following screenshot shows you how toodo this.</span></span>
   
    ![Nastavení Maven][image-hdi-hivejson-maven]
3. <span data-ttu-id="3e462-166">Klon hello projekt z [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) webu github.</span><span class="sxs-lookup"><span data-stu-id="3e462-166">Clone hello project from [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site.</span></span> <span data-ttu-id="3e462-167">To provedete kliknutím na tlačítko "Stáhnout Zip" hello, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="3e462-167">You can do this by clicking on hello “Download Zip” button as shown in hello following screenshot.</span></span>
   
    ![Klonování hello projektu][image-hdi-hivejson-serde]

<span data-ttu-id="3e462-169">4: přejděte toohello složku, kam jste si stáhli tento balíček a potom na typ "balíček mvn".</span><span class="sxs-lookup"><span data-stu-id="3e462-169">4: Go toohello folder where you have downloaded this package and then type “mvn package”.</span></span> <span data-ttu-id="3e462-170">To by měl vytvořit hello jar potřebné soubory, které můžete zkopírovat přes toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e462-170">This should create hello necessary jar files that you can then copy over toohello cluster.</span></span>

<span data-ttu-id="3e462-171">5: přejděte toohello cílové složky v kořenové složce hello, kam jste stáhli balíček hello.</span><span class="sxs-lookup"><span data-stu-id="3e462-171">5: Go toohello target folder under hello root folder where you downloaded hello package.</span></span> <span data-ttu-id="3e462-172">Nahrajte hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar soubor toohead uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e462-172">Upload hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar file toohead-node of your cluster.</span></span> <span data-ttu-id="3e462-173">Je obvykle umístíte binární složce hello hive: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin nebo něco podobného.</span><span class="sxs-lookup"><span data-stu-id="3e462-173">I usually put it under hello hive binary folder: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin or something similar.</span></span>

<span data-ttu-id="3e462-174">6: hello hive řádek, zadejte "Přidat jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar".</span><span class="sxs-lookup"><span data-stu-id="3e462-174">6: In hello hive prompt, type “add jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar”.</span></span> <span data-ttu-id="3e462-175">Vzhledem k tomu, že v mém případě hello jar je ve složce C:\apps\dist\hive-0.13.x\bin hello, bylo možné přidat přímo hello jar s názvem hello znázorněné:</span><span class="sxs-lookup"><span data-stu-id="3e462-175">Since in my case, hello jar is in hello C:\apps\dist\hive-0.13.x\bin folder, I can directly add hello jar with hello name as shown:</span></span>

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Přidání JAR tooyour projektu][image-hdi-hivejson-addjar]

<span data-ttu-id="3e462-177">Teď můžete je připraven toouse hello SerDe toorun dotazy na dokument JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3e462-177">Now, you are ready toouse hello SerDe toorun queries against hello JSON document.</span></span>

<span data-ttu-id="3e462-178">Hello následující příkaz vytvoří tabulku se definované schéma:</span><span class="sxs-lookup"><span data-stu-id="3e462-178">hello following statement creates a table with a defined schema:</span></span>

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

<span data-ttu-id="3e462-179">toolist hello křestní jméno a příjmení hello student</span><span class="sxs-lookup"><span data-stu-id="3e462-179">toolist hello first name and last name of hello student</span></span>

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

<span data-ttu-id="3e462-180">Zde je výsledek hello z konzoly hello Hive.</span><span class="sxs-lookup"><span data-stu-id="3e462-180">Here is hello result from hello Hive console.</span></span>

![Dotaz SerDe 1][image-hdi-hivejson-serde_query1]

<span data-ttu-id="3e462-182">Součet hello toocalculate skóre hello dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="3e462-182">toocalculate hello sum of scores of hello JSON document</span></span>

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

<span data-ttu-id="3e462-183">Hello předcházející dotaz používá [laterální zobrazení Rozbalit](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello pole skóre, takže může být sčítají.</span><span class="sxs-lookup"><span data-stu-id="3e462-183">hello preceding query uses [lateral view explode](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello array of scores so that they can be summed.</span></span>

<span data-ttu-id="3e462-184">Toto je výstup hello z konzoly hello Hive.</span><span class="sxs-lookup"><span data-stu-id="3e462-184">Here is hello output from hello Hive console.</span></span>

![Dotaz SerDe 2][image-hdi-hivejson-serde_query2]

<span data-ttu-id="3e462-186">toofind, který předměty dané student má vypočítat skóre více než 80 body:</span><span class="sxs-lookup"><span data-stu-id="3e462-186">toofind which subjects a given student has scored more than 80 points:</span></span>

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

<span data-ttu-id="3e462-187">Hello předchozí dotaz vrátí pole Hive na rozdíl od get\_json\_objekt, který vrátí řetězec.</span><span class="sxs-lookup"><span data-stu-id="3e462-187">hello preceding query returns a Hive array unlike get\_json\_object, which returns a string.</span></span>

![Dotaz SerDe 3][image-hdi-hivejson-serde_query3]

<span data-ttu-id="3e462-189">Pokud chcete, aby tooskil nesprávný formát JSON, jak bylo vysvětleno v hello [stránce wikiwebu](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) z této SerDe můžete toho dosáhnout zadáním hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="3e462-189">If you want tooskil malformed JSON, then as explained in hello [wiki page](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) of this SerDe you can achieve that by typing hello following code:</span></span>  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a><span data-ttu-id="3e462-190">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3e462-190">Summary</span></span>
<span data-ttu-id="3e462-191">Na závěr hello typ operátoru JSON v Hive, který zvolíte, závisí na váš scénář.</span><span class="sxs-lookup"><span data-stu-id="3e462-191">In conclusion, hello type of JSON operator in Hive that you choose depends on your scenario.</span></span> <span data-ttu-id="3e462-192">Pokud máte jednoduchý dokumentu JSON a máte jenom jeden toolook pole – můžete toouse hello Hive UDF get\_json\_objektu.</span><span class="sxs-lookup"><span data-stu-id="3e462-192">If you have a simple JSON document and you only have one field toolook up on – you can choose toouse hello Hive UDF get\_json\_object.</span></span> <span data-ttu-id="3e462-193">Pokud máte více než jeden klíč toolook, můžete použít json_tuple.</span><span class="sxs-lookup"><span data-stu-id="3e462-193">If you have more than one key toolook up on, then you can use json_tuple.</span></span> <span data-ttu-id="3e462-194">Pokud máte vnořené dokumentu, měli byste použít hello JSON SerDe.</span><span class="sxs-lookup"><span data-stu-id="3e462-194">If you have a nested document, then you should use hello JSON SerDe.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e462-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e462-195">Next steps</span></span>

<span data-ttu-id="3e462-196">Další související články naleznete v části</span><span class="sxs-lookup"><span data-stu-id="3e462-196">For other related articles, see</span></span>

* [<span data-ttu-id="3e462-197">Použití Hive a HiveQL s Hadoop v HDInsight tooanalyze ukázkového souboru Apache log4j</span><span class="sxs-lookup"><span data-stu-id="3e462-197">Use Hive and HiveQL with Hadoop in HDInsight tooanalyze a sample Apache log4j file</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3e462-198">Analýza dat zpoždění letu pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e462-198">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="3e462-199">Analýza dat Twitteru pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e462-199">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="3e462-200">Spustit úlohu Hadoop pomocí Azure Cosmos DB a HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e462-200">Run a Hadoop job using Azure Cosmos DB and HDInsight</span></span>](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

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
