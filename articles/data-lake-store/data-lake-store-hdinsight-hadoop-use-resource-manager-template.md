---
title: "toocreate aaaUse šablony Azure HDInsight a Data Lake Store | Microsoft Docs"
description: "Použijte toocreate šablony Azure Resource Manager a clusterů HDInsight pomocí Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: nitinme
ms.openlocfilehash: eb88a626f2837dcc29295f3f73a91757059c3bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a><span data-ttu-id="c38ee-103">Vytvoření clusteru HDInsight s Data Lake Store pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="c38ee-103">Create an HDInsight cluster with Data Lake Store using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c38ee-104">Pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="c38ee-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="c38ee-105">Pomocí prostředí PowerShell (pro výchozí úložiště)</span><span class="sxs-lookup"><span data-stu-id="c38ee-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="c38ee-106">Pomocí prostředí PowerShell (pro další úložiště)</span><span class="sxs-lookup"><span data-stu-id="c38ee-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="c38ee-107">Pomocí Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="c38ee-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="c38ee-108">Zjistěte, jak tooconfigure toouse prostředí Azure PowerShell HDInsight cluster s Azure Data Lake Store, **jako další úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c38ee-108">Learn how toouse Azure PowerShell tooconfigure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span>

<span data-ttu-id="c38ee-109">Pro typy podporované clusteru Data Lake Store použít jako výchozí úložiště nebo účtu další úložiště.</span><span class="sxs-lookup"><span data-stu-id="c38ee-109">For supported cluster types, Data Lake Store be used as an default storage or additional storage account.</span></span> <span data-ttu-id="c38ee-110">Pokud se použije jako další úložiště Data Lake Store, hello výchozí účet úložiště pro clustery hello budou mít pořád Azure úložiště objektů BLOB (WASB) a hello clusteru související soubory (například protokoly atd.) se zapisují stále toohello výchozí úložiště, při hello dat, které chtít tooprocess můžou být uložené v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-110">When Data Lake Store is used as additional storage, hello default storage account for hello clusters will still be Azure Storage Blobs (WASB) and hello cluster-related files (such as logs, etc.) are still written toohello default storage, while hello data that you want tooprocess can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="c38ee-111">Pomocí Data Lake Store jako další úložiště účet nemá negativní vliv na výkon nebo hello možnost tooread a zápis toohello úložiště z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-111">Using Data Lake Store as an additional storage account does not impact performance or hello ability tooread/write toohello storage from hello cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="c38ee-112">Pro úložiště clusteru HDInsight pomocí Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c38ee-112">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="c38ee-113">Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="c38ee-113">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="c38ee-114">Clustery HDInsight možnost toocreate s přístupem k tooData Lake Store jako výchozí úložiště je k dispozici pro HDInsight verze 3.5 a 3.6.</span><span class="sxs-lookup"><span data-stu-id="c38ee-114">Option toocreate HDInsight clusters with access tooData Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="c38ee-115">Clustery HDInsight možnost toocreate s přístupem k tooData Lake Store jako další úložiště je k dispozici pro HDInsight verze 3.2, 3,4, 3.5 a 3.6.</span><span class="sxs-lookup"><span data-stu-id="c38ee-115">Option toocreate HDInsight clusters with access tooData Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="c38ee-116">V tomto článku jsme zřízení clusteru Hadoop s Data Lake Store jako další úložiště.</span><span class="sxs-lookup"><span data-stu-id="c38ee-116">In this article, we provision a Hadoop cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="c38ee-117">Pokyny jak toocreate Hadoop cluster s Data Lake Store jako výchozí úložiště, najdete v části [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c38ee-117">For instructions on how toocreate a Hadoop cluster with Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c38ee-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c38ee-118">Prerequisites</span></span>
<span data-ttu-id="c38ee-119">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="c38ee-119">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="c38ee-120">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="c38ee-120">**An Azure subscription**.</span></span> <span data-ttu-id="c38ee-121">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c38ee-121">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c38ee-122">**Azure PowerShell 1.0 nebo vyšší**.</span><span class="sxs-lookup"><span data-stu-id="c38ee-122">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="c38ee-123">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c38ee-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="c38ee-124">**Azure Active Directory instanční objekt**.</span><span class="sxs-lookup"><span data-stu-id="c38ee-124">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="c38ee-125">Kroky v tomto kurzu poskytují pokyny o tom, toocreate objektu služby ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c38ee-125">Steps in this tutorial provide instructions on how toocreate a service principal in Azure AD.</span></span> <span data-ttu-id="c38ee-126">Ale musí být webu možné toocreate pro Azure AD správce toobe hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="c38ee-126">However, you must be an Azure AD administrator toobe able toocreate a service principal.</span></span> <span data-ttu-id="c38ee-127">Pokud jste správce Azure AD, můžete přeskočit tento požadavek a pokračovat v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-127">If you are an Azure AD administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    <span data-ttu-id="c38ee-128">**Pokud si nejste správce Azure AD**, nebudete moct tooperform hello kroky požadované toocreate hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="c38ee-128">**If you are not an Azure AD administrator**, you will not be able tooperform hello steps required toocreate a service principal.</span></span> <span data-ttu-id="c38ee-129">V takovém případě musíte vám správce Azure AD nejdřív vytvořit objekt služby, před vytvořením clusteru HDInsight s Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-129">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="c38ee-130">Navíc hello instanční objekt musí být vytvořen pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="c38ee-130">Also, hello service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a><span data-ttu-id="c38ee-131">Vytvoření clusteru HDInsight s Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c38ee-131">Create an HDInsight cluster with Azure Data Lake Store</span></span>
<span data-ttu-id="c38ee-132">Hello šablony Resource Manageru a hello požadavků na používání hello šablony jsou dostupné na webu GitHub na [nasadit cluster HDInsight Linux s novou Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="c38ee-132">hello Resource Manager template, and hello prerequisites for using hello template, are available on GitHub at [Deploy a HDInsight Linux cluster with new Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span></span> <span data-ttu-id="c38ee-133">Postupujte podle pokynů hello na tento odkaz toocreate zadaný cluster služby HDInsight s Azure Data Lake Store jako další úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-133">Follow hello instructions provided at this link toocreate an HDInsight cluster with Azure Data Lake Store as hello additional storage.</span></span>

<span data-ttu-id="c38ee-134">odkaz hello výše uvedených pokynů Hello vyžadují prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c38ee-134">hello instructions at hello link mentioned above require PowerShell.</span></span> <span data-ttu-id="c38ee-135">Než začnete pomocí těchto pokynů, zkontrolujte, zda že přihlásit tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c38ee-135">Before you start with those instructions, make sure you log in tooyour Azure account.</span></span> <span data-ttu-id="c38ee-136">Z plochy otevřete nové okno Azure PowerShell a zadejte následující fragmenty hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-136">From your desktop, open a new Azure PowerShell window, and enter hello following snippets.</span></span> <span data-ttu-id="c38ee-137">Když výzvami toolog, ujistěte se můžete přihlásit jako jeden ze správců/vlastník předplatného hello:</span><span class="sxs-lookup"><span data-stu-id="c38ee-137">When prompted toolog in, make sure you log in as one of hello subscription admininistrators/owner:</span></span>

```
# Log in tooyour Azure account
Login-AzureRmAccount

# List all hello subscriptions associated tooyour account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-toohello-azure-data-lake-store"></a><span data-ttu-id="c38ee-138">Nahrát ukázková data toohello Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c38ee-138">Upload sample data toohello Azure Data Lake Store</span></span>
<span data-ttu-id="c38ee-139">šablony Resource Manageru Hello vytvoří nový účet Data Lake Store a přidruží ji s clusterem HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-139">hello Resource Manager template creates a new Data Lake Store account and associates it with hello HDInsight cluster.</span></span> <span data-ttu-id="c38ee-140">Teď musíte nahrát některých ukázkových dat toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-140">You must now upload some sample data toohello Data Lake Store.</span></span> <span data-ttu-id="c38ee-141">Budete potřebovat později v hello úlohy kurz toorun z clusteru služby HDInsight, které přístup k datům v Data Lake Store hello tato data.</span><span class="sxs-lookup"><span data-stu-id="c38ee-141">You'll need this data later in hello tutorial toorun jobs from an HDInsight cluster that access data in hello Data Lake Store.</span></span> <span data-ttu-id="c38ee-142">Pokyny najdete v části tooupload data [nahrát soubor tooyour Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span><span class="sxs-lookup"><span data-stu-id="c38ee-142">For instructions on how tooupload data, see [Upload a file tooyour Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span></span> <span data-ttu-id="c38ee-143">Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="c38ee-143">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

## <a name="set-relevant-acls-on-hello-sample-data"></a><span data-ttu-id="c38ee-144">Nastavit příslušné seznamy ACL na hello ukázková data</span><span class="sxs-lookup"><span data-stu-id="c38ee-144">Set relevant ACLs on hello sample data</span></span>
<span data-ttu-id="c38ee-145">toomake se, že je dostupný z clusteru HDInsight hello hello ukázková data, který nahrajete, musíte zajistit, že aplikace hello Azure AD, která je použité tooestablish identity mezi hello HDInsight cluster a Data Lake Store má přístup toohello soubor nebo složku, která se Probíhá tooaccess.</span><span class="sxs-lookup"><span data-stu-id="c38ee-145">toomake sure hello sample data you upload is accessible from hello HDInsight cluster, you must ensure that hello Azure AD application that is used tooestablish identity between hello HDInsight cluster and Data Lake Store has access toohello file/folder you are trying tooaccess.</span></span> <span data-ttu-id="c38ee-146">toodo, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-146">toodo this, perform hello following steps.</span></span>

1. <span data-ttu-id="c38ee-147">Najít název hello hello aplikaci Azure AD, který je přidružen HDInsight cluster a hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-147">Find hello name of hello Azure AD application that is associated with HDInsight cluster and hello Data Lake Store.</span></span> <span data-ttu-id="c38ee-148">Jedním ze způsobů toolook pro název hello je tooopen hello HDInsight clusteru okno, které jste vytvořili pomocí šablony Resource Manageru hello, klikněte na tlačítko hello **identita AAD clusteru** kartě a vyhledejte hodnotu hello **instančního objektu Zobrazovaný název**.</span><span class="sxs-lookup"><span data-stu-id="c38ee-148">One way toolook for hello name is tooopen hello HDInsight cluster blade that you created using hello Resource Manager template, click hello **Cluster AAD Identity** tab, and look for hello value of **Service Principal Display Name**.</span></span>
2. <span data-ttu-id="c38ee-149">Nyní poskytují přístup toothis aplikaci Azure AD na hello soubor nebo složku, které chcete tooaccess z clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="c38ee-149">Now, provide access toothis Azure AD application on hello file/folder that you want tooaccess from hello HDInsight cluster.</span></span> <span data-ttu-id="c38ee-150">tooset hello vpravo seznamy ACL na hello soubor nebo složku v Data Lake Store najdete v části [zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span><span class="sxs-lookup"><span data-stu-id="c38ee-150">tooset hello right ACLs on hello file/folder in Data Lake Store, see [Securing data in Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span></span>

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a><span data-ttu-id="c38ee-151">Spustit testovací úlohy na hello HDInsight clusteru toouse hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c38ee-151">Run test jobs on hello HDInsight cluster toouse hello Data Lake Store</span></span>
<span data-ttu-id="c38ee-152">Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na hello clusteru tootest této hello HDInsight clusteru můžete získat přístup k Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-152">After you have configured an HDInsight cluster, you can run test jobs on hello cluster tootest that hello HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="c38ee-153">toodo tedy jsme se spustí úlohy Hive ukázka, která vytvoří tabulku pomocí hello ukázková data, který jste nahráli starší tooyour Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-153">toodo so, we will run a sample Hive job that creates a table using hello sample data that you uploaded earlier tooyour Data Lake Store.</span></span>

<span data-ttu-id="c38ee-154">V této části budete SSH do služby clusteru HDInsight Linux a spuštění hello ukázkový dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="c38ee-154">In this section you will SSH into an HDInsight Linux cluster and run hello a sample Hive query.</span></span> <span data-ttu-id="c38ee-155">Pokud používáte klienta se systémem Windows, doporučujeme používat **PuTTY**, který si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="c38ee-155">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="c38ee-156">Další informace o použití klienta PuTTY, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="c38ee-156">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

1. <span data-ttu-id="c38ee-157">Po připojení, spusťte hello Hive rozhraní příkazového řádku pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c38ee-157">Once connected, start hello Hive CLI by using hello following command:</span></span>

   ```
   hive
   ```
2. <span data-ttu-id="c38ee-158">Pomocí hello rozhraní příkazového řádku, zadejte následující příkazy toocreate novou tabulku s názvem hello **vozidel** pomocí hello ukázkových dat v Data Lake Store hello:</span><span class="sxs-lookup"><span data-stu-id="c38ee-158">Using hello CLI, enter hello following statements toocreate a new table named **vehicles** by using hello sample data in hello Data Lake Store:</span></span>

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   <span data-ttu-id="c38ee-159">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="c38ee-159">You should see an output similar toohello following:</span></span>

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="c38ee-160">Přístup k Data Lake Store pomocí příkazů HDFS</span><span class="sxs-lookup"><span data-stu-id="c38ee-160">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="c38ee-161">Po nakonfigurování hello HDInsight clusteru toouse Data Lake Store můžete hello HDFS prostředí příkazy tooaccess hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="c38ee-161">Once you have configured hello HDInsight cluster toouse Data Lake Store, you can use hello HDFS shell commands tooaccess hello store.</span></span>

<span data-ttu-id="c38ee-162">V této části budete SSH do služby clusteru HDInsight Linux a spuštění hello příkazů HDFS.</span><span class="sxs-lookup"><span data-stu-id="c38ee-162">In this section you will SSH into an HDInsight Linux cluster and run hello HDFS commands.</span></span> <span data-ttu-id="c38ee-163">Pokud používáte klienta se systémem Windows, doporučujeme používat **PuTTY**, který si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="c38ee-163">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="c38ee-164">Další informace o použití klienta PuTTY, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="c38ee-164">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

<span data-ttu-id="c38ee-165">Po připojení použijte hello následující soubory o hello toolist příkazů systému souborů HDFS v hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-165">Once connected, use hello following HDFS filesystem command toolist hello files in hello Data Lake Store.</span></span>

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

<span data-ttu-id="c38ee-166">To by měl seznamu hello souboru, který jste nahráli starší toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c38ee-166">This should list hello file that you uploaded earlier toohello Data Lake Store.</span></span>

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

<span data-ttu-id="c38ee-167">Můžete taky hello `hdfs dfs -put` příkaz tooupload některé soubory toohello Data Lake Store a pak použijte `hdfs dfs -ls` tooverify jestli hello soubory úspěšně se nahrál.</span><span class="sxs-lookup"><span data-stu-id="c38ee-167">You can also use hello `hdfs dfs -put` command tooupload some files toohello Data Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c38ee-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c38ee-168">Next steps</span></span>
* [<span data-ttu-id="c38ee-169">Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="c38ee-169">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-wasb-distcp.md)
