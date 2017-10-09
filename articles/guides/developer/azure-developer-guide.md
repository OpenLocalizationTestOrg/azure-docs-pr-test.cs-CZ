---
title: "aaaGet spuštění Průvodce pro vývojáře v Azure | Microsoft Docs"
description: "Toto téma obsahuje základní informace pro vývojáře vyhledávání tooget pomocí platformy Microsoft Azure hello pro potřeby jejich vývoj spuštěna."
services: 
cloud: 
documentationcenter: 
author: ggailey777
manager: erikre
ms.assetid: 
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: glenga
ms.openlocfilehash: 72dc2678db7738923d4bc7783e297fea6fcded83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-guide-for-azure-developers"></a>Úvodní příručka pro vývojáře v Azure

## <a name="what-is-azure"></a>Co je Azure?

Azure je kompletní cloud platforma, která může hostování existující aplikace, zjednodušit hello vývoji nových aplikací a i zvýšit místní aplikace. Azure integruje hello cloudové služby, je nutné, aby toodevelop, testování, nasadit a spravovat aplikace – s využitím hello efektivitu technologie cloud computing.

Hostováním aplikací v Azure, můžete začněte v malém rozsahu a snadného škálování aplikace s růstem vaší poptávku zákazníků. Azure nabízí také hello spolehlivost, který je potřeba pro vysokou dostupnost aplikace, i včetně převzetí služeb při selhání mezi různých oblastech. Hello [portál Azure](https://portal.azure.com) umožňuje snadno spravovat všechny vaše služby Azure. Můžete také spravovat vaše služby programově pomocí rozhraní API a šablony specifickou pro službu.

**Komu to určen**: Tento průvodce je úvod toohello platformy Azure pro vývojáře aplikací. Poskytuje pokyny a směru, je nutné, aby toostart vytváření nové aplikace v Azure nebo migrujete existující tooAzure aplikace.

## <a name="where-do-i-start"></a>Kde mám začít?

U všech služeb hello, které nabízí Azure může být složitý úkol toofigure na služby, které budete potřebovat toosupport vaší architektury řešení. Tato část označuje hello Azure services běžně používají vývojáři. Seznam všech služeb Azure, najdete v části hello [dokumentace k Azure](../../index.md).

Nejdřív musíte rozhodnout, jak toohost aplikaci v Azure. Potřebujete toomanage celé infrastruktury jako virtuální počítač (VM). Můžete použít hello platformy správy zařízení, které poskytuje Azure? Možná potřebovat provádění kódu toohost bez serveru framework pouze?

Aplikace musí cloudového úložiště, které Azure poskytuje několik možností. Můžete využít výhod ověřování Azure enterprise. Existují zde také nástrojů pro vývoj cloudové a monitorování a většina hostitelských služeb nabízejí integraci DevOps.

Nyní se podívejme se na některé z hello specifických služeb, které doporučujeme příčin pro vaše aplikace.

### <a name="application-hosting"></a>Hostování aplikací

Azure poskytuje několik toorun nabídky cloudové výpočetní tak, že nemáte tooworry o hello infrastruktury podrobnosti vaší aplikace. Můžete snadno škálovat nahoru i horizontální navýšení kapacity vašich prostředků s růstem využití vaší aplikace.

Azure nabízí služby, které podporují vaší aplikace vývoj a hostování potřebám. Azure poskytuje infrastrukturu jako služba (IaaS) toogive úplné řízení přes hostování aplikací. Azure platforma jako služba (PaaS) nabídky poskytují hello plně spravované služby, které potřebujete toopower vaší aplikace. Existuje i true bez serveru hostování v Azure kde potřebujete toodo je zápis kódu.

![Hostování možnosti Azure aplikace](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Když chcete hello nejrychlejší cestu toopublish webové projekty, zvažte Azure App Service. Služby App Service umožňuje snadno tooextend vaší webové aplikace toosupport mobilních klientů a publikování snadno spotřebované rozhraní REST API. Tato platforma poskytuje ověřování s využitím sociálních sítí, na základě provoz automatické škálování testování v produkčním a nepřetržitý a na základě kontejner nasazení.

Když vytvoříte aplikaci v App Service, vyberte jednu z hello následující typy:

- [Webové aplikace](../../app-service-web/app-service-web-overview.md): umožňuje hostování webů a webových aplikací, které jsou zapsány v rozhraní .NET, Java, PHP, Node.js a Python.

- [Mobilní aplikace](../../app-service-mobile/app-service-mobile-value-prop.md): rozšiřuje webové aplikace toosupport přístupu z mobilních zařízení. Umožňuje ověřování s sociálních sítí a Azure Active Directory (Azure AD), poskytuje úložiště back-end a integruje se službou [Azure Notification Hubs](../../notification-hubs/notification-hubs-push-notification-overview.md) pro nabízená oznámení.

- [Aplikace API](../../app-service-api/app-service-api-apps-why-best-platform.md): umožňuje více bezpečně vystavit vaše rozhraní API v cloudu hello s Swagger metadata tak, aby klienti můžete snadno je můžou využívat.

Vzhledem k tomu, že všechny třemi typy aplikací sdílení hello runtime služby App Service, hostí web, podporu mobilních klientů a vystavit vaše rozhraní API v Azure, všechny z hello stejné projekt nebo řešení. toolearn Další informace o App Service najdete v části [jak funguje App Service](../../app-service/app-service-how-works-readme.md).

Služby App Service má byly navrženy s DevOps v paměti. Podporuje různé nástroje pro publikování a nepřetržité integrace nasazení, včetně Githubu webhooků, volaných, Visual Studio Team Services, TeamCity a dalších.

Můžete migrovat existující aplikace tooApp služby pomocí hello [nástroj pro migraci online](https://www.migratetoazure.net/).

>**Když toouse**: použití App Service, když se migraci stávající tooAzure webové aplikace, a když potřebujete plně spravovaná hostující platforma pro webové aplikace. Také můžete službě App Service při potřebovat toosupport mobilních klientů nebo vystavit rozhraní REST API s vaší aplikací.

>**Začínáme**: služby App Service umožňuje snadno toocreate a nasadit první [webové aplikace](../../app-service-web/web-sites-dotnet-get-started.md), [mobilní aplikace](../../app-service-mobile/app-service-mobile-ios-get-started.md), nebo [aplikace API](../../app-service-api/app-service-api-dotnet-get-started.md).

>**Teď vyzkoušet**: App Service umožňuje zřídit platformu hello tootry krátkodobou aplikace bez nutnosti toosign pro účet Azure. Zkuste hello platformy a [vytvořit aplikaci aplikační služby Azure](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Jako poskytovatel infrastruktury jako služba (IaaS), Azure umožňuje nasadit tooor migrace vaší aplikace tooeither Windows nebo virtuální počítače s Linuxem. Společně s Azure Virtual Network podporuje virtuální počítače Azure hello nasazení systému Windows nebo virtuální počítače s Linuxem tooAzure. S virtuálními počítači máte celkový kontrolu nad hello konfigurace počítače hello. Pokud používáte virtuální počítače, jste zodpovědná za serveru instalaci, konfiguraci, údržby a operační systém opravy softwaru.

Z důvodu hello úroveň kontroly, zda máte s virtuálními počítači můžete spustit širokou škálu úloh serveru v Azure, které se nehodí do modelu PaaS. Mezi tyto úlohy patří databázových serverů, Windows Server Active Directory a Microsoft SharePoint. Další informace naleznete v dokumentaci k virtuálním počítačům hello pro buď [Linux](/azure/virtual-machines/linux/) nebo [Windows](/azure/virtual-machines/windows/).

>**Když toouse**: použití virtuálních počítačů, pokud chcete úplnou kontrolu nad vaší aplikace infrastrukturu nebo toomigrate místní aplikace úlohy tooAzure bez nutnosti změny toomake.

>**Začínáme**: vytvoření [virtuálního počítače s Linuxem](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) nebo [virtuální počítač s Windows](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) z hello portálu Azure.

#### <a name="azure-functions-serverless"></a>Azure Functions (bez serveru)

Spíše než z starosti vytvoření a správu celou aplikaci nebo hello infrastruktury toorun vašeho kódu. Co v případě může právě zadejte kód a nechat ji spouštět v odpovědi tooevents nebo podle plánu?  [Azure Functions](../../azure-functions/functions-overview.md) je a "bez serveru" – styl nabídky, že umožňuje psaní právě hello kód, který potřebujete. S funkcemi provádění kódu se aktivuje požadavků HTTP, webhooků, události cloudové služby, nebo podle plánu. Můžete kód v jazyce vývoj podle vlastní volby, například C\#, F\#, Node.js, Python nebo PHP. S fakturace na základě spotřeby platíte jenom za hello čas, která se spustí kódu a Azure škáluje podle potřeby.

>**Když toouse**: použití funkcí Azure, když máte kód, který je aktivován jinými službami Azure webové události, nebo podle plánu. Můžete také použít funkce při nebudete potřebovat hello režijní náklady na dokončený projekt hostované nebo pokud chcete pouze toopay hello dobu, kdy byl kód spuštěný. Další, najdete v části toolearn [přehled Azure Functions](../../azure-functions/functions-overview.md).

>**Začínáme**: postupujte podle hello funkce Rychlý úvodní kurz příliš[vytvoření první funkce](../../azure-functions/functions-create-first-azure-function.md) z portálu hello.

>**Teď vyzkoušet**: Azure Functions umožňuje svůj kód spustit bez nutnosti toosign pro účet Azure. Vyzkoušet nyní na a [vytvoření první funkce Azure](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric je platforma distribuovaných systémů, která umožňuje snadno toobuild balíčku, nasazovat a spravovat škálovatelného a spolehlivého mikroslužeb. Také poskytuje komplexní aplikace možnosti správy pro zřizování, nasazení, monitorování, upgrade nebo opravy a odstranění nasazené aplikace. Aplikace, které se spouštějí na sdílenému fondu počítačů, můžete začněte v malém rozsahu a škálování toohundreds nebo tisíce počítačů podle potřeby.

Service Fabric podporuje WebAPI s Open Web Interface pro .NET (OWIN) a ASP.NET Core. Poskytuje sady SDK pro vytváření služeb v systému Linux v .NET Core a Java. toolearn Další informace o Service Fabric, najdete v části hello [Service Fabric studijní](https://azure.microsoft.com/documentation/learning-paths/service-fabric/).

>**Když toouse:** Service Fabric je vhodné použít při vytváření aplikací nebo přepisování existující aplikace toouse architektury mikroslužby. Service Fabric použijte, pokud potřebujete další kontrolu nad nebo přímý přístup k podkladové infrastruktury hello.

>**Začínáme:** [vytvoření první aplikace Azure Service Fabric](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Vylepšení aplikace se službami Azure

Kromě tooapplication hostování, Azure poskytuje nabídky služeb, které můžete vylepšit hello funkce, vývoj a údržba vaší aplikace v hello cloudové i místní.

#### <a name="hosted-storage-and-data-access"></a>Hostované úložiště a přístup k datům

Většina aplikací musí ukládat data, bez ohledu na proto to jak rozhodnete toohost vaší aplikace v Azure, zvažte jednu nebo více hello následující služby úložiště a data.

-   **Azure SQL Database**: verzi služby Azure hello stroji Microsoft SQL Server pro ukládání relačních tabulková data v cloudu hello. SQL Database nabízí předvídatelný výkon, škálovatelnost bez výpadku, kontinuity podnikových procesů a ochranu dat.

    >**Když toouse**: Pokud vaše aplikace vyžaduje úložiště dat se referenční integrity, transakční podporu a podporu pro dotazy TSQL.

    >**Začínáme**: [vytvořit databázi SQL během několika minut pomocí portálu Azure hello](../../sql-database/sql-database-get-started.md).

-   **Úložiště Azure**: nabízí odolné, vysoce dostupné úložiště pro objekty BLOB, fronty, soubory a jiné druhy nonrelational data. Úložiště poskytuje hello základy úložiště pro virtuální počítače.

    >**Když toouse**: Pokud aplikace ukládá nonrelational dat, jako jsou páry klíč hodnota (tabulky), objekty BLOB, soubory sdílených složek nebo zpráv (fronty).

    >**Začínáme**: vyberte jednu z těchto typů úložiště: [objekty BLOB](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [tabulky](../../cosmos-db/table-storage-how-to-use-dotnet.md), [fronty](../../storage/queues/storage-dotnet-how-to-use-queues.md), nebo [soubory](../../storage/files/storage-dotnet-how-to-use-files.md).

-   **Azure DocumentDB**: plně spravované a škálovatelné NoSQL databáze služby, která funkce dotazů SQL přes data objektu. Dostanete DocumentDB pomocí existujících ovladačů pro MongoDB.
    >**Když toouse:** když aplikace potřebuje dotazy SQL možné tooexecute toobe přes dokumentů JSON, nebo pokud používáte MongoDB.

    >**Začínáme**: [sestavení C DocumentDB # konzolové aplikace](../../documentdb/documentdb-get-started.md). Pokud jste vývojář MongoDB, najdete v části [DocumentDB podporou protokolů pro MongoDB](../../documentdb/documentdb-protocol-mongodb.md).

Můžete použít [Azure Data Factory](../../data-factory/data-factory-introduction.md) toomove existující místní data tooAzure. Pokud nejsou připravené toomove toohello dat v cloudu, [hybridní připojení](../../biztalk-services/integration-hybrid-connection-overview.md) v BizTalk Services umožňuje připojení App Service hostované tooon místní prostředky aplikace. Můžete také připojit tooAzure dat služby a služby úložiště z místní aplikace.

#### <a name="docker-support"></a>Podpora docker

Docker kontejnery, formu virtualizace operačního systému, umožňuje nasadit aplikace způsobem efektivnější a předvídatelný. Kontejnerizované aplikace funguje v produkční hello stejně způsobem jako v systémech vývoj a testování. Kontejnery můžete spravovat pomocí nástrojů pro standardní Docker. Můžete pomocí existujících dovedností a toodeploy oblíbených open source nástroje a spravovat aplikace založené na kontejneru v Azure.

Azure nabízí několik způsobů toouse kontejnery ve svých aplikacích.

-   **Rozšíření Azure virtuální počítač Docker**: umožňuje nakonfigurovat tooact nástroje Docker jako Docker hostitele virtuálního počítače.

    >**Když toouse**: Pokud chcete nasazení toogenerate konzistentní kontejner pro vaše aplikace na virtuálním počítači, nebo když chcete toouse [Docker Compose](https://docs.docker.com/compose/overview/).

    >**Začínáme**: [vytvořit prostředí Docker v Azure pomocí rozšíření virtuálního počítače Docker hello](../../virtual-machines/virtual-machines-linux-dockerextension.md).

-   **Azure Container Service**: umožňuje vytvářet, konfigurovat a spravovat cluster virtuálních počítačů, které jsou předkonfigurované toorun kontejnerizované aplikace. toolearn Další informace o kontejneru služby, najdete v části [Azure Container Service ÚVOD](../../container-service/container-service-intro.md).

    >**Když toouse**: Pokud budete potřebovat toobuild produkční prostředí, škálovatelné prostředí, které poskytují další plánování a nástroje pro správu nebo když nasazujete clusteru Docker Swarm.

    >**Začínáme**: [nasadit cluster Container Service](../../container-service/dcos-swarm/container-service-deployment.md).

-   **Počítač docker**: umožňuje nainstalovat a spravovat pomocí příkazů počítač docker modulu Docker na virtuální hostitele.

    >**Když toouse**: když potřebujete tooquickly prototypu aplikace tak, že vytvoříte jeden hostitel Docker.

-   **Vlastní image Docker pro službu App Service**: umožňuje používat kontejnery Docker z registru kontejneru nebo kontejner zákazníka při nasazení webové aplikace v systému Linux.

    >**Když toouse**: při nasazení webové aplikace na bitovou kopii Docker tooa Linux.

    >**Začínáme**: [používat vlastní image Docker pro službu App Service v systému Linux](../../app-service-web/app-service-linux-using-custom-docker-image.md).

### <a name="authentication"></a>Authentication

Zásadní toonot pouze vědět, kdo používá vaše aplikace, ale i prostředky tooyour tooprevent neoprávněného přístupu. Azure poskytuje několik způsobů tooauthenticate vašim klientům aplikace.

-   **Azure Active Directory (Azure AD)**: hello víceklientských, cloudových identit a přístupu služba management společnosti Microsoft. Jednotné přihlašování (SSO) tooyour aplikací můžete přidat integrací s Azure AD. Vlastnosti directory můžete přistupovat pomocí hello Azure AD Graph API přímo nebo hello Microsoft Graph API. Můžete integrovat s podporou služby Azure AD pro hello OAuth2.0 autorizace framework a Open ID Connect pomocí nativní koncové body protokolu HTTP nebo REST a hello knihovny ověřování multiplatform Azure AD.

    >**Když toouse**: Pokud chcete tooprovide přihlašováním, pracovat s daty na základě grafu nebo ověřování založené na doméně uživatelů.

    >**Začínáme**: toolearn více, viz hello [Příručka pro vývojáře Azure Active Directory](../../active-directory/active-directory-developers-guide.md).

-   **Ověřování služby aplikace**: když zvolíte aplikace služby App Service toohost, získáte integrované ověření podpory pro Azure AD, společně s zprostředkovatelů identity sociálních – včetně Facebook, Google, Microsoft a Twitter.

    >**Když toouse**: Pokud chcete tooenable ověřování v aplikaci služby App Service pomocí Azure AD, zprostředkovatelů sociálních identity, nebo obojí.

    >**Začínáme**: toolearn Další informace o ověřování v App Service najdete v části [ověřování a autorizace ve službě Azure App Service](../../app-service/app-service-authentication-overview.md).

toolearn Další informace o doporučeném zabezpečení v Azure, najdete v části [osvědčené postupy zabezpečení Azure a vzory](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>Monitorování

Aplikace fungovaly v Azure, bude nutné toobe možné toomonitor výkon, podívejte se na problémy a v tématu jak zákazníci používají vaši aplikaci. Azure poskytuje několik možností monitorování.

-   **Visual Studio Application Insights**: hostované služby Azure rozšiřitelnou analytickou službu, který se integruje s Visual Studio toomonitor za chodu webových aplikací. Nabízí hello dat, je nutné, aby toocontinuously zlepšení hello výkonnost a použitelnost aplikací, zda jste hostované v Azure, nebo ne.

    >**Začínáme**: postupujte podle hello [Application Insights kurzu](../../application-insights/app-insights-overview.md).

-   **Azure monitorování**: služba, která vám pomůže toovisualize, dotaz, trasy, archivování a act na hello metriky a protokoly, které jsou generovány nástrojem infrastrukturu Azure a prostředky. Monitorování poskytuje hello zobrazení dat najdete v části v hello portál Azure a je jednoho zdroje pro monitorování prostředků Azure.
 
    >**Začínáme**: [Začínáme s Azure monitorování](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>Integrace DevOps

Jestli je zřizování virtuálních počítačů nebo publikování webových aplikacích s průběžnou integraci, Azure se integruje s většinou oblíbených nástrojů DevOps hello. S podporou pro nástroje, například volaných, Githubu, Puppet, Chef, TeamCity, Ansible, služby VSTS a dalších můžete pracovat s nástroji hello, že už máte a maximalizovat vaše stávající prostředí.

>**Teď vyzkoušet:** [vyzkoušet několik hello DevOps integrace](https://azure.microsoft.com/try/devops/).

>**Začínáme**: toosee DevOps možnosti pro aplikaci služby App Service najdete v části [tooAzure průběžné nasazování služby App Service](../../app-service-web/app-service-continuous-deployment.md).


## <a name="azure-regions"></a>Oblast Azure

Azure je globální Cloudová platforma, která je obecně k dispozici v mnoha oblastech kolem hello, world. Při zřizování služby, aplikace nebo virtuální počítač v Azure, budete vyzváni tooselect oblast, která představuje konkrétní datové centrum, kde běží aplikace nebo kde jsou data uložena. Tyto oblasti odpovídají toospecific umístění, které jsou publikovány na hello [oblastí Azure](https://azure.microsoft.com/regions/) stránky.

### <a name="choose-hello-best-region-for-your-application-and-data"></a>Zvolte hello nejlepší oblast pro vaše aplikace a data

Jednou z výhod hello pomocí Azure je nasazení vaší aplikace toovarious datových centrech kolem hello zeměkouli. Hello oblast, který zvolíte, může ovlivnit výkon hello vaší aplikace. Například je lepší toochoose oblasti, která je blíž toomost latence tooreduce vaši zákazníci v síťové požadavky. Můžete také chtít tooselect vaší oblasti toomeet hello zákonným požadavkům na distribuci aplikace v některých zemích. Je to vždy nejlepší data aplikací toostore postupem hello stejném datovém centru nebo v datovém centru jako blízké jako možné toohello datové centrum, který je hostitelem vaší aplikace.

### <a name="multi-region-apps"></a>Aplikace s více oblast

I když je nepravděpodobné, není znemožňuje, aby celého datového centra toogo offline z důvodu událost například přírodní katastrofě nebo selhání Internetu. Je nejvhodnější aplikace důležitá obchodní toohost postupů ve více než jeden datacenter tooprovide maximální dostupnosti. Použití více oblastí může také snížit latenci pro globální uživatele a poskytují další možnosti flexibilitu při aktualizaci aplikace.

Některé služby, jako je virtuální počítač a aplikační služby, použijte [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) tooenable více oblast podporu převzetí služeb při selhání mezi oblasti toosupport vysokou dostupnost podnikových aplikací. Příklad, naleznete v části [Azure referenční architektura: webové aplikace s vysokou dostupností](../../guidance/guidance-web-apps-multi-region.md).

>**Když toouse**: Pokud máte enterprise a vysoká dostupnost aplikací využívajících replikaci a převzetí služeb při selhání.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Jak spravovat mých aplikací a projektů?

Azure nabízí bohatou sadu možností pro vás toocreate a spravovat prostředky Azure, aplikací a projektů – prostřednictvím kódu programu i v hello [portál Azure](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>Rozhraní příkazového řádku a prostředí PowerShell

Azure poskytuje dva způsoby toomanage vašim aplikacím a službám z příkazového řádku hello pomocí Bash, terminálu, hello příkazového řádku nebo vaše příkazového řádku nástroje výběru. Obvykle můžete hello stejné úlohy provádět z příkazového řádku hello jako hello portál Azure – například vytváření a konfigurace virtuálních počítačů, virtuálních sítí, webové aplikace a dalším službám.

-   [Rozhraní příkazového řádku Azure (CLI)](../../xplat-cli-install.md): umožňuje připojit tooan předplatného Azure a program různé úlohy s prostředky Azure z příkazového řádku hello.

-   [Prostředí Azure PowerShell](../../powershell-install-configure.md): poskytuje sadu moduly rutin, které umožňují toomanage Azure prostředky pomocí prostředí Windows PowerShell.

### <a name="azure-portal"></a>portál Azure

Hello portál Azure je webová aplikace můžete použít toocreate, spravovat a odebrat prostředky Azure a služby. Hello portál Azure je umístěn v <https://portal.azure.com>. Ho zahrnují přizpůsobitelné řídicí panel, nástroje pro správu prostředků Azure a nastavení přístupu toosubscription a fakturační informace. Další informace najdete v tématu hello [přehled portálu Azure](../../azure-portal-overview.md).

### <a name="rest-apis"></a>Rozhraní REST API

Azure je založen na sadu rozhraní REST API, které podporují hello portál Azure uživatelského rozhraní. Většina těchto rozhraní REST API jsou taky podporované toolet prostřednictvím kódu programu zřizovat a spravovat prostředky Azure a aplikací z libovolného zařízení využívajících Internet. Hello kompletní sadu dokumentace k REST API, najdete v části hello [referenční informace sady Azure REST SDK](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>Rozhraní API

Kromě toho tooREST rozhraní API, řadou služeb Azure vám také umožní programově spravovat prostředky z vaší aplikace pomocí sady SDK specifické pro platformu Azure, včetně sady SDK pro hello následující vývojové platformy:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](http://azure.github.io/azure-sdk-for-node/)
-   [Java](https://docs.microsoft.com/java/api/)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)

Služby, jako [Mobile Apps](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) a [Azure Media Services](../../media-services/media-services-dotnet-how-to-use.md) zadejte klientské sady SDK toolet získat přístup ke službám z webových a mobilních klientských aplikací.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Spuštění aplikace v Azure pravděpodobně zahrnuje práci s více službami Azure, všechny nástroje, které následují hello stejný životní cyklus a si lze představit jako logickou jednotku. Webové aplikace může například použít webové aplikace, databáze SQL, úložiště, Azure Redis Cache a Azure Content Delivery Network services. [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) vám umožňuje pracovat s prostředky hello v aplikaci jako se skupinou. Můžete nasadit, aktualizovat nebo odstranit všechny prostředky hello v rámci jediné koordinované operace.

Kromě toho toologically seskupování a správu souvisejících prostředků, zahrnuje možnosti nasazení, které umožňují přizpůsobit hello nasazení a konfigurace souvisejících prostředků Azure Resource Manager. Například můžete pomocí Správce prostředků, nasadit a nakonfigurovat aplikaci, která se skládá z více virtuálních počítačů, nástroj pro vyrovnávání zatížení a Azure SQL database jako na jednu jednotku.

Tato nasazení můžete vyvíjet pomocí šablony Azure Resource Manager, který je dokument formátu JSON. Šablony umožňují definovat nasazení a Správa aplikací pomocí deklarativní šablony, nikoli skripty. Vaše šablony může fungovat v různých prostředích, jako je například testování, pracovní a provozní. Například pomocí šablony můžete přidat úložiště GitHub tooa tlačítko, která nasazuje hello kódu v sadě tooa úložišti hello služeb Azure s jedním kliknutím.

>**Když toouse**: hello pomocí Správce prostředků šablony, pokud chcete nasazení na základě šablon pro vaši aplikaci, kterou lze spravovat programově pomocí rozhraní REST API, rozhraní příkazového řádku Azure a prostředí Azure PowerShell.

>**Začínáme**: tooget spuštění pomocí šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Vysvětlení účtů, odběry a fakturace

Jako vývojáři jsme jako toodive přímo v kódu hello a zkuste tooget spustit co nejrychleji s provedení naše aplikace spustit. Určitě chceme tooencourage toostart jako snadno v Azure funguje. toohelp bylo nabízí snadný, Azure [bezplatnou zkušební verzi](https://azure.microsoft.com/free/). Některé služby i mít "Vyzkoušet zdarma" funkce, jako je třeba [Azure App Service](https://tryappservice.azure.com/), který nevyžaduje příliš i vytvořit účet. Jako fun je toodive do kódování a nasazení tooAzure vaše aplikace je také důležité tootake některé toounderstand čas, jak funguje Azure z hlediska uživatelské účty, odběry a fakturace.

### <a name="what-is-an-azure-account"></a>Co je Azure účet?

možnost toocreate toobe nebo pracovat s předplatné Azure, musí mít účet Azure. Účet Azure je jednoduše identity ve službě Azure AD nebo v adresáři, jako je například pracovní nebo školní organizace, která je důvěryhodná Azure AD. Pokud nemáte patří toosuch organizace, můžete vždy vytvořit odběr pomocí Account Microsoft, který je důvěryhodný pro Azure AD. toolearn Další informace o integraci místního systému Windows Server Active Directory s Azure AD, najdete v části [integrace místních identit s Azure Active Directory](../../active-directory/active-directory-aadconnect.md).

Každé předplatné služby Azure má vztah důvěryhodnosti s instancí služby Azure AD. To znamená, že tento adresář tooauthenticate uživatelů, služeb a zařízení. Několik předplatných může důvěřovat hello stejný adresář, ale předplatné důvěřuje pouze jednomu adresáři. Další, najdete v části toolearn [asociování předplatných Azure se službou Azure Active Directory](../../active-directory/active-directory-how-subscriptions-associated-directory.md).

Kromě toho toodefining identity jednotlivých účet Azure, označované taky jako *uživatelé*, můžete také definovat *skupiny* ve službě Azure AD. Vytváření skupin uživatelů je dobře toomanage přístup tooresources v předplatném pomocí řízení přístupu na základě role (RBAC). toolearn jak toocreate skupinách naleznete v tématu [vytvořte skupinu ve verzi preview služby Azure Active Directory](../../active-directory/active-directory-groups-create-azure-portal.md). Můžete také vytvořit a spravovat skupiny s [pomocí prostředí PowerShell](../../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Správa předplatných

Předplatné je logické jednotce služeb Azure, která je propojená tooan účet Azure. Každý přidružený účet má roli v předplatném. Fakturace služby Azure se provádí na základě za předplatné. Pro seznam nabídky k dispozici předplatného hello podle typu, najdete v části [Microsoft Azure nabízí podrobnosti](https://azure.microsoft.com/support/legal/offer-details/).

#### <a name="administrator-roles"></a>Role správce

Předplatné Azure má několik rolí správce účtu, které můžete kdykoli přiřadit.

-   **Správce účtu**: Tato role má plnou kontrolu nad hello předplatného a je hello účet, který je zodpovědný za fakturace.

-   **Správce služeb**: Tato role má kontrolu nad všechny hello služby v rámci předplatného hello. Ve výchozím nastavení, je to hello stejný účet jako hello správce účtu.

-   **Spolusprávcem**: Tato role má hello stejný přístup jako hello Správce služeb, s tím rozdílem, že ho nelze změnit přidružení hello tooan hello předplatné Azure directory.

najdete v části Další informace o rolích správce toolearn [jak tooadd nebo změna role Správce služby Azure](../../billing/billing-add-change-azure-subscription-administrator.md#add-an-admin-for-a-subscription).

#### <a name="resource-groups"></a>Skupiny prostředků

Při zřizování nových služeb Azure, můžete udělat v daném předplatném. Jednotlivé služby Azure, které se také nazývají prostředky, se vytvoří v kontextu hello skupiny prostředků. Skupiny prostředků bylo snazší toodeploy a spravovat prostředky aplikace. Skupiny prostředků by měl obsahovat všechny hello prostředky pro vaše aplikace, která chcete toowork s jako jednotku. Prostředky můžete přesouvat mezi skupinami prostředků a i toodifferent odběry. toolearn o přesun prostředků, najdete v části [přesunout skupiny prostředků toonew prostředků nebo předplatného](../../resource-group-move-resources.md).

Hello Průzkumníka prostředků Azure je skvělý nástroj pro vizualizaci hello prostředky, které jste již vytvořili v rámci vašeho předplatného. Další, najdete v části toolearn [tooview použití Průzkumníku prostředků Azure a úpravám prostředků](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-tooresources"></a>Udělení přístupu tooresources

Když povolíte přístup k prostředkům tooAzure, vždycky je osvědčeným postupem zajistit, že uživatelé s hello alespoň oprávnění to je požadovaná tooperform danou úlohu.

-   **Řízení přístupu na základě role (RBAC)**: V Azure, můžete udělit přístup toouser účty (objektů) v zadaném oboru: předplatné, skupinu prostředků nebo jednotlivé prostředky. RBAC umožňuje nasadit sadu prostředků do skupiny prostředků a udělte oprávnění tooa konkrétní uživatel nebo skupina. Je také umožňují omezit přístup k prostředkům hello tooonly, které patří toohello cílová skupina prostředků. Můžete také udělit přístup tooa jediný zdroj, například virtuální počítač nebo virtuální sítě. toogrant přístup, přiřadíte roli toohello uživatele, skupiny nebo objektu služby. Existuje mnoho předdefinovaných rolí, a můžete také definovat vlastní role.

    >**Když toouse**: když potřebujete vyladění správy přístupu pro uživatele a skupiny.

    >**Začínáme**: více, najdete v části toolearn [Začínáme se správou přístupu v hello portál Azure](../../active-directory/role-based-access-control-what-is.md).

-   **Hlavní objekty služeb**: kromě tooproviding přístup toouser objekty a skupiny, můžete udělit hello stejný přístup tooa instanční objekt.

    > **Když toouse**: když je prostřednictvím kódu programu správu prostředků Azure, nebo zda udělení přístupu pro aplikace. Další informace najdete v tématu [supplication vytvoření služby Active Directory a objektu zabezpečení](../../resource-group-create-service-principal-portal.md).

#### <a name="tags"></a>Značky

Azure Resource Manager umožňuje přiřazení vlastních značek tooindividual prostředky. Značky, které jsou páry klíč hodnota, může být užitečné, když potřebujete tooorganize prostředky pro fakturace nebo monitorování. Značky poskytují tootrack prostředky způsob, jak v několika skupinách prostředků. Můžete přiřadit značky hello portálu v hello šablony Azure Resource Manageru nebo programově pomocí hello REST API, hello rozhraní příkazového řádku Azure nebo PowerShell. Můžete přiřadit více značek tooeach prostředků. Další, najdete v části toolearn [pomocí značky tooorganize vašich prostředků Azure](../../resource-group-using-tags.md).

### <a name="billing"></a>Fakturace

V hello přesunout z výpočetních toocloud hostované služby v místě sledování a odhadnout využití služby a související náklady jsou důležité aspekty. Je možné tooestimate důležité toobe nové prostředky náklady toorun měsíčně. Je také nutné mít tooproject toobe, jak vypadá hello fakturace pro daný měsíc podle aktuální výdaje hello.

#### <a name="get-resource-usage-data"></a>Získat data použití prostředků

Azure poskytuje sadu fakturace REST API, která poskytnout přístup tooresource využívání a informace metadat pro předplatná Azure. Tyto fakturace rozhraní API udělte hello možnost toobetter předpovědi a spravovat Azure náklady. Můžete sledovat a analyzují výdaje rozdělený, vytvářet výstrahy, útraty a předpovídat budoucí fakturace na základě aktuální trendů využití.

>**Začínáme**: toolearn Další informace o použití hello fakturace rozhraní API, najdete v části [přehled využití fakturace Azure a rozhraní API RateCard](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Předpovídat budoucí náklady

I když je náročné náklady tooestimate dopředu, Azure má [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) můžete použít při odhadnout náklady na hello nasazené prostředky. Můžete také použít okno hello fakturace na portálu hello hello fakturace rozhraní REST API tooestimate budoucí náklady na základě aktuální spotřeby.

>**Začínáme**: najdete v části [přehled využití fakturace Azure a rozhraní API RateCard](../../billing-usage-rate-card-overview.md).

#### <a name="set-up-billing-alerts"></a>Nastavení upozornění fakturace

Poté, co nasadíte vaší aplikace nebo řešení v Azure, můžete vytvořit výstrahy, které odesílání že e-mailem při přístupu hello výdaje limity, které jsou definovány v hello výstrahy.

>**Začínáme**: více, najdete v části toolearn [nastavit výstrahy pro vaše předplatné Microsoft Azure billing](../../billing-set-up-alerts.md).
