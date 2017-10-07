---
title: "aaaGet začít s příklad HBase v HDInsight - Azure | Microsoft Docs"
description: "Postupujte podle tohoto toostart příklad Apache HBase pomocí hadoop v HDInsight. Vytvářejte tabulky z hello prostředí HBase a dotazujte je pomocí Hive."
keywords: "příkaz hbase,příklad hbase"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 43419780142b320b16180a2b1f25020dee2f7a11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a><span data-ttu-id="ea6bf-105">Začínáme s příkladem Apache HBase ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="ea6bf-105">Get started with an Apache HBase example in HDInsight</span></span>

<span data-ttu-id="ea6bf-106">Zjistěte, jak toocreate cluster HBase v HDInsight, vytvářet tabulky HBase a dotazovat tabulky pomocí Hive.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-106">Learn how toocreate an HBase cluster in HDInsight, create HBase tables, and query tables by using Hive.</span></span> <span data-ttu-id="ea6bf-107">Obecné informace o HBase najdete v tématu [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview].</span><span class="sxs-lookup"><span data-stu-id="ea6bf-107">For general HBase information, see [HDInsight HBase overview][hdinsight-hbase-overview].</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a><span data-ttu-id="ea6bf-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ea6bf-108">Prerequisites</span></span>
<span data-ttu-id="ea6bf-109">Než začnete při tomto příkladu HBase, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-109">Before you begin trying this HBase example, you must have hello following items:</span></span>

* <span data-ttu-id="ea6bf-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-110">**An Azure subscription**.</span></span> <span data-ttu-id="ea6bf-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ea6bf-112">[Secure Shell (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> 
* <span data-ttu-id="ea6bf-113">[curl](http://curl.haxx.se/download.html).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-113">[curl](http://curl.haxx.se/download.html).</span></span>

## <a name="create-hbase-cluster"></a><span data-ttu-id="ea6bf-114">Vytvoření clusteru HBase</span><span class="sxs-lookup"><span data-stu-id="ea6bf-114">Create HBase cluster</span></span>
<span data-ttu-id="ea6bf-115">Hello následující postup používá toocreate šablony Azure Resource Manager verze 3.4 systémem Linux HBase clusteru a hello závislé výchozí účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-115">hello following procedure uses an Azure Resource Manager template toocreate a version 3.4 Linux-based HBase cluster and hello dependent default Azure Storage account.</span></span> <span data-ttu-id="ea6bf-116">toounderstand hello parametrů použitých v postupu hello a ostatní metody tvorby clusteru najdete v části [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-116">toounderstand hello parameters used in hello procedure and other cluster creation methods, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="ea6bf-117">Klikněte na tlačítko hello následující šablony hello tooopen bitové kopie v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-117">Click hello following image tooopen hello template in hello Azure portal.</span></span> <span data-ttu-id="ea6bf-118">Šablona Hello je umístěna v kontejneru veřejného objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-118">hello template is located in a public blob container.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="ea6bf-119">Z hello **vlastní nasazení** okno, zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-119">From hello **Custom deployment** blade, enter hello following values:</span></span>
   
   * <span data-ttu-id="ea6bf-120">**Předplatné**: Vyberte předplatné Azure, který je použité toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-120">**Subscription**: Select your Azure subscription that is used toocreate hello cluster.</span></span>
   * <span data-ttu-id="ea6bf-121">**Skupina prostředků:** Vytvořte skupinu správy prostředků Azure nebo použijte již existující.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-121">**Resource group**: Create an Azure Resource Management group or use an existing one.</span></span>
   * <span data-ttu-id="ea6bf-122">**Umístění**: Zadejte hello umístění skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-122">**Location**: Specify hello location of hello resource group.</span></span> 
   * <span data-ttu-id="ea6bf-123">**Název clusteru**: Zadejte název pro hello HBase cluster.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-123">**ClusterName**: Enter a name for hello HBase cluster.</span></span>
   * <span data-ttu-id="ea6bf-124">**Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je **správce**.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-124">**Cluster login name and password**: hello default login name is **admin**.</span></span>
   * <span data-ttu-id="ea6bf-125">**SSH uživatelské jméno a heslo**: výchozí uživatelské jméno hello **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-125">**SSH username and password**: hello default username is **sshuser**.</span></span>  <span data-ttu-id="ea6bf-126">Můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-126">You can rename it.</span></span>
     
     <span data-ttu-id="ea6bf-127">Další parametry jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-127">Other parameters are optional.</span></span>  
     
     <span data-ttu-id="ea6bf-128">Každý cluster obsahuje závislost účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-128">Each cluster has an Azure Storage account dependency.</span></span> <span data-ttu-id="ea6bf-129">Po odstranění clusteru s podporou hello data zachovají hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-129">After you delete a cluster, hello data retains in hello storage account.</span></span> <span data-ttu-id="ea6bf-130">Hello clusteru výchozí název účtu úložiště je název clusteru hello s "úložiště" připojí.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-130">hello cluster default storage account name is hello cluster name with "store" appended.</span></span> <span data-ttu-id="ea6bf-131">Je pevně kódovaný v části proměnných šablony hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-131">It is hardcoded in hello template variables section.</span></span>
3. <span data-ttu-id="ea6bf-132">Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**a potom klikněte na **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-132">Select **I agree toohello terms and conditions stated above**, and then click **Purchase**.</span></span> <span data-ttu-id="ea6bf-133">Trvá přibližně 20 minut toocreate cluster.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-133">It takes about 20 minutes toocreate a cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="ea6bf-134">Po odstranění clusteru služby HBase můžete vytvořit jiný cluster HBase pomocí hello stejného výchozího kontejneru blob.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-134">After an HBase cluster is deleted, you can create another HBase cluster by using hello same default blob container.</span></span> <span data-ttu-id="ea6bf-135">Hello nový cluster převezme tabulky HBase hello, kterou jste vytvořili v původním clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-135">hello new cluster picks up hello HBase tables you created in hello original cluster.</span></span> <span data-ttu-id="ea6bf-136">tooavoid nekonzistence, doporučujeme zakázat hello tabulek HBase, před odstraněním clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-136">tooavoid inconsistencies, we recommend that you disable hello HBase tables before you delete hello cluster.</span></span>
> 
> 

## <a name="create-tables-and-insert-data"></a><span data-ttu-id="ea6bf-137">Vytváření tabulek a vkládání dat</span><span class="sxs-lookup"><span data-stu-id="ea6bf-137">Create tables and insert data</span></span>
<span data-ttu-id="ea6bf-138">Pomocí SSH tooconnect tooHBase clusterů a potom pomocí tabulky HBase toocreate prostředí HBase, vkládání dat a dotaz na data.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-138">You can use SSH tooconnect tooHBase clusters and then use HBase Shell toocreate HBase tables, insert data, and query data.</span></span> <span data-ttu-id="ea6bf-139">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-139">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="ea6bf-140">Pro většinu osob se data zobrazí v tabulkovém formátu hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-140">For most people, data appears in hello tabular format:</span></span>

![Tabulková data HDInsight HBase][img-hbase-sample-data-tabular]

<span data-ttu-id="ea6bf-142">V HBase (implementaci BigTable), hello stejné data vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-142">In HBase (an implementation of BigTable), hello same data looks like:</span></span>

![Velké objemy tabulkových dat HDInsight HBase][img-hbase-sample-data-bigtable]


<span data-ttu-id="ea6bf-144">**toouse hello prostředí HBase**</span><span class="sxs-lookup"><span data-stu-id="ea6bf-144">**toouse hello HBase shell**</span></span>

1. <span data-ttu-id="ea6bf-145">Ze SSH spusťte následující příkaz HBase hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-145">From SSH, run hello following HBase command:</span></span>
   
    ```bash
    hbase shell
    ```

2. <span data-ttu-id="ea6bf-146">Vytvořte HBase se skupinami o dvou sloupcích:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-146">Create an HBase with two-column families:</span></span>

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. <span data-ttu-id="ea6bf-147">Vložte některá data:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-147">Insert some data:</span></span>
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![Prostředí HDInsight Hadoop HBase][img-hbase-shell]
4. <span data-ttu-id="ea6bf-149">Získání jednoho řádku</span><span class="sxs-lookup"><span data-stu-id="ea6bf-149">Get a single row</span></span>
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    <span data-ttu-id="ea6bf-150">Zobrazí se hello stejné výsledky jako pomocí příkazu hello vyhledávání, protože existuje pouze jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-150">You shall see hello same results as using hello scan command because there is only one row.</span></span>
   
    <span data-ttu-id="ea6bf-151">Další informace o schématu tabulky HBase hello najdete v tématu [Úvod tooHBase návrhu schématu][hbase-schema].</span><span class="sxs-lookup"><span data-stu-id="ea6bf-151">For more information about hello HBase table schema, see [Introduction tooHBase Schema Design][hbase-schema].</span></span> <span data-ttu-id="ea6bf-152">Další příkazy HBase najdete v tématu [Referenční příručka Apache HBase][hbase-quick-start].</span><span class="sxs-lookup"><span data-stu-id="ea6bf-152">For more HBase commands, see [Apache HBase reference guide][hbase-quick-start].</span></span>
5. <span data-ttu-id="ea6bf-153">Ukončete prostředí hello</span><span class="sxs-lookup"><span data-stu-id="ea6bf-153">Exit hello shell</span></span>
   
    ```hbaseshell
    exit
    ```

<span data-ttu-id="ea6bf-154">**toobulk načítání dat do tabulky kontaktů HBase hello**</span><span class="sxs-lookup"><span data-stu-id="ea6bf-154">**toobulk load data into hello contacts HBase table**</span></span>

<span data-ttu-id="ea6bf-155">HBase obsahuje několik metod načítání dat do tabulek.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-155">HBase includes several methods of loading data into tables.</span></span>  <span data-ttu-id="ea6bf-156">Další informace naleznete v tématu [Hromadné načítání](http://hbase.apache.org/book.html#arch.bulk.load).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-156">For more information, see [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load).</span></span>

<span data-ttu-id="ea6bf-157">Ukázkový datový soubor najdete ve veřejném kontejneru objektů blob: *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-157">A sample data file can be found in a public blob container, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span></span>  <span data-ttu-id="ea6bf-158">obsah Hello hello datového souboru je:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-158">hello content of hello data file is:</span></span>

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

<span data-ttu-id="ea6bf-159">Volitelně můžete vytvořte textový soubor a nahrajte hello soubor tooyour úložiště vlastní účet.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-159">You can optionally create a text file and upload hello file tooyour own storage account.</span></span> <span data-ttu-id="ea6bf-160">Hello pokyny najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="ea6bf-160">For hello instructions, see [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data].</span></span>

> [!NOTE]
> <span data-ttu-id="ea6bf-161">Tento postup používá tabulku kontaktů HBase hello, které jste vytvořili v posledním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-161">This procedure uses hello Contacts HBase table you have created in hello last procedure.</span></span>
> 

1. <span data-ttu-id="ea6bf-162">Ze SSH spusťte následující příkaz tootransform hello dat souboru tooStoreFiles a uložit do relativní cesty určené položkou Dimporttsv.bulk.output hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-162">From SSH, run hello following command tootransform hello data file tooStoreFiles and store at a relative path specified by Dimporttsv.bulk.output.</span></span>  <span data-ttu-id="ea6bf-163">Pokud jste v prostředí HBase, použijte hello ukončení příkazu tooexit.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-163">If you are in HBase Shell, use hello exit command tooexit.</span></span>

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. <span data-ttu-id="ea6bf-164">Spusťte následující příkaz tooupload hello data z tabulky HBase toohello/storedatafileoutput hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-164">Run hello following command tooupload hello data from  /example/data/storeDataFileOutput toohello HBase table:</span></span>
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. <span data-ttu-id="ea6bf-165">Můžete otevřít hello prostředí HBase a použít hello kontroly příkaz toolist hello obsahu tabulky.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-165">You can open hello HBase shell, and use hello scan command toolist hello table content.</span></span>

## <a name="use-hive-tooquery-hbase"></a><span data-ttu-id="ea6bf-166">Použijte Hive tooquery HBase</span><span class="sxs-lookup"><span data-stu-id="ea6bf-166">Use Hive tooquery HBase</span></span>

<span data-ttu-id="ea6bf-167">Data v tabulkách HBase můžete dotazovat pomocí Hive.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-167">You can query data in HBase tables by using Hive.</span></span> <span data-ttu-id="ea6bf-168">V této části vytvoříte tabulku Hive, mapuje toohello tabulky HBase a použije tooquery hello data v tabulce HBase.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-168">In this section, you create a Hive table that maps toohello HBase table and uses it tooquery hello data in your HBase table.</span></span>

1. <span data-ttu-id="ea6bf-169">Otevřete **PuTTY**a připojte toohello cluster.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-169">Open **PuTTY**, and connect toohello cluster.</span></span>  <span data-ttu-id="ea6bf-170">Hello pokyny naleznete v předchozím postupu hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-170">See hello instructions in hello previous procedure.</span></span>
2. <span data-ttu-id="ea6bf-171">Z relace SSH hello použijte následující příkaz toostart Beeline hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-171">From hello SSH session, use hello following command toostart Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="ea6bf-172">Další informace o Beeline najdete v tématu [Použití Hivu s Hadoopem ve službě HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-172">For more information about Beeline, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
       
3. <span data-ttu-id="ea6bf-173">Spusťte následující skript toocreate HiveQL hello tabulku Hive, která mapuje toohello tabulky HBase.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-173">Run hello following HiveQL script  toocreate a Hive table that maps toohello HBase table.</span></span> <span data-ttu-id="ea6bf-174">Ujistěte se, že jste vytvořili hello ukázkové tabulky odkazované dříve v tomto kurzu pomocí prostředí HBase hello před spuštěním tohoto prohlášení.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-174">Make sure that you have created hello sample table referenced earlier in this tutorial by using hello HBase shell before you run this statement.</span></span>

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. <span data-ttu-id="ea6bf-175">Spusťte následující HiveQL skriptu tooquery hello data v tabulce HBase hello hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-175">Run hello following HiveQL script tooquery hello data in hello HBase table:</span></span>

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a><span data-ttu-id="ea6bf-176">Použití rozhraní REST API HBase pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="ea6bf-176">Use HBase REST APIs using Curl</span></span>

<span data-ttu-id="ea6bf-177">Hello rozhraní API REST je zabezpečeno pomocí [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-177">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="ea6bf-178">Požadavky se vždy provádět pomocí HTTPS (Secure HTTP) toohelp Ujistěte se, že vaše přihlašovací údaje jsou odeslány bezpečně toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-178">You shall always make requests by using Secure HTTP (HTTPS) toohelp ensure that your credentials are securely sent toohello server.</span></span>

2. <span data-ttu-id="ea6bf-179">Použijte následující příkaz toolist hello existujících tabulek HBase hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-179">Use hello following command toolist hello existing HBase tables:</span></span>

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. <span data-ttu-id="ea6bf-180">Použijte následující příkaz toocreate novou tabulku HBase se dvěma sloupci rodiny hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-180">Use hello following command toocreate a new HBase table with two-column families:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    <span data-ttu-id="ea6bf-181">schéma Hello je k dispozici ve formátu JSon hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-181">hello schema is provided in hello JSon format.</span></span>
4. <span data-ttu-id="ea6bf-182">Použijte následující příkaz tooinsert hello některá data:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-182">Use hello following command tooinsert some data:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    <span data-ttu-id="ea6bf-183">Je nutné base64 kódování hello hodnoty zadané v přepínači -d hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-183">You must base64 encode hello values specified in hello -d switch.</span></span> <span data-ttu-id="ea6bf-184">V příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-184">In hello example:</span></span>
   
   * <span data-ttu-id="ea6bf-185">MTAwMA==: 1000</span><span class="sxs-lookup"><span data-stu-id="ea6bf-185">MTAwMA==: 1000</span></span>
   * <span data-ttu-id="ea6bf-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span><span class="sxs-lookup"><span data-stu-id="ea6bf-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span></span>
   * <span data-ttu-id="ea6bf-187">Sm9obiBEb2xl: John Dole</span><span class="sxs-lookup"><span data-stu-id="ea6bf-187">Sm9obiBEb2xl: John Dole</span></span>
     
     <span data-ttu-id="ea6bf-188">[klíč řádku false](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) vám umožní tooinsert více hodnot (dávkové).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) allows you tooinsert multiple (batched) values.</span></span>
5. <span data-ttu-id="ea6bf-189">Použijte následující příkaz tooget řádek hello:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-189">Use hello following command tooget a row:</span></span>
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

<span data-ttu-id="ea6bf-190">Další informace o HBase Rest naleznete v tématu [Referenční příručka Apache HBase](https://hbase.apache.org/book.html#_rest).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-190">For more information about HBase Rest, see [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).</span></span>

> [!NOTE]
> <span data-ttu-id="ea6bf-191">Thrift není podporovaný HBase v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-191">Thrift is not supported by HBase in HDInsight.</span></span>
>
> <span data-ttu-id="ea6bf-192">Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello zadáním hello uživatelské jméno a heslo pro správce clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-192">When using Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span> <span data-ttu-id="ea6bf-193">Název clusteru hello musíte také použít jako součást hello identifikátor URI (Uniform Resource) použít toosend hello požadavky toohello serveru:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-193">You must also use hello cluster name as part of hello Uniform Resource Identifier (URI) used toosend hello requests toohello server:</span></span>
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    <span data-ttu-id="ea6bf-194">Mělo by se zobrazit odpověď podobná toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-194">You should receive a response similar toohello following response:</span></span>
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a><span data-ttu-id="ea6bf-195">Kontrola stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="ea6bf-195">Check cluster status</span></span>
<span data-ttu-id="ea6bf-196">HBase v HDInsight se dodává s webovým uživatelským rozhraním pro sledování clusterů.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-196">HBase in HDInsight ships with a Web UI for monitoring clusters.</span></span> <span data-ttu-id="ea6bf-197">Pomocí hello webového uživatelského rozhraní, můžete požádat statistické údaje nebo informace o oblastech.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-197">Using hello Web UI, you can request statistics or information about regions.</span></span>

<span data-ttu-id="ea6bf-198">**hello tooaccess HBase hlavního uživatelského rozhraní**</span><span class="sxs-lookup"><span data-stu-id="ea6bf-198">**tooaccess hello HBase Master UI**</span></span>

1. <span data-ttu-id="ea6bf-199">Přihlaste se k hello hello webové uživatelské rozhraní Ambari na https://&lt;Clustername >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-199">Sign into hello hello Ambari Web UI at https://&lt;Clustername>.azurehdinsight.net.</span></span>
2. <span data-ttu-id="ea6bf-200">Klikněte na tlačítko **HBase** hello levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-200">Click **HBase** from hello left menu.</span></span>
3. <span data-ttu-id="ea6bf-201">Klikněte na tlačítko **rychlé odkazy** hello horní části stránky hello, bod toohello active Zookeeper uzel propojení a pak klikněte na tlačítko **HBase hlavního uživatelského rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-201">Click **Quick links** on hello top of hello page, point toohello active Zookeeper node link, and then click **HBase Master UI**.</span></span>  <span data-ttu-id="ea6bf-202">Hello uživatelského rozhraní je otevřen v jiné kartě prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-202">hello UI is opened in another browser tab:</span></span>

  ![Hlavní uživatelské rozhraní HDInsight HBase](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  <span data-ttu-id="ea6bf-204">Hello HBase hlavního uživatelského rozhraní obsahuje hello následující části:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-204">hello HBase Master UI contains hello following sections:</span></span>

  - <span data-ttu-id="ea6bf-205">oblastní servery</span><span class="sxs-lookup"><span data-stu-id="ea6bf-205">region servers</span></span>
  - <span data-ttu-id="ea6bf-206">zálohování hlavních serverů</span><span class="sxs-lookup"><span data-stu-id="ea6bf-206">backup masters</span></span>
  - <span data-ttu-id="ea6bf-207">tabulky</span><span class="sxs-lookup"><span data-stu-id="ea6bf-207">tables</span></span>
  - <span data-ttu-id="ea6bf-208">úlohy</span><span class="sxs-lookup"><span data-stu-id="ea6bf-208">tasks</span></span>
  - <span data-ttu-id="ea6bf-209">atributy softwaru</span><span class="sxs-lookup"><span data-stu-id="ea6bf-209">software attributes</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="ea6bf-210">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="ea6bf-210">Delete hello cluster</span></span>
<span data-ttu-id="ea6bf-211">tooavoid nekonzistence, doporučujeme zakázat hello tabulek HBase, před odstraněním clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-211">tooavoid inconsistencies, we recommend that you disable hello HBase tables before you delete hello cluster.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="ea6bf-212">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ea6bf-212">Troubleshoot</span></span>

<span data-ttu-id="ea6bf-213">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="ea6bf-213">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea6bf-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea6bf-214">Next steps</span></span>
<span data-ttu-id="ea6bf-215">V tomto článku jste se dozvěděli, jak hello toocreate HBase cluster a jak toocreate tabulek a zobrazení hello data v těchto tabulkách z prostředí HBase.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-215">In this article, you learned how toocreate an HBase cluster and how toocreate tables and view hello data in those tables from hello HBase shell.</span></span> <span data-ttu-id="ea6bf-216">Také jste zjistili, jak toouse podregistru dotazování na data v tabulkách HBase a jak toouse hello REST API HBase C# toocreate tabulky HBase a načtení dat z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-216">You also learned how toouse a Hive query on data in HBase tables and how toouse hello HBase C# REST APIs toocreate an HBase table and retrieve data from hello table.</span></span>

<span data-ttu-id="ea6bf-217">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="ea6bf-217">toolearn more, see:</span></span>

* <span data-ttu-id="ea6bf-218">[Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="ea6bf-218">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
