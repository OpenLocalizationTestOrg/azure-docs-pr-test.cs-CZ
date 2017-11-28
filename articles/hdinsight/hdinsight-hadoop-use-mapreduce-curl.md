---
title: aaaUse MapReduce a Curl s Hadoop v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak tooremotely spuštění úloh MapReduce s Hadoop v HDInsight pomocí Curl."
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
ms.openlocfilehash: 16920205bacf9699f88090568099e0508a172b3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a><span data-ttu-id="0de84-103">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí REST</span><span class="sxs-lookup"><span data-stu-id="0de84-103">Run MapReduce jobs with Hadoop on HDInsight using REST</span></span>

<span data-ttu-id="0de84-104">Zjistěte, jak toouse hello WebHCat REST API toorun MapReduce úlohy systému Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0de84-104">Learn how toouse hello WebHCat REST API toorun MapReduce jobs on a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="0de84-105">Curl je použité toodemonstrate, jak můžete pracovat s HDInsight pomocí nezpracovaná úlohy MapReduce toorun požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="0de84-105">Curl is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests toorun MapReduce jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="0de84-106">Pokud jste již obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma hello [co potřebujete tooknow o systémem Linux Hadoop v HDInsight](hdinsight-hadoop-linux-information.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0de84-106">If you are already familiar with using Linux-based Hadoop servers, but you are new tooHDInsight, see hello [What you need tooknow about Linux-based Hadoop on HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>


## <span data-ttu-id="0de84-107"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="0de84-107"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="0de84-108">Na clusteru HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="0de84-108">A Hadoop on HDInsight cluster</span></span>
* [<span data-ttu-id="0de84-109">Curl</span><span class="sxs-lookup"><span data-stu-id="0de84-109">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="0de84-110">jq</span><span class="sxs-lookup"><span data-stu-id="0de84-110">jq</span></span>](http://stedolan.github.io/jq/)

## <span data-ttu-id="0de84-111"><a id="curl"></a>Spuštění úloh MapReduce pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="0de84-111"><a id="curl"></a>Run MapReduce jobs using Curl</span></span>

> [!NOTE]
> <span data-ttu-id="0de84-112">Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello tím, že poskytuje hello HDInsight clusteru správce uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="0de84-112">When you use Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello HDInsight cluster administrator user name and password.</span></span> <span data-ttu-id="0de84-113">Hello název clusteru musí používat jako součást hello identifikátor URI, který je použité toosend hello požadavky toohello server.</span><span class="sxs-lookup"><span data-stu-id="0de84-113">You must use hello cluster name as part of hello URI that is used toosend hello requests toohello server.</span></span>
>
> <span data-ttu-id="0de84-114">Hello příkazy v této části, nahraďte **uživatelské jméno** s hello uživatele tooauthenticate toohello clusteru, a **heslo** s hello heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="0de84-114">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="0de84-115">Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="0de84-115">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
>
> <span data-ttu-id="0de84-116">Hello REST API zabezpečené pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="0de84-116">hello REST API is secured by using [basic access authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="0de84-117">Vždy doporučujeme provádět požadavky pomocí protokolu HTTPS tooensure vaše přihlašovací údaje jsou odeslány bezpečně toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="0de84-117">You should always make requests by using HTTPS tooensure that your credentials are securely sent toohello server.</span></span>


1. <span data-ttu-id="0de84-118">Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:</span><span class="sxs-lookup"><span data-stu-id="0de84-118">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="0de84-119">Mělo by se zobrazit odpověď podobná toohello následující JSON:</span><span class="sxs-lookup"><span data-stu-id="0de84-119">You should receive a response similar toohello following JSON:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="0de84-120">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="0de84-120">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="0de84-121">**-u**: Určuje hello uživatelské jméno a heslo použít tooauthenticate hello požadavku</span><span class="sxs-lookup"><span data-stu-id="0de84-121">**-u**: Indicates hello user name and password used tooauthenticate hello request</span></span>
   * <span data-ttu-id="0de84-122">**-G**: označuje, že tato operace je požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="0de84-122">**-G**: Indicates that this operation is a GET request</span></span>

     <span data-ttu-id="0de84-123">Dobrý den zahájení hello identifikátor URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello stejné pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="0de84-123">hello beginning of hello URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span>

2. <span data-ttu-id="0de84-124">toosubmit úlohu MapReduce hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0de84-124">toosubmit a MapReduce job, use hello following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    <span data-ttu-id="0de84-125">Hello konec hello identifikátor URI (/ mapreduce/jar) informuje WebHCat, že tento požadavek spustí úlohu MapReduce z třídy v souboru jar.</span><span class="sxs-lookup"><span data-stu-id="0de84-125">hello end of hello URI (/mapreduce/jar) tells WebHCat that this request starts a MapReduce job from a class in a jar file.</span></span> <span data-ttu-id="0de84-126">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="0de84-126">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="0de84-127">**-d**: `-G` se nepoužívá, takže hello požadavek výchozí metodu POST toohello.</span><span class="sxs-lookup"><span data-stu-id="0de84-127">**-d**: `-G` is not used, so hello request defaults toohello POST method.</span></span> <span data-ttu-id="0de84-128">`-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="0de84-128">`-d` specifies hello data values that are sent with hello request.</span></span>
    * <span data-ttu-id="0de84-129">**User.Name**: hello uživateli, který spouští příkaz hello</span><span class="sxs-lookup"><span data-stu-id="0de84-129">**user.name**: hello user who is running hello command</span></span>
    * <span data-ttu-id="0de84-130">**JAR**: spustili hello umístění soubor jar hello, který obsahuje toobe – třída</span><span class="sxs-lookup"><span data-stu-id="0de84-130">**jar**: hello location of hello jar file that contains class toobe ran</span></span>
    * <span data-ttu-id="0de84-131">**Třída**: hello třídu, která obsahuje logiku MapReduce hello</span><span class="sxs-lookup"><span data-stu-id="0de84-131">**class**: hello class that contains hello MapReduce logic</span></span>
    * <span data-ttu-id="0de84-132">**arg**: toobe argumenty hello předán toohello úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="0de84-132">**arg**: hello arguments toobe passed toohello MapReduce job.</span></span> <span data-ttu-id="0de84-133">V takovém případě hello vstupní textového souboru a hello adresář, který se používají pro výstup hello</span><span class="sxs-lookup"><span data-stu-id="0de84-133">In this case, hello input text file and hello directory that are used for hello output</span></span>

     <span data-ttu-id="0de84-134">Tento příkaz by měla vrátit ID úlohy, které můžou být použité toocheck hello stav úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="0de84-134">This command should return a job ID that can be used toocheck hello status of hello job:</span></span>

       <span data-ttu-id="0de84-135">{"id": "job_1415651640909_0026"}</span><span class="sxs-lookup"><span data-stu-id="0de84-135">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="0de84-136">Stav hello toocheck hello úlohy, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0de84-136">toocheck hello status of hello job, use hello following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="0de84-137">Nahraďte hello **JOBID** s hodnotou hello vráceném v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="0de84-137">Replace hello **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="0de84-138">Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak hello JOBID by `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="0de84-138">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then hello JOBID would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="0de84-139">Pokud se dokončí úloha hello, vrácená hello stavu `SUCCEEDED`.</span><span class="sxs-lookup"><span data-stu-id="0de84-139">If hello job is complete, hello state returned is `SUCCEEDED`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0de84-140">Tento požadavek Curl vrátí dokumentu JSON s informacemi o úloze hello.</span><span class="sxs-lookup"><span data-stu-id="0de84-140">This Curl request returns a JSON document with information about hello job.</span></span> <span data-ttu-id="0de84-141">Se používá Jq tooretrieve pouze hello hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="0de84-141">Jq is used tooretrieve only hello state value.</span></span>

4. <span data-ttu-id="0de84-142">Pokud stav hello hello úlohy se změnil příliš`SUCCEEDED`, výsledky hello hello úlohy můžete načíst z úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="0de84-142">When hello state of hello job has changed too`SUCCEEDED`, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="0de84-143">Hello `statusdir` parametr, který je předán s hello dotazu obsahuje umístění hello hello výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="0de84-143">hello `statusdir` parameter that is passed with hello query contains hello location of hello output file.</span></span> <span data-ttu-id="0de84-144">V tomto příkladu je umístění hello `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="0de84-144">In this example, hello location is `/example/curl`.</span></span> <span data-ttu-id="0de84-145">Tato adresa ukládá hello výstup hello úlohy v hello clustery výchozí úložiště na `/example/curl`.</span><span class="sxs-lookup"><span data-stu-id="0de84-145">This address stores hello output of hello job in hello clusters default storage at `/example/curl`.</span></span>

<span data-ttu-id="0de84-146">Můžete zobrazit seznam a stažení těchto souborů pomocí hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0de84-146">You can list and download these files by using hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="0de84-147">Další informace o práci s objekty BLOB z hello rozhraní příkazového řádku Azure najdete v tématu hello [hello pomocí Azure CLI 2.0 s Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0de84-147">For more information on working with blobs from hello Azure CLI, see hello [Using hello Azure CLI 2.0 with Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.</span></span>

## <span data-ttu-id="0de84-148"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="0de84-148"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="0de84-149">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0de84-149">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="0de84-150">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0de84-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="0de84-151">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0de84-151">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="0de84-152">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0de84-152">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="0de84-153">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0de84-153">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="0de84-154">Další informace o rozhraní REST hello, který se používá v tomto článku najdete v tématu hello [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span><span class="sxs-lookup"><span data-stu-id="0de84-154">For more information about hello REST interface that is used in this article, see hello [WebHCat Reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).</span></span>
