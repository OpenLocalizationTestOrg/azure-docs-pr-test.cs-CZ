---
title: "aaaDjango a MySQL v Azure pomocí nástroje Python Tools 2.2 pro Visual Studio"
description: "Zjistěte, jak toouse hello Python Tools pro Visual Studio toocreate webové aplikace Django, která ukládá data do instance databáze MySQL a nasaďte ji tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: c60a50b5-8b5e-4818-a442-16362273dabb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1597c391d20c8e8ef629b4e4d05c9eb64c83bffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="2d4a2-103">Django a MySQL v Azure s nástroji Python Tools 2.2 pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d4a2-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="2d4a2-104">V tomto kurzu budete používat [Python Tools pro Visual Studio](https://www.visualstudio.com/vs/python) dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-104">In this tutorial, you'll use [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="2d4a2-105">Dozvíte se, jak toouse službu MySQL hostovanou v Azure, jak tooconfigure hello webové aplikace toouse MySQL a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="2d4a2-105">You'll learn how toouse a MySQL service hosted on Azure, how tooconfigure hello web app toouse MySQL, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

> [!NOTE]
> <span data-ttu-id="2d4a2-106">Hello informace obsažené v tomto kurzu jsou také k dispozici v hello následující video:</span><span class="sxs-lookup"><span data-stu-id="2d4a2-106">hello information contained in this tutorial is also available in hello following video:</span></span>
> 
> <span data-ttu-id="2d4a2-107">[PTVS 2.1: aplikace Django s MySQL][video]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-107">[PTVS 2.1: Django app with MySQL][video]</span></span>
> 
> 

<span data-ttu-id="2d4a2-108">V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami Azure Table Storage, MySQL a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL, and SQL Database services.</span></span> <span data-ttu-id="2d4a2-109">Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="2d4a2-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d4a2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2d4a2-110">Prerequisites</span></span>
* <span data-ttu-id="2d4a2-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="2d4a2-111">Visual Studio 2015</span></span>
* <span data-ttu-id="2d4a2-112">[Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-112">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>
* <span data-ttu-id="2d4a2-113">[Python Tools 2.2 pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-113">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="2d4a2-114">[Python Tools 2.2 pro Visual Studio – ukázky VSIX]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="2d4a2-115">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-115">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="2d4a2-116">Django 1.9 nebo novější</span><span class="sxs-lookup"><span data-stu-id="2d4a2-116">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of hello hello previous include. -->

> [!NOTE]
> <span data-ttu-id="2d4a2-117">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-117">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2d4a2-118">Není požadována platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-118">No credit card is required, and no commitments are necessary.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="2d4a2-119">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="2d4a2-119">Create hello Project</span></span>
<span data-ttu-id="2d4a2-120">V této části vytvoříte projekt sady Visual Studio pomocí vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-120">In this section, you'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="2d4a2-121">Vytvoříte virtuální prostředí a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-121">You'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="2d4a2-122">Vytvoříte místní databázi pomocí SQLite.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-122">You'll create a local database using sqlite.</span></span> <span data-ttu-id="2d4a2-123">Poté místně spustíte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-123">Then you'll run hello application locally.</span></span>

1. <span data-ttu-id="2d4a2-124">V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-124">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="2d4a2-125">šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-125">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="2d4a2-126">Vyberte **hlasovací webový projekt Django** a klikněte na tlačítko OK toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-126">Select **Polls Django Web Project** and click OK toocreate hello project.</span></span>
   
    ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. <span data-ttu-id="2d4a2-128">Bude výzvami tooinstall externí balíčky.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-128">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="2d4a2-129">Vyberte možnost **Instalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-129">Select **Install into a virtual environment**.</span></span>
   
    ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="2d4a2-131">Vyberte **Python 2.7** nebo **Python 3.4** jako základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-131">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
    ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="2d4a2-133">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-133">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="2d4a2-134">Pak vyberte možnost **Vytvořit superživatele Django**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-134">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="2d4a2-135">Tím se otevřete Konzola pro správu Django a ve složce projektu hello se vytvoří databáze sqlite.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-135">This will open a Django Management Console and create a sqlite database in hello project folder.</span></span> <span data-ttu-id="2d4a2-136">Postupujte podle výzvy toocreate hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-136">Follow hello prompts toocreate a user.</span></span>
7. <span data-ttu-id="2d4a2-137">Potvrďte, že hello aplikace funguje tak, že stisknete `F5`.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-137">Confirm that hello application works by pressing `F5`.</span></span>
8. <span data-ttu-id="2d4a2-138">Klikněte na tlačítko **přihlásit** z hello navigačního panelu v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-138">Click **Log in** from hello navigation bar at hello top.</span></span>
   
    ![Navigační panel Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="2d4a2-140">Zadejte přihlašovací údaje hello hello uživatele, kterého jste vytvořili při synchronizaci databáze hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-140">Enter hello credentials for hello user you created when you synchronized hello database.</span></span>
   
    ![Přihlašovací formulář](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="2d4a2-142">Klikněte na možnost **Vytvořit ukázková hlasování**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-142">Click **Create Sample Polls**.</span></span>
    
     ![Vytvoření ukázkových hlasování](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="2d4a2-144">Klikněte na hlasování a hlasujte.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-144">Click on a poll and vote.</span></span>
    
     ![Hlasování v ukázkových hlasováních](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a><span data-ttu-id="2d4a2-146">Vytvoření databáze MySQL Database</span><span class="sxs-lookup"><span data-stu-id="2d4a2-146">Create a MySQL Database</span></span>
<span data-ttu-id="2d4a2-147">Pro databázi hello vytvoříte databázi ClearDB MySQL hostovanou v Azure.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-147">For hello database, you'll create a ClearDB MySQL hosted database on Azure.</span></span>

<span data-ttu-id="2d4a2-148">Alternativně můžete vytvořit svůj vlastní virtuální počítač spuštěný v Azure a poté sami nainstalovat a spravovat MySQL.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-148">As an alternative, you can create your own Virtual Machine running in Azure, then install and administer MySQL yourself.</span></span>

<span data-ttu-id="2d4a2-149">Následujícím postupem můžete vytvořit databázi s bezplatným plánem.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-149">You can create a database with a free plan by following these steps.</span></span>

1. <span data-ttu-id="2d4a2-150">Přihlaste se toohello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="2d4a2-150">Log in toohello [Azure Portal].</span></span>
2. <span data-ttu-id="2d4a2-151">V horní části podokna navigace hello hello, klikněte na položku **nový**, pak klikněte na tlačítko **Data + úložiště**a potom klikněte na **databáze MySQL**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-151">At hello Top of hello navigation pane, click **NEW**, then click **Data + Storage**, and then click **MySQL Database**.</span></span>
3. <span data-ttu-id="2d4a2-152">Nakonfigurujte novou databázi MySQL hello tak, že vytvoříte novou skupinu prostředků a vyberte pro ni vhodné umístění hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-152">Configure hello new MySQL database by creating a new resource group and select hello appropriate location for it.</span></span>
4. <span data-ttu-id="2d4a2-153">Po vytvoření databáze MySQL hello klikněte na tlačítko **vlastnosti** v okně databáze hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-153">Once hello MySQL database is created, click **Properties** in hello database blade.</span></span>
5. <span data-ttu-id="2d4a2-154">Hello kopie tlačítko tooput hello hodnotu **PŘIPOJOVACÍ řetězec** hello schránky.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-154">Use hello copy button tooput hello value of **CONNECTION STRING** on hello clipboard.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="2d4a2-155">Konfigurace hello projektu</span><span class="sxs-lookup"><span data-stu-id="2d4a2-155">Configure hello Project</span></span>
<span data-ttu-id="2d4a2-156">V této části nakonfigurujete naše webové aplikace toouse hello databáze MySQL, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-156">In this section, you'll configure our web app toouse hello MySQL database you just created.</span></span> <span data-ttu-id="2d4a2-157">Nainstalujete také další databází MySQL požadované toouse balíčků Python s rozhraním Django.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-157">You'll also install additional Python packages required toouse MySQL databases with Django.</span></span> <span data-ttu-id="2d4a2-158">Poté místně spustíte hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-158">Then you'll run hello web app locally.</span></span>

1. <span data-ttu-id="2d4a2-159">V sadě Visual Studio otevřete **settings.py**, z hello *ProjectName* složky.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-159">In Visual Studio, open **settings.py**, from hello *ProjectName* folder.</span></span> <span data-ttu-id="2d4a2-160">Dočasně vložte připojovací řetězec hello v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-160">Temporarily paste hello connection string in hello editor.</span></span> <span data-ttu-id="2d4a2-161">Hello připojovací řetězec je v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="2d4a2-161">hello connection string is in this format:</span></span>
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    <span data-ttu-id="2d4a2-162">Změna hello výchozí databázi **modul** toouse MySQL a nastavte hodnoty pro hello **název**, **uživatele**, **heslo** a  **HOSTITELE** z hello **CONNECTIONSTRING**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-162">Change hello default database **ENGINE** toouse MySQL, and set hello values for **NAME**, **USER**, **PASSWORD** and **HOST** from hello **CONNECTIONSTRING**.</span></span>
   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }
2. <span data-ttu-id="2d4a2-163">V Průzkumníku řešení klikněte v části **prostředí Python**, klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-163">In Solution Explorer, under **Python Environments**, right-click on hello virtual environment and select **Install Python Package**.</span></span>
3. <span data-ttu-id="2d4a2-164">Instalovat balíček hello `mysqlclient` pomocí **pip**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-164">Install hello package `mysqlclient` using **pip**.</span></span>
   
    ![Dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. <span data-ttu-id="2d4a2-166">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-166">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="2d4a2-167">Pak vyberte možnost **Vytvořit superživatele Django**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-167">Then select **Django Create Superuser**.</span></span>
   
    <span data-ttu-id="2d4a2-168">Tím se vytvoří hello tabulky pro databázi MySQL hello, kterou jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-168">This will create hello tables for hello MySQL database you created in hello previous section.</span></span> <span data-ttu-id="2d4a2-169">Postupujte podle toocreate hello vyzve uživatele, který nemá toomatch hello uživatele v databázi sqlite hello vytvořené v první části tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-169">Follow hello prompts toocreate a user, which doesn't have toomatch hello user in hello sqlite database created in hello first section of this article.</span></span>
5. <span data-ttu-id="2d4a2-170">Spuštění aplikace hello s `F5`.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-170">Run hello application with `F5`.</span></span> <span data-ttu-id="2d4a2-171">Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v databázi MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-171">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in hello MySQL database.</span></span>

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="2d4a2-172">Publikování hello webové aplikace tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="2d4a2-172">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="2d4a2-173">Hello .NET SDK služby Azure poskytuje snadno toodeploy tooAzure vaší webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-173">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="2d4a2-174">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-174">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
    ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="2d4a2-176">Klikněte na **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-176">Click on **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="2d4a2-177">Klikněte na **nový** toocreate novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-177">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="2d4a2-178">Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="2d4a2-178">Fill in hello following fields and click **Create**:</span></span>
   
   * <span data-ttu-id="2d4a2-179">**Název webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="2d4a2-179">**Web App name**</span></span>
   * <span data-ttu-id="2d4a2-180">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="2d4a2-180">**App Service plan**</span></span>
   * <span data-ttu-id="2d4a2-181">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="2d4a2-181">**Resource group**</span></span>
   * <span data-ttu-id="2d4a2-182">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="2d4a2-182">**Region**</span></span>
   * <span data-ttu-id="2d4a2-183">Nechte **databázový server** nastavit příliš**žádná databáze.**</span><span class="sxs-lookup"><span data-stu-id="2d4a2-183">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="2d4a2-184">Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-184">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="2d4a2-185">Webový prohlížeč se automaticky otevře toohello publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-185">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="2d4a2-186">Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **MySQL** databáze, které jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-186">You should see hello web app working as expected, using hello **MySQL** database hosted on Azure.</span></span>
   
    ![Webový prohlížeč](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    <span data-ttu-id="2d4a2-188">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2d4a2-188">Congratulations!</span></span> <span data-ttu-id="2d4a2-189">Úspěšně jste publikovali vaší tooAzure založenou na MySQL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-189">You have successfully published your MySQL-based web app tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d4a2-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d4a2-190">Next steps</span></span>
<span data-ttu-id="2d4a2-191">Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, rozhraní Django a MySQL.</span><span class="sxs-lookup"><span data-stu-id="2d4a2-191">Follow these links toolearn more about Python Tools for Visual Studio, Django and MySQL.</span></span>

* <span data-ttu-id="2d4a2-192">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-192">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="2d4a2-193">[Webové projekty]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-193">[Web Projects]</span></span>
  * <span data-ttu-id="2d4a2-194">[Projekty cloudových služeb]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-194">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="2d4a2-195">[Vzdálené ladění v Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-195">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="2d4a2-196">[Dokumentace rozhraní Django]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-196">[Django Documentation]</span></span>
* <span data-ttu-id="2d4a2-197">[MySQL]</span><span class="sxs-lookup"><span data-stu-id="2d4a2-197">[MySQL]</span></span>

<span data-ttu-id="2d4a2-198">Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="2d4a2-198">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

<!--Link references-->

[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[portálu Azure]: https://portal.azure.com
[Python Tools for Visual Studio]: https://www.visualstudio.com/vs/python/
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 pro Visual Studio – ukázky VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Vzdálené ladění v Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webové projekty]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Dokumentace rozhraní Django]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo
