---
title: "Django a MySQL v Azure s nástroji Python Tools 2.2 pro Visual Studio"
description: "Naučte se používat nástroje Python Tools pro Visual Studio k vytvoření webové aplikace Django, která ukládá data do instance databáze MySQL, a k nasazení této aplikace do webové aplikace služby Azure App Service Web Apps."
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
ms.openlocfilehash: fd85337ecdc638a4c18065a0ce94f697da8197f1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="44671-103">Django a MySQL v Azure s nástroji Python Tools 2.2 pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44671-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="44671-104">V tomto kurzu budete používat nástroje [Python Tools pro Visual Studio](https://www.visualstudio.com/vs/python) k vytvoření jednoduché hlasovací webové aplikace pomocí jedné z ukázkových šablon PTVS.</span><span class="sxs-lookup"><span data-stu-id="44671-104">In this tutorial, you'll use [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="44671-105">Naučíte se používat službu MySQL hostovanou v Azure, konfigurovat webovou aplikaci k používání MySQL a publikovat webovou aplikaci do služby [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="44671-105">You'll learn how to use a MySQL service hosted on Azure, how to configure the web app to use MySQL, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

> [!NOTE]
> <span data-ttu-id="44671-106">Informace obsažené v tomto kurzu jsou také k dispozici v následujícím videu:</span><span class="sxs-lookup"><span data-stu-id="44671-106">The information contained in this tutorial is also available in the following video:</span></span>
> 
> <span data-ttu-id="44671-107">[PTVS 2.1: aplikace Django s MySQL][video]</span><span class="sxs-lookup"><span data-stu-id="44671-107">[PTVS 2.1: Django app with MySQL][video]</span></span>
> 
> 

<span data-ttu-id="44671-108">Ve [Středisku pro vývojáře Python] naleznete další články týkající se vývoje služby Azure App Service Web Apps s nástroji PTVS pomocí webového rozhraní Bottle, Flask a Django, se službami Azure Table Storage, MySQL a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="44671-108">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL, and SQL Database services.</span></span> <span data-ttu-id="44671-109">Ačkoli se tento článek zaměřuje na službu App Service, postup při vývoji služeb [Azure Cloud Services] je obdobný.</span><span class="sxs-lookup"><span data-stu-id="44671-109">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44671-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44671-110">Prerequisites</span></span>
* <span data-ttu-id="44671-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="44671-111">Visual Studio 2015</span></span>
* <span data-ttu-id="44671-112">[Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]</span><span class="sxs-lookup"><span data-stu-id="44671-112">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>
* <span data-ttu-id="44671-113">[Python Tools 2.2 pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="44671-113">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="44671-114">[Python Tools 2.2 pro Visual Studio – ukázky VSIX]</span><span class="sxs-lookup"><span data-stu-id="44671-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="44671-115">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="44671-115">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="44671-116">Django 1.9 nebo novější</span><span class="sxs-lookup"><span data-stu-id="44671-116">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [!NOTE]
> <span data-ttu-id="44671-117">Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="44671-117">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="44671-118">Není požadována platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="44671-118">No credit card is required, and no commitments are necessary.</span></span>
> 
> 

## <a name="create-the-project"></a><span data-ttu-id="44671-119">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="44671-119">Create the Project</span></span>
<span data-ttu-id="44671-120">V této části vytvoříte projekt sady Visual Studio pomocí vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="44671-120">In this section, you'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="44671-121">Vytvoříte virtuální prostředí a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="44671-121">You'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="44671-122">Vytvoříte místní databázi pomocí SQLite.</span><span class="sxs-lookup"><span data-stu-id="44671-122">You'll create a local database using sqlite.</span></span> <span data-ttu-id="44671-123">Poté aplikaci místně spustíte.</span><span class="sxs-lookup"><span data-stu-id="44671-123">Then you'll run the application locally.</span></span>

1. <span data-ttu-id="44671-124">V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="44671-124">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="44671-125">Šablony projektů z [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou dostupné v části **Python**, **Ukázky**.</span><span class="sxs-lookup"><span data-stu-id="44671-125">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="44671-126">Vyberte položku **Hlasovací webový projekt Django** a kliknutím na tlačítko OK vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="44671-126">Select **Polls Django Web Project** and click OK to create the project.</span></span>
   
    ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. <span data-ttu-id="44671-128">Zobrazí se výzva k instalaci externích balíčků.</span><span class="sxs-lookup"><span data-stu-id="44671-128">You will be prompted to install external packages.</span></span> <span data-ttu-id="44671-129">Vyberte možnost **Instalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="44671-129">Select **Install into a virtual environment**.</span></span>
   
    ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="44671-131">Jako základní překladač vyberte jazyk **Python 2.7** nebo **Python 3.4**.</span><span class="sxs-lookup"><span data-stu-id="44671-131">Select **Python 2.7** or **Python 3.4** as the base interpreter.</span></span>
   
    ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="44671-133">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte položku **Python** a poté položku **Django migrace**.</span><span class="sxs-lookup"><span data-stu-id="44671-133">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="44671-134">Pak vyberte možnost **Vytvořit superživatele Django**.</span><span class="sxs-lookup"><span data-stu-id="44671-134">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="44671-135">Tím se otevřete Konzola pro správu Django a ve složce projektu se vytvoří databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="44671-135">This will open a Django Management Console and create a sqlite database in the project folder.</span></span> <span data-ttu-id="44671-136">Postupujte podle výzev a vytvořte uživatele.</span><span class="sxs-lookup"><span data-stu-id="44671-136">Follow the prompts to create a user.</span></span>
7. <span data-ttu-id="44671-137">Stisknutím klávesy `F5` se ujistěte, zda aplikace pracuje.</span><span class="sxs-lookup"><span data-stu-id="44671-137">Confirm that the application works by pressing `F5`.</span></span>
8. <span data-ttu-id="44671-138">V horním navigačním panelu klikněte na možnost **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="44671-138">Click **Log in** from the navigation bar at the top.</span></span>
   
    ![Navigační panel Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="44671-140">Zadejte přihlašovací údaje uživatele, kterého jste vytvořili při synchronizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="44671-140">Enter the credentials for the user you created when you synchronized the database.</span></span>
   
    ![Přihlašovací formulář](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="44671-142">Klikněte na možnost **Vytvořit ukázková hlasování**.</span><span class="sxs-lookup"><span data-stu-id="44671-142">Click **Create Sample Polls**.</span></span>
    
     ![Vytvoření ukázkových hlasování](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="44671-144">Klikněte na hlasování a hlasujte.</span><span class="sxs-lookup"><span data-stu-id="44671-144">Click on a poll and vote.</span></span>
    
     ![Hlasování v ukázkových hlasováních](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a><span data-ttu-id="44671-146">Vytvoření databáze MySQL Database</span><span class="sxs-lookup"><span data-stu-id="44671-146">Create a MySQL Database</span></span>
<span data-ttu-id="44671-147">Jako databázi vytvoříte databázi ClearDB hostovanou v MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="44671-147">For the database, you'll create a ClearDB MySQL hosted database on Azure.</span></span>

<span data-ttu-id="44671-148">Alternativně můžete vytvořit svůj vlastní virtuální počítač spuštěný v Azure a poté sami nainstalovat a spravovat MySQL.</span><span class="sxs-lookup"><span data-stu-id="44671-148">As an alternative, you can create your own Virtual Machine running in Azure, then install and administer MySQL yourself.</span></span>

<span data-ttu-id="44671-149">Následujícím postupem můžete vytvořit databázi s bezplatným plánem.</span><span class="sxs-lookup"><span data-stu-id="44671-149">You can create a database with a free plan by following these steps.</span></span>

1. <span data-ttu-id="44671-150">Přihlaste se k [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="44671-150">Log in to the [Azure Portal].</span></span>
2. <span data-ttu-id="44671-151">V horní části podokna navigace klikněte na položku **NOVÉ**, **Data + úložiště** a poté na položku **Databáze MySQL**.</span><span class="sxs-lookup"><span data-stu-id="44671-151">At the Top of the navigation pane, click **NEW**, then click **Data + Storage**, and then click **MySQL Database**.</span></span>
3. <span data-ttu-id="44671-152">Nakonfigurujte novou databázi MySQL tím, že vytvoříte novou skupinu prostředků a vyberte pro ni vhodné umístění.</span><span class="sxs-lookup"><span data-stu-id="44671-152">Configure the new MySQL database by creating a new resource group and select the appropriate location for it.</span></span>
4. <span data-ttu-id="44671-153">Po vytvoření databáze MySQL klikněte v okně databáze na možnost **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="44671-153">Once the MySQL database is created, click **Properties** in the database blade.</span></span>
5. <span data-ttu-id="44671-154">Tlačítkem kopírování vložte hodnotu **PŘIPOJOVACÍ ŘETĚZEC** do schránky.</span><span class="sxs-lookup"><span data-stu-id="44671-154">Use the copy button to put the value of **CONNECTION STRING** on the clipboard.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="44671-155">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="44671-155">Configure the Project</span></span>
<span data-ttu-id="44671-156">V této části nakonfigurujete webovou aplikaci k používání databáze MySQL, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="44671-156">In this section, you'll configure our web app to use the MySQL database you just created.</span></span> <span data-ttu-id="44671-157">Nainstalujete také další balíčky Python, které jsou nezbytné pro používání databází MySQL s rozhraním Django.</span><span class="sxs-lookup"><span data-stu-id="44671-157">You'll also install additional Python packages required to use MySQL databases with Django.</span></span> <span data-ttu-id="44671-158">Poté webovou aplikaci místně spustíte.</span><span class="sxs-lookup"><span data-stu-id="44671-158">Then you'll run the web app locally.</span></span>

1. <span data-ttu-id="44671-159">V sadě Visual Studio otevřete soubor **settings.py** ze složky *NázevProjektu*.</span><span class="sxs-lookup"><span data-stu-id="44671-159">In Visual Studio, open **settings.py**, from the *ProjectName* folder.</span></span> <span data-ttu-id="44671-160">Dočasně vložte připojovací řetězec do editoru.</span><span class="sxs-lookup"><span data-stu-id="44671-160">Temporarily paste the connection string in the editor.</span></span> <span data-ttu-id="44671-161">Připojovací řetězec má tento formát:</span><span class="sxs-lookup"><span data-stu-id="44671-161">The connection string is in this format:</span></span>
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    <span data-ttu-id="44671-162">Změňte výchozí parametr databáze **ENGINE** tak, aby používal MySQL, a nastavte hodnoty parametrů **NAME**, **USER**, **PASSWORD** a **HOST** z **CONNECTIONSTRING**.</span><span class="sxs-lookup"><span data-stu-id="44671-162">Change the default database **ENGINE** to use MySQL, and set the values for **NAME**, **USER**, **PASSWORD** and **HOST** from the **CONNECTIONSTRING**.</span></span>
   
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
2. <span data-ttu-id="44671-163">V Průzkumníku řešení v části **Prostředí Python** klikněte pravým tlačítkem na virtuální prostředí a vyberte položku **Instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="44671-163">In Solution Explorer, under **Python Environments**, right-click on the virtual environment and select **Install Python Package**.</span></span>
3. <span data-ttu-id="44671-164">Nainstalujte balíček `mysqlclient` pomocí funkce **pip**.</span><span class="sxs-lookup"><span data-stu-id="44671-164">Install the package `mysqlclient` using **pip**.</span></span>
   
    ![Dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. <span data-ttu-id="44671-166">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte položku **Python** a poté položku **Django migrace**.</span><span class="sxs-lookup"><span data-stu-id="44671-166">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="44671-167">Pak vyberte možnost **Vytvořit superživatele Django**.</span><span class="sxs-lookup"><span data-stu-id="44671-167">Then select **Django Create Superuser**.</span></span>
   
    <span data-ttu-id="44671-168">Tím se vytvoří tabulky pro databázi MySQL, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="44671-168">This will create the tables for the MySQL database you created in the previous section.</span></span> <span data-ttu-id="44671-169">Postupujte podle výzev a vytvořte uživatele, který se nemusí shodovat s uživatelem v databázi SQLite vytvořené v první části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="44671-169">Follow the prompts to create a user, which doesn't have to match the user in the sqlite database created in the first section of this article.</span></span>
5. <span data-ttu-id="44671-170">Spusťte aplikaci klávesou `F5`.</span><span class="sxs-lookup"><span data-stu-id="44671-170">Run the application with `F5`.</span></span> <span data-ttu-id="44671-171">Hlasování vytvořená pomocí položky **Vytvořit ukázková hlasování** a data odeslaná při hlasování budou serializována v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="44671-171">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in the MySQL database.</span></span>

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="44671-172">Publikování webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="44671-172">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="44671-173">Sada Azure .NET SDK poskytuje snadný způsob, jak nasadit webovou aplikaci do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="44671-173">The Azure .NET SDK provides an easy way to deploy your web app to Azure App Service.</span></span>

1. <span data-ttu-id="44671-174">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="44671-174">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>
   
    ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="44671-176">Klikněte na **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="44671-176">Click on **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="44671-177">Kliknutím na možnost **Nové** vytvořte novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="44671-177">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="44671-178">Vyplňte následující pole a klikněte na možnost **Vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="44671-178">Fill in the following fields and click **Create**:</span></span>
   
   * <span data-ttu-id="44671-179">**Název webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="44671-179">**Web App name**</span></span>
   * <span data-ttu-id="44671-180">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="44671-180">**App Service plan**</span></span>
   * <span data-ttu-id="44671-181">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="44671-181">**Resource group**</span></span>
   * <span data-ttu-id="44671-182">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="44671-182">**Region**</span></span>
   * <span data-ttu-id="44671-183">Položku **Databázový server** ponechte nastavenou na možnost **Bez databáze**</span><span class="sxs-lookup"><span data-stu-id="44671-183">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="44671-184">Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="44671-184">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="44671-185">Automaticky se otevře webový prohlížeč s publikovanou webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="44671-185">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="44671-186">Měli byste vidět, že webová aplikace funguje očekávaným způsobem a využívá databázi **MySQL** hostovanou v Azure.</span><span class="sxs-lookup"><span data-stu-id="44671-186">You should see the web app working as expected, using the **MySQL** database hosted on Azure.</span></span>
   
    ![Webový prohlížeč](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    <span data-ttu-id="44671-188">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="44671-188">Congratulations!</span></span> <span data-ttu-id="44671-189">Úspěšně jste publikovali webovou aplikaci založenou na MySQL do Azure.</span><span class="sxs-lookup"><span data-stu-id="44671-189">You have successfully published your MySQL-based web app to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44671-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44671-190">Next steps</span></span>
<span data-ttu-id="44671-191">Potřebujete-li další informace o nástrojích Python Tools pro Visual Studio, rozhraní Django a MySQL, použijte tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="44671-191">Follow these links to learn more about Python Tools for Visual Studio, Django and MySQL.</span></span>

* <span data-ttu-id="44671-192">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="44671-192">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="44671-193">[Webové projekty]</span><span class="sxs-lookup"><span data-stu-id="44671-193">[Web Projects]</span></span>
  * <span data-ttu-id="44671-194">[Projekty cloudových služeb]</span><span class="sxs-lookup"><span data-stu-id="44671-194">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="44671-195">[Vzdálené ladění v Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="44671-195">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="44671-196">[Dokumentace rozhraní Django]</span><span class="sxs-lookup"><span data-stu-id="44671-196">[Django Documentation]</span></span>
* <span data-ttu-id="44671-197">[MySQL]</span><span class="sxs-lookup"><span data-stu-id="44671-197">[MySQL]</span></span>

<span data-ttu-id="44671-198">Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="44671-198">For more information, see the [Python Developer Center](/develop/python/).</span></span>

<!--Link references-->

[Středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
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
