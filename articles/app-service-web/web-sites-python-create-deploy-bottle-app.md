---
title: "aaaPython webové aplikace s Bottle v Azure"
description: "Kurz vás seznámí s toorunning webové aplikace Python v Azure App Service Web Apps."
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
ms.openlocfilehash: 98acd7d8fcdbba326625121c20f9237d2663ea1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-bottle-in-azure"></a><span data-ttu-id="c9ea3-103">Vytvoření webové aplikace pomocí Bottle v Azure</span><span class="sxs-lookup"><span data-stu-id="c9ea3-103">Creating web apps with Bottle in Azure</span></span>
<span data-ttu-id="c9ea3-104">Tento kurz popisuje, jak tooget spuštění Python v Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-104">This tutorial describes how tooget started running Python in Azure App Service Web Apps.</span></span> <span data-ttu-id="c9ea3-105">Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!</span><span class="sxs-lookup"><span data-stu-id="c9ea3-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="c9ea3-106">Jak vaše aplikace bude rozšiřovat, můžete přepnout toopaid hostování a můžete také integrovat se všemi hello jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="c9ea3-107">Vytvoříte webovou aplikaci pomocí hello Bottle webová architektura (viz alternativní verze tohoto kurzu pro [Django](web-sites-python-create-deploy-django-app.md) a [Flask](web-sites-python-create-deploy-flask-app.md)).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-107">You will create a web app using hello Bottle web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Flask](web-sites-python-create-deploy-flask-app.md)).</span></span> <span data-ttu-id="c9ea3-108">Vytvoření webové aplikace hello hello Azure Marketplace, nastavíte nasazení Git a klonovat úložiště hello místně.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-108">You will create hello web app from hello Azure Marketplace, set up Git deployment, and clone hello repository locally.</span></span> <span data-ttu-id="c9ea3-109">Pak bude spuštěn místně hello webové aplikace, měnit, potvrzení a vložit je příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-109">Then you will run hello web app locally, make changes, commit and push them too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="c9ea3-110">Hello kurzu se dozvíte, jak toodo to ze systému Windows nebo Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="c9ea3-111">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c9ea3-112">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c9ea3-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9ea3-113">Prerequisites</span></span>
* <span data-ttu-id="c9ea3-114">Windows, Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="c9ea3-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="c9ea3-115">Python 2.7 nebo 3.4</span><span class="sxs-lookup"><span data-stu-id="c9ea3-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="c9ea3-116">setuptools, pip, virtualenv (pouze Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="c9ea3-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="c9ea3-117">Git</span><span class="sxs-lookup"><span data-stu-id="c9ea3-117">Git</span></span>
* <span data-ttu-id="c9ea3-118">[Python Tools 2.2 pro Visual Studio][Python Tools 2.2 pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná</span><span class="sxs-lookup"><span data-stu-id="c9ea3-118">[Python Tools 2.2 for Visual Studio][Python Tools 2.2 for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="c9ea3-119">**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="c9ea3-120">Windows</span><span class="sxs-lookup"><span data-stu-id="c9ea3-120">Windows</span></span>
<span data-ttu-id="c9ea3-121">Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].</span><span class="sxs-lookup"><span data-stu-id="c9ea3-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="c9ea3-122">Tím se nainstaluje hello 32bitovou verzi jazyka Python, setuptools, pip, virtualenv atd (32bitová verze jazyka Python je nainstalovaných v hello hostitelských počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span> <span data-ttu-id="c9ea3-123">Alternativně můžete získat jazyk Python z webu [python.org].</span><span class="sxs-lookup"><span data-stu-id="c9ea3-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="c9ea3-124">V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].</span><span class="sxs-lookup"><span data-stu-id="c9ea3-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="c9ea3-125">Pokud používáte Visual Studio, můžete použít integrované hello podporu Git.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="c9ea3-126">Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="c9ea3-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="c9ea3-127">Tato položka je nepovinná, ale pokud máte [Visual Studio], včetně hello volné Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, pak tato položka vám poskytne skvělé rozhraní IDE Python.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="c9ea3-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="c9ea3-128">Mac/Linux</span></span>
<span data-ttu-id="c9ea3-129">Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-hello-azure-portal"></a><span data-ttu-id="c9ea3-130">Vytvoření webové aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c9ea3-130">Web app creation on hello Azure Portal</span></span>
<span data-ttu-id="c9ea3-131">Hello prvním krokem při vytváření aplikace je toocreate hello webové aplikace pomocí hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span>  

1. <span data-ttu-id="c9ea3-132">Přihlaste se k hello portálu Azure a klikněte na tlačítko hello **nový** tlačítko v levém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span> 
2. <span data-ttu-id="c9ea3-133">Hello vyhledávacího pole zadejte "python".</span><span class="sxs-lookup"><span data-stu-id="c9ea3-133">In hello search box, type "python".</span></span>
3. <span data-ttu-id="c9ea3-134">Ve výsledcích hledání hello, vyberte **Bottle**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-134">In hello search results, select **Bottle**, then click **Create**.</span></span>
4. <span data-ttu-id="c9ea3-135">Nakonfigurujte hello nové Bottle aplikace, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-135">Configure hello new Bottle app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="c9ea3-136">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="c9ea3-137">Nakonfigurujte publikování Git pro nově vytvořenou webovou aplikaci pomocí následujících pokynů hello [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-137">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="c9ea3-138">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="c9ea3-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="c9ea3-139">Obsah úložiště Git</span><span class="sxs-lookup"><span data-stu-id="c9ea3-139">Git repository contents</span></span>
<span data-ttu-id="c9ea3-140">Zde je uveden přehled hello soubory, které se nachází ve hello počátečním úložišti Git, které jsme klonovat v další části hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-140">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

<span data-ttu-id="c9ea3-141">Hlavní zdroje pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-141">Main sources for hello application.</span></span> <span data-ttu-id="c9ea3-142">Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="c9ea3-143">Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \app.py

<span data-ttu-id="c9ea3-144">Podpora serveru místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-144">Local development server support.</span></span> <span data-ttu-id="c9ea3-145">Pomocí této aplikace hello toorun místně.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-145">Use this toorun hello application locally.</span></span>

    \BottleWebProject.pyproj
    \BottleWebProject.sln

<span data-ttu-id="c9ea3-146">Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="c9ea3-146">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="c9ea3-147">Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-147">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="c9ea3-148">Externí balíčky vyžadované touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-148">External packages needed by this application.</span></span> <span data-ttu-id="c9ea3-149">skript nasazení Hello nástrojem pip instalaci hello balíčky uvedené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-149">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="c9ea3-150">Konfigurační soubory služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-150">IIS configuration files.</span></span> <span data-ttu-id="c9ea3-151">Hello skript nasazení použije příslušný soubor web.x.y.config hello a zkopírujte ho jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-151">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="c9ea3-152">Volitelné soubory – přizpůsobení nasazení</span><span class="sxs-lookup"><span data-stu-id="c9ea3-152">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="c9ea3-153">Volitelné soubory – modul Python runtime</span><span class="sxs-lookup"><span data-stu-id="c9ea3-153">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="c9ea3-154">Další soubory na serveru</span><span class="sxs-lookup"><span data-stu-id="c9ea3-154">Additional files on server</span></span>
<span data-ttu-id="c9ea3-155">Některé soubory existují na serveru hello, ale nebyly přidány toohello úložiště git.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-155">Some files exist on hello server but are not added toohello git repository.</span></span> <span data-ttu-id="c9ea3-156">Tyto soubory jsou vytvořeny skriptem nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-156">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="c9ea3-157">Konfigurační soubor služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-157">IIS configuration file.</span></span> <span data-ttu-id="c9ea3-158">Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-158">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="c9ea3-159">Virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-159">Python virtual environment.</span></span> <span data-ttu-id="c9ea3-160">Vytvoří se během nasazení, pokud ještě neexistuje kompatibilní virtuální prostředí na hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-160">Created during deployment if a compatible virtual environment doesn't already exist on hello web app.</span></span>  <span data-ttu-id="c9ea3-161">Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, ale pip instalaci přeskočí, pokud hello balíčky jsou už nainstalované.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-161">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="c9ea3-162">Hello následující 3 části popisují, jak tooproceed s hello vývoj webových aplikací ve 3 různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-162">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="c9ea3-163">Windows s nástroji Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ea3-163">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="c9ea3-164">Windows s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="c9ea3-164">Windows, with command line</span></span>
* <span data-ttu-id="c9ea3-165">Mac/Linux s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="c9ea3-165">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="c9ea3-166">Vývoj webových aplikací – Windows – Python Tools pro sadu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9ea3-166">Web App development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="c9ea3-167">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="c9ea3-167">Clone hello repository</span></span>
<span data-ttu-id="c9ea3-168">Nejprve naklonujte úložiště hello pomocí hello adresy url poskytnuté na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-168">First, clone hello repository using hello url provided on hello Azure Portal.</span></span> <span data-ttu-id="c9ea3-169">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-169">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="c9ea3-170">Otevřete soubor řešení hello (.sln), který je součástí hello kořenovém hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-170">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="c9ea3-171">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="c9ea3-171">Create virtual environment</span></span>
<span data-ttu-id="c9ea3-172">Nyní vytvoříme virtuální prostředí pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-172">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="c9ea3-173">Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-173">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="c9ea3-174">Zkontrolujte, zda je název hello hello prostředí `env`.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-174">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="c9ea3-175">Vyberte základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-175">Select hello base interpreter.</span></span> <span data-ttu-id="c9ea3-176">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello **nastavení aplikace** okně vaší webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-176">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="c9ea3-177">Zkontrolujte, že je zaškrtnuté hello možnost toodownload a nainstalovat balíčky.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-177">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="c9ea3-178">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-178">Click **Create**.</span></span> <span data-ttu-id="c9ea3-179">Tato akce vytvoří hello virtuální prostředí a instalaci závislostí uvedených v souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-179">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="c9ea3-180">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="c9ea3-180">Run using development server</span></span>
<span data-ttu-id="c9ea3-181">Stisknutím klávesy F5 toostart ladění a webový prohlížeč se automaticky otevře s místně spuštěnou stránkou toohello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-181">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

<span data-ttu-id="c9ea3-182">Můžete nastavit zarážky v hello zdrojů, používání hello sledování systému windows, atd. V tématu hello [Python Tools pro Visual Studio dokumentaci] Další informace o hello různých funkcí.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-182">You can set breakpoints in hello sources, use hello watch windows, etc. See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="c9ea3-183">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="c9ea3-183">Make changes</span></span>
<span data-ttu-id="c9ea3-184">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-184">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="c9ea3-185">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-185">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a><span data-ttu-id="c9ea3-186">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="c9ea3-186">Install more packages</span></span>
<span data-ttu-id="c9ea3-187">Aplikace může mít kromě jazyka Python a Bottle závislosti.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-187">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="c9ea3-188">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-188">You can install additional packages using pip.</span></span> <span data-ttu-id="c9ea3-189">tooinstall balíčku, klikněte pravým tlačítkem myši na virtuální prostředí hello a vyberte **instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-189">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="c9ea3-190">Například tooinstall hello Azure SDK pro Python, která dává vám přístup tooAzure úložiště, služby service bus a dalším službám Azure, zadejte `azure`:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-190">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

<span data-ttu-id="c9ea3-191">Klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **generovat soubor requirements.txt** tooupdate requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-191">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="c9ea3-192">Poté potvrďte úložiště Git toohello toorequirements.txt změny hello.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-192">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="c9ea3-193">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="c9ea3-193">Deploy tooAzure</span></span>
<span data-ttu-id="c9ea3-194">tootrigger nasazení, klikněte na **synchronizace** nebo **Push**.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-194">tootrigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="c9ea3-195">Synchronizace provádí vložení změn (push) i přijetí změn (pull).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-195">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

<span data-ttu-id="c9ea3-196">Hello první nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-196">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="c9ea3-197">Sada Visual Studio nezobrazuje průběh hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-197">Visual Studio doesn't show hello progress of hello deployment.</span></span> <span data-ttu-id="c9ea3-198">Pokud chcete výstup hello tooreview, najdete v tématu hello na [řešení potíží – nasazení](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-198">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="c9ea3-199">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-199">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="c9ea3-200">Vývoj webových aplikací – Windows – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="c9ea3-200">Web app development - Windows - Command Line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="c9ea3-201">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="c9ea3-201">Clone hello repository</span></span>
<span data-ttu-id="c9ea3-202">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-202">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="c9ea3-203">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-203">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="c9ea3-204">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="c9ea3-204">Create virtual environment</span></span>
<span data-ttu-id="c9ea3-205">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-205">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="c9ea3-206">Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-206">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="c9ea3-207">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello okně Nastavení aplikace pro vaši webovou aplikaci v hello portál Azure)</span><span class="sxs-lookup"><span data-stu-id="c9ea3-207">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade for your web app in hello Azure Portal)</span></span>

<span data-ttu-id="c9ea3-208">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-208">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="c9ea3-209">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-209">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="c9ea3-210">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-210">Install any external packages required by your application.</span></span> <span data-ttu-id="c9ea3-211">Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-211">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="c9ea3-212">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="c9ea3-212">Run using development server</span></span>
<span data-ttu-id="c9ea3-213">Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-213">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python app.py

<span data-ttu-id="c9ea3-214">Hello konzola zobrazí adresa URL hello a port hello server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-214">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

<span data-ttu-id="c9ea3-215">Potom otevřete adresu URL toothat webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-215">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="c9ea3-216">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="c9ea3-216">Make changes</span></span>
<span data-ttu-id="c9ea3-217">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-217">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="c9ea3-218">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-218">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="c9ea3-219">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="c9ea3-219">Install more packages</span></span>
<span data-ttu-id="c9ea3-220">Aplikace může mít kromě jazyka Python a Bottle závislosti.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-220">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="c9ea3-221">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-221">You can install additional packages using pip.</span></span> <span data-ttu-id="c9ea3-222">Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-222">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="c9ea3-223">Ujistěte se, že tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-223">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="c9ea3-224">Potvrzení změn hello:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-224">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="c9ea3-225">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="c9ea3-225">Deploy tooAzure</span></span>
<span data-ttu-id="c9ea3-226">tootrigger nasazení nabízené hello změní tooAzure:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-226">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="c9ea3-227">Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-227">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="c9ea3-228">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-228">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="c9ea3-229">Vývoj webových aplikací – Mac/Linux – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="c9ea3-229">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="c9ea3-230">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="c9ea3-230">Clone hello repository</span></span>
<span data-ttu-id="c9ea3-231">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-231">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="c9ea3-232">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-232">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="c9ea3-233">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="c9ea3-233">Create virtual environment</span></span>
<span data-ttu-id="c9ea3-234">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-234">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="c9ea3-235">Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-235">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="c9ea3-236">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello okně Nastavení aplikace webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="c9ea3-236">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="c9ea3-237">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-237">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="c9ea3-238">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-238">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="c9ea3-239">nebo pyvenv env</span><span class="sxs-lookup"><span data-stu-id="c9ea3-239">or pyvenv env</span></span>

<span data-ttu-id="c9ea3-240">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-240">Install any external packages required by your application.</span></span> <span data-ttu-id="c9ea3-241">Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-241">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="c9ea3-242">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="c9ea3-242">Run using development server</span></span>
<span data-ttu-id="c9ea3-243">Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-243">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python app.py

<span data-ttu-id="c9ea3-244">Hello konzola zobrazí adresa URL hello a port hello server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-244">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

<span data-ttu-id="c9ea3-245">Potom otevřete adresu URL toothat webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-245">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="c9ea3-246">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="c9ea3-246">Make changes</span></span>
<span data-ttu-id="c9ea3-247">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-247">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="c9ea3-248">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-248">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="c9ea3-249">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="c9ea3-249">Install more packages</span></span>
<span data-ttu-id="c9ea3-250">Aplikace může mít kromě jazyka Python a Bottle závislosti.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-250">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="c9ea3-251">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-251">You can install additional packages using pip.</span></span> <span data-ttu-id="c9ea3-252">Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-252">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="c9ea3-253">Ujistěte se, že tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-253">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="c9ea3-254">Potvrzení změn hello:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-254">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="c9ea3-255">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="c9ea3-255">Deploy tooAzure</span></span>
<span data-ttu-id="c9ea3-256">tootrigger nasazení nabízené hello změní tooAzure:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-256">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="c9ea3-257">Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-257">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="c9ea3-258">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="c9ea3-258">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="c9ea3-259">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="c9ea3-259">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="c9ea3-260">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="c9ea3-260">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="c9ea3-261">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9ea3-261">Next Steps</span></span>
<span data-ttu-id="c9ea3-262">Použijte tyto odkazy toolearn více informací o Bottle a nástrojích Python Tools pro sadu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-262">Follow these links toolearn more about Bottle and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="c9ea3-263">[Bottle dokumentace]</span><span class="sxs-lookup"><span data-stu-id="c9ea3-263">[Bottle Documentation]</span></span>
* <span data-ttu-id="c9ea3-264">[Python Tools pro Visual Studio dokumentaci]</span><span class="sxs-lookup"><span data-stu-id="c9ea3-264">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="c9ea3-265">Informace o používání Azure Table Storage a MongoDB:</span><span class="sxs-lookup"><span data-stu-id="c9ea3-265">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="c9ea3-266">[Bottle a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="c9ea3-266">[Bottle and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="c9ea3-267">[Bottle a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="c9ea3-267">[Bottle and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="c9ea3-268">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="c9ea3-268">What's changed</span></span>
* <span data-ttu-id="c9ea3-269">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="c9ea3-269">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Bottle a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md
[Bottle a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md

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
[Bottle dokumentace]: http://bottlepy.org/docs/dev/index.html

