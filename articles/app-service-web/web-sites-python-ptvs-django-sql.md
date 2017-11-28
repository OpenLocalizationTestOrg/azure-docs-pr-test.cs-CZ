---
title: "aaaDjango a SQL Database na Azure pomocí nástroje Python Tools 2.2 pro Visual Studio"
description: "Zjistěte, jak toouse hello Python Tools pro Visual Studio toocreate webové aplikace Django, která ukládá data v instanci databáze SQL a nasaďte ji tooAzure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 3a677e64-b5a9-4d43-b9c0-66246368b483
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: b5b2ef4f3292e7df85007465c5394c8660a7d231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="77b4c-103">Django a databáze SQL Database v Azure s nástroji Python Tools 2.2 pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77b4c-103">Django and SQL Database on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="77b4c-104">V tomto kurzu použijeme [Python Tools pro Visual Studio] dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="77b4c-105">V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span><span class="sxs-lookup"><span data-stu-id="77b4c-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span></span>

<span data-ttu-id="77b4c-106">Jsme dozvíte, jak toouse databázi SQL hostované v Azure, jak tooconfigure hello webové aplikace toouse databázi SQL a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="77b4c-106">We'll learn how toouse a SQL database hosted on Azure, how tooconfigure hello web app toouse a SQL database, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="77b4c-107">V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami Azure Table Storage, MySQL a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="77b4c-107">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="77b4c-108">Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].</span><span class="sxs-lookup"><span data-stu-id="77b4c-108">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77b4c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="77b4c-109">Prerequisites</span></span>
* <span data-ttu-id="77b4c-110">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="77b4c-110">Visual Studio 2015</span></span>
* <span data-ttu-id="77b4c-111">[Python 2.7 (32bitová verze)]</span><span class="sxs-lookup"><span data-stu-id="77b4c-111">[Python 2.7 32-bit]</span></span>
* <span data-ttu-id="77b4c-112">[Python Tools 2.2 pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="77b4c-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="77b4c-113">[Python Tools 2.2 pro Visual Studio – ukázky VSIX]</span><span class="sxs-lookup"><span data-stu-id="77b4c-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="77b4c-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="77b4c-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="77b4c-115">Django 1.9 nebo novější</span><span class="sxs-lookup"><span data-stu-id="77b4c-115">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="77b4c-116">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="77b4c-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="77b4c-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="77b4c-117">No credit cards required; no commitments.</span></span>
>
>

## <a name="create-hello-project"></a><span data-ttu-id="77b4c-118">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="77b4c-118">Create hello Project</span></span>
<span data-ttu-id="77b4c-119">V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="77b4c-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="77b4c-120">Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="77b4c-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="77b4c-121">Vytvoříme místní databázi pomocí sqlite.</span><span class="sxs-lookup"><span data-stu-id="77b4c-121">We'll create a local database using sqlite.</span></span> <span data-ttu-id="77b4c-122">Potom jsme budete hello webovou aplikaci spouštět místně.</span><span class="sxs-lookup"><span data-stu-id="77b4c-122">Then we'll run hello web app locally.</span></span>

1. <span data-ttu-id="77b4c-123">V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-123">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="77b4c-124">šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-124">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="77b4c-125">Vyberte **hlasovací webový projekt Django** a klikněte na tlačítko OK toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="77b4c-125">Select **Polls Django Web Project** and click OK toocreate hello project.</span></span>

     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. <span data-ttu-id="77b4c-127">Bude výzvami tooinstall externí balíčky.</span><span class="sxs-lookup"><span data-stu-id="77b4c-127">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="77b4c-128">Vyberte možnost **Instalovat do virtuálního prostředí**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-128">Select **Install into a virtual environment**.</span></span>

     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="77b4c-130">Vyberte **Python 2.7** jako základní překladač hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-130">Select **Python 2.7** as hello base interpreter.</span></span>

     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="77b4c-132">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-132">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="77b4c-133">Pak vyberte možnost **Vytvořit superživatele Django**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-133">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="77b4c-134">Tím se otevřete Konzola pro správu Django a ve složce projektu hello se vytvoří databáze sqlite.</span><span class="sxs-lookup"><span data-stu-id="77b4c-134">This will open a Django Management Console and create a sqlite database in hello project folder.</span></span> <span data-ttu-id="77b4c-135">Postupujte podle výzvy toocreate hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="77b4c-135">Follow hello prompts toocreate a user.</span></span>
7. <span data-ttu-id="77b4c-136">Potvrďte, že hello aplikace funguje tak, že stisknete <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="77b4c-136">Confirm that hello application works by pressing <kbd>F5</kbd>.</span></span>
8. <span data-ttu-id="77b4c-137">Klikněte na tlačítko **přihlásit** z hello navigačního panelu v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-137">Click **Log in** from hello navigation bar at hello top.</span></span>

     ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="77b4c-139">Zadejte přihlašovací údaje hello hello uživatele, kterého jste vytvořili při synchronizaci databáze hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-139">Enter hello credentials for hello user you created when you synchronized hello database.</span></span>

     ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="77b4c-141">Klikněte na možnost **Vytvořit ukázková hlasování**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-141">Click **Create Sample Polls**.</span></span>

      ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="77b4c-143">Klikněte na hlasování a hlasujte.</span><span class="sxs-lookup"><span data-stu-id="77b4c-143">Click on a poll and vote.</span></span>

      ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a><span data-ttu-id="77b4c-145">Vytvoření databáze SQL</span><span class="sxs-lookup"><span data-stu-id="77b4c-145">Create a SQL Database</span></span>
<span data-ttu-id="77b4c-146">Pro databázi hello vytvoříme Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="77b4c-146">For hello database, we'll create an Azure SQL database.</span></span>

<span data-ttu-id="77b4c-147">Pomocí následujícího postupu můžete vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="77b4c-147">You can create a database by following these steps.</span></span>

1. <span data-ttu-id="77b4c-148">Přihlaste se k hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="77b4c-148">Log into hello [Azure Portal].</span></span>
2. <span data-ttu-id="77b4c-149">Hello dolní části navigačního podokna hello, klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-149">At hello bottom of hello navigation pane, click **NEW**.</span></span> <span data-ttu-id="77b4c-150">, klikněte na tlačítko **Data + úložiště** > **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-150">, click **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="77b4c-151">Konfigurace hello novou databázi SQL tak, že vytvoříte novou skupinu prostředků a vyberte hello pro ni vhodné umístění.</span><span class="sxs-lookup"><span data-stu-id="77b4c-151">Configure hello new SQL Database by creating a new resource group and select hello appropriate location for it.</span></span>
4. <span data-ttu-id="77b4c-152">Po vytvoření hello databáze SQL, klikněte na tlačítko **otevřete v sadě Visual Studio** v okně databáze hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-152">Once hello SQL Database is created, click **Open in Visual Studio** in hello database blade.</span></span>
5. <span data-ttu-id="77b4c-153">Klikněte na tlačítko **nakonfigurovat bránu firewall**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-153">Click **Configure your firewall**.</span></span>
6. <span data-ttu-id="77b4c-154">V hello **nastavení brány Firewall** okně Přidat pravidlo brány firewall s **počáteční IP** a **KONCOVÁ IP adresa** nastavit toohello veřejnou IP adresu počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="77b4c-154">In hello **Firewall Settings** blade, add a firewall rule with **START IP** and **END IP** set toohello public IP address of your development machine.</span></span> <span data-ttu-id="77b4c-155">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-155">Click **Save**.</span></span>

   <span data-ttu-id="77b4c-156">To vám umožní připojení toohello databázového serveru z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="77b4c-156">This will allow connections toohello database server from your development machine.</span></span>
7. <span data-ttu-id="77b4c-157">Zpět v okně databáze hello, klikněte na tlačítko **vlastnosti**, pak klikněte na tlačítko **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-157">Back in hello database blade, click **Properties**, then click **Show database connection strings**.</span></span>
8. <span data-ttu-id="77b4c-158">Hello kopie tlačítko tooput hello hodnotu **ADO.NET** hello schránky.</span><span class="sxs-lookup"><span data-stu-id="77b4c-158">Use hello copy button tooput hello value of **ADO.NET** on hello clipboard.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="77b4c-159">Konfigurace hello projektu</span><span class="sxs-lookup"><span data-stu-id="77b4c-159">Configure hello Project</span></span>
<span data-ttu-id="77b4c-160">V této části nakonfigurujeme naše webové aplikace toouse hello SQL databázi, kterou jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="77b4c-160">In this section, we'll configure our web app toouse hello SQL database we just created.</span></span> <span data-ttu-id="77b4c-161">Nainstalujeme také další Python balíčky požadované toouse SQL databáze s rozhraním Django.</span><span class="sxs-lookup"><span data-stu-id="77b4c-161">We'll also install additional Python packages required toouse SQL databases with Django.</span></span> <span data-ttu-id="77b4c-162">Potom jsme budete hello webovou aplikaci spouštět místně.</span><span class="sxs-lookup"><span data-stu-id="77b4c-162">Then we'll run hello web app locally.</span></span>

1. <span data-ttu-id="77b4c-163">V sadě Visual Studio otevřete **settings.py**, z hello *ProjectName* složky.</span><span class="sxs-lookup"><span data-stu-id="77b4c-163">In Visual Studio, open **settings.py**, from hello *ProjectName* folder.</span></span> <span data-ttu-id="77b4c-164">Dočasně vložte připojovací řetězec hello v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-164">Temporarily paste hello connection string in hello editor.</span></span> <span data-ttu-id="77b4c-165">Hello připojovací řetězec je v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="77b4c-165">hello connection string is in this format:</span></span>

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

<span data-ttu-id="77b4c-166">Upravit definici hello `DATABASES` toouse hello hodnoty výše.</span><span class="sxs-lookup"><span data-stu-id="77b4c-166">Edit hello definition of `DATABASES` toouse hello values above.</span></span>

        DATABASES = {
            'default': {
                'ENGINE': 'sql_server.pyodbc',
                'NAME': '<DatabaseName>',
                'USER': '<UserName>',
                'PASSWORD': '{your_password_here}',
                'HOST': '<ServerName>',
                'PORT': '<ServerPort>',
                'OPTIONS': {
                    'driver': 'SQL Server Native Client 11.0',
                    'MARS_Connection': 'True',
                }
            }
        }

1. <span data-ttu-id="77b4c-167">V Průzkumníku řešení klikněte v části **prostředí Python**, klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **instalovat balíček Python**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-167">In Solution Explorer, under **Python Environments**, right-click on hello virtual environment and select **Install Python Package**.</span></span>
2. <span data-ttu-id="77b4c-168">Instalovat balíček hello `pyodbc` pomocí **pip**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-168">Install hello package `pyodbc` using **pip**.</span></span>

     ![Python dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. <span data-ttu-id="77b4c-170">Instalovat balíček hello `django-pyodbc-azure` pomocí **pip**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-170">Install hello package `django-pyodbc-azure` using **pip**.</span></span>

     ![Python dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. <span data-ttu-id="77b4c-172">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-172">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="77b4c-173">Pak vyberte možnost **Vytvořit superživatele Django**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-173">Then select **Django Create Superuser**.</span></span>

   <span data-ttu-id="77b4c-174">Tím se vytvoří hello tabulky pro hello databázi SQL, které jsme vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-174">This will create hello tables for hello SQL database we created in hello previous section.</span></span> <span data-ttu-id="77b4c-175">Postupujte podle toocreate hello vyzve uživatele, který nemá toomatch hello uživatele v databázi sqlite hello vytvořené v první části hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-175">Follow hello prompts toocreate a user, which doesn't have toomatch hello user in hello sqlite database created in hello first section.</span></span>
5. <span data-ttu-id="77b4c-176">Spuštění aplikace hello s `F5`.</span><span class="sxs-lookup"><span data-stu-id="77b4c-176">Run hello application with `F5`.</span></span> <span data-ttu-id="77b4c-177">Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v databázi SQL hello.</span><span class="sxs-lookup"><span data-stu-id="77b4c-177">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in hello SQL database.</span></span>

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="77b4c-178">Publikování hello webové aplikace tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="77b4c-178">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="77b4c-179">Hello .NET SDK služby Azure poskytuje snadno toodeploy váš web webové aplikace tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="77b4c-179">hello Azure .NET SDK provides an easy way toodeploy your web web app tooAzure App Service Web Apps.</span></span>

1. <span data-ttu-id="77b4c-180">V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-180">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>

     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="77b4c-182">Klikněte na položku **Microsoft Azure Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="77b4c-183">Klikněte na **nový** toocreate novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77b4c-183">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="77b4c-184">Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-184">Fill in hello following fields and click **Create**.</span></span>

   * <span data-ttu-id="77b4c-185">**Název webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="77b4c-185">**Web App name**</span></span>
   * <span data-ttu-id="77b4c-186">**Plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="77b4c-186">**App Service plan**</span></span>
   * <span data-ttu-id="77b4c-187">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="77b4c-187">**Resource group**</span></span>
   * <span data-ttu-id="77b4c-188">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="77b4c-188">**Region**</span></span>
   * <span data-ttu-id="77b4c-189">Nechte **databázový server** nastavit příliš**žádná databáze.**</span><span class="sxs-lookup"><span data-stu-id="77b4c-189">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="77b4c-190">Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="77b4c-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="77b4c-191">Webový prohlížeč se automaticky otevře toohello publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="77b4c-191">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="77b4c-192">Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **SQL** databáze, které jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="77b4c-192">You should see hello web app working as expected, using hello **SQL** database hosted on Azure.</span></span>

   <span data-ttu-id="77b4c-193">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="77b4c-193">Congratulations!</span></span>

     ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="77b4c-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77b4c-195">Next steps</span></span>
<span data-ttu-id="77b4c-196">Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, rozhraní Django a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="77b4c-196">Follow these links toolearn more about Python Tools for Visual Studio, Django and SQL Database.</span></span>

* <span data-ttu-id="77b4c-197">[Dokumentace nástrojů Python Tools pro Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="77b4c-197">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="77b4c-198">[Webové projekty]</span><span class="sxs-lookup"><span data-stu-id="77b4c-198">[Web Projects]</span></span>
  * <span data-ttu-id="77b4c-199">[Projekty cloudových služeb]</span><span class="sxs-lookup"><span data-stu-id="77b4c-199">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="77b4c-200">[Vzdálené ladění v Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="77b4c-200">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="77b4c-201">[Dokumentace rozhraní Django]</span><span class="sxs-lookup"><span data-stu-id="77b4c-201">[Django Documentation]</span></span>
* <span data-ttu-id="77b4c-202">[SQL Database]</span><span class="sxs-lookup"><span data-stu-id="77b4c-202">[SQL Database]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="77b4c-203">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="77b4c-203">What's changed</span></span>
* <span data-ttu-id="77b4c-204">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="77b4c-204">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->
[portálu Azure]: https://portal.azure.com
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 pro Visual Studio – ukázky VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Vzdálené ladění v Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webové projekty]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Dokumentace rozhraní Django]: https://www.djangoproject.com/
[SQL Database]: /documentation/services/sql-database/
