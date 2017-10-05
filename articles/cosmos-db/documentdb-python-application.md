---
title: "Kurz webové aplikace Python Flask pro službu Azure Cosmos DB | Dokumentace Microsoftu"
description: "Projděte si databázový kurz na téma, jak pomocí služby Azure Cosmos DB ukládat data a přistupovat k nim z webové aplikace Python Flask hostované v Azure. Naleznete zde řešení pro vývoj aplikací."
keywords: "Vývoj aplikací, python flask, webová aplikace python, vývoj pro web python"
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/09/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed5284b5a265840c43dbc9890082a7c038d22975
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a><span data-ttu-id="d9755-105">Sestavení webové aplikace Python Flask využívající službu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d9755-105">Build a Python Flask web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d9755-106">.NET</span><span class="sxs-lookup"><span data-stu-id="d9755-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="d9755-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="d9755-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="d9755-108">Java</span><span class="sxs-lookup"><span data-stu-id="d9755-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="d9755-109">Python</span><span class="sxs-lookup"><span data-stu-id="d9755-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="d9755-110">V tomto kurzu se dozvíte, jak pomocí služby Azure Cosmos DB zajistit ukládání a přístup k datům z webové aplikace Python hostované v Azure. Předpokládá se, že máte zkušenosti s používáním Pythonu a služby Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="d9755-110">This tutorial shows you how to use Azure Cosmos DB to store and access data from a Python web application hosted on Azure and presumes that you have some prior experience using Python and Azure websites.</span></span>

<span data-ttu-id="d9755-111">Tento databázový kurz se zabývá:</span><span class="sxs-lookup"><span data-stu-id="d9755-111">This database tutorial covers:</span></span>

1. <span data-ttu-id="d9755-112">Vytváření a zřizování účtu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9755-112">Creating and provisioning a Cosmos DB account.</span></span>
2. <span data-ttu-id="d9755-113">Vytvoření aplikace Python Flask.</span><span class="sxs-lookup"><span data-stu-id="d9755-113">Creating a Python Flask application.</span></span>
3. <span data-ttu-id="d9755-114">Připojením ke službě Cosmos DB a jejím používáním z webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d9755-114">Connecting to and using Cosmos DB from your web application.</span></span>
4. <span data-ttu-id="d9755-115">Nasazení webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="d9755-115">Deploying the web application to Azure.</span></span>

<span data-ttu-id="d9755-116">Podle postupu uvedeného v tomto kurzu sestavíte jednoduchou hlasovací aplikaci, která vám umožní hlasovat v anketě.</span><span class="sxs-lookup"><span data-stu-id="d9755-116">By following this tutorial, you will build a simple voting application that allows you to vote for a poll.</span></span>

![Snímek obrazovky hlasovací aplikace vytvořené v tomto kurzu databáze](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a><span data-ttu-id="d9755-118">Předpoklady pro databázový kurz</span><span class="sxs-lookup"><span data-stu-id="d9755-118">Database tutorial prerequisites</span></span>
<span data-ttu-id="d9755-119">Než budete postupovat podle pokynů tohoto článku, měli byste se ujistit, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="d9755-119">Before following the instructions in this article, you should ensure that you have the following installed:</span></span>

* <span data-ttu-id="d9755-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d9755-120">An active Azure account.</span></span> <span data-ttu-id="d9755-121">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="d9755-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d9755-122">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9755-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
 
    <span data-ttu-id="d9755-123">NEBO</span><span class="sxs-lookup"><span data-stu-id="d9755-123">OR</span></span> 

    <span data-ttu-id="d9755-124">Místní instalaci [emulátoru služby Azure Cosmos DB](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="d9755-124">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="d9755-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d9755-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="d9755-126">[Python Tools pro Visual Studio](https://github.com/Microsoft/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="d9755-126">[Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span></span>  
* <span data-ttu-id="d9755-127">[Microsoft Azure SDK pro Python 2.7](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d9755-127">[Microsoft Azure SDK for Python 2.7](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="d9755-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="d9755-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="d9755-129">Pokud Python 2.7 instalujete poprvé, ujistěte se, že na obrazovce přizpůsobit Python 2.7.13 vyberete **přidat python.exe do cesty**.</span><span class="sxs-lookup"><span data-stu-id="d9755-129">If you are installing Python 2.7 for the first time, ensure that in the Customize Python 2.7.13 screen, you select **Add python.exe to Path**.</span></span>
> 
> ![Snímek obrazovky Customize Python 2.7.11 (Přizpůsobit Python 2.7.11), kde je nutné vybrat možnost Add python.exe to Path (Přidat python.exe do proměnné PATH)](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* <span data-ttu-id="d9755-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span><span class="sxs-lookup"><span data-stu-id="d9755-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="d9755-132">Krok 1: Vytvoření účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d9755-132">Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="d9755-133">Začněme vytvořením účtu služby Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9755-133">Let's start by creating an Cosmos DB account.</span></span> <span data-ttu-id="d9755-134">Pokud již účet máte nebo pokud používáte pro účely tohoto kurzu emulátor služby Azure Cosmos DB, můžete přeskočit na [Krok 2: Vytvoření nové webové aplikace Python Flask](#step-2-create-a-new-python-flask-web-application).</span><span class="sxs-lookup"><span data-stu-id="d9755-134">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create a new Python Flask web application](#step-2-create-a-new-python-flask-web-application).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
<span data-ttu-id="d9755-135">Nyní vám ukážeme, jak od základů vytvořit novou webovou aplikaci Python Flask.</span><span class="sxs-lookup"><span data-stu-id="d9755-135">We will now walk through how to create a new Python Flask web application from the ground up.</span></span>

## <a name="step-2-create-a-new-python-flask-web-application"></a><span data-ttu-id="d9755-136">Krok 2: Vytvoření nové webové aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="d9755-136">Step 2: Create a new Python Flask web application</span></span>
1. <span data-ttu-id="d9755-137">V nástroji Visual Studio najeďte myší v nabídce **Soubor** na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d9755-137">In Visual Studio, on the **File** menu, point to **New**, and then click **Project**.</span></span>
   
    <span data-ttu-id="d9755-138">Zobrazí se dialogové okno **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="d9755-138">The **New Project** dialog box appears.</span></span>
2. <span data-ttu-id="d9755-139">V levém podokně rozbalte **Šablony**, pak **Python** a klikněte na **Web**.</span><span class="sxs-lookup"><span data-stu-id="d9755-139">In the left pane, expand **Templates** and then **Python**, and then click **Web**.</span></span> 
3. <span data-ttu-id="d9755-140">Ve středním podokně vyberte **Webový projekt Flask**, do pole **Název** zadejte **tutorial** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9755-140">Select **Flask  Web Project** in the center pane, then in the **Name** box type **tutorial**, and then click **OK**.</span></span> <span data-ttu-id="d9755-141">Nezapomeňte, že názvy balíčků Python by měly být celé malými písmeny, jak je popsáno v [průvodci správným stylem pro kód Python](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span><span class="sxs-lookup"><span data-stu-id="d9755-141">Remember that Python package names should be all lowercase, as described in the [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span></span>
   
    <span data-ttu-id="d9755-142">Je-li pro vás Python Flask nový, je to rozhraní pro vývoj webových aplikací, které urychluje tvorbu webových aplikací v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="d9755-142">For those new to Python Flask, it is a web application development framework that helps you build web applications in Python faster.</span></span>
   
    ![Snímek obrazovky okna Nový projekt v nástroji Visual Studio se zvýrazněným Pythonem vlevo, vybraným webovým projektem Python Flask uprostřed a názvem tutorial v poli Název](./media/documentdb-python-application/image9.png)
4. <span data-ttu-id="d9755-144">V okně **Python Tools for Visual Studio** klikněte na **Nainstalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="d9755-144">In the **Python Tools for Visual Studio** window, click **Install into a virtual environment**.</span></span> 
   
    ![Snímek obrazovky databázového kurzu – okno Python Tools for Visual Studio](./media/documentdb-python-application/python-install-virtual-environment.png)
5. <span data-ttu-id="d9755-146">V okně **Přidání virtuálního prostředí** můžete přijmout výchozí hodnoty a použít Python 2.7 jako základní prostředí, protože PyDocumentDB v tuto chvíli nepodporuje Python 3.x. Pak klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d9755-146">In the **Add Virtual Environment** window, you can accept the defaults and use Python 2.7 as the base environment because PyDocumentDB does not currently support Python 3.x, and then click **Create**.</span></span> <span data-ttu-id="d9755-147">Tím se pro projekt nastaví požadované virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="d9755-147">This sets up the required Python virtual environment for your project.</span></span>
   
    ![Snímek obrazovky databázového kurzu – okno Python Tools for Visual Studio](./media/documentdb-python-application/image10_A.png)
   
    <span data-ttu-id="d9755-149">Jakmile je prostředí úspěšně nainstalováno, ve výstupním okně se zobrazí `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`.</span><span class="sxs-lookup"><span data-stu-id="d9755-149">The output window displays `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` when the environment is successfully installed.</span></span>

## <a name="step-3-modify-the-python-flask-web-application"></a><span data-ttu-id="d9755-150">Krok 3: Úprava webové aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="d9755-150">Step 3: Modify the Python Flask web application</span></span>
### <a name="add-the-python-flask-packages-to-your-project"></a><span data-ttu-id="d9755-151">Přidání balíčků Python Flask do projektu</span><span class="sxs-lookup"><span data-stu-id="d9755-151">Add the Python Flask packages to your project</span></span>
<span data-ttu-id="d9755-152">Po nastavení projektu bude nutné do projektu přidat požadované balíčky Flask, včetně pydocumentdb, tedy balíčku Python pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="d9755-152">After your project is set up, you'll need to add the required Flask packages to your project, including pydocumentdb, the Python package for DocumentDB.</span></span>

1. <span data-ttu-id="d9755-153">V Průzkumníkovi řešení otevřete soubor s názvem **requirements.txt** a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d9755-153">In Solution Explorer, open the file named **requirements.txt** and replace the contents with the following:</span></span>
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. <span data-ttu-id="d9755-154">Uložte soubor **requirements.txt**.</span><span class="sxs-lookup"><span data-stu-id="d9755-154">Save the **requirements.txt** file.</span></span> 
3. <span data-ttu-id="d9755-155">V Průzkumníkovi řešení klikněte pravým tlačítkem na **env** a pak levým na **Nainstalovat z requirements.txt**.</span><span class="sxs-lookup"><span data-stu-id="d9755-155">In Solution Explorer, right-click **env** and click **Install from requirements.txt**.</span></span>
   
    ![Snímek obrazovky, na kterém je zvoleno env (Python 2.7) a v seznamu je zvýrazněna možnost Instalovat z requirements.txt](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    <span data-ttu-id="d9755-157">Po úspěšné instalaci se ve výstupním okně zobrazí následující:</span><span class="sxs-lookup"><span data-stu-id="d9755-157">After successful installation, the output window displays the following:</span></span>
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > <span data-ttu-id="d9755-158">Ve výjimečných případech se ve výstupním okně může zobrazit informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="d9755-158">In rare cases, you might see a failure in the output window.</span></span> <span data-ttu-id="d9755-159">Pokud se to stane, podívejte se, jestli chyba nesouvisí s vyčištěním.</span><span class="sxs-lookup"><span data-stu-id="d9755-159">If this happens, check if the error is related to cleanup.</span></span> <span data-ttu-id="d9755-160">Někdy se vyčištění nepovede, ale instalace je přesto úspěšná (posuňte se ve výstupním okně nahoru, kde to je možné ověřit).</span><span class="sxs-lookup"><span data-stu-id="d9755-160">Sometimes the cleanup fails, but the installation will still be successful (scroll up in the output window to verify this).</span></span> <span data-ttu-id="d9755-161">Instalaci můžete zkontrolovat [ověřením virtuálního prostředí](#verify-the-virtual-environment).</span><span class="sxs-lookup"><span data-stu-id="d9755-161">You can check your installation by [Verifying the virtual environment](#verify-the-virtual-environment).</span></span> <span data-ttu-id="d9755-162">Pokud se instalace nepovedla, ale ověření proběhlo úspěšně, můžete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d9755-162">If the installation failed but the verification is successful, it's OK to continue.</span></span>
   > 
   > 

### <a name="verify-the-virtual-environment"></a><span data-ttu-id="d9755-163">Ověření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="d9755-163">Verify the virtual environment</span></span>
<span data-ttu-id="d9755-164">Ujistěme se, že se vše správně nainstalovalo.</span><span class="sxs-lookup"><span data-stu-id="d9755-164">Let's make sure that everything is installed correctly.</span></span>

1. <span data-ttu-id="d9755-165">Sestavte řešení stisknutím **CTRL**+**SHIFT**+**B**.</span><span class="sxs-lookup"><span data-stu-id="d9755-165">Build the solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="d9755-166">Po úspěšném sestavení spusťte web klávesou **F5**.</span><span class="sxs-lookup"><span data-stu-id="d9755-166">Once the build succeeds, start the website by pressing **F5**.</span></span> <span data-ttu-id="d9755-167">Tímto se spustí vývojový server Flask a webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="d9755-167">This launches the Flask development server and starts your web browser.</span></span> <span data-ttu-id="d9755-168">Měla by se zobrazit následující stránka:</span><span class="sxs-lookup"><span data-stu-id="d9755-168">You should see the following page.</span></span>
   
    ![Prázdný webový vývojový projekt Python Flask zobrazený v prohlížeči](./media/documentdb-python-application/image12.png)
3. <span data-ttu-id="d9755-170">Klávesami **SHIFT**+**F5** v nástroji Visual Studio ukončete ladění webu.</span><span class="sxs-lookup"><span data-stu-id="d9755-170">Stop debugging the website by pressing **Shift**+**F5** in Visual Studio.</span></span>

### <a name="create-database-collection-and-document-definitions"></a><span data-ttu-id="d9755-171">Vytvoření definic databáze, kolekcí a dokumentů</span><span class="sxs-lookup"><span data-stu-id="d9755-171">Create database, collection, and document definitions</span></span>
<span data-ttu-id="d9755-172">Nyní vytvořte hlasovací aplikaci tak, že přidáme nové soubory a jiné aktualizujeme.</span><span class="sxs-lookup"><span data-stu-id="d9755-172">Now let's create your voting application by adding new files and updating others.</span></span>

1. <span data-ttu-id="d9755-173">V Průzkumníkovi řešení klikněte pravým tlačítkem na projekt **tutorial**, pak levým na **Přidat** a nakonec také levým na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="d9755-173">In Solution Explorer, right-click the **tutorial** project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="d9755-174">Vyberte **Prázdný soubor Python** a pojmenujte jej **forms.py**.</span><span class="sxs-lookup"><span data-stu-id="d9755-174">Select **Empty Python File** and name the file **forms.py**.</span></span>  
2. <span data-ttu-id="d9755-175">Do souboru forms.py přidejte následující kód a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="d9755-175">Add the following code to the forms.py file, and then save the file.</span></span>

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-the-required-imports-to-viewspy"></a><span data-ttu-id="d9755-176">Přidání požadovaných importů do views.py</span><span class="sxs-lookup"><span data-stu-id="d9755-176">Add the required imports to views.py</span></span>
1. <span data-ttu-id="d9755-177">V Průzkumníkovi řešení rozbalte složku **tutorial** a otevřete soubor **views.py**.</span><span class="sxs-lookup"><span data-stu-id="d9755-177">In Solution Explorer, expand the **tutorial** folder, and open the **views.py** file.</span></span> 
2. <span data-ttu-id="d9755-178">Do horní části souboru **views.py** přidejte následující příkazy import a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="d9755-178">Add the following import statements to the top of the **views.py** file, then save the file.</span></span> <span data-ttu-id="d9755-179">Tyto příkazu importují balíčky Flask a PythonSDK pro službu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d9755-179">These import Cosmos DB's PythonSDK and the Flask packages.</span></span>
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a><span data-ttu-id="d9755-180">Vytvoření databáze, kolekce a dokumentu</span><span class="sxs-lookup"><span data-stu-id="d9755-180">Create database, collection, and document</span></span>
* <span data-ttu-id="d9755-181">Na konec téhož souboru **views.py** přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d9755-181">Still in **views.py**, add the following code to the end of the file.</span></span> <span data-ttu-id="d9755-182">Ten vytvoří databázi, která se použije ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="d9755-182">This takes care of creating the database used by the form.</span></span> <span data-ttu-id="d9755-183">V souboru **views.py** neodstraňujte žádný kód, který tam již je.</span><span class="sxs-lookup"><span data-stu-id="d9755-183">Do not delete any of the existing code in **views.py**.</span></span> <span data-ttu-id="d9755-184">Pouze nový kód přidejte na konec.</span><span class="sxs-lookup"><span data-stu-id="d9755-184">Simply append this to the end.</span></span>

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a><span data-ttu-id="d9755-185">Čtení databáze, kolekce a dokumentu a odeslání formuláře</span><span class="sxs-lookup"><span data-stu-id="d9755-185">Read database, collection, document, and submit form</span></span>
* <span data-ttu-id="d9755-186">Na konec téhož souboru **views.py** přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d9755-186">Still in **views.py**, add the following code to the end of the file.</span></span> <span data-ttu-id="d9755-187">Tento kód má na starosti nastavení formuláře a čtení databáze, kolekce a dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d9755-187">This takes care of setting up the form, reading the database, collection, and document.</span></span> <span data-ttu-id="d9755-188">V souboru **views.py** neodstraňujte žádný kód, který tam již je.</span><span class="sxs-lookup"><span data-stu-id="d9755-188">Do not delete any of the existing code in **views.py**.</span></span> <span data-ttu-id="d9755-189">Pouze nový kód přidejte na konec.</span><span class="sxs-lookup"><span data-stu-id="d9755-189">Simply append this to the end.</span></span>

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-the-html-files"></a><span data-ttu-id="d9755-190">Vytvoření souborů HTML</span><span class="sxs-lookup"><span data-stu-id="d9755-190">Create the HTML files</span></span>
1. <span data-ttu-id="d9755-191">V Průzkumníkovi řešení klikněte pravým tlačítkem ve složce **tutorial** na složku **šablony**, pak levým na **Přidat** a nakonec také levým na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="d9755-191">In Solution Explorer, in the **tutorial** folder, right click the **templates** folder, click **Add**, and then click **New Item**.</span></span> 
2. <span data-ttu-id="d9755-192">Vyberte **Stránka HTML** do pole název zadejte **create.html**.</span><span class="sxs-lookup"><span data-stu-id="d9755-192">Select **HTML Page**, and then in the name box type **create.html**.</span></span> 
3. <span data-ttu-id="d9755-193">Opakováním kroků 1 a 2 vytvořte dva další soubory HTML: results.html a vote.html.</span><span class="sxs-lookup"><span data-stu-id="d9755-193">Repeat steps 1 and 2 to create two additional HTML files: results.html and vote.html.</span></span>
4. <span data-ttu-id="d9755-194">Do souboru **create.html** do elementu `<body>` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d9755-194">Add the following code to **create.html** in the `<body>` element.</span></span> <span data-ttu-id="d9755-195">Tento kód zobrazí zprávu, že jsme vytvořili novou databázi, kolekci a dokument.</span><span class="sxs-lookup"><span data-stu-id="d9755-195">It displays a message stating that we created a new database, collection, and document.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. <span data-ttu-id="d9755-196">Do souboru **results.html** do elementu `<body` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d9755-196">Add the following code to **results.html** in the `<body`> element.</span></span> <span data-ttu-id="d9755-197">Slouží k zobrazení výsledků ankety.</span><span class="sxs-lookup"><span data-stu-id="d9755-197">It displays the results of the poll.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of the vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. <span data-ttu-id="d9755-198">Do souboru **vote.html** do elementu `<body` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d9755-198">Add the following code to **vote.html** in the `<body`> element.</span></span> <span data-ttu-id="d9755-199">Zobrazuje anketu a přijímá hlasy.</span><span class="sxs-lookup"><span data-stu-id="d9755-199">It displays the poll and accepts the votes.</span></span> <span data-ttu-id="d9755-200">Při registraci hlasů se ovládací prvek předává do views.py, kde rozpoznáme udělené hlasy a patřičně připojíme dokument.</span><span class="sxs-lookup"><span data-stu-id="d9755-200">On registering the votes, the control is passed over to views.py where we will recognize the vote cast and append the document accordingly.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way to host an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. <span data-ttu-id="d9755-201">Ve složce **šablony** nahraďte obsah souboru **index.html** následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="d9755-201">In the **templates** folder, replace the contents of **index.html** with the following.</span></span> <span data-ttu-id="d9755-202">Tato stránka slouží jako cílová stránka aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9755-202">This serves as the landing page for your application.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-initpy"></a><span data-ttu-id="d9755-203">Přidejte konfigurační soubor a změňte \_\_init\_\_.py.</span><span class="sxs-lookup"><span data-stu-id="d9755-203">Add a configuration file and change the \_\_init\_\_.py</span></span>
1. <span data-ttu-id="d9755-204">V Průzkumníkovi řešení klikněte pravým tlačítkem na projekt **tutorial**, pak levým na **Přidat**, dále na **Nová položka**, vyberte **Prázdný soubor Python** a pojmenujte jej **config.py**.</span><span class="sxs-lookup"><span data-stu-id="d9755-204">In Solution Explorer, right-click the **tutorial** project, click **Add**, click **New Item**, select **Empty Python File**, and then name the file **config.py**.</span></span> <span data-ttu-id="d9755-205">Formuláře ve Flasku vyžadují tento konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="d9755-205">This config file is required by forms in Flask.</span></span> <span data-ttu-id="d9755-206">Můžete jej použít i k poskytnutí tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="d9755-206">You can use it to provide a secret key as well.</span></span> <span data-ttu-id="d9755-207">Ten ale pro tento kurz není nezbytný.</span><span class="sxs-lookup"><span data-stu-id="d9755-207">This key is not needed for this tutorial though.</span></span>
2. <span data-ttu-id="d9755-208">Do souboru config.py přidejte následující kód. V dalším kroku bude nutné upravit hodnoty **DOCUMENTDB\_HOST** a **DOCUMENTDB\_KEY**.</span><span class="sxs-lookup"><span data-stu-id="d9755-208">Add the following code to config.py, you'll need to alter the values of **DOCUMENTDB\_HOST** and **DOCUMENTDB\_KEY** in the next step.</span></span>
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. <span data-ttu-id="d9755-209">Na webu [Azure Portal](https://portal.azure.com/) přejděte do okna **Klíče** tak, že kliknete na **Procházet**, **Účty služby Azure Cosmos DB**, dvakrát kliknete na název účtu, který chcete použít, a v oblasti **Základy** kliknete na tlačítko **Klíče**.</span><span class="sxs-lookup"><span data-stu-id="d9755-209">In the [Azure portal](https://portal.azure.com/), navigate to the **Keys** blade by clicking **Browse**, **Azure Cosmos DB Accounts**, double-click the name of the account to use, and then click the **Keys** button in the **Essentials** area.</span></span> <span data-ttu-id="d9755-210">V okně **Klíče** zkopírujte hodnotu **URI** a vložte ji do souboru **config.py** jako hodnotu vlastnosti **DOCUMENTDB\_HOST**.</span><span class="sxs-lookup"><span data-stu-id="d9755-210">In the **Keys** blade, copy the **URI** value and paste it into the **config.py** file, as the value for the **DOCUMENTDB\_HOST** property.</span></span> 
4. <span data-ttu-id="d9755-211">Zpět na Portálu Azure v okně **Klíče** zkopírujte hodnotu **Primární klíč** nebo **Sekundární klíč** a vložte ji do souboru **config.py** jako hodnotu vlastnosti **DOCUMENTDB\_KEY**.</span><span class="sxs-lookup"><span data-stu-id="d9755-211">Back in the Azure portal, in the **Keys** blade, copy the value of the **Primary Key** or the **Secondary Key**, and paste it into the **config.py** file, as the value for the **DOCUMENTDB\_KEY** property.</span></span>
5. <span data-ttu-id="d9755-212">Do souboru **\_\_init\_\_.py** přidejte následující řádek.</span><span class="sxs-lookup"><span data-stu-id="d9755-212">In the **\_\_init\_\_.py** file, add the following line.</span></span> 
   
        app.config.from_object('config')
   
    <span data-ttu-id="d9755-213">Obsah souboru tak bude následující:</span><span class="sxs-lookup"><span data-stu-id="d9755-213">So that the content of the file is:</span></span>
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. <span data-ttu-id="d9755-214">Po přidání všech souborů by Průzkumník řešení měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d9755-214">After adding all the files, Solution Explorer should look like this:</span></span>
   
    ![Snímek obrazovky okna Průzkumníka řešení v nástroji Visual Studio](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a><span data-ttu-id="d9755-216">Krok 4: Místní spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d9755-216">Step 4: Run your web application locally</span></span>
1. <span data-ttu-id="d9755-217">Sestavte řešení stisknutím **CTRL**+**SHIFT**+**B**.</span><span class="sxs-lookup"><span data-stu-id="d9755-217">Build the solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="d9755-218">Po úspěšném sestavení spusťte web klávesou **F5**.</span><span class="sxs-lookup"><span data-stu-id="d9755-218">Once the build succeeds, start the website by pressing **F5**.</span></span> <span data-ttu-id="d9755-219">Na obrazovce byste měli vidět následující.</span><span class="sxs-lookup"><span data-stu-id="d9755-219">You should see the following on your screen.</span></span>
   
    ![Snímek obrazovky hlasovací aplikace vytvořené pomocí Pythonu a služby Azure Cosmos DB zobrazené ve webovém prohlížeči](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. <span data-ttu-id="d9755-221">Klikněte na **Create/Clear the Voting Database** (Vytvořit nebo vyčistit hlasovací databázi), aby se vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="d9755-221">Click **Create/Clear the Voting Database** to generate the database.</span></span>
   
    ![Snímek obrazovky webové aplikace a její stránky Vytvořit – podrobnosti o vývoji](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. <span data-ttu-id="d9755-223">Pak vyberte svou možnost kliknutím na **Vote** (Hlasovat).</span><span class="sxs-lookup"><span data-stu-id="d9755-223">Then, click **Vote** and select your option.</span></span>
   
    ![Snímek obrazovky webové aplikace se zobrazenou otázkou k hlasování](./media/documentdb-python-application/cosmos-db-vote.png)
5. <span data-ttu-id="d9755-225">Každý hlas, který udělíte, navýší příslušný čítač.</span><span class="sxs-lookup"><span data-stu-id="d9755-225">For every vote you cast, it increments the appropriate counter.</span></span>
   
    ![Snímek obrazovky se zobrazenou stránkou Results of the vote (Výsledek hlasování)](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. <span data-ttu-id="d9755-227">Kombinací kláves SHIFT+F5 ukončete ladění projektu.</span><span class="sxs-lookup"><span data-stu-id="d9755-227">Stop debugging the project by pressing Shift+F5.</span></span>

## <a name="step-5-deploy-the-web-application-to-azure"></a><span data-ttu-id="d9755-228">Krok 5: Nasazení webové aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="d9755-228">Step 5: Deploy the web application to Azure</span></span>
<span data-ttu-id="d9755-229">Teď, když máte je aplikace dokončena a správně pracovat se Cosmos DB, vytvoříme ji nasaďte do Azure.</span><span class="sxs-lookup"><span data-stu-id="d9755-229">Now that you have the complete application working correctly against Cosmos DB, we're going to deploy this to Azure.</span></span>

1. <span data-ttu-id="d9755-230">Klikněte pravým tlačítkem na projekt v Průzkumníkovi řešení (ujistěte se, že aplikace již není spuštěná místně) a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d9755-230">Right-click the project in Solution Explorer (make sure you're not still running it locally) and select **Publish**.</span></span>  
   
     ![Snímek obrazovky projektu tutorial vybraného v Průzkumníkovi řešení se zvýrazněnou možností Publikovat](./media/documentdb-python-application/image20.png)
2. <span data-ttu-id="d9755-232">V **publikovat** dialogové okno, vyberte **Microsoft Azure App Service**, vyberte **vytvořit nový**a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d9755-232">In the **Publish** dialog box, select **Microsoft Azure App Service**, select **Create New**, and then click **Publish**.</span></span>
   
    ![Snímek obrazovky okna publikování webu se zvýrazněnou službou Microsoft Azure App Service](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. <span data-ttu-id="d9755-234">V **vytvořit službu App Service** dialogovém okně zadejte název pro vaši webovou aplikaci spolu s vaší **předplatné**, **skupiny prostředků**, a **plán služby App Service**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d9755-234">In the **Create App Service** dialog box, enter the name for your web app along with your **Subscription**, **Resource Group**, and **App Service Plan**, then click **Create**.</span></span>
   
    ![Snímek obrazovky okna Okno Microsoft Azure Web Apps](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. <span data-ttu-id="d9755-236">Během pár sekund bude Visual Studio dokončí publikování app service a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!</span><span class="sxs-lookup"><span data-stu-id="d9755-236">In a few seconds, Visual Studio will finish publishing your app service and launch a browser where you can see your handiwork running in Azure!</span></span>

    ![Snímek obrazovky okna Okno Microsoft Azure Web Apps](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a><span data-ttu-id="d9755-238">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d9755-238">Troubleshooting</span></span>
<span data-ttu-id="d9755-239">Pokud je toto první aplikace Python, kterou jste spustili na svém počítači, ujistěte se, že vaše proměnná PATH obsahuje následující složky (nebo ekvivalentní umístění instalací):</span><span class="sxs-lookup"><span data-stu-id="d9755-239">If this is the first Python app you've run on your computer, ensure that the following folders (or the equivalent installation locations) are included in your PATH variable:</span></span>

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

<span data-ttu-id="d9755-240">Pokud jste projekt pojmenovali jinak než **tutorial** a na stránce hlasování se zobrazí chyba, ujistěte se, že soubor **\_\_init\_\_.py** odkazuje na následujícím řádku na správný projekt: `import tutorial.view`.</span><span class="sxs-lookup"><span data-stu-id="d9755-240">If you receive an error on your vote page, and you named your project something other than **tutorial**, make sure that **\_\_init\_\_.py** references the correct project name in the line: `import tutorial.view`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9755-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9755-241">Next steps</span></span>
<span data-ttu-id="d9755-242">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="d9755-242">Congratulations!</span></span> <span data-ttu-id="d9755-243">Právě dokončení svou první webovou aplikaci Python, pomocí Cosmos DB a publikovali jste ji na Azure.</span><span class="sxs-lookup"><span data-stu-id="d9755-243">You have just completed your first Python web application using Cosmos DB and published it to Azure.</span></span>

<span data-ttu-id="d9755-244">Toto téma často aktualizujeme a vylepšujeme podle vaší zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="d9755-244">We update and improve this topic frequently based on your feedback.</span></span>  <span data-ttu-id="d9755-245">Až kurz dokončíte, použijte prosím hlasovací tlačítka v horní a dolní části této stránky a sdělte nám svůj názor, jaká vylepšení byste si přáli vidět.</span><span class="sxs-lookup"><span data-stu-id="d9755-245">Once you've completed the tutorial, please using the voting buttons at the top and bottom of this page, and be sure to include your feedback on what improvements you want to see made.</span></span> <span data-ttu-id="d9755-246">Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích.</span><span class="sxs-lookup"><span data-stu-id="d9755-246">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="d9755-247">Přidat další funkce k vaší webové aplikaci, podívejte se na rozhraní API dostupná v [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="d9755-247">To add additional functionality to your web application, review the APIs available in the [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span></span>

<span data-ttu-id="d9755-248">Další informace o Azure, nástroji Visual Studio a Pythonu najdete v [Centru pro vývojáře v Pythonu](https://azure.microsoft.com/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="d9755-248">For more information about Azure, Visual Studio, and Python, see the [Python Developer Center](https://azure.microsoft.com/develop/python/).</span></span> 

<span data-ttu-id="d9755-249">Další kurzy Pythonu Flask najdete na stránce [Velký kurz na Flask, část I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span><span class="sxs-lookup"><span data-stu-id="d9755-249">For additional Python Flask tutorials, see [The Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span></span> 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
