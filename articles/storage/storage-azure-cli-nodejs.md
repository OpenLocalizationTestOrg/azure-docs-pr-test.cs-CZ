---
title: "aaaUsing hello 1.0 rozhraní příkazového řádku Azure s Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozhraní příkazového řádku Azure (Azure CLI) 1.0, s toocreate Azure Storage a spravovat účty úložiště a pracovat s Azure BLOB a soubory. Hello rozhraní příkazového řádku Azure je nástroj pro různé platformy"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 786f2be64875f5094f09fd6e4a47532889c3a82f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-10-with-azure-storage"></a>Použití hello 1.0 rozhraní příkazového řádku Azure s Azure Storage

## <a name="overview"></a>Přehled

Hello rozhraní příkazového řádku Azure poskytuje sadu softwaru open source, příkazy a platformy pro práci s hello platformě Azure. Poskytuje mnohem hello stejné funkce najít v hello [portál Azure](https://portal.azure.com) také bohaté data přístup k funkcím.

V této příručce, jsme budete prozkoumat jak toouse [rozhraní příkazového řádku Azure (Azure CLI)](../cli-install-nodejs.md) tooperform řadu vývoj a správu úloh s Azure Storage. Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější Azure CLI před použitím tohoto průvodce.

Tato příručka předpokládá, že chápete základní koncepty hello Azure Storage. Příručka Hello obsahuje řadu skriptů toodemonstrate hello využití hello rozhraní příkazového řádku Azure s Azure Storage. Být jisti tooupdate hello skriptu proměnné na základě vaší konfigurace před spuštěním každý skript.

> [!NOTE]
> Hello příručka obsahuje příklady příkazu a skript příkazového řádku Azure CLI hello klasické účty úložiště. V tématu [hello pomocí Azure CLI pro Mac, Linux a Windows se správou prostředků Azure](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) pro příkazy příkazového řádku Azure CLI pro účty úložiště Resource Manager.
>
>

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-hello-azure-cli-in-5-minutes"></a>Začínáme s Azure Storage a hello rozhraní příkazového řádku Azure během 5 minut
Tato příručka používá Ubuntu příklady, ale jiné platformy operačního systému proveďte podobně.

**Nové tooAzure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému. Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).

V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.

**Po vytvoření odběru služby Microsoft Azure a účet:**

1. Stáhněte a nainstalujte hello příkazového řádku Azure CLI hello pokynů uvedených v [hello instalace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
2. Hello rozhraní příkazového řádku Azure je po instalace, bude možné toouse hello azure příkaz z vaší rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) tooaccess hello rozhraní příkazového řádku Azure. Typ hello _azure_ příkaz a jste měli vidět následující výstup hello.

    ![Výstup příkazu Azure][Image1]
3. Hello rozhraní příkazového řádku, zadejte `azure storage` toolist se všechny hello úložiště azure příkazy a získat první dojem o hello funkce hello příkazového řádku Azure CLI. Můžete zadat název příkazu s **-h** parametr (například `azure storage share create -h`) podrobností toosee syntaxe příkazu.
4. Nyní sdělíme vám jednoduchý skript, který ukazuje základní rozhraní příkazového řádku Azure tooaccess příkazy Azure Storage. skript Hello nejprve vás vyzve tooset dvě proměnné pro váš účet úložiště a klíč. Skript hello pak vytvořit nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující kontejner toothat bitové kopie souboru (binární rozsáhlý objekt). Po hello skript obsahuje seznam všech objektů BLOB v kontejneru, stáhne hello bitové kopie souboru toohello cílový adresář, který existuje v místním počítači hello.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating hello container..."
    azure storage container create $container_name

    echo "Uploading hello image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing hello blobs..."
    azure storage blob list $container_name

    echo "Downloading hello image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. V místním počítači otevřete upřednostňované textový editor (systémem vim např.). Zadejte hello výše skript do textového editoru.
6. Nyní musíte tooupdate hello skriptu proměnné na základě svého nastavení konfigurace.

   * **< Storage_account_name >** použít hello zadané ve skriptu hello název nebo zadejte nový název pro účet úložiště. **Důležité:** hello název účtu úložiště hello musí být jedinečný v Azure. Musí být malými písmeny, příliš!
   * **< storage_account_key >** hello přístupový klíč účtu úložiště.
   * **< Container_name >** použít hello zadané ve skriptu hello název nebo zadejte nový název pro váš kontejner.
   * **< Image_to_upload >** zadejte obrázku tooa cestu v místním počítači, jako například: "~ / images/HelloWorld.png".
   * **< Destination_folder >** Enter toostore souborů cesta tooa místního adresáře stáhli ze služby Azure Storage, například: "~/downloadImages".
7. Po aktualizaci hello nezbytné proměnné v vim, stisknutím kombinace kláves `ESC`, `:`, `wq!` toosave hello skriptu.
8. toorun tento skript, jednoduše zadejte hello název souboru skriptu v konzole hello bash. Po spuštění tohoto skriptu, měli byste mít místní cílovou složku, která zahrnuje hello stáhnout soubor bitové kopie. Hello následující snímek obrazovky ukazuje příklad výstupu:

Po spuštění skriptu hello byste měli mít místní cílovou složku, která zahrnuje hello stáhnout soubor bitové kopie.

## <a name="manage-storage-accounts-with-hello-azure-cli"></a>Správa účtů úložiště s hello rozhraní příkazového řádku Azure
### <a name="connect-tooyour-azure-subscription"></a>Připojit tooyour předplatného Azure
Zatímco většina příkazů hello úložiště bude fungovat bez předplatného Azure, doporučujeme předplatné tooyour tooconnect z hello rozhraní příkazového řádku Azure. tooconfigure hello rozhraní příkazového řádku Azure toowork s předplatným, postupujte podle kroků hello v [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Vytvořit nový účet úložiště
toouse úložiště Azure, budete potřebovat účet úložiště. Po nakonfigurování vaše počítače tooconnect tooyour předplatné, můžete vytvořit nový účet úložiště Azure.

```azurecli
azure storage account create <account_name>
```

Hello název svého účtu úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Nastavit výchozí účet úložiště Azure v proměnné prostředí
Můžete mít více účtů úložiště v rámci vašeho předplatného. Můžete zvolit jeden z nich a nastavit ho v hello proměnné prostředí pro všechny úložiště hello příkazů v hello stejné relace. To vám umožní toorun hello rozhraní příkazového řádku Azure úložiště příkazy bez zadání hello úložiště účtu a klíč explicitně.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Jiný způsob tooset výchozí účet úložiště používá připojovací řetězec. Za prvé získáte hello připojovací řetězec pomocí příkazu:

```azurecli
azure storage account connectionstring show <account_name>
```

Zkopírujte připojovací řetězec výstup hello a nastavte ji tooenvironment proměnné:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Vytvářet a spravovat objekty BLOB
Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS. V této části se předpokládá, že jste již obeznámeni s koncepty úložiště objektů Blob v Azure hello. Podrobné informace najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Vytvoření kontejneru
Každý objekt blob v úložišti Azure musí být v kontejneru. Můžete vytvořit kontejner privátní pomocí hello `azure storage container create` příkaz:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**. tooprevent anonymní přístup k tooblobs parametru oprávnění sady hello příliš**vypnout**. Ve výchozím nastavení nový kontejner hello je privátní a je přístupný jenom vlastník účtu hello. čtení tooallow anonymní veřejný přístup k prostředkům tooblob, ale není toocontainer metadata nebo toohello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**Blob**. čtení tooallow úplné veřejný přístup k prostředkům tooblob, metadata kontejneru a hello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**kontejneru**. Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky. Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

tooupload objekty BLOB v kontejneru tooa, můžete použít hello `azure storage blob upload`. Ve výchozím nastavení odešle tento příkaz hello objekt blob bloku tooa místní soubory. Typ hello toospecify pro hello blob, můžete použít hello `--blobtype` parametr.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Stáhnout objekty BLOB z kontejneru
Hello následující příklad ukazuje, jak z kontejneru objektů BLOB toodownload.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Kopírování objektů BLOB
Můžete asynchronně kopírovat objekty blob v rámci účtů a oblastí nebo napříč nimi.

Hello následující příklad ukazuje, jak toocopy objekty BLOB z jednoho úložiště účet tooanother. V této ukázce jsme vytvořit kontejner, kde jsou veřejně, objekty BLOB anonymně přístupné.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Tato ukázka provede asynchronní kopírování. Můžete sledovat stav hello každé operace kopírování spuštěním hello `azure storage blob copy show` operaci.

Všimněte si, že hello zdrojová adresa URL zadaná pro operace kopírování hello musí být veřejně přístupná, nebo zahrnout token SAS (sdílený přístupový podpis).

### <a name="delete-a-blob"></a>Odstranění objektu blob
toodelete objekt blob, použijte hello následující příkaz:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Vytvořit a spravovat sdílené složky
Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB hello. Virtuální počítače Microsoft Azure a cloudové služby i místní aplikace můžou sdílet souborová data přes sdílené složky. Můžete spravovat sdílené složky a data souborů prostřednictvím hello rozhraní příkazového řádku Azure. Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](storage-dotnet-how-to-use-files.md) nebo [jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Vytvoření sdílené složky
Azure File sdílená složka je sdílená složka SMB v Azure. Všechny adresáře a soubory musí být vytvořeny ve sdílené složce. Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů, až toohello limity kapacity účtu úložiště hello. Hello následující příklad vytvoří sdílenou složku s názvem **název_sdílené_položky**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Vytvoření adresáře
Adresář poskytuje volitelné hierarchická struktura pro sdílenou složku Azure. Hello následující příklad vytvoří adresář s názvem **adresář** v hello sdílené složky.

```azurecli
azure storage directory create myshare myDir
```

Všimněte si, že cesta k adresáři může obsahovat několik úrovní *například*, **/ b**. Nicméně je nutné zajistit, že existují všechny nadřazeného adresáře. Například pro cestu **/ b**, je nutné vytvořit adresář **** nejdřív poté vytvořte adresář **b**.

### <a name="upload-a-local-file-toodirectory"></a>Nahrát toodirectory místního souboru
Hello následujícím příkladu se uloží soubor z **~/temp/samplefile.txt** toohello **adresář** adresáře. Cesta k souboru hello upravte tak, aby ukazoval tooa platný soubor na místním počítači:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Všimněte si, že soubor ve sdílené složce hello může být až velikost too1 TB.

### <a name="list-hello-files-in-hello-share-root-or-directory"></a>Zobrazení seznamu souborů hello v kořenové hello sdílenou složku nebo adresář
Můžete vytvořit seznam hello soubory a podadresáře v kořenové sdílenou složku nebo adresář pomocí hello následující příkaz:

```azurecli
azure storage file list myshare myDir
```

Všimněte si, že tento název adresáře hello je volitelné pro hello výpis operaci. Pokud tento parametr vynechán, hello příkaz vypíše obsah hello hello kořenový adresář sdílené složky hello.

### <a name="copy-files"></a>Kopírování souborů
Od verze 0.9.8 rozhraní příkazového řádku Azure, můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob. Dole ukážeme, jak se tooperform tyto zkopírovat operace pomocí rozhraní příkazového řádku. toocopy nový adresář toohello souboru:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

toocopy adresář souboru tooa objektů blob:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Další kroky

Reference k příkazům 1.0 rozhraní příkazového řádku Azure můžete najít pro práci s prostředky úložiště tady:

* [Azure CLI příkazy v režimu Resource Manager](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Azure CLI příkazy v režimu Azure Service Management](../cli-install-nodejs.md)

Může také jako tootry hello [Azure CLI 2.0](storage-azure-cli.md), naše rozhraní příkazového řádku generace napsané v Pythonu, pro použití s modelem nasazení Resource Manager hello.

[Image1]: ./media/storage-azure-cli/azure_command.png
