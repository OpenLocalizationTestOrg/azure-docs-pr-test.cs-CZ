---
title: "aaaIsolation ve veřejném cloudu Azure hello | Microsoft Docs"
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
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 271e5f0d00abcfd404ce6c50cfb7d1ac26360c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="isolation-in-hello-azure-public-cloud"></a>Izolace ve veřejném cloudu Azure hello
##  <a name="introduction"></a>Úvod
### <a name="overview"></a>Přehled
tooassist aktuální a potenciální zákazníci Azure pochopit a využívat hello různé související se zabezpečením možnosti dostupné v a okolního hello platformy Azure, společnost Microsoft vyvinula řadu dokumenty White Paper, zabezpečení přehledy, osvědčené postupy a Kontrolní seznamy.
témata Hello rozsahu z hlediska spektra a hloubky a jsou pravidelně aktualizovány. Tento dokument je součástí této řady dle souhrnu v následující abstraktní části hello.

### <a name="azure-platform"></a>Platformy Azure
Azure je platforma otevřené a flexibilní cloudové služby, která podporuje hello nejširší výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení. Můžete například provést následující věci:
- Spusťte Linux kontejnery s integrace Dockeru;
- Vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js; a
- Sestavení back EndY pro iOS, Android a Windows zařízení.

Microsoft Azure podporuje hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti.

Když sestavení, nebo migrovat prostředky IT, poskytovatele služeb veřejného cloudu, se spoléhat na dané organizace dalo tooprotect aplikace a data s hello služeb a hello ovládací prvky poskytují toomanage hello zabezpečení vašeho cloudu prostředky.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům podle jejich potřeb zabezpečení. Kromě toho Azure poskytuje širokou škálu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vašich nasazení. Tento dokument vám tyto požadavky splňují pomůže.

### <a name="abstract"></a>Abstraktní

Microsoft Azure umožňuje toorun aplikace a virtuální počítače (VM) na sdílený fyzické infrastruktuře. Jedna z aplikací toorunning prvotní hospodářského motivace hello v cloudovém prostředí je hello možnost toodistribute hello náklady sdílených prostředků mezi více zákazníků. Tento postup víceklientský ke zlepšení efektivity multiplexní prostředky mezi různorodých zákazníků s nízkými náklady. Bohužel se navíc přidává hello riziko sdílení fyzických serverů a dalších toorun prostředky infrastruktury citlivých aplikací a virtuálních počítačů, které může patřit tooan libovolný a potenciálně uživateli se zlými úmysly.

Tento článek popisuje, jak Microsoft Azure poskytuje izolaci proti škodlivým i škodlivý uživatele a funguje jako vodítko pro architekturu řešení cloud tím, že nabízí různé možnosti tooarchitects izolace. Tento dokument white paper se zaměřuje na hello technologie platformy Azure a ovládacích prvků zabezpečení zákazníka přístupem a nebude pokoušet tooaddress SLA, ceny modely a aspekty postupem DevOps.

## <a name="tenant-level-isolation"></a>Úroveň izolace klienta
Jeden z hello primární výhody technologie cloud computing je koncept sdílené běžné infrastruktury napříč řady zákazníků současně, což tooeconomies měřítka. Tento koncept se nazývá víceklientská architektura. Microsoft funguje nepřetržitě tooensure, hello víceklientské architektuře Microsoft Azure Cloud podporuje standardy zabezpečení, utajení, ochrany osobních údajů, integrita a dostupnost.

Na pracovišti povolenou podporu cloudu hello klienta lze definovat jako klient nebo organizace, která vlastní a spravuje konkrétní instanci dané cloudové služby. S platformou identity hello poskytované Microsoft Azure klient je jednoduše vyhrazenou instancí služby Azure Active Directory (Azure AD), vaše organizace obdrží a vlastní, když se přihlásí ke cloudové službě Microsoftu.

Každý adresář služby Azure AD je oddělený od ostatních adresářů služby Azure AD. Stejně jako je podniková kancelářská budova je konkrétní tooonly zabezpečený prostředek vaší organizace, adresář služby Azure AD se také navrženou toobe o zabezpečený prostředek k použití výhradně vaší organizace. Architektura služby Azure AD Hello izoluje zákazníka dat a identity informace z společně identitě. To znamená, že se uživatelé a správci jednoho adresáře služby Azure AD nemohou dostat – ať už omylem nebo záměrně – k datům v jiném adresáři.

### <a name="azure-tenancy"></a>Azure klientů
Azure klientů (předplatné Azure) odkazuje vztah "zákazníka nebo billing" tooa a jedinečný [klienta](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) v [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Úrovně izolaci klientů v Microsoft Azure je dosaženo pomocí služby Azure Active Directory a [ovládací prvky založené na rolích](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) nabízené ho. Každé předplatné Azure je přidružen jeden adresář Azure Active Directory (AD).

Uživatelé, skupiny a aplikací z adresáře můžete spravovat prostředky v hello předplatného Azure. Můžete přiřadit tyto přístupová práva pomocí hello portálu Azure, nástroje příkazového řádku Azure a rozhraní API pro správu Azure. Klient služby Azure AD je logicky izolované pomocí hranice zabezpečení, aby žádné zákazníka lze zobrazit nebo ohrozit společné klientů, závadně nebo náhodně. Azure AD je spuštěna na serverech "holý počítač" izolované v segmentu oddělené sítě, kde filtrování paketů na úrovni hostitele a brány Windows Firewall blokovat nežádoucí připojení a provozu.

- Toodata přístupu ve službě Azure AD vyžaduje ověření uživatele prostřednictvím [služby tokenů zabezpečení (STS)](https://docs.microsoft.com/azure/app-service-web/web-sites-authentication-authorization). Informace o existence hello uživatele, povoleném stavu a role je používán hello autorizace systému toodetermine, zda hello požadovaný přístup toohello cíl klient není autorizován pro tohoto uživatele v této relaci.

![Azure klientů](./media/azure-isolation/azure-isolation-fig1.png)


- Klienti používají diskrétní kontejnery a neexistuje žádný vztah mezi tyto.

- Žádný přístup mezi klienty, pokud ho správce klienta uděluje prostřednictvím federaci nebo zřizování uživatelských účtů z jiných klientů.

- Fyzický přístup tooservers, která tvoří služby hello Azure AD a přímý přístup tooAzure AD na back-end systémy, je omezen.

- Azure AD Uživatelé mají žádný přístup k prostředkům toophysical nebo umístění, a proto není možné pro je toobypass hello logické RBAC zásad kontroly uvádí následující.

Pro diagnostiku a potřebuje údržby je provozní model, který využívá systém zvýšení oprávnění za běhu vyžaduje a používat. Azure AD Privileged Identity Management (PIM) zavádí koncepci hello oprávněné správce. [Oprávněné admins](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) by měla být uživatelům, kteří potřebují privilegovaný přístup teď a potom, ale ne každý den. Hello role je neaktivní, dokud hello uživatel potřebuje přístup, pak se dokončit proces aktivace a stane aktivní správce po předem určenou dobu.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Azure Active Directory je hostitelem každého klienta v jeho vlastní chráněné kontejneru s tooand zásad a oprávnění v rámci kontejneru hello výhradně vlastněna a řízena hello klienta.

Koncept Hello kontejnerů klienta je úzce ingrained v adresářové službě hello na všechny vrstvy, z portálů všechny hello způsob toopersistent úložiště.

I v případě, že metadata od víc klientů služby Azure Active Directory se ukládají na hello stejný fyzický disk, neexistuje žádný vztah mezi hello kontejnerů než co je definováno hello adresářová služba, která zase závisí hello správce klienta.

### <a name="azure-role-based-access-control-rbac"></a>Řízení přístupu Azure na základě rolí (RBAC)
[Azure na základě rolí řízení přístupu (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) pomáhá vám tooshare různé součásti, které jsou k dispozici v rámci předplatného Azure tím, že poskytuje vyladění správy přístupu pro Azure. Azure RBAC vám umožní toosegregate povinností v rámci vaší organizace a udělení přístupu na základě na to, co uživatelé musí tooperform svou práci. Namísto udělení každý uživatel neomezený oprávnění v předplatného Azure nebo prostředky, můžete povolit jenom určité akce.

Azure RBAC má tři základní rolí, které se vztahují tooall typy prostředků:

- **Vlastník** má úplný přístup k prostředkům tooall včetně tooothers přístup správné toodelegate hello.

- **Přispěvatel** můžete vytvářet a spravovat všechny typy prostředků Azure, ale nelze udělit přístup tooothers.

- **Čtečka** můžete zobrazit stávající prostředky Azure.

![Řízení přístupu Azure na základě rolí](./media/azure-isolation/azure-isolation-fig3.png)

Hello zbytek hello RBAC role v Azure povolit správu konkrétních prostředků Azure. Například hello role Přispěvatel virtuálních počítačů umožňuje toocreate hello uživatele a spravovat virtuální počítače. Nedává je toohello přístup k virtuální síti Azure nebo hello podsítě, která hello virtuální počítač připojí k.

[Předdefinované role RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) seznamu hello role v Azure k dispozici. Určuje hello operace a že každý předdefinovaná role uděluje toousers oboru. Pokud se díváte toodefine vlastní role pro ještě větší kontrolu, najdete v části Jak toobuild [vlastní role v Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Některé další možnosti pro Azure Active Directory patří:
- Azure AD umožňuje aplikacím tooSaaS jednotné přihlašování, bez ohledu na to, kde jsou hostované. Některé aplikace jsou federované pomocí Azure AD, jiné používají jednotné přihlašování pomocí hesla. Federované aplikace také podporují zřizování uživatelů a [heslo překlenutí vyrovnávací paměti](https://www.techopedia.com/definition/31415/password-vault).

- Přístup k toodata v [Azure Storage](https://azure.microsoft.com/services/storage/) je řízena pomocí ověřování. Každý účet úložiště má primární klíč ([klíč účtu úložiště](https://docs.microsoft.com/azure/storage/storage-create-storage-account), nebo SAK) a sekundární tajný klíč (hello sdílený přístupový podpis nebo SAS).

- Azure AD poskytuje Identity jako služby pomocí federování pomocí [Active Directory Federation Services](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), synchronizace a replikace s místních adresářů.

- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) je hello služba služby Multi-Factor authentication, která vyžaduje tooverify uživatelé přihlášení pomocí mobilní aplikace, telefonního hovoru nebo textové zprávy. Použít s Azure AD toohelp zabezpečené místní prostředky pomocí ověřování Azure Multi-Factor Authentication server hello a také s vlastní aplikace a adresáře pomocí hello SDK.

- [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) umožňuje připojení k doméně služby Active Directory tooan virtuální počítače Azure bez nasazení řadiče domény. Můžete přihlásit toothese virtuální počítače s svoje podnikové přihlašovací údaje služby Active Directory a spravovat virtuální počítače připojené k doméně pomocí zásad skupiny směrné plány zabezpečení tooenforce na všechny vaše virtuální počítače Azure.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) poskytuje službu vysoce dostupný globální identity správy určených aplikací, která je škálovatelná toohundreds milionů identit. Dá se integrovat do mobilních i webových platforem. Vaši uživatelé mohou zaregistrovat v tooall aplikace prostřednictvím přizpůsobitelné pomocí svých účtů na sociálních nebo vytváření přihlašovacích údajů.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Izolace z Microsoft Administrators & odstranění dat
Společnost Microsoft má silné míry tooprotect data z nevhodný přístup nebo použití neoprávněnou osobou. Tyto provozní postupy a ovládací prvky jsou zajišťované hello [Online služby podmínky](http://aka.ms/Online-Services-Terms), které nabízejí smluvními závazky, které řídí přístup k datům tooyour.

-   Microsoft technici nemají výchozí přístup tooyour data v cloudu hello. Místo toho že je uděleno oprávnění, pod správu dohledu, pouze v případě potřeby. Tento přístup je pečlivě řídí a protokolovat a odvolat, když už ho nepotřebují.

-   Microsoft může najímáme jiné společnosti tooprovide omezené služby jeho jménem. Subdodavatelů může přístup dat pouze toodeliver hello oddělení služeb zákazníkům, které si najímáme je tooprovide a mají zakázáno používat za žádným jiným účelem. Navíc jsou smluvně vázané toomaintain hello důvěrnost údajů naše zákazníky.

Služby pro firmy s auditované certifikace, jako jsou ISO/IEC 27001 pravidelně ověřit společností Microsoft a schválené auditní podnicích, které provedení ukázka audity tooattest této přístup pouze pro účely legitimní podnikání. Kdykoli a z jakéhokoli důvodu jsou k dispozici vždy zákaznických údajů.

Pokud odstraníte všechna data, odstraní Microsoft Azure hello data, včetně všech v mezipaměti nebo záložní kopie. Pro služby ve oboru dojde k odstranění této do 90 dnů po skončení období uchování hello hello. (V oboru služby jsou definovány v části podmínky pro zpracování dat hello naše [Online služby podmínky](http://aka.ms/Online-Services-Terms).)

Pokud je disk disková jednotka používané pro úložiště vyskytne selhání hardwaru, je bezpečně [vymazat nebo zničeno](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) než Microsoft vrátí toohello výrobce pro nahrazení nebo oprava. Hello data na jednotce hello přepsána tooensure, který hello data nelze obnovit, a to jakýmkoli způsobem.

## <a name="compute-isolation"></a>Výpočetní izolace
Microsoft Azure poskytuje různé cloudové výpočetní služby, které zahrnují široký výběr výpočetních instancích & služby, které je možné škálovat nahoru a dolů automaticky toomeet hello potřebám vaší aplikace nebo enterprise. Tyto instance výpočetní a služby nabízejí izolaci na více úrovních toosecure data zároveň zachovávají hello flexibilitu při konfiguraci této poptávky zákazníků.

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Technologie Hyper-V a kořenové OS izolaci mezi kořenové virtuálních počítačů & hostované virtuální počítače
Výpočetní platformě Azure je založena na virtualizaci počítače, což znamená, že veškerý kód zákazníka provede ve virtuálním počítači technologie Hyper-V. Na každém uzlu Azure (nebo koncový bod sítě) je Hypervisor, který spouští přímo přes hello hardwaru a rozděluje uzlu do proměnné číslo z hostované virtuální počítače (VM).


![Technologie Hyper-V a kořenové OS izolaci mezi kořenové virtuálních počítačů & hostované virtuální počítače](./media/azure-isolation/azure-isolation-fig4.jpg)


Každý uzel má také jeden speciální kořenové virtuální počítač, který se spouští hello hostitelským operačním systémem. Kritické hranic je hello izolace hello kořenových virtuálních počítačů z hello hostované virtuální počítače a hello hostované virtuální počítače od sebe navzájem, spravuje hello hypervisoru a hello kořenový operačního systému. párování hypervisoru/root OS Hello využívá společnosti Microsoft desetiletí prostředí zabezpečení operačního systému a novější learning od společnosti Microsoft Hyper-V, tooprovide silnou izolaci virtuálních počítačů hosta.

Hello platformy Azure používá virtualizovaném prostředí. Uživatelské instance fungovat jako samostatné virtuální počítače, které nemají přístup tooa fyzického hostitelského serveru a tato izolace je požadováno pomocí fyzického procesoru úrovně oprávnění (prstenec-0 nebo prstenec-3).

Prstenec 0 je hello nejvíce privilegovaných a 3 je alespoň hello. Hello hostovaný operační systém běží v 1 menší privilegovaných prstenec a spouští aplikace hello nejnižšími oprávněními prstenec 3. Tato virtualizace fyzické prostředky vede jasně oddělené tooa mezi hostovaný operační systém a hypervisoru, což vede k další bezpečnostní oddělení mezi dvěma hello.

Hello Azure hypervisoru chová jako micro jádra a předává všechny požadavky na hardware přístup z hosta virtuálního počítače toohello hostitele pro zpracování pomocí sdílené paměti rozhraní s názvem VMBus. To zabrání uživatelům v získání přímého přístupu pro čtení, zápisu a spouštění toohello systému a snižuje riziko hello sdílení systémové prostředky.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Pokročilé algoritmus umístění virtuálních počítačů a ochranu před útoky straně kanálu
Všechny cross-VM útok zahrnuje dva kroky: umístění virtuálních počítačů řízenou nežádoucí osoba na hello stejné hostitele jako jeden z hello postižené virtuální počítače a potom před nedodržením tooeither hranic izolace hello ukrást citlivé postižené informace nebo ovlivnit její výkon pro greed nebo vandalismu. Microsoft Azure poskytuje pomocí služby pokročilé algoritmus umístění virtuálních počítačů a ochranu před útoky kanál všechny známé straně, které jsou včetně virtuálních počítačů aktivní sousedním ochrany na oba kroky.

### <a name="hello-azure-fabric-controller"></a>Hello Kontroleru prostředků infrastruktury Azure
Hello Kontroleru prostředků infrastruktury Azure zodpovídá za přidělování prostředků infrastruktury tootenant úlohy a spravuje jednosměrný komunikaci z počítačů toovirtual hello hostitele. Hello virtuálních počítačů uvádění algoritmus řadiče prostředků infrastruktury Azure hello je složitější a téměř nemožné toopredict jako fyzické úrovni hostitele.

![Hello Kontroleru prostředků infrastruktury Azure](./media/azure-isolation/azure-isolation-fig5.png)

Hello Azure hypervisoru vynucuje paměti a proces oddělení mezi virtuálními počítači a bezpečně směruje síťový provoz tooguest operačního systému klientů. Tím se eliminuje možnost a straně kanál útoku na úrovni virtuálního počítače.

V Azure, hello kořenová virtuálního počítače je speciální: jeho operačním systémem, posílené názvem kořenové hello operačního systému hostujícím agenta prostředků infrastruktury (IM). DM se používají v agenty hostů toomanage (GA) zapnout v rámci hostované operační systémy na zákazníka virtuálních počítačů. DM také spravovat uzly úložiště.

Hello kolekce Azure hypervisoru kořenový operačního systému nebo DM a zákazník virtuálních počítačů nebo plynu se skládá z výpočetního uzlu. DM spravuje kontroleru prostředků infrastruktury (FC), která již existuje mimo uzly výpočetního prostředí a úložiště (výpočetní clustery a clustery úložiště jsou spravovány samostatné FCs). Pokud zákazník aktualizací konfigurační soubor jejich aplikace je spuštěna, hello FC komunikuje s hello FA, poté kontaktuje plynu, který upozornit hello aplikace hello změny konfigurace. V případě hello selhání hardwaru bude hello FC automaticky vyhledat dostupného hardwaru a restartujte hello virtuálních počítačů existuje.

![Kontroler prostředků infrastruktury Azure](./media/azure-isolation/azure-isolation-fig6.jpg)

Je jednosměrná komunikace od agentů tooan Kontroleru prostředků infrastruktury. Hello agent implementuje službou pomocí protokolu SSL, která pouze odpovídá toorequests z hello řadiče. Nelze zahájit, připojení toohello řadiče nebo jiné privilegované interní uzly. Hello FC zpracovává všechny odpovědi, jako kdyby nedůvěryhodné.


![Kontroleru prostředků infrastruktury](./media/azure-isolation/azure-isolation-fig7.png)

Izolace rozšiřuje z hello kořenové virtuálních počítačů z hostů virtuálních počítačů a hello hostované virtuální počítače od sebe navzájem. Výpočetní uzly jsou i izolované od uzlů úložiště v zájmu lepší ochrany.


Hello hypervisoru a operační systém hostitele hello poskytují síťový paket – filtry toohelp zajistil, že nedůvěryhodné virtuální počítače nelze generovat falešné provoz nebo přijímat přenosy, které nebyly upraveny toothem, přímé přenosy tooprotected infrastruktury koncových bodů nebo odesílání a přijímání nevhodný přenosy všesměrového vysílání.


### <a name="additional-rules-configured-by-fabric-controller-agent-tooisolate-vm"></a>Další pravidla nakonfigurovaná tooIsolate Fabric řadiče agenta virtuálního počítače
Ve výchozím nastavení je veškerý provoz při vytvoření virtuálního počítače, a pak hello fabric řadiče agenta konfiguruje hello paketu filtrovat tooadd pravidel a výjimek tooallow oprávnění provoz zablokované.

Existují dvě kategorie pravidel, která jsou naprogramovaný tak:

-   **Počítač pravidla konfigurace nebo infrastruktury:** ve výchozím nastavení, všechna komunikace je zablokovaná. Že jsou výjimky tooallow toosend virtuálního počítače a přijímat přenosy DHCP a DNS. Virtuální počítače můžete také odeslat provoz toohello "veřejná" internet a odesílání přenosů tooother virtuálních počítačů v rámci hello stejnou virtuální síť Azure a hello server aktivace operačního systému. virtuální počítače Hello seznam povolených odchozí cíle nezahrnuje Azure směrovač podsítě, správu Azure a další vlastnosti společnosti Microsoft.

-   **Role konfigurační soubor:** definuje hello příchozí seznamy řízení přístupu (ACL) na základě hello klienta služby modelu.

### <a name="vlan-isolation"></a>Izolace sítě VLAN
Existují tři sítí VLAN v každém clusteru:

![Izolace sítě VLAN](./media/azure-isolation/azure-isolation-fig8.jpg)


-   Hello hlavní VLAN – propojení uzlů nedůvěryhodné zákazníka

-   obsahuje důvěryhodné FCs a podporuje systémy zprostředkovatele Hello FC VLAN –

-   Hello zařízení sítě VLAN – obsahuje důvěryhodné sítě a jiných zařízeních infrastruktury

Komunikace je povolen ze sítě VLAN FC toohello hello hlavní sítě VLAN, ale nelze inicializovat z hello hlavní VLAN toohello FC sítě VLAN. Komunikace je také blokovat hello hlavní VLAN toohello zařízení sítě VLAN. To zaručuje, že i v případě, že dojde k narušení uzlu se systémem kód zákazníka, nelze útokům uzly na hello FC nebo zařízení sítě VLAN.

## <a name="storage-isolation"></a>Izolace úložiště
### <a name="logical-isolation-between-compute-and-storage"></a>Logická izolace mezi výpočetního prostředí a úložiště
Microsoft Azure jako součást základní návrh odděluje výpočtů na základě virtuálního počítače z úložiště. Toto oddělení umožňuje výpočtu a tooscale úložiště nezávisle, což jednodušší tooprovide víceklientský a izolace.

Proto tomu úložiště Azure běží na samostatné hardware s žádné tooAzure připojení k síti mohou výpočetní pouze s výjimkou logicky. [To](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) znamená, že když je vytvořen virtuální disk, místo na disku není přidělená pro své celý kapacity. Místo toho je vytvořen tabulky, který mapuje adresy na virtuální disk tooareas hello na fyzickém disku hello a tato tabulka je původně prázdné. **Hello poprvé, co zákazník zapisuje data na virtuálním disku hello, je přiděleno místo na fyzickém disku hello a tooit ukazatel je umístěn v tabulce hello.**
### <a name="isolation-using-storage-access-control"></a>Řízení přístupu úložiště pomocí izolace
**Řízení přístupu v Azure Storage** má model řízení jednoduché přístupu. Každé předplatné Azure můžete vytvořit jeden nebo více účtů úložiště. Každý účet úložiště má jeden tajný klíč, který je použité toocontrol přístup tooall data v daném účtu úložiště.

![Řízení přístupu úložiště pomocí izolace](./media/azure-isolation/azure-isolation-fig9.png)

**Přístup k datům tooAzure úložiště (včetně tabulek)** se dá řídit přes [SAS (sdíleného přístupového podpisu)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) token, který uděluje obor přístupu. Hello SAS je vytvořen pomocí šablony dotazu (URL) podepsané hello [SAK (klíč účtu úložiště)](https://msdn.microsoft.com/library/azure/ee460785.aspx). Který [podepsané URL](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) možné přidělit tooanother procesu (který se delegovala), který lze potom vyplňte podrobnosti hello hello dotazu a podání žádosti o hello hello úložiště služby. SAS umožňuje toogrant přístupu na základě času tooclients aniž by odhalil tajný klíč účtu úložiště hello.

Hello SAS znamená, že jsme můžete udělit, že klient omezenými oprávněními, tooobjects v našem účet úložiště v zadaném časovém intervalu a se zadanou sadou oprávnění. Tato omezená oprávnění jsme můžete udělit bez nutnosti tooshare klíče pro přístup k účtu.

### <a name="ip-level-storage-isolation"></a>Izolace úrovně úložiště IP
Můžete určit brány firewall a definovat rozsah IP adres pro klienty důvěryhodné. S rozsah IP adres pouze klienti, kteří mají IP adresu v rozsahu hello definované připojit příliš[Azure Storage](https://docs.microsoft.com/azure/storage/storage-security-guide).

Úložiště dat IP se dají chránit před neoprávněnými uživateli prostřednictvím sítě mechanismus, který je použité tooallocate tunelové vyhrazené nebo vyhrazené provoz tooIP úložiště.

### <a name="encryption"></a>Šifrování
Azure nabízí následující typy dat tooprotect šifrování:
-   Šifrování během přenosu

-   Šifrování v klidovém stavu

#### <a name="encryption-in-transit"></a>Šifrování během přenosu
Šifrování během přenosu je mechanismus ochrany dat při přenosu v sítích. S Azure Storage můžete zabezpečit pomocí dat:

-   [Šifrování transportní vrstvy](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), jako je například HTTPS při přenosu dat do nebo z Azure Storage.

-   [Propojit šifrování](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), jako je například šifrování protokolu SMB 3.0 pro sdílené složky Azure File.

-   [Šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), tooencrypt hello data předtím, než bude převedena do úložiště a toodecrypt hello dat po se přenáší z úložiště.

#### <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Pro mnoho společností [šifrování dat v klidovém stavu](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) je povinný krok k suverenity data o ochraně osobních údajů a dodržování předpisů a data. Existují tři Azure funkcí, které poskytují šifrování dat, která je "v klidovém stavu":

-   [Šifrování služby úložiště](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) vám umožní toorequest, že služba úložiště hello automaticky šifrování dat při zápisu ho tooAzure úložiště.

-   [Šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) taky nabízí funkci hello šifrování v klidovém stavu.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) vám umožní disky tooencrypt hello operačního systému a datové disky používané virtuálním počítačem IaaS.

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) pro virtuální počítače (VM) vám pomůže adresu zabezpečení organizace a požadavky na dodržování předpisů šifrováním vaše disky virtuálního počítače (včetně spouštěcí a datovými disky) s klíči a zásad řízení v [klíče služby Azure Trezor](https://azure.microsoft.com/services/key-vault/).

Šifrování disku řešení pro systém Windows Hello je založena na [Microsoft BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx), a hello Linux řešení je založena na [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

řešení Hello podporuje následující scénáře pro virtuální počítače IaaS, pokud jsou povolené v Microsoft Azure hello:
-   Integrace s Azure Key Vault

-   Úroveň Standard virtuálních počítačů: A, D, DS, G, GS a tak dále, virtuální počítače IaaS řady

-   Povolení šifrování v systému Windows a virtuálních počítačů IaaS Linux

-   Zakázáním šifrování na operačního systému a datové disky pro virtuální počítače IaaS Windows

-   Zakázáním šifrování na datových jednotkách pro virtuální počítače IaaS Linux

-   Povolení šifrování na virtuální počítače IaaS se systémem Windows klientského operačního systému

-   Povolení šifrování u svazků s připojení cesty

-   Povolení šifrování u virtuálních počítačů Linux, které jsou nakonfigurované s diskem prokládání (RAID) pomocí [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   Povolení šifrování na virtuální počítače s Linuxem pomocí [LVM (Správce logických svazku)](https://msdn.microsoft.com/library/windows/desktop/bb540532) pro datové disky

-   Povolení šifrování na virtuálních počítačích Windows, které jsou nakonfigurované pomocí prostorů úložiště

-   Jsou podporovány všechny veřejné oblasti Azure

řešení Hello nepodporuje následující scénáře, funkce a technologie v hello verzi hello:

-   Úroveň Basic virtuálních počítačů IaaS

-   Zakázáním šifrování na jednotce operačního systému pro virtuální počítače IaaS Linux

-   Virtuální počítače IaaS, vytvořené pomocí metody vytvoření virtuálního počítače classic hello

-   Integrace s vaší místní služby správy klíčů

-   Soubory Azure (systém souborů sdíleného), Network File System (NFS), dynamické svazky a virtuální počítače Windows, které jsou nakonfigurované s systémy na bázi softwaru diskového pole RAID

## <a name="sql-azure-database-isolation"></a>Izolace databáze Azure SQL
Databáze SQL je služba relační databáze v cloudu Microsoft hello podle hello předním databázovém serveru Microsoft SQL Server modul a umožňuje zpracovávat kritické úlohy. SQL Database nabízí předvídatelný data izolaci na úrovni účtu, geography / oblast na základě a na základě v síti – všechny s téměř nulové správy.

### <a name="sql-azure-application-model"></a>Model aplikace Azure SQL

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) databáze je služba relační databáze založené na cloudu založený na technologie SQL Server. Poskytuje vysoce dostupná, škálovatelná, víceklientské databáze služba hostovaná společností Microsoft v cloudu.

Z hlediska aplikací SQL Azure poskytuje hello následující hierarchie: Každá úroveň má jeden mnoho omezení úrovně níže.

![Model aplikace Azure SQL](./media/azure-isolation/azure-isolation-fig10.png)

Hello účet a předplatné jsou fakturace tooassociate koncepty platformy Microsoft Azure a správu.

Logické servery a databáze jsou specifické pro službu SQL Azure koncepty a jsou spravované pomocí služby SQL Azure, zadaný OData a TSQL rozhraní nebo přes portál SQL Azure, která je integrována do portálu Azure.

Servery SQL Azure nejsou fyzický nebo virtuální počítač instancí, místo toho jsou kolekce databází, sdílení zásad správy a zabezpečení, které jsou uloženy v proto názvem "logické hlavní" databáze.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Logické hlavní databáze patří:

-   Přihlášeních SQL použít tooconnect toohello serveru

-   Pravidla brány firewall

Fakturace a využití související informace pro databáze SQL Azure z hello stejného logického serveru nejsou zaručit toobe na hello stejné fyzické instanci v clusteru SQL Azure, místo toho aplikace musíte zadat název databáze cílové hello při připojování.

Z hlediska zákazníka je vytvořen logický server v grafické geografické oblasti během hello skutečné vytváření hello serveru se stane v jednom hello clusterů v oblasti hello.

### <a name="isolation-through-network-topology"></a>Izolace prostřednictvím topologie sítě

Při vytvoření logického serveru a jeho název DNS je zaregistrován, název DNS hello body toohello tak názvem "brány virtuální adresa IP. adresa v hello konkrétní datové centrum, kde je umístěn hello server.

Za hello VIP (virtuální IP adresy) máme kolekci služeb bezstavové brány. Obecně platí získat brány podílejí po koordinaci potřeby mezi více zdrojů dat (hlavní databázi, databázi uživatele atd.). Služby brány implementovat hello následující:
-   **Proxy připojení TDS.** To zahrnuje vyhledání uživatele databáze v clusteru hello back-end, implementace hello přihlašovací sekvence a pak předávání hello TDS pakety toohello back-end a back.

-   **Správa databáze.** To zahrnuje implementace kolekci pracovních toodo CREATE/ALTER/DROP database operací. Hello databázových operací může vyvolat analýzy rozšíření TDS paketů nebo explicitní rozhraní API OData.

-   Operace CREATE/ALTER/DROP přihlášení nebo uživatele

-   Operace správy logický server prostřednictvím rozhraní API OData

![Izolace prostřednictvím topologie sítě](./media/azure-isolation/azure-isolation-fig12.png)

Hello vrstvy za hello brány se nazývá "back-end". Toto je ukládat všechny hello data vysokou dostupností způsobem. Každá položka dat je uvedené toobelong tooa "oddílu" nebo "převzetí služeb při selhání jednotka", každý z nich má aspoň tři repliky. Repliky jsou uloženy a replikují stroj SQL Server a spravuje převzetí služeb při selhání systému, na které se často označuje tooas "prostředky infrastruktury".

Obecně platí hello back-end systému nekomunikuje odchozí tooother systémy jako bezpečnostní opatření. Toto je vyhrazené toohello systémy ve vrstvě hello front-endu (brány). Hello brány vrstvy počítače mají omezenou oprávnění na prostor pro útoky na hello back-end počítače toominimize hello jako vhodný mechanismus obrany zabezpečení.

### <a name="isolation-by-machine-function-and-access"></a>Izolace tak, že počítač funkce a přístup
Azure SQL (se skládá z služby spuštěné na jiný počítač funkce. SQL Azure je rozdělené do cloudu databáze "back-end" a "front-end" (správu nebo brány) prostředích s hello zásadně pouze probíhající provoz do back-end a ne na. Hello front-end prostředí může komunikovat toohello mimo world jiných služeb a obecně platí, má jenom omezené oprávnění v hello back-end (položka hello dostatek toocall potřebám tooinvoke body ji).

## <a name="networking-isolation"></a>Izolace sítě
Nasazení Azure má několik vrstev izolace sítě. Hello následující diagram znázorňuje různé úrovně izolace sítě, které Azure poskytuje toocustomers. Tyto vrstvy jsou nativní v hello Azure k samotné platformě a uživatelsky definované funkce. Příchozí z hello Internet, Azure DDoS poskytuje izolaci proti rozsáhlé útoky na Azure. Další vrstva izolace Hello je zákazník definovaný veřejné IP adresy (koncových bodů), které jsou používané toodetermine které přenos dat prostřednictvím hello cloudové služby toohello virtuální sítě. Nativní Azure virtuální izolace sítě zajišťuje úplný izolaci od všech ostatních sítí, a že jenom přenosy dat prostřednictvím uživateli nakonfigurovanému cesty a metody. Tyto cesty a metody jsou další vrstva hello, kde skupiny Nsg, UDR a síťových virtuálních zařízení můžou být použité toocreate izolace hranice tooprotect hello aplikace nasazení v síti hello chráněný.

![Izolace sítě](./media/azure-isolation/azure-isolation-fig13.png)

**Izolace provozu:** A [virtuální sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) je hranice izolace provozu hello na hello platformy Azure. Virtuální počítače (VM) v jednu virtuální síť nemůže komunikovat přímo tooVMs v jinou virtuální síť, i když jsou obě virtuální sítě vytvořené pomocí hello tentýž zákazník. Izolace je kritické vlastnosti, které zajišťuje, aby virtuální počítače zákazníka a komunikace zůstane privátní virtuální sítě.

[Podsíť](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#subnets) nabízí další úroveň izolace s ve virtuální síti na základě rozsahu IP. IP adresy ve virtuální síti hello, virtuální sítě lze rozdělit do několika podsítí pro organizaci a zabezpečení. Virtuální počítače a PaaS role instance nasazené toosubnets (stejných nebo různých) v rámci virtuální sítě můžete vzájemně komunikovat bez jakékoli další konfigurace. Můžete také nakonfigurovat [skupinu zabezpečení sítě (Nsg)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#network-security-groups-nsg) tooallow nebo odepřít instance virtuálního počítače tooa síťový provoz na základě pravidel nakonfigurované v seznamu řízení přístupu (ACL) NSG. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello instance virtuálních počítačů v této podsíti.

## <a name="next-steps"></a>Další kroky

- [Možnosti izolace sítě pro počítače v systému Windows Azure virtuální sítě](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

To zahrnuje scénář pro front-end a back-end classic hello je kde počítače v konkrétní back endovou síť nebo podsíť může povolit jenom určité klienty nebo jiné počítače tooconnect tooa konkrétní koncový bod založený na o povolených IP adres.

- [Výpočetní izolace](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure poskytuje různé výpočetní služby založené na cloudu, které zahrnují široký výběr výpočetní instance a službám, které je možné škálovat nahoru a dolů automaticky toomeet hello potřebám vaší aplikace nebo enterprise.

- [Izolace úložiště](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure odděluje výpočetní zákazníka na základě virtuálního počítače z úložiště. Toto oddělení umožňuje výpočtu a tooscale úložiště nezávisle, což jednodušší tooprovide víceklientský a izolace. Proto tomu úložiště Azure běží na samostatné hardware s žádné tooAzure připojení k síti mohou výpočetní pouze s výjimkou logicky. Všechny žádosti o spuštění pomocí protokolu HTTP nebo HTTPS na základě volby zákazníka.

