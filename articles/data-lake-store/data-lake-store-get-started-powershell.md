---
title: "prostředí PowerShell tooget aaaUse začít s Azure Data Lake Store | Microsoft Docs"
description: "Použití Azure PowerShell toocreate účtu Data Lake Store a provádění základních operací"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 9c958bfd63e412ec0b0a4113a149f61aee026bc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a><span data-ttu-id="abc79-103">Začínáme s Azure Data Lake Store pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="abc79-103">Get started with Azure Data Lake Store using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="abc79-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="abc79-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="abc79-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="abc79-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="abc79-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="abc79-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="abc79-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="abc79-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="abc79-108">REST API</span><span class="sxs-lookup"><span data-stu-id="abc79-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="abc79-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="abc79-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="abc79-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="abc79-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="abc79-111">Python</span><span class="sxs-lookup"><span data-stu-id="abc79-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="abc79-112">Zjistěte, jak toouse prostředí Azure PowerShell toocreate Azure Data Lake ukládání účtu a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="abc79-112">Learn how toouse Azure PowerShell toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abc79-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="abc79-113">Prerequisites</span></span>
<span data-ttu-id="abc79-114">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="abc79-114">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="abc79-115">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="abc79-115">**An Azure subscription**.</span></span> <span data-ttu-id="abc79-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abc79-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="abc79-117">**Azure PowerShell 1.0 nebo vyšší**.</span><span class="sxs-lookup"><span data-stu-id="abc79-117">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="abc79-118">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="abc79-118">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="authentication"></a><span data-ttu-id="abc79-119">Authentication</span><span class="sxs-lookup"><span data-stu-id="abc79-119">Authentication</span></span>
<span data-ttu-id="abc79-120">Tento článek používá jednodušší metodu ověřování s Data Lake Store, kde jsou výzvami tooenter přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="abc79-120">This article uses a simpler authentication approach with Data Lake Store where you are prompted tooenter your Azure account credentials.</span></span> <span data-ttu-id="abc79-121">Hello přístup úrovně tooData Lake Store účtu a systém souborů je pak řídí hello úroveň přístupu tohoto hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="abc79-121">hello access level tooData Lake Store account and file system is then governed by hello access level of hello logged in user.</span></span> <span data-ttu-id="abc79-122">Nicméně existují další postupy jako dobře tooauthenticate s Data Lake Store, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="abc79-122">However, there are other approaches as well tooauthenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="abc79-123">Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="abc79-123">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="abc79-124">Vytvoření účtu Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-124">Create an Azure Data Lake Store account</span></span>
1. <span data-ttu-id="abc79-125">Z plochy otevřete nové okno prostředí Windows PowerShell, zadejte hello následující fragment kódu toolog v tooyour účet Azure, nastavte hello předplatné a zaregistrujte poskytovatele Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="abc79-125">From your desktop, open a new Windows PowerShell window, and enter hello following snippet toolog in tooyour Azure account, set hello subscription, and register hello Data Lake Store provider.</span></span> <span data-ttu-id="abc79-126">Když výzvami toolog, ujistěte se můžete přihlásit jako jeden ze správců/vlastník předplatného hello:</span><span class="sxs-lookup"><span data-stu-id="abc79-126">When prompted toolog in, make sure you log in as one of hello subscription admininistrators/owner:</span></span>

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. <span data-ttu-id="abc79-127">Účet Azure Data Lake Store je přidružený ke skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="abc79-127">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="abc79-128">Začněte vytvořením skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="abc79-128">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="abc79-129">![Vytvoření skupiny prostředků Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Vytvoření skupiny prostředků Azure")</span><span class="sxs-lookup"><span data-stu-id="abc79-129">![Create an Azure Resource Group](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")</span></span>
3. <span data-ttu-id="abc79-130">Vytvořte účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abc79-130">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="abc79-131">Hello název, vámi zadaných musí obsahovat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="abc79-131">hello name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="abc79-132">![Vytvoření účtu Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Vytvoření účtu Azure Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="abc79-132">![Create an Azure Data Lake Store account](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")</span></span>
4. <span data-ttu-id="abc79-133">Ověřte, že hello účet je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="abc79-133">Verify that hello account is successfully created.</span></span>

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    <span data-ttu-id="abc79-134">Výstup by měl proběhnout Hello **True**.</span><span class="sxs-lookup"><span data-stu-id="abc79-134">hello output for this should be **True**.</span></span>

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a><span data-ttu-id="abc79-135">Vytváření struktur adresářů v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-135">Create directory structures in your Azure Data Lake Store</span></span>
<span data-ttu-id="abc79-136">Můžete vytvářet adresáře v rámci vaší toomanage účtu Azure Data Lake Store a ukládat data.</span><span class="sxs-lookup"><span data-stu-id="abc79-136">You can create directories under your Azure Data Lake Store account toomanage and store data.</span></span>

1. <span data-ttu-id="abc79-137">Zadejte kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="abc79-137">Specify a root directory.</span></span>

        $myrootdir = "/"
2. <span data-ttu-id="abc79-138">Vytvořte nový adresář s názvem **mynewdirectory** v kořenovém adresáři zadané hello.</span><span class="sxs-lookup"><span data-stu-id="abc79-138">Create a new directory called **mynewdirectory** under hello specified root.</span></span>

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. <span data-ttu-id="abc79-139">Ověřte, že tento nový adresář hello je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="abc79-139">Verify that hello new directory is successfully created.</span></span>

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    <span data-ttu-id="abc79-140">Měl by se zobrazit výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="abc79-140">It should show an output like hello following:</span></span>

    <span data-ttu-id="abc79-141">![Ověření adresáře](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Ověření adresáře")</span><span class="sxs-lookup"><span data-stu-id="abc79-141">![Verify Directory](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")</span></span>

## <a name="upload-data-tooyour-azure-data-lake-store"></a><span data-ttu-id="abc79-142">Nahrání dat tooyour Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-142">Upload data tooyour Azure Data Lake Store</span></span>
<span data-ttu-id="abc79-143">Můžete nahrát data Lake Store tooData přímo na hello kořenové úrovni nebo tooa adresář, který jste vytvořili v rámci účtu hello.</span><span class="sxs-lookup"><span data-stu-id="abc79-143">You can upload your data tooData Lake Store directly at hello root level or tooa directory that you created within hello account.</span></span> <span data-ttu-id="abc79-144">Hello níže zobrazené fragmenty kódu ukazují, jak tooupload adresáře toohello některých ukázkových dat (**mynewdirectory**) jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="abc79-144">hello snippets below demonstrate how tooupload some sample data toohello directory (**mynewdirectory**) you created in hello previous section.</span></span>

<span data-ttu-id="abc79-145">Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="abc79-145">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="abc79-146">Stáhněte si soubor hello a uložte ho do místního adresáře v počítači, například C:\sampledata\.</span><span class="sxs-lookup"><span data-stu-id="abc79-146">Download hello file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a><span data-ttu-id="abc79-147">Přejmenování, stažení a odstranění dat z Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-147">Rename, download, and delete data from your Data Lake Store</span></span>
<span data-ttu-id="abc79-148">toorename soubor, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="abc79-148">toorename a file, use hello following command:</span></span>

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="abc79-149">toodownload soubor, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="abc79-149">toodownload a file, use hello following command:</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

<span data-ttu-id="abc79-150">toodelete soubor, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="abc79-150">toodelete a file, use hello following command:</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="abc79-151">Po zobrazení výzvy zadejte **Y** toodelete hello položky.</span><span class="sxs-lookup"><span data-stu-id="abc79-151">When prompted, enter **Y** toodelete hello item.</span></span> <span data-ttu-id="abc79-152">Pokud máte více než jeden soubor toodelete, můžete zadat všechny hello cesty oddělené čárkou.</span><span class="sxs-lookup"><span data-stu-id="abc79-152">If you have more than one file toodelete, you can provide all hello paths separated by comma.</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a><span data-ttu-id="abc79-153">Odstranění účtu Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-153">Delete your Azure Data Lake Store account</span></span>
<span data-ttu-id="abc79-154">Použijte následující příkaz toodelete hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abc79-154">Use hello following command toodelete your Data Lake Store account.</span></span>

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

<span data-ttu-id="abc79-155">Po zobrazení výzvy zadejte **Y** toodelete hello účtu.</span><span class="sxs-lookup"><span data-stu-id="abc79-155">When prompted, enter **Y** toodelete hello account.</span></span>

## <a name="performance-guidance-while-using-powershell"></a><span data-ttu-id="abc79-156">Průvodce výkonem při použití prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="abc79-156">Performance guidance while using PowerShell</span></span>

<span data-ttu-id="abc79-157">Níže jsou hello nejdůležitější nastavení, které mohou být ujít tooget hello nejlepší výkon při použití prostředí PowerShell toowork s Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="abc79-157">Below are hello most important settings that can be tuned tooget hello best performance while using PowerShell toowork with Data Lake Store:</span></span>

| <span data-ttu-id="abc79-158">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="abc79-158">Property</span></span>            | <span data-ttu-id="abc79-159">Výchozí</span><span class="sxs-lookup"><span data-stu-id="abc79-159">Default</span></span> | <span data-ttu-id="abc79-160">Popis</span><span class="sxs-lookup"><span data-stu-id="abc79-160">Description</span></span> |
|---------------------|---------|-------------|
| <span data-ttu-id="abc79-161">PerFileThreadCount</span><span class="sxs-lookup"><span data-stu-id="abc79-161">PerFileThreadCount</span></span>  | <span data-ttu-id="abc79-162">10</span><span class="sxs-lookup"><span data-stu-id="abc79-162">10</span></span>      | <span data-ttu-id="abc79-163">Tento parametr umožňuje toochoose hello počet paralelních vláken pro nahrávání nebo stahování každý soubor.</span><span class="sxs-lookup"><span data-stu-id="abc79-163">This parameter enables you toochoose hello number of parallel threads for uploading or downloading each file.</span></span> <span data-ttu-id="abc79-164">Toto číslo představuje hello maximální počet vláken, které mohou být přiděleny na soubor, ale může získat méně vláken v závislosti na váš scénář (například pokud odesíláte soubor 1 KB, i když je požádat o 20 vláken obdržíte jedno vlákno).</span><span class="sxs-lookup"><span data-stu-id="abc79-164">This number represents hello max threads that can be allocated per file, but you may get less threads depending on your scenario (e.g. if you are uploading a 1KB file, you will get one thread even if you ask for 20 threads).</span></span>  |
| <span data-ttu-id="abc79-165">ConcurrentFileCount</span><span class="sxs-lookup"><span data-stu-id="abc79-165">ConcurrentFileCount</span></span> | <span data-ttu-id="abc79-166">10</span><span class="sxs-lookup"><span data-stu-id="abc79-166">10</span></span>      | <span data-ttu-id="abc79-167">Tento parametr je určený zejména pro nahrávání nebo stahování složek.</span><span class="sxs-lookup"><span data-stu-id="abc79-167">This parameter is specifically for uploading or downloading folders.</span></span> <span data-ttu-id="abc79-168">Tento parametr určuje hello počet souběžných souborů, které můžete nahrát nebo stáhnout.</span><span class="sxs-lookup"><span data-stu-id="abc79-168">This parameter determines hello number of concurrent files that can be uploaded or downloaded.</span></span> <span data-ttu-id="abc79-169">Toto číslo představuje hello maximální počet souběžných souborů, které je možné nahrát nebo stáhnout najednou, ale může získat méně souběžnosti v závislosti na váš scénář (například pokud nahráváte dva soubory, zobrazí se dvě nahrávání souborů souběžných i v případě, že je požádat o 15).</span><span class="sxs-lookup"><span data-stu-id="abc79-169">This number represents hello maximum number of concurrent files that can be uploaded or downloaded at one time, but you may get less concurrency depending on your scenario (e.g. if you are uploading two files, you will get two concurrent file uploads even if you ask for 15).</span></span> |

<span data-ttu-id="abc79-170">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="abc79-170">**Example**</span></span>

<span data-ttu-id="abc79-171">Tento příkaz stáhne soubory z místního disku uživatele toohello Azure Data Lake Store pomocí 20 vláken na soubor a 100 souběžných soubory.</span><span class="sxs-lookup"><span data-stu-id="abc79-171">This command downloads files from Azure Data Lake Store toohello user's local drive using 20 threads per file and 100 concurrent files.</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-hello-value-tooset-for-these-parameters"></a><span data-ttu-id="abc79-172">Jak zjistím hello tooset hodnoty pro tyto parametry?</span><span class="sxs-lookup"><span data-stu-id="abc79-172">How do I determine hello value tooset for these parameters?</span></span>

<span data-ttu-id="abc79-173">Tady je několik rad, kterými se můžete řídit.</span><span class="sxs-lookup"><span data-stu-id="abc79-173">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="abc79-174">**Krok 1: Určení počet celkový počet vláken hello** -byste měli začít výpočtu toouse počet celkový počet vláken hello.</span><span class="sxs-lookup"><span data-stu-id="abc79-174">**Step 1: Determine hello total thread count** - You should start by calculating hello total thread count toouse.</span></span> <span data-ttu-id="abc79-175">Podle obecných pokynů byste měli použít 6 vláken na každé fyzické jádro.</span><span class="sxs-lookup"><span data-stu-id="abc79-175">As a general guideline, you should use 6 threads for each physical core.</span></span>

        Total thread count = total physical cores * 6

    <span data-ttu-id="abc79-176">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="abc79-176">**Example**</span></span>

    <span data-ttu-id="abc79-177">Za předpokladu, že používáte hello prostředí PowerShell příkazy z D14 virtuálního počítače, který má 16 jader</span><span class="sxs-lookup"><span data-stu-id="abc79-177">Assuming you are running hello PowerShell commands from a D14 VM that has 16 cores</span></span>

        Total thread count = 16 cores * 6 = 96 threads


* <span data-ttu-id="abc79-178">**Krok 2: Vypočítat PerFileThreadCount** -jsme vypočítat naše PerFileThreadCount na základě velikosti hello hello souborů.</span><span class="sxs-lookup"><span data-stu-id="abc79-178">**Step 2: Calculate PerFileThreadCount**  - We calculate our PerFileThreadCount based on hello size of hello files.</span></span> <span data-ttu-id="abc79-179">Pro soubory je menší než 2,5 GB není bez nutnosti toochange tento parametr protože stačí hello výchozí hodnotu 10.</span><span class="sxs-lookup"><span data-stu-id="abc79-179">For files smaller than 2.5GB, there is no need toochange this parameter because hello default of 10 is sufficient.</span></span> <span data-ttu-id="abc79-180">Pro soubory větší než 2,5 GB byste měli používat 10 vláken, hello základ pro hello první 2,5 GB a přidejte 1 vlákno pro každý další 256 MB nárůst velikosti souboru.</span><span class="sxs-lookup"><span data-stu-id="abc79-180">For files larger than 2.5GB, you should use 10 threads as hello base for hello first 2.5GB and add 1 thread for each additional 256MB increase in file size.</span></span> <span data-ttu-id="abc79-181">Pokud kopírujete složku se soubory velmi rozdílných velikostí, zvažte jejich seskupení podle podobných velikostí.</span><span class="sxs-lookup"><span data-stu-id="abc79-181">If you are copying a folder with a large range of file sizes, consider grouping them into similar file sizes.</span></span> <span data-ttu-id="abc79-182">Velmi rozdílné velikosti souborů mohou způsobit, že výkon nebude optimální.</span><span class="sxs-lookup"><span data-stu-id="abc79-182">Having dissimilar file sizes may cause non-optimal performance.</span></span> <span data-ttu-id="abc79-183">Pokud není možné toogroup podobné velikosti souborů, měli byste nastavit PerFileThreadCount podle hello největší velikost souboru.</span><span class="sxs-lookup"><span data-stu-id="abc79-183">If that's not possible toogroup similar file sizes, you should set PerFileThreadCount based on hello largest file size.</span></span>

        PerFileThreadCount = 10 threads for hello first 2.5GB + 1 thread for each additional 256MB increase in file size

    <span data-ttu-id="abc79-184">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="abc79-184">**Example**</span></span>

    <span data-ttu-id="abc79-185">Za předpokladu, že máte 100 souborů od too10GB 1 GB, použijeme hello 10 GB, stejně jako hello největší velikost pro rovnice, který bude číst jako následující hello souboru.</span><span class="sxs-lookup"><span data-stu-id="abc79-185">Assuming you have 100 files ranging from 1GB too10GB, we use hello 10GB as hello largest file size for equation, which would read like hello following.</span></span>

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* <span data-ttu-id="abc79-186">**Krok 3: Vypočítat ConcurrentFilecount** -počet použití hello celkový počet vláken a PerFileThreadCount toocalculate ConcurrentFileCount podle následující rovnice hello.</span><span class="sxs-lookup"><span data-stu-id="abc79-186">**Step 3: Calculate ConcurrentFilecount** - Use hello total thread count and PerFileThreadCount toocalculate ConcurrentFileCount based on hello following equation.</span></span>

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    <span data-ttu-id="abc79-187">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="abc79-187">**Example**</span></span>

    <span data-ttu-id="abc79-188">Podle hello ukázkové hodnoty, které jsme pomocí</span><span class="sxs-lookup"><span data-stu-id="abc79-188">Based on hello example values we have been using</span></span>

        96 = 40 * ConcurrentFileCount

    <span data-ttu-id="abc79-189">Ano **ConcurrentFileCount** je **2.4**, který jsme můžete příliš zaokrouhlení**2**.</span><span class="sxs-lookup"><span data-stu-id="abc79-189">So, **ConcurrentFileCount** is **2.4**, which we can round off too**2**.</span></span>

### <a name="further-tuning"></a><span data-ttu-id="abc79-190">Další ladění</span><span class="sxs-lookup"><span data-stu-id="abc79-190">Further tuning</span></span>

<span data-ttu-id="abc79-191">Může vyžadovat další ladění, protože je rozsah toowork velikosti souboru s.</span><span class="sxs-lookup"><span data-stu-id="abc79-191">You might require further tuning because there is a range of file sizes toowork with.</span></span> <span data-ttu-id="abc79-192">Hello výše výpočtu funguje dobře v případě, že všechny nebo většinu hello soubory jsou větší a blíže toohello rozsah 10GB.</span><span class="sxs-lookup"><span data-stu-id="abc79-192">hello above calculation works well if all or most of hello files are larger and closer toohello 10GB range.</span></span> <span data-ttu-id="abc79-193">Pokud ale bude existovat mnoho různých velikostí souborů a mnoho jich bude menších, mohli byste hodnotu PerFileThreadCount snížit.</span><span class="sxs-lookup"><span data-stu-id="abc79-193">If instead, there are many different files sizes with many files being smaller, then you could reduce PerFileThreadCount.</span></span> <span data-ttu-id="abc79-194">Snížením hello PerFileThreadCount jsme může zvýšit ConcurrentFileCount.</span><span class="sxs-lookup"><span data-stu-id="abc79-194">By reducing hello PerFileThreadCount, we can increase ConcurrentFileCount.</span></span> <span data-ttu-id="abc79-195">Ano Pokud předpokládáme, že většina našich soubory je menší v rozsahu 5GB hello, můžeme znovu proveďte naše výpočet:</span><span class="sxs-lookup"><span data-stu-id="abc79-195">So, if we assume that most of our files are smaller in hello 5GB range, we can re-do our calculation:</span></span>

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

<span data-ttu-id="abc79-196">Ano **ConcurrentFileCount** bude nyní 96/20, což je 4.8, zaokrouhlen příliš**4**.</span><span class="sxs-lookup"><span data-stu-id="abc79-196">So, **ConcurrentFileCount** will now be 96/20, which is 4.8, rounded off too**4**.</span></span>

<span data-ttu-id="abc79-197">Můžete pokračovat tootune tato nastavení můžete změnit hello **PerFileThreadCount** nahoru a dolů v závislosti na hello distribuce vaší velikosti souborů.</span><span class="sxs-lookup"><span data-stu-id="abc79-197">You can continue tootune these settings by changing hello **PerFileThreadCount** up and down depending on hello distribution of your file sizes.</span></span>

### <a name="limitation"></a><span data-ttu-id="abc79-198">Omezení</span><span class="sxs-lookup"><span data-stu-id="abc79-198">Limitation</span></span>

* <span data-ttu-id="abc79-199">**Počet souborů je menší než ConcurrentFileCount**: Pokud je menší než hello hello počet souborů, které odesíláte **ConcurrentFileCount** , jste vypočítali, pak by měl snížit  **ConcurrentFileCount** toobe rovna toohello počet souborů.</span><span class="sxs-lookup"><span data-stu-id="abc79-199">**Number of files is less than ConcurrentFileCount**: If hello number of files you are uploading is smaller than hello **ConcurrentFileCount** that you calculated, then you should reduce **ConcurrentFileCount** toobe equal toohello number of files.</span></span> <span data-ttu-id="abc79-200">Můžete použít všechny zbývající tooincrease vláken **PerFileThreadCount**.</span><span class="sxs-lookup"><span data-stu-id="abc79-200">You can use any remaining threads tooincrease **PerFileThreadCount**.</span></span>

* <span data-ttu-id="abc79-201">**Příliš mnoho vláken**: Pokud zvýšíte přístup z více vláken počet příliš mnoho bez zvýšit velikost vašeho clusteru, spusťte hello riziko snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="abc79-201">**Too many threads**: If you increase thread count too much without increasing your cluster size, you run hello risk of degraded performance.</span></span> <span data-ttu-id="abc79-202">Může být nutné vyřešit problémy při kontextu přechodu na hello procesoru.</span><span class="sxs-lookup"><span data-stu-id="abc79-202">There can be contention issues when context-switching on hello CPU.</span></span>

* <span data-ttu-id="abc79-203">**Nedostatečná souběžnosti**: Pokud hello souběžnosti nestačí, pak clusteru může být příliš malá.</span><span class="sxs-lookup"><span data-stu-id="abc79-203">**Insufficient concurrency**: If hello concurrency is not sufficient, then your cluster may be too small.</span></span> <span data-ttu-id="abc79-204">Můžete zvýšit hello počet uzlů v clusteru, který vám poskytne další souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="abc79-204">You can increase hello number of nodes in your cluster which will give you more concurrency.</span></span>

* <span data-ttu-id="abc79-205">**Chyby omezování**: Pokud je souběžnost příliš vysoká, může docházet k chybám omezování.</span><span class="sxs-lookup"><span data-stu-id="abc79-205">**Throttling errors**: You may see throttling errors if your concurrency is too high.</span></span> <span data-ttu-id="abc79-206">Pokud vidíte chyby omezení, musí snižte hello souběžnosti nebo kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="abc79-206">If you are seeing throttling errors, you should either reduce hello concurrency or contact us.</span></span>

## <a name="next-steps"></a><span data-ttu-id="abc79-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abc79-207">Next steps</span></span>
* [<span data-ttu-id="abc79-208">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-208">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="abc79-209">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-209">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="abc79-210">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abc79-210">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

