---
title: "Vytvoření webové aplikace pomocí rozhraní Django v Azure"
description: "Tento kurz vás seznámí s postupem spuštění webové aplikace v jazyce Python ve službě Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 9be1a05a-9460-49ae-94fb-9798f82c11cf
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2016
ms.author: huvalo
ms.openlocfilehash: 388a2db21dd1669b48b3204aaa322d7915905506
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-web-apps-with-django-in-azure"></a><span data-ttu-id="01c7e-103">Vytvoření webové aplikace pomocí rozhraní Django v Azure</span><span class="sxs-lookup"><span data-stu-id="01c7e-103">Creating web apps with Django in Azure</span></span>
<span data-ttu-id="01c7e-104">Tento kurz popisuje, jak začít a spustit jazyk Python ve službě [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="01c7e-104">This tutorial describes how to get started running Python on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="01c7e-105">Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!</span><span class="sxs-lookup"><span data-stu-id="01c7e-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="01c7e-106">Souběžně s růstem aplikace můžete přejít na placené hostování a můžete také integrovat se všemi ostatními službami Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="01c7e-107">Vytvoříte aplikaci pomocí webového rozhraní Django (pro rozhraní [Flask](web-sites-python-create-deploy-flask-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md) jsou k dispozici alternativní verze tohoto kurzu).</span><span class="sxs-lookup"><span data-stu-id="01c7e-107">You will create an application using the Django web framework (see alternate versions of this tutorial for [Flask](web-sites-python-create-deploy-flask-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span> <span data-ttu-id="01c7e-108">Vytvoříte webovou aplikaci z Azure Marketplace, nastavíte nasazení Git a místně naklonujete úložiště.</span><span class="sxs-lookup"><span data-stu-id="01c7e-108">You will create the web app from the Azure Marketplace, set up Git deployment, and clone the repository locally.</span></span> <span data-ttu-id="01c7e-109">Poté místně spustíte aplikaci, provedete změny, potvrdíte je a nuceně vložíte do Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-109">Then you will run the application locally, make changes, commit and push them to Azure.</span></span> <span data-ttu-id="01c7e-110">V tomto kurzu se dozvíte, jak to provést ze systému Windows nebo Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="01c7e-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="01c7e-111">Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01c7e-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="01c7e-112">Není vyžadována platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="01c7e-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="01c7e-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01c7e-113">Prerequisites</span></span>
* <span data-ttu-id="01c7e-114">Windows, Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="01c7e-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="01c7e-115">Python 2.7 nebo 3.4</span><span class="sxs-lookup"><span data-stu-id="01c7e-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="01c7e-116">setuptools, pip, virtualenv (pouze Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="01c7e-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="01c7e-117">Git</span><span class="sxs-lookup"><span data-stu-id="01c7e-117">Git</span></span>
* <span data-ttu-id="01c7e-118">[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="01c7e-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="01c7e-119">**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="01c7e-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="01c7e-120">Windows</span><span class="sxs-lookup"><span data-stu-id="01c7e-120">Windows</span></span>
<span data-ttu-id="01c7e-121">Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].</span><span class="sxs-lookup"><span data-stu-id="01c7e-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="01c7e-122">Tím se nainstaluje 32bitová verze jazyka Python, setuptools, pip, virtualenv atd. (32bitová verze jazyka Python je nainstalována v hostitelských počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="01c7e-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span> <span data-ttu-id="01c7e-123">Alternativně můžete získat jazyk Python z webu [python.org].</span><span class="sxs-lookup"><span data-stu-id="01c7e-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="01c7e-124">V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].</span><span class="sxs-lookup"><span data-stu-id="01c7e-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="01c7e-125">Pokud používáte Visual Studio, můžete použít integrovanou podporu Git.</span><span class="sxs-lookup"><span data-stu-id="01c7e-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="01c7e-126">Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="01c7e-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="01c7e-127">Tato položka je volitelná, ale pokud máte sadu [Visual Studio], včetně bezplatné sady Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, tato položka vám poskytne skvělé rozhraní IDE pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="01c7e-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="01c7e-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="01c7e-128">Mac/Linux</span></span>
<span data-ttu-id="01c7e-129">Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.</span><span class="sxs-lookup"><span data-stu-id="01c7e-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-portal"></a><span data-ttu-id="01c7e-130">Vytvoření webové aplikace v portálu</span><span class="sxs-lookup"><span data-stu-id="01c7e-130">Web App Creation on Portal</span></span>
<span data-ttu-id="01c7e-131">Prvním krokem při vytváření aplikace je vytvoření webové aplikace pomocí [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01c7e-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="01c7e-132">Přihlaste se k portálu Azure a v levém dolním rohu klikněte na tlačítko **NOVÉ**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span>
2. <span data-ttu-id="01c7e-133">Do vyhledávacího pole zadejte „python“.</span><span class="sxs-lookup"><span data-stu-id="01c7e-133">In the search box, type "python".</span></span>
3. <span data-ttu-id="01c7e-134">Ve výsledcích hledání vyberte položku **Django** (publikováno PTVS) a klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-134">In the search results, select **Django** (published by PTVS), then click **Create**.</span></span>
4. <span data-ttu-id="01c7e-135">Nakonfigurujte novou aplikaci Django, například pro ni vytvořte nový plán služby App Service a novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="01c7e-135">Configure the new Django app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="01c7e-136">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="01c7e-137">Pro nově vytvořenou webovou aplikaci nakonfigurujte publikování Git podle pokynů uvedených v tématu [Místní nasazení GIT ve službě Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="01c7e-137">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="01c7e-138">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="01c7e-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="01c7e-139">Obsah úložiště Git</span><span class="sxs-lookup"><span data-stu-id="01c7e-139">Git repository contents</span></span>
<span data-ttu-id="01c7e-140">Zde je uveden přehled souborů, které naleznete v počátečním úložišti Git, jež budeme v následující části klonovat.</span><span class="sxs-lookup"><span data-stu-id="01c7e-140">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

<span data-ttu-id="01c7e-141">Hlavní zdroje pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01c7e-141">Main sources for the application.</span></span> <span data-ttu-id="01c7e-142">Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.</span><span class="sxs-lookup"><span data-stu-id="01c7e-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span> <span data-ttu-id="01c7e-143">Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.</span><span class="sxs-lookup"><span data-stu-id="01c7e-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \manage.py

<span data-ttu-id="01c7e-144">Serverová podpora místní správy a vývoje.</span><span class="sxs-lookup"><span data-stu-id="01c7e-144">Local management and development server support.</span></span> <span data-ttu-id="01c7e-145">Tuto položku použijte k místnímu spuštění aplikace, synchronizaci databáze atd.</span><span class="sxs-lookup"><span data-stu-id="01c7e-145">Use this to run the application locally, synchronize the database, etc.</span></span>

    \db.sqlite3

<span data-ttu-id="01c7e-146">Výchozí databáze.</span><span class="sxs-lookup"><span data-stu-id="01c7e-146">Default database.</span></span> <span data-ttu-id="01c7e-147">Obsahuje nezbytné tabulky pro spuštění aplikace, ale neobsahuje žádné uživatele (chcete-li vytvořit uživatele, synchronizujte databázi).</span><span class="sxs-lookup"><span data-stu-id="01c7e-147">Includes the necessary tables for the application to run, but does not contain any users (synchronize the database to create a user).</span></span>

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

<span data-ttu-id="01c7e-148">Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="01c7e-148">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="01c7e-149">Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.</span><span class="sxs-lookup"><span data-stu-id="01c7e-149">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="01c7e-150">Externí balíčky vyžadované touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="01c7e-150">External packages needed by this application.</span></span> <span data-ttu-id="01c7e-151">Skript nasazení nainstaluje nástrojem pip balíčky uvedené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="01c7e-151">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="01c7e-152">Konfigurační soubory služby IIS.</span><span class="sxs-lookup"><span data-stu-id="01c7e-152">IIS configuration files.</span></span> <span data-ttu-id="01c7e-153">Skript nasazení použije příslušný soubor web.x.y.config a zkopíruje jej jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="01c7e-153">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="01c7e-154">Volitelné soubory – přizpůsobení nasazení</span><span class="sxs-lookup"><span data-stu-id="01c7e-154">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="01c7e-155">Volitelné soubory – modul Python runtime</span><span class="sxs-lookup"><span data-stu-id="01c7e-155">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="01c7e-156">Další soubory na serveru</span><span class="sxs-lookup"><span data-stu-id="01c7e-156">Additional files on server</span></span>
<span data-ttu-id="01c7e-157">Na serveru existují některé soubory, které nejsou přidány do úložiště git.</span><span class="sxs-lookup"><span data-stu-id="01c7e-157">Some files exist on the server but are not added to the git repository.</span></span> <span data-ttu-id="01c7e-158">Tyto soubory jsou vytvořeny skriptem nasazení.</span><span class="sxs-lookup"><span data-stu-id="01c7e-158">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="01c7e-159">Konfigurační soubor služby IIS.</span><span class="sxs-lookup"><span data-stu-id="01c7e-159">IIS configuration file.</span></span> <span data-ttu-id="01c7e-160">Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.</span><span class="sxs-lookup"><span data-stu-id="01c7e-160">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="01c7e-161">Virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="01c7e-161">Python virtual environment.</span></span> <span data-ttu-id="01c7e-162">Toto prostředí je vytvořeno během nasazení, pokud ve webové aplikaci ještě neexistuje kompatibilní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="01c7e-162">Created during deployment if a compatible virtual environment doesn't already exist on the web app.</span></span> <span data-ttu-id="01c7e-163">Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, avšak nástroj pip instalaci přeskočí, pokud jsou dané balíčky již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="01c7e-163">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="01c7e-164">Následující 3 části popisují postup při vývoji webové aplikace ve 3 různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="01c7e-164">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="01c7e-165">Windows s nástroji Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01c7e-165">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="01c7e-166">Windows s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="01c7e-166">Windows, with command line</span></span>
* <span data-ttu-id="01c7e-167">Mac/Linux s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="01c7e-167">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="01c7e-168">Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01c7e-168">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="01c7e-169">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="01c7e-169">Clone the repository</span></span>
<span data-ttu-id="01c7e-170">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-170">First, clone the repository using the URL provided on the Azure Portal.</span></span> <span data-ttu-id="01c7e-171">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="01c7e-171">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="01c7e-172">Otevřete soubor řešení (.sln), který je zahrnut v kořenovém adresáři úložiště.</span><span class="sxs-lookup"><span data-stu-id="01c7e-172">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="01c7e-173">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="01c7e-173">Create virtual environment</span></span>
<span data-ttu-id="01c7e-174">Nyní vytvoříme virtuální prostředí pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="01c7e-174">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="01c7e-175">Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-175">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="01c7e-176">Ujistěte se, zda název prostředí je `env`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-176">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="01c7e-177">Vyberte základní překladač.</span><span class="sxs-lookup"><span data-stu-id="01c7e-177">Select the base interpreter.</span></span> <span data-ttu-id="01c7e-178">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="01c7e-178">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="01c7e-179">Ujistěte se, zda je zaškrtnutá možnost stažení a instalace balíčků.</span><span class="sxs-lookup"><span data-stu-id="01c7e-179">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="01c7e-180">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-180">Click **Create**.</span></span> <span data-ttu-id="01c7e-181">Tím dojde k vytvoření virtuálního prostředí a instalaci závislostí uvedených v souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="01c7e-181">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="create-a-superuser"></a><span data-ttu-id="01c7e-182">Vytvoření superuživatele</span><span class="sxs-lookup"><span data-stu-id="01c7e-182">Create a superuser</span></span>
<span data-ttu-id="01c7e-183">V databázi, která je součástí aplikace, není definován žádný superuživatel.</span><span class="sxs-lookup"><span data-stu-id="01c7e-183">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="01c7e-184">Chcete-li používat funkci přihlašování v aplikaci nebo rozhraní správce Django (pokud se jej rozhodnete povolit), bude nutné vytvořit superuživatele.</span><span class="sxs-lookup"><span data-stu-id="01c7e-184">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="01c7e-185">Spusťte tento příkaz z příkazového řádku ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="01c7e-185">Run this from the command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="01c7e-186">Postupujte podle výzev a nastavte uživatelské jméno, heslo atd.</span><span class="sxs-lookup"><span data-stu-id="01c7e-186">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="01c7e-187">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="01c7e-187">Run using development server</span></span>
<span data-ttu-id="01c7e-188">Stisknutím klávesy F5 spusťte ladění, čímž se automaticky otevře webový prohlížeč s místně spuštěnou stránkou.</span><span class="sxs-lookup"><span data-stu-id="01c7e-188">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

<span data-ttu-id="01c7e-189">Můžete nastavit zarážky ve zdrojích, používat okna kukátka atd. Další informace o jednotlivých funkcích naleznete v části [Dokumentace nástrojů Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="01c7e-189">You can set breakpoints in the sources, use the watch windows, etc. See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="01c7e-190">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="01c7e-190">Make changes</span></span>
<span data-ttu-id="01c7e-191">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="01c7e-191">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="01c7e-192">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="01c7e-192">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a><span data-ttu-id="01c7e-193">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="01c7e-193">Install more packages</span></span>
<span data-ttu-id="01c7e-194">Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.</span><span class="sxs-lookup"><span data-stu-id="01c7e-194">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="01c7e-195">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="01c7e-195">You can install additional packages using pip.</span></span> <span data-ttu-id="01c7e-196">Chcete-li nainstalovat balíček, klikněte pravým tlačítkem na virtuální prostředí a vyberte možnost **Instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-196">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="01c7e-197">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte `azure`:</span><span class="sxs-lookup"><span data-stu-id="01c7e-197">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

<span data-ttu-id="01c7e-198">Klikněte pravým tlačítkem na virtuální prostředí a výběrem možnosti **Generovat soubor requirements.txt** aktualizujte soubor requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="01c7e-198">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="01c7e-199">Poté potvrďte změny souboru requirements.txt do úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="01c7e-199">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="01c7e-200">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="01c7e-200">Deploy to Azure</span></span>
<span data-ttu-id="01c7e-201">Chcete-li aktivovat nasazení, klikněte na možnost **Synchronizovat** nebo **Vložit změny (push)**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-201">To trigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="01c7e-202">Synchronizace provádí vložení změn (push) i přijetí změn (pull).</span><span class="sxs-lookup"><span data-stu-id="01c7e-202">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

<span data-ttu-id="01c7e-203">První nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.</span><span class="sxs-lookup"><span data-stu-id="01c7e-203">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="01c7e-204">Sada Visual Studio nezobrazuje průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="01c7e-204">Visual Studio doesn't show the progress of the deployment.</span></span> <span data-ttu-id="01c7e-205">Chcete-li překontrolovat výstup, informace naleznete v tématu [Řešení potíží – nasazení](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="01c7e-205">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="01c7e-206">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-206">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="01c7e-207">Vývoj webových aplikací – Windows – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="01c7e-207">Web app development - Windows - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="01c7e-208">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="01c7e-208">Clone the repository</span></span>
<span data-ttu-id="01c7e-209">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="01c7e-209">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="01c7e-210">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="01c7e-210">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="01c7e-211">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="01c7e-211">Create virtual environment</span></span>
<span data-ttu-id="01c7e-212">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).</span><span class="sxs-lookup"><span data-stu-id="01c7e-212">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="01c7e-213">Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="01c7e-213">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="01c7e-214">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně Nastavení aplikace webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="01c7e-214">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="01c7e-215">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="01c7e-215">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="01c7e-216">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="01c7e-216">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="01c7e-217">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="01c7e-217">Install any external packages required by your application.</span></span> <span data-ttu-id="01c7e-218">Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="01c7e-218">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="01c7e-219">Vytvoření superuživatele</span><span class="sxs-lookup"><span data-stu-id="01c7e-219">Create a superuser</span></span>
<span data-ttu-id="01c7e-220">V databázi, která je součástí aplikace, není definován žádný superuživatel.</span><span class="sxs-lookup"><span data-stu-id="01c7e-220">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="01c7e-221">Chcete-li používat funkci přihlašování v aplikaci nebo rozhraní správce Django (pokud se jej rozhodnete povolit), bude nutné vytvořit superuživatele.</span><span class="sxs-lookup"><span data-stu-id="01c7e-221">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="01c7e-222">Spusťte tento příkaz z příkazového řádku ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="01c7e-222">Run this from the command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="01c7e-223">Postupujte podle výzev a nastavte uživatelské jméno, heslo atd.</span><span class="sxs-lookup"><span data-stu-id="01c7e-223">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="01c7e-224">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="01c7e-224">Run using development server</span></span>
<span data-ttu-id="01c7e-225">Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="01c7e-225">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python manage.py runserver

<span data-ttu-id="01c7e-226">Konzola zobrazí adresa URL a port, jimž server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="01c7e-226">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

<span data-ttu-id="01c7e-227">Poté tuto adresu URL otevřete ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="01c7e-227">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="01c7e-228">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="01c7e-228">Make changes</span></span>
<span data-ttu-id="01c7e-229">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="01c7e-229">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="01c7e-230">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="01c7e-230">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="01c7e-231">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="01c7e-231">Install more packages</span></span>
<span data-ttu-id="01c7e-232">Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.</span><span class="sxs-lookup"><span data-stu-id="01c7e-232">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="01c7e-233">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="01c7e-233">You can install additional packages using pip.</span></span> <span data-ttu-id="01c7e-234">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="01c7e-234">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="01c7e-235">Nezapomeňte aktualizovat soubor requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="01c7e-235">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="01c7e-236">Potvrďte změny:</span><span class="sxs-lookup"><span data-stu-id="01c7e-236">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="01c7e-237">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="01c7e-237">Deploy to Azure</span></span>
<span data-ttu-id="01c7e-238">Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:</span><span class="sxs-lookup"><span data-stu-id="01c7e-238">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="01c7e-239">Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="01c7e-239">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="01c7e-240">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-240">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="01c7e-241">Vývoj webových aplikací – Mac/Linux – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="01c7e-241">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="01c7e-242">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="01c7e-242">Clone the repository</span></span>
<span data-ttu-id="01c7e-243">Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="01c7e-243">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="01c7e-244">Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="01c7e-244">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="01c7e-245">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="01c7e-245">Create virtual environment</span></span>
<span data-ttu-id="01c7e-246">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).</span><span class="sxs-lookup"><span data-stu-id="01c7e-246">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="01c7e-247">Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.</span><span class="sxs-lookup"><span data-stu-id="01c7e-247">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="01c7e-248">Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně Nastavení aplikace webové aplikace na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="01c7e-248">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="01c7e-249">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="01c7e-249">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="01c7e-250">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="01c7e-250">For Python 3.4:</span></span>

    python -m venv env

<span data-ttu-id="01c7e-251">nebo</span><span class="sxs-lookup"><span data-stu-id="01c7e-251">or</span></span>

    pyvenv env

<span data-ttu-id="01c7e-252">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="01c7e-252">Install any external packages required by your application.</span></span> <span data-ttu-id="01c7e-253">Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="01c7e-253">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="01c7e-254">Vytvoření superuživatele</span><span class="sxs-lookup"><span data-stu-id="01c7e-254">Create a superuser</span></span>
<span data-ttu-id="01c7e-255">V databázi, která je součástí aplikace, není definován žádný superuživatel.</span><span class="sxs-lookup"><span data-stu-id="01c7e-255">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="01c7e-256">Chcete-li používat funkci přihlašování v aplikaci nebo rozhraní správce Django (pokud se jej rozhodnete povolit), bude nutné vytvořit superuživatele.</span><span class="sxs-lookup"><span data-stu-id="01c7e-256">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="01c7e-257">Spusťte tento příkaz z příkazového řádku ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="01c7e-257">Run this from the command-line from your project folder:</span></span>

    env/bin/python manage.py createsuperuser

<span data-ttu-id="01c7e-258">Postupujte podle výzev a nastavte uživatelské jméno, heslo atd.</span><span class="sxs-lookup"><span data-stu-id="01c7e-258">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="01c7e-259">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="01c7e-259">Run using development server</span></span>
<span data-ttu-id="01c7e-260">Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="01c7e-260">You can launch the application under a development server with the following command:</span></span>

    env/bin/python manage.py runserver

<span data-ttu-id="01c7e-261">Konzola zobrazí adresa URL a port, jimž server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="01c7e-261">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

<span data-ttu-id="01c7e-262">Poté tuto adresu URL otevřete ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="01c7e-262">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="01c7e-263">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="01c7e-263">Make changes</span></span>
<span data-ttu-id="01c7e-264">Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.</span><span class="sxs-lookup"><span data-stu-id="01c7e-264">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="01c7e-265">Jakmile změny otestujete, potvrďte je do úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="01c7e-265">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="01c7e-266">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="01c7e-266">Install more packages</span></span>
<span data-ttu-id="01c7e-267">Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.</span><span class="sxs-lookup"><span data-stu-id="01c7e-267">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="01c7e-268">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="01c7e-268">You can install additional packages using pip.</span></span> <span data-ttu-id="01c7e-269">Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="01c7e-269">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="01c7e-270">Nezapomeňte aktualizovat soubor requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="01c7e-270">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="01c7e-271">Potvrďte změny:</span><span class="sxs-lookup"><span data-stu-id="01c7e-271">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="01c7e-272">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="01c7e-272">Deploy to Azure</span></span>
<span data-ttu-id="01c7e-273">Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:</span><span class="sxs-lookup"><span data-stu-id="01c7e-273">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="01c7e-274">Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="01c7e-274">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="01c7e-275">Chcete-li zobrazit změny, přejděte na adresu URL Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-275">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="01c7e-276">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="01c7e-276">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="01c7e-277">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="01c7e-277">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a><span data-ttu-id="01c7e-278">Řešení potíží – statické soubory</span><span class="sxs-lookup"><span data-stu-id="01c7e-278">Troubleshooting - Static Files</span></span>
<span data-ttu-id="01c7e-279">Rozhraní Django obsahuje koncepci shromažďování statických souborů.</span><span class="sxs-lookup"><span data-stu-id="01c7e-279">Django has the concept of collecting static files.</span></span> <span data-ttu-id="01c7e-280">Tato akce kopíruje veškeré statické soubory z původního umístění do jediné složky.</span><span class="sxs-lookup"><span data-stu-id="01c7e-280">This takes all the static files from their original location and copies them to a single folder.</span></span> <span data-ttu-id="01c7e-281">V případě této aplikace jsou kopírovány do složky `/static`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-281">For this application, they are copied to `/static`.</span></span>

<span data-ttu-id="01c7e-282">Tato akce se provádí proto, že statické soubory mohou pocházet z různých „aplikací“ Django.</span><span class="sxs-lookup"><span data-stu-id="01c7e-282">This is done because static files may come from different Django 'apps'.</span></span> <span data-ttu-id="01c7e-283">Statické soubory z rozhraní správce Django jsou například umístěny v podsložce knihovny Django ve virtuálním prostředí.</span><span class="sxs-lookup"><span data-stu-id="01c7e-283">For example, the static files from the Django admin interfaces are located in a Django library subfolder in the virtual environment.</span></span> <span data-ttu-id="01c7e-284">Statické soubory definované touto aplikací jsou umístěny ve složce `/app/static`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-284">Static files defined by this application are located in `/app/static`.</span></span> <span data-ttu-id="01c7e-285">Při používání vícero „aplikací“ Django se statické soubory nacházejí na více místech.</span><span class="sxs-lookup"><span data-stu-id="01c7e-285">As you use more Django 'apps', you'll have static files located in multiple places.</span></span>

<span data-ttu-id="01c7e-286">Při spuštění aplikace v režimu ladění obsluhuje aplikace statické soubory z původního umístění.</span><span class="sxs-lookup"><span data-stu-id="01c7e-286">When running the application in debug mode, the application serves the static files from their original location.</span></span>

<span data-ttu-id="01c7e-287">Při spuštění aplikace v režimu vydání aplikace statické soubory **neobsluhuje**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-287">When running the application in release mode, the application does **not** serve the static files.</span></span> <span data-ttu-id="01c7e-288">Za obsluhu souborů je odpovědný webový server.</span><span class="sxs-lookup"><span data-stu-id="01c7e-288">It is the responsibility of the web server to serve the files.</span></span> <span data-ttu-id="01c7e-289">U této aplikace bude služba IIS obsluhovat statické soubory z adresáře `/static`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-289">For this application, IIS will serve the static files from `/static`.</span></span>

<span data-ttu-id="01c7e-290">Shromažďování statických souborů je prováděno automaticky v rámci skriptu nasazení, přičemž dříve shromážděné soubory jsou vymazány.</span><span class="sxs-lookup"><span data-stu-id="01c7e-290">The collection of static files is done automatically as part of the deployment script, clearing previously collected files.</span></span> <span data-ttu-id="01c7e-291">To znamená, že shromažďování probíhá při každém nasazení, čímž nasazení mírně zpomaluje, ale současně zajišťuje, aby nebyly dostupné zastaralé soubory, a předchází tak potenciálním potížím se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="01c7e-291">This means the collection occurs on every deployment, slowing down deployment a bit, but it ensures that obsolete files won't be available, avoiding a potential security issue.</span></span>

<span data-ttu-id="01c7e-292">Chcete-li u aplikace rozhraní Django přeskočit shromažďování statických souborů:</span><span class="sxs-lookup"><span data-stu-id="01c7e-292">If you want to skip collection of static files for your Django application:</span></span>

    \.skipDjango

<span data-ttu-id="01c7e-293">Potom bude nutné provést shromažďování ručně v místním počítači:</span><span class="sxs-lookup"><span data-stu-id="01c7e-293">Then you'll need to do the collection manually on your local machine:</span></span>

    env\scripts\python manage.py collectstatic

<span data-ttu-id="01c7e-294">Potom odeberte složku `\static` z `.gitignore` a přidejte ji do úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="01c7e-294">Then remove the `\static` folder from `.gitignore` and add it to the Git repository.</span></span>

## <a name="troubleshooting---settings"></a><span data-ttu-id="01c7e-295">Řešení potíží – nastavení</span><span class="sxs-lookup"><span data-stu-id="01c7e-295">Troubleshooting - Settings</span></span>
<span data-ttu-id="01c7e-296">Různá nastavení aplikace lze změnit v souboru `DjangoWebProject/settings.py`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-296">Various settings for the application can be changed in `DjangoWebProject/settings.py`.</span></span>

<span data-ttu-id="01c7e-297">Z důvodu usnadnění práce vývojářů je povolen režim ladění.</span><span class="sxs-lookup"><span data-stu-id="01c7e-297">For developer convenience, debug mode is enabled.</span></span> <span data-ttu-id="01c7e-298">Díky tomu je například možné při místním spuštění zobrazit obrázky a další statický obsah, aniž by bylo nutné shromažďovat statické soubory.</span><span class="sxs-lookup"><span data-stu-id="01c7e-298">One nice side effect of that is you'll be able to see images and other static content when running locally, without having to collect static files.</span></span>

<span data-ttu-id="01c7e-299">Chcete-li zakázat režim ladění:</span><span class="sxs-lookup"><span data-stu-id="01c7e-299">To disable debug mode:</span></span>

    DEBUG = False

<span data-ttu-id="01c7e-300">Když je zakázáno ladění, hodnotu položky `ALLOWED_HOSTS` je nutné aktualizovat tak, aby obsahovala název hostitele Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-300">When debug is disabled, the value for `ALLOWED_HOSTS` needs to be updated to include the Azure host name.</span></span> <span data-ttu-id="01c7e-301">Příklad:</span><span class="sxs-lookup"><span data-stu-id="01c7e-301">For example:</span></span>

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

<span data-ttu-id="01c7e-302">Případně lze povolit všechny hostitele:</span><span class="sxs-lookup"><span data-stu-id="01c7e-302">or to enable any:</span></span>

    ALLOWED_HOSTS = (
        '*',
    )

<span data-ttu-id="01c7e-303">Je pravděpodobné, že v praxi budete chtít získávání názvu hostitele a přepínání mezi režimem ladění a vydání vyřešit komplexněji.</span><span class="sxs-lookup"><span data-stu-id="01c7e-303">In practice, you may want to do something more complex to deal with switching between debug and release mode, and getting the host name.</span></span>

<span data-ttu-id="01c7e-304">Můžete nastavit proměnné prostředí prostřednictvím stránky **KONFIGURACE** portálu Azure, a to v části **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01c7e-304">You can set environment variables through the Azure portal **CONFIGURE** page, in the **app settings** section.</span></span>  <span data-ttu-id="01c7e-305">To může být užitečné pro nastavení hodnot, které nechcete zobrazit ve zdrojích (připojovací řetězce, hesla atd.) nebo které chcete v Azure a v místním počítači nastavit odlišně.</span><span class="sxs-lookup"><span data-stu-id="01c7e-305">This can be useful for setting values that you may not want to appear in the sources (connection strings, passwords, etc), or that you want to set differently between Azure and your local machine.</span></span> <span data-ttu-id="01c7e-306">V souboru `settings.py` můžete dotazem zjišťovat proměnné prostředí pomocí `os.getenv`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-306">In `settings.py`, you can query the environment variables using `os.getenv`.</span></span>

## <a name="using-a-database"></a><span data-ttu-id="01c7e-307">Používání databáze</span><span class="sxs-lookup"><span data-stu-id="01c7e-307">Using a Database</span></span>
<span data-ttu-id="01c7e-308">Databáze, která je součástí aplikace, je databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="01c7e-308">The database that is included with the application is a sqlite database.</span></span> <span data-ttu-id="01c7e-309">Jedná se o praktickou a užitečnou výchozí databázi pro použití při vývoji, protože nevyžaduje téměř žádné nastavení.</span><span class="sxs-lookup"><span data-stu-id="01c7e-309">This is a convenient and useful default database to use for development, as it requires almost no setup.</span></span> <span data-ttu-id="01c7e-310">Databáze je uložena v souboru db.sqlite3 ve složce projektu.</span><span class="sxs-lookup"><span data-stu-id="01c7e-310">The database is stored in the db.sqlite3 file in the project folder.</span></span>

<span data-ttu-id="01c7e-311">Azure poskytuje databázové služby, které lze snadno použít z aplikace Django.</span><span class="sxs-lookup"><span data-stu-id="01c7e-311">Azure provides database services which are easy to use from a Django application.</span></span> <span data-ttu-id="01c7e-312">Kurzy zaměřené na používání databáze [SQL Database] a [MySQL] z aplikace Django vysvětlují postup vytvoření databázové služby, změny nastavení databáze v souboru `DjangoWebProject/settings.py` a popisují knihovny, které je nutné nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="01c7e-312">Tutorials for using [SQL Database] and [MySQL] from a Django application show the steps necessary to create the database service, change the database settings in `DjangoWebProject/settings.py`, and the libraries required to install.</span></span>

<span data-ttu-id="01c7e-313">Samozřejmě, pokud raději spravujete své vlastní databázové servery, můžete k tomu použít virtuální počítače se systémem Windows nebo Linux spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="01c7e-313">Of course, if you prefer to manage your own database servers, you can do so using Windows or Linux virtual machines running on Azure.</span></span>

## <a name="django-admin-interface"></a><span data-ttu-id="01c7e-314">Rozhraní správce Django</span><span class="sxs-lookup"><span data-stu-id="01c7e-314">Django Admin Interface</span></span>
<span data-ttu-id="01c7e-315">Jakmile začnete vytvářet modely, budete chtít databázi naplnit určitými daty.</span><span class="sxs-lookup"><span data-stu-id="01c7e-315">Once you start building your models, you'll want to populate the database with some data.</span></span> <span data-ttu-id="01c7e-316">Jedním ze snadných způsobů, jak interaktivně přidávat a upravovat obsah, je použít rozhraní pro správu Django.</span><span class="sxs-lookup"><span data-stu-id="01c7e-316">An easy way to do add and edit content interactively is to use the Django administration interface.</span></span>

<span data-ttu-id="01c7e-317">Kód rozhraní pro správu je uveden jako komentář ve zdrojích aplikace, ale je přehledně označen tak, abyste jej mohli snadno povolit (vyhledejte „admin“).</span><span class="sxs-lookup"><span data-stu-id="01c7e-317">The code for the admin interface is commented out in the application sources, but it's clearly marked so you can easily enable it (search for 'admin').</span></span>

<span data-ttu-id="01c7e-318">Po jeho povolení synchronizujte databázi, spusťte aplikaci a přejděte do `/admin`.</span><span class="sxs-lookup"><span data-stu-id="01c7e-318">After it's enabled, synchronize the database, run the application and navigate to `/admin`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01c7e-319">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01c7e-319">Next Steps</span></span>
<span data-ttu-id="01c7e-320">Potřebujete-li další informace o rozhraní Django a nástrojích Python Tools pro Visual Studio, použijte tyto odkazy:</span><span class="sxs-lookup"><span data-stu-id="01c7e-320">Follow these links to learn more about Django and Python Tools for Visual Studio:</span></span>

* <span data-ttu-id="01c7e-321">[Dokumentace rozhraní Django]</span><span class="sxs-lookup"><span data-stu-id="01c7e-321">[Django Documentation]</span></span>
* <span data-ttu-id="01c7e-322">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="01c7e-322">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="01c7e-323">Informace týkající se použití databáze SQL Database a MySQL naleznete v tématech:</span><span class="sxs-lookup"><span data-stu-id="01c7e-323">For information on using SQL Database and MySQL:</span></span>

* <span data-ttu-id="01c7e-324">[Django a MySQL v Azure s nástroji Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="01c7e-324">[Django and MySQL on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="01c7e-325">[Django a SQL Database v Azure s nástroji Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="01c7e-325">[Django and SQL Database on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="01c7e-326">Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="01c7e-326">For more information, see the [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="01c7e-327">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="01c7e-327">What's changed</span></span>
* <span data-ttu-id="01c7e-328">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="01c7e-328">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Django a MySQL v Azure s nástroji Python Tools pro Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django a SQL Database v Azure s nástroji Python Tools pro Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL Database]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK pro Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK pro Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git pro Windows]: http://msysgit.github.io/
[GitHub pro Windows]: https://windows.github.com/
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentace rozhraní Django]: https://www.djangoproject.com/
