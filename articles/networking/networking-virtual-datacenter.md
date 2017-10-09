---
title: "aaaMicrosoft datového centra Azure virtuální | Microsoft Docs"
description: "Zjistěte, jak toobuild vaše virtuální datového centra v Azure"
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jonor
ms.openlocfilehash: 84f77b16edaece202a6a94b6107f1c9585ec7f38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-virtual-data-center"></a>Microsoft Azure virtuální datového centra
**Microsoft Azure**: rychlejší, šetřit peníze, integraci místní aplikace a data

## <a name="overview"></a>Přehled
Migrace tooAzure místní aplikace, i bez žádné významné změny (přístup označuje jako "navýšení a posunutí"), poskytuje organizacím hello výhod infrastruktury zabezpečené a nákladově efektivní. Ale toomake hello nejvíce z flexibility hello je možné pomocí cloud computing, by se měl podniky vyvíjet jejich architektury tootake výhod možnosti Azure. Microsoft Azure poskytuje flexibilně škálovatelné služby a infrastruktury, možnosti podnikové úrovni a spolehlivost a mnoho možností pro hybridní připojení. Zákazníci zvolit hello tooaccess těchto cloudových služeb, buď přes Internet nebo s Azure ExpressRoute, které poskytuje připojení k privátní síti. Hello platformy Microsoft Azure umožňuje zákazníkům tooseamlessly rozšířit svoji infrastrukturu do cloudu hello a vytvářet vícevrstvé architektury. Kromě toho partnery společnosti Microsoft nabízí rozšířené možnosti prostřednictvím nabídky služeb zabezpečení a virtuální zařízení, které jsou optimalizované toorun v Azure.

Tento článek obsahuje přehled vzory a návrhy, které se dají použít toosolve hello architektury škálování, výkonu a zabezpečení otázky, které mnoho zákazníků čelí u o přesunutí en masse toohello cloudu. Přehled jak toofit různé organizace IT role do správy hello a zásad správného řízení systému hello také popsané, s důrazem toosecurity požadavky a optimalizaci nákladů.

## <a name="what-is-a-virtual-data-center"></a>Co je virtuální datové centrum?
V hello počátků, cloudové řešení měla navrženou toohost jeden, relativně izolované aplikace ve veřejných spektrum hello. Tento přístup fungovala správně pro několik let. Však jako hello výhody technologie cloud se objevily řešení a více rozsáhlé úlohy byly hostované na cloudové hello adresování zabezpečení, spolehlivost, výkon a náklady riziko z hlediska nasazení v jedné nebo více oblastí se aktivovala důležitých v rámci hello životní cyklus hello cloudové služby.

Hello následující cloudové nasazení diagram znázorňuje některé příklady zabezpečení mezer (červeným rámečkem) a místo pro optimalizace sítě virtuálního zařízení napříč úlohy (žlutý pole).

[![0]][0]

z této nezbytné pro škálování toosupport podnikové úlohy se narodil Hello virtuální datového centra (vDC) a hello nemusí toodeal s problémy hello zavedená při podpoře aplikace ve velkém měřítku ve veřejném cloudu hello.

VDC není právě hello aplikace úlohy v cloudu hello, ale také hello sítě, zabezpečení, správy a infrastruktury (třeba DNS a Directory Services). Obvykle poskytuje také back tooan privátní připojení místní sítě nebo datového centra. Více úloh přesunout tooAzure, je důležité toothink o hello podporuje infrastrukturu a objekty, které tyto úlohy jsou umístěny v. Přemýšlení pečlivě o tom, jak jsou strukturovaná prostředky se můžete vyhnout hello, jak narůstá počet stovky "zatížení ostrovy", které se musí spravovat samostatně nezávislé datový tok, modely zabezpečení a problémy dodržování předpisů.

Virtuální datového centra je v podstatě kolekcí entity, ale běžné podpůrné funkce, funkce a infrastruktury. Zobrazením vašich úloh jako integrované vDC můžou realizovat snížené náklady kvůli tooeconomies měřítka, optimalizované zabezpečení prostřednictvím součásti a data toku centralizace, společně s jednodušší operace, správu a audity dodržování předpisů.

> [!NOTE]
> Je důležité toounderstand, který hello vDC **není** diskrétní Azure produktu, ale hello kombinaci různých funkcí a možností příliš splnění požadavků na přesný. vDC představuje způsob přemýšlíte o úlohy a toomaximize Azure využití prostředků a dalo v cloudu hello. Hello virtuálního řadiče domény je proto modulární přístup jak toobuild IT služeb uvedených v hello Azure, bere ohledy organizační role a zodpovědnosti.

Hello vDC může pomoci podnikům získat úlohy a aplikace do Azure pro hello následující scénáře:

-   Hostování více souvisejících úloh
-   Úlohy migrace z tooAzure místní prostředí
-   Implementaci sdílených nebo centralizované zabezpečení a požadavky na přístup v rámci úlohy
-   Kombinování DevOps a centralizované IT správně pro velký podnik

Dobrý den klíče toounlock hello výhody virtuálních řadičů domény, je centralizované topologii (rozbočovače a koncových) se smíšenými funkce Azure: [virtuální síť Azure][VNet], [skupiny Nsg] [ NSG], [VNet Peering][VNetPeering], [trasy definované uživatelem (UDR)][UDR]a identit Azure s [ Řízení přístupu na základě role (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Kdo potřebuje virtuální datové centrum?
Vyžadující toomove víc než několik úloh do Azure můžete využívat přemýšlení o pomocí společných prostředků Azure zákazníků. V závislosti na velikosti hello se i jedné aplikace, můžete využít pomocí hello vzory a součásti použít toobuild virtuálních řadičů domény.

Pokud má vaše organizace centralizované IT, sítě, zabezpečení, nebo tým nebo oddělení pro dodržování předpisů, virtuálních řadičů domény může pomoct vynutit zásady body, oddělení cla a zajistit jednotnost hello základní společné součásti současně vám dá týmy aplikací jako mnohem volnost a řízení, jako je vhodné pro vaše požadavky.

Organizace, které hledáte tooDevOps můžete využít hello vDC koncepty tooprovide autorizovaný kapsami prostředků Azure a zajistit, aby měly celkový ovládací prvek v rámci dané skupiny (předplatné nebo prostředek skupiny v běžné předplatné), ale hello sítě a hranice zabezpečení zůstat kompatibilní podle definice centralizovaných zásad v rozbočovači virtuální sítě a skupině prostředků.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Požadavky na implementaci virtuální datového centra
Při navrhování vDC, existuje několik tooconsider hrají problémy:

-   Identitu a adresářové služby
-   Infrastruktura zabezpečení
-   Připojení toohello cloudu
-   Připojení v rámci cloudu hello

##### <a name="identity-and-directory-service"></a>*Identitu a adresářové služby*
Identitu a adresářové služby jsou klíčovým prvkem všechna data centra, jak místně a v cloudu hello. Identita je související tooall aspektů tooservices přístup a autorizaci v rámci hello vDC. toohelp zajistěte, aby jenom oprávnění uživatelé a procesy získat přístup k účtu Azure a prostředky, Azure používá několik typů přihlašovacích údajů pro ověřování. Mezi ně patří hesla (tooaccess hello účet Azure), kryptografické klíče, digitální podpisy a certifikáty. [*Azure Multi-Factor Authentication* (MFA)] [ MFA] představuje další vrstvu zabezpečení pro přístup ke službám Azure. Azure MFA poskytuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo oznámení mobilní aplikace – a poskytují zákazníkům metoda hello toochoose dávají přednost.

Všechny velký podnik potřebuje toodefine proces správy identit, které popisuje správu hello jednotlivých identit, jejich ověřování, autorizace, rolí a oprávnění v rámci nebo hello vDC. Hello cílů tohoto procesu musí být tooincrease zabezpečení a produktivitu při snížení nákladů, výpadku a opakující se ruční úlohy.

Enterprise nebo organizace můžou vyžadovat náročné směs služby pro různé řádku z – firmy pole (LOB) a zaměstnanci často mají různé role při zapojená do různých projektů. Virtuálních řadičů domény vyžaduje funkční spolupráce mezi různými týmy, každý s definicemi konkrétní roli, tooget systémy s operačním systémem s funkčním zásad správného řízení. matice Hello odpovědnosti, přístupu a oprávnění může být velmi složité. Správa identit v vDC se implementuje pomocí [ *Azure Active Directory* (AAD)] [ AAD] a řízení přístupu na základě Role (RBAC).

Adresářová služba je infrastruktura sdílené informace pro hledání, správa, správě a uspořádání položek každý den a síťovým prostředkům. Tyto prostředky mohou zahrnovat svazky, složky, soubory, tiskárny, uživatelé, skupiny, zařízení a jiné objekty. Všechny prostředky v síti hello považuje objekt hello adresářový server. Informace o zdroji, jsou uloženy jako kolekce atributů přiřazených k této prostředků nebo objekt.

Všechny služby Microsoft online obchodní spoléhají na Azure Active Directory (AAD) pro přihlášení a je třeba další identity. Azure Active Directory je komplexní, vysoce dostupné cloudové řešení pro správu identit a přístupu, které představuje kombinaci základních adresářových služeb, rozšířené správy identit a správy přístupu k aplikacím. AAD lze integrovat s místní služby Active Directory tooenable jednoho přihlášení pro všechny cloudové a místní hostované aplikace (místní). atributy uživatele Hello místní služby Active Directory může být automaticky synchronizované tooAAD.

Jeden globální správce není požadováno tooassign všechna oprávnění v virtuálních řadičů domény. Místo toho každý určitého oddělení (nebo skupiny uživatelů nebo služeb v hello adresářové služby) může mít hello oprávnění požadované toomanage vlastní prostředky v rámci virtuální kopie disků. Strukturování oprávnění vyžaduje vyrovnávání. Příliš mnoho oprávnění může mít dopad na výkon efektivitu a příliš málo nebo přijít oprávnění může zvýšit rizika zabezpečení. Azure na základě rolí řízení přístupu (RBAC) pomáhá tooaddress tento problém tím, že nabízí vyladění správy přístupu pro prostředky virtuálních řadičů domény.

##### <a name="security-infrastructure"></a>*Infrastruktura zabezpečení*
Infrastruktura zabezpečení v kontextu hello vDC, je především související toohello oddělení provozu v hello vDC konkrétnímu virtuálnímu síťovému segmentu, a toocontrol příchozí a odchozí tok v rámci hello vDC. Azure je založen na víceklientské architektuře, která brání neoprávněným i neúmyslně se překrývající provoz mezi nasazení, pomocí izolace virtuální síť (VNet), seznamy řízení přístupu (ACL), zatížení nástroje pro vyrovnávání a filtry IP, společně s zásady tok provozu. Překlad síťových adres (NAT) odděluje interní síťové přenosy od externích přenosů.

Hello prostředků infrastruktury Azure přiděluje prostředky infrastruktury tootenant úlohy a spravuje tooand komunikaci z virtuálních počítačů (VM). Hello Azure hypervisoru vynucuje paměti a proces oddělení mezi virtuální počítače a bezpečně trasy síťový provoz tooguest operačního systému klientů.

##### <a name="connectivity-toohello-cloud"></a>*Připojení toohello cloudu*
Hello vDC potřebuje připojení se externí sítě toooffer služby toocustomers, partnery nebo interní uživatele. To obvykle znamená připojení pouze toohello Internet, ale také tooon místní sítí a datacentry.

Zákazníci můžete sestavit jejich toocontrol zásady zabezpečení, co a jak konkrétní vDC hostované služby jsou přístupné do a z Internetu pomocí virtuálních síťových zařízení (s filtrování a provoz kontroly) a vlastní zásady směrování a sítě filtrování (hello Směrování definované uživatelem a skupiny zabezpečení sítě).

Podniky často potřebují tooconnect virtuálních tooon místních datových center nebo jiným prostředkům. Hello připojení mezi Azure a místními sítěmi je proto velmi důležitý aspekt při navrhování efektivní architektura. Podniků má dva různé způsoby toocreate propojení mezi vDC a místně v Azure: přenosu přes hello Internet nebo privátní přímé připojení.

[ **Azure VPN Site-to-Site** ] [ VPN] služby propojení prostřednictvím Internetu hello mezi místními sítěmi a šifrované hello vDC, navázat prostřednictvím zabezpečeného připojení (tunelových propojení protokolu IPsec/IKE). Azure připojení Site-to-Site je toocreate flexibilní, rychlá a nevyžaduje žádné další nákup, jak připojit všechna připojení přes internet hello.

[**ExpressRoute** ] [ ExR] je služba Azure připojení, která umožňuje vytvářet privátní připojení mezi vDC a hello místní sítě. Připojení ExpressRoute se nepřenášejí přes hello veřejného Internetu a nabízí vyšší zabezpečení, spolehlivost a vyšší rychlosti (až too10 GB/s) společně s konzistentní latence. ExpressRoute je velmi užitečná pro virtuálních, jako zákazníci můžete získat výhody hello pravidla dodržování předpisů, které jsou přidružené k privátní připojení ExpressRoute.

Nasazení připojení ExpressRoute zahrnuje zapojení u poskytovatele služeb ExpressRoute. Pro zákazníky, kteří potřebují toostart rychle je běžné použití tooinitially tooestablish připojení Site-to-Site VPN mezi hello virtuálních řadičů domény a místní prostředky a potom migrovat tooExpressRoute připojení.

##### <a name="connectivity-within-hello-cloud"></a>*Připojení v rámci cloudu hello*
[Virtuální sítě] [ VNet] a [VNet Peering] [ VNetPeering] jsou hello základní síťové služby připojení uvnitř virtuální kopie disků. Virtuální síť zaručuje přirozené hranice izolace pro prostředky virtuálních řadičů domény a vztahy virtuální sítě umožňuje spojovacího mezi různých virtuálních sítí v rámci hello stejné oblasti Azure. Řízení přenosů dat uvnitř virtuální sítě a mezi virtuálními sítěmi potřebovat toomatch sadu zabezpečení pravidla zadaná pomocí seznamů řízení přístupu ([skupinu zabezpečení sítě][NSG]), [síťových virtuálních zařízení ] [ NVA]a vlastní směrovacích tabulek ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Přehled virtuální datového centra

### <a name="topology"></a>topologie
Rozbočovače a koncových modelu rozšířené hello virtuální datového centra v rámci jedné oblasti Azure

[![1]][1]

Centrum Hello je hello centrální zóny, která řídí a kontroluje příchozí a odchozí provoz mezi různým zónám: Internet, na místních počítačích a hello koncových. hvězdicová topologie Hello poskytuje hello oddělení IT efektivní způsob tooenforce zabezpečení zásady v centrálním umístění, navíc snižuje potenciální hello chybné konfigurace a ohrožení.

Hello hub obsahuje společné součásti služby hello spotřebovávají koncových hello. Tady je několik příkladů typické běžné centrální služby:

-   infrastrukturu služby Active Directory Windows Hello (s hello týkající se služby AD FS) požadované pro ověřování uživatelů třetích stran, přístup k z nedůvěryhodné sítě před získávání přístupu toohello úlohy v hello ramenem
-   Služba DNS tooresolve pojmenování pro zatížení hello v hello koncových, tooaccess prostředkům na pracovišti a na Internetu hello
-   Infrastrukturu veřejných KLÍČŮ, tooimplement jednotného přihlašování na úlohy
-   Řízení toku (TCP/UDP) mezi hello koncových a Internet
-   Řízení toku mezi hello ramenem a místně
-   V případě potřeby toku řízení mezi jeden ramenem a další

Hello vDC snižuje celkové náklady na pomocí infrastruktury sdílené rozbočovače hello mezi více servery.

Hello role každý paprsek může být toohost různé typy úloh. Hello koncových můžete zadat taky modulární přístup pro opakovatelná nasazení (například vývoje a testování, testování přijetí uživateli předprodukční režim a produkci) z hello stejné úlohy. Hello koncových můžete také být použité toosegregate a povolit různé skupiny v rámci vaší organizace (například skupiny DevOps). Uvnitř ramenem je možné toodeploy základní zatížení nebo složité úlohy několika vrstvami s přenosy, řízení mezi vrstvami hello.

##### <a name="subscription-limits-and-multiple-hubs"></a>Limity předplatného a více rozbočovače
V Azure všechny komponenty, ať typ hello nasadí předplatné Azure. izolace Hello součástí Azure v různých předplatných Azure můžete vyhovět požadavkům hello různé objekty LOBs, jako je například nastavení různých úrovní přístupu a autorizaci.

Jeden vDC můžete škálovat toolarge počet koncových, i když stejně jako u každé IT systému existují omezení platformy. Hello rozbočovače nasazení je vázané tooa konkrétní předplatné Azure, který má omezení a limity (například maximální počet partnerských vztahů VNet - najdete v části [předplatného Azure a omezení služby, kvóty a omezení] [ Limits] podrobnosti). V případech, kdy omezení může být problém, můžete škálovat hello architektura až další rozšířením hello modelu z clusteru tooa jednoho rozbočovače koncových rozbočovače a koncových. Více centra v jedné nebo více oblastech Azure může být připojen pomocí Express Route nebo sítě site-to-site VPN.

[![2]][2]

Hello zavedení více centra zvyšuje náklady a správa úsilí hello hello systému a by pouze oprávněné podle škálovatelnost (příklady: omezení systému nebo redundance) a místní replikace (příklady: výkonu nebo havárii obnovení koncového uživatele). V scénářům, které vyžadují více centra musí všechny rozbočovače hello zajistit toooffer hello stejné nastavení služby pro provozní snadné.

##### <a name="interconnection-between-spokes"></a>Propojení mezi servery
V jednom ramenem je možné tooimplement komplexní úlohy s více vrstvami. Vícevrstvé konfigurace může být implementovaná pomocí podsítě (jeden pro každou vrstvu) v hello stejnou virtuální síť a filtrování hello toků pomocí skupin Nsg.

Na dobrý den druhé straně, na architekt může být vhodné toodeploy vícevrstvé zatížení mezi více virtuálních sítí. Pomocí virtuální sítě partnerského vztahu, můžete koncových připojit tooother koncových v hello rozbočovače stejné nebo jiné rozbočovače. Typickým příkladem tohoto scénáře je případ hello, kde aplikační servery zpracování nejsou v jeden ramenem (VNet), pokud databáze hello je nasazena v různých ramenem (VNet). V takovém případě je snadno toointerconnect hello koncových s partnerský vztah virtuální sítě a zabránit procházejících hello rozbočovače. Pečlivě zkontrolujte architektuře a zabezpečení by měl být provádět tooensure, který obcházení hello centra není důležité zabezpečení nebo auditování body, které může existovat pouze v centru hello vynechat.

[![3]][3]

Koncových může být také vzájemně propojena tooa ramenem, který funguje jako centrum. Tento postup vytvoří dvě úrovně hierarchie: hello ramenem hello vyšší úrovni (úroveň 0), stane hello centra z hierarchie hello nižší koncových (úroveň 1). Hello koncových z vDC potřebovat tooforward hello provoz toohello centrální rozbočovač tooreach out toohello do místní sítě nebo Internetu. Architekturu s dvě úrovně rozbočovače představuje komplexní směrování, která odebere hello výhod vztahu jednoduché hvězdicovou rozbočovače.

I když Azure umožňuje komplexních topologiích, základní principů hello konceptu vDC hello je opakovatelnost a jednoduchost. úsilí vynaložené na správu toominimize, Návrh jednoduché rozbočovače ramenem hello je hello doporučená vDC referenční architektura.

### <a name="components"></a>Komponenty
Virtuální datového centra se skládá ze čtyř typů základní součásti: **infrastruktury**, **hraniční sítě**, **úlohy**, a **monitorování**.

Každý typ součásti se skládá z různých funkce Azure a prostředky. Vaše vDC se skládá z instance více typů součásti a více variace hello stejný typ součásti. Například můžete mít velký počet instancí jiné, logicky oddělených zatížení, které představují různé aplikace. Můžete používat tyto typy různé součásti a instance tooultimately sestavení hello vDC.

[![4]][4]

Hello předchozí Architektura vysoké úrovně vDC ukazuje různé součásti typy používané v jiných zónách hello rozbočovače koncových topologie. Hello diagram znázorňuje součásti infrastruktury v různých částí architektury hello.

Pro přístup k osvědčených postupů (pro místní řadič domény nebo vDC) práva a oprávnění musí být na základě skupiny. Plánování práce se skupinami, místo jednotlivým uživatelům pomáhá zachování zásady přístupu konzistentně napříč týmy a pomůckách v minimalizovat chyby konfigurace. Přiřazení a odebrání tooand uživatelé z příslušných skupin pomáhá udržovat aktuální oprávnění hello konkrétního uživatele.

Každou skupinu role by měl mít jedinečnou předponu na jejich názvy, takže je easy tooidentify, které skupiny zatížením, které souvisí. Například zatížení hostování ověřovací služba možná skupiny s názvem *AuthServiceNetOps, AuthServiceSecOps, AuthServiceDevOps a AuthServiceInfraOps.* Stejně tak pro centralizované role nebo rolí není související tooa konkrétní službu, může být uvedena "Corp" *CorpNetOps* třeba.

Mnoho organizací používá varianta hello následující skupiny tooprovide hlavní rozdělení rolí:

-   Hello *centrální skupinu IT (Corp)* má součásti hello vlastnictví práva toocontrol infrastruktury (například sítě a zabezpečení) a proto potřebuje toohave hello role Přispěvatel na předplatné hello (a mít kontrolu nad Rozbočovač Hello) a sítě oprávněním přispěvatele v koncových hello. Velké organizace často rozdělit tyto odpovědnosti správy mezi několik týmů. například; Skupina síťových operací (CorpNetOps) (s výhradní aktivní sítě) a skupinu zabezpečení operací (CorpSecOps) (zodpovědná za zásady brány firewall a zabezpečení). V tomto případě konkrétní potřebovat dvě různé skupiny toobe vytvořené pro přiřazení vlastních rolí.
-   Hello *dev & testovací (AppDevOps) skupina* má hello odpovědnost toodeploy úlohy (aplikace nebo služby). Tato skupina má hello role Přispěvatel virtuálních počítačů pro nasazení IaaS a/nebo jednu nebo více PaaS Přispěvatel rolí (najdete v části [předdefinované role pro řízení přístupu][Roles]). Volitelně hello dev & testovacího týmu může být nutné toohave viditelnost na zásady zabezpečení (Nsg) a zásady směrování (UDR) uvnitř hello rozbočovače nebo konkrétní ramenem. V přidání toohello role Přispěvatel pro úlohy, proto tato skupina by také potřebovat hello role čtečky sítě.
-   Hello *operace a údržba skupiny (CorpInfraOps nebo AppInfraOps)* mají odpovědnost hello správy úloh v produkčním prostředí. Tato skupina musí toobe Přispěvatel předplatné na úlohy v žádné produkční předplatné. Některé organizace mohou také měli zvážit, pokud potřebují skupinu team další eskalaci podpory s hello role Přispěvatel předplatného v produkčním prostředí a v rámci předplatného hello centrální rozbočovač, v pořadí toofix možné konfigurace problémy v produkčním prostředí hello prostředí.

VDC strukturovaná tak, aby skupiny vytvořené pro hello Centrální správa hello rozbočovače skupiny IT odpovídající skupiny na úrovni pracovního vytížení hello. Kromě toho toomanaging prostředky jenom hello centrální IT skupinám rozbočovačů by mít toocontrol externí přístup a nejvyšší úrovně oprávnění na základě předplatného hello. Skupiny úloh však bude možné toocontrol prostředků a oprávnění své virtuální síti nezávisle na centrálního oddělení IT.

Hello vDC potřebám toobe rozděleného toosecurely hostitele více projektů mezi různé řádku z – firmy (pole LOB). Všechny projekty vyžadují různé izolované prostředí (vývojářů, UAT, produkční). Samostatné předplatná Azure pro každou z těchto prostředích poskytují přirozené izolaci.

[![5]][5]

Hello předchozí diagram znázorňuje hello vztah mezi projekty v organizaci, uživatelé, skupiny a hello prostředí, kde hello Azure součásti používají.

Obvykle v oddělení IT prostředí (nebo vrstvě) je systém, ve kterém je více aplikací, nasadit a spustit. Velké podniky použít vývojové prostředí (kde změny původně vytvářeny a testovány) a provozním prostředí (co koncoví uživatelé použít). Tato prostředí jsou často oddělené s několika přípravná prostředí mezi je tooallow dvoufázového nasazení (zavedení), testování a vrácení zpět v případě problémů. Nasazení architektury výrazně liší, ale obvykle je stále následovaný hello základní proces začínající na vývoj (vývoj) a končí na produkční (PRODUKČNÍMU).

Běžné architektura pro tyto typy vícevrstvé prostředí se skládá z DevOps (vývoj a testování), UAT (pracovní) a provozní prostředí. Organizace mohou využívat jeden nebo více Azure AD klienty toodefine přístup a práva toothese prostředí. Hello předchozí diagram ukazuje případu tam, kde dva různé klienty Azure AD se používají: jeden pro DevOps a UAT a hello jiných výhradně pro produkční.

Hello přítomnosti různých Azure AD klienty vynucuje hello oddělení mezi prostředími. Hello stejnou skupinu uživatelů (jako třeba centrálního oddělení IT) musí tooauthenticate pomocí jiný identifikátor URI tooaccess jiný klient AD upravit hello role nebo oprávnění buď hello DevOps nebo produkční prostředí projektu. Hello přítomnost jiný uživatel ověřování tooaccess různých prostředích snižuje možné výpadky a jiné potíže způsobené lidské chyby.

#### <a name="component-type-infrastructure"></a>Typ součásti: infrastruktury
Tento typ součásti je, kde se nachází většina hello podpůrná infrastruktura. Je také kde vaše centralizované IT, zabezpečení, nebo dodržování předpisů týmy tráví většinu své doby.

[![6]][6]

Součásti infrastruktury zajišťují propojení mezi různými součástmi hello služby vDC a jsou k dispozici v hello rozbočovače a koncových hello. Hello zodpovědnost za správu a údržbu součásti infrastruktury hello je obvykle přiřazen toohello střed IT nebo tým pro zabezpečení.

Jedním z hello primární úlohy hello adaptérů infrastruktury IT napříč hello enterprise je tooguarantee hello konzistence schémat IP adresu. Hello privátní IP adresy přiřazené místo toohello vDC musí toobe konzistentní a není překrývající se s privátní IP adresy přiřazené na vaše místní sítě.

Zatímco NAT na hello místní hraniční směrovače nebo v Azure prostředí se můžete vyhnout konfliktům IP adres, přidá součásti infrastruktury tooyour komplikace. Zjednodušení správy je jedním z cílů hello klíče virtuálních řadičů domény, takže pomocí NAT toohandle IP obavy není doporučené řešení.

Součásti infrastruktury obsahovat hello následující funkce:

-   [**Identitu a adresářové služby**][AAD]. Typ prostředku přístup tooevery v Azure je řízena identity uložené v adresářové službě. úložiště služby directory Hello pouze hello seznam uživatelů, ale také hello tooresources práva přístup v rámci konkrétní předplatného Azure. Tyto služby může existovat jenom pro cloud nebo mohou být synchronizovány s identitou místně uložené ve službě Active Directory.
-   [**Virtuální síť**][VPN]. Virtuální sítě jsou jedním z hlavních komponent vDC a povolte toocreate hranici izolace přenosů na hello platformy Azure. Virtuální síť se skládá z jedné nebo více segmentech virtuální sítě, každý s konkrétní IP předpony sítě (podsítě). Hello virtuální sítě definuje oblast interní hraniční kde virtuální počítače IaaS a PaaS služby můžete vytvořit privátní komunikaci. Virtuální počítače (a služby PaaS) v jedné virtuální sítě nemůže komunikovat přímo tooVMs (a PaaS services) v jinou virtuální síť, i v případě, že jsou obě virtuální sítě vytvořené pomocí hello tentýž zákazník, v části hello stejného předplatného. Izolace je kritické vlastnosti, které zajišťuje, aby virtuální počítače zákazníka a komunikace zůstane privátní virtuální sítě.
-   [**UDR**][UDR]. Ve výchozím nastavení podle hello systémovou tabulku směrování se směruje provoz ve virtuální síti. Trasy se definují uživatele je vlastní směrovací tabulky, správci sítě můžete přidružit tooone nebo toooverwrite podsítě další hello chování hello systémovou tabulku směrování a zadejte cestu k komunikace v rámci virtuální sítě. Hello přítomnost udr zaručuje, aby odchozí provoz z hello ramenem přenosu přes konkrétní vlastní virtuální počítače nebo virtuální zařízení sítě a nástroje pro vyrovnávání zatížení nachází v centru hello a koncových hello.
-   [**SKUPINA NSG**][NSG]. Skupina zabezpečení sítě je seznam pravidel zabezpečení, které fungují jako provoz filtrování zdrojů IP, cílové IP, protokoly, porty zdrojové IP a cílové IP porty. Hello NSG můžou být použité tooa podsítě, karty virtuální síťovou kartu spojené s virtuální počítač Azure, nebo obojí. skupiny Nsg Hello jsou nezbytné tooimplement správné toku řízení v hello rozbočovače a koncových hello. Hello úroveň zabezpečení poskytované hello NSG je funkce, které porty, otevřete a pro jaké účely. Zákazníci by měl použít filtry další jednotlivé virtuální počítače s založené na hostiteli brány firewall například IPtables nebo hello brány Windows Firewall.
-   **DNS**. Hello překlad prostředků do hello virtuální sítě o virtuální kopie disků je zajišťováno prostřednictvím DNS. rozsah Hello překlad názvu výchozí hello DNS je omezená toohello virtuální sítě. Obvykle vlastní služba DNS musí toobe nasazení v centru hello jako součást společných služeb, ale hello hlavní spotřebitelé služeb DNS jsou umístěny v ramenem hello. V případě potřeby zákazníci vytvářet hierarchická struktura DNS s delegováním koncových toohello zón DNS.
-   [** Předplatné] [ SubMgmt] a [správu skupiny prostředků][RGMgmt]**. Předplatné definuje přirozené hranice toocreate více skupin prostředků v Azure. Prostředky v předplatném se sestaví společně v logické kontejnery s názvem skupiny prostředků. Hello skupiny prostředků představuje prostředky pro tooorganize hello logické skupiny virtuálních řadičů domény.
-   [**RBAC**][RBAC]. Prostřednictvím RBAC je možné toomap organizační roli společně s práva tooaccess konkrétní prostředky Azure, umožní vám toorestrict uživatelé tooonly určité podmnožiny akce. S RBAC můžete udělit přístup přiřazením toousers hello příslušné role, skupiny a aplikace v rámci oboru příslušné hello. předplatné Azure, skupinu prostředků nebo jediný zdroj, může být Hello rozsah přiřazení role. RBAC umožňuje dědičnosti oprávnění. Role přiřazené v nadřazeném oboru také uděluje přístup toohello podřízené objekty jsou v něm obsažena. RBAC můžete oddělit povinností a udělit pouze hello množství toousers přístup, že potřebují tooperform svou práci. Například použijte RBAC toolet jednoho zaměstnance spravovat virtuální počítače v předplatném, zatímco jiné můžete spravovat databáze SQL v rámci hello stejného předplatného.
-   [**VNet Peering**][VNetPeering]. Hello základní funkce používá toocreate hello infrastruktura vDC je VNet partnerský vztah, mechanismus, který se připojuje dvě virtuální sítě (virtuální sítě) v hello stejné oblasti přes síť hello hello Azure datového centra.

#### <a name="component-type-perimeter-networks"></a>Typ součásti: Hraniční sítě
[Hraniční síť] [ DMZ] součásti (také označované jako zóna DMZ síť) umožňují tooprovide připojení k místní nebo sítě fyzické datového centra, společně s tooand žádné připojení z Internetu hello. Je také kde vaší sítě a zabezpečení pravděpodobně týmy tráví většinu jejich doby.

Příchozí pakety musí procházet skrz hello zabezpečovací zařízení v centru hello, jako jsou brány firewall hello, ID a IP adresy, dříve, než dorazila hello back-end serverů v koncových hello. Internetový pakety z úlohy hello by také procházet skrz hello zabezpečovací zařízení v hraniční síti hello pro vynucení zásad, kontrolu a auditování účely, před opuštěním hello sítě.

Komponenty hraniční sítě poskytují hello následující funkce:

-   [Virtuální sítě][VNet], [UDR][UDR], [NSG][NSG]
-   [Virtuální síťové zařízení][NVA]
-   [Nástroj pro vyrovnávání zatížení][ALB]
-   [Aplikační brána][AppGW] / [firewall webových aplikací][WAF]
-   [Veřejné IP adresy][PIP]

Obvykle hello centrální IT a zabezpečení týmy mají odpovědnost za definice požadavek a operací hello hraniční sítě.

[![7]][7]

Hello předchozí diagram znázorňuje hello vynucení ze dvou okruhu s toohello přístup k Internetu a místní sítě, i v centru hello trvalé. V jednom rozbočovači hello hraniční sítě toointernet můžete škálovat toosupport velkého počtu objekty LOBs, pomocí více farmy brány Web Application firewall (WAFs) nebo brány firewall.

[**Virtuální sítě** ] [ VNet] rozbočovače hello je obvykle založený na virtuální síť s více podsítěmi toohost hello jiný typ služby filtrování a zkontrolujete tooor přenosy z Internetu prostřednictvím NVAs, WAFs, hello a Azure Application Gateway.

[**UDR** ] [ UDR] pomocí UDR zákazníci můžou nasazovat brány firewall, ID nebo IP adresy a jiné virtuální zařízení a směrovat síťový provoz prostřednictvím těchto zabezpečovací zařízení pro vynucení hranic zásad zabezpečení, auditování a kontroly. Udr lze vytvořit v obou hello rozbočovače a hello koncových tooguarantee, tranzitů provoz přes hello konkrétní vlastní virtuální počítače, virtuální zařízení sítě a používané hello vDC nástroje pro vyrovnávání zatížení. tooguarantee, který přenosy generované z virtuálních počítačů, které jsou uložené ve hello ramenem přenosu toohello v správný virtuální zařízení, musí UDR toobe nastavená v hello podsítě hello ramenem nastavením hello front-end IP adresu služby Vyrovnávání zatížení interní hello jako další směrování hello. Vyrovnávání zatížení interní Hello distribuuje hello interní provoz toohello virtuální zařízení (fond back-end pro vyrovnávání zatížení).

[![8]][8]

[**Síťových virtuálních zařízení** ] [ NVA] v centru hello hello hraniční síti s toohello přístup k Internetu je obvykle spravovat prostřednictvím farmu brány firewall nebo brány firewall systému webové aplikace (WAFs).

Různé objekty LOBs běžně používají mnoho webových aplikací a tyto aplikace mívají toosuffer z různých ohrožení zabezpečení a potenciální zneužití. Brány firewall webových aplikací je zvláštní druh produkt používat toodetect útoky na webové aplikace (HTTP či HTTPS) do větší hloubky než obecné brány firewall. Porovnání s tradiční Typografie technologii brány firewall, WAFs mají sadu konkrétní funkce tooprotect interní webové servery před hrozbami.

Brány firewall farmy je skupina brány firewall práce v kombinaci pod hello stejné společné správy sadu zabezpečení pravidla tooprotect hello zatížení hostovaná v koncových hello a řízení přístupu tooon místní sítě. Brány firewall farmy má menší specializuje softwaru ve srovnání s firewall webových aplikací, ale má široký aplikace obor toofilter a zkontrolovat libovolného typu provoz ve výstupní a vstupní. Brány firewall farmy jsou obvykle implementované v Azure pomocí síťových virtuálních zařízení (NVAs), které jsou k dispozici v hello Azure marketplace.

Je doporučeno toouse jednu sadu NVAs pro přenosy na hello Internetu, a druhý pro provoz pocházející místně. Použití jen jednu sadu NVAs pro obě je bezpečnostní riziko, protože poskytuje žádné zabezpečení hraniční mezi dvěma sadami hello síťových přenosů. Pomocí samostatných NVAs snižuje složitost hello zabezpečení pravidla pro kontrolu a udělá z něj vymazat, která pravidla bude odpovídat toowhich příchozí žádosti o síti.

Většina velké podniky spravovat víc domén. Azure DNS se dá použít toohost hello záznamy DNS pro konkrétní domény. Například lze registrovat hello virtuální IP adresa (VIP) Vyrovnávání zatížení Azure externí hello (nebo hello WAFs) v záznamu A hello záznamu Azure DNS.

[**Azure Vyrovnávání zatížení** ] [ ALB] pro vyrovnávání zatížení Azure nabízí vysokou dostupnost služby vrstvy 4 (TCP, UDP), která můžete distribuovat příchozí komunikaci mezi instance služby, které jsou definované v sadě s vyrovnáváním zatížení. Data odeslaná, že nástroj pro vyrovnávání zatížení toohello z front-endu koncových bodů (veřejné koncové body IP nebo privátní IP koncových bodů) mohou být znovu distribuovány s nebo bez sada tooa překlad adres fondu IP adres back-end (příklady probíhá; Virtuální síťová zařízení nebo virtuálních počítačů).

Azure Vyrovnávání zatížení můžete probe hello stav hello také různé instance serveru, a když dojde k selhání sondu toorespond hello zatížení vyrovnávání zastaví odesílání přenosů toohello není v pořádku instance. V vDC máme hello přítomnost externím vyrovnáváním zatížení v centru hello (například vyrovnávat hello provoz tooNVAs) a v koncových hello (tooperform úkoly, jako je vyrovnávání přenosů mezi různé virtuální počítače vícevrstvé aplikace).

[**Aplikační brána** ] [ AppGW] Microsoft Azure Application Gateway je vyhrazené virtuální zařízení poskytuje aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaši aplikaci. Umožní vám toooptimize webové farmy produktivitu přesměrováním zátěže procesoru náročné SSL ukončení toohello aplikační brány. Poskytuje také další možnosti směrování vrstvy 7 včetně kruhové dotazování distribuce příchozí provoz, spřažení na základě souboru cookie relace, na základě cestu směrování adres URL a hello možnost toohost více webů za jeden Application Gateway. Brány firewall webových aplikací (firewall webových aplikací) je k dispozici jako součást hello aplikační brány firewall webových aplikací SKU. Tato SKU poskytuje ochranu tooweb aplikací z běžných chyb zabezpečení webové a zneužití. Application Gateway je možné nakonfigurovat jako internetovou bránu nebo jen jako interní bránu, případně jako kombinaci obojího. 

[**Veřejné IP adresy** ] [ PIP] hello povolení funkce některá Azure můžete tooassociate služby koncové body tooa veřejnou IP adresu umožňující tooyour prostředků toobe přístupné z Internetu. Tento koncový bod používá překládání adres (NAT) tooroute provoz toohello interní adresa a port na hello virtuální síť Azure. Tato cesta je hello primární způsob toopass externích přenosů do virtuální sítě hello. Hello veřejné IP adresy může být nakonfigurované toodetermine, jaký provoz, je předaná a jak a kde je přeložená na toohello virtuální sítě.

#### <a name="component-type-monitoring"></a>Typ součásti: monitorování
Monitorování komponenty poskytují viditelnost a výstrah ze všech ostatních typů součásti hello. Všechny týmy měli mít přístup toomonitoring pro součásti hello a služby, kterým mají přístup. Pokud máte centralizovanou pomoc HelpDesk nebo operations týmy, potřebují by toohave integrované přístup toohello data poskytnutá tyto součásti.

Azure nabízí různé typy protokolování a monitorování služeb tootrack hello chování Azure hostovaným prostředkům. Zásady správného řízení a kontrolu nad úlohy v Azure je na základě jenom na shromažďování dat protokolu, ale také hello možnost tootrigger akce na základě konkrétní hlášené událostí.

Existují dva hlavní typy protokolů v Azure:

-   [**Protokoly aktivity** ] [ ActLog] (označované také jako "Operační protokol") umožní získat přehled o hello operace, které byly provedeny na prostředky v hello předplatného Azure. Tyto protokoly sestavy hello události rovině řízení pro vaše předplatné. Každý prostředek Azure vytvoří protokoly auditu.

-   [**Azure diagnostické protokoly** ] [ DiagLog] jsou protokolů generovaných prostředek, které poskytují bohatou a často data o operaci hello tohoto prostředku. obsah Hello tyto protokoly se liší podle typu prostředku.

[![9]][9]

V vDC je velmi důležité tootrack hello skupiny Nsg protokoly, zejména tyto informace:

-   [**Protokoly událostí**][NSGLog]: poskytuje informace o jaké pravidla NSG jsou použité tooVMs a instance rolí na základě adresy MAC.
-   [**Čítač protokoly**][NSGLog]: sleduje více než jednou. Každá skupina NSG pravidlo byl proveden toodeny nebo povolení provozu.

Všechny protokoly mohou být uloženy v účtech úložiště Azure pro audit, statické analýzy nebo pro účely zálohování. Když hello protokoly jsou uložené v účtu úložiště Azure, zákazníci mohou používat různé typy z rozhraní tooretrieve připravená data, analyzovat a vizualizovat tento stav hello tooreport dat a stavu prostředků cloudu.

Velké podniky by již jste získali standardní rozhraní pro monitorování místních systémů a můžete rozšířit, protokoly toointegrate framework generované Cloudová nasazení. Organizace, které chcete tookeep všechny hello protokolování v cloudu hello [Microsoft Operations Management Suite (OMS)] [ OMS] je je služba skvělou volbou. Vzhledem k tomu, že je OMS implementována jako cloudová služba, je možné ji zprovoznit velmi rychle a s minimální investicí do služeb infrastruktury. OMS můžete také integrovat s součástí produktu System Center, jako je například tooextend System Center Operations Manager stávající investice správy do cloudu hello.

Analýzy protokolů OMS je součástí hello OMS framework toohelp shromáždit, porovnejte, vyhledávání a postupovat podle data protokolu a výkonu generovaných operačních systémů, aplikací, cloudové infrastruktury komponenty. Nabízí zákazníkům v reálném čase Statistika provozu pomocí integrovaného hledání a vlastní řídicí panely tooanalyze všechny záznamy hello v všechny úlohy v virtuálních řadičů domény.

#### <a name="component-type-workloads"></a>Typ součásti: úlohy
Zatížení součásti jsou, kde jsou umístěné vaše vlastní aplikace a služby. Je také kde vaše aplikace vývojové týmy tráví většinu jejich doby.

Možnosti Hello zatížení jsou skutečně nekonečná. Následující Hello je uvedeno několik možných zatížení typů hello:

**Interní obchodní aplikace**

Obchodní aplikace jsou probíhající operace kritické toohello počítače aplikace podniku. OBCHODNÍCH aplikací mít některé společné vlastnosti:

-   **Interaktivní**. OBCHODNÍ aplikace, jsou interaktivní svou povahou: zadávání dat, a se vrátí výsledek nebo sestavy.
-   **Řízených daty**. Aplikace LOB jsou data s častým toohello databáze nebo jiného úložiště náročné.
-   **Integrované**. OBCHODNÍ aplikace nabídka integrace s jinými systémy v rámci nebo mimo organizaci hello.

**Určeno webů (Internet nebo interní přístupem) pro odběratele** většinu aplikací, které pracují se hello Internetu jsou webové servery. Azure nabízí možnost toorun hello webového serveru na virtuální počítač IaaS nebo ze [Azure Web Apps] [ WebApps] lokality (PaaS). Azure Web Apps podporují integraci s virtuální sítě, který umožní hello nasazení hello webové aplikace v hello ramenem systému vDC. Při použití hello integrace virtuální sítě nepotřebujete tooexpose koncový bod sítě Internet pro vaše aplikace ale můžete použít hello prostředky privátního Internetu jiných směrovatelné adresu z vaší privátní virtuální sítě místo toho.

**Big Data nebo Analytics** okamžiku potřeby tooscale tooa velmi velký objem, nemusí správně škálování databáze. Hadoop technologie nabízí systému toorun distribuované dotazy paralelně na velký počet uzlů. Zákazníci mají hello možnost toorun data zatížení ve virtuálních počítačů IaaS nebo PaaS ([HDInsight][HDI]). HDInsight podporuje nasazování do virtuální sítě na základě polohy, může být nasazený tooa clusteru ramenem hello vDC.

**Události a zasílání zpráv**
[Azure Event Hubs] [ EventHubs] je služba přijímání velkého rozsahu telemetrii, která shromažďuje, transformuje a ukládá miliony událostí. Jako distribuované streamování platformu nabízí nízkou latencí a konfigurovat doba uchovávání, povolení tooingest masivní objemy telemetrická data do Azure a čtení dat z více aplikací. Službě Event Hubs může podporovat jeden datový proud v reálném čase i na základě batch kanály.

Vysoce spolehlivé cloudové služby mezi aplikací a služeb pro zasílání zpráv se dají implementovat pomocí [Azure Service Bus] [ ServiceBus] , nabízí asynchronní zprostředkované zasílání zpráv mezi klientem a serverem, spolu s strukturovaná možnosti zasílání zpráv a publikování a přihlášení k odběru objektů first in first out (FIFO).

[![10]][10]

### <a name="multiple-vdc"></a>Více vDC
Pokud má tento článek zaměřuje na jedné vDC, popisující hello základní komponenty a architektura, která přispívat odolné vDC tooa. Funkce Azure, jako je například Azure zatížení vyrovnávání NVAs, skupiny dostupnosti sady škálování, společně s jiným mechanismem přispívat tooa systém, který umožní toobuild plnou SLA úrovně do služeb produkční.

Ale jeden vDC je hostovaná v jedné oblasti a je snadno napadnutelný toomajor výpadku, které by mohly ovlivnit danou celou oblast. Zákazníkům, kteří mají tooachieve vysoké SLA potřebujete tooprotect hello služby prostřednictvím nasazení hello stejné projektu ve virtuálních dva (nebo více), umístěny v různých oblastech.

V přidání tooSLA obavy existuje několik běžné scénáře, kde nasazení více virtuálních dává smysl:

-   Místní nebo globální přítomnosti
-   Zotavení po havárii
-   Mechanismus toodivert provoz mezi řadiči domény

#### <a name="regionalglobal-presence"></a>Místní nebo globální přítomnosti
Datových center Azure jsou k dispozici v mnoha oblastech po celém světě. Když vyberete několik datových center Azure, mají zákazníci dva faktory související s tooconsider: zeměpisné vzdálenosti a latenci. Zákazníci potřebovat tooevaluate hello zeměpisné vzdálenost mezi hello virtuálních a hello vzdálenost mezi hello vDC a hello koncoví uživatelé toooffer hello nejlepších výsledků.

Hello oblast Azure, kde jsou hostované virtuálních také potřebovat tooconform zákonným požadavkům vymezenému žádné právní jurisdikce, pod kterou vaše organizace provozuje.

#### <a name="disaster-recovery"></a>Zotavení po havárii
Hello implementace plánu zotavení po havárii je důrazně související toohello typu úlohy obavy a stav úlohy hello možnost toosynchronize hello mezi různé virtuálních. V ideálním případě má většina zákazníků toosynchronize dat aplikací mezi systémem ve dvou různých virtuálních tooimplement rychlé převzetí služeb při selhání mechanismus nasazení. Většina aplikací jsou toolatency citlivé a které můžou způsobit potenciální časový limit a prodlevu synchronizace dat.

Synchronizace nebo monitorování prezenčního signálu v různých virtuálních aplikací vyžaduje komunikaci mezi nimi. Dva uloženými v různých oblastech mohou připojené prostřednictvím:

-   Privátní partnerský vztah ExpressRoute při hello vDC rozbočovače jsou připojené toohello stejnému okruhu ExpressRoute
-   více okruhů ExpressRoute připojení přes páteřní vaší podnikové síti a vaše vDC OK připojené toohello okruhy ExpressRoute
-   Připojení Site-to-Site VPN mezi vaší vDC centra v každé oblasti Azure

Hello připojení ExpressRoute je obvykle hello preferované mechanismu kvůli větší šířku pásma a konzistentní latence při procházejících hello páteřní společnosti Microsoft.

Neexistuje žádné magic recepturách toovalidate aplikace distribuovány mezi dva (nebo více) různých virtuálních umístěné v různých oblastech. Zákazníci by měla spustit sítě kvalifikace testy tooverify hello latencí a šířkou pásma hello připojení a cílová, zda je příslušná data synchronní nebo asynchronní replikaci a jaké hello plánovanou dobu optimální obnovení (RTO) může být pro vaše úlohy.

#### <a name="mechanism-toodivert-traffic-between-dc"></a>Mechanismus toodivert provoz mezi řadiči domény
Jeden efektivní technika toodivert hello provoz příchozí v tooanother jeden řadič domény je založený na DNS. [Azure Traffic Manager] [ TM] používá hello systému DNS (Domain Name) mechanismus toodirect hello koncového uživatele provoz toohello nejvhodnější veřejný koncový bod v konkrétní virtuálních řadičů domény. Prostřednictvím sondy Traffic Manager pravidelně kontroluje stav služby hello veřejné koncové body v různých virtuálních a v případě selhání těchto koncových bodů, směruje automaticky toohello sekundární virtuálních řadičů domény.

Traffic Manager funguje na veřejné koncové body Azure a lze jej použít, například toocontrol nebo přesměrovat provoz tooAzure virtuální počítače a webové aplikace v hello vhodné virtuálních řadičů domény. Správce provozu odolný proti i v vzhled hello neúspěšného celou oblast Azure a můžou řídit hello distribuce provozu generovaného uživateli pro koncové body služby v různých virtuálních na základě několika kritérií (například selhání služby v konkrétní vDC, nebo výběrem Hello vDC s hello nejnižší latenci sítě pro hello klienta).

### <a name="conclusion"></a>Závěr
Hello virtuální datového centra je při migraci center toodata přístup do cloudu hello, který používá kombinaci funkce a možnosti toocreate škálovatelná architektura v Azure, který maximalizuje využití prostředků cloudu, snížení nákladů a zjednodušení systému zásady správného řízení. Koncept vDC Hello je založena na topologii koncových rozbočovače, poskytuje společné sdílených služeb v centru hello a povolení konkrétní aplikace nebo zatížení v koncových hello. V DC odpovídá hello struktura rolí společnosti, kde různá oddělení (centrálního oddělení IT, DevOps, operace a údržba) pracují společně se na konkrétní seznam rolí a úloh. VDC splňuje požadavky na migraci "Navýšení a Shift" hello, ale také poskytuje celou řadu výhod toonative Cloudová nasazení.

## <a name="references"></a>Odkazy
Hello následující funkce byly popsané v tomto dokumentu. Klikněte na tlačítko Další toolearn odkazy hello.

| | | |
|-|-|-|
|Funkce sítě|Vyrovnávání zatížení|Připojení|
|[Virtuální sítě Azure][VNet]</br>[Skupiny zabezpečení sítě][NSG]</br>[Protokolů NSG][NSGLog]</br>[Směrování definované uživatelem][UDR]</br>[Virtuální síťová zařízení][NVA]</br>[Veřejné IP adresy][PIP]|[Pro vyrovnávání zatížení Azure (L3)][ALB]</br>[Aplikační brána (L7)][AppGW]</br>[Brány Firewall webových aplikací][WAF]</br>[Azure Traffic Manager][TM] |[Partnerský vztah virtuální sítě][VNetPeering]</br>[Virtuální privátní síť][VPN]</br>[ExpressRoute][ExR]
|Identita</br>|Monitorování</br>|Osvědčené postupy</br>|
|[Azure Active Directory][AAD]</br>[Vícefaktorové ověřování][MFA]</br>[Ovládací prvky přístupu na základě role][RBAC]</br>[Výchozí role AAD][Roles] |[Protokoly aktivity][ActLog]</br>[Diagnostické protokoly][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br> |[Hraniční sítě osvědčené postupy][DMZ]</br>[Správa předplatného][SubMgmt]</br>[Správa skupin prostředků][RGMgmt]</br>[Limity předplatného Azure][Limits] |
|Jinými službami Azure|
|[Webové aplikace Azure][WebApps]</br>[HDInsights (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Další kroky
 - Prozkoumejte [VNet Peering][VNetPeering], hello podpůrný technologie pro vDC rozbočovače a příčkou návrhy
 - Implementace [AAD] [ AAD] tooget začít s [RBAC] [ RBAC] zkoumání
 - Vyvinout model správy předplatného a prostředků a RBAC modelu toomeet hello struktura, požadavky a zásady vaší organizace. plánování je nejdůležitější aktivity Hello. Podobně jako praktické naplánujte pro uspořádání, při slučování, nové řádky produktu atd.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "Příklady překrývají součásti" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "Podrobný ukázkový hvězdicové vDC"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "Cluster rozbočovače a koncových"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "Ramenem ramenem"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "Úrovně Blokový diagram serveru hello vDC"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "Uživatelé, skupiny, odběry a projektů"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "Diagram základní infrastruktury"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "Diagram základní infrastruktury"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "Partnerský vztah virtuální sítě a hraniční sítě"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "Vysokoúrovňový diagram pro monitorování"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "Vysokoúrovňový diagram pro pracovní vytížení"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg 
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[WebApps]: https://docs.microsoft.com/azure/app-service-web/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
