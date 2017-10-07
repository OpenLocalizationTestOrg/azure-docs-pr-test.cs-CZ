---
title: "aaaTen způsobů, jak na hello datové vědy virtuálního počítače | Microsoft Docs"
description: "Provádění různých zkoumání dat a modelování úloh v hello vědecké zpracování dat virtuálního počítače."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 4dfe22f14f00208c63e26ce44b05123c9ac4b850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ten-things-you-can-do-on-hello-data-science-virtual-machine"></a>Deset kroky, které můžete provést na hello vědecké zpracování dat virtuálního počítače
Hello Microsoft Data vědecké účely virtuálního počítače (DSVM) je výkonný data vědecké účely vývojového prostředí, které umožňuje vám tooperform různé úlohy zkoumání a modelování data. Hello prostředí dodává již vytvořené a připojené oblíbených daty analytics nástroje, které umožňují snadno tooget rychle začít s analýzy pro místní, Cloudová nebo hybridní nasazení. Hello DSVM úzce spolupracuje s řadou služeb Azure a je možné tooread a zpracování dat, které jsou již uloženy v Azure, Azure SQL Data Warehouse, Azure Storage a Azure Data Lake nebo v Azure Cosmos DB. Můžete využít i jiné analytics nástroje, například Azure Machine Learning a Azure Data Factory.

V tomto článku jsme vás provedou postupem toouse vaše DSVM tooperform různé vědě data úloh a komunikovat s jinými službami Azure. Tady jsou některé hello věcí, které můžete provést jak na hello DSVM:

1. Prozkoumejte data a vyvíjet modely místně na hello DSVM použití serveru Microsoft R, Python
2. Použít tooexperiment poznámkového bloku Jupyter s daty v prohlížeči pomocí R Microsoft Python 2, Python 3, verze enterprise připravené R určená pro škálovatelnost a výkon
3. Zprovoznit modely vytvářeny pomocí R a Python v Azure Machine Learning, tak klientské aplikace mají přístup k vaší modelů pomocí jednoduchého webového rozhraní služby
4. Správa prostředků Azure pomocí portálu Azure nebo prostředí Powershell
5. Rozšíření prostor úložiště a sdílet rozsáhlých datových sad / kódu napříč celý tým vytvoření Azure File storage jako připojit jednotku ve vašem DSVM
6. Sdílet kódu se svým týmem pomocí Githubu a přístup k vaší úložiště pomocí hello předem nainstalovaná Git klienti - Git Bash a Git grafickým uživatelským rozhraním.
7. Přístup k různým Azure data a analýzy službám jako úložiště objektů blob v Azure, Azure Data Lake, Azure HDInsight (Hadoop), Cosmos databáze Azure, Azure SQL Data Warehouse & databáze
8. Vytvářejte sestavy a řídicí panel pomocí hello Power BI Desktop předinstalované na hello DSVM a nasadit je v cloudu hello
9. Dynamicky škálovat vaše DSVM toomeet, musí váš projekt
10. Nainstalujte další nástroje na virtuálním počítači   

> [!NOTE]
> Další využití poplatky pro řadu hello další datové úložiště a analýzy služby uvedené v tomto článku. Podrobnosti najdete toohello [Azure – ceny](https://azure.microsoft.com/pricing/) stránce Podrobnosti.
> 
> 

**Požadavky**

* Budete potřebovat předplatné Azure. Můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/free/).
* Pokyny pro zřízení virtuálního počítače vědecké účely dat na portálu Azure hello jsou k dispozici v [vytvoření virtuálního počítače](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Prozkoumejte data a vývoj modelů pomocí serveru Microsoft R nebo Python
Analytické údaje dat můžete použít jazyky jako R a Python toodo na hello DSVM vpravo.

Pro R můžete použít IDE názvem "Revolution R Enterprise 8.0", který se nachází v nabídce start hello nebo plochy hello. Společnost Microsoft poskytuje další knihovny nad hello otevřený zdroj/CRAN-R tooenable škálovatelné analytics hello možnost tooanalyze data a větší než velikost paměti hello povolená díky paralelní bloku analýzy. Můžete taky nainstalovat IDE R z vaše volba jako [Rstudia](https://www.rstudio.com/products/rstudio-desktop/).

Pro jazyk Python můžete použít rozhraní IDE, jako Visual Studio Community Edition, který má hello Python Tools pro Visual Studio (PTVS) rozšíření předinstalován. Ve výchozím nastavení je nakonfigurován pouze základní Python 2.7 na PTVS (bez jakékoli knihovnu analytics jako SciKit, Pandas). Pořadí tooenable Anaconda Python 2.7 a 3.5 je třeba toodo hello následující:

* Vytvoření vlastního prostředí pro každou verzi přechodem příliš**nástroje** -> **Python Tools** -> **prostředí Python** a potom kliknutím na " **+ Vlastní**"v hello Community Edition sady Visual Studio 2015
* Zadejte popis a nastavte prostředí hello cesty předponu jako *c:\anaconda* Anaconda Python 2.7 nebo *c:\anaconda\envs\py35* pro Anaconda Python 3.5
* Klikněte na tlačítko **automatické rozpoznání** a potom **použít** toosave hello prostředí.

Zde je jaké nastavení vlastního prostředí hello vypadá jako v sadě Visual Studio.

![Instalační program PTVS](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

V tématu hello [dokumentaci k těmto nástrojům](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) získáte další informace o tom toocreate prostředí Python.

Nyní jsou nastavíte toocreate nový projekt Python. Přejděte příliš**soubor** -> **nový** -> **projektu** -> **Python** a vyberte typ hello Aplikace Python, které vytváříte. Můžete nastavit hello prostředí Python pro hello aktuální projekt toohello požadovanou verzi (Anaconda 2.7 nebo 3.5): klikněte pravým tlačítkem na hello **prostředí Python**, vyberte **prostředí Python přidat nebo odebrat**, a potom vyberte hello požadovaného s projektem hello tooassociate prostředí. Můžete najít další informace o práci s PTVS na produktu hello [dokumentace](https://github.com/Microsoft/PTVS/wiki) stránky.

## <a name="2-using-a-jupyter-notebook-tooexplore-and-model-your-data-with-python-or-r"></a>2. Pomocí poznámkového bloku Jupyter tooexplore a modelu vaše data v Pythonu nebo R
Hello Poznámkový blok Jupyter je výkonný prostředí, který poskytuje založené na prohlížeči "IDE" pro zkoumání dat a modelování. V poznámkovém bloku Jupyter můžete použít Python 2, Python 3 nebo R (Open Source a hello Microsoft R Server).

toolaunch hello Poznámkový blok Jupyter klikněte na ikonu nabídky start hello / ikony na ploše s názvem **Poznámkový blok Jupyter**. Na hello DSVM můžete také vyhledat příliš "https://localhost:9999 /" tooaccess hello Jupiter Poznámkový blok. Pokud budete vyzváni k zadání hesla, použijte pokyny uvedené v hello ***jak toocreate silné heslo na serveru poznámkového bloku Jupyter hello*** části hello [zřídit hello Microsoft Data vědecké účely virtuálního počítače](machine-learning-data-science-provision-vm.md)toocreate tématu poznámkového bloku Jupyter hello tooaccess silné heslo. 

Po spuštění hello Poznámkový blok, měli byste vidět adresář, který obsahuje pár příklad poznámkových bloků, které jsou předem zabalené do hello DSVM. Nyní můžete:

* Klikněte na hello poznámkového bloku toosee hello kódu.
* Spusťte jednotlivých buněk tak, že stisknete **zadejte SHIFT**.
* Kliknutím na spustit celý poznámkový blok hello **buňky** -> **spustit**
* Kliknutím na hello Jupyter ikona (levém horním rohu) a potom kliknutím na vytvořit nový poznámkový blok **nový** tlačítko na hello vpravo a pak vyberete hello poznámkového bloku jazyka (také označované jako jádra).   

> [!NOTE]
> Momentálně podporujeme Python 2.7, Python 3.5 a R. hello R jádra podporuje programování v Open source R jak hello enterprise škálovatelné R Server společnosti Microsoft.   
> 
> 

Jakmile jsou v poznámkovém bloku hello můžete prozkoumat vaše data, sestavení modelu hello, hello model pomocí vybraného knihovny testů.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. Vytvářet modely R nebo Python a Operationalize jejich používání pomocí Azure Machine Learning
Jakmile máte vytvořené a ověřit modelu hello dalším krokem je obvykle toodeploy ho do produkčního prostředí. To umožňuje vašeho klienta aplikace tooinvoke hello modelu předpovědi v reálném čase, nebo na základě režimu dávky. Azure Machine Learning poskytuje mechanismus toooperationalize model součástí R nebo Python.

Pokud jste zprovoznit modelu v Azure Machine Learning, je vystaven webové služby, umožňuje klientům toomake volání REST, která předejte vstupní parametry a přijímat předpovědi z modelu hello jako výstup.   

> [!NOTE]
> Pokud jste ještě nezaregistrovali jste se pro Azure Machine Learning, lze získat volného prostoru nebo standardní pracovní prostor z hello [Azure Machine Learning Studio](https://studio.azureml.net/) domovské stránky a kliknutím na "Začínáme".   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Sestavení a zprovoznit Python modely
Zde je fragment kódu vyvinuté v poznámkového bloku Jupyter Python, který sestaví jednoduchého modelu pomocí hello SciKit další knihovny.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

Metoda Hello používá toodeploy vaše tooAzure python modely Machine Learning předpovědi zabalí hello hello modelu do funkce a upraví s atributy poskytované hello předem nainstalovaná knihovna python Azure Machine Learning, které označují počítači Azure ID pracovního prostoru Learning, klíč rozhraní API a hello vstup a vrátí parametry.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Klient nyní můžete provést volání toohello webové služby. Existují obálky pohodlí, které vytvořit hello požadavky REST API. Zde je ukázka kódu tooconsume hello webové služby.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> Knihovna Azure Machine Learning Hello je podporována pouze na Python 2.7 aktuálně.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Sestavení a zprovoznit R modely
Můžete nasadit R modely vytvořené na hello datové vědy virtuálního počítače nebo jinde na Azure Machine Learning způsobem, který je podobný toohow, které se provádí pro jazyk Python. Jeho hello kroky:

* Vytvořte tooprovide souboru settings.json ID pracovního prostoru a ověření tokenu, jak je znázorněno v následující ukázka kódu hello.
* Obálka pro hello modelu předpovědi funkce zápisu
* volání ```publishWebService``` v hello toopass knihovny Azure Machine Learning v hello funkce obálku.  

Zde je postup a kód fragmenty hello, které je možné použít tooset nahoru, sestavení, publikovat a využívat model jako webovou službu v Azure Machine Learning.

#### <a name="setup"></a>Nastavení
1. Instalovat balíček Machine Learning R hello zadáním ```install.packages("AzureML")``` v Revolution R Enterprise 8.0 IDE nebo vaše R IDE.
2. Stáhněte si RTools z [zde](https://cran.r-project.org/bin/windows/Rtools/). Je nutné hello zip nástroj v cestě hello (a s názvem zip.exe) toooperationalize váš balíček R do Machine Learning.
3. Vytvořte soubor settings.json pod adresář s názvem ```.azureml``` pod domovského adresáře a zadejte parametry hello z pracovního prostoru Azure Machine Learning:

Settings.JSON strukturu souborů:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>Vytvoření modelu v R a publikujete ho v Azure Machine Learning
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function toopublish based on hello model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-hello-model-deployed-in-azure-machine-learning"></a>Využívat hello model nasadit v Azure Machine Learning
model hello tooconsume z klientské aplikace, používáme hello Azure Machine Learning knihovny toolook až hello publikované webové služby podle názvu pomocí hello `services` koncový bod rozhraní API volání toodetermine hello. Pak stačí zavolat hello `consume` funkce a předávat hello dat rámce toobe předpovědět.
Následující kód Hello je model hello použité tooconsume publikován jako webové služby Azure Machine Learning.

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use hello last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Další informace o hello knihovně Azure Machine Learning R naleznete [zde](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Správa prostředků Azure pomocí portálu Azure nebo prostředí Powershell
Hello DSVM nejen vám umožní toobuild řešení analytics místně na hello virtuálního počítače, ale také umožňuje tooaccess služeb v cloudu Azure společnosti Microsoft. Azure poskytuje několik výpočty, úložiště, služby analýzy dat a jiných služeb, které můžete spravovat a přistupovat z vašeho DSVM.

tooadminister předplatné a cloudových prostředků Azure můžete použít prohlížeč a bodu toothe [portál Azure](https://portal.azure.com). Můžete taky tooadminister prostředí Azure Powershell vaše předplatné Azure a prostředky pomocí skriptu.
Můžete spustit prostředí Azure Powershell pomocí zástupce na ploše hello nebo ze hello spusťte nabídky s názvem "Microsoft Azure Powershell". Odkazovat na [dokumentace k Microsoft Azure Powershell](../powershell-azure-resource-manager.md) Další informace o tom, jak můžete spravovat vaše předplatné Azure a prostředkům pomocí skriptů prostředí Windows Powershell.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Rozšíření prostor úložiště pomocí systému sdílený soubor
Datových vědců můžete sdílet rozsáhlých datových sad, kódu nebo jiným prostředkům v rámci týmu hello. Hello DSVM samotné má přibližně 70GB volného místa. tooextend úložiště, můžete použít hello souboru služby Azure a připojte ho na hello DSVM nebo přístup přes rozhraní REST API.   

> [!NOTE]
> Maximální místo sdílení souboru služby Azure hello Hello je 5TB a maximální velikost jednotlivých souborů je 1TB.   
> 
> 

Můžete vytvořit prostředí Azure Powershell toocreate serveru sdílení souboru služby Azure. Zde je hello toorun skriptu v prostředí Azure PowerShell toocreate serveru služby Sdílení souborů Azure.

    # Authenticate tooAzure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under hello FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List hello share tooconfirm that everything worked
    Get-AzureStorageFile -Share $s


Teď, když vytvoříte sdílenou složku Azure, můžete ji připojit žádné virtuální počítač v Azure. Důrazně doporučujeme, že je tento hello virtuálních počítačů ve stejném datovém centru Azure jako hello úložiště účet tooavoid latence a datový přenos poplatky. Tady jsou hello příkazy toomount hello jednotku na hello DSVM, který můžete spustit v prostředí Azure Powershell.

    # Get storage key of hello storage account that has hello Azure file share from Azure portal. Store it securely on hello VM tooavoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount hello Azure file share as Z: drive on hello VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Nyní můžete zobrazit tuto jednotku stejně jako všechny normální jednotky na hello virtuálních počítačů.

## <a name="6-share-code-with-your-team-using-github"></a>6. Sdílet kódu se svým týmem pomocí Githubu
GitHub je úložiště kódu, kde můžete najít spoustu ukázkový kód a zdroje k různým nástrojům pomocí různých technologií, které jsou sdíleny komunity vývojářů hello. Jako hello technologie tootrack a úložiště verzí soubory kódu hello používá Git. GitHub je také platforma, kde můžete vytvořit vlastní úložiště toostore sdílené kód a dokumentace, váš tým implementovat verze ovládací prvky a také kteří mají přístup tooview a přispívat kódu. Navštivte hello [stránky nápovědy Githubu](https://help.github.com/) pro další informace o použití Git. Můžete použít Githubu jako jeden ze způsobů toocollaborate hello se svým týmem, použít kód vyvinuté hello komunity a přispívat kód back toohello komunity.

Hello DSVM už dodává s klientskými nástroji načíst na obou příkazového řádku jako dobře tooaccess grafickým uživatelským rozhraním úložiště GitHub. Nástroj příkazového řádku toowork Hello s Gitu a Githubu se nazývá Git Bash. Visual Studio instalovaného na hello DSVM má hello Git rozšíření. Můžete najít ikony spuštění těchto nástrojů v nabídce start hello a vzdálené ploše hello.

toodownload kód z úložiště Githubu použijete hello ```git clone``` příkaz. Například úložiště vědecké účely data toodownload publikovaný microsoftem do aktuální adresář hello můžete spustit hello následující příkaz, jakmile jsou v ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

V sadě Visual Studio, můžete provést hello stejné operace klonování. Hello následující snímek obrazovky ukazuje, jak tooaccess Gitu a Githubu nástroje v sadě Visual Studio.

![Git v sadě Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

Můžete najít další informace o použití Git toowork s úložiště GitHub z několika zdrojů, které jsou k dispozici na webu github.com. Hello [tahák](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) je užitečné referenční.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Přístup k různým Azure službám data a analýzy
### <a name="azure-blob"></a>Azure Blob
Objektů blob v Azure je spolehlivé a ekonomické cloudové úložiště pro data velká a malá. Dejte nám se podívejte na tom, jak můžete přesunout tooAzure data objektů Blob a přístup k datům uloženým v objektu Blob Azure.

**Požadavek**

* **Vytvoření účtu úložiště objektů Blob v Azure z [portál Azure](https://portal.azure.com).**

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Potvrďte, že hello předem nainstalovaný nástroj příkazového řádku AzCopy se nachází zde ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Při spuštění tohoto nástroje můžete přidat hello obsahující hello azcopy.exe tooyour cesta prostředí proměnné tooavoid zadáním hello celý příkaz cesta k adresáři. Další informace o nástroj AzCopy naleznete příliš[dokumentaci k AzCopy](../storage/common/storage-use-azcopy.md)
* Spusťte nástroj Azure Storage Explorer hello. Lze ji stáhnout z [Microsoft Azure Storage Explorer](http://storageexplorer.com/). 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

**Přesun dat z virtuálních počítačů tooAzure objektů Blob: AzCopy**

toomove dat mezi vaší místní soubory a úložiště objektů blob, můžete pomocí nástroje AzCopy v příkazového řádku nebo prostředí PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Nahraďte **C:\myfolder** toohello cestu, kde je uložen soubor, **můj_účet_úložiště** tooyour název účtu úložiště objektů blob, **můj_kontejner** toohello název kontejneru, **klíč účtu úložiště** tooyour přístupový klíč k úložišti objektů blob. Můžete najít přihlašovací údaje účtu úložiště v [portál Azure](https://portal.azure.com).

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

V prostředí PowerShell nebo z příkazového řádku, spusťte příkaz AzCopy. Tady je některé příklady použití nástroje AzCopy příkazu:

    # Copy *.sql from local machine tooa Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container tooLocal machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Po spuštění vaší AzCopy příkaz toocopy tooan objektů blob v Azure najdete v části souboru se zobrazí v Azure Storage Explorer za chvíli.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Přesun dat z virtuálních počítačů tooAzure objektů Blob: Azure Storage Explorer**

Můžete také uložit data z místního souboru hello v virtuálního počítače pomocí Průzkumníka úložiště Azure:

* kontejner tooa tooupload dat, vyberte hello cílový kontejner a klikněte na tlačítko hello **nahrát** tlačítko.![ Nahrát ve Storage Exploreru](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)
* Klikněte na hello **...**  toohello napravo od hello **soubory** pole, vyberte jeden nebo více souborů tooupload hello systém souborů a klikněte na **nahrát** toobegin nahrávání souborů hello.![ Nahrát tooblob soubory](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)

**Čtení dat z Azure Blob: modul čtečky Machine Learning**

V nástroji Azure Machine Learning Studio můžete použít **importovat Data modulu** tooread data z objektu blob služby.

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Čtení dat z Azure Blob: Python ODBC**

Můžete použít **BlobService** knihovny tooread data přímo z objektu blob v programu Poznámkový blok Jupyter nebo Python.

Nejprve importujte požadované balíčky:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Potom zařaďte přihlašovací údaje účtu Azure Blob a čtení dat z objektu Blob:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds toodownload "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'hello size of hello data is: %d rows and  %d columns' % df1.shape

Hello data se čtou v jako snímek dat:

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Storage je velkého rozsahu úložiště pro úlohy analýzy velkých objemů dat a kompatibilní s Hadoop Distributed File System (HDFS). Funguje s ekosystémem Hadoop hello i hello Azure Data Lake Analytics. Ukážeme, jak můžete přesun dat do hello Azure Data Lake Store a spustit analytics pomocí Azure Data Lake Analytics.

**Požadavek**

* Vytvoření vaší Azure Data Lake Analytics v [portál Azure](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* Hello **nástrojů Azure Data Lake** v **Visual Studio** najít v této [odkaz](https://www.microsoft.com/download/details.aspx?id=49504) je již nainstalován na hello Visual Studio Community Edition, který je na virtuálním počítači hello. Po spuštění sady Visual Studio a protokolování ve vašem předplatném Azure, měli byste vidět účet Azure Data Analytics a úložiště v levém panelu hello sady Visual Studio.

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Přesun dat z virtuálních počítačů tooData Lake: Azure Data Lake Explorer**

Můžete použít **Azure Data Lake Explorer** tooupload data z místních souborů hello v úložišti Lake tooData virtuálního počítače.

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Můžete také vytvořit datový kanál tooproductionize vaše tooor přesun dat z Azure Data Lake pomocí hello [Azure dat Factory(ADF)](https://azure.microsoft.com/services/data-factory/). Označujeme si toothis [článku](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide vás hello kroky toobuild hello data kanálů.

**Čtení dat z Azure Blob tooData Lake: U-SQL**

Pokud máte data uložená v úložišti objektů Blob v Azure, můžete přímo přečíst data z objektu blob úložiště Azure v dotazu U-SQL. Před skládání dotazu U-SQL, zajistěte, aby byl váš účet úložiště objektů blob propojené tooyour Azure Data Lake. Přejděte příliš**portál Azure**, najít řídicí panel Azure Data Lake Analytics, klikněte na **přidat zdroj dat**, vyberte typ úložiště příliš**Azure Storage** a připojte ve službě Azure Storage Název účtu a klíč. Pak je možné tooreference hello data uložená v účtu úložiště hello.

![Zadejte účet úložiště a klíč](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

V sadě Visual Studio můžete číst data z úložiště objektů blob, udělat některé manipulaci s daty, funkce inženýrství a výstup hello Výsledná data tooeither Azure Data Lake nebo úložiště objektů Blob Azure. Když odkazujete hello dat v úložišti objektů blob, použijte **wasb: / /**; když odkazujete hello data v Azure Data Lake použití **swbhdfs: / /**

![Data rámečku](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

Můžete použít následující dotazy U-SQL v sadě Visual Studio hello:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    too"swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    too"wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Odeslaná toohello serveru po dotazu se zobrazí diagram zobrazující hello stav úlohy.

![Diagram stavu úlohy](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

**Dotaz na data v Data Lake: U-SQL**

Po hello datové sady je konzumována do Azure Data Lake, můžete použít [jazykem U-SQL](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery a zkoumat hello data. Jazyk U-SQL je podobné Tool SQL, ale spojuje některé funkce z jazyka C#, tak, aby uživatelé můžete napsat vlastní moduly, uživatelem definované funkce a atd. V předchozím kroku hello můžete použít skripty hello.

Po hello dotaz je odeslaná tooserver tripdata_summary. Sdílený svazek clusteru najdete krátce v **Azure Data Lake Explorer**, může náhled hello dat klikněte pravým tlačítkem na soubor hello.

![Soubor v Průzkumníku služby Azure Data Lake](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

informace o souboru toosee hello:

![Souhrn souborů](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>Clustery HDInsight Hadoop
Azure HDInsight je spravovaná služba Apache Hadoop, Spark, HBase nebo Storm v cloudu hello. Můžete snadno pracovat s Azure HDInsight clustery z hello datové vědy virtuálního počítače.

**Požadavek**

* Vytvoření účtu úložiště objektů Blob v Azure z [portál Azure](https://portal.azure.com). Tento účet úložiště je použité toostore data pro clustery služby HDInsight.

![Vytvořit účet úložiště objektů Blob v Azure](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Přizpůsobení clusterů systému Hadoop HDInsight Azure z [portálu Azure](machine-learning-data-science-customize-hadoop-cluster.md)
  
  * Je nutné propojit hello úložiště účet vytvořený k vašemu clusteru HDInsight, když je vytvořeno. Tento účet úložiště se používá pro přístup k datům, která může být zpracována v rámci clusteru hello.

![Odkaz toostorage účet vytvořený s clusterem HDInsight](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* Je nutné povolit **vzdáleného přístupu** toohello hlavního uzlu clusteru hello po jejím vytvoření. Pamatovat přihlašovací údaje vzdáleného přístupu hello zde určíte (liší od nastavení zadané pro cluster hello při jeho vytváření): je nutné v následujícím postupu hello.

![Povolte vzdálený přístup](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Vytvořte pracovní prostor služby Azure Machine Learning. Vaše strojovým učením jsou uloženy v tomto pracovním prostoru Machine Learning. Vyberte možnosti hello zvýrazněná portálu, jak ukazuje následující snímek obrazovky hello:

![Vytvoření pracovního prostoru Azure Machine Learningu](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* Zadejte parametry hello pracovního prostoru

![Zadejte parametry pracovního prostoru Machine Learning](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Nahrání dat pomocí IPython Poznámkový blok. Importujte ho nejprve požadované balíčky, zařaďte přihlašovací údaje, vytvořit db ve vašem účtu úložiště a pak načíst data tooHDI clustery.

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create hello connection tooHive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage tooHDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* Alternativně můžete to provést [návod](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC taxíkem data tooHDI clusteru. Hlavní kroky zahrnují:
  
  * AzCopy: stažení komprimované CSV z místní složky tooyour veřejného objektu blob
  * AzCopy: nahrajte rozbalené sdílených svazcích clusteru z místní složky tooHDI clusteru
  * Přihlaste se k hlavnímu uzlu clusteru Hadoop hello a příprava pro analýzu nahodilého dat

Po hello dat je načíst tooHDI cluster, můžete zkontrolovat vaše data v Azure Storage Explorer. A máte databáze nyctaxidb, vytvořené v clusteru HDI.

**Zkoumání dat: dotazů Hive v Pythonu**

Vzhledem k tomu, že hello data je v clusteru Hadoop, můžete použít hello pyodbc balíček tooconnect tooHadoop clustery a dotaz do databáze pomocí Hive toodo zkoumání a funkce inženýrství. Můžete zobrazit hello existující tabulky, které jsme vytvořili v kroku požadovaných hello.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Zobrazit existující tabulky](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Podívejme se na hello počet záznamů v každém měsíci a hello frekvence šikmý nebo není v tabulce hello cesty:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Vykreslení počet záznamů v každém měsíci](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Vykreslení frekvencí tipu](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Můžeme také výpočetní hello vzdálenost mezi výstupní umístění a dropoff umístění a porovnejte je toohello dojít vzdálenost.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![Vyzvednutí a dropoff tabulky](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![Vykreslení vyzvednutí/dropoff vzdálenost tootrip vzdálenost](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Nyní Pojďme Příprava nižší vzorků (1 %) sady dat pro modelování. Tyto údaje můžete použít v modul čtečky Machine Learning.

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of hello join into hello preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Po chvíli uvidíte, že načtení dat hello v clusterů systému Hadoop:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabulka dat](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**Čtení dat z HDI pomocí Machine Learning: modul čtečky**

Můžete také používat hello **čtečky** modulu v Machine Learning Studio tooaccess hello databázi v clusteru Hadoop. Připojte hello pověření HDI clusterů a účet úložiště Azure tooenable sestavení ing modelů strojového učení pomocí databáze v clusterech HDI.

![Vlastnosti modulu Reader](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

Hello scored datovou sadu lze následně zobrazit:

![Zobrazení skóre pro magnitudu datové sady](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse & databáze
Azure SQL Data Warehouse je elastické datového skladu jako služba podnikových prostředí systému SQL Server.

Můžete zřídit Azure SQL Data Warehouse pomocí následujících pokynů hello v tomto [článku](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). Po zřízení Azure SQL Data Warehouse, můžete to [návod](machine-learning-data-science-process-sqldw-walkthrough.md) odesílání dat toodo, zkoumání a modelování pomocí dat v rámci hello SQL Data Warehouse.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos DB je databáze NoSQL v cloudu hello. Ho můžete toowork s dokumenty jako JSON a můžete dokumenty hello toostore a dotazů.

Je nutné toodo hello následujících za požadavky kroky tooaccess Azure Cosmos DB z hello DSVM.

1. Instalace DocumentDB Python SDK (Spustit ```pip install pydocumentdb``` z příkazového řádku)
2. Vytvoření účtu Azure Cosmos databáze a databáze z [portálu Azure](https://portal.azure.com)
3. Stáhnout "Nástroj pro migraci Azure Cosmos DB" z [zde](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) a extrahování directory tooa podle svého výběru
4. Umožňuje importovat data JSON (sopka data) uložené na [veřejného objektu blob](https://cahandson.blob.core.windows.net/samples/volcano.json) do databáze Cosmos s následující příkaz parametry toohello nástroj pro migraci (dtui.exe z hello adresáře, kam jste nainstalovali nástroj pro migraci DB Cosmos hello). Zadejte hello zdrojové a cílové umístění s těmito parametry:
   
    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[klíče]; Database = sopka /t.Collection:volcano1

Jakmile importujete hello data, můžete přejít tooJupyter a s názvem Poznámkový blok otevřete hello *DocumentDBSample* obsahující python code tooaccess DocumentDB a provádět některé základní dotazování. Další informace o Cosmos DB návštěvou hello služby [stránky dokumentace, která](https://docs.microsoft.com/azure/cosmos-db/).

## <a name="8-build-reports-and-dashboard-using-hello-power-bi-desktop"></a>8. Vytvářejte sestavy a řídicí panel pomocí hello Power BI Desktop
Dejte nám Vizualizujte souboru hello sopka JSON, který jsme viděli v předchozím příkladu Cosmos DB v Power BI toogain visual přehledy hello data hello. Podrobné pokyny jsou k dispozici v hello [Power BI článku](../cosmos-db/powerbi-visualize.md). Zde jsou základní kroky hello:

1. Otevřít Power BI Desktop a do "Získat Data". Zadejte adresu URL hello jako: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Měli byste vidět importovaných jako seznam záznamů JSON hello
3. Převést hello seznamu tooa tabulky, Power BI mohli pracovat s hello stejné
4. Rozbalte hello sloupce kliknutím na hello rozbalte ikonu (hello jeden "šipku vlevo a šipku vpravo" ikonou hello na hello napravo od hello sloupec)
5. Všimněte si, že umístění je na pole "Záznam". Rozbalte hello záznam a vyberte pouze hello souřadnice. Souřadnice je sloupec seznamu
6. Umožňuje přidat nový sloupec tooconvert hello seznamu souřadnic sloupec do čárkami samostatný LatLong sloupec zřetězení hello dva elementy v poli seznamu souřadnic hello pomocí vzorce hello ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Nakonec převést hello ```Elevation``` tooDecimal sloupce a vyberte hello **Zavřít** a **použít**.

Místo předchozích kroků, vložte následující kód, který skriptů se hello postupem v hello hello Upřesnit v Power BI, který vám umožní transformace dat hello toowrite v jazyce dotazu.

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted tooTable" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted tooTable", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



Nyní máte hello data v Power BI datového modelu. Power BI ploše by měl vypadat takto:

![Power BI Desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

Můžete začít vytvářet sestavy a vizualizací pomocí hello datového modelu. Můžete provést hello kroky v tomto [Power BI článku](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild sestavy. Konečný výsledek Hello je sestava, která vypadá jako následující hello.

![Power BI Desktop zobrazení sestavy - Power BI connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-toomeet-your-project-needs"></a>9. Dynamicky škálovat vaše DSVM toomeet, musí váš projekt
Je možné škálovat nahoru a dolů hello DSVM toomeet, musí váš projekt. Pokud nepotřebujete toouse hello virtuálních počítačů v hello večer nebo o víkendech, můžete právě vypnout hello virtuálních počítačů z hello [portál Azure](https://portal.azure.com).

> [!NOTE]
> Poplatky za výpočty dojít, pokud používáte pouze hello tlačítko vypnutí operačního systému na hello virtuálních počítačů.  
> 
> 

Pokud potřebujete toohandle některé rozsáhlé analýzy a potřebovat větší kapacitu procesoru nebo paměti a disku můžete najít velké volba velikostí virtuálních počítačů z hlediska jader procesoru, kapacita paměti a disku typy (včetně jednotek SSD), které splňují vaše výpočetní a rozpočtových potřebám. Hello úplný seznam virtuálních počítačů spolu s jejich hodinové výpočetní ceny je k dispozici na hello [ceny virtuálních počítačů Azure](https://azure.microsoft.com/pricing/details/virtual-machines/) stránky.

Podobně pokud snižuje potřeba kapacity zpracování virtuálního počítače (například: přesunut tooa hlavní úloh Hadoop nebo clusteru Spark), je možné škálovat dolů hello clusteru z hello [portál Azure](https://portal.azure.com) a toohello nastavení virtuálního počítače instance. Zde je snímek.

![Nastavení instance virtuálních počítačů](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. Nainstalujte další nástroje na virtuálním počítači
Budeme mít zabalené několik nástrojů, které se domníváme, jsou možné tooaddress řadu hello běžným potřebám analýzy dat a který by měl šetří čas vyhnout nutnosti tooinstall a konfigurace vašeho prostředí po jednom a ušetřit peníze platebním pouze pro prostředky, které použití.

Můžete využít další Azure data a analytické služby profilovaným v tooenhance Tento článek prostředí analýzy. Chápeme, že v některých případech může vyžadovat vašim potřebám dalších nástrojů, včetně některá vlastnické nástroje třetích stran. Máte plný přístup správce na hello virtuálního počítače tooinstall nové nástroje, které potřebujete. Můžete taky nainstalovat další balíčky Python a R, která nejsou předem nainstalovaná. Pro jazyk Python můžete použít buď ```conda``` nebo ```pip```. R můžete použít hello ```install.packages()``` v hello R konzole nebo pomocí hello IDE a vyberte "**balíčky** -> **instalovat balíčky...** ".

## <a name="summary"></a>Souhrn
To jsou jen některé hello věcí, které můžete provést jak na hello Microsoft Data vědecké účely virtuálního počítače. Existuje mnoho dalších věcí, které můžete provést toomake ho prostředí efektivní analýzu.

