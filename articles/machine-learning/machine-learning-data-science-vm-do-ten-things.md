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
# <a name="ten-things-you-can-do-on-hello-data-science-virtual-machine"></a><span data-ttu-id="c8cd8-103">Deset kroky, které můžete provést na hello vědecké zpracování dat virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c8cd8-103">Ten things you can do on hello Data science Virtual Machine</span></span>
<span data-ttu-id="c8cd8-104">Hello Microsoft Data vědecké účely virtuálního počítače (DSVM) je výkonný data vědecké účely vývojového prostředí, které umožňuje vám tooperform různé úlohy zkoumání a modelování data.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-104">hello Microsoft Data Science Virtual Machine (DSVM) is a powerful data science development environment that enables you tooperform various data exploration and modeling tasks.</span></span> <span data-ttu-id="c8cd8-105">Hello prostředí dodává již vytvořené a připojené oblíbených daty analytics nástroje, které umožňují snadno tooget rychle začít s analýzy pro místní, Cloudová nebo hybridní nasazení.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-105">hello environment comes already built and bundled with several popular data analytics tools that make it easy tooget started quickly with your analysis for On-premises, Cloud or hybrid deployments.</span></span> <span data-ttu-id="c8cd8-106">Hello DSVM úzce spolupracuje s řadou služeb Azure a je možné tooread a zpracování dat, které jsou již uloženy v Azure, Azure SQL Data Warehouse, Azure Storage a Azure Data Lake nebo v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-106">hello DSVM works closely with many Azure services and is able tooread and process data that is already stored on Azure, in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage, or in Azure Cosmos DB.</span></span> <span data-ttu-id="c8cd8-107">Můžete využít i jiné analytics nástroje, například Azure Machine Learning a Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-107">It can also leverage other analytics tools such as Azure Machine Learning and Azure Data Factory.</span></span>

<span data-ttu-id="c8cd8-108">V tomto článku jsme vás provedou postupem toouse vaše DSVM tooperform různé vědě data úloh a komunikovat s jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-108">In this article we walk you through how toouse your DSVM tooperform various data science tasks and interact with other Azure services.</span></span> <span data-ttu-id="c8cd8-109">Tady jsou některé hello věcí, které můžete provést jak na hello DSVM:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-109">Here are some of hello things you can do on hello DSVM:</span></span>

1. <span data-ttu-id="c8cd8-110">Prozkoumejte data a vyvíjet modely místně na hello DSVM použití serveru Microsoft R, Python</span><span class="sxs-lookup"><span data-stu-id="c8cd8-110">Explore data and develop models locally on hello DSVM using Microsoft R Server, Python</span></span>
2. <span data-ttu-id="c8cd8-111">Použít tooexperiment poznámkového bloku Jupyter s daty v prohlížeči pomocí R Microsoft Python 2, Python 3, verze enterprise připravené R určená pro škálovatelnost a výkon</span><span class="sxs-lookup"><span data-stu-id="c8cd8-111">Use a Jupyter notebook tooexperiment with your data on a browser using Python 2, Python 3, Microsoft R an enterprise ready version of R designed for scalability and performance</span></span>
3. <span data-ttu-id="c8cd8-112">Zprovoznit modely vytvářeny pomocí R a Python v Azure Machine Learning, tak klientské aplikace mají přístup k vaší modelů pomocí jednoduchého webového rozhraní služby</span><span class="sxs-lookup"><span data-stu-id="c8cd8-112">Operationalize models built using R and Python on Azure Machine Learning so client applications can access your models using a simple web services interface</span></span>
4. <span data-ttu-id="c8cd8-113">Správa prostředků Azure pomocí portálu Azure nebo prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="c8cd8-113">Administer your Azure resources using  Azure portal or Powershell</span></span>
5. <span data-ttu-id="c8cd8-114">Rozšíření prostor úložiště a sdílet rozsáhlých datových sad / kódu napříč celý tým vytvoření Azure File storage jako připojit jednotku ve vašem DSVM</span><span class="sxs-lookup"><span data-stu-id="c8cd8-114">Extend your storage space and share large-scale datasets / code across your whole team by creating an Azure File storage as a mountable drive on your DSVM</span></span>
6. <span data-ttu-id="c8cd8-115">Sdílet kódu se svým týmem pomocí Githubu a přístup k vaší úložiště pomocí hello předem nainstalovaná Git klienti - Git Bash a Git grafickým uživatelským rozhraním.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-115">Share code with your team using GitHub and access your repository using hello pre-installed Git clients - Git Bash, Git GUI.</span></span>
7. <span data-ttu-id="c8cd8-116">Přístup k různým Azure data a analýzy službám jako úložiště objektů blob v Azure, Azure Data Lake, Azure HDInsight (Hadoop), Cosmos databáze Azure, Azure SQL Data Warehouse & databáze</span><span class="sxs-lookup"><span data-stu-id="c8cd8-116">Access various Azure data and analytics services like Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & databases</span></span>
8. <span data-ttu-id="c8cd8-117">Vytvářejte sestavy a řídicí panel pomocí hello Power BI Desktop předinstalované na hello DSVM a nasadit je v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="c8cd8-117">Build reports and dashboard using hello Power BI Desktop pre-installed on hello DSVM and deploy them on hello cloud</span></span>
9. <span data-ttu-id="c8cd8-118">Dynamicky škálovat vaše DSVM toomeet, musí váš projekt</span><span class="sxs-lookup"><span data-stu-id="c8cd8-118">Dynamically scale your DSVM toomeet your project needs</span></span>
10. <span data-ttu-id="c8cd8-119">Nainstalujte další nástroje na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="c8cd8-119">Install additional tools on your virtual machine</span></span>   

> [!NOTE]
> <span data-ttu-id="c8cd8-120">Další využití poplatky pro řadu hello další datové úložiště a analýzy služby uvedené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-120">Additional usage charges apply for many of hello additional data storage and analytics services listed in this article.</span></span> <span data-ttu-id="c8cd8-121">Podrobnosti najdete toohello [Azure – ceny](https://azure.microsoft.com/pricing/) stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-121">Please refer toohello [Azure Pricing](https://azure.microsoft.com/pricing/) page for details.</span></span>
> 
> 

<span data-ttu-id="c8cd8-122">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-122">**Prerequisites**</span></span>

* <span data-ttu-id="c8cd8-123">Budete potřebovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-123">You need an Azure subscription.</span></span> <span data-ttu-id="c8cd8-124">Můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-124">You can sign up for a free trial [here](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="c8cd8-125">Pokyny pro zřízení virtuálního počítače vědecké účely dat na portálu Azure hello jsou k dispozici v [vytvoření virtuálního počítače](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-125">Instructions for provisioning a Data Science Virtual Machine on hello Azure portal are available at [Creating a virtual machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span></span>

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a><span data-ttu-id="c8cd8-126">1. Prozkoumejte data a vývoj modelů pomocí serveru Microsoft R nebo Python</span><span class="sxs-lookup"><span data-stu-id="c8cd8-126">1. Explore data and develop models using Microsoft R Server or Python</span></span>
<span data-ttu-id="c8cd8-127">Analytické údaje dat můžete použít jazyky jako R a Python toodo na hello DSVM vpravo.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-127">You can use languages like R and Python toodo your data analytics right on hello DSVM.</span></span>

<span data-ttu-id="c8cd8-128">Pro R můžete použít IDE názvem "Revolution R Enterprise 8.0", který se nachází v nabídce start hello nebo plochy hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-128">For R, you can use an IDE called "Revolution R Enterprise 8.0" that can be found on hello start menu or hello desktop.</span></span> <span data-ttu-id="c8cd8-129">Společnost Microsoft poskytuje další knihovny nad hello otevřený zdroj/CRAN-R tooenable škálovatelné analytics hello možnost tooanalyze data a větší než velikost paměti hello povolená díky paralelní bloku analýzy.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-129">Microsoft has provided additional libraries on top of hello Open source/CRAN-R tooenable scalable analytics and hello ability tooanalyze data larger than hello memory size allowed by doing parallel chunked analysis.</span></span> <span data-ttu-id="c8cd8-130">Můžete taky nainstalovat IDE R z vaše volba jako [Rstudia](https://www.rstudio.com/products/rstudio-desktop/).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-130">You can also install an R IDE of your choice like [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span></span>

<span data-ttu-id="c8cd8-131">Pro jazyk Python můžete použít rozhraní IDE, jako Visual Studio Community Edition, který má hello Python Tools pro Visual Studio (PTVS) rozšíření předinstalován.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-131">For Python, you can use an IDE like Visual Studio Community Edition which has hello Python Tools for Visual Studio (PTVS) extension pre-installed.</span></span> <span data-ttu-id="c8cd8-132">Ve výchozím nastavení je nakonfigurován pouze základní Python 2.7 na PTVS (bez jakékoli knihovnu analytics jako SciKit, Pandas).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-132">By default, only a basic Python 2.7 is configured on PTVS (without any analytics library like SciKit, Pandas).</span></span> <span data-ttu-id="c8cd8-133">Pořadí tooenable Anaconda Python 2.7 a 3.5 je třeba toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-133">In order tooenable Anaconda Python 2.7 and 3.5, you need toodo hello following:</span></span>

* <span data-ttu-id="c8cd8-134">Vytvoření vlastního prostředí pro každou verzi přechodem příliš**nástroje** -> **Python Tools** -> **prostředí Python** a potom kliknutím na " **+ Vlastní**"v hello Community Edition sady Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="c8cd8-134">Create custom environments for each version by navigating too**Tools** -> **Python Tools** -> **Python Environments** and then clicking "**+ Custom**" in hello Visual Studio 2015 Community Edition</span></span>
* <span data-ttu-id="c8cd8-135">Zadejte popis a nastavte prostředí hello cesty předponu jako *c:\anaconda* Anaconda Python 2.7 nebo *c:\anaconda\envs\py35* pro Anaconda Python 3.5</span><span class="sxs-lookup"><span data-stu-id="c8cd8-135">Give a description and set hello environment prefix paths as *c:\anaconda* for Anaconda Python 2.7 OR *c:\anaconda\envs\py35* for Anaconda Python 3.5</span></span>
* <span data-ttu-id="c8cd8-136">Klikněte na tlačítko **automatické rozpoznání** a potom **použít** toosave hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-136">Click **Auto Detect** and then **Apply** toosave hello environment.</span></span>

<span data-ttu-id="c8cd8-137">Zde je jaké nastavení vlastního prostředí hello vypadá jako v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-137">Here is what hello custom environment setup looks like in Visual Studio.</span></span>

![Instalační program PTVS](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

<span data-ttu-id="c8cd8-139">V tématu hello [dokumentaci k těmto nástrojům](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) získáte další informace o tom toocreate prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-139">See hello [PTVS documentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) for additional details on how toocreate Python Environments.</span></span>

<span data-ttu-id="c8cd8-140">Nyní jsou nastavíte toocreate nový projekt Python.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-140">Now you are set up toocreate a new Python project.</span></span> <span data-ttu-id="c8cd8-141">Přejděte příliš**soubor** -> **nový** -> **projektu** -> **Python** a vyberte typ hello Aplikace Python, které vytváříte.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-141">Navigate too**File** -> **New** -> **Project** -> **Python** and select hello type of Python application you are building.</span></span> <span data-ttu-id="c8cd8-142">Můžete nastavit hello prostředí Python pro hello aktuální projekt toohello požadovanou verzi (Anaconda 2.7 nebo 3.5): klikněte pravým tlačítkem na hello **prostředí Python**, vyberte **prostředí Python přidat nebo odebrat**, a potom vyberte hello požadovaného s projektem hello tooassociate prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-142">You can set hello Python environment for hello current project toohello desired version (Anaconda 2.7 or 3.5): right-click hello **Python environment**, select **Add/Remove Python Environments**, and then select hello desired environment tooassociate with hello project.</span></span> <span data-ttu-id="c8cd8-143">Můžete najít další informace o práci s PTVS na produktu hello [dokumentace](https://github.com/Microsoft/PTVS/wiki) stránky.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-143">You can find more information about working with PTVS on hello product [documentation](https://github.com/Microsoft/PTVS/wiki) page.</span></span>

## <a name="2-using-a-jupyter-notebook-tooexplore-and-model-your-data-with-python-or-r"></a><span data-ttu-id="c8cd8-144">2. Pomocí poznámkového bloku Jupyter tooexplore a modelu vaše data v Pythonu nebo R</span><span class="sxs-lookup"><span data-stu-id="c8cd8-144">2. Using a Jupyter Notebook tooexplore and model your data with Python or R</span></span>
<span data-ttu-id="c8cd8-145">Hello Poznámkový blok Jupyter je výkonný prostředí, který poskytuje založené na prohlížeči "IDE" pro zkoumání dat a modelování.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-145">hello Jupyter Notebook is a powerful environment that provides a browser-based "IDE" for data exploration and modeling.</span></span> <span data-ttu-id="c8cd8-146">V poznámkovém bloku Jupyter můžete použít Python 2, Python 3 nebo R (Open Source a hello Microsoft R Server).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-146">You can use Python 2, Python 3 or R (both Open Source and hello Microsoft R Server) in a Jupyter Notebook.</span></span>

<span data-ttu-id="c8cd8-147">toolaunch hello Poznámkový blok Jupyter klikněte na ikonu nabídky start hello / ikony na ploše s názvem **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-147">toolaunch hello Jupyter Notebook click on hello start menu icon / desktop icon titled **Jupyter Notebook**.</span></span> <span data-ttu-id="c8cd8-148">Na hello DSVM můžete také vyhledat příliš "https://localhost:9999 /" tooaccess hello Jupiter Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-148">On hello DSVM you can also browse too"https://localhost:9999/" tooaccess hello Jupiter Notebook.</span></span> <span data-ttu-id="c8cd8-149">Pokud budete vyzváni k zadání hesla, použijte pokyny uvedené v hello ***jak toocreate silné heslo na serveru poznámkového bloku Jupyter hello*** části hello [zřídit hello Microsoft Data vědecké účely virtuálního počítače](machine-learning-data-science-provision-vm.md)toocreate tématu poznámkového bloku Jupyter hello tooaccess silné heslo.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-149">If it prompts you for a password, use instructions provided in hello ***How toocreate a strong password on hello Jupyter notebook server*** section of hello [Provision hello Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) topic toocreate a strong password tooaccess hello Jupyter notebook.</span></span> 

<span data-ttu-id="c8cd8-150">Po spuštění hello Poznámkový blok, měli byste vidět adresář, který obsahuje pár příklad poznámkových bloků, které jsou předem zabalené do hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-150">Once you have opened hello notebook, you should see a directory that contains a few example notebooks that are pre-packaged into hello DSVM.</span></span> <span data-ttu-id="c8cd8-151">Nyní můžete:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-151">Now you can:</span></span>

* <span data-ttu-id="c8cd8-152">Klikněte na hello poznámkového bloku toosee hello kódu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-152">click on hello notebook toosee hello code.</span></span>
* <span data-ttu-id="c8cd8-153">Spusťte jednotlivých buněk tak, že stisknete **zadejte SHIFT**.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-153">execute each cell by pressing **SHIFT-ENTER**.</span></span>
* <span data-ttu-id="c8cd8-154">Kliknutím na spustit celý poznámkový blok hello **buňky** -> **spustit**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-154">run hello entire notebook by clicking on **Cell** -> **Run**</span></span>
* <span data-ttu-id="c8cd8-155">Kliknutím na hello Jupyter ikona (levém horním rohu) a potom kliknutím na vytvořit nový poznámkový blok **nový** tlačítko na hello vpravo a pak vyberete hello poznámkového bloku jazyka (také označované jako jádra).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-155">create a new notebook by clicking on hello Jupyter Icon (left top corner) and then clicking **New** button on hello right and then choosing hello notebook language (also known as kernels).</span></span>   

> [!NOTE]
> <span data-ttu-id="c8cd8-156">Momentálně podporujeme Python 2.7, Python 3.5 a R. hello R jádra podporuje programování v Open source R jak hello enterprise škálovatelné R Server společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-156">Currently we support Python 2.7, Python 3.5 and R. hello R kernel supports programming in both Open source R as well as hello enterprise scalable Microsoft R Server.</span></span>   
> 
> 

<span data-ttu-id="c8cd8-157">Jakmile jsou v poznámkovém bloku hello můžete prozkoumat vaše data, sestavení modelu hello, hello model pomocí vybraného knihovny testů.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-157">Once you are in hello notebook you can explore your data, build hello model, test hello model using your choice of libraries.</span></span>

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a><span data-ttu-id="c8cd8-158">3. Vytvářet modely R nebo Python a Operationalize jejich používání pomocí Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c8cd8-158">3. Build models using R or Python and Operationalize them using Azure Machine Learning</span></span>
<span data-ttu-id="c8cd8-159">Jakmile máte vytvořené a ověřit modelu hello dalším krokem je obvykle toodeploy ho do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-159">Once you have built and validated your model hello next step is usually toodeploy it into production.</span></span> <span data-ttu-id="c8cd8-160">To umožňuje vašeho klienta aplikace tooinvoke hello modelu předpovědi v reálném čase, nebo na základě režimu dávky.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-160">This allows your client applications tooinvoke hello model predictions on a real time or on a batch mode basis.</span></span> <span data-ttu-id="c8cd8-161">Azure Machine Learning poskytuje mechanismus toooperationalize model součástí R nebo Python.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-161">Azure Machine Learning provides a mechanism toooperationalize a model built in either R or Python.</span></span>

<span data-ttu-id="c8cd8-162">Pokud jste zprovoznit modelu v Azure Machine Learning, je vystaven webové služby, umožňuje klientům toomake volání REST, která předejte vstupní parametry a přijímat předpovědi z modelu hello jako výstup.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-162">When you operationalize your model in Azure Machine Learning, a web service is exposed that allows clients toomake REST calls that pass in input parameters and receive predictions from hello model as outputs.</span></span>   

> [!NOTE]
> <span data-ttu-id="c8cd8-163">Pokud jste ještě nezaregistrovali jste se pro Azure Machine Learning, lze získat volného prostoru nebo standardní pracovní prostor z hello [Azure Machine Learning Studio](https://studio.azureml.net/) domovské stránky a kliknutím na "Začínáme".</span><span class="sxs-lookup"><span data-stu-id="c8cd8-163">If you have not yet signed up for Azure Machine Learning, you can obtain a free workspace or a standard workspace by visiting hello [Azure Machine Learning Studio](https://studio.azureml.net/) home page and clicking on "Get Started".</span></span>   
> 
> 

### <a name="build-and-operationalize-python-models"></a><span data-ttu-id="c8cd8-164">Sestavení a zprovoznit Python modely</span><span class="sxs-lookup"><span data-stu-id="c8cd8-164">Build and Operationalize Python models</span></span>
<span data-ttu-id="c8cd8-165">Zde je fragment kódu vyvinuté v poznámkového bloku Jupyter Python, který sestaví jednoduchého modelu pomocí hello SciKit další knihovny.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-165">Here is a snippet of code developed in a Python Jupyter Notebook that builds a simple model using hello SciKit-learn library.</span></span>

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

<span data-ttu-id="c8cd8-166">Metoda Hello používá toodeploy vaše tooAzure python modely Machine Learning předpovědi zabalí hello hello modelu do funkce a upraví s atributy poskytované hello předem nainstalovaná knihovna python Azure Machine Learning, které označují počítači Azure ID pracovního prostoru Learning, klíč rozhraní API a hello vstup a vrátí parametry.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-166">hello method used toodeploy your python models tooAzure Machine Learning wraps hello prediction of hello model into a function and decorates it with attributes provided by hello pre-installed Azure Machine Learning python library that denote your Azure Machine Learning workspace ID, API Key, and hello input and return parameters.</span></span>  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

<span data-ttu-id="c8cd8-167">Klient nyní můžete provést volání toohello webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-167">A client can now make calls toohello web service.</span></span> <span data-ttu-id="c8cd8-168">Existují obálky pohodlí, které vytvořit hello požadavky REST API.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-168">There are convenience wrappers that construct hello REST API requests.</span></span> <span data-ttu-id="c8cd8-169">Zde je ukázka kódu tooconsume hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-169">Here is a sample code tooconsume hello web service.</span></span>

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> <span data-ttu-id="c8cd8-170">Knihovna Azure Machine Learning Hello je podporována pouze na Python 2.7 aktuálně.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-170">hello Azure Machine Learning library is only supported on Python 2.7 currently.</span></span>   
> 
> 

### <a name="build-and-operationalize-r-models"></a><span data-ttu-id="c8cd8-171">Sestavení a zprovoznit R modely</span><span class="sxs-lookup"><span data-stu-id="c8cd8-171">Build and Operationalize R models</span></span>
<span data-ttu-id="c8cd8-172">Můžete nasadit R modely vytvořené na hello datové vědy virtuálního počítače nebo jinde na Azure Machine Learning způsobem, který je podobný toohow, které se provádí pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-172">You can deploy R models built on hello Data Science Virtual Machine or elsewhere onto Azure Machine Learning in a manner that is similar toohow it is done for Python.</span></span> <span data-ttu-id="c8cd8-173">Jeho hello kroky:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-173">Her are hello steps:</span></span>

* <span data-ttu-id="c8cd8-174">Vytvořte tooprovide souboru settings.json ID pracovního prostoru a ověření tokenu, jak je znázorněno v následující ukázka kódu hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-174">create a settings.json file tooprovide your workspace ID and auth token as shown in hello following code sample.</span></span>
* <span data-ttu-id="c8cd8-175">Obálka pro hello modelu předpovědi funkce zápisu</span><span class="sxs-lookup"><span data-stu-id="c8cd8-175">write a wrapper for hello model's predict function.</span></span>
* <span data-ttu-id="c8cd8-176">volání ```publishWebService``` v hello toopass knihovny Azure Machine Learning v hello funkce obálku.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-176">call ```publishWebService``` in hello Azure Machine Learning library toopass in hello function wrapper.</span></span>  

<span data-ttu-id="c8cd8-177">Zde je postup a kód fragmenty hello, které je možné použít tooset nahoru, sestavení, publikovat a využívat model jako webovou službu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-177">Here is hello procedure and code snippets that can be used tooset up, build, publish, and consume a model as a web service in Azure Machine Learning.</span></span>

#### <a name="setup"></a><span data-ttu-id="c8cd8-178">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c8cd8-178">Setup</span></span>
1. <span data-ttu-id="c8cd8-179">Instalovat balíček Machine Learning R hello zadáním ```install.packages("AzureML")``` v Revolution R Enterprise 8.0 IDE nebo vaše R IDE.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-179">Install hello Machine Learning R package by typing ```install.packages("AzureML")``` in Revolution R Enterprise 8.0 IDE or your R IDE.</span></span>
2. <span data-ttu-id="c8cd8-180">Stáhněte si RTools z [zde](https://cran.r-project.org/bin/windows/Rtools/).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-180">Download RTools from [here](https://cran.r-project.org/bin/windows/Rtools/).</span></span> <span data-ttu-id="c8cd8-181">Je nutné hello zip nástroj v cestě hello (a s názvem zip.exe) toooperationalize váš balíček R do Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-181">You need hello zip utility in hello path (and named zip.exe) toooperationalize your R package into Machine Learning.</span></span>
3. <span data-ttu-id="c8cd8-182">Vytvořte soubor settings.json pod adresář s názvem ```.azureml``` pod domovského adresáře a zadejte parametry hello z pracovního prostoru Azure Machine Learning:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-182">Create a settings.json file under a directory called ```.azureml``` under your home directory and enter hello parameters from your Azure Machine Learning workspace:</span></span>

<span data-ttu-id="c8cd8-183">Settings.JSON strukturu souborů:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-183">settings.json File structure:</span></span>

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a><span data-ttu-id="c8cd8-184">Vytvoření modelu v R a publikujete ho v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c8cd8-184">Build a model in R and publish it in Azure Machine Learning</span></span>
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

#### <a name="consume-hello-model-deployed-in-azure-machine-learning"></a><span data-ttu-id="c8cd8-185">Využívat hello model nasadit v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c8cd8-185">Consume hello model deployed in Azure Machine Learning</span></span>
<span data-ttu-id="c8cd8-186">model hello tooconsume z klientské aplikace, používáme hello Azure Machine Learning knihovny toolook až hello publikované webové služby podle názvu pomocí hello `services` koncový bod rozhraní API volání toodetermine hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-186">tooconsume hello model from a client application, we use hello Azure Machine Learning library toolook up hello published web service by name using hello `services` API call toodetermine hello endpoint.</span></span> <span data-ttu-id="c8cd8-187">Pak stačí zavolat hello `consume` funkce a předávat hello dat rámce toobe předpovědět.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-187">Then you just call hello `consume` function and pass in hello data frame toobe predicted.</span></span>
<span data-ttu-id="c8cd8-188">Následující kód Hello je model hello použité tooconsume publikován jako webové služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-188">hello following code is used tooconsume hello model published as an Azure Machine Learning web service.</span></span>

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use hello last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

<span data-ttu-id="c8cd8-189">Další informace o hello knihovně Azure Machine Learning R naleznete [zde](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-189">More information about hello Azure Machine Learning R library can be found [here](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span></span>

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a><span data-ttu-id="c8cd8-190">4. Správa prostředků Azure pomocí portálu Azure nebo prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="c8cd8-190">4. Administer your Azure resources using Azure portal or Powershell</span></span>
<span data-ttu-id="c8cd8-191">Hello DSVM nejen vám umožní toobuild řešení analytics místně na hello virtuálního počítače, ale také umožňuje tooaccess služeb v cloudu Azure společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-191">hello DSVM not only allows you toobuild your analytics solution locally on hello virtual machine, but also allows you tooaccess services on Microsoft's Azure cloud.</span></span> <span data-ttu-id="c8cd8-192">Azure poskytuje několik výpočty, úložiště, služby analýzy dat a jiných služeb, které můžete spravovat a přistupovat z vašeho DSVM.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-192">Azure provides several compute, storage, data analytics services and other services that you can administer and access from your DSVM.</span></span>

<span data-ttu-id="c8cd8-193">tooadminister předplatné a cloudových prostředků Azure můžete použít prohlížeč a bodu toothe [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-193">tooadminister your Azure subscription and cloud resources you can use your browser and point toothe [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c8cd8-194">Můžete taky tooadminister prostředí Azure Powershell vaše předplatné Azure a prostředky pomocí skriptu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-194">You can also use Azure Powershell tooadminister your Azure subscription and resources via a script.</span></span>
<span data-ttu-id="c8cd8-195">Můžete spustit prostředí Azure Powershell pomocí zástupce na ploše hello nebo ze hello spusťte nabídky s názvem "Microsoft Azure Powershell".</span><span class="sxs-lookup"><span data-stu-id="c8cd8-195">You can run Azure Powershell from a shortcut on hello desktop or from hello start menu titled "Microsoft Azure Powershell".</span></span> <span data-ttu-id="c8cd8-196">Odkazovat na [dokumentace k Microsoft Azure Powershell](../powershell-azure-resource-manager.md) Další informace o tom, jak můžete spravovat vaše předplatné Azure a prostředkům pomocí skriptů prostředí Windows Powershell.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-196">Refer to [Microsoft Azure Powershell documentation](../powershell-azure-resource-manager.md) for more information on how you can administer your Azure subscription and resources using Windows Powershell scripts.</span></span>

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a><span data-ttu-id="c8cd8-197">5. Rozšíření prostor úložiště pomocí systému sdílený soubor</span><span class="sxs-lookup"><span data-stu-id="c8cd8-197">5. Extend your storage space with a shared file system</span></span>
<span data-ttu-id="c8cd8-198">Datových vědců můžete sdílet rozsáhlých datových sad, kódu nebo jiným prostředkům v rámci týmu hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-198">Data scientists can share large datasets, code or other resources within hello team.</span></span> <span data-ttu-id="c8cd8-199">Hello DSVM samotné má přibližně 70GB volného místa.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-199">hello DSVM itself has about 70GB of space available.</span></span> <span data-ttu-id="c8cd8-200">tooextend úložiště, můžete použít hello souboru služby Azure a připojte ho na hello DSVM nebo přístup přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-200">tooextend your storage, you can use hello Azure File Service and either mount it on hello DSVM or access it via a REST API.</span></span>   

> [!NOTE]
> <span data-ttu-id="c8cd8-201">Maximální místo sdílení souboru služby Azure hello Hello je 5TB a maximální velikost jednotlivých souborů je 1TB.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-201">hello maximum space of hello Azure File Service share is 5TB and individual file size limit is 1TB.</span></span>   
> 
> 

<span data-ttu-id="c8cd8-202">Můžete vytvořit prostředí Azure Powershell toocreate serveru sdílení souboru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-202">You can use Azure Powershell toocreate an Azure File Service share.</span></span> <span data-ttu-id="c8cd8-203">Zde je hello toorun skriptu v prostředí Azure PowerShell toocreate serveru služby Sdílení souborů Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-203">Here is hello script toorun under Azure PowerShell toocreate an Azure File service share.</span></span>

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


<span data-ttu-id="c8cd8-204">Teď, když vytvoříte sdílenou složku Azure, můžete ji připojit žádné virtuální počítač v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-204">Now that you have created an Azure file share, you can mount it in any virtual machine in Azure.</span></span> <span data-ttu-id="c8cd8-205">Důrazně doporučujeme, že je tento hello virtuálních počítačů ve stejném datovém centru Azure jako hello úložiště účet tooavoid latence a datový přenos poplatky.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-205">It is highly recommended that hello VM is in same Azure data center as hello storage account tooavoid latency and data transfer charges.</span></span> <span data-ttu-id="c8cd8-206">Tady jsou hello příkazy toomount hello jednotku na hello DSVM, který můžete spustit v prostředí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-206">Here are hello commands toomount hello drive on hello DSVM that you can run on Azure Powershell.</span></span>

    # Get storage key of hello storage account that has hello Azure file share from Azure portal. Store it securely on hello VM tooavoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount hello Azure file share as Z: drive on hello VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


<span data-ttu-id="c8cd8-207">Nyní můžete zobrazit tuto jednotku stejně jako všechny normální jednotky na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-207">Now you can access this drive as you would any normal drive on hello VM.</span></span>

## <a name="6-share-code-with-your-team-using-github"></a><span data-ttu-id="c8cd8-208">6. Sdílet kódu se svým týmem pomocí Githubu</span><span class="sxs-lookup"><span data-stu-id="c8cd8-208">6. Share code with your team using GitHub</span></span>
<span data-ttu-id="c8cd8-209">GitHub je úložiště kódu, kde můžete najít spoustu ukázkový kód a zdroje k různým nástrojům pomocí různých technologií, které jsou sdíleny komunity vývojářů hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-209">GitHub is a code repository where you can find a lot of sample code and sources for different tools using various technologies shared by hello developer community.</span></span> <span data-ttu-id="c8cd8-210">Jako hello technologie tootrack a úložiště verzí soubory kódu hello používá Git.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-210">It uses Git as hello technology tootrack and store versions of hello code files.</span></span> <span data-ttu-id="c8cd8-211">GitHub je také platforma, kde můžete vytvořit vlastní úložiště toostore sdílené kód a dokumentace, váš tým implementovat verze ovládací prvky a také kteří mají přístup tooview a přispívat kódu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-211">GitHub is also a platform where you can create your own repository toostore your team's shared code and documentation, implement version control and also control who have access tooview and contribute code.</span></span> <span data-ttu-id="c8cd8-212">Navštivte hello [stránky nápovědy Githubu](https://help.github.com/) pro další informace o použití Git.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-212">Please visit hello [GitHub help pages](https://help.github.com/) for more information on using Git.</span></span> <span data-ttu-id="c8cd8-213">Můžete použít Githubu jako jeden ze způsobů toocollaborate hello se svým týmem, použít kód vyvinuté hello komunity a přispívat kód back toohello komunity.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-213">You can use GitHub as one of hello ways toocollaborate with your team, use code developed by hello community and contribute code back toohello community.</span></span>

<span data-ttu-id="c8cd8-214">Hello DSVM už dodává s klientskými nástroji načíst na obou příkazového řádku jako dobře tooaccess grafickým uživatelským rozhraním úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-214">hello DSVM already comes loaded with client tools on both command-line as well GUI tooaccess GitHub repository.</span></span> <span data-ttu-id="c8cd8-215">Nástroj příkazového řádku toowork Hello s Gitu a Githubu se nazývá Git Bash.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-215">hello command-line tool toowork with Git and GitHub is called Git Bash.</span></span> <span data-ttu-id="c8cd8-216">Visual Studio instalovaného na hello DSVM má hello Git rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-216">Visual Studio installed on hello DSVM has hello Git extensions.</span></span> <span data-ttu-id="c8cd8-217">Můžete najít ikony spuštění těchto nástrojů v nabídce start hello a vzdálené ploše hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-217">You can find start-up icons for these tools on hello start menu and hello desktop.</span></span>

<span data-ttu-id="c8cd8-218">toodownload kód z úložiště Githubu použijete hello ```git clone``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-218">toodownload code from a GitHub repository you use hello ```git clone``` command.</span></span> <span data-ttu-id="c8cd8-219">Například úložiště vědecké účely data toodownload publikovaný microsoftem do aktuální adresář hello můžete spustit hello následující příkaz, jakmile jsou v ```git-bash```.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-219">For example toodownload data science repository published by Microsoft into hello current directory you can run hello following command once you are in ```git-bash```.</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="c8cd8-220">V sadě Visual Studio, můžete provést hello stejné operace klonování.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-220">In Visual Studio, you can do hello same clone operation.</span></span> <span data-ttu-id="c8cd8-221">Hello následující snímek obrazovky ukazuje, jak tooaccess Gitu a Githubu nástroje v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-221">hello  following screen-shot shows how tooaccess Git and GitHub tools in Visual Studio.</span></span>

![Git v sadě Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

<span data-ttu-id="c8cd8-223">Můžete najít další informace o použití Git toowork s úložiště GitHub z několika zdrojů, které jsou k dispozici na webu github.com. Hello [tahák](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) je užitečné referenční.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-223">You can find more information on using Git toowork with your GitHub repository from several resources available on github.com. hello [cheat sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is a useful reference.</span></span>

## <a name="7-access-various-azure-data-and-analytics-services"></a><span data-ttu-id="c8cd8-224">7. Přístup k různým Azure službám data a analýzy</span><span class="sxs-lookup"><span data-stu-id="c8cd8-224">7. Access various Azure data and analytics services</span></span>
### <a name="azure-blob"></a><span data-ttu-id="c8cd8-225">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="c8cd8-225">Azure Blob</span></span>
<span data-ttu-id="c8cd8-226">Objektů blob v Azure je spolehlivé a ekonomické cloudové úložiště pro data velká a malá.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-226">Azure blob is a reliable, economical cloud storage for data big and small.</span></span> <span data-ttu-id="c8cd8-227">Dejte nám se podívejte na tom, jak můžete přesunout tooAzure data objektů Blob a přístup k datům uloženým v objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-227">Let us look at how you can move data tooAzure Blob and access data stored in an Azure Blob.</span></span>

<span data-ttu-id="c8cd8-228">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-228">**Prerequisite**</span></span>

* <span data-ttu-id="c8cd8-229">**Vytvoření účtu úložiště objektů Blob v Azure z [portál Azure](https://portal.azure.com).**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-229">**Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).**</span></span>

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="c8cd8-231">Potvrďte, že hello předem nainstalovaný nástroj příkazového řádku AzCopy se nachází zde ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-231">Confirm that hello pre-installed command-line AzCopy tool is found at ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span></span> <span data-ttu-id="c8cd8-232">Při spuštění tohoto nástroje můžete přidat hello obsahující hello azcopy.exe tooyour cesta prostředí proměnné tooavoid zadáním hello celý příkaz cesta k adresáři.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-232">You can add hello directory containing hello azcopy.exe tooyour PATH environment variable tooavoid typing hello full command path when running this tool.</span></span> <span data-ttu-id="c8cd8-233">Další informace o nástroj AzCopy naleznete příliš[dokumentaci k AzCopy](../storage/common/storage-use-azcopy.md)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-233">For more info on AzCopy tool please refer too[AzCopy documentation](../storage/common/storage-use-azcopy.md)</span></span>
* <span data-ttu-id="c8cd8-234">Spusťte nástroj Azure Storage Explorer hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-234">Start hello Azure Storage Explorer tool.</span></span> <span data-ttu-id="c8cd8-235">Lze ji stáhnout z [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-235">It can be downloaded from [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span> 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

<span data-ttu-id="c8cd8-237">**Přesun dat z virtuálních počítačů tooAzure objektů Blob: AzCopy**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-237">**Move data from VM tooAzure Blob: AzCopy**</span></span>

<span data-ttu-id="c8cd8-238">toomove dat mezi vaší místní soubory a úložiště objektů blob, můžete pomocí nástroje AzCopy v příkazového řádku nebo prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-238">toomove data between your local files and blob storage, you can use AzCopy in command-line or PowerShell:</span></span>

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

<span data-ttu-id="c8cd8-239">Nahraďte **C:\myfolder** toohello cestu, kde je uložen soubor, **můj_účet_úložiště** tooyour název účtu úložiště objektů blob, **můj_kontejner** toohello název kontejneru, **klíč účtu úložiště** tooyour přístupový klíč k úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-239">Replace **C:\myfolder** toohello path where your file is stored, **mystorageaccount** tooyour blob storage account name, **mycontainer** toohello container name, **storage account key** tooyour blob storage access key.</span></span> <span data-ttu-id="c8cd8-240">Můžete najít přihlašovací údaje účtu úložiště v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-240">You can find your storage account credentials in [Azure portal](https://portal.azure.com).</span></span>

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

<span data-ttu-id="c8cd8-242">V prostředí PowerShell nebo z příkazového řádku, spusťte příkaz AzCopy.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-242">Run AzCopy command in PowerShell or from a command prompt.</span></span> <span data-ttu-id="c8cd8-243">Tady je některé příklady použití nástroje AzCopy příkazu:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-243">Here is some example usage of AzCopy command:</span></span>

    # Copy *.sql from local machine tooa Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container tooLocal machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



<span data-ttu-id="c8cd8-244">Po spuštění vaší AzCopy příkaz toocopy tooan objektů blob v Azure najdete v části souboru se zobrazí v Azure Storage Explorer za chvíli.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-244">Once you run your AzCopy command toocopy tooan Azure blob you see your file shows up in Azure Storage Explorer shortly.</span></span>

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

<span data-ttu-id="c8cd8-246">**Přesun dat z virtuálních počítačů tooAzure objektů Blob: Azure Storage Explorer**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-246">**Move data from VM tooAzure Blob: Azure Storage Explorer**</span></span>

<span data-ttu-id="c8cd8-247">Můžete také uložit data z místního souboru hello v virtuálního počítače pomocí Průzkumníka úložiště Azure:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-247">You can also upload data from hello local file in your VM using Azure Storage Explorer:</span></span>

* <span data-ttu-id="c8cd8-248">kontejner tooa tooupload dat, vyberte hello cílový kontejner a klikněte na tlačítko hello **nahrát** tlačítko.![ Nahrát ve Storage Exploreru](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-248">tooupload data tooa container, select hello target container and click hello **Upload** button.![Upload in Storage Explorer](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span></span>
* <span data-ttu-id="c8cd8-249">Klikněte na hello **...**  toohello napravo od hello **soubory** pole, vyberte jeden nebo více souborů tooupload hello systém souborů a klikněte na **nahrát** toobegin nahrávání souborů hello.![ Nahrát tooblob soubory](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-249">Click on hello **...** toohello right of hello **Files** box, select one or multiple files tooupload from hello file system and click **Upload** toobegin uploading hello files.![Upload files tooblob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span></span>

<span data-ttu-id="c8cd8-250">**Čtení dat z Azure Blob: modul čtečky Machine Learning**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-250">**Read data from Azure Blob: Machine Learning reader module**</span></span>

<span data-ttu-id="c8cd8-251">V nástroji Azure Machine Learning Studio můžete použít **importovat Data modulu** tooread data z objektu blob služby.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-251">In Azure Machine Learning Studio you can use an **Import Data module** tooread data from your blob.</span></span>

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

<span data-ttu-id="c8cd8-253">**Čtení dat z Azure Blob: Python ODBC**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-253">**Read data from Azure Blob: Python ODBC**</span></span>

<span data-ttu-id="c8cd8-254">Můžete použít **BlobService** knihovny tooread data přímo z objektu blob v programu Poznámkový blok Jupyter nebo Python.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-254">You can use **BlobService** library tooread data directly from blob in a Jupyter Notebook or Python program.</span></span>

<span data-ttu-id="c8cd8-255">Nejprve importujte požadované balíčky:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-255">First, import required packages:</span></span>

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

<span data-ttu-id="c8cd8-256">Potom zařaďte přihlašovací údaje účtu Azure Blob a čtení dat z objektu Blob:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-256">Then plug in your Azure Blob account credentials and read data from Blob:</span></span>

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

<span data-ttu-id="c8cd8-257">Hello data se čtou v jako snímek dat:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-257">hello data is read in as a data frame:</span></span>

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a><span data-ttu-id="c8cd8-259">Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="c8cd8-259">Azure Data Lake</span></span>
<span data-ttu-id="c8cd8-260">Azure Data Lake Storage je velkého rozsahu úložiště pro úlohy analýzy velkých objemů dat a kompatibilní s Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-260">Azure Data Lake Storage is a hyper-scale repository for big data analytics workloads and compatible with Hadoop Distributed File System (HDFS).</span></span> <span data-ttu-id="c8cd8-261">Funguje s ekosystémem Hadoop hello i hello Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-261">It works with both hello Hadoop ecosystem and hello Azure Data Lake Analytics.</span></span> <span data-ttu-id="c8cd8-262">Ukážeme, jak můžete přesun dat do hello Azure Data Lake Store a spustit analytics pomocí Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-262">We show how you can move data into hello Azure Data Lake Store and run analytics using Azure Data Lake Analytics.</span></span>

<span data-ttu-id="c8cd8-263">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-263">**Prerequisite**</span></span>

* <span data-ttu-id="c8cd8-264">Vytvoření vaší Azure Data Lake Analytics v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-264">Create your Azure Data Lake Analytics in [Azure portal](https://portal.azure.com).</span></span>

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* <span data-ttu-id="c8cd8-266">Hello **nástrojů Azure Data Lake** v **Visual Studio** najít v této [odkaz](https://www.microsoft.com/download/details.aspx?id=49504) je již nainstalován na hello Visual Studio Community Edition, který je na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-266">hello  **Azure Data Lake Tools** in **Visual Studio** found at this  [link](https://www.microsoft.com/download/details.aspx?id=49504) is already installed on hello Visual Studio Community Edition which is on hello virtual machine.</span></span> <span data-ttu-id="c8cd8-267">Po spuštění sady Visual Studio a protokolování ve vašem předplatném Azure, měli byste vidět účet Azure Data Analytics a úložiště v levém panelu hello sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-267">After starting Visual Studio and logging in your Azure subscription, you should see your Azure Data Analytics account and storage in hello left panel of Visual Studio.</span></span>

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

<span data-ttu-id="c8cd8-269">**Přesun dat z virtuálních počítačů tooData Lake: Azure Data Lake Explorer**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-269">**Move data from VM tooData Lake: Azure Data Lake Explorer**</span></span>

<span data-ttu-id="c8cd8-270">Můžete použít **Azure Data Lake Explorer** tooupload data z místních souborů hello v úložišti Lake tooData virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-270">You can use **Azure Data Lake Explorer** tooupload data from hello local files in your Virtual Machine tooData Lake storage.</span></span>

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

<span data-ttu-id="c8cd8-272">Můžete také vytvořit datový kanál tooproductionize vaše tooor přesun dat z Azure Data Lake pomocí hello [Azure dat Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-272">You can also build a data pipeline tooproductionize your data movement tooor from Azure Data Lake using hello [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span></span> <span data-ttu-id="c8cd8-273">Označujeme si toothis [článku](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide vás hello kroky toobuild hello data kanálů.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-273">We refer you toothis [article](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide you through hello steps toobuild hello data pipelines.</span></span>

<span data-ttu-id="c8cd8-274">**Čtení dat z Azure Blob tooData Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-274">**Read data from Azure Blob tooData Lake: U-SQL**</span></span>

<span data-ttu-id="c8cd8-275">Pokud máte data uložená v úložišti objektů Blob v Azure, můžete přímo přečíst data z objektu blob úložiště Azure v dotazu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-275">If your data resides in Azure Blob storage, you can directly read data from Azure storage blob in U-SQL query.</span></span> <span data-ttu-id="c8cd8-276">Před skládání dotazu U-SQL, zajistěte, aby byl váš účet úložiště objektů blob propojené tooyour Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-276">Before composing your U-SQL query, make sure your blob storage account is linked tooyour Azure Data Lake.</span></span> <span data-ttu-id="c8cd8-277">Přejděte příliš**portál Azure**, najít řídicí panel Azure Data Lake Analytics, klikněte na **přidat zdroj dat**, vyberte typ úložiště příliš**Azure Storage** a připojte ve službě Azure Storage Název účtu a klíč.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-277">Go too**Azure portal**, find your Azure Data Lake Analytics dashboard, click **Add Data Source**, select storage type too**Azure Storage** and plug in your Azure Storage Account Name and Key.</span></span> <span data-ttu-id="c8cd8-278">Pak je možné tooreference hello data uložená v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-278">Then you are able tooreference hello data stored in hello storage account.</span></span>

![Zadejte účet úložiště a klíč](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

<span data-ttu-id="c8cd8-280">V sadě Visual Studio můžete číst data z úložiště objektů blob, udělat některé manipulaci s daty, funkce inženýrství a výstup hello Výsledná data tooeither Azure Data Lake nebo úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-280">In Visual Studio, you can read data from blob storage, do some data manipulation, feature engineering, and output hello resulting data tooeither Azure Data Lake or Azure Blob Storage.</span></span> <span data-ttu-id="c8cd8-281">Když odkazujete hello dat v úložišti objektů blob, použijte **wasb: / /**; když odkazujete hello data v Azure Data Lake použití **swbhdfs: / /**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-281">When you reference hello data in blob storage, use **wasb://**; when you reference hello data in Azure Data Lake, use **swbhdfs://**</span></span>

![Data rámečku](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

<span data-ttu-id="c8cd8-283">Můžete použít následující dotazy U-SQL v sadě Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-283">You may use hello following U-SQL queries in Visual Studio:</span></span>

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



<span data-ttu-id="c8cd8-284">Odeslaná toohello serveru po dotazu se zobrazí diagram zobrazující hello stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-284">After your query is submitted toohello server, a diagram showing hello status of your job is displayed.</span></span>

![Diagram stavu úlohy](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

<span data-ttu-id="c8cd8-286">**Dotaz na data v Data Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-286">**Query data in Data Lake: U-SQL**</span></span>

<span data-ttu-id="c8cd8-287">Po hello datové sady je konzumována do Azure Data Lake, můžete použít [jazykem U-SQL](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery a zkoumat hello data.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-287">After hello dataset is ingested into Azure Data Lake, you can use [U-SQL language](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery and explore hello data.</span></span> <span data-ttu-id="c8cd8-288">Jazyk U-SQL je podobné Tool SQL, ale spojuje některé funkce z jazyka C#, tak, aby uživatelé můžete napsat vlastní moduly, uživatelem definované funkce a atd. V předchozím kroku hello můžete použít skripty hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-288">U-SQL language is similar tooT-SQL, but combines some features from C# so that users can write customized modules, user-defined functions, and etc. You can use hello scripts in hello previous step.</span></span>

<span data-ttu-id="c8cd8-289">Po hello dotaz je odeslaná tooserver tripdata_summary. Sdílený svazek clusteru najdete krátce v **Azure Data Lake Explorer**, může náhled hello dat klikněte pravým tlačítkem na soubor hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-289">After hello query is submitted tooserver, tripdata_summary.CSV can be found shortly in **Azure Data Lake Explorer**, you may preview hello data by right-click hello file.</span></span>

![Soubor v Průzkumníku služby Azure Data Lake](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

<span data-ttu-id="c8cd8-291">informace o souboru toosee hello:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-291">toosee hello file information:</span></span>

![Souhrn souborů](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a><span data-ttu-id="c8cd8-293">Clustery HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c8cd8-293">HDInsight Hadoop Clusters</span></span>
<span data-ttu-id="c8cd8-294">Azure HDInsight je spravovaná služba Apache Hadoop, Spark, HBase nebo Storm v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-294">Azure HDInsight is a managed Apache Hadoop, Spark, HBase, and Storm service on hello cloud.</span></span> <span data-ttu-id="c8cd8-295">Můžete snadno pracovat s Azure HDInsight clustery z hello datové vědy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-295">You can work easily with Azure HDInsight clusters from hello data science virtual machine.</span></span>

<span data-ttu-id="c8cd8-296">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-296">**Prerequisite**</span></span>

* <span data-ttu-id="c8cd8-297">Vytvoření účtu úložiště objektů Blob v Azure z [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-297">Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c8cd8-298">Tento účet úložiště je použité toostore data pro clustery služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-298">This storage account is used toostore data for HDInsight clusters.</span></span>

![Vytvořit účet úložiště objektů Blob v Azure](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="c8cd8-300">Přizpůsobení clusterů systému Hadoop HDInsight Azure z [portálu Azure](machine-learning-data-science-customize-hadoop-cluster.md)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-300">Customize Azure HDInsight Hadoop Clusters from [Azure portal](machine-learning-data-science-customize-hadoop-cluster.md)</span></span>
  
  * <span data-ttu-id="c8cd8-301">Je nutné propojit hello úložiště účet vytvořený k vašemu clusteru HDInsight, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-301">You must link hello storage account created with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="c8cd8-302">Tento účet úložiště se používá pro přístup k datům, která může být zpracována v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-302">This storage account is used for accessing data that can be processed within hello cluster.</span></span>

![Odkaz toostorage účet vytvořený s clusterem HDInsight](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* <span data-ttu-id="c8cd8-304">Je nutné povolit **vzdáleného přístupu** toohello hlavního uzlu clusteru hello po jejím vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-304">You must enable **Remote Access** toohello head node of hello cluster after it is created.</span></span> <span data-ttu-id="c8cd8-305">Pamatovat přihlašovací údaje vzdáleného přístupu hello zde určíte (liší od nastavení zadané pro cluster hello při jeho vytváření): je nutné v následujícím postupu hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-305">Remember hello remote access credentials you specify here (different from those specified for hello cluster at its creation): you need them in hello subsequent procedure.</span></span>

![Povolte vzdálený přístup](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* <span data-ttu-id="c8cd8-307">Vytvořte pracovní prostor služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-307">Create an Azure Machine Learning workspace.</span></span> <span data-ttu-id="c8cd8-308">Vaše strojovým učením jsou uloženy v tomto pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-308">Your Machine Learning Experiments are stored in this Machine Learning workspace.</span></span> <span data-ttu-id="c8cd8-309">Vyberte možnosti hello zvýrazněná portálu, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-309">Select hello highlighted options in Portal as shown in hello following screenshot:</span></span>

![Vytvoření pracovního prostoru Azure Machine Learningu](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* <span data-ttu-id="c8cd8-311">Zadejte parametry hello pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="c8cd8-311">Then enter hello parameters for your workspace</span></span>

![Zadejte parametry pracovního prostoru Machine Learning](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* <span data-ttu-id="c8cd8-313">Nahrání dat pomocí IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-313">Upload data using IPython Notebook.</span></span> <span data-ttu-id="c8cd8-314">Importujte ho nejprve požadované balíčky, zařaďte přihlašovací údaje, vytvořit db ve vašem účtu úložiště a pak načíst data tooHDI clustery.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-314">First import required packages, plug in credentials, create a db in your storage account, then load data tooHDI clusters.</span></span>

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


* <span data-ttu-id="c8cd8-315">Alternativně můžete to provést [návod](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC taxíkem data tooHDI clusteru.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-315">Alternately,  you can follow this [walkthrough](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi data tooHDI cluster.</span></span> <span data-ttu-id="c8cd8-316">Hlavní kroky zahrnují:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-316">Major steps include:</span></span>
  
  * <span data-ttu-id="c8cd8-317">AzCopy: stažení komprimované CSV z místní složky tooyour veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="c8cd8-317">AzCopy: download zipped CSV's from public blob tooyour local folder</span></span>
  * <span data-ttu-id="c8cd8-318">AzCopy: nahrajte rozbalené sdílených svazcích clusteru z místní složky tooHDI clusteru</span><span class="sxs-lookup"><span data-stu-id="c8cd8-318">AzCopy: upload unzipped CSV's from local folder tooHDI cluster</span></span>
  * <span data-ttu-id="c8cd8-319">Přihlaste se k hlavnímu uzlu clusteru Hadoop hello a příprava pro analýzu nahodilého dat</span><span class="sxs-lookup"><span data-stu-id="c8cd8-319">Log into hello head node of Hadoop cluster and prepare for exploratory data analysis</span></span>

<span data-ttu-id="c8cd8-320">Po hello dat je načíst tooHDI cluster, můžete zkontrolovat vaše data v Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-320">After hello data is loaded tooHDI cluster, you can check your data in Azure Storage Explorer.</span></span> <span data-ttu-id="c8cd8-321">A máte databáze nyctaxidb, vytvořené v clusteru HDI.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-321">And you have a database nyctaxidb created in HDI cluster.</span></span>

<span data-ttu-id="c8cd8-322">**Zkoumání dat: dotazů Hive v Pythonu**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-322">**Data exploration: Hive Queries in Python**</span></span>

<span data-ttu-id="c8cd8-323">Vzhledem k tomu, že hello data je v clusteru Hadoop, můžete použít hello pyodbc balíček tooconnect tooHadoop clustery a dotaz do databáze pomocí Hive toodo zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-323">Since hello data is in Hadoop cluster, you can use hello pyodbc package tooconnect tooHadoop Clusters and query database using Hive toodo exploration and feature engineering.</span></span> <span data-ttu-id="c8cd8-324">Můžete zobrazit hello existující tabulky, které jsme vytvořili v kroku požadovaných hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-324">You can view hello existing tables we created in hello prerequisite step.</span></span>

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Zobrazit existující tabulky](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

<span data-ttu-id="c8cd8-326">Podívejme se na hello počet záznamů v každém měsíci a hello frekvence šikmý nebo není v tabulce hello cesty:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-326">Let's look at hello number of records in each month and hello frequencies of tipped or not in hello trip table:</span></span>

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

<span data-ttu-id="c8cd8-329">Můžeme také výpočetní hello vzdálenost mezi výstupní umístění a dropoff umístění a porovnejte je toohello dojít vzdálenost.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-329">We can also compute hello distance between pickup location and dropoff location and then compare it toohello trip distance.</span></span>

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

<span data-ttu-id="c8cd8-332">Nyní Pojďme Příprava nižší vzorků (1 %) sady dat pro modelování.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-332">Now let's prepare a down-sampled (1%) set of data for modeling.</span></span> <span data-ttu-id="c8cd8-333">Tyto údaje můžete použít v modul čtečky Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-333">We can use this data in Machine Learning reader module.</span></span>

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

<span data-ttu-id="c8cd8-334">Po chvíli uvidíte, že načtení dat hello v clusterů systému Hadoop:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-334">After a while, you can see hello data has been loaded in Hadoop clusters:</span></span>

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabulka dat](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

<span data-ttu-id="c8cd8-336">**Čtení dat z HDI pomocí Machine Learning: modul čtečky**</span><span class="sxs-lookup"><span data-stu-id="c8cd8-336">**Read data from HDI using Machine Learning: reader module**</span></span>

<span data-ttu-id="c8cd8-337">Můžete také používat hello **čtečky** modulu v Machine Learning Studio tooaccess hello databázi v clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-337">You may also use hello **reader** module in Machine Learning Studio tooaccess hello database in Hadoop cluster.</span></span> <span data-ttu-id="c8cd8-338">Připojte hello pověření HDI clusterů a účet úložiště Azure tooenable sestavení ing modelů strojového učení pomocí databáze v clusterech HDI.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-338">Plug in hello credentials of your HDI clusters and Azure Storage Account tooenable build ing machine learning models using database in HDI clusters.</span></span>

![Vlastnosti modulu Reader](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

<span data-ttu-id="c8cd8-340">Hello scored datovou sadu lze následně zobrazit:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-340">hello scored dataset can then be viewed:</span></span>

![Zobrazení skóre pro magnitudu datové sady](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a><span data-ttu-id="c8cd8-342">Azure SQL Data Warehouse & databáze</span><span class="sxs-lookup"><span data-stu-id="c8cd8-342">Azure SQL Data Warehouse & databases</span></span>
<span data-ttu-id="c8cd8-343">Azure SQL Data Warehouse je elastické datového skladu jako služba podnikových prostředí systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-343">Azure SQL Data Warehouse is an elastic data warehouse as a service with enterprise-class SQL Server experience.</span></span>

<span data-ttu-id="c8cd8-344">Můžete zřídit Azure SQL Data Warehouse pomocí následujících pokynů hello v tomto [článku](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-344">You can provision your Azure SQL Data Warehouse by following hello instructions provided in this [article](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span></span> <span data-ttu-id="c8cd8-345">Po zřízení Azure SQL Data Warehouse, můžete to [návod](machine-learning-data-science-process-sqldw-walkthrough.md) odesílání dat toodo, zkoumání a modelování pomocí dat v rámci hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-345">Once you provision your Azure SQL Data Warehouse, you can use this [walkthrough](machine-learning-data-science-process-sqldw-walkthrough.md) toodo data upload, exploration and modeling using data within hello SQL Data Warehouse.</span></span>

#### <a name="azure-cosmos-db"></a><span data-ttu-id="c8cd8-346">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c8cd8-346">Azure Cosmos DB</span></span>
<span data-ttu-id="c8cd8-347">Azure Cosmos DB je databáze NoSQL v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-347">Azure Cosmos DB is a NoSQL database in hello cloud.</span></span> <span data-ttu-id="c8cd8-348">Ho můžete toowork s dokumenty jako JSON a můžete dokumenty hello toostore a dotazů.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-348">It allows you toowork with documents like JSON and allows you toostore and query hello documents.</span></span>

<span data-ttu-id="c8cd8-349">Je nutné toodo hello následujících za požadavky kroky tooaccess Azure Cosmos DB z hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-349">You need toodo hello following per-requisites steps tooaccess Azure Cosmos DB from hello DSVM.</span></span>

1. <span data-ttu-id="c8cd8-350">Instalace DocumentDB Python SDK (Spustit ```pip install pydocumentdb``` z příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-350">Install DocumentDB Python SDK (Run ```pip install pydocumentdb``` from command prompt)</span></span>
2. <span data-ttu-id="c8cd8-351">Vytvoření účtu Azure Cosmos databáze a databáze z [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-351">Create an Azure Cosmos DB account and a database from [Azure portal](https://portal.azure.com)</span></span>
3. <span data-ttu-id="c8cd8-352">Stáhnout "Nástroj pro migraci Azure Cosmos DB" z [zde](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) a extrahování directory tooa podle svého výběru</span><span class="sxs-lookup"><span data-stu-id="c8cd8-352">Download "Azure Cosmos DB Migration Tool" from [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) and extract tooa directory of your choice</span></span>
4. <span data-ttu-id="c8cd8-353">Umožňuje importovat data JSON (sopka data) uložené na [veřejného objektu blob](https://cahandson.blob.core.windows.net/samples/volcano.json) do databáze Cosmos s následující příkaz parametry toohello nástroj pro migraci (dtui.exe z hello adresáře, kam jste nainstalovali nástroj pro migraci DB Cosmos hello).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-353">Import JSON data (volcano data) stored on a [public blob](https://cahandson.blob.core.windows.net/samples/volcano.json) into Cosmos DB with following command parameters toohello migration tool (dtui.exe from hello directory where you installed hello Cosmos DB Migration Tool).</span></span> <span data-ttu-id="c8cd8-354">Zadejte hello zdrojové a cílové umístění s těmito parametry:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-354">Enter hello source and target location with these parameters:</span></span>
   
    <span data-ttu-id="c8cd8-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[klíče]; Database = sopka /t.Collection:volcano1</span><span class="sxs-lookup"><span data-stu-id="c8cd8-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1</span></span>

<span data-ttu-id="c8cd8-356">Jakmile importujete hello data, můžete přejít tooJupyter a s názvem Poznámkový blok otevřete hello *DocumentDBSample* obsahující python code tooaccess DocumentDB a provádět některé základní dotazování.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-356">Once you import hello data, you can go tooJupyter and open hello notebook titled *DocumentDBSample* which contains python code tooaccess DocumentDB and do some basic querying.</span></span> <span data-ttu-id="c8cd8-357">Další informace o Cosmos DB návštěvou hello služby [stránky dokumentace, která](https://docs.microsoft.com/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-357">You can learn more about Cosmos DB by visiting hello service [documentation page](https://docs.microsoft.com/azure/cosmos-db/).</span></span>

## <a name="8-build-reports-and-dashboard-using-hello-power-bi-desktop"></a><span data-ttu-id="c8cd8-358">8. Vytvářejte sestavy a řídicí panel pomocí hello Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="c8cd8-358">8. Build reports and dashboard using hello Power BI Desktop</span></span>
<span data-ttu-id="c8cd8-359">Dejte nám Vizualizujte souboru hello sopka JSON, který jsme viděli v předchozím příkladu Cosmos DB v Power BI toogain visual přehledy hello data hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-359">Let us visualize hello Volcano JSON file that we saw in hello preceding Cosmos DB example in Power BI toogain visual insights into hello data.</span></span> <span data-ttu-id="c8cd8-360">Podrobné pokyny jsou k dispozici v hello [Power BI článku](../cosmos-db/powerbi-visualize.md).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-360">Detailed steps are available in hello [Power BI article](../cosmos-db/powerbi-visualize.md).</span></span> <span data-ttu-id="c8cd8-361">Zde jsou základní kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-361">Here are hello high-level steps:</span></span>

1. <span data-ttu-id="c8cd8-362">Otevřít Power BI Desktop a do "Získat Data".</span><span class="sxs-lookup"><span data-stu-id="c8cd8-362">Open Power BI Desktop and do "Get Data".</span></span> <span data-ttu-id="c8cd8-363">Zadejte adresu URL hello jako: https://cahandson.blob.core.windows.net/samples/volcano.json</span><span class="sxs-lookup"><span data-stu-id="c8cd8-363">Specify hello URL as: https://cahandson.blob.core.windows.net/samples/volcano.json</span></span>
2. <span data-ttu-id="c8cd8-364">Měli byste vidět importovaných jako seznam záznamů JSON hello</span><span class="sxs-lookup"><span data-stu-id="c8cd8-364">You should see hello JSON records imported as a list</span></span>
3. <span data-ttu-id="c8cd8-365">Převést hello seznamu tooa tabulky, Power BI mohli pracovat s hello stejné</span><span class="sxs-lookup"><span data-stu-id="c8cd8-365">Convert hello list tooa table so Power BI can work with hello same</span></span>
4. <span data-ttu-id="c8cd8-366">Rozbalte hello sloupce kliknutím na hello rozbalte ikonu (hello jeden "šipku vlevo a šipku vpravo" ikonou hello na hello napravo od hello sloupec)</span><span class="sxs-lookup"><span data-stu-id="c8cd8-366">Expand hello columns by clicking on hello expand icon (hello one with hello "left arrow and a right arrow" icon on hello right of hello column)</span></span>
5. <span data-ttu-id="c8cd8-367">Všimněte si, že umístění je na pole "Záznam".</span><span class="sxs-lookup"><span data-stu-id="c8cd8-367">Notice that location is a "Record" field.</span></span> <span data-ttu-id="c8cd8-368">Rozbalte hello záznam a vyberte pouze hello souřadnice.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-368">Expand hello record and select only hello coordinates.</span></span> <span data-ttu-id="c8cd8-369">Souřadnice je sloupec seznamu</span><span class="sxs-lookup"><span data-stu-id="c8cd8-369">Coordinate is a list column</span></span>
6. <span data-ttu-id="c8cd8-370">Umožňuje přidat nový sloupec tooconvert hello seznamu souřadnic sloupec do čárkami samostatný LatLong sloupec zřetězení hello dva elementy v poli seznamu souřadnic hello pomocí vzorce hello ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-370">Add a new column tooconvert hello list coordinate column into a comma separate LatLong column concatenating hello two elements in hello coordinate list field using hello formula ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span></span>
7. <span data-ttu-id="c8cd8-371">Nakonec převést hello ```Elevation``` tooDecimal sloupce a vyberte hello **Zavřít** a **použít**.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-371">Finally convert hello ```Elevation``` column tooDecimal and select hello **Close** and **Apply**.</span></span>

<span data-ttu-id="c8cd8-372">Místo předchozích kroků, vložte následující kód, který skriptů se hello postupem v hello hello Upřesnit v Power BI, který vám umožní transformace dat hello toowrite v jazyce dotazu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-372">Instead of preceding steps, you can paste hello following code that scripts out hello steps used in hello Advanced Editor in Power BI that allows you toowrite hello data transformations in a query language.</span></span>

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted tooTable" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted tooTable", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



<span data-ttu-id="c8cd8-373">Nyní máte hello data v Power BI datového modelu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-373">You now have hello data in your Power BI data model.</span></span> <span data-ttu-id="c8cd8-374">Power BI ploše by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c8cd8-374">Your Power BI desktop should appear as follows:</span></span>

![Power BI Desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

<span data-ttu-id="c8cd8-376">Můžete začít vytvářet sestavy a vizualizací pomocí hello datového modelu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-376">You can start building reports and visualizations using hello data model.</span></span> <span data-ttu-id="c8cd8-377">Můžete provést hello kroky v tomto [Power BI článku](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild sestavy.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-377">You can follow hello steps in this [Power BI article](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild a report.</span></span> <span data-ttu-id="c8cd8-378">Konečný výsledek Hello je sestava, která vypadá jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-378">hello end result is a report that looks like hello following.</span></span>

![Power BI Desktop zobrazení sestavy - Power BI connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-toomeet-your-project-needs"></a><span data-ttu-id="c8cd8-380">9. Dynamicky škálovat vaše DSVM toomeet, musí váš projekt</span><span class="sxs-lookup"><span data-stu-id="c8cd8-380">9. Dynamically scale your DSVM toomeet your project needs</span></span>
<span data-ttu-id="c8cd8-381">Je možné škálovat nahoru a dolů hello DSVM toomeet, musí váš projekt.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-381">You can scale up and down hello DSVM toomeet your project needs.</span></span> <span data-ttu-id="c8cd8-382">Pokud nepotřebujete toouse hello virtuálních počítačů v hello večer nebo o víkendech, můžete právě vypnout hello virtuálních počítačů z hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8cd8-382">If you don't need toouse hello VM in hello evening or weekends, you can just shut down hello VM from hello [Azure portal](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="c8cd8-383">Poplatky za výpočty dojít, pokud používáte pouze hello tlačítko vypnutí operačního systému na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-383">You incur compute charges if you use just hello Operating system shutdown button on hello VM.</span></span>  
> 
> 

<span data-ttu-id="c8cd8-384">Pokud potřebujete toohandle některé rozsáhlé analýzy a potřebovat větší kapacitu procesoru nebo paměti a disku můžete najít velké volba velikostí virtuálních počítačů z hlediska jader procesoru, kapacita paměti a disku typy (včetně jednotek SSD), které splňují vaše výpočetní a rozpočtových potřebám.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-384">If you need toohandle some large-scale analysis and need more CPU and/or memory and/or disk capacity you can find a large choice of VM sizes in terms of CPU cores, memory capacity and disk types (including Solid state drives) that meet your compute and budgetary needs.</span></span> <span data-ttu-id="c8cd8-385">Hello úplný seznam virtuálních počítačů spolu s jejich hodinové výpočetní ceny je k dispozici na hello [ceny virtuálních počítačů Azure](https://azure.microsoft.com/pricing/details/virtual-machines/) stránky.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-385">hello full list of VMs along with their hourly compute pricing is available on hello [Azure Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) page.</span></span>

<span data-ttu-id="c8cd8-386">Podobně pokud snižuje potřeba kapacity zpracování virtuálního počítače (například: přesunut tooa hlavní úloh Hadoop nebo clusteru Spark), je možné škálovat dolů hello clusteru z hello [portál Azure](https://portal.azure.com) a toohello nastavení virtuálního počítače instance.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-386">Similarly, if your need for VM processing capacity reduces (for example: you moved a major workload tooa Hadoop or a Spark cluster), you can scale down hello cluster from hello [Azure portal](https://portal.azure.com) and going toohello settings of your VM instance.</span></span> <span data-ttu-id="c8cd8-387">Zde je snímek.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-387">Here is a screenshot.</span></span>

![Nastavení instance virtuálních počítačů](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a><span data-ttu-id="c8cd8-389">10. Nainstalujte další nástroje na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="c8cd8-389">10. Install additional tools on your virtual machine</span></span>
<span data-ttu-id="c8cd8-390">Budeme mít zabalené několik nástrojů, které se domníváme, jsou možné tooaddress řadu hello běžným potřebám analýzy dat a který by měl šetří čas vyhnout nutnosti tooinstall a konfigurace vašeho prostředí po jednom a ušetřit peníze platebním pouze pro prostředky, které použití.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-390">We have packaged several tools that we believe are able tooaddress many of hello common data analytics needs and that should save you time by avoiding having tooinstall and configure your environments one by one and save you money by paying only for resources that you use.</span></span>

<span data-ttu-id="c8cd8-391">Můžete využít další Azure data a analytické služby profilovaným v tooenhance Tento článek prostředí analýzy.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-391">You can leverage other Azure data and analytics services profiled in this article tooenhance your analytics environment.</span></span> <span data-ttu-id="c8cd8-392">Chápeme, že v některých případech může vyžadovat vašim potřebám dalších nástrojů, včetně některá vlastnické nástroje třetích stran.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-392">We understand that in some cases your needs may require additional tools, including some proprietary third-party tools.</span></span> <span data-ttu-id="c8cd8-393">Máte plný přístup správce na hello virtuálního počítače tooinstall nové nástroje, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-393">You have full administrative access on hello virtual machine tooinstall new tools you need.</span></span> <span data-ttu-id="c8cd8-394">Můžete taky nainstalovat další balíčky Python a R, která nejsou předem nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-394">You can also install additional packages in Python and R that are not pre-installed.</span></span> <span data-ttu-id="c8cd8-395">Pro jazyk Python můžete použít buď ```conda``` nebo ```pip```.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-395">For Python you can use either ```conda``` or ```pip```.</span></span> <span data-ttu-id="c8cd8-396">R můžete použít hello ```install.packages()``` v hello R konzole nebo pomocí hello IDE a vyberte "**balíčky** -> **instalovat balíčky...** ".</span><span class="sxs-lookup"><span data-stu-id="c8cd8-396">For R you can use hello ```install.packages()``` in hello R console or use hello IDE and choose "**Packages** -> **Install Packages...**".</span></span>

## <a name="summary"></a><span data-ttu-id="c8cd8-397">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c8cd8-397">Summary</span></span>
<span data-ttu-id="c8cd8-398">To jsou jen některé hello věcí, které můžete provést jak na hello Microsoft Data vědecké účely virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-398">These are just some of hello things you can do on hello Microsoft Data Science Virtual Machine.</span></span> <span data-ttu-id="c8cd8-399">Existuje mnoho dalších věcí, které můžete provést toomake ho prostředí efektivní analýzu.</span><span class="sxs-lookup"><span data-stu-id="c8cd8-399">There are many more things you can do toomake it an effective analytics environment.</span></span>

