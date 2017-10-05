---
title: "10 způsobů, jak na virtuální počítač vědecké účely dat | Microsoft Docs"
description: "Proveďte různé zkoumání dat a modelování úloh na vědecké zpracování dat virtuálního počítače."
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
ms.openlocfilehash: 45af1cd3a05b483429d2307659f1882ef28921f6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a><span data-ttu-id="4ed96-103">Deset věcí, které můžete provádět na virtuálních počítačích pro vědecké zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="4ed96-103">Ten things you can do on the Data science Virtual Machine</span></span>
<span data-ttu-id="4ed96-104">Virtuální počítač pro vědecké účely dat společnosti Microsoft (DSVM) je výkonný data vědecké účely vývojového prostředí, které umožňuje provádět různé úlohy zkoumání a modelování data.</span><span class="sxs-lookup"><span data-stu-id="4ed96-104">The Microsoft Data Science Virtual Machine (DSVM) is a powerful data science development environment that enables you to perform various data exploration and modeling tasks.</span></span> <span data-ttu-id="4ed96-105">Prostředí obsahuje již vytvořené a připojené několik oblíbených data analýzy nástroje, které usnadňují rychle začít používat analýzy pro místní, Cloudová nebo hybridní nasazení.</span><span class="sxs-lookup"><span data-stu-id="4ed96-105">The environment comes already built and bundled with several popular data analytics tools that make it easy to get started quickly with your analysis for On-premises, Cloud or hybrid deployments.</span></span> <span data-ttu-id="4ed96-106">DSVM úzce spolupracuje s řadou služeb Azure a se bude moct číst a zpracovávat data, která je již uložen v Azure, Azure SQL Data Warehouse, Azure Storage a Azure Data Lake nebo v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4ed96-106">The DSVM works closely with many Azure services and is able to read and process data that is already stored on Azure, in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage, or in Azure Cosmos DB.</span></span> <span data-ttu-id="4ed96-107">Můžete využít i jiné analytics nástroje, například Azure Machine Learning a Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4ed96-107">It can also leverage other analytics tools such as Azure Machine Learning and Azure Data Factory.</span></span>

<span data-ttu-id="4ed96-108">V tomto článku jsme vás provede procesem použití vašeho DSVM k provádění různých úloh datové vědy a komunikovat s jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="4ed96-108">In this article we walk you through how to use your DSVM to perform various data science tasks and interact with other Azure services.</span></span> <span data-ttu-id="4ed96-109">Tady jsou některé z akcí, které můžete provést jak na DSVM:</span><span class="sxs-lookup"><span data-stu-id="4ed96-109">Here are some of the things you can do on the DSVM:</span></span>

1. <span data-ttu-id="4ed96-110">Prozkoumejte data a vyvíjet modely místně na DSVM použití serveru Microsoft R, Python</span><span class="sxs-lookup"><span data-stu-id="4ed96-110">Explore data and develop models locally on the DSVM using Microsoft R Server, Python</span></span>
2. <span data-ttu-id="4ed96-111">Pomocí poznámkového bloku Jupyter a experimentovat s daty v prohlížeči pomocí R Python 2, Python 3, Microsoft enterprise připravené verze R určená pro škálovatelnost a výkon</span><span class="sxs-lookup"><span data-stu-id="4ed96-111">Use a Jupyter notebook to experiment with your data on a browser using Python 2, Python 3, Microsoft R an enterprise ready version of R designed for scalability and performance</span></span>
3. <span data-ttu-id="4ed96-112">Zprovoznit modely vytvářeny pomocí R a Python v Azure Machine Learning, tak klientské aplikace mají přístup k vaší modelů pomocí jednoduchého webového rozhraní služby</span><span class="sxs-lookup"><span data-stu-id="4ed96-112">Operationalize models built using R and Python on Azure Machine Learning so client applications can access your models using a simple web services interface</span></span>
4. <span data-ttu-id="4ed96-113">Správa prostředků Azure pomocí portálu Azure nebo prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="4ed96-113">Administer your Azure resources using  Azure portal or Powershell</span></span>
5. <span data-ttu-id="4ed96-114">Rozšíření prostor úložiště a sdílet rozsáhlých datových sad / kódu napříč celý tým vytvoření Azure File storage jako připojit jednotku ve vašem DSVM</span><span class="sxs-lookup"><span data-stu-id="4ed96-114">Extend your storage space and share large-scale datasets / code across your whole team by creating an Azure File storage as a mountable drive on your DSVM</span></span>
6. <span data-ttu-id="4ed96-115">Sdílet kódu se svým týmem pomocí Githubu a přístup k vaší úložiště pomocí předem nainstalovaná klienti Git - Git Bash a Git grafickým uživatelským rozhraním.</span><span class="sxs-lookup"><span data-stu-id="4ed96-115">Share code with your team using GitHub and access your repository using the pre-installed Git clients - Git Bash, Git GUI.</span></span>
7. <span data-ttu-id="4ed96-116">Přístup k různým Azure data a analýzy službám jako úložiště objektů blob v Azure, Azure Data Lake, Azure HDInsight (Hadoop), Cosmos databáze Azure, Azure SQL Data Warehouse & databáze</span><span class="sxs-lookup"><span data-stu-id="4ed96-116">Access various Azure data and analytics services like Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & databases</span></span>
8. <span data-ttu-id="4ed96-117">Vytvářejte sestavy a řídicí panel pomocí Power BI Desktop předinstalované na DSVM a nasadit je do cloudu</span><span class="sxs-lookup"><span data-stu-id="4ed96-117">Build reports and dashboard using the Power BI Desktop pre-installed on the DSVM and deploy them on the cloud</span></span>
9. <span data-ttu-id="4ed96-118">Dynamické škálování vašeho DSVM potřebám vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="4ed96-118">Dynamically scale your DSVM to meet your project needs</span></span>
10. <span data-ttu-id="4ed96-119">Nainstalujte další nástroje na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="4ed96-119">Install additional tools on your virtual machine</span></span>   

> [!NOTE]
> <span data-ttu-id="4ed96-120">Další využití poplatky pro mnoho dalších datové úložiště a analýzy služby uvedené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4ed96-120">Additional usage charges apply for many of the additional data storage and analytics services listed in this article.</span></span> <span data-ttu-id="4ed96-121">Podrobnosti najdete [Azure – ceny](https://azure.microsoft.com/pricing/) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4ed96-121">Please refer to the [Azure Pricing](https://azure.microsoft.com/pricing/) page for details.</span></span>
> 
> 

<span data-ttu-id="4ed96-122">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="4ed96-122">**Prerequisites**</span></span>

* <span data-ttu-id="4ed96-123">Budete potřebovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4ed96-123">You need an Azure subscription.</span></span> <span data-ttu-id="4ed96-124">Můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4ed96-124">You can sign up for a free trial [here](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="4ed96-125">Pokyny pro zřízení virtuálního počítače vědecké účely dat na portálu Azure jsou k dispozici v [vytvoření virtuálního počítače](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="4ed96-125">Instructions for provisioning a Data Science Virtual Machine on the Azure portal are available at [Creating a virtual machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span></span>

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a><span data-ttu-id="4ed96-126">1. Prozkoumejte data a vývoj modelů pomocí serveru Microsoft R nebo Python</span><span class="sxs-lookup"><span data-stu-id="4ed96-126">1. Explore data and develop models using Microsoft R Server or Python</span></span>
<span data-ttu-id="4ed96-127">Jazyky jako R a Python můžete provádět analýzy vaše data na DSVM vpravo.</span><span class="sxs-lookup"><span data-stu-id="4ed96-127">You can use languages like R and Python to do your data analytics right on the DSVM.</span></span>

<span data-ttu-id="4ed96-128">Pro R můžete použít IDE názvem "Revolution R Enterprise 8.0", který se nachází v nabídce start nebo plochy.</span><span class="sxs-lookup"><span data-stu-id="4ed96-128">For R, you can use an IDE called "Revolution R Enterprise 8.0" that can be found on the start menu or the desktop.</span></span> <span data-ttu-id="4ed96-129">Společnost Microsoft poskytuje další knihovny nad otevřený zdroj/CRAN-R umožnit škálovatelný analýzy a schopnost analyzovat data větší než velikost paměti, který je povolená díky paralelní bloku analysis.</span><span class="sxs-lookup"><span data-stu-id="4ed96-129">Microsoft has provided additional libraries on top of the Open source/CRAN-R to enable scalable analytics and the ability to analyze data larger than the memory size allowed by doing parallel chunked analysis.</span></span> <span data-ttu-id="4ed96-130">Můžete taky nainstalovat IDE R z vaše volba jako [Rstudia](https://www.rstudio.com/products/rstudio-desktop/).</span><span class="sxs-lookup"><span data-stu-id="4ed96-130">You can also install an R IDE of your choice like [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span></span>

<span data-ttu-id="4ed96-131">Pro jazyk Python můžete použít rozhraní IDE, jako Visual Studio Community Edition, který má nástroje Python Tools pro Visual Studio (PTVS) rozšíření předinstalován.</span><span class="sxs-lookup"><span data-stu-id="4ed96-131">For Python, you can use an IDE like Visual Studio Community Edition which has the Python Tools for Visual Studio (PTVS) extension pre-installed.</span></span> <span data-ttu-id="4ed96-132">Ve výchozím nastavení je nakonfigurován pouze základní Python 2.7 na PTVS (bez jakékoli knihovnu analytics jako SciKit, Pandas).</span><span class="sxs-lookup"><span data-stu-id="4ed96-132">By default, only a basic Python 2.7 is configured on PTVS (without any analytics library like SciKit, Pandas).</span></span> <span data-ttu-id="4ed96-133">Chcete-li povolit Anaconda Python 2.7 a 3.5, musíte udělat následující:</span><span class="sxs-lookup"><span data-stu-id="4ed96-133">In order to enable Anaconda Python 2.7 and 3.5, you need to do the following:</span></span>

* <span data-ttu-id="4ed96-134">Vytvoření vlastního prostředí pro každou verzi přechodem na **nástroje** -> **Python Tools** -> **prostředí Python** a potom kliknutím na možnost " **+ Vlastní**"v aplikaci Visual Studio 2015 Community Edition</span><span class="sxs-lookup"><span data-stu-id="4ed96-134">Create custom environments for each version by navigating to **Tools** -> **Python Tools** -> **Python Environments** and then clicking "**+ Custom**" in the Visual Studio 2015 Community Edition</span></span>
* <span data-ttu-id="4ed96-135">Zadejte popis a nastavte prostředí cesty předponu jako *c:\anaconda* Anaconda Python 2.7 nebo *c:\anaconda\envs\py35* pro Anaconda Python 3.5</span><span class="sxs-lookup"><span data-stu-id="4ed96-135">Give a description and set the environment prefix paths as *c:\anaconda* for Anaconda Python 2.7 OR *c:\anaconda\envs\py35* for Anaconda Python 3.5</span></span>
* <span data-ttu-id="4ed96-136">Klikněte na tlačítko **automatické rozpoznání** a potom **použít** uložit prostředí.</span><span class="sxs-lookup"><span data-stu-id="4ed96-136">Click **Auto Detect** and then **Apply** to save the environment.</span></span>

<span data-ttu-id="4ed96-137">Zde je, jak nastavení vlastní prostředí vypadá v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed96-137">Here is what the custom environment setup looks like in Visual Studio.</span></span>

![Instalační program PTVS](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

<span data-ttu-id="4ed96-139">Najdete v článku [dokumentaci k těmto nástrojům](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) další podrobnosti o tom, jak vytvořit prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="4ed96-139">See the [PTVS documentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) for additional details on how to create Python Environments.</span></span>

<span data-ttu-id="4ed96-140">Nyní jsou nastavení k vytvoření nového projektu Python.</span><span class="sxs-lookup"><span data-stu-id="4ed96-140">Now you are set up to create a new Python project.</span></span> <span data-ttu-id="4ed96-141">Přejděte na **soubor** -> **nový** -> **projektu** -> **Python** a vyberte typ Aplikace Python, které vytváříte.</span><span class="sxs-lookup"><span data-stu-id="4ed96-141">Navigate to **File** -> **New** -> **Project** -> **Python** and select the type of Python application you are building.</span></span> <span data-ttu-id="4ed96-142">Prostředí Python pro aktuální projekt můžete nastavit na požadovanou verzi (Anaconda 2.7 nebo 3.5): klikněte pravým tlačítkem myši **prostředí Python**, vyberte **prostředí Python přidat nebo odebrat**a potom vyberte požadované prostředí, které chcete přidružit k projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-142">You can set the Python environment for the current project to the desired version (Anaconda 2.7 or 3.5): right-click the **Python environment**, select **Add/Remove Python Environments**, and then select the desired environment to associate with the project.</span></span> <span data-ttu-id="4ed96-143">Můžete najít další informace o práci s PTVS na produktu [dokumentace](https://github.com/Microsoft/PTVS/wiki) stránky.</span><span class="sxs-lookup"><span data-stu-id="4ed96-143">You can find more information about working with PTVS on the product [documentation](https://github.com/Microsoft/PTVS/wiki) page.</span></span>

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a><span data-ttu-id="4ed96-144">2. Pomocí poznámkového bloku Jupyter a prozkoumejte modelování svá data pomocí Python nebo R</span><span class="sxs-lookup"><span data-stu-id="4ed96-144">2. Using a Jupyter Notebook to explore and model your data with Python or R</span></span>
<span data-ttu-id="4ed96-145">Poznámkového bloku Jupyter je výkonný prostředí, který poskytuje založené na prohlížeči "IDE" pro zkoumání dat a modelování.</span><span class="sxs-lookup"><span data-stu-id="4ed96-145">The Jupyter Notebook is a powerful environment that provides a browser-based "IDE" for data exploration and modeling.</span></span> <span data-ttu-id="4ed96-146">Můžete použít Python 2, Python 3 nebo R (Open Source a serveru Microsoft R) v poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4ed96-146">You can use Python 2, Python 3 or R (both Open Source and the Microsoft R Server) in a Jupyter Notebook.</span></span>

<span data-ttu-id="4ed96-147">Spusťte Poznámkový blok Jupyter kliknutím na ikonu nabídky start / ikony na ploše s názvem **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="4ed96-147">To launch the Jupyter Notebook click on the start menu icon / desktop icon titled **Jupyter Notebook**.</span></span> <span data-ttu-id="4ed96-148">Na DSVM můžete také vyhledat "https://localhost:9999 /" pro přístup k Jupiter poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="4ed96-148">On the DSVM you can also browse to "https://localhost:9999/" to access the Jupiter Notebook.</span></span> <span data-ttu-id="4ed96-149">Pokud budete vyzváni k zadání hesla, použijte pokyny uvedené v ***jak vytvořit silné heslo na serveru poznámkového bloku Jupyter*** části [zřízení virtuálního počítače Microsoft Data vědecké účely](machine-learning-data-science-provision-vm.md) téma vytvořit silné heslo pro přístup k poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4ed96-149">If it prompts you for a password, use instructions provided in the ***How to create a strong password on the Jupyter notebook server*** section of the [Provision the Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) topic to create a strong password to access the Jupyter notebook.</span></span> 

<span data-ttu-id="4ed96-150">Po spuštění poznámkového bloku, měli byste vidět adresář, který obsahuje pár příklad poznámkových bloků, které jsou předem zabalené do DSVM.</span><span class="sxs-lookup"><span data-stu-id="4ed96-150">Once you have opened the notebook, you should see a directory that contains a few example notebooks that are pre-packaged into the DSVM.</span></span> <span data-ttu-id="4ed96-151">Nyní můžete:</span><span class="sxs-lookup"><span data-stu-id="4ed96-151">Now you can:</span></span>

* <span data-ttu-id="4ed96-152">klikněte v poznámkovém bloku zobrazit kód.</span><span class="sxs-lookup"><span data-stu-id="4ed96-152">click on the notebook to see the code.</span></span>
* <span data-ttu-id="4ed96-153">Spusťte jednotlivých buněk tak, že stisknete **zadejte SHIFT**.</span><span class="sxs-lookup"><span data-stu-id="4ed96-153">execute each cell by pressing **SHIFT-ENTER**.</span></span>
* <span data-ttu-id="4ed96-154">Kliknutím na spustit celý poznámkový blok **buňky** -> **spustit**</span><span class="sxs-lookup"><span data-stu-id="4ed96-154">run the entire notebook by clicking on **Cell** -> **Run**</span></span>
* <span data-ttu-id="4ed96-155">Kliknutím na ikonu Jupyter (levém horním rohu) a potom kliknutím na vytvořit nový poznámkový blok **nový** tlačítko vpravo a pak vyberete poznámkového bloku jazyka (také označované jako jádra).</span><span class="sxs-lookup"><span data-stu-id="4ed96-155">create a new notebook by clicking on the Jupyter Icon (left top corner) and then clicking **New** button on the right and then choosing the notebook language (also known as kernels).</span></span>   

> [!NOTE]
> <span data-ttu-id="4ed96-156">Momentálně podporujeme Python 2.7, Python 3.5 a R. Jádro R podporuje programování v Open source R jak podniku škálovatelné R Server společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ed96-156">Currently we support Python 2.7, Python 3.5 and R. The R kernel supports programming in both Open source R as well as the enterprise scalable Microsoft R Server.</span></span>   
> 
> 

<span data-ttu-id="4ed96-157">Jakmile jsou v poznámkovém bloku, které můžete prozkoumat vaše data, sestavení modelu, otestování modelu pomocí vybraného knihovny.</span><span class="sxs-lookup"><span data-stu-id="4ed96-157">Once you are in the notebook you can explore your data, build the model, test the model using your choice of libraries.</span></span>

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a><span data-ttu-id="4ed96-158">3. Vytvářet modely R nebo Python a Operationalize jejich používání pomocí Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4ed96-158">3. Build models using R or Python and Operationalize them using Azure Machine Learning</span></span>
<span data-ttu-id="4ed96-159">Jakmile máte vytvořené a ověřit váš model dalším krokem je obvykle k nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4ed96-159">Once you have built and validated your model the next step is usually to deploy it into production.</span></span> <span data-ttu-id="4ed96-160">To umožňuje aplikacím vyvolání předpovědi modelu v reálném čase, nebo na základě režimu dávky vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="4ed96-160">This allows your client applications to invoke the model predictions on a real time or on a batch mode basis.</span></span> <span data-ttu-id="4ed96-161">Azure Machine Learning poskytuje mechanismus pro zprovoznit model součástí R nebo Python.</span><span class="sxs-lookup"><span data-stu-id="4ed96-161">Azure Machine Learning provides a mechanism to operationalize a model built in either R or Python.</span></span>

<span data-ttu-id="4ed96-162">Když jste zprovoznit modelu v Azure Machine Learning, je vystaven webové služby, která umožňuje klientům volání REST, které předejte vstupní parametry a přijímat předpovědi jako výstup z modelu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-162">When you operationalize your model in Azure Machine Learning, a web service is exposed that allows clients to make REST calls that pass in input parameters and receive predictions from the model as outputs.</span></span>   

> [!NOTE]
> <span data-ttu-id="4ed96-163">Pokud jste ještě nezaregistrovali jste se pro Azure Machine Learning, můžete získat volného prostoru nebo standardní pracovní prostor, navštivte stránky [Azure Machine Learning Studio](https://studio.azureml.net/) domovské stránky a kliknutím na "Začínáme".</span><span class="sxs-lookup"><span data-stu-id="4ed96-163">If you have not yet signed up for Azure Machine Learning, you can obtain a free workspace or a standard workspace by visiting the [Azure Machine Learning Studio](https://studio.azureml.net/) home page and clicking on "Get Started".</span></span>   
> 
> 

### <a name="build-and-operationalize-python-models"></a><span data-ttu-id="4ed96-164">Sestavení a zprovoznit Python modely</span><span class="sxs-lookup"><span data-stu-id="4ed96-164">Build and Operationalize Python models</span></span>
<span data-ttu-id="4ed96-165">Zde je fragment kódu vyvinuté v poznámkového bloku Jupyter Python, který sestaví jednoduchého modelu pomocí SciKit další knihovny.</span><span class="sxs-lookup"><span data-stu-id="4ed96-165">Here is a snippet of code developed in a Python Jupyter Notebook that builds a simple model using the SciKit-learn library.</span></span>

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

<span data-ttu-id="4ed96-166">Metoda použít k nasazení do Azure Machine Learning zabalí předpovědi modelu do funkce modely python a upraví s atributy poskytované předem nainstalovaná knihovna python Azure Machine Learning, které označují vaší Azure Machine Learning ID pracovního prostoru, klíč rozhraní API a parametry vstupní a zpět.</span><span class="sxs-lookup"><span data-stu-id="4ed96-166">The method used to deploy your python models to Azure Machine Learning wraps the prediction of the model into a function and decorates it with attributes provided by the pre-installed Azure Machine Learning python library that denote your Azure Machine Learning workspace ID, API Key, and the input and return parameters.</span></span>  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

<span data-ttu-id="4ed96-167">Klienta můžete nyní volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="4ed96-167">A client can now make calls to the web service.</span></span> <span data-ttu-id="4ed96-168">Existují obálky pohodlí, které vytvořit požadavky REST API.</span><span class="sxs-lookup"><span data-stu-id="4ed96-168">There are convenience wrappers that construct the REST API requests.</span></span> <span data-ttu-id="4ed96-169">Tady je ukázkový kód využívají webové služby.</span><span class="sxs-lookup"><span data-stu-id="4ed96-169">Here is a sample code to consume the web service.</span></span>

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> <span data-ttu-id="4ed96-170">Knihovna Azure Machine Learning je podporována pouze na Python 2.7 aktuálně.</span><span class="sxs-lookup"><span data-stu-id="4ed96-170">The Azure Machine Learning library is only supported on Python 2.7 currently.</span></span>   
> 
> 

### <a name="build-and-operationalize-r-models"></a><span data-ttu-id="4ed96-171">Sestavení a zprovoznit R modely</span><span class="sxs-lookup"><span data-stu-id="4ed96-171">Build and Operationalize R models</span></span>
<span data-ttu-id="4ed96-172">Můžete nasadit R modely vytvořené na datové vědě virtuální počítač nebo jinde do Azure Machine Learning způsobem, který je podobný tomu pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="4ed96-172">You can deploy R models built on the Data Science Virtual Machine or elsewhere onto Azure Machine Learning in a manner that is similar to how it is done for Python.</span></span> <span data-ttu-id="4ed96-173">Jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="4ed96-173">Her are the steps:</span></span>

* <span data-ttu-id="4ed96-174">Vytvořte soubor settings.json zajistit pracovního prostoru ID a ověření tokenu, jak znázorňuje následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-174">create a settings.json file to provide your workspace ID and auth token as shown in the following code sample.</span></span>
* <span data-ttu-id="4ed96-175">Obálka pro modelu předpovědi funkce zápisu</span><span class="sxs-lookup"><span data-stu-id="4ed96-175">write a wrapper for the model's predict function.</span></span>
* <span data-ttu-id="4ed96-176">volání ```publishWebService``` v knihovně Azure Machine Learning předávat obálku funkce.</span><span class="sxs-lookup"><span data-stu-id="4ed96-176">call ```publishWebService``` in the Azure Machine Learning library to pass in the function wrapper.</span></span>  

<span data-ttu-id="4ed96-177">Zde je postup a kód fragmenty kódu, které slouží k nastavení, vytvářet, publikovat a využívat model jako webovou službu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4ed96-177">Here is the procedure and code snippets that can be used to set up, build, publish, and consume a model as a web service in Azure Machine Learning.</span></span>

#### <a name="setup"></a><span data-ttu-id="4ed96-178">Nastavení</span><span class="sxs-lookup"><span data-stu-id="4ed96-178">Setup</span></span>
1. <span data-ttu-id="4ed96-179">Instalovat balíček Machine Learning R zadáním ```install.packages("AzureML")``` v Revolution R Enterprise 8.0 IDE nebo vaše R IDE.</span><span class="sxs-lookup"><span data-stu-id="4ed96-179">Install the Machine Learning R package by typing ```install.packages("AzureML")``` in Revolution R Enterprise 8.0 IDE or your R IDE.</span></span>
2. <span data-ttu-id="4ed96-180">Stáhněte si RTools z [zde](https://cran.r-project.org/bin/windows/Rtools/).</span><span class="sxs-lookup"><span data-stu-id="4ed96-180">Download RTools from [here](https://cran.r-project.org/bin/windows/Rtools/).</span></span> <span data-ttu-id="4ed96-181">Je nutné zip nástroj v cestě (a s názvem zip.exe) pro zprovoznění váš balíček R do Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4ed96-181">You need the zip utility in the path (and named zip.exe) to operationalize your R package into Machine Learning.</span></span>
3. <span data-ttu-id="4ed96-182">Vytvořte soubor settings.json pod adresář s názvem ```.azureml``` pod domovského adresáře a zadejte parametry z pracovního prostoru Azure Machine Learning:</span><span class="sxs-lookup"><span data-stu-id="4ed96-182">Create a settings.json file under a directory called ```.azureml``` under your home directory and enter the parameters from your Azure Machine Learning workspace:</span></span>

<span data-ttu-id="4ed96-183">Settings.JSON strukturu souborů:</span><span class="sxs-lookup"><span data-stu-id="4ed96-183">settings.json File structure:</span></span>

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a><span data-ttu-id="4ed96-184">Vytvoření modelu v R a publikujete ho v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4ed96-184">Build a model in R and publish it in Azure Machine Learning</span></span>
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a><span data-ttu-id="4ed96-185">Využívat model nasadit v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4ed96-185">Consume the model deployed in Azure Machine Learning</span></span>
<span data-ttu-id="4ed96-186">Využívat modelu z klientské aplikace, budeme používat knihovny Azure Machine Learning k vyhledání publikovanou webovou službu pomocí názvu `services` volání rozhraní API k určení koncový bod.</span><span class="sxs-lookup"><span data-stu-id="4ed96-186">To consume the model from a client application, we use the Azure Machine Learning library to look up the published web service by name using the `services` API call to determine the endpoint.</span></span> <span data-ttu-id="4ed96-187">Pak stačí zavolat `consume` funkce a předejte v data snímku na předpovědět.</span><span class="sxs-lookup"><span data-stu-id="4ed96-187">Then you just call the `consume` function and pass in the data frame to be predicted.</span></span>
<span data-ttu-id="4ed96-188">Následující kód slouží k využívat model publikován jako webové služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4ed96-188">The following code is used to consume the model published as an Azure Machine Learning web service.</span></span>

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

<span data-ttu-id="4ed96-189">Další informace o knihovně Azure Machine Learning R naleznete [zde](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span><span class="sxs-lookup"><span data-stu-id="4ed96-189">More information about the Azure Machine Learning R library can be found [here](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span></span>

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a><span data-ttu-id="4ed96-190">4. Správa prostředků Azure pomocí portálu Azure nebo prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="4ed96-190">4. Administer your Azure resources using Azure portal or Powershell</span></span>
<span data-ttu-id="4ed96-191">DSVM nejen je možné sestavit vaše řešení analytics místně na virtuálním počítači, ale také umožňuje přístup ke službám v cloudu Azure společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4ed96-191">The DSVM not only allows you to build your analytics solution locally on the virtual machine, but also allows you to access services on Microsoft's Azure cloud.</span></span> <span data-ttu-id="4ed96-192">Azure poskytuje několik výpočty, úložiště, služby analýzy dat a jiných služeb, které můžete spravovat a přistupovat z vašeho DSVM.</span><span class="sxs-lookup"><span data-stu-id="4ed96-192">Azure provides several compute, storage, data analytics services and other services that you can administer and access from your DSVM.</span></span>

<span data-ttu-id="4ed96-193">Ke správě předplatného a cloudových prostředků Azure můžete použít prohlížeč a přejděte [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4ed96-193">To administer your Azure subscription and cloud resources you can use your browser and point to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4ed96-194">Prostředí Azure Powershell můžete použít také ke správě vašeho předplatného Azure a prostředky pomocí skriptu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-194">You can also use Azure Powershell to administer your Azure subscription and resources via a script.</span></span>
<span data-ttu-id="4ed96-195">Prostředí Azure Powershell můžete spustit pomocí zástupce na ploše nebo z nabídky start s názvem "Microsoft Azure Powershell".</span><span class="sxs-lookup"><span data-stu-id="4ed96-195">You can run Azure Powershell from a shortcut on the desktop or from the start menu titled "Microsoft Azure Powershell".</span></span> <span data-ttu-id="4ed96-196">Odkazovat na [dokumentace k Microsoft Azure Powershell](../powershell-azure-resource-manager.md) Další informace o tom, jak můžete spravovat vaše předplatné Azure a prostředkům pomocí skriptů prostředí Windows Powershell.</span><span class="sxs-lookup"><span data-stu-id="4ed96-196">Refer to [Microsoft Azure Powershell documentation](../powershell-azure-resource-manager.md) for more information on how you can administer your Azure subscription and resources using Windows Powershell scripts.</span></span>

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a><span data-ttu-id="4ed96-197">5. Rozšíření prostor úložiště pomocí systému sdílený soubor</span><span class="sxs-lookup"><span data-stu-id="4ed96-197">5. Extend your storage space with a shared file system</span></span>
<span data-ttu-id="4ed96-198">Datových vědců můžete sdílet rozsáhlých datových sad, kódu nebo jiným prostředkům v rámci týmu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-198">Data scientists can share large datasets, code or other resources within the team.</span></span> <span data-ttu-id="4ed96-199">DSVM samotné má přibližně 70GB volného místa.</span><span class="sxs-lookup"><span data-stu-id="4ed96-199">The DSVM itself has about 70GB of space available.</span></span> <span data-ttu-id="4ed96-200">Rozšířit úložiště, můžete použít službu souboru Azure a buď ji připojit na DSVM nebo přístup přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="4ed96-200">To extend your storage, you can use the Azure File Service and either mount it on the DSVM or access it via a REST API.</span></span>   

> [!NOTE]
> <span data-ttu-id="4ed96-201">Maximální místo sdílené složky souboru služby Azure je 5TB a maximální velikost jednotlivých souborů je 1TB.</span><span class="sxs-lookup"><span data-stu-id="4ed96-201">The maximum space of the Azure File Service share is 5TB and individual file size limit is 1TB.</span></span>   
> 
> 

<span data-ttu-id="4ed96-202">Prostředí Azure Powershell můžete použít k vytvoření Azure služba souborů složky.</span><span class="sxs-lookup"><span data-stu-id="4ed96-202">You can use Azure Powershell to create an Azure File Service share.</span></span> <span data-ttu-id="4ed96-203">Zde je skript, který chcete spustit v prostředí Azure PowerShell k vytvoření služby Azure sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4ed96-203">Here is the script to run under Azure PowerShell to create an Azure File service share.</span></span>

    # Authenticate to Azure.
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
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


<span data-ttu-id="4ed96-204">Teď, když vytvoříte sdílenou složku Azure, můžete ji připojit žádné virtuální počítač v Azure.</span><span class="sxs-lookup"><span data-stu-id="4ed96-204">Now that you have created an Azure file share, you can mount it in any virtual machine in Azure.</span></span> <span data-ttu-id="4ed96-205">Důrazně doporučujeme, aby virtuální počítač ve stejném datovém centru Azure jako účet úložiště, náklady na přenos latence a data.</span><span class="sxs-lookup"><span data-stu-id="4ed96-205">It is highly recommended that the VM is in same Azure data center as the storage account to avoid latency and data transfer charges.</span></span> <span data-ttu-id="4ed96-206">Tady jsou příkazy připojit jednotku na DSVM, který můžete spustit v prostředí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="4ed96-206">Here are the commands to mount the drive on the DSVM that you can run on Azure Powershell.</span></span>

    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


<span data-ttu-id="4ed96-207">Nyní můžete zobrazit tuto jednotku stejně jako všechny normální jednotky ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="4ed96-207">Now you can access this drive as you would any normal drive on the VM.</span></span>

## <a name="6-share-code-with-your-team-using-github"></a><span data-ttu-id="4ed96-208">6. Sdílet kódu se svým týmem pomocí Githubu</span><span class="sxs-lookup"><span data-stu-id="4ed96-208">6. Share code with your team using GitHub</span></span>
<span data-ttu-id="4ed96-209">GitHub je úložiště kódu, kde můžete najít spoustu ukázkový kód a zdroje k různým nástrojům pomocí různých technologií, které jsou sdíleny komunity vývojářů.</span><span class="sxs-lookup"><span data-stu-id="4ed96-209">GitHub is a code repository where you can find a lot of sample code and sources for different tools using various technologies shared by the developer community.</span></span> <span data-ttu-id="4ed96-210">Git jako technologie používá ke sledování a uložit verzí soubory kódu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-210">It uses Git as the technology to track and store versions of the code files.</span></span> <span data-ttu-id="4ed96-211">GitHub je také platforma, kde můžete vytvořit vlastní úložiště ukládání sdíleného kódu vašeho týmu a dokumentace, implementovat verzí a také ovládat, kteří mají přístup k zobrazení a přispívat kódu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-211">GitHub is also a platform where you can create your own repository to store your team's shared code and documentation, implement version control and also control who have access to view and contribute code.</span></span> <span data-ttu-id="4ed96-212">Navštivte [stránky nápovědy Githubu](https://help.github.com/) pro další informace o použití Git.</span><span class="sxs-lookup"><span data-stu-id="4ed96-212">Please visit the [GitHub help pages](https://help.github.com/) for more information on using Git.</span></span> <span data-ttu-id="4ed96-213">GitHub můžete použít jako jeden ze způsobů, jak se svým týmem spolupracovat, použít kód vyvinuté komunitou a přispívat kódu zpět do komunity.</span><span class="sxs-lookup"><span data-stu-id="4ed96-213">You can use GitHub as one of the ways to collaborate with your team, use code developed by the community and contribute code back to the community.</span></span>

<span data-ttu-id="4ed96-214">DSVM už dodává s klientskými nástroji načíst na obou příkazového řádku jako dobře grafického uživatelského rozhraní pro přístup k úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="4ed96-214">The DSVM already comes loaded with client tools on both command-line as well GUI to access GitHub repository.</span></span> <span data-ttu-id="4ed96-215">Nástroj příkazového řádku pro práci s Gitu a Githubu se nazývá Git Bash.</span><span class="sxs-lookup"><span data-stu-id="4ed96-215">The command-line tool to work with Git and GitHub is called Git Bash.</span></span> <span data-ttu-id="4ed96-216">Na DSVM nainstalovanou sadu Visual Studio má rozšíření Git.</span><span class="sxs-lookup"><span data-stu-id="4ed96-216">Visual Studio installed on the DSVM has the Git extensions.</span></span> <span data-ttu-id="4ed96-217">Můžete najít ikony spuštění těchto nástrojů v nabídce start a na ploše.</span><span class="sxs-lookup"><span data-stu-id="4ed96-217">You can find start-up icons for these tools on the start menu and the desktop.</span></span>

<span data-ttu-id="4ed96-218">Ke stažení kód z úložiště Githubu použijete ```git clone``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4ed96-218">To download code from a GitHub repository you use the ```git clone``` command.</span></span> <span data-ttu-id="4ed96-219">Například ke stažení úložiště vědecké účely data publikovaný microsoftem do aktuální adresář můžete spustit následující příkaz v ```git-bash```.</span><span class="sxs-lookup"><span data-stu-id="4ed96-219">For example to download data science repository published by Microsoft into the current directory you can run the following command once you are in ```git-bash```.</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="4ed96-220">V sadě Visual Studio můžete to udělat stejné operace klonování.</span><span class="sxs-lookup"><span data-stu-id="4ed96-220">In Visual Studio, you can do the same clone operation.</span></span> <span data-ttu-id="4ed96-221">Následující snímek obrazovky ukazuje, jak pro přístup k Gitu a Githubu nástroje v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed96-221">The  following screen-shot shows how to access Git and GitHub tools in Visual Studio.</span></span>

![Git v sadě Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

<span data-ttu-id="4ed96-223">Můžete najít další informace o použití Git pracovat s úložiště GitHub z několika zdrojů, které jsou k dispozici na webu github.com. [Tahák](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) je užitečné referenční.</span><span class="sxs-lookup"><span data-stu-id="4ed96-223">You can find more information on using Git to work with your GitHub repository from several resources available on github.com. The [cheat sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is a useful reference.</span></span>

## <a name="7-access-various-azure-data-and-analytics-services"></a><span data-ttu-id="4ed96-224">7. Přístup k různým Azure službám data a analýzy</span><span class="sxs-lookup"><span data-stu-id="4ed96-224">7. Access various Azure data and analytics services</span></span>
### <a name="azure-blob"></a><span data-ttu-id="4ed96-225">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="4ed96-225">Azure Blob</span></span>
<span data-ttu-id="4ed96-226">Objektů blob v Azure je spolehlivé a ekonomické cloudové úložiště pro data velká a malá.</span><span class="sxs-lookup"><span data-stu-id="4ed96-226">Azure blob is a reliable, economical cloud storage for data big and small.</span></span> <span data-ttu-id="4ed96-227">Dejte nám se podívejte na tom, jak můžete přesunout data do Azure Blob a přístup k datům uloženým v objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4ed96-227">Let us look at how you can move data to Azure Blob and access data stored in an Azure Blob.</span></span>

<span data-ttu-id="4ed96-228">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4ed96-228">**Prerequisite**</span></span>

* <span data-ttu-id="4ed96-229">**Vytvoření účtu úložiště objektů Blob v Azure z [portál Azure](https://portal.azure.com).**</span><span class="sxs-lookup"><span data-stu-id="4ed96-229">**Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).**</span></span>

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="4ed96-231">Potvrďte, že je nástroj příkazového řádku AzCopy předem nainstalovaná nalezený na ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span><span class="sxs-lookup"><span data-stu-id="4ed96-231">Confirm that the pre-installed command-line AzCopy tool is found at ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span></span> <span data-ttu-id="4ed96-232">Můžete přidat adresář obsahující azcopy.exe do vaší proměnné prostředí PATH na nemuseli zadávat cestu celý příkaz i při spuštění tohoto nástroje.</span><span class="sxs-lookup"><span data-stu-id="4ed96-232">You can add the directory containing the azcopy.exe to your PATH environment variable to avoid typing the full command path when running this tool.</span></span> <span data-ttu-id="4ed96-233">Další informace o nástroj AzCopy naleznete [dokumentaci k AzCopy](../storage/common/storage-use-azcopy.md)</span><span class="sxs-lookup"><span data-stu-id="4ed96-233">For more info on AzCopy tool please refer to [AzCopy documentation](../storage/common/storage-use-azcopy.md)</span></span>
* <span data-ttu-id="4ed96-234">Spusťte nástroj Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="4ed96-234">Start the Azure Storage Explorer tool.</span></span> <span data-ttu-id="4ed96-235">Lze ji stáhnout z [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="4ed96-235">It can be downloaded from [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span> 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

<span data-ttu-id="4ed96-237">**Přesun dat z virtuálního počítače do objektu Blob Azure: AzCopy**</span><span class="sxs-lookup"><span data-stu-id="4ed96-237">**Move data from VM to Azure Blob: AzCopy**</span></span>

<span data-ttu-id="4ed96-238">Pro přesun dat mezi vaší místní soubory a úložiště objektů blob, můžete použít AzCopy v příkazového řádku nebo prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4ed96-238">To move data between your local files and blob storage, you can use AzCopy in command-line or PowerShell:</span></span>

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

<span data-ttu-id="4ed96-239">Nahraďte **C:\myfolder** na cestu, kde je uložen soubor, **můj_účet_úložiště** na název účtu úložiště objektů blob **můj_kontejner** název kontejneru, **klíč účtu úložiště** na přístupový klíč k úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4ed96-239">Replace **C:\myfolder** to the path where your file is stored, **mystorageaccount** to your blob storage account name, **mycontainer** to the container name, **storage account key** to your blob storage access key.</span></span> <span data-ttu-id="4ed96-240">Můžete najít přihlašovací údaje účtu úložiště v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4ed96-240">You can find your storage account credentials in [Azure portal](https://portal.azure.com).</span></span>

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

<span data-ttu-id="4ed96-242">V prostředí PowerShell nebo z příkazového řádku, spusťte příkaz AzCopy.</span><span class="sxs-lookup"><span data-stu-id="4ed96-242">Run AzCopy command in PowerShell or from a command prompt.</span></span> <span data-ttu-id="4ed96-243">Tady je některé příklady použití nástroje AzCopy příkazu:</span><span class="sxs-lookup"><span data-stu-id="4ed96-243">Here is some example usage of AzCopy command:</span></span>

    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



<span data-ttu-id="4ed96-244">Po spuštění příkazu AzCopy zkopírujte do Azure blob zobrazí ukazuje soubor nahoru v Azure Storage Explorer za chvíli.</span><span class="sxs-lookup"><span data-stu-id="4ed96-244">Once you run your AzCopy command to copy to an Azure blob you see your file shows up in Azure Storage Explorer shortly.</span></span>

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

<span data-ttu-id="4ed96-246">**Přesun dat z virtuálního počítače do objektu Blob Azure: Azure Storage Explorer**</span><span class="sxs-lookup"><span data-stu-id="4ed96-246">**Move data from VM to Azure Blob: Azure Storage Explorer**</span></span>

<span data-ttu-id="4ed96-247">Můžete také nahrát data z místního souboru v virtuálního počítače pomocí Průzkumníka úložiště Azure:</span><span class="sxs-lookup"><span data-stu-id="4ed96-247">You can also upload data from the local file in your VM using Azure Storage Explorer:</span></span>

* <span data-ttu-id="4ed96-248">Pokud chcete nahrát data do kontejneru, vyberte cílový kontejner a klikněte na **nahrát** tlačítko.![ Nahrát ve Storage Exploreru](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span><span class="sxs-lookup"><span data-stu-id="4ed96-248">To upload data to a container, select the target container and click the **Upload** button.![Upload in Storage Explorer](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span></span>
* <span data-ttu-id="4ed96-249">Klikněte na **...**  napravo **soubory** , vyberte jeden nebo více souborů ze systému souborů a klikněte na **nahrát** zahájíte nahrávání souborů.![ Nahrání souborů do objektu blob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span><span class="sxs-lookup"><span data-stu-id="4ed96-249">Click on the **...** to the right of the **Files** box, select one or multiple files to upload from the file system and click **Upload** to begin uploading the files.![Upload files to blob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span></span>

<span data-ttu-id="4ed96-250">**Čtení dat z Azure Blob: modul čtečky Machine Learning**</span><span class="sxs-lookup"><span data-stu-id="4ed96-250">**Read data from Azure Blob: Machine Learning reader module**</span></span>

<span data-ttu-id="4ed96-251">V nástroji Azure Machine Learning Studio můžete použít **importovat Data modulu** číst data z objektu blob služby.</span><span class="sxs-lookup"><span data-stu-id="4ed96-251">In Azure Machine Learning Studio you can use an **Import Data module** to read data from your blob.</span></span>

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

<span data-ttu-id="4ed96-253">**Čtení dat z Azure Blob: Python ODBC**</span><span class="sxs-lookup"><span data-stu-id="4ed96-253">**Read data from Azure Blob: Python ODBC**</span></span>

<span data-ttu-id="4ed96-254">Můžete použít **BlobService** knihovny číst data přímo z objektu blob v programu Poznámkový blok Jupyter nebo Python.</span><span class="sxs-lookup"><span data-stu-id="4ed96-254">You can use **BlobService** library to read data directly from blob in a Jupyter Notebook or Python program.</span></span>

<span data-ttu-id="4ed96-255">Nejprve importujte požadované balíčky:</span><span class="sxs-lookup"><span data-stu-id="4ed96-255">First, import required packages:</span></span>

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

<span data-ttu-id="4ed96-256">Potom zařaďte přihlašovací údaje účtu Azure Blob a čtení dat z objektu Blob:</span><span class="sxs-lookup"><span data-stu-id="4ed96-256">Then plug in your Azure Blob account credentials and read data from Blob:</span></span>

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
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

<span data-ttu-id="4ed96-257">Data se čtou v jako snímek dat:</span><span class="sxs-lookup"><span data-stu-id="4ed96-257">The data is read in as a data frame:</span></span>

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a><span data-ttu-id="4ed96-259">Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="4ed96-259">Azure Data Lake</span></span>
<span data-ttu-id="4ed96-260">Azure Data Lake Storage je velkého rozsahu úložiště pro úlohy analýzy velkých objemů dat a kompatibilní s Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="4ed96-260">Azure Data Lake Storage is a hyper-scale repository for big data analytics workloads and compatible with Hadoop Distributed File System (HDFS).</span></span> <span data-ttu-id="4ed96-261">Funguje s ekosystémem Hadoop a Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="4ed96-261">It works with both the Hadoop ecosystem and the Azure Data Lake Analytics.</span></span> <span data-ttu-id="4ed96-262">Ukážeme, jak můžete přesun dat do Azure Data Lake Store a spustit analytics pomocí Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="4ed96-262">We show how you can move data into the Azure Data Lake Store and run analytics using Azure Data Lake Analytics.</span></span>

<span data-ttu-id="4ed96-263">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4ed96-263">**Prerequisite**</span></span>

* <span data-ttu-id="4ed96-264">Vytvoření vaší Azure Data Lake Analytics v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4ed96-264">Create your Azure Data Lake Analytics in [Azure portal](https://portal.azure.com).</span></span>

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* <span data-ttu-id="4ed96-266">**Nástrojů Azure Data Lake** v **Visual Studio** najít v této [odkaz](https://www.microsoft.com/download/details.aspx?id=49504) je již nainstalován na Visual Studio Community Edition, který je na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="4ed96-266">The  **Azure Data Lake Tools** in **Visual Studio** found at this  [link](https://www.microsoft.com/download/details.aspx?id=49504) is already installed on the Visual Studio Community Edition which is on the virtual machine.</span></span> <span data-ttu-id="4ed96-267">Po spuštění sady Visual Studio a protokolování ve vašem předplatném Azure, měli byste vidět účet Azure Data Analytics a úložiště v levém panelu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ed96-267">After starting Visual Studio and logging in your Azure subscription, you should see your Azure Data Analytics account and storage in the left panel of Visual Studio.</span></span>

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

<span data-ttu-id="4ed96-269">**Přesun dat z virtuálního počítače do Data Lake: Azure Data Lake Explorer**</span><span class="sxs-lookup"><span data-stu-id="4ed96-269">**Move data from VM to Data Lake: Azure Data Lake Explorer**</span></span>

<span data-ttu-id="4ed96-270">Můžete použít **Azure Data Lake Explorer** ukládat data z místních souborů ve virtuálním počítači do úložiště Data Lake.</span><span class="sxs-lookup"><span data-stu-id="4ed96-270">You can use **Azure Data Lake Explorer** to upload data from the local files in your Virtual Machine to Data Lake storage.</span></span>

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

<span data-ttu-id="4ed96-272">Můžete také vytvořit datového kanálu pro productionize vaše přesun dat do nebo z pomocí Azure Data Lake [Azure dat Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="4ed96-272">You can also build a data pipeline to productionize your data movement to or from Azure Data Lake using the [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span></span> <span data-ttu-id="4ed96-273">Označujeme je to [článku](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) vás provedou kroky k vytvoření data kanálů.</span><span class="sxs-lookup"><span data-stu-id="4ed96-273">We refer you to this [article](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) to guide you through the steps to build the data pipelines.</span></span>

<span data-ttu-id="4ed96-274">**Čtení dat z objektu Blob Azure do Data Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="4ed96-274">**Read data from Azure Blob to Data Lake: U-SQL**</span></span>

<span data-ttu-id="4ed96-275">Pokud máte data uložená v úložišti objektů Blob v Azure, můžete přímo přečíst data z objektu blob úložiště Azure v dotazu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="4ed96-275">If your data resides in Azure Blob storage, you can directly read data from Azure storage blob in U-SQL query.</span></span> <span data-ttu-id="4ed96-276">Před skládání dotazu U-SQL, zajistěte, aby že váš účet úložiště objektů blob je propojený s vaší Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="4ed96-276">Before composing your U-SQL query, make sure your blob storage account is linked to your Azure Data Lake.</span></span> <span data-ttu-id="4ed96-277">Přejděte na **portál Azure**, najít řídicí panel Azure Data Lake Analytics, klikněte na **přidat zdroj dat**, vyberte typ úložiště pro **Azure Storage** a zařadit ho ve vašem účtu úložiště Azure Název a klíč.</span><span class="sxs-lookup"><span data-stu-id="4ed96-277">Go to **Azure portal**, find your Azure Data Lake Analytics dashboard, click **Add Data Source**, select storage type to **Azure Storage** and plug in your Azure Storage Account Name and Key.</span></span> <span data-ttu-id="4ed96-278">Budete pak moci referenční data uložená v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4ed96-278">Then you are able to reference the data stored in the storage account.</span></span>

![Zadejte účet úložiště a klíč](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

<span data-ttu-id="4ed96-280">V sadě Visual Studio můžete číst data z úložiště objektů blob, provést některé manipulaci s daty, konstruování a výstupní Výsledná data do Azure Data Lake nebo úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4ed96-280">In Visual Studio, you can read data from blob storage, do some data manipulation, feature engineering, and output the resulting data to either Azure Data Lake or Azure Blob Storage.</span></span> <span data-ttu-id="4ed96-281">Když odkazujete data v úložišti objektů blob, použijte **wasb: / /**; když odkazujete data v Azure Data Lake použití **swbhdfs: / /**</span><span class="sxs-lookup"><span data-stu-id="4ed96-281">When you reference the data in blob storage, use **wasb://**; when you reference the data in Azure Data Lake, use **swbhdfs://**</span></span>

![Data rámečku](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

<span data-ttu-id="4ed96-283">V sadě Visual Studio můžete použít následující dotazy U-SQL:</span><span class="sxs-lookup"><span data-stu-id="4ed96-283">You may use the following U-SQL queries in Visual Studio:</span></span>

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
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



<span data-ttu-id="4ed96-284">Po dotazu je odeslána na server, se zobrazí diagram zobrazující stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="4ed96-284">After your query is submitted to the server, a diagram showing the status of your job is displayed.</span></span>

![Diagram stavu úlohy](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

<span data-ttu-id="4ed96-286">**Dotaz na data v Data Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="4ed96-286">**Query data in Data Lake: U-SQL**</span></span>

<span data-ttu-id="4ed96-287">Po datová sada je konzumována do Azure Data Lake, můžete použít [jazykem U-SQL](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) pro dotazování a data prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="4ed96-287">After the dataset is ingested into Azure Data Lake, you can use [U-SQL language](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) to query and explore the data.</span></span> <span data-ttu-id="4ed96-288">Jazyk U-SQL je podobná T-SQL, ale spojuje některé funkce z jazyka C#, tak, aby uživatelé můžete napsat vlastní moduly, uživatelem definované funkce a atd. Můžete použít skripty v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="4ed96-288">U-SQL language is similar to T-SQL, but combines some features from C# so that users can write customized modules, user-defined functions, and etc. You can use the scripts in the previous step.</span></span>

<span data-ttu-id="4ed96-289">Po dotazu je odeslána na server, tripdata_summary. Sdílený svazek clusteru najdete krátce v **Azure Data Lake Explorer**, vám může zobrazte náhled dat, podle klikněte pravým tlačítkem na soubor.</span><span class="sxs-lookup"><span data-stu-id="4ed96-289">After the query is submitted to server, tripdata_summary.CSV can be found shortly in **Azure Data Lake Explorer**, you may preview the data by right-click the file.</span></span>

![Soubor v Průzkumníku služby Azure Data Lake](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

<span data-ttu-id="4ed96-291">Chcete-li zobrazit informace o souboru:</span><span class="sxs-lookup"><span data-stu-id="4ed96-291">To see the file information:</span></span>

![Souhrn souborů](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a><span data-ttu-id="4ed96-293">Clustery HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="4ed96-293">HDInsight Hadoop Clusters</span></span>
<span data-ttu-id="4ed96-294">Azure HDInsight je spravovaná služba Apache Hadoop, Spark, HBase nebo Storm v cloudu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-294">Azure HDInsight is a managed Apache Hadoop, Spark, HBase, and Storm service on the cloud.</span></span> <span data-ttu-id="4ed96-295">Můžete snadno pracovat s Azure HDInsight clustery z datové vědy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4ed96-295">You can work easily with Azure HDInsight clusters from the data science virtual machine.</span></span>

<span data-ttu-id="4ed96-296">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4ed96-296">**Prerequisite**</span></span>

* <span data-ttu-id="4ed96-297">Vytvoření účtu úložiště objektů Blob v Azure z [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4ed96-297">Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4ed96-298">Tento účet úložiště se používá k ukládání dat pro clustery služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4ed96-298">This storage account is used to store data for HDInsight clusters.</span></span>

![Vytvořit účet úložiště objektů Blob v Azure](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="4ed96-300">Přizpůsobení clusterů systému Hadoop HDInsight Azure z [portálu Azure](machine-learning-data-science-customize-hadoop-cluster.md)</span><span class="sxs-lookup"><span data-stu-id="4ed96-300">Customize Azure HDInsight Hadoop Clusters from [Azure portal](machine-learning-data-science-customize-hadoop-cluster.md)</span></span>
  
  * <span data-ttu-id="4ed96-301">Je nutné propojit účtu úložiště vytvořeném k vašemu clusteru HDInsight, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="4ed96-301">You must link the storage account created with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="4ed96-302">Tento účet úložiště se používá pro přístup k datům, která může být zpracována v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="4ed96-302">This storage account is used for accessing data that can be processed within the cluster.</span></span>

![Odkaz na účet úložiště, které jsou vytvořené pomocí clusteru HDInsight](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* <span data-ttu-id="4ed96-304">Je nutné povolit **vzdáleného přístupu** k hlavnímu uzlu clusteru po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="4ed96-304">You must enable **Remote Access** to the head node of the cluster after it is created.</span></span> <span data-ttu-id="4ed96-305">Pamatovat přihlašovací údaje vzdáleného přístupu, zde určíte (liší od nastavení zadané pro cluster při jeho vytváření): budete potřebovat v v následujícím postupu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-305">Remember the remote access credentials you specify here (different from those specified for the cluster at its creation): you need them in the subsequent procedure.</span></span>

![Povolte vzdálený přístup](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* <span data-ttu-id="4ed96-307">Vytvořte pracovní prostor služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4ed96-307">Create an Azure Machine Learning workspace.</span></span> <span data-ttu-id="4ed96-308">Vaše strojovým učením jsou uloženy v tomto pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4ed96-308">Your Machine Learning Experiments are stored in this Machine Learning workspace.</span></span> <span data-ttu-id="4ed96-309">Vyberte zvýrazněný možnosti portálu, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="4ed96-309">Select the highlighted options in Portal as shown in the following screenshot:</span></span>

![Vytvoření pracovního prostoru Azure Machine Learningu](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* <span data-ttu-id="4ed96-311">Zadejte parametry pro pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="4ed96-311">Then enter the parameters for your workspace</span></span>

![Zadejte parametry pracovního prostoru Machine Learning](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* <span data-ttu-id="4ed96-313">Nahrání dat pomocí IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="4ed96-313">Upload data using IPython Notebook.</span></span> <span data-ttu-id="4ed96-314">Importujte ho nejprve požadované balíčky, zařaďte přihlašovací údaje, vytvořit db ve vašem účtu úložiště a pak načíst data do clusterů HDI.</span><span class="sxs-lookup"><span data-stu-id="4ed96-314">First import required packages, plug in credentials, create a db in your storage account, then load data to HDI clusters.</span></span>

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


        #Create the connection to Hive using ODBC
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


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* <span data-ttu-id="4ed96-315">Alternativně můžete to provést [návod](machine-learning-data-science-process-hive-walkthrough.md) odesílat data NYC taxíkem do clusteru HDI.</span><span class="sxs-lookup"><span data-stu-id="4ed96-315">Alternately,  you can follow this [walkthrough](machine-learning-data-science-process-hive-walkthrough.md) to upload NYC Taxi data to HDI cluster.</span></span> <span data-ttu-id="4ed96-316">Hlavní kroky zahrnují:</span><span class="sxs-lookup"><span data-stu-id="4ed96-316">Major steps include:</span></span>
  
  * <span data-ttu-id="4ed96-317">AzCopy: Stáhněte komprimované CSV z veřejného objektu blob do místní složky</span><span class="sxs-lookup"><span data-stu-id="4ed96-317">AzCopy: download zipped CSV's from public blob to your local folder</span></span>
  * <span data-ttu-id="4ed96-318">AzCopy: nahrajte rozbalené sdílených svazcích clusteru z místní složky do clusteru HDI</span><span class="sxs-lookup"><span data-stu-id="4ed96-318">AzCopy: upload unzipped CSV's from local folder to HDI cluster</span></span>
  * <span data-ttu-id="4ed96-319">Přihlaste se k hlavnímu uzlu clusteru Hadoop a příprava pro analýzu nahodilého dat</span><span class="sxs-lookup"><span data-stu-id="4ed96-319">Log into the head node of Hadoop cluster and prepare for exploratory data analysis</span></span>

<span data-ttu-id="4ed96-320">Po načtení dat do clusteru HDI, můžete zkontrolovat vaše data v Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="4ed96-320">After the data is loaded to HDI cluster, you can check your data in Azure Storage Explorer.</span></span> <span data-ttu-id="4ed96-321">A máte databáze nyctaxidb, vytvořené v clusteru HDI.</span><span class="sxs-lookup"><span data-stu-id="4ed96-321">And you have a database nyctaxidb created in HDI cluster.</span></span>

<span data-ttu-id="4ed96-322">**Zkoumání dat: dotazů Hive v Pythonu**</span><span class="sxs-lookup"><span data-stu-id="4ed96-322">**Data exploration: Hive Queries in Python**</span></span>

<span data-ttu-id="4ed96-323">Vzhledem k tomu, že data jsou v clusteru Hadoop, můžete se připojit k databázi clusterů systému Hadoop a dotazování pomocí Hive uděláte zkoumání a konstruování pyodbc balíčku.</span><span class="sxs-lookup"><span data-stu-id="4ed96-323">Since the data is in Hadoop cluster, you can use the pyodbc package to connect to Hadoop Clusters and query database using Hive to do exploration and feature engineering.</span></span> <span data-ttu-id="4ed96-324">Můžete zobrazit existující tabulky, které jsme vytvořili v kroku požadovaných součástí.</span><span class="sxs-lookup"><span data-stu-id="4ed96-324">You can view the existing tables we created in the prerequisite step.</span></span>

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Zobrazit existující tabulky](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

<span data-ttu-id="4ed96-326">Podívejme se na počet záznamů v každém měsíci a frekvence šikmý nebo není v tabulce cesty:</span><span class="sxs-lookup"><span data-stu-id="4ed96-326">Let's look at the number of records in each month and the frequencies of tipped or not in the trip table:</span></span>

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

<span data-ttu-id="4ed96-329">Můžeme také výpočetní vzdálenost mezi výstupní umístění a dropoff umístění a porovnejte je s vzdálenost cesty.</span><span class="sxs-lookup"><span data-stu-id="4ed96-329">We can also compute the distance between pickup location and dropoff location and then compare it to the trip distance.</span></span>

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


![Vykreslení vyzvednutí/dropoff vzdálenost vzdálenost cesty](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

<span data-ttu-id="4ed96-332">Nyní Pojďme Příprava nižší vzorků (1 %) sady dat pro modelování.</span><span class="sxs-lookup"><span data-stu-id="4ed96-332">Now let's prepare a down-sampled (1%) set of data for modeling.</span></span> <span data-ttu-id="4ed96-333">Tyto údaje můžete použít v modul čtečky Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4ed96-333">We can use this data in Machine Learning reader module.</span></span>

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

        --- now insert contents of the join into the preceding internal table

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

<span data-ttu-id="4ed96-334">Po chvíli uvidíte, že data se načetl v clusterů systému Hadoop:</span><span class="sxs-lookup"><span data-stu-id="4ed96-334">After a while, you can see the data has been loaded in Hadoop clusters:</span></span>

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabulka dat](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

<span data-ttu-id="4ed96-336">**Čtení dat z HDI pomocí Machine Learning: modul čtečky**</span><span class="sxs-lookup"><span data-stu-id="4ed96-336">**Read data from HDI using Machine Learning: reader module**</span></span>

<span data-ttu-id="4ed96-337">Můžete také používat **čtečky** modulu v nástroji Machine Learning Studio pro přístup k databázi v clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="4ed96-337">You may also use the **reader** module in Machine Learning Studio to access the database in Hadoop cluster.</span></span> <span data-ttu-id="4ed96-338">Připojte přihlašovací údaje HDI clusterů a účet úložiště Azure, abyste umožnili sestavení ing modelů strojového učení pomocí databáze v clusterech HDI.</span><span class="sxs-lookup"><span data-stu-id="4ed96-338">Plug in the credentials of your HDI clusters and Azure Storage Account to enable build ing machine learning models using database in HDI clusters.</span></span>

![Vlastnosti modulu Reader](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

<span data-ttu-id="4ed96-340">Scored datovou sadu lze následně zobrazit:</span><span class="sxs-lookup"><span data-stu-id="4ed96-340">The scored dataset can then be viewed:</span></span>

![Zobrazení skóre pro magnitudu datové sady](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a><span data-ttu-id="4ed96-342">Azure SQL Data Warehouse & databáze</span><span class="sxs-lookup"><span data-stu-id="4ed96-342">Azure SQL Data Warehouse & databases</span></span>
<span data-ttu-id="4ed96-343">Azure SQL Data Warehouse je elastické datového skladu jako služba podnikových prostředí systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4ed96-343">Azure SQL Data Warehouse is an elastic data warehouse as a service with enterprise-class SQL Server experience.</span></span>

<span data-ttu-id="4ed96-344">Můžete zřídit Azure SQL Data Warehouse pomocí následujících pokynů uvedených v tomto [článku](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="4ed96-344">You can provision your Azure SQL Data Warehouse by following the instructions provided in this [article](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span></span> <span data-ttu-id="4ed96-345">Po zřízení Azure SQL Data Warehouse, můžete to [návod](machine-learning-data-science-process-sqldw-walkthrough.md) udělat nahrání dat, zkoumání a modelování pomocí dat v rámci SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4ed96-345">Once you provision your Azure SQL Data Warehouse, you can use this [walkthrough](machine-learning-data-science-process-sqldw-walkthrough.md) to do data upload, exploration and modeling using data within the SQL Data Warehouse.</span></span>

#### <a name="azure-cosmos-db"></a><span data-ttu-id="4ed96-346">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4ed96-346">Azure Cosmos DB</span></span>
<span data-ttu-id="4ed96-347">Azure Cosmos DB je databáze NoSQL v cloudu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-347">Azure Cosmos DB is a NoSQL database in the cloud.</span></span> <span data-ttu-id="4ed96-348">Ji umožňuje pracovat s dokumenty jako JSON a umožňuje ukládání a dotazování dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4ed96-348">It allows you to work with documents like JSON and allows you to store and query the documents.</span></span>

<span data-ttu-id="4ed96-349">Je třeba provést následující kroky na požadavky pro přístup k databázi Azure Cosmos z DSVM.</span><span class="sxs-lookup"><span data-stu-id="4ed96-349">You need to do the following per-requisites steps to access Azure Cosmos DB from the DSVM.</span></span>

1. <span data-ttu-id="4ed96-350">Instalace DocumentDB Python SDK (Spustit ```pip install pydocumentdb``` z příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="4ed96-350">Install DocumentDB Python SDK (Run ```pip install pydocumentdb``` from command prompt)</span></span>
2. <span data-ttu-id="4ed96-351">Vytvoření účtu Azure Cosmos databáze a databáze z [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="4ed96-351">Create an Azure Cosmos DB account and a database from [Azure portal](https://portal.azure.com)</span></span>
3. <span data-ttu-id="4ed96-352">Stáhnout "Nástroj pro migraci Azure Cosmos DB" z [zde](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) a extrahování k adresáři podle svého výběru</span><span class="sxs-lookup"><span data-stu-id="4ed96-352">Download "Azure Cosmos DB Migration Tool" from [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) and extract to a directory of your choice</span></span>
4. <span data-ttu-id="4ed96-353">Umožňuje importovat data JSON (sopka data) uložené na [veřejného objektu blob](https://cahandson.blob.core.windows.net/samples/volcano.json) do databáze Cosmos s následující parametry příkazu pro nástroj pro migraci (dtui.exe z adresáře, kam jste nainstalovali nástroj pro migraci DB Cosmos).</span><span class="sxs-lookup"><span data-stu-id="4ed96-353">Import JSON data (volcano data) stored on a [public blob](https://cahandson.blob.core.windows.net/samples/volcano.json) into Cosmos DB with following command parameters to the migration tool (dtui.exe from the directory where you installed the Cosmos DB Migration Tool).</span></span> <span data-ttu-id="4ed96-354">Zadejte umístění zdrojové a cílové s těmito parametry:</span><span class="sxs-lookup"><span data-stu-id="4ed96-354">Enter the source and target location with these parameters:</span></span>
   
    <span data-ttu-id="4ed96-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[klíče]; Database = sopka /t.Collection:volcano1</span><span class="sxs-lookup"><span data-stu-id="4ed96-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1</span></span>

<span data-ttu-id="4ed96-356">Jakmile importujete data, můžete přejít do Jupyter a otevřete Poznámkový blok s názvem *DocumentDBSample* obsahující kód python přístup k DocumentDB a provádět některé základní dotazování.</span><span class="sxs-lookup"><span data-stu-id="4ed96-356">Once you import the data, you can go to Jupyter and open the notebook titled *DocumentDBSample* which contains python code to access DocumentDB and do some basic querying.</span></span> <span data-ttu-id="4ed96-357">Další informace o Cosmos DB návštěvou službu [stránky dokumentace, která](https://docs.microsoft.com/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="4ed96-357">You can learn more about Cosmos DB by visiting the service [documentation page](https://docs.microsoft.com/azure/cosmos-db/).</span></span>

## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a><span data-ttu-id="4ed96-358">8. Vytvářejte sestavy a řídicí panel pomocí Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="4ed96-358">8. Build reports and dashboard using the Power BI Desktop</span></span>
<span data-ttu-id="4ed96-359">Dejte nám Vizualizujte soubor sopka JSON, který jsme viděli v předchozím příkladu Cosmos DB v Power BI k visual proniknout do data.</span><span class="sxs-lookup"><span data-stu-id="4ed96-359">Let us visualize the Volcano JSON file that we saw in the preceding Cosmos DB example in Power BI to gain visual insights into the data.</span></span> <span data-ttu-id="4ed96-360">Podrobné pokyny jsou k dispozici v [Power BI článku](../cosmos-db/powerbi-visualize.md).</span><span class="sxs-lookup"><span data-stu-id="4ed96-360">Detailed steps are available in the [Power BI article](../cosmos-db/powerbi-visualize.md).</span></span> <span data-ttu-id="4ed96-361">Zde jsou základní kroky:</span><span class="sxs-lookup"><span data-stu-id="4ed96-361">Here are the high-level steps:</span></span>

1. <span data-ttu-id="4ed96-362">Otevřít Power BI Desktop a do "Získat Data".</span><span class="sxs-lookup"><span data-stu-id="4ed96-362">Open Power BI Desktop and do "Get Data".</span></span> <span data-ttu-id="4ed96-363">Zadejte adresu URL jako: https://cahandson.blob.core.windows.net/samples/volcano.json</span><span class="sxs-lookup"><span data-stu-id="4ed96-363">Specify the URL as: https://cahandson.blob.core.windows.net/samples/volcano.json</span></span>
2. <span data-ttu-id="4ed96-364">Měli byste vidět importovaných jako seznam záznamů JSON</span><span class="sxs-lookup"><span data-stu-id="4ed96-364">You should see the JSON records imported as a list</span></span>
3. <span data-ttu-id="4ed96-365">Převést seznam na tabulku, Power BI mohli pracovat se stejným</span><span class="sxs-lookup"><span data-stu-id="4ed96-365">Convert the list to a table so Power BI can work with the same</span></span>
4. <span data-ttu-id="4ed96-366">Rozbalte sloupce kliknutím na ikonu rozbalení (jeden ikonou "šipku vlevo a šipku vpravo" na pravé straně sloupec)</span><span class="sxs-lookup"><span data-stu-id="4ed96-366">Expand the columns by clicking on the expand icon (the one with the "left arrow and a right arrow" icon on the right of the column)</span></span>
5. <span data-ttu-id="4ed96-367">Všimněte si, že umístění je na pole "Záznam".</span><span class="sxs-lookup"><span data-stu-id="4ed96-367">Notice that location is a "Record" field.</span></span> <span data-ttu-id="4ed96-368">Rozbalte položku a vyberte pouze souřadnice.</span><span class="sxs-lookup"><span data-stu-id="4ed96-368">Expand the record and select only the coordinates.</span></span> <span data-ttu-id="4ed96-369">Souřadnice je sloupec seznamu</span><span class="sxs-lookup"><span data-stu-id="4ed96-369">Coordinate is a list column</span></span>
6. <span data-ttu-id="4ed96-370">Umožňuje přidat nový sloupec a převádět souřadnic sloupec seznamu čárkami samostatný LatLong sloupec zřetězení dva elementy pole souřadnic seznamu pomocí vzorce ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span><span class="sxs-lookup"><span data-stu-id="4ed96-370">Add a new column to convert the list coordinate column into a comma separate LatLong column concatenating the two elements in the coordinate list field using the formula ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span></span>
7. <span data-ttu-id="4ed96-371">Nakonec převést ```Elevation``` sloupec desetinných míst a vyberte **Zavřít** a **použít**.</span><span class="sxs-lookup"><span data-stu-id="4ed96-371">Finally convert the ```Elevation``` column to Decimal and select the **Close** and **Apply**.</span></span>

<span data-ttu-id="4ed96-372">Místo předchozích kroků, můžete vložte následující kód, skripty out s postupem v editoru Advanced v Power BI, který umožňuje zapisovat transformace dat v dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="4ed96-372">Instead of preceding steps, you can paste the following code that scripts out the steps used in the Advanced Editor in Power BI that allows you to write the data transformations in a query language.</span></span>

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



<span data-ttu-id="4ed96-373">Nyní máte data v Power BI datového modelu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-373">You now have the data in your Power BI data model.</span></span> <span data-ttu-id="4ed96-374">Power BI ploše by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="4ed96-374">Your Power BI desktop should appear as follows:</span></span>

![Power BI Desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

<span data-ttu-id="4ed96-376">Můžete začít vytvářet sestavy a vizualizací pomocí datového modelu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-376">You can start building reports and visualizations using the data model.</span></span> <span data-ttu-id="4ed96-377">Můžete postupovat podle kroků v tomto [Power BI článku](../cosmos-db/powerbi-visualize.md#build-the-reports) pro vytvoření sestavy.</span><span class="sxs-lookup"><span data-stu-id="4ed96-377">You can follow the steps in this [Power BI article](../cosmos-db/powerbi-visualize.md#build-the-reports) to build a report.</span></span> <span data-ttu-id="4ed96-378">Konečný výsledek je sestava, která vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="4ed96-378">The end result is a report that looks like the following.</span></span>

![Power BI Desktop zobrazení sestavy - Power BI connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a><span data-ttu-id="4ed96-380">9. Dynamické škálování vašeho DSVM potřebám vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="4ed96-380">9. Dynamically scale your DSVM to meet your project needs</span></span>
<span data-ttu-id="4ed96-381">Je možné škálovat nahoru a dolů DSVM potřebám vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-381">You can scale up and down the DSVM to meet your project needs.</span></span> <span data-ttu-id="4ed96-382">Pokud nemusíte používat virtuální počítač v večer nebo o víkendech, můžete právě vypnout virtuální počítač z [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4ed96-382">If you don't need to use the VM in the evening or weekends, you can just shut down the VM from the [Azure portal](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="4ed96-383">Pokud používáte pouze tlačítko vypnutí operačního systému ve virtuálním počítači platit poplatky výpočty.</span><span class="sxs-lookup"><span data-stu-id="4ed96-383">You incur compute charges if you use just the Operating system shutdown button on the VM.</span></span>  
> 
> 

<span data-ttu-id="4ed96-384">Pokud je potřeba zpracovat některé rozsáhlé analýzy a potřebovat větší kapacitu procesoru nebo paměti a disku můžete najít velké volba velikostí virtuálních počítačů z hlediska jader procesoru, kapacita paměti a disku typy (včetně jednotek SSD), které splňují vaše výpočetní a rozpočtových potřebuje.</span><span class="sxs-lookup"><span data-stu-id="4ed96-384">If you need to handle some large-scale analysis and need more CPU and/or memory and/or disk capacity you can find a large choice of VM sizes in terms of CPU cores, memory capacity and disk types (including Solid state drives) that meet your compute and budgetary needs.</span></span> <span data-ttu-id="4ed96-385">Úplný seznam virtuálních počítačů spolu s jejich hodinové výpočetní ceny je k dispozici na [ceny virtuálních počítačů Azure](https://azure.microsoft.com/pricing/details/virtual-machines/) stránky.</span><span class="sxs-lookup"><span data-stu-id="4ed96-385">The full list of VMs along with their hourly compute pricing is available on the [Azure Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) page.</span></span>

<span data-ttu-id="4ed96-386">Podobně pokud snižuje potřeba kapacity zpracování virtuálního počítače (například: přesunout hlavní zatížení na Hadoop nebo Spark cluster), je možné škálovat dolů je cluster ze [portál Azure](https://portal.azure.com) a nastavení vaší instance virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4ed96-386">Similarly, if your need for VM processing capacity reduces (for example: you moved a major workload to a Hadoop or a Spark cluster), you can scale down the cluster from the [Azure portal](https://portal.azure.com) and going to the settings of your VM instance.</span></span> <span data-ttu-id="4ed96-387">Zde je snímek.</span><span class="sxs-lookup"><span data-stu-id="4ed96-387">Here is a screenshot.</span></span>

![Nastavení instance virtuálních počítačů](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a><span data-ttu-id="4ed96-389">10. Nainstalujte další nástroje na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="4ed96-389">10. Install additional tools on your virtual machine</span></span>
<span data-ttu-id="4ed96-390">Budeme mít zabalené několik nástrojů, které se domníváme, se můžou na adresu řadu běžné potřeby analýzy dat a který by měl ušetřit čas vyhnout nutnosti instalace a konfigurace vašeho prostředí po jednom a uložte peníze platebním pouze pro prostředky, že používáte.</span><span class="sxs-lookup"><span data-stu-id="4ed96-390">We have packaged several tools that we believe are able to address many of the common data analytics needs and that should save you time by avoiding having to install and configure your environments one by one and save you money by paying only for resources that you use.</span></span>

<span data-ttu-id="4ed96-391">Můžete využít další data a analýzy služby Azure profilovaným v tomto článku k vylepšení prostředí analýzy.</span><span class="sxs-lookup"><span data-stu-id="4ed96-391">You can leverage other Azure data and analytics services profiled in this article to enhance your analytics environment.</span></span> <span data-ttu-id="4ed96-392">Chápeme, že v některých případech může vyžadovat vašim potřebám dalších nástrojů, včetně některá vlastnické nástroje třetích stran.</span><span class="sxs-lookup"><span data-stu-id="4ed96-392">We understand that in some cases your needs may require additional tools, including some proprietary third-party tools.</span></span> <span data-ttu-id="4ed96-393">Máte plný přístup správce na virtuální počítač pro instalaci nové nástroje, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="4ed96-393">You have full administrative access on the virtual machine to install new tools you need.</span></span> <span data-ttu-id="4ed96-394">Můžete taky nainstalovat další balíčky Python a R, která nejsou předem nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="4ed96-394">You can also install additional packages in Python and R that are not pre-installed.</span></span> <span data-ttu-id="4ed96-395">Pro jazyk Python můžete použít buď ```conda``` nebo ```pip```.</span><span class="sxs-lookup"><span data-stu-id="4ed96-395">For Python you can use either ```conda``` or ```pip```.</span></span> <span data-ttu-id="4ed96-396">Pro R můžete použít ```install.packages()``` v R konzole nebo pomocí rozhraní IDE a zvolte "**balíčky** -> **instalovat balíčky...** ".</span><span class="sxs-lookup"><span data-stu-id="4ed96-396">For R you can use the ```install.packages()``` in the R console or use the IDE and choose "**Packages** -> **Install Packages...**".</span></span>

## <a name="summary"></a><span data-ttu-id="4ed96-397">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4ed96-397">Summary</span></span>
<span data-ttu-id="4ed96-398">Toto jsou jen některé z akcí, které můžete provést na Microsoft Data vědecké účely virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4ed96-398">These are just some of the things you can do on the Microsoft Data Science Virtual Machine.</span></span> <span data-ttu-id="4ed96-399">Existuje mnoho dalších věcí, které můžete provést, aby bylo prostředí efektivní analýzu.</span><span class="sxs-lookup"><span data-stu-id="4ed96-399">There are many more things you can do to make it an effective analytics environment.</span></span>

