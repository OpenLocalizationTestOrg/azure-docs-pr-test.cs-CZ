---
title: "Python webové aplikace s Bottle v Azure"
description: "Tento kurz vás seznámí s postupem spuštění webové aplikace v jazyce Python ve službě Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 84f043b8-9d05-479f-a9d0-d0d9a32a0bb9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2016
ms.author: huvalo
ms.openlocfilehash: de5831defc395cd8a4033be8c1fc5dc6cbc9d683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-web-apps-with-bottle-in-azure"></a><span data-ttu-id="ba333-103">Vytvoření webové aplikace pomocí Bottle v Azure</span><span class="sxs-lookup"><span data-stu-id="ba333-103">Creating web apps with Bottle in Azure</span></span>
<span data-ttu-id="ba333-104">Tento kurz popisuje, jak začít pracovat s Python v Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="ba333-104">This tutorial describes how to get started running Python in Azure App Service Web Apps.</span></span> <span data-ttu-id="ba333-105">Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!</span><span class="sxs-lookup"><span data-stu-id="ba333-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="ba333-106">Souběžně s růstem aplikace můžete přejít na placené hostování a můžete také integrovat se všemi ostatními službami Azure.</span><span class="sxs-lookup"><span data-stu-id="ba333-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="ba333-107">Vytvoříte webovou aplikaci pomocí webového rozhraní Bottle (viz alternativní verze tohoto kurzu pro [Django](web-sites-python-create-deploy-django-app.md) a [Flask](web-sites-python-create-deploy-flask-app.md)).</span><span class="sxs-lookup"><span data-stu-id="ba333-107">You will create a web app using the Bottle web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Flask](web-sites-python-create-deploy-flask-app.md)).</span></span> <span data-ttu-id="ba333-108">Vytvoříte webovou aplikaci z Azure Marketplace, nastavíte nasazení Git a místně naklonujete úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba333-108">You will create the web app from the Azure Marketplace, set up Git deployment, and clone the repository locally.</span></span> <span data-ttu-id="ba333-109">Pak bude místní spuštění webové aplikace, měnit, potvrzení a vložit je na [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="ba333-109">Then you will run the web app locally, make changes, commit and push them to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="ba333-110">V tomto kurzu se dozvíte, jak to provést ze systému Windows nebo Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="ba333-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="ba333-111">Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba333-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="ba333-112">Není vyžadována platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="ba333-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ba333-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ba333-113">Prerequisites</span></span>
* <span data-ttu-id="ba333-114">Windows, Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="ba333-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="ba333-115">Python 2.7 nebo 3.4</span><span class="sxs-lookup"><span data-stu-id="ba333-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="ba333-116">setuptools, pip, virtualenv (pouze Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="ba333-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="ba333-117">Git</span><span class="sxs-lookup"><span data-stu-id="ba333-117">Git</span></span>
* <span data-ttu-id="ba333-118">[Python Tools 2.2 pro Visual Studio][Python Tools 2.2 pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná</span><span class="sxs-lookup"><span data-stu-id="ba333-118">[Python Tools 2.2 for Visual Studio][Python Tools 2.2 for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="ba333-119">**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ba333-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="ba333-120">Windows</span><span class="sxs-lookup"><span data-stu-id="ba333-120">Windows</span></span>
<span data-ttu-id="ba333-121">Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].</span><span class="sxs-lookup"><span data-stu-id="ba333-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="ba333-122">Tím se nainstaluje 32bitová verze jazyka Python, setuptools, pip, virtualenv atd. (32bitová verze jazyka Python je nainstalována v hostitelských počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="ba333-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span> <span data-ttu-id="ba333-123">Alternativně můžete získat jazyk Python z webu [python.org].</span><span class="sxs-lookup"><span data-stu-id="ba333-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="ba333-124">V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].</span><span class="sxs-lookup"><span data-stu-id="ba333-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="ba333-125">Pokud používáte Visual Studio, můžete použít integrovanou podporu Git.</span><span class="sxs-lookup"><span data-stu-id="ba333-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="ba333-126">Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="ba333-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="ba333-127">Tato položka je volitelná, ale pokud máte sadu [Visual Studio], včetně bezplatné sady Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, tato položka vám poskytne skvělé rozhraní IDE pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="ba333-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="ba333-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="ba333-128">Mac/Linux</span></span>
<span data-ttu-id="ba333-129">Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.</span><span class="sxs-lookup"><span data-stu-id="ba333-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-the-azure-portal"></a><span data-ttu-id="ba333-130">Vytvoření webové aplikace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ba333-130">Web app creation on the Azure Portal</span></span>
<span data-ttu-id="ba333-131">Prvním krokem při vytváření aplikace je vytvoření webové aplikace pomocí [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ba333-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span>  

1. <span data-ttu-id="ba333-132">Přihlaste se k portálu Azure a v levém dolním rohu klikněte na tlačítko **NOVÉ**.</span><span class="sxs-lookup"><span data-stu-id="ba333-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span> 
2. <span data-ttu-id="ba333-133">Do vyhledávacího pole zadejte „python“.</span><span class="sxs-lookup"><span data-stu-id="ba333-133">In the search box, type "python".</span></span>
3. <span data-ttu-id="ba333-134">Ve výsledcích hledání vyberte **Bottle**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ba333-134">In the search results, select **Bottle**, then click **Create**.</span></span>
4. <span data-ttu-id="ba333-135">Nakonfigurujte novou aplikaci Bottle, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni.</span><span class="sxs-lookup"><span data-stu-id="ba333-135">Configure the new Bottle app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="ba333-136">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ba333-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="ba333-137">Pro nově vytvořenou webovou aplikaci nakonfigurujte publikování Git podle pokynů uvedených v tématu [Místní nasazení GIT ve službě Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="ba333-137">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="ba333-138">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="ba333-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="ba333-139">Obsah úložiště Git</span><span class="sxs-lookup"><span data-stu-id="ba333-139">Git repository contents</span></span>
<span data-ttu-id="ba333-140">Zde je uveden přehled souborů, které naleznete v počátečním úložišti Git, jež budeme v následující části klonovat.</span><span class="sxs-lookup"><span data-stu-id="ba333-140">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

<span data-ttu-id="ba333-141">Hlavní zdroje pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba333-141">Main sources for the application.</span></span> <span data-ttu-id="ba333-142">Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.</span><span class="sxs-lookup"><span data-stu-id="ba333-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="ba333-143">Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.</span><span class="sxs-lookup"><span data-stu-id="ba333-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \app.py

<span data-ttu-id="ba333-144">Podpora serveru místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="ba333-144">Local development server support.</span></span> <span data-ttu-id="ba333-145">Použijte ke spuštění aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="ba333-145">Use this to run the application locally.</span></span>

    \BottleWebProject.pyproj
    \BottleWebProject.sln

<span data-ttu-id="ba333-146">Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="ba333-146">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="ba333-147">Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.</span><span class="sxs-lookup"><span data-stu-id="ba333-147">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="ba333-148">Externí balíčky vyžadované touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="ba333-148">External packages needed by this application.</span></span> <span data-ttu-id="ba333-149">Skript nasazení nainstaluje nástrojem pip balíčky uvedené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="ba333-149">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="ba333-150">Konfigurační soubory služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ba333-150">IIS configuration files.</span></span> <span data-ttu-id="ba333-151">Skript nasazení použije příslušný soubor web.x.y.config a zkopíruje jej jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="ba333-151">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="ba333-152">Volitelné soubory – přizpůsobení nasazení</span><span class="sxs-lookup"><span data-stu-id="ba333-152">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="ba333-153">Volitelné soubory – modul Python runtime</span><span class="sxs-lookup"><span data-stu-id="ba333-153">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="ba333-154">Další soubory na serveru</span><span class="sxs-lookup"><span data-stu-id="ba333-154">Additional files on server</span></span>
<span data-ttu-id="ba333-155">Na serveru existují některé soubory, které nejsou přidány do úložiště git.</span><span class="sxs-lookup"><span data-stu-id="ba333-155">Some files exist on the server but are not added to the git repository.</span></span> <span data-ttu-id="ba333-156">Tyto soubory jsou vytvořeny skriptem nasazení.</span><span class="sxs-lookup"><span data-stu-id="ba333-156">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="ba333-157">Konfigurační soubor služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ba333-157">IIS configuration file.</span></span> <span data-ttu-id="ba333-158">Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.</span><span class="sxs-lookup"><span data-stu-id="ba333-158">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="ba333-159">Virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="ba333-159">Python virtual environment.</span></span> <span data-ttu-id="ba333-160">Toto prostředí je vytvořeno během nasazení, pokud ve webové aplikaci ještě neexistuje kompatibilní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba333-160">Created during deployment if a compatible virtual environment doesn't already exist on the web app.</span></span>  <span data-ttu-id="ba333-161">Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, avšak nástroj pip instalaci přeskočí, pokud jsou dané balíčky již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="ba333-161">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="ba333-162">Následující 3 části popisují postup při vývoji webové aplikace ve 3 různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="ba333-162">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="ba333-163">Windows s nástroji Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba333-163">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="ba333-164">Windows s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="ba333-164">Windows, with command line</span></span>
* <span data-ttu-id="ba333-165">Mac/Linux s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="ba333-165">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="ba333-166">Vývoj webových aplikací – Windows – Python Tools pro sadu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba333-166">Web App development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="ba333-167">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="ba333-167">Clone the repository</span></span>
<span data-ttu-id="ba333-168">Nejprve naklonujte úložiště pomocí adresy url poskytnuté na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba333-168">First, clone the repository using the url provided on the Azure Portal.</span></span> <span data-ttu-id="ba333-169">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="ba333-169">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="ba333-170">Otevřete soubor řešení (.sln), který je zahrnut v kořenovém adresáři úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba333-170">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="ba333-171">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="ba333-171">Create virtual environment</span></span>
<span data-ttu-id="ba333-172">Nyní vytvoříme virtuální prostředí pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="ba333-172">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="ba333-173">Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.</span><span class="sxs-lookup"><span data-stu-id="ba333-173">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="ba333-174">Ujistěte se, zda název prostředí je `env`.</span><span class="sxs-lookup"><span data-stu-id="ba333-174">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="ba333-175">Vyberte základní překladač.</span><span class="sxs-lookup"><span data-stu-id="ba333-175">Select the base interpreter.</span></span> <span data-ttu-id="ba333-176">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="ba333-176">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="ba333-177">Ujistěte se, zda je zaškrtnutá možnost stažení a instalace balíčků.</span><span class="sxs-lookup"><span data-stu-id="ba333-177">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="ba333-178">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ba333-178">Click **Create**.</span></span> <span data-ttu-id="ba333-179">Tím dojde k vytvoření virtuálního prostředí a instalaci závislostí uvedených v souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="ba333-179">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="ba333-180">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="ba333-180">Run using development server</span></span>
<span data-ttu-id="ba333-181">Stisknutím klávesy F5 spusťte ladění, čímž se automaticky otevře webový prohlížeč s místně spuštěnou stránkou.</span><span class="sxs-lookup"><span data-stu-id="ba333-181">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

<span data-ttu-id="ba333-182">Můžete nastavit zarážky ve zdrojích, používat okna kukátka atd. Další informace o jednotlivých funkcích naleznete v části [Dokumentace nástrojů Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="ba333-182">You can set breakpoints in the sources, use the watch windows, etc. See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="ba333-183">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="ba333-183">Make changes</span></span>
<span data-ttu-id="ba333-184">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba333-184">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="ba333-185">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="ba333-185">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a><span data-ttu-id="ba333-186">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="ba333-186">Install more packages</span></span>
<span data-ttu-id="ba333-187">Aplikace může mít kromě jazyka Python a Bottle závislosti.</span><span class="sxs-lookup"><span data-stu-id="ba333-187">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="ba333-188">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="ba333-188">You can install additional packages using pip.</span></span> <span data-ttu-id="ba333-189">Chcete-li nainstalovat balíček, klikněte pravým tlačítkem na virtuální prostředí a vyberte možnost **Instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="ba333-189">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="ba333-190">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte `azure`:</span><span class="sxs-lookup"><span data-stu-id="ba333-190">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

<span data-ttu-id="ba333-191">Klikněte pravým tlačítkem na virtuální prostředí a výběrem možnosti **Generovat soubor requirements.txt** aktualizujte soubor requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="ba333-191">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="ba333-192">Poté potvrďte změny souboru requirements.txt do úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="ba333-192">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="ba333-193">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="ba333-193">Deploy to Azure</span></span>
<span data-ttu-id="ba333-194">Chcete-li aktivovat nasazení, klikněte na možnost **Synchronizovat** nebo **Vložit změny (push)**.</span><span class="sxs-lookup"><span data-stu-id="ba333-194">To trigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="ba333-195">Synchronizace provádí vložení změn (push) i přijetí změn (pull).</span><span class="sxs-lookup"><span data-stu-id="ba333-195">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

<span data-ttu-id="ba333-196">První nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.</span><span class="sxs-lookup"><span data-stu-id="ba333-196">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="ba333-197">Sada Visual Studio nezobrazuje průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="ba333-197">Visual Studio doesn't show the progress of the deployment.</span></span> <span data-ttu-id="ba333-198">Chcete-li překontrolovat výstup, informace naleznete v tématu [Řešení potíží – nasazení](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="ba333-198">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="ba333-199">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="ba333-199">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="ba333-200">Vývoj webových aplikací – Windows – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="ba333-200">Web app development - Windows - Command Line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="ba333-201">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="ba333-201">Clone the repository</span></span>
<span data-ttu-id="ba333-202">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="ba333-202">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="ba333-203">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="ba333-203">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="ba333-204">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="ba333-204">Create virtual environment</span></span>
<span data-ttu-id="ba333-205">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).</span><span class="sxs-lookup"><span data-stu-id="ba333-205">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="ba333-206">Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba333-206">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="ba333-207">Nezapomeňte použít stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo v okně Nastavení aplikace pro vaši webovou aplikaci na portálu Azure)</span><span class="sxs-lookup"><span data-stu-id="ba333-207">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade for your web app in the Azure Portal)</span></span>

<span data-ttu-id="ba333-208">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="ba333-208">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="ba333-209">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="ba333-209">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="ba333-210">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="ba333-210">Install any external packages required by your application.</span></span> <span data-ttu-id="ba333-211">Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="ba333-211">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="ba333-212">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="ba333-212">Run using development server</span></span>
<span data-ttu-id="ba333-213">Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="ba333-213">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python app.py

<span data-ttu-id="ba333-214">Konzola zobrazí adresa URL a port, jimž server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="ba333-214">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

<span data-ttu-id="ba333-215">Poté tuto adresu URL otevřete ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ba333-215">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="ba333-216">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="ba333-216">Make changes</span></span>
<span data-ttu-id="ba333-217">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba333-217">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="ba333-218">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="ba333-218">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="ba333-219">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="ba333-219">Install more packages</span></span>
<span data-ttu-id="ba333-220">Aplikace může mít kromě jazyka Python a Bottle závislosti.</span><span class="sxs-lookup"><span data-stu-id="ba333-220">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="ba333-221">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="ba333-221">You can install additional packages using pip.</span></span> <span data-ttu-id="ba333-222">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ba333-222">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="ba333-223">Nezapomeňte aktualizovat soubor requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="ba333-223">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="ba333-224">Potvrďte změny:</span><span class="sxs-lookup"><span data-stu-id="ba333-224">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="ba333-225">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="ba333-225">Deploy to Azure</span></span>
<span data-ttu-id="ba333-226">Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:</span><span class="sxs-lookup"><span data-stu-id="ba333-226">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="ba333-227">Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="ba333-227">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="ba333-228">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="ba333-228">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="ba333-229">Vývoj webových aplikací – Mac/Linux – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="ba333-229">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="ba333-230">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="ba333-230">Clone the repository</span></span>
<span data-ttu-id="ba333-231">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="ba333-231">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="ba333-232">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="ba333-232">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="ba333-233">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="ba333-233">Create virtual environment</span></span>
<span data-ttu-id="ba333-234">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).</span><span class="sxs-lookup"><span data-stu-id="ba333-234">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="ba333-235">Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba333-235">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="ba333-236">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně Nastavení aplikace webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="ba333-236">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="ba333-237">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="ba333-237">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="ba333-238">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="ba333-238">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="ba333-239">nebo pyvenv env</span><span class="sxs-lookup"><span data-stu-id="ba333-239">or pyvenv env</span></span>

<span data-ttu-id="ba333-240">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="ba333-240">Install any external packages required by your application.</span></span> <span data-ttu-id="ba333-241">Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="ba333-241">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="ba333-242">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="ba333-242">Run using development server</span></span>
<span data-ttu-id="ba333-243">Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="ba333-243">You can launch the application under a development server with the following command:</span></span>

    env/bin/python app.py

<span data-ttu-id="ba333-244">Konzola zobrazí adresa URL a port, jimž server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="ba333-244">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

<span data-ttu-id="ba333-245">Poté tuto adresu URL otevřete ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ba333-245">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="ba333-246">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="ba333-246">Make changes</span></span>
<span data-ttu-id="ba333-247">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba333-247">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="ba333-248">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="ba333-248">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="ba333-249">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="ba333-249">Install more packages</span></span>
<span data-ttu-id="ba333-250">Aplikace může mít kromě jazyka Python a Bottle závislosti.</span><span class="sxs-lookup"><span data-stu-id="ba333-250">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="ba333-251">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="ba333-251">You can install additional packages using pip.</span></span> <span data-ttu-id="ba333-252">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ba333-252">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="ba333-253">Nezapomeňte aktualizovat soubor requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="ba333-253">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="ba333-254">Potvrďte změny:</span><span class="sxs-lookup"><span data-stu-id="ba333-254">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="ba333-255">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="ba333-255">Deploy to Azure</span></span>
<span data-ttu-id="ba333-256">Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:</span><span class="sxs-lookup"><span data-stu-id="ba333-256">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="ba333-257">Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="ba333-257">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="ba333-258">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="ba333-258">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="ba333-259">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="ba333-259">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="ba333-260">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="ba333-260">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="ba333-261">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba333-261">Next Steps</span></span>
<span data-ttu-id="ba333-262">Další informace o Bottle a nástrojích Python Tools pro sadu Visual Studio na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="ba333-262">Follow these links to learn more about Bottle and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="ba333-263">[Bottle dokumentace]</span><span class="sxs-lookup"><span data-stu-id="ba333-263">[Bottle Documentation]</span></span>
* <span data-ttu-id="ba333-264">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="ba333-264">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="ba333-265">Informace o používání Azure Table Storage a MongoDB:</span><span class="sxs-lookup"><span data-stu-id="ba333-265">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="ba333-266">[Bottle a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="ba333-266">[Bottle and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="ba333-267">[Bottle a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="ba333-267">[Bottle and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="ba333-268">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="ba333-268">What's changed</span></span>
* <span data-ttu-id="ba333-269">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="ba333-269">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="ba333-270">[Bottle a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="ba333-270">[Bottle and MongoDB on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span></span>
<span data-ttu-id="ba333-271">[Bottle a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="ba333-271">[Bottle and Azure Table Storage on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span></span>

<!--External Link references-->
<span data-ttu-id="ba333-272">[Azure SDK pro Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span><span class="sxs-lookup"><span data-stu-id="ba333-272">[Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span></span>
<span data-ttu-id="ba333-273">[Azure SDK pro Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span><span class="sxs-lookup"><span data-stu-id="ba333-273">[Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span></span>
<span data-ttu-id="ba333-274">[python.org]: http://www.python.org/</span><span class="sxs-lookup"><span data-stu-id="ba333-274">[python.org]: http://www.python.org/</span></span>
<span data-ttu-id="ba333-275">[Git pro Windows]: http://msysgit.github.io/</span><span class="sxs-lookup"><span data-stu-id="ba333-275">[Git for Windows]: http://msysgit.github.io/</span></span>
<span data-ttu-id="ba333-276">[GitHub pro Windows]: https://windows.github.com/</span><span class="sxs-lookup"><span data-stu-id="ba333-276">[GitHub for Windows]: https://windows.github.com/</span></span>
<span data-ttu-id="ba333-277">[Python Tools pro Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="ba333-277">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="ba333-278">[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="ba333-278">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="ba333-279">[Visual Studio]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="ba333-279">[Visual Studio]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="ba333-280">[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs </span><span class="sxs-lookup"><span data-stu-id="ba333-280">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs </span></span>
<span data-ttu-id="ba333-281">[Bottle dokumentace]: http://bottlepy.org/docs/dev/index.html</span><span class="sxs-lookup"><span data-stu-id="ba333-281">[Bottle Documentation]: http://bottlepy.org/docs/dev/index.html</span></span>

