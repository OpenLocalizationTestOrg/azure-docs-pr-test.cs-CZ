---
title: "Vytvoření webové aplikace pomocí Flask v Azure"
description: "Kurz vás seznámí s webovou aplikaci Python spuštěné v Azure."
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
ms.openlocfilehash: 29457a39ee3df0bbdbc9869cdce0e14bd85b7302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a>Vytvoření webové aplikace pomocí Flask v Azure
Tento kurz popisuje, jak začít a spustit jazyk Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).  Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python!  Souběžně s růstem aplikace můžete přejít na placené hostování a můžete také integrovat se všemi ostatními službami Azure.

Vytvoříte aplikaci pomocí webového rozhraní Flask (viz alternativní verze tohoto kurzu pro [Django](web-sites-python-create-deploy-django-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md)).  Vytvoření webu z Galerie Azure, nastavíte nasazení Git a místně naklonujete úložiště.  Poté místně spustíte aplikaci, provedete změny, potvrdíte je a nuceně vložíte do Azure.  V tomto kurzu se dozvíte, jak to provést ze systému Windows nebo Mac/Linux.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Není vyžadována platební karta a nevzniká žádný závazek.
> 
> 

## <a name="prerequisites"></a>Požadavky
* Windows, Mac nebo Linux
* Python 2.7 nebo 3.4
* setuptools, pip, virtualenv (pouze Python 2.7)
* Git
* [Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná.

**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.

### <a name="windows"></a>Windows
Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4].  Tím se nainstaluje 32bitová verze jazyka Python, setuptools, pip, virtualenv atd. (32bitová verze jazyka Python je nainstalována v hostitelských počítačích Azure).  Alternativně můžete získat jazyk Python z webu [python.org].

V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows].  Pokud používáte Visual Studio, můžete použít integrovanou podporu Git.

Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio].  Tato položka je volitelná, ale pokud máte sadu [Visual Studio], včetně bezplatné sady Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, tato položka vám poskytne skvělé rozhraní IDE pro jazyk Python.

### <a name="maclinux"></a>Mac/Linux
Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.

## <a name="web-app-create-on-the-azure-portal"></a>Vytvořit webovou aplikaci na portálu Azure
Prvním krokem při vytváření aplikace je vytvoření webové aplikace pomocí [Azure Portal](https://portal.azure.com). 

1. Přihlaste se k portálu Azure a v levém dolním rohu klikněte na tlačítko **NOVÉ**. 
2. Klikněte na možnost **Web + mobilní zařízení**.
3. Do vyhledávacího pole zadejte „python“.
4. Ve výsledcích hledání vyberte **Flask**, pak klikněte na tlačítko **vytvořit**.
5. Nakonfigurujte novou aplikaci Flask, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni. Poté klikněte na možnost **Vytvořit**.
6. Pro nově vytvořenou webovou aplikaci nakonfigurujte publikování Git podle pokynů uvedených v tématu [Místní nasazení GIT ve službě Azure App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Přehled aplikace
### <a name="git-repository-contents"></a>Obsah úložiště Git
Zde je uveden přehled souborů, které naleznete v počátečním úložišti Git, jež budeme v následující části klonovat.

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

Hlavní zdroje pro aplikaci.  Skládá se ze 3 stran (index, about, contact) s rozložením předlohy.  Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.

    \runserver.py

Podpora serveru místní vývoj. Použijte ke spuštění aplikace místně.

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].

    \ptvs_virtualenv_proxy.py

Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.

    \requirements.txt

Externí balíčky vyžadované touto aplikací. Skript nasazení nainstaluje nástrojem pip balíčky uvedené v tomto souboru.

    \web.2.7.config
    \web.3.4.config

Konfigurační soubory služby IIS.  Skript nasazení použije příslušný soubor web.x.y.config a zkopíruje jej jako soubor web.config.

### <a name="optional-files---customizing-deployment"></a>Volitelné soubory – přizpůsobení nasazení
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Volitelné soubory – modul Python runtime
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Další soubory na serveru
Na serveru existují některé soubory, které nejsou přidány do úložiště git.  Tyto soubory jsou vytvořeny skriptem nasazení.

    \web.config

Konfigurační soubor služby IIS.  Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.

    \env\

Virtuální prostředí Python.  Vytvoří se během nasazení, pokud ještě neexistuje kompatibilní virtuální prostředí na aplikaci.  Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, avšak nástroj pip instalaci přeskočí, pokud jsou dané balíčky již nainstalovány.

Následující 3 části popisují postup při vývoji webové aplikace ve 3 různých prostředích:

* Windows s nástroji Python Tools pro Visual Studio
* Windows s příkazovým řádkem
* Mac/Linux s příkazovým řádkem

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio
### <a name="clone-the-repository"></a>Klonování úložiště
Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure. Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).

Otevřete soubor řešení (.sln), který je zahrnut v kořenovém adresáři úložiště.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Nyní vytvoříme virtuální prostředí pro místní vývoj.  Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.

* Ujistěte se, zda název prostředí je `env`.
* Vyberte základní překladač.  Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).
* Ujistěte se, zda je zaškrtnutá možnost stažení a instalace balíčků.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

Klikněte na možnost **Vytvořit**.  Tím dojde k vytvoření virtuálního prostředí a instalaci závislostí uvedených v souboru requirements.txt.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Stisknutím klávesy F5 spusťte ladění, čímž se automaticky otevře webový prohlížeč s místně spuštěnou stránkou.

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

Můžete nastavit zarážky ve zdrojích, používat okna kukátka atd.  Další informace o jednotlivých funkcích naleznete v části [Dokumentace nástrojů Python Tools pro Visual Studio].

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.

Jakmile změny otestujete, potvrďte je do úložiště Git:

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít závislosti kromě Python a Flask.

Další balíčky můžete nainstalovat pomocí nástroje pip.  Chcete-li nainstalovat balíček, klikněte pravým tlačítkem na virtuální prostředí a vyberte možnost **Instalovat balíček Python**.

Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte `azure`:

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

Klikněte pravým tlačítkem na virtuální prostředí a výběrem možnosti **Generovat soubor requirements.txt** aktualizujte soubor requirements.txt.

Poté potvrďte změny souboru requirements.txt do úložiště Git.

### <a name="deploy-to-azure"></a>Nasazení do Azure
Chcete-li aktivovat nasazení, klikněte na možnost **Synchronizovat** nebo **Vložit změny (push)**.  Synchronizace provádí vložení změn (push) i přijetí změn (pull).

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

První nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.

Sada Visual Studio nezobrazuje průběh nasazení.  Chcete-li překontrolovat výstup, informace naleznete v tématu [Řešení potíží – nasazení](#troubleshooting-deployment).

Chcete-li zobrazit změny, přejděte na adresu URL Azure.

## <a name="web-app-development---windows---command-line"></a>Vývoj webových aplikací – Windows – příkazový řádek
### <a name="clone-the-repository"></a>Klonování úložiště
Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené. Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).  Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.

Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).

Pro jazyk Python 2.7:

    c:\python27\python.exe -m virtualenv env

Pro jazyk Python 3.4:

    c:\python34\python.exe -m venv env

Nainstalujte veškeré případné externí balíčky požadované aplikací. Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:

    env\scripts\python runserver.py

Konzola zobrazí adresa URL a port, jimž server naslouchá:

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

Poté tuto adresu URL otevřete ve webovém prohlížeči.

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.

Jakmile změny otestujete, potvrďte je do úložiště Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít závislosti kromě Python a Flask.

Další balíčky můžete nainstalovat pomocí nástroje pip.  Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:

    env\scripts\pip install azure

Nezapomeňte aktualizovat soubor requirements.txt:

    env\scripts\pip freeze > requirements.txt

Potvrďte změny:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Nasazení do Azure
Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:

    git push azure master

Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.

Chcete-li zobrazit změny, přejděte na adresu URL Azure.

## <a name="web-app-development---maclinux---command-line"></a>Vývoj webových aplikací – Mac/Linux – příkazový řádek
### <a name="clone-the-repository"></a>Klonování úložiště
Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené. Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště).  Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.

Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).

Pro jazyk Python 2.7:

    python -m virtualenv env

Pro jazyk Python 3.4:

    python -m venv env
nebo pyvenv env

Nainstalujte veškeré případné externí balíčky požadované aplikací. Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:

    env/bin/python runserver.py

Konzola zobrazí adresa URL a port, jimž server naslouchá:

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

Poté tuto adresu URL otevřete ve webovém prohlížeči.

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.

Jakmile změny otestujete, potvrďte je do úložiště Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít závislosti kromě Python a Flask.

Další balíčky můžete nainstalovat pomocí nástroje pip.  Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:

    env/bin/pip install azure

Nezapomeňte aktualizovat soubor requirements.txt:

    env/bin/pip freeze > requirements.txt

Potvrďte změny:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Nasazení do Azure
Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:

    git push azure master

Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.

Chcete-li zobrazit změny, přejděte na adresu URL Azure.

## <a name="troubleshooting---package-installation"></a>Řešení potíží – instalace balíčku
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Řešení potíží – virtuální prostředí
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Další kroky
Další informace o Flask a nástrojích Python Tools pro sadu Visual Studio na následujících odkazech: 

* [Dokumentace flask]
* [Dokumentace nástrojů Python Tools pro Visual Studio]

Informace o používání Azure Table Storage a MongoDB:

* [Flask a MongoDB v Azure s nástroji Python Tools pro sadu Visual Studio]
* [Flask a úložiště Azure Table v Azure s nástroji Python Tools pro sadu Visual Studio]

Další informace naleznete také [středisku pro vývojáře Python](/develop/python/).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

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
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentace flask]: http://flask.pocoo.org/ 

