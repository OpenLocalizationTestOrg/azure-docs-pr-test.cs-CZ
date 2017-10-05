---
title: "Vytvoření webové aplikace pomocí Flask v Azure"
description: "Kurz vás seznámí s webovou aplikaci Python spuštěné v Azure."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: b7f4ca3a-0b52-4108-90a7-f7e07ac73da0
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/20/2016
ms.author: huvalo
ms.openlocfilehash: 29457a39ee3df0bbdbc9869cdce0e14bd85b7302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a><span data-ttu-id="67229-103">Vytvoření webové aplikace pomocí Flask v Azure</span><span class="sxs-lookup"><span data-stu-id="67229-103">Creating web apps with Flask in Azure</span></span>
<span data-ttu-id="67229-104">Tento kurz popisuje, jak začít a spustit jazyk Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="67229-104">This tutorial describes how to get started running Python in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>  <span data-ttu-id="67229-105">Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!</span><span class="sxs-lookup"><span data-stu-id="67229-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span>  <span data-ttu-id="67229-106">Souběžně s růstem aplikace můžete přejít na placené hostování a můžete také integrovat se všemi ostatními službami Azure.</span><span class="sxs-lookup"><span data-stu-id="67229-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="67229-107">Vytvoříte aplikaci pomocí webového rozhraní Flask (viz alternativní verze tohoto kurzu pro [Django](web-sites-python-create-deploy-django-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="67229-107">You will create an application using the Flask web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span>  <span data-ttu-id="67229-108">Vytvoření webu z Galerie Azure, nastavíte nasazení Git a místně naklonujete úložiště.</span><span class="sxs-lookup"><span data-stu-id="67229-108">You will create the website from the Azure gallery, set up Git deployment, and clone the repository locally.</span></span>  <span data-ttu-id="67229-109">Poté místně spustíte aplikaci, provedete změny, potvrdíte je a nuceně vložíte do Azure.</span><span class="sxs-lookup"><span data-stu-id="67229-109">Then you will run the application locally, make changes, commit and push them to Azure.</span></span>  <span data-ttu-id="67229-110">V tomto kurzu se dozvíte, jak to provést ze systému Windows nebo Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="67229-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="67229-111">Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67229-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="67229-112">Není vyžadována platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="67229-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="67229-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="67229-113">Prerequisites</span></span>
* <span data-ttu-id="67229-114">Windows, Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="67229-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="67229-115">Python 2.7 nebo 3.4</span><span class="sxs-lookup"><span data-stu-id="67229-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="67229-116">setuptools, pip, virtualenv (pouze Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="67229-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="67229-117">Git</span><span class="sxs-lookup"><span data-stu-id="67229-117">Git</span></span>
* <span data-ttu-id="67229-118">[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="67229-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="67229-119">**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="67229-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="67229-120">Windows</span><span class="sxs-lookup"><span data-stu-id="67229-120">Windows</span></span>
<span data-ttu-id="67229-121">Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].</span><span class="sxs-lookup"><span data-stu-id="67229-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span>  <span data-ttu-id="67229-122">Tím se nainstaluje 32bitová verze jazyka Python, setuptools, pip, virtualenv atd. (32bitová verze jazyka Python je nainstalována v hostitelských počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="67229-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span>  <span data-ttu-id="67229-123">Alternativně můžete získat jazyk Python z webu [python.org].</span><span class="sxs-lookup"><span data-stu-id="67229-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="67229-124">V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].</span><span class="sxs-lookup"><span data-stu-id="67229-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span>  <span data-ttu-id="67229-125">Pokud používáte Visual Studio, můžete použít integrovanou podporu Git.</span><span class="sxs-lookup"><span data-stu-id="67229-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="67229-126">Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="67229-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span>  <span data-ttu-id="67229-127">Tato položka je volitelná, ale pokud máte sadu [Visual Studio], včetně bezplatné sady Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, tato položka vám poskytne skvělé rozhraní IDE pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="67229-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="67229-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="67229-128">Mac/Linux</span></span>
<span data-ttu-id="67229-129">Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.</span><span class="sxs-lookup"><span data-stu-id="67229-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-create-on-the-azure-portal"></a><span data-ttu-id="67229-130">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="67229-130">Web app create on the Azure Portal</span></span>
<span data-ttu-id="67229-131">Prvním krokem při vytváření aplikace je vytvoření webové aplikace pomocí [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="67229-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span> 

1. <span data-ttu-id="67229-132">Přihlaste se k portálu Azure a v levém dolním rohu klikněte na tlačítko **NOVÉ**.</span><span class="sxs-lookup"><span data-stu-id="67229-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span> 
2. <span data-ttu-id="67229-133">Klikněte na možnost **Web + mobilní zařízení**.</span><span class="sxs-lookup"><span data-stu-id="67229-133">Click **Web + Mobile**.</span></span>
3. <span data-ttu-id="67229-134">Do vyhledávacího pole zadejte „python“.</span><span class="sxs-lookup"><span data-stu-id="67229-134">In the search box, type "python".</span></span>
4. <span data-ttu-id="67229-135">Ve výsledcích hledání vyberte **Flask**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67229-135">In the search results, select **Flask**, then click **Create**.</span></span>
5. <span data-ttu-id="67229-136">Nakonfigurujte novou aplikaci Flask, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni.</span><span class="sxs-lookup"><span data-stu-id="67229-136">Configure the new Flask app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="67229-137">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67229-137">Then, click **Create**.</span></span>
6. <span data-ttu-id="67229-138">Pro nově vytvořenou webovou aplikaci nakonfigurujte publikování Git podle pokynů uvedených v tématu [Místní nasazení GIT ve službě Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="67229-138">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="67229-139">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="67229-139">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="67229-140">Obsah úložiště Git</span><span class="sxs-lookup"><span data-stu-id="67229-140">Git repository contents</span></span>
<span data-ttu-id="67229-141">Zde je uveden přehled souborů, které naleznete v počátečním úložišti Git, jež budeme v následující části klonovat.</span><span class="sxs-lookup"><span data-stu-id="67229-141">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

<span data-ttu-id="67229-142">Hlavní zdroje pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67229-142">Main sources for the application.</span></span>  <span data-ttu-id="67229-143">Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.</span><span class="sxs-lookup"><span data-stu-id="67229-143">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="67229-144">Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.</span><span class="sxs-lookup"><span data-stu-id="67229-144">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \runserver.py

<span data-ttu-id="67229-145">Podpora serveru místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="67229-145">Local development server support.</span></span> <span data-ttu-id="67229-146">Použijte ke spuštění aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="67229-146">Use this to run the application locally.</span></span>

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

<span data-ttu-id="67229-147">Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="67229-147">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="67229-148">Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.</span><span class="sxs-lookup"><span data-stu-id="67229-148">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="67229-149">Externí balíčky vyžadované touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="67229-149">External packages needed by this application.</span></span> <span data-ttu-id="67229-150">Skript nasazení nainstaluje nástrojem pip balíčky uvedené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="67229-150">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="67229-151">Konfigurační soubory služby IIS.</span><span class="sxs-lookup"><span data-stu-id="67229-151">IIS configuration files.</span></span>  <span data-ttu-id="67229-152">Skript nasazení použije příslušný soubor web.x.y.config a zkopíruje jej jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="67229-152">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="67229-153">Volitelné soubory – přizpůsobení nasazení</span><span class="sxs-lookup"><span data-stu-id="67229-153">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="67229-154">Volitelné soubory – modul Python runtime</span><span class="sxs-lookup"><span data-stu-id="67229-154">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="67229-155">Další soubory na serveru</span><span class="sxs-lookup"><span data-stu-id="67229-155">Additional files on server</span></span>
<span data-ttu-id="67229-156">Na serveru existují některé soubory, které nejsou přidány do úložiště git.</span><span class="sxs-lookup"><span data-stu-id="67229-156">Some files exist on the server but are not added to the git repository.</span></span>  <span data-ttu-id="67229-157">Tyto soubory jsou vytvořeny skriptem nasazení.</span><span class="sxs-lookup"><span data-stu-id="67229-157">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="67229-158">Konfigurační soubor služby IIS.</span><span class="sxs-lookup"><span data-stu-id="67229-158">IIS configuration file.</span></span>  <span data-ttu-id="67229-159">Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.</span><span class="sxs-lookup"><span data-stu-id="67229-159">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="67229-160">Virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="67229-160">Python virtual environment.</span></span>  <span data-ttu-id="67229-161">Vytvoří se během nasazení, pokud ještě neexistuje kompatibilní virtuální prostředí na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67229-161">Created during deployment if a compatible virtual environment doesn't already exist on the app.</span></span>  <span data-ttu-id="67229-162">Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, avšak nástroj pip instalaci přeskočí, pokud jsou dané balíčky již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="67229-162">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="67229-163">Následující 3 části popisují postup při vývoji webové aplikace ve 3 různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="67229-163">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="67229-164">Windows s nástroji Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="67229-164">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="67229-165">Windows s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="67229-165">Windows, with command line</span></span>
* <span data-ttu-id="67229-166">Mac/Linux s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="67229-166">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="67229-167">Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="67229-167">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="67229-168">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="67229-168">Clone the repository</span></span>
<span data-ttu-id="67229-169">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="67229-169">First, clone the repository using the URL provided on the Azure Portal.</span></span> <span data-ttu-id="67229-170">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="67229-170">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="67229-171">Otevřete soubor řešení (.sln), který je zahrnut v kořenovém adresáři úložiště.</span><span class="sxs-lookup"><span data-stu-id="67229-171">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="67229-172">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="67229-172">Create virtual environment</span></span>
<span data-ttu-id="67229-173">Nyní vytvoříme virtuální prostředí pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="67229-173">Now we'll create a virtual environment for local development.</span></span>  <span data-ttu-id="67229-174">Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.</span><span class="sxs-lookup"><span data-stu-id="67229-174">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="67229-175">Ujistěte se, zda název prostředí je `env`.</span><span class="sxs-lookup"><span data-stu-id="67229-175">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="67229-176">Vyberte základní překladač.</span><span class="sxs-lookup"><span data-stu-id="67229-176">Select the base interpreter.</span></span>  <span data-ttu-id="67229-177">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="67229-177">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="67229-178">Ujistěte se, zda je zaškrtnutá možnost stažení a instalace balíčků.</span><span class="sxs-lookup"><span data-stu-id="67229-178">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="67229-179">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67229-179">Click **Create**.</span></span>  <span data-ttu-id="67229-180">Tím dojde k vytvoření virtuálního prostředí a instalaci závislostí uvedených v souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="67229-180">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="67229-181">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="67229-181">Run using development server</span></span>
<span data-ttu-id="67229-182">Stisknutím klávesy F5 spusťte ladění, čímž se automaticky otevře webový prohlížeč s místně spuštěnou stránkou.</span><span class="sxs-lookup"><span data-stu-id="67229-182">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

<span data-ttu-id="67229-183">Můžete nastavit zarážky ve zdrojích, používat okna kukátka atd.  Další informace o jednotlivých funkcích naleznete v části [Dokumentace nástrojů Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="67229-183">You can set breakpoints in the sources, use the watch windows, etc.  See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="67229-184">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="67229-184">Make changes</span></span>
<span data-ttu-id="67229-185">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="67229-185">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="67229-186">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="67229-186">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a><span data-ttu-id="67229-187">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="67229-187">Install more packages</span></span>
<span data-ttu-id="67229-188">Aplikace může mít závislosti kromě Python a Flask.</span><span class="sxs-lookup"><span data-stu-id="67229-188">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="67229-189">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="67229-189">You can install additional packages using pip.</span></span>  <span data-ttu-id="67229-190">Chcete-li nainstalovat balíček, klikněte pravým tlačítkem na virtuální prostředí a vyberte možnost **Instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="67229-190">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="67229-191">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte `azure`:</span><span class="sxs-lookup"><span data-stu-id="67229-191">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

<span data-ttu-id="67229-192">Klikněte pravým tlačítkem na virtuální prostředí a výběrem možnosti **Generovat soubor requirements.txt** aktualizujte soubor requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="67229-192">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="67229-193">Poté potvrďte změny souboru requirements.txt do úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="67229-193">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="67229-194">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="67229-194">Deploy to Azure</span></span>
<span data-ttu-id="67229-195">Chcete-li aktivovat nasazení, klikněte na možnost **Synchronizovat** nebo **Vložit změny (push)**.</span><span class="sxs-lookup"><span data-stu-id="67229-195">To trigger a deployment, click on **Sync** or **Push**.</span></span>  <span data-ttu-id="67229-196">Synchronizace provádí vložení změn (push) i přijetí změn (pull).</span><span class="sxs-lookup"><span data-stu-id="67229-196">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

<span data-ttu-id="67229-197">První nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.</span><span class="sxs-lookup"><span data-stu-id="67229-197">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="67229-198">Sada Visual Studio nezobrazuje průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="67229-198">Visual Studio doesn't show the progress of the deployment.</span></span>  <span data-ttu-id="67229-199">Chcete-li překontrolovat výstup, informace naleznete v tématu [Řešení potíží – nasazení](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="67229-199">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="67229-200">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="67229-200">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="67229-201">Vývoj webových aplikací – Windows – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="67229-201">Web app development - Windows - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="67229-202">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="67229-202">Clone the repository</span></span>
<span data-ttu-id="67229-203">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="67229-203">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="67229-204">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="67229-204">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="67229-205">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="67229-205">Create virtual environment</span></span>
<span data-ttu-id="67229-206">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).</span><span class="sxs-lookup"><span data-stu-id="67229-206">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span>  <span data-ttu-id="67229-207">Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="67229-207">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="67229-208">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="67229-208">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="67229-209">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="67229-209">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="67229-210">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="67229-210">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="67229-211">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="67229-211">Install any external packages required by your application.</span></span> <span data-ttu-id="67229-212">Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="67229-212">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="67229-213">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="67229-213">Run using development server</span></span>
<span data-ttu-id="67229-214">Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="67229-214">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python runserver.py

<span data-ttu-id="67229-215">Konzola zobrazí adresa URL a port, jimž server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="67229-215">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

<span data-ttu-id="67229-216">Poté tuto adresu URL otevřete ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="67229-216">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="67229-217">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="67229-217">Make changes</span></span>
<span data-ttu-id="67229-218">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="67229-218">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="67229-219">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="67229-219">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="67229-220">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="67229-220">Install more packages</span></span>
<span data-ttu-id="67229-221">Aplikace může mít závislosti kromě Python a Flask.</span><span class="sxs-lookup"><span data-stu-id="67229-221">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="67229-222">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="67229-222">You can install additional packages using pip.</span></span>  <span data-ttu-id="67229-223">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="67229-223">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="67229-224">Nezapomeňte aktualizovat soubor requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="67229-224">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="67229-225">Potvrďte změny:</span><span class="sxs-lookup"><span data-stu-id="67229-225">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="67229-226">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="67229-226">Deploy to Azure</span></span>
<span data-ttu-id="67229-227">Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:</span><span class="sxs-lookup"><span data-stu-id="67229-227">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="67229-228">Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="67229-228">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="67229-229">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="67229-229">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="67229-230">Vývoj webových aplikací – Mac/Linux – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="67229-230">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="67229-231">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="67229-231">Clone the repository</span></span>
<span data-ttu-id="67229-232">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="67229-232">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="67229-233">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="67229-233">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="67229-234">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="67229-234">Create virtual environment</span></span>
<span data-ttu-id="67229-235">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).</span><span class="sxs-lookup"><span data-stu-id="67229-235">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span>  <span data-ttu-id="67229-236">Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="67229-236">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="67229-237">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="67229-237">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="67229-238">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="67229-238">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="67229-239">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="67229-239">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="67229-240">nebo pyvenv env</span><span class="sxs-lookup"><span data-stu-id="67229-240">or pyvenv env</span></span>

<span data-ttu-id="67229-241">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="67229-241">Install any external packages required by your application.</span></span> <span data-ttu-id="67229-242">Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="67229-242">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="67229-243">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="67229-243">Run using development server</span></span>
<span data-ttu-id="67229-244">Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="67229-244">You can launch the application under a development server with the following command:</span></span>

    env/bin/python runserver.py

<span data-ttu-id="67229-245">Konzola zobrazí adresa URL a port, jimž server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="67229-245">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

<span data-ttu-id="67229-246">Poté tuto adresu URL otevřete ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="67229-246">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="67229-247">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="67229-247">Make changes</span></span>
<span data-ttu-id="67229-248">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="67229-248">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="67229-249">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="67229-249">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="67229-250">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="67229-250">Install more packages</span></span>
<span data-ttu-id="67229-251">Aplikace může mít závislosti kromě Python a Flask.</span><span class="sxs-lookup"><span data-stu-id="67229-251">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="67229-252">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="67229-252">You can install additional packages using pip.</span></span>  <span data-ttu-id="67229-253">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="67229-253">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="67229-254">Nezapomeňte aktualizovat soubor requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="67229-254">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="67229-255">Potvrďte změny:</span><span class="sxs-lookup"><span data-stu-id="67229-255">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="67229-256">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="67229-256">Deploy to Azure</span></span>
<span data-ttu-id="67229-257">Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:</span><span class="sxs-lookup"><span data-stu-id="67229-257">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="67229-258">Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="67229-258">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="67229-259">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="67229-259">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="67229-260">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="67229-260">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="67229-261">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="67229-261">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="67229-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67229-262">Next Steps</span></span>
<span data-ttu-id="67229-263">Další informace o Flask a nástrojích Python Tools pro sadu Visual Studio na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="67229-263">Follow these links to learn more about Flask and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="67229-264">[Dokumentace flask]</span><span class="sxs-lookup"><span data-stu-id="67229-264">[Flask Documentation]</span></span>
* <span data-ttu-id="67229-265">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="67229-265">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="67229-266">Informace o používání Azure Table Storage a MongoDB:</span><span class="sxs-lookup"><span data-stu-id="67229-266">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="67229-267">[Flask a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="67229-267">[Flask and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="67229-268">[Flask a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="67229-268">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="67229-269">Další informace naleznete také [středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="67229-269">For more information, see also the [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="67229-270">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="67229-270">What's changed</span></span>
* <span data-ttu-id="67229-271">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="67229-271">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="67229-272">[Flask a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure</span><span class="sxs-lookup"><span data-stu-id="67229-272">[Flask and MongoDB on Azure with Python Tools for Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure</span></span>
<span data-ttu-id="67229-273">[Flask a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]: web-sites-python-ptvs-flask-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="67229-273">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-flask-table-storage.md</span></span>

<!--External Link references-->
<span data-ttu-id="67229-274">[Azure SDK pro Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span><span class="sxs-lookup"><span data-stu-id="67229-274">[Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span></span>
<span data-ttu-id="67229-275">[Azure SDK pro Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span><span class="sxs-lookup"><span data-stu-id="67229-275">[Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span></span>
<span data-ttu-id="67229-276">[python.org]: http://www.python.org/</span><span class="sxs-lookup"><span data-stu-id="67229-276">[python.org]: http://www.python.org/</span></span>
<span data-ttu-id="67229-277">[Git pro Windows]: http://msysgit.github.io/</span><span class="sxs-lookup"><span data-stu-id="67229-277">[Git for Windows]: http://msysgit.github.io/</span></span>
<span data-ttu-id="67229-278">[GitHub pro Windows]: https://windows.github.com/</span><span class="sxs-lookup"><span data-stu-id="67229-278">[GitHub for Windows]: https://windows.github.com/</span></span>
<span data-ttu-id="67229-279">[Python Tools pro Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="67229-279">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="67229-280">[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="67229-280">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="67229-281">[Visual Studio]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="67229-281">[Visual Studio]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="67229-282">[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs</span><span class="sxs-lookup"><span data-stu-id="67229-282">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs</span></span>
<span data-ttu-id="67229-283">[Dokumentace flask]: http://flask.pocoo.org/</span><span class="sxs-lookup"><span data-stu-id="67229-283">[Flask Documentation]: http://flask.pocoo.org/</span></span> 

