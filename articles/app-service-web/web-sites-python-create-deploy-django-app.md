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
# <a name="creating-web-apps-with-django-in-azure"></a>Vytvoření webové aplikace pomocí rozhraní Django v Azure
Tento kurz popisuje, jak tooget spuštění Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python! Jak vaše aplikace bude rozšiřovat, můžete přepnout toopaid hostování a můžete také integrovat se všemi hello jinými službami Azure.

Vytvoříte aplikaci pomocí hello Django webová architektura (viz alternativní verze tohoto kurzu pro [Flask](web-sites-python-create-deploy-flask-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md)). Vytvoření webové aplikace hello hello Azure Marketplace, nastavíte nasazení Git a klonovat úložiště hello místně. Bude potom místní spuštění aplikace hello, proveďte změny, potvrzení a vložit je tooAzure. Hello kurzu se dozvíte, jak toodo to ze systému Windows nebo Mac/Linux.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
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
Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4]. Tím se nainstaluje hello 32bitovou verzi jazyka Python, setuptools, pip, virtualenv atd (32bitová verze jazyka Python je nainstalovaných v hello hostitelských počítačích Azure). Alternativně můžete získat jazyk Python z webu [python.org].

V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows]. Pokud používáte Visual Studio, můžete použít integrované hello podporu Git.

Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio]. Tato položka je nepovinná, ale pokud máte [Visual Studio], včetně hello volné Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, pak tato položka vám poskytne skvělé rozhraní IDE Python.

### <a name="maclinux"></a>Mac/Linux
Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.

## <a name="web-app-creation-on-portal"></a>Vytvoření webové aplikace v portálu
Hello prvním krokem při vytváření aplikace je toocreate hello webové aplikace pomocí hello [portálu Azure](https://portal.azure.com).

1. Přihlaste se k hello portálu Azure a klikněte na tlačítko hello **nový** tlačítko v levém dolním rohu hello.
2. Hello vyhledávacího pole zadejte "python".
3. Ve výsledcích hledání hello, vyberte **Django** (publikováno PTVS), pak klikněte na tlačítko **vytvořit**.
4. Nakonfigurujte hello novou aplikaci Django, jako je například vytváření nového plánu služby App Service a novou skupinu prostředků pro ni. Poté klikněte na možnost **Vytvořit**.
5. Nakonfigurujte publikování Git pro nově vytvořenou webovou aplikaci pomocí následujících pokynů hello [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Přehled aplikace
### <a name="git-repository-contents"></a>Obsah úložiště Git
Zde je uveden přehled hello soubory, které se nachází ve hello počátečním úložišti Git, které jsme klonovat v další části hello.

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

Hlavní zdroje pro aplikaci hello. Skládá se ze 3 stran (index, about, contact) s rozložením předlohy. Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.

    \manage.py

Serverová podpora místní správy a vývoje. Pomocí této aplikace hello toorun místně, synchronizaci databáze hello atd.

    \db.sqlite3

Výchozí databáze. Zahrnuje hello nezbytné tabulky pro toorun aplikace hello, ale neobsahuje žádné uživatele (synchronizovat hello databáze toocreate uživatele).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].

    \ptvs_virtualenv_proxy.py

Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.

    \requirements.txt

Externí balíčky vyžadované touto aplikací. skript nasazení Hello nástrojem pip instalaci hello balíčky uvedené v tomto souboru.

    \web.2.7.config
    \web.3.4.config

Konfigurační soubory služby IIS. Hello skript nasazení použije příslušný soubor web.x.y.config hello a zkopírujte ho jako soubor web.config.

### <a name="optional-files---customizing-deployment"></a>Volitelné soubory – přizpůsobení nasazení
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Volitelné soubory – modul Python runtime
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Další soubory na serveru
Některé soubory existují na serveru hello, ale nebyly přidány toohello úložiště git. Tyto soubory jsou vytvořeny skriptem nasazení hello.

    \web.config

Konfigurační soubor služby IIS. Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.

    \env\

Virtuální prostředí Python. Vytvoří se během nasazení, pokud ještě neexistuje kompatibilní virtuální prostředí na hello webové aplikace. Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, ale pip instalaci přeskočí, pokud hello balíčky jsou už nainstalované.

Hello následující 3 části popisují, jak tooproceed s hello vývoj webových aplikací ve 3 různých prostředích:

* Windows s nástroji Python Tools pro Visual Studio
* Windows s příkazovým řádkem
* Mac/Linux s příkazovým řádkem

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio
### <a name="clone-hello-repository"></a>Klon hello úložiště
Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello. Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).

Otevřete soubor řešení hello (.sln), který je součástí hello kořenovém hello úložiště.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Nyní vytvoříme virtuální prostředí pro místní vývoj. Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.

* Zkontrolujte, zda je název hello hello prostředí `env`.
* Vyberte základní překladač hello. Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello **nastavení aplikace** okně vaší webové aplikace v hello portál Azure).
* Zkontrolujte, že je zaškrtnuté hello možnost toodownload a nainstalovat balíčky.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Klikněte na možnost **Vytvořit**. Tato akce vytvoří hello virtuální prostředí a instalaci závislostí uvedených v souboru requirements.txt.

### <a name="create-a-superuser"></a>Vytvoření superuživatele
součástí aplikace hello databáze Hello nemá definován žádný superuživatel. V pořadí toouse hello funkce přihlašování v aplikaci hello nebo rozhraní správce Django hello (Pokud se rozhodnete tooenable ho), budete potřebovat toocreate superuživatele.

Spusťte z hello příkazového řádku ze složky projektu:

    env\scripts\python manage.py createsuperuser

Postupujte podle hello výzvy tooset hello uživatelské jméno, heslo atd.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Stisknutím klávesy F5 toostart ladění a webový prohlížeč se automaticky otevře s místně spuštěnou stránkou toohello.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Můžete nastavit zarážky v hello zdrojů, používání hello sledování systému windows, atd. V tématu hello [Python Tools pro Visual Studio dokumentaci] Další informace o hello různých funkcí.

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.

Jakmile změny otestujete, potvrďte je toohello úložiště Git:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.

Další balíčky můžete nainstalovat pomocí nástroje pip. tooinstall balíčku, klikněte pravým tlačítkem myši na virtuální prostředí hello a vyberte **instalovat balíček Python**.

Například tooinstall hello Azure SDK pro Python, která dává vám přístup tooAzure úložiště, služby service bus a dalším službám Azure, zadejte `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Klikněte pravým tlačítkem na virtuální prostředí hello a vyberte **generovat soubor requirements.txt** tooupdate requirements.txt.

Poté potvrďte úložiště Git toohello toorequirements.txt změny hello.

### <a name="deploy-tooazure"></a>Nasazení tooAzure
tootrigger nasazení, klikněte na **synchronizace** nebo **Push**. Synchronizace provádí vložení změn (push) i přijetí změn (pull).

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

Hello první nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.

Sada Visual Studio nezobrazuje průběh hello hello nasazení. Pokud chcete výstup hello tooreview, najdete v tématu hello na [řešení potíží – nasazení](#troubleshooting-deployment).

Procházejte toohello adresy URL Azure tooview změny.

## <a name="web-app-development---windows---command-line"></a>Vývoj webových aplikací – Windows – příkazový řádek
### <a name="clone-hello-repository"></a>Klon hello úložiště
Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené. Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště). Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.

Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello okně Nastavení aplikace webové aplikace v hello portál Azure).

Pro jazyk Python 2.7:

    c:\python27\python.exe -m virtualenv env

Pro jazyk Python 3.4:

    c:\python34\python.exe -m venv env

Nainstalujte veškeré případné externí balíčky požadované aplikací. Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Vytvoření superuživatele
součástí aplikace hello databáze Hello nemá definován žádný superuživatel. V pořadí toouse hello funkce přihlašování v aplikaci hello nebo rozhraní správce Django hello (Pokud se rozhodnete tooenable ho), budete potřebovat toocreate superuživatele.

Spusťte z hello příkazového řádku ze složky projektu:

    env\scripts\python manage.py createsuperuser

Postupujte podle hello výzvy tooset hello uživatelské jméno, heslo atd.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:

    env\scripts\python manage.py runserver

Hello konzola zobrazí adresa URL hello a port hello server naslouchá:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Potom otevřete adresu URL toothat webového prohlížeče.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.

Jakmile změny otestujete, potvrďte je toohello úložiště Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.

Další balíčky můžete nainstalovat pomocí nástroje pip. Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:

    env\scripts\pip install azure

Ujistěte se, že tooupdate requirements.txt:

    env\scripts\pip freeze > requirements.txt

Potvrzení změn hello:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a>Nasazení tooAzure
tootrigger nasazení nabízené hello změní tooAzure:

    git push azure master

Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.

Procházejte toohello adresy URL Azure tooview změny.

## <a name="web-app-development---maclinux---command-line"></a>Vývoj webových aplikací – Mac/Linux – příkazový řádek
### <a name="clone-hello-repository"></a>Klon hello úložiště
Nejprve naklonujte úložiště hello pomocí hello adresy URL poskytnuté na portálu Azure hello a přidat hello úložiště Azure jako vzdálené. Další informace najdete v tématu [místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej toohello úložiště). Virtuální prostředí v Python nejsou přemístitelná, takže každý vývojář pracující na aplikaci hello vytvoří vlastní místně.

Zajistěte, aby toouse hello stejnou verzi jazyka Python, který je vybraný pro vaši webovou aplikaci (v souboru runtime.txt nebo hello okně Nastavení aplikace webové aplikace v hello portál Azure).

Pro jazyk Python 2.7:

    python -m virtualenv env

Pro jazyk Python 3.4:

    python -m venv env

nebo

    pyvenv env

Nainstalujte veškeré případné externí balíčky požadované aplikací. Můžete použít soubor requirements.txt hello na nejnižší hello hello úložiště tooinstall hello balíčků ve virtuálním prostředí:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Vytvoření superuživatele
součástí aplikace hello databáze Hello nemá definován žádný superuživatel. V pořadí toouse hello funkce přihlašování v aplikaci hello nebo rozhraní správce Django hello (Pokud se rozhodnete tooenable ho), budete potřebovat toocreate superuživatele.

Spusťte z hello příkazového řádku ze složky projektu:

    env/bin/python manage.py createsuperuser

Postupujte podle hello výzvy tooset hello uživatelské jméno, heslo atd.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Hello aplikace v rámci vývojového serveru můžete spustit s hello následující příkaz:

    env/bin/python manage.py runserver

Hello konzola zobrazí adresa URL hello a port hello server naslouchá:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Potom otevřete adresu URL toothat webového prohlížeče.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že změny toohello aplikace zdrojů a/nebo šablon.

Jakmile změny otestujete, potvrďte je toohello úložiště Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.

Další balíčky můžete nainstalovat pomocí nástroje pip. Například tooinstall hello Azure SDK pro Python, která umožňuje přístup k tooAzure úložiště, služby service bus a dalším službám Azure, zadejte:

    env/bin/pip install azure

Ujistěte se, že tooupdate requirements.txt:

    env/bin/pip freeze > requirements.txt

Potvrzení změn hello:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a>Nasazení tooAzure
tootrigger nasazení nabízené hello změní tooAzure:

    git push azure master

Zobrazí se výstup hello hello skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.

Procházejte toohello adresy URL Azure tooview změny.

## <a name="troubleshooting---package-installation"></a>Řešení potíží – instalace balíčku
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Řešení potíží – virtuální prostředí
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a>Řešení potíží – statické soubory
Rozhraní Django obsahuje koncepci shromažďování statických souborů hello. To trvá všechny hello statické soubory z původního umístění a zkopíruje je tooa jedna složka. Pro tuto aplikaci se kopírují příliš`/static`.

Tato akce se provádí proto, že statické soubory mohou pocházet z různých „aplikací“ Django. Například hello statické soubory z rozhraní správce Django hello jsou umístěny v podsložce knihovny Django ve virtuálním prostředí hello. Statické soubory definované touto aplikací jsou umístěny ve složce `/app/static`. Při používání vícero „aplikací“ Django se statické soubory nacházejí na více místech.

Při spuštění aplikace hello v režimu ladění, slouží hello aplikace hello statické soubory z původního umístění.

Při spuštění v režimu vydání aplikace hello, nemá aplikace hello **není** obsluhovat statické soubory hello. Je zodpovědností hello hello webový server tooserve hello soubory. Pro tuto aplikaci, služba IIS bude poskytovat hello statické soubory z `/static`.

Hello shromažďování statických souborů je prováděno automaticky v rámci hello skriptu nasazení, přičemž dříve shromážděné soubory. To znamená hello shromažďování probíhá při každém nasazení zpomalení nasazení trochu, ale zajišťuje, že zastaralé soubory nebudete mít k dispozici, zabraňující potenciálním potížím se zabezpečením.

Pokud chcete u aplikace rozhraní Django tooskip shromažďování statických souborů:

    \.skipDjango

Potom budete potřebovat toodo hello kolekce ručně na místním počítači:

    env\scripts\python manage.py collectstatic

Pak odeberte hello `\static` složky z `.gitignore` a přidejte ji toohello úložiště Git.

## <a name="troubleshooting---settings"></a>Řešení potíží – nastavení
Různá nastavení pro aplikace hello lze změnit v `DjangoWebProject/settings.py`.

Z důvodu usnadnění práce vývojářů je povolen režim ladění. Jeden dobrý vedlejším účinkem této je, že budete mít toosee obrázky a další statický obsah při spuštění místně, bez nutnosti toocollect statické soubory.

režim ladění toodisable:

    DEBUG = False

Pokud je zakázáno ladění, hello hodnotu `ALLOWED_HOSTS` toobe potřeby aktualizovat název hostitele Azure tooinclude hello. Například:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

nebo tooenable žádné:

    ALLOWED_HOSTS = (
        '*',
    )

V praxi může být vhodné toodo něco složitější toodeal s přepínání mezi ladění a vydání režimu a získávání názvu hostitele hello.

Můžete nastavit proměnné prostředí prostřednictvím portálu Azure hello **konfigurace** stránku hello **nastavení aplikace** části.  To může být užitečné pro nastavení hodnot, nemusí chcete tooappear v hello zdrojích (připojovací řetězce, hesla atd.), nebo zda chcete tooset jinak mezi Azure a v místním počítači. V `settings.py`, se můžete dotazovat pomocí proměnné prostředí hello `os.getenv`.

## <a name="using-a-database"></a>Používání databáze
Hello databáze, která je součástí aplikace hello je databáze sqlite. Toto je toouse praktickou a užitečnou výchozí databázi pro vývoj, protože nevyžaduje téměř žádné nastavení. Hello databáze je uložena v souboru db.sqlite3 hello ve složce projektu hello.

Azure poskytuje databázové služby, které jsou snadno toouse z aplikace Django. Kurzy zaměřené na používání [SQL Database] a [MySQL] z aplikace Django zobrazení hello kroky nezbytné toocreate hello databáze služby, změnit nastavení databáze hello v `DjangoWebProject/settings.py`a hello požadované tooinstall knihovny.

Samozřejmě pokud dáváte přednost toomanage své vlastní databázové servery, můžete provést tak pomocí systému Windows nebo Linux virtuální počítače běžící v Azure.

## <a name="django-admin-interface"></a>Rozhraní správce Django
Jakmile začnete vytvářet modely, budete muset toopopulate hello databáze s některá data. Snadný způsob toodo přidávat a upravovat obsah interaktivně je rozhraní pro správu Django toouse hello.

Hello kód pro rozhraní správce hello je označeno jako komentář ve zdrojích aplikace hello, ale je přehledně označen tak je možné snadno povolit (vyhledejte "admin").

Když je tato funkce povolená, synchronizovat hello databáze, spusťte aplikaci hello a přejděte příliš`/admin`.

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o rozhraní Django a nástrojích Python Tools pro sadu Visual Studio:

* [Dokumentace rozhraní Django]
* [Python Tools pro Visual Studio dokumentaci]

Informace týkající se použití databáze SQL Database a MySQL naleznete v tématech:

* [Django a MySQL v Azure s nástroji Python Tools pro Visual Studio]
* [Django a SQL Database v Azure s nástroji Python Tools pro Visual Studio]

Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

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
