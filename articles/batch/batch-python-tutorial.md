---
title: "aaaTutorial - použití hello Azure Batch SDK pro jazyk Python | Microsoft Docs"
description: "Informace hello základními koncepty Azure Batch a vytvoření jednoduché řešení používá Python."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: 42cae157-d43d-47f8-88f5-486ccfd334f4
ms.service: batch
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4d5152aeef31848c50a7f2aae5e7a7e0e1e9535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-sdk-for-python"></a>Začínáme s hello Batch SDK pro jazyk Python

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Další hello Základy [Azure Batch] [ azure_batch] a hello [Batch Python] [ py_azure_sdk] klienta probereme malou aplikaci Batch napsanou v Pythonu. Podíváme na to jak dva ukázkové skripty použití hello Batch služby tooprocess paralelní úlohy na virtuálních počítačích s Linuxem v cloudu hello a způsob, jakým interagují s [Azure Storage](../storage/common/storage-introduction.md) pro přípravě a načítání souborů. Budete další běžné pracovním postupem aplikací Batch a získáte základní přehled o hello hlavním součástem služby Batch například úlohách, úkolech, fondech a výpočetních uzlů.

![Pracovní postup řešení Batch (Basic)][11]<br/>

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte praktické znalosti Pythonu a umíte do jisté míry pracovat s Linuxem. Předpokládá také, že jste možnost toosatisfy hello účet vytvoření požadavky, které jsou uvedeny níže Azure a hello Batch a služby úložiště.

### <a name="accounts"></a>Účty
* **Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet Azure][azure_free_account].
* **Účet Batch**: Po pořízení předplatného Azure si [vytvořte účet Azure Batch](batch-account-create-portal.md).
* **Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/common/storage-create-storage-account.md).

### <a name="code-sample"></a>Ukázka kódu
kurz Python Hello [ukázka kódu] [ github_article_samples] je jedním z mnoha ukázek kódu služby Batch najít v hello hello [azure-batch-samples] [ github_samples] úložiště v GitHub. Všechny ukázky hello si můžete stáhnout kliknutím **klonovat nebo stáhnout > stáhnout ZIP** na domovskou stránku hello úložiště nebo kliknutím hello [azure-batch-samples-master.zip] [ github_samples_zip]přímý odkaz ke stažení. Po extrahování obsahu souboru ZIP hello hello hello dva skripty v tomto kurzu se nacházejí v hello `article_samples` directory:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Prostředí Python
toorun hello *python_tutorial_client.py* ukázkový skript na místní pracovní stanici, musíte **překladač Pythonu** kompatibilní s verzí **2.7** nebo **3.3 +**. Hello skript byl otestován v Linuxu i Windows.

### <a name="cryptography-dependencies"></a>závislosti kryptografie
Je nutné nainstalovat hello závislosti pro hello [kryptografie] [ crypto] knihovny, vyžadují hello `azure-batch` a `azure-storage` balíčků Python. Proveďte jeden z hello následující operace, které jsou vhodné pro vaši platformu nebo prostudujte toohello [kryptografie instalace] [ crypto_install] podrobnosti o další informace:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* CentOS

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* Windows

    `pip install cryptography`

> [!NOTE]
> Pokud instalace pro jazyk Python 3.3 + v systému Linux, použijte pro závislosti Python hello hello python3 ekvivalenty. Například na Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`
>
>

### <a name="azure-packages"></a>Balíčky Azure
Dále nainstalujte hello **Azure Batch** a **Azure Storage** balíčků Python. Oba balíčky můžete nainstalovat pomocí **pip** a hello *requirements.txt* nachází tady:

`/azure-batch-samples/Python/Batch/requirements.txt`

Zadejte následující **pip** příkaz tooinstall balíčků Batch a Storage hello:

`pip install -r requirements.txt`

Nebo můžete nainstalovat hello [azure-batch] [ pypi_batch] a [azure-storage] [ pypi_storage] balíčky Pythonu ručně:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> Pokud používáte Neprivilegovaný účet, může být nutné tooprefix příkazům `sudo`. Například, `sudo pip install -r requirements.txt`. Další informace o instalaci balíčků Pythonu najdete v článku [Instalace balíčků][pypi_install] na webu python.org.
>
>

## <a name="batch-python-tutorial-code-sample"></a>Ukázka kódu Pythonu pro službu Batch
Ukázka kódu Pythonu Batch Hello se skládá ze dvou skriptů Pythonu a několika datových souborů.

* **python_tutorial_client.PY**: komunikuje s tooexecute služby Batch a Storage hello paralelní úlohy na výpočetních uzlech (virtuálních počítačů). Hello *python_tutorial_client.py* skript se spustí na místní pracovní stanici.
* **python_tutorial_task.PY**: hello skript, který běží na výpočetních uzlech v Azure tooperform hello samotnou práci. V ukázce hello *python_tutorial_task.py* hello analyzuje text v souboru staženém ze služby Azure Storage (vstupní soubor hello). Pak se vytvoří textový soubor (hello výstupního souboru), obsahuje seznam hello první tři slova, která se zobrazují ve vstupním souboru hello. Po vytvoření výstupního souboru hello, *python_tutorial_task.py* nahrávání hello tooAzure soubor úložiště. Díky tomu je k dispozici ke stažení toohello klientského skriptu běží na pracovní stanici. Hello *python_tutorial_task.py* skript se spustí paralelně v několika výpočetních uzlech v hello služby Batch.
* **./data/taskdata\*.txt**: tyto tři textové soubory zajišťují vstup hello pro hello úlohy, které běží na hello výpočetních uzlů.

Hello následující diagram znázorňuje primární operace hello, které provádí hello klientský skript a skript úkolu. Tento základní pracovní postup je typický pro mnoho výpočetních řešení, která jsou vytvořená pomocí služby Batch. Když nepředvádí všechny funkce, které jsou k dispozici v hello služby Batch, téměř každý scénář Batch zahrnuje části tohoto pracovního postupu.

![Ukázkový pracovní postup služby Batch][8]<br/>

[**Krok 1.**](#step-1-create-storage-containers) Ve službě Azure Blob Storage vytvořte **kontejnery** .<br/>
[**Krok 2.**](#step-2-upload-task-script-and-data-files) Nahrajte toocontainers úloh skript a vstupní soubory.<br/>
[**Krok 3.**](#step-3-create-batch-pool) Vytvořte **fond** Batch.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Hello fondu **StartTask** stahování hello úlohy skriptu (python_tutorial_task.py) toonodes připojí hello fondu.<br/>
[**Krok 4.**](#step-4-create-batch-job) Vytvořte **úlohu** Batch.<br/>
[**Krok 5.**](#step-5-add-tasks-to-job) Přidat **úlohy** toohello úlohy.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Hello úkoly jsou naplánované tooexecute na uzlech.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Každý úkol stáhne svoje vstupní data ze služby Azure Storage a potom zahájí spuštění.<br/>
[**Krok 6.**](#step-6-monitor-tasks) Sledujte úkoly.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Jako úkoly při dokončení odesílají svoje výstupní data tooAzure úložiště.<br/>
[**Krok 7.**](#step-7-download-task-output) Stáhněte si výstup úkolu ze služby Storage.

Jak jsme už zmínili, ne každé řešení Batch provede právě tyto kroky a může jich dokonce obsahovat mnohem víc, ale tato ukázka představuje procesy, které běžně bývají v řešení Batch.

## <a name="prepare-client-script"></a>Příprava klientského skriptu
Než spustíte ukázkový text hello, přidat přihlašovací údaje účtu Batch a Storage příliš*python_tutorial_client.py*. Pokud jste tak ještě neučinili, otevřete soubor hello v vaše oblíbené hello editor a aktualizace následující řádky pomocí svých přihlašovacích údajů.

```python
# Update hello Batch and Storage account credential strings below with hello values
# unique tooyour accounts. These are used when constructing connection strings
# for hello Batch and Storage client objects.

# Batch account credentials
BATCH_ACCOUNT_NAME = ""
BATCH_ACCOUNT_KEY = ""
BATCH_ACCOUNT_URL = ""

# Storage account credentials
STORAGE_ACCOUNT_NAME = ""
STORAGE_ACCOUNT_KEY = ""
```

Přihlašovací údaje účtu Batch a Storage v rámci hello okně účtu každé služby můžete najít v hello [portál Azure][azure_portal]:

![Přihlašovací údaje hello portálu služby batch][9]
![přihlašovací údaje Storage na portálu hello][10]<br/>

V následujících částech hello, budeme analyzovat kroky hello používá hello skriptů tooprocess úloh ve hello služby Batch. Doporučujeme vám pravidelně toorefer toohello skriptů v editoru při práci procházení hello zbývající části článku hello.

Přejděte toohello následující řádek ve **python_tutorial_client.py** toostart s krok 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Krok 1: Vytvoření kontejnerů služby Storage
![Vytvoření kontejnerů ve službě Azure Storage][1]
<br/>

Batch obsahuje vestavěnou podporu pro komunikaci se službou Azure Storage. Kontejnery v účtu úložiště bude poskytovat hello soubory potřebné hello úlohy, které běží v účtu Batch. Hello kontejnery také poskytují místě toostore hello výstupní data, která hello úkoly vytvářejí. Hello nejprve thing hello *python_tutorial_client.py* skript se vytvoří tři kontejnery ve [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):

* **aplikace**: Tento kontejner bude ukládat skript v jazyce Python hello spouštěné úkoly hello *python_tutorial_task.py*.
* **vstupní**: budou úkoly stahovat z hello hello datové soubory tooprocess *vstupní* kontejneru.
* **výstup**: když úkoly dokončí zpracování vstupního souboru, odešlou výsledky toohello hello *výstup* kontejneru.

V pořadí toointeract s úložiště účtu a vytvoření kontejnerů používáme hello [azure-storage] [ pypi_storage] balíček toocreate [BlockBlobService] [ py_blockblobservice] objekt – hello "klienta objektů blob." Poté vytvoříme tři kontejnery v účtu úložiště hello pomocí klienta objektů blob hello.

```python
import azure.storage.blob as azureblob

# Create hello blob client, for use in obtaining references to
# blob storage containers and uploading files toocontainers.
blob_client = azureblob.BlockBlobService(
    account_name=STORAGE_ACCOUNT_NAME,
    account_key=STORAGE_ACCOUNT_KEY)

# Use hello blob client toocreate hello containers in Azure Storage if they
# don't yet exist.
APP_CONTAINER_NAME = 'application'
INPUT_CONTAINER_NAME = 'input'
OUTPUT_CONTAINER_NAME = 'output'
blob_client.create_container(APP_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(INPUT_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(OUTPUT_CONTAINER_NAME, fail_on_exist=False)
```

Po vytvoření kontejnerů hello hello aplikaci teď můžete nahrát hello soubory, které se použijí tak, že úlohy hello.

> [!TIP]
> [Jak toouse Azure Blob storage z Pythonu](../storage/blobs/storage-python-how-to-use-blob-storage.md) nabízí pěkný přehled o práci s Azure Storage kontejnery a objekty BLOB ve službě. Musí být v hello horní části seznamu čtení jako začátek práce s Batch.
>
>

## <a name="step-2-upload-task-script-and-data-files"></a>Krok 2: Odeslání skriptu úkolu a datových souborů
![Soubory toocontainers odeslání úloh aplikace a vstupních (data)][2]
<br/>

V souboru hello operace nahrávání *python_tutorial_client.py* nejdřív definuje kolekce **aplikace** a **vstupní** cesty k souborům, které jsou v místním počítači hello. Pak odešle tyto soubory toohello kontejnery, které jste vytvořili v předchozím kroku hello.

```python
# Paths toohello task script. This script will be executed by hello tasks that
# run on hello compute nodes.
application_file_paths = [os.path.realpath('python_tutorial_task.py')]

# hello collection of data files that are toobe processed by hello tasks.
input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                    os.path.realpath('./data/taskdata2.txt'),
                    os.path.realpath('./data/taskdata3.txt')]

# Upload hello application script tooAzure Storage. This is hello script that
# will process hello data files, and is executed by each of hello tasks on the
# compute nodes.
application_files = [
    upload_file_to_container(blob_client, APP_CONTAINER_NAME, file_path)
    for file_path in application_file_paths]

# Upload hello data files. This is hello data that will be processed by each of
# hello tasks executed on hello compute nodes in hello pool.
input_files = [
    upload_file_to_container(blob_client, INPUT_CONTAINER_NAME, file_path)
    for file_path in input_file_paths]
```

Pomocí seznamu, hello `upload_file_to_container` funkce je volána pro každý soubor v kolekcích hello a dva [ResourceFile] [ py_resource_file] se vyplní kolekce. Hello `upload_file_to_container` funkce se zobrazí níže:

```python
def upload_file_to_container(block_blob_client, container_name, path):
    """
    Uploads a local file tooan Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: hello name of hello Azure Blob storage container.
    :param str file_path: hello local path toohello file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """

    import datetime
    import azure.storage.blob as azureblob
    import azure.batch.models as batchmodels

    blob_name = os.path.basename(path)

    print('Uploading file {} toocontainer [{}]...'.format(path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles
A [ResourceFile] [ py_resource_file] poskytuje úlohy v dávce s soubor tooa hello adresy URL ve službě Azure Storage, který je stažený tooa výpočetního uzlu před spuštěním této úlohy. Hello [ResourceFile][py_resource_file]. **blob_source** vlastnost určuje úplnou adresu URL hello hello souboru, protože existuje ve službě Azure Storage. Adresa URL Hello mohou obsahovat také sdílený přístupový podpis (SAS), který poskytuje soubor toohello zabezpečený přístup. Většina typů úkolů ve službě Batch obsahuje vlastnost *ResourceFiles* včetně:

* [CloudTask][py_task]
* [StartTask][py_starttask]
* [JobPreparationTask][py_jobpreptask]
* [JobReleaseTask][py_jobreltask]

Tato ukázka nepoužívá hello JobPreparationTask nebo JobReleaseTask typy úloh, ale si můžete přečíst více o nich [spustit přípravy a dokončení úlohy na Azure Batch výpočetních uzlech](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Sdílený přístupový podpis (SAS)
Sdílené přístupové podpisy jsou řetězce, které zajišťují zabezpečený přístup toocontainers a objekty BLOB ve službě Azure Storage. Hello *python_tutorial_client.py* skript používá objektu blob i a sdílené přístupové podpisy kontejneru a ukazuje, jak tyto sdílené tooobtain přistupovat k řetězce podpisu ze hello služby úložiště.

* **Sdílené přístupové podpisy objektů blob**: StartTask fondu hello používá sdílené přístupové podpisy objektů blob při stahování hello skriptu úkolu a vstupní data souborů ze služby Storage (viz [krok 3](#step-3-create-batch-pool) níže). Hello `upload_file_to_container` fungovat v *python_tutorial_client.py* obsahuje hello kód, který získá sdílený přístupový podpis jednotlivých objektů blob. Dělá to tak, že zavoláte [BlockBlobService.make_blob_url] [ py_make_blob_url] v modulu hello úložiště.
* **Sdílený přístupový podpis kontejneru**: když každý úkol dokončí svojí práci ve výpočetním uzlu hello, odešle svůj výstupní soubor toohello *výstup* kontejneru ve službě Azure Storage. toodo, *python_tutorial_task.py* používá sdílený přístupový podpis kontejneru, který poskytuje přístup pro zápis toohello kontejneru. Hello `get_container_sas_token` fungovat v *python_tutorial_client.py* získá sdílený přístupový podpis kontejneru hello, který se potom předá jako toohello úlohy argument příkazového řádku. Krok #5 [přidat úlohy tooa úlohy](#step-5-add-tasks-to-job), popisuje použití hello hello kontejneru SAS.

> [!TIP]
> Podívejte se na dvě části řady hello na sdílených přístupových podpisů [část 1: vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a [část 2: vytvoření a použití SAS s hello služby objektů Blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn informace o zajišťování bezpečného přístupu toodata ve vašem účtu úložiště.
>
>

## <a name="step-3-create-batch-pool"></a>Krok 3: Vytvoření fondu služby Batch
![Vytvoření fondu Batch][3]
<br/>

**Fond** Batch je kolekce výpočetních uzlů (virtuálních počítačů), na kterých služba Batch provádí úkoly z úlohy.

Po odeslání hello úlohy skriptu a data souborů toohello účet úložiště, *python_tutorial_client.py* spustí jeho komunikaci se službou Batch hello pomocí modulu Batch Python hello. toodo, [BatchServiceClient] [ py_batchserviceclient] je vytvořena:

```python
# Create a Batch service client. We'll now be interacting with hello Batch
# service in addition tooStorage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

Dále fond výpočetních uzlů je vytvořen v hello účtu Batch pomocí volání příliš`create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with hello specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for hello new pool.
    :param list resource_files: A collection of resource files for hello pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify hello commands for hello pool's start task. hello start task is run
    # on each node as it joins hello pool, and when it's rebooted or re-imaged.
    # We use hello start task tooprep hello node for running our task script.
    task_commands = [
        # Copy hello python_tutorial_task.py script toohello "shared" directory
        # that all tasks that run on hello node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and hello dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install hello azure-storage module so that hello task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get hello node agent SKU and image reference for hello virtual machine
    # configuration.
    # For more information about hello virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Při vytváření fondu můžete definovat [PoolAddParameter] [ py_pooladdparam] který určuje několik vlastností fondu hello:

* **ID** hello fondu (*id* – povinné)<p/>Stejně jako u většiny entit ve službě Batch musí mít nový fond v rámci účtu Batch jedinečné ID. Váš kód odkazoval toothis fond pomocí jeho ID, a je identifikaci fondu hello v hello Azure [portál][azure_portal].
* **Počet výpočetních uzlů** (*target_dedicated* – povinné)<p/>Tato vlastnost určuje, kolik virtuálních počítačů musí být nasazené ve fondu hello. Je důležité toonote, aby všechny účty Batch mají výchozí **kvóty** , limity hello počet **jader** (a tedy výpočetních uzlů) v účtu Batch. Můžete najít hello výchozí kvóty a pokyny o tom, příliš[navýšení kvóty](batch-quota-limit.md#increase-a-quota) (například hello maximální počet jader na účtu Batch) v [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md). Možná vás někdy napadne otázka, proč váš fond nedosahuje víc než X uzlů. hello příčinou může být tato kvóta na jádra.
* **Operační systém** uzlů (*virtual_machine_configuration***nebo***cloud_service_configuration* – povinné)<p/>Ve skriptu *python_tutorial_client.py* vytvoříme fond linuxových uzlů pomocí [VirtualMachineConfiguration][py_vm_config]. Hello `select_latest_verified_vm_image_with_node_agent_sku` fungovat v `common.helpers` usnadňuje práci s [Marketplace virtuálních počítačů Azure] [ vm_marketplace] bitové kopie. Další informace o používání imagí z Marketplace najdete v tématu [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md).
* **Velikost výpočetních uzlů** (*vm_size* – povinné)<p/>Vzhledem k tomu, že zadáváme linuxové uzly pro naší [VirtualMachineConfiguration][py_vm_config], zadáme velikost virtuálního počítače (v této ukázce `STANDARD_A1`) podle článku [Velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Další informace opět najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) 
* **Spustit úkol** (*start_task* – nepovinné)<p/>Společně s hello výše fyzickými vlastnostmi uzlu můžete určit také [StartTask] [ py_starttask] hello fondu (není nutné). Hello StartTask se spustí na každém uzlu jako takový uzel připojí hello fondu a pokaždé, když je uzel restartován. Hello StartTask je zvláště užitečná pro přípravu výpočetních uzlů k provádění hello úkolů, například při instalaci aplikace hello, které běží vaše úkoly.<p/>V této ukázkové aplikaci hello StartTask zkopíruje hello soubory, které stáhne ze služby Storage (které jsou určené pomocí hello StartTask **resource_files** vlastnost) z hello StartTask *pracovní adresář* toohello *sdílené* adresář, kterému mají přístup všechny úkoly spuštěné na uzlu hello. V podstatě zkopíruje `python_tutorial_task.py` toohello sdílet adresáře v každém uzlu hello uzel připojí hello fondu, takže všechny úlohy, které běží na uzlu hello k němu přístup.

Můžete si povšimnout volání toohello hello `wrap_commands_in_shell` pomocné funkce. Tato funkce vezme kolekci samostatných příkazů a vytvoří jeden příkazový řádek, který odpovídá vlastnosti příkazového řádku úkolu.

Také je zajímavé ve fragmentu kódu hello výše hello použití dvou proměnných prostředí v hello **command_line** vlastnost hello StartTask: `AZ_BATCH_TASK_WORKING_DIR` a `AZ_BATCH_NODE_SHARED_DIR`. Každý výpočetní uzel v rámci fondu Batch je automaticky nakonfigurovaný pomocí řady proměnných prostředí, které jsou specifické tooBatch. Jakýkoli proces spuštěný úkolem má přístup k proměnným prostředí toothese.

> [!TIP]
> toofind Další informace o hello proměnné prostředí, které jsou dostupné na výpočetní uzlech ve fondu Batch, také a informace o pracovních adresářích úkolu najdete v části **nastavení prostředí pro úkoly** a **souborů a adresářů**  v hello [přehled funkcí Azure Batch](batch-api-basics.md).
>
>

## <a name="step-4-create-batch-job"></a>Krok 4: Vytvoření úlohy Batch
![Vytvoření úlohy Batch][4]<br/>

**Úloha** Batch je kolekcí úkolů a je přidružená k fondu výpočetních uzlů. Hello úlohy pro úlohu provést hello přidružené fondu výpočetních uzlů.

Úlohu můžete použít nejen k uspořádání a sledování úkolů v souvisejících úlohách, ale také pro nastavení určitých omezení – například maximálního runtime hello hello úlohy (a rozšíření, její úkoly) a priority úloh ve vztahu tooother úloh v hello účtu Batch. V tomto příkladu však hello úlohy je přidruženo pouze hello fondu, který byl vytvořen v kroku #3. Žádné další vlastnosti se nekonfigurují.

Všechny úlohy Batch jsou přidružené ke konkrétnímu fondu. Toto přidružení označuje uzlů, které hello úlohy provést. Zadejte fond hello pomocí hello [PoolInformation] [ py_poolinfo] vlastnost, jak je znázorněno v následující fragment kódu hello.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with hello specified ID, associated with hello specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID for hello job.
    :param str pool_id: hello ID for hello pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Teď, když vytvořil úlohu úlohy se přidají pracovní tooperform hello.

## <a name="step-5-add-tasks-toojob"></a>Krok 5: Přidejte toojob úlohy
![Přidat toojob úlohy][5]<br/>
*(1) úkoly jsou přidány toohello úlohy, hello (2) úkoly jsou naplánované toorun na uzlech a hello (3) úkoly stahují hello data souborů tooprocess*

Batch **úlohy** jsou hello jednotlivé jednotky práce, které jsou spouštěny na hello výpočetních uzlů. Úkol má příkazový řádek a spouští hello skripty nebo spustitelné soubory, které zadáte v takovém příkazovém řádku.

tooactually práci, musí úkoly nejprve přidat úloha tooa. Každý [CloudTask] [ py_task] nastavena vlastnost příkazového řádku a [ResourceFiles] [ py_resource_file] (stejně jako u StartTask fondu hello) této hello úkol stáhne toohello uzlu předtím, než se jeho příkazový řádek automaticky spustí. Každý úkol v ukázce hello zpracovává jenom jeden soubor. Proto jeho kolekce ResourceFiles obsahuje jen jeden prvek.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in hello collection toohello specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID of hello job toowhich tooadd hello tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: hello ID of an Azure Blob storage container to
    which hello tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    hello specified Azure Blob storage container.
    """

    print('Adding {} tasks toojob [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [!IMPORTANT]
> Když přistoupí k proměnným prostředí, jako například `$AZ_BATCH_NODE_SHARED_DIR` nebo spuštění aplikace nebyla nalezena v uzlu hello `PATH`, musí příkazové řádky úkolu vyvolání hello skořápce explicitně, jako třeba s `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Tento požadavek není nutný, pokud vaše úkoly spouští aplikace v uzlu hello `PATH` a neodkazují žádné proměnné prostředí.
>
>

V rámci hello `for` ve smyčce ve fragmentu kódu hello výše, uvidíte, že hello příkazového řádku pro úlohu hello je vytvořený pomocí pěti argumentů příkazového řádku, které se předávají příliš*python_tutorial_task.py*:

1. **FilePath**: Toto je soubor toohello hello místní cestu, protože existuje na uzlu hello. Když hello objekt ResourceFile v `upload_file_to_container` byl vytvořen v kroku 2 výše, název souboru hello použitou pro tuto vlastnost (hello `file_path` parametr v konstruktoru ResourceFile hello). To znamená, že hello soubor najdete v hello stejné adresáře na uzlu hello jako *python_tutorial_task.py*.
2. **NUMWORDS**: hello horní *N* slova zasílány toohello výstupní soubor.
3. **storageaccount**: musí být nahrán hello název hello účet úložiště, který vlastní výstup úlohy hello kontejneru toowhich hello.
4. **storagecontainer**: název hello hello úložiště kontejneru toowhich hello výstupní soubory musí být nahrán.
5. **sastoken**: hello sdílený přístupový podpis (SAS), který poskytuje přístup pro zápis toohello **výstup** kontejneru ve službě Azure Storage. Hello *python_tutorial_task.py* skript používá tento sdílený přístupový podpis při vytváření svého odkazu BlockBlobService. To poskytuje přístup pro zápis toohello kontejneru bez potřeby přístupového klíče pro účet úložiště hello.

```python
# NOTE: Taken from python_tutorial_task.py

# Create hello blob client using hello container's SAS token.
# This allows us toocreate a client that provides write
# access only toohello container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Krok 6: Sledování úkolů
![Sledujte úkoly.][6]<br/>
*Hello skript (1) monitorování hello stav dokončení úkolů a hello (2) úkoly nahrávají výsledek data tooAzure úložiště*

Pokud úkoly přidáte tooa úlohy, jsou automaticky zařazeny do fronty a naplánovaných pro spuštění na výpočetních uzlech ve fondu hello spojené s úlohou hello. Na základě hello nastavení, které zadáte, služba Batch zpracovává všechny služby Řízení front úloh, plánování, opakování a dalších úloh správy povinností za vás.

Toomonitoring provádění úkolů existuje mnoho přístupů. Hello `wait_for_tasks_to_complete` fungovat v *python_tutorial_client.py* nabízí jednoduchý příklad sledování úkolů určité stavu, v tomto případě hello [Dokončit] [ py_taskstate] stav.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in hello specified job reach hello Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello id of hello job whose tasks should be toomonitored.
    :param timedelta timeout: hello duration toowait for task completion. If all
    tasks in hello specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Krok 7: Stažení výstupu úkolu
![Stažení výstupu úkolu ze služby Storage][7]<br/>

Teď, když hello dokončení úlohy hello výstup z úlohy hello si můžete stáhnout ze služby Azure Storage. To lze provést pomocí volání příliš`download_blobs_from_container` v *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from hello specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: hello Azure Blob storage container from which to
     download files.
    :param directory_path: hello local directory toowhich toodownload hello files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] too{}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [!NOTE]
> Hello volání příliš`download_blobs_from_container` v *python_tutorial_client.py* Určuje, že hello soubory by měly být stažené tooyour domovský adresář. Myslíte, že volné toomodify to výstupní umístění.
>
>

## <a name="step-8-delete-containers"></a>Krok 8: Odstranění kontejnerů
Vzhledem k tomu, že musíte platit za data uložená ve službě Azure Storage, vždycky je vhodné tooremove, všechny objekty BLOB, které jsou už nutná pro úlohy Batch. V *python_tutorial_client.py*, to se provádí pomocí tří volání příliš[BlockBlobService.delete_container][py_delete_container]:

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a>Krok 9: Odstranění hello úlohy a fondu hello
V posledním kroku hello jsou výzvami toodelete hello úlohy a hello fondu, které byly vytvořeny hello *python_tutorial_client.py* skriptu. I když se vám neúčtují poplatky za úlohy a úkoly samotné, *účtují* se vám poplatky za výpočetní uzly. Proto doporučujeme, abyste uzly přidělovali, jen když je to potřeba. Odstraňování nepoužívaných fondů by mělo být součástí vašeho standardního procesu údržby.

Hello BatchServiceClient na [JobOperations] [ py_job] a [PoolOperations] [ py_pool] mají odpovídající metody odstranění, které jsou voláno, pokud Potvrdit odstranění:

```python
# Clean up Batch resources (if hello user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> Pamatujte, že se vám účtují poplatky za výpočetní prostředky, takže odstranění nepoužívaných fondů vám ušetří náklady. Mějte také, že odstraněním fondu odstraníte všechny výpočetní uzly v tomto fondu, a že všechna data na uzlech hello neopravitelné po odstranění fondu hello.
>
>

## <a name="run-hello-sample-script"></a>Spustit ukázkový skript hello
Když spustíte hello *python_tutorial_client.py* skript z hello kurzu [ukázka kódu][github_article_samples], výstup konzoly hello je podobné toohello následující. Dojde k pozastavení při `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` hello fondu výpočetních uzlů se vytvoření, spuštění, a jsou provedeny hello příkazy ve spouštěcím úkolu fondu hello. Použití hello [portál Azure] [ azure_portal] toomonitor fondu, výpočetních uzlů, úlohy a úkolů během a po spuštění. Použití hello [portál Azure] [ azure_portal] nebo hello [Microsoft Azure Storage Explorer] [ storage_explorer] tooview hello úložiště prostředků (kontejnerů a objektů BLOB) vytvořená aplikace hello.

> [!TIP]
> Spustit hello *python_tutorial_client.py* skript z v rámci hello `azure-batch-samples/Python/Batch/article_samples` adresáře. Používá relativní cesta pro hello `common.helpers` importu modulu, může se zobrazit `ImportError: No module named 'common'` Pokud nespouštět skript hello v tomto adresáři.
>
>

Typická doba provádění je **přibližně 5 – 7 minut** při spuštění ukázkové hello ve výchozí konfiguraci.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py toocontainer [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt toocontainer [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks toojob [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached hello 'Completed' state within hello specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] too/home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] too/home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] too/home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER tooexit...
```

## <a name="next-steps"></a>Další kroky
Působí volné toomake změny příliš*python_tutorial_client.py* a *python_tutorial_task.py* výpočetní tooexperiment jiné scénáře. Zkuste například přidat prodlevu provádění příliš*python_tutorial_task.py* toosimulate dlouho běžící úlohy a monitorovat je hello portálu. Zkuste přidat další úkoly nebo upravte hello počet výpočetních uzlů. Přidejte logiku toocheck pro a povolit hello použití existující doba provádění toospeed fondu.

Teď, když jste obeznámeni s hello základní pracovní postup řešení Batch, je čas toodig v toohello další funkce hello služby Batch.

* Zkontrolujte hello [funkcí přehled Azure Batch](batch-api-basics.md) článek, který doporučujeme, pokud jste novou službu toohello.
* Spuštění na hello další články vývoj Batch pod **vývoje** v hello [Batch studijní][batch_learning_path].
* Podívejte se na různé implementace zpracování úlohy hello "nejčastějších N slov" pomocí služby Batch v hello [TopNWords] [ github_topnwords] ukázka.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage
[pypi_install]: https://packaging.python.org/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Vytvoření kontejnerů ve službě Azure Storage"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Soubory toocontainers odeslání úloh aplikace a vstupních (data)"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Vytvoření fondu Batch"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Vytvoření úlohy Batch"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Přidat toojob úlohy"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Stažení výstupu úkolu ze služby Storage"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Pracovní postup řešení Batch (úplný diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Přihlašovací údaje Batch na portálu"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Přihlašovací údaje služby Storage na portálu"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Pracovní postup řešení Batch (minimální diagram)"
