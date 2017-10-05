---
title: "Začínáme s příkladem HBase ve službě HDInsight – Azure | Dokumentace Microsoftu"
description: "Postupujte podle tohoto příkladu Apache HBase a začněte používat Hadoop ve službě HDInsight. Vytvářejte tabulky z prostředí HBase a dotazujte je pomocí Hive."
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
ms.openlocfilehash: bbd8a838062795ee03ae02dc5e3fd45d841a6e17
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a><span data-ttu-id="c6ac3-105">Začínáme s příkladem Apache HBase ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6ac3-105">Get started with an Apache HBase example in HDInsight</span></span>

<span data-ttu-id="c6ac3-106">Naučte se vytvářet cluster HBase v HDInsight, vytvářet tabulky HBase a dotazovat tabulky pomocí Hive.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-106">Learn how to create an HBase cluster in HDInsight, create HBase tables, and query tables by using Hive.</span></span> <span data-ttu-id="c6ac3-107">Obecné informace o HBase najdete v tématu [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview].</span><span class="sxs-lookup"><span data-stu-id="c6ac3-107">For general HBase information, see [HDInsight HBase overview][hdinsight-hbase-overview].</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a><span data-ttu-id="c6ac3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c6ac3-108">Prerequisites</span></span>
<span data-ttu-id="c6ac3-109">Než se pustíte do tohoto příkladu HBase, musíte mít následující položky:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-109">Before you begin trying this HBase example, you must have the following items:</span></span>

* <span data-ttu-id="c6ac3-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-110">**An Azure subscription**.</span></span> <span data-ttu-id="c6ac3-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="c6ac3-112">[Secure Shell (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> 
* <span data-ttu-id="c6ac3-113">[curl](http://curl.haxx.se/download.html).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-113">[curl](http://curl.haxx.se/download.html).</span></span>

## <a name="create-hbase-cluster"></a><span data-ttu-id="c6ac3-114">Vytvoření clusteru HBase</span><span class="sxs-lookup"><span data-stu-id="c6ac3-114">Create HBase cluster</span></span>
<span data-ttu-id="c6ac3-115">Následující postup používá šablonu Azure Resource Manageru pro vytvoření clusteru HBase se systémem Linux verze 3.4 a výchozího účtu služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-115">The following procedure uses an Azure Resource Manager template to create a version 3.4 Linux-based HBase cluster and the dependent default Azure Storage account.</span></span> <span data-ttu-id="c6ac3-116">Pro lepší pochopení parametrů použitých v postupu a dalších metod vytvoření clusteru si projděte téma [Vytvoření Hadoop clusterů se systémem Linux v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-116">To understand the parameters used in the procedure and other cluster creation methods, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="c6ac3-117">Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-117">Click the following image to open the template in the Azure portal.</span></span> <span data-ttu-id="c6ac3-118">Šablona se nachází ve veřejném kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-118">The template is located in a public blob container.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="c6ac3-119">V okně **Vlastní nasazení** zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-119">From the **Custom deployment** blade, enter the following values:</span></span>
   
   * <span data-ttu-id="c6ac3-120">**Předplatné:** Vyberte předplatné Azure, které se použije k vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-120">**Subscription**: Select your Azure subscription that is used to create the cluster.</span></span>
   * <span data-ttu-id="c6ac3-121">**Skupina prostředků:** Vytvořte skupinu správy prostředků Azure nebo použijte již existující.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-121">**Resource group**: Create an Azure Resource Management group or use an existing one.</span></span>
   * <span data-ttu-id="c6ac3-122">**Umístění**: Zadejte umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-122">**Location**: Specify the location of the resource group.</span></span> 
   * <span data-ttu-id="c6ac3-123">**Název clusteru:** Zadejte název pro cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-123">**ClusterName**: Enter a name for the HBase cluster.</span></span>
   * <span data-ttu-id="c6ac3-124">**Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je **admin**.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-124">**Cluster login name and password**: The default login name is **admin**.</span></span>
   * <span data-ttu-id="c6ac3-125">**Uživatelské jméno a heslo SSH**: výchozí uživatelské jméno **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-125">**SSH username and password**: The default username is **sshuser**.</span></span>  <span data-ttu-id="c6ac3-126">Můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-126">You can rename it.</span></span>
     
     <span data-ttu-id="c6ac3-127">Další parametry jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-127">Other parameters are optional.</span></span>  
     
     <span data-ttu-id="c6ac3-128">Každý cluster obsahuje závislost účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-128">Each cluster has an Azure Storage account dependency.</span></span> <span data-ttu-id="c6ac3-129">Po odstranění clusteru se data zachovají na účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-129">After you delete a cluster, the data retains in the storage account.</span></span> <span data-ttu-id="c6ac3-130">Výchozí název účtu úložiště clusteru je název clusteru s připojenou příponou „úložiště“.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-130">The cluster default storage account name is the cluster name with "store" appended.</span></span> <span data-ttu-id="c6ac3-131">Je pevně kódovaný v části proměnných šablon.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-131">It is hardcoded in the template variables section.</span></span>
3. <span data-ttu-id="c6ac3-132">Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře** a klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-132">Select **I agree to the terms and conditions stated above**, and then click **Purchase**.</span></span> <span data-ttu-id="c6ac3-133">Vytvoření clusteru trvá přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-133">It takes about 20 minutes to create a cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="c6ac3-134">Po odstranění clusteru služby HBase můžete vytvořit jiný cluster HBase pomocí stejného výchozího kontejneru blob.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-134">After an HBase cluster is deleted, you can create another HBase cluster by using the same default blob container.</span></span> <span data-ttu-id="c6ac3-135">Nový cluster převezme tabulky HBase, které jste vytvořili v původním clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-135">The new cluster picks up the HBase tables you created in the original cluster.</span></span> <span data-ttu-id="c6ac3-136">Aby se zabránilo nekonzistencím, doporučujeme zakázat tabulky HBase před odstraněním clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-136">To avoid inconsistencies, we recommend that you disable the HBase tables before you delete the cluster.</span></span>
> 
> 

## <a name="create-tables-and-insert-data"></a><span data-ttu-id="c6ac3-137">Vytváření tabulek a vkládání dat</span><span class="sxs-lookup"><span data-stu-id="c6ac3-137">Create tables and insert data</span></span>
<span data-ttu-id="c6ac3-138">SSH můžete použít při připojení ke clusterům HBase a používání prostředí HBase k vytváření tabulek HBase, vkládání dat a dotazování na data.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-138">You can use SSH to connect to HBase clusters and then use HBase Shell to create HBase tables, insert data, and query data.</span></span> <span data-ttu-id="c6ac3-139">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-139">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="c6ac3-140">Pro většinu osob se data zobrazí v tabulkovém formátu:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-140">For most people, data appears in the tabular format:</span></span>

![Tabulková data HDInsight HBase][img-hbase-sample-data-tabular]

<span data-ttu-id="c6ac3-142">V HBase (implementace BigTable) vypadají stejná data následovně:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-142">In HBase (an implementation of BigTable), the same data looks like:</span></span>

![Velké objemy tabulkových dat HDInsight HBase][img-hbase-sample-data-bigtable]


<span data-ttu-id="c6ac3-144">**Použití prostředí HBase**</span><span class="sxs-lookup"><span data-stu-id="c6ac3-144">**To use the HBase shell**</span></span>

1. <span data-ttu-id="c6ac3-145">Ze SSH spusťte následující příkaz HBase:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-145">From SSH, run the following HBase command:</span></span>
   
    ```bash
    hbase shell
    ```

2. <span data-ttu-id="c6ac3-146">Vytvořte HBase se skupinami o dvou sloupcích:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-146">Create an HBase with two-column families:</span></span>

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. <span data-ttu-id="c6ac3-147">Vložte některá data:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-147">Insert some data:</span></span>
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![Prostředí HDInsight Hadoop HBase][img-hbase-shell]
4. <span data-ttu-id="c6ac3-149">Získání jednoho řádku</span><span class="sxs-lookup"><span data-stu-id="c6ac3-149">Get a single row</span></span>
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    <span data-ttu-id="c6ac3-150">Měly by se zobrazit stejné výsledky jako pomocí příkazu vyhledávání, protože existuje pouze jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-150">You shall see the same results as using the scan command because there is only one row.</span></span>
   
    <span data-ttu-id="c6ac3-151">Další informace o schématu tabulky HBase najdete v tématu [Úvod do navrhování schémat HBase][hbase-schema].</span><span class="sxs-lookup"><span data-stu-id="c6ac3-151">For more information about the HBase table schema, see [Introduction to HBase Schema Design][hbase-schema].</span></span> <span data-ttu-id="c6ac3-152">Další příkazy HBase najdete v tématu [Referenční příručka Apache HBase][hbase-quick-start].</span><span class="sxs-lookup"><span data-stu-id="c6ac3-152">For more HBase commands, see [Apache HBase reference guide][hbase-quick-start].</span></span>
5. <span data-ttu-id="c6ac3-153">Opusťte prostředí</span><span class="sxs-lookup"><span data-stu-id="c6ac3-153">Exit the shell</span></span>
   
    ```hbaseshell
    exit
    ```

<span data-ttu-id="c6ac3-154">**Hromadné načítání dat do tabulky kontaktů HBase**</span><span class="sxs-lookup"><span data-stu-id="c6ac3-154">**To bulk load data into the contacts HBase table**</span></span>

<span data-ttu-id="c6ac3-155">HBase obsahuje několik metod načítání dat do tabulek.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-155">HBase includes several methods of loading data into tables.</span></span>  <span data-ttu-id="c6ac3-156">Další informace naleznete v tématu [Hromadné načítání](http://hbase.apache.org/book.html#arch.bulk.load).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-156">For more information, see [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load).</span></span>

<span data-ttu-id="c6ac3-157">Ukázkový datový soubor najdete ve veřejném kontejneru objektů blob: *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-157">A sample data file can be found in a public blob container, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span></span>  <span data-ttu-id="c6ac3-158">Obsah datového souboru je:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-158">The content of the data file is:</span></span>

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

<span data-ttu-id="c6ac3-159">Volitelně můžete vytvořit textový soubor a nahrát ho do vlastního účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-159">You can optionally create a text file and upload the file to your own storage account.</span></span> <span data-ttu-id="c6ac3-160">Pokyny najdete v tématu [Nahrávání dat pro úlohy Hadoop do služby HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="c6ac3-160">For the instructions, see [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data].</span></span>

> [!NOTE]
> <span data-ttu-id="c6ac3-161">Tento postup používá tabulku kontaktů HBase, kterou jste vytvořili v posledním postupu.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-161">This procedure uses the Contacts HBase table you have created in the last procedure.</span></span>
> 

1. <span data-ttu-id="c6ac3-162">Ze SSH spusťte následující příkaz, který transformuje datový soubor na StoreFiles a uloží ho do relativní cesty určené parametrem Dimporttsv.bulk.output.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-162">From SSH, run the following command to transform the data file to StoreFiles and store at a relative path specified by Dimporttsv.bulk.output.</span></span>  <span data-ttu-id="c6ac3-163">Pokud jste v prostředí HBase, odejděte pomocí příkazu exit.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-163">If you are in HBase Shell, use the exit command to exit.</span></span>

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. <span data-ttu-id="c6ac3-164">Spusťte následující příkaz a nahrajte data z adresy /example/data/storeDataFileOutput do tabulky HBase:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-164">Run the following command to upload the data from  /example/data/storeDataFileOutput to the HBase table:</span></span>
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. <span data-ttu-id="c6ac3-165">Prostředí HBase můžete otevřít a použít příkaz skenování k zobrazení seznamu obsahu tabulky.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-165">You can open the HBase shell, and use the scan command to list the table content.</span></span>

## <a name="use-hive-to-query-hbase"></a><span data-ttu-id="c6ac3-166">Použití Hive k dotazování HBase</span><span class="sxs-lookup"><span data-stu-id="c6ac3-166">Use Hive to query HBase</span></span>

<span data-ttu-id="c6ac3-167">Data v tabulkách HBase můžete dotazovat pomocí Hive.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-167">You can query data in HBase tables by using Hive.</span></span> <span data-ttu-id="c6ac3-168">V této části vytvoříte tabulku Hive, která se namapuje na tabulku HBase, a použijete ji k dotazování dat v tabulce HBase.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-168">In this section, you create a Hive table that maps to the HBase table and uses it to query the data in your HBase table.</span></span>

1. <span data-ttu-id="c6ac3-169">Otevřete **PuTTY** a připojte se ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-169">Open **PuTTY**, and connect to the cluster.</span></span>  <span data-ttu-id="c6ac3-170">Pokyny naleznete v předchozím postupu.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-170">See the instructions in the previous procedure.</span></span>
2. <span data-ttu-id="c6ac3-171">Z relace SSH pomocí následujícího příkazu spusťte Beeline:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-171">From the SSH session, use the following command to start Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="c6ac3-172">Další informace o Beeline najdete v tématu [Použití Hivu s Hadoopem ve službě HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-172">For more information about Beeline, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
       
3. <span data-ttu-id="c6ac3-173">Spusťte následující skript HiveQL k vytvoření tabulky Hive, která se mapuje na tabulku HBase.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-173">Run the following HiveQL script  to create a Hive table that maps to the HBase table.</span></span> <span data-ttu-id="c6ac3-174">Před spuštěním tohoto prohlášení ověřte, zda jste vytvořili ukázkové tabulky odkazované dříve v tomto kurzu pomocí prostředí HBase.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-174">Make sure that you have created the sample table referenced earlier in this tutorial by using the HBase shell before you run this statement.</span></span>

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. <span data-ttu-id="c6ac3-175">Spusťte následující skript HiveQL pro dotaz na data v tabulce HBase:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-175">Run the following HiveQL script to query the data in the HBase table:</span></span>

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a><span data-ttu-id="c6ac3-176">Použití rozhraní REST API HBase pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="c6ac3-176">Use HBase REST APIs using Curl</span></span>

<span data-ttu-id="c6ac3-177">Rozhraní API REST je zabezpečeno pomocí [základního ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-177">The REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="c6ac3-178">Požadavky byste vždy měli provádět pomocí protokolu HTTPS (Secure HTTP), čímž pomůžete zajistit, že se přihlašovací údaje budou na server odesílat bezpečně.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-178">You shall always make requests by using Secure HTTP (HTTPS) to help ensure that your credentials are securely sent to the server.</span></span>

2. <span data-ttu-id="c6ac3-179">Pomocí následujícího příkazu můžete zobrazit seznam existujících tabulek HBase:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-179">Use the following command to list the existing HBase tables:</span></span>

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. <span data-ttu-id="c6ac3-180">Pokud chcete vytvořit novou tabulku HBase se dvěma skupinami sloupců, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-180">Use the following command to create a new HBase table with two-column families:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    <span data-ttu-id="c6ac3-181">Schéma je k dispozici ve formátu JSon.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-181">The schema is provided in the JSon format.</span></span>
4. <span data-ttu-id="c6ac3-182">Chcete-li vložit nějaká data použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-182">Use the following command to insert some data:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    <span data-ttu-id="c6ac3-183">Hodnoty určené v přepínači -d musíte zakódovat base64.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-183">You must base64 encode the values specified in the -d switch.</span></span> <span data-ttu-id="c6ac3-184">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-184">In the example:</span></span>
   
   * <span data-ttu-id="c6ac3-185">MTAwMA==: 1000</span><span class="sxs-lookup"><span data-stu-id="c6ac3-185">MTAwMA==: 1000</span></span>
   * <span data-ttu-id="c6ac3-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span><span class="sxs-lookup"><span data-stu-id="c6ac3-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span></span>
   * <span data-ttu-id="c6ac3-187">Sm9obiBEb2xl: John Dole</span><span class="sxs-lookup"><span data-stu-id="c6ac3-187">Sm9obiBEb2xl: John Dole</span></span>
     
     <span data-ttu-id="c6ac3-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) umožňuje vložit více (dávkových) hodnot.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) allows you to insert multiple (batched) values.</span></span>
5. <span data-ttu-id="c6ac3-189">Pro získání řádku použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-189">Use the following command to get a row:</span></span>
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

<span data-ttu-id="c6ac3-190">Další informace o HBase Rest naleznete v tématu [Referenční příručka Apache HBase](https://hbase.apache.org/book.html#_rest).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-190">For more information about HBase Rest, see [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).</span></span>

> [!NOTE]
> <span data-ttu-id="c6ac3-191">Thrift není podporovaný HBase v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-191">Thrift is not supported by HBase in HDInsight.</span></span>
>
> <span data-ttu-id="c6ac3-192">Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména a hesla pro správce clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-192">When using Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the user name and password for the HDInsight cluster administrator.</span></span> <span data-ttu-id="c6ac3-193">Název clusteru také musíte použít jako součást identifikátoru URI (Uniform Resource Identifier) sloužícího k odesílání požadavků na server:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-193">You must also use the cluster name as part of the Uniform Resource Identifier (URI) used to send the requests to the server:</span></span>
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    <span data-ttu-id="c6ac3-194">Měla by se zobrazit odpověď podobná následující odpovědi:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-194">You should receive a response similar to the following response:</span></span>
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a><span data-ttu-id="c6ac3-195">Kontrola stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="c6ac3-195">Check cluster status</span></span>
<span data-ttu-id="c6ac3-196">HBase v HDInsight se dodává s webovým uživatelským rozhraním pro sledování clusterů.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-196">HBase in HDInsight ships with a Web UI for monitoring clusters.</span></span> <span data-ttu-id="c6ac3-197">Pomocí webového uživatelského rozhraní, můžete žádat o statistické údaje nebo informace o oblastech.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-197">Using the Web UI, you can request statistics or information about regions.</span></span>

<span data-ttu-id="c6ac3-198">**Přístup k hlavnímu uživatelskému rozhraní HBase**</span><span class="sxs-lookup"><span data-stu-id="c6ac3-198">**To access the HBase Master UI**</span></span>

1. <span data-ttu-id="c6ac3-199">Přihlaste se k webovému uživatelskému rozhraní Ambari na adrese https://&lt;název_clusteru>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-199">Sign into the the Ambari Web UI at https://&lt;Clustername>.azurehdinsight.net.</span></span>
2. <span data-ttu-id="c6ac3-200">V nabídce vlevo klikněte na **HBase**.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-200">Click **HBase** from the left menu.</span></span>
3. <span data-ttu-id="c6ac3-201">V horní části stránky klikněte na **Rychlé odkazy**, najeďte myší na odkaz na aktivní uzel Zookeeper a klikněte na **Hlavní uživatelské rozhraní HBase**.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-201">Click **Quick links** on the top of the page, point to the active Zookeeper node link, and then click **HBase Master UI**.</span></span>  <span data-ttu-id="c6ac3-202">Uživatelské rozhraní se otevře na nové kartě prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-202">The UI is opened in another browser tab:</span></span>

  ![Hlavní uživatelské rozhraní HDInsight HBase](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  <span data-ttu-id="c6ac3-204">Hlavní uživatelské rozhraní HBase obsahuje tyto části:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-204">The HBase Master UI contains the following sections:</span></span>

  - <span data-ttu-id="c6ac3-205">oblastní servery</span><span class="sxs-lookup"><span data-stu-id="c6ac3-205">region servers</span></span>
  - <span data-ttu-id="c6ac3-206">zálohování hlavních serverů</span><span class="sxs-lookup"><span data-stu-id="c6ac3-206">backup masters</span></span>
  - <span data-ttu-id="c6ac3-207">tabulky</span><span class="sxs-lookup"><span data-stu-id="c6ac3-207">tables</span></span>
  - <span data-ttu-id="c6ac3-208">úlohy</span><span class="sxs-lookup"><span data-stu-id="c6ac3-208">tasks</span></span>
  - <span data-ttu-id="c6ac3-209">atributy softwaru</span><span class="sxs-lookup"><span data-stu-id="c6ac3-209">software attributes</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="c6ac3-210">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="c6ac3-210">Delete the cluster</span></span>
<span data-ttu-id="c6ac3-211">Aby se zabránilo nekonzistencím, doporučujeme zakázat tabulky HBase před odstraněním clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-211">To avoid inconsistencies, we recommend that you disable the HBase tables before you delete the cluster.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="c6ac3-212">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c6ac3-212">Troubleshoot</span></span>

<span data-ttu-id="c6ac3-213">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="c6ac3-213">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6ac3-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6ac3-214">Next steps</span></span>
<span data-ttu-id="c6ac3-215">V tomto článku jste se dozvěděli, jak vytvořit cluster HBase a jak vytvářet tabulky a zobrazovat data v těchto tabulkách z prostředí HBase.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-215">In this article, you learned how to create an HBase cluster and how to create tables and view the data in those tables from the HBase shell.</span></span> <span data-ttu-id="c6ac3-216">Také jste se naučili, jak používat dotazy na data Hive v tabulkách HBase a jak používat rozhraní REST API HBase C# k vytvoření tabulky HBase a načtení dat z tabulky.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-216">You also learned how to use a Hive query on data in HBase tables and how to use the HBase C# REST APIs to create an HBase table and retrieve data from the table.</span></span>

<span data-ttu-id="c6ac3-217">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c6ac3-217">To learn more, see:</span></span>

* <span data-ttu-id="c6ac3-218">[Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="c6ac3-218">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>

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
