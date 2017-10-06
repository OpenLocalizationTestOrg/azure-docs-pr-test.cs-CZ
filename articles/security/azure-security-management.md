---
title: "zabezpečení vzdálené správy aaaEnhance v Azure | Microsoft Docs"
description: "Tento článek popisuje kroky pro zlepšení zabezpečení vzdálené správy při správě prostředí Microsoft Azure včetně cloudových služeb, služby Virtual Machines a vlastních aplikací."
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 2431feba-3364-4a63-8e66-858926061dd3
ms.service: security
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 9262cfb98bfe51d15fbad8f18997c4573668d9ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-management-in-azure"></a>Správa zabezpečení v Azure
Předplatitelé služby Azure mohou svoje cloudová prostředí spravovat z více zařízení. Můžou k tomu využívat pracovní stanice, počítače vývojářů a dokonce i privilegovaná zařízení koncových uživatelů, která mají oprávnění ke konkrétním úlohám. V některých případech se funkce správy provádějí prostřednictvím webových konzol, například hello [portál Azure](https://azure.microsoft.com/features/azure-portal/). V jiných případech může být tooAzure přímé připojení z místních systémů prostřednictvím virtuálních privátních sítí (VPN), terminálových služeb, protokolů klientských aplikací nebo (prostřednictvím kódu programu) hello rozhraní API pro správu služby Azure (SMAPI). Kromě toho můžou být koncové body klienta buď připojené k doménám nebo izolované a nespravované, jako například tablety nebo smartphony.

I když více možností přístup a správu nabízejí bohatou sadu možností, může tato variabilita zvýšit významné riziko tooa cloudu nasazení. Může být obtížné toomanage, sledování a audit akcí správy. Tato variabilita může také zvýšit ohrožení bezpečnosti prostřednictvím neregulovaného přístupu koncových bodů tooclient, které se používají pro správu cloudových služeb. Používání obecných nebo osobních pracovních stanic k vývoji a správě infrastruktury otevírá možnosti útoků z nečekaných směrů, například prohlížení webu (např. útok typu watering hole) nebo e-mailu (např. sociální inženýrství a phishing).

![][1]

v tomto typu prostředí zvyšuje Hello potenciální útokům, protože je náročné tooconstruct zásady zabezpečení a mechanismy tooappropriately spravovat přístup k rozhraní tooAzure (například rozhraní SMAPI) z rozmanitých koncových bodů.

### <a name="remote-management-threats"></a>Vzdálená správa hrozeb
Útočníci často usilují toogain privilegovaného přístupu získáním přihlašovacích údajů k účtu (například prolomením hesla hrubou silou, phishing a sběrem přihlašovacích údajů) nebo zmást uživatele spustili škodlivý kód (například ze škodlivých webů s jednotky pomocí stáhne nebo z přílohy škodlivé e-mailu). V prostředí cloudu spravovaného účtu narušení může způsobit tooan zvýšenému riziku kvůli tooanywhere, kdykoli přístup.

I s přísnou kontrolou primárních účtů správců může být nižší úrovně uživatelské účty používané tooexploit slabá místa ve strategii zabezpečení. Nedostatek odpovídajícího bezpečnostního školení může také způsobit toobreaches prostřednictvím náhodného zpřístupnění informací o účtu.

Pokud pracovní stanici uživatele používáte i pro úlohy správy, může být ohrožena na mnoha různých místech. Jestli uživatele procházení hello webové, pomocí 3. stran a open source nástrojů nebo otevřít soubor škodlivé dokumentu, který obsahuje trojského koně.

Obecně platí nejvíce cílených útoků, které může být následek úniky dat trasovat toobrowser zneužití, moduly plug-in (například Flash, PDF, Java) a spear phishing (e-mail) na stolních počítačích. Tyto počítače mít úrovni správce nebo tooaccess oprávnění na úrovni služby za provozu serverů nebo síťová zařízení, když jsou používány pro vývoj nebo správu jiných prostředků.

### <a name="operational-security-fundamentals"></a>Základy provozního zabezpečení
Pro bezpečnější správu a provoz můžete prostor pro útoky na klienta minimalizovat snížením hello počet možných vstupních bodů. Můžete to provést prostřednictvím zásad zabezpečení: „oddělení povinností“ a „odloučení prostředí“.

Vzájemnou izolací citlivých funkcí navzájem toodecrease hello pravděpodobnost, že na jedné úrovni vede tooa porušení v jiném. Příklady:

* Úlohy správy nikdy Nekombinujte s aktivity, které můžou způsobit ohrožení tooa (například malware v e-mailu správce, který potom nakazí server infrastruktury).
* Pracovní stanice používaná pro velmi citlivé operace nesmí být stejné systém používal pro vysoce rizikové účely, například procházení hello Internet hello.

Snížit hello systému vůči útokům tím, že odeberete nepotřebný software. Příklad:

* Standard pro správu, podporu nebo vývoj pracovní stanice by neměly vyžadovat instalaci e-mailového klienta nebo jiných aplikací zvyšujících produktivitu, pokud je hlavním účelem hello zařízení toomanage cloudové služby.

Klientské systémy, které mají tooinfrastructure přístup správce, by měla být součástí vystaveny toohello nejpřísnějším možným zásadám tooreduce bezpečnostní rizika. Příklady:

* Zásady zabezpečení mohou zahrnovat nastavení zásad skupiny, které odepřou otevřený přístup k Internetu z hello zařízení a použití omezující konfiguraci brány firewall.
* V případě potřeby přímého přístupu použijte VPN se zabezpečením pomocí protokolu IP (IPsec).
* Nakonfigurujte samostatné domény služby Active Directory pro správu a vývoj.
* Izolujte a filtrujte síťový provoz pracovní stanice používané ke správě.
* Používejte antimalwarový software.
* Implementace služby Multi-Factor authentication tooreduce hello riziko krádeže přihlašovacích údajů.

Sloučením přístupových prostředků a odstraněním nespravovaných koncových bodů také zjednodušíte úlohy správy.

### <a name="providing-security-for-azure-remote-management"></a>Zajištění zabezpečení pro vzdálenou správu Azure
Azure poskytuje zabezpečení mechanismy tooaid správci, kteří spravují cloudové služby Azure a virtuálních počítačů. Mezi tyto mechanismy patří:

* ověřování a [řízení přístupu na bázi rolí](../active-directory/role-based-access-control-configure.md)
* sledování, protokolování a auditování
* certifikáty a šifrovaná komunikace
* webový portál pro správu
* filtrování síťových paketů

Konfigurace zabezpečení na straně klienta a datacenter nasazení brány pro správu je možné toorestrict a monitorování správce přístup toocloud aplikacím a datům.

> [!NOTE]
> Některá doporučení v tomto článku můžou vést k vyšší spotřebě datových, síťových nebo počítačových prostředků a můžou zvýšit náklady na licence nebo předplatné.
>
>

## <a name="hardened-workstation-for-management"></a>Posílené pracovní stanice pro správu
Hello cílem posílení pracovní stanice je tooeliminate všechny, ale nejdůležitější funkce hello potřeba pro ni toooperate, což hello potenciální prostor pro útok co nejmenší. Posílení systému zahrnuje minimalizaci hello počtu nainstalovaných služeb a aplikací, omezení provádění aplikací, omezení tooonly přístup k síti, co je potřeba, a udržování provozu toodate hello systému. Kromě toho používání posílené pracovní stanice pro správu odlučuje nástroje a činnosti správy od jiných úloh koncových uživatelů.

V podnikovém prostředí místně můžete omezit prostor pro útok hello fyzickou infrastrukturu pomocí vyhrazené správy sítí, serverových místností s přístupem na kartu a pracovních stanic, které běží v chráněných oblastech sítě hello. V cloudu nebo modelu hybridního IT probíhá péče o služby Zabezpečené správy může být složitější z důvodu nedostatku hello prostředků tooIT fyzický přístup. Implementace řešení ochrany vyžaduje pečlivou konfiguraci softwaru, procesy zaměřené na zabezpečení a všestranné zásady.

Pomocí softwaru s minimálními oprávněními nároků v uzamknuté pracovní stanici pro správu cloudu – a pro vývoj aplikací – můžete snížit riziko bezpečnostních incidentů hello protože tak můžete standardizovat prostředí vzdálené správy a vývoj hello. Konfigurace posílené pracovní stanice může zabránit hello ohrožení zabezpečení účtů, které jsou používané toomanage důležitých cloudových prostředků ukončením mnoho běžných kudy přichází malware a různá zneužití. Konkrétně můžete použít [Windows AppLocker](http://technet.microsoft.com/library/dd759117.aspx) a toocontrol technologie Hyper-V a izolování chování klientského systému a ke zmírnění hrozeb, včetně e-mailu nebo procházení Internetu.

Na posílené pracovní stanice hello správce spustí standardní uživatelský účet (která blokuje úrovni pro správu) a přidružené aplikace se řídí pomocí seznamu povolených položek. Hello základní prvky posílené pracovní stanice jsou následující:

* Aktivní vyhledávání a opravy chyb. Nasazení antimalwarového softwaru, provádění pravidelných prověřování ohrožení zabezpečení a aktualizace všech pracovních stanic pomocí nejnovějších aktualizací zabezpečení hello včas.
* Omezená funkčnost. Odinstalování všech nepotřebných aplikací a zakázání nepotřebných služeb (spouštění).
* Posílení zabezpečení sítě. Pomocí brány Windows Firewall pravidla tooallow pouze platné IP adresy, porty a adresy URL související tooAzure správy. Ujistěte se také blokují této pracovní stanici toohello příchozí vzdálená připojení.
* Omezení spouštění. Povolení jenom jedné sady předdefinovaných spustitelných souborů, které jsou potřebné pro správu toorun (označované tooas "výchozí odmítnutí"). Ve výchozím nastavení, nesmíte dát uživatelům oprávnění toorun žádné program, pokud není výslovně uveden v hello seznamu povolených.
* Nejnižší oprávnění. Uživatelé pracovní stanice pro správu by neměl mít žádná oprávnění správce na samotném místním počítači hello. Tímto způsobem nemůžou změnit konfiguraci systému hello nebo hello systémové soubory, ať už úmyslně nebo náhodně.

To vše můžete vynutit pomocí [objekty zásad skupiny](https://www.microsoft.com/download/details.aspx?id=2612) (GPO) ve službě Active Directory Domain Services (AD DS) a jejich použití prostřednictvím vaší tooall Správa účtů pro správu (místní) domény.

### <a name="managing-services-applications-and-data"></a>Správa služeb, aplikací a dat
Konfigurace cloudových služeb Azure se provádí prostřednictvím hello portál Azure nebo rozhraní SMAPI, prostřednictvím rozhraní příkazového řádku prostředí Windows PowerShell hello nebo uživatelské aplikace, která tato rozhraní RESTful využívá. Mezi služby, které tyto mechanismy využívají, patří Azure Active Directory (Azure AD), Azure Storage, weby Azure, Azure Virtual Network a další.

Virtuální počítač – nasazené aplikace poskytují vlastní klientské nástroje a rozhraní podle potřeby, třeba hello Microsoft Management Console (MMC), konzola pro správu podniku (například Microsoft System Center nebo Windows Intune) nebo jinou správu aplikace – Microsoft SQL Server Management Studio, např. Tyto nástroje jsou obvykle umístěny v podnikovém prostředí nebo v klientské síti. Můžou záviset na konkrétních síťových protokolech, například na protokolu RDP (Remote Desktop Protocol), které vyžadují přímé stavové připojení. Některé mohou mít povolené webové rozhraní, které by neměl být zveřejněno nebo zpřístupněno hello Internetu.

Můžete omezit přístup tooinfrastructure a platforma správy služby v Azure pomocí [služby Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication.md), [certifikátů X.509 pro správu](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/)a pravidel brány firewall. Hello portál Azure a rozhraní SMAPI vyžadují zabezpečení TLS (Transport Layer). Však služby a aplikace, které nasadíte do Azure vyžaduje tootake ochranná opatření, které jsou vhodné závisí na vaší aplikaci. Tyto mechanismy můžete snadněji povolit prostřednictvím konfigurace standardizované posílené pracovní stanice.

### <a name="management-gateway"></a>Brána pro správu
toocentralize všechny správce přístup a zjednodušit sledování a protokolování, můžete nasadit vyhrazený [Brána vzdálené plochy](https://technet.microsoft.com/library/dd560672) serveru (Brána VP) v místní síti připojené tooyour prostředí Azure.

Brána vzdálené plochy je proxy služba protokolu RDP založená na zásadách, která vynucuje požadavky na zabezpečení. Zavedení brány VP spolu s architekturou NAP (Network Access Protection) pro Windows Server pomáhá zajistit, že se připojí pouze klienti, kteří splňují konkrétní bezpečnostní kritéria stanovená objekty zásad skupiny (GPO) ve službě Active Directory Domain Services (AD DS). Navíc platí:

* Zřízení [certifikát pro správu Azure](http://msdn.microsoft.com/library/azure/gg551722.aspx) na hello brány VP, aby byla jediným hostitelem hello povolené tooaccess hello portálu Azure.
* Připojení brány VP toohello hello stejné [doména správy](http://technet.microsoft.com/library/bb727085.aspx) jako hello pracovní stanice správce. To je nezbytné, pokud používáte síť site-to-site IPsec VPN nebo ExpressRoute v rámci domény, která má jednosměrný vztah důvěryhodnosti tooAzure AD, nebo pokud federujete přihlašovací údaje mezi místní instancí AD DS a Azure AD.
* Konfigurace [zásady autorizace připojení klienta](http://technet.microsoft.com/library/cc753324.aspx) toolet hello Brána VP ověřte, zda název počítače klienta hello je platný (připojený k doméně) a povolené tooaccess hello portálu Azure.
* Protokol IPsec pro [Azure VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) toofurther chránit před odcizením tokenu a odposlechu provoz správy nebo zvažte použití izolovaného internetového odkazu prostřednictvím [Azure ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).
* Povolte vícefaktorové ověřování (prostřednictvím služby [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)) nebo ověřování pomocí čipové karty, aby ho mohli používat správci, kteří se připojují prostřednictvím brány VP.
* Nakonfigurujte zdroj [omezení podle IP adresy](http://azure.microsoft.com/blog/2013/08/27/confirming-dynamic-ip-address-restrictions-in-windows-azure-web-sites/) nebo [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) v Azure toominimize hello počet koncových bodů povolených správy.

## <a name="security-guidelines"></a>Pokyny pro zabezpečení
Obecně platí, pomáhá pracovní stanice správce toosecure pro použití s cloudem hello je podobné toohello postupům používaným pro všechny místní pracovní stanice – například minimalizovaná sestavení a omezující oprávnění. Některé jedinečné aspekty správy cloudu se více podobná tooremote nebo enterprise out-of-band management. Mezi ně patří hello používání a auditování přihlašovacích údajů, rozšířené zabezpečení vzdáleného přístupu a detekce hrozeb a odpovědi.

### <a name="authentication"></a>Authentication
Můžete použít Azure přihlašovací omezení tooconstrain zdrojové IP adresy pro přístup k nástroje pro správu a požadavkům na audit přístupu. toohelp Azure identifikací klientů pro správu (pracovní stanice nebo aplikace), můžete nakonfigurovat rozhraní SMAPI (prostřednictvím zákazníkem vyvinutých nástrojů, jako je například rutiny prostředí Windows PowerShell) a hello Azure portálu toorequire správu klientské certifikáty toobe nainstalovaná, kromě tooSSL certifikáty. V případě přístupu pro správce doporučujeme vyžadovat vícefaktorové ověřování.

Některé aplikace nebo služby, které nasadíte do Azure, můžou mít vlastní mechanismy ověřování pro přístup koncových uživatelů i správce, zatímco ostatní plně využívají výhody Azure AD. V závislosti na tom, jestli federujete přihlašovací údaje prostřednictvím služby Active Directory Federation Services (AD FS), pomocí synchronizace adresářů nebo Správa uživatelských účtů výhradně v hello cloudu, pomocí [Microsoft Identity Manager](https://technet.microsoft.com/library/mt218776.aspx) (součástí Azure AD Premium) pomůže vám se správou životních cyklů identit mezi prostředky hello.

### <a name="connectivity"></a>Připojení
Několik mechanismů jsou k dispozici toohelp zabezpečení klientských připojení tooyour virtuálních sítí Azure. Dva z těchto mechanismů [site-to-site VPN](https://channel9.msdn.com/series/Azure-Site-to-Site-VPN) (S2S) a [point-to-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) (P2S) umožňují používání hello standardního protokolu IPsec (S2S) nebo hello [Secure Socket Tunneling Protocol](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx)(SSTP) (P2S) pro šifrování a tunelové propojení. Když se Azure připojuje správu toopublic přístupných služeb Azure, jako je například hello portálu Azure, Azure vyžaduje protokol Secure HTTPS (Hypertext Transfer).

Samostatná Posílená pracovní stanice, které není připojeno tooAzure přes bránu VP by měl použít hello protokolem SSTP point-to-site VPN toocreate hello počáteční připojení toohello Azure Virtual Network a potom vytvořte virtuální tooindividual připojení RDP počítače z s hello tunelového připojení sítě VPN.

### <a name="management-auditing-vs-policy-enforcement"></a>Auditování správy vs. vynucení zásad
Obvykle se používají dva přístupy pomáháte procesů správy toosecure: auditování a vynucování zásad. Provádění obou zajistí všestrannou kontrolu, ale nemusí být ve všech situacích možné. Kromě toho má každý přístup různé úrovně rizika, nákladů a úsilí, které souvisejí se správou zabezpečení, zejména ve vztahu toohello úroveň důvěryhodnosti jednotlivců i systémových architektur.

Sledování, protokolování a auditování poskytují základ pro sledování a pochopení aktivit správy, ale nemusí být vždy proveditelné tooaudit podrobností kvůli toohello objemu vygenerovaných dat dokončení všech akcí v. Auditování účinnosti zásad správy hello hello je nejlepším postupem je ale.

Vynucení zásad zahrnující přísnou kontrolu přístupu staví programové mechanismy na místo, které může řídit akce správce, a pomáhá zajistit používání všech možných ochranných opatření. Protokolování zajišťuje důkazy o vynucení v přidání tooa záznam, kdo to udělal, odkud a kdy. Protokolování také umožňuje tooaudit a porovnávání informace o tom, jak správci dodržují zásady, a poskytuje důkazy o aktivitách.

## <a name="client-configuration"></a>Konfigurace klienta
V případě posílených pracovních stanic doporučujeme, tři primární konfigurace. Hello největších differentiators mezi nimi jsou náklady, použitelnost a usnadnění, při zachování podobné profil zabezpečení přes všechny možnosti. Hello následující tabulka nabízí krátkou analýzu výhod a rizik tooeach hello. (Všimněte si, že "podnikovým Počítačem" odkazuje tooa standardní stolní počítač konfiguraci, která se nasazuje pro všechny uživatele domény, bez ohledu na role.)

| Konfigurace | Výhody | Nevýhody |
| --- | --- | --- |
| Samostatná posílená pracovní stanice |Přísně řízená pracovní stanice |vyšší náklady na vyhrazené stolní počítače |
| - | Menší riziko zneužití aplikací |Zvýšené úsilí vynaložené na správu |
| - | Jasné oddělení povinností | - |
| Podnikový počítač jako virtuální počítač |Snížení náklady na hardware | - |
| - | Odloučení role a aplikací | - |
| Toogo Windows se nástroj BitLocker drive encryption |Kompatibilita s většinou počítačů |Sledování prostředků |
| - | Nákladová efektivnost a přenositelnost | - |
| - | Izolované prostředí správy |- |

Je důležité, aby hello posílené pracovní stanice je hello hostitele a není hello hosta stálo mezi hello hostitelem operačního systému a hello hardwaru. Následující hello "zásady čistého zdroje" (také označované jako "bezpečný původ") znamená, které hostují hello by měl být hello nejvíce zesílené zabezpečení. Jinak hello posílené pracovní stanice (Host) je tooattacks subjektu v hello systému, který je hostitelem.

Můžete odloučit ještě pomocí vyhrazených imagů systému pro každou posílenou pracovní stanici s pouze hello nástroje pro správu funkcí a oprávnění potřebné pro správu vyberte Azure a cloudovým aplikacím s konkrétní místní GPO v AD DS pro hello nezbytné úlohy.

Pro prostředí IT, které nemají žádné místní infrastrukturu (například žádný přístup tooa místní služby AD DS instanci pro objekty zásad skupiny, protože jsou všechny servery v cloudu hello) služby jako [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) můžete zjednodušit nasazování a údržbu konfigurací pracovních stanic.

### <a name="stand-alone-hardened-workstation-for-management"></a>Samostatná posílená pracovní stanice určená pro správu
V případě samostatné posílené pracovní stanice mají správci stolní nebo přenosný počítač používaný pro úlohy správy a jiný, samostatný stolní nebo přenosný počítač pro úlohy, které nepotřebují žádné oprávnění. Pracovní stanice vyhrazené toomanaging služeb Azure nepotřebuje jiné aplikace nainstalované. Kromě toho používání pracovních stanic, které podporují [služby TPM (Trusted Platform Module)](https://technet.microsoft.com/library/cc766159) (TPM) nebo podobné technologie šifrování na úrovni hardwaru, usnadňuje ověřování zařízení a ochranu před napadením. TPM může také podporovat ochranu celého svazku systémové jednotky hello pomocí [nástroj BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx).

V případě samostatné posílené pracovní stanice hello (zobrazené dole) je hello místní instance brány Windows Firewall (nebo brány firewall klienta jiného výrobce) nakonfigurovaná tooblock příchozí připojení, například RDP. Hello správce se může přihlásit toohello posílené pracovní stanice a spusťte na relaci protokolu RDP, který se připojuje tooAzure po navázání připojení k virtuální síti Azure VPN, ale nemohou přihlásit tooa podnikových počítačů a použití protokolu RDP tooconnect toohello Posílená pracovní stanice sám sebe.

![][2]

### <a name="corporate-pc-as-virtual-machine"></a>Podnikový počítač jako virtuální počítač
V případech, kde samostatné samostatné posílené pracovní stanice nákladově nevýhodná nebo nepohodlné, hello posílené pracovní stanice může být hostitelem bez oprávnění správce úloh pro tooperform virtuálního počítače.

![][3]

tooavoid několika bezpečnostním rizikům, která mohou vyplývat z používání jedné pracovní stanice pro správu systémů a další každodenní pracovní úlohy, můžete nasadit toohello virtuální počítač Windows Hyper-V Posílená pracovní stanice. Tento virtuální počítač můžete použít jako hello podnikový počítač. Hello prostředí s podnikovými počítači může zůstat izolované od hello hostitele, která omezuje prostor pro útok a odebere denní aktivity uživatele hello (například e-mailu) od citlivých úloh správy.

Hello podnikových PC virtuální počítač běží v chráněném prostoru a poskytuje uživatelské aplikace. Hello hostitel zůstává "čistým zdrojem" a vynucuje přísné síťové zásady v hello kořenovém operačním systému (například blokuje přístup o RDP z virtuálního počítače hello).

### <a name="windows-toogo"></a>Windows tooGo
Další alternativní toorequiring samostatná Posílená pracovní stanice je toouse [Windows tooGo](https://technet.microsoft.com/library/hh831833.aspx) jednotky, funkce, která podporuje možnost spouštění z USB na straně klienta. Windows tooGo umožňuje uživatelům tooboot kompatibilního počítače tooan izolovaným imagem systému spouštění ze šifrovaného USB flash disku. Poskytuje další ovládací prvky pro vzdáleně spravované koncové body, protože hello image může plně spravovat pomocí podniková skupina IT pomocí přísných bezpečnostních zásad, minimalistického sestavení operačního systému a podpory čipu TPM.

V hello obrázku je hello přenosný image systém připojený k doméně, který je pouze tooAzure předkonfigurované tooconnect, vyžaduje vícefaktorové ověřování a blokuje veškerý provoz netýká správy. Pokud uživatel spustí hello stejný počítač toohello standardním podnikovým imagem a pokusí přístup k bráně VP nástrojům pro správu Azure, zablokuje se hello relace. Windows tooGo stane hello kořenové úrovni operačního systému a žádné další vrstvy jsou požadované (hostitelský operační systém, hypervisor, virtuální počítač), může být zranitelnější toooutside útoky.

![][4]

Je důležité toonote, že jsou jednotky USB flash ztraceny snadněji než průměrná stolní počítač. Použití nástroje BitLocker tooencrypt hello celý svazek, společně s silné heslo, je méně pravděpodobné, že útočník může použít obrazu disku hello ke škodlivým účelům. Navíc pokud dojde ke ztrátě hello USB flash disk, odvolání a [vydání nového certifikátu pro správu](https://technet.microsoft.com/library/hh831574.aspx) společně s rychlé heslo můžete resetovat snížila zranitelnost. Protokoly auditu správy jsou uloženy v Azure, ne na hello klienta, čímž se ještě sníží potenciální ztrátě dat.

## <a name="best-practices"></a>Osvědčené postupy
Vezměte v úvahu následující pokyny, když spravujete aplikace a data v Azure hello.

### <a name="dos-and-donts"></a>Pro a proti
Nepředpokládejte, že vzhledem k tomu, že pracovní stanice byl uzamčen, další běžné požadavky na zabezpečení není nutné toobe splněny. Hello potenciální riziko je vyšší kvůli vyšší úrovni přístupu, které obvykle mají účty správců. V tabulce hello níže jsou uvedeny příklady rizik a jim odpovídající bezpečné postupy.

| Chybný postup | Správný postup |
| --- | --- |
| Neposílejte přihlašovací údaje pro přístup správce nebo jiné tajné údaje e-mailem (např. SSL nebo certifikáty pro správu). |Zachovávejte důvěrnost tak, že názvy účtů a hesla sdělíte ústně (ale neukládejte je do hlasové pošty), provádějte vzdálenou instalaci certifikátů klienta a serveru (prostřednictvím šifrované relace), stahujte z chráněných síťových sdílených položek nebo data předávejte ručně pomocí vyměnitelného média. |
| - | Životní cykly certifikátu pro správu spravujte proaktivně. |
| Neukládejte hesla účtů do úložiště aplikací (například tabulek, webů SharePoint nebo sdílených složek) v nezašifrované podobě nebo bez použití algoritmu hash. |Vytvoření zásady pro správu zabezpečení a zásady pro posilování systému a použít je tooyour vývojové prostředí. |
| - | Použití [Enhanced Mitigation Experience Toolkit 5.5](https://technet.microsoft.com/security/jj653751) Připnutí certifikátu pravidla tooensure správný přístup k tooAzure SSL/TLS webům. |
| Nesdílejte účty a hesla mezi správci a nepoužívejte stejné heslo pro několik účtů nebo služeb (zejména v případě účtů na sociálních sítích nebo účtů pro jiné běžné aktivity). |Vytvořit vyhrazenou toomanage účet Microsoft vašeho předplatného Azure – účet, který se nepoužívá pro osobní e-mailu. |
| Konfigurační soubory neposílejte e-mailem. |Konfigurační soubory a profily vždy instalujte z důvěryhodného zdroje (například ze šifrovaného USB flash disku). Nepoužívejte mechanismy, který se dají snadno ohrozit, například e-mail. |
| Nepoužívejte slabá nebo jednoduchá přihlašovací hesla. |Prosazujte zásady silných hesel, cykly vypršení platnosti (změna po prvním použití), časové limity konzoly a automatické uzamykání účtů. K přístupu do úložiště hesel používejte systém pro správu hesel klientů s vícefaktorovým ověřováním. |
| Nevystavujte porty toohello správu Internetu. |Uzamčení Azure porty a přístup pro správu toorestrict IP adresy. Další informace najdete v tématu hello [zabezpečení sítě Azure](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) dokumentu white paper. |
| - | Používejte bránu firewall, připojení VPN a architektury NAP pro všechna připojení správy. |

## <a name="azure-operations"></a>Provoz Azure
Provozní technici a zaměstnanci podpory Microsoftu, kteří přistupují k produkčním systémům Azure, používají [posílené pracovní stanice s virtuálními počítači](#stand-alone-hardened-workstation-for-management), které jsou na nich zřízené pro potřeby interního přístupu k podnikové síti a k aplikacím (například e-mailu, intranetu atd.). Všechny pracovní stanice správu jsou vybavené čipy TPM a spouštěcí jednotka hostitele hello je zašifrovaná pomocí Bitlockeru jsou připojené k tooa speciální organizační jednotce (OU) v primární podnikové doméně Microsoftu.

Posilování systému se vynucuje prostřednictvím zásad skupiny s centralizovanou aktualizací softwaru. Auditování a analýz protokoly událostí (například zabezpečení a Applockeru) jsou shromážděná z pracovní stanice pro správu a uložit tooa centrálního umístění.

Kromě toho jump Box v síti Microsoftu a které vyžadují dvoufaktorové ověřování jsou použité tooconnect tooAzure produkční sítě.

## <a name="azure-security-checklist"></a>Kontrolní seznam zabezpečení Azure
Minimalizace hello počet úloh, které můžou správci provádět na posílené pracovní stanice pomůže minimalizovat hello prostoru k útokům na vývoj a správu prostředí. Použití hello následující technologie toohelp chránit vaše posílené pracovní stanice:

* Posílení Internet Exploreru. prohlížeč Internet Explorer Hello (nebo libovolného webového prohlížeče, k tomuto účelu) je klíčovou vstupní branou škodlivého kódu z důvodu tooits rozsáhlá připojení k externím serverům. Projděte zásady svého klienta a vynuťte běh v chráněném režimu, zákaz doplňků, zákaz stahování souborů a používejte filtrování [Microsoft SmartScreen](https://technet.microsoft.com/library/jj618329.aspx). Ověřte, že se zobrazují upozornění týkající se zabezpečení. Využijte výhody nastavení zón internetu a vytvořte si seznam důvěryhodných webů, pro které máte nakonfigurované přiměřené posílení zabezpečení. Blokujte všechny ostatní weby a kódy v prohlížeči, například ActiveX a Javu.
* Standardní uživatel. Spuštění jako standardního uživatele přináší řadu výhod, hello největší z nich je, že krádež přihlašovacích údajů správce prostřednictvím malwaru je podstatně obtížnější. Kromě toho standardní uživatelský účet nemá oprávnění vyšší úrovně hello kořenovém operačním systému a mnoho možností konfigurace a rozhraní API jsou ve výchozím nastavení uzamčeno.
* AppLocker. Můžete použít [Applockeru](http://technet.microsoft.com/library/ee619725.aspx) toorestrict hello programů a skriptů, které můžou uživatelé spouštět. AppLocker můžete spustit v režimu auditování nebo vynucení. AppLocker ve výchozím nastavení, má pravidlo, které umožňuje uživatelům, kteří mají tokenu toorun správce veškerý kód na klientovi hello. Toto pravidlo existuje tooprevent správci zablokovat sami out a platí pouze tooelevated tokeny. Další informace najdete v článku o integritě kódu jako součásti [zabezpečení jádra](http://technet.microsoft.com/library/dd348705.aspx) systému Windows Server.
* Podepisování kódu. Podepisování kódu všech nástrojů a skriptů, které správci používají, nabízí ovladatelné mechanismy pro nasazování zásad, které slouží k uzamčení aplikací. Hodnoty hash není škálování s rychlým změnám toohello kódu a cesty k souborům neposkytují vysokou úroveň zabezpečení. Doporučujeme, abyste zkombinovali pravidla nástroje AppLocker pomocí prostředí PowerShell [zásady spouštění](http://technet.microsoft.com/library/ee176961.aspx) , může obsahovat pouze konkrétních podepsaných kódů a skriptů toobe [provést](http://technet.microsoft.com/library/hh849812.aspx).
* Zásady skupiny. Vytvořte globální zásady správy, který je pracovní stanice použité tooany domény, který se používá pro správu (a zablokujte přístup ze všech ostatních) a účty toouser ověřen na těchto pracovních stanicích.
* Zřizování s rozšířeným zabezpečením. Chrání vaše základní image posílené pracovní stanice toohelp ochrana proti manipulaci. Použít bezpečnostní opatření, například šifrování a izolaci toostore obrázků, virtuální počítače a skriptů a omezte přístup (možná proces se používá kontrolovatelný kontrola v a rezervace).
* Opravy chyb. Udržujte konzistentní sestavení (nebo mějte samostatné Image pro vývoj, operace a další úlohy správy), kontrolovat změny a malware pravidelně, zachovat hello sestavení až toodate a počítače aktivujte jenom v případě potřeby.
* Šifrování. Ověření, zda pracovní stanice pro správu TPM toomore bezpečně povolit [systém souborů EFS](https://technet.microsoft.com/library/cc700811.aspx) (EFS) a Bitlockeru. Pokud používáte Windows tooGo, použijte jenom šifrované USB klíče spolu s Bitlockerem.
* Řízení. Pomocí rozhraní Windows všichni správci hello, například sdílení souborů toocontrol GPO v AD DS. Zahrňte pracovní stanice pro správu do procesů auditování, sledování a protokolování. Sledujte všechny přístupy a chování správců a vývojářů.

## <a name="summary"></a>Souhrn
Používání konfigurace posílené pracovní stanice ke správě cloudových služeb Azure, služby Virtual Machines a aplikací vám může pomoct s omezením řady rizik a hrozeb, které vyplývají ze vzdálené správy kritické infrastruktury IT. Azure i Windows poskytují mechanismy, můžete použít toohelp chránit a řízení komunikace, ověřování a chování klienta.

## <a name="next-steps"></a>Další kroky
Hello následující prostředky jsou k dispozici tooprovide další obecné informace o Azure a souvisejících službách Microsoftu, v položkách toospecific přidání odkazujeme v tomto dokumentu:

* [Zabezpečení privilegovaného přístupu](https://technet.microsoft.com/library/mt631194.aspx) – získejte hello technické podrobnosti o navrhování a vytváření zabezpečené administrativní pracovní stanice pro správu Azure
* [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Security/AzureSecurity) – Další informace o možnostech platformy Azure, které chrání hello prostředků infrastruktury Azure a hello zatížení tohoto spustit v Azure
* [Microsoft Security Response Center](http://www.microsoft.com/security/msrc/default.aspx) – kde mohou být oznámeny slabá místa zabezpečení společnosti Microsoft, včetně problémů s Azure, nebo prostřednictvím e-mailu příliš[secure@microsoft.com](mailto:secure@microsoft.com)
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si toodate na hello nejnovější v zabezpečení Azure

<!--Image references-->
[1]: ./media/azure-security-management/typical-management-network-topology.png
[2]: ./media/azure-security-management/stand-alone-hardened-workstation-topology.png
[3]: ./media/azure-security-management/hardened-workstation-enabled-with-hyper-v.png
[4]: ./media/azure-security-management/hardened-workstation-using-windows-to-go-on-a-usb-flash-drive.png
