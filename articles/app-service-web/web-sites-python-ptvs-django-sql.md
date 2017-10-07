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
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a>Django a databáze SQL Database v Azure s nástroji Python Tools 2.2 pro Visual Studio
V tomto kurzu použijeme [Python Tools pro Visual Studio] dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello. V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).

Jsme dozvíte, jak toouse databázi SQL hostované v Azure, jak tooconfigure hello webové aplikace toouse databázi SQL a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami Azure Table Storage, MySQL a SQL Database. Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2015
* [Python 2.7 (32bitová verze)]
* [Python Tools 2.2 pro Visual Studio]
* [Python Tools 2.2 pro Visual Studio – ukázky VSIX]
* [Azure SDK Tools for VS 2015]
* Django 1.9 nebo novější

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a name="create-hello-project"></a>Vytvoření projektu hello
V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony. Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky. Vytvoříme místní databázi pomocí sqlite. Potom jsme budete hello webovou aplikaci spouštět místně.

1. V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.
2. šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**. Vyberte **hlasovací webový projekt Django** a klikněte na tlačítko OK toocreate hello projektu.

     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. Bude výzvami tooinstall externí balíčky. Vyberte možnost **Instalovat do virtuálního prostředí**.

     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. Vyberte **Python 2.7** jako základní překladač hello.

     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.  Pak vyberte možnost **Vytvořit superživatele Django**.
6. Tím se otevřete Konzola pro správu Django a ve složce projektu hello se vytvoří databáze sqlite. Postupujte podle výzvy toocreate hello uživatele.
7. Potvrďte, že hello aplikace funguje tak, že stisknete <kbd>F5</kbd>.
8. Klikněte na tlačítko **přihlásit** z hello navigačního panelu v horní části hello.

     ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. Zadejte přihlašovací údaje hello hello uživatele, kterého jste vytvořili při synchronizaci databáze hello.

     ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klikněte na možnost **Vytvořit ukázková hlasování**.

      ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. Klikněte na hlasování a hlasujte.

      ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a>Vytvoření databáze SQL
Pro databázi hello vytvoříme Azure SQL database.

Pomocí následujícího postupu můžete vytvořit databázi.

1. Přihlaste se k hello [portálu Azure].
2. Hello dolní části navigačního podokna hello, klikněte na **nový**. , klikněte na tlačítko **Data + úložiště** > **SQL Database**.
3. Konfigurace hello novou databázi SQL tak, že vytvoříte novou skupinu prostředků a vyberte hello pro ni vhodné umístění.
4. Po vytvoření hello databáze SQL, klikněte na tlačítko **otevřete v sadě Visual Studio** v okně databáze hello.
5. Klikněte na tlačítko **nakonfigurovat bránu firewall**.
6. V hello **nastavení brány Firewall** okně Přidat pravidlo brány firewall s **počáteční IP** a **KONCOVÁ IP adresa** nastavit toohello veřejnou IP adresu počítači pro vývoj. Klikněte na **Uložit**.

   To vám umožní připojení toohello databázového serveru z vývojovém počítači.
7. Zpět v okně databáze hello, klikněte na tlačítko **vlastnosti**, pak klikněte na tlačítko **zobrazit databázové připojovací řetězce**.
8. Hello kopie tlačítko tooput hello hodnotu **ADO.NET** hello schránky.

## <a name="configure-hello-project"></a>Konfigurace hello projektu
V této části nakonfigurujeme naše webové aplikace toouse hello SQL databázi, kterou jsme právě vytvořili. Nainstalujeme také další Python balíčky požadované toouse SQL databáze s rozhraním Django. Potom jsme budete hello webovou aplikaci spouštět místně.

1. V sadě Visual Studio otevřete **settings.py**, z hello *ProjectName* složky. Dočasně vložte připojovací řetězec hello v editoru hello. Hello připojovací řetězec je v tomto formátu:

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

Upravit definici hello `DATABASES` toouse hello hodnoty výše.

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

1. V Průzkumníku řešení klikněte v části **prostředí Python**, klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **instalovat balíček Python**.
2. Instalovat balíček hello `pyodbc` pomocí **pip**.

     ![Python dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. Instalovat balíček hello `django-pyodbc-azure` pomocí **pip**.

     ![Python dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.  Pak vyberte možnost **Vytvořit superživatele Django**.

   Tím se vytvoří hello tabulky pro hello databázi SQL, které jsme vytvořili v předchozí části hello. Postupujte podle toocreate hello vyzve uživatele, který nemá toomatch hello uživatele v databázi sqlite hello vytvořené v první části hello.
5. Spuštění aplikace hello s `F5`. Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v databázi SQL hello.

## <a name="publish-hello-web-app-tooazure-app-service"></a>Publikování hello webové aplikace tooAzure služby App Service
Hello .NET SDK služby Azure poskytuje snadno toodeploy váš web webové aplikace tooAzure App Service Web Apps.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.

     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. Klikněte na položku **Microsoft Azure Web Apps**.
3. Klikněte na **nový** toocreate novou webovou aplikaci.
4. Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**.

   * **Název webové aplikace**
   * **Plán služby App Service**
   * **Skupina prostředků**
   * **Oblast**
   * Nechte **databázový server** nastavit příliš**žádná databáze.**
5. Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.
6. Webový prohlížeč se automaticky otevře toohello publikované webové aplikace. Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **SQL** databáze, které jsou hostované v Azure.

   Blahopřejeme!

     ![Webový prohlížeč](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, rozhraní Django a databáze SQL.

* [Dokumentace nástrojů Python Tools pro Visual Studio]
  * [Webové projekty]
  * [Projekty cloudových služeb]
  * [Vzdálené ladění v Microsoft Azure]
* [Dokumentace rozhraní Django]
* [SQL Database]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

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
