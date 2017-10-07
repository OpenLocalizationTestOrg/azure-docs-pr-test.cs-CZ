---
title: "aaaPython Flask webové aplikace kurz pro Azure Cosmos DB | Microsoft Docs"
description: "Projděte si databázový kurz na používání Azure Cosmos DB toostore a přístup dat z webové aplikace Python Flask hostované v Azure. Naleznete zde řešení pro vývoj aplikací."
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
ms.openlocfilehash: 87b73c656ed96a7efbd162843a1529d435f027f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a><span data-ttu-id="dffbc-105">Sestavení webové aplikace Python Flask využívající službu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="dffbc-105">Build a Python Flask web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dffbc-106">.NET</span><span class="sxs-lookup"><span data-stu-id="dffbc-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="dffbc-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="dffbc-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="dffbc-108">Java</span><span class="sxs-lookup"><span data-stu-id="dffbc-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="dffbc-109">Python</span><span class="sxs-lookup"><span data-stu-id="dffbc-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="dffbc-110">Tento kurz ukazuje, jak data toostore a přístup k databázi Azure Cosmos toouse z Python webové aplikace hostované v Azure a předpokládá, že máte zkušenosti s používáním Pythonu a webů Azure.</span><span class="sxs-lookup"><span data-stu-id="dffbc-110">This tutorial shows you how toouse Azure Cosmos DB toostore and access data from a Python web application hosted on Azure and presumes that you have some prior experience using Python and Azure websites.</span></span>

<span data-ttu-id="dffbc-111">Tento databázový kurz se zabývá:</span><span class="sxs-lookup"><span data-stu-id="dffbc-111">This database tutorial covers:</span></span>

1. <span data-ttu-id="dffbc-112">Vytváření a zřizování účtu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dffbc-112">Creating and provisioning a Cosmos DB account.</span></span>
2. <span data-ttu-id="dffbc-113">Vytvoření aplikace Python Flask.</span><span class="sxs-lookup"><span data-stu-id="dffbc-113">Creating a Python Flask application.</span></span>
3. <span data-ttu-id="dffbc-114">Připojení tooand pomocí Cosmos DB z webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dffbc-114">Connecting tooand using Cosmos DB from your web application.</span></span>
4. <span data-ttu-id="dffbc-115">Nasazení hello webové aplikace tooAzure.</span><span class="sxs-lookup"><span data-stu-id="dffbc-115">Deploying hello web application tooAzure.</span></span>

<span data-ttu-id="dffbc-116">Podle tohoto kurzu sestavíte jednoduchou hlasovací aplikaci, která vám umožní toovote v anketě.</span><span class="sxs-lookup"><span data-stu-id="dffbc-116">By following this tutorial, you will build a simple voting application that allows you toovote for a poll.</span></span>

![Snímek obrazovky hlasovací aplikace hello vytvořené v tomto kurzu databáze](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a><span data-ttu-id="dffbc-118">Předpoklady pro databázový kurz</span><span class="sxs-lookup"><span data-stu-id="dffbc-118">Database tutorial prerequisites</span></span>
<span data-ttu-id="dffbc-119">Před následující hello pokyny v tomto článku, se ujistěte, že máte nainstalované tyto položky hello:</span><span class="sxs-lookup"><span data-stu-id="dffbc-119">Before following hello instructions in this article, you should ensure that you have hello following installed:</span></span>

* <span data-ttu-id="dffbc-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="dffbc-120">An active Azure account.</span></span> <span data-ttu-id="dffbc-121">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="dffbc-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="dffbc-122">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dffbc-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
 
    <span data-ttu-id="dffbc-123">NEBO</span><span class="sxs-lookup"><span data-stu-id="dffbc-123">OR</span></span> 

    <span data-ttu-id="dffbc-124">Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="dffbc-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="dffbc-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="dffbc-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="dffbc-126">[Python Tools pro Visual Studio](https://github.com/Microsoft/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="dffbc-126">[Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span></span>  
* <span data-ttu-id="dffbc-127">[Microsoft Azure SDK pro Python 2.7](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="dffbc-127">[Microsoft Azure SDK for Python 2.7](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="dffbc-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="dffbc-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="dffbc-129">Pokud instalujete Python 2.7 pro hello poprvé, ujistěte se, že v úvodní obrazovka přizpůsobit Python 2.7.13, vyberete **přidat python.exe tooPath**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-129">If you are installing Python 2.7 for hello first time, ensure that in hello Customize Python 2.7.13 screen, you select **Add python.exe tooPath**.</span></span>
> 
> ![Snímek obrazovky úvodní obrazovka přizpůsobit Python 2.7.11, kde je nutné tooselect přidat python.exe tooPath](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* <span data-ttu-id="dffbc-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span><span class="sxs-lookup"><span data-stu-id="dffbc-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="dffbc-132">Krok 1: Vytvoření účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="dffbc-132">Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="dffbc-133">Začněme vytvořením účtu služby Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dffbc-133">Let's start by creating an Cosmos DB account.</span></span> <span data-ttu-id="dffbc-134">Pokud již máte účet, nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[krok 2: vytvoření nové webové aplikace Python Flask](#step-2-create-a-new-python-flask-web-application).</span><span class="sxs-lookup"><span data-stu-id="dffbc-134">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create a new Python Flask web application](#step-2-create-a-new-python-flask-web-application).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
<span data-ttu-id="dffbc-135">Nyní vám ukážeme jak toocreate nové webové aplikace Python Flask z hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="dffbc-135">We will now walk through how toocreate a new Python Flask web application from hello ground up.</span></span>

## <a name="step-2-create-a-new-python-flask-web-application"></a><span data-ttu-id="dffbc-136">Krok 2: Vytvoření nové webové aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="dffbc-136">Step 2: Create a new Python Flask web application</span></span>
1. <span data-ttu-id="dffbc-137">V sadě Visual Studio na hello **soubor** nabídce bodu příliš**nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-137">In Visual Studio, on hello **File** menu, point too**New**, and then click **Project**.</span></span>
   
    <span data-ttu-id="dffbc-138">Hello **nový projekt** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dffbc-138">hello **New Project** dialog box appears.</span></span>
2. <span data-ttu-id="dffbc-139">V levém podokně hello rozbalte **šablony** a potom **Python**a potom klikněte na **webové**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-139">In hello left pane, expand **Templates** and then **Python**, and then click **Web**.</span></span> 
3. <span data-ttu-id="dffbc-140">Vyberte **webový projekt Flask** v prostředním podokně hello a potom v hello **název** zadejte **kurzu**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-140">Select **Flask  Web Project** in hello center pane, then in hello **Name** box type **tutorial**, and then click **OK**.</span></span> <span data-ttu-id="dffbc-141">Mějte na paměti, že názvy balíčků Python by měl být celé malými písmeny, jak je popsáno v hello [průvodci správným stylem pro kód Python](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span><span class="sxs-lookup"><span data-stu-id="dffbc-141">Remember that Python package names should be all lowercase, as described in hello [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span></span>
   
    <span data-ttu-id="dffbc-142">Tyto nové tooPython Flask je architektura vývoj webových aplikací, které urychluje tvorbu webových aplikací v Pythonu rychlejší.</span><span class="sxs-lookup"><span data-stu-id="dffbc-142">For those new tooPython Flask, it is a web application development framework that helps you build web applications in Python faster.</span></span>
   
    ![Snímek obrazovky znázorňující hello okno Nový projekt v sadě Visual Studio se zvýrazněným hello vlevo, vybraným hello uprostřed a názvem tutorial hello v poli Název hello Python Flask webovým projektem Python](./media/documentdb-python-application/image9.png)
4. <span data-ttu-id="dffbc-144">V hello **Python Tools pro Visual Studio** okně klikněte na tlačítko **nainstalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-144">In hello **Python Tools for Visual Studio** window, click **Install into a virtual environment**.</span></span> 
   
    ![Snímek obrazovky databázového kurzu hello – nástroje Python Tools pro Visual Studio – okno](./media/documentdb-python-application/python-install-virtual-environment.png)
5. <span data-ttu-id="dffbc-146">V hello **Přidání virtuálního prostředí** okně můžete přijmout výchozí hodnoty hello a použít Python 2.7 jako základní prostředí hello, protože pydocumentdb v tuto aktuálně nepodporuje Python 3.x a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-146">In hello **Add Virtual Environment** window, you can accept hello defaults and use Python 2.7 as hello base environment because PyDocumentDB does not currently support Python 3.x, and then click **Create**.</span></span> <span data-ttu-id="dffbc-147">Toto nastaví hello požadované virtuální prostředí Python pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="dffbc-147">This sets up hello required Python virtual environment for your project.</span></span>
   
    ![Snímek obrazovky databázového kurzu hello – nástroje Python Tools pro Visual Studio – okno](./media/documentdb-python-application/image10_A.png)
   
    <span data-ttu-id="dffbc-149">Zobrazí okno výstup Hello `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` při hello prostředí úspěšně nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="dffbc-149">hello output window displays `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` when hello environment is successfully installed.</span></span>

## <a name="step-3-modify-hello-python-flask-web-application"></a><span data-ttu-id="dffbc-150">Krok 3: Úprava webové aplikace Python Flask hello</span><span class="sxs-lookup"><span data-stu-id="dffbc-150">Step 3: Modify hello Python Flask web application</span></span>
### <a name="add-hello-python-flask-packages-tooyour-project"></a><span data-ttu-id="dffbc-151">Přidání projektu tooyour balíčků Python Flask hello</span><span class="sxs-lookup"><span data-stu-id="dffbc-151">Add hello Python Flask packages tooyour project</span></span>
<span data-ttu-id="dffbc-152">Po nastavení projektu, budete potřebovat tooadd hello požadované balíčky tooyour projekt Flask, včetně pydocumentdb, tedy hello balíčku Python pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="dffbc-152">After your project is set up, you'll need tooadd hello required Flask packages tooyour project, including pydocumentdb, hello Python package for DocumentDB.</span></span>

1. <span data-ttu-id="dffbc-153">V Průzkumníku řešení otevřete soubor hello s názvem **requirements.txt** a nahraďte obsah hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="dffbc-153">In Solution Explorer, open hello file named **requirements.txt** and replace hello contents with hello following:</span></span>
   
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
2. <span data-ttu-id="dffbc-154">Uložit hello **requirements.txt** souboru.</span><span class="sxs-lookup"><span data-stu-id="dffbc-154">Save hello **requirements.txt** file.</span></span> 
3. <span data-ttu-id="dffbc-155">V Průzkumníkovi řešení klikněte pravým tlačítkem na **env** a pak levým na **Nainstalovat z requirements.txt**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-155">In Solution Explorer, right-click **env** and click **Install from requirements.txt**.</span></span>
   
    ![Snímek obrazovky znázorňující env (Python 2.7) vybraná při instalaci z requirements.txt zvýrazněných v seznamu hello](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    <span data-ttu-id="dffbc-157">Po úspěšné instalaci se zobrazí okno výstup hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="dffbc-157">After successful installation, hello output window displays hello following:</span></span>
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > <span data-ttu-id="dffbc-158">Ve výjimečných případech může se zobrazit chyba v okně výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-158">In rare cases, you might see a failure in hello output window.</span></span> <span data-ttu-id="dffbc-159">Pokud k tomu dojde, zkontrolujte, jestli chyba hello je související toocleanup.</span><span class="sxs-lookup"><span data-stu-id="dffbc-159">If this happens, check if hello error is related toocleanup.</span></span> <span data-ttu-id="dffbc-160">Někdy hello čištění nezdaří, ale bude stále k úspěšné instalace hello (Posunout nahoru v tooverify okno výstup hello to).</span><span class="sxs-lookup"><span data-stu-id="dffbc-160">Sometimes hello cleanup fails, but hello installation will still be successful (scroll up in hello output window tooverify this).</span></span> <span data-ttu-id="dffbc-161">Instalaci můžete zkontrolovat [ověření hello virtuální prostředí](#verify-the-virtual-environment).</span><span class="sxs-lookup"><span data-stu-id="dffbc-161">You can check your installation by [Verifying hello virtual environment](#verify-the-virtual-environment).</span></span> <span data-ttu-id="dffbc-162">Pokud hello instalace se nezdařila, ale je hello ověření úspěšné, je OK toocontinue.</span><span class="sxs-lookup"><span data-stu-id="dffbc-162">If hello installation failed but hello verification is successful, it's OK toocontinue.</span></span>
   > 
   > 

### <a name="verify-hello-virtual-environment"></a><span data-ttu-id="dffbc-163">Ověřte hello virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="dffbc-163">Verify hello virtual environment</span></span>
<span data-ttu-id="dffbc-164">Ujistěme se, že se vše správně nainstalovalo.</span><span class="sxs-lookup"><span data-stu-id="dffbc-164">Let's make sure that everything is installed correctly.</span></span>

1. <span data-ttu-id="dffbc-165">Vytvoření řešení hello stisknutím **Ctrl**+**Shift**+**B**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-165">Build hello solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="dffbc-166">Po úspěšném sestavení hello spustit hello webu stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-166">Once hello build succeeds, start hello website by pressing **F5**.</span></span> <span data-ttu-id="dffbc-167">Tím se spustí vývojový server Flask hello a spustí webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="dffbc-167">This launches hello Flask development server and starts your web browser.</span></span> <span data-ttu-id="dffbc-168">Měli byste vidět následující stránku hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-168">You should see hello following page.</span></span>
   
    ![Hello prázdný Python Flask webový vývojový projekt zobrazí v prohlížeči](./media/documentdb-python-application/image12.png)
3. <span data-ttu-id="dffbc-170">Ukončete ladění webu hello stisknutím **Shift**+**F5** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dffbc-170">Stop debugging hello website by pressing **Shift**+**F5** in Visual Studio.</span></span>

### <a name="create-database-collection-and-document-definitions"></a><span data-ttu-id="dffbc-171">Vytvoření definic databáze, kolekcí a dokumentů</span><span class="sxs-lookup"><span data-stu-id="dffbc-171">Create database, collection, and document definitions</span></span>
<span data-ttu-id="dffbc-172">Nyní vytvořte hlasovací aplikaci tak, že přidáme nové soubory a jiné aktualizujeme.</span><span class="sxs-lookup"><span data-stu-id="dffbc-172">Now let's create your voting application by adding new files and updating others.</span></span>

1. <span data-ttu-id="dffbc-173">V Průzkumníku řešení klikněte pravým tlačítkem na hello **kurzu** projektu, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-173">In Solution Explorer, right-click hello **tutorial** project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="dffbc-174">Vyberte **prázdný soubor Python** a název souboru hello **forms.py**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-174">Select **Empty Python File** and name hello file **forms.py**.</span></span>  
2. <span data-ttu-id="dffbc-175">Přidejte následující kód toohello forms.py soubor hello a pak hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="dffbc-175">Add hello following code toohello forms.py file, and then save hello file.</span></span>

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-hello-required-imports-tooviewspy"></a><span data-ttu-id="dffbc-176">Přidat tooviews.py hello požadované importy</span><span class="sxs-lookup"><span data-stu-id="dffbc-176">Add hello required imports tooviews.py</span></span>
1. <span data-ttu-id="dffbc-177">V Průzkumníku řešení rozbalte hello **kurzu** složku a otevřete hello **views.py** souboru.</span><span class="sxs-lookup"><span data-stu-id="dffbc-177">In Solution Explorer, expand hello **tutorial** folder, and open hello **views.py** file.</span></span> 
2. <span data-ttu-id="dffbc-178">Přidejte následující import příkazy toohello horní části hello hello **views.py** uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-178">Add hello following import statements toohello top of hello **views.py** file, then save hello file.</span></span> <span data-ttu-id="dffbc-179">Tyto importovat Cosmos DB PythonSDK a hello balíčky Flask.</span><span class="sxs-lookup"><span data-stu-id="dffbc-179">These import Cosmos DB's PythonSDK and hello Flask packages.</span></span>
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a><span data-ttu-id="dffbc-180">Vytvoření databáze, kolekce a dokumentu</span><span class="sxs-lookup"><span data-stu-id="dffbc-180">Create database, collection, and document</span></span>
* <span data-ttu-id="dffbc-181">Pořád ještě v **views.py**, přidejte následující kód toohello konec souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-181">Still in **views.py**, add hello following code toohello end of hello file.</span></span> <span data-ttu-id="dffbc-182">To má na starosti vytvoření databáze hello používá hello formuláře.</span><span class="sxs-lookup"><span data-stu-id="dffbc-182">This takes care of creating hello database used by hello form.</span></span> <span data-ttu-id="dffbc-183">Neodstraňujte žádný existující kód hello v **views.py**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-183">Do not delete any of hello existing code in **views.py**.</span></span> <span data-ttu-id="dffbc-184">Tomuto elementu end toohello jednoduše připojte.</span><span class="sxs-lookup"><span data-stu-id="dffbc-184">Simply append this toohello end.</span></span>

```python
@app.route('/create')
def create():
    """Renders hello contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt toodelete hello database.  This allows this toobe used toorecreate as well as create
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


### <a name="read-database-collection-document-and-submit-form"></a><span data-ttu-id="dffbc-185">Čtení databáze, kolekce a dokumentu a odeslání formuláře</span><span class="sxs-lookup"><span data-stu-id="dffbc-185">Read database, collection, document, and submit form</span></span>
* <span data-ttu-id="dffbc-186">Pořád ještě v **views.py**, přidejte následující kód toohello konec souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-186">Still in **views.py**, add hello following code toohello end of hello file.</span></span> <span data-ttu-id="dffbc-187">To má na starosti nastavení formuláře hello a čtení hello databáze, kolekce a dokumentu.</span><span class="sxs-lookup"><span data-stu-id="dffbc-187">This takes care of setting up hello form, reading hello database, collection, and document.</span></span> <span data-ttu-id="dffbc-188">Neodstraňujte žádný existující kód hello v **views.py**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-188">Do not delete any of hello existing code in **views.py**.</span></span> <span data-ttu-id="dffbc-189">Tomuto elementu end toohello jednoduše připojte.</span><span class="sxs-lookup"><span data-stu-id="dffbc-189">Simply append this toohello end.</span></span>

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

        # Take hello data from hello deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model toopass tooresults.html
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


### <a name="create-hello-html-files"></a><span data-ttu-id="dffbc-190">Vytvoření souborů HTML hello</span><span class="sxs-lookup"><span data-stu-id="dffbc-190">Create hello HTML files</span></span>
1. <span data-ttu-id="dffbc-191">V Průzkumníku řešení v hello **kurzu** složky správné, klikněte na tlačítko hello **šablony** složku, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-191">In Solution Explorer, in hello **tutorial** folder, right click hello **templates** folder, click **Add**, and then click **New Item**.</span></span> 
2. <span data-ttu-id="dffbc-192">Vyberte **stránku HTML**a pak do pole zadejte název hello **create.html**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-192">Select **HTML Page**, and then in hello name box type **create.html**.</span></span> 
3. <span data-ttu-id="dffbc-193">Opakujte kroky 1 a 2 toocreate dva další soubory HTML: results.html a vote.html.</span><span class="sxs-lookup"><span data-stu-id="dffbc-193">Repeat steps 1 and 2 toocreate two additional HTML files: results.html and vote.html.</span></span>
4. <span data-ttu-id="dffbc-194">Přidejte následující kód příliš hello**create.html** v hello `<body>` elementu.</span><span class="sxs-lookup"><span data-stu-id="dffbc-194">Add hello following code too**create.html** in hello `<body>` element.</span></span> <span data-ttu-id="dffbc-195">Tento kód zobrazí zprávu, že jsme vytvořili novou databázi, kolekci a dokument.</span><span class="sxs-lookup"><span data-stu-id="dffbc-195">It displays a message stating that we created a new database, collection, and document.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. <span data-ttu-id="dffbc-196">Přidejte následující kód příliš hello**results.html** v hello `<body`> elementu.</span><span class="sxs-lookup"><span data-stu-id="dffbc-196">Add hello following code too**results.html** in hello `<body`> element.</span></span> <span data-ttu-id="dffbc-197">Zobrazuje výsledky hello hello dotazování.</span><span class="sxs-lookup"><span data-stu-id="dffbc-197">It displays hello results of hello poll.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of hello vote</h2>
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
6. <span data-ttu-id="dffbc-198">Přidejte následující kód příliš hello**vote.html** v hello `<body`> elementu.</span><span class="sxs-lookup"><span data-stu-id="dffbc-198">Add hello following code too**vote.html** in hello `<body`> element.</span></span> <span data-ttu-id="dffbc-199">Zobrazí hello dotazování a přijímá hlasy hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-199">It displays hello poll and accepts hello votes.</span></span> <span data-ttu-id="dffbc-200">Při registraci hlasů hello, hello ovládací prvek předává přes tooviews.py kde jsme rozpozná hello udělené hlasy a připojí dokument hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="dffbc-200">On registering hello votes, hello control is passed over tooviews.py where we will recognize hello vote cast and append hello document accordingly.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way toohost an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. <span data-ttu-id="dffbc-201">V hello **šablony** složky, nahraďte obsah hello **index.html** s následující hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-201">In hello **templates** folder, replace hello contents of **index.html** with hello following.</span></span> <span data-ttu-id="dffbc-202">Tato stránka slouží jako hello úvodní stránka pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dffbc-202">This serves as hello landing page for your application.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear hello Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-hello-initpy"></a><span data-ttu-id="dffbc-203">Přidejte konfigurační soubor a změňte hello \_ \_init\_\_.py</span><span class="sxs-lookup"><span data-stu-id="dffbc-203">Add a configuration file and change hello \_\_init\_\_.py</span></span>
1. <span data-ttu-id="dffbc-204">V Průzkumníku řešení klikněte pravým tlačítkem na hello **kurzu** projektu, klikněte na tlačítko **přidat**, klikněte na tlačítko **nová položka**, vyberte **prázdný soubor Python**a potom Název souboru hello **config.py**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-204">In Solution Explorer, right-click hello **tutorial** project, click **Add**, click **New Item**, select **Empty Python File**, and then name hello file **config.py**.</span></span> <span data-ttu-id="dffbc-205">Formuláře ve Flasku vyžadují tento konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="dffbc-205">This config file is required by forms in Flask.</span></span> <span data-ttu-id="dffbc-206">Můžete ho tooprovide tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="dffbc-206">You can use it tooprovide a secret key as well.</span></span> <span data-ttu-id="dffbc-207">Ten ale pro tento kurz není nezbytný.</span><span class="sxs-lookup"><span data-stu-id="dffbc-207">This key is not needed for this tutorial though.</span></span>
2. <span data-ttu-id="dffbc-208">Přidejte následující hello tooconfig.py kódu, budete potřebovat hodnoty hello tooalter **DOCUMENTDB\_hostitele** a **DOCUMENTDB\_klíč** v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-208">Add hello following code tooconfig.py, you'll need tooalter hello values of **DOCUMENTDB\_HOST** and **DOCUMENTDB\_KEY** in hello next step.</span></span>
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. <span data-ttu-id="dffbc-209">V hello [portál Azure](https://portal.azure.com/), přejděte toohello **klíče** okno kliknutím **Procházet**, **Azure Cosmos DB účty**, klikněte dvakrát na název hello Dobrý den účtu toouse a pak klikněte na tlačítko hello **klíče** tlačítka na hello **Essentials** oblasti.</span><span class="sxs-lookup"><span data-stu-id="dffbc-209">In hello [Azure portal](https://portal.azure.com/), navigate toohello **Keys** blade by clicking **Browse**, **Azure Cosmos DB Accounts**, double-click hello name of hello account toouse, and then click hello **Keys** button in hello **Essentials** area.</span></span> <span data-ttu-id="dffbc-210">V hello **klíče** okno, kopie hello **URI** a vložte ji do hello **config.py** souboru jako hodnota hello hello **DOCUMENTDB\_hostitele**  vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dffbc-210">In hello **Keys** blade, copy hello **URI** value and paste it into hello **config.py** file, as hello value for hello **DOCUMENTDB\_HOST** property.</span></span> 
4. <span data-ttu-id="dffbc-211">Zpět v hello portál Azure, v hello **klíče** okně kopie hello hodnotu hello **primární klíč** nebo hello **sekundární klíč**a vložte jej do hello **config.py**  souboru jako hodnota hello hello **DOCUMENTDB\_klíč** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dffbc-211">Back in hello Azure portal, in hello **Keys** blade, copy hello value of hello **Primary Key** or hello **Secondary Key**, and paste it into hello **config.py** file, as hello value for hello **DOCUMENTDB\_KEY** property.</span></span>
5. <span data-ttu-id="dffbc-212">V hello  **\_ \_init\_\_.py** soubor, přidejte následující řádek hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-212">In hello **\_\_init\_\_.py** file, add hello following line.</span></span> 
   
        app.config.from_object('config')
   
    <span data-ttu-id="dffbc-213">Tak, aby obsah hello hello souboru je:</span><span class="sxs-lookup"><span data-stu-id="dffbc-213">So that hello content of hello file is:</span></span>
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. <span data-ttu-id="dffbc-214">Po přidání všech souborů hello, Průzkumník řešení by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="dffbc-214">After adding all hello files, Solution Explorer should look like this:</span></span>
   
    ![Snímek obrazovky okna Průzkumníka řešení Visual Studio hello](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a><span data-ttu-id="dffbc-216">Krok 4: Místní spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="dffbc-216">Step 4: Run your web application locally</span></span>
1. <span data-ttu-id="dffbc-217">Vytvoření řešení hello stisknutím **Ctrl**+**Shift**+**B**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-217">Build hello solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="dffbc-218">Po úspěšném sestavení hello spustit hello webu stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-218">Once hello build succeeds, start hello website by pressing **F5**.</span></span> <span data-ttu-id="dffbc-219">Měli byste vidět následující hello na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="dffbc-219">You should see hello following on your screen.</span></span>
   
    ![Snímek obrazovky znázorňující hello Python + Azure Cosmos DB hlasovací aplikace zobrazí ve webovém prohlížeči](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. <span data-ttu-id="dffbc-221">Klikněte na tlačítko **Create/Clear hello hlasování databáze** toogenerate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="dffbc-221">Click **Create/Clear hello Voting Database** toogenerate hello database.</span></span>
   
    ![Snímek obrazovky hello vytvořit stránku hello webové aplikace – podrobnosti o vývoji](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. <span data-ttu-id="dffbc-223">Pak vyberte svou možnost kliknutím na **Vote** (Hlasovat).</span><span class="sxs-lookup"><span data-stu-id="dffbc-223">Then, click **Vote** and select your option.</span></span>
   
    ![Snímek obrazovky aplikace hello webové aplikace se zobrazenou otázkou k hlasování](./media/documentdb-python-application/cosmos-db-vote.png)
5. <span data-ttu-id="dffbc-225">Pro každý hlas, který udělíte navýší příslušný čítač hello.</span><span class="sxs-lookup"><span data-stu-id="dffbc-225">For every vote you cast, it increments hello appropriate counter.</span></span>
   
    ![Snímek obrazovky hello výsledky zobrazené stránce hlasování hello](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. <span data-ttu-id="dffbc-227">Zastavte ladění projektu hello stisknutím kláves Shift + F5.</span><span class="sxs-lookup"><span data-stu-id="dffbc-227">Stop debugging hello project by pressing Shift+F5.</span></span>

## <a name="step-5-deploy-hello-web-application-tooazure"></a><span data-ttu-id="dffbc-228">Krok 5: Nasazení hello webové aplikace tooAzure</span><span class="sxs-lookup"><span data-stu-id="dffbc-228">Step 5: Deploy hello web application tooAzure</span></span>
<span data-ttu-id="dffbc-229">Teď, když máte hello hotové aplikace správně pracovat se Cosmos DB, vytvoříme toodeploy tento tooAzure.</span><span class="sxs-lookup"><span data-stu-id="dffbc-229">Now that you have hello complete application working correctly against Cosmos DB, we're going toodeploy this tooAzure.</span></span>

1. <span data-ttu-id="dffbc-230">Klikněte pravým tlačítkem na projekt hello v Průzkumníkovi řešení (ujistěte se, že nejste spuštěná místně) a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-230">Right-click hello project in Solution Explorer (make sure you're not still running it locally) and select **Publish**.</span></span>  
   
     ![Snímek obrazovky hello tutorial vybraného v Průzkumníkovi řešení se zvýrazněnou možností publikovat hello](./media/documentdb-python-application/image20.png)
2. <span data-ttu-id="dffbc-232">V hello **publikovat** dialogové okno, vyberte **Microsoft Azure App Service**, vyberte **vytvořit nový**a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-232">In hello **Publish** dialog box, select **Microsoft Azure App Service**, select **Create New**, and then click **Publish**.</span></span>
   
    ![Snímek obrazovky okna publikování webu hello se zvýrazněnou službou Microsoft Azure App Service](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. <span data-ttu-id="dffbc-234">V hello **vytvořit službu App Service** dialogovém okně zadejte název hello pro vaši webovou aplikaci spolu s vaší **předplatné**, **skupiny prostředků**, a **plán služby App Service** , pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dffbc-234">In hello **Create App Service** dialog box, enter hello name for your web app along with your **Subscription**, **Resource Group**, and **App Service Plan**, then click **Create**.</span></span>
   
    ![Snímek obrazovky okna okno Microsoft Azure webové aplikace hello](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. <span data-ttu-id="dffbc-236">Během pár sekund bude Visual Studio dokončí publikování app service a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!</span><span class="sxs-lookup"><span data-stu-id="dffbc-236">In a few seconds, Visual Studio will finish publishing your app service and launch a browser where you can see your handiwork running in Azure!</span></span>

    ![Snímek obrazovky okna okno Microsoft Azure webové aplikace hello](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a><span data-ttu-id="dffbc-238">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="dffbc-238">Troubleshooting</span></span>
<span data-ttu-id="dffbc-239">Pokud je to první aplikace Python hello jste spustili na počítači, ujistěte se, že hello následující složky (nebo hello ekvivalentní umístění instalací) jsou součástí vaše proměnná PATH:</span><span class="sxs-lookup"><span data-stu-id="dffbc-239">If this is hello first Python app you've run on your computer, ensure that hello following folders (or hello equivalent installation locations) are included in your PATH variable:</span></span>

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

<span data-ttu-id="dffbc-240">Pokud narazíte na chyby na stránce hlasování a jste projekt pojmenovali jinak než **kurzu**, ujistěte se, že  **\_ \_init\_\_.py** odkazy na hello název správný projekt v řádku hello: `import tutorial.view`.</span><span class="sxs-lookup"><span data-stu-id="dffbc-240">If you receive an error on your vote page, and you named your project something other than **tutorial**, make sure that **\_\_init\_\_.py** references hello correct project name in hello line: `import tutorial.view`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dffbc-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dffbc-241">Next steps</span></span>
<span data-ttu-id="dffbc-242">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="dffbc-242">Congratulations!</span></span> <span data-ttu-id="dffbc-243">Právě dokončení svou první webovou aplikaci Python, pomocí Cosmos DB a publikovali jste ji tooAzure.</span><span class="sxs-lookup"><span data-stu-id="dffbc-243">You have just completed your first Python web application using Cosmos DB and published it tooAzure.</span></span>

<span data-ttu-id="dffbc-244">Toto téma často aktualizujeme a vylepšujeme podle vaší zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="dffbc-244">We update and improve this topic frequently based on your feedback.</span></span>  <span data-ttu-id="dffbc-245">Jednou jste dokončili kurz hello, použijte prosím hlasovací tlačítka v hello horní a dolní části této stránky hello a být jisti tooinclude svůj názor, jaká vylepšení chcete toosee provedeny.</span><span class="sxs-lookup"><span data-stu-id="dffbc-245">Once you've completed hello tutorial, please using hello voting buttons at hello top and bottom of this page, and be sure tooinclude your feedback on what improvements you want toosee made.</span></span> <span data-ttu-id="dffbc-246">Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích.</span><span class="sxs-lookup"><span data-stu-id="dffbc-246">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="dffbc-247">tooadd další funkce tooyour webovou aplikaci, zkontrolujte hello rozhraní API dostupná v hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="dffbc-247">tooadd additional functionality tooyour web application, review hello APIs available in hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span></span>

<span data-ttu-id="dffbc-248">Další informace o Azure, Visual Studio a Pythonu najdete v tématu hello [středisku pro vývojáře Python](https://azure.microsoft.com/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="dffbc-248">For more information about Azure, Visual Studio, and Python, see hello [Python Developer Center](https://azure.microsoft.com/develop/python/).</span></span> 

<span data-ttu-id="dffbc-249">Další kurzy Pythonu Flask najdete v části [hello Flask velký kurz, část I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span><span class="sxs-lookup"><span data-stu-id="dffbc-249">For additional Python Flask tutorials, see [hello Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span></span> 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
