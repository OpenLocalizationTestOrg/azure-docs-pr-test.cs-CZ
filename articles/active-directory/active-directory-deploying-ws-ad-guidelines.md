---
title: "aaaGuidelines pro nasazení systému Windows Server Active Directory ve virtuálních počítačích Azure | Microsoft Docs"
description: "Pokud víte, jak se toodeploy AD Domain Services a federační služby AD na místní, zjistěte, jak fungují na virtuálních počítačích Azure."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: 04df4c46-e6b6-4754-960a-57b823d617fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: femila
ms.openlocfilehash: 9ad5a5f138a6402cbb656d9160545846051207b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Pokyny pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure
Tento článek vysvětluje hello důležitých rozdílů mezi nasazení systému Windows Server Active Directory Domain Services (AD DS) a služby Active Directory Federation Services (AD FS) místně a jejich nasazení na virtuálních počítačích Microsoft Azure.

## <a name="scope-and-audience"></a>Obor a cílová skupina
Hello článek je určený pro ty již došlo k nasazení služby Active Directory v místě. Pokrývá hello rozdíly mezi na Microsoft Azure virtuální počítače nebo Azure virtual Network a nasazeních služby Active Directory tradičních místních nasazení služby Active Directory. Virtuální počítače Azure a virtuálních sítí Azure jsou součástí služby infrastruktury jako služba (IaaS) nabídky pro organizace tooleverage výpočetních prostředků v cloudu hello.

Pro ty, které se s nasazení služby AD, najdete v části hello [Průvodce nasazením služby AD DS](https://technet.microsoft.com/library/cc753963) nebo [plánování nasazení služby AD FS](https://technet.microsoft.com/library/dn151324.aspx) podle potřeby.

Tento článek předpokládá, že se seznámit s následující koncepty hello čtečky hello:

* Nasazení systému Windows Server AD DS a Správa
* Nasazení a konfigurace infrastruktury služby DNS toosupport systému Windows Server AD DS
* Nasazení systému Windows Server AD FS a Správa
* Nasazení, konfiguraci a správu předávající strany aplikací (weby a webové služby), které můžou využívat tokeny služby Windows Server AD FS
* Koncepty obecné virtuálního počítače, například jak tooconfigure a virtuální počítač, virtuální disky a virtuální sítě

V tomto článku klade důraz hello požadavky pro hybridním scénáři nasazení, ve které systému Windows Server AD DS nebo AD FS se částečně nasadit místně a částečně nasazují na virtuálních počítačích Azure. dokument Hello nejprve popisuje hello kritické rozdíly mezi systémem Windows Server AD DS a AD FS na virtuálních počítačích Azure a místními a důležité rozhodovací body, které ovlivňují návrhu a nasazení. Zbývající Hello hello dokumentu vysvětluje pokyny pro každou hello rozhodovací body v podrobněji a jak tooapply hello scénáře nasazení toovarious pokyny.

Tento článek se nezabývá hello konfiguraci [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), což je služba založené na REST, která poskytuje funkce řízení správy a přístupu identit pro cloudové aplikace. Azure Active Directory (Azure AD) a Windows Server služby AD DS se však navrženou toowork společně tooprovide řešení správy přístupu a identit pro hybridní dnešních IT prostředí a moderní aplikace. toohelp pochopit rozdíly hello a vztahy mezi službami Windows Server AD DS a Azure AD, zvažte následující hello:

1. Můžete spustit služby systému Windows Server AD DS v hello cloudu na virtuálních počítačích Azure při použití Azure tooextend překážek místní datacentra do cloudu hello.
2. Může použít Azure AD toogive aplikace uživatelům jednoho přihlášení tooSoftware jako služba (SaaS). Microsoft Office 365 používá tuto technologii, například a aplikace běžící v Azure nebo jiných platforem cloudu jej také použít.
3. Můžete použít Azure AD (jeho služby Řízení přístupu) toolet uživatelé přihlašování pomocí identit ze sítě Facebook, Google, Microsoft a jiných tooapplications zprostředkovatelé identity, hostovaných v cloudu hello nebo místně.

Další informace o těchto rozdílech najdete v tématu [Azure Identity](fundamentals-identity.md).

## <a name="related-resources"></a>Související prostředky
Můžete stáhnout a spustit hello [posouzení připravenosti na virtuální počítač Azure](https://www.microsoft.com/download/details.aspx?id=40898). Hello assessment bude automaticky kontrolovat v místním prostředí a vytvářet vlastní sestavy založené na hello pokyny nalezen v toto téma toohelp migrujete tooAzure prostředí hello.

Doporučujeme také nejdřív zkontrolovali hello kurzy, příručky a videa, které se týkají hello následující témata:

* [Konfigurace virtuální sítě Cloud-Only v hello portálu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
* [Konfigurace VPN typu Site-to-Site v hello portálu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Instalace nové doménové struktury služby Active Directory na virtuální síť Azure](active-directory-new-forest-virtual-machine.md)
* [Instalace repliky řadiče domény služby Active Directory v Azure](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IaaS IT specialista: Základy (01) virtuálního počítače](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS IT specialista: (05) vytváření virtuální sítě a připojení mezi různými místy](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Úvod
Hello základní požadavky pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure se liší velmi málo z nasazení v místní virtuální počítače (a toosome rozsah, fyzických počítačů). Například v hello případě systému Windows Server AD DS, pokud jsou řadiče domény hello (řadiče domény), které nasazujete na virtuálních počítačích Azure repliky v existující místní podnikové doméně nebo doménové struktuře a potom v hello lze do značné míry zacházet hello nasazení Azure stejně jako jste zacházet jakoukoli jinou lokalitu další Windows Server Active Directory. To znamená podsítě musí být definován v systému Windows Server AD DS, vytvořili lokality, podsítě hello propojené toothat lokality a připojené tooother lokalit pomocí odkazů na příslušné lokality. Existují, ale některé rozdíly, které jsou společné tooall Azure nasazení a některé, který se liší podle scénář toohello konkrétní nasazení. Níže jsou uvedeny dva základní rozdíly:

### <a name="azure-virtual-machines-may-need-connectivity-toohello-on-premises-corporate-network"></a>Virtuální počítače Azure může být nutné připojení k toohello místní podnikové síti.
Připojení virtuálních počítačů Azure back tooan místní podnikové sítě vyžaduje virtuální síť Azure, která zahrnuje site-to-site nebo bodu lokality virtuální privátní sítě (VPN) schopný tooseamlessly součást připojit virtuální počítače Azure a místní počítače. Tato součást VPN může také povolit místní domény členského počítače tooaccess Windows Server Active Directory domény, jejichž řadičích domény hostovaných výhradně na virtuálních počítačích Azure. Je důležité toonote, ale, že pokud hello VPN se nezdaří, ověřování a další operace, které jsou závislé na Windows Server Active Directory se také nezdaří. Když uživatelé může být schopný toosign pomocí stávající pověření uložená v mezipaměti, všechny peer-to-peer nebo pokusy o ověření klienta se serverem pro které lístky mít ještě toobe vydané nebo se staly zastaralé se nezdaří.

V tématu [virtuální sítě](http://azure.microsoft.com/documentation/services/virtual-network/) pro předvedení video a seznam podrobných návodů, včetně [konfigurace VPN typu Site-to-Site v hello portál Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Windows Server Active Directory můžete taky nasadit v Azure virtuální síti, která nemá připojení k místní síti. Hello pokyny v tomto tématu, ale předpokládá, že virtuální sítě Azure je použít, protože poskytuje možnosti, které jsou nezbytné tooWindows serveru adresování IP adres.
> 
> 

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statické IP adresy musí být nakonfigurované v prostředí Azure PowerShell.
Dynamické adresy jsou přiděleny ve výchozím nastavení, ale místo toho použijte tooassign rutiny Set-AzureStaticVNetIP hello statickou IP adresu. Statickou IP adresu, která zachová prostřednictvím službou opravy a vypnutí nebo restartování virtuálního počítače, který nastavuje. Další informace najdete v tématu [statické interní IP adresu pro virtuální počítače](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Termíny a definice
Hello následuje doplňovat seznam termínů pro různé Azure technologie, které se bude odkazovat v tomto článku.

* **Virtuální počítače Azure**: hello nabídkou IaaS v Azure, která umožňuje zákazníkům toodeploy virtuální počítače se systémem téměř všechny tradičně místní zatížení serveru.
* **Virtuální síť Azure**: hello síťové služby v Azure, která umožňuje zákazníkům vytvářet a spravovat virtuální sítě v Azure a bezpečně připojit je tootheir vlastní místní síťové infrastruktury pomocí virtuální privátní sítě (VPN).
* **Virtuální IP adresy**: internetového IP adres, který není vázaná tooa konkrétní počítač nebo síťové karty rozhraní. Cloudové služby jsou přiřazeny virtuální IP adresu pro příjem síťový provoz, který je přesměrovaného tooan virtuálního počítače Azure. Virtuální IP adresa je vlastnost cloudových služeb, která může obsahovat jeden nebo více virtuálních počítačích Azure. Všimněte si také, že virtuální sítě Azure může obsahovat jeden nebo více cloudových služeb. Virtuální IP adresy poskytují nativní funkce Vyrovnávání zatížení.
* **Dynamickou IP adresu**: Toto je hello IP adresu, která je jenom interní. Ho by měl být nakonfigurovaný jako statickou IP adresu (pomocí rutiny Set-AzureStaticVNetIP hello) pro virtuální počítače, které jsou hostiteli rolí serveru hello řadiče domény a DNS.
* **Služba opravy**: hello procesu, ve kterém Azure automaticky vrátí service tooa spuštěná stavu znovu po zjištění hello služby se nezdařilo. Služba opravy je jeden z hello aspektů Azure, který podporuje dostupnost a odolnost. Při nepravděpodobné, výsledek hello následující služby opravy incident pro řadič domény spuštěn na virtuálním počítači je podobné tooan neplánované restart, ale má několik vedlejší účinky:
  
  * změní Hello virtuální síťový adaptér v hello virtuálních počítačů
  * změní Hello adresa MAC virtuálního síťového adaptéru hello
  * změní Hello procesoru/procesoru ID hello virtuálních počítačů
  * Hello konfigurace protokolu IP hello virtuálního síťového adaptéru nedojde ke změně, dokud, hello virtuální počítač je připojený tooa virtuální sítě a hello IP adresu Virtuálního počítače je statický.
  
  Žádná z těchto projevů ovlivnit Windows Server Active Directory, protože nemá žádné závislosti na adresu MAC hello nebo ID procesoru/procesoru, a všechna nasazení systému Windows Server Active Directory v Azure – se doporučují toobe spustit na virtuální síť Azure, jak je uvedeno výš .

## <a name="is-it-safe-toovirtualize-windows-server-active-directory-domain-controllers"></a>Je bezpečné toovirtualize řadiče domény Windows Server Active Directory?
Nasazení systému Windows Server Active Directory řadiče domény na virtuálních počítačích Azure je subjektu toohello stejné pokyny jako spuštěná řadiče domény na místě ve virtuálním počítači. Spuštění virtualizovaných řadičů domény je bezpečné postupem, tak dlouho, dokud se řídit pokyny pro zálohování a obnovení řadiče domény. Další informace o omezení a pokyny pro spuštění virtualizovaných řadičů domény najdete v tématu [spouštění řadičů domény v Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisory zadejte nebo trivialize technologie, které může způsobit problémy pro mnoho distribuovaných systémů, včetně systému Windows Server Active Directory. Například na fyzickém serveru, můžete vytvořit kopii disku nebo pomocí nepodporované metody tooroll back hello stavu serveru, včetně použití sítě SAN a tak dále, ale učinit na fyzickém serveru je mnohem obtížnější než obnovení snímku virtuálního počítače v hypervisoru. Azure nabízí funkce, které může mít za následek hello stejné nežádoucího stavu. Například byste neměli kopírovat soubory virtuálního pevného disku řadičů domény s namísto provádění pravidelných záloh, protože obnovení je může mít za následek podobné funkce obnovení toousing snímku situaci.

Takové odvolání zavést bublinách USN, které můžou vést odlišných stavy toopermanently mezi řadiče domény. Například, může způsobit problémy:

* Přetrvávajících odstraněných objektů
* Nekonzistentní hesla
* Hodnoty atributů nekonzistentní
* Schéma se neshoduje, pokud je hlavní server schémat hello vrácena zpět

Další informace o tom, jak dopad na řadičích domény najdete v tématu [hodnoty USN a vrácení hodnot USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Od verze Windows Server 2012, [další bezpečnostní opatření jsou integrované v tooAD DS](https://technet.microsoft.com/library/hh831734.aspx). Tato bezpečnostní opatření pomoc při ochraně virtualizovaných řadičů domény proti hello zmíněnými problémy, tak dlouho, dokud hello základní platformu hypervisoru VM-GenerationID podporuje. Azure podporuje VM-GenerationID, což znamená, že řadiče domény se systémem Windows Server 2012 nebo novější na Azure virtuální počítače mají další bezpečnostní opatření hello.

> [!NOTE]
> Měli byste vypnout a restartovat virtuální počítač, který spouští hello role řadiče domény v Azure v rámci hello hostovaného operačního systému místo použití hello **vypnout** možnost v hello Azure Portal. V současné době pomocí portálu tooshut hello dolů virtuálního počítače způsobí, že toobe virtuálních počítačů hello navrácena. Deallocated virtuální počítač má výhod hello není nabíhání poplatků za, ale také obnoví hello VM-GenerationID, což je žádoucí, aby řadič domény. Při obnovení hello VM-GenerationID hello invocationID databáze hello AD DS je také obnovit, budou zahozeny hello fond identifikátorů RID a adresáře SYSVOL je označena jako neautoritativní. Další informace najdete v tématu [tooActive Úvod virtualizace Directory Domain Services (AD DS)](https://technet.microsoft.com/library/hh831734.aspx) a [bezpečná virtualizace DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).
> 
> 

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Proč nasazení systému Windows Server AD DS na virtuálních počítačích Azure?
Mnoho scénářů nasazení systému Windows Server AD DS jsou vhodné pro nasazení jako virtuální počítače na platformě Azure. Předpokládejme například, že máte společnosti v Evropě, které musí uživatelé tooauthenticate ve vzdáleném umístění v Asii. Hello společnosti nebyla nasazena dříve systému Windows Server Active Directory řadiče domény v Asii kvůli toohello náklady toodeploy je a omezené znalosti toomanage hello serverů po nasazení. V důsledku toho jsou žádosti o ověření z Asie obsluhovány pomocí řadiče domény v Evropě s získat výsledky. V takovém případě můžete nasadit řadič domény na virtuální počítač, který jste zadali, je nutné spustit v rámci hello datové centrum Azure v Asii. Tento řadič domény tooan virtuální síť Azure, který je připojený přímo toohello vzdáleného umístění bude zlepšit výkon ověřování se připojuje.

Azure je také vhodné jako nahraďte toootherwise nákladná po havárii (DR) obnovení lokality. Hello relativně nízkonákladové hostování malý počet řadičů domény a jedné virtuální sítě v Azure představuje alternativu atraktivní.

Nakonec můžete toodeploy síťové aplikace v Azure, například SharePoint, které vyžaduje Windows Server Active Directory, ale nemá žádné závislosti na hello místní sítě nebo hello podnikové Windows Server Active Directory. V takovém případě nasazení izolované doménové struktury na Azure toomeet hello SharePoint požadavky na serveru je optimální. Znovu nasazení síťových aplikací, které vyžadují připojení toohello místní sítě a hello podnikové služby Active Directory je také podporována.

> [!NOTE]
> Vzhledem k tomu, které poskytuje připojení vrstvy 3, hello součásti sítě VPN, který zajišťuje, že připojení mezi virtuální sítě Azure a místní sítě můžete také povolit členské servery se systémem tooleverage řadiče domény místní, které běží jako virtuální počítače Azure na Azure virtuální síť. Ale pokud hello VPN není k dispozici, komunikace mezi místní počítače a řadiče domény založené na Azure nebude fungovat, což vede k ověřování a různých dalších chyb.  
> 
> 

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Uvádí vedle sebe mezi nasazením řadiče domény Windows Server Active Directory ve virtuálních počítačích Azure a místními
* Pro každý scénář nasazení systému Windows Server Active Directory, která obsahuje více než jeden virtuální počítač je nutné toouse virtuální sítě Azure konzistenci IP adres. Všimněte si, že tato příručka předpokládá, že řadiče domény běží na virtuální síť Azure.
* Stejně jako u řadičů domény na místě, se doporučuje statické IP adresy. Statickou IP adresu můžete nakonfigurovat jenom pomocí prostředí Azure PowerShell. V tématu [statické interní IP adresu pro virtuální počítače](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) další podrobnosti. Pokud máte monitorování systémů nebo jiná řešení, které kontrolují pro konfiguraci statických IP adres v rámci hello hostovaného operačního systému, můžete přiřadit hello stejné statické IP adresy toohello vlastnosti síťového adaptéru z hello virtuálních počítačů. Ale mějte na paměti, že tento síťový adaptér hello se zahodí, pokud hello virtuálních počítačů zde nevyskytlo službou opravy nebo je vypnutý hello portálu a má zrušeno jeho adresy. V takovém případě hello statickou IP adresu v rámci hosta hello toobe potřebovat resetovat.
* Nasazení virtuálních počítačů ve virtuální síti nenaznačuje (ani vyžadovat) připojení tooan zpět do místní sítě; virtuální síť Hello jenom umožňuje tuto možnost. Musíte vytvořit virtuální síť pro privátní komunikaci mezi Azure a v místní síti. Je nutné toodeploy koncový bod sítě VPN v místní síti hello. Hello VPN se otevře z Azure toohello do místní sítě. Další informace najdete v tématu [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md) a [konfigurace VPN typu Site-to-Site v hello portálu Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Možnost příliš[vytvořit síť VPN point-to-site](../vpn-gateway/vpn-gateway-point-to-site-create.md) je k dispozici tooconnect jednotlivých počítačů se systémem Windows přímo tooan virtuální síť Azure.
> 
> 

* Bez ohledu na to, jestli vytvoříte virtuální sítě nebo Ne, vytvořit Azure poplatky za odchozí přenosy, ale není příchozí. Různých volbách návrhu Windows Server Active Directory může ovlivnit, kolik odchozí provoz generuje pomocí nasazení. Například nasazení řadiče domény jen pro čtení (RODC) omezuje odchozí provoz protože nereplikuje odchozí. Ale hello rozhodnutí toodeploy řadič musí toobe porovnat hello nutné tooperform operací proti hello řadiče domény a hello zápisu [kompatibility](https://technet.microsoft.com/library/cc755190) , aplikace a služby v lokalitě hello mít s řadičů jen pro čtení. Další informace o poplatky za provozu najdete v tématu [Azure ceny na přehled](http://azure.microsoft.com/pricing/).
* Když máte plnou kontrolu nad co toouse prostředky serveru pro virtuální počítače na místní, jako je například paměti RAM, velikost disku a tak dále, v Azure musíte vybrat ze seznamu velikostí předkonfigurované serveru. Pro řadič domény je potřeba datový disk v přidání toohello disk operačního systému v databázi služby Active Directory pro Windows Server hello toostore pořadí.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Můžete nasadit systém Windows Server AD FS na virtuálních počítačích Azure?
Ano, můžete nasadit systém Windows Server AD FS na virtuálních počítačích Azure a hello [osvědčené postupy pro nasazení služby AD FS](https://technet.microsoft.com/library/dn151324.aspx) místní použít stejnou měrou tooAD FS nasazení v Azure. Ale některé hello osvědčených postupů, jako Vyrovnávání zatížení a vysoké dostupnosti vyžadovat technologie nad rámec co služby AD FS nabízí sám sebe. Musí být uvedeny základní infrastruktura hello. Umožňuje zkontrolovat některé z těchto osvědčené postupy a zjistit, jak lze dosáhnout pomocí virtuální sítě Azure a virtuální počítače Azure.

1. **Nikdy neměli zveřejňovat servery tokenu služba TOKENŮ zabezpečení přímo toohello Internetu.**
   
    To je důležité, protože hello STS vydává tokeny zabezpečení. V důsledku toho servery služby tokenů zabezpečení, jako je serverů služby AD FS zacházet se hello stejná úroveň ochrany jako řadič domény. Pokud dojde k narušení služby tokenů zabezpečení, uživatelé se zlými úmysly hello možnost tooissue přístupové tokeny potenciálně obsahující deklarace identity jejich zvolíte aplikací strany toorelying a dalších serverů služby tokenů zabezpečení v důvěřující organizace.
2. **Nasazení řadiče domény služby Active Directory pro všechny uživatele domény ve stejné sítě jako servery služby AD FS hello hello.**
   
    Servery služby AD FS pomocí tooauthenticate uživatele Active Directory Domain Services. Doporučujeme toodeploy řadiče domény na hello stejné sítě jako servery hello AD FS. To poskytuje provozní kontinuitu, v případě hello propojení mezi hello síť Azure a místní sítě je poškozený a umožňuje nižší latenci a vyšší výkon pro přihlášení.
3. **Nasazení několika uzlů služby AD FS pro vysokou dostupnost a vyrovnávání zatížení hello.**
   
    Ve většině případů nepřijatelné hello selhání aplikace, která umožňuje služby AD FS, protože hello aplikace, které vyžadují tokeny zabezpečení jsou často klíčové operace. Výsledkem je a protože služby AD FS se nyní nachází v hello kritické cesty tooaccessing kritické podnikové procesy, služba hello AD FS musí být vysoce dostupný prostřednictvím více serverů služby AD FS a proxy servery služby AD FS. distribuce tooachieve požadavků, nástroje pro vyrovnávání zatížení se obvykle nasazují před hello AD FS proxy servery a servery hello služby AD FS.
4. **Nasaďte jeden nebo více uzlů Proxy webových aplikací pro přístup k Internetu.**
   
    Pokud se uživatelé musí aplikace tooaccess chráněného službou hello AD FS, hello hello AD FS služby potřebám toobe dostupný z Internetu. Toho se dosáhne nasazení služby Proxy webových aplikací hello. Důrazně doporučujeme toodeploy více než jeden uzel pro účely hello vysokou dostupnost a vyrovnávání zatížení.
5. **Omezte přístup z hello Proxy webových aplikací uzly toointernal síťovým prostředkům.**
   
    hello tooallow tooaccess externích uživatelů služby AD FS z Internetu, musíte uzly toodeploy Proxy webových aplikací (nebo Proxy služby AD FS v dřívějších verzích systému Windows Server). Hello proxy webových aplikací, které jsou uzly přímo zveřejněné toohello Internetu. Nejsou požadované toobe připojený k doméně a pouze potřebovat přistupují toohello AD FS servery přes porty TCP 443 a 80. Důrazně doporučujeme tuto komunikaci tooall blokováno. ostatní počítače (zejména řadiče domény).
   
    To je obvykle dosažených místní prostřednictvím DMZ. Brány firewall pomocí režimu povolených operaci toorestrict provozu z hello DMZ toohello do místní sítě (tedy jenom provoz z hello IP adresy zadané a přes zadané porty je povolené, a všechny ostatní přenosy jsou blokovány).

Hello následující diagram znázorňuje tradiční místní nasazení služby AD FS.

![Diagram nasazení tradičních místních Active Directory Federation Services](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Ale protože Azure nenabízí nativní, plně vybavený brány firewall schopností, potřebovat další možnosti toobe používá toorestrict provoz. Hello následující tabulka uvádí jednotlivé možnosti a jeho výhody a nevýhody.

| Možnost | Výhody | Nevýhodou |
| --- | --- | --- |
| [Seznamy ACL síť Azure](../virtual-network/virtual-networks-acl.md) |Méně nákladná a jednodušší počáteční konfigurace |Konfigurace seznamu ACL další sítě vyžaduje, když nových virtuálních počítačů se přidají toohello nasazení |
| [Barracuda NG brány firewall](https://www.barracuda.com/products/ngfirewall) |Seznam povolených adres režimu operace a vyžaduje žádná konfigurace seznamu ACL sítě |Vyšší náklady a složitost pro počáteční nastavení |

Postup vysoké úrovně toodeploy Hello služby AD FS v tomto případě jsou následující:

1. Vytvoření virtuální sítě s připojením mezi různými místy, buď pomocí sítě VPN nebo [ExpressRoute](http://azure.microsoft.com/services/expressroute/).
2. Nasazení řadiče domény ve virtuální síti hello. Tento krok je volitelný, ale doporučené.
3. Nasaďte servery služby AD FS připojeném k doméně ve virtuální síti hello.
4. Vytvoření [s vyrovnáváním zatížení interní](http://azure.microsoft.com/blog/internal-load-balancing/) , obsahuje servery hello služby AD FS a používá nový privátní IP adresy v rámci virtuální sítě hello (dynamickou IP adresu).
   
   1. Aktualizujte toocreate hello plně kvalifikovaný název domény toopoint toohello privátní (dynamické) IP adresa DNS sady hello interní skupinu s vyrovnáváním zatížení.
5. Vytvoření cloudové služby (nebo samostatné virtuální sítě) pro hello uzly Proxy webových aplikací.
6. Nasaďte uzly hello Proxy webových aplikací v hello cloudovou službu nebo virtuální sítě
   
   1. Vytvořte sadu externí skupinu s vyrovnáváním zatížení, který obsahuje uzel, hello Proxy webových aplikací.
   2. Aktualizujte hello externí DNS název (FQDN) toopoint toohello cloudové služby veřejnou IP adresu (hello virtuální IP adresy).
   3. Konfigurace služby AD FS proxy toouse hello plně kvalifikovaný název domény, která odpovídá sadu toohello interní skupinu s vyrovnáváním zatížení pro servery hello AD FS.
   4. Aktualizovat weby založené na deklaracích toouse hello externí plně kvalifikovaný název domény pro jejich poskytovatele deklarací identity.
7. Omezení přístupu mezi počítači tooany Proxy webových aplikací ve virtuální síti hello AD FS.

toorestrict provoz hello sady vyrovnáváním zatížení nástroje pro vyrovnávání zatížení Azure interní hello potřebuje toobe nakonfigurované pro pouze provoz tooTCP porty 80 a 443 a všechny ostatní přenosy toohello interní dynamické IP adresy hello skupinu s vyrovnáváním zatížení se ukončí.

![Diagram sítě ACL služby AD FS heslem, TCP 443 a 80 povolené.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Provoz toohello AD FS servery by byl povolen pouze pomocí hello následující zdroje:

* Nástroje pro vyrovnávání zatížení Azure interní Hello.
* IP adresa Hello správce hello do místní sítě.

> [!WARNING]
> návrh Hello zabránit dosažení ostatní virtuální počítače v hello virtuální síť Azure nebo žádná umístění v místní síti hello uzly Proxy webových aplikací. Můžete provést konfiguraci pravidel brány firewall v hello místní zařízení u připojení Express Route nebo hello zařízení VPN pro připojení VPN typu site-to-site.
> 
> 

Jako možnost toothis nevýhodou je hello nutné tooconfigure hello síť seznamy ACL pro více zařízení, včetně interní vyrovnávání zátěže, servery hello služby AD FS a žádné jiné servery, které se přidají toohello virtuální sítě. Pokud žádné zařízení přidána toohello nasazení bez konfigurace sítě ACL toorestrict provoz tooit, hello celé nasazení může být ohrožena. Pokud hello IP adresy Proxy webových aplikací uzlů hello někdy, musí se obnovit hello sítě seznamy řízení přístupu (což znamená, že proxy hello by měl být nakonfigurovaný toouse [statické IP adresy dynamické](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![Služba AD FS v Azure s sítě ACL.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Další možností je toouse hello [Barracuda NG Firewall](https://www.barracuda.com/products/ngfirewall) zařízení toocontrol provoz mezi servery hello služby AD FS a proxy servery služby AD FS. Tato možnost je v souladu s osvědčenými postupy pro zabezpečení a vysokou dostupnost a vyžaduje menší správy po počáteční instalaci hello, protože zařízení brány Firewall NG Barracuda hello poskytuje seznam povolených adres režimu správy brány firewall a dá se nainstalovat přímo na virtuální síť Azure. Který eliminuje hello nutné tooconfigure sítě ACL kdykoli na nový server se přidá toohello nasazení. Ale tuto možnost přidá počáteční nasazení složitost a náklady.

V takovém případě se místo jeden nasadí dvě virtuální sítě. Zavoláme vám je VNet1 a VNet2. VNet1 hello proxy a VNet2 obsahuje hello STSs a hello síťové připojení back toohello podnikové síti. VNet1 je proto fyzicky (i když prakticky) izolované od VNet2 a pak z podnikové sítě hello. VNet1 pak připojené tooVNet2 používá speciální technologie tunelových propojení známé jako přenosu nezávislé sítě architektura (VPDI). Hello VPDI tunelové propojení je připojený tooeach hello virtuální sítě pomocí brány firewall Barracuda NG – jeden Barracuda na každém hello virtuální sítě.  Pro vysokou dostupnost se doporučuje nasazení dvou barakudy v každé virtuální síti; jednu aktivní hello jiných pasivní. Nabízejí velmi bohaté fungující brána firewall může možnosti, které nám umožňují toomimic hello operace DMZ tradičních místních v Azure.

![Služba AD FS v Azure s bránou firewall.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Další informace najdete v tématu [služby AD FS: rozšířit deklaracemi identity místní aplikace front-endu toohello Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-tooad-fs-deployment-if-hello-goal-is-office-365-sso-alone"></a>Nasazení služby alternativní tooAD služby FS, pokud hello cílem je Office 365 SSO samostatně
Pokud je vaším cílem je pouze tooenable přihlášení pro Office 365 je jiný úplně alternativní toodeploying AD FS. V takovém případě můžete jednoduše nasadit DirSync s heslo synchronizace místně a dosáhnout hello stejný konečný výsledek s minimálním nasazení složitost, protože tento přístup nevyžaduje služby AD FS a Azure.

Hello následující tabulka porovnává fungování hello přihlášení procesy a bez nasazení služby AD FS.

| Office 365 jeden přihlašování pomocí služby AD FS a DirSync | Office 365 stejné přihlášení pomocí nástroje DirSync + synchronizace hesla |
| --- | --- |
| 1. hello uživatel protokolů v podnikové síti tooa a je ověřený tooWindows Server Active Directory. |1. hello uživatel protokolů v podnikové síti tooa a je ověřený tooWindows Server Active Directory. |
| 2. hello uživatel pokusí tooaccess Office 365 (jsem @contoso.com). |2. hello uživatel pokusí tooaccess Office 365 (jsem @contoso.com). |
| 3. Office 365 přesměruje uživatele tooAzure hello AD. |3. Office 365 přesměruje uživatele tooAzure hello AD. |
| 4. Vzhledem k tomu, že Azure AD nemůže ověřit uživatele hello a jste srozuměni s tím, že existuje vztah důvěryhodnosti s AD FS na místě, přesměruje uživatele tooAD hello služby FS. |4. Azure AD nemůže přímo přijímat lístky protokolu Kerberos a neexistuje žádný vztah důvěryhodnosti, požaduje tento uživatel hello zadejte přihlašovací údaje. |
| 5. hello uživatel odešle protokolu Kerberos lístek tokenů zabezpečení toohello AD FS. |5. hello uživatel zadá hello stejné místní heslo, a je Azure AD ověří proti hello uživatelské jméno a heslo, které byl synchronizován pomocí nástroje DirSync. |
| 6. Služba AD FS transformuje hello toohello lístek protokolu Kerberos nutné token formátu nebo deklarací identity a přesměruje uživatele tooAzure hello AD. |6. Azure AD přesměruje uživatele tooOffice hello 365. |
| 7. hello uživatel se ověřuje tooAzure AD (jiné transformace dojde). |7. hello uživatel může přihlásit tooOffice 365 a aplikace Outlook Web Access pomocí tokenu hello Azure AD. |
| 8. Azure AD přesměruje uživatele tooOffice hello 365. | |
| 9. hello uživatel je přihlášený bezobslužně na tooOffice 365. | |

V Office 365 s nástrojem DirSync hello s scénář synchronizace hesel (žádné AD FS) jednotné přihlašování je nahrazena "stejné přihlášení" kde "stejné" jednoduše znamená, že uživatelé musí znovu zadat jejich stejné místních přihlašovacích údajů při přístupu k Office 365. Všimněte si, že tato data můžete zapamatovávají hello uživatele prohlížeče toohelp snížit další výzvy.

### <a name="additional-food-for-thought"></a>Další myšlenku for jídlo
* Pokud nasadíte proxy služby AD FS na virtuální počítač Azure, je potřeba připojení toohello AD FS servery. Pokud jsou na místě, doporučuje se, že využíváte připojení VPN typu site-to-site hello poskytované hello virtuální sítě tooallow hello Proxy webových aplikací uzly toocommunicate s jejich serverech AD FS.
* Pokud nasadíte server služby AD FS na Azure virtuální počítače, připojení řadiče domény služby Active Directory serveru tooWindows, úložiště atributů a konfigurační databáze je nutné a může také vyžadovat ExpressRoute nebo připojení site-to-site VPN mezi hello Azure virtuální sítě a hello místní síť.
* Poplatky jsou použity tooall provoz z virtuálního počítače Azure (odchozí provoz). Pokud cena hello řízení faktor, je vhodné toodeploy hello Proxy webových aplikací, uzly v Azure, ponechat hello služby AD FS servery místně. Pokud na virtuálních počítačích Azure také se nasazuje hello AD FS serverů, budou dodatečné poplatky vzniklých tooauthenticate místních uživatelů. Výchozí přenos způsobuje náklady bez ohledu na to, zda je procházení hello ExpressRoute nebo hello připojení site-to-site VPN.
* Pokud se rozhodnete toouse Azure nativní server možnosti vyrovnávání zatížení pro vysokou dostupnost serverů služby AD FS, Všimněte si, že vyrovnávání zatížení poskytuje sondy, které jsou používané toodetermine hello stavu hello virtuálních počítačů v rámci hello cloudové služby. V případě hello virtuální počítače Azure (jako názvem na rozdíl od tooweb nebo pracovních rolí) musí být vlastní test paměti použít, protože hello agent, který odpovídá toohello výchozí sondy se nenachází na virtuálních počítačích Azure. Pro jednoduchost, můžete použít vlastní test paměti TCP – to vyžaduje pouze, připojení protokolu TCP (segment TCP SYN odesílají a odpověděl toowith segment TCP SYN ACK) byly úspěšně navázáno toodetermine stavu virtuálního počítače. Můžete nakonfigurovat vlastní test paměti toouse hello žádné toowhich port TCP, které jsou aktivně naslouchá virtuálních počítačů.

> [!NOTE]
> Počítače, které potřebují hello tooexpose stejné nastavení portů přímo toohello Internetu (například port 80 a 443) nelze sdílet hello stejné cloudové služby. Proto se doporučuje vytvořit vyhrazené cloudové služby, pro vaše servery služby Windows Server AD FS v pořadí tooavoid potenciální překrývá mezi požadavky na porty pro aplikace a služby Windows Server AD FS.
> 
> 

## <a name="deployment-scenarios"></a>Scénáře nasazení
Hello následující část popisuje běžné nasazení scénáře toodraw pozornost tooimportant aspekty, které musí vzít v úvahu. Každý scénář má odkazy toomore podrobnosti o hello rozhodnutí a tooconsider faktorů.

1. [Služba AD DS: Nasazení aplikace podporující AD DS s žádný požadavek na připojení k podnikové síti](#BKMK_CloudOnly)
   
    Například služby SharePoint internetového je nasazen na virtuální počítač Azure. Hello aplikace nemá žádné závislosti v prostředcích podnikové sítě. Hello aplikace vyžaduje Windows Server AD DS, ale nevyžaduje hello podnikové služby Windows Server AD DS.
2. [Služby AD FS: Deklaracemi identity místní aplikace front-endu toohello Internetu rozšíření](#BKMK_CloudOnlyFed)
   
    Například deklaracemi aplikace, který byl úspěšně nasadit místně a používat firemní uživatelé musí toobecome přístupné z Internetu hello. aplikace Hello musí toobe přistupovat přímo přes hello Internet obchodními partnery pomocí své vlastní firemní identity a také existující podnikovým uživatelům.
3. [Služby AD DS: Nasazení Windows Server AD DS podporující aplikaci, která vyžaduje připojení toohello podnikové sítě](#BKMK_HybridExt)
   
    Například aplikace podporující LDAP, který podporuje ověřování integrované v systému Windows a Windows Server AD DS se používá jako úložiště pro data konfigurace a profil uživatele je nasazen na virtuální počítač Azure. Je žádoucí, aby hello tooleverage aplikace hello existující podnikové služby Windows Server AD DS a poskytovat jednotné přihlašování. aplikace Hello není pracujícím s deklaracemi.

### <a name="BKMK_CloudOnly"></a>1. Služba AD DS: Nasazení aplikace podporující AD DS s žádný požadavek na připojení k podnikové síti
![Nasazení služby AD DS jenom pro cloud](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**obrázek 1**

#### <a name="description"></a>Popis
SharePoint je nasazen na virtuální počítač Azure a hello aplikace nemá žádné závislosti v prostředcích podnikové sítě. Hello aplikace vyžaduje Windows Server AD DS však *není* vyžadují hello podnikové služby Windows Server AD DS. Vzhledem k tomu, že jsou uživatelé samoobslužné zřízené prostřednictvím aplikace hello do domény hello systému Windows Server AD DS, která je také hostovaná v hello cloudu na virtuálních počítačích Azure, je požadováno žádné protokolu Kerberos nebo federované vztahy důvěryhodnosti.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>Aspekty scénář a jak používat technologie oblasti toohello scénář
* [Topologie sítě](#BKMK_NetworkTopology): vytvoření virtuální sítě Azure bez připojení mezi různými místy (také označované jako připojení site-to-site).
* [Konfigurace nasazení řadiče domény](#BKMK_DeploymentConfig): nasaďte nový řadič domény do nové, jednou doménou, doménové struktury Windows Server Active Directory. To by měly být nasazeny společně s server Windows DNS hello.
* [Topologie lokality služby Active Directory pro Windows Server](#BKMK_ADSiteTopology): použití hello výchozí server Windows Server Active Directory (všechny počítače se bude v Default-First-Site-Name).
* [Adresování IP a DNS](#BKMK_IPAddressDNS):
  
  * Nastavte statickou IP adresu pro hello řadič domény pomocí rutiny Set-AzureStaticVNetIP Azure Powershellu hello.
  * Instalace a konfigurace serveru Windows Server DNS na hello počet řadičů domény v Azure.
  * Konfigurace vlastností virtuální sítě hello hello název a IP adresu hello virtuálního počítače, který je hostitelem role serveru hello řadič domény a DNS.
* [Globální katalog](#BKMK_GC): hello prvního řadiče domény v doménové struktuře hello musí být server globálního katalogu. Další řadiče domény by také nakonfigurovat jako GC, protože v doménová struktura jedinou doménu, globálního katalogu hello nevyžaduje žádné další práce z hello řadiče domény.
* [Umístění databáze hello systému Windows Server AD DS a adresáře SYSVOL](#BKMK_PlaceDB): přidejte tooDCs disku data spuštěná jako virtuální počítače Azure v pořadí toostore hello Windows Server Active Directory databáze, protokoly a adresáře SYSVOL.
* [Zálohování a obnovení](#BKMK_BUR): Určete, kam chcete toostore zálohování stavu systému. V případě potřeby přidejte další data disku toohello virtuálního počítače řadiče domény toostore zálohy.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: rozšířit deklaracemi identity místní aplikace front-endu toohello Internetu
![Federaci se připojení mezi různými místy](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**na obrázku 2**

#### <a name="description"></a>Popis
Deklaracemi identity aplikace, která byla úspěšně nasadit místně a používá toobecome potřebám podnikovým uživatelům z hello Internet přímo přístupné. Hello aplikace slouží jako databázi SQL tooa front-endu webové ve které ukládá data. servery SQL Hello používá aplikace hello jsou taky umístěné v podnikové síti hello. Dva STSs systému Windows Server AD FS a nástroj na Vyrovnávání zatížení se nasadit místně tooprovide přístup toohello podnikovým uživatelům. Nyní aplikace Hello musí toobe kromě přistupovat přímo přes hello Internet obchodními partnery pomocí své vlastní firemní identity a také existující podnikovým uživatelům.

V toosimplify úsilí a plnění hello nasazení a konfigurace potřebám tento nový požadavek, bude rozhodnuto, že dva další webové frontends a dva proxy servery služby Windows Server AD FS nainstalovat na virtuálních počítačích Azure. Všechny čtyři virtuální počítače budou zveřejněné přímo toohello Internetu a budou poskytovat připojení toohello místní síti pomocí Azure Virtual Network site-to-site VPN funkce.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>Aspekty scénář a jak používat technologie oblasti toohello scénář
* [Topologie sítě](#BKMK_NetworkTopology): vytvoření virtuální sítě Azure a [nakonfigurovat připojení mezi různými místy](../vpn-gateway/vpn-gateway-site-to-site-create.md).
  
  > [!NOTE]
  > Pro každou z certifikátů služby Windows Server AD FS hello zajistěte, aby hello URL definované v rámci šablony certifikátu hello a výsledné certifikáty hello dosažitelný z instancí hello systému Windows Server AD FS, které jsou spuštěné v Azure. Může to vyžadovat tooparts připojení mezi různými místy infrastruktury PKI. Pro příklad, pokud koncový bod hello CRL je založený na LDAP a hostované výhradně místní, připojení mezi různými místy bude požadován. Pokud to není žádoucí, může být nezbytné toouse certifikátů vystavených certifikační Autority, jejichž seznam CRL je přístupný prostřednictvím hello Internetu.
  > 
  > 
* [Konfigurace cloudových služeb](#BKMK_CloudSvcConfig): Zajistěte, abyste měli dva cloudové služby v pořadí poskytují dvě vyrovnáváním zatížení virtuální IP adresy. virtuální IP adresy Hello první cloudové služby bude směrovanou toohello dvě služby Windows Server AD FS proxy virtuální počítače na porty 80 a 443. Hello systému Windows Server AD FS nakonfigurovaný server proxy, které budou virtuální počítače toopoint toohello IP adresu hello místní služby Vyrovnávání zatížení, aby naskenováno předních hello STSs systému Windows Server AD FS. virtuální IP adresy Hello druhý cloudové služby bude směrovanou toohello dva virtuální počítače se systémem front-endu webové hello znovu na porty 80 a 443. Nakonfigurovat vlastní test paměti tooensure hello služby Vyrovnávání zatížení pouze přesměruje přenosy toofunctioning systému Windows Server AD FS proxy a webové front-endu virtuálních počítačů.
* [Konfigurace federačního serveru](#BKMK_FedSrvConfig): konfigurovat Windows Server AD FS jako federační server (STS) toogenerate zabezpečení tokeny pro doménovou strukturu Windows Server Active Directory hello vytvořeny v cloudu hello. Nastavit vztahy důvěryhodnosti zprostředkovatele federace deklarace identity s hello různých partnerů, které chcete tooaccept identity z a nakonfigurovat hello různé aplikace, které chcete toogenerate tokenů pro vztahy důvěryhodnosti předávající strany.
  
    Ve většině scénářů Windows Server AD FS proxy servery jsou nasazeny v postavení internetového z bezpečnostních důvodů při jejich systému Windows Server AD FS federation protějšky zůstat izolované od přímé připojení k Internetu. Bez ohledu na to váš scénář nasazení musíte nakonfigurovat cloudové služby s virtuální IP adresy, které zajistí veřejně vystavené IP adresu a port, který je možné tooload vyrovnávání v rámci vaší dva instancí tokenů zabezpečení systému Windows Server AD FS nebo proxy instancí.
* [Konfigurace vysoké dostupnosti systému Windows Server AD FS](#BKMK_ADFSHighAvail): je vhodné toodeploy farmu služby Windows Server AD FS se aspoň dva servery pro převzetí služeb při selhání a vyrovnávání zatížení. Můžete například chcete tooconsider pomocí hello interní databáze Windows (WID) pro Windows Server AD FS konfigurační data a pomocí hello interní Vyrovnávání zatížení funkce Azure toodistribute příchozí požadavky napříč hello servery ve farmě hello.

Další informace najdete v tématu hello [Průvodce nasazením služby AD DS](https://technet.microsoft.com/library/cc753963).

### <a name="BKMK_HybridExt"></a>3. Služby AD DS: Nasazení Windows Server AD DS podporující aplikaci, která vyžaduje připojení toohello podnikové sítě
![Nasazení služby AD DS mezi různými místy](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**obrázek 3**

#### <a name="description"></a>Popis
Aplikace podporující LDAP je nasazen na virtuální počítač Azure. Podporuje ověřování integrované v systému Windows a Windows Server AD DS se používá jako úložiště pro konfiguraci a uživatelská data profilu. cílem Hello hello tooleverage aplikace hello existující podnikové služby Windows Server AD DS a také jednotné přihlašování. aplikace Hello není pracujícím s deklaracemi. Uživatelé musí také tooaccess hello aplikaci přímo z hello Internetu. toooptimize pro výkon a nákladová, bude rozhodnuto, že dva řadiče domény Další, které jsou součástí podnikové doméně hello nasadit souběžně s hello aplikaci v Azure.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>Aspekty scénář a jak používat technologie oblasti toohello scénář
* [Topologie sítě](#BKMK_NetworkTopology): vytvoření virtuální sítě Azure s [připojení mezi různými místy](../vpn-gateway/vpn-gateway-site-to-site-create.md).
* [Metoda instalace](#BKMK_InstallMethod): nasazovat repliky řadičů domény z hello podnikové domény Windows Server Active Directory. U repliky řadiče domény, můžete nainstalovat Windows Server AD DS na hello virtuálních počítačů a volitelně použití hello instalovat z média (IFM) funkce tooreduce hello množství dat, která potřebuje toobe replikované toohello nový řadič domény během instalace. Podívejte se kurz [instalace repliky řadiče domény služby Active Directory v Azure](active-directory-install-replica-active-directory-domain-controller.md). I když použijete instalaci z média, může být efektivnější toobuild hello místní virtuální řadič domény a přesunout hello celý virtuální pevný Disk (VHD) toohello cloudu namísto replikace během instalace systému Windows Server AD DS. Pro zabezpečení se doporučuje odstranit hello virtuálního pevného disku z místní sítě hello poté, co byl zkopírovaný tooAzure.
* [Topologie lokality služby Active Directory pro Windows Server](#BKMK_ADSiteTopology): vytvoření nového webu Azure v Active Directory Sites and Services. Vytvoření Windows Server Active Directory podsítě objektu toorepresent hello virtuální síť Azure a přidejte web toohello hello podsítě. Vytvořte nové propojení lokalit, která zahrnuje novou lokalitu Azure hello a hello lokality, ve které hello virtuální síť Azure koncového bodu VPN nachází v pořadí toocontrol a optimalizovat Windows Server Active Directory tooand provoz z Azure.
* [Adresování IP a DNS](#BKMK_IPAddressDNS):
  
  * Nastavte statickou IP adresu pro hello řadič domény pomocí rutiny Set-AzureStaticVNetIP Azure Powershellu hello.
  * Instalace a konfigurace serveru Windows Server DNS na hello počet řadičů domény v Azure.
  * Konfigurace vlastností virtuální sítě hello hello název a IP adresu hello virtuálního počítače, který je hostitelem role serveru hello řadič domény a DNS.
* [Geograficky distribuovaná řadiče domény](#BKMK_DistributedDCs): podle potřeby nakonfigurujte další virtuální sítě. Pokud vaše topologie lokality služby Active Directory vyžaduje řadiče domény v zeměpisných oblastí, které odpovídají toodifferent Azure oblastí, než jste má lokalit služby Active Directory toocreate odpovídajícím způsobem.
* [Řadiče domény jen pro čtení](#BKMK_RODC): můžete nasadit řadič v hello Azure lokality, v závislosti na požadavcích na provádění operací s hello řadič domény a hello kompatibility aplikací a služeb zápisu hello lokality s řadičů jen pro čtení. Další informace o kompatibilitě aplikací najdete v tématu hello [příručka kompatibility aplikace řadiče domény jen pro čtení](https://technet.microsoft.com/library/cc755190).
* [Globální katalog](#BKMK_GC): GC jsou požadavky na přihlášení potřebné tooservice v doménových strukturách s více doménami. Pokud nenasazujete serverem globálního katalogu v hello Azure lokality, se budou vynakládá odchozí přenosy podle požadavků na ověření způsobit dotazy GC v jiných lokalitách. toominimize, který provoz, můžete povolit univerzální skupinu členství ukládání do mezipaměti pro hello Azure lokality v Active Directory Sites and Services.
  
    Pokud nasadíte globální Katalog, nakonfigurujte propojení lokalit a lokality propojuje náklady, takže není upřednostňovaný jako zdrojový řadič domény pomocí jiných GC, které je třeba tooreplicate globální Katalog hello v hello Azure site hello stejné oddíly částečné domény.
* [Umístění databáze hello systému Windows Server AD DS a adresáře SYSVOL](#BKMK_PlaceDB): Přidání datového disku tooDCs běžící na virtuálních počítačích Azure v pořadí toostore hello Windows Server Active Directory databáze, protokoly a adresáře SYSVOL.
* [Zálohování a obnovení](#BKMK_BUR): Určete, kam chcete toostore zálohování stavu systému. V případě potřeby přidejte další data disku toohello virtuálního počítače řadiče domény toostore zálohy.

## <a name="deployment-decisions-and-factors"></a>Rozhodnutí o nasazení a faktory
Tato tabulka shrnuje hello Windows Server Active Directory technologické oblasti, které dopad v hello předcházející scénáře a hello odpovídající tooconsider rozhodnutí, s podrobnostmi toomore odkazy níže. Některé oblasti technologie nemusí být scénář nasazení použít tooevery a některé oblasti technologie může být více důležitých scénář nasazení tooa než ostatní technologické oblasti.

Například pokud nasadíte ve virtuální síti repliky řadiče domény a doménové struktury má pouze jednu doménu, pak vyberete toodeploy server globálního katalogu v takovém případě nebude scénář nasazení kritické toohello protože nevytvoří žádné další replikace požadavky. Na hello druhé straně, pokud doménová struktura hello má několik domén a pak hello rozhodnutí toodeploy globálního katalogu ve virtuální síti můžou ovlivnit dostupnou šířku pásma, výkon, ověřování, vyhledávání v adresáři a tak dále.

| Oblast technologie Windows Server Active Directory | Rozhodnutí | Faktory |
| --- | --- | --- |
| [Topologie sítě](#BKMK_NetworkTopology) |Vytvoření virtuální sítě? |<li>Tooaccess požadavky Corp prostředky</li> <li>Authentication</li> <li>Správa účtů</li> |
| [Konfigurace nasazení řadiče domény](#BKMK_DeploymentConfig) |<li>Nasazení samostatné doménové struktuře bez žádné vztahy důvěryhodnosti?</li> <li>Nasazení nové doménové struktury s federací?</li> <li>Nasazení nové doménové struktury s vztah důvěryhodnosti doménové struktury Windows Server Active Directory nebo pomocí protokolu Kerberos?</li> <li>Doménová struktura Corp rozšířit nasazení repliky řadiče domény?</li> <li>Doménová struktura Corp rozšířit nasazení nové podřízené domény nebo větve doménové struktury?</li> |<li>Zabezpečení</li> <li>Dodržování předpisů</li> <li>Náklady</li> <li>Odolnost proti chybám a odolnost proti chybám</li> <li>Kompatibilita aplikací</li> |
| [Topologie lokality služby Active Directory pro Windows Server](#BKMK_ADSiteTopology) |Jak máte nakonfigurovat podsítě, weby a spojení sítí s přenosy toooptimize Azure Virtual Network a minimalizovat náklady? |<li>Definice podsítě a lokality</li> <li>Vlastnosti propojení lokalit a upozornění na změnu</li> <li>Komprese replikace</li> |
| [Adresování IP a DNS](#BKMK_IPAddressDNS) |Jak tooconfigure IP adresy a překlad názvů? |<li>Použít hello použití hello Set-AzureStaticVNetIP rutiny tooassign statickou IP adresu</li> <li>Instalace serveru Windows Server DNS a konfigurace hello vlastnosti virtuální sítě s hello název a IP adresu hello virtuálního počítače, který je hostitelem role serveru hello řadič domény a DNS</li> |
| [Geograficky distribuovaná řadiče domény](#BKMK_DistributedDCs) |Jak tooreplicate tooDCs na samostatné virtuální sítě? |Pokud vaše topologie lokality služby Active Directory vyžaduje řadiče domény v zeměpisných oblastí, které odpovídá toodifferent Azure oblastí, než jste má lokalit služby Active Directory toocreate odpovídajícím způsobem. [Konfigurace virtuální sítě toovirtual síťové připojení](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) tooreplicate mezi řadiči domény v samostatných virtuálních sítích. |
| [Řadiče domény jen pro čtení](#BKMK_RODC) |Použít jen pro čtení nebo zapisovatelného řadiče domény? |<li>HBI nebo PII atributy filtru</li> <li>Filtrovat tajné klíče</li> <li>Odchozí přenosy limit</li> |
| [Globální katalog](#BKMK_GC) |Nainstalovat globálního katalogu? |<li>Pro doménovou strukturu jednou doménou Ujistěte se, GC všechny řadiče domény</li> <li>Pro doménová struktura GC jsou nutné k ověření.</li> |
| [Metoda instalace](#BKMK_InstallMethod) |Jak tooinstall řadič domény v Azure? |Buď: <li>Instalace služby AD DS pomocí prostředí Windows PowerShell nebo nástroj Dcpromo</li> <li>Přesunout virtuální pevný disk jako místní virtuální řadič domény</li> |
| [Umístění databáze hello systému Windows Server AD DS a adresáře SYSVOL](#BKMK_PlaceDB) |Kde je databáze toostore systému Windows Server AD DS, protokolů a složku SYSVOL? |Změňte Dcpromo.exe výchozí hodnoty. Tyto důležité soubory služby Active Directory *musí* umístit do Azure datových disků místo disky operačního systému, které implementují ukládání do mezipaměti. |
| [Zálohování a obnovení](#BKMK_BUR) |Jak toosafeguard a obnovit data? |Vytváření záloh stavu systému |
| [Konfigurace federačního serveru](#BKMK_FedSrvConfig) |<li>Nasazení nové doménové struktury s federací v cloudu hello?</li> <li>Nasazení služby AD FS na místě a vystavit proxy server v cloudu hello?</li> |<li>Zabezpečení</li> <li>Dodržování předpisů</li> <li>Náklady</li> <li>Tooapplications přístup k obchodním partnerům</li> |
| [Konfigurace cloudových služeb](#BKMK_CloudSvcConfig) |Cloudová služba je implicitně nasazené hello prvním vytvoření virtuálního počítače. Potřebujete toodeploy další cloudové služby? |<li>Virtuální počítač nebo virtuální počítače vyžaduje přímé expozice toohello Internetu?</li> <li> Vyžaduje hello služby Vyrovnávání zatížení?</li> |
| [Požadavky na federační server pro veřejné a privátní IP adres (dynamické IP adresy a virtuální IP adresy)](#BKMK_FedReqVIPDIP) |<li>Potřebuje hello systému Windows Server AD FS instance toobe přímo z hello Internetu?</li> <li>Vyžaduje nasazení v cloudu hello aplikace hello vlastní internetový IP adresu a port?</li> |Vytvořte jeden cloudových služeb pro každý virtuální IP adresy, který je požadován pro vaše nasazení |
| [Konfigurace vysoké dostupnosti systému Windows Server AD FS](#BKMK_ADFSHighAvail) |<li>Počet uzlů v mé serverové farmy služby Windows Server AD FS?</li> <li>Kolik toodeploy uzly ve farmě Moje systému Windows Server AD FS proxy?</li> |Odolnost proti chybám a odolnost proti chybám |

### <a name="BKMK_NetworkTopology"></a>Topologie sítě
V pořadí toomeet hello IP adresu konzistence a požadavky na DNS systému Windows Server AD DS, je nutné vytvořit toofirst [virtuální síť Azure](../virtual-network/virtual-networks-overview.md) a připojte tooit vaše virtuální počítače. Při jeho vytváření, musíte se rozhodnout, zda toooptionally rozšíření připojení tooyour místní podnikové sítě, který transparentně připojení Azure virtuální počítače tooon místní počítače – můžete toho dosáhnout pomocí tradičních technologií, VPN a vyžaduje, aby koncový bod VPN být odhalen na hello okraje podnikové sítě hello. To znamená hello VPN je zahájena z podnikové sítě Azure toohello, ne naopak.

Všimněte si, že další poplatky při rozšiřování tooyour virtuální sítě do místní sítě nad rámec hello standardní poplatky, které se vztahují tooeach virtuálních počítačů. Konkrétně existují poplatky za dobu procesoru hello brány virtuální sítě Azure a pro hello odchozí přenosy generované každý virtuální počítač, který komunikuje s místní počítače napříč hello sítě VPN. Další informace o poplatky provoz sítě najdete v tématu [Azure ceny na přehled](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Konfigurace nasazení řadiče domény
Hello způsob, jak nakonfigurovat hello řadiče domény závisí na hello požadavky pro hello služby můžete chtít toorun v Azure. Například můžete nasadit novou doménovou strukturu, jsou izolovány od podnikové doménové struktuře, pro testování testování konceptu, nové aplikace nebo některých jiných krátkodobou projektu, který vyžaduje adresářové služby, ale nejsou specifické přístup toointernal podnikovým prostředkům.

Jako výhody izolované doménové struktuře, které řadiče domény se nereplikuje s místní řadičů domény, výsledkem je menší odchozí síťový provoz generované systémem hello samostatně, přímo snížení nákladů. Další informace o poplatky provoz sítě najdete v tématu [Azure ceny na přehled](http://azure.microsoft.com/pricing/).

Například předpokládejme, že máte požadavky na ochranu osobních údajů pro službu, ale služba hello závisí na tooyour přístup interní Windows Server Active Directory. Pokud jsou povolena toohost dat pro službu hello v hello cloudu, můžete nasadit novou podřízenou doménu pro svoji interní doménovou strukturu v Azure. V takovém případě můžete nasadit řadič domény pro hello nové podřízené domény (bez hello globálního katalogu v aspekty ochrany osobních údajů adresu toohelp pořadí). Tento scénář, společně s repliku nasazení řadiče domény, vyžaduje virtuální sítě pro připojení k síti s vaší místní řadiče domény.

Pokud vytváříte novou doménovou strukturu, vyberte, zda toouse [vztahy důvěryhodnosti služby Active Directory](https://technet.microsoft.com/library/cc771397) nebo [federačních vztahy důvěryhodnosti](https://technet.microsoft.com/library/dd807036). Vyrovnávat požadavky hello závisí na kompatibility, zabezpečení, dodržování předpisů, náklady a odolnost proti chybám. Například tootake výhod [selektivní ověřování](https://technet.microsoft.com/library/cc755844) můžete zvolit toodeploy novou doménovou strukturu v Azure a vytvoření vztahu důvěryhodnosti Windows Server Active Directory mezi hello místní doménové struktury a doménové struktury hello cloudu. Pokud aplikace hello deklaracemi identity, ale můžete nasadit federačních vztahy důvěryhodnosti místo vztahy důvěryhodnosti doménové struktury služby Active Directory. Dalším faktorem bude, že hello náklady replikace tooeither více dat tím, že rozšíří místní cloudu toohello Windows Server Active Directory nebo vytvářet více odchozí přenosy dat v důsledku ověřování a zátěž dotazu.

Požadavky pro dostupnost a odolnost proti chybám také ovlivnit volbu. Například pokud dojde k přerušení hello odkaz, aplikace využívat vztah důvěryhodnosti protokolu Kerberos nebo vztah důvěryhodnosti federace jsou všechny pravděpodobně zcela přerušený Pokud jste nasadili dostatečná infrastrukturu v Azure. Konfigurace alternativních nasazení, třeba repliky řadiče domény (zapisovatelného nebo řadičů jen pro čtení) zvýšit pravděpodobnost hello je možné tootolerate odkaz výpadků.

### <a name="BKMK_ADSiteTopology"></a>Topologie lokality služby Active Directory pro Windows Server
Budete potřebovat toocorrectly definovat lokality a lokality odkazy v pořadí toooptimize provozu a minimalizovat náklady. Lokality, propojení lokalit a podsítí ovlivnit hello topologii replikace mezi řadiče domény a hello tok provozu ověřování. Vezměte v úvahu hello následující poplatky za provoz a nasadit a nakonfigurovat podle toohello požadavky vašemu scénáři nasazení řadiče domény:

* Je nominální poplatek za hodinu pro samotné bráně hello:
  
  * Může být spuštění a zastavení podle potřeby
  * Pokud je zastavena, virtuální počítače Azure jsou izolované od podnikové sítě hello
* Příchozí provoz je zdarma
* Účtovat odchozí přenosy podle příliš[Azure ceny na přehled](http://azure.microsoft.com/pricing/). Vlastnosti lokality propojení mezi místními servery a servery hello cloudu můžete optimalizovat následujícím způsobem:
  
  * Pokud používáte více virtuálních sítí, konfigurace odkazů na lokality hello a jejich náklady správně tooprevent systému Windows Server AD DS z nastavování priorit hello Azure lokality více než jeden, který může poskytovat stejné úrovně služeb bez poplatků hello. Můžete také zakázat hello most všechny lokality možnost propojení (BASL), (která je povolena ve výchozím nastavení). Tím se zajistí, že pouze přímo připojené lokality replikovat sebou. Řadiče domény v přechodně připojené lokality již nejsou možné tooreplicate navzájem přímo, ale musí replikovat pomocí běžných web nebo weby. Pokud z nějakého důvodu nedostupná hello zprostředkující lokalit, replikace mezi řadiče domény v lokalitách přechodně připojené nedojde, i v případě, že připojení mezi lokalitami hello je k dispozici. Nakonec kde žádoucí zůstanou části chování, díky přechodné replikaci, vytvořte lokality mostů propojení lokalit, které obsahují hello odpovídající-spojení sítí a sítí, jako je například podnikové sítě na místě.
  * [Konfigurace náklady na propojení sítí](https://technet.microsoft.com/library/cc794882) správně tooavoid nezamýšleným provoz. Například pokud **zkuste další nejbližší lokalitu** nastavení povoleno, zkontrolujte, zda virtuální síťový hello lokality nejsou hello vedle nejbližší zvýšením hello náklady související hello propojení objektu, který připojí zpět toohello hello Azure site Podniková síť.
  * Konfigurace propojení lokalit [intervaly](https://technet.microsoft.com/library/cc794878) a [plány](https://technet.microsoft.com/library/cc816906) podle tooconsistency požadavky a počet změn objektů. Zarovnává plán replikace s latencí tolerance. Řadiče domény replikovat jenom hello poslední stav hodnoty, takže klesá intervalu replikace hello může významně snížit náklady, pokud je dostatečnou míru změn objektu.
* Pokud je minimalizovat náklady prioritu, zajistěte replikace je naplánována a oznámení o změně není povolen. Toto je výchozí konfigurace hello při replikaci mezi lokalitami. Toto není důležité, pokud nasazujete řadič ve virtuální síti, protože hello RODC nebudou replikovat změny odchozí. Ale pokud nasazujete zapisovatelný řadič domény, můžete Ujistěte se, že propojení lokalit hello není nakonfigurované tooreplicate aktualizace s nepotřebné frekvence. Pokud nasadíte server globálního katalogu (GC), zkontrolujte každé jiné lokality, která obsahuje globální Katalog replikuje oddílů domény ze zdrojového řadiče domény v lokalitě, která je propojená s propojení lokalit nebo lokality odkazy, které mají nižší náklady než hello GC v hello Azure site.
* Je možné toofurther stále snižuje objem síťového přenosu vytvořená replikací mezi lokalitami tak, že změníte algoritmus komprese hello replikace. řídí algoritmus komprese Hello hello REG_DWORD registru položka HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator algoritmus komprese. Hello výchozí hodnota je 3, který korelaci toohello algoritmus Xpress komprimovat. Můžete změnit too2 hello hodnotu, která změny hello tooMSZip algoritmus. Ve většině případů to zvýší hello kompresi, ale odrazu hello výdajů využití procesoru. Další informace najdete v tématu [jak služby Active Directory funguje replikační topologie](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>Adresování IP a DNS
Virtuální počítače Azure jsou ve výchozím nastavení přidělené "pronajaté DHCP adresy". Protože s virtuálním počítačem po dobu životnosti hello hello virtuálního počítače ukládat dynamické adresy virtuální síť Azure, jsou splněny požadavky hello služby Windows Server AD DS.

Výsledkem je když použijete dynamickou adresu v Azure, jste v vliv pomocí statické IP adresy, protože je směrovatelné hello dobu zapůjčení hello a hello dobu zapůjčení hello je rovna toohello životnost hello cloudové služby.

Dynamické adresy hello je však zrušeno, pokud hello virtuálního počítače je vypnutí. tooprevent hello IP adresu z navrácena, můžete [pomocí Set-AzureStaticVNetIP tooassign statickou IP adresu](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Pro překlad IP adres nasadit vlastní (nebo využívat stávající) infrastruktuře serverů DNS; Azure DNS nesplňuje hello advanced potřebám název řešení Windows serveru AD DS. Například ho nepodporuje dynamické záznamy SRV a tak dále. Překlad je položky kritické konfigurace pro klienty připojené k doméně a řadiče domény. Řadiče domény musí být schopný registrace záznamy o prostředcích a překlad záznamy o prostředcích další řadič domény.
Odolnost proti chybám a výkonu z důvodů je služba Windows Server DNS hello optimální tooinstall na hello řadiče domény se spuštěným v Azure. Nakonfigurujte vlastnosti virtuální síť Azure hello se hello název a IP adresu serveru DNS hello. Při spuštění nástroje ostatní virtuální počítače ve virtuální síti hello, jejich překladač nastavení klienta DNS budou nakonfigurovány pomocí serveru DNS v rámci hello dynamické přidělování IP adres.

> [!NOTE]
> Nemůže připojit k místní počítače tooa služby Active Directory systému Windows Server AD DS domény, jehož hostitelem je Azure přímo přes hello Internet. Hello požadavky na porty pro služby Active Directory a připojení k doméně operace hello vykreslit ho nepraktické toodirectly zveřejněte hello potřebné porty a v platnosti, celý řadič domény toohello Internetu.
> 
> 

Virtuální počítače zaregistrovat jejich název DNS automaticky při spuštění nebo když dojde ke změně názvu.

Další informace o tomto příkladu a další příklad, který popisuje, jak tooprovision hello první virtuální počítač a na něm nainstalovat službu AD DS najdete v tématu [instalaci nové doménové struktury služby Active Directory v Microsoft Azure](active-directory-new-forest-virtual-machine.md). Další informace o používání prostředí Windows PowerShell najdete v tématu [nainstalovat Azure PowerShell](/powershell/azureps-cmdlets-docs) a [rutiny pro správu Azure](/powershell/module/azurerm.compute/#virtual_machines).

### <a name="BKMK_DistributedDCs"></a>Geograficky distribuovaná řadiče domény
Azure nabízí výhody při hostování více řadičů domény v různých virtuálních sítích:

* Odolnost proti chybám více lokalit
* Fyzické blízkosti toobranch poboček (nižší latenci)

Informace o konfiguraci přímou komunikaci mezi virtuálními sítěmi najdete v tématu [nakonfigurovat virtuální síť připojení k síti toovirtual](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Řadiče domény jen pro čtení
Je třeba toochoose, zda pro čtení toodeploy-pouze nebo zapisovatelného řadiče domény. Je možné nakloněné toodeploy řadičů jen pro čtení, protože nebude mít fyzické kontrolu nad je však řadičů jen pro čtení jsou navrženou toobe nasazené v umístění, kde jejich fyzické zabezpečení jsou v ohrožení, jako je firemní pobočky.

Azure nemá k dispozici hello fyzické bezpečnostní riziko pobočkové sítě, ale řadičů jen pro čtení může stále prokázat toobe větší finanční efektivita protože hello funkce, které poskytují prostředí vhodné toothese i když velmi různých důvodů. Například pro mít žádná odchozí replikace a jsou možné tooselectively naplnit tajné klíče (hesla). Na hello nevýhodou hello nedostatečná těchto tajných klíčů může vyžadovat toovalidate odchozí přenosy na vyžádání je jako ověřuje uživatele nebo počítač. Ale tajných klíčů můžete selektivně naplněnými a uložili do mezipaměti.

Řadičů jen pro čtení poskytnout další výhody v a kolem HBI a PII obavy, protože můžete přidat, že atributy, které obsahují citlivá data toohello RODC filtrovat sadu atributů (DM). Hello DM je přizpůsobitelné sadu atributů, které nejsou replikované tooRODCs. Hello DM můžete použít jako ochranu v případě, že nejsou povoleny nebo nechcete, aby toostore identifikovatelné osobní údaje nebo HBI v Azure. Další informace najdete v tématu [RODC filtrovanou sadu atributů [(https://technet.microsoft.com/library/cc753459)].

Ujistěte se, že aplikace bude kompatibilní s řadičů jen pro čtení plánování toouse. Mnoho aplikací systému Windows Server Active Directory povolena funkce fungují dobře u řadičů jen pro čtení, ale některé aplikace mohou provádět neefektivnímu nebo nezdaří, pokud nemají přístup tooa zapisovatelný řadič domény. Další informace najdete v tématu [kompatibility příručka aplikace pro řadiče domény jen pro čtení](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globální katalog
Jestli potřebujete toochoose tooinstall na globální katalog (GC). V jedné doméně doménové struktury měli byste nakonfigurovat všechny řadiče domény jako servery globálního katalogu. Protože bude mít žádný další replikace provoz se nebude zvýšit náklady.

V doménové struktuře s více doménami GC jsou nezbytné tooexpand univerzálního členství ve skupinách během procesu ověření hello. Pokud nasadíte není globální Katalog, úlohy na hello virtuální sítě, které ověřují se řadič domény v Azure nepřímo generování ověřování pro odchozí provoz tooquery GC místní během jednotlivé pokusy o ověření.

Hello náklady spojené s GC jsou méně předvídatelný, protože hostují všechny domény (v rámci). Pokud hello zatížení hostitelem služby internetového a ověřuje uživatele ve službě Windows Server AD DS, může být zcela nepředvídatelným hello náklady. toohelp omezit GC dotazy mimo hello cloudu webu během ověřování, můžete [povolit univerzální skupiny členství ukládání do mezipaměti](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Metoda instalace
Jak potřebujete toochoose tooinstall hello řadiče domény ve virtuální síti hello:

* Povýšení nového řadiče domény. Další informace najdete v tématu [nainstalovat novou doménovou strukturu služby Active Directory na virtuální síť Azure](active-directory-new-forest-virtual-machine.md).
* Přesunete virtuální pevný disk ve místní virtuální řadič domény toohello cloudu hello. V takovém případě Ujistěte se, že hello místní virtuální řadič domény je "přesunut," Ne "zkopírovaný" nebo "klonovaný."

Virtuální počítače pouze Azure můžete použijte u řadičů domény (jako názvem na rozdíl od tooAzure "web" nebo "pracovník" role virtuálních počítačů). Jsou odolné a odolnost stavu pro řadič domény se vyžaduje. Virtuální počítače Azure jsou navrženy pro úlohy, jako například řadiče domény.

Použijte nástroj SYSPREP toodeploy nebo klonování řadiče domény. Hello možnost tooclone řadiče domény je pouze k dispozici od verze Windows serveru 2012. Hello funkce klonování vyžaduje podporu VMGenerationID v hello základní hypervisoru. Technologie Hyper-V ve Windows serveru 2012 a Azure virtuálních sítích obě podporují VMGenerationID, dodavatelů softwaru třetích stran virtualizace.

### <a name="BKMK_PlaceDB"></a>Umístění databáze hello systému Windows Server AD DS a adresáře SYSVOL
Vyberte, kde toolocate hello databáze systému Windows Server AD DS, protokolům a adresáři SYSVOL. Musí být nasazený v Azure datových disků.

> [!NOTE]
> Azure datových disků jsou omezené too1 TB.
> 
> 

Ve výchozím nastavení provádět dat diskových jednotek není mezipaměti zápisy. Data diskové jednotky, které jsou připojené tooa virtuálních počítačů pomocí zápisu prostřednictvím ukládání do mezipaměti. Zápis prostřednictvím mezipaměti díky opravdu hello zápisu je potvrzená toodurable úložiště Azure před hello transakce je dokončena z hlediska hello operačního systému hello Virtuálního počítače. Poskytuje odolnost na náklady hello nepatrně pomalejší zápisů.

To je důležité pro Windows Server AD DS, protože zápis disku ukládání do mezipaměti by způsobila neplatnost předpoklady hello řadiče domény. Windows Server AD DS pokus toodisable ukládání do mezipaměti, ale je funkční toohello diskové vstupně-výstupní operace systému toohonor ji. Selhání toodisable ukládání do mezipaměti, za určitých okolností zavést vrácení hodnot USN, které jsou výsledkem přetrvávání odstraněných objektů a jiné problémy.

Jako osvědčený postup pro virtuálních řadičů domény hello následující:

* Nastavení hello předvoleb mezipaměti hostitele hello Azure datový disk pro žádnou. To brání problémy s ukládání do mezipaměti pro operace služby AD DS.
* Uložit hello databáze, protokolům a adresáři SYSVOL na hello buď stejná data na disku nebo oddělení datových disků. Obvykle se jedná o samostatný disk z disku hello používá pro operační systém hello sám sebe. klíče takeaway Hello je hello databázi služby Windows Server AD DS a adresáře SYSVOL nesmí být uložené na typ disku operačního systému Azure. Ve výchozím nastavení hello proces instalace služby AD DS nainstaluje tyto komponenty v % SystemRoot %, což není doporučeno pro Azure.

### <a name="BKMK_BUR"></a>Zálohování a obnovení
Uvědomte si, co je a není podporována pro zálohování a obnovení řadiče domény obecně, a přesněji řečeno, soubory, které jsou spuštěna ve virtuálním počítači. V tématu [zálohování a obnovení aspekty virtualizovaných řadičů domény](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Vytvořte zálohování stavu systému pomocí pouze zálohovací software, který má konkrétně informace o zálohování požadavky pro Windows Server AD DS, jako je zálohování serveru.

Zkopírujte nebo klonovat soubory virtuálního pevného disku řadičů domény s namísto provádění pravidelných záloh. Obnovení někdy měli požadováno, v tom pomocí klonovaného nebo zkopírovaný virtuální pevné disky bez systému Windows Server 2012 a podporovaném hypervisoru zavedete bublinách USN.

### <a name="BKMK_FedSrvConfig"></a>Konfigurace federačního serveru
Konfigurace Hello federačních serverů služby AD FS Windows serveru (STSs) závisí částečně na tom, jestli hello aplikace, které chcete toodeploy v Azure potřebují tooaccess prostředkům v místní síti.

Pokud aplikace hello splňovat následující kritéria hello, můžete nasadit aplikace hello v izolaci z vaší místní sítě.

* Přijmou tokenů zabezpečení SAML
* Jsou exposable toohello Internetu
* Není přístupu k prostředkům na místě

V takovém případě nakonfigurujte Windows Server AD FS STSs následujícím způsobem:

1. Konfigurace doménové struktury izolované jednou doménou v Azure.
2. Zajištění doménové struktury toohello federovaný přístup pomocí konfigurace farmy federačních serverů systému Windows Server AD FS.
3. Konfigurace služby systému Windows Server AD FS (farmy federačních serverů a farmu proxy federačních serverů) v hello místní doménové struktuře.
4. Vytvořte vztah důvěryhodnosti federace mezi hello na místě a Azure instance služby AD FS Windows serveru.

Na hello druhé straně, pokud hello aplikace vyžadují přístup k prostředkům tooon místní, můžete nakonfigurovat Windows Server AD FS hello aplikaci v Azure následujícím způsobem:

1. Nakonfigurujte připojení mezi místní sítí a Azure.
2. Konfigurace farmy federačních serverů služby Windows Server AD FS v hello místní doménové struktuře.
3. Konfigurace farmy proxy federačních serverů služby AD FS Windows serveru na Azure.

Tato konfigurace je výhod hello snižuje hello úniku místních prostředků, podobně jako tooconfiguring systému Windows Server AD FS s aplikacemi v hraniční síti.

Všimněte si, v obou těchto případech můžete vytvořit vztahy důvěryhodnosti s více poskytovatelů identit, pokud je potřeba business-to-business spolupráce.

### <a name="BKMK_CloudSvcConfig"></a>Konfigurace cloudových služeb
Cloudové služby jsou nezbytné, pokud chcete tooexpose virtuální počítač, přímo toohello Internet nebo tooexpose internetové vyrovnáváním zatížení aplikace. To je možné, protože jednotlivých cloudových služeb nabízí jediné konfigurovat virtuální IP adresy.

### <a name="BKMK_FedReqVIPDIP"></a>Požadavky na federační server pro veřejné a privátní IP adres (dynamické IP adresy a virtuální IP adresy)
Každý virtuální počítač Azure obdrží dynamickou IP adresu. Dynamickou IP adresu je privátní adresa přístupné pouze v rámci Azure. Ve většině případů však bude nutné tooconfigure virtuální IP adresu pro vaše nasazení služby AD FS Windows serveru. virtuální IP adresy Hello je nezbytné tooexpose toohello koncových bodů služby AD FS Windows serveru Internet a se použije pro ověřování a průběžnou správu federovanými partnery a klienty. Virtuální IP adresa je vlastnost cloudové služby, který obsahuje jeden nebo více virtuálních počítačích Azure. Pokud hello deklaracemi identity aplikace nasazené v Azure a Windows Server AD FS internetového a sdílenou složku běžné porty, každá bude vyžadovat své vlastní virtuální IP adresy a proto je nutné toocreate jeden cloudové služby pro aplikaci hello a druhý pro Windows Server AD FS.

Definice hello podmínky virtuální IP adresy a dynamickou IP adresu, najdete v části [termíny a definice](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Konfigurace vysoké dostupnosti systému Windows Server AD FS
I když je možné toodeploy samostatné služby Windows Server AD FS federační služby, je doporučeno toodeploy farmy se aspoň dva uzly pro tokenů zabezpečení AD FS a proxy servery pro provozní prostředí.

Najdete v části [aspekty topologie služby AD FS 2.0 nasazení](https://technet.microsoft.com/library/gg982489) v hello [AD FS 2.0 návrhu průvodce](https://technet.microsoft.com/library/dd807036) toodecide, jakou konfiguraci nasazení možnosti nejlépe vyhovovaly potřebám vaší konkrétní.

> [!NOTE]
> Ve službě Vyrovnávání zatížení tooget pořadí pro Windows Server AD FS koncových bodů v Azure, a nakonfigurovat všechny členy farmy hello systému Windows Server AD FS v hello stejné cloudové služby a použít hello Vyrovnávání zatížení funkce Azure pro protokol HTTP (s výchozí 80) a porty protokolu HTTPS (výchozí hodnota 443). Další informace najdete v tématu [Azure pro vyrovnávání zatížení testu](https://msdn.microsoft.com/library/azure/jj151530).
> Windows Server Vyrovnávání zatížení sítě (NLB) není podporována v Azure.
> 
> 

