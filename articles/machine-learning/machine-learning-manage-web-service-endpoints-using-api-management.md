---
title: "jak toomanage AzureML webové služby pomocí rozhraní API správy aaaLearn | Microsoft Docs"
description: "Příručka znázorňující, jak toomanage AzureML webové služby pomocí rozhraní API správy."
keywords: "strojového učení, rozhraní api management"
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 6e764fbfd71be6cc908a1c8d3d8889969fc651a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-how-toomanage-azureml-web-services-using-api-management"></a>Zjistěte, jak toomanage AzureML webové služby pomocí rozhraní API Management
## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak tooquickly začněte používat API Management toomanage AzureML webové služby.

## <a name="what-is-azure-api-management"></a>Co je Azure API Management?
Azure API Management je služba Azure, která umožňuje spravovat koncové body REST API definováním přístupu uživatele, omezení využití a řídicí panel monitorování. Klikněte na tlačítko [sem](https://azure.microsoft.com/services/api-management/) podrobnosti o službě Azure API Management. Klikněte na tlačítko [sem](../api-management/api-management-get-started.md) informace o tom, jak tooget práce s Azure API Management. Tento další průvodce, která tato příručka je založena na, popisuje další témata, včetně konfigurace oznámení, cenovou úroveň, zpracování odpovědi, ověřování uživatelů, vytvoření produktů, developer odběry a dashboarding využití.

## <a name="what-is-azureml"></a>Co je AzureML?
AzureML je služba Azure pro machine learning, která vám umožní tooeasily sestavení, nasazení a sdílet pokročilou analýzu řešení. Klikněte na tlačítko [sem](https://azure.microsoft.com/services/machine-learning/) podrobnosti o AzureML.

## <a name="prerequisites"></a>Požadavky
toocomplete této příručce, budete potřebovat:

* Účet Azure. Pokud nemáte účet Azure, klikněte na tlačítko [sem](https://azure.microsoft.com/pricing/free-trial/) podrobnosti o tom toocreate Bezplatný zkušební účet.
* Účet AzureML. Pokud nemáte účet AzureML, klikněte na tlačítko [sem](https://studio.azureml.net/) podrobnosti o tom toocreate Bezplatný zkušební účet.
* Hello prostoru, služby a api_key pro experimentu AzureML nasadit jako webovou službu. Klikněte na tlačítko [sem](machine-learning-create-experiment.md) podrobnosti o jak experimentovat toocreate AzureML. Klikněte na tlačítko [sem](machine-learning-publish-a-machine-learning-web-service.md) podrobnosti o způsobu toodeploy AzureML experimentovat jako webovou službu. Příloha A případně obsahuje pokyny, jak toocreate a testovací jednoduché AzureML experiment a nasadit jako webovou službu.

## <a name="create-an-api-management-instance"></a>Vytvoření instance služby API Management
Níže jsou hello kroky pro používání rozhraní API správy toomanage AzureML webovou službu. Nejprve vytvořte instanci služby. Přihlaste se toohello [portálu Classic](https://manage.windowsazure.com/) a klikněte na tlačítko **nový** > **aplikační služby** > **API Management**  >  **Vytvořit**.

![Vytvoření instance](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Zadejte jedinečný **URL**. Tato příručka používá **demoazureml** – budete potřebovat toochoose něco jiný. Zvolte hello potřeby **předplatné** a **oblast** pro instanci služby. Po provedení výběru klikněte na tlačítko Další hello.

![Vytvoření service-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Zadejte hodnotu pro hello **název organizace**. Tato příručka používá **demoazureml** – budete potřebovat toochoose něco jiný. Zadejte e-mailovou adresu v hello **e-mailu správce** pole. Tato e-mailová adresa se používá pro oznámení z hello systému API Management.

![Vytvoření service-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Klikněte na zaškrtávací políčko toocreate hello instanci služby. *Zabírají toothirty minut pro nové služby toobe vytvořit*.

## <a name="create-hello-api"></a>Vytvoření rozhraní API hello
Po vytvoření instance služby hello hello dalším krokem je toocreate hello API. Rozhraní API se skládá ze sady operací, které můžete vyvolat z klientské aplikace. Operace rozhraní API jsou směrovány přes proxy server tooexisting webové služby. Tento průvodce vytvoří rozhraní API tohoto proxy toohello existující záznamy o prostředku AzureML a BES webové služby.

Rozhraní API jsou vytvořena a nakonfigurována hello rozhraní API portálu vydavatele, který je přístupný prostřednictvím portálu Azure Classic hello. tooreach hello vydavatele portálu, vyberte instanci služby.

![Vyberte instanci služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klikněte na tlačítko **spravovat** v hello portálu Azure Classic služby API Management.

![Spravovat službu](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidat rozhraní API**.

![rozhraní API. správu nabídky](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Typ **API ukázkový AzureML** jako hello **název webového rozhraní API**. Typ **https://ussouthcentral.services.azureml.net** jako hello **adresu URL webové služby**. Typ **azureml ukázku** jako hello **přípona adresy URL webového rozhraní API**. Zkontrolujte **HTTPS** jako hello **adresy URL webového rozhraní API** schéma. Vyberte **Starter** jako **produkty**. Po dokončení klikněte na tlačítko **Uložit** toocreate hello rozhraní API.

![Přidat – nový – rozhraní api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-hello-operations"></a>Přidání operací hello
Klikněte na tlačítko **operace přidání** toothis tooadd operace rozhraní API.

![Přidání operace](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Hello **operaci nového** okno se zobrazí a hello **podpis** ve výchozím nastavení bude vybraná karta.

## <a name="add-rrs-operation"></a>Záznamy o prostředku operace přidání
Nejprve vytvořte operace hello AzureML RRS služby. Vyberte **POST** jako hello **příkaz HTTP**. Typ **/services/ /workspaces/ {prostoru} {služby} / execute? api-version = {apiversion} & Podrobnosti = {Podrobnosti}** jako hello **adresa URL šablony**. Typ **RRS provést** jako hello **zobrazovaný název**.

![přidat záznamy o prostředku operace podpisu](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**. Klikněte na tlačítko **Uložit** toosave tuto operaci.

![přidat záznamy o prostředku operace odpověď](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>Přidání BES operací
Snímky obrazovky nejsou zahrnuty pro hello BES operací, jako jsou velmi podobné toothose pro přidání hello RRS operaci.

### <a name="submit-but-not-start-a-batch-execution-job"></a>Odeslání (ale nespustí) úlohy Batch Execution
Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API. Vyberte **POST** pro hello **příkaz HTTP**. Typ **/services/ /workspaces/ {prostoru} {služby} / úloh? api-version = {apiversion}** pro hello **adresa URL šablony**. Typ **BES odeslání** pro hello **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**. Klikněte na tlačítko **Uložit** toosave tuto operaci.

### <a name="start-a-batch-execution-job"></a>Spuštění úlohy Batch Execution
Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API. Vyberte **POST** pro hello **příkaz HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid} / start? api-version = {apiversion}** pro hello **adresa URL šablony**. Typ **BES spustit** pro hello **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**. Klikněte na tlačítko **Uložit** toosave tuto operaci.

### <a name="get-hello-status-or-result-of-a-batch-execution-job"></a>Získat stav hello nebo výsledek úlohy Batch Execution
Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API. Vyberte **získat** pro hello **příkaz HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid}? api-version = {apiversion}** pro hello **adresa URL šablony**. Typ **BES stav** pro hello **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**. Klikněte na tlačítko **Uložit** toosave tuto operaci.

### <a name="delete-a-batch-execution-job"></a>Odstranit úlohu Batch Execution
Klikněte na tlačítko **operace přidání** tooadd hello AzureML BES operace toohello rozhraní API. Vyberte **odstranit** pro hello **příkaz HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid}? api-version = {apiversion}** pro hello **adresa URL šablony**. Typ **BES odstranit** pro hello **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na hello vlevo a vyberte **200 OK**. Klikněte na tlačítko **Uložit** toosave tuto operaci.

## <a name="call-an-operation-from-hello-developer-portal"></a>Volání operace z hello portál pro vývojáře
Operace lze volat přímo z portálu pro vývojáře hello, který nabízí pohodlný způsob tooview a otestovat hello operace rozhraní API. V tomto kroku průvodce budete volat hello **RRS provést** metoda, která byla přidána toohello **API ukázkový AzureML**. Klikněte na tlačítko **portál pro vývojáře** hello nabídce v hello top napravo od hello portálu Classic.

![portál pro vývojáře](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Klikněte na tlačítko **rozhraní API** z hello horní nabídce a pak klikněte na tlačítko **API ukázkový AzureML** toosee hello operations k dispozici.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Vyberte **RRS provést** pro operaci hello. Klikněte na tlačítko **vyzkoušet**.

![Try it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Žádost o parametry, zadejte vaše **prostoru**, **služby**, **2.0** pro hello **apiversion**, a **true**pro hello **podrobnosti**. Můžete najít váš **prostoru** a **služby** na řídicím panelu hello AzureML webové služby (najdete v části **testování hello webové služby** v příloze A).

Hlavičky žádosti, klikněte na tlačítko **přidat hlavičku** a typ **Content-Type** a **application/json**, pak klikněte na tlačítko **přidat hlavičku** a typ **autorizace** a **nosiče <YOUR AZUREML SERVICE API-KEY>** . Můžete najít váš **klíč rozhraní api** na řídicím panelu hello AzureML webové služby (najdete v části **testování hello webové služby** v příloze A).

Typ **{"Vstupy": {"input1": {"ColumnNames": ["Col2"], "hodnoty": [["Toto je dobrý den"]]}}, "GlobalParameters": {}}** tělo žádosti hello.

![rozhraní api azureml demo](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klikněte na tlačítko **odeslat**.

![Odeslat](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Po vyvolání operace portál pro vývojáře hello zobrazí hello **požadovaná adresa URL** z back endové službě hello, hello **stav odpovědi**, hello **hlavičky odpovědi**, a jakýkoli **obsah odpovědi**.

![Stav odpovědi](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Příloha A - vytváření a testování jednoduché AzureML webové služby
### <a name="creating-hello-experiment"></a>Vytvoření experimentu hello
Níže jsou hello kroky pro vytvoření jednoduchého experimentu AzureML a jeho nasazení jako webové služby. jako vstup sloupec libovolný textu Hello trvá webové služby a vrátí sadu funkcí vyjádřena jako celá čísla. Například:

| Text | Hash textu |
| --- | --- |
| To je dobrý den |1 1 2 2 0 2 0 1 |

Nejdřív pomocí prohlížeče podle vaší volby, přejděte na: [https://studio.azureml.net/](https://studio.azureml.net/) a zadejte vaše přihlašovací údaje toolog v. Dále vytvořte nový prázdný experiment.

![Hledat experimentu – šablony](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Přejmenujte ji příliš**SimpleFeatureHashingExperiment**. Rozbalte položku **uložit datové sady** a přetáhněte ji **kniha recenze z Amazon** do experimentu.

![jednoduché – funkce-algoritmu hash experimentu](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Rozbalte položku **transformaci dat** a **manipulaci s** a přetáhněte ji **výběr sloupců v datové sadě** do experimentu. Připojit **kniha recenze z Amazon** příliš**výběr sloupců v datové sadě**.

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Klikněte na tlačítko **výběr sloupců v datové sadě** a pak klikněte na **spustit selektor sloupců** a vyberte **Col2**. Klikněte na tlačítko zaškrtnutí tooapply hello tyto změny.

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Rozbalte položku **Analýza textu** a přetáhněte ji **Hashování** do experimentu hello. Připojit **výběr sloupců v datové sadě** příliš**Hashování**.

![připojit sloupce projektu](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Typ **3** pro hello **algoritmu hash bitsize**. Tím se vytvoří 8 (23) sloupce.

![použití algoritmu hash bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

V tomto okamžiku může být vhodné tooclick **spustit** tootest hello experimentu.

![Spustit](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Vytvoření webové služby
Teď vytvořte webovou službu. Rozbalte položku **webové služby** a přetáhněte ji **vstup** do experimentu. Připojit **vstup** příliš**Hashování**. Také přetáhněte **výstup** do experimentu. Připojit **výstup** příliš**Hashování**.

![výstup na--hashování](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klikněte na tlačítko **publikování webové služby**.

![publikování – webové služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klikněte na tlačítko **Ano** toopublish hello experimentu.

![Ano publikování](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-hello-web-service"></a>Testování hello webové služby
Webové služby AzureML se skládá z RSS (požadavků a odpovědí služby) a koncové body BES (dávky spuštění služby). RSS je pro synchronní zpracování. BES je pro provádění asynchronní úlohy. tootest vaší webové služby s hello ukázkový Python zdroj níže, musíte toodownload a hello instalace Azure SDK pro jazyk Python (viz: [jak tooinstall Python](../python-how-to-install.md)).

Budete také potřebovat hello **prostoru**, **služby**, a **api_key** experimentu pro zdroj ukázek hello níže. Pracovní prostor hello a služby zjistíte kliknutím na možnost **požadavků a odpovědí** nebo **Batch Execution** svého experimentu v řídicím panelu hello webových služeb.

![Najít prostoru a service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Můžete najít hello **api_key** kliknutím experimentu v řídicím panelu hello webových služeb.

![najít klíč rozhraní api](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>Koncový bod RRS testu
##### <a name="test-button"></a>Tlačítko Test
Koncový bod snadno tootest hello RRS je tooclick **Test** na řídicí panel hello webové služby.

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Typ **to je dobrý den** pro **col2**. Klikněte na tlačítko zaškrtnutí hello.

![zadávání dat](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Zobrazí se něco podobného jako

![Ukázkový výstup](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Ukázkový kód
Jiný způsob tootest vaše RRS je z vašeho kódu klienta. Pokud kliknete na tlačítko **požadavků a odpovědí** hello řídicí panel a posuňte toohello dole, uvidíte ukázkový kód pro C#, Python a R. Zobrazí se také hello syntaxe hello RRS požadavku, včetně hello požadavek URI, hlavičky a text.

Tato příručka ukazuje příklad Python funkční. Budete potřebovat toomodify její hello **prostoru**, **služby**, a **api_key** experimentu.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("hello request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a>Koncový bod BES testu
Klikněte na tlačítko **spuštění dávky** hello řídicí panel a posuňte toohello dole. Zobrazí se ukázkový kód pro C#, Python a R. Zobrazí se také hello syntaxe hello BES požadavky toosubmit úlohu, spustit úlohu, získat stav hello nebo výsledků úlohy a odstranit úlohu.

Tato příručka ukazuje příklad Python funkční. Je třeba toomodify její hello **prostoru**, **služby**, a **api_key** experimentu. Kromě toho bude nutné toomodify hello **název účtu úložiště**, **klíč účtu úložiště**, a **název kontejneru úložiště**. A konečně, budete potřebovat toomodify hello umístění hello **vstupní soubor** a hello umístění hello **výstupní soubor**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH hello API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH hello LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH hello LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("hello request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading hello result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written toohello file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("hello results for " + outputName + " are available at hello following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "hello results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading hello input tooblob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded hello input tooblob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting hello job...")
    # submit hello job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove hello enclosing double-quotes
    print("Job ID: " + job_id)
    # start hello job
    print("Starting hello job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking hello job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in hello follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
