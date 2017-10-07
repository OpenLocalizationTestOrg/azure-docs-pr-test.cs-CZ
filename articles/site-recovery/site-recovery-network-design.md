---
title: "aaaDesigning infrastruktury sítě pro zotavení po havárii | Microsoft Docs"
description: "Tento článek popisuje aspekty návrhu sítě Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pratshar
ms.openlocfilehash: 18bafa1b8b334192bfaf2f4aeba9f090011041ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>Navrhování sítě pro zotavení po havárii

Tento článek je řízené tooIT profesionály, kteří jsou zodpovědní za architektury, implementaci a podpora kontinuity podnikových procesů a po havárii (BCDR) pro obnovení infrastruktury, a kteří chtějí toosupport tooleverage Microsoft Azure Site Recovery (ASR) a Vylepšete své BCDR služby. Tento článek popisuje praktické informace na strukturování sítě v lokalitě pro obnovení po havárii, je jej Azure nebo jiný místní lokality. 

## <a name="overview"></a>Přehled
[Azure Site Recovery (ASR)](https://azure.microsoft.com/services/site-recovery/) je služba Microsoft Azure, která orchestruje hello ochrany a obnovení virtualizovaných aplikací pro účely obnovení (BCDR) po havárii kontinuity podnikání. Tento dokument je určený tooguide hello čtečky prostřednictvím hello proces návrhu sítě hello zaměřením na architektury rozsahy IP adres a podsítí v lokalitě obnovení po havárii hello, při replikaci virtuálních počítačů (VM) pomocí Site Recovery.

Kromě toho tento článek ukazuje, jak Site Recovery umožňuje architektury a implementace služeb ve více lokalitách virtuální datacenter toosupport BCDR v době testu nebo po havárii.

Ve světě, kde každý očekává 24 hodin denně 7 připojení, je důležitější než někdy tookeep vaši infrastrukturu a aplikace fungovaly. účelem Hello kontinuity podnikových procesů a zotavení po havárii (BCDR) je součástí toorestore se nezdařilo, takže hello organizace můžete rychle obnovit normální činnost. Vývoj toodeal strategie zotavení po havárii s událostmi pravděpodobně, zhoubné je velmi náročná. To je z důvodu toohello vyplývajících obtížné předpovídat budoucí hello, zejména ve vztahu tooimprobable události a hello vysoké náklady tooprovide odpovídající míry ochrany proti dalekosáhlou catastrophes.

Zásadní pro plánování BCDR, cíl doba obnovení (RTO) a cíl bodu obnovení (RPO) musí být definovaný jako součást plánu zotavení po havárii. Při havárii narazilo hello zákazníka datového centra, pomocí Azure Site Recovery, mohou zákazníci rychle (nízkou RTO) převedení do online režimu replikované virtuální počítače umístěné v hello sekundárního datového centra nebo Microsoft Azure s minimální únikem (nízké RPO).

Převzetí služeb při selhání je možné díky automatické obnovení systému, který původně zkopíruje určené virtuální počítače z hello primární datové centrum toohello sekundárního datového centra nebo tooAzure (v závislosti na scénáři hello) a pak se pravidelně aktualizuje hello repliky. Při plánování infrastruktury, by měly být považovány návrh sítě potenciální potíže, které by mohly bránit vám v schůzku společnosti RTO a plánovaný bod obnovení cíle.  

Když správci plánování toodeploy řešení zotavení po havárii, je mezi klíčové otázky hello dojmů, jak hello virtuální počítač bude dostupné po dokončení převzetí služeb při selhání hello. Automatické obnovení systému umožňuje hello správce toochoose hello sítě toowhich virtuální počítač by bylo připojené tooafter převzetí služeb při selhání. Pokud je primární lokalita hello Azure, nebo je místní web spravován serverem VMM, potom toho dosáhnete pomocí mapování sítě. Další informace o [mapování sítě v Azure tooAzure zotavení po Havárii](site-recovery-network-mapping-azure-to-azure.md) a [mapování sítě z nástroje VMM](site-recovery-network-mapping.md)


Při navrhování sítě hello hello obnovení lokality, hello správce má dvě možnosti:

* Použijte jiný rozsah IP adres pro síť hello v lokalitě pro obnovení. V tomto scénáři hello virtuální počítač po převzetí služeb při selhání se získat novou adresu IP a hello Správce by měl toodo aktualizace služby DNS. 
* Pomocí stejného rozsahu IP adres pro síť hello v lokalitě obnovení hello. V některých scénářích správci upřednostňovat tooretain hello IP adresy, které mají v primární lokalitě hello i po převzetí služeb při selhání hello. V případě normální správce být tooupdate hello trasy tooindicate hello nové umístění hello IP adresy. Ale v hello scénáři, kde je nasazená podsíť roztažené mezi hello primární a hello obnovení lokality, ponechá hello IP adresy pro virtuální počítače hello stane atraktivní možnosti. Roztažení podsíť z místní síti tooan síť Azure nebo mezi dvěma sítěmi Azure není možné.  

Když správci plánování toodeploy řešení zotavení po havárii, jedním z hello klíčové otázky na jejich paměti je jak hello aplikace budou dostupné po dokončení převzetí služeb při selhání hello. Moderní aplikace jsou téměř vždy závislá na sítě toosome stupně, takže fyzicky přesun služby z jedné lokality tooanother představuje síťové výzvu. Existují dva hlavní způsoby, které se tento problém řeší v řešení zotavení po havárii. prvním přístupem Hello je toomaintain pevné IP adresy. I když hello služby přesouvání a hello hostitelské servery, které se v různých fyzických lokacích trvat aplikace hello konfiguraci IP adresy s nimi toohello nové umístění. druhý přístup Hello zahrnuje úplně změnu hello IP adresa během hello přechod do hello obnovené lokalitě. Každý přístup má více změn implementace, které jsou shrnuté níž.

Při navrhování sítě hello hello obnovení lokality, hello správce má dvě možnosti:

## <a name="option-1-retain-ip-addresses"></a>Možnost 1: Zachovat IP adresy
Z hlediska proces obnovení po havárii pomocí pevné IP adresy zobrazí toobe hello nejjednodušší metoda tooimplement, ale má několik potenciální problémy, které v praxi hello alespoň oblíbených přístup. Azure Site Recovery poskytuje možnost hello tooretain hello IP adresy ve všech scénářích. Předtím, než jeden rozhodne tooretain IP, příslušné měla být věnována toohello omezení, které ukládá na možnostech hello převzetí služeb při selhání. Dejte nám se podívejte na hello faktory, které vám mohou pomoci toomake, rozhodnutí tooretain IP adresy, nebo ne. Toho lze dosáhnout dvěma způsoby pomocí roztažené podsíť nebo převzetím úplné podsítě.

### <a name="stretched-subnet"></a>Roztažené podsítě
Zde hello podsíť je k dispozici současně v primární a zotavení po Havárii umístění. Jednoduše řečeno, to znamená můžete přesunout serveru a jeho IP (vrstvy 3) konfigurace toohello druhý web a hello sítě bude směrovat hello provoz toohello nové umístění automaticky. Toto je trivial toodeal s z hlediska serveru, ale má určité problémy:

* Z hlediska vrstvy 2 (Datová vrstva odkaz) bude vyžadovat síťová zařízení, která můžete spravovat roztažené sítě VLAN, ale to se staly menší problém, jako je nyní k dispozici. druhý a obtížnější problém Hello je, že pomocí roztažení hello doména selhání potenciální hello sítě VLAN je rozšířená tooboth lokalit, v podstatě stane jediným bodem selhání. To je nepravděpodobné situaci, došlo, všesměrového vysílání storm se spustil, ale nebylo možné izolované. Našli smíšený názory týkající se těchto potíží poslední jsme viděli mnoho implementací úspěšné, stejně jako "se nikdy implementaci tuto technologii zde".
* Roztažené podsíť není možné, pokud používáte Microsoft Azure jako hello lokality zotavení po Havárii.

### <a name="subnet-failover"></a>Podsíť převzetí služeb při selhání
Je možné tooimplement podsíť převzetí služeb při selhání tooobtain hello výhody řešení podsíť hello roztažen tak popsané výše bez roztažení hello podsítě ve více lokalitách. Zde jakékoli dané podsíti by nacházet v lokalitě 1 nebo 2 lokality, ale nikdy v obou lokalitách současně. V pořadí toomaintain hello adresní prostor IP adres v hello události převzetí služeb při selhání, je možné tooprogrammatically uspořádat pro hello směrovač infrastruktury toomove hello podsítě z jedné lokality tooanother. Ve scénáři hello převzetí služeb při selhání, způsobem podsítě související přesunutí s hello chráněných virtuálních počítačů. Hello Hlavní nevýhodou toothis přístup je v případě hello selhání máte podsíť hello celou toomove, který může být OK ale může to mít vliv aspekty členitosti hello převzetí služeb při selhání.

Podívejme se na tom, jak fiktivních enterprise s názvem Contoso, je možné tooreplicate jeho umístění pro obnovení virtuálních počítačů tooa při selhání přes celou podsíť hello. Nejprve Podíváme se na tom, jak Contoso je možné toomanage podsítě při replikaci virtuálních počítačů mezi dvěma místními umístěními a pak se budeme zabývat jak podsíť převzetí služeb při selhání funguje, když [Azure se používá jako lokality pro obnovení po havárii hello](#failover-to-azure).

#### <a name="failover-from-on-premises-tooazure"></a>Převzetí služeb při selhání z místní tooAzure 
Azure Site Recovery (ASR) umožňuje Azure toobe použít jako lokality pro obnovení po havárii pro virtuální počítače.  

Podívejme se na scénář, kde fiktivní společnost s názvem společnosti Woodgrove Bank má místní infrastrukturu hostování jejich řádku obchodních aplikací a jejich hostují své mobilní aplikace v Azure. Připojení mezi virtuálními počítači Woodgrove Bank v Azure a místními servery poskytuje site-to-site (S2S) virtuální privátní sítě (VPN) nebo ExpressRoute. S2S VPN umožňuje společnosti Woodgrove Bank virtuální sítě v Azure toobe vidět jako rozšíření společnosti Woodgrove Bank místní sítě. Tato komunikace se aktivuje S2S VPN mezi hraniční společnosti Woodgrove Bank a virtuální síť Azure. Nyní Woodgrove chce toouse automatické obnovení systému tooreplicate jeho úlohy běžící tooanother primární oblasti Azure oblast Azure. Tato možnost splňuje potřeby hello Woodgrove, která chce ekonomické možnost zotavení po Havárii a je možné toostore data ve veřejných cloudových prostředích. Woodgrove má toodeal s aplikacemi a konfigurace, které závisí na pevně IP adresy, proto mají požadavek tooretain IP adresy pro své aplikace po selhání nad tooanother oblasti v Azure.

Woodgrove se rozhodla tooassign IP adresy z IP adres rozsah (172.16.1.0/24, 172.16.2.0/24) tooits prostředků běžící v Azure.

Pro možnost tooreplicate Woodgrove toobe tooAzure jeho virtuální počítače a přitom zachovat hello IP adresy, virtuální síť Azure musí toobe vytvořili. Je nutné rozšíření hello místní sítě tak, aby aplikace můžete převzetí služeb při selhání hello místní lokality tooAzure bezproblémově. Azure umožňuje tooadd site-to-site, jakož i point-to-site VPN připojení toohello virtuálním sítím vytvořeným v Azure. Při nastavování připojení site-to-site, síť Azure vám umožní tooroute provoz toohello místní umístění (Azure místní sítě nazve je) pouze v případě, že hello rozsah IP adres se liší od hello místní rozsah adres IP, protože není Azure Podpora roztažení podsítě.  To znamená, že pokud máte 192.168.1.0/24 podsítě v místě, není možné přidat místní sítě 192.168.1.0/24 v hello síť Azure. Toto je očekávané, protože Azure není známo, že nejsou žádné aktivní virtuální počítače v podsíti hello a této podsíti hello se vytváří jenom pro účely zotavení po Havárii. nesmí být v konfliktu toobe možné toocorrectly směrovat síťový provoz z podsítě hello síť Azure v síti hello a místní sítí hello.

![Před převzetí služeb při selhání podsíť](./media/site-recovery-network-design/network-design7.png)

Před převzetí služeb při selhání

toohelp Woodgrove splnění jejich obchodní požadavky, potřebujeme tooimplement hello následující pracovních postupů:

* Vytvořit další síť, dejte nám volání síti pro obnovení, kde by se vytvořily hello při selhání virtuálního počítače.
* tooensure že hello IP adresu pro virtuální počítač se uchovávají po selhání, přejděte na kartu Konfigurace toohello v části Vlastnosti virtuálního počítače, zadejte hello stejnou IP Adresou, která hello virtuálních počítačů má místní a pak klikněte na Uložit. Když hello virtuálních počítačů při selhání, přiřadí Azure Site Recovery hello zadané IP toohello virtuálního počítače.

![Vlastnosti sítě](./media/site-recovery-network-design/network-design8.png)

Po aktivaci hello převzetí služeb při selhání a vytvoří se v síti pro obnovení s IP Adresou hello potřeby hello hello virtuální počítače, sítě toothis připojení lze navázat pomocí [tooVnet virtuální síť připojení](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). V případě potřeby, tato akce může provádět skriptování.  Jak již bylo zmíněno v předchozí části hello o převzetí podsíť i v případě hello tooAzure převzetí služeb při selhání, směrování by toobe správně změnili tooreflect této 192.168.1.0/24 přesunula teď tooAzure.

![Po převzetí služeb při selhání podsíť](./media/site-recovery-network-design/network-design9.png)

Po převzetí služeb při selhání

Pokud nemáte Azure Network, jak je znázorněno na obrázku hello výše. Můžete vytvořit připojení site toosite vpn mezi 'Primární lokality' a 'Síti pro obnovení' po převzetí služeb při selhání hello.  


#### <a name="failover-tooa-secondary-on-premises-site"></a>Převzetí služeb při selhání tooa sekundární místní lokalitu
Dejte nám podívejte se na scénář kde chceme zachovat IP hello jednotlivých hello virtuálních počítačů a převzetí služeb při selhání hello dokončete podsíť společně. aplikace běžící v podsíti 192.168.1.0/24 má primární lokalita Hello. Když se stane hello převzetí služeb při selhání, všechny hello virtuální počítače, které jsou součástí této podsíti se převzít služby při selhání toohello obnovení lokality a zachovat jejich IP adresy. Bude mít trasy toobe odpovídajícím způsobem upravit tooreflect hello fakt, že všechny virtuální počítače hello patřící toosubnet 192.168.1.0/24 teď přesunutí toohello obnovení lokality.

V hello následující obrázek hello tras mezi primární lokality a lokality obnovení, třetí a primární lokalitou a třetí lokality a lokality obnovení bude mít toobe odpovídajícím způsobem upravit.

Následující obrázky Hello ukazuje hello podsítě před hello převzetí služeb při selhání. Podsíť 192.168.0.1/24 aktivní na hello primární lokalitě před hello převzetí služeb při selhání a stane aktivní z hello obnovení lokality po převzetí služeb při selhání hello

![Před převzetí služeb při selhání](./media/site-recovery-network-design/network-design2.png)

Před převzetí služeb při selhání

Následující obrázek Hello ukazuje sítě a podsítě po převzetí služeb při selhání.

![Po převzetí služeb při selhání](./media/site-recovery-network-design/network-design3.png)

Po převzetí služeb při selhání

V sekundární lokalitě je místní a používáte VMM serveru toomanage následně po povolení ochrany pro konkrétní virtuální počítač, automatické obnovení systému budou přidělit síťových prostředků podle toohello následující pracovní postup:

* Automatické obnovení systému přidělí IP adresu pro každé síťové rozhraní na virtuálním počítači hello z hello fond statických IP adres definované v příslušné síti hello pro každou instanci System Center VMM.
* Pokud správce hello definuje hello stejný fond IP adres pro síť hello v lokalitě obnovení hello jako že hello fondu IP adres z hello by přidělit sítě v primární lokalitě hello při přidělování virtuálního počítače repliky ASR. hello IP adresu toohello hello stejné IP adresa, který hello primárního virtuálního počítače.  Hello IP je vyhrazená v nástroji VMM, ale není nastavena jako IP adresa převzetí služeb při selhání na hostiteli Hyper-v hello. Těsně před hello převzetí služeb při selhání je nastavit IP adresu převzetí služeb při selhání na hostiteli technologie Hyper-v.


Pokud hello stejnou IP Adresou není k dispozici, automatické obnovení systému byste přidělit některé další dostupnou IP adresu z fondu IP adres definované hello.

Po hello virtuálních počítačů je povoleno pro ochranu můžete použít následující ukázka skriptu tooverify hello IP, který byl přidělen toohello virtuálního počítače. Hello stejnou IP Adresou by být nastaven jako IP převzetí služeb při selhání a přiřazení toohello virtuálních počítačů během hello převzetí služeb při selhání:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> V hello scénáři, kde virtuální počítače používat službu DHCP hello správy IP adres je zcela mimo hello kontrolu nad automatické obnovení systému. Správce má tooensure, která může obsluhovat hello DHCP slouží hello IP adresy serverů v lokalitě hello obnovení z hello stejného rozsahu, který hello primární lokality.
>
>



## <a name="option-2-changing-ip-addresses"></a>Možnost 2: Při změně IP adres
Tento přístup zdá se, že toobe hello současných převažujících podle co jsme viděli. Trvá formu hello změna hello IP adresu každý virtuální počítač, který je součástí hello převzetí služeb při selhání. Z nevýhod tohoto přístupu vyžaduje hello příchozí síťové too'learn, který byl v IPx aplikace hello je nyní v IPy. I když IPx a IPy jsou logické názvy, záznamy DNS obvykle mají toobe změnit nebo vyprázdněných v celé síti hello a položky v mezipaměti v tabulkách sítě mají toobe aktualizovat ani vyprázdněny, proto s prodlevou může se zobrazit v závislosti na tom, jak je infrastruktura DNS hello nastavená. Tyto problémy lze zmírnit pomocí nízké hodnoty TTL v případě hello aplikací intranetu a použitím [Manager provoz Azure s automatické obnovení systému](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) pro internet aplikace založené na

### <a name="changing-hello-ip-addresses---illustration"></a>Změna IP adresy hello - obrázku
Dejte nám podívejte se na scénář hello kde plánujete toouse různých IP adres napříč hello primární a hello obnovení lokality. V následující ukázka hello máme také třetí lokality, ze kterých můžete přístup k hello aplikace hostované na primární server nebo obnovení lokality.

![Různé IP - před převzetí služeb při selhání](./media/site-recovery-network-design/network-design10.png)


Hello obrázku výše jsou některé aplikace hostované v podsíti 192.168.1.0/24 podsítí v primární lokalitě hello a nejsou nakonfigurované toocome až hello obnovení lokality v podsíti 172.16.1.0/24 po převzetí služeb při selhání. Trasy připojení nebo síť VPN byla správně nakonfigurována, aby všechny tři servery navzájem přístup.

Jako hello obrázek níže znázorňuje při přechodu jednoho nebo více aplikací bude možné obnovit v podsíti obnovení hello. V takovém případě jsme nejsou omezené toofailover hello celou podsíť v hello stejnou dobu. Žádné změny jsou požadované tooreconfigure VPN nebo síťové trasy. Převzetí služeb při selhání a některé aktualizace DNS se ujistěte se, aby aplikace zůstaly dostupné. Pokud je nakonfigurované tooallow dynamické aktualizace hello DNS by zaregistrovat hello virtuální počítače sami pomocí nových IP hello po spuštění po převzetí služeb.

![Různé IP - po převzetí služeb při selhání](./media/site-recovery-network-design/network-design11.png)


Po převzetí služeb při selhání hello repliky virtuální počítač může mít IP adresu, která není hello stejné jako IP adresa hello hello primárního virtuálního počítače. Virtuální počítače zaktualizuje hello server DNS, který používají po spuštění. Záznamy DNS obvykle toobe změnit nebo vyprázdněných v celé síti hello a položky v mezipaměti v tabulkách sítě toobe aktualizovat ani vyprázdněny, takže není neobvyklé toobe potýkají s výpadky při provádění těchto změn stavu. Tento problém můžete zmírnit:

* Použití nízké hodnoty TTL pro aplikace v síti intranet.
* Pomocí Azure provoz Manager automatické obnovení systému pro internet na základě aplikací.
* Pomocí následující hello skript v rámci vaší obnovení plánování tooupdate hello DNS Server tooensure včasné aktualizace (hello skriptu Pokud není potřeba hello registraci dynamického DNS je nakonfigurován)

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-hello-ip-addresses--dr-tooazure"></a>Změna hello IP adresy – tooAzure zotavení po Havárii
Hello [nastavení infrastruktury sítě pro Microsoft Azure jako web pro zotavení po havárii](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) příspěvek blogu vysvětluje, jak toosetup hello požadované síťové infrastruktury Azure při zachování IP adresy není požadavkem. Začíná na Azure a pak končí s postupy popisující hello aplikace a pak podívejte se na jak toosetup sítě místní a toodo testovací převzetí služeb a plánované převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
[Další informace](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) jak Site Recovery mapuje zdrojové a cílové sítě, když se VMM server použít toomanage hello primární lokality.
