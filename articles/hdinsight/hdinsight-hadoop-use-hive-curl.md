---
title: aaaUse Hadoop Hive s Curl v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak odeslat tooremotely Pig úlohy tooHDInsight pomocí Curl."
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
ms.openlocfilehash: e725829ad2adcf3540f44375e3e87b7cdaebd15e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a><span data-ttu-id="8df68-103">Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí REST</span><span class="sxs-lookup"><span data-stu-id="8df68-103">Run Hive queries with Hadoop in HDInsight using REST</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="8df68-104">Zjistěte, jak toouse hello WebHCat REST API toorun Hive dotazy s Hadoop v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8df68-104">Learn how toouse hello WebHCat REST API toorun Hive queries with Hadoop on Azure HDInsight cluster.</span></span>

<span data-ttu-id="8df68-105">[Curl](http://curl.haxx.se/) je použité toodemonstrate, jak můžete pracovat s HDInsight pomocí nezpracované požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="8df68-105">[Curl](http://curl.haxx.se/) is used toodemonstrate how you can interact with HDInsight by using raw HTTP requests.</span></span> <span data-ttu-id="8df68-106">Hello [jq](http://stedolan.github.io/jq/) nástroj je použité tooprocess hello JSON data vrácená z požadavky REST.</span><span class="sxs-lookup"><span data-stu-id="8df68-106">hello [jq](http://stedolan.github.io/jq/) utility is used tooprocess hello JSON data returned from REST requests.</span></span>

> [!NOTE]
> <span data-ttu-id="8df68-107">Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma hello [co potřebujete tooknow o Hadoop na HDInsight se systémem Linux](hdinsight-hadoop-linux-information.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8df68-107">If you are already familiar with using Linux-based Hadoop servers, but are new tooHDInsight, see hello [What you need tooknow about Hadoop on Linux-based HDInsight](hdinsight-hadoop-linux-information.md) document.</span></span>

## <span data-ttu-id="8df68-108"><a id="curl"></a>Spouštění dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="8df68-108"><a id="curl"></a>Run Hive queries</span></span>

> [!NOTE]
> <span data-ttu-id="8df68-109">Pokud používáte cURL nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello zadáním hello uživatelské jméno a heslo pro správce clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-109">When using cURL or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span>
>
> <span data-ttu-id="8df68-110">Hello příkazy v této části, nahraďte **uživatelské jméno** s hello uživatele tooauthenticate toohello clusteru a nahraďte **heslo** s hello heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-110">For hello commands in this section, replace **USERNAME** with hello user tooauthenticate toohello cluster, and replace **PASSWORD** with hello password for hello user account.</span></span> <span data-ttu-id="8df68-111">Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="8df68-111">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>
>
> <span data-ttu-id="8df68-112">Hello rozhraní API REST je zabezpečeno pomocí [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="8df68-112">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="8df68-113">toohelp Ujistěte se, že vaše přihlašovací údaje jsou bezpečně odesílá serveru toohello, vždy provádět požadavky pomocí Secure HTTP (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="8df68-113">toohelp ensure that your credentials are securely sent toohello server, always make requests by using Secure HTTP (HTTPS).</span></span>

1. <span data-ttu-id="8df68-114">Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:</span><span class="sxs-lookup"><span data-stu-id="8df68-114">From a command line, use hello following command tooverify that you can connect tooyour HDInsight cluster:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    <span data-ttu-id="8df68-115">Zobrazí se podobné toohello odpovědi následující text:</span><span class="sxs-lookup"><span data-stu-id="8df68-115">You receive a response similar toohello following text:</span></span>

        {"status":"ok","version":"v1"}

    <span data-ttu-id="8df68-116">Hello parametry použité v tomto příkazu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="8df68-116">hello parameters used in this command are as follows:</span></span>

   * <span data-ttu-id="8df68-117">**-u** -hello uživatelské jméno a heslo použít tooauthenticate hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="8df68-117">**-u** - hello user name and password used tooauthenticate hello request.</span></span>
   * <span data-ttu-id="8df68-118">**-G** – označuje, že tento požadavek je operaci GET.</span><span class="sxs-lookup"><span data-stu-id="8df68-118">**-G** - Indicates that this request is a GET operation.</span></span>

     <span data-ttu-id="8df68-119">Dobrý den, od adresy URL hello, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello stejné pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="8df68-119">hello beginning of hello URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hello same for all requests.</span></span> <span data-ttu-id="8df68-120">Cesta Hello **/status**, signalizuje tento požadavek hello tooreturn stav WebHCat (také známé jako Templeton) pro hello server.</span><span class="sxs-lookup"><span data-stu-id="8df68-120">hello path, **/status**, indicates that hello request is tooreturn a status of WebHCat (also known as Templeton) for hello server.</span></span> <span data-ttu-id="8df68-121">Můžete také vyžadovat hello verzi Hive pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8df68-121">You can also request hello version of Hive by using hello following command:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     <span data-ttu-id="8df68-122">Tento požadavek vrátí odpověď podobnou toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="8df68-122">This request returns a response similar toohello following text:</span></span>

       <span data-ttu-id="8df68-123">{"module": "hive", "verze": "0.13.0.2.1.6.0-2103"}</span><span class="sxs-lookup"><span data-stu-id="8df68-123">{"module":"hive","version":"0.13.0.2.1.6.0-2103"}</span></span>

2. <span data-ttu-id="8df68-124">Hello použijte následující tabulku s názvem toocreate **log4jLogs**:</span><span class="sxs-lookup"><span data-stu-id="8df68-124">Use hello following toocreate a table named **log4jLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="8df68-125">Hello následující parametry použité s touto žádostí:</span><span class="sxs-lookup"><span data-stu-id="8df68-125">hello following parameters used with this request:</span></span>

   * <span data-ttu-id="8df68-126">**-d** – od `-G` se nepoužívá, žádost hello výchozí toohello metodu POST.</span><span class="sxs-lookup"><span data-stu-id="8df68-126">**-d** - Since `-G` is not used, hello request defaults toohello POST method.</span></span> <span data-ttu-id="8df68-127">`-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="8df68-127">`-d` specifies hello data values that are sent with hello request.</span></span>

     * <span data-ttu-id="8df68-128">**User.Name** -hello uživatele, který spouští příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-128">**user.name** - hello user that is running hello command.</span></span>
     * <span data-ttu-id="8df68-129">**spuštění** -hello tooexecute příkazy HiveQL.</span><span class="sxs-lookup"><span data-stu-id="8df68-129">**execute** - hello HiveQL statements tooexecute.</span></span>
     * <span data-ttu-id="8df68-130">**statusdir** -hello adresář, který hello stavu pro tuto úlohu je zapsán do.</span><span class="sxs-lookup"><span data-stu-id="8df68-130">**statusdir** - hello directory that hello status for this job is written to.</span></span>

     <span data-ttu-id="8df68-131">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="8df68-131">These statements perform hello following actions:</span></span>
   * <span data-ttu-id="8df68-132">**VYŘAĎTE tabulku** -hello tabulka již existuje, je odstraněn.</span><span class="sxs-lookup"><span data-stu-id="8df68-132">**DROP TABLE** - If hello table already exists, it is deleted.</span></span>
   * <span data-ttu-id="8df68-133">**Vytvoření externí tabulky** -vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="8df68-133">**CREATE EXTERNAL TABLE** - Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="8df68-134">Externí tabulky uložit jenom hello Definice tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="8df68-134">External tables store only hello table definition in Hive.</span></span> <span data-ttu-id="8df68-135">Hello data je ponechán v hello původního umístění.</span><span class="sxs-lookup"><span data-stu-id="8df68-135">hello data is left in hello original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="8df68-136">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="8df68-136">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="8df68-137">Například automatizované data nahrát procesu nebo jiná operace MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8df68-137">For example, an automated data upload process or another MapReduce operation.</span></span>
     >
     > <span data-ttu-id="8df68-138">Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="8df68-138">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

   * <span data-ttu-id="8df68-139">**Řádek formátu** – jak hello datového naformátován.</span><span class="sxs-lookup"><span data-stu-id="8df68-139">**ROW FORMAT** - How hello data is formatted.</span></span> <span data-ttu-id="8df68-140">Hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="8df68-140">hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="8df68-141">**ULOŽENÉ umístění textový soubor AS** – tam, kde jsou uložena hello data (adresář hello příklad nebo data) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="8df68-141">**STORED AS TEXTFILE LOCATION** - Where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="8df68-142">**Vyberte** -počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu hello **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="8df68-142">**SELECT** - Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="8df68-143">Tento příkaz vrátí hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8df68-143">This statement returns a value of **3** as there are three rows that contain this value.</span></span>

     > [!NOTE]
     > <span data-ttu-id="8df68-144">Všimněte si, že hello mezery mezi příkazy HiveQL jsou nahrazovány hello `+` znak při použití s Curl.</span><span class="sxs-lookup"><span data-stu-id="8df68-144">Notice that hello spaces between HiveQL statements are replaced by hello `+` character when used with Curl.</span></span> <span data-ttu-id="8df68-145">Hodnoty v uvozovkách, které obsahují mezery, jako je například hello oddělovač, by neměl být nahrazen `+`.</span><span class="sxs-lookup"><span data-stu-id="8df68-145">Quoted values that contain a space, such as hello delimiter, should not be replaced by `+`.</span></span>

   * <span data-ttu-id="8df68-146">**INPUT__FILE__NAME jako '% 25.log'** – tento příkaz omezení hello vyhledávání tooonly používané soubory končící na. log.</span><span class="sxs-lookup"><span data-stu-id="8df68-146">**INPUT__FILE__NAME LIKE '%25.log'** - This statement limits hello search tooonly use files ending in .log.</span></span>

     > [!NOTE]
     > <span data-ttu-id="8df68-147">Hello `%25` je hello kódovaná adresou URL formuláře %, je skutečný stav hello `like '%.log'`.</span><span class="sxs-lookup"><span data-stu-id="8df68-147">hello `%25` is hello URL encoded form of %, so hello actual condition is `like '%.log'`.</span></span> <span data-ttu-id="8df68-148">Hello % má adresa URL toobe kódování, jako je považován za zvláštní znak v adresách URL.</span><span class="sxs-lookup"><span data-stu-id="8df68-148">hello % has toobe URL encoded, as it is treated as a special character in URLs.</span></span>

     <span data-ttu-id="8df68-149">Tento příkaz by měl vrátit ID úlohy, které můžou být použité toocheck hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-149">This command should return a job ID that can be used toocheck hello status of hello job.</span></span>

       <span data-ttu-id="8df68-150">{"id": "job_1415651640909_0026"}</span><span class="sxs-lookup"><span data-stu-id="8df68-150">{"id":"job_1415651640909_0026"}</span></span>

3. <span data-ttu-id="8df68-151">Stav hello toocheck hello úlohy, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8df68-151">toocheck hello status of hello job, use hello following command:</span></span>

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    <span data-ttu-id="8df68-152">Nahraďte **JOBID** s hodnotou hello vráceném v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-152">Replace **JOBID** with hello value returned in hello previous step.</span></span> <span data-ttu-id="8df68-153">Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.</span><span class="sxs-lookup"><span data-stu-id="8df68-153">For example, if hello return value was `{"id":"job_1415651640909_0026"}`, then **JOBID** would be `job_1415651640909_0026`.</span></span>

    <span data-ttu-id="8df68-154">Pokud hello úloha dokončí, je stav hello **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="8df68-154">If hello job has finished, hello state is **SUCCEEDED**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8df68-155">Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informace o úloze hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-155">This Curl request returns a JavaScript Object Notation (JSON) document with information about hello job.</span></span> <span data-ttu-id="8df68-156">Se používá Jq tooretrieve pouze hello hodnotu stavu.</span><span class="sxs-lookup"><span data-stu-id="8df68-156">Jq is used tooretrieve only hello state value.</span></span>

4. <span data-ttu-id="8df68-157">Jakmile se stav hello hello úlohy se změnil příliš**úspěšné**, výsledky hello hello úlohy můžete načíst z úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="8df68-157">Once hello state of hello job has changed too**SUCCEEDED**, you can retrieve hello results of hello job from Azure Blob storage.</span></span> <span data-ttu-id="8df68-158">Hello `statusdir` parametr předaný s hello dotazu obsahuje hello umístění výstupního souboru hello; v tomto případě **/příklad/curl**.</span><span class="sxs-lookup"><span data-stu-id="8df68-158">hello `statusdir` parameter passed with hello query contains hello location of hello output file; in this case, **/example/curl**.</span></span> <span data-ttu-id="8df68-159">Tato adresa ukládá hello výstup hello **příklad/curl** adresáře v hello clusterů výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="8df68-159">This address stores hello output in hello **example/curl** directory in hello clusters default storage.</span></span>

    <span data-ttu-id="8df68-160">Můžete zobrazit seznam a stažení těchto souborů pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8df68-160">You can list and download these files by using hello [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="8df68-161">Další informace o používání hello rozhraní příkazového řádku Azure s Azure Storage najdete v tématu hello [použití Azure CLI 2.0 s Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8df68-161">For more information on using hello Azure CLI with Azure Storage, see hello [Use Azure CLI 2.0 with Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) document.</span></span>

5. <span data-ttu-id="8df68-162">Hello použijte následující příkazy toocreate novou "interní" tabulku s názvem **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="8df68-162">Use hello following statements toocreate a new 'internal' table named **errorLogs**:</span></span>

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    <span data-ttu-id="8df68-163">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="8df68-163">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="8df68-164">**Vytvoření tabulka není v případě existuje** -vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8df68-164">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="8df68-165">Tento příkaz vytvoří interní tabulku, která je uložena v datovém skladu hello Hive a je zcela spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="8df68-165">This statement creates an internal table, which is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="8df68-166">Na rozdíl od externích tabulek se odstranit interní tabulku odstraní hello základní data.</span><span class="sxs-lookup"><span data-stu-id="8df68-166">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

   * <span data-ttu-id="8df68-167">**ULOŽENÉ ORC AS** – ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="8df68-167">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="8df68-168">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="8df68-168">ORC is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="8df68-169">**PŘEPSAT INSERT... Vyberte** -vybere řádky z hello **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vloží hello data do hello **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="8df68-169">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>
   * <span data-ttu-id="8df68-170">**Vyberte** -vybere všechny řádky z hello nové **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="8df68-170">**SELECT** - Selects all rows from hello new **errorLogs** table.</span></span>

6. <span data-ttu-id="8df68-171">Pomocí ID úlohy hello vrátil stav hello toocheck hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="8df68-171">Use hello job ID returned toocheck hello status of hello job.</span></span> <span data-ttu-id="8df68-172">Jakmile ho proběhla úspěšně, použijte hello rozhraní příkazového řádku Azure, jak je popsáno výše toodownload a zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="8df68-172">Once it has succeeded, use hello Azure CLI as described previously toodownload and view hello results.</span></span> <span data-ttu-id="8df68-173">výstup Hello by měla obsahovat tři řádky, které obsahují **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="8df68-173">hello output should contain three lines, all of which contain **[ERROR]**.</span></span>

## <span data-ttu-id="8df68-174"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="8df68-174"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="8df68-175">Obecné informace o Hive s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8df68-175">For general information on Hive with HDInsight:</span></span>

* [<span data-ttu-id="8df68-176">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8df68-176">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="8df68-177">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8df68-177">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8df68-178">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8df68-178">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="8df68-179">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8df68-179">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="8df68-180">Pokud používáte Tez s Hive, zobrazit hello následující dokumenty pro ladění informace:</span><span class="sxs-lookup"><span data-stu-id="8df68-180">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="8df68-181">Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="8df68-181">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

<span data-ttu-id="8df68-182">Další informace o hello REST API, které jsou v tomto dokumentu najdete v tématu hello [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8df68-182">For more information on hello REST API used in this document, see hello [WebHCat reference](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) document.</span></span>

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


