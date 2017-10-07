---
title: "aaaDeploy tooAzure vaše aplikace služby App Service | Microsoft Docs"
description: "Zjistěte, jak toodeploy tooAzure vaše aplikace služby App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: f1464f71-2624-400e-86a2-e687e385804f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: 5c84e4ca502874209d750c94efeb86a59aa71a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service"></a>Nasazení vaší aplikace tooAzure služby App Service
Tento článek vám pomůže určit hello nejlepší možnost toodeploy hello soubory pro vaši webovou aplikaci, back-end mobilní aplikace nebo aplikace API příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)a provede vás tooappropriate prostředky s konkrétní tooyour pokyny upřednostňovanou možnost ověřování.

## <a name="overview"></a>Přehled nasazení služby Azure App Service
Aplikační služba Azure udržuje hello aplikační architektury pro vás (ASP.NET, PHP, Node.js atd.). Některé architektury jsou povolené ve výchozím nastavení při jiné, jako je Java a Python, může být nutné tooenable konfigurace jednoduché zaškrtnutí ho. Kromě toho můžete přizpůsobit aplikační rozhraní, například verze PHP hello nebo počtu bitů hello vaší runtime. Další informace najdete v tématu [konfigurace aplikace v Azure App Service](web-sites-configure.md).

Vzhledem k tomu, že nemáte tooworry o hello webového serveru nebo aplikaci framework, nasazení vaší aplikace tooApp služby je řádu nasazení kódu, binární soubory, soubory obsahu a jejich odpovídajících adresářovou strukturu, toohello [   **/Web /Wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo hello **/lokality/wwwroot/App_Data/úlohy/** adresář pro webové úlohy). App Service podporuje tři procesy jiné nasazení. Všechny metody nasazení hello v tomto článku použijte jednu z hello následující procesy: 

* [Protokol FTP nebo FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): vaše oblíbené protokol FTP nebo FTPS povolen nástroj toomove tooAzure vaše soubory díky [FileZilla](https://filezilla-project.org) vybavená toofull integrovaného vývojového prostředí, například [NetBeans](https://netbeans.org). Toto je výhradně procesu nahrávání souboru. Žádné další služby poskytované službou App Service, jako je například Správa verzí, Správa struktura souborů atd. 
* [Kudu (Git/Mercurial nebo OneDrive nebo Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Kudu je hello [modul nasazení](https://github.com/projectkudu/kudu/wiki) ve službě App Service. Push vaší tooKudu kód přímo z jakékoli úložiště. Kudu poskytuje přidané služeb vždy, když je kód poslat tooit, včetně správy verzí, obnovení balíčků nástroje MSBuild, a [webové háky](https://github.com/projectkudu/kudu/wiki/Web-hooks) průběžné nasazování a Další automatizované úlohy. modul nasazení Kudu Hello podporuje 3 různé typy zdrojů nasazení:   
  
  * Synchronizace obsahu ze OneDrive nebo Dropbox   
  * Na základě úložiště průběžné nasazování s automatickou synchronizaci z Githubu, Bitbucket a Visual Studio Team Services  
  * Nasazení na základě úložiště s ruční synchronizaci z místní Git  
* [Nasazení webové](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): tooApp kód nasazení služby přímo od Microsoftu. vaše oblíbené nástroje, jako je pomocí sady Visual Studio hello stejné nástrojů, který automatizuje nasazení tooIIS servery. Tento nástroj podporuje nasazení jenom rozdílové, vytvoření databáze, transformací připojovací řetězce, atd. V tomto nasazení aplikace, kterou binární soubory jsou vytvořeny předtím, než budou tooAzure, se liší od Kudu nasazení webu. Podobné tooFTP žádné další služby poskytované službou App Service.

Nástroje pro vývoj oblíbených webových podporují jeden nebo více z těchto procesů nasazení. Během hello nástroj, který zvolíte určuje hello procesů nasazení můžete využít, hello funkce skutečného DevOps k dispozici, závisí na kombinaci hello procesu nasazení hello a hello konkrétních nástrojů zvolíte. Například, pokud provádíte nasazení webu z [Visual Studio s Azure SDK](#vspros), i když automatizace Nezískávat z modulu Kudu, získat obnovení balíčků a MSBuild automatizace v sadě Visual Studio. 

> [!NOTE]
> Tyto procesy nasazení není ve skutečnosti [zřídit hello prostředků Azure](../azure-resource-manager/resource-group-template-deploy-portal.md) vyžadující vaší aplikace. Ale většinu hello propojené postupy tooarticles ukazují, jak tooprovision hello aplikace a nasazení váš kód tooit klient server. Můžete také získat další možnosti pro zřizování prostředků Azure v hello [automatizovat nasazení pomocí nástroje příkazového řádku](#automate) části.
> 
> 

## <a name="ftp"></a>Nasadit ručně nahrávání souborů s FTP
Pokud jste použité toomanually kopírování váš webový server webového obsahu tooa, můžete použít [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) nástroj toocopy soubory, jako je například Průzkumník Windows nebo [FileZilla](https://filezilla-project.org/).

Specialisté Hello ručně kopírování souborů jsou:

* Znalosti a minimální složitější nástrojů FTP. 
* Znalost, přesně kde se bude vaše soubory.
* Zvýšení zabezpečení s FTPS.

Nevýhody Hello soubory kopírujete ručně, jsou:

* S tooknow jak toodeploy soubory toohello správné adresářů ve službě App Service. 
* Správa verzí pro vrácení zpět, když dojde k selhání.
* Žádná historie předdefinované nasazení pro řešení potíží s nasazení.
* Potenciální dlouho nasazení případech, protože nemáte poskytují pouze rozdílový kopírování a jednoduše zkopírujte všechny soubory hello celou řadu nástrojů FTP.  

### <a name="howtoftp"></a>Jak tooupload soubory s FTP
Hello [portálu Azure](https://portal.azure.com) získáte všechny informace hello je nutné aplikaci tooyour tooconnect adresářů pomocí FTP a FTPS.

* [Nasazení vaší aplikace tooAzure služby App Service pomocí protokolu FTP](app-service-deploy-ftp.md)

## <a name="dropbox"></a>Nasadit synchronizovat se složkou cloudu
Dobrou alternativou příliš[soubory kopírujete ručně](#ftp) synchronizuje soubory a složky tooApp služby z cloudové služby úložiště jako je OneDrive nebo Dropbox. Synchronizuje se složkou cloudu využívá hello Kudu proces pro nasazení (viz [přehled procesů nasazení](#overview)).

Specialisté Hello synchronizace se složkou cloud jsou:

* Jednoduchost nasazení. Služby, jako je OneDrive nebo Dropbox mají klienti plochy synchronizace, tak místní pracovní adresář je také adresáře nasazení.
* Nasazení jedním kliknutím.
* Všechny funkce v modul nasazení Kudu hello je k dispozici (např. obnovení balíčků, automatizace).

cons Hello synchronizace se složkou cloudové jsou:

* Správa verzí pro vrácení zpět, když dojde k selhání.
* Bez automatického nasazení je ruční synchronizaci.

### <a name="howtodropbox"></a>Jak toodeploy podle synchronizovat se složkou cloudu
V hello [portálu Azure](https://portal.azure.com), lze určit složku pro synchronizaci obsahu v cloudovém úložišti OneDrive nebo Dropbox, pracovat s kódu aplikace a obsah v této složce a tooApp synchronizační službu s hello klikněte tlačítko.

* [Synchronizace obsahu ze složky tooAzure cloudové služby App Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Nasazení nepřetržitě ze služby založené na cloudu zdroj ovládacího prvku
Pokud váš vývojový tým používá služby pro správu (SCM) cloudové zdrojového kódu jako [Visual Studio Team Services](http://www.visualstudio.com/), [Githubu](https://www.github.com), nebo [BitBucket](https://bitbucket.org/), můžete nakonfigurovat aplikace Služba toointegrate s úložiště a nasaďte nepřetržitě. 

Specialisté Hello nasazení ze služby Řízení cloudové zdroje jsou:

* Verze řízení tooenable vrácení zpět.
* Možnost tooconfigure průběžné nasazování pro Git (a Mercurial, kde je to možné) úložiště. 
* Nasazení specifické pro firemní pobočky, můžete nasadit různé větví toodifferent [sloty](web-sites-staged-publishing.md).
* Všechny funkce v modul nasazení Kudu hello je k dispozici (např. nasazení správy verzí, vrácení zpět, obnovení balíčků, automatizace).

Hello con nasazení ze služby Řízení cloudové zdroje je:

* Některé znalost hello příslušných SCM služba vyžaduje.

### <a name="vsts"></a>Jak řídit toodeploy nepřetržitě ze zdroje cloudové služby
V hello [portálu Azure](https://portal.azure.com), můžete nakonfigurovat průběžné nasazování z Githubu, Bitbucket a Visual Studio Team Services.

* [Nepřetržité nasazení tooAzure služby App Service](app-service-continuous-deployment.md). 

toofind out jak hello tooconfigure průběžné nasazování ručně z úložiště v cloudu nejsou uvedené pomocí portálu Azure (například [GitLab](https://gitlab.com/)), najdete v části [nastavení průběžné nasazování pomocí ruční kroky](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="localgitdeployment"></a>Nasazení z místní Git
Pokud váš vývojový tým používá místní místního zdroje kódu (SCM) služba management podle Git, to můžete nakonfigurovat jako zdroj tooApp nasazení služby. 

Odborníci na nasazení z místní Git jsou:

* Verze řízení tooenable vrácení zpět.
* Nasazení specifické pro firemní pobočky, můžete nasadit různé větví toodifferent [sloty](web-sites-staged-publishing.md).
* Všechny funkce v modul nasazení Kudu hello je k dispozici (např. nasazení správy verzí, vrácení zpět, obnovení balíčků, automatizace).

Cons nasazení z místní Git je:

* Některé znalost hello příslušných SCM systém požadovaný.
* Žádná řešení klíč pro průběžné nasazování. 

### <a name="vsts"></a>Jak toodeploy z místní Git
V hello [portálu Azure](https://portal.azure.com), můžete nakonfigurovat místní nasazení Git.

* [Místní nasazení Git tooAzure služby App Service](app-service-deploy-local-git.md). 
* [Publikování aplikací tooWeb z jakékoli úložiště git/hg](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Nasazení pomocí rozhraní IDE
Pokud už používáte [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) s [Azure SDK](https://azure.microsoft.com/downloads/), nebo jako ostatní sady IDE [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org), a [ IntelliJ IDEA](https://www.jetbrains.com/idea/), tooAzure přímo z můžete nasadit v rámci vašeho rozhraní IDE. Tato možnost je ideální pro jednotlivé developer.

Visual Studio podporuje všechny procesy tři nasazení (FTP, Git a nasazení webu), v závislosti na vaši volbu, zatímco jiné integrovaného vývojového prostředí můžete nasadit tooApp služby, pokud mají integrace FTP nebo Git (viz [přehled procesů nasazení](#overview)).

Specialisté Hello nasazení pomocí rozhraní IDE jsou:

* Potenciálně minimalizujte hello nástrojů pro vaše životního cyklu aplikace začátku do konce. Vyvíjet, ladit, sledovat a nasazení vaší aplikace tooAzure všechny bez přesouvání mimo vaší IDE. 

cons Hello nasazení pomocí rozhraní IDE jsou:

* Přidání složitosti v nástrojů.
* Stále vyžaduje systému správy zdrojů pro týmový projekt.

<a name="vspros"></a>Další specialisté nasazení pomocí sady Visual Studio se sadou Azure SDK jsou:

* Azure SDK díky prvotřídní občanů prostředků Azure v sadě Visual Studio. Vytvořit, odstranit, upravit, spusťte a ukončete aplikace, back-end hello dotaz do databáze SQL, hello live ladění aplikace Azure a mnoho dalšího. 
* Živé úpravy souborů kódu v Azure.
* Živé ladění aplikací v Azure.
* Integrované Průzkumník Azure.
* Nasazení jenom rozdílové. 

### <a name="vs"></a>Jak toodeploy přímo ze sady Visual Studio
* [Začínáme s Azure a ASP.NET](app-service-web-get-started-dotnet.md). Jak toocreate a nasadit jednoduchý webový projekt ASP.NET MVC pomocí sady Visual Studio a nasazení webu.
* [Jak tooDeploy webové úlohy Azure pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md). Jak tooconfigure konzolové aplikace projekty tak, aby jejich nasazení jako webové úlohy.  
* [Nasazení webu ASP.NET pomocí sady Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Série kurz 12 část, která obsahuje podrobnější řadu úloh nasazení než ostatní hello v tomto seznamu. Některé funkce Azure nasazení byly přidány od byla zapsána hello kurzu, ale poznámky přidat později vysvětlit, co chybí.
* [Nasazení tooAzure webu ASP.NET v sadě Visual Studio 2012 z úložiště Git přímo](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Vysvětluje, jak toodeploy rozhraní ASP.NET web projektu v sadě Visual Studio, pomocí hello Git modulu plug-in toocommit hello kód tooGit a propojením úložiště Git Azure toohello. Spouštění v sadě Visual Studio 2013, podporu Git je integrovaná a nevyžaduje instalaci modulu plug-in.

### <a name="aztk"></a>Jak toodeploy pomocí hello Azure sadách pro Eclipse a IntelliJ IDEA
Společnost Microsoft neposkytuje možné toodeploy tooAzure webové aplikace přímo z prostředí Eclipse a IntelliJ prostřednictvím hello [nástrojů Azure pro Eclipse](../azure-toolkit-for-eclipse.md) a [nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij.md). Hello následující kurzy ilustruje hello kroky, které jsou součástí nasazení jednoduché text "Hello" world webové aplikace tooAzure pomocí buď IDE:

* [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse](app-service-web-eclipse-create-hello-world-web-app.md). Tento kurz ukazuje, jak toouse hello Azure Toolkit pro Eclipse toocreate a nasazení webové aplikace Hello World služby Azure.
* [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ](app-service-web-intellij-create-hello-world-web-app.md). Tento kurz ukazuje, jak toouse hello Azure Toolkit pro IntelliJ toocreate a nasazení webové aplikace Hello World služby Azure.

## <a name="automate"></a>Automatizovat nasazení pomocí nástroje příkazového řádku
Pokud dáváte přednost hello terminál příkazového řádku jako vývojové prostředí hello výběru, je pro vaši aplikaci služby App Service pomocí nástroje příkazového řádku skriptu úlohy nasazení. 

Odborníci na nasazení pomocí nástroje příkazového řádku jsou:

* Povolí skriptování scénáře nasazení.
* Integrate zřizování prostředků Azure a nasazení kódu.
* Nasazení Azure integrate do existující skripty nepřetržité integrace.

Cons nasazení pomocí nástroje příkazového řádku jsou:

* Není pro vývojáře, upřednostňují grafickým uživatelským rozhraním.

### <a name="automatehow"></a>Jak tooautomate nasazení pomocí nástroje příkazového řádku

V tématu [automatizovat nasazení vaší aplikace Azure pomocí nástroje příkazového řádku](app-service-deploy-command-line.md) seznam příkazového řádku tootutorials nástrojů a odkazy. 

## <a name="nextsteps"></a>Další kroky
V některých případech můžete chtít toobe možné tooeasily přepínat přepínat mezi testovací a produkční verzi aplikace. Další informace najdete v tématu [připravený nasazení ve webových aplikacích](web-sites-staged-publishing.md).

S plánem zálohování a obnovení na místě, je důležitou součástí každého pracovního postupu nasazení. Informace o hello služby App Service zálohování a obnovení funkce najdete v tématu [webových aplikací Zálohování](web-sites-backup.md).  

Informace o přístupu toomanage řízení přístupu na základě Role toouse Azure tooApp nasazení služby najdete v tématu [RBAC a webové aplikace publikování](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).

