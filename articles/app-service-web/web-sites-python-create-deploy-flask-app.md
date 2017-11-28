---
title: "aaaCreating webové aplikace s Flask v Azure"
description: "Kurz vás seznámí s toorunning webové aplikace Python v Azure."
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
ms.openlocfilehash: 3362047b3106b4380b5971e47cbd8042d38a792b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a><span data-ttu-id="fa930-103">Vytvoření webové aplikace pomocí Flask v Azure</span><span class="sxs-lookup"><span data-stu-id="fa930-103">Creating web apps with Flask in Azure</span></span>
<span data-ttu-id="fa930-104">Tento kurz popisuje, jak tooget spuštění Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="fa930-104">This tutorial describes how tooget started running Python in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>  <span data-ttu-id="fa930-105">Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!</span><span class="sxs-lookup"><span data-stu-id="fa930-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span>  <span data-ttu-id="fa930-106">Jak vaše aplikace bude rozšiřovat, můžete přepnout toopaid hostování a můžete také integrovat se všemi hello jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="fa930-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="fa930-107">Vytvoříte aplikaci pomocí hello Flask webová architektura (viz alternativní verze tohoto kurzu pro [Django](web-sites-python-create-deploy-django-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="fa930-107">You will create an application using hello Flask web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span>  <span data-ttu-id="fa930-108">Vytvoření webu hello z hello Galerie Azure, nastavíte nasazení Git a klonovat úložiště hello místně.</span><span class="sxs-lookup"><span data-stu-id="fa930-108">You will create hello website from hello Azure gallery, set up Git deployment, and clone hello repository locally.</span></span>  <span data-ttu-id="fa930-109">Bude potom místní spuštění aplikace hello, proveďte změny, potvrzení a vložit je tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fa930-109">Then you will run hello application locally, make changes, commit and push them tooAzure.</span></span>  <span data-ttu-id="fa930-110">Hello kurzu se dozvíte, jak toodo to ze systému Windows nebo Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="fa930-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="fa930-111">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="fa930-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="fa930-112">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="fa930-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="fa930-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fa930-113">Prerequisites</span></span>
* <span data-ttu-id="fa930-114">Windows, Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="fa930-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="fa930-115">Python 2.7 nebo 3.4</span><span class="sxs-lookup"><span data-stu-id="fa930-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="fa930-116">setuptools, pip, virtualenv (pouze Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="fa930-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="fa930-117">Git</span><span class="sxs-lookup"><span data-stu-id="fa930-117">Git</span></span>
* <span data-ttu-id="fa930-118">[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="fa930-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="fa930-119">**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="fa930-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="fa930-120">Windows</span><span class="sxs-lookup"><span data-stu-id="fa930-120">Windows</span></span>
<span data-ttu-id="fa930-121">Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].</span><span class="sxs-lookup"><span data-stu-id="fa930-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span>  <span data-ttu-id="fa930-122">Tím se nainstaluje hello 32bitovou verzi jazyka Python, setuptools, pip, virtualenv atd (32bitová verze jazyka Python je nainstalovaných v hello hostitelských počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="fa930-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span>  <span data-ttu-id="fa930-123">Alternativně můžete získat jazyk Python z webu [python.org].</span><span class="sxs-lookup"><span data-stu-id="fa930-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="fa930-124">V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].</span><span class="sxs-lookup"><span data-stu-id="fa930-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span>  <span data-ttu-id="fa930-125">Pokud používáte Visual Studio, můžete použít integrované hello podporu Git.</span><span class="sxs-lookup"><span data-stu-id="fa930-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="fa930-126">Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="fa930-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span>  <span data-ttu-id="fa930-127">Tato položka je nepovinná, ale pokud máte [Visual Studio], včetně hello volné Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, pak tato položka vám poskytne skvělé rozhraní IDE Python.</span><span class="sxs-lookup"><span data-stu-id="fa930-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="fa930-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="fa930-128">Mac/Linux</span></span>
<span data-ttu-id="fa930-129">Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.</span><span class="sxs-lookup"><span data-stu-id="fa930-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-create-on-hello-azure-portal"></a><span data-ttu-id="fa930-130">Vytvořit webovou aplikaci na hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fa930-130">Web app create on hello Azure Portal</span></span>
<span data-ttu-id="fa930-131">Hello prvním krokem při vytváření aplikace je toocreate hello webové aplikace pomocí hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa930-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span> 

1. <span data-ttu-id="fa930-132">Přihlaste se k hello portálu Azure a klikněte na tlačítko hello **nový** tlačítko v levém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span> 
2. <span data-ttu-id="fa930-133">Klikněte na možnost **Web + mobilní zařízení**.</span><span class="sxs-lookup"><span data-stu-id="fa930-133">Click **Web + Mobile**.</span></span>
3. <span data-ttu-id="fa930-134">Hello vyhledávacího pole zadejte "python".</span><span class="sxs-lookup"><span data-stu-id="fa930-134">In hello search box, type "python".</span></span>
4. <span data-ttu-id="fa930-135">Ve výsledcích hledání hello, vyberte **Flask**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fa930-135">In hello search results, select **Flask**, then click **Create**.</span></span>
5. <span data-ttu-id="fa930-136">Nakonfigurujte hello nové Flask aplikace, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni.</span><span class="sxs-lookup"><span data-stu-id="fa930-136">Configure hello new Flask app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="fa930-137">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fa930-137">Then, click **Create**.</span></span>
6. <span data-ttu-id="fa930-138">Nakonfigurujte publikování Git pro nově vytvořenou webovou aplikaci pomocí následujících pokynů hello [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="fa930-138">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="fa930-139">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="fa930-139">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="fa930-140">Obsah úložiště Git</span><span class="sxs-lookup"><span data-stu-id="fa930-140">Git repository contents</span></span>
<span data-ttu-id="fa930-141">Zde je uveden přehled hello soubory, které se nachází ve hello počátečním úložišti Git, které jsme klonovat v další části hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-141">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

<span data-ttu-id="fa930-142">Hlavní zdroje pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-142">Main sources for hello application.</span></span>  <span data-ttu-id="fa930-143">Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.</span><span class="sxs-lookup"><span data-stu-id="fa930-143">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="fa930-144">Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.</span><span class="sxs-lookup"><span data-stu-id="fa930-144">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \runserver.py

<span data-ttu-id="fa930-145">Podpora serveru místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="fa930-145">Local development server support.</span></span> <span data-ttu-id="fa930-146">Pomocí této aplikace hello toorun místně.</span><span class="sxs-lookup"><span data-stu-id="fa930-146">Use this toorun hello application locally.</span></span>

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

<span data-ttu-id="fa930-147">Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="fa930-147">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="fa930-148">Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.</span><span class="sxs-lookup"><span data-stu-id="fa930-148">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="fa930-149">Externí balíčky vyžadované touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="fa930-149">External packages needed by this application.</span></span> <span data-ttu-id="fa930-150">skript nasazení Hello nástrojem pip instalaci hello balíčky uvedené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="fa930-150">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="fa930-151">Konfigurační soubory služby IIS.</span><span class="sxs-lookup"><span data-stu-id="fa930-151">IIS configuration files.</span></span>  <span data-ttu-id="fa930-152">Hello skript nasazení použije příslušný soubor web.x.y.config hello a zkopírujte ho jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="fa930-152">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="fa930-153">Volitelné soubory – přizpůsobení nasazení</span><span class="sxs-lookup"><span data-stu-id="fa930-153">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="fa930-154">Volitelné soubory – modul Python runtime</span><span class="sxs-lookup"><span data-stu-id="fa930-154">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="fa930-155">Další soubory na serveru</span><span class="sxs-lookup"><span data-stu-id="fa930-155">Additional files on server</span></span>
<span data-ttu-id="fa930-156">Některé soubory existují na serveru hello, ale nebyly přidány toohello úložiště git.</span><span class="sxs-lookup"><span data-stu-id="fa930-156">Some files exist on hello server but are not added toohello git repository.</span></span>  <span data-ttu-id="fa930-157">Tyto soubory jsou vytvořeny skriptem nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-157">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="fa930-158">Konfigurační soubor služby IIS.</span><span class="sxs-lookup"><span data-stu-id="fa930-158">IIS configuration file.</span></span>  <span data-ttu-id="fa930-159">Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.</span><span class="sxs-lookup"><span data-stu-id="fa930-159">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="fa930-160">Virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="fa930-160">Python virtual environment.</span></span>  <span data-ttu-id="fa930-161">Vytvoří se během nasazení, pokud ještě neexistuje kompatibilní virtuální prostředí v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-161">Created during deployment if a compatible virtual environment doesn't already exist on hello app.</span></span>  <span data-ttu-id="fa930-162">Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, ale pip instalaci přeskočí, pokud hello balíčky jsou už nainstalované.</span><span class="sxs-lookup"><span data-stu-id="fa930-162">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="fa930-163">Hello následující 3 části popisují, jak tooproceed s hello vývoj webových aplikací ve 3 různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="fa930-163">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="fa930-164">Windows s nástroji Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa930-164">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="fa930-165">Windows s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="fa930-165">Windows, with command line</span></span>
* <span data-ttu-id="fa930-166">Mac/Linux s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="fa930-166">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="fa930-167">Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa930-167">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="fa930-168">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="fa930-168">Clone hello repository</span></span>
<span data-ttu-id="fa930-169">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-169">First, clone hello repository using hello URL provided on hello Azure Portal.</span></span> <span data-ttu-id="fa930-170">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="fa930-170">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="fa930-171">Otevřete soubor řešení hello (.sln), který je součástí hello kořenovém hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa930-171">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="fa930-172">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="fa930-172">Create virtual environment</span></span>
<span data-ttu-id="fa930-173">Nyní vytvoříme virtuální prostředí pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="fa930-173">Now we'll create a virtual environment for local development.</span></span>  <span data-ttu-id="fa930-174">Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.</span><span class="sxs-lookup"><span data-stu-id="fa930-174">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="fa930-175">Zkontrolujte, zda je název hello hello prostředí `env`.</span><span class="sxs-lookup"><span data-stu-id="fa930-175">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="fa930-176">Vyberte základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-176">Select hello base interpreter.</span></span>  <span data-ttu-id="fa930-177">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello **nastavení aplikace** okně vaší webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="fa930-177">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="fa930-178">Zkontrolujte, že je zaškrtnuté hello možnost toodownload a nainstalovat balíčky.</span><span class="sxs-lookup"><span data-stu-id="fa930-178">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="fa930-179">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fa930-179">Click **Create**.</span></span>  <span data-ttu-id="fa930-180">Tato akce vytvoří hello virtuální prostředí a instalaci závislostí uvedených v souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="fa930-180">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="fa930-181">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="fa930-181">Run using development server</span></span>
<span data-ttu-id="fa930-182">Stisknutím klávesy F5 toostart ladění a webový prohlížeč se automaticky otevře s místně spuštěnou stránkou toohello.</span><span class="sxs-lookup"><span data-stu-id="fa930-182">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

<span data-ttu-id="fa930-183">Můžete nastavit zarážky v hello zdrojů, používání hello sledování systému windows, atd.  V tématu hello [Python Tools pro Visual Studio dokumentaci] Další informace o hello různých funkcí.</span><span class="sxs-lookup"><span data-stu-id="fa930-183">You can set breakpoints in hello sources, use hello watch windows, etc.  See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="fa930-184">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="fa930-184">Make changes</span></span>
<span data-ttu-id="fa930-185">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="fa930-185">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="fa930-186">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="fa930-186">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a><span data-ttu-id="fa930-187">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="fa930-187">Install more packages</span></span>
<span data-ttu-id="fa930-188">Aplikace může mít závislosti kromě Python a Flask.</span><span class="sxs-lookup"><span data-stu-id="fa930-188">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="fa930-189">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="fa930-189">You can install additional packages using pip.</span></span>  <span data-ttu-id="fa930-190">tooinstall balíčku, klikněte pravým tlačítkem myši na virtuální prostředí hello a vyberte **instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="fa930-190">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="fa930-191">Například tooinstall hello Azure SDK pro Python, která dává vám přístup tooAzure úložiště, služby service bus a dalším službám Azure, zadejte `azure`:</span><span class="sxs-lookup"><span data-stu-id="fa930-191">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

<span data-ttu-id="fa930-192">Klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **generovat soubor requirements.txt** tooupdate requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="fa930-192">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="fa930-193">Poté potvrďte úložiště Git toohello toorequirements.txt změny hello.</span><span class="sxs-lookup"><span data-stu-id="fa930-193">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="fa930-194">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="fa930-194">Deploy tooAzure</span></span>
<span data-ttu-id="fa930-195">tootrigger nasazení, klikněte na **synchronizace** nebo **Push**.</span><span class="sxs-lookup"><span data-stu-id="fa930-195">tootrigger a deployment, click on **Sync** or **Push**.</span></span>  <span data-ttu-id="fa930-196">Synchronizace provádí vložení změn (push) i přijetí změn (pull).</span><span class="sxs-lookup"><span data-stu-id="fa930-196">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

<span data-ttu-id="fa930-197">Hello první nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.</span><span class="sxs-lookup"><span data-stu-id="fa930-197">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="fa930-198">Sada Visual Studio nezobrazuje průběh hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="fa930-198">Visual Studio doesn't show hello progress of hello deployment.</span></span>  <span data-ttu-id="fa930-199">Pokud chcete výstup hello tooreview, najdete v tématu hello na [řešení potíží – nasazení](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="fa930-199">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="fa930-200">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="fa930-200">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="fa930-201">Vývoj webových aplikací – Windows – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="fa930-201">Web app development - Windows - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="fa930-202">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="fa930-202">Clone hello repository</span></span>
<span data-ttu-id="fa930-203">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="fa930-203">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="fa930-204">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="fa930-204">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="fa930-205">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="fa930-205">Create virtual environment</span></span>
<span data-ttu-id="fa930-206">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště).</span><span class="sxs-lookup"><span data-stu-id="fa930-206">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span>  <span data-ttu-id="fa930-207">Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.</span><span class="sxs-lookup"><span data-stu-id="fa930-207">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="fa930-208">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello **nastavení aplikace** okně vaší webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="fa930-208">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="fa930-209">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="fa930-209">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="fa930-210">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="fa930-210">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="fa930-211">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="fa930-211">Install any external packages required by your application.</span></span> <span data-ttu-id="fa930-212">Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="fa930-212">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="fa930-213">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="fa930-213">Run using development server</span></span>
<span data-ttu-id="fa930-214">Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fa930-214">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python runserver.py

<span data-ttu-id="fa930-215">Hello konzola zobrazí adresa URL hello a port hello server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="fa930-215">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

<span data-ttu-id="fa930-216">Potom otevřete adresu URL toothat webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fa930-216">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="fa930-217">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="fa930-217">Make changes</span></span>
<span data-ttu-id="fa930-218">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="fa930-218">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="fa930-219">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="fa930-219">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="fa930-220">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="fa930-220">Install more packages</span></span>
<span data-ttu-id="fa930-221">Aplikace může mít závislosti kromě Python a Flask.</span><span class="sxs-lookup"><span data-stu-id="fa930-221">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="fa930-222">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="fa930-222">You can install additional packages using pip.</span></span>  <span data-ttu-id="fa930-223">Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="fa930-223">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="fa930-224">Ujistěte se, že tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="fa930-224">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="fa930-225">Potvrzení změn hello:</span><span class="sxs-lookup"><span data-stu-id="fa930-225">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="fa930-226">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="fa930-226">Deploy tooAzure</span></span>
<span data-ttu-id="fa930-227">tootrigger nasazení nabízené hello změní tooAzure:</span><span class="sxs-lookup"><span data-stu-id="fa930-227">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="fa930-228">Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="fa930-228">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="fa930-229">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="fa930-229">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="fa930-230">Vývoj webových aplikací – Mac/Linux – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="fa930-230">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="fa930-231">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="fa930-231">Clone hello repository</span></span>
<span data-ttu-id="fa930-232">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="fa930-232">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="fa930-233">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="fa930-233">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="fa930-234">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="fa930-234">Create virtual environment</span></span>
<span data-ttu-id="fa930-235">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště).</span><span class="sxs-lookup"><span data-stu-id="fa930-235">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span>  <span data-ttu-id="fa930-236">Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.</span><span class="sxs-lookup"><span data-stu-id="fa930-236">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="fa930-237">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello **nastavení aplikace** okně vaší webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="fa930-237">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="fa930-238">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="fa930-238">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="fa930-239">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="fa930-239">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="fa930-240">nebo pyvenv env</span><span class="sxs-lookup"><span data-stu-id="fa930-240">or pyvenv env</span></span>

<span data-ttu-id="fa930-241">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="fa930-241">Install any external packages required by your application.</span></span> <span data-ttu-id="fa930-242">Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="fa930-242">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="fa930-243">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="fa930-243">Run using development server</span></span>
<span data-ttu-id="fa930-244">Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fa930-244">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python runserver.py

<span data-ttu-id="fa930-245">Hello konzola zobrazí adresa URL hello a port hello server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="fa930-245">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

<span data-ttu-id="fa930-246">Potom otevřete adresu URL toothat webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fa930-246">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="fa930-247">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="fa930-247">Make changes</span></span>
<span data-ttu-id="fa930-248">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="fa930-248">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="fa930-249">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="fa930-249">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="fa930-250">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="fa930-250">Install more packages</span></span>
<span data-ttu-id="fa930-251">Aplikace může mít závislosti kromě Python a Flask.</span><span class="sxs-lookup"><span data-stu-id="fa930-251">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="fa930-252">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="fa930-252">You can install additional packages using pip.</span></span>  <span data-ttu-id="fa930-253">Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="fa930-253">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="fa930-254">Ujistěte se, že tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="fa930-254">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="fa930-255">Potvrzení změn hello:</span><span class="sxs-lookup"><span data-stu-id="fa930-255">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="fa930-256">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="fa930-256">Deploy tooAzure</span></span>
<span data-ttu-id="fa930-257">tootrigger nasazení nabízené hello změní tooAzure:</span><span class="sxs-lookup"><span data-stu-id="fa930-257">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="fa930-258">Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="fa930-258">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="fa930-259">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="fa930-259">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="fa930-260">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="fa930-260">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="fa930-261">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="fa930-261">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="fa930-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa930-262">Next Steps</span></span>
<span data-ttu-id="fa930-263">Použijte tyto odkazy toolearn více informací o Flask a nástrojích Python Tools pro sadu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fa930-263">Follow these links toolearn more about Flask and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="fa930-264">[Dokumentace flask]</span><span class="sxs-lookup"><span data-stu-id="fa930-264">[Flask Documentation]</span></span>
* <span data-ttu-id="fa930-265">[Python Tools pro Visual Studio dokumentaci]</span><span class="sxs-lookup"><span data-stu-id="fa930-265">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="fa930-266">Informace o používání Azure Table Storage a MongoDB:</span><span class="sxs-lookup"><span data-stu-id="fa930-266">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="fa930-267">[Flask a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="fa930-267">[Flask and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="fa930-268">[Flask a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="fa930-268">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="fa930-269">Další informace najdete v tématu taky hello [středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="fa930-269">For more information, see also hello [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="fa930-270">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="fa930-270">What's changed</span></span>
* <span data-ttu-id="fa930-271">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="fa930-271">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Flask a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure
[Flask a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]: web-sites-python-ptvs-flask-table-storage.md

<!--External Link references-->
[Azure SDK pro Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK pro Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git pro Windows]: http://msysgit.github.io/
[GitHub pro Windows]: https://windows.github.com/
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools pro Visual Studio dokumentaci]: http://aka.ms/ptvsdocs
[Dokumentace flask]: http://flask.pocoo.org/ 

