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
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a>Django a MySQL v Azure s nástroji Python Tools 2.2 pro Visual Studio
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto kurzu budete používat [Python Tools pro Visual Studio](https://www.visualstudio.com/vs/python) dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello. Dozvíte se, jak toouse službu MySQL hostovanou v Azure, jak tooconfigure hello webové aplikace toouse MySQL a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

> [!NOTE]
> Hello informace obsažené v tomto kurzu jsou také k dispozici v hello následující video:
> 
> [PTVS 2.1: aplikace Django s MySQL][video]
> 
> 

V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami Azure Table Storage, MySQL a SQL Database. Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2015
* [Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]
* [Python Tools 2.2 pro Visual Studio]
* [Python Tools 2.2 pro Visual Studio – ukázky VSIX]
* [Azure SDK Tools for VS 2015]
* Django 1.9 nebo novější

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of hello hello previous include. -->

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Není požadována platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-hello-project"></a>Vytvoření projektu hello
V této části vytvoříte projekt sady Visual Studio pomocí vzorové šablony. Vytvoříte virtuální prostředí a nainstalujte požadované balíčky. Vytvoříte místní databázi pomocí SQLite. Poté místně spustíte aplikaci hello.

1. V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.
2. šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**. Vyberte **hlasovací webový projekt Django** a klikněte na tlačítko OK toocreate hello projektu.
   
    ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. Bude výzvami tooinstall externí balíčky. Vyberte možnost **Instalovat do virtuálního prostředí**.
   
    ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. Vyberte **Python 2.7** nebo **Python 3.4** jako základní překladač hello.
   
    ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.  Pak vyberte možnost **Vytvořit superživatele Django**.
6. Tím se otevřete Konzola pro správu Django a ve složce projektu hello se vytvoří databáze sqlite. Postupujte podle výzvy toocreate hello uživatele.
7. Potvrďte, že hello aplikace funguje tak, že stisknete `F5`.
8. Klikněte na tlačítko **přihlásit** z hello navigačního panelu v horní části hello.
   
    ![Navigační panel Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. Zadejte přihlašovací údaje hello hello uživatele, kterého jste vytvořili při synchronizaci databáze hello.
   
    ![Přihlašovací formulář](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klikněte na možnost **Vytvořit ukázková hlasování**.
    
     ![Vytvoření ukázkových hlasování](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. Klikněte na hlasování a hlasujte.
    
     ![Hlasování v ukázkových hlasováních](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a>Vytvoření databáze MySQL Database
Pro databázi hello vytvoříte databázi ClearDB MySQL hostovanou v Azure.

Alternativně můžete vytvořit svůj vlastní virtuální počítač spuštěný v Azure a poté sami nainstalovat a spravovat MySQL.

Následujícím postupem můžete vytvořit databázi s bezplatným plánem.

1. Přihlaste se toohello [portálu Azure].
2. V horní části podokna navigace hello hello, klikněte na položku **nový**, pak klikněte na tlačítko **Data + úložiště**a potom klikněte na **databáze MySQL**.
3. Nakonfigurujte novou databázi MySQL hello tak, že vytvoříte novou skupinu prostředků a vyberte pro ni vhodné umístění hello.
4. Po vytvoření databáze MySQL hello klikněte na tlačítko **vlastnosti** v okně databáze hello.
5. Hello kopie tlačítko tooput hello hodnotu **PŘIPOJOVACÍ řetězec** hello schránky.

## <a name="configure-hello-project"></a>Konfigurace hello projektu
V této části nakonfigurujete naše webové aplikace toouse hello databáze MySQL, kterou jste právě vytvořili. Nainstalujete také další databází MySQL požadované toouse balíčků Python s rozhraním Django. Poté místně spustíte hello webové aplikace.

1. V sadě Visual Studio otevřete **settings.py**, z hello *ProjectName* složky. Dočasně vložte připojovací řetězec hello v editoru hello. Hello připojovací řetězec je v tomto formátu:
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    Změna hello výchozí databázi **modul** toouse MySQL a nastavte hodnoty pro hello **název**, **uživatele**, **heslo** a  **HOSTITELE** z hello **CONNECTIONSTRING**.
   
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
2. V Průzkumníku řešení klikněte v části **prostředí Python**, klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **instalovat balíček Python**.
3. Instalovat balíček hello `mysqlclient` pomocí **pip**.
   
    ![Dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **Python**a potom vyberte **Django migrovat**.  Pak vyberte možnost **Vytvořit superživatele Django**.
   
    Tím se vytvoří hello tabulky pro databázi MySQL hello, kterou jste vytvořili v předchozí části hello. Postupujte podle toocreate hello vyzve uživatele, který nemá toomatch hello uživatele v databázi sqlite hello vytvořené v první části tohoto článku hello.
5. Spuštění aplikace hello s `F5`. Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v databázi MySQL hello.

## <a name="publish-hello-web-app-tooazure-app-service"></a>Publikování hello webové aplikace tooAzure služby App Service
Hello .NET SDK služby Azure poskytuje snadno toodeploy tooAzure vaší webové aplikace služby App Service.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.
   
    ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. Klikněte na **Microsoft Azure App Service**.
3. Klikněte na **nový** toocreate novou webovou aplikaci.
4. Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**:
   
   * **Název webové aplikace**
   * **Plán služby App Service**
   * **Skupina prostředků**
   * **Oblast**
   * Nechte **databázový server** nastavit příliš**žádná databáze.**
5. Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.
6. Webový prohlížeč se automaticky otevře toohello publikované webové aplikace. Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **MySQL** databáze, které jsou hostované v Azure.
   
    ![Webový prohlížeč](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    Blahopřejeme! Úspěšně jste publikovali vaší tooAzure založenou na MySQL webové aplikace.

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, rozhraní Django a MySQL.

* [Dokumentace nástrojů Python Tools pro Visual Studio]
  * [Webové projekty]
  * [Projekty cloudových služeb]
  * [Vzdálené ladění v Microsoft Azure]
* [Dokumentace rozhraní Django]
* [MySQL]

Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).

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
