---
title: "Použití nástroje Hadoop Sqoop se Curl v HDInsight - Azure | Microsoft Docs"
description: "Naučte se vzdáleně odesílání úloh Sqoop do HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 0975aedf58c6e110726dd3308eae5f9ad3907cc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a><span data-ttu-id="b26d9-103">Spuštění úloh Sqoop se systémem Hadoop v prostředí HDInsight pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="b26d9-103">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="b26d9-104">Další informace o použití Curl ke spuštění úloh Sqoop na cluster Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b26d9-104">Learn how to use Curl to run Sqoop jobs on a Hadoop cluster in HDInsight.</span></span>

<span data-ttu-id="b26d9-105">Curl se používá k ukazují, jak můžete pracovat s HDInsight pomocí nezpracované požadavky HTTP na spouštět, monitorovat a načíst výsledky úlohy Sqoop.</span><span class="sxs-lookup"><span data-stu-id="b26d9-105">Curl is used to demonstrate how you can interact with HDInsight by using raw HTTP requests to run, monitor, and retrieve the results of Sqoop jobs.</span></span> <span data-ttu-id="b26d9-106">To funguje tak, že pomocí WebHCat REST API (dříve označované jako Templeton) poskytované clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b26d9-106">This works by using the WebHCat REST API (formerly known as Templeton) provided by your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="b26d9-107">Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové do HDInsight, projděte si téma [informace o používání HDInsight v Linuxu](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="b26d9-107">If you are already familiar with using Linux-based Hadoop servers, but are new to HDInsight, see [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b26d9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b26d9-108">Prerequisites</span></span>
<span data-ttu-id="b26d9-109">Pokud chcete provést kroky v tomto článku, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="b26d9-109">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="b26d9-110">Hadoop v clusteru HDInsight (Linux nebo na základě Windows)</span><span class="sxs-lookup"><span data-stu-id="b26d9-110">A Hadoop on HDInsight cluster (Linux or Windows-based)</span></span>
* [<span data-ttu-id="b26d9-111">Curl</span><span class="sxs-lookup"><span data-stu-id="b26d9-111">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="b26d9-112">jq</span><span class="sxs-lookup"><span data-stu-id="b26d9-112">jq</span></span>](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a><span data-ttu-id="b26d9-113">Odesílání úloh Sqoop pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="b26d9-113">Submit Sqoop jobs by using Curl</span></span>
> [!NOTE]
> <span data-ttu-id="b26d9-114">Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména a hesla pro správce clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b26d9-114">When using Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the user name and password for the HDInsight cluster administrator.</span></span> <span data-ttu-id="b26d9-115">Název clusteru také musíte použít jako součást identifikátoru URI (Uniform Resource Identifier) sloužícímu k odesílání požadavků na server.</span><span class="sxs-lookup"><span data-stu-id="b26d9-115">You must also use the cluster name as part of the Uniform Resource Identifier (URI) used to send the requests to the server.</span></span>
> 
> <span data-ttu-id="b26d9-116">Pro příkazy v této části nahraďte **UŽIVATELSKÉ JMÉNO** uživatelem pro ověření do clusteru a nahraďte **HESLO** heslem pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="b26d9-116">For the commands in this section, replace **USERNAME** with the user to authenticate to the cluster, and replace **PASSWORD** with the password for the user account.</span></span> <span data-ttu-id="b26d9-117">Nahraďte **CLUSTERNAME** názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="b26d9-117">Replace **CLUSTERNAME** with the name of your cluster.</span></span>
> 
> <span data-ttu-id="b26d9-118">Rozhraní API REST je zabezpečeno pomocí [základního ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="b26d9-118">The REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="b26d9-119">Vždy doporučujeme provádět požadavky pomocí protokolu HTTPS (Secure HTTP) a pomoci tak zajistit, že přihlašovací údaje budou na server odeslány bezpečně.</span><span class="sxs-lookup"><span data-stu-id="b26d9-119">You should always make requests by using Secure HTTP (HTTPS) to help ensure that your credentials are securely sent to the server.</span></span>
> 
> 

1. <span data-ttu-id="b26d9-120">Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b26d9-120">From a command line, use the following command to verify that you can connect to your HDInsight cluster:</span></span>
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    <span data-ttu-id="b26d9-121">Měla by se zobrazit odpověď podobná následujícímu:</span><span class="sxs-lookup"><span data-stu-id="b26d9-121">You should receive a response similar to the following:</span></span>
   
        {"status":"ok","version":"v1"}
   
    <span data-ttu-id="b26d9-122">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="b26d9-122">The parameters used in this command are as follows:</span></span>
   
   * <span data-ttu-id="b26d9-123">**-u** – uživatelské jméno a heslo použité pro ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="b26d9-123">**-u** - The user name and password used to authenticate the request.</span></span>
   * <span data-ttu-id="b26d9-124">**-G** – označuje, že se jedná o požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="b26d9-124">**-G** - Indicates that this is a GET request.</span></span>
     
     <span data-ttu-id="b26d9-125">Na začátek adresu URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, budou stejné pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="b26d9-125">The beginning of the URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, will be the same for all requests.</span></span> <span data-ttu-id="b26d9-126">Cesta **/status**, označuje, že požadavek je vrátit stav WebHCat (také známé jako Templeton) pro server.</span><span class="sxs-lookup"><span data-stu-id="b26d9-126">The path, **/status**, indicates that the request is to return a status of WebHCat (also known as Templeton) for the server.</span></span> 
2. <span data-ttu-id="b26d9-127">Použijte následující postupy se odeslat úlohu sqoop:</span><span class="sxs-lookup"><span data-stu-id="b26d9-127">Use the following to submit a sqoop job:</span></span>

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    <span data-ttu-id="b26d9-128">Parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="b26d9-128">The parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="b26d9-129">**-d** – od `-G` se nepoužívá, použije se výchozí hodnota žádost na metodu POST.</span><span class="sxs-lookup"><span data-stu-id="b26d9-129">**-d** - Since `-G` is not used, the request defaults to the POST method.</span></span> <span data-ttu-id="b26d9-130">`-d`Určuje datových hodnot, které se odesílají s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="b26d9-130">`-d` specifies the data values that are sent with the request.</span></span>

        * <span data-ttu-id="b26d9-131">**User.Name** -uživatel, který spouští příkaz.</span><span class="sxs-lookup"><span data-stu-id="b26d9-131">**user.name** - The user that is running the command.</span></span>

        * <span data-ttu-id="b26d9-132">**příkaz** -Sqoop příkaz k provedení.</span><span class="sxs-lookup"><span data-stu-id="b26d9-132">**command** - The Sqoop command to execute.</span></span>

        * <span data-ttu-id="b26d9-133">**statusdir** -adresáře, který budou zapisovat do stavu pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="b26d9-133">**statusdir** - The directory that the status for this job will be written to.</span></span>

    <span data-ttu-id="b26d9-134">Tento příkaz by měl vrátit ID úlohy, který slouží ke kontrole stavu úlohy.</span><span class="sxs-lookup"><span data-stu-id="b26d9-134">This command should return a job ID that can be used to check the status of the job.</span></span>

        {"id":"job_1415651640909_0026"}

1. <span data-ttu-id="b26d9-135">Chcete-li zkontrolovat stav úlohy, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="b26d9-135">To check the status of the job, use the following command.</span></span> <span data-ttu-id="b26d9-136">Nahraďte **JOBID** se hodnota vrácená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b26d9-136">Replace **JOBID** with the value returned in the previous step.</span></span> <span data-ttu-id="b26d9-137">Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="b26d9-137">For example, if the return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    <span data-ttu-id="b26d9-138">Pokud se úloha dokončí, bude stav **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="b26d9-138">If the job has finished, the state will be **SUCCEEDED**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b26d9-139">Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informacemi o úloze; jq se používá k načtení jenom hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="b26d9-139">This Curl request returns a JavaScript Object Notation (JSON) document with information about the job; jq is used to retrieve only the state value.</span></span>
   > 
   > 
2. <span data-ttu-id="b26d9-140">Jakmile se stav úlohy se změnila na **úspěšné**, můžete načíst výsledky úlohy z Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="b26d9-140">Once the state of the job has changed to **SUCCEEDED**, you can retrieve the results of the job from Azure Blob storage.</span></span> <span data-ttu-id="b26d9-141">`statusdir` Parametr předaný s dotaz obsahuje umístění výstupního souboru; v tomto případě **wasb: / / / Příklad/curl**.</span><span class="sxs-lookup"><span data-stu-id="b26d9-141">The `statusdir` parameter passed with the query contains the location of the output file; in this case, **wasb:///example/curl**.</span></span> <span data-ttu-id="b26d9-142">Tato adresa ukládá výstup úlohy v **příklad/curl** adresář na výchozí kontejner úložiště používané clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b26d9-142">This address stores the output of the job in the **example/curl** directory on the default storage container used by your HDInsight cluster.</span></span>
   
    <span data-ttu-id="b26d9-143">Můžete zobrazit seznam a stáhnout tyto soubory pomocí [rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b26d9-143">You can list and download these files by using the [Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="b26d9-144">Například pro zobrazení seznamu souborů v **příklad/curl**, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b26d9-144">For example, to list files in **example/curl**, use the following command:</span></span>
   
        azure storage blob list <container-name> example/curl
   
    <span data-ttu-id="b26d9-145">Ke stažení souboru, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b26d9-145">To download a file, use the following:</span></span>
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > <span data-ttu-id="b26d9-146">Buď musíte zadat název účtu úložiště, který obsahuje objekt blob pomocí `-a` a `-k` parametry, nebo sady **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b26d9-146">You must either specify the storage account name that contains the blob by using the `-a` and `-k` parameters, or set the **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** environment variables.</span></span> <span data-ttu-id="b26d9-147">Najdete v části < href = "hdinsight data.md nahrávání" target = "_blank" Další informace.</span><span class="sxs-lookup"><span data-stu-id="b26d9-147">See <a href="hdinsight-upload-data.md" target="_blank" for more information.</span></span>
   > 
   > 

## <a name="limitations"></a><span data-ttu-id="b26d9-148">Omezení</span><span class="sxs-lookup"><span data-stu-id="b26d9-148">Limitations</span></span>
* <span data-ttu-id="b26d9-149">Hromadné export - s Linuxovým systémem HDInsight, Sqoop konektor umožňuje exportovat data do systému Microsoft SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="b26d9-149">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="b26d9-150">Dávkování - s HDInsight se systémem Linux, při použití `-batch` přepnout při vložení, Sqoop provede několik vloží místo dávkování operace insert.</span><span class="sxs-lookup"><span data-stu-id="b26d9-150">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop will perform multiple inserts instead of batching the insert operations.</span></span>

## <a name="summary"></a><span data-ttu-id="b26d9-151">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b26d9-151">Summary</span></span>
<span data-ttu-id="b26d9-152">Jak je ukázáno v tomto dokumentu, můžete spouštět, monitorovat a zobrazit výsledky Sqoop úloh na clusteru HDInsight nezpracovaná požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="b26d9-152">As demonstrated in this document, you can use a raw HTTP request to run, monitor, and view the results of Sqoop jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="b26d9-153">Další informace o rozhraní REST používané v tomto článku najdete v tématu <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API průvodce</a>.</span><span class="sxs-lookup"><span data-stu-id="b26d9-153">For more information on the REST interface used in this article, see the <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b26d9-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b26d9-154">Next steps</span></span>
<span data-ttu-id="b26d9-155">Obecné informace o Hive s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b26d9-155">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="b26d9-156">Použití nástroje Sqoop se systémem Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b26d9-156">Use Sqoop with Hadoop on HDInsight</span></span>](hdinsight-use-sqoop.md)

<span data-ttu-id="b26d9-157">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b26d9-157">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="b26d9-158">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b26d9-158">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="b26d9-159">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b26d9-159">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b26d9-160">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b26d9-160">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

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


