---
title: "Používání Hadoop Hive s Curl v HDInsight - Azure | Microsoft Docs"
description: "Naučte se vzdáleně odesílání úloh Pig do HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 8a4f217b046121f85be0585eab18d90c44f21b9e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a><span data-ttu-id="e6223-103">Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí REST</span><span class="sxs-lookup"><span data-stu-id="e6223-103">Run Hive queries with Hadoop in HDInsight using REST</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="e6223-104">Naučte se používat rozhraní REST API WebHCat ke spouštění dotazů Hive se systémem Hadoop v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e6223-104">Learn how to use the WebHCat REST API to run Hive queries with Hadoop on Azure HDInsight cluster.</span></span>

<span data-ttu-id="e6223-105">[Curl](http://curl.haxx.se/) se používá k ukazují, jak můžete pracovat s HDInsight pomocí nezpracované požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="e6223-105">[Curl](http://curl.haxx.se/) is used to demonstrate how you can interact with HDInsight by using raw HTTP requests.</span></span> <span data-ttu-id="e6223-106">[Jq](http://stedolan.github.io/jq/) nástroj se používá ke zpracování dat JSON vrácená z požadavky REST.</span><span class="sxs-lookup"><span data-stu-id="e6223-106">The [jq](http://stedolan.github.io/jq/) utility is used to process the JSON data returned from REST requests.</span></span>

> [!NOTE]
> <span data-ttu-id="e6223-107">Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové do HDInsight, najdete v článku [co potřebujete vědět o Hadoop v HDInsight se systémem Linux](hdinsight-hadoop-linux-information.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e6223-107">If you are already familiar with using Linux-based Hadoop servers, but are new to HDInsight, see the [What you need to know about Hadoop on Linux-based HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>

## <span data-ttu-id="e6223-108"><a id="curl"></a>Spouštění dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="e6223-108"><a id="curl"></a>Run Hive queries</span></span>

> [!NOTE]
> <span data-ttu-id="e6223-109">Pokud používáte cURL nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména a hesla pro správce clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e6223-109">When using cURL or any other REST communication with WebHCat, you must authenticate the requests by providing the user name and password for the HDInsight cluster administrator.</span></span>
>
> <span data-ttu-id="e6223-110">Pro příkazy v této části nahraďte **UŽIVATELSKÉ JMÉNO** uživatelem pro ověření do clusteru a nahraďte **HESLO** heslem pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="e6223-110">For the commands in this section, replace **USERNAME** with the user to authenticate to the cluster, and replace **PASSWORD** with the password for the user account.</span></span> <span data-ttu-id="e6223-111">Nahraďte **CLUSTERNAME** názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="e6223-111">Replace **CLUSTERNAME** with the name of your cluster.</span></span>
>
> <span data-ttu-id="e6223-112">Rozhraní API REST je zabezpečeno pomocí [základního ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="e6223-112">The REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="e6223-113">K zajištění, že vaše přihlašovací údaje jsou bezpečně odeslat na server, vždy proveďte požadavky pomocí Secure HTTP (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e6223-113">To help ensure that your credentials are securely sent to the server, always make requests by using Secure HTTP (HTTPS).</span></span>

1. <span data-ttu-id="e6223-114">Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e6223-114">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="e6223-115">Můžete zobrazit odpověď podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="e6223-115">You receive a response similar to the following text:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="e6223-116">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="e6223-116">The parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="e6223-117">**-u** – uživatelské jméno a heslo použité pro ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="e6223-117">**-u** - The user name and password used to authenticate the request.</span></span>
   * <span data-ttu-id="e6223-118">**-G** – označuje, že tento požadavek je operaci GET.</span><span class="sxs-lookup"><span data-stu-id="e6223-118">**-G** - Indicates that this request is a GET operation.</span></span>

     <span data-ttu-id="e6223-119">Na začátek adresu URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, je stejný pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="e6223-119">The beginning of the URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is the same for all requests.</span></span> <span data-ttu-id="e6223-120">Cesta **/status**, označuje, že požadavek je vrátit stav WebHCat (také známé jako Templeton) pro server.</span><span class="sxs-lookup"><span data-stu-id="e6223-120">The path, **/status**, indicates that the request is to return a status of WebHCat (also known as Templeton) for the server.</span></span> <span data-ttu-id="e6223-121">Můžete také vyžadovat verzi Hive pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e6223-121">You can also request the version of Hive by using the following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     <span data-ttu-id="e6223-122">Tento požadavek vrátí odpověď podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="e6223-122">This request returns a response similar to the following text:</span></span>

       <span data-ttu-id="e6223-123">{"module": "hive", "verze": "0.13.0.2.1.6.0-2103"}</span><span class="sxs-lookup"><span data-stu-id="e6223-123">{"module":"hive","version":"0.13.0.2.1.6.0-2103"}</span></span>

2. <span data-ttu-id="e6223-124">Následující informace vám pomůžou vytvořit tabulku s názvem **log4jLogs**:</span><span class="sxs-lookup"><span data-stu-id="e6223-124">Use the following to create a table named **log4jLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="e6223-125">Použité s touto žádostí následující parametry:</span><span class="sxs-lookup"><span data-stu-id="e6223-125">The following parameters used with this request:</span></span>

   * <span data-ttu-id="e6223-126">**-d** – od `-G` se nepoužívá, použije se výchozí hodnota žádost na metodu POST.</span><span class="sxs-lookup"><span data-stu-id="e6223-126">**-d** - Since `-G` is not used, the request defaults to the POST method.</span></span> <span data-ttu-id="e6223-127">`-d`Určuje datových hodnot, které se odesílají s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="e6223-127">`-d` specifies the data values that are sent with the request.</span></span>

     * <span data-ttu-id="e6223-128">**User.Name** -uživatel, který spouští příkaz.</span><span class="sxs-lookup"><span data-stu-id="e6223-128">**user.name** - The user that is running the command.</span></span>
     * <span data-ttu-id="e6223-129">**spuštění** -příkazy HiveQL k provedení.</span><span class="sxs-lookup"><span data-stu-id="e6223-129">**execute** - The HiveQL statements to execute.</span></span>
     * <span data-ttu-id="e6223-130">**statusdir** -adresáře, který stav pro tuto úlohu je zapsán do.</span><span class="sxs-lookup"><span data-stu-id="e6223-130">**statusdir** - The directory that the status for this job is written to.</span></span>

     <span data-ttu-id="e6223-131">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="e6223-131">These statements perform the following actions:</span></span>
   * <span data-ttu-id="e6223-132">**VYŘAĎTE tabulku** – Pokud tabulka již existuje, je odstraněn.</span><span class="sxs-lookup"><span data-stu-id="e6223-132">**DROP TABLE** - If the table already exists, it is deleted.</span></span>
   * <span data-ttu-id="e6223-133">**Vytvoření externí tabulky** -vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="e6223-133">**CREATE EXTERNAL TABLE** - Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="e6223-134">Externí tabulky uložit pouze definici tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="e6223-134">External tables store only the table definition in Hive.</span></span> <span data-ttu-id="e6223-135">Data je ponechán v původním umístění.</span><span class="sxs-lookup"><span data-stu-id="e6223-135">The data is left in the original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="e6223-136">Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="e6223-136">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="e6223-137">Například automatizované data nahrát procesu nebo jiná operace MapReduce.</span><span class="sxs-lookup"><span data-stu-id="e6223-137">For example, an automated data upload process or another MapReduce operation.</span></span>
     >
     > <span data-ttu-id="e6223-138">Vyřazení externí tabulku nemá **není** odstranit data, jenom definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="e6223-138">Dropping an external table does **not** delete the data, only the table definition.</span></span>

   * <span data-ttu-id="e6223-139">**Řádek formátu** – jak data formátována.</span><span class="sxs-lookup"><span data-stu-id="e6223-139">**ROW FORMAT** - How the data is formatted.</span></span> <span data-ttu-id="e6223-140">Pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="e6223-140">The fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="e6223-141">**ULOŽENÉ umístění textový soubor AS** – se uloží data (například/datový adresář) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="e6223-141">**STORED AS TEXTFILE LOCATION** - Where the data is stored (the example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="e6223-142">**Vyberte** -počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="e6223-142">**SELECT** - Selects a count of all rows where column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="e6223-143">Tento příkaz vrátí hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e6223-143">This statement returns a value of **3** as there are three rows that contain this value.</span></span>

     > [!NOTE]
     > <span data-ttu-id="e6223-144">Všimněte si, že jsou nahrazovány mezery mezi příkazy HiveQL `+` znak při použití s Curl.</span><span class="sxs-lookup"><span data-stu-id="e6223-144">Notice that the spaces between HiveQL statements are replaced by the `+` character when used with Curl.</span></span> <span data-ttu-id="e6223-145">Hodnoty v uvozovkách, které obsahují mezery, jako je například oddělovač, by neměl být nahrazen `+`.</span><span class="sxs-lookup"><span data-stu-id="e6223-145">Quoted values that contain a space, such as the delimiter, should not be replaced by `+`.</span></span>

   * <span data-ttu-id="e6223-146">**INPUT__FILE__NAME jako '% 25.log'** – tento příkaz omezí výsledky vyhledávání na používat jenom soubory, které končí na. log.</span><span class="sxs-lookup"><span data-stu-id="e6223-146">**INPUT__FILE__NAME LIKE '%25.log'** - This statement limits the search to only use files ending in .log.</span></span>

     > [!NOTE]
     > <span data-ttu-id="e6223-147">`%25` Je kódovaná adresou URL formuláře %, je skutečný stav `like '%.log'`.</span><span class="sxs-lookup"><span data-stu-id="e6223-147">The `%25` is the URL encoded form of %, so the actual condition is `like '%.log'`.</span></span> <span data-ttu-id="e6223-148">% Musí být adresa URL kódování, jako je považován za zvláštní znak v adresách URL.</span><span class="sxs-lookup"><span data-stu-id="e6223-148">The % has to be URL encoded, as it is treated as a special character in URLs.</span></span>

     <span data-ttu-id="e6223-149">Tento příkaz by měl vrátit ID úlohy, který slouží ke kontrole stavu úlohy.</span><span class="sxs-lookup"><span data-stu-id="e6223-149">This command should return a job ID that can be used to check the status of the job.</span></span>

       <span data-ttu-id="e6223-150">{"id": "job_1415651640909_0026"}</span><span class="sxs-lookup"><span data-stu-id="e6223-150">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="e6223-151">Pokud chcete zkontrolovat stav úlohy, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6223-151">To check the status of the job, use the following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="e6223-152">Nahraďte **JOBID** se hodnota vrácená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="e6223-152">Replace **JOBID** with the value returned in the previous step.</span></span> <span data-ttu-id="e6223-153">Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="e6223-153">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="e6223-154">Pokud se úloha dokončí, je stav **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="e6223-154">If the job has finished, the state is **SUCCEEDED**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e6223-155">Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informace o úloze.</span><span class="sxs-lookup"><span data-stu-id="e6223-155">This Curl request returns a JavaScript Object Notation (JSON) document with information about the job.</span></span> <span data-ttu-id="e6223-156">Jq se používá k načtení jenom hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="e6223-156">Jq is used to retrieve only the state value.</span></span>

4. <span data-ttu-id="e6223-157">Jakmile se stav úlohy se změnila na **úspěšné**, můžete načíst výsledky úlohy z Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="e6223-157">Once the state of the job has changed to **SUCCEEDED**, you can retrieve the results of the job from Azure Blob storage.</span></span> <span data-ttu-id="e6223-158">`statusdir` Parametr předaný s dotaz obsahuje umístění výstupního souboru; v tomto případě **/příklad/curl**.</span><span class="sxs-lookup"><span data-stu-id="e6223-158">The `statusdir` parameter passed with the query contains the location of the output file; in this case, **/example/curl**.</span></span> <span data-ttu-id="e6223-159">Tato adresa ukládá výstup v **příklad/curl** adresáře v úložišti výchozí clustery.</span><span class="sxs-lookup"><span data-stu-id="e6223-159">This address stores the output in the **example/curl** directory in the clusters default storage.</span></span>

    <span data-ttu-id="e6223-160">Můžete zobrazit seznam a stáhnout tyto soubory pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e6223-160">You can list and download these files by using the [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="e6223-161">Další informace o používání rozhraní příkazového řádku Azure s Azure Storage najdete v tématu [použití Azure CLI 2.0 s Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e6223-161">For more information on using the Azure CLI with Azure Storage, see the [Use Azure CLI 2.0 with Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) document.</span></span>

5. <span data-ttu-id="e6223-162">Použít následující příkazy k vytvoření nové "interní" tabulku s názvem **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="e6223-162">Use the following statements to create a new 'internal' table named **errorLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="e6223-163">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="e6223-163">These statements perform the following actions:</span></span>

   * <span data-ttu-id="e6223-164">**Vytvoření tabulka není v případě existuje** -vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e6223-164">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="e6223-165">Tento příkaz vytvoří interní tabulku, která je uložena v datovém skladu Hive a je zcela spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="e6223-165">This statement creates an internal table, which is stored in the Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="e6223-166">Na rozdíl od externích tabulek se odstraní vyřazení interní tabulku v základních datech.</span><span class="sxs-lookup"><span data-stu-id="e6223-166">Unlike external tables, dropping an internal table deletes the underlying data as well.</span></span>

   * <span data-ttu-id="e6223-167">**ULOŽENÉ ORC AS** – ukládá data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="e6223-167">**STORED AS ORC** - Stores the data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="e6223-168">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="e6223-168">ORC is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="e6223-169">**PŘEPSAT INSERT... Vyberte** -vybere řádky z **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vkládá data do **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="e6223-169">**INSERT OVERWRITE ... SELECT** - Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.</span></span>
   * <span data-ttu-id="e6223-170">**Vyberte** -vybere všechny řádky z nové **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="e6223-170">**SELECT** - Selects all rows from the new **errorLogs** table.</span></span>

6. <span data-ttu-id="e6223-171">Použijte úlohu s ID vrátil a zkontrolujte stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="e6223-171">Use the job ID returned to check the status of the job.</span></span> <span data-ttu-id="e6223-172">Jakmile ho proběhla úspěšně, použijte rozhraní příkazového řádku Azure jak je popsáno dříve ke stažení a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="e6223-172">Once it has succeeded, use the Azure CLI as described previously to download and view the results.</span></span> <span data-ttu-id="e6223-173">Výstup by měl obsahovat tři řádky, které obsahují **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="e6223-173">The output should contain three lines, all of which contain **[ERROR]**.</span></span>

## <span data-ttu-id="e6223-174"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6223-174"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="e6223-175">Obecné informace o Hive s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e6223-175">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="e6223-176">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6223-176">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="e6223-177">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e6223-177">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="e6223-178">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6223-178">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="e6223-179">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6223-179">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="e6223-180">Pokud používáte s Hive Tez, najdete v následujících dokumentech pro ladění informace:</span><span class="sxs-lookup"><span data-stu-id="e6223-180">If you are using Tez with Hive, see the following documents for debugging information:</span></span>

* [<span data-ttu-id="e6223-181">Použití zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="e6223-181">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

<span data-ttu-id="e6223-182">Další informace o rozhraní API REST v tomto dokumentu najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e6223-182">For more information on the REST API used in this document, see the [WebHCat reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.</span></span>

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


