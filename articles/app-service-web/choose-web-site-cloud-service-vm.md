---
title: "aaaAzure porovnání služby App Service, virtuální počítače, Service Fabric a Cloud Services | Microsoft Docs"
description: "Zjistěte, jak toochoose mezi Azure App Service, virtuální počítače, Service Fabric a Cloud Services pro hostování webových aplikací."
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7577332ed049df66178c7b2cd5c440a7f93a7865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Porovnání Azure App Service, virtuální počítače, Service Fabric a cloudové služby
## <a name="overview"></a>Přehled
Azure nabízí několik způsobů toohost webů: [Azure App Service][Azure App Service], [virtuální počítače][Virtual Machines], [Service Fabric ] [ Service Fabric], a [cloudových služeb][Cloud Services]. Tento článek vám pomůže pochopit hello možnosti a ujistěte se, hello správná volba pro vaši webovou aplikaci.

Aplikační služba Azure je hello nejlepší volbou pro většinu webové aplikace. Nasazení a správy jsou integrované do hello platformy, lokalit můžete škálovat rychle toohandle vysoké přenosové zatížení a Správce vyrovnávání a provoz integrované zatížení hello zajištění vysoké dostupnosti. Můžete přesunout existující lokality tooAzure služby App Service snadno s [nástroj pro migraci online](https://www.migratetoazure.net/), použít open-source aplikace z hello Galerie webových aplikací, nebo vytvořit nové lokality pomocí hello framework a nástrojů dle vašeho výběru. Hello [WebJobs] [ WebJobs] funkci umožňuje snadno tooadd pozadí úlohy zpracování tooyour webové aplikace App Service.

Service Fabric je vhodný, pokud vytváříte novou aplikaci nebo přepisovat existující aplikaci toouse architektury mikroslužby. Aplikace, které se spouštějí na sdílenému fondu počítačů, můžete začněte v malém rozsahu a zvýší toomassive škálování se stovkami nebo tisíci počítačů podle potřeby. Stavové služby, takže bude tooconsistently snadno a spolehlivě uložení stavu aplikace a Service Fabric automaticky spravuje služba rozdělení do oddílů, škálování a dostupnost za vás.  Service Fabric podporuje také WebAPI s Open Web Interface pro .NET (OWIN) a ASP.NET Core.  Porovnání tooApp služby, Service Fabric taky poskytuje další kontrolu nad nebo přímý přístup k podkladové infrastruktury hello. Můžete vzdálené do vašich serverů nebo můžete nakonfigurovat úlohy spuštění serveru. Cloudové služby je podobné tooService prostředků infrastruktury v nástroji stupeň kontroly a snadné použití, ale nyní je starší verze služby a Service Fabric se doporučuje pro nový vývoj.

Pokud máte existující aplikace, které by vyžadovaly toorun významné změny v App Service nebo Service Fabric, může zvolit virtuální počítače v toosimplify pořadí migrace toohello cloudu. Ale správně konfigurace, zabezpečení a údržbě virtuální počítače vyžaduje mnohem víc času a IT znalosti porovná tooAzure služby App Service a Service Fabric. Pokud uvažujete o virtuálních počítačích Azure, ujistěte se, brát v účtu hello průběžnou údržbu náročnost toopatch, aktualizovat a spravovat prostředí virtuálních počítačů. Virtuální počítače Azure je infrastruktury jako služba (IaaS), zatímco služba App Service a Service Fabric jsou platformy jako služba (Paas). 

## <a name="features"></a>Porovnání funkcí
Hello následující tabulka porovnává hello možnosti služby App Service, cloudové služby, virtuální počítače a Service Fabric toohelp provedete hello nejlepší volbou. Aktuální informace o hello SLA pro jednotlivé možnosti najdete v tématu [smlouvy o úrovni služeb Azure](https://azure.microsoft.com/support/legal/sla/).

| Funkce | Služby App Service (webové aplikace) | Cloudové služby (webové role) | Virtuální počítače | Service Fabric | Poznámky |
| --- | --- | --- | --- | --- | --- |
| Téměř rychlých nasazení |X | | |X |Nasazení aplikace nebo aplikaci aktualizace tooa cloudové služby nebo vytvoření virtuálního počítače, trvá několik minut minimálně; nasazení webové aplikace tooa aplikace trvá sekund. |
| Škálování toolarger počítače bez znovu ho zaveďte |X | | |X | |
| Instancemi webového serveru sdílet obsah a konfiguraci, což znamená, nemáte tooredeploy nebo překonfigurujte při změně měřítka. |X | | |X | |
| Prostředí s více nasazení (provozní prostředí a Fázování) |X |X | |X |Service Fabric vám umožní toohave několika prostředí pro aplikace nebo toodeploy různé verze vaší aplikace-souběžného. |
| Automatická správa aktualizací operačního systému |X |X | | |Automatické aktualizace operačního systému jsou plánované pro budoucí Service Fabric verzi. |
| Přepínání bezproblémové platformy (snadný přesun mezi 32bitová a 64bitová verze) |X |X | | | |
| Nasaďte kód prostřednictvím GIT, FTP |X | |X | | |
| Nasazení kódu pomocí nástroje nasazení webu |X | |X | |Cloudové služby podporuje hello instancí Web Deploy toodeploy aktualizace tooindividual role. Ale nelze ho použít pro počáteční nasazení role, a pokud použijete nasazení webu pro aktualizaci máte toodeploy samostatně tooeach instance role. Více instancí jsou potřeba v pořadí tooqualify pro hello cloudové služby SLA pro provozní prostředí. |
| Podpora služby WebMatrix |X | |X | | |
| Přístup k tooservices Service Bus, úložiště, databáze SQL |X |X |X |X | |
| Hostitele web nebo webovou vrstvu služby vícevrstvé architektury |X |X |X |X | |
| Střední vrstvy hostitele vícevrstvé architektury |X |X |X |X |Webové aplikace služby App Service můžete snadno hostitele REST API střední vrstvy a hello [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) funkce může hostovat úloh zpracování na pozadí. Webové úlohy můžete spustit vyhrazené webové stránce tooachieve nezávislé škálovatelnost pro vrstvu hello. Hello preview [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md) funkce poskytuje i další funkce pro hostování služby REST. |
| Integrovaná podpora MySQL jako služby |X |X |X | |Cloudové služby můžete integrovat MySQL jako službu prostřednictvím nabídky na ClearDB, ale ne jako součást pracovního postupu portálu Azure hello. |
| Podpora pro technologii ASP.NET, classic ASP, Node.js, PHP, Python |X |X |X |X |Service Fabric podporuje vytvoření hello front-endu webové pomocí [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) nebo jakéhokoli typu aplikace (Node.js, Java atd.) můžete nasadit jako [spustitelný soubor hosta](../service-fabric/service-fabric-deploy-existing-app.md). |
| Horizontální navýšení kapacity toomultiple instance bez znovu ho zaveďte |X |X |X |X |Virtuální počítače můžete škálovat instance toomultiple, ale hello služby spuštěné na nich musí být napsané toohandle tomto Škálováním na více systémů. Máte tooconfigure požadavky na tooroute nástroje pro vyrovnávání zatížení mezi hello počítačů a vytvořte na skupinu vztahů tooprevent souběžných restartování všech instancí z důvodu selhání toomaintenance nebo hardwaru. |
| Podpora pro protokol SSL |X |X |X |X |Pro webové aplikace služby App Service SSL u vlastních názvů domén je podporována pouze pro režim Basic a Standard. Informace o použití protokolu SSL s webovými aplikacemi najdete v tématu [konfigurace certifikát protokolu SSL pro web Azure](app-service-web-tutorial-custom-ssl.md). |
| Integrace aplikace Visual Studio |X |X |X |X | |
| Vzdálené ladění |X |X |X | | |
| Nasazení kódu do sady TFS |X |X |X |X | |
| Izolace pomocí sítě [Azure Virtual Network](/azure/virtual-network/) |X |X |X |X |Viz také [weby Azure virtuální sítě integrace](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Podpora pro [Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Monitorování integrované koncového bodu |X |X |X | | |
| Tooservers přístup ke vzdálené ploše | |X |X |X | |
| Nainstalujte všechny vlastní MSI | |X |X |X |Service Fabric vám umožní toohost jakéhokoliv spustitelného souboru, soubor jako [spustitelný soubor hosta](../service-fabric/service-fabric-deploy-existing-app.md) nebo můžete nainstalovat libovolnou aplikaci hello virtuálních počítačů. |
| Možnost toodefine, spouštění spuštění úlohy | |X |X |X | |
| Můžete naslouchání tooETW události | |X |X |X | |

## <a name="scenarios"></a>Scénáře a doporučení
Tady jsou některé běžné scénáře aplikace s doporučeními jako toowhich Azure webového hostingu možnost může být nejvhodnější pro jednotlivé.

* [Potřebuji front-end webové s zpracování na pozadí a databáze back-end toorun obchodní aplikace integrované s místními prostředky.](#onprem)
* [Potřebuji spolehlivý způsob toohost Moje podnikové web, který přizpůsobí také a poskytuje globální přístup.](#corp)
* [Je nutné aplikaci služby IIS 6 v systému Windows Server 2003.](#iis6)
* [Jsem vlastníka malé firmy a potřebný nenákladné způsob toohost mé lokality, ale s budoucí růst v paměti.](#smallbusiness)
* [Jsem web nebo grafický Návrhář a I toodesign a vytváření webů jménem zákazníka.](#designer)
* [I mě migraci mé vícevrstvou aplikaci s front-endu toohello webové cloudu.](#multitier)
* [Moje aplikace závisí na vysoce přizpůsobenou Windows nebo Linux prostředí a I chcete toomove ho toohello cloudu.](#custom)
* [My web používá software s otevřeným zdrojem, a chcete toohost ho v Azure.](#oss)
* [Je nutné-obchodní aplikace, která potřebuje tooconnect toohello podnikové síti.](#lob)
* [Chci toohost REST API nebo webové služby pro mobilních klientů.](#mobile)

### <a id="onprem"></a>Potřebuji front-end webové s zpracování na pozadí a databáze back-end toorun obchodní aplikace integrované s místními prostředky.
Azure App Service je vynikající řešení pro komplexní podnikové aplikace. Umožňuje vyvíjet aplikace, které automaticky škálovat na platformě vyrovnáváním zatížení, jsou zabezpečené službou Active Directory a připojení tooyour místních prostředků. Usnadňuje správu tyto aplikace snadno prostřednictvím špičkových portál a rozhraní API a umožňuje toogain přehled o tom, jak zákazníci používají je přehled nástroje pro aplikaci. Hello [Webjobs] [ Webjobs] funkce umožňuje spouštět procesy na pozadí a úkoly v rámci vaší webové vrstvy, při hybridní připojení a funkce virtuální sítě nastavit jej jako snadno tooconnect back tooon místní prostředky. Aplikační služba Azure poskytuje tři 9 SLA pro webové aplikace a umožňuje:

* Spolehlivě spuštění aplikací na cloudové platformy samoopravitelné, automatické opravy.
* Automaticky škálovat prostřednictvím globální sítě datových center.
* Zálohování a obnovení pro zotavení po havárii.
* Vyhovovat ISO, SOC2 a PCI.
* Integrace s Active Directory

### <a id="corp"></a>Potřebuji spolehlivý způsob toohost Moje podnikové web, který přizpůsobí také a poskytuje globální přístup.
Azure App Service je vynikající řešení pro hostování podnikové weby. Webové aplikace tooscale umožňuje rychle a snadno toomeet potřebují prostřednictvím globální sítě datových center. Nabízí místní reach, odolnost proti chybám a inteligentní provoz správy. Všechny na platformu, která poskytuje nástroje pro správu špičkových což vám toogain přehled o stavu lokality a provoz lokality snadno a rychle. Aplikační služba Azure poskytuje tři 9 SLA pro webové aplikace a umožňuje:

* Spolehlivě spuštění své weby na cloudové platformy samoopravitelné, automatické opravy.
* Automaticky škálovat prostřednictvím globální sítě datových center.
* Zálohování a obnovení pro zotavení po havárii.
* Správa protokolů a provozu pomocí integrovaných nástrojů.
* Vyhovovat ISO, SOC2 a PCI.
* Integrace s Active Directory

### <a id="iis6"></a>Je nutné aplikaci služby IIS 6 v systému Windows Server 2003.
Azure App Service umožňuje snadno tooavoid hello náklady na infrastrukturu přidružené migrace starší aplikací služby IIS 6. Společnost Microsoft vytvořila [snadno toouse nástrojů pro migraci a migrace podrobné pokyny](https://www.movemetowebsites.net/) , umožňují toocheck kompatibility a identifikovat změny, které je třeba toobe provedeny. Integrace s Visual Studio, sady TFS a běžné nástroje pro systém CMS umožňuje snadno toodeploy IIS6 aplikace přímo toohello cloudu. Po nasazení hello portál Azure poskytuje robustní správu nástroje, které umožňují tooscale dolů toomanage náklady a až toomeet vyžádání podle potřeby. Nástroj pro migraci hello můžete:

* Snadno a rychle migrujte starší verze systému Windows Server 2003 webové aplikace toohello cloudu.
* OPT tooleave vaše připojené SQL databáze místní toocreate hybridní aplikace.
* Automaticky přesunout databáze SQL společně s starší verzi aplikace.

### <a id="smallbusiness"></a>Jsem vlastníka malé firmy a potřebný nenákladné způsob toohost mé lokality, ale s budoucí růst v paměti.
Azure App Service je vynikající řešení pro tento scénář, protože můžete jej začít používat bezplatné a následně přidat další možnosti, když je potřebujete. Každý bezplatných webových aplikací se dodává s doménou poskytovaný platformou Azure (*your_company*. azurewebsites.net), a platforma hello obsahuje integrované nasazení a nástroje pro správu, jakož i galerii aplikací, aby byla snadno tooget spuštěna. Existuje mnoho dalších služeb a škálování možnosti, které umožňují hello lokality tooevolve uživatele zvýšené poptávky. Azure App Service můžete:

* Začínat hello úroveň free a pak škálovat podle potřeby.
* Použijte tooquickly galerii aplikací hello nastavení oblíbených webových aplikací, jako je WordPress.
* Přidejte další služby a funkce tooyour aplikaci Azure podle potřeby.
* Zabezpečení webové aplikace s protokolem HTTPS.

### <a id="designer"></a>Jsem web nebo grafický Návrhář a I toodesign a vytvářet weby jménem zákazníka
Pro webových vývojářů a návrhářů snadno se integruje s celou řadu architektur a nástroje Azure App Service, zahrnuje podporu nasazení Git a FTP a nabízí úzkou integraci s nástroje a služby, jako je například Visual Studio a SQL Database. App Service umožňuje:

* Pomocí nástroje příkazového řádku pro [automatizovaných úloh,][scripting].
* Práce s oblíbených jazyků, jako [.Net][dotnet], [PHP][PHP], [Node.js] [ nodejs], a [Python][Python].
* Vyberte tři různé úrovně škálování pro škálování nahoru toovery vysoké kapacity.
* Integrace s jinými službami Azure, jako například [SQL Database][sqldatabase], [Service Bus] [ servicebus] a [úložiště] [ Storage], nebo partnerů nabídky z hello [úložiště Azure][azurestore], jako jsou například MySQL a MongoDB.
* Integrate nástroje, jako je Visual Studio, Git, služba WebMatrix, Web Deploy, sady TFS a FTP.

### <a id="multitier"></a>I mě migraci mé vícevrstvou aplikaci s front-endu toohello webový Cloud
Pokud používáte vícevrstvé aplikace, například webový server, který připojí tooa databáze Azure App Service je vhodný, která nabízí úzkou integraci s Azure SQL Database. A hello WebJobs funkci můžete používat pro spuštěné procesy back-end.

Zvolte Service Fabric pro jednu nebo více vaší vrstev, pokud potřebujete více ovládat hello serveru prostředí, například hello možnost tooremote do vašeho serveru nebo konfigurace serveru spuštění úloh.

Vyberte virtuální počítače pro jeden nebo více vaší vrstev Pokud chcete, aby toouse vlastní image počítače nebo spuštěním serverového softwaru nebo služby, které nelze nakonfigurovat v Service Fabric.

### <a id="custom"></a>Moje aplikace závisí na vysoce přizpůsobenou Windows nebo Linux prostředí a I chcete toomove ho toohello cloudu.
Pokud vaše aplikace vyžaduje komplexní instalace nebo konfigurace softwaru a hello operačního systému, je nejlepším řešením hello pravděpodobně virtuálních počítačů. Virtuální počítače můžete:

* Použijte toostart Galerie hello virtuální počítač s operačním systémem, jako je například systému Windows nebo Linux a potom je upravit pro vaše požadavky aplikací.
* Vytvořit a nahrát vlastní image toorun serveru existující místní na virtuálním počítači v Azure.

### <a id="oss"></a>My web používá software s otevřeným zdrojem, a chcete toohost ho v Azure
Pokud vaše framework s otevřeným zdrojem je podporována v App Service, hello jazyků a rozhraní vyžaduje vaše aplikace jsou konfigurovány pro vás automaticky. Služby App Service umožňuje:

* Pomocí mnoha oblíbených open zdroj jazyků, jako například [.NET][dotnet], [PHP][PHP], [Node.js] [ nodejs], a [Python][Python].
* Nastavte WordPress, Drupal, Umbraco, DNN a mnoho dalších aplikací, webových třetích stran.
* Migrovat existující aplikaci nebo vytvořte novou z hello galerii aplikací.

Pokud vaše s otevřeným zdrojem framework není podporován v App Service, můžete spustit ji na jeden z hello jiné službě Azure web hostování možnosti. S virtuálními počítači, instalaci a konfiguraci bitové kopie počítače hello, což může být Windows hello softwaru nebo systémem Linux.

### <a id="lob"></a>Je nutné-obchodní aplikace, která potřebuje tooconnect toohello podnikové sítě
Pokud chcete toocreate-obchodní aplikace, může váš web vyžadovat tooservices přímý přístup nebo dat v podnikové síti hello. To je možné služby App Service, Service Fabric a virtuálních počítačů pomocí hello [služby Azure Virtual Network](/azure/virtual-network/). V App Service můžete hello [funkce integrace virtuální sítě](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), což umožňuje toorun vaše aplikace Azure, jako kdyby byly ve vaší podnikové síti.

### <a id="mobile"></a>Chci, aby toohost REST API nebo webové služby pro mobilní klienty
Založené na protokolu HTTP webové služby umožňují toosupport širokou škálu klientů, včetně mobilních klientů. Architektury, jako je ASP.NET Web API integrovat s Visual Studio toomake jednodušší toocreate a využívat služby REST.  Tyto služby jsou viditelné z koncový bod webové, tak, aby byl možný toouse žádné webových hostitelských technika na Azure toosupport tento scénář. Služby App Service je však je služba skvělou volbou pro hostování rozhraní REST API. App Service umožňuje:

* Rychle vytvořit [mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md) nebo [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md) toohost hello HTTP webové služby v jednom z globálně distribuované datových centrech Azure.
* Migrovat stávající služby nebo vytvořit nové.
* Dosáhnout SLA pro dostupnost s jednu instanci, nebo horizontální navýšení kapacity toomultiple vyhrazené počítače.
* Použití hello publikovat lokality tooprovide rozhraní REST API tooany klientů protokolu HTTP, včetně mobilních klientů.

> [!NOTE]
> Pokud chcete začít s Azure App Service před registrací účtu tooget, přejděte příliš<a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci v Azure App Service zdarma. Nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 

## <a id="nextsteps"></a>Další kroky
Další informace o hello tři webového hostingu možnosti najdete v tématu [představení Azure](../fundamentals-introduction-to-azure.md).

tooget začít s hello zvolené možnosti pro vaši aplikaci, najdete v části hello následující prostředky:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Virtuální počítače Azure](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
