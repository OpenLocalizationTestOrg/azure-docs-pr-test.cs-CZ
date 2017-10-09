---
title: "technické možnosti zabezpečení aaaAzure | Microsoft Docs"
description: "Další informace o cloudové výpočetní služby, které zahrnují široký výběr výpočetních instancích & služby, které je možné škálovat nahoru a dolů automaticky toomeet hello potřebám vaší aplikace nebo enterprise."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/26/2017
ms.author: TomSh
ms.openlocfilehash: a0ef17883be54dab4cb6b597204f3197dc05c28c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-technical-capabilities"></a>Technické možnosti zabezpečení Azure

tooassist aktuální a potenciální zákazníci Azure pochopit a využívat hello různé související se zabezpečením možnosti dostupné v a okolního hello platformě Azure, společnost Microsoft vyvinula řadu dokumenty White Paper, zabezpečení přehledy, osvědčené postupy a Kontrolní seznamy. témata Hello rozsahu z hlediska spektra a hloubky a jsou pravidelně aktualizovány. Tento dokument je součástí této řady dle souhrnu v hello abstraktní části. Další informace o této série zabezpečení Azure naleznete na adrese (URL).

## <a name="azure-platform"></a>Platforma Azure

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) cloudové platformy se skládá z infrastruktury a aplikační služby, s použitím integrovaného datové služby a pokročilou analýzu a nástroje pro vývojáře a služby hostované v rámci veřejného cloudu dat společnosti Microsoft centrech. Zákazníci používat Azure pro mnoho různých kapacity a scénářů, na základní výpočetní, sítě a úložiště, toomobile webové aplikace služby, toofull cloudu scénáře, jako je Internet věcí a se dají použít s technologiích s otevřeným zdrojem a nasadit jako hybridní cloud nebo hostovaným v rámci datového centra zákazníka. Azure poskytuje technologie cloud jako stavební bloky toohelp společnosti ušetřili náklady, inovacemi. Zajistěte rychle a aktivně spravovat systémy. Pokud sestavení nebo migrovat poskytovatele cloudové tooa IT prostředky, se spoléháte na dané organizace dalo tooprotect vaší aplikace a data se službami hello a ovládací prvky hello poskytují zabezpečení hello toomanage vaše cloudové prostředky.

Microsoft Azure je hello pouze cloudové výpočetní zprostředkovatele, který nabízí aplikace zabezpečené a konzistentní platformy a infrastruktury jako služba pro toowork týmy v rámci své skillsets jiný cloud a úrovně projektu složitost integrované daty služby a analýzy, která odkrýt intelligence z dat bez ohledu na existuje napříč společnosti Microsoft a jiných společností než Microsoft platforem, otevřete rozhraní a nástroje, poskytuje volba pro integraci s místní i cloudové služby Azure v rámci nasazení cloudu místních datových center. Jako součást hello důvěryhodné cloudu Microsoftu zákazníci spoléhají na Azure špičkový zabezpečení, spolehlivost, dodržování předpisů, ochrany osobních údajů a hello velká síti osoby, partneři a procesy toosupport organizace v cloudu hello.

Microsoft Azure můžete:

- Urychlit inovací hello cloudu.

- & Aplikace se statistickými Power obchodních rozhodnutí.

- Volně sestavení a nasazení odkudkoli.

- Chraňte své firmy.

## <a name="scope"></a>Rozsah

Hello ústředním bodem tohoto dokumentu se vztahuje na funkce zabezpečení a funkce podporu Microsoft Azure základní komponenty, konkrétně [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction), [databází SQL Azure Microsoft](https://docs.microsoft.com/azure/sql-database/), [Model virtuálního počítače Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/  )a hello nástroje a infrastruktura, která ho všechny spravovat. Tento dokument white paper soustředit na k dispozici tooyou technických možností Microsoft Azure jako zákazníci toofulfil jejich role při ochraně hello zabezpečení a ochrana osobních údajů svých dat.

Hello význam vysvětlení tohoto modelu sdílenou odpovědnost je nezbytné pro zákazníky, kteří jsou přesunutí toohello cloudu. Poskytovatelé cloudové nabízejí značné výhody pro zabezpečení a dodržování předpisů úsilí, ale tyto výhody nejsou zbaveny hello zákazníkem už od ochranu svých uživatelů, aplikací a nabídek služeb.

Pro řešení IaaS hello zákazník zodpovídá nebo má sdílenou odpovědnost pro zabezpečení a správu hello operačního systému, konfiguraci sítě, aplikace, identity, klientů a data.  Řešení PaaS sestavení na IaaS nasazení, zákazník hello stále zodpovídá nebo má sdílenou odpovědnost pro zabezpečení a správu aplikací, identity, klienty a data. Pro řešení SaaS, Nonetheless, pokračuje hello zákazníka toobe odpovědnost za. Musí zajistit správnou klasifikaci dat, a sdílejí zodpovědnost toomanage svých uživatelů a zařízení koncový bod.

Tento dokument neposkytuje podrobné pokrytí všech hello související komponenty platformy Microsoft Azure, například weby Azure, Azure Active Directory, HDInsight, Media Services a dalším službám, které jsou na základě na základní komponenty hello. I když je k dispozici minimální úroveň obecné informace, se předpokládá čtečky obeznámeni s základní koncepty Azure, jak je popsáno v dalších odkazů na od společnosti Microsoft a součástí odkazů uvedených v tomto dokumentu.


## <a name="available-security-technical-capabilities-toofulfil-user-customer-responsibility---big-picture"></a>K zabezpečení technické možnosti toofulfil uživatele (zákazníka) odpovědnost – přehled

Microsoft Azure poskytuje služby, které pomáhají zákazníkům splňovat hello zabezpečení, ochrany osobních údajů a dodržování předpisů potřebám. Hello následující obrázek, který pomáhá popisují různé služby Azure k dispozici pro uživatele toobuild Infrastruktura zabezpečení a dodržování aplikace na základě oborových standardů.

![K dispozici zabezpečení technické možnosti Big obrázek](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Spravovat a řídit, identity a přístup (chránit)

Azure pomáhá chránit firmy a osobní údaje povolením jste toomanage identity uživatelů a přihlašovacích údajů a řízení přístupu.

### <a name="azure-active-directory"></a>Azure active directory

Microsoft identit a přístupu řešení Nápověda pro správu IT chránit tooapplications přístup a prostředky v podnikovém datovém centru hello a do cloudu hello povolení další úrovně ověřování, jako je vícefaktorové ověřování a podmíněného zásady přístupu. Monitorování podezřelé aktivity přes pokročilé zabezpečení vytváření sestav a auditování, výstrahy, pomáhá zmírnit potenciální potíže se zabezpečením. [Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) poskytuje jeden toothousands přihlášení (SaaS) cloudových aplikací a aplikací tooweb přístup spustit místně.

Výhody zabezpečení služby Azure Active Directory (AD) zahrnují možnost hello:

- Vytvořit a spravovat jedinou identitu pro každého uživatele v rámci podniku hybridní udržování synchronizace uživatele, skupiny a zařízení.

- Zadejte přístup přihlášení tooyour aplikace včetně tisícům předem integrovaných aplikací SaaS.

- Povolit aplikaci přístup k zabezpečení vynucením vícefaktorového ověřování založeného na pravidlech pro místní i cloudové aplikace.

- Poskytování zabezpečeného vzdáleného přístupu tooon místní webové aplikace prostřednictvím proxy aplikace služby Azure AD.

[Portál služby Azure active directory](http://aad.portal.azure.com/) je k dispozici součástí portálu azure. Z tohoto řídicího panelu můžete získat přehled o stavu hello vaší organizace a snadno ponořte do Správa hello adresáře, uživatele nebo přístup k aplikaci.

![Azure active directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Toto jsou základní možnosti správy identit Azure:

- Jednotné přihlašování

- Ověřování pomocí služby Multi-Factor Authentication

- Sledování zabezpečení, výstrahy a sestavy na základě learning počítače

- Správa identit a přístupu zákazníků

- Registrace zařízení

- Správa privilegovaných identit

- Ochrana identit

#### <a name="single-sign-on"></a>Jednotné přihlašování

[Jednotné přihlašování (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) znamená, je možné tooaccess všechny hello aplikace a prostředky, které potřebujete toodo podnikání, po přihlášení pouze jednou pomocí jediného uživatelského účtu. Jakmile se přihlásíte, můžete přístup všechny aplikace hello budete potřebovat, aniž by musel být požadované tooauthenticate (zadejte například heslo) ještě jednou.

Mnoho organizací závisí software jako služba (SaaS) aplikace například Office 365, pole a Salesforce pro produktivita koncového uživatele. V minulosti tooindividually IT pracovníci potřeba vytvářet a aktualizovat uživatelské účty v každé aplikaci SaaS, a uživatelé měli tooremember heslo pro každou aplikaci SaaS.

[Azure AD rozšiřuje místní služby Active Directory do cloudu hello](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), povolíte uživatelům toouse jejich organizace primární účet toonot pouze přihlášení tootheir zařízení připojených k doméně a prostředkům společnosti, ale také všechny hello webu a aplikace SaaS potřebné pro své úlohy.

Jenom uživatelé nemají toomanage více sad uživatelských jmen a hesel, přístup k aplikaci lze automaticky zřízeného nebo zrušte zřízené na základě organizační skupiny a jejich stav jako zaměstnanec. [Azure AD, představuje ovládací prvky zásad správného řízení zabezpečení a přístup](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) , umožňují toocentrally spravovat přístup uživatelů v rámci aplikací SaaS.

#### <a name="multi-factor-authentication"></a>Ověřování pomocí služby Multi-Factor Authentication

[Azure Multi-Factor authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) je metoda ověřování, který vyžaduje hello použití více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení toouser přihlášení a transakce. [MFA pomáhá chránit](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) přístup toodata a aplikacím při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování přes celou řadu možností ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a třetích stran tokeny OAuth.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Sledování zabezpečení, výstrahy a sestavy na základě learning počítače

Sledování zabezpečení a výstrah a na základě learning sestav počítače, které identifikují nekonzistentní přístupové vzorce vám umožní chránit vaši firmu. Můžete použít Azure Active Directory přístup a použití sestav toogain přehled hello integrity a zabezpečení adresáře vaší organizace. Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby se adekvátní naplánovat toomitigate těchto rizik.

V hello portál Azure classic nebo prostřednictvím [Azure Active directory portálu](http://aad.portal.azure.com/), [sestavy](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) jsou rozdělené do hello následující způsoby:

- Sestavy anomálií – obsahovat přihlášení, jsme našli toobe neobvyklé události. Naším cílem je toomake si vědom tyto aktivity a díky kterému budete mít toodecide toobe o tom, zda je událost podezřelé.

- Integrované sestavy aplikací – poskytují přehled o tom, jak cloudové aplikace jsou používány ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíci cloudových aplikací.

- Zprávy o chybách – znamenat chyby, které mohou nastat při zřizování účtů tooexternal aplikace.

- Uživatelská sestavy – zobrazí zařízení nebo přihlášení v data aktivit pro konkrétního uživatele.

- Protokoly aktivity – obsahovat záznam všech auditované události v rámci hello posledních 24 hodin, 7 dní, nebo posledních 30 dnů a změny aktivity skupiny a poslední aktivita resetování a registraci hesla.

#### <a name="consumer-identity-and-access-management"></a>Správa identit a přístupu zákazníků

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) je vysokou dostupností, globální, služba identity management určených aplikací, škáluje toohundreds milionů identit. Dá se integrovat do mobilních i webových platforem. Uživatele mohou přihlásit tooall aplikace prostřednictvím přizpůsobitelné pomocí svých účtů na sociálních nebo vytvořením nové přihlašovací údaje.

V hello za příliš vývojáři aplikací[registrace a přihlašování uživatelů](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) do svých aplikací mít by napsali vlastní kód. A použili by místní databáze nebo systémy toostore uživatelských jmen a hesel. Azure Active Directory B2C nabízí vaší organizace lepší způsob toointegrate správu identit uživatelů do aplikace pomocí hello zabezpečené, na standardech postavené platformy a velké sady rozšiřitelných zásad.

Pokud používáte Azure Active Directory B2C, vaši uživatelé mohou zaregistrovat do pro vaše aplikace pomocí svých účtů na sociálních (Facebook, Google, Amazon, LinkedIn) nebo vytvořením nové přihlašovací údaje (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo).

Registrace zařízení

[Registrace zařízení služby Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) je hello základ pro zařízení na základě [podmíněného přístupu](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) scénáře. Když je zařízení registrováno, poskytuje Azure Active Directory Device Registration hello zařízení s identitou, která je použité tooauthenticate hello zařízení při přihlášení uživatele hello. Hello ověření zařízení a atributy hello hello zařízení, lze potom použít tooenforce zásady podmíněného přístupu pro aplikace, které jsou hostované v cloudu hello a místní.

V kombinaci s [správu mobilních zařízení (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) řešení, jako je například Intune, hello atributy zařízení ve službě Azure Active Directory jsou aktualizovány o další informace o zařízení hello. Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů.

#### <a name="privileged-identity-management"></a>Správa privilegovaných identit

[Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) vám umožní spravovat, řídit a monitorovat privilegované identity a přístup k tooresources ve službě Azure AD a také další služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.

Uživatelé někdy potřebují toocarry out privilegované operace v Azure nebo Office 365 prostředků nebo jiných aplikací SaaS. Často to znamená organizace mají toogive je trvalé privilegovaný přístup v Azure AD. Toto je rostoucí bezpečnostní riziko pro hostované cloudové prostředky, protože organizace nelze monitorovat dostatečně tyto činnosti uživatelů s jejich oprávněními správce. Kromě toho pokud uživatelský účet s privilegovaného přístupu je ohrožen, že jeden porušení zabezpečení by mohlo mít vliv jejich celkové zabezpečení cloudu. Azure AD Privileged Identity Management pomáhá tooresolve toto riziko.

Azure AD Privileged Identity Management vám umožní:

- Uvidíte, kteří uživatelé jsou správci Azure AD

- Povolit na vyžádání "právě v čase" tooMicrosoft přístup pro správu Online služeb, jako třeba Office 365 a Intune

- Získání sestavy o historii přístup správce a změny v přiřazení správců

- Dostávat upozornění na přístup tooa privilegované role

#### <a name="identity-protection"></a>Ochrana identit

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) je služba zabezpečení, která poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci. Ochrana identity používá možností detekce anomálií existující Azure služby Active Directory (k dispozici prostřednictvím neobvyklé aktivity sestav Azure AD) a zavádí nové typy událostí rizik, které můžete zjišťovat anomálie v reálném čase.

## <a name="secured-resource-access-in-azure"></a>Přístup k zabezpečeným prostředkům v Azure

Spustí se z hlediska fakturační řízení přístupu v Azure. Hello vlastníka účtu Azure, získat přístup, když přejdete hello [centra účtů Azure](https://account.windowsazure.com/subscriptions), je hello správce účtu (AA). Předplatné je kontejner pro fakturaci, ale také slouží jako hranice zabezpečení: každého předplatného se službu správce (SA), který můžete přidat, odebrat a úpravy prostředků Azure v tomto předplatném pomocí hello [portál Azure classic](https://manage.windowsazure.com/). Výchozí SA Hello nového předplatného je hello AA, ale hello AA můžete změnit hello SA v hello centra účtů Azure.

![Přístup k zabezpečeným prostředkům v Azure](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Odběry mají také přidružení s adresářem. Hello directory definuje sadu uživatelů. Může jít o uživatele z hello práci nebo ve škole, který vytvořil adresář hello a může se jednat o externí uživatele (to znamená, Accounts Microsoft). Odběry jsou přístupné pro podmnožinu těchto directory uživatelů, kteří mají přiřazený jako služba správce nebo Spolusprávce (CA); Hello pouze výjimkou je, že starší verze důvodů Accounts Microsoft (dříve Windows Live ID) může být přiřazen jako SA nebo certifikační Autority bez se nachází v adresáři hello.

Zaměřené na zabezpečení společnosti by měla soustředit na poskytnutí zaměstnanci hello přesný oprávnění, které potřebují. Příliš mnoho oprávnění můžou zpřístupnit tooattackers účtu. Příliš málo oprávnění znamená, že zaměstnanci nelze práci efektivně. [Azure na základě rolí řízení přístupu (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) pomůže vyřešit tento problém tak, že nabídka vyladění správy přístupu pro Azure.

![Přístup k zabezpečeným prostředkům ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

Pomocí RBAC, můžete v rámci týmu oddělit povinností a udělit pouze hello množství toousers přístup, že potřebují tooperform svou práci. Namísto udělení každý uživatel neomezený oprávnění v vašeho předplatného Azure nebo prostředky, můžete povolit jenom určité akce. Například použijte RBAC toolet jednoho zaměstnance spravovat virtuální počítače v předplatném, zatímco jiné můžete spravovat SQL databáze, v rámci hello stejné předplatné.

![Přístup k zabezpečeným prostředkům v Azure(RBAC)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Zabezpečení dat Azure a šifrování (Ochrana)

Jeden z hello klíče toodata ochrany v cloudu hello je monitorování účtů pro hello možné stavy, která může nastat vaše data a jaké ovládací prvky jsou k dispozici pro tento stav. Zabezpečení dat Azure a šifrování se podle doporučení hello kolem hello následující stavy dat.

- Na rest: To zahrnuje všechny informace, které kontejnerů, objektů úložiště a typy, které existují staticky na fyzickém médiu, být ho magnetické nebo optický disk.

- Na cestě: Při se přenosu dat mezi komponenty, umístění nebo programy, například přes síť hello přes service bus (z místní toocloud a naopak, včetně hybridních připojení, jako je například ExpressRoute), nebo při vstupu a výstupu proces, ho je představit jako za provozu.

### <a name="encryption--rest"></a>Šifrování @ rest

tooachieve šifrování v klidovém stavu každého z následujících hello:

Nejméně jedna z hello doporučená šifrování modely popsané v následující data tabulky tooencrypt hello podporovat.

| Modely šifrování |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Šifrování na serveru | Šifrování na serveru | Šifrování na serveru | Šifrování klienta
| Šifrování na straně serveru pomocí klíče spravované služby | Šifrování na straně serveru pomocí Customer-Managed klíčů v Azure Key Vault | Šifrování na straně serveru pomocí klíčů zákazníků spravovat místní |
| • Azure zprostředkovatelé prostředků provádět operace šifrování a dešifrování hello <br> • Microsoft spravuje hello klíče <br>• Úplné cloudu funkce | • Azure zprostředkovatelé prostředků provádět operace šifrování a dešifrování hello<br>• Zákazníka ovládací prvky klíče přes Azure Key Vault<br>• Úplné cloudu funkce | • Azure zprostředkovatelé prostředků provádět operace šifrování a dešifrování hello <br>• Zákazníka klíče místní ovládací prvky <br> • Úplné cloudu funkce| • Služeb azure nejde zobrazit dešifrovaná data <br>• Zákazníkům zachovat klíče místně (nebo v jiné zabezpečené úložiště). Klíče nejsou k dispozici tooAzure služby <br>• Snížit cloudu funkce|

### <a name="enabling-encryption-at-rest"></a>Povolení šifrování v klidovém stavu

**Identifikujte všechny umístění úložiště dat**

cílem Hello šifrování v klidovém stavu je tooencrypt všechna data. Díky tomu eliminuje hello možnost chybí důležitá data nebo všechny trvalou umístění. Výčet všech dat uložených v aplikaci. 

> [!Note] 
> Nejen "application data" nebo "PII, ale žádná data týkající se tooapplication včetně účet metadata (předplatné mapování, informace o smlouvě, PII).

Zvažte, co jste ukládá používají toostore data. Například:

- Externího úložiště (například SQL Azure Documentdb, HDInsights, Data Lake, atd.)

- Dočasné úložiště (žádné místní mezipaměť obsahující data klienta)

- Mezipaměť v paměti (možné zařadit do hello stránkovacího souboru.)

### <a name="leverage-hello-existing-encryption-at-rest-support-in-azure"></a>Využít existující šifrování hello prostřednictvím rest podpory v Azure

Pro každý úložiště, které používáte využívají hello existující šifrování v podporu Rest.

- Azure Storage: Viz [šifrování služby úložiště Azure pro Data v klidovém stavu](https://docs.microsoft.com/azure/storage/storage-service-encryption),

- SQL Azure: Najdete v části [transparentní šifrování dat (šifrování TDE), vždycky šifrovaná SQL](https://msdn.microsoft.com/library/mt163865.aspx)

- Virtuální počítač & místní disk úložiště ([Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

Pro virtuální počítač a místní úložiště disku použijte Azure Disk Encryption podporována:

IaaS

Služby virtuálních počítačů IaaS (Windows nebo Linux) by měl používat [Azure Disk Encryption](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) tooencrypt svazky obsahující data zákazníků.

PaaS v2

Služby provozované na PaaS v2 Service Fabric pomocí šifrování disku Azure pro sadu škálování virtuálního počítače [VMSS] tooencrypt jejich virtuální počítače PaaS v2.

PaaS v1

Azure Disk Encryption není aktuálně podporována u na PaaS v1. Proto je nutné použít aplikaci úroveň šifrování tooencrypt trvalá data v klidovém stavu.  To zahrnuje, ale není omezeno na data aplikací, dočasné soubory, protokoly a výpisy stavu systému.

Většina služeb mají pokusit o šifrování hello tooleverage zprostředkovatele prostředku úložiště. Některé služby mají toodo explicitní šifrování, například všechny trvalé materiál klíče (certifikáty, kořenové / hlavní klíče) musí být uložen v Key Vault.

Pokud podporujete šifrování na straně služby spravované zákazníkem klíče je potřeba toobe způsob, jak pro hello zákazníka tooget hello klíč toous. Hello podporované a doporučeno způsob toodo integrací trezoru službou klíč s Azure (AZURE). V takovém případě zákazníků můžete přidávat a spravovat jejich klíče v Azure Key Vault. Další zákazník jak toouse službou AZURE prostřednictvím [Začínáme s Key Vault](http://go.microsoft.com/fwlink/?linkid=521402).

toointegrate s Azure Key Vault, měli byste přidat kód toorequest klíč z službou AZURE v případě potřeby k dešifrování.

- V tématu [Azure Key Vault – krok za krokem](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) informace o tom, toointegrate se službou AZURE.

Pokud podporujete zákazníka spravované klíče, potřebujete tooprovide UX pro hello zákazníka toospecify které toouse Key Vault (nebo identifikátor URI trezoru klíčů).

Jak šifrování v klidovém stavu zahrnuje hello šifrování dat hostitele, infrastruktury a klientů, hello ztrátě hello klíče z důvodu selhání toosystem nebo škodlivé aktivity může znamenat, dojde ke ztrátě všech dat hello zašifrovaná. Proto je důležité, zda má šifrování na Rest řešení obnovení po havárii komplexní scénáře selhání odolné toosystem a podezřelé aktivity.

Služby, které implementují šifrování v klidovém stavu jsou obvykle přesto náchylné toohello šifrovací klíče nebo data ponechána nešifrované podobě na jednotce hello hostitele (například v hello soubor stránky hello hostitele operačního systému.) Proto služby zajistit, aby byl hello svazek hostitele pro jejich služby se šifrují. toofacilitate tento tým výpočetní aktivoval nasazení hello šifrování hostitele, který používá [Bitlocker](https://technet.microsoft.com/library/dn306081.aspx) NKP a rozšíření toohello DCM služby a agent tooencrypt hello svazek hostitele.

Většina služeb jsou implementovány na standardní virtuálních počítačích Azure. Tyto služby by měl získat [hostitele šifrování](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) automaticky při výpočetní povolí ho. Pro služby spuštěné v výpočetní spravované clustery hostitelů je povolené šifrování automaticky jako je Windows Server 2016 nasazuje.

### <a name="encryption-in-transit"></a>Šifrování během přenosu

Základní součástí strategie ochrany dat by měly být ochrany dat během přenosu. Vzhledem k tomu, že se data přenášejí a zpět z mnoha umístění, hello obecné doporučení je vždy použít data tooexchange protokoly SSL/TLS v různých umístěních. V některých případech může být vhodné tooisolate hello celý komunikační kanál mezi místní a cloudové infrastruktury pomocí virtuální privátní sítě (VPN).

Pro přesun mezi vaší místní infrastruktury a Azure data měli byste zvážit příslušná bezpečnostní opatření, například HTTPS nebo VPN.

Pro organizace, které potřebují toosecure přístup z více tooAzure nacházejí na místních pracovní stanice, použijte [Azure site-to-site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Organizace, které potřebují přístup toosecure z jedné pracovní stanice nachází místní tooAzure, použijte [Point-to-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Větších datových sad se dají přesunout přes vyhrazené vysokorychlostní propojení WAN, jako [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Pokud si zvolíte toouse ExpressRoute, můžete také šifrování dat hello hello úrovni aplikace pomocí [SSL/TLS](https://support.microsoft.com/kb/257591) nebo jiné protokoly pro zvýšení ochrany.

Pokud jsou interakci s Azure Storage prostřednictvím hello portálu Azure, všechny transakce dojít přes HTTPS. [Rozhraní API REST úložiště](https://msdn.microsoft.com/library/azure/dd179355.aspx) přes protokol HTTPS může být také použít toointeract s [Azure Storage](https://azure.microsoft.com/services/storage/) a [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organizace, které nesplní tooprotect přenášených dat budou náchylnější pro [útokům man-in-the-middle](https://technet.microsoft.com/library/gg195821.aspx), [odposlouchávání](https://technet.microsoft.com/library/gg195641.aspx)a zneužití relace. Tyto útoky lze získat přístup k datům tooconfidential hello prvním krokem.

Další informace o Azure VPN možnost přečíst článek hello [plánování a návrhu pro bránu VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Vynutit šifrování dat na úrovni souborů

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) toohelp zásady šifrování, identity a autorizace používá zabezpečit soubory a e-mailu. Azure RMS funguje napříč více zařízeními – telefony, tablety a počítače pomocí ochrany v rámci vaší organizace i mimo vaši organizaci. Tato možnost je možné, protože Azure RMS přidá úroveň ochrany, která zůstává s hello data, i když opustí prostory vaší organizace.

Pokud používáte Azure RMS tooprotect vaše soubory, používáte standardní kryptografie s plnou podporu systému [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Pokud využíváte Azure RMS pro ochranu dat, máte hello záruku toho, že hello je ochrana stále hello souborem, i když je zkopírovaný toostorage, který není pod kontrolou hello oddělení IT, třeba do cloudového úložiště. Hello stejné dochází u souborů, které sdílejí prostřednictvím e-mailu, hello soubor je chráněn jako e-mailovou zprávu tooan přílohy, s pokyny, jak tooopen hello chráněné přílohy.
Při plánování pro Azure RMS přijetí doporučujeme hello následující:

- Nainstalujte hello [aplikace sdílení RMS](https://technet.microsoft.com/library/dn339006.aspx). Tato aplikace se integruje s Office aplikace nainstalováním Office doplňku tak, aby uživatelé mohli snadno chránit soubory přímo.

- Nakonfigurujte aplikace a toosupport služby Azure RMS

- Vytvoření [vlastní šablony](https://technet.microsoft.com/library/dn642472.aspx) , podle vlastních podnikových požadavků. Příklad: šablonu pro horní tajných dat, která má být použita v všechny hlavní tajný klíč související s e-mailů.

Organizace, které jsou slabé na [klasifikace dat](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) a ochranu souborů může být náchylnější úniku toodata. Bez ochrany správný soubor organizace nebude mít tooobtain podnikových statistik, monitorování možného zneužití a zabránit toofiles škodlivým přístupem.

> [!Note]
> Další informace o Azure RMS pomocí přečtení článku hello [Začínáme se službou Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).

## <a name="secure-your-application-protect"></a>Zabezpečení vaší aplikace (Ochrana)
Sice zodpovídají za zabezpečení hello infrastruktury a platformy, které vaše aplikace běží v Azure, je vaše odpovědnosti toosecure vaše vlastní aplikace. Jinými slovy, budete potřebovat toodevelop, nasazovat a spravovat kódu aplikace a obsah zabezpečené způsobem. Bez tohoto kódu aplikace nebo obsah může být snadno napadnutelný toothreats.

### <a name="web-application-firewall-waf"></a>Firewall webových aplikací (WAF)
[Brány firewall webových aplikací (firewall webových aplikací)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) je funkce [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) poskytuje centralizovanou ochrany webových aplikací z běžných zneužití a ohrožení zabezpečení.

Brány firewall webových aplikací je na základě pravidel z hello [sady pravidel základní OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 nebo 2.2.9. Webové aplikace se čím dál častěji stávají cílem škodlivých útoků, které zneužívají běžně známé chyby zabezpečení. Jsou běžné mezi tyto zneužitím prostřednictvím injektáže SQL, skriptování mezi weby útoků tooname pár. Zabránění takové útoky v kódu aplikace může být náročné což může vyžadovat přísných Údržba, opravy a monitorování v několika vrstev topologie aplikace hello. Brány firewall centralizované webových aplikací pomáhá zkontrolujte mnohem jednodušší správu zabezpečení a nabízí lepší záruku tooapplication správci proti hrozbám nebo vniknutí. Řešení firewall webových aplikací můžete rovněž reagovat ohrožení zabezpečení tooa rychlejší podle opravy známých ohrožení zabezpečení do centrálního umístění a zabezpečení těchto jednotlivých webových aplikací. Existující application Gateway může být snadno převedený tooa webové aplikace povolena brána firewall aplikační brány.

Některé z běžných chyb webové hello které brány firewall webových aplikací chrání před zahrnuje:

- Ochrana před útoky prostřednictvím injektáže SQL.

- Ochrana před skriptováním mezi weby.

- Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.

- Ochrana před narušením protokolu HTTP.

- Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.

- Ochrana před roboty, prohledávacími moduly a skenery.

- Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)

> [!Note]
> Podrobnější seznam pravidel a jejich ochrany naleznete v následujících hello [základní sady pravidel](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure také poskytuje několik snadno použitelné funkce toohelp zabezpečit příchozí a odchozí přenosy pro vaši aplikaci. Azure pomáhá zákazníkům zabezpečit svůj kód aplikace poskytnutím externě k dispozici také funkce tooscan webové aplikace pro chyby zabezpečení.

- [Zabezpečte svoji webovou aplikaci pomocí různých způsobů ověřování a autorizace](https://docs.microsoft.com/azure/app-service-web/web-sites-authentication-authorization)

    - [Nastavení ověřování Azure Active Directory pro vaši aplikaci](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)


- [Zabezpečení provozu tooyour aplikace povolením Transport Layer Security (TLS/SSL) - HTTPS](https://docs.microsoft.com/azure/app-service-web/web-sites-configure-ssl-certificate)

    - [Vynutit veškerý příchozí provoz přes připojení HTTPS](http://microsoftazurewebsitescheatsheet.info/)

  - [Povolit zabezpečení striktní přenosu (HSTS)](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)


- [Omezit přístup tooyour aplikace tak, že IP adresa klienta](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Omezit přístup tooyour aplikace tak, že chování klienta - požadavek četnost a souběžnost](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Kontrola kódu webové aplikace pro chyby pomocí Tinfoil Security Scanning.](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [Konfigurace TLS vzájemné ověřování toorequire klientské certifikáty tooconnect tooyour webové aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth)

- [Konfigurovat certifikát klienta pro použití z vaší aplikace toosecurely připojení tooexternal prostředků](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Odebrání nástrojů pro standardní server hlavičky tooavoid z tímto způsobem vaší aplikace](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Bezpečně připojte vaše aplikace s prostředky v privátní síti prostřednictvím sítě VPN Point-To-Site](https://docs.microsoft.com/azure/app-service-web/web-sites-integrate-with-vnet)

- [Bezpečně připojte vaše aplikace s prostředky v privátní síti používáte hybridní připojení](https://docs.microsoft.com/azure/app-service-web/web-sites-hybrid-connection-get-started)

Azure App Service používá hello stejné Antimalwarové řešení používá Azure Cloud Services a virtuálních počítačů. toolearn Další informace najdete v tooour [antimalwarových dokumentaci](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>Zabezpečení sítě (Ochrana)
Microsoft Azure obsahuje robustní sítě toosupport infrastruktury, aplikace a požadavky na připojení služby. Připojení k síti je možné mezi prostředky, které jsou umístěné v Azure, mezi místními a Azure hostované prostředky a tooand z hello Internet a Azure.

Hello [Azure síťové infrastruktury](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) umožňuje toosecurely můžete připojit prostředky Azure tooeach jiné s [virtuální sítě (virtuální sítě)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace hello cloudu Azure síť vyhrazený tooyour předplatného. Můžete propojit virtuální sítě tooyour místní sítě.

![Zabezpečení sítě (Ochrana)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Pokud potřebujete řízení úrovně přístupu k základní síti (podle IP adresy a hello TCP nebo UDP protokoly) a pak můžete použít [skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg). Skupina zabezpečení sítě (NSG) je základní stavových paketů filtrování brány firewall a umožňuje toocontrol přístup na základě [5 řazené kolekce členů](https://www.techopedia.com/definition/28190/5-tuple).

Azure sítě podporuje hello možnost toocustomize hello směrování chování pro síťový provoz na virtuálních sítí Azure. To provedete tak, že nakonfigurujete [trasy definované uživatelem](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) v Azure.

[Vynuceného tunelování](https://www.petri.com/azure-forced-tunneling) je mechanismus, který můžete použít tooensure, které vaše služby nejsou povoleny tooinitiate toodevices připojení na hello Internetu.

Azure podporuje vyhrazený propojení WAN připojení tooyour do místní sítě a virtuální síť Azure s [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). Hello propojení mezi Azure a váš web používá vyhrazené připojení, které nejde přes hello veřejného Internetu. Pokud vaše aplikace Azure běží v několik datových center, můžete použít [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) tooroute žádosti od uživatelů inteligentně mezi instancemi aplikace hello. Také je možné směrovat provoz tooservices není spuštěna v Azure, pokud jsou přístupné z Internetu hello.

## <a name="virtual-machine-security-protect"></a>Virtuální počítač zabezpečení (Ochrana)

[Virtuální počítače Azure](https://docs.microsoft.com/azure/virtual-machines/) umožňuje nasadit širokou škálu řešení výpočetní agilní způsobem. Díky podpoře systémů Windows, Linux, SQL Server, Oracle, IBM, SAP a Azure BizTalk Services můžete nasadit jakoukoli úlohu a jakýkoli jazyk skoro v každém operačním systému.

V Azure, můžete použít [antimalwarový software](https://docs.microsoft.com/azure/security/azure-security-antimalware) od dodavatelů zabezpečení, jako je Microsoft, Symantec, Trend Micro a Kaspersky tooprotect virtuálních počítačů ze škodlivých souborů, adwaru a dalšími hrozbami.

Antimalware od Microsoftu pro Azure Cloud Services a virtuální počítače je funkce ochrany v reálném čase, který pomáhá identifikovat a odstraňovat viry, spyware a další škodlivý software. Antimalware od Microsoftu poskytuje konfigurovat výstrahy, když známé tooinstall pokusy o škodlivého nebo nežádoucího softwaru, sám sebe nebo spustit v Azure systémech.

[Zálohování Azure](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) škálovatelné řešení, které chrání data aplikací s nulové kapitálovými investicemi a minimálními provozní náklady. Chyby aplikací můžou poškodit vaše data a lidské omyly zase můžou způsobit chyby v aplikacích. S Azure Backup jsou chráněné virtuální počítače s Windows a Linux.

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) pomáhá orchestraci replikace, převzetí služeb při selhání a obnovení úloh a aplikací, aby byly k dispozici ze sekundární lokality Pokud primární lokalita ocitne mimo provoz.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>Zajištění dodržování předpisů: cloudových služeb z důvodu kontrolní seznam opatrností (Ochrana)

Microsoft vyvinul [hello cloudové služby kvůli opatrností kontrolní seznam](https://aka.ms/cloudchecklist.download) toohelp organizace náležitou opatrností stejně jako se zvažte přesunutí toohello cloudu. Poskytuje strukturu pro organizaci všech velikost a typ – privátní podnikům a organizacím veřejných, včetně government na všech úrovních a neziskové organizace – tooidentify vlastní výkonu, služby, Správa dat a cíle zásad správného řízení a požadavky. To jim umožňuje toocompare hello nabídky poskytovatelů služeb jiný cloud, nakonec které tvoří základ hello pro smlouvy o cloudových služeb.

kontrolní seznam Hello poskytuje rozhraní, aby klauzule pomocí klauzule s novou standardní mezinárodní pro cloudové služby smlouvy, ISO/IEC 19086. Tento standard poskytuje sada důležité informace týkající se organizace toohelp je rozhodnutí o přijetí cloudu a vytvořte běžné základů pro porovnání nabídek služeb cloudu.

kontrolní seznam Hello zvýší úroveň důkladně vetted přesunutí toohello cloudu, poskytuje strukturovaných pokyny a konzistentní, opakovatelných přístup pro výběr poskytovatele cloudové služby.

Přijetí cloudu je již jednoduše technologie rozhodnutí. Protože touch kontrolní seznam požadavků na každý aspekt organizace, budou sloužit tooconvene všechny klíče interní-rozhodují – hello ředitel IT a ředitel zabezpečení informací, jakož i právní, riziko Odborníci v oblasti správy, nákup a dodržování předpisů. Tím se zvyšuje efektivita hello hello rozhodovací proces a základů rozhodnutí v zvuk odůvodnění, což snižuje pravděpodobnost hello nepředpokládaného roadblocks tooadoption.

Kromě toho hello kontrolní seznam:

- Zpřístupní klíče diskusní témata pro vedoucí pracovníky na hello začátku procesu přijetí hello cloudu.

- Podporuje důkladné obchodní diskusí o předpisy a hello cíle organizace pro ochrany osobních údajů, identifikovatelné osobní údaje (PII) a data zabezpečení.

- Pomáhá organizacím identifikovat všechny potenciální problémy, které mohou ovlivnit projekt cloudové.

- Poskytuje sadu otázky se hello konzistentní stejných podmínek, definice, metriky a výsledek pro každého zprostředkovatele toosimplify hello proces porovnání nabídek z poskytovatele různých cloudových služeb.

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Azure ověření zabezpečení infrastruktury a aplikací (zjistit)

[Zabezpečení provozu Azure](https://docs.microsoft.com/azure/security/azure-operational-security) odkazuje toohello služeb, ovládací prvky a funkce dostupné toousers pro ochranu svá data, aplikace a dalších prostředků ve službě Microsoft Azure.

![ověření zabezpečení (zjistit)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Zabezpečení provozu Azure je založený na rozhraní, které zahrnuje hello poznatky získané při různých možnostech, které jsou jedinečné tooMicrosoft, včetně hello Microsoft SDL Security Development Lifecycle (), hello Centre odpovědi zabezpečení Microsoft program a hloubkové povědomí o šířku threat počítačové bezpečnosti hello.

### <a name="microsoft-operations-management-suiteoms"></a>Microsoft operations management suite(OMS)

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) je hello řešení pro správu IT pro hello hybridní cloud. Použít samostatně nebo tooextend, které vám vaše stávající nasazení produktu System Center, OMS hello maximální flexibilitu a řízení pro správu cloudové infrastruktury.

![Microsoft operations management suite(OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

S OMS můžete spravovat libovolnou instancí ve všech cloudu, včetně místní, Azure, AWS, Windows Server, Linux, VMware a OpenStack, při nižších nákladech, než konkurenční řešení. Vytvořené pro první cloudu hello, world, OMS nabízí nový způsob toomanaging při vaší organizace, která je hello nejrychlejší, nákladově nejefektivnější způsob nové obchodní toomeet vyzve a zohlednit nové úlohy, aplikace a cloudové prostředí.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) poskytuje služby monitorování pro OMS získáváním dat ze spravovaných prostředků do centrálního úložiště. Tato data můžou zahrnovat události, údaje o výkonu nebo vlastní data poskytnutá prostřednictvím hello rozhraní API. Jakmile získány, je k dispozici pro výstrahy, analýzu a export dat hello.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Tato metoda vám umožní tooconsolidate data z různých zdrojů, takže můžete kombinovat data ze služeb Azure s vaší stávající místní prostředí. Také jasně odděluje hello shromažďování dat hello od hello akce prováděné na tato data tak, aby všechny akce jsou k dispozici tooall druhy dat.

### <a name="azure-security-center"></a>Azure security center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) pomáhá zabránit, zjišťovat a odpovědět toothreats s lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Security Center analyzuje stav zabezpečení hello vaše prostředky Azure tooidentify potenciální ohrožení zabezpečení. Seznam doporučení vás provede procesem konfigurace potřebných kontrol hello.

Příklady obsahují:

- Zřizování antimalwaru, toohelp identifikovat a odebrat škodlivý software

- Konfigurace sítě zabezpečení skupiny a pravidel toocontrol provoz tooVMs

- Zřizování firewallů webových aplikací toohelp bránit proti útokům, které cílí na vaše webové aplikace

- Nasazení chybějících aktualizací systému

- Adresování konfigurací operačního systému, které neodpovídají hello doporučená standardních hodnot

Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, hello sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall. Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení. Příklady zahrnují zjišťování následujících situací:

- Ohrožené virtuální počítače komunikaci s známé škodlivé IP adresy

- Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows

- Útoky hrubou silou na virtuální počítače

- Výstrahy zabezpečení z integrovaných antimalwarových programů a bran firewall

### <a name="azure-monitor"></a>Azure monitorování

[Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) poskytuje tooinformation ukazatele na konkrétní typy prostředků. Nabízí vizualizace, dotaz, směrování, výstrahy, automatické škálování a automatizace na data z hello infrastrukturu Azure (protokol aktivit) a každý jednotlivých prostředků Azure (diagnostických protokolů).

Cloudové aplikace jsou komplexní s mnoha přesunutí částmi. Monitorování poskytuje tooensure data, která vaše aplikace zůstává nahoru a spuštěn v dobrém stavu. Pomáhá také můžete toostave vypnout potenciální problémy a řešení potíží s uplynulou těch, které jsou.

![Azure monitorování](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) kromě toho můžete použít monitorování hlubšímu porozumění toogain data o vaší aplikaci. Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.

Auditování zabezpečení sítě je důležité pro zjišťování chyb zabezpečení sítě a zajištění dodržování zabezpečení IT a modelu regulačních zásad správného řízení. Pomocí zobrazení skupiny zabezpečení je může načíst skupinu zabezpečení sítě a zabezpečení pravidla hello nakonfigurované, stejně jako hello pravidla efektivní zabezpečení. Hello seznam pravidel použít můžete určit, že hello porty, které jsou otevřené a ss sítě ohrožení zabezpečení.

### <a name="network-watcher"></a>Sledovací proces sítě

[Sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) je místní služba, která vám umožní toomonitor a diagnostikovat podmínky na úrovni sítě v, do a z Azure. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure. Tato služba obsahuje zachytáváním paketů, další směrování, IP tok ověření, zobrazení skupiny zabezpečení, tok protokolů NSG. Scénář úrovně monitorování obsahuje zobrazení tooend end síťovým prostředkům v monitorování kontrast tooindividual sítě prostředků.

### <a name="storage-analytics"></a>Analýza služby Storage

[Analytika úložiště](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) můžete ukládat metriky, které zahrnují data statistiky a kapacity agregované transakcí týkající se požadavků tooa úložiště služby. Transakce jsou v úrovni operaci hello rozhraní API, a také na úrovni služby úložiště hello a hlásí kapacity na úrovni služby úložiště hello. Metriky dat můžete použít tooanalyze využití služby úložiště, diagnostikovat problémy s požadavků na službu úložiště hello a tooimprove hello výkon aplikací, které používají služby přístup.

### <a name="application-insights"></a>Application insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) je rozšiřitelný služba Správa výkonu aplikace (APM) pro vývojáře, kteří ve více platformách. Použít toomonitor za provozu webové aplikace. Automaticky zjišťuje anomálie ve výkonu. Zahrnuje výkonné analytics nástroje toohelp diagnostikovat problémy a toounderstand co uživatelé dělají s vaší aplikací. Je navržen toohelp neustále průběžně zlepšují výkon a použitelnost. Funguje pro aplikace na širokou škálu platforem včetně .NET, Node.js a J2EE, hostovaný místně nebo v cloudu hello. Se integruje s váš proces devOps a má připojovací body tooa různé nástroje pro vývoj.

Monitoruje tyto parametry:

- **Frekvence požadavků, doby odezvy a míra selhání** – Zjistěte, které stránky jsou nejoblíbenější a v kterou denní dobu a kde jsou vaši uživatelé. Zjistíte, která stránka si vede nejlépe. Pokud se při zvýšení počtu požadavků zvýší i doba odezvy a míra selhání, máte pravděpodobně potíže s prostředky.

- **Míra závislosti, doby odezvy a míra selhání** – Zjistěte, jestli vás nezpomalují externí služby.

- **Výjimky** – analyzovat statistiku hello agregován, nebo vyberte určité instance a přejít k podrobnostem hello trasování zásobníku a související požadavky. Hlásí se výjimky serveru i prohlížeče.

- **Zobrazení a načítání stránek** – Tyto informace hlásí prohlížeče uživatelů.

- **Volání AJAX z webové stránky** -sazby, doby odezvy a selhání sazby.

- **Počty uživatelů a relací.**

- **Čítače výkonu** ze serverových počítačů s Windows nebo Linuxem, jako je třeba CPU, paměť a využití sítě.

- **Diagnostika hostitele** z Dockeru nebo Azure.

- **Protokoly trasování diagnostiky** z vaší aplikace – umožňují zjistit korelaci mezi požadavky a událostmi trasování.

- **Vlastní události a metriky** napíšete sami v kódu hello klienta nebo serveru, tootrack obchodní události, jako je například položek prodaných nebo hry won.
Hello infrastrukturu aplikace obvykle tvoří celá řada komponent, může být virtuální počítač, účet úložiště a virtuální síti, nebo webové aplikace, databáze, databázový server a služby 3. stran. Tyto komponenty nevidíte jako samostatné entity, ale jako související a vzájemně provázané části jedné entity. Chcete toodeploy, spravovat a monitorovat jako skupinu. [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) vám umožní toowork s hello prostředky ve vašem řešení jako se skupinou.

Můžete nasadit, aktualizovat nebo odstranit všechny hello prostředky pro vaše řešení v rámci jediné koordinované operace. Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako například v testovacím, přípravném nebo produkčním prostředí. Resource Manager poskytuje zabezpečení, auditování a označování funkce toohelp spravovat prostředky po nasazení.

**Hello výhody použití Resource Manager**

Resource Manager poskytuje několik výhod:

- Můžete nasadit, spravovat a monitorovat všechny hello prostředky pro vaše řešení jako skupina, nikoli zpracovávat jednotlivě.

- Můžete opakovaně nasadit řešení v celém hello životního cyklu a mít přitom jistotu, že vaše prostředky jsou nasazeny v konzistentním stavu.

- Infrastrukturu můžete spravovat pomocí deklarativních šablon místo skriptů.

- Můžete definovat hello závislosti mezi prostředky, takže se nasadí ve správném pořadí hello.

- Služby tooall řízení přístupu můžete použít ve vaší skupině prostředků, protože do platformy pro správu hello je nativně integrováno řízení přístupu na základě Role (RBAC).

- Můžete použít značky tooresources toologically uspořádat všechny prostředky hello ve vašem předplatném.

- Můžete zpřehlednit fakturaci vaší organizace zobrazením náklady pro skupinu prostředků, sdílení hello stejnou značku.

> [!Note]
> Resource Manager poskytuje nový způsob toodeploy a správy vašich řešení. Pokud jste použili hello dřívější model nasazení a chcete toolearn o změnách hello naleznete [nasazení Resource Manager principy a nasazení classic](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Další kroky

Další informace o zabezpečení načtením některá témata s našimi podrobné zabezpečení:

- [Auditování a protokolování](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Kybernetická](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Návrh a provozního zabezpečení](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Šifrování](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Správu identit a přístupu](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Zabezpečení sítě](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Řízení rizik](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)
