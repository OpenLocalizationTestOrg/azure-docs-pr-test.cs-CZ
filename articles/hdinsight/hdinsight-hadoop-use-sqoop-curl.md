---
title: aaaUse Hadoop Sqoop s Curl v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak odeslat tooremotely tooHDInsight Sqoop úlohy pomocí Curl."
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
ms.openlocfilehash: d9c09a6704ab6c5f48be50ed6d6314ec406df8ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a><span data-ttu-id="ee27c-103">Spuštění úloh Sqoop se systémem Hadoop v prostředí HDInsight pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="ee27c-103">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="ee27c-104">Zjistěte, jak clusteru toouse Curl toorun Sqoop úloh na Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee27c-104">Learn how toouse Curl toorun Sqoop jobs on a Hadoop cluster in HDInsight.</span></span>

<span data-ttu-id="ee27c-105">Curl je použité toodemonstrate, jak můžete pracovat s HDInsight pomocí nezpracovaná toorun požadavky HTTP, monitorování a načíst výsledky hello Sqoop úloh.</span><span class="sxs-lookup"><span data-stu-id="ee27c-105">Curl is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests toorun, monitor, and retrieve hello results of Sqoop jobs.</span></span> <span data-ttu-id="ee27c-106">Tento postup funguje s použitím hello WebHCat rozhraní API REST (dříve označované jako Templeton) poskytované clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee27c-106">This works by using hello WebHCat REST API (formerly known as Templeton) provided by your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="ee27c-107">Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma [informace o používání HDInsight v Linuxu](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="ee27c-107">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ee27c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ee27c-108">Prerequisites</span></span>
<span data-ttu-id="ee27c-109">toocomplete hello kroky v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="ee27c-109">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="ee27c-110">Hadoop v clusteru HDInsight (Linux nebo na základě Windows)</span><span class="sxs-lookup"><span data-stu-id="ee27c-110">A Hadoop on HDInsight cluster (Linux or Windows-based)</span></span>
* [<span data-ttu-id="ee27c-111">Curl</span><span class="sxs-lookup"><span data-stu-id="ee27c-111">Curl</span></span>](http://curl.haxx.se/)
* [<span data-ttu-id="ee27c-112">jq</span><span class="sxs-lookup"><span data-stu-id="ee27c-112">jq</span></span>](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a><span data-ttu-id="ee27c-113">Odesílání úloh Sqoop pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="ee27c-113">Submit Sqoop jobs by using Curl</span></span>
> [!NOTE]
> <span data-ttu-id="ee27c-114">Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello zadáním hello uživatelské jméno a heslo pro správce clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="ee27c-114">When using Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span> <span data-ttu-id="ee27c-115">Název clusteru hello musíte použít také jako součást hello identifikátor URI (Uniform Resource) použít toosend hello požadavky toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="ee27c-115">You must also use hello cluster name as part of hello Uniform Resource Identifier (URI) used toosend hello requests toohello server.</span></span>
> 
> <span data-ttu-id="ee27c-116">Hello příkazy v této části, nahraďte **uživatelské jméno** s hello uživatele tooauthenticate toohello clusteru a nahraďte **heslo** s hello heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="ee27c-116">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and replace **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="ee27c-117">Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="ee27c-117">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
> 
> <span data-ttu-id="ee27c-118">Hello rozhraní API REST je zabezpečeno pomocí [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="ee27c-118">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="ee27c-119">Vždy doporučujeme provádět požadavky pomocí HTTPS (Secure HTTP) toohelp Ujistěte se, že vaše přihlašovací údaje jsou odeslány bezpečně toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="ee27c-119">You should always make requests by using Secure HTTP (HTTPS) toohelp ensure that your credentials are securely sent toohello server.</span></span>
> 
> 

1. <span data-ttu-id="ee27c-120">Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:</span><span class="sxs-lookup"><span data-stu-id="ee27c-120">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    <span data-ttu-id="ee27c-121">Měli byste obdržet a odpovědi podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ee27c-121">You should receive a response similar toohello following:</span></span>
   
        {"status":"ok","version":"v1"}
   
    <span data-ttu-id="ee27c-122">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ee27c-122">hello parameters used in this command are as follows:</span></span>
   
   * <span data-ttu-id="ee27c-123">**-u** -hello uživatelské jméno a heslo použít tooauthenticate hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ee27c-123">**-u** - hello user name and password used tooauthenticate hello request.</span></span>
   * <span data-ttu-id="ee27c-124">**-G** – označuje, že se jedná o požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="ee27c-124">**-G** - Indicates that this is a GET request.</span></span>
     
     <span data-ttu-id="ee27c-125">Dobrý den, od adresy URL hello, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, bude hello stejné pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="ee27c-125">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, will be hello same for all requests.</span></span> <span data-ttu-id="ee27c-126">Cesta Hello **/status**, signalizuje tento požadavek hello tooreturn stav WebHCat (také známé jako Templeton) pro hello server.</span><span class="sxs-lookup"><span data-stu-id="ee27c-126">hello path, **/status**, indicates that hello request is tooreturn a status of WebHCat (also known as Templeton) for hello server.</span></span> 
2. <span data-ttu-id="ee27c-127">Použijte následující toosubmit úlohu sqoop hello:</span><span class="sxs-lookup"><span data-stu-id="ee27c-127">Use hello following toosubmit a sqoop job:</span></span>

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    <span data-ttu-id="ee27c-128">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ee27c-128">hello parameters used in this command are as follows:</span></span>

    * <span data-ttu-id="ee27c-129">**-d** – od `-G` se nepoužívá, žádost hello výchozí toohello metodu POST.</span><span class="sxs-lookup"><span data-stu-id="ee27c-129">**-d** - Since `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="ee27c-130">`-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ee27c-130">`-d` specifies hello data values that are sent with hello request.</span></span>

        * <span data-ttu-id="ee27c-131">**User.Name** -hello uživatele, který spouští příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ee27c-131">**user.name** - hello user that is running hello command.</span></span>

        * <span data-ttu-id="ee27c-132">**příkaz** -hello tooexecute příkaz Sqoop.</span><span class="sxs-lookup"><span data-stu-id="ee27c-132">**command** - hello Sqoop command tooexecute.</span></span>

        * <span data-ttu-id="ee27c-133">**statusdir** -hello adresář, který hello stavu pro tuto úlohu se zapíšou do.</span><span class="sxs-lookup"><span data-stu-id="ee27c-133">**statusdir** - hello directory that hello status for this job will be written to.</span></span>

    <span data-ttu-id="ee27c-134">Tento příkaz by měl vrátit ID úlohy, které můžou být použité toocheck hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="ee27c-134">This command should return a job ID that can be used toocheck hello status of hello job.</span></span>

        {"id":"job_1415651640909_0026"}

1. <span data-ttu-id="ee27c-135">Stav hello toocheck hello úlohy, hello použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="ee27c-135">toocheck hello status of hello job, use hello following command.</span></span> <span data-ttu-id="ee27c-136">Nahraďte **JOBID** s hodnotou hello vráceném v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ee27c-136">Replace **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="ee27c-137">Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="ee27c-137">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    <span data-ttu-id="ee27c-138">Pokud hello úloha dokončí, bude mít stav hello **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="ee27c-138">If hello job has finished, hello state will be **SUCCEEDED**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ee27c-139">Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informacemi o úloze hello; se používá jq tooretrieve pouze hello hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="ee27c-139">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job; jq is used tooretrieve only hello state value.</span></span>
   > 
   > 
2. <span data-ttu-id="ee27c-140">Jakmile se stav hello hello úlohy se změnil příliš**úspěšné**, výsledky hello hello úlohy můžete načíst z úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="ee27c-140">Once hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="ee27c-141">Hello `statusdir` parametr předaný s hello dotazu obsahuje hello umístění výstupního souboru hello; v tomto případě **wasb: / / / Příklad/curl**.</span><span class="sxs-lookup"><span data-stu-id="ee27c-141">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, **wasb:///example/curl**.</span></span> <span data-ttu-id="ee27c-142">Tato adresa ukládá hello výstup hello úlohy v hello **příklad/curl** v hello výchozí kontejner úložiště používané clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee27c-142">This address stores hello output of hello job in hello **example/curl** directory on hello default storage container used by your HDInsight cluster.</span></span>
   
    <span data-ttu-id="ee27c-143">Můžete zobrazit seznam a stažení těchto souborů pomocí hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ee27c-143">You can list and download these files by using hello [Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="ee27c-144">Například soubory toolist **příklad/curl**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ee27c-144">For example, toolist files in **example/curl**, use hello following command:</span></span>
   
        azure storage blob list <container-name> example/curl
   
    <span data-ttu-id="ee27c-145">toodownload soubor, použijte následující hello:</span><span class="sxs-lookup"><span data-stu-id="ee27c-145">toodownload a file, use hello following:</span></span>
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > <span data-ttu-id="ee27c-146">Buď musíte zadat název účtu úložiště hello, která obsahuje objekt blob hello pomocí hello `-a` a `-k` parametry, nebo sadu hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ee27c-146">You must either specify hello storage account name that contains hello blob by using hello `-a` and `-k` parameters, or set hello **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** environment variables.</span></span> <span data-ttu-id="ee27c-147">Najdete v části < href = "hdinsight data.md nahrávání" target = "_blank" Další informace.</span><span class="sxs-lookup"><span data-stu-id="ee27c-147">See <a href="hdinsight-upload-data.md" target="_blank" for more information.</span></span>
   > 
   > 

## <a name="limitations"></a><span data-ttu-id="ee27c-148">Omezení</span><span class="sxs-lookup"><span data-stu-id="ee27c-148">Limitations</span></span>
* <span data-ttu-id="ee27c-149">Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="ee27c-149">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="ee27c-150">Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop provede několik vloží místo dávkování operace insert hello.</span><span class="sxs-lookup"><span data-stu-id="ee27c-150">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop will perform multiple inserts instead of batching hello insert operations.</span></span>

## <a name="summary"></a><span data-ttu-id="ee27c-151">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ee27c-151">Summary</span></span>
<span data-ttu-id="ee27c-152">Jak je ukázáno v tomto dokumentu, můžete použít nezpracovaná toorun požadavku HTTP, monitorování a zobrazení výsledků hello Sqoop úloh na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee27c-152">As demonstrated in this document, you can use a raw HTTP request toorun, monitor, and view hello results of Sqoop jobs on your HDInsight cluster.</span></span>

<span data-ttu-id="ee27c-153">Další informace o rozhraní REST hello používané v tomto článku najdete v tématu hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API průvodce</a>.</span><span class="sxs-lookup"><span data-stu-id="ee27c-153">For more information on hello REST interface used in this article, see hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee27c-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee27c-154">Next steps</span></span>
<span data-ttu-id="ee27c-155">Obecné informace o Hive s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ee27c-155">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="ee27c-156">Použití nástroje Sqoop se systémem Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee27c-156">Use Sqoop with Hadoop on HDInsight</span></span>](hdinsight-use-sqoop.md)

<span data-ttu-id="ee27c-157">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ee27c-157">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="ee27c-158">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee27c-158">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ee27c-159">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee27c-159">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ee27c-160">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee27c-160">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

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


