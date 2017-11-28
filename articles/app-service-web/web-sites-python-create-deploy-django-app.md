---
title: "aaaCreating webové aplikace pomocí rozhraní Django v Azure"
description: "Kurz vás seznámí s toorunning webové aplikace Python v Azure App Service Web Apps."
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
ms.openlocfilehash: 26a131da358748bd6fe4ee5c114d0a8f91b83cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-django-in-azure"></a><span data-ttu-id="90b96-103">Vytvoření webové aplikace pomocí rozhraní Django v Azure</span><span class="sxs-lookup"><span data-stu-id="90b96-103">Creating web apps with Django in Azure</span></span>
<span data-ttu-id="90b96-104">Tento kurz popisuje, jak tooget spuštění Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="90b96-104">This tutorial describes how tooget started running Python on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="90b96-105">Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!</span><span class="sxs-lookup"><span data-stu-id="90b96-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="90b96-106">Jak vaše aplikace bude rozšiřovat, můžete přepnout toopaid hostování a můžete také integrovat se všemi hello jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="90b96-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="90b96-107">Vytvoříte aplikaci pomocí hello Django webová architektura (viz alternativní verze tohoto kurzu pro [Flask](web-sites-python-create-deploy-flask-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="90b96-107">You will create an application using hello Django web framework (see alternate versions of this tutorial for [Flask](web-sites-python-create-deploy-flask-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span> <span data-ttu-id="90b96-108">Vytvoření webové aplikace hello hello Azure Marketplace, nastavíte nasazení Git a klonovat úložiště hello místně.</span><span class="sxs-lookup"><span data-stu-id="90b96-108">You will create hello web app from hello Azure Marketplace, set up Git deployment, and clone hello repository locally.</span></span> <span data-ttu-id="90b96-109">Bude potom místní spuštění aplikace hello, proveďte změny, potvrzení a vložit je tooAzure.</span><span class="sxs-lookup"><span data-stu-id="90b96-109">Then you will run hello application locally, make changes, commit and push them tooAzure.</span></span> <span data-ttu-id="90b96-110">Hello kurzu se dozvíte, jak toodo to ze systému Windows nebo Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="90b96-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="90b96-111">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="90b96-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="90b96-112">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="90b96-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="90b96-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90b96-113">Prerequisites</span></span>
* <span data-ttu-id="90b96-114">Windows, Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="90b96-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="90b96-115">Python 2.7 nebo 3.4</span><span class="sxs-lookup"><span data-stu-id="90b96-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="90b96-116">setuptools, pip, virtualenv (pouze Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="90b96-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="90b96-117">Git</span><span class="sxs-lookup"><span data-stu-id="90b96-117">Git</span></span>
* <span data-ttu-id="90b96-118">[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="90b96-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="90b96-119">**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="90b96-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="90b96-120">Windows</span><span class="sxs-lookup"><span data-stu-id="90b96-120">Windows</span></span>
<span data-ttu-id="90b96-121">Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].</span><span class="sxs-lookup"><span data-stu-id="90b96-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="90b96-122">Tím se nainstaluje hello 32bitovou verzi jazyka Python, setuptools, pip, virtualenv atd (32bitová verze jazyka Python je nainstalovaných v hello hostitelských počítačích Azure).</span><span class="sxs-lookup"><span data-stu-id="90b96-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span> <span data-ttu-id="90b96-123">Alternativně můžete získat jazyk Python z webu [python.org].</span><span class="sxs-lookup"><span data-stu-id="90b96-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="90b96-124">V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].</span><span class="sxs-lookup"><span data-stu-id="90b96-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="90b96-125">Pokud používáte Visual Studio, můžete použít integrované hello podporu Git.</span><span class="sxs-lookup"><span data-stu-id="90b96-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="90b96-126">Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="90b96-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="90b96-127">Tato položka je nepovinná, ale pokud máte [Visual Studio], včetně hello volné Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, pak tato položka vám poskytne skvělé rozhraní IDE Python.</span><span class="sxs-lookup"><span data-stu-id="90b96-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="90b96-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="90b96-128">Mac/Linux</span></span>
<span data-ttu-id="90b96-129">Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.</span><span class="sxs-lookup"><span data-stu-id="90b96-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-portal"></a><span data-ttu-id="90b96-130">Vytvoření webové aplikace v portálu</span><span class="sxs-lookup"><span data-stu-id="90b96-130">Web App Creation on Portal</span></span>
<span data-ttu-id="90b96-131">Hello prvním krokem při vytváření aplikace je toocreate hello webové aplikace pomocí hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90b96-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="90b96-132">Přihlaste se k hello portálu Azure a klikněte na tlačítko hello **nový** tlačítko v levém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span>
2. <span data-ttu-id="90b96-133">Hello vyhledávacího pole zadejte "python".</span><span class="sxs-lookup"><span data-stu-id="90b96-133">In hello search box, type "python".</span></span>
3. <span data-ttu-id="90b96-134">Ve výsledcích hledání hello, vyberte **Django** (publikováno PTVS), pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90b96-134">In hello search results, select **Django** (published by PTVS), then click **Create**.</span></span>
4. <span data-ttu-id="90b96-135">Nakonfigurujte hello novou aplikaci Django, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni.</span><span class="sxs-lookup"><span data-stu-id="90b96-135">Configure hello new Django app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="90b96-136">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90b96-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="90b96-137">Nakonfigurujte publikování Git pro nově vytvořenou webovou aplikaci pomocí následujících pokynů hello [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="90b96-137">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="90b96-138">Přehled aplikace</span><span class="sxs-lookup"><span data-stu-id="90b96-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="90b96-139">Obsah úložiště Git</span><span class="sxs-lookup"><span data-stu-id="90b96-139">Git repository contents</span></span>
<span data-ttu-id="90b96-140">Zde je uveden přehled hello soubory, které se nachází ve hello počátečním úložišti Git, které jsme klonovat v další části hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-140">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

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

<span data-ttu-id="90b96-141">Hlavní zdroje pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-141">Main sources for hello application.</span></span> <span data-ttu-id="90b96-142">Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.</span><span class="sxs-lookup"><span data-stu-id="90b96-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span> <span data-ttu-id="90b96-143">Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.</span><span class="sxs-lookup"><span data-stu-id="90b96-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \manage.py

<span data-ttu-id="90b96-144">Serverová podpora místní správy a vývoje.</span><span class="sxs-lookup"><span data-stu-id="90b96-144">Local management and development server support.</span></span> <span data-ttu-id="90b96-145">Pomocí této aplikace hello toorun místně, synchronizaci databáze hello atd.</span><span class="sxs-lookup"><span data-stu-id="90b96-145">Use this toorun hello application locally, synchronize hello database, etc.</span></span>

    \db.sqlite3

<span data-ttu-id="90b96-146">Výchozí databáze.</span><span class="sxs-lookup"><span data-stu-id="90b96-146">Default database.</span></span> <span data-ttu-id="90b96-147">Zahrnuje hello nezbytné tabulky pro toorun aplikace hello, ale neobsahuje žádné uživatele (synchronizovat hello databáze toocreate uživatele).</span><span class="sxs-lookup"><span data-stu-id="90b96-147">Includes hello necessary tables for hello application toorun, but does not contain any users (synchronize hello database toocreate a user).</span></span>

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

<span data-ttu-id="90b96-148">Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="90b96-148">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="90b96-149">Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.</span><span class="sxs-lookup"><span data-stu-id="90b96-149">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="90b96-150">Externí balíčky vyžadované touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="90b96-150">External packages needed by this application.</span></span> <span data-ttu-id="90b96-151">skript nasazení Hello nástrojem pip instalaci hello balíčky uvedené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="90b96-151">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="90b96-152">Konfigurační soubory služby IIS.</span><span class="sxs-lookup"><span data-stu-id="90b96-152">IIS configuration files.</span></span> <span data-ttu-id="90b96-153">Hello skript nasazení použije příslušný soubor web.x.y.config hello a zkopírujte ho jako soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="90b96-153">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="90b96-154">Volitelné soubory – přizpůsobení nasazení</span><span class="sxs-lookup"><span data-stu-id="90b96-154">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="90b96-155">Volitelné soubory – modul Python runtime</span><span class="sxs-lookup"><span data-stu-id="90b96-155">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="90b96-156">Další soubory na serveru</span><span class="sxs-lookup"><span data-stu-id="90b96-156">Additional files on server</span></span>
<span data-ttu-id="90b96-157">Některé soubory existují na serveru hello, ale nebyly přidány toohello úložiště git.</span><span class="sxs-lookup"><span data-stu-id="90b96-157">Some files exist on hello server but are not added toohello git repository.</span></span> <span data-ttu-id="90b96-158">Tyto soubory jsou vytvořeny skriptem nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-158">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="90b96-159">Konfigurační soubor služby IIS.</span><span class="sxs-lookup"><span data-stu-id="90b96-159">IIS configuration file.</span></span> <span data-ttu-id="90b96-160">Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.</span><span class="sxs-lookup"><span data-stu-id="90b96-160">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="90b96-161">Virtuální prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="90b96-161">Python virtual environment.</span></span> <span data-ttu-id="90b96-162">Vytvoří se během nasazení, pokud ještě neexistuje kompatibilní virtuální prostředí na hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="90b96-162">Created during deployment if a compatible virtual environment doesn't already exist on hello web app.</span></span> <span data-ttu-id="90b96-163">Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, ale pip instalaci přeskočí, pokud hello balíčky jsou už nainstalované.</span><span class="sxs-lookup"><span data-stu-id="90b96-163">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="90b96-164">Hello následující 3 části popisují, jak tooproceed s hello vývoj webových aplikací ve 3 různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="90b96-164">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="90b96-165">Windows s nástroji Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90b96-165">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="90b96-166">Windows s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="90b96-166">Windows, with command line</span></span>
* <span data-ttu-id="90b96-167">Mac/Linux s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="90b96-167">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="90b96-168">Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90b96-168">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="90b96-169">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="90b96-169">Clone hello repository</span></span>
<span data-ttu-id="90b96-170">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-170">First, clone hello repository using hello URL provided on hello Azure Portal.</span></span> <span data-ttu-id="90b96-171">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="90b96-171">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="90b96-172">Otevřete soubor řešení hello (.sln), který je součástí hello kořenovém hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="90b96-172">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="90b96-173">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="90b96-173">Create virtual environment</span></span>
<span data-ttu-id="90b96-174">Nyní vytvoříme virtuální prostředí pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="90b96-174">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="90b96-175">Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.</span><span class="sxs-lookup"><span data-stu-id="90b96-175">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="90b96-176">Zkontrolujte, zda je název hello hello prostředí `env`.</span><span class="sxs-lookup"><span data-stu-id="90b96-176">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="90b96-177">Vyberte základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-177">Select hello base interpreter.</span></span> <span data-ttu-id="90b96-178">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello **nastavení aplikace** okně vaší webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="90b96-178">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="90b96-179">Zkontrolujte, že je zaškrtnuté hello možnost toodownload a nainstalovat balíčky.</span><span class="sxs-lookup"><span data-stu-id="90b96-179">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="90b96-180">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90b96-180">Click **Create**.</span></span> <span data-ttu-id="90b96-181">Tato akce vytvoří hello virtuální prostředí a instalaci závislostí uvedených v souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="90b96-181">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="create-a-superuser"></a><span data-ttu-id="90b96-182">Vytvoření superuživatele</span><span class="sxs-lookup"><span data-stu-id="90b96-182">Create a superuser</span></span>
<span data-ttu-id="90b96-183">součástí aplikace hello databáze Hello nemá definován žádný superuživatel.</span><span class="sxs-lookup"><span data-stu-id="90b96-183">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="90b96-184">V pořadí toouse hello funkce přihlašování v aplikaci hello nebo rozhraní správce Django hello (Pokud se rozhodnete tooenable ho), budete potřebovat toocreate superuživatele.</span><span class="sxs-lookup"><span data-stu-id="90b96-184">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="90b96-185">Spusťte z hello příkazového řádku ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="90b96-185">Run this from hello command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="90b96-186">Postupujte podle hello výzvy tooset hello uživatelské jméno, heslo atd.</span><span class="sxs-lookup"><span data-stu-id="90b96-186">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="90b96-187">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="90b96-187">Run using development server</span></span>
<span data-ttu-id="90b96-188">Stisknutím klávesy F5 toostart ladění a webový prohlížeč se automaticky otevře s místně spuštěnou stránkou toohello.</span><span class="sxs-lookup"><span data-stu-id="90b96-188">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

<span data-ttu-id="90b96-189">Můžete nastavit zarážky v hello zdrojů, používání hello sledování systému windows, atd. V tématu hello [Python Tools pro Visual Studio dokumentaci] Další informace o hello různých funkcí.</span><span class="sxs-lookup"><span data-stu-id="90b96-189">You can set breakpoints in hello sources, use hello watch windows, etc. See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="90b96-190">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="90b96-190">Make changes</span></span>
<span data-ttu-id="90b96-191">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="90b96-191">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="90b96-192">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="90b96-192">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a><span data-ttu-id="90b96-193">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="90b96-193">Install more packages</span></span>
<span data-ttu-id="90b96-194">Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.</span><span class="sxs-lookup"><span data-stu-id="90b96-194">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="90b96-195">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="90b96-195">You can install additional packages using pip.</span></span> <span data-ttu-id="90b96-196">tooinstall balíčku, klikněte pravým tlačítkem myši na virtuální prostředí hello a vyberte **instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="90b96-196">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="90b96-197">Například tooinstall hello Azure SDK pro Python, která dává vám přístup tooAzure úložiště, služby service bus a dalším službám Azure, zadejte `azure`:</span><span class="sxs-lookup"><span data-stu-id="90b96-197">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

<span data-ttu-id="90b96-198">Klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **generovat soubor requirements.txt** tooupdate requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="90b96-198">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="90b96-199">Poté potvrďte úložiště Git toohello toorequirements.txt změny hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-199">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="90b96-200">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="90b96-200">Deploy tooAzure</span></span>
<span data-ttu-id="90b96-201">tootrigger nasazení, klikněte na **synchronizace** nebo **Push**.</span><span class="sxs-lookup"><span data-stu-id="90b96-201">tootrigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="90b96-202">Synchronizace provádí vložení změn (push) i přijetí změn (pull).</span><span class="sxs-lookup"><span data-stu-id="90b96-202">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

<span data-ttu-id="90b96-203">Hello první nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.</span><span class="sxs-lookup"><span data-stu-id="90b96-203">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="90b96-204">Sada Visual Studio nezobrazuje průběh hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="90b96-204">Visual Studio doesn't show hello progress of hello deployment.</span></span> <span data-ttu-id="90b96-205">Pokud chcete výstup hello tooreview, najdete v tématu hello na [řešení potíží – nasazení](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="90b96-205">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="90b96-206">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="90b96-206">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="90b96-207">Vývoj webových aplikací – Windows – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="90b96-207">Web app development - Windows - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="90b96-208">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="90b96-208">Clone hello repository</span></span>
<span data-ttu-id="90b96-209">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="90b96-209">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="90b96-210">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="90b96-210">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="90b96-211">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="90b96-211">Create virtual environment</span></span>
<span data-ttu-id="90b96-212">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště).</span><span class="sxs-lookup"><span data-stu-id="90b96-212">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="90b96-213">Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.</span><span class="sxs-lookup"><span data-stu-id="90b96-213">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="90b96-214">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello okně Nastavení aplikace webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="90b96-214">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="90b96-215">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="90b96-215">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="90b96-216">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="90b96-216">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="90b96-217">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="90b96-217">Install any external packages required by your application.</span></span> <span data-ttu-id="90b96-218">Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="90b96-218">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="90b96-219">Vytvoření superuživatele</span><span class="sxs-lookup"><span data-stu-id="90b96-219">Create a superuser</span></span>
<span data-ttu-id="90b96-220">součástí aplikace hello databáze Hello nemá definován žádný superuživatel.</span><span class="sxs-lookup"><span data-stu-id="90b96-220">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="90b96-221">V pořadí toouse hello funkce přihlašování v aplikaci hello nebo rozhraní správce Django hello (Pokud se rozhodnete tooenable ho), budete potřebovat toocreate superuživatele.</span><span class="sxs-lookup"><span data-stu-id="90b96-221">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="90b96-222">Spusťte z hello příkazového řádku ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="90b96-222">Run this from hello command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="90b96-223">Postupujte podle hello výzvy tooset hello uživatelské jméno, heslo atd.</span><span class="sxs-lookup"><span data-stu-id="90b96-223">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="90b96-224">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="90b96-224">Run using development server</span></span>
<span data-ttu-id="90b96-225">Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="90b96-225">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python manage.py runserver

<span data-ttu-id="90b96-226">Hello konzola zobrazí adresa URL hello a port hello server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="90b96-226">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

<span data-ttu-id="90b96-227">Potom otevřete adresu URL toothat webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="90b96-227">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="90b96-228">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="90b96-228">Make changes</span></span>
<span data-ttu-id="90b96-229">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="90b96-229">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="90b96-230">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="90b96-230">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="90b96-231">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="90b96-231">Install more packages</span></span>
<span data-ttu-id="90b96-232">Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.</span><span class="sxs-lookup"><span data-stu-id="90b96-232">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="90b96-233">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="90b96-233">You can install additional packages using pip.</span></span> <span data-ttu-id="90b96-234">Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="90b96-234">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="90b96-235">Ujistěte se, že tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="90b96-235">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="90b96-236">Potvrzení změn hello:</span><span class="sxs-lookup"><span data-stu-id="90b96-236">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="90b96-237">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="90b96-237">Deploy tooAzure</span></span>
<span data-ttu-id="90b96-238">tootrigger nasazení nabízené hello změní tooAzure:</span><span class="sxs-lookup"><span data-stu-id="90b96-238">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="90b96-239">Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="90b96-239">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="90b96-240">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="90b96-240">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="90b96-241">Vývoj webových aplikací – Mac/Linux – příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="90b96-241">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="90b96-242">Klon hello úložiště</span><span class="sxs-lookup"><span data-stu-id="90b96-242">Clone hello repository</span></span>
<span data-ttu-id="90b96-243">Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="90b96-243">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="90b96-244">Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="90b96-244">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="90b96-245">Vytvoření virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="90b96-245">Create virtual environment</span></span>
<span data-ttu-id="90b96-246">Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště).</span><span class="sxs-lookup"><span data-stu-id="90b96-246">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="90b96-247">Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.</span><span class="sxs-lookup"><span data-stu-id="90b96-247">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="90b96-248">Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello okně Nastavení aplikace webové aplikace v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="90b96-248">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="90b96-249">Pro jazyk Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="90b96-249">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="90b96-250">Pro jazyk Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="90b96-250">For Python 3.4:</span></span>

    python -m venv env

<span data-ttu-id="90b96-251">nebo</span><span class="sxs-lookup"><span data-stu-id="90b96-251">or</span></span>

    pyvenv env

<span data-ttu-id="90b96-252">Nainstalujte veškeré případné externí balíčky požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="90b96-252">Install any external packages required by your application.</span></span> <span data-ttu-id="90b96-253">Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:</span><span class="sxs-lookup"><span data-stu-id="90b96-253">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="90b96-254">Vytvoření superuživatele</span><span class="sxs-lookup"><span data-stu-id="90b96-254">Create a superuser</span></span>
<span data-ttu-id="90b96-255">součástí aplikace hello databáze Hello nemá definován žádný superuživatel.</span><span class="sxs-lookup"><span data-stu-id="90b96-255">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="90b96-256">V pořadí toouse hello funkce přihlašování v aplikaci hello nebo rozhraní správce Django hello (Pokud se rozhodnete tooenable ho), budete potřebovat toocreate superuživatele.</span><span class="sxs-lookup"><span data-stu-id="90b96-256">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="90b96-257">Spusťte z hello příkazového řádku ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="90b96-257">Run this from hello command-line from your project folder:</span></span>

    env/bin/python manage.py createsuperuser

<span data-ttu-id="90b96-258">Postupujte podle hello výzvy tooset hello uživatelské jméno, heslo atd.</span><span class="sxs-lookup"><span data-stu-id="90b96-258">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="90b96-259">Spuštění pomocí vývojového serveru</span><span class="sxs-lookup"><span data-stu-id="90b96-259">Run using development server</span></span>
<span data-ttu-id="90b96-260">Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="90b96-260">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python manage.py runserver

<span data-ttu-id="90b96-261">Hello konzola zobrazí adresa URL hello a port hello server naslouchá:</span><span class="sxs-lookup"><span data-stu-id="90b96-261">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

<span data-ttu-id="90b96-262">Potom otevřete adresu URL toothat webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="90b96-262">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="90b96-263">Provedení změn</span><span class="sxs-lookup"><span data-stu-id="90b96-263">Make changes</span></span>
<span data-ttu-id="90b96-264">Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="90b96-264">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="90b96-265">Jakmile změny otestujete, potvrďte je toohello úložiště Git:</span><span class="sxs-lookup"><span data-stu-id="90b96-265">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="90b96-266">Instalace dalších balíčků</span><span class="sxs-lookup"><span data-stu-id="90b96-266">Install more packages</span></span>
<span data-ttu-id="90b96-267">Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.</span><span class="sxs-lookup"><span data-stu-id="90b96-267">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="90b96-268">Další balíčky můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="90b96-268">You can install additional packages using pip.</span></span> <span data-ttu-id="90b96-269">Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:</span><span class="sxs-lookup"><span data-stu-id="90b96-269">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="90b96-270">Ujistěte se, že tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="90b96-270">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="90b96-271">Potvrzení změn hello:</span><span class="sxs-lookup"><span data-stu-id="90b96-271">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="90b96-272">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="90b96-272">Deploy tooAzure</span></span>
<span data-ttu-id="90b96-273">tootrigger nasazení nabízené hello změní tooAzure:</span><span class="sxs-lookup"><span data-stu-id="90b96-273">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="90b96-274">Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="90b96-274">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="90b96-275">Procházejte toohello adresy URL Azure tooview změny.</span><span class="sxs-lookup"><span data-stu-id="90b96-275">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="90b96-276">Řešení potíží – instalace balíčku</span><span class="sxs-lookup"><span data-stu-id="90b96-276">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="90b96-277">Řešení potíží – virtuální prostředí</span><span class="sxs-lookup"><span data-stu-id="90b96-277">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a><span data-ttu-id="90b96-278">Řešení potíží – statické soubory</span><span class="sxs-lookup"><span data-stu-id="90b96-278">Troubleshooting - Static Files</span></span>
<span data-ttu-id="90b96-279">Rozhraní Django obsahuje koncepci shromažďování statických souborů hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-279">Django has hello concept of collecting static files.</span></span> <span data-ttu-id="90b96-280">To trvá všechny hello statické soubory z původního umístění a zkopíruje je tooa jedna složka.</span><span class="sxs-lookup"><span data-stu-id="90b96-280">This takes all hello static files from their original location and copies them tooa single folder.</span></span> <span data-ttu-id="90b96-281">Pro tuto aplikaci se kopírují příliš`/static`.</span><span class="sxs-lookup"><span data-stu-id="90b96-281">For this application, they are copied too`/static`.</span></span>

<span data-ttu-id="90b96-282">Tato akce se provádí proto, že statické soubory mohou pocházet z různých „aplikací“ Django.</span><span class="sxs-lookup"><span data-stu-id="90b96-282">This is done because static files may come from different Django 'apps'.</span></span> <span data-ttu-id="90b96-283">Například hello statické soubory z rozhraní správce Django hello jsou umístěny v podsložce knihovny Django ve virtuálním prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-283">For example, hello static files from hello Django admin interfaces are located in a Django library subfolder in hello virtual environment.</span></span> <span data-ttu-id="90b96-284">Statické soubory definované touto aplikací jsou umístěny ve složce `/app/static`.</span><span class="sxs-lookup"><span data-stu-id="90b96-284">Static files defined by this application are located in `/app/static`.</span></span> <span data-ttu-id="90b96-285">Při používání vícero „aplikací“ Django se statické soubory nacházejí na více místech.</span><span class="sxs-lookup"><span data-stu-id="90b96-285">As you use more Django 'apps', you'll have static files located in multiple places.</span></span>

<span data-ttu-id="90b96-286">Při spuštění aplikace hello v režimu ladění, slouží hello aplikace hello statické soubory z původního umístění.</span><span class="sxs-lookup"><span data-stu-id="90b96-286">When running hello application in debug mode, hello application serves hello static files from their original location.</span></span>

<span data-ttu-id="90b96-287">Při spuštění v režimu vydání aplikace hello, nemá aplikace hello **není** obsluhovat statické soubory hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-287">When running hello application in release mode, hello application does **not** serve hello static files.</span></span> <span data-ttu-id="90b96-288">Je zodpovědností hello hello webový server tooserve hello soubory.</span><span class="sxs-lookup"><span data-stu-id="90b96-288">It is hello responsibility of hello web server tooserve hello files.</span></span> <span data-ttu-id="90b96-289">Pro tuto aplikaci, služba IIS bude poskytovat hello statické soubory z `/static`.</span><span class="sxs-lookup"><span data-stu-id="90b96-289">For this application, IIS will serve hello static files from `/static`.</span></span>

<span data-ttu-id="90b96-290">Hello shromažďování statických souborů je prováděno automaticky v rámci hello skriptu nasazení, přičemž dříve shromážděné soubory.</span><span class="sxs-lookup"><span data-stu-id="90b96-290">hello collection of static files is done automatically as part of hello deployment script, clearing previously collected files.</span></span> <span data-ttu-id="90b96-291">To znamená hello shromažďování probíhá při každém nasazení zpomalení nasazení trochu, ale zajišťuje, že zastaralé soubory nebudete mít k dispozici, zabraňující potenciálním potížím se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="90b96-291">This means hello collection occurs on every deployment, slowing down deployment a bit, but it ensures that obsolete files won't be available, avoiding a potential security issue.</span></span>

<span data-ttu-id="90b96-292">Pokud chcete u aplikace rozhraní Django tooskip shromažďování statických souborů:</span><span class="sxs-lookup"><span data-stu-id="90b96-292">If you want tooskip collection of static files for your Django application:</span></span>

    \.skipDjango

<span data-ttu-id="90b96-293">Potom budete potřebovat toodo hello kolekce ručně na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="90b96-293">Then you'll need toodo hello collection manually on your local machine:</span></span>

    env\scripts\python manage.py collectstatic

<span data-ttu-id="90b96-294">Pak odeberte hello `\static` složky z `.gitignore` a přidejte ji toohello úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="90b96-294">Then remove hello `\static` folder from `.gitignore` and add it toohello Git repository.</span></span>

## <a name="troubleshooting---settings"></a><span data-ttu-id="90b96-295">Řešení potíží – nastavení</span><span class="sxs-lookup"><span data-stu-id="90b96-295">Troubleshooting - Settings</span></span>
<span data-ttu-id="90b96-296">Různá nastavení pro aplikace hello lze změnit v `DjangoWebProject/settings.py`.</span><span class="sxs-lookup"><span data-stu-id="90b96-296">Various settings for hello application can be changed in `DjangoWebProject/settings.py`.</span></span>

<span data-ttu-id="90b96-297">Z důvodu usnadnění práce vývojářů je povolen režim ladění.</span><span class="sxs-lookup"><span data-stu-id="90b96-297">For developer convenience, debug mode is enabled.</span></span> <span data-ttu-id="90b96-298">Jeden dobrý vedlejším účinkem této je, že budete mít toosee obrázky a další statický obsah při spuštění místně, bez nutnosti toocollect statické soubory.</span><span class="sxs-lookup"><span data-stu-id="90b96-298">One nice side effect of that is you'll be able toosee images and other static content when running locally, without having toocollect static files.</span></span>

<span data-ttu-id="90b96-299">režim ladění toodisable:</span><span class="sxs-lookup"><span data-stu-id="90b96-299">toodisable debug mode:</span></span>

    DEBUG = False

<span data-ttu-id="90b96-300">Pokud je zakázáno ladění, hello hodnotu `ALLOWED_HOSTS` toobe potřeby aktualizovat název hostitele Azure tooinclude hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-300">When debug is disabled, hello value for `ALLOWED_HOSTS` needs toobe updated tooinclude hello Azure host name.</span></span> <span data-ttu-id="90b96-301">Například:</span><span class="sxs-lookup"><span data-stu-id="90b96-301">For example:</span></span>

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

<span data-ttu-id="90b96-302">nebo tooenable žádné:</span><span class="sxs-lookup"><span data-stu-id="90b96-302">or tooenable any:</span></span>

    ALLOWED_HOSTS = (
        '*',
    )

<span data-ttu-id="90b96-303">V praxi může být vhodné toodo něco složitější toodeal s přepínání mezi ladění a vydání režimu a získávání názvu hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-303">In practice, you may want toodo something more complex toodeal with switching between debug and release mode, and getting hello host name.</span></span>

<span data-ttu-id="90b96-304">Můžete nastavit proměnné prostředí prostřednictvím portálu Azure hello **konfigurace** stránku hello **nastavení aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="90b96-304">You can set environment variables through hello Azure portal **CONFIGURE** page, in hello **app settings** section.</span></span>  <span data-ttu-id="90b96-305">To může být užitečné pro nastavení hodnot, nemusí chcete tooappear v hello zdrojích (připojovací řetězce, hesla atd.), nebo zda chcete tooset jinak mezi Azure a v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="90b96-305">This can be useful for setting values that you may not want tooappear in hello sources (connection strings, passwords, etc), or that you want tooset differently between Azure and your local machine.</span></span> <span data-ttu-id="90b96-306">V `settings.py`, se můžete dotazovat pomocí proměnné prostředí hello `os.getenv`.</span><span class="sxs-lookup"><span data-stu-id="90b96-306">In `settings.py`, you can query hello environment variables using `os.getenv`.</span></span>

## <a name="using-a-database"></a><span data-ttu-id="90b96-307">Používání databáze</span><span class="sxs-lookup"><span data-stu-id="90b96-307">Using a Database</span></span>
<span data-ttu-id="90b96-308">Hello databáze, která je součástí aplikace hello je databáze sqlite.</span><span class="sxs-lookup"><span data-stu-id="90b96-308">hello database that is included with hello application is a sqlite database.</span></span> <span data-ttu-id="90b96-309">Toto je toouse praktickou a užitečnou výchozí databázi pro vývoj, protože nevyžaduje téměř žádné nastavení.</span><span class="sxs-lookup"><span data-stu-id="90b96-309">This is a convenient and useful default database toouse for development, as it requires almost no setup.</span></span> <span data-ttu-id="90b96-310">Hello databáze je uložena v souboru db.sqlite3 hello ve složce projektu hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-310">hello database is stored in hello db.sqlite3 file in hello project folder.</span></span>

<span data-ttu-id="90b96-311">Azure poskytuje databázové služby, které jsou snadno toouse z aplikace Django.</span><span class="sxs-lookup"><span data-stu-id="90b96-311">Azure provides database services which are easy toouse from a Django application.</span></span> <span data-ttu-id="90b96-312">Kurzy zaměřené na používání [SQL Database] a [MySQL] z aplikace Django zobrazení hello kroky nezbytné toocreate hello databáze služby, změnit nastavení databáze hello v `DjangoWebProject/settings.py`a hello požadované tooinstall knihovny.</span><span class="sxs-lookup"><span data-stu-id="90b96-312">Tutorials for using [SQL Database] and [MySQL] from a Django application show hello steps necessary toocreate hello database service, change hello database settings in `DjangoWebProject/settings.py`, and hello libraries required tooinstall.</span></span>

<span data-ttu-id="90b96-313">Samozřejmě pokud dáváte přednost toomanage své vlastní databázové servery, můžete provést tak pomocí systému Windows nebo Linux virtuální počítače běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="90b96-313">Of course, if you prefer toomanage your own database servers, you can do so using Windows or Linux virtual machines running on Azure.</span></span>

## <a name="django-admin-interface"></a><span data-ttu-id="90b96-314">Rozhraní správce Django</span><span class="sxs-lookup"><span data-stu-id="90b96-314">Django Admin Interface</span></span>
<span data-ttu-id="90b96-315">Jakmile začnete vytvářet modely, budete muset toopopulate hello databáze s některá data.</span><span class="sxs-lookup"><span data-stu-id="90b96-315">Once you start building your models, you'll want toopopulate hello database with some data.</span></span> <span data-ttu-id="90b96-316">Snadný způsob toodo přidávat a upravovat obsah interaktivně je rozhraní pro správu Django toouse hello.</span><span class="sxs-lookup"><span data-stu-id="90b96-316">An easy way toodo add and edit content interactively is toouse hello Django administration interface.</span></span>

<span data-ttu-id="90b96-317">Hello kód pro rozhraní správce hello je označeno jako komentář ve zdrojích aplikace hello, ale je přehledně označen tak je možné snadno povolit (vyhledejte "admin").</span><span class="sxs-lookup"><span data-stu-id="90b96-317">hello code for hello admin interface is commented out in hello application sources, but it's clearly marked so you can easily enable it (search for 'admin').</span></span>

<span data-ttu-id="90b96-318">Když je tato funkce povolená, synchronizovat hello databáze, spusťte aplikaci hello a přejděte příliš`/admin`.</span><span class="sxs-lookup"><span data-stu-id="90b96-318">After it's enabled, synchronize hello database, run hello application and navigate too`/admin`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90b96-319">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90b96-319">Next Steps</span></span>
<span data-ttu-id="90b96-320">Použijte tyto odkazy toolearn Další informace o rozhraní Django a nástrojích Python Tools pro sadu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="90b96-320">Follow these links toolearn more about Django and Python Tools for Visual Studio:</span></span>

* <span data-ttu-id="90b96-321">[Dokumentace rozhraní Django]</span><span class="sxs-lookup"><span data-stu-id="90b96-321">[Django Documentation]</span></span>
* <span data-ttu-id="90b96-322">[Python Tools pro Visual Studio dokumentaci]</span><span class="sxs-lookup"><span data-stu-id="90b96-322">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="90b96-323">Informace týkající se použití databáze SQL Database a MySQL naleznete v tématech:</span><span class="sxs-lookup"><span data-stu-id="90b96-323">For information on using SQL Database and MySQL:</span></span>

* <span data-ttu-id="90b96-324">[Django a MySQL v Azure s nástroji Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="90b96-324">[Django and MySQL on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="90b96-325">[Django a SQL Database v Azure s nástroji Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="90b96-325">[Django and SQL Database on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="90b96-326">Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="90b96-326">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="90b96-327">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="90b96-327">What's changed</span></span>
* <span data-ttu-id="90b96-328">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="90b96-328">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
[Python Tools pro Visual Studio dokumentaci]: http://aka.ms/ptvsdocs
[Dokumentace rozhraní Django]: https://www.djangoproject.com/
