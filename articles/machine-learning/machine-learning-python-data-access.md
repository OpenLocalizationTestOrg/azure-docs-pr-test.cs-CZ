---
title: "datové sady aaaAccess s knihovnou klienta Machine Learning Python | Microsoft Docs"
description: "Nainstalovat a používat hello tooaccess knihovny klienta Python a bezpečně spravovat Azure Machine Learning data z místního prostředí Python."
services: machine-learning
documentationcenter: python
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: huvalo;bradsev
ms.openlocfilehash: f55067118f13c52bf677930a20836ce6989f8187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-datasets-with-python-using-hello-azure-machine-learning-python-client-library"></a><span data-ttu-id="bb480-103">Přístup k datové sady s Pythonem pomocí klientské knihovny Azure Machine Learning Python hello</span><span class="sxs-lookup"><span data-stu-id="bb480-103">Access datasets with Python using hello Azure Machine Learning Python client library</span></span>
<span data-ttu-id="bb480-104">Hello preview Klientská knihovna pro Microsoft Azure Machine Learning Python můžete povolit zabezpečený přístup tooyour Azure Machine Learning datové sady z místního prostředí Python a umožňuje hello vytváření a správu datových sad v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="bb480-104">hello preview of Microsoft Azure Machine Learning Python client library can enable secure access tooyour Azure Machine Learning datasets from a local Python environment and enables hello creation and management of datasets in a workspace.</span></span>

<span data-ttu-id="bb480-105">Toto téma obsahuje pokyny o tom, jak:</span><span class="sxs-lookup"><span data-stu-id="bb480-105">This topic provides instructions on how to:</span></span>

* <span data-ttu-id="bb480-106">Nainstalujte klientské knihovny pro Machine Learning Python hello</span><span class="sxs-lookup"><span data-stu-id="bb480-106">install hello Machine Learning Python client library</span></span> 
* <span data-ttu-id="bb480-107">přístup a datové sady, včetně pokyny, jak nahrát tooget autorizace tooaccess Azure Machine Learning datové sady z vaší místní prostředí Python</span><span class="sxs-lookup"><span data-stu-id="bb480-107">access and upload datasets, including instructions on how tooget authorization tooaccess Azure Machine Learning datasets from your local Python environment</span></span>
* <span data-ttu-id="bb480-108">přístup k zprostředkující datové sady z experimentů</span><span class="sxs-lookup"><span data-stu-id="bb480-108">access intermediate datasets from experiments</span></span>
* <span data-ttu-id="bb480-109">používat hello Python klienta knihovny tooenumerate datové sady, získat přístup k metadatům, přečíst obsah hello datové sady, vytvořte nové datové sady a aktualizovat existující datové sady</span><span class="sxs-lookup"><span data-stu-id="bb480-109">use hello Python client library tooenumerate datasets, access metadata, read hello contents of a dataset, create new datasets and update existing datasets</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <span data-ttu-id="bb480-110"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="bb480-110"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="bb480-111">Klientská knihovna pro Python Hello otestovala pod hello následující prostředí:</span><span class="sxs-lookup"><span data-stu-id="bb480-111">hello Python client library has been tested under hello following environments:</span></span>

* <span data-ttu-id="bb480-112">Windows, Mac a Linux</span><span class="sxs-lookup"><span data-stu-id="bb480-112">Windows, Mac and Linux</span></span>
* <span data-ttu-id="bb480-113">Python 2.7, 3.3 a 3.4</span><span class="sxs-lookup"><span data-stu-id="bb480-113">Python 2.7, 3.3 and 3.4</span></span>

<span data-ttu-id="bb480-114">Má závislost na hello následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="bb480-114">It has a dependency on hello following packages:</span></span>

* <span data-ttu-id="bb480-115">požadavky</span><span class="sxs-lookup"><span data-stu-id="bb480-115">requests</span></span>
* <span data-ttu-id="bb480-116">Python dateutil</span><span class="sxs-lookup"><span data-stu-id="bb480-116">python-dateutil</span></span>
* <span data-ttu-id="bb480-117">pandas</span><span class="sxs-lookup"><span data-stu-id="bb480-117">pandas</span></span>

<span data-ttu-id="bb480-118">Doporučujeme použít distribuci jazyka Python, jako [Anaconda](http://continuum.io/downloads#all) nebo [zápoje](https://store.enthought.com/downloads/), které jsou součástí Python, IPython a nainstalované hello tři balíčky uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="bb480-118">We recommend using a Python distribution such as [Anaconda](http://continuum.io/downloads#all) or [Canopy](https://store.enthought.com/downloads/), which come with Python, IPython and hello three packages listed above installed.</span></span> <span data-ttu-id="bb480-119">I když IPython není nezbytně nutné, je skvělé prostředí pro manipulace a vizualizace dat interaktivně.</span><span class="sxs-lookup"><span data-stu-id="bb480-119">Although IPython is not strictly required, it is a great environment for manipulating and visualizing data interactively.</span></span>

### <span data-ttu-id="bb480-120"><a name="installation"></a>Jak tooinstall hello Klientská knihovna pro Azure Machine Learning Python</span><span class="sxs-lookup"><span data-stu-id="bb480-120"><a name="installation"></a>How tooinstall hello Azure Machine Learning Python client library</span></span>
<span data-ttu-id="bb480-121">Klientská knihovna pro Azure Machine Learning Python Hello také musí být nainstalované toocomplete hello úlohy uvedené v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="bb480-121">hello Azure Machine Learning Python client library must also be installed toocomplete hello tasks outlined in this topic.</span></span> <span data-ttu-id="bb480-122">Je k dispozici z hello [indexu balíčků Pythonu](https://pypi.python.org/pypi/azureml).</span><span class="sxs-lookup"><span data-stu-id="bb480-122">It is available from hello [Python Package Index](https://pypi.python.org/pypi/azureml).</span></span> <span data-ttu-id="bb480-123">tooinstall ji ve vašem prostředí Python, spusťte následující příkaz z vaší místní prostředí Python hello:</span><span class="sxs-lookup"><span data-stu-id="bb480-123">tooinstall it in your Python environment, run hello following command from your local Python environment:</span></span>

    pip install azureml

<span data-ttu-id="bb480-124">Alternativně můžete stáhnout a nainstalovat z hello zdrojů na [githubu](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span><span class="sxs-lookup"><span data-stu-id="bb480-124">Alternatively, you can download and install from hello sources on [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span></span>

    python setup.py install

<span data-ttu-id="bb480-125">Pokud máte nainstalovaný na počítači git, můžete přímo z úložiště git hello pip tooinstall:</span><span class="sxs-lookup"><span data-stu-id="bb480-125">If you have git installed on your machine, you can use pip tooinstall directly from hello git repository:</span></span>

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <span data-ttu-id="bb480-126"><a name="datasetAccess"></a>Použít Studio Code fragmenty tooaccess datové sady</span><span class="sxs-lookup"><span data-stu-id="bb480-126"><a name="datasetAccess"></a>Use Studio Code snippets tooaccess datasets</span></span>
<span data-ttu-id="bb480-127">Klientská knihovna pro Hello Python poskytuje existující datové sady tooyour programový přístup z experimenty, které byly spuštěny.</span><span class="sxs-lookup"><span data-stu-id="bb480-127">hello Python client library gives you programmatic access tooyour existing datasets from experiments that have been run.</span></span>

<span data-ttu-id="bb480-128">Z hello Studio webové rozhraní můžete vygenerovat fragmenty kódu, které zahrnují všechny potřebné informace toodownload hello a deserializaci datové sady jako objekty Pandas DataFrame na počítači pro umístění.</span><span class="sxs-lookup"><span data-stu-id="bb480-128">From hello Studio web interface, you can generate code snippets that include all hello necessary information toodownload and deserialize datasets as Pandas DataFrame objects on your location machine.</span></span>

### <span data-ttu-id="bb480-129"><a name="security"></a>Zabezpečení pro přístup k datům</span><span class="sxs-lookup"><span data-stu-id="bb480-129"><a name="security"></a>Security for data access</span></span>
<span data-ttu-id="bb480-130">Hello fragmenty kódu poskytované Studio pro použití s hello Python Klientská knihovna obsahuje id pracovního prostoru a autorizační token.</span><span class="sxs-lookup"><span data-stu-id="bb480-130">hello code snippets provided by Studio for use with hello Python client library includes your workspace id and authorization token.</span></span> <span data-ttu-id="bb480-131">Tyto poskytnout úplný přístup tooyour prostoru a musí být chráněny, jako je heslo.</span><span class="sxs-lookup"><span data-stu-id="bb480-131">These provide full access tooyour workspace and must be protected, like a password.</span></span>

<span data-ttu-id="bb480-132">Z bezpečnostních důvodů hello funkce fragmentu kódu je pouze k dispozici toousers, které mají své role nastavit jako **vlastníka** pro pracovní prostor hello.</span><span class="sxs-lookup"><span data-stu-id="bb480-132">For security reasons, hello code snippet functionality is only available toousers that have their role set as **Owner** for hello workspace.</span></span> <span data-ttu-id="bb480-133">Vaše role se zobrazí v Azure Machine Learning Studio na hello **uživatelé** v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="bb480-133">Your role is displayed in Azure Machine Learning Studio on hello **USERS** page under **Settings**.</span></span>

![Zabezpečení][security]

<span data-ttu-id="bb480-135">Pokud vaše role není nastaven jako **vlastníka**, můžete požádat o toobe nového pozvání jako vlastníka, nebo požádejte vlastníka hello hello prostoru tooprovide vám hello fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="bb480-135">If your role is not set as **Owner**, you can either request toobe reinvited as an owner, or ask hello owner of hello workspace tooprovide you with hello code snippet.</span></span>

<span data-ttu-id="bb480-136">tooobtain hello autorizační token, můžete provést jednu z následujících akcí hello:</span><span class="sxs-lookup"><span data-stu-id="bb480-136">tooobtain hello authorization token, you can do one of hello following:</span></span>

* <span data-ttu-id="bb480-137">Požádejte o token z vlastníka.</span><span class="sxs-lookup"><span data-stu-id="bb480-137">Ask for a token from an owner.</span></span> <span data-ttu-id="bb480-138">Přístup k nim vlastníci jejich autorizace tokeny ze stránky nastavení hello jejich pracovního prostoru v Studio.</span><span class="sxs-lookup"><span data-stu-id="bb480-138">Owners can access their authorization tokens from hello Settings page of their workspace in Studio.</span></span> <span data-ttu-id="bb480-139">Vyberte **nastavení** z hello levé podokno a klikněte na tlačítko **AUTORIZACE TOKENY** toosee hello primární a sekundární tokeny.</span><span class="sxs-lookup"><span data-stu-id="bb480-139">Select **Settings** from hello left pane and click **AUTHORIZATION TOKENS** toosee hello primary and secondary tokens.</span></span>  <span data-ttu-id="bb480-140">Hello primární nebo sekundární autorizace tokeny hello lze ve fragmentu kódu hello, ale doporučujeme vlastníky sdílet jenom tokeny hello sekundární ověřování.</span><span class="sxs-lookup"><span data-stu-id="bb480-140">Although either hello primary or hello secondary authorization tokens can be used in hello code snippet, it is recommended that owners only share hello secondary authorization tokens.</span></span>

![Tokeny ověřování](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* <span data-ttu-id="bb480-142">Požádejte toobe povýší toorole vlastníka.</span><span class="sxs-lookup"><span data-stu-id="bb480-142">Ask toobe promoted toorole of owner.</span></span>  <span data-ttu-id="bb480-143">toodo to aktuálního vlastníka hello prostoru potřebám toofirst můžete odebrat z pracovního prostoru hello pak znovu vás pozval tooit jako vlastníka.</span><span class="sxs-lookup"><span data-stu-id="bb480-143">toodo this, a current owner of hello workspace needs toofirst remove you from hello workspace then re-invite you tooit as an owner.</span></span>

<span data-ttu-id="bb480-144">Po získání vývojáři id pracovního prostoru hello a autorizace token, jsou možné tooaccess hello prostoru pomocí fragmentu kódu hello bez ohledu na jejich role.</span><span class="sxs-lookup"><span data-stu-id="bb480-144">Once developers have obtained hello workspace id and authorization token, they are able tooaccess hello workspace using hello code snippet regardless of their role.</span></span>

<span data-ttu-id="bb480-145">Tokeny ověřování jsou spravovány v hello **AUTORIZACE TOKENY** v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="bb480-145">Authorization tokens are managed on hello **AUTHORIZATION TOKENS** page under **SETTINGS**.</span></span> <span data-ttu-id="bb480-146">Můžete je obnovit, ale tento postup odvolá přístup toohello předchozí token.</span><span class="sxs-lookup"><span data-stu-id="bb480-146">You can regenerate them, but this procedure revokes access toohello previous token.</span></span>

### <span data-ttu-id="bb480-147"><a name="accessingDatasets"></a>Datové sady přístup z místních aplikací Python</span><span class="sxs-lookup"><span data-stu-id="bb480-147"><a name="accessingDatasets"></a>Access datasets from a local Python application</span></span>
1. <span data-ttu-id="bb480-148">V nástroji Machine Learning Studio, klikněte na tlačítko **datové sady** hello navigačním panelu na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="bb480-148">In Machine Learning Studio, click **DATASETS** in hello navigation bar on hello left.</span></span>
2. <span data-ttu-id="bb480-149">Vyberte datovou sadu hello chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="bb480-149">Select hello dataset you would like tooaccess.</span></span> <span data-ttu-id="bb480-150">Vyberte některé z datové sady hello z hello **Moje datové sady** seznamu nebo z hello **UKÁZKY** seznamu.</span><span class="sxs-lookup"><span data-stu-id="bb480-150">You can select any of hello datasets from hello **MY DATASETS** list or from hello **SAMPLES** list.</span></span>
3. <span data-ttu-id="bb480-151">Z hello dolním panelu nástrojů, klikněte na tlačítko **generování dat přístupového kódu**.</span><span class="sxs-lookup"><span data-stu-id="bb480-151">From hello bottom toolbar, click **Generate Data Access Code**.</span></span> <span data-ttu-id="bb480-152">Je-li hello data ve formátu, který je kompatibilní s hello Python klientské knihovny, toto tlačítko k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bb480-152">If hello data is in a format incompatible with hello Python client library, this button is disabled.</span></span>
   
    ![Datové sady][datasets]
4. <span data-ttu-id="bb480-154">Vyberte hello fragmentu kódu zobrazeném okně hello a zkopírujte ho tooyour schránky.</span><span class="sxs-lookup"><span data-stu-id="bb480-154">Select hello code snippet from hello window that appears and copy it tooyour clipboard.</span></span>
   
    ![Přístupový kód][dataset-access-code]
5. <span data-ttu-id="bb480-156">Hello kód vložte do poznámkového bloku hello místní aplikace Python.</span><span class="sxs-lookup"><span data-stu-id="bb480-156">Paste hello code into hello notebook of your local Python application.</span></span>
   
    ![Poznámkový blok][ipython-dataset]

## <span data-ttu-id="bb480-158"><a name="accessingIntermediateDatasets"></a>Zprostředkující datové sady přístup z experimentů Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bb480-158"><a name="accessingIntermediateDatasets"></a>Access intermediate datasets from Machine Learning experiments</span></span>
<span data-ttu-id="bb480-159">Po spuštění experimentu v hello Machine Learning Studio, je možné tooaccess hello zprostředkující datové sady z uzlů výstup hello modulů.</span><span class="sxs-lookup"><span data-stu-id="bb480-159">After an experiment is run in hello Machine Learning Studio, it is possible tooaccess hello intermediate datasets from hello output nodes of modules.</span></span> <span data-ttu-id="bb480-160">Zprostředkující datové sady jsou data, která byla vytvořena a používá se pro zprostředkující kroky při byl spuštěn nástroj modelu.</span><span class="sxs-lookup"><span data-stu-id="bb480-160">Intermediate datasets are data that has been created and used for intermediate steps when a model tool has been run.</span></span>

<span data-ttu-id="bb480-161">Zprostředkující datové sady je přístupný, dokud hello formát dat je kompatibilní s knihovnou klienta Python hello.</span><span class="sxs-lookup"><span data-stu-id="bb480-161">Intermediate datasets can be accessed as long as hello data format is compatible with hello Python client library.</span></span>

<span data-ttu-id="bb480-162">Hello jsou podporovány následující formáty (konstanty pro tyto jsou v hello `azureml.DataTypeIds` třída):</span><span class="sxs-lookup"><span data-stu-id="bb480-162">hello following formats are supported (constants for these are in hello `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="bb480-163">Ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="bb480-163">PlainText</span></span>
* <span data-ttu-id="bb480-164">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="bb480-164">GenericCSV</span></span>
* <span data-ttu-id="bb480-165">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="bb480-165">GenericTSV</span></span>
* <span data-ttu-id="bb480-166">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="bb480-166">GenericCSVNoHeader</span></span>
* <span data-ttu-id="bb480-167">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="bb480-167">GenericTSVNoHeader</span></span>

<span data-ttu-id="bb480-168">Můžete určit formát hello podržením ukazatele nad uzlem výstup modulu.</span><span class="sxs-lookup"><span data-stu-id="bb480-168">You can determine hello format by hovering over a module output node.</span></span> <span data-ttu-id="bb480-169">Zobrazí se společně s hello název uzlu, v popisu tlačítka.</span><span class="sxs-lookup"><span data-stu-id="bb480-169">It is displayed along with hello node name, in a tooltip.</span></span>

<span data-ttu-id="bb480-170">Některé moduly hello, například hello [rozdělení] [ split] modulu, výstupní formát tooa s názvem `Dataset`, která není podporována hello Python klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="bb480-170">Some of hello modules, such as hello [Split][split] module, output tooa format named `Dataset`, which is not supported by hello Python client library.</span></span>

![Formát datové sady][dataset-format]

<span data-ttu-id="bb480-172">Je třeba toouse převodu modulu, například [převést tooCSV][convert-to-csv], tooget výstup do podporovaného formátu.</span><span class="sxs-lookup"><span data-stu-id="bb480-172">You need toouse a conversion module, such as [Convert tooCSV][convert-to-csv], tooget an output into a supported format.</span></span>

![Formát GenericCSV][csv-format]

<span data-ttu-id="bb480-174">Hello následující postup příklad, který vytvoří experimentu, ji spustí a přistupuje k hello zprostředkující datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="bb480-174">hello following steps show an example that creates an experiment, runs it and accesses hello intermediate dataset.</span></span>

1. <span data-ttu-id="bb480-175">Vytvoření nového experimentu.</span><span class="sxs-lookup"><span data-stu-id="bb480-175">Create a new experiment.</span></span>
2. <span data-ttu-id="bb480-176">Vložit **datovou sadu pro dospělé úplné zjišťování příjem binární klasifikace** modulu.</span><span class="sxs-lookup"><span data-stu-id="bb480-176">Insert an **Adult Census Income Binary Classification dataset** module.</span></span>
3. <span data-ttu-id="bb480-177">Vložení [rozdělení] [ split] modul a připojte její výstup modulu toohello vstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="bb480-177">Insert a [Split][split] module, and connect its input toohello dataset module output.</span></span>
4. <span data-ttu-id="bb480-178">Vložení [převést tooCSV] [ convert-to-csv] modulu a připojte jeho vstupní tooone hello [rozdělení] [ split] výstupy modulu.</span><span class="sxs-lookup"><span data-stu-id="bb480-178">Insert a [Convert tooCSV][convert-to-csv] module and connect its input tooone of hello [Split][split] module outputs.</span></span>
5. <span data-ttu-id="bb480-179">Uložit hello experiment, spouštět a počkejte na její toofinish systémem.</span><span class="sxs-lookup"><span data-stu-id="bb480-179">Save hello experiment, run it, and wait for it toofinish running.</span></span>
6. <span data-ttu-id="bb480-180">Klikněte na uzel výstup hello na hello [převést tooCSV] [ convert-to-csv] modulu.</span><span class="sxs-lookup"><span data-stu-id="bb480-180">Click hello output node on hello [Convert tooCSV][convert-to-csv] module.</span></span>
7. <span data-ttu-id="bb480-181">Jakmile se zobrazí hello kontextové nabídky, vyberte **generování dat přístupového kódu**.</span><span class="sxs-lookup"><span data-stu-id="bb480-181">When hello context menu appears, select **Generate Data Access Code**.</span></span>
   
    ![Kontextové nabídky][experiment]
8. <span data-ttu-id="bb480-183">Fragment kódu hello vyberte a zkopírujte ho tooyour schránky z okna hello, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bb480-183">Select hello code snippet and copy it tooyour clipboard from hello window that appears.</span></span>
   
    ![Přístupový kód][intermediate-dataset-access-code]
9. <span data-ttu-id="bb480-185">Vložte kód hello v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="bb480-185">Paste hello code in your notebook.</span></span>
   
    ![Poznámkový blok][ipython-intermediate-dataset]
10. <span data-ttu-id="bb480-187">Můžete vizualizovat data hello pomocí matplotlib.</span><span class="sxs-lookup"><span data-stu-id="bb480-187">You can visualize hello data using matplotlib.</span></span> <span data-ttu-id="bb480-188">Zobrazí se v histogram pro sloupec stáří hello:</span><span class="sxs-lookup"><span data-stu-id="bb480-188">This displays in a histogram for hello age column:</span></span>
    
    ![Histogram][ipython-histogram]

## <span data-ttu-id="bb480-190"><a name="clientApis"></a>Použít hello Machine Learning Python klienta knihovny tooaccess, číst, vytvářet a spravovat datové sady</span><span class="sxs-lookup"><span data-stu-id="bb480-190"><a name="clientApis"></a>Use hello Machine Learning Python client library tooaccess, read, create, and manage datasets</span></span>
### <a name="workspace"></a><span data-ttu-id="bb480-191">Pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="bb480-191">Workspace</span></span>
<span data-ttu-id="bb480-192">pracovní prostor Hello je hello vstupní bod pro hello Python klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="bb480-192">hello workspace is hello entry point for hello Python client library.</span></span> <span data-ttu-id="bb480-193">Zadejte hello `Workspace` třídy s vašeho pracovního prostoru id a autorizační token toocreate instance:</span><span class="sxs-lookup"><span data-stu-id="bb480-193">Provide hello `Workspace` class with your workspace id and authorization token toocreate an instance:</span></span>

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a><span data-ttu-id="bb480-194">Zobrazení výčtu datových sad</span><span class="sxs-lookup"><span data-stu-id="bb480-194">Enumerate datasets</span></span>
<span data-ttu-id="bb480-195">tooenumerate všechny datové sady v daném prostoru:</span><span class="sxs-lookup"><span data-stu-id="bb480-195">tooenumerate all datasets in a given workspace:</span></span>

    for ds in ws.datasets:
        print(ds.name)

<span data-ttu-id="bb480-196">tooenumerate jenom hello uživatelem vytvořené datové sady:</span><span class="sxs-lookup"><span data-stu-id="bb480-196">tooenumerate just hello user-created datasets:</span></span>

    for ds in ws.user_datasets:
        print(ds.name)

<span data-ttu-id="bb480-197">tooenumerate jenom hello příklad datové sady:</span><span class="sxs-lookup"><span data-stu-id="bb480-197">tooenumerate just hello example datasets:</span></span>

    for ds in ws.example_datasets:
        print(ds.name)

<span data-ttu-id="bb480-198">Datové sady se můžete dostat pomocí názvu (což je malá a velká písmena):</span><span class="sxs-lookup"><span data-stu-id="bb480-198">You can access a dataset by name (which is case-sensitive):</span></span>

    ds = ws.datasets['my dataset name']

<span data-ttu-id="bb480-199">Nebo můžete k němu přístup podle indexu:</span><span class="sxs-lookup"><span data-stu-id="bb480-199">Or you can access it by index:</span></span>

    ds = ws.datasets[0]


### <a name="metadata"></a><span data-ttu-id="bb480-200">Metadata</span><span class="sxs-lookup"><span data-stu-id="bb480-200">Metadata</span></span>
<span data-ttu-id="bb480-201">Datové sady mít metadata, přidání toocontent.</span><span class="sxs-lookup"><span data-stu-id="bb480-201">Datasets have metadata, in addition toocontent.</span></span> <span data-ttu-id="bb480-202">(Zprostředkující datové sady jsou toothis pravidlo výjimky a nemají žádné metadata.)</span><span class="sxs-lookup"><span data-stu-id="bb480-202">(Intermediate datasets are an exception toothis rule and do not have any metadata.)</span></span>

<span data-ttu-id="bb480-203">Některé hodnoty metadata jsou přiřazeny uživatelem hello v okamžiku vytvoření:</span><span class="sxs-lookup"><span data-stu-id="bb480-203">Some metadata values are assigned by hello user at creation time:</span></span>

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

<span data-ttu-id="bb480-204">Jiné jsou hodnoty přiřadila Azure ML:</span><span class="sxs-lookup"><span data-stu-id="bb480-204">Others are values assigned by Azure ML:</span></span>

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

<span data-ttu-id="bb480-205">V tématu hello `SourceDataset` třídu pro další na hello k dispozici metadata.</span><span class="sxs-lookup"><span data-stu-id="bb480-205">See hello `SourceDataset` class for more on hello available metadata.</span></span>

### <a name="read-contents"></a><span data-ttu-id="bb480-206">Načíst obsah</span><span class="sxs-lookup"><span data-stu-id="bb480-206">Read contents</span></span>
<span data-ttu-id="bb480-207">fragmenty kódu Hello poskytované Machine Learning Studio automaticky stáhnout a deserializovat objekt Pandas DataFrame tooa datovou sadu hello.</span><span class="sxs-lookup"><span data-stu-id="bb480-207">hello code snippets provided by Machine Learning Studio automatically download and deserialize hello dataset tooa Pandas DataFrame object.</span></span> <span data-ttu-id="bb480-208">To lze provést pomocí hello `to_dataframe` metoda:</span><span class="sxs-lookup"><span data-stu-id="bb480-208">This is done with hello `to_dataframe` method:</span></span>

    frame = ds.to_dataframe()

<span data-ttu-id="bb480-209">Pokud dáváte přednost toodownload hello nezpracovaná data a provést deserializaci hello sami, který je možnost.</span><span class="sxs-lookup"><span data-stu-id="bb480-209">If you prefer toodownload hello raw data, and perform hello deserialization yourself, that is an option.</span></span> <span data-ttu-id="bb480-210">Momentálně hello Toto je jediná možnost hello formátů, jako je například "ARFF', které hello Python klientské knihovny nelze deserializovat.</span><span class="sxs-lookup"><span data-stu-id="bb480-210">At hello moment, this is hello only option for formats such as 'ARFF', which hello Python client library cannot deserialize.</span></span>

<span data-ttu-id="bb480-211">obsah hello tooread jako text:</span><span class="sxs-lookup"><span data-stu-id="bb480-211">tooread hello contents as text:</span></span>

    text_data = ds.read_as_text()

<span data-ttu-id="bb480-212">obsah hello tooread jako binární:</span><span class="sxs-lookup"><span data-stu-id="bb480-212">tooread hello contents as binary:</span></span>

    binary_data = ds.read_as_binary()

<span data-ttu-id="bb480-213">Můžete otevřít také jenom datový proud toohello obsah:</span><span class="sxs-lookup"><span data-stu-id="bb480-213">You can also just open a stream toohello contents:</span></span>

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a><span data-ttu-id="bb480-214">Vytvořit novou sadu dat</span><span class="sxs-lookup"><span data-stu-id="bb480-214">Create a new dataset</span></span>
<span data-ttu-id="bb480-215">Klientská knihovna pro Python Hello umožňuje tooupload datové sady z programu Python.</span><span class="sxs-lookup"><span data-stu-id="bb480-215">hello Python client library allows you tooupload datasets from your Python program.</span></span> <span data-ttu-id="bb480-216">Tyto datové sady jsou pak k dispozici pro použití v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="bb480-216">These datasets are then available for use in your workspace.</span></span>

<span data-ttu-id="bb480-217">Pokud máte v Pandas DataFrame vaše data, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="bb480-217">If you have your data in a Pandas DataFrame, use hello following code:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="bb480-218">Pokud už je serializováno vaše data, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="bb480-218">If your data is already serialized, you can use:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="bb480-219">Hello Python klientské knihovny je možné tooserialize Pandas DataFrame toohello následující formáty (konstanty pro tyto jsou v hello `azureml.DataTypeIds` třída):</span><span class="sxs-lookup"><span data-stu-id="bb480-219">hello Python client library is able tooserialize a Pandas DataFrame toohello following formats (constants for these are in hello `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="bb480-220">Ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="bb480-220">PlainText</span></span>
* <span data-ttu-id="bb480-221">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="bb480-221">GenericCSV</span></span>
* <span data-ttu-id="bb480-222">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="bb480-222">GenericTSV</span></span>
* <span data-ttu-id="bb480-223">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="bb480-223">GenericCSVNoHeader</span></span>
* <span data-ttu-id="bb480-224">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="bb480-224">GenericTSVNoHeader</span></span>

### <a name="update-an-existing-dataset"></a><span data-ttu-id="bb480-225">Aktualizovat existující datovou sadu</span><span class="sxs-lookup"><span data-stu-id="bb480-225">Update an existing dataset</span></span>
<span data-ttu-id="bb480-226">Pokud se pokusíte tooupload novou datovou sadu s názvem, který odpovídá existující datovou sadu, měli byste obdržet chybu konfliktu.</span><span class="sxs-lookup"><span data-stu-id="bb480-226">If you try tooupload a new dataset with a name that matches an existing dataset, you should get a conflict error.</span></span>

<span data-ttu-id="bb480-227">tooupdate existující datovou sadu, musíte nejdřív tooget odkaz na datovou sadu existující toohello:</span><span class="sxs-lookup"><span data-stu-id="bb480-227">tooupdate an existing dataset, you first need tooget a reference toohello existing dataset:</span></span>

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="bb480-228">Potom pomocí `update_from_dataframe` tooserialize a nahraďte obsah hello hello datovou sadu v Azure:</span><span class="sxs-lookup"><span data-stu-id="bb480-228">Then use `update_from_dataframe` tooserialize and replace hello contents of hello dataset on Azure:</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="bb480-229">Pokud chcete tooserialize hello data tooa jiný formát, zadejte hodnotu pro hello volitelné `data_type_id` parametr.</span><span class="sxs-lookup"><span data-stu-id="bb480-229">If you want tooserialize hello data tooa different format, specify a value for hello optional `data_type_id` parameter.</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="bb480-230">Volitelně můžete nastavit nový popis zadáním hodnoty pro hello `description` parametr.</span><span class="sxs-lookup"><span data-stu-id="bb480-230">You can optionally set a new description by specifying a value for hello `description` parameter.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toofeb 2015'

<span data-ttu-id="bb480-231">Volitelně můžete nastavit nový název a zadat hodnotu pro hello `name` parametr.</span><span class="sxs-lookup"><span data-stu-id="bb480-231">You can optionally set a new name by specifying a value for hello `name` parameter.</span></span> <span data-ttu-id="bb480-232">Od této chvíle budete načíst datové sady hello pomocí hello pouze nový název.</span><span class="sxs-lookup"><span data-stu-id="bb480-232">From now on, you'll retrieve hello dataset using hello new name only.</span></span> <span data-ttu-id="bb480-233">Hello následující kód aktualizuje hello dat, název a popis.</span><span class="sxs-lookup"><span data-stu-id="bb480-233">hello following code updates hello data, name and description.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up toofeb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

<span data-ttu-id="bb480-234">Hello `data_type_id`, `name` a `description` parametry jsou volitelné a výchozí tootheir předchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bb480-234">hello `data_type_id`, `name` and `description` parameters are optional and default tootheir previous value.</span></span> <span data-ttu-id="bb480-235">Hello `dataframe` vždy parametr je povinný.</span><span class="sxs-lookup"><span data-stu-id="bb480-235">hello `dataframe` parameter is always required.</span></span>

<span data-ttu-id="bb480-236">Pokud už je serializováno vaše data, použijte `update_from_raw_data` místo `update_from_dataframe`.</span><span class="sxs-lookup"><span data-stu-id="bb480-236">If your data is already serialized, use `update_from_raw_data` instead of `update_from_dataframe`.</span></span> <span data-ttu-id="bb480-237">Pokud předáte právě v `raw_data` místo `dataframe`, funguje podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bb480-237">If you just pass in `raw_data` instead of  `dataframe`, it works in a similar way.</span></span>

<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

