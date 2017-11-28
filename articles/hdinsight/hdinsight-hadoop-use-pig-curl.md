---
title: aaaUse Hadoop Pig s REST v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak toouse REST toorun Pig Latin úloh na Hadoop cluster v Azure HDInsight."
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
ms.openlocfilehash: 760139e3caad9103d8c9d34e7f548d476014b5ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a><span data-ttu-id="28218-103">Spuštění úlohy Pig s Hadoop v HDInsight pomocí REST</span><span class="sxs-lookup"><span data-stu-id="28218-103">Run Pig jobs with Hadoop on HDInsight by using REST</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="28218-104">Zjistěte, jak toorun Pig Latin úlohy tak, že cluster Azure HDInsight tooan požadavky REST.</span><span class="sxs-lookup"><span data-stu-id="28218-104">Learn how toorun Pig Latin jobs by making REST requests tooan Azure HDInsight cluster.</span></span> <span data-ttu-id="28218-105">Curl je použité toodemonstrate, jak mohou komunikovat s HDInsight pomocí hello WebHCat REST API.</span><span class="sxs-lookup"><span data-stu-id="28218-105">Curl is used toodemonstrate how you can interact with HDInsight using hello WebHCat REST API.</span></span>

> [!NOTE]
> <span data-ttu-id="28218-106">Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma [systémem Linux HDInsight tipy](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="28218-106">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see [Linux-based HDInsight Tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="28218-107"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="28218-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="28218-108">Clusteru Azure HDInsight (Hadoop v HDInsight) (systémem Linux nebo systému Windows)</span><span class="sxs-lookup"><span data-stu-id="28218-108">An Azure HDInsight (Hadoop on HDInsight) cluster (Linux-based or Windows-based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="28218-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="28218-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="28218-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="28218-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* [<span data-ttu-id="28218-111">Curl</span><span class="sxs-lookup"><span data-stu-id="28218-111">Curl</span></span>](http://curl.haxx.se/)

* [<span data-ttu-id="28218-112">jq</span><span class="sxs-lookup"><span data-stu-id="28218-112">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="28218-113"><a id="curl"></a>Spuštění úlohy Pig pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="28218-113"><a id="curl"></a>Run Pig jobs by using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="28218-114">Hello rozhraní API REST je zabezpečeno pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="28218-114">hello REST API is secured via [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="28218-115">Vždy proveďte požadavky pomocí přihlašovacích údajů jsou odeslány bezpečně toohello server tooensure Secure HTTP (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="28218-115">Always make requests by using Secure HTTP (HTTPS) tooensure that your credentials are securely sent toohello server.</span></span>
>
> <span data-ttu-id="28218-116">Když používáte hello příkazy v této části, vyměňte `USERNAME` s hello uživatele tooauthenticate toohello clusteru a nahraďte `PASSWORD` s hello heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="28218-116">When using hello commands in this section, replace `USERNAME` with hello user tooauthenticate toohello cluster, and replace `PASSWORD` with hello password for hello user account.</span></span> <span data-ttu-id="28218-117">Nahraďte `CLUSTERNAME` s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="28218-117">Replace `CLUSTERNAME` with hello name of your cluster.</span></span>
>


1. <span data-ttu-id="28218-118">Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:</span><span class="sxs-lookup"><span data-stu-id="28218-118">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="28218-119">Měli byste obdržet hello následující odpověď JSON:</span><span class="sxs-lookup"><span data-stu-id="28218-119">You should receive hello following JSON response:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="28218-120">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="28218-120">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="28218-121">**-u**: hello uživatelské jméno a heslo použít tooauthenticate hello požadavku</span><span class="sxs-lookup"><span data-stu-id="28218-121">**-u**: hello user name and password used tooauthenticate hello request</span></span>
    * <span data-ttu-id="28218-122">**-G**: označuje, že tento požadavek je požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="28218-122">**-G**: Indicates that this request is a GET request</span></span>

     <span data-ttu-id="28218-123">Dobrý den, od adresy URL hello, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello stejné pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="28218-123">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span> <span data-ttu-id="28218-124">Cesta Hello **/status**, signalizuje tento požadavek hello tooreturn hello stav WebHCat (také známé jako Templeton) pro hello server.</span><span class="sxs-lookup"><span data-stu-id="28218-124">hello path, **/status**, indicates that hello request is tooreturn hello status of WebHCat (also known as Templeton) for hello server.</span></span>

2. <span data-ttu-id="28218-125">Použijte následující kód toosubmit clusteru toohello úlohy Pig Latin hello:</span><span class="sxs-lookup"><span data-stu-id="28218-125">Use hello following code toosubmit a Pig Latin job toohello cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    <span data-ttu-id="28218-126">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="28218-126">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="28218-127">**-d**: protože `-G` se nepoužívá, žádost hello výchozí toohello metodu POST.</span><span class="sxs-lookup"><span data-stu-id="28218-127">**-d**: Because `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="28218-128">`-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="28218-128">`-d` specifies hello data values that are sent with hello request.</span></span>

    * <span data-ttu-id="28218-129">**User.Name**: hello uživateli, který spouští příkaz hello</span><span class="sxs-lookup"><span data-stu-id="28218-129">**user.name**: hello user who is running hello command</span></span>
    * <span data-ttu-id="28218-130">**spuštění**: hello Pig Latin příkazy tooexecute</span><span class="sxs-lookup"><span data-stu-id="28218-130">**execute**: hello Pig Latin statements tooexecute</span></span>
    * <span data-ttu-id="28218-131">**statusdir**: hello adresář, který hello stavu pro tuto úlohu je zapsán do</span><span class="sxs-lookup"><span data-stu-id="28218-131">**statusdir**: hello directory that hello status for this job is written to</span></span>

    > [!NOTE]
    > <span data-ttu-id="28218-132">Všimněte si, že hello prostory v příkazech Pig Latin jsou nahrazovány hello `+` znak při použití s Curl.</span><span class="sxs-lookup"><span data-stu-id="28218-132">Notice that hello spaces in Pig Latin statements are replaced by hello `+` character when used with Curl.</span></span>

    <span data-ttu-id="28218-133">Tento příkaz by měla vrátit ID úlohy, které můžou být použité toocheck hello stav hello úlohy, například:</span><span class="sxs-lookup"><span data-stu-id="28218-133">This command should return a job ID that can be used toocheck hello status of hello job, for example:</span></span>

        {"id":"job_1415651640909_0026"}

3. <span data-ttu-id="28218-134">Stav hello toocheck hello úlohy, hello použijte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="28218-134">toocheck hello status of hello job, use hello following command</span></span>

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     <span data-ttu-id="28218-135">Nahraďte `JOBID` s hodnotou hello vráceném v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="28218-135">Replace `JOBID` with hello value returned in hello previous step.</span></span> <span data-ttu-id="28218-136">Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak `JOBID` je `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="28218-136">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then `JOBID` is `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="28218-137">Pokud hello úloha dokončí, je stav hello **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="28218-137">If hello job has finished, hello state is **SUCCEEDED**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="28218-138">Tento požadavek Curl vrací JavaScript Object Notation (JSON) dokumentu s informacemi o hello úlohy a jq je použité tooretrieve hello pouze hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="28218-138">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job, and jq is used tooretrieve only hello state value.</span></span>

## <span data-ttu-id="28218-139"><a id="results"></a>Zobrazení výsledků</span><span class="sxs-lookup"><span data-stu-id="28218-139"><a id="results"></a>View results</span></span>

<span data-ttu-id="28218-140">Pokud stav hello hello úlohy se změnil příliš**úspěšné**, můžete načíst výsledky hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="28218-140">When hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job.</span></span> <span data-ttu-id="28218-141">Hello `statusdir` parametr předaný s hello dotazu obsahuje hello umístění výstupního souboru hello; v tomto případě `/example/pigcurl`.</span><span class="sxs-lookup"><span data-stu-id="28218-141">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, `/example/pigcurl`.</span></span>

<span data-ttu-id="28218-142">HDInsight, můžete použít jako úložiště dat výchozí hello Azure Storage nebo Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="28218-142">HDInsight can use either Azure Storage or Azure Data Lake Store as hello default data store.</span></span> <span data-ttu-id="28218-143">Existují různé způsoby tooget v hello dat podle toho, který používáte.</span><span class="sxs-lookup"><span data-stu-id="28218-143">There are various ways tooget at hello data depending on which one you use.</span></span> <span data-ttu-id="28218-144">Další informace najdete v tématu hello úložiště v tématu hello [HDInsight se systémem Linux informace](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="28218-144">For more information, see hello storage section of hello [Linux-based HDInsight information](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) document.</span></span>

## <span data-ttu-id="28218-145"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="28218-145"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="28218-146">Jak je ukázáno v tomto dokumentu, můžete použít nezpracovaná toorun požadavku HTTP, monitorování a zobrazení výsledků hello Pig úloh na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="28218-146">As demonstrated in this document, you can use a raw HTTP request toorun, monitor, and view hello results of Pig jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="28218-147">Další informace o rozhraní REST hello používané v tomto článku najdete v tématu hello [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="28218-147">For more information about hello REST interface used in this article, see hello [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>

## <span data-ttu-id="28218-148"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="28218-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="28218-149">Obecné informace o Pig v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="28218-149">For general information about Pig on HDInsight:</span></span>

* [<span data-ttu-id="28218-150">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="28218-150">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="28218-151">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="28218-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="28218-152">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="28218-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="28218-153">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="28218-153">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
