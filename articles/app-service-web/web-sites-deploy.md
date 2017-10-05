---
title: "Nasazení aplikace do služby Azure App Service | Microsoft Docs"
description: "Informace o nasazení aplikace do služby Azure App Service."
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
ms.openlocfilehash: f41be4e00a9250b07ca260c2858e5fc45143f746
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-azure-app-service"></a>Nasazení aplikace do služby Azure App Service
Tento článek vám pomůže určit nejlepší možnost nasadit soubory pro vaši webovou aplikaci, back-end mobilní aplikace nebo aplikaci API [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)a provede vás odpovídající prostředky s pokyny, které jsou specifické pro upřednostňovanou možnost ověřování .

## <a name="overview"></a>Přehled nasazení služby Azure App Service
Aplikační služba Azure udržuje rozhraní pro vás (ASP.NET, PHP, Node.js atd.). Některé architektury jsou povolené ve výchozím nastavení při jiné, jako je Java a Python, může být nutné jednoduché zaškrtnutí konfiguraci, kterou chcete povolit. Kromě toho můžete přizpůsobit aplikační rozhraní, jako je například verze PHP nebo počtu bitů vaší runtime. Další informace najdete v tématu [konfigurace aplikace v Azure App Service](web-sites-configure.md).

Vzhledem k tomu, že nemáte si dělat starosti rozhraní webového serveru nebo aplikace, nasazení aplikace do služby App Service je řádu nasazení kódu, binární soubory, soubory obsahu a jejich odpovídajících adresářovou strukturu, do [ **/site/ Wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo **/lokality/wwwroot/App_Data/úlohy/** adresář pro webové úlohy). App Service podporuje tři procesy jiné nasazení. Všechny metody nasazení v tomto článku použijte jednu z následujících postupů: 

* [Protokol FTP nebo FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): použití vaše oblíbené protokol FTP nebo FTPS povolen nástroj pro přesun souborů do Azure, z [FileZilla](https://filezilla-project.org) k plné integrovaného vývojového prostředí jako [NetBeans](https://netbeans.org). Toto je výhradně procesu nahrávání souboru. Žádné další služby poskytované službou App Service, jako je například Správa verzí, Správa struktura souborů atd. 
* [Kudu (Git/Mercurial nebo OneDrive nebo Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Kudu je [modul nasazení](https://github.com/projectkudu/kudu/wiki) ve službě App Service. Nabízená kódu Kudu přímo z jakékoli úložiště. Kudu poskytuje přidané služeb vždy, když kód vložena do, včetně správy verzí, obnovení balíčků nástroje MSBuild, a [webové háky](https://github.com/projectkudu/kudu/wiki/Web-hooks) průběžné nasazování a Další automatizované úlohy. Modul nasazení Kudu podporuje 3 různé typy zdrojů nasazení:   
  
  * Synchronizace obsahu ze OneDrive nebo Dropbox   
  * Na základě úložiště průběžné nasazování s automatickou synchronizaci z Githubu, Bitbucket a Visual Studio Team Services  
  * Nasazení na základě úložiště s ruční synchronizaci z místní Git  
* [Nasazení webové](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): nasazení kódu do služby App Service přímo od Microsoftu. vaše oblíbené nástroje, jako je sadě Visual Studio pomocí stejné nástrojů, který automatizuje nasazení na servery služby IIS. Tento nástroj podporuje nasazení jenom rozdílové, vytvoření databáze, transformací připojovací řetězce, atd. Nasazení webu se liší od Kudu v tom, že binární soubory aplikace jsou postaveny před jejich nasazením do Azure. Podobně jako u FTP, žádné další služby poskytované službou App Service.

Nástroje pro vývoj oblíbených webových podporují jeden nebo více z těchto procesů nasazení. Zatímco nástroj, který zvolíte zjišťuje procesů nasazení, které můžete využít, funkce skutečného DevOps k dispozici závisí na kombinaci proces nasazení a konkrétní nástroje, které zvolíte. Například, pokud provádíte nasazení webu z [Visual Studio s Azure SDK](#vspros), i když automatizace Nezískávat z modulu Kudu, získat obnovení balíčků a MSBuild automatizace v sadě Visual Studio. 

> [!NOTE]
> Tyto procesy nasazení není ve skutečnosti [zřízení prostředků Azure](../azure-resource-manager/resource-group-template-deploy-portal.md) vyžadující vaší aplikace. Ale většinu propojené články s návody ukazují, jak zřídit aplikace a nasazení kódu do ní začátku do konce. Můžete také získat další možnosti pro zřizování prostředků v Azure [automatizovat nasazení pomocí nástroje příkazového řádku](#automate) části.
> 
> 

## <a name="ftp"></a>Nasadit ručně nahrávání souborů s FTP
Pokud se používají k ruční kopírování webovému obsahu na webový server, můžete použít [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) nástroj pro kopírování souborů, jako je například Průzkumník Windows nebo [FileZilla](https://filezilla-project.org/).

Specialisté ručně kopírování soubory jsou:

* Znalosti a minimální složitější nástrojů FTP. 
* Znalost, přesně kde se bude vaše soubory.
* Zvýšení zabezpečení s FTPS.

Nevýhody soubory kopírujete ručně, jsou:

* Museli vědět, jak nasadit soubory do správného adresáře ve službě App Service. 
* Správa verzí pro vrácení zpět, když dojde k selhání.
* Žádná historie předdefinované nasazení pro řešení potíží s nasazení.
* Potenciální dlouho nasazení případech, protože nemáte celou řadu nástrojů FTP poskytují pouze rozdílový kopírování a jednoduše zkopírujte všechny soubory.  

### <a name="howtoftp"></a>Postup nahrání souborů s FTP
[Portálu Azure](https://portal.azure.com) obsahuje všechny informace, které potřebujete připojení k adresářům vaší aplikace pomocí FTP a FTPS.

* [Nasazení aplikace do Azure App Service pomocí protokolu FTP](app-service-deploy-ftp.md)

## <a name="dropbox"></a>Nasadit synchronizovat se složkou cloudu
Dobrou alternativou k [soubory kopírujete ručně](#ftp) je synchronizovat soubory a složky do služby App Service z cloudové služby úložiště jako je OneDrive nebo Dropbox. Synchronizuje se složkou cloudu využívá proces Kudu pro nasazení (viz [přehled procesů nasazení](#overview)).

Specialisté synchronizace se složkou cloud jsou:

* Jednoduchost nasazení. Služby, jako je OneDrive nebo Dropbox mají klienti plochy synchronizace, tak místní pracovní adresář je také adresáře nasazení.
* Nasazení jedním kliknutím.
* Jsou k dispozici všechny funkce v modul nasazení Kudu (např. obnovení balíčků, automatizace).

Cons synchronizace se složkou cloud jsou:

* Správa verzí pro vrácení zpět, když dojde k selhání.
* Bez automatického nasazení je ruční synchronizaci.

### <a name="howtodropbox"></a>Nasazení na základě synchronizovat se složkou cloudu
V [portálu Azure](https://portal.azure.com), můžete určit složku pro synchronizaci obsahu v cloudovém úložišti OneDrive nebo Dropbox, pracovat kódu aplikace a obsah v této složce a kliknutím tlačítko Synchronizovat do služby App Service.

* [Synchronizace obsahu ze složky cloudu do služby Azure App Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Nasazení nepřetržitě ze služby založené na cloudu zdroj ovládacího prvku
Pokud váš vývojový tým používá služby pro správu (SCM) cloudové zdrojového kódu jako [Visual Studio Team Services](http://www.visualstudio.com/), [Githubu](https://www.github.com), nebo [BitBucket](https://bitbucket.org/), můžete nakonfigurovat aplikace Služba integrovat úložiště a průběžně nasazení. 

Specialisté nasazení ze služby Řízení cloudové zdroje jsou:

* Správa verzí povolit vrácení zpět.
* Umožňuje konfiguraci průběžné nasazování pro Git (a Mercurial, kde je to možné) úložiště. 
* Nasazení specifické pro firemní pobočky, můžete nasadit různé větví jiný [sloty](web-sites-staged-publishing.md).
* Jsou k dispozici všechny funkce v modul nasazení Kudu (např. nasazení správy verzí, vrácení zpět, obnovení balíčků, automatizace).

Con nasazení ze služby Řízení cloudové zdroje je:

* Některé znalosti o příslušných SCM služba vyžaduje.

### <a name="vsts"></a>Postup nasazení nepřetržitě ze služby založené na cloudu zdroj ovládacího prvku
V [portálu Azure](https://portal.azure.com), můžete nakonfigurovat průběžné nasazování z Githubu, Bitbucket a Visual Studio Team Services.

* [Nepřetržité nasazení do Azure App Service](app-service-continuous-deployment.md). 

Chcete zjistit, jak nakonfigurovat průběžné nasazování ručně z úložiště v cloudu nejsou uvedené pomocí portálu Azure (například [GitLab](https://gitlab.com/)), najdete v části [nastavení průběžné nasazování pomocí ruční kroky](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="localgitdeployment"></a>Nasazení z místní Git
Pokud váš vývojový tým používá místní místního zdroje kódu (SCM) služba management podle Git, to můžete nakonfigurovat jako zdroj nasazení do služby App Service. 

Odborníci na nasazení z místní Git jsou:

* Správa verzí povolit vrácení zpět.
* Nasazení specifické pro firemní pobočky, můžete nasadit různé větví jiný [sloty](web-sites-staged-publishing.md).
* Jsou k dispozici všechny funkce v modul nasazení Kudu (např. nasazení správy verzí, vrácení zpět, obnovení balíčků, automatizace).

Cons nasazení z místní Git je:

* Některé znalostní báze odpovídající SCM systému vyžaduje.
* Žádná řešení klíč pro průběžné nasazování. 

### <a name="vsts"></a>Postup nasazení z místní Git
V [portálu Azure](https://portal.azure.com), můžete nakonfigurovat místní nasazení Git.

* [Místní Git nasazení do Azure App Service](app-service-deploy-local-git.md). 
* [Publikování webových aplikací z jakékoli úložiště git/hg](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Nasazení pomocí rozhraní IDE
Pokud už používáte [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) s [Azure SDK](https://azure.microsoft.com/downloads/), nebo jako ostatní sady IDE [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org), a [ IntelliJ IDEA](https://www.jetbrains.com/idea/), můžete nasadit do Azure přímo z v rámci vašeho rozhraní IDE. Tato možnost je ideální pro jednotlivé developer.

Visual Studio podporuje všechny procesy tři nasazení (FTP, Git a nasazení webu), v závislosti na vaši volbu, zatímco jiné integrovaného vývojového prostředí můžete nasadit do služby App Service, pokud mají integrace FTP nebo Git (viz [přehled procesů nasazení](#overview)).

Specialisté nasazení pomocí rozhraní IDE jsou:

* Potenciálně minimalizujte nástrojů pro vaše životního cyklu aplikace začátku do konce. Vyvíjet, ladit, sledovat a nasazení aplikace do Azure všechna bez přesouvání mimo vaší IDE. 

Cons nasazení pomocí rozhraní IDE jsou:

* Přidání složitosti v nástrojů.
* Stále vyžaduje systému správy zdrojů pro týmový projekt.

<a name="vspros"></a>Další specialisté nasazení pomocí sady Visual Studio se sadou Azure SDK jsou:

* Azure SDK díky prvotřídní občanů prostředků Azure v sadě Visual Studio. Vytvořit, odstranit, upravit, spustit a zastavit aplikace, dotazu SQL databázi back-end, live ladění aplikace Azure a mnoho dalšího. 
* Živé úpravy souborů kódu v Azure.
* Živé ladění aplikací v Azure.
* Integrované Průzkumník Azure.
* Nasazení jenom rozdílové. 

### <a name="vs"></a>Jak nasadit přímo ze sady Visual Studio
* [Začínáme s Azure a ASP.NET](app-service-web-get-started-dotnet.md). Postup vytvoření a nasazení jednoduchého webového projektu ASP.NET MVC pomocí sady Visual Studio a nasazení webu.
* [Jak nasadit Azure WebJobs pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md). Postup konfigurace projektů konzolové aplikace, takže jejich nasazení jako webové úlohy.  
* [Nasazení webu ASP.NET pomocí sady Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Série kurz 12 část, která obsahuje podrobnější řadu úloh nasazení než ostatní uživatelé v tomto seznamu. Některé funkce Azure nasazení byly přidány od byla zapsána kurz, ale poznámky přidat později vysvětlit, co chybí.
* [Nasazení webu ASP.NET do Azure v sadě Visual Studio 2012 z úložiště Git přímo](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Vysvětluje, jak nasadit webový projekt ASP.NET v sadě Visual Studio, pomocí modulu plug-in Git potvrzení kód Git a připojování Azure do úložiště Git. Spouštění v sadě Visual Studio 2013, podporu Git je integrovaná a nevyžaduje instalaci modulu plug-in.

### <a name="aztk"></a>Postup nasazení pomocí sad nástrojů Azure pro Eclipse a IntelliJ IDEA
Microsoft umožňuje nasadit webové aplikace do Azure přímo z prostředí Eclipse a IntelliJ prostřednictvím [nástrojů Azure pro Eclipse](../azure-toolkit-for-eclipse.md) a [nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij.md). Následující kurzy popisují kroky, které jsou součástí nasazení jednoduché text "Hello" world webové aplikace do Azure pomocí buď IDE:

* [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse](app-service-web-eclipse-create-hello-world-web-app.md). V tomto kurzu se dozvíte, jak používat sady nástrojů Azure pro Eclipse k vytvoření a nasazení Hello World webovou aplikaci pro Azure.
* [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ](app-service-web-intellij-create-hello-world-web-app.md). V tomto kurzu se dozvíte, jak používat Azure nástrojů pro IntelliJ k vytvoření a nasazení Hello World webovou aplikaci pro Azure.

## <a name="automate"></a>Automatizovat nasazení pomocí nástroje příkazového řádku
Pokud dáváte přednost terminál příkazového řádku jako vývojové prostředí výběru, je pro vaši aplikaci služby App Service pomocí nástroje příkazového řádku skriptu úlohy nasazení. 

Odborníci na nasazení pomocí nástroje příkazového řádku jsou:

* Povolí skriptování scénáře nasazení.
* Integrate zřizování prostředků Azure a nasazení kódu.
* Nasazení Azure integrate do existující skripty nepřetržité integrace.

Cons nasazení pomocí nástroje příkazového řádku jsou:

* Není pro vývojáře, upřednostňují grafickým uživatelským rozhraním.

### <a name="automatehow"></a>Jak automatizovat nasazení pomocí nástroje příkazového řádku

V tématu [automatizovat nasazení vaší aplikace Azure pomocí nástroje příkazového řádku](app-service-deploy-command-line.md) seznam nástroje příkazového řádku a odkazy na výukové programy. 

## <a name="nextsteps"></a>Další kroky
V některých případech můžete chtít moci snadno přepínat mezi testovací a produkční verzi aplikace. Další informace najdete v tématu [připravený nasazení ve webových aplikacích](web-sites-staged-publishing.md).

S plánem zálohování a obnovení na místě, je důležitou součástí každého pracovního postupu nasazení. Informace o App Service zálohování a obnovení funkce, najdete v části [webových aplikací Zálohování](web-sites-backup.md).  

Informace o tom, jak používat řízení přístupu na základě Role Azure můžete spravovat přístup k nasazení služby App Service najdete v tématu [RBAC a webové aplikace publikování](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).

