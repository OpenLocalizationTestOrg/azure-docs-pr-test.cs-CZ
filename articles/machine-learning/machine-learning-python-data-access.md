---
title: "Přístup k datové sady s knihovnou klienta Machine Learning Python | Microsoft Docs"
description: "Nainstalovat a používat klientské knihovny Python přístup k datům a spravovat Azure Machine Learning bezpečně z místního prostředí Python."
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
ms.openlocfilehash: e3ae712e0f8d386f637520fbbff4b348bc86f32d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a><span data-ttu-id="c5a60-103">Přístup k datovým sadám pomocí Pythonu a klientské knihovny služby Azure Machine Learning pro Python</span><span class="sxs-lookup"><span data-stu-id="c5a60-103">Access datasets with Python using the Azure Machine Learning Python client library</span></span>
<span data-ttu-id="c5a60-104">Ve verzi preview Klientská knihovna pro Microsoft Azure Machine Learning Python můžete povolit zabezpečený přístup k vaší Azure Machine Learning datové sady z místního prostředí Python a umožňuje vytváření a správu datových sad v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="c5a60-104">The preview of Microsoft Azure Machine Learning Python client library can enable secure access to your Azure Machine Learning datasets from a local Python environment and enables the creation and management of datasets in a workspace.</span></span>

<span data-ttu-id="c5a60-105">Toto téma obsahuje pokyny o tom, jak:</span><span class="sxs-lookup"><span data-stu-id="c5a60-105">This topic provides instructions on how to:</span></span>

* <span data-ttu-id="c5a60-106">nainstaluje klientské knihovny pro Machine Learning Python</span><span class="sxs-lookup"><span data-stu-id="c5a60-106">install the Machine Learning Python client library</span></span> 
* <span data-ttu-id="c5a60-107">přístup a nahrání datové sady, včetně pokyny, jak získat oprávnění pro přístup k Azure Machine Learning datové sady z místního prostředí Python</span><span class="sxs-lookup"><span data-stu-id="c5a60-107">access and upload datasets, including instructions on how to get authorization to access Azure Machine Learning datasets from your local Python environment</span></span>
* <span data-ttu-id="c5a60-108">přístup k zprostředkující datové sady z experimentů</span><span class="sxs-lookup"><span data-stu-id="c5a60-108">access intermediate datasets from experiments</span></span>
* <span data-ttu-id="c5a60-109">Použijte klientskou knihovnu Python výčet datové sady, získat přístup k metadatům, přečíst obsah datové sady, vytvořit nové datové sady a aktualizovat existující datové sady</span><span class="sxs-lookup"><span data-stu-id="c5a60-109">use the Python client library to enumerate datasets, access metadata, read the contents of a dataset, create new datasets and update existing datasets</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <span data-ttu-id="c5a60-110"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5a60-110"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="c5a60-111">Klientská knihovna Python byl testován v následujících prostředích:</span><span class="sxs-lookup"><span data-stu-id="c5a60-111">The Python client library has been tested under the following environments:</span></span>

* <span data-ttu-id="c5a60-112">Windows, Mac a Linux</span><span class="sxs-lookup"><span data-stu-id="c5a60-112">Windows, Mac and Linux</span></span>
* <span data-ttu-id="c5a60-113">Python 2.7, 3.3 a 3.4</span><span class="sxs-lookup"><span data-stu-id="c5a60-113">Python 2.7, 3.3 and 3.4</span></span>

<span data-ttu-id="c5a60-114">Má závislost na následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="c5a60-114">It has a dependency on the following packages:</span></span>

* <span data-ttu-id="c5a60-115">požadavky</span><span class="sxs-lookup"><span data-stu-id="c5a60-115">requests</span></span>
* <span data-ttu-id="c5a60-116">Python dateutil</span><span class="sxs-lookup"><span data-stu-id="c5a60-116">python-dateutil</span></span>
* <span data-ttu-id="c5a60-117">pandas</span><span class="sxs-lookup"><span data-stu-id="c5a60-117">pandas</span></span>

<span data-ttu-id="c5a60-118">Doporučujeme použít distribuci jazyka Python, jako [Anaconda](http://continuum.io/downloads#all) nebo [zápoje](https://store.enthought.com/downloads/), které jsou součástí Python, IPython a nainstalované tři balíčky uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="c5a60-118">We recommend using a Python distribution such as [Anaconda](http://continuum.io/downloads#all) or [Canopy](https://store.enthought.com/downloads/), which come with Python, IPython and the three packages listed above installed.</span></span> <span data-ttu-id="c5a60-119">I když IPython není nezbytně nutné, je skvělé prostředí pro manipulace a vizualizace dat interaktivně.</span><span class="sxs-lookup"><span data-stu-id="c5a60-119">Although IPython is not strictly required, it is a great environment for manipulating and visualizing data interactively.</span></span>

### <span data-ttu-id="c5a60-120"><a name="installation"></a>Postup instalace knihovnu klienta služby Azure Machine Learning Python</span><span class="sxs-lookup"><span data-stu-id="c5a60-120"><a name="installation"></a>How to install the Azure Machine Learning Python client library</span></span>
<span data-ttu-id="c5a60-121">Knihovnu klienta služby Azure Machine Learning Python musí být nainstalován také dokončit úlohy popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-121">The Azure Machine Learning Python client library must also be installed to complete the tasks outlined in this topic.</span></span> <span data-ttu-id="c5a60-122">Je k dispozici z [indexu balíčků Pythonu](https://pypi.python.org/pypi/azureml).</span><span class="sxs-lookup"><span data-stu-id="c5a60-122">It is available from the [Python Package Index](https://pypi.python.org/pypi/azureml).</span></span> <span data-ttu-id="c5a60-123">K její instalaci ve vašem prostředí Python, spusťte následující příkaz z místního prostředí Python:</span><span class="sxs-lookup"><span data-stu-id="c5a60-123">To install it in your Python environment, run the following command from your local Python environment:</span></span>

    pip install azureml

<span data-ttu-id="c5a60-124">Alternativně můžete stáhnout a nainstalovat ze zdroje na [githubu](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span><span class="sxs-lookup"><span data-stu-id="c5a60-124">Alternatively, you can download and install from the sources on [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span></span>

    python setup.py install

<span data-ttu-id="c5a60-125">Pokud máte nainstalovaný na počítači git, můžete instalovat přímo z úložiště git pip:</span><span class="sxs-lookup"><span data-stu-id="c5a60-125">If you have git installed on your machine, you can use pip to install directly from the git repository:</span></span>

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <span data-ttu-id="c5a60-126"><a name="datasetAccess"></a>Fragmenty kódu Studio použít pro přístup k datové sady</span><span class="sxs-lookup"><span data-stu-id="c5a60-126"><a name="datasetAccess"></a>Use Studio Code snippets to access datasets</span></span>
<span data-ttu-id="c5a60-127">Klientská knihovna Python umožňuje programový přístup k vaší stávající datové sady z experimenty, které byly spuštěny.</span><span class="sxs-lookup"><span data-stu-id="c5a60-127">The Python client library gives you programmatic access to your existing datasets from experiments that have been run.</span></span>

<span data-ttu-id="c5a60-128">Z webové rozhraní Studio můžete vygenerovat fragmenty kódu, které zahrnují všechny potřebné informace ke stažení a deserializovat datové sady jako objekty Pandas DataFrame na počítači pro umístění.</span><span class="sxs-lookup"><span data-stu-id="c5a60-128">From the Studio web interface, you can generate code snippets that include all the necessary information to download and deserialize datasets as Pandas DataFrame objects on your location machine.</span></span>

### <span data-ttu-id="c5a60-129"><a name="security"></a>Zabezpečení pro přístup k datům</span><span class="sxs-lookup"><span data-stu-id="c5a60-129"><a name="security"></a>Security for data access</span></span>
<span data-ttu-id="c5a60-130">Fragmenty kódu poskytované Studio pro použití s knihovnou klienta Python zahrnují id pracovního prostoru a autorizační token.</span><span class="sxs-lookup"><span data-stu-id="c5a60-130">The code snippets provided by Studio for use with the Python client library includes your workspace id and authorization token.</span></span> <span data-ttu-id="c5a60-131">Tyto poskytnutí plného přístupu do pracovního prostoru a musí být chráněny, jako je heslo.</span><span class="sxs-lookup"><span data-stu-id="c5a60-131">These provide full access to your workspace and must be protected, like a password.</span></span>

<span data-ttu-id="c5a60-132">Z bezpečnostních důvodů funkce fragmentu kódu je k dispozici pouze uživatelé, kteří mají roli nastavit jako **vlastníka** pro pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="c5a60-132">For security reasons, the code snippet functionality is only available to users that have their role set as **Owner** for the workspace.</span></span> <span data-ttu-id="c5a60-133">Vaše role se zobrazí v Azure Machine Learning Studio na **uživatelé** v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c5a60-133">Your role is displayed in Azure Machine Learning Studio on the **USERS** page under **Settings**.</span></span>

![Zabezpečení][security]

<span data-ttu-id="c5a60-135">Pokud vaše role není nastaven jako **vlastníka**, můžete buď žádost o nového pozvání jako vlastníka, nebo požádejte vlastníka pracovního prostoru, kde přinášejí fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-135">If your role is not set as **Owner**, you can either request to be reinvited as an owner, or ask the owner of the workspace to provide you with the code snippet.</span></span>

<span data-ttu-id="c5a60-136">Pokud chcete získat autorizačním tokenem, můžete provést jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="c5a60-136">To obtain the authorization token, you can do one of the following:</span></span>

* <span data-ttu-id="c5a60-137">Požádejte o token z vlastníka.</span><span class="sxs-lookup"><span data-stu-id="c5a60-137">Ask for a token from an owner.</span></span> <span data-ttu-id="c5a60-138">Přístup k nim vlastníci jejich autorizace tokeny na stránce nastavení jejich pracovního prostoru v Studio.</span><span class="sxs-lookup"><span data-stu-id="c5a60-138">Owners can access their authorization tokens from the Settings page of their workspace in Studio.</span></span> <span data-ttu-id="c5a60-139">Vyberte **nastavení** levém podokně a klikněte na **AUTORIZACE TOKENY** zobrazíte primární a sekundární tokenů.</span><span class="sxs-lookup"><span data-stu-id="c5a60-139">Select **Settings** from the left pane and click **AUTHORIZATION TOKENS** to see the primary and secondary tokens.</span></span>  <span data-ttu-id="c5a60-140">Přestože primární nebo sekundární autorizace tokeny můžete použít ve fragmentu kódu, se doporučuje vlastníky sdílet jenom tokeny sekundární ověřování.</span><span class="sxs-lookup"><span data-stu-id="c5a60-140">Although either the primary or the secondary authorization tokens can be used in the code snippet, it is recommended that owners only share the secondary authorization tokens.</span></span>

![Tokeny ověřování](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* <span data-ttu-id="c5a60-142">Požádejte o povýšit na roli vlastníka.</span><span class="sxs-lookup"><span data-stu-id="c5a60-142">Ask to be promoted to role of owner.</span></span>  <span data-ttu-id="c5a60-143">Aktuální vlastník pracovního prostoru musí k tomu, odeberte nejprve můžete z pracovního prostoru a poté znovu vás pozval k němu jako vlastníka.</span><span class="sxs-lookup"><span data-stu-id="c5a60-143">To do this, a current owner of the workspace needs to first remove you from the workspace then re-invite you to it as an owner.</span></span>

<span data-ttu-id="c5a60-144">Po získání vývojáři id pracovního prostoru a autorizace tokenu, budou mít přístup k pracovním prostoru pomocí fragmentu kódu bez ohledu na jejich role.</span><span class="sxs-lookup"><span data-stu-id="c5a60-144">Once developers have obtained the workspace id and authorization token, they are able to access the workspace using the code snippet regardless of their role.</span></span>

<span data-ttu-id="c5a60-145">Tokeny ověřování jsou spravovány v **AUTORIZACE TOKENY** v části **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c5a60-145">Authorization tokens are managed on the **AUTHORIZATION TOKENS** page under **SETTINGS**.</span></span> <span data-ttu-id="c5a60-146">Můžete je obnovit, ale tento postup odebere přístup k předchozím tokenem.</span><span class="sxs-lookup"><span data-stu-id="c5a60-146">You can regenerate them, but this procedure revokes access to the previous token.</span></span>

### <span data-ttu-id="c5a60-147"><a name="accessingDatasets"></a>Datové sady přístup z místních aplikací Python</span><span class="sxs-lookup"><span data-stu-id="c5a60-147"><a name="accessingDatasets"></a>Access datasets from a local Python application</span></span>
1. <span data-ttu-id="c5a60-148">V nástroji Machine Learning Studio, klikněte na tlačítko **datové sady** na navigačním panelu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="c5a60-148">In Machine Learning Studio, click **DATASETS** in the navigation bar on the left.</span></span>
2. <span data-ttu-id="c5a60-149">Vyberte datovou sadu, kterou jste chtěli přístup.</span><span class="sxs-lookup"><span data-stu-id="c5a60-149">Select the dataset you would like to access.</span></span> <span data-ttu-id="c5a60-150">Můžete vybrat libovolný datových sad z **Moje datové sady** seznamu nebo z **UKÁZKY** seznamu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-150">You can select any of the datasets from the **MY DATASETS** list or from the **SAMPLES** list.</span></span>
3. <span data-ttu-id="c5a60-151">V dolním panelu nástrojů klikněte na **generování dat přístupového kódu**.</span><span class="sxs-lookup"><span data-stu-id="c5a60-151">From the bottom toolbar, click **Generate Data Access Code**.</span></span> <span data-ttu-id="c5a60-152">Pokud jsou data ve formátu, který je kompatibilní s knihovnou klienta Python, toto tlačítko k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c5a60-152">If the data is in a format incompatible with the Python client library, this button is disabled.</span></span>
   
    ![Datové sady][datasets]
4. <span data-ttu-id="c5a60-154">Vyberte fragmentu kódu zobrazeném okně, které se zobrazí a zkopírujte jej do schránky.</span><span class="sxs-lookup"><span data-stu-id="c5a60-154">Select the code snippet from the window that appears and copy it to your clipboard.</span></span>
   
    ![Přístupový kód][dataset-access-code]
5. <span data-ttu-id="c5a60-156">Kód vložte do poznámkového bloku místní aplikace Python.</span><span class="sxs-lookup"><span data-stu-id="c5a60-156">Paste the code into the notebook of your local Python application.</span></span>
   
    ![Poznámkový blok][ipython-dataset]

## <span data-ttu-id="c5a60-158"><a name="accessingIntermediateDatasets"></a>Zprostředkující datové sady přístup z experimentů Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c5a60-158"><a name="accessingIntermediateDatasets"></a>Access intermediate datasets from Machine Learning experiments</span></span>
<span data-ttu-id="c5a60-159">Po spuštění experimentu v nástroji Machine Learning Studio, je možné k zprostředkující datové sady z výstupních uzlů modulů.</span><span class="sxs-lookup"><span data-stu-id="c5a60-159">After an experiment is run in the Machine Learning Studio, it is possible to access the intermediate datasets from the output nodes of modules.</span></span> <span data-ttu-id="c5a60-160">Zprostředkující datové sady jsou data, která byla vytvořena a používá se pro zprostředkující kroky při byl spuštěn nástroj modelu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-160">Intermediate datasets are data that has been created and used for intermediate steps when a model tool has been run.</span></span>

<span data-ttu-id="c5a60-161">Zprostředkující datové sady je přístupný, dokud formát dat je kompatibilní s knihovnou klienta Python.</span><span class="sxs-lookup"><span data-stu-id="c5a60-161">Intermediate datasets can be accessed as long as the data format is compatible with the Python client library.</span></span>

<span data-ttu-id="c5a60-162">Jsou podporovány následující formáty (jsou konstanty pro tyto `azureml.DataTypeIds` třída):</span><span class="sxs-lookup"><span data-stu-id="c5a60-162">The following formats are supported (constants for these are in the `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="c5a60-163">Ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="c5a60-163">PlainText</span></span>
* <span data-ttu-id="c5a60-164">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="c5a60-164">GenericCSV</span></span>
* <span data-ttu-id="c5a60-165">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="c5a60-165">GenericTSV</span></span>
* <span data-ttu-id="c5a60-166">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="c5a60-166">GenericCSVNoHeader</span></span>
* <span data-ttu-id="c5a60-167">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="c5a60-167">GenericTSVNoHeader</span></span>

<span data-ttu-id="c5a60-168">Můžete určit formát podržením ukazatele nad uzlem výstup modulu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-168">You can determine the format by hovering over a module output node.</span></span> <span data-ttu-id="c5a60-169">Zobrazí se společně s název uzlu, v popisu tlačítka.</span><span class="sxs-lookup"><span data-stu-id="c5a60-169">It is displayed along with the node name, in a tooltip.</span></span>

<span data-ttu-id="c5a60-170">Některé moduly, například [rozdělení] [ split] modulu, výstup do formátu s názvem `Dataset`, který nepodporuje Python klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="c5a60-170">Some of the modules, such as the [Split][split] module, output to a format named `Dataset`, which is not supported by the Python client library.</span></span>

![Formát datové sady][dataset-format]

<span data-ttu-id="c5a60-172">Budete muset použít převod modulu, například [převést na sdílený svazek clusteru][convert-to-csv], chcete-li získat výstup do podporovaného formátu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-172">You need to use a conversion module, such as [Convert to CSV][convert-to-csv], to get an output into a supported format.</span></span>

![Formát GenericCSV][csv-format]

<span data-ttu-id="c5a60-174">Následující postup ukazuje příklad, který vytvoří experimentu, ji spustí a přistupuje k zprostředkující datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-174">The following steps show an example that creates an experiment, runs it and accesses the intermediate dataset.</span></span>

1. <span data-ttu-id="c5a60-175">Vytvoření nového experimentu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-175">Create a new experiment.</span></span>
2. <span data-ttu-id="c5a60-176">Vložit **datovou sadu pro dospělé úplné zjišťování příjem binární klasifikace** modulu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-176">Insert an **Adult Census Income Binary Classification dataset** module.</span></span>
3. <span data-ttu-id="c5a60-177">Vložení [rozdělení] [ split] modul a připojte se k výstupu modulu datová sada vstupem.</span><span class="sxs-lookup"><span data-stu-id="c5a60-177">Insert a [Split][split] module, and connect its input to the dataset module output.</span></span>
4. <span data-ttu-id="c5a60-178">Vložení [převést na sdílený svazek clusteru] [ convert-to-csv] modulu a připojte se vstupem do jednoho z [rozdělení] [ split] výstupy modulu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-178">Insert a [Convert to CSV][convert-to-csv] module and connect its input to one of the [Split][split] module outputs.</span></span>
5. <span data-ttu-id="c5a60-179">Uložte experiment, spouštět a počkejte na dokončení spuštění.</span><span class="sxs-lookup"><span data-stu-id="c5a60-179">Save the experiment, run it, and wait for it to finish running.</span></span>
6. <span data-ttu-id="c5a60-180">Klikněte na uzel výstup na [převést na sdílený svazek clusteru] [ convert-to-csv] modulu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-180">Click the output node on the [Convert to CSV][convert-to-csv] module.</span></span>
7. <span data-ttu-id="c5a60-181">Po zobrazení v místní nabídce vyberte **generování dat přístupového kódu**.</span><span class="sxs-lookup"><span data-stu-id="c5a60-181">When the context menu appears, select **Generate Data Access Code**.</span></span>
   
    ![Kontextové nabídky][experiment]
8. <span data-ttu-id="c5a60-183">Vyberte fragmentu kódu a zkopírujte jej do schránky z okna, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c5a60-183">Select the code snippet and copy it to your clipboard from the window that appears.</span></span>
   
    ![Přístupový kód][intermediate-dataset-access-code]
9. <span data-ttu-id="c5a60-185">Vložte kód v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="c5a60-185">Paste the code in your notebook.</span></span>
   
    ![Poznámkový blok][ipython-intermediate-dataset]
10. <span data-ttu-id="c5a60-187">Můžete vizualizovat data pomocí matplotlib.</span><span class="sxs-lookup"><span data-stu-id="c5a60-187">You can visualize the data using matplotlib.</span></span> <span data-ttu-id="c5a60-188">Zobrazí se v histogram stáří sloupce:</span><span class="sxs-lookup"><span data-stu-id="c5a60-188">This displays in a histogram for the age column:</span></span>
    
    ![Histogram][ipython-histogram]

## <span data-ttu-id="c5a60-190"><a name="clientApis"></a>Použijte klientskou knihovnu Machine Learning Python přístup, číst, vytvářet a spravovat datové sady</span><span class="sxs-lookup"><span data-stu-id="c5a60-190"><a name="clientApis"></a>Use the Machine Learning Python client library to access, read, create, and manage datasets</span></span>
### <a name="workspace"></a><span data-ttu-id="c5a60-191">Pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="c5a60-191">Workspace</span></span>
<span data-ttu-id="c5a60-192">Pracovní prostor je vstupní bod pro klientská knihovna Python.</span><span class="sxs-lookup"><span data-stu-id="c5a60-192">The workspace is the entry point for the Python client library.</span></span> <span data-ttu-id="c5a60-193">Zadejte `Workspace` s id pracovního prostoru a autorizační token pro vytvoření instance:</span><span class="sxs-lookup"><span data-stu-id="c5a60-193">Provide the `Workspace` class with your workspace id and authorization token to create an instance:</span></span>

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a><span data-ttu-id="c5a60-194">Zobrazení výčtu datových sad</span><span class="sxs-lookup"><span data-stu-id="c5a60-194">Enumerate datasets</span></span>
<span data-ttu-id="c5a60-195">Výčet všech datových sad v daném prostoru:</span><span class="sxs-lookup"><span data-stu-id="c5a60-195">To enumerate all datasets in a given workspace:</span></span>

    for ds in ws.datasets:
        print(ds.name)

<span data-ttu-id="c5a60-196">Výčet právě vytvořené uživatelem datové sady:</span><span class="sxs-lookup"><span data-stu-id="c5a60-196">To enumerate just the user-created datasets:</span></span>

    for ds in ws.user_datasets:
        print(ds.name)

<span data-ttu-id="c5a60-197">Chcete získat výčet jenom datové sady příklad:</span><span class="sxs-lookup"><span data-stu-id="c5a60-197">To enumerate just the example datasets:</span></span>

    for ds in ws.example_datasets:
        print(ds.name)

<span data-ttu-id="c5a60-198">Datové sady se můžete dostat pomocí názvu (což je malá a velká písmena):</span><span class="sxs-lookup"><span data-stu-id="c5a60-198">You can access a dataset by name (which is case-sensitive):</span></span>

    ds = ws.datasets['my dataset name']

<span data-ttu-id="c5a60-199">Nebo můžete k němu přístup podle indexu:</span><span class="sxs-lookup"><span data-stu-id="c5a60-199">Or you can access it by index:</span></span>

    ds = ws.datasets[0]


### <a name="metadata"></a><span data-ttu-id="c5a60-200">Metadata</span><span class="sxs-lookup"><span data-stu-id="c5a60-200">Metadata</span></span>
<span data-ttu-id="c5a60-201">Datové sady mít metadata, kromě obsahu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-201">Datasets have metadata, in addition to content.</span></span> <span data-ttu-id="c5a60-202">(Zprostředkující datové sady představují výjimku toto pravidlo a nemají žádné metadata.)</span><span class="sxs-lookup"><span data-stu-id="c5a60-202">(Intermediate datasets are an exception to this rule and do not have any metadata.)</span></span>

<span data-ttu-id="c5a60-203">Některé hodnoty metadata jsou přiřazeny uživatelem v okamžiku vytvoření:</span><span class="sxs-lookup"><span data-stu-id="c5a60-203">Some metadata values are assigned by the user at creation time:</span></span>

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

<span data-ttu-id="c5a60-204">Jiné jsou hodnoty přiřadila Azure ML:</span><span class="sxs-lookup"><span data-stu-id="c5a60-204">Others are values assigned by Azure ML:</span></span>

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

<span data-ttu-id="c5a60-205">Najdete v článku `SourceDataset` třídy pro další informace o dostupných metadata.</span><span class="sxs-lookup"><span data-stu-id="c5a60-205">See the `SourceDataset` class for more on the available metadata.</span></span>

### <a name="read-contents"></a><span data-ttu-id="c5a60-206">Načíst obsah</span><span class="sxs-lookup"><span data-stu-id="c5a60-206">Read contents</span></span>
<span data-ttu-id="c5a60-207">Fragmenty kódu poskytované Machine Learning Studio automaticky stáhnout a deserializaci na objekt Pandas DataFrame datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-207">The code snippets provided by Machine Learning Studio automatically download and deserialize the dataset to a Pandas DataFrame object.</span></span> <span data-ttu-id="c5a60-208">To lze provést pomocí `to_dataframe` metoda:</span><span class="sxs-lookup"><span data-stu-id="c5a60-208">This is done with the `to_dataframe` method:</span></span>

    frame = ds.to_dataframe()

<span data-ttu-id="c5a60-209">Pokud dáváte přednost stáhnout nezpracovaná data a provádět deserializace sami, který je možnost.</span><span class="sxs-lookup"><span data-stu-id="c5a60-209">If you prefer to download the raw data, and perform the deserialization yourself, that is an option.</span></span> <span data-ttu-id="c5a60-210">V tuto chvíli Toto je jediná možnost pro formáty například 'ARFF', které nelze deserializovat Klientská knihovna Python.</span><span class="sxs-lookup"><span data-stu-id="c5a60-210">At the moment, this is the only option for formats such as 'ARFF', which the Python client library cannot deserialize.</span></span>

<span data-ttu-id="c5a60-211">Číst obsah jako text:</span><span class="sxs-lookup"><span data-stu-id="c5a60-211">To read the contents as text:</span></span>

    text_data = ds.read_as_text()

<span data-ttu-id="c5a60-212">Číst obsah jako binární:</span><span class="sxs-lookup"><span data-stu-id="c5a60-212">To read the contents as binary:</span></span>

    binary_data = ds.read_as_binary()

<span data-ttu-id="c5a60-213">Můžete také stačí otevřít datový proud do obsahu:</span><span class="sxs-lookup"><span data-stu-id="c5a60-213">You can also just open a stream to the contents:</span></span>

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a><span data-ttu-id="c5a60-214">Vytvořit novou sadu dat</span><span class="sxs-lookup"><span data-stu-id="c5a60-214">Create a new dataset</span></span>
<span data-ttu-id="c5a60-215">Klientská knihovna Python vám umožní nahrát datové sady z programu Python.</span><span class="sxs-lookup"><span data-stu-id="c5a60-215">The Python client library allows you to upload datasets from your Python program.</span></span> <span data-ttu-id="c5a60-216">Tyto datové sady jsou pak k dispozici pro použití v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="c5a60-216">These datasets are then available for use in your workspace.</span></span>

<span data-ttu-id="c5a60-217">Pokud máte v Pandas DataFrame vaše data, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="c5a60-217">If you have your data in a Pandas DataFrame, use the following code:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="c5a60-218">Pokud už je serializováno vaše data, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="c5a60-218">If your data is already serialized, you can use:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="c5a60-219">Klientská knihovna Python je schopen serializovat Pandas DataFrame do následujících formátů (konstanty pro tyto jsou v `azureml.DataTypeIds` třída):</span><span class="sxs-lookup"><span data-stu-id="c5a60-219">The Python client library is able to serialize a Pandas DataFrame to the following formats (constants for these are in the `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="c5a60-220">Ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="c5a60-220">PlainText</span></span>
* <span data-ttu-id="c5a60-221">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="c5a60-221">GenericCSV</span></span>
* <span data-ttu-id="c5a60-222">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="c5a60-222">GenericTSV</span></span>
* <span data-ttu-id="c5a60-223">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="c5a60-223">GenericCSVNoHeader</span></span>
* <span data-ttu-id="c5a60-224">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="c5a60-224">GenericTSVNoHeader</span></span>

### <a name="update-an-existing-dataset"></a><span data-ttu-id="c5a60-225">Aktualizovat existující datovou sadu</span><span class="sxs-lookup"><span data-stu-id="c5a60-225">Update an existing dataset</span></span>
<span data-ttu-id="c5a60-226">Pokud se pokusíte odeslat novou datovou sadu s názvem, který odpovídá existující datovou sadu, měli byste obdržet chybu konfliktu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-226">If you try to upload a new dataset with a name that matches an existing dataset, you should get a conflict error.</span></span>

<span data-ttu-id="c5a60-227">Pokud chcete aktualizovat existující datovou sadu, musíte nejprve získat odkaz na existující datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="c5a60-227">To update an existing dataset, you first need to get a reference to the existing dataset:</span></span>

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="c5a60-228">Potom pomocí `update_from_dataframe` k serializaci a nahraďte jeho obsah datové sady v Azure:</span><span class="sxs-lookup"><span data-stu-id="c5a60-228">Then use `update_from_dataframe` to serialize and replace the contents of the dataset on Azure:</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="c5a60-229">Pokud chcete k serializaci dat do jiného formátu, zadejte hodnotu pro volitelné `data_type_id` parametr.</span><span class="sxs-lookup"><span data-stu-id="c5a60-229">If you want to serialize the data to a different format, specify a value for the optional `data_type_id` parameter.</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="c5a60-230">Volitelně můžete nastavit nový popis zadáním hodnoty pro `description` parametr.</span><span class="sxs-lookup"><span data-stu-id="c5a60-230">You can optionally set a new description by specifying a value for the `description` parameter.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

<span data-ttu-id="c5a60-231">Volitelně můžete nastavit nový název tak, že zadáte hodnotu `name` parametr.</span><span class="sxs-lookup"><span data-stu-id="c5a60-231">You can optionally set a new name by specifying a value for the `name` parameter.</span></span> <span data-ttu-id="c5a60-232">Od této chvíle budete načíst datové sady pomocí nového názvu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-232">From now on, you'll retrieve the dataset using the new name only.</span></span> <span data-ttu-id="c5a60-233">Následující kód aktualizuje data, název a popis.</span><span class="sxs-lookup"><span data-stu-id="c5a60-233">The following code updates the data, name and description.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

<span data-ttu-id="c5a60-234">`data_type_id`, `name` a `description` parametry jsou volitelné a výchozí jejich předchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c5a60-234">The `data_type_id`, `name` and `description` parameters are optional and default to their previous value.</span></span> <span data-ttu-id="c5a60-235">`dataframe` Vždy parametr je povinný.</span><span class="sxs-lookup"><span data-stu-id="c5a60-235">The `dataframe` parameter is always required.</span></span>

<span data-ttu-id="c5a60-236">Pokud už je serializováno vaše data, použijte `update_from_raw_data` místo `update_from_dataframe`.</span><span class="sxs-lookup"><span data-stu-id="c5a60-236">If your data is already serialized, use `update_from_raw_data` instead of `update_from_dataframe`.</span></span> <span data-ttu-id="c5a60-237">Pokud předáte právě v `raw_data` místo `dataframe`, funguje podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c5a60-237">If you just pass in `raw_data` instead of  `dataframe`, it works in a similar way.</span></span>

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

