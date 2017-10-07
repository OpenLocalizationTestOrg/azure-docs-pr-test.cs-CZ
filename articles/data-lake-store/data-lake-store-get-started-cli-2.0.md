---
title: "aaaUse příkazového řádku Azure 2.0 rozhraní tooget začít s Azure Data Lake Store | Microsoft Docs"
description: "Pomocí příkazového řádku Azure a platformy 2.0 toocreate účtu Data Lake Store a provádění základních operací"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 374dcd6cdbc13ad19f6c65502329986ecae60ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a><span data-ttu-id="e6a26-103">Začínáme s Azure Data Lake Store s použitím rozhraní příkazového řádku Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e6a26-103">Get started with Azure Data Lake Store using Azure CLI 2.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e6a26-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6a26-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="e6a26-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6a26-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="e6a26-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="e6a26-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="e6a26-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="e6a26-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="e6a26-108">REST API</span><span class="sxs-lookup"><span data-stu-id="e6a26-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="e6a26-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e6a26-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="e6a26-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="e6a26-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="e6a26-111">Python</span><span class="sxs-lookup"><span data-stu-id="e6a26-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="e6a26-112">Zjistěte, jak toouse Azure CLI 2.0 toocreate Azure Data Lake ukládání účtu a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6a26-112">Learn how toouse Azure CLI 2.0 toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="e6a26-113">Hello Azure CLI 2.0 je Azure nové prostředí příkazového řádku pro správu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e6a26-113">hello Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="e6a26-114">Je možné používat ho v systémech macOS, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="e6a26-114">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="e6a26-115">Další informace najdete v [přehledu rozhraní příkazového řádku Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e6a26-115">For more information, see [Overview of Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).</span></span> <span data-ttu-id="e6a26-116">Můžete také prohlédnout hello [referenční informace o Azure Data Lake Store rozhraní příkazového řádku 2.0](https://docs.microsoft.com/cli/azure/dls) úplný seznam příkazů a syntaxe.</span><span class="sxs-lookup"><span data-stu-id="e6a26-116">You can also look at hello [Azure Data Lake Store CLI 2.0 reference](https://docs.microsoft.com/cli/azure/dls) for a complete list of commands and syntax.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="e6a26-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6a26-117">Prerequisites</span></span>
<span data-ttu-id="e6a26-118">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-118">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="e6a26-119">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="e6a26-119">**An Azure subscription**.</span></span> <span data-ttu-id="e6a26-120">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e6a26-120">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="e6a26-121">**Rozhraní příkazového řádku Azure CLI 2.0** – Pokyny najdete v článku [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e6a26-121">**Azure CLI 2.0** - See [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) for instructions.</span></span>

## <a name="authentication"></a><span data-ttu-id="e6a26-122">Authentication</span><span class="sxs-lookup"><span data-stu-id="e6a26-122">Authentication</span></span>

<span data-ttu-id="e6a26-123">Tento článek využívá jednodušší přístup ověřování ve službě Data Lake Store, kdy se přihlašujete jako koncový uživatel.</span><span class="sxs-lookup"><span data-stu-id="e6a26-123">This article uses a simpler authentication approach with Data Lake Store where you log in as an end-user user.</span></span> <span data-ttu-id="e6a26-124">Hello přístup úrovně tooData Lake Store účtu a systém souborů je pak řídí hello úroveň přístupu tohoto hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e6a26-124">hello access level tooData Lake Store account and file system is then governed by hello access level of hello logged in user.</span></span> <span data-ttu-id="e6a26-125">Nicméně existují další postupy jako dobře tooauthenticate s Data Lake Store, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="e6a26-125">However, there are other approaches as well tooauthenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="e6a26-126">Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="e6a26-126">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>


## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="e6a26-127">Přihlaste se tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="e6a26-127">Log in tooyour Azure subscription</span></span>

1. <span data-ttu-id="e6a26-128">Přihlaste se ke svému předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="e6a26-128">Log into your Azure subscription.</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="e6a26-129">V dalším kroku hello získáte toouse kódu.</span><span class="sxs-lookup"><span data-stu-id="e6a26-129">You get a code toouse in hello next step.</span></span> <span data-ttu-id="e6a26-130">Použít https://aka.ms/devicelogin stránky tooopen hello webový prohlížeč a zadejte kód tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-130">Use a web browser tooopen hello page https://aka.ms/devicelogin and enter hello code tooauthenticate.</span></span> <span data-ttu-id="e6a26-131">Jste výzvami toolog v pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e6a26-131">You are prompted toolog in using your credentials.</span></span>

2. <span data-ttu-id="e6a26-132">Po přihlášení se zobrazí okno hello všechny hello předplatná Azure, které jsou spojeny s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="e6a26-132">Once you log in, hello window lists all hello Azure subscriptions that are associated with your account.</span></span> <span data-ttu-id="e6a26-133">Použijte následující příkaz toouse konkrétní předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-133">Use hello following command toouse a specific subscription.</span></span>
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="e6a26-134">Vytvoření účtu Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-134">Create an Azure Data Lake Store account</span></span>

1. <span data-ttu-id="e6a26-135">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e6a26-135">Create a new resource group.</span></span> <span data-ttu-id="e6a26-136">V hello následující příkaz poskytují hello chcete toouse hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="e6a26-136">In hello following command, provide hello parameter values you want toouse.</span></span> <span data-ttu-id="e6a26-137">Pokud název umístění hello obsahuje mezery, dejte ho do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="e6a26-137">If hello location name contains spaces, put it in quotes.</span></span> <span data-ttu-id="e6a26-138">Například „East US 2“.</span><span class="sxs-lookup"><span data-stu-id="e6a26-138">For example "East US 2".</span></span> 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. <span data-ttu-id="e6a26-139">Vytvoření účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-139">Create hello Data Lake Store account.</span></span>
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="e6a26-140">Vytváření složek v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-140">Create folders in a Data Lake Store account</span></span>

<span data-ttu-id="e6a26-141">Můžete vytvořit složky pod vaší toomanage účtu Azure Data Lake Store a ukládat data.</span><span class="sxs-lookup"><span data-stu-id="e6a26-141">You can create folders under your Azure Data Lake Store account toomanage and store data.</span></span> <span data-ttu-id="e6a26-142">Hello použijte následující příkaz toocreate složky s názvem **mynewfolder** v kořenovém adresáři hello hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e6a26-142">Use hello following command toocreate a folder called **mynewfolder** at hello root of hello Data Lake Store.</span></span>

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> <span data-ttu-id="e6a26-143">Hello `--folder` parametr zajistí, že příkaz hello vytvoří složku.</span><span class="sxs-lookup"><span data-stu-id="e6a26-143">hello `--folder` parameter ensures that hello command creates a folder.</span></span> <span data-ttu-id="e6a26-144">Pokud není tento parametr zadán, hello příkaz vytvoří prázdný soubor názvem mynewfolder v kořenovém adresáři hello hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e6a26-144">If this parameter is not present, hello command creates an empty file called mynewfolder at hello root of hello Data Lake Store account.</span></span>
> 
>

## <a name="upload-data-tooa-data-lake-store-account"></a><span data-ttu-id="e6a26-145">Nahrání dat účtu Data Lake Store tooa</span><span class="sxs-lookup"><span data-stu-id="e6a26-145">Upload data tooa Data Lake Store account</span></span>

<span data-ttu-id="e6a26-146">Můžete nahrát data tooData Lake Store přímo na hello kořenové úrovni nebo tooa složky, která jste vytvořili v rámci účtu hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-146">You can upload data tooData Lake Store directly at hello root level or tooa folder that you created within hello account.</span></span> <span data-ttu-id="e6a26-147">Hello níže zobrazené fragmenty kódu ukazují, jak tooupload některé složky toohello ukázkových dat (**mynewfolder**) jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-147">hello snippets below demonstrate how tooupload some sample data toohello folder (**mynewfolder**) you created in hello previous section.</span></span>

<span data-ttu-id="e6a26-148">Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="e6a26-148">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="e6a26-149">Stáhněte si soubor hello a uložte ho do místního adresáře v počítači, například C:\sampledata\.</span><span class="sxs-lookup"><span data-stu-id="e6a26-149">Download hello file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> <span data-ttu-id="e6a26-150">Pro cíl hello musíte zadat úplnou cestu hello včetně názvu souboru hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-150">For hello destination, you must specify hello complete path including hello file name.</span></span>
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a><span data-ttu-id="e6a26-151">Zobrazení seznamu souborů v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-151">List files in a Data Lake Store account</span></span>

<span data-ttu-id="e6a26-152">Použijte následující příkaz toolist hello soubory v účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-152">Use hello following command toolist hello files in a Data Lake Store account.</span></span>

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

<span data-ttu-id="e6a26-153">výstup Hello by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="e6a26-153">hello output of this should be similar toohello following:</span></span>

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a><span data-ttu-id="e6a26-154">Přejmenování, stažení a odstranění dat z účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-154">Rename, download, and delete data from a Data Lake Store account</span></span> 

* <span data-ttu-id="e6a26-155">**toorename soubor**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-155">**toorename a file**, use hello following command:</span></span>
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* <span data-ttu-id="e6a26-156">**toodownload soubor**, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-156">**toodownload a file**, use hello following command.</span></span> <span data-ttu-id="e6a26-157">Ujistěte se, zda text hello cílová cesta již existuje.</span><span class="sxs-lookup"><span data-stu-id="e6a26-157">Make sure hello destination path you specify already exists.</span></span>
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > <span data-ttu-id="e6a26-158">Pokud neexistuje, vytvoří příkaz Hello hello cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="e6a26-158">hello command creates hello destination folder if it does not exist.</span></span>
    > 
    >

* <span data-ttu-id="e6a26-159">**toodelete soubor**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-159">**toodelete a file**, use hello following command:</span></span>
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    <span data-ttu-id="e6a26-160">Pokud chcete složku hello toodelete **mynewfolder** a soubor hello **vehicle1_09142014_copy.csv** společně v jednom příkazu, použijte hello--recurse parametr</span><span class="sxs-lookup"><span data-stu-id="e6a26-160">If you want toodelete hello folder **mynewfolder** and hello file **vehicle1_09142014_copy.csv** together in one command, use hello --recurse parameter</span></span>

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a><span data-ttu-id="e6a26-161">Práce s oprávněními a seznamy řízení přístupu (ACL) pro účet Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-161">Work with permissions and ACLs for a Data Lake Store account</span></span>

<span data-ttu-id="e6a26-162">V této části se dozvíte o toomanage seznamy řízení přístupu a oprávnění pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="e6a26-162">In this section you learn about how toomanage ACLs and permissions using Azure CLI 2.0.</span></span> <span data-ttu-id="e6a26-163">Podrobný rozbor implementace seznamů řízení přístupu v Azure Data Lake Store najdete v článku [Řízení přístupu v Azure Data Lake Store](data-lake-store-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="e6a26-163">For a detailed discussion on how ACLs are implemented in Azure Data Lake Store, see [Access control in Azure Data Lake Store](data-lake-store-access-control.md).</span></span>

* <span data-ttu-id="e6a26-164">**Vlastník hello tooupdate soubor nebo složku**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-164">**tooupdate hello owner of a file/folder**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="e6a26-165">**tooupdate hello oprávnění pro soubor nebo složku**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-165">**tooupdate hello permissions for a file/folder**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* <span data-ttu-id="e6a26-166">**tooget hello seznamy ACL pro danou cestu**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-166">**tooget hello ACLs for a given path**, use hello following command:</span></span>

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    <span data-ttu-id="e6a26-167">výstup Hello by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="e6a26-167">hello output should be similar toohello following:</span></span>

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* <span data-ttu-id="e6a26-168">**tooset položka pro seznam ACL**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-168">**tooset an entry for an ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* <span data-ttu-id="e6a26-169">**tooremove položka pro seznam ACL**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-169">**tooremove an entry for an ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="e6a26-170">**tooremove celý výchozím nastavení seznamu ACL**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-170">**tooremove an entire default ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* <span data-ttu-id="e6a26-171">**tooremove celý seznam ACL jiné než výchozí**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e6a26-171">**tooremove an entire non-default ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="e6a26-172">Odstranění účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-172">Delete a Data Lake Store account</span></span>
<span data-ttu-id="e6a26-173">Použijte následující příkaz toodelete účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="e6a26-173">Use hello following command toodelete a Data Lake Store account.</span></span>

```azurecli
az dls account delete --account mydatalakestore
```

<span data-ttu-id="e6a26-174">Po zobrazení výzvy zadejte **Y** toodelete hello účtu.</span><span class="sxs-lookup"><span data-stu-id="e6a26-174">When prompted, enter **Y** toodelete hello account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6a26-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6a26-175">Next steps</span></span>

* [<span data-ttu-id="e6a26-176">Referenční informace k rozhraní příkazového řádku Azure Data Lake Store CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e6a26-176">Azure Data Lake Store CLI 2.0 reference</span></span>](https://docs.microsoft.com/cli/azure/dls)
* [<span data-ttu-id="e6a26-177">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-177">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="e6a26-178">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-178">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="e6a26-179">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e6a26-179">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
