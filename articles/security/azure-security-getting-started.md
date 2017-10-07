---
title: "aaaGetting začít s zabezpečení Microsoft Azure | Microsoft Docs"
description: "Tento článek obsahuje přehled o funkcích zabezpečení Microsoft Azure a obecné požadavky pro organizace, které provádíte migraci jejich poskytovatele tooa cloudové prostředky."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 8d8a0088-c85a-48e7-bd04-2bc7b78b0691
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: c61dd99ffd0143bc7b165367dadadc977ffcf47b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-microsoft-azure-security"></a>Začínáme se zabezpečením Microsoft Azure
Při vytvoření nebo migraci poskytovatele cloudové tooa IT prostředky, se spoléháte na dané organizace dalo tooprotect vaší aplikace a data se službami hello a ovládací prvky hello poskytují zabezpečení hello toomanage vaše cloudové prostředky.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům podle jejich potřeb zabezpečení. Kromě toho Azure poskytuje širokou škálu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vašich nasazení.

V tomto přehledovém článku o zabezpečení Azure se budeme zabývat následujícími tématy:

* Služby a funkce, které můžete použít toohelp Azure zabezpečení služeb a dat v rámci Azure.
* Jak Microsoft zabezpečuje hello infrastrukturu Azure toohelp Chraňte vaše data a aplikace.

## <a name="identity-and-access-management"></a>Správa identit a přístupu
Řízení přístupu tooIT infrastruktury, datům a aplikacím je velmi důležité. Microsoft Azure poskytuje tyto možnosti službami například Azure Active Directory (Azure AD), Azure Storage a podpora pro mnoho standardů a rozhraní API.

[Azure AD](../active-directory/active-directory-whatis.md) je úložiště identit a modul, který poskytuje ověřování, autorizace a řízení přístupu pro organizace, uživatelé, skupiny a objekty. Azure AD také nabízí vývojářům účinný způsob správy identit toointegrate ve svých aplikacích. Standardní protokoly, jako [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx), a [OpenID Connect](http://openid.net/connect/) Zkontrolujte možné přihlášení na platformách, například .NET, Java, Node.js a PHP.

Hello založené na REST rozhraní Graph API umožňuje vývojářům tooread zápisu toohello adresáře a z libovolné platformě. Prostřednictvím podpory pro [OAuth 2.0](http://oauth.net/2/), vývojáři mohou vytvářet mobilních a webových aplikací, které se integrují s společnosti Microsoft a třetích stran webovým rozhraním API a vytvářet své vlastní zabezpečenému webovému rozhraní API. Open Source klientské knihovny jsou dostupné pro .NET, Windows Store, iOS a Android. Další knihovny se vyvíjejí.

### <a name="how-azure-enables-identity-and-access-management"></a>Umožnění správy identit a přístupu v Azure
Azure AD lze použít jako samostatný cloudový adresář pro vaši organizaci nebo jako integrované řešení se stávající místní službou Active Directory. Některé funkce integrace zahrnují synchronizaci adresářů a jednotné přihlašování (SSO). Tyto rozšířit rozsah hello vaší existující místních identit do cloudu hello a zlepšit hello správu a činnost koncového uživatele.

K dalším možnostem správy identit a přístupu patří následující:

* Azure AD umožňuje [jednotného přihlašování k](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) tooSaaS aplikace, bez ohledu na to, kde jsou hostované. Některé aplikace jsou federované pomocí Azure AD, jiné používají jednotné přihlašování pomocí hesla. Federované aplikace můžou také podporovat zřizování uživatelů a ukládání hesel do trezoru.
* Přístup k toodata v [Azure Storage](https://azure.microsoft.com/services/storage/) je řízena pomocí ověřování. Každý účet úložiště má primární klíč ([klíč účtu úložiště](https://msdn.microsoft.com/library/azure/ee460785.aspx), nebo SAK) a sekundární tajný klíč (hello sdílený přístupový podpis nebo SAS).
* Azure AD poskytuje Identity jako služby pomocí federování pomocí [Active Directory Federation Services](../active-directory/fundamentals-identity.md), synchronizace a replikace s místních adresářů.
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) je hello služba služby Multi-Factor authentication, která vyžaduje tooverify uživatelé přihlášení pomocí mobilní aplikace, telefonního hovoru nebo textové zprávy. Použít s Azure AD toohelp zabezpečené místní prostředky pomocí ověřování Azure Multi-Factor Authentication server hello a také s vlastní aplikace a adresáře pomocí hello SDK.
* [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) umožňuje připojit virtuální počítače Azure tooa doméně bez nasazení řadiče domény. Můžete přihlásit toothese virtuální počítače s svoje podnikové přihlašovací údaje služby Active Directory a spravovat virtuální počítače připojené k doméně pomocí zásad skupiny směrné plány zabezpečení tooenforce na všechny vaše virtuální počítače Azure.
* [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) poskytuje službu vysoce dostupný globální identity správy určených aplikací, která je škálovatelná toohundreds milionů identit. Dá se integrovat do mobilních i webových platforem. Vaši uživatelé mohou zaregistrovat v tooall aplikace prostřednictvím přizpůsobitelné pomocí svých účtů na sociálních nebo vytvořením nové přihlašovací údaje.

## <a name="data-access-control-and-encryption"></a>Řízení přístupu k datům a šifrování
Microsoft využívá Principy hello oddělení povinností a [nejnižší oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) v průběhu operace v Azure. Přístup k toodata pracovníky podpory Azure vyžaduje výslovná oprávnění a uděleno na základě "v běhu", která je zaznamenána a auditovat, pak zruší se po dokončení hello zapojení.

Azure také poskytuje několik možností pro ochranu dat při přenosu i v klidu. To zahrnuje šifrování dat, soubory, aplikace, služby, komunikace a jednotky. Můžete šifrovat informace před jeho umístění v Azure a také ukládání klíčů ve vašem místních datových center.

![Microsoft Antimalware v Azure](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Technologie šifrování v Azure
Můžete shromáždit informace o prostředí předplatné tooyour přístup pro správu pomocí [generování sestav Azure AD](../active-directory/active-directory-reporting-audit-events.md). Můžete nakonfigurovat [nástroj BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx) na virtuální pevné disky obsahující citlivé údaje v Azure.

Další možnosti v Azure, která vám pomůže tookeep zabezpečení vašich dat patří:

* Vývojáři aplikace můžete vytvářet šifrování do aplikace hello nasadí v Azure pomocí hello Windows [rozhraní CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) a rozhraní .NET Framework.
* Úplné řízení hello klíče pomocí šifrování na straně klienta pro úložiště objektů Blob v Azure. Služba úložiště Hello nikdy vidí hello klíče a neschopnost dešifrování dat hello.
* [Azure Rights Management (Azure RMS)](https://technet.microsoft.com/library/jj585026.aspx) (s hello [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) poskytuje souborů a dat na úrovni šifrování a úniku dat prevence přes správu na základě zásad přístupu.
* Azure podporuje [úrovni tabulky a sloupce úroveň šifrování (TDE/vy)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) v systému SQL Server virtuální počítače a třetí strany podporuje místní servery správy klíčů v datových centrech.
* Klíče účtu úložiště, sdílené přístupové podpisy, certifikáty pro správu a jiných klíčů jsou jedinečné tooeach klientovi Azure.
* Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) hybridní storage šifruje data prostřednictvím pár 128-bit veřejného a privátního klíče před nahráním ho tooAzure úložiště.
* Azure podporuje a používá řada mechanismů šifrování, včetně SSL/TLS, protokol IPsec a AES, v závislosti na hello datové typy, kontejnery a přenosy.

## <a name="virtualization"></a>Virtualizace
Hello platformy Azure používá virtualizovaném prostředí. Uživatelské instance fungovat jako samostatné virtuální počítače, které nemají přístup tooa fyzického hostitelského serveru, a pomocí fyzických se vynucuje tato izolace [úrovně oprávnění procesoru (prstenec-0 nebo prstenec-3)](https://en.wikipedia.org/wiki/Protection_ring).

Prstenec 0 je hello nejvíce privilegovaných a 3 je alespoň hello. Hello hostovaný operační systém běží v 1 menší privilegovaných prstenec a spouští aplikace hello nejnižšími oprávněními prstenec 3. Tato virtualizace fyzické prostředky vede jasně oddělené tooa mezi hostovaný operační systém a hypervisoru, což vede k další bezpečnostní oddělení mezi dvěma hello.

Hello Azure hypervisoru chová jako micro jádra a předává všechny požadavky na hardware přístup z hosta virtuálního počítače toohello hostitele pro zpracování pomocí sdílené paměti rozhraní s názvem VMBus. To zabrání uživatelům v získání přímého přístupu pro čtení, zápisu a spouštění toohello systému a snižuje riziko hello sdílení systémové prostředky.

![Microsoft Antimalware v Azure](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Způsob implementace virtualizace v Azure
Azure používá bránu firewall hypervisoru (paket filtr), která je implementovaná v hello hypervisoru a nakonfigurované agentem kontroleru prostředků infrastruktury. Tato funkce pomáhá chránit klienty před neoprávněným přístupem. Ve výchozím nastavení, je veškerý provoz při vytvoření virtuálního počítače, a pak hello fabric řadiče agenta konfiguruje hello paketu filtru tooadd zablokované *pravidel a výjimek* tooallow oprávnění provoz.

Programují se tady dvě kategorie pravidel:

* **Počítač pravidla konfigurace nebo infrastrukturu**: ve výchozím nastavení, všechna komunikace je zablokovaná. Že jsou výjimky tooallow toosend virtuálního počítače a přijímat přenosy DHCP a DNS. Virtuální počítače také mohou odesílat provoz toohello "veřejná" internet a odesílání provoz tooother virtuálních počítačů v rámci clusteru hello a hello OS aktivace serveru. virtuální počítače Hello seznam povolených odchozí cíle nezahrnuje Azure směrovač podsítě, správu Azure back end a dalších vlastností společnosti Microsoft.
* **Role konfigurační soubor**: definuje hello příchozí seznamy řízení přístupu (ACL) na základě hello klienta služby modelu. Například pokud klient má front-end webový na portu 80 pro určité virtuální počítač, pak Azure otevře TCP port 80 tooall IP adresy Pokud konfigurujete koncový bod v hello [modelu nasazení Azure classic](../azure-resource-manager/resource-manager-deployment-model.md). Pokud virtuální počítač hello back end a pracovní role, která běží, potom otevře hello pracovní role toohello virtuální počítač v rámci hello stejné klienta.

## <a name="isolation"></a>Izolace
Požadavek na zabezpečení jinou důležitá cloudu je oddělení tooprevent neoprávněným i neúmyslně se překrývající přenosu informací mezi nasazení ve víceklientské architektuře sdílené.

Azure implementuje [sítě řízení přístupu](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) a oddělení prostřednictvím izolace sítě VLAN, seznamy ACL, zatížení nástroje pro vyrovnávání a filtry IP adres. Omezuje externích přenosů příchozí tooports a protokolů na virtuální počítače, které definujete. Azure implementuje filtrování tooprevent maskování přenos v síti a omezit komponenty platformy tootrusted příchozí a odchozí provoz. Zásady toku provozu jsou implementovány v zařízeních ochrany hranic, která ve výchozím nastavení odmítají provoz.

![Microsoft Antimalware v Azure](./media/azure-security-getting-started/sec-azgsfig3.PNG)

Překlad adres (NAT) se používá tooseparate interní síťové přenosy od externích přenosů. Vnitřní provoz nelze směrovat vně. [Virtuální IP adresy](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx), pro které je povoleno vnější směrování, se přeloží na [interní dynamické IP adresy](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx), které je možné směrovat jen v rámci Azure.

Externích přenosů tooAzure virtuálních počítačů je bránou firewall pomocí seznamů řízení přístupu na směrovačích, nástroje pro vyrovnávání zatížení a přepínače vrstvy 3. Povoleny jsou jen konkrétní známé protokoly. Seznamy ACL jsou v místě toolimit přenosy pocházející z hostované virtuální počítače sítí VLAN tooother používá pro správu. Kromě toho provoz filtrovat pomocí filtrů IP adres na hello hostitelský operační systém další omezení provozu hello na obou datové vrstvy odkaz a sítě.

### <a name="how-azure-implements-isolation"></a>Způsob implementace izolace v Azure
Hello Kontroleru prostředků infrastruktury Azure zodpovídá za přidělování prostředků infrastruktury tootenant úlohy a spravuje jednosměrný komunikaci z počítačů toovirtual hello hostitele. Hello Azure hypervisoru vynucuje paměti a proces oddělení mezi virtuálními počítači a bezpečně směruje síťový provoz tooguest operačního systému klientů. Azure také implementuje izolaci pro klienty, úložiště a virtuální sítě.

* Každý klient Azure AD je logicky izolované pomocí hranice zabezpečení.
* Účty úložiště Azure jsou jedinečné tooeach předplatného a přístupu musí být ověřeny pomocí klíč účtu úložiště.
* Virtuální sítě jsou logicky izolované pomocí kombinace jedinečný privátní IP adresy, brány firewall a seznamy ACL IP. Nástroje pro vyrovnávání zatížení směrovat provoz toohello příslušné klienty podle definice koncových bodů.

## <a name="virtual-networks-and-firewalls"></a>Virtuální sítě a brány firewall
Hello [distribuované a virtuální sítí](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) v Azure Nápověda zajistěte, aby byl privátní síťový provoz logicky izolované od provoz na jiné virtuální sítě Azure.

![Microsoft Antimalware v Azure](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Vaše předplatné může obsahovat více izolované privátní sítě (a bránu firewall, Vyrovnávání zatížení a překlad síťových adres).

Azure poskytuje tři primární úrovně oddělení sítě v každém Azure clusteru toologically segregate provoz. [Virtuální místní sítě](https://azure.microsoft.com/services/virtual-network/) (sítě VLAN) se používají tooseparate zákazníka provoz z hello zbytek hello síť Azure. Přístup toohello síť Azure z mimo hello clusteru je omezen pomocí nástroje pro vyrovnávání zatížení.

Tooand provoz sítě z virtuálních počítačů musí projít přes virtuální přepínač hello hypervisoru. komponenta filtru IP Hello v kořenovém hello OS izoluje hello kořenové virtuální počítač z hello hostované virtuální počítače a hello hostované virtuální počítače od sebe navzájem. Provede filtrování toorestrict komunikace mezi uzly a hello klienta veřejného Internetu (podle konfigurace služby hello zákazníka), je oddělení od ostatních klientů.

Filtr IP Hello pomáhá zabránit hostované virtuální počítače z:

* Generování podvodná provoz.
* Toothem příjem přenosů není provedena.
* Směrování provozu tooprotected infrastruktury koncové body.
* Odesílání nebo přijímání nevhodných vysílání provoz.

Můžete umístit virtuální počítače do [virtuálních sítí Azure](https://azure.microsoft.com/documentation/services/virtual-network/). Tyto virtuální sítě jsou podobné toohello sítě, které nakonfigurujete v místním prostředí, kde jsou obvykle přidruženy virtuální přepínač. Virtuální počítače připojené toohello, který může komunikovat stejné virtuální síti sebou bez další konfigurace. Můžete také nakonfigurovat různé podsítě v rámci virtuální sítě.

Můžete použít následující ve vaší virtuální síti Azure Virtual Network technologie toohelp zabezpečený komunikační hello:

* [**Skupin zabezpečení (Nsg) sítě**](../virtual-network/virtual-networks-nsg.md). Můžete použít NSG toocontrol provoz tooone nebo více instancí virtuálního počítače ve virtuální síti. Skupina zabezpečení sítě obsahuje pravidla pro řízení přístupu, která povolují nebo zakazují provoz na základě směru přenosu, protokolu, zdrojové adresy a portu a cílové adresy a portu.
* [**Uživatelem definované směrování**](../virtual-network/virtual-networks-udr-overview.md). Můžete ovládat hello směrování paketů prostřednictvím virtuálního zařízení tak, že vytvoříte trasy definované uživatelem, které určují hello další segment směrování paketů odesílaných tooa určité podsíti toogo tooa virtuální sítě zabezpečení zařízení.
* [**Předávání IP**](../virtual-network/virtual-networks-udr-overview.md). Zabezpečení zařízení virtuální sítě musí být schopný tooreceive příchozí provoz, který není adresovaný tooitself. tooallow přenosy dat virtuálních počítačů tooreceive řešit tooother cíle, povolíte předávání IP pro virtuální počítač hello.
* [**Vynucené tunelování**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Vynucené tunelování umožňuje přesměrování nebo "Vynutit" veškerý provoz vázaný Internet generované virtuálních počítačů v back tooyour místní umístění virtuální sítě přes tunelové propojení VPN typu site-to-site pro kontrolu a auditování
* [**Seznamy ACL koncových bodů**](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Můžete řídit, které počítače jsou povolena příchozí připojení z hello Internet tooa virtuálního počítače ve vaší virtuální síti definováním seznamy ACL koncových bodů.
* [**Řešení zabezpečení pro partnerské sítě**](https://azure.microsoft.com/marketplace/). Existuje řada partnerských řešení zabezpečení sítě, které je přístupné z hello Azure Marketplace.

### <a name="how-azure-implements-virtual-networks-and-firewalls"></a>Jak Azure implementuje virtuální sítě a brány firewall
Azure implementuje filtrováním paketů brány firewall na všechny hostitele a hostů virtuální počítače ve výchozím nastavení. Bitové kopie operačního systému Windows z hello Azure Marketplace taky mít ve výchozím nastavení povolená brána Windows Firewall. Nástroje pro vyrovnávání zatížení v hello hraniční sítě Azure veřejné řízení komunikace na základě na seznamy ACL IP, které spravují správci zákazníka.

Pokud Azure přesouvá data zákazníka v rámci běžných operací nebo při havárii, činí tak přes privátní šifrované komunikační kanály. Další možnosti zaměstnaní Azure toouse v virtuální sítě a brány firewall jsou:

* **Brány firewall hostitele nativní**: Azure Service Fabric a Azure Storage spouštět v nativním operační systém, který nemá žádné hypervisoru. Proto je brána firewall systému windows hello nakonfigurovaný s hello předchozí dvě sady pravidel. Úložiště spustí nativní toooptimize výkonu.
* **Brány firewall hostitele**: brány firewall hostitele hello je tooprotect hello hostitelský operační systém, který spouští hello hypervisoru. Hello pravidla jsou naprogramovaných tooallow jediným řadičem hello Service Fabric a přejít na určitém portu polí tootalk toohello hostitelský operační systém. Hello ostatní výjimky jsou tooallow DHCP odpovědi a odpovědi DNS. Azure pomocí konfiguračního souboru počítač, který má hello šablony pravidel brány firewall pro hello hostitelský operační systém. Hostitel Hello, sám je chráněn před útokem externí Windows communication toopermit brány firewall nakonfigurovat pouze ze známých, ověřený zdroje.
* **Brány firewall hosta**: replikuje hello pravidla v filtr paketů přepínač hello virtuální počítač ale naprogramovaných v různých softwaru (například hello brány Windows Firewall část hello hostovaný operační systém). Hello hosta virtuálního počítače brány firewall může být nakonfigurované toorestrict tooor komunikaci z hello hosta virtuálního počítače, i když je povolena komunikace hello podle konfigurací na hostiteli hello filtr IP. Například můžete toouse hello hosta virtuálního počítače brány firewall toorestrict komunikaci mezi dvěma vaší virtuální sítě, které byly nakonfigurované tooconnect tooone jiné.
* **Úložiště brány firewall (FW)**: hello brány firewall na front-endu hello úložiště filtry toobe provoz jenom na porty 80/443 a další porty potřebné nástroje. Hello brány firewall na back-end hello úložiště omezuje toocome komunikaci pouze z front-endové úložné servery.
* **Brána virtuální sítě**: hello [brány virtuální sítě Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) slouží jako brány mezi různými místy hello připojení úlohy v Azure Virtual Network tooyour místními lokalitami. Je požadováno tooconnect tooon místní servery pomocí [tunelových propojení IPsec site-to-site VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), nebo pomocí [ExpressRoute](../expressroute/expressroute-introduction.md) okruhů. U tunelového propojení protokolu IPsec/IKE VPN Gateway hello provádět ověřování metodou handshake IKE a vytvořit hello tunelových propojení IPsec S2S VPN mezi hello virtuálními sítěmi a místními servery. Brány virtuální sítě také ukončit [point-to-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

## <a name="secure-remote-access"></a>Zabezpečený vzdálený přístup
Data uložená v cloudu hello musí mít dostatečná zneužití tooprevent povolena ochrana a zachování důvěrnosti a integrity při během přenosu. To zahrnuje mechanismy řízení sítě, které jsou propojeny s identitou organizace umožňující audit a založenou na zásadách a s mechanismy správy přístupu.

Předdefinované kryptografických technologie umožňuje tooencrypt komunikaci v rámci a mezi nasazení, mezi oblastí Azure a z Azure tooon místní datacentra. Správce přístupu toovirtual počítače prostřednictvím [relací vzdálené plochy](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), [vzdáleného prostředí Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx), a hello portálu Azure jsou vždy šifrována.

toosecurely rozšířit místní cloudu toohello datacenter, Azure poskytuje i [site-to-site VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) a [point-to-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), plus vyhrazené propojení s [ExpressRoute](../expressroute/expressroute-introduction.md)(tooAzure připojení virtuální sítě prostřednictvím sítě VPN se šifrují,).

### <a name="how-azure-implements-secure-remote-access"></a>Způsob implementace zabezpečeného vzdáleného přístupu v Azure
Musí být ověřeny vždy připojení toohello portál Azure a vyžadují SSL/TLS. Můžete nakonfigurovat správu certifikátů tooenable zabezpečené správy. Zabezpečení Standardní protokoly, jako [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) a [IPsec](https://en.wikipedia.org/wiki/IPsec) jsou plně podporované.

[Azure ExpressRoute](../expressroute/expressroute-introduction.md) vám umožňuje vytvářet privátní připojení mezi datovými centry Azure a infrastrukturou, která se nachází ve vašem umístění nebo v prostředí ve společném umístění. Připojení ExpressRoute se nepřenášejí prostřednictvím hello veřejného Internetu. Nabízí další spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než Typická internetové odkazy. V některých případech přenosu dat mezi místními umístěními a Azure pomocí připojení ExpressRoute přispět také k výraznému snížení nákladů.

## <a name="logging-and-monitoring"></a>Protokolování a monitorování
Azure poskytuje ověřený protokolování zabezpečení relevantní události, které generují záznam pro audit a je inženýrství toobe odolná tootampering. To zahrnuje informace o systému, jako je například protokolů událostí zabezpečení v Azure infrastruktury virtuálních počítačů a Azure AD. Sledování událostí zabezpečení zahrnuje shromažďování událostí, jako jsou například změny v DHCP nebo DNS IP adresy serverů; tooports pokusy o přístup, protokoly nebo IP adresy, které jsou blokována návrhu; změny v nastavení zabezpečení zásad nebo brány firewall; Vytvoření účtu nebo skupiny; a neočekávané procesy nebo instalace ovladače.

![Microsoft Antimalware v Azure](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Protokoly auditu se záznamy přístupu a aktivit privilegovaných uživatelů, pokusů o oprávněný a neoprávněný přístup, výjimek systému a událostí zabezpečení informací se uchovávají po nastavenou dobu. Hello uchování protokolů je svého uvážení, protože konfigurace protokolu shromažďování a uchovávání tooyour vlastní požadavky.

### <a name="how-azure-implements-logging-and-monitoring"></a>Způsob implementace protokolování a monitorování v Azure
Azure nasadí výpočetní tooeach agentů správy agentů (MA) a monitorování zabezpečení v Azure (ASM), úložiště nebo uzel topologie fabric pod správu, zda se jedná o nativní nebo virtuální. Každý Agent správy je účet úložiště nakonfigurované tooauthenticate tooa služby team certifikátem získané z úložiště certifikátů Azure hello a dál předem nakonfigurované diagnostiky a účet úložiště toohello data událostí. Tyto agenty nejsou nasazené toocustomers virtuálních počítačů.

Správci Azure přístup k protokolům prostřednictvím webového portálu pro ověřený a řízené přístup toohello protokoly. Správce může protokoly parsovat, filtrovat, zjišťovat jejich korelaci a analyzovat je. účty úložiště tým služby Azure Hello protokoly jsou chráněné ze Správce přímý přístup toohelp zabránit před manipulací protokolu.

Microsoft shromažďuje protokoly ze síťových zařízení pomocí protokolu Syslog hello a ze serverů hostitele pomocí služby Microsoft Audit Collection Services (ACS). Tyto protokoly jsou umístěny do protokolu databáze, ze kterého jsou generovány výstrahy pro podezřelé události. Hello správce můžete přístup a tyto protokoly analyzovat.

[Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx) je funkce Azure, která vám umožní toocollect diagnostických dat z aplikace běžící v Azure. Toto je diagnostických dat pro ladění a řešení potíží s měření výkonu, sledování využití prostředků, analýza provozu, plánování kapacity a auditování. Po shromáždění diagnostických dat hello, může být přenášená tooan účtu úložiště Azure pro trvalost. Přenosy lze buď naplánovat nebo na vyžádání.

## <a name="threat-mitigation"></a>Zmírnění hrozeb
Přidání tooisolation, šifrování a filtrování aktivuje se Azure několik mechanismů ke zmírnění hrozeb a infrastrukturu tooprotect procesy a služby. Ty zahrnují interní kontrolní mechanismy a použít technologie toodetect ohrožení a oprava pokročilé například DDoS, zvýšení úrovně oprávnění a hello [OWASP horní-10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

ovládací prvky zabezpečení Hello a postupy pro řízení rizik Microsoft má v místě toosecure jeho cloudové infrastruktury snížit riziko bezpečnostních incidentů hello. V hello vyskytne událost incidentu hello týmu správy incidentu zabezpečení (SIM) v rámci týmu Microsoft Online Services zabezpečení a dodržování předpisů (OSSC) hello je připraven toorespond kdykoli.

### <a name="how-azure-implements-threat-mitigation"></a>Způsob implementace snižování rizik při ohrožení v Azure
Azure má v zmírnění hrozeb tooimplement místní kontrolních mechanismů pro zabezpečení a také toohelp zákazníkům zmírnit potenciální hrozby ve svých prostředích. Hello následující seznam shrnuje funkce zmírnění hrozeb hello které nabízí Azure:

* [Azure antimalwarových](azure-security-antimalware.md) je povoleno ve výchozím nastavení ve všech serverech infrastruktury. Volitelně ji můžete povolit v rámci virtuálních počítačů.
* Společnost Microsoft udržuje nepřetržité monitorování napříč servery, sítě a aplikace toodetect hrozby a zabránit zneužití. Automatizované výstrahy upozornění správců neobvyklé chování, povolení tootake opravné akce v interních i externích hrozeb.
* Můžete nasadit řešení třetí strany zabezpečení v rámci vašich předplatných, jako jsou brány firewall webových aplikací z [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).
* Společnosti Microsoft testování toopenetration přístupu zahrnuje "[síťových adaptérů Red](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)," což zahrnuje Odborníci v oblasti zabezpečení Microsoft napadení (bez zákazníka) za provozu produkční systémy v Azure tootest obranu proti reálného, Upřesnit , trvalé hrozby.
* Integrované nasazení systémy spravovat hello distribuce a instalace opravy zabezpečení v rámci hello platformy Azure.

## <a name="next-steps"></a>Další kroky
[Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/)

[Blog týmu zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/)

[Středisko Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Blog služby Active Directory](http://blogs.technet.com/b/ad/)
