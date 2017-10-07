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
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a>Začínáme s Azure Data Lake Store s použitím rozhraní příkazového řádku Azure CLI 2.0
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Zjistěte, jak toouse Azure CLI 2.0 toocreate Azure Data Lake ukládání účtu a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).

Hello Azure CLI 2.0 je Azure nové prostředí příkazového řádku pro správu prostředků Azure. Je možné používat ho v systémech macOS, Linux a Windows. Další informace najdete v [přehledu rozhraní příkazového řádku Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview). Můžete také prohlédnout hello [referenční informace o Azure Data Lake Store rozhraní příkazového řádku 2.0](https://docs.microsoft.com/cli/azure/dls) úplný seznam příkazů a syntaxe.


## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Rozhraní příkazového řádku Azure CLI 2.0** – Pokyny najdete v článku [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="authentication"></a>Authentication

Tento článek využívá jednodušší přístup ověřování ve službě Data Lake Store, kdy se přihlašujete jako koncový uživatel. Hello přístup úrovně tooData Lake Store účtu a systém souborů je pak řídí hello úroveň přístupu tohoto hello přihlášeného uživatele. Nicméně existují další postupy jako dobře tooauthenticate s Data Lake Store, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**. Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).


## <a name="log-in-tooyour-azure-subscription"></a>Přihlaste se tooyour předplatného Azure

1. Přihlaste se ke svému předplatnému Azure.

    ```azurecli
    az login
    ```

    V dalším kroku hello získáte toouse kódu. Použít https://aka.ms/devicelogin stránky tooopen hello webový prohlížeč a zadejte kód tooauthenticate hello. Jste výzvami toolog v pomocí svých přihlašovacích údajů.

2. Po přihlášení se zobrazí okno hello všechny hello předplatná Azure, které jsou spojeny s vaším účtem. Použijte následující příkaz toouse konkrétní předplatné hello.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu Azure Data Lake Store

1. Vytvořte novou skupinu prostředků. V hello následující příkaz poskytují hello chcete toouse hodnoty parametrů. Pokud název umístění hello obsahuje mezery, dejte ho do uvozovek. Například „East US 2“. 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Vytvoření účtu Data Lake Store hello.
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a>Vytváření složek v účtu Data Lake Store

Můžete vytvořit složky pod vaší toomanage účtu Azure Data Lake Store a ukládat data. Hello použijte následující příkaz toocreate složky s názvem **mynewfolder** v kořenovém adresáři hello hello Data Lake Store.

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> Hello `--folder` parametr zajistí, že příkaz hello vytvoří složku. Pokud není tento parametr zadán, hello příkaz vytvoří prázdný soubor názvem mynewfolder v kořenovém adresáři hello hello účtu Data Lake Store.
> 
>

## <a name="upload-data-tooa-data-lake-store-account"></a>Nahrání dat účtu Data Lake Store tooa

Můžete nahrát data tooData Lake Store přímo na hello kořenové úrovni nebo tooa složky, která jste vytvořili v rámci účtu hello. Hello níže zobrazené fragmenty kódu ukazují, jak tooupload některé složky toohello ukázkových dat (**mynewfolder**) jste vytvořili v předchozí části hello.

Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Stáhněte si soubor hello a uložte ho do místního adresáře v počítači, například C:\sampledata\.

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Pro cíl hello musíte zadat úplnou cestu hello včetně názvu souboru hello.
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a>Zobrazení seznamu souborů v účtu Data Lake Store

Použijte následující příkaz toolist hello soubory v účtu Data Lake Store hello.

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

výstup Hello by měl vypadat podobně jako toohello následující:

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

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a>Přejmenování, stažení a odstranění dat z účtu Data Lake Store 

* **toorename soubor**, použijte následující příkaz hello:
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **toodownload soubor**, použijte následující příkaz hello. Ujistěte se, zda text hello cílová cesta již existuje.
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > Pokud neexistuje, vytvoří příkaz Hello hello cílovou složku.
    > 
    >

* **toodelete soubor**, použijte následující příkaz hello:
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Pokud chcete složku hello toodelete **mynewfolder** a soubor hello **vehicle1_09142014_copy.csv** společně v jednom příkazu, použijte hello--recurse parametr

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a>Práce s oprávněními a seznamy řízení přístupu (ACL) pro účet Data Lake Store

V této části se dozvíte o toomanage seznamy řízení přístupu a oprávnění pomocí Azure CLI 2.0. Podrobný rozbor implementace seznamů řízení přístupu v Azure Data Lake Store najdete v článku [Řízení přístupu v Azure Data Lake Store](data-lake-store-access-control.md).

* **Vlastník hello tooupdate soubor nebo složku**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* **tooupdate hello oprávnění pro soubor nebo složku**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* **tooget hello seznamy ACL pro danou cestu**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    výstup Hello by měl vypadat podobně jako toohello následující:

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

* **tooset položka pro seznam ACL**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* **tooremove položka pro seznam ACL**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* **tooremove celý výchozím nastavení seznamu ACL**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* **tooremove celý seznam ACL jiné než výchozí**, použijte následující příkaz hello:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a>Odstranění účtu Data Lake Store
Použijte následující příkaz toodelete účtu Data Lake Store hello.

```azurecli
az dls account delete --account mydatalakestore
```

Po zobrazení výzvy zadejte **Y** toodelete hello účtu.

## <a name="next-steps"></a>Další kroky

* [Referenční informace k rozhraní příkazového řádku Azure Data Lake Store CLI 2.0](https://docs.microsoft.com/cli/azure/dls)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
