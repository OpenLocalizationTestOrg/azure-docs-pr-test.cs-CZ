---
title: "Použijte Hadoop Pig s REST v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak se použije ke spuštění úlohy Pig Latin na cluster Hadoop v prostředí Azure HDInsight REST."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: a86864a779b0de1c6d5669cfbba0f3e1a27f1ff1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a><span data-ttu-id="239bc-103">Spuštění úlohy Pig s Hadoop v HDInsight pomocí REST</span><span class="sxs-lookup"><span data-stu-id="239bc-103">Run Pig jobs with Hadoop on HDInsight by using REST</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="239bc-104">Zjistěte, jak spouštět úlohy Pig Latin tím, že požadavky REST do clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="239bc-104">Learn how to run Pig Latin jobs by making REST requests to an Azure HDInsight cluster.</span></span> <span data-ttu-id="239bc-105">Curl se používá k ukazují, jak mohou komunikovat s HDInsight pomocí rozhraní REST API WebHCat.</span><span class="sxs-lookup"><span data-stu-id="239bc-105">Curl is used to demonstrate how you can interact with HDInsight using the WebHCat REST API.</span></span>

> [!NOTE]
> <span data-ttu-id="239bc-106">Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové do HDInsight, projděte si téma [systémem Linux HDInsight tipy](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="239bc-106">If you are already familiar with using Linux-based Hadoop servers, but are new to HDInsight, see [Linux-based HDInsight Tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="239bc-107"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="239bc-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="239bc-108">Clusteru Azure HDInsight (Hadoop v HDInsight) (systémem Linux nebo systému Windows)</span><span class="sxs-lookup"><span data-stu-id="239bc-108">An Azure HDInsight (Hadoop on HDInsight) cluster (Linux-based or Windows-based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="239bc-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="239bc-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="239bc-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="239bc-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="239bc-111">Curl</span><span class="sxs-lookup"><span data-stu-id="239bc-111">Curl</span></span>](http://curl.haxx.se/)

* [<span data-ttu-id="239bc-112">jq</span><span class="sxs-lookup"><span data-stu-id="239bc-112">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="239bc-113"><a id="curl"></a>Spuštění úlohy Pig pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="239bc-113"><a id="curl"></a>Run Pig jobs by using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="239bc-114">Rozhraní API REST je zabezpečeno pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="239bc-114">The REST API is secured via [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="239bc-115">Vždy proveďte požadavky pomocí Secure HTTP (HTTPS) k zajištění, že vaše přihlašovací údaje jsou bezpečně odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="239bc-115">Always make requests by using Secure HTTP (HTTPS) to ensure that your credentials are securely sent to the server.</span></span>
>
> <span data-ttu-id="239bc-116">Když pomocí příkazů v této části, vyměňte `USERNAME` uživatelem pro ověření clusteru a nahraďte `PASSWORD` s heslem pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="239bc-116">When using the commands in this section, replace `USERNAME` with the user to authenticate to the cluster, and replace `PASSWORD` with the password for the user account.</span></span> <span data-ttu-id="239bc-117">Nahraďte `CLUSTERNAME` názvem svého clusteru.</span><span class="sxs-lookup"><span data-stu-id="239bc-117">Replace `CLUSTERNAME` with the name of your cluster.</span></span>
>


1. <span data-ttu-id="239bc-118">Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="239bc-118">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="239bc-119">Mělo by se zobrazit následující odpověď JSON:</span><span class="sxs-lookup"><span data-stu-id="239bc-119">You should receive the following JSON response:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="239bc-120">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="239bc-120">The parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="239bc-121">**-u**: uživatelské jméno a heslo použité k ověření žádosti</span><span class="sxs-lookup"><span data-stu-id="239bc-121">**-u**: The user name and password used to authenticate the request</span></span>
    * <span data-ttu-id="239bc-122">**-G**: označuje, že tento požadavek je požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="239bc-122">**-G**: Indicates that this request is a GET request</span></span>

     <span data-ttu-id="239bc-123">Na začátek adresu URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, je stejný pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="239bc-123">The beginning of the URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is the same for all requests.</span></span> <span data-ttu-id="239bc-124">Cesta **/status**, označuje, že požadavek je vrátit stav WebHCat (také známé jako Templeton) pro server.</span><span class="sxs-lookup"><span data-stu-id="239bc-124">The path, **/status**, indicates that the request is to return the status of WebHCat (also known as Templeton) for the server.</span></span>

2. <span data-ttu-id="239bc-125">Použijte následující kód se odeslat úlohu Pig Latin do clusteru:</span><span class="sxs-lookup"><span data-stu-id="239bc-125">Use the following code to submit a Pig Latin job to the cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    <span data-ttu-id="239bc-126">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="239bc-126">The parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="239bc-127">**-d**: protože `-G` se nepoužívá, použije se výchozí hodnota žádost na metodu POST.</span><span class="sxs-lookup"><span data-stu-id="239bc-127">**-d**: Because `-G` is not used, the request defaults to the POST method.</span></span> <span data-ttu-id="239bc-128">`-d`Určuje datových hodnot, které se odesílají s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="239bc-128">`-d` specifies the data values that are sent with the request.</span></span>

    * <span data-ttu-id="239bc-129">**User.Name**: uživatel, který spouští příkaz</span><span class="sxs-lookup"><span data-stu-id="239bc-129">**user.name**: The user who is running the command</span></span>
    * <span data-ttu-id="239bc-130">**spuštění**: příkazy Pig Latin provést</span><span class="sxs-lookup"><span data-stu-id="239bc-130">**execute**: The Pig Latin statements to execute</span></span>
    * <span data-ttu-id="239bc-131">**statusdir**: adresář, který stav pro tuto úlohu je zapsán do</span><span class="sxs-lookup"><span data-stu-id="239bc-131">**statusdir**: The directory that the status for this job is written to</span></span>

    > [!NOTE]
    > <span data-ttu-id="239bc-132">Všimněte si, že jsou nahrazovány prostory v příkazech Pig Latin `+` znak při použití s Curl.</span><span class="sxs-lookup"><span data-stu-id="239bc-132">Notice that the spaces in Pig Latin statements are replaced by the `+` character when used with Curl.</span></span>

    <span data-ttu-id="239bc-133">Tento příkaz by měla vrátit ID úlohy, které je možné zkontrolovat stav úlohy, například:</span><span class="sxs-lookup"><span data-stu-id="239bc-133">This command should return a job ID that can be used to check the status of the job, for example:</span></span>

        {"id":"job_1415651640909_0026"}

3. <span data-ttu-id="239bc-134">Pokud chcete zkontrolovat stav úlohy, použijte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="239bc-134">To check the status of the job, use the following command</span></span>

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     <span data-ttu-id="239bc-135">Nahraďte `JOBID` se hodnota vrácená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="239bc-135">Replace `JOBID` with the value returned in the previous step.</span></span> <span data-ttu-id="239bc-136">Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak `JOBID` je `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="239bc-136">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then `JOBID` is `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="239bc-137">Pokud se úloha dokončí, je stav **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="239bc-137">If the job has finished, the state is **SUCCEEDED**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="239bc-138">Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informace o úloze a jq se používá k načtení jenom hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="239bc-138">This Curl request returns a JavaScript Object Notation (JSON) document with information about the job, and jq is used to retrieve only the state value.</span></span>

## <span data-ttu-id="239bc-139"><a id="results"></a>Zobrazení výsledků</span><span class="sxs-lookup"><span data-stu-id="239bc-139"><a id="results"></a>View results</span></span>

<span data-ttu-id="239bc-140">Pokud se změnila stav úlohy na **úspěšné**, můžete načíst výsledky úlohy.</span><span class="sxs-lookup"><span data-stu-id="239bc-140">When the state of the job has changed to **SUCCEEDED**, you can retrieve the results of the job.</span></span> <span data-ttu-id="239bc-141">`statusdir` Parametr předaný s dotaz obsahuje umístění výstupního souboru; v tomto případě `/example/pigcurl`.</span><span class="sxs-lookup"><span data-stu-id="239bc-141">The `statusdir` parameter passed with the query contains the location of the output file; in this case, `/example/pigcurl`.</span></span>

<span data-ttu-id="239bc-142">HDInsight, můžete použít jako výchozího datového úložiště Azure Storage nebo Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="239bc-142">HDInsight can use either Azure Storage or Azure Data Lake Store as the default data store.</span></span> <span data-ttu-id="239bc-143">Existují různé způsoby, jak získat na data, podle toho, který používáte.</span><span class="sxs-lookup"><span data-stu-id="239bc-143">There are various ways to get at the data depending on which one you use.</span></span> <span data-ttu-id="239bc-144">Další informace najdete v části úložiště [HDInsight se systémem Linux informace](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="239bc-144">For more information, see the storage section of the [Linux-based HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) document.</span></span>

## <span data-ttu-id="239bc-145"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="239bc-145"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="239bc-146">Jak je ukázáno v tomto dokumentu, můžete spouštět, monitorovat a zobrazit výsledky úlohy Pig v clusteru HDInsight nezpracovaná požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="239bc-146">As demonstrated in this document, you can use a raw HTTP request to run, monitor, and view the results of Pig jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="239bc-147">Další informace o rozhraní REST používané v tomto článku najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="239bc-147">For more information about the REST interface used in this article, see the [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>

## <span data-ttu-id="239bc-148"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="239bc-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="239bc-149">Obecné informace o Pig v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="239bc-149">For general information about Pig on HDInsight:</span></span>

* [<span data-ttu-id="239bc-150">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="239bc-150">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="239bc-151">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="239bc-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="239bc-152">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="239bc-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="239bc-153">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="239bc-153">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
