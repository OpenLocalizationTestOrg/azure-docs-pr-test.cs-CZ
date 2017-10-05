---
title: "Použití MapReduce a Curl s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Naučte se vzdáleně spouštět úlohy MapReduce s Hadoop v HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 8238bb829df95dcb8c99c0b7fff53c627a56f47c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a><span data-ttu-id="c62d3-103">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí REST</span><span class="sxs-lookup"><span data-stu-id="c62d3-103">Run MapReduce jobs with Hadoop on HDInsight using REST</span></span>

<span data-ttu-id="c62d3-104">Naučte se používat rozhraní REST API WebHCat ke spuštění úloh MapReduce systému Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c62d3-104">Learn how to use the WebHCat REST API to run MapReduce jobs on a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="c62d3-105">Curl se používá k ukazují, jak můžete pracovat s HDInsight pomocí nezpracovaná požadavků HTTP ke spuštění úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="c62d3-105">Curl is used to demonstrate how you can interact with HDInsight by using raw HTTP requests to run MapReduce jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="c62d3-106">Pokud jste již obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou pro vás nové do HDInsight, najdete v článku [co potřebujete vědět o systémem Linux Hadoop v HDInsight](hdinsight-hadoop-linux-information.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c62d3-106">If you are already familiar with using Linux-based Hadoop servers, but you are new to HDInsight, see the [What you need to know about Linux-based Hadoop on HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>


## <span data-ttu-id="c62d3-107"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="c62d3-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="c62d3-108">Na clusteru HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c62d3-108">A Hadoop on HDInsight cluster</span></span>
* [<span data-ttu-id="c62d3-109">Curl</span><span class="sxs-lookup"><span data-stu-id="c62d3-109">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="c62d3-110">jq</span><span class="sxs-lookup"><span data-stu-id="c62d3-110">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="c62d3-111"><a id="curl"></a>Spuštění úloh MapReduce pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="c62d3-111"><a id="curl"></a>Run MapReduce jobs using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="c62d3-112">Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména správce clusteru HDInsight a hesla.</span><span class="sxs-lookup"><span data-stu-id="c62d3-112">When you use Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the HDInsight cluster administrator user name and password.</span></span> <span data-ttu-id="c62d3-113">Název clusteru musí používat jako součást identifikátoru URI, který se používá k odesílání požadavků na server.</span><span class="sxs-lookup"><span data-stu-id="c62d3-113">You must use the cluster name as part of the URI that is used to send the requests to the server.</span></span>
>
> <span data-ttu-id="c62d3-114">Pro příkazy v této části nahraďte **uživatelské jméno** uživatelem pro ověření clusteru, a **heslo** s heslem pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="c62d3-114">For the commands in this section, replace **USERNAME** with the user to authenticate to the cluster, and **PASSWORD** with the password for the user account.</span></span> <span data-ttu-id="c62d3-115">Nahraďte **CLUSTERNAME** názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="c62d3-115">Replace **CLUSTERNAME** with the name of your cluster.</span></span>
>
> <span data-ttu-id="c62d3-116">Rozhraní API REST je zabezpečená pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="c62d3-116">The REST API is secured by using [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="c62d3-117">Vždy doporučujeme provádět požadavky pomocí protokolu HTTPS k zajištění, že vaše přihlašovací údaje jsou bezpečně odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="c62d3-117">You should always make requests by using HTTPS to ensure that your credentials are securely sent to the server.</span></span>


1. <span data-ttu-id="c62d3-118">Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c62d3-118">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="c62d3-119">Mělo by se zobrazit odpověď podobná následujícím kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="c62d3-119">You should receive a response similar to the following JSON:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="c62d3-120">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="c62d3-120">The parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="c62d3-121">**-u**: Určuje uživatelské jméno a heslo použité k ověření žádosti</span><span class="sxs-lookup"><span data-stu-id="c62d3-121">**-u**: Indicates the user name and password used to authenticate the request</span></span>
   * <span data-ttu-id="c62d3-122">**-G**: označuje, že tato operace je požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="c62d3-122">**-G**: Indicates that this operation is a GET request</span></span>

     <span data-ttu-id="c62d3-123">Na začátek identifikátor URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, je stejný pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="c62d3-123">The beginning of the URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is the same for all requests.</span></span>

2. <span data-ttu-id="c62d3-124">Chcete-li odeslat úlohu MapReduce, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c62d3-124">To submit a MapReduce job, use the following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    <span data-ttu-id="c62d3-125">Konec identifikátor URI (/ mapreduce/jar) informuje WebHCat, že tento požadavek spustí úlohu MapReduce z třídy v souboru jar.</span><span class="sxs-lookup"><span data-stu-id="c62d3-125">The end of the URI (/mapreduce/jar) tells WebHCat that this request starts a MapReduce job from a class in a jar file.</span></span> <span data-ttu-id="c62d3-126">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="c62d3-126">The parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="c62d3-127">**-d**: `-G` se nepoužívá, takže požadavek výchozí metodu POST.</span><span class="sxs-lookup"><span data-stu-id="c62d3-127">**-d**: `-G` is not used, so the request defaults to the POST method.</span></span> <span data-ttu-id="c62d3-128">`-d`Určuje datových hodnot, které se odesílají s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="c62d3-128">`-d` specifies the data values that are sent with the request.</span></span>
    * <span data-ttu-id="c62d3-129">**User.Name**: uživatel, který spouští příkaz</span><span class="sxs-lookup"><span data-stu-id="c62d3-129">**user.name**: The user who is running the command</span></span>
    * <span data-ttu-id="c62d3-130">**JAR**: umístění na soubor jar, který obsahuje třídu být spustili</span><span class="sxs-lookup"><span data-stu-id="c62d3-130">**jar**: The location of the jar file that contains class to be ran</span></span>
    * <span data-ttu-id="c62d3-131">**Třída**: třídu, která obsahuje logiku MapReduce</span><span class="sxs-lookup"><span data-stu-id="c62d3-131">**class**: The class that contains the MapReduce logic</span></span>
    * <span data-ttu-id="c62d3-132">**arg**: argumenty, které mají být předána do úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="c62d3-132">**arg**: The arguments to be passed to the MapReduce job.</span></span> <span data-ttu-id="c62d3-133">V tomto případě, vstupní textového souboru a adresáře, které se používají pro výstup</span><span class="sxs-lookup"><span data-stu-id="c62d3-133">In this case, the input text file and the directory that are used for the output</span></span>

     <span data-ttu-id="c62d3-134">Tento příkaz by měla vrátit ID úlohy, který slouží ke kontrole stavu úlohy:</span><span class="sxs-lookup"><span data-stu-id="c62d3-134">This command should return a job ID that can be used to check the status of the job:</span></span>

       <span data-ttu-id="c62d3-135">{"id": "job_1415651640909_0026"}</span><span class="sxs-lookup"><span data-stu-id="c62d3-135">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="c62d3-136">Pokud chcete zkontrolovat stav úlohy, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c62d3-136">To check the status of the job, use the following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="c62d3-137">Nahraďte **JOBID** se hodnota vrácená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="c62d3-137">Replace the **JOBID** with the value returned in the previous step.</span></span> <span data-ttu-id="c62d3-138">Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak by byl JOBID `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-138">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then the JOBID would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="c62d3-139">Pokud tato úloha skončí, vrácená stav `SUCCEEDED`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-139">If the job is complete, the state returned is `SUCCEEDED`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c62d3-140">Tento požadavek Curl vrátí dokumentu JSON s informacemi o úloze.</span><span class="sxs-lookup"><span data-stu-id="c62d3-140">This Curl request returns a JSON document with information about the job.</span></span> <span data-ttu-id="c62d3-141">Jq se používá k načtení jenom hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="c62d3-141">Jq is used to retrieve only the state value.</span></span>

4. <span data-ttu-id="c62d3-142">Pokud se změnila stav úlohy na `SUCCEEDED`, můžete načíst výsledky úlohy z Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c62d3-142">When the state of the job has changed to `SUCCEEDED`, you can retrieve the results of the job from Azure Blob storage.</span></span> <span data-ttu-id="c62d3-143">`statusdir` Parametr, který je předán s dotaz obsahuje umístění výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="c62d3-143">The `statusdir` parameter that is passed with the query contains the location of the output file.</span></span> <span data-ttu-id="c62d3-144">V tomto příkladu je umístění `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-144">In this example, the location is `/example/curl`.</span></span> <span data-ttu-id="c62d3-145">Tuto adresu ukládá výstup úlohy do úložiště výchozí clustery na `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-145">This address stores the output of the job in the clusters default storage at `/example/curl`.</span></span>

<span data-ttu-id="c62d3-146">Můžete zobrazit seznam a stáhnout tyto soubory pomocí [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c62d3-146">You can list and download these files by using the [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="c62d3-147">Další informace o práci s objekty BLOB z příkazového řádku Azure najdete v tématu [použití Azure CLI 2.0 s Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c62d3-147">For more information on working with blobs from the Azure CLI, see the [Using the Azure CLI 2.0 with Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.</span></span>

## <span data-ttu-id="c62d3-148"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="c62d3-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="c62d3-149">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c62d3-149">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="c62d3-150">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c62d3-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="c62d3-151">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c62d3-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="c62d3-152">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c62d3-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="c62d3-153">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c62d3-153">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="c62d3-154">Další informace o rozhraní REST, který se používá v tomto článku najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="c62d3-154">For more information about the REST interface that is used in this article, see the [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>