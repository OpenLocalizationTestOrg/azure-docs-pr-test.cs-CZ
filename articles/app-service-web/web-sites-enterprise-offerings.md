---
title: "Nabídky aplikace webové služby Azure App Service pro podnik"
description: "Ukazuje, jak používat Azure App Service Web Apps vytvářet podnikové řešení webu pro vaši organizaci"
services: app-service\web
documentationcenter: 
author: apwestgarth
manager: erikre
editor: 
ms.assetid: cf9ac3b2-0493-4461-8b64-251d3a5cd5b5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: anwestg
ms.openlocfilehash: 4d46654f42a3fd5c9b491f1b565c2acfa0dc52c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-web-apps-offerings-for-enterprise-whitepaper"></a>Nabídky aplikace webové služby Azure App Service pro dokument White Paper Enterprise
Nutnost snížit náklady a poskytovat rychlejší IT řešeními v prostředí rychle vyvíjející se vytvoří nové výzvy pro vývojáře, odborníci v oblasti IT a správci. Uživatelé jsou stále hledá jejich obchodnímu systému (LOB) webových aplikací jako rychlý, reakce a dostupné z libovolného zařízení. Ve stejnou dobu firmách se pokoušíte využít vyšší produktivitu a efektivitu, který přichází z integrace s cloudu a mobilní služby, což může být v zařízeních pomocí služby Active Directory pro spolupráce v Office 365 pomocí dat je použito z interní obchodní aplikace, která zase vyžaduje data z implementace společnosti Salesforce něco stejně jednoduché jako jednotné přihlašování. [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) je cloudové služby podnikové třídy pro vývoj, testování a spouštění webových a mobilních aplikací, webovým rozhraním API a obecné weby. Může sloužit ke spuštění podnikové weby, weby v intranetu, obchodních aplikací a digitální marketingových kampaní na globální sítě datových center optimalizované pro škálování a dostupnosti, společně s podporou pro průběžnou integraci a postupů moderní DevOps.  

Tento dokument White Paper označuje možnosti [webové aplikace](/services/app-service/web/) služby speciálně zaměřuje na kterých běží obchodní webové aplikace, který po sobě zakrývá migrace stávajících webových aplikací a nasazení nové obchodní webové aplikace na platformě.

## <a name="audience"></a>Cílová skupina
Odborníci v oblasti IT, architekty a správce, kteří se zaměřujete na migraci do cloudu webové zátěží, které jsou aktuálně spuštěné na místě. Webové úlohy můžou sahat buď obchodní zaměstnanci nebo obchodní partnery webových aplikací.

## <a name="introduction"></a>Úvod
App Service Web Apps je představuje ideální platformu, na které se mají hostitele externí i interní webové aplikace a služby poskytuje nákladově efektivní, vysoce škálovatelné a spravované řešení, což vám umožní soustředit na obchodní hodnotu pro vaše uživatele doručování než výdaje poměrně velké množství času a peníze Údržba a podpora oddělte prostředí. Webové aplikace nabízí flexibilní platformu, na které chcete nasadit enterprise webových aplikací nabízí možnost pokračovat k ověřování na základě místní služby Active Directory prostřednictvím integrace s Microsoft Azure Active Directory, podporují nasazení snadno a rychle provedení použití vaše interní průběžnou integraci a nasazení postupy, při automatickém škálování růst s obchodním potřebám – všechny na spravované platformu, která vám umožní zaměřit se na vaše aplikace a není vaší infrastruktury.

## <a name="problem-definition"></a>Definice problému
Rychle mění povahu IT, se přesuňte směrem od hostuje na tradiční serverech s jejich vysoké kapitálové náklady na časy rozsáhlé na takový, který používá na vyžádání pomocí služeb, které automaticky škálovat pro zvládání zatížení. IT oddělení jsou požádána snížit náklady a náklady na nároky infrastruktury a údržby se zaměřením na snižuje KAPITÁLOVÉ a také zvýšení flexibility. Konci životnosti starší platforem infrastruktury, například Windows Server 2003, vznikají IT oddělení zkontrolovat migrace na cloud jako potenciální způsob, jak zabránit nové dlouhodobé kapitálové náklady. V minulosti ředitelé informačních technologií by rozhodnutí nákupu pro jiných oddělení; však stále CMOs a jiné obchodní jednotky oznámení trvá více active role v jak stráví jejich nároky a co je návratnost jejich investic. Stále firmách potřebovat jejím pracovníkům s zaměstnanci funguje vzdáleně, výdaje delší dobu se zákazníky, kteří potřebují přístup k systémy starosti být mnohem víc mobilních než kdy dřív.

Obchodní potřeby změnit každý měsíc, každý týden, každý den. Společnosti hledají rychlých globálním měřítku, s regulární aktualizované služby úplné nových funkcí, poskytuje třetí strany nebo interně.  V některých případech společnosti také hledají funkcí pro izolaci svých aplikací a přístup k prostředkům a přitom se taky provedení využívání zařízení veřejného cloudu. Uživatelé mají vyšší očekávání, s mnoha, které využijí služby ve své vlastní privátní život, jako je například Office 365. Očekávané tak, aby měl přístup ke službám bohaté funkce podobné, aktuální, v jejich pracovní dobu životnosti. Aby se zvládl tomuto požadavku IT musí služby vzhled pomohou firem až po povolení této prostřednictvím výběr a integraci s třetích stran, pečlivě výběr platformy, které můžete přizpůsobit potřebám firmy, a přitom se zároveň spolehlivé s celkové snížené náklady na vlastnictví.

K poskytování okamžitou obchodní výhody, doručování nové funkce na základě časté hledáte vývojové týmy. Hledají nákladově efektivní a spolehlivé platformy, která se integruje s existující nástroje a postupy – vývoj, testovací, vydání; a pracovní společně s IT oddělení automatizuje nasazení, správu a výstrahy, všechny s cílem nepřeruší.

<a name="highlevel"></a>
## <a name="high-level-solution"></a>Řešení na vysoké úrovni
Webové platformy a rozhraní se stále používá k vývoji, testování a hostování obchodních aplikací.  Typické řádek obchodní aplikace, například systém výdajů interní zaměstnance často sestávající výhradně z webové aplikace s zálohování databáze k uložení dat o připojení k aplikaci.

Služby App Service Web Apps je vhodný pro hostování takové aplikace nabídky škálovatelného a spolehlivého infrastruktury, která je spravovat a opravit s téměř nulové ruční zásah a výpadky. Platforma Microsoft Azure poskytuje řada možností úložiště dat pro podporu webové aplikace hostované na webových aplikací z Microsoft Azure SQL Database spravované škálovatelné relační databáze jako služby, pro oblíbené služby od našich partnerů, jako jsou databáze MySQL ClearDB a MongoDB.

Alternativní způsob je chcete-li použít existující investice místně. V ukázkovém scénáři systému náklady zaměstnance, můžete chtít zachovat data store v rámci vlastní interní infrastrukturu. To může být pro integraci s interní systémy (vytváření sestav, mzdy, fakturace atd.) nebo pro uspokojení požadavku na IT zásad správného řízení.  Služba Web Apps poskytuje několik metod, které vám umožní připojit se k místní infrastruktuře na povolení:

* [Prostředí App Service](app-service-app-service-environment-intro.md) -prostředí App Service (App Service Environment) jsou nové funkce Premium, která byla nedávno přidat k nabídce Microsoft Azure App Service.  ASEs poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém rozsahu a současně nabízí izolace a bezpečný přístup   
* [Hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) – hybridní připojení patří mezi funkce služby Microsoft Azure BizTalk Services a povolte webové aplikace pro připojení k místní prostředky bezpečně, například SQL Server, MySQL, webová rozhraní API a vlastní webové služby.
* [Integrace virtuální sítě](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) – integrace webové aplikace s virtuální sítí Azure umožňuje webovou aplikaci připojit k virtuální síti Azure, který pak může být připojen k na místní infrastruktuře prostřednictvím sítě site-to-site VPN.

Následující diagramy zobrazit v ní vysoké úrovně příklad řešení s možností připojení pro na místní prostředky.  První příklad ukazuje, jak toho lze dosáhnout pomocí standardní funkce služby Azure App Service a u druhé se zobrazí, jak to může být opírá premium nabídky prostředí App Service.

Pomocí funkce standardní aplikace služby:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions.png "Using Standard App Service Features")

Používání služby App Service Environment:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions-ASE.png "Using an App Service Environment")

## <a name="business-benefits"></a>Výhody pro firmy
Služby App Service Web Apps poskytuje hostitel obchodní výhody, které umožňují funkce být mnohem víc a nákladově efektivní doručování pro obchodní potřeby.

### <a name="paas-model"></a>PaaS Model
Služby App Service Web Apps je založen na platforma jako služba model, který poskytuje řadu úspory nákladů a efektivitu.  Už nemusí být tráví hodin Správa virtuálních počítačů, opravy, operační systémy a architektury. Webové aplikace je automaticky opravou prostředí, která umožňuje zaměřit se na správu webových aplikací a není virtuální počítače, a týmy možnost poskytovat další obchodní hodnotu.

PaaS Model podporu webových aplikací umožňuje profesionálové zabývající metodiky DevOps ke splnění svých cílů. Jako obchodní, to znamená úplné správy a integrace v rámci celý životní cyklus aplikace, včetně vývoj, testování, vydání, monitorování a správy a podpory.

Pro vývojové týmy průběžnou integraci a nasazení pracovní postupy lze konfigurovat v Visual Studio Team Services, GitHub, TeamCity, Hudsonem nebo BitBucket, povolení automatizované sestavení, testování a nasazení povolení rychlejší verze cyklů a současně snižuje třením součástí vydání ve stávající infrastruktuře. Webové aplikace také podporuje vytváření více testovacích a přípravných prostředí pro pracovní postup vydání, už je třeba rezervovat nebo přidělit hardwaru pro tyto účely, můžete vytvořit tolik prostředí, jak chcete a definovat vlastní povýšení k uvolnění pracovního postupu. Jako obchodní, které může rozhodnout, že verze slot testovací od správy zdrojového kódu, abyste provedli několik testů a po úspěšném dokončení povýšení pro fázi slot a nakonec Prohodit do produkčního prostředí bez výpadků s výhodu v podobě webové aplikace hostované na webové aplikace jsou předem načtena a za provozu a poskytuje prostředí nejlepší možný zákazníka.  Kromě toho firmy může využívat z testování v produkčním služby App Service Web Apps k přímé části přenosů do jiné patice, ověřit změny, před přepnutím veškerý provoz do nové nasazení nebo obnovení veškerý provoz do předchozí nasazení.

Operace týmy si být jisti, že jsou nejlepší možný pozici reagování na všechny problémy s žádným z jejich webové aplikace hostované na webové aplikace pomocí předdefinovaných funkcí monitorování a výstrahy. Měli oddělení jste už investovali do analýzy a řešení monitorování takové z Microsoft Visual Studio Application Insights, New Relic a AppDynamics. Tyto jsou také plně podporovány ve webových aplikacích povolení kontinuity a známých prostředích, ze kterého chcete monitorovat webové aplikace.

Nakonec Web Apps poskytuje funkce, které automaticky zálohovat vaší aplikací a hostované databází přesměrování na kontejner úložiště objektů Blob Azure. Poskytuje snadný způsob a velmi nákladově efektivní metodu, pomocí které k obnovení po havárii, přičemž redukuje nutnost pro komplexní na místní hardware a software.

### <a name="ease-of-migration"></a>Snadné migrace
Hardware Údržba a oběh je klíče problém pro firmy, protože urychlení verze cyklů pro hardware a operační systémy. Možná máte několik serverů Windows Server 2003 R2, které přicházejí na konec podpory v 2015, ale stále hostují klíče webové aplikace pro vaši organizaci? Služby App Service Web Apps je skvělým kandidátem na kterém k hostování těchto webových aplikací a abyste zefektivní majetku hardwaru firmy. Web Apps poskytuje přístup k řadu specifikace hardwaru, které jsou spravované a udržovat jako součást služby, takže není nutné zohlednit v nahrazování a náklady na správu v rámci vaší infrastruktury nároky.  Migrace může být stejně jednoduché jako kopii a operace z existující nasazení do webové aplikace nebo složitější migrace vložit, kde bude pomocí pomocníka webové aplikace migrace přidejte hodnotu. Migrované webové aplikace můžete sledovat úplné spektrum služby Azure, integraci další služby k webovým aplikacím. Zvažte například přidání k řízení přístupu k aplikaci na základě přidružení uživatelů na skupiny zabezpečení Azure Active Directory. Dalším příkladem může být přidání služby mezipaměti ke zlepšení výkonu a snižování latence, pokud celkový lepší uživatelské prostředí.

### <a name="enterprise-class-hosting"></a>Hostování Enterprise – třída
Služby App Service Web Apps poskytuje stabilní a spolehlivé platformy, která ověřené být schopna zpracovávat širokou škálu obchodní potřeby z malé interní úlohy vývoj a testování na vysoce škálovat intenzivní provoz weby. Pomocí webové aplikace provádíte použití stejné podnikové třídy hostující platforma Microsoft jako společnost používá pro webové úlohy vysoké hodnoty. Webové aplikace, společně s na platformu Azure, všechny služby jsou integrované s zabezpečení a dodržování předpisů s zákonné požadavky, jako je například ISO (ISO/IEC 27001: 2005); SOC1 a atestace podle SOC2 SSAE 16/ISAE 3402, HIPAA BAA, PCI a Fedramp jádro každý element a funkce, pro další informace najdete v tématu [http://aka.ms/azurecompliance](/support/trust-center/compliance/).

Platforma Microsoft Azure umožňuje Role na základě autorizace ovládací prvky povolení podnikové úrovně řízení k prostředkům v rámci webové aplikace. RBAC dává podniky implementovat vlastní zásady správy přístupu pro všechny svoje prostředky v daném prostředí Azure přiřazování uživatelů do skupiny a pak do těchto skupin proti asset například webovou aplikaci přiřazením požadovaná oprávnění. Další informace o RBAC v Azure najdete v tématu [http://aka.ms/azurerbac](../active-directory/role-based-access-control-configure.md). S využitím webové aplikace, může být v prostředí s bezpečném nasazených webových aplikací a máte plnou kontrolu, do které území jsou nasazeny vaše prostředky.

Prostředí Azure App Service [http://aka.ms/aseintro](http://aka.ms/aseintro) jsou nová možnost plán služeb premium pro podnikoví zákazníci chtějí využívat Azure App Service a tyto poskytuje plně izolovaném a vyhrazeném prostředí.  To umožňuje podnikovým zákazníkům umožňují nasadit aplikace, které můžete využít výhod velmi velký rozsah, a přitom se také s plnou kontrolu nad příchozí a odchozí síťový provoz a ASEs povolit aplikací mít vysokorychlostní zabezpečené připojení prostřednictvím virtuálních sítí a místních prostředků.

Služby App Service Web Apps jsou také moci plně využívat vaše na místní investice tím, že nabízí možnost připojit zpět k interním prostředkům, například datového skladu nebo prostředí služby SharePoint. Jak je popsáno v [řešení vysoké úrovně](#high-level-solution) můžete provést pomocí hybridních připojení a připojení k virtuální síti navázal spojení se na místní infrastrukturu a službách.

### <a name="global-scale"></a>Globální škálování
Služby App Service Web Apps je globální a škálovatelné platforma, povolení webových aplikací s růstem a přizpůsobit rostoucím potřebám rychle a s minimálními dlouhodobého hlediska plánování a náklady. V typických scénářů místní infrastruktury, rozšíření a zvyšování požadavků na geograficky a místně by vyžadovat velké množství správy, plánování a výdaje a zajišťují i spravovat další infrastrukturu. Webové aplikace nabízí schopnost škálování webových aplikací s ebb a toku vaše požadavky. Například pomocí aplikace výdaje jako příklad pro většinu měsíce jsou vaši uživatelé světla uživatelů aplikace, ale konečným termínem každý měsíc pro odesílání výdajů, které je třeba zadat a využití zvyšuje ve vaší aplikaci, webové aplikace má schopnost automaticky zřizovat další infrastrukturu pro vaši aplikaci a potom po použití má subsided znovu ji můžete škálovat zpět do infrastruktury standardních hodnot, které definujete.

Webové aplikace je k dispozici globálně v 24 datových centrech po celém světě a rostoucí. Nejaktuálnější seznam oblastí a umístění najdete v tématu [http://aka.ms/azlocations](http://aka.ms/azlocations). S webovými aplikacemi vaší firmy snadno dosáhnout globální sítě a škálování. S růstem vaší společnosti do nové oblasti vytváření sestav aplikace řídicí panely, které používáte a hostitele ve webových aplikacích můžete snadno nasadit do dalších datových center a obsluhovat místní uživatelé daleko rychleji pomocí kombinace webové aplikace a Azure Traffic Manager, všechny s výhodu v podobě škálovatelné infrastruktury pod schopnost smlouvy a rozbalte potřeby změny místní pobočky.

## <a name="solution-details"></a>Podrobnosti o řešení
Podívejme se na příklad scénáře migrace aplikací. To popisuje podrobnosti o tom, jak funkce App Service Web Apps se do společně poskytnout vynikající řešení a obchodní hodnotu.

V tomto příkladu je řádek obchodní aplikace, které se budeme zabývat vyúčtování aplikace, která umožňuje uživatelům odesílat své náklady pro náhradu. Aplikace je hostovaná na Windows Server 2003 R2 spuštění služby IIS 6 a databáze je databáze systému SQL Server 2005. Z důvodu vybereme možnost starším serveru leží s příchozí End ze služby pro Windows Server 2003 R2 a SQL Server 2005 a máme [nástroje](http://aka.ms/websitesmigration) a [pokyny](http://aka.ms/websitesmigrationresources) automaticky migraci úloh do Azure. Si uvědomit způsobem používaným v tomto příkladu se vztahují na celou važnost neshody scénářů migrace.

### <a name="migrate-existing-application"></a>Migrujte existující aplikace
Kroku jedna celkového řešení pro přesunutí-obchodní aplikace do webové aplikace je identifikace architektura a prostředky existující aplikace. V příkladu v tomto dokumentu je webová aplikace ASP.NET hostované na jednom serveru služby IIS s databází hostované na samostatném serveru SQL, jak je znázorněno na obrázku níže. Zaměstnanci přihlášení k systému pomocí kombinace uživatelského jména a hesla, zadejte podrobnosti o výdaje a nahrát naskenované kopie potvrzení, do databáze, pro každou položku náklady.

![](./media/web-sites-enterprise-offerings/on-premise-app-example.png)

#### <a name="items-to-consider"></a>Položky, které je třeba zvážit
Když aplikace migrace z místního prostředí, můžete chtít mějte na paměti několik omezení webové aplikace. Tady jsou některé klíčové témata znát při migraci webové aplikace do webové aplikace ([http://aka.ms/websitesmigrationresources](http://aka.ms/websitesmigrationresources)):

* Port vazby – podporuje Web Apps pouze port 80 pro protokol HTTP a port 443 pro komunikaci přes protokol HTTPS. Pokud vaše aplikace používá jiný port, můžete po migraci budou aplikace používat portu 80 pro protokol HTTP a port 443 pro komunikaci přes protokol HTTPS. To je často neškodné problém, protože to je běžné v místních nasazení, aby použití jiné porty překonat použití názvů domén, zejména v prostředích, vývoj a testování
* Ověřování – webové aplikace podporuje anonymní ověřování ve výchozím nastavení a ověřování pomocí formulářů určená aplikace. Webové aplikace můžete nabízet ověřování systému Windows, při aplikaci pouze integrované s Azure Active Directory a AD FS. Toto je funkce, která je podrobněji popsána [sem](http://aka.ms/azurebizapp)
* GAC na základě sestavení – webové aplikace neumožňuje nasazení sestavení do globální mezipaměti sestavení (GAC). Proto pokud migrované aplikace využívá této funkce na místě, zvažte přesunutí sestavení do složky bin aplikace.
* IIS5 Režimu kompatibility – Web Apps nepodporuje režimu kompatibility IIS5 a jako takový spusťte každou instanci webové aplikace a všech webových aplikací v rámci nadřazená instance webové aplikace v pracovním procesu, v rámci jeden fond aplikací.
* Použití knihovny COM – Web Apps neumožňuje registraci komponenty modelu COM na platformě. Proto aplikace upravuje použít u všech součástí modelu COM, ty by musela být přepsaná ve spravovaném kódu a nasazovat s aplikace.
* Filtry ISAPI – filtry ISAPI může být podporováno ve webových aplikacích. Budou muset být nasazen jako součást aplikace a zaregistroval v souboru web.config webové aplikace. Další informace najdete v tématu [http://aka.ms/azurewebsitesxdt](web-sites-transform-extend.md).

Jakmile tato témata byly zohledněny, musí být webové aplikace připravené pro Cloud. A nemusíte si dělat starosti Pokud některá témata nejsou splněny plně, nástroj pro migraci získáte usilovně k migraci.

Další kroky v procesu migrace se k vytvoření webové aplikace služby App Service a Azure SQL Database. Existuje více velikostí instancí webové aplikace s různými počet jader procesoru a objemy paměti RAM k dispozici pro vás, abyste vybrali založené na požadavku vaší webové aplikace. Další informace a ceny, najdete v části [https://azure.microsoft.com/pricing/details/app-service/](https://azure.microsoft.com/pricing/details/app-service/). Microsoft Azure SQL Database, je určen ke všem splnit požadavky obchodní musí s různými úrovně služeb a úrovně výkonu. Další informace najdete na [https://azure.microsoft.com/pricing/details/sql-database/](https://azure.microsoft.com/pricing/details/sql-database/). Po vytvoření aplikace se nahraje App Service Web Apps, buď prostřednictvím protokolu FTP nebo WebDeploy a poté přesuňte do databáze.

Do této migrace řešení používá Azure SQL Database, ale který je není pouze databáze, která je podporována v Azure. Společnosti můžete také využít MySQL, MongoDB, Azure Cosmos DB a mnoho dalších prostřednictvím rozšíření, které lze zakoupit v [úložiště Azure](/marketplace/partner-program/).

Při vytváření databáze SQL Azure řadu možností je možné importovat existující databázi z místního serveru z generování skriptu k použití existující databáze [datové vrstvy aplikace Export a Import](http://aka.ms/dacpac).

Databáze aplikace výdaje byl vytvořen tak, že vytvoříte novou databázi SQL Azure, připojení k databázi pomocí aplikace SQL Server Management Studio a pak spustit skript k sestavení schéma databáze a jeho naplnění dat z místní databázi.

V posledním kroku tento první fáze migrace tak musíte aktualizovat připojovací řetězce k databázi pro aplikaci. Toho lze dosáhnout prostřednictvím portálu Azure. Pro každou webovou aplikaci můžete upravit nastavení konkrétní aplikace, včetně všech připojovací řetězce, který používá aplikace pro připojení k jakékoli databázi používá.

### <a name="alternatives-to-using-azure-sql-database"></a>Alternativy k používání Azure SQL Database
Platformy Azure nabízí řadu alternativy k používání Azure SQL Database jako primární databáze webové aplikace, to je umožnit různé úlohy, tj. pomocí řešení NoSQL nebo povolit platformu podle potřeb obchodní data. Například firma může sdílet data, která nesmí být uložená mimo pracoviště nebo v prostředí veřejného cloudu a proto vypadat udržovat používání jejich místní databáze.

#### <a name="connectivity-to-on-premises-resources"></a>Připojení k na místní prostředky
Služby App Service Web Apps nabízí několik možností připojení na místní prostředky, jako jsou třeba databáze, povolení opakované použití existující infrastruktury vysokou hodnotu. Možnosti jsou, jak je uvedeno dále:

* Prostředí App Service jsou izolované a vytvořit v podsíti virtuální sítě, proto povolení prostředí ke komunikaci s privátní koncové body umístěné v rámci stejné virtuální síti - [http://aka.ms/appserviceasenetworking](http://aka.ms/appserviceasenetworking)
* Integrace virtuální sítě webové aplikace podporuje integraci mezi webovými aplikacemi a virtuální síť Azure, povolení přístupu k prostředkům, které jsou spuštěné v vaší virtuální síti, která, pokud je připojené k vaší místní sítí pomocí připojení site-to-site VPN a umožňuje připojení přímo na vaše systémy na místní.
* Hybridní připojení patří mezi funkce služby Azure BizTalk Services a poskytují snadný způsob, jak připojit k jednotlivé místní prostředkům, například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb.

#### <a name="scale-and-resiliency"></a>Škálování a odolnosti
S růstem obchodní pracovních sil, prostřednictvím akvizicích nebo přírodní organickým růstem, takže příliš musí škálování aplikace splňovat tyto požadavky na nový web. Je skutečně dnes běžně setkat ještě lepší šíření blízko týmy a vzdálení zaměstnanci, můžete tak třeba vynutit společnosti, pobočky v USA, Evropy a Asie, na základě mobilní prodejní v mnoha další teritoria. Webové aplikace má schopnost zpracovávat změny elastické škálování plynule a automaticky.

Služby App Service Web Apps umožňuje webové aplikace nakonfigurovat tak, aby škálování automaticky prostřednictvím portálu Azure, v závislosti na dva vektory – Naplánované časy nebo podle využití procesoru. Webové aplikace automatické škálování poskytuje velmi flexibilní a nákladově efektivní způsob, jak nahrazovat větší změny v využití pro všechny podnikové aplikace z webové aplikace, jako našich výdajů systém generování sestav pro marketingové webů, ke kterým dojde vysoké shluku provozu po krátkou dobu povýšení. Další informace a pokyny k škálování webových aplikací pomocí webových aplikací najdete v tématu [postup škálování weby](web-sites-scale.md).

Kromě škálování flexibilita webových aplikací celkový platforma umožňuje kontinuity podnikových procesů a odolnost prostřednictvím možné distribuce webových aplikací a jejich prostředky mezi různými datovými centry a zeměpisné oblasti.

## <a name="summary"></a>Souhrn
Služby App Service Web Apps nabízí flexibilní, nákladově efektivní a pohotově reagujících řešení dynamické potřebám firmy v prostředí s rychle se měnící. Webové aplikace pomáhá podnikům zvýšit produktivitu a efektivitu podle provádění použití spravovaná platforma s moderním možnostmi DevOps a snížené rukou na správu, při poskytování enterprise v škálování, odolnost, zabezpečení a integrace s místními prostředky.

## <a name="call-to-action"></a>Výzva k akci
Další informace o službě Azure App Service Web Apps, najdete v článku [http://aka.ms/enterprisewebsites](/services/websites/enterprise/) Další informace můžete použít jako zdroj kde přihlásit ke zkušební verzi dnes v [https://azure.microsoft.com/pricing/free-trial/](https://azure.microsoft.com/pricing/free-trial/) vyhodnotit službu a zjišťovat výhody pro vaši organizaci.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
