---
title: "aaaUsing hello 2.0 rozhraní příkazového řádku Azure s Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozhraní příkazového řádku Azure (Azure CLI) 2.0, s toocreate Azure Storage a spravovat účty úložiště a pracovat s Azure BLOB a soubory. Hello Azure CLI 2.0 je nástroj napříč platformami napsané v Pythonu."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: marsma
ms.openlocfilehash: 38402373dcd87f1ef05471a02353c77d58f1a9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-20-with-azure-storage"></a>Použití hello 2.0 rozhraní příkazového řádku Azure s Azure Storage

Hello open source, a platformy Azure CLI 2.0 obsahuje sadu příkazů pro práci s hello platformy Azure. Poskytuje mnohem hello stejné funkce najít v hello [portál Azure](https://portal.azure.com), včetně přístupu bohaté data.

V této příručce, jsme ukazují, jak toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform několik úloh práci s prostředky v účtu úložiště Azure. Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější verzi 2.0 rozhraní příkazového řádku hello před použitím tohoto průvodce.

Příklady Hello v příručce hello předpokládají použití hello hello prostředí Bash na Ubuntu, ale jiné platformy proveďte podobně. 

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Požadavky
Tato příručka předpokládá, že chápete základní koncepty hello Azure Storage. Předpokládá také, že jste možnost toosatisfy hello účet vytvoření požadavky, které jsou uvedeny níže Azure a hello služby úložiště.

### <a name="accounts"></a>Účty
* **Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořit bezplatný účet Azure](https://azure.microsoft.com/free/).
* **Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md).

### <a name="install-hello-azure-cli-20"></a>Nainstalujte hello 2.0 rozhraní příkazového řádku Azure

Stáhněte a nainstalujte hello 2.0 rozhraní příkazového řádku Azure podle pokynů hello uvedených v [nainstalovat Azure CLI 2.0](/cli/azure/install-az-cli2).

> [!TIP]
> Pokud máte potíže s instalací hello, podívejte se na hello [řešení potíží s instalací](/cli/azure/install-az-cli2#installation-troubleshooting) části článku hello a hello [nainstalovat řešení potíží s](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) Průvodce na Githubu.
>

## <a name="working-with-hello-cli"></a>Práce s hello rozhraní příkazového řádku

Po instalaci hello rozhraní příkazového řádku, můžete hello `az` příkazu v příkazech rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) hello tooaccess rozhraní příkazového řádku Azure. Typ hello `az` příkaz toosee úplný seznam hello základní příkazy (hello následující příklad výstupu byla zkrácena):

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome toohello cool new Azure CLI!

Here are hello base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

Ve svém rozhraní příkazového řádku spusťte příkaz hello `az storage --help` toolist hello `storage` příkaz podskupiny. popisy Hello hello podskupiny poskytovat přehled o hello hello funkce, rozhraní příkazového řádku Azure poskytuje pro práci s vaše prostředky úložiště.

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use hello standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues tooeffectively scale applications according tootraffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-hello-cli-tooyour-azure-subscription"></a>Připojení rozhraní příkazového řádku tooyour hello předplatného Azure

toowork s hello prostředky ve vašem předplatném Azure, musíte nejprve se přihlásit tooyour účet Azure s `az login`. Existuje několik způsobů, které se můžete přihlásit:

* **Interaktivní přihlášení**:`az login`
* **Přihlaste se pomocí uživatelského jména a hesla**:`az login -u johndoe@contoso.com -p VerySecret`
  * To nebude fungovat s účty Microsoft nebo účty, které používají službu Multi-Factor authentication.
* **Přihlaste se pomocí objektu služby**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-20-sample-script"></a>Azure CLI 2.0 ukázkový skript

V dalším kroku jsme budete pracovat se skript malé prostředí, který vystavuje pár základních toointeract příkazy Azure CLI 2.0 s prostředky Azure Storage. skript Hello nejprve vytvoří nový kontejner ve vašem účtu úložiště, a poté odešle existující kontejner toothat souboru (jako objekt blob). Potom zobrazí seznam všech objektů BLOB v kontejneru hello a nakonec stáhne cíl tooa hello souboru v místním počítači, který zadáte.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating hello container..."
az storage container create --name $container_name

echo "Uploading hello file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing hello blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading hello file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

**Nakonfigurujte a spusťte skript hello**

1. Otevřete svém oblíbeném textovém editoru, pak zkopírujte a vložte hello předcházející skriptu do editoru hello.

2. Potom aktualizujte hello skriptu proměnné tooreflect nastavení konfigurace. Nahraďte následující hodnoty jako zadaný hello:

   * **\<storage_account_name\>**  hello název účtu úložiště.
   * **\<storage_account_key\>**  hello primární nebo sekundární přístupový klíč účtu úložiště.
   * **\<container_name\>**  název hello nový kontejner toocreate, jako je například "azure-rozhraní příkazového řádku – ukázka container".
   * **\<blob_name\>**  název hello cílový objekt blob v kontejneru hello.
   * **\<file_to_upload\>**  hello toosmall cestě k souboru v místním počítači, jako například "~ / images/HelloWorld.png".
   * **\<destination_file\>**  hello cílová cesta souboru, jako například "~ / downloadedImage.png".

3. Po aktualizaci hello nezbytné proměnné, uložte skript hello a ukončete editor. Další kroky Hello předpokládá, že jste s názvem vašeho skriptu **my_storage_sample.sh**.

4. Označte hello skriptu jako spustitelný soubor, v případě potřeby:`chmod +x my_storage_sample.sh`

5. Spusťte skript hello. Například v Bash:`./my_storage_sample.sh`

Měli vidět výstup podobný toohello následující a hello  **\<destination_file\>**  jste zadali v hello skriptu by se měla objevit v místním počítači.

```
Creating hello container...
{
  "created": true
}
Uploading hello file...
Percent complete: %100.0
Listing hello blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading hello file...
Name
---------
README.md
Done
```

> [!TIP]
> Hello předchozí výstup je v **tabulky** formátu. Můžete určit, který výstupní formát toouse zadáním hello `--output` argument v příkazech rozhraní příkazového řádku, nebo nastavit globálně pomocí `az configure`.
>

## <a name="manage-storage-accounts"></a>Správa účtů úložiště

### <a name="create-a-new-storage-account"></a>Vytvořit nový účet úložiště
toouse Azure Storage, potřebujete účet úložiště. Můžete vytvořit nový účet úložiště Azure, po dokončení konfigurace počítače příliš[připojení odběru tooyour](#connect-to-your-azure-subscription).

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* `--location`[Vyžaduje]: umístění. Například "západní USA".
* `--name`[Vyžaduje]: název účtu úložiště hello. Hello název musí mít délku 3 too24 znaků a používat jenom malé alfanumerické znaky.
* `--resource-group`[Vyžaduje]: název skupiny prostředků.
* `--sku`[Vyžaduje]: hello účet úložiště SKU. Povolené hodnoty:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a>Nastavení proměnných prostředí výchozí účet úložiště Azure
Můžete mít více účtů úložiště ve vašem předplatném Azure. tooselect jeden z nich příkazy toouse pro všechny následné úložiště, můžete nastavit tyto proměnné prostředí:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Jiný způsob tooset výchozí účet úložiště je pomocí připojovacího řetězce. Nejdřív získat hello připojovací řetězec s hello `show-connection-string` příkaz:

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

Potom kopie hello výstup připojovací řetězec a nastavte hello `AZURE_STORAGE_CONNECTION_STRING` proměnnou prostředí (může být nutné tooenclose hello připojovací řetězec v uvozovkách):

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> Všechny příklady v následující části tohoto článku hello předpokládají, že jste nastavili hello `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY` proměnné prostředí.
>

## <a name="create-and-manage-blobs"></a>Vytvářet a spravovat objekty BLOB
Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS. V této části se předpokládá, že jste již obeznámeni s koncepty úložiště objektů Blob v Azure. Podrobné informace najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Vytvoření kontejneru
Každý objekt blob v úložišti Azure musí být v kontejneru. Kontejner můžete vytvořit pomocí hello `az storage container create` příkaz:

```azurecli
az storage container create --name <container_name>
```

Můžete nastavit jednu ze tří úrovní přístupu pro čtení pro nový kontejner zadáním hello volitelné `--public-access` argument:

* `off`(výchozí): datový kontejner je privátní toohello vlastníka účtu.
* `blob`: Pro objekty BLOB veřejný přístup pro čtení.
* `container`: Veřejné pro čtení a seznamu přístup toohello celého kontejneru.

Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-tooa-container"></a>Nahrát tooa kontejneru objektů blob
Podporuje Azure úložiště objektů Blob bloku, připojit a objekty BLOB stránky. Nahrávání objekty BLOB kontejneru tooa pomocí hello `blob upload` příkaz:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 Ve výchozím nastavení, hello `blob upload` příkaz odešle objekty BLOB toopage *.vhd soubory nebo objekty BLOB bloku, jinak hodnota. toospecify, jiné při zadejte nahrát objekt blob, můžete použít hello `--type` argument – povolené hodnoty jsou `append`, `block`, a `page`.

 Další informace o typech hello jiný objektu blob, najdete v části [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Stažení objektu blob z kontejneru
Tento příklad ukazuje, jak toodownload objekt blob z kontejneru:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru

Seznam hello objektů BLOB v kontejneru s hello [az úložiště objektů blob seznamu](/cli/azure/storage/blob#list) příkaz.

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a>Kopírování objektů BLOB
Můžete asynchronně kopírovat objekty blob v rámci účtů a oblastí nebo napříč nimi.

Hello následující příklad ukazuje, jak toocopy objekty BLOB z jednoho úložiště účet tooanother. Nejdříve vytvoříme kontejner v účtu úložiště zdroj hello, zadání veřejný přístup pro čtení pro jeho objekty BLOB. V dalším kroku jsme nahrát kontejner toohello souborů a nakonec kopie hello objektů blob z tohoto kontejneru do kontejneru v hello cílový účet úložiště.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob toocontainer in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account toodestination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

V hello výše příklad hello cílový kontejner již musí existovat v hello cílový účet úložiště pro toosucceed operace kopírování hello. Kromě toho hello zdrojový objekt blob zadaný v hello `--source-uri` argument musí buď zahrnují token sdílený přístupový podpis (SAS), nebo veřejně přístupný, jako v následujícím příkladu.

### <a name="delete-a-blob"></a>Odstranění objektu blob
toodelete objekt blob, použijte hello `blob delete` příkaz:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Vytvořit a spravovat sdílené složky
Azure File storage nabízí sdílené úložiště pro aplikace, které používají hello zpráva bloku protokol Server (SMB). Virtuální počítače Microsoft Azure a cloudové služby i místní aplikace můžou sdílet souborová data přes sdílené složky. Můžete spravovat sdílené složky a data souborů prostřednictvím hello rozhraní příkazového řádku Azure. Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](storage-dotnet-how-to-use-files.md) nebo [jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Vytvoření sdílené složky
Azure File sdílená složka je sdílená složka SMB v Azure. Všechny adresáře a soubory musí být vytvořeny ve sdílené složce. Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů, až toohello limity kapacity účtu úložiště hello. Hello následující příklad vytvoří sdílenou složku s názvem **název_sdílené_položky**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Vytvoření adresáře
Adresář poskytuje hierarchické uspořádání sdílenou složku Azure. Hello následující příklad vytvoří adresář s názvem **adresář** v hello sdílené složky.

```azurecli
az storage directory create --name myDir --share-name myshare
```

Cestu k adresáři může obsahovat více úrovních, například **dir1/dir2**. Nicméně je nutné zajistit, že před vytvořením podadresáři existují všechny nadřazeného adresáře. Například pro cestu **dir1/dir2**, je nutné nejprve vytvořit adresář **dir1**, pak vytvořit adresář **dir2**.

### <a name="upload-a-local-file-tooa-share"></a>Nahrát tooa místní sdílené složky
Hello následujícím příkladu se uloží soubor z **~/temp/samplefile.txt** tooroot hello **název_sdílené_položky** sdílené složky. Hello `--source` argument určuje hello existující tooupload místního souboru.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Stejně jako u vytváření adresáře, můžete určit cestu k adresáři v rámci hello sdílenou složku tooupload hello tooan existující adresář souboru ve složce hello:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

Soubor ve sdílené složce hello může být až velikost too1 TB.

### <a name="list-hello-files-in-a-share"></a>Seznam hello soubory ve sdílené složce
Můžete vytvořit seznam souborů a adresářů ve sdílené složce pomocí hello `az storage file list` příkaz:

```azurecli
# List hello files in hello root of a share
az storage file list --share-name myshare --output table

# List hello files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List hello files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a>Kopírování souborů      
Můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob. Například toocopy tooa adresář souboru v jiné sdílené složce:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a>Další kroky
Tady jsou některé další zdroje informací o další informace o práci s hello 2.0 rozhraní příkazového řádku Azure.

* [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Přehled příkazů Azure CLI 2.0](/cli/azure)
* [Azure CLI 2.0 na Githubu](https://github.com/Azure/azure-cli)
