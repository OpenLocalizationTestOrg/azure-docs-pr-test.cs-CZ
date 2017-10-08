---
title: "aaaOverview a architektura SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Přehled architektury jak tooDeploy SAP HANA v Azure (velké instance)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e3ee6864af37ac322635eaef43e3c20101e3a769
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Přehled SAP HANA (velké instance) a architektura v Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Co je SAP HANA v Azure (velké instance)?

SAP HANA v Azure (velké Instance) je tooAzure jedinečné řešení. Kromě toho tooproviding, které nabízí Azure Virtual Machines hello za účelem nasazení a spuštění SAP HANA Azure hello možnost toorun a nasazení SAP HANA na úplné obnovení serverů, které jsou vyhrazené tooyou jako zákazník. Hello SAP HANA v Azure (velké instance) řešení je založena na nesdílené serveru nebo úplné obnovení hardware, který je přiřazen tooyou jako zákazník. hardware serveru Hello je součástí větší razítek, které obsahují výpočty a serveru, sítě a infrastruktury úložiště. Který, jako kombinace je HANA TDI certifikaci. nabídku služby Hello SAP HANA v Azure (velké instance) nabízí různé identifikátory SKU jiný server nebo velikosti od jednotky, které mají 72 procesory a toounits 768 GB paměti, které mají 960 procesorů a 20 TB paměti.

izolace zákazníka Hello v rámci infrastruktury razítko hello se provádí v klientech, které podrobně vypadá takto:

- Sítě: Izolace zákazníků v rámci infrastruktury zásobníku prostřednictvím virtuální sítě na zákazníka přiřadit klienta. Klient je přiřazen tooa jednoho zákazníka. Zákazník může mít několik klientů. izolace sítě Hello klientů, které zakazuje síťovou komunikaci mezi klienty v úrovni razítka infrastruktury hello. I když klienti patří toohello tentýž zákazník.
- Součástí úložišť: tooit přiřazené izolaci prostřednictvím úložiště virtuálních počítačů, které mají svazky úložiště. Tooone úložiště virtuálního počítače pouze lze přiřadit svazky úložiště. Úložiště virtuálního počítače je přiřazen výhradně tooone jednoho klienta v zásobníku certifikovaných infrastruktury SAP HANA TDI hello. V důsledku je přístupná v jedné konkrétní a související klientovi pouze svazky úložiště přiřazené tooa úložiště virtuálního počítače. A nejsou viditelné mezi různými klienty nasazené hello.
- Server nebo hostitele: jednotka serveru nebo hostitele není sdílena mezi zákazníky nebo klientů. Server nebo hostitele nasazené tooa zákazníka, je atomic úplné obnovení výpočetní jednotka, která je přiřazena tooone jednoho klienta. **Ne** se používá vytváření oddílů hardwarové nebo softwarové dělení vést ke, jako zákazník, sdílení hostitel nebo server s jinou zákazníka. Úložné svazky, které jsou přiřazeny toohello úložiště virtuálního počítače hello konkrétní klienta jsou připojené toosuch serveru. Klient může mít jeden jednotky toomany serveru různé identifikátory SKU výhradně přiřazen.
- V rámci SAP HANA s časovým razítkem infrastrukturu Azure (velké Instance) jsou mnoha různými klienty nasazení a izolované proti sobě prostřednictvím hello klienta koncepty na úrovni sítě, úložiště a výpočty. 


Tyto jednotky úplné obnovení serveru jsou podporované toorun SAP HANA pouze. vrstvy aplikace SAP Hello nebo zatížení střední VMware běží ve virtuálních počítačích Microsoft Azure. razítka infrastruktury Hello hello SAP HANA systémem Azure (velké Instance) jednotky jsou připojené toohello Azure Network páteřním, tak, které je k dispozici s nízkou latencí připojení mezi SAP HANA na jednotkách Azure (velké Instance) a virtuální počítače Azure.

Tento dokument je jeden z pěti dokumenty, které pokrývají hello tématem SAP HANA v Azure (velké Instance). V tomto dokumentu jsme přejděte prostřednictvím hello základní architekturu a odpovědnosti, služeb a hlavní prostřednictvím funkce hello řešení. Pro většinu hello oblastech, jako je vytváření sítí a připojení hello jiné čtyři dokumenty nepřekrývají podrobnosti a přejít k podrobnostem seznamy. Hello dokumentaci SAP HANA v Azure (velké Instance) nezahrnuje aspekty SAP NetWeaver instalaci nebo nasazení SAP NetWeaver ve virtuálních počítačích Azure. Toto téma je zahrnuté v samostatné dokumentaci najít v hello stejné dokumentace kontejneru. 


Hello pět částí Tato příručka popisuje hello následující témata:

- [Přehled SAP HANA (velké Instance) a architektura v Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastruktura SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Jak tooinstall a konfigurace SAP HANA (velké instance) na Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (velké instance) vysokou dostupnost a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Poradce při potížích s SAP HANA (velké instance) a sledování v Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="definitions"></a>Definice

Několik společné definice se často používá v hello architektury a technické příručce nasazení. Poznámka: hello následující podmínky a jejich významů:

- **IaaS:** infrastruktury jako služby
- **PaaS:** platforma jako služba
- **SaaS:** Software jako služba
- **Součást SAP:** jednotlivých aplikací SAP, jako je například ECC, BW, správce řešení nebo v podnikovém portálu. SAP součástí může být založen na tradičních technologií ABAP nebo Java nebo jiných NetWeaver na základě aplikaci, například obchodních objektů.
- **Prostředí SAP:** jeden nebo více součástí SAP logicky seskupeny tooperform obchodní funkci, jako je třeba vývoje, QAS, školení, zotavení po Havárii nebo výroby.
- **SAP na šířku:** odkazuje toohello celý SAP prostředků vaší IT na šířku. Hello SAP šířku zahrnuje všechny produkční a mimo provozní prostředí.
- **Systém SAP:** hello kombinace databázového systému a vrstva aplikace SAP ERP vývojový systém, SAP BW testovací systém, produkční systému SAP CRM, atd. Azure nasazení nepodporují dělení tyto dvě vrstvy mezi místními a Azure. Znamená systému SAP je buď nasadit místně, nebo je nasazené v Azure. Můžete však nasadit hello různých systémech šířku SAP do Azure nebo místní. Například můžete nasadit systémy vývoj a testování SAP CRM hello v Azure, při nasazování hello SAP CRM produkční systému místně. Pro SAP HANA v Azure (velké instance) je určena hostitele hello SAP aplikační vrstvu SAP systémů ve virtuálních počítačích Azure a související SAP HANA instanci na jednotce v razítku velké Instance HANA hello hello.
- **Velké Instance razítka:** zásobníku infrastruktury hardwaru, který je SAP HANA TDI certifikaci a vyhrazené instance SAP HANA toorun v rámci Azure.
- **SAP HANA v Azure (velké instance):** oficiální název hello nabídka v Azure toorun HANA instance v na SAP HANA TDI certifikaci hardwaru, která je nasazena v velké Instance razítka v různých oblastech Azure. Hello související termín **HANA velké Instance** je zkratka pro SAP HANA v Azure (velké instance) a je široce používá tato příručka technického nasazení.
- **Mezi různými místy:** popisuje scénář, kde jsou virtuální počítače nasazené tooan předplatné Azure, který má site-to-site, více lokalit nebo připojením ExpressRoute mezi místní hello datových centrech a Azure. Dokumentace k společné Azure, tyto typy nasazení jsou také popsány jako mezi různými místy scénáře. Hello důvod pro připojení hello je tooextend místními doménami, místní Active Directory nebo OpenLDAP a místní DNS do Azure. Hello místně na šířku je rozšířené toohello Azure prostředky hello předplatná Azure. S touto příponou, hello virtuálních počítačů může být součástí hello místní domény. Uživatelé domény hello místní domény může přistupovat k serverům hello a službu lze spouštět na ty virtuální počítače (např. služby databázového systému). Komunikace a název rozlišení mezi virtuální počítače nasazené na místě a nasazené virtuální počítače Azure je možné. Například je hello Typický scénář, ve které většina SAP jsou nasazené prostředky. V tématu příručky hello z [plánování a návrhu pro bránu VPN](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [vytvoření virtuální sítě s připojením Site-to-Site pomocí portálu Azure hello](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnější informace.
- **Klienta:** zákazníka nasazené v HANA velké instance razítka získá izolované do "klienta." Klient je izolovaná hello sítí, úložiště a výpočetní vrstvy od ostatních klientů. Tak že úložiště a výpočetní jednotky různými klienty přiřazené toohello nelze vidět, nebo vzájemně komunikovat v hello HANA velké Instance razítka úroveň. Zákazník můžete zvolit toohave nasazení do jiných klientů. Dokonce i pak není žádná komunikace mezi klienty na hello úrovni HANA velké Instance razítka.

Existují různé další prostředky, které byly publikovány na hello tématu nasazení SAP zatížení na veřejném cloudu Microsoft Azure. Důrazně doporučujeme, aby každý, kdo plánování a provádění nasazení SAP HANA v Azure je zkušeného a s ohledem na objekty zabezpečení hello Azure IaaS a hello nasazení SAP zatížení v Azure IaaS. Hello následující zdroje obsahují další informace a by měla odkazovat než budete pokračovat:


- [Použití SAP řešení na virtuálních počítačích Microsoft Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Certifikace

Kromě hello certifikační NetWeaver, SAP vyžaduje speciální certifikační pro SAP HANA toosupport SAP HANA na určité infrastruktuře, jako je například Azure IaaS.

Jádro Hello Poznámka SAP NetWeaver a tooa stupeň SAP HANA certifikační [SAP Poznámka #1928533 – SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533).

To [SAP Poznámka #2316233 - SAP HANA v Microsoft Azure (velké instance)](https://launchpad.support.sap.com/#/notes/2316233/E) je také důležité. Vysvětluje řešení hello popsané v tomto průvodci. Kromě toho jsou podporované toorun SAP HANA v hello typu GS5 virtuálních počítačů Azure. [Informace o tomto případě je publikována na webu SAP hello](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

Hello SAP HANA na řešení Azure (velké instance) označuje tooin &#2316233; Poznámka SAP poskytuje Microsoft a SAP zákazníkům hello možnost toodeploy velké SAP Business Suite SAP Business Warehouse (BW), S/4 HANA BW/4HANA nebo jiných SAP HANA úlohy v Azure. Hello řešení je založena na hello SAP HANA-certifikované razítko vyhrazený hardware ([SAP HANA přizpůsobit Datacenter integrace – TDI](https://scn.sap.com/docs/DOC-63140)). Spuštěna jako SAP HANA TDI nakonfigurované řešení poskytuje s jistotou hello vědět, že všechny aplikace na základě SAP HANA (včetně SAP Business Suite na SAP HANA, SAP Business Warehouse (BW) na SAP HANA, S4/HANA a BW4/HANA) budou toowork na hello hardware infrastruktury.

Porovnání toorunning SAP HANA v Azure Virtual Machines toto řešení obsahuje výhody – poskytuje pro mnohem větší objemy paměti. Pokud chcete tooenable toto řešení, jsou některé klíčové aspekty toounderstand:

- Hello SAP aplikační vrstvu a aplikací bez SAP spustit v Azure virtuální počítače (VM hostovaným ve hello obvykle Azure hardwaru razítka).
- Zákazník místní infrastrukturu, datových center a nasazení aplikací jsou Cloudová platforma Microsoft Azure toohello připojené prostřednictvím ExpressRoute Azure (doporučeno) nebo virtuální privátní sítě (VPN). Active Directory (AD) a DNS jsou také rozšířit do Azure.
- instance databáze SAP HANA Hello pro pracovní vytížení HANA běží na SAP HANA v Azure (velké instance). Hello velké Instance razítka je připojený do sítě Azure, takže software na virtuálních počítačích Azure mohou komunikovat s instancí HANA hello spuštěnou v HANA velké instancí.
- Hardware SAP HANA v Azure (velké instance) je vyhrazený hardware, které jsou součástí infrastruktury jako služby (IaaS) s SUSE Linux Enterprise Server nebo Red Hat Enterprise Linux předinstalován. Jak pomocí Azure Virtual Machines další aktualizace a údržba toohello operačního systému je vaší povinností.
- Instalaci HANA nebo žádné další součásti potřebné toorun SAP HANA na jednotkách HANA velké instancí je vaší povinností, stejně jako všechny příslušné probíhající operace a správy SAP HANA v Azure.
- Kromě toho zde popsané toohello řešení, můžete nainstalovat další součásti ve vašem předplatném Azure, která se připojuje tooSAP HANA v Azure (velké instance).  Například součásti, které umožňují komunikaci s nebo přímo databázi SAP HANA toohello (jump servery, servery s RDP, SAP HANA Studio SAP Data Services pro SAP BI scénáře, nebo řešení monitorování sítě).
- Jako v Azure nabízí velké instancí HANA podpůrné funkce vysoké dostupnosti a zotavení po havárii.

## <a name="architecture"></a>Architektura

Na hlavní hello SAP HANA na řešení Azure (velké instance) má hello SAP aplikační vrstvu umístěný ve virtuálních počítačích Azure a hello umístěný na hardwaru SAP TDI nakonfigurované v razítku velké Instance v databázové vrstvě hello stejné oblasti Azure, která je připojena tooAzure IaaS.

> [!NOTE]
> Je třeba toodeploy hello SAP aplikační vrstvu v hello stejné oblasti Azure jako vrstva databázového systému SAP hello. Toto pravidlo je dobře zdokumentovaný v publikované informace o SAP zatížení v Azure. 

poskytuje služby konfigurace certifikovaným hardwarem SAP TDI (nevirtuálním, úplného operačního systému, vysoce výkonné server pro databázi SAP HANA hello) a možnost hello a flexibilitu Azure tooscale zprostředkovatele Hello přehled architektury SAP HANA v Azure (velké instance) prostředky pro hello SAP toomeet vrstvy aplikace vašim potřebám.

![Přehled architektury systému SAP HANA v Azure (velké instance)](./media/hana-overview-architecture/image1-architecture.png)

Architektura Hello vidět je rozdělené do tří částí:

- **Práva:** místní infrastruktury s různými aplikacemi v datových centrech koncovým uživatelům přístup k obchodních aplikací (například SAP). V ideálním případě by to místní infrastruktury je připojen tooAzure s Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Centrum:** ukazuje Azure IaaS a v takovém případě použijte virtuální počítače Azure toohost SAP nebo jiné aplikace, které používají SAP HANA jako systém databázového systému. Menší HANA instancí, které poskytují funkce s pamětí hello virtuálních počítačích Azure nasazených ve virtuálních počítačích Azure spolu s jejich aplikační vrstvu. Další informace o [virtuální počítače](https://azure.microsoft.com/services/virtual-machines/).
<br />Azure sítě je použité toogroup SAP systémy společně s jiných aplikací do virtuální sítě Azure (virtuální sítě). Tyto virtuální sítě připojit místní tooon systémy, jakož i tooSAP HANA v Azure (velké instance).
<br />SAP NetWeaver aplikace a databáze, které jsou podporovány toorun v Microsoft Azure, najdete v části [SAP podporu Poznámka #1928533 – SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533). Dokumentaci k nasazení SAP řešení v Azure zkontrolujte:

  -  [Pomocí SAP na virtuálních počítačích s Windows (VM)](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Použití SAP řešení na virtuálních počítačích Microsoft Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Vlevo:** ukazuje hello SAP HANA TDI certifikaci hardwaru v razítku velké instanci Azure hello. Hello velké Instance HANA jednotky jsou připojené toohello virtuálních sítí Azure svého předplatného pomocí hello stejnou technologii jako hello připojení z místně do Azure.

razítko Hello velké Instance Azure, samotné kombinuje hello následující součásti:

- **Výpočty:** servery, které jsou založeny na procesory Intel Xeon E7-8890v3 nebo Intel Xeon E7-8890v4, s hello nezbytné výpočetní schopnosti, které jsou certifikované SAP HANA.
- **Síť:** A unified vysokorychlostní síťové fabric, která propojení hello výpočty, úložiště a LAN součásti.
- **Úložiště:** infrastruktury úložiště, které je přístupné přes jednotné síťových prostředcích infrastruktury. Kapacita úložiště konkrétní je zadaný v závislosti na hello konkrétní SAP HANA v konfiguraci Azure (velké instance) nasazuje (větší kapacitu úložiště je k dispozici v dalších měsíčních nákladů).

V infrastruktuře víceklientské hello hello velké Instance razítka jsou zákazníci nasadit jako izolované klienty. V nasazení hello klienta musíte v rámci Azure registraci tooname předplatné Azure. Tato hello toobe má předplatné Azure hello velké instance HANA přechází toobe účtují proti. Tyto klienty mít relace 1:1 toohello předplatného Azure. Sítě vhodné, že je možné tooaccess Instance jednotku velké HANA nasadit v jednoho klienta v jedné oblasti Azure z jiné sítě Azure Vnet, která patří toodifferent předplatných Azure. I když tyto předplatná Azure potřebovat toobelong toohello stejné Azure registrace. 

Stejně jako u virtuálních počítačích Azure, SAP HANA v Azure (velké instance) je k dispozici v několika oblastmi Azure. V možnosti zotavení po havárii toooffer pořadí můžete zvolit tooopt v. Různé velké Instance razítka v rámci jedné politickým geografické oblasti jsou připojené tooeach jiné. Například HANA velké Instance razítka v oblasti USA – západ a oblasti USA – východ jsou připojené prostřednictvím propojení s vyhrazenou síť hello za účelem zotavení po Havárii replikace. 

Stejně jako, můžete si vybrat mezi různé typy virtuálních počítačů s virtuálními počítači Azure, můžete z různých SKU z HANA velké instancí, které jsou přizpůsobené pro různé zatížení typy SAP HANA. SAP platí poměr soketu tooprocessor paměti pro různou zátěží podle generace procesor Intel hello – čtyři různé typy SKU nabízí:

Od července 2017 je k dispozici v několika konfigurací ve hello Azure oblastech z oblasti USA – západ i USA – východ, Austrálie – východ, Austrálie – jihovýchod, západní Evropa a Severní Evropa SAP HANA v Azure (velké instance):

| Řešení SAP | Procesor | Memory (Paměť) | Úložiště | Dostupnost |
| --- | --- | --- | --- | --- |
| Optimalizovaná pro OLAP: SAP BW BW/4HANA<br /> nebo SAP HANA pro obecné zatížení OLAP | Na Azure S72 SAP HANA<br /> – 2 procesor Intel Xeon® x E7 8890 v3<br /> 36 jader procesoru a 72 procesoru vláken |  768 GB |  3 TB | Dostupné |
| --- | Na Azure S144 SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v3<br /> 72 jader procesoru a 144 procesoru vláken |  1,5 TB |  6 TB | Nenabízí se už |
| --- | Na Azure S192 SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v4<br /> 96 jader procesoru a 192 procesoru vláken |  2.0 TB |  8 TB | Dostupné |
| --- | Na Azure S384 SAP HANA<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 jader procesoru a 384 procesoru vláken |  4.0 TB |  16 TB | Připraveno tooOrder |
| Optimalizovaná pro OLTP: SAP Business Suite<br /> na SAP HANA nebo S nebo 4HANA (OLTP)<br /> Obecné OLTP | Na Azure S72m SAP HANA<br /> – 2 procesor Intel Xeon® x E7 8890 v3<br /> 36 jader procesoru a 72 procesoru vláken |  1,5 TB |  6 TB | Dostupné |
|---| Na Azure S144m SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v3<br /> 72 jader procesoru a 144 procesoru vláken |  3.0 TB |  12 TB | Nenabízí se už |
|---| Na Azure S192m SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v4<br /> 96 jader procesoru a 192 procesoru vláken  |  4.0 TB |  16 TB | Dostupné |
|---| Na Azure S384m SAP HANA<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 jader procesoru a 384 procesoru vláken |  6.0 TB |  18 TB | Připraveno tooOrder |
|---| Na Azure S384xm SAP HANA<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 jader procesoru a 384 procesoru vláken |  8.0 TB |  22 TB |  Připraveno tooOrder |
|---| Na Azure S576 SAP HANA<br /> – Procesor Intel Xeon® x 12 E7 8890 v4<br /> 288 jader procesoru a 576 procesoru vláken |  12.0 TB |  28 TB | Připraveno tooOrder |
|---| Na Azure S768 SAP HANA<br /> – Procesor Intel Xeon® x 16 E7 8890 v4<br /> 384 jader procesoru a 768 procesoru vláken |  16.0 TB |  36 TB | Připraveno tooOrder |
|---| Na Azure S960 SAP HANA<br /> – 20 procesor Intel Xeon® x E7 8890 v4<br /> 480 jader procesoru a 960 procesoru vláken |  20.0 TB |  46 TB | Připraveno tooOrder |

- Jader procesoru = součet jiný-technologie hyper-threaded jader procesoru hello Sum hello procesorů jednotky server hello.
- Vláken procesoru = součet výpočetní vláken poskytované technologie hyper-threaded jader procesoru hello Sum hello procesorů jednotky server hello. Všechny jednotky jsou nakonfigurované ve výchozím nastavení toouse technologie Hyper-Threading.


Hello výše uvedené různé konfigurace, které jsou k dispozici nebo jsou 'nenabízí už' se odkazuje v [SAP podporu Poznámka #2316233 – SAP HANA v Microsoft Azure (velké instance)](https://launchpad.support.sap.com/#/notes/2316233/E). Hello konfigurace, které jsou označeny jako 'Připravené tooOrder' bude brzy najít jejich vstupu v hello Poznámka SAP. Ale tyto instance SKU lze provést řazení již pro hello šesti různých oblastech Azure hello HANA velké Instance služby je k dispozici.

Hello konkrétní konfigurace vybrali jsou závislé na zatížení, prostředky procesoru a požadovanou paměť. Je možné pro OLTP zatížení tooleverage hello SKU, které jsou optimalizované pro úlohy OLAP. 

základní pro všechny nabídky hello Hello hardwaru jsou SAP HANA TDI certifikaci. Ale jsme rozlišit mezi dvěma různými třídami hardware, který rozděluje hello SKU do:

- S72, S72m, S144, S144m, S192 a S192m, které označujeme tooas hello "Typ I třídy" z jednotky SKU.
- S384, S384m, S384xm, S576, S768 a S960, které označujeme tooas hello "Typu II třídou" SKU.

Je důležité toonote, která dokončení HANA velké Instance razítka není přidělena výhradně pro jednoho zákazníka & č. 39; použijte s. Tuto skutečnost platí toohello stojany s výpočetní a úložnou kapacitu připojené prostřednictvím síťových prostředcích infrastruktury také nasazené v Azure. Velké instancí HANA infrastruktury, jako je například Azure, nasadí různých zákazníků &quot;klienty&quot; , které jsou izolované od sebe navzájem v hello následující tři úrovně:

- Sítě: Izolace pomocí virtuální sítě v rámci hello HANA velké Instance razítka.
- Úložiště: Izolaci prostřednictvím úložiště virtuálních počítačů, které mají svazky úložiště přiřadit a izolovat svazky úložiště mezi klienty.
- Výpočetní: Vyhrazené přiřazení serveru jednotky tooa jednoho klienta. Žádné pevné nebo softwarové oddíly jednotky serverů. Bez sdílení jedné jednotky serveru nebo hostitele mezi klienty. 

Nasazení hello velké instancí HANA jednotek mezi různými klienty jako takový, nejsou viditelné tooeach jiné. Ani velké jednotky Instance HANA nasazené v jiných klientů může komunikovat navzájem přímo na úrovni razítko velké Instance HANA hello. Pouze HANA velké Instance jednotky v rámci jednoho klienta může komunikovat tooeach jiné na hello úrovni HANA velké Instance razítka.
Nasazené klienta v hello velké Instance razítka není přiřazen vhodné tooone fakturace předplatného Azure. Ale hello wise sítě, který je přístupný z virtuální sítě Azure z jiných předplatných Azure v rámci stejné Azure registrace. Pokud nasadíte pomocí jiného předplatného Azure v hello stejné oblasti Azure, můžete také zvolit tooask pro klienta oddělené HANA velké Instance.

Existují významné rozdíly mezi systémem SAP HANA HANA velké instancí a SAP HANA běžící na virtuálních počítačích Azure nasazené v Azure:

- Neexistuje žádné vrstva virtualizace pro SAP HANA v Azure (velké instance). Zobrazí výkon hello hello základní holého hardwaru.
- Na rozdíl od Azure je na serveru Azure (velké instance) hello SAP HANA vyhrazené tooa konkrétního zákazníka. Není možné, že jednotky serverů nebo hostitele je pevný nebo konfigurace soft-rozdělena na oddíly. V důsledku toho jednotce HANA velké Instance slouží jako přiřadit jako klient celou tooa a že tooyou jako zákazník. Restartování nebo vypnutí serveru hello automaticky nevede toohello operačního systému a SAP HANA nasazován na jiném serveru. (Pro typ I třídy SKU, hello jedinou výjimkou je, pokud server může dojít k potížím a opětovné nasazení musí toobe provést na jiném serveru.)
- Na rozdíl od Azure, kde jsou vybrané typy procesoru hostitele pro hello nejlepší poměr ceny a výkonu, jsou typy procesoru hello zvolené pro SAP HANA v Azure (velké instance) hello nejvyšší provádění řádku procesor Intel E7v3 a E7v4 hello.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Spuštěním několika instancí SAP HANA na jednu jednotku velké Instance HANA
Je možné toohost více než jeden aktivní instance SAP HANA na jednotkách HANA velké Instance hello. V pořadí toostill poskytují možnosti hello úložiště snímků a zotavení po havárii, taková konfigurace vyžaduje svazku nastavit na jednu instanci. Od nyní hello velké Instance HANA jednotky lze dále rozdělit takto:

- S72, S72m, S144, S192: Spouštění v přírůstcích po 256 GB s 256 GB hello nejmenší jednotky. Různé násobky jako 256 GB, 512 GB a tak dále, může být maximální kombinovaná toohello hello paměti hello jednotky.
- S144m a S192m: V přírůstcích po 256 GB s jednotkou nejmenší hello 512 GB. Různé násobky jako 512 GB, 768 GB a tak dále, může být maximální kombinovaná toohello hello paměti hello jednotky.
- Zadejte třídu II: V přírůstcích po 512 GB s hello nejmenší od jednotky 2 TB. Různé násobky jako 512 GB, 1 TB, 1,5 TB a tak dále, může být maximální kombinovaná toohello hello paměti hello jednotky.

Některé příklady spuštěním několika instancí SAP HANA může vypadat podobně jako:

| Skladová jednotka (SKU) | Velikost paměti | Velikost Storage | Velikosti s více databází |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | Instance HANA 1 × 768 GB<br /> nebo Instance 1 × 512 GB + 1 × 256 GB Instance<br /> nebo instance 3 x 256 GB | 
| S72m | 768 GB | 3 TB | Instance 3x512GB HANA<br />nebo Instance 1 × 512 GB + 1 × 1 TB Instance<br />nebo instance 6 x 256 GB<br />nebo instance 1x1.5 TB | 
| S192m | 4 TB | 16 TB | Instance 8 x 512 GB<br />nebo instance 4 x 1 TB<br />nebo instancí 4 x 512 GB + 2 × 1 TB instancí<br />nebo instancí 4 x 768 GB + instancí 2 x 512 GB<br />nebo Instance 1 × 4 TB |
| S384xm | 8 TB | 22 TB | Instance 4 x 2 TB<br />nebo instancí 2 x 4 TB<br />nebo instancí 2 × 3 TB + 1 × 2 TB instancí<br />nebo instancí 2x2.5 TB + 1 × 3 TB instancí<br />nebo Instance. 1 x 8 TB |


Můžete získat představu hello. Existují také další varianty jistě. 


## <a name="operations-model-and-responsibilities"></a>Operace modelu a odpovědnosti

Služba Hello systému SAP HANA v Azure (velké instance) je zarovnán služby Azure IaaS. Zobrazí instance HANA velké instancí s nainstalovaným operačním systémem, která je optimalizovaná pro SAP HANA. Jak se virtuální počítače Azure IaaS, většina úloh hello posílení hello operačního systému, instalace další software, který budete potřebovat, instalace HANA, operační hello operačního systému a HANA a aktualizaci hello operačního systému a HANA je vaší povinností. Microsoft bez vynucení aktualizace operačního systému, nebo HANA na vás.

![Odpovědnosti SAP HANA v Azure (velké instance)](./media/hana-overview-architecture/image2-responsibilities.png)

Jak můžete vidět v diagramu hello výše, SAP HANA v Azure (velké instance) je víceklientské infrastruktury jako služby nabídky. A v důsledku toho je hello dělení zodpovědnosti v hello OS infrastruktury hranice, hello většina součástí. Microsoft je zodpovědná za všechny aspekty služby hello níže hello řádku hello operačního systému a odpovídáte výše hello řádek, včetně hello operačního systému. Nejaktuálnější místní metody, které může nasazení dodržování předpisů, zabezpečení, správy aplikací, základ a operačního systému správy, můžete pokračovat v toobe použít. systémy Hello se zobrazí jako, pokud jsou ve vaší síti všechny namapoval.

Ale tato služba je optimalizovaná pro SAP HANA, existují oblasti, kde jste s Microsoftem musí toowork společně toouse hello základní možnosti infrastruktury pro dosažení co nejlepších výsledků.

Hello následující seznam obsahuje více podrobností na každém hello vrstev a vaše odpovědnosti:

**Práce v síti,** všechny hello interní sítě pro razítko velké Instance hello systémem SAP HANA toohello jeho přístup k úložišti připojení mezi instancemi hello (pro Škálováním na více systémů a další funkce), na šířku toohello připojení a připojení tooAzure hello SAP aplikační vrstvu je hostitelem v Azure Virtual Machines. Zahrnuje také připojení k síti WAN mezi datovými centry Azure pro zotavení po havárii pro účely replikace. Všechny sítě jsou rozděleny do oddílů hello klienta a provedli QOS.

**Úložiště:** hello virtualizované oddílů úložiště pro všechny svazky, které jsou potřebné hello SAP HANA servery, a také pro snímky. 

**Servery:** hello vyhrazené fyzické servery toorun hello databáze SAP HANA přiřazené tootenants. Hello servery hello typ I třídy SKU jsou abstrahované hardwaru. S těmito typy serverů konfigurace serveru hello shromažďování a uchovávání v profilech, které lze přesunout z jednoho fyzického hardwaru tooanother fyzický hardware. Takové (ruční) přesunu profilu operacemi je možné porovnávat trochu tooAzure služby opravy. Hello servery hello SKU – třída typu II nejsou nabízí takové funkce.

**SDDC:** software pro správu hello, které jsou používané toomanage data centra jako softwarově definované entity. To umožňuje používat Microsoft toopool prostředky pro škálování, dostupnosti a z důvodů výkonu.

**Podporované OS:** hello OS zvolíte (SUSE Linux nebo Red Hat Linux), běží na serverech hello. Image Hello operačního systému, jsou uvedeny jsou hello Image poskytované hello jednotlivých Linux dodavatele tooMicrosoft hello za účelem spuštění SAP HANA. Jste požadované toohave předplatné s dodavatelem Linux hello bitové kopie pro konkrétní optimalizované SAP HANA hello. Vaše odpovědnosti zahrnují registrace hello obrázky s dodavatelem hello operačního systému. Ze hello bod předání společností Microsoft budete také zodpovědná za jakékoli další opravy operačního systému Linux hello. Tato opravy také obsahuje další balíčky, které může být nezbytné pro úspěšnou instalaci SAP HANA (naleznete v dokumentaci k instalaci HANA a SAP poznámky na tooSAP) a které nebyly zahrnuty podle hello konkrétní dodavatele Linux v jejich SAP HANA optimalizované bitové kopie operačního systému. Hello zodpovědností zákazníků hello zahrnuje taky opravy z hello operačního systému, který je související toomalfunction/optimalizace hello operačního systému a jeho ovladače související toohello konkrétní serverový hardware. Nebo žádné zabezpečení nebo funkční opravy hello operačního systému. Odpovědnosti zákazníka je také monitorování a plánování kapacity služby:

- Využití prostředků procesoru
- Paměťové nároky
- Místo na disku svazky související toofree, IOPS a čekací doba
- Svazek provoz sítě mezi HANA velké Instance a SAP aplikační vrstvu

Základní infrastruktura Hello velké instancí HANA poskytuje funkce pro zálohování a obnovení hello svazku operačního systému. Pomocí této funkce je také vaší povinností.

**Middleware:** především hello SAP HANA Instance. Správu, operace a monitorování jsou vaše zodpovědnosti. Není funkce zadaný, díky kterému toouse úložiště snímků pro zálohování a obnovení a zotavení po havárii pro účely. Tyto možnosti jsou poskytovány hello infrastruktury. Vaše odpovědnosti však také zahrnovat navrhování vysoké dostupnosti nebo zotavení po havárii s těmito možnostmi, jejich využití a monitorování úložiště snímků již byly úspěšně provedeny.

**Data:** vaše data spravovaná přes SAP HANA a další data, jako je zálohování sdílí soubory umístěné na svazcích nebo souboru. Vaše odpovědnosti zahrnují monitorování volného místa na disku a správu obsahu hello na svazcích hello a monitorování hello úspěšné provedení zálohy diskové svazky a úložiště snímků. Úspěšné provedení lokalit tooDR data replikace je však hello odpovědnost společnosti Microsoft.

**Aplikace:** hello instancí aplikace SAP nebo v případě aplikace bez SAP, hello aplikační vrstvu těchto aplikací. Vaše odpovědnosti jsou nasazení, operace a správa a monitorování těchto aplikací souvisejících toocapacity plánování využití prostředků procesoru, využití paměti, využívání Azure Storage a spotřeba šířky pásma sítě v rámci Virtuálních sítí Azure a z virtuálních sítí Azure tooSAP HANA v Azure (velké instance).

**WAN:** hello připojení je vytvořit z místních tooAzure nasazení, pro zatížení. Naše zákazníky s instancí velké HANA použijte Azure ExpressRoute pro připojení k síti. Toto připojení není součástí hello SAP HANA na řešení Azure (velké instance), takže jste zodpovědní za hello nastavení pro toto připojení.

**Archiv:** možná budete chtít tooarchive kopií dat pomocí vlastních metod v účtech úložiště. Archivace vyžaduje správu, dodržování předpisů, náklady a operace. Jste zodpovědní za generování archivu kopií a záloh v Azure a uložit je do kompatibilní způsobem.

V tématu hello [SLA pro SAP HANA v Azure (velké instance)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Nastavení velikosti

Nastavení velikosti pro velké instance HANA je nejsou jiné než dimenzování pro HANA obecně. U stávajících a nasadit systémy, který má být toomove z jiných RDBMS tooHANA, SAP poskytuje řadu sestav, které běží na vaše stávající systémy SAP. Pokud je databáze hello přesunutý tooHANA, tyto sestavy zkontrolujte, zda text hello data a výpočet požadavků na paměť pro instanci HANA hello. Následující poznámky k SAP tooget hello přečíst další informace o tom, jak toorun tyto sestavy a jak tooobtain jejich nejnovější opravy nebo verze:

- [Poznámka SAP #1793345 - dimenzování pro sadě SAP HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [Poznámka: #1872170 - Suite na velikosti sestavy HANA a S/4 HANA SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [Poznámka SAP #2121330 – nejčastější dotazy: SAP BW na HANA Změna velikosti sestav](https://launchpad.support.sap.com/#/notes/2121330)
- [Poznámka: #1736976 - sestavu nastavení velikosti pro BW HANA SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Poznámka #2296290 - nová sestava nastavení velikosti pro BW na HANA](https://launchpad.support.sap.com/#/notes/2296290)

Pro implementace zelená pole je rychlé přizpůsobení velikosti symbolů SAP požadavky na paměť k dispozici toocalculate hello implementace SAP softwaru nad HANA.

Požadavky na paměť pro HANA se zvyšuje s růstem datový svazek, tak chcete vědět, využití paměti hello teď toobe a být schopný toopredict je toobe probíhající v hello budoucí. Požadavky na paměť hello podle, poté můžete namapovat vaše vyžádání do jedné z velkých SKU Instance HANA hello.

## <a name="requirements"></a>Požadavky

Tento seznam sestaví požadavky na spuštění SAP HANA v Azure (větší instance).

**Microsoft Azure:**

- Předplatné Azure, který může být propojený tooSAP HANA v Azure (velké instance).
- Kontrakt Microsoft Premier Support. V tématu [SAP podporu Poznámka #2015553 – SAP v Microsoft Azure: podpora požadavky](https://launchpad.support.sap.com/#/notes/2015553) konkrétní informace související s toorunning SAP v Azure. HANA velké instance jednotky pomocí 384 a více procesorů, budete také potřebovat tooextend hello tooinclude smlouvu Premier Support Azure rychlé odpovědi (ARR).
- Sledování hello HANA velké instancí SKU je nutné po provedení velikosti uplatňovat s SAP.

**Připojení k síti:**

- Azure ExpressRoute mezi místní tooAzure: tooconnect místní datacenter tooAzure, ujistěte se, tooorder alespoň 1 GB/s připojení z vašeho poskytovatele internetových služeb. 

**Operační systém:**

- Licence pro SUSE Linux Enterprise Server 12 pro SAP aplikace.

> [!NOTE] 
> Hello dodaných společností Microsoft operačního systému není registrován u SUSE, ani je propojená s instancí SMT.

- SUSE Linux předplatné správy nástroj (SMT) nasazené v Azure na virtuální počítač Azure. Tímto způsobem hello možnost pro SAP HANA na Azure (velké instance) toobe zaregistrován a v uvedeném pořadí aktualizován SUSE (jako je žádný přístup k Internetu v rámci HANA velké instance datového centra). 
- Licence pro Red Hat Enterprise Linux 6.7 nebo 7.2 pro SAP HANA.

> [!NOTE]
> Hello dodaných společností Microsoft operačního systému není registrován u Red Hat, ani je it připojené tooa Red Hat odběr Manager Instance.

- Red Hat odběr Manager nasazené v Azure na virtuální počítač Azure. Hello Red Hat odběr Manager poskytuje možnost hello pro SAP HANA na Azure (velké instance) toobe zaregistrován a v uvedeném pořadí aktualizován Red Hat (jako je žádný přímý přístup k Internetu z v rámci klienta hello nasazené na hello Azure velké Instance razítka).
- SAP vyžaduje toohave podporu smlouvy s poskytovatelem Linux. Tento požadavek není řešením hello faktu HANA velké instance nebo hello vymazat, který vaše spuštění Linux v Azure. Na rozdíl od s některými hello Linux Azure Galerie obrázků, poplatek služby hello není součástí hello řešení nabídka HANA velké instancí. Se nachází na můžete jako zákazník toofulfill hello požadavky SAP o smlouvách, podporu s distributorem Linux hello.   
   - Pro systémy SUSE Linux vyhledat hello požadavky podpory smlouvě v [SAP Poznámka #1984787 - SUSE LINUX Enterprise Server 12: poznámky k instalaci](https://launchpad.support.sap.com/#/notes/1984787) a [&#1056161; Poznámka SAP - SUSE Priority podporu pro aplikace SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux musíte toohave hello správné předplatné úrovně, které zahrnují podporu a služby (aktualizace toohello operační systémy velkých instancí HANA. Red Hat doporučuje získávání předplatné "RHEL pro SAP obchodních aplikací". Týkající se podpory a služeb, zkontrolujte [SAP Poznámka #2002167 - Red Hat Enterprise Linux 7.x: instalace a Upgrade](https://launchpad.support.sap.com/#/notes/2002167) a [SAP Poznámka #1496410 - Red Hat Enterprise Linux 6.x: instalace a Upgrade](https://launchpad.support.sap.com/#/notes/1496410) podrobnosti.

**Databáze:**

- Licence a součásti instalace softwaru pro SAP HANA (platforma nebo enterprise edition).

**Aplikace:**

- Licence a softwarové součásti instalace pro všechny aplikace SAP připojení tooSAP HANA a související smlouvy o podpoře SAP.
- Licence a softwarové součásti od instalace pro všechny aplikace bez SAP používá ve vztahu tooSAP HANA v prostředí Azure (velké instance) a související smlouvy o podpoře.

**Dovedností:**

- Zkušenosti a znalosti o Azure IaaS a jeho komponenty.
- Zkušenosti a znalosti o nasazení SAP zatížení v Azure.
- Instalace SAP HANA certifikované osobní.
- SAP architekti dovednosti toodesign vysokou dostupnost a zotavení po havárii kolem SAP HANA.

**SAP:**

- Očekává se, SAP zákazníky a mají podporu kontrakt s SAP
- Zejména pro implementace na hello typu II třídu HANA velké Instance SKU důrazně doporučujeme tooconsult s SAP ve verzích systému SAP HANA a závěrečné konfigurace na velké velikostí škálování hardwaru.


## <a name="storage"></a>Úložiště

Hello rozložení úložiště pro SAP HANA v Azure (velké instance) se nakonfiguruje prostřednictvím SAP HANA na Azure Service Management prostřednictvím SAP Doporučené pokyny, zdokumentována hello [požadavky na úložiště SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) dokumentu white paper.

Hello HANA velké instancí hello typ I třídy se dodávají s čtyřikrát hello paměti svazek jako svazek úložiště. Pro typ hello II třída velké Instance HANA jednotek není hello úložiště budete toobe čtyřikrát více. jednotky Hello se dodávají s svazek, který je určený pro ukládání HANA zálohování transakčního protokolu. Najít další podrobnosti v [jak tooinstall a konfigurace SAP HANA (velké instance) na Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Viz následující tabulka uvádí přidělení úložiště hello. Hello tabulka uvádí zhruba hello kapacity pro zadaný s různými jednotkami velké Instance HANA hello jiné svazky hello.

| HANA velké Instance SKU | Hana nebo dat | Hana a protokolování | / sdílené Hana | Hana/log/zálohování |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1024 GB | 1536 GB | 1024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12 000 GB | 2050 GB | 2050 GB | 2 040 GB |
| S384xm | 16 000 GB | 2050 GB | 2050 GB | 2 040 GB |
| S576 | 20 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | 28,000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36,000 GB | 4100 GB | 2050 GB | 4100 GB |


Skutečné nasazené svazky se může lišit podle trochu nasazení a nástroj, který je velikosti svazků používaných tooshow hello.

Pokud jste rozdělit HANA velké Instance SKU, bude vypadat několik příkladů možné dělení částí:

| Oddíl paměti v GB | Hana nebo dat | Hana a protokolování | / sdílené Hana | Hana/log/zálohování |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Tyto velikosti jsou čísla hrubý svazku, která mírně podle nasazení se může lišit a nástroje používají toolook v hello svazky. Také existují další oddíl velikosti thinkable, jako je 2,5 TB. Velikosti úložiště se počítá pomocí podobné vzorec jako použité pro oddíly hello výše. Hello termín "oddíly, neznamená, že hello operační systém, paměť nebo prostředky procesoru se žádným způsobem rozdělena na oddíly. Právě označuje oddílů pro úložiště pro různé instance HANA hello můžete chtít toodeploy na jednom jedna Instance HANA velké jednotky. 

Jako zákazník může mít potřebujete další úložiště, je třeba hello možnost tooadd úložiště toopurchase další úložiště v jednotkách 1 TB. Toto dodatečné úložiště se dá přidat jako další svazek nebo může být použité tooextend jeden nebo více hello existující svazky. Není možné toodecrease hello velikosti svazků hello původně nasazení a většinou zdokumentovat, hello tabulek výše. Je také není možné toochange hello názvů hello svazků nebo připojení. svazky úložiště Hello jak bylo popsáno výše jsou připojené toohello velké Instance HANA jednotky jako NFS4 svazky.

Jako zákazník můžete toouse úložiště snímků pro zálohování a obnovení a po havárii pro účely obnovení. Další informace o tomto tématu jsou podrobně popsané na [SAP HANA (velké instance) vysokou dostupnost a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Šifrování neaktivních uložených dat
Hello úložiště použitého pro velké instancí HANA umožňuje transparentní šifrování dat hello, který je uložen na discích hello. V době nasazení HANA velké jednotky Instance máte možnost toohave hello tento druh povolit šifrování. Můžete také zvolit toochange tooencrypted svazky po již hello nasazení. Přesunutí Hello ze svazků tooencrypted bez šifrování je transparentní a nevyžaduje s prodlevou. 

S hello typ I třídy SKU, spouštěcí svazek hello hello, logická jednotka je uložený ve, je zašifrován. V případě hello typu třídy II SKU z HANA velké instancí je nutné tooencrypt hello spouštění logické jednotky s metody operačního systému. 


## <a name="networking"></a>Sítě

Architektura Hello sítí Azure je klíčovou komponentou toosuccessful nasazení SAP aplikací na velké instance HANA. SAP HANA na nasazení Azure (velké instance) obvykle mají větší šířku SAP v několika různých řešeních pro SAP s různou velikost databáze, využití prostředků procesoru a využití paměti. Pravděpodobně ne všechny tyto systémy SAP je založena na SAP HANA tak vaše SAP šířku by pravděpodobně hybridní, který používá:

- Nasazení SAP systémy místně. Z důvodu velikostí tootheir nelze tyto systémy aktuálně hostované v Azure; classic Příkladem může být produkční SAP ERP systému na Microsoft SQL Server (jako hello databáze), který vyžaduje další procesoru nebo paměti prostředky virtuálních počítačích Azure můžete zadat.
- Nasazení na základě SAP HANA SAP systémy místně.
- Nasazené SAP systémy ve virtuálních počítačích Azure. Tyto systémy můžou být vývoj, testování, izolovaného prostoru, nebo produkční instance pro žádné z hello SAP NetWeaver aplikacím, které můžete úspěšně nasazovat v Azure (na virtuálních počítačích), na základě poptávky prostředků využívání a paměti. Tyto systémy také může být na základě databází, jako je SQL Server (najdete v části [SAP podporu Poznámka #1928533 – SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533/E)) nebo SAP HANA (najdete v části [certifikované platformy IaaS aplikace SAP HANA](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Nasazení SAP aplikační servery v Azure (na virtuálních počítačích), které využívají SAP HANA v Azure (velké Instance) v Azure velké Instance razítka.

Při typických hybridní SAP na šířku (se čtyřmi nebo více různé scénáře nasazení) existují mnoho zákazníků případů dokončení SAP šířku běžící v Azure. Jako virtuální počítače Microsoft Azure se stávají výkonnější, se zvyšuje hello počtu zákazníků přesunutí svá řešení SAP do Azure.

Azure sítě v kontextu hello systémů SAP nasazené v Azure není složité. Je založena na hello následující zásady:

- Virtuální sítě Azure (virtuální sítě) potřebovat okruh Azure ExpressRoute toohello toobe připojení, která se připojuje tooon místní sítě.
- Okruh ExpressRoute připojení místní tooAzure obvykle by měl mít šířky pásma s 1 GB/s nebo vyšší. Tato minimální šířku pásma přenosu dat mezi místními systémy a systémy s operačním systémem na virtuálních počítačích Azure (stejně jako systémy tooAzure připojení ze koncoví uživatelé na místě) umožňuje odpovídající šířku pásma.
- Všechny systémy SAP v Azure nutné toobe nastavené v toocommunicate sítě Azure Vnet mezi sebou.
- Active Directory a DNS hostované na místě jsou rozšířené do Azure prostřednictvím ExpressRoute z místní.


> [!NOTE] 
> Z fakturace hlediska jenom jedno předplatné Azure může být propojený jenom tooone jednoho klienta v velké Instance razítka v určité oblasti Azure a naopak jednoho klienta razítko velké Instance může být propojený jenom tooone předplatného Azure. Tuto skutečnost není různých tooany jiné fakturovatelný objekty v Azure

Nasazení SAP HANA v Azure (velké instance) v několika různých oblastech Azure, má za následek toobe samostatné klienta nasazené v hello velké Instance razítka. Však můžete spustit i v rámci hello šířku stejné SAP, pokud tyto instance jsou součástí hello stejného předplatného Azure. 

> [!IMPORTANT] 
> SAP HANA v Azure (velké instance) podporuje pouze nasazení Správa prostředků Azure.

 

### <a name="additional-azure-vnet-information"></a>Další informace o virtuální síť Azure

V pořadí tooconnect tooExpressRoute virtuální síť Azure, je nutné vytvořit bránu Azure (viz [o brány virtuální sítě pro ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Služba Azure gateway lze použít buď pomocí ExpressRoute tooan infrastruktury mimo Azure (nebo tooan Azure velké instance razítka), nebo tooconnect mezi virtuálními sítěmi Azure (najdete v části [konfigurace připojení VNet-to-VNet pro Resource Manager pomocí prostředí PowerShell ](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Tak dlouho, dokud tato připojení pocházejí z různých směrovače MS Enterprise okraje (MSEE) se můžete připojit hello služba Azure gateway tooa maximálně čtyři různá připojení ExpressRoute.  Další informace najdete v tématu [infrastruktury SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Hello propustnost poskytuje služba Azure gateway je jiný pro oba případy použití (viz [o službě VPN Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Hello maximální propustnost, kterou jsme můžete dosáhnout s bránou virtuální sítě je 10 GB/s, pomocí připojení ExpressRoute. Mějte na paměti, že kopírování souborů mezi virtuální počítač Azure umístěný ve službě Azure VNet a systému místní (jako jedna kopie datový proud) není dosahuje hello úplnou propustnost různých SKU brány hello. tooleverage hello dokončení šířky pásma hello brány virtuální sítě, musíte použít víc datových proudů nebo různých kopírování souborů v paralelní datové proudy jednoho souboru.


### <a name="networking-architecture-for-hana-large-instances"></a>Architektura sítě pro velké instance HANA
Hello sítě architektura pro velké instance HANA jak je uvedeno níže, je možné oddělit do čtyř částí:

- Místní sítě a tooAzure připojení ExpressRoute. Tato část je hello zákazníkům domény a tooAzure připojené prostřednictvím ExpressRoute. Toto je část hello v pravé dolní hello hello grafiky níže.
- Azure sítě jako stručně výše popsané s virtuálními sítěmi Azure, který znovu mít brány. Toto je oblast, kde je nutné toofind hello odpovídající návrhy pro vaše požadavky na aplikace, zabezpečení a požadavky na dodržování předpisů. Pomocí HANA velké instancí je jiný bod úvahu z hlediska počtu virtuálních sítí a Azure toochoose SKU brány z. Toto je část hello v pravé horní hello hello grafiky.
- Připojení velké instancí HANA prostřednictvím ExpressRoute technologie do Azure. Tato část nasazení a postará Microsoft. Stačí toodo zákazníka je tooprovide některé rozsahy IP adres a po nasazení hello vaše prostředky v HANA velké instance připojení hello ExpressRoute okruhu toohello Azure VNet(s) (viz [infrastruktury SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- Sítě v HANA velké instance, který je ve většině případů transparentní pro vás jako zákazník.

![Virtuální síť Azure připojené tooSAP HANA na Azure (velké instance) a na místě](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

fakt Hello použít HANA velké instancí nezmění tooget požadavek hello vaše místní prostředky připojené prostřednictvím ExpressRoute tooAzure. Také nemění hello požadavek na jeden nebo více virtuálních sítí, které používají virtuální počítače Azure hello které hostitele hello aplikační vrstvu, která se připojuje instancí HANA toohello hostovaná v instanci HANA velké jednotky. 

nasazení tooSAP Hello rozdíl v čistě Azure, se dodává s fakty hello který:

- Hello velké Instance HANA jednotky vašeho zákazníka klienta jsou připojené prostřednictvím jiného okruhu ExpressRoute do vaší VNet(s) Azure. V pořadí tooseparate zatížení podmínky, hello hello místní tooAzure ExpressRoute virtuální sítě a odkazy mezi virtuálními sítěmi Azure a HANA velké instancí nesdílejí stejný směrovače.
- profil Hello zatížení mezi hello SAP aplikační vrstvu a hello HANA Instance je jiný druh mnoho malých požadavků a shluků jako data přenáší (sady výsledků) ze SAP HANA do hello aplikační vrstvu.
- Architektura aplikace SAP Hello je citlivější toonetwork latenci než typické scénáře, kde získá data vyměňují mezi místními a Azure.
- Brána virtuální sítě Hello má alespoň dvě připojení ExpressRoute a obě připojení sdílet hello maximální šířka pásma pro příchozí data hello brány virtuální sítě.

latence sítě Hello zkušeného mezi virtuálními počítači Azure a HANA velké jednotky instance může být vyšší než typické latence operace round-trip sítě virtuálních počítačů VM. Závisí na hello oblast Azure, hodnoty hello měřenou může překročit hello 0.7 ms čekací doba přenosu jsou klasifikovány jako pod průměrem v [SAP Poznámka #1100926 – nejčastější dotazy: výkon sítě](https://launchpad.support.sap.com/#/notes/1100926/E). Nicméně zákazníkům nasazené aplikace na základě SAP HANA produkční SAP velmi úspěšně na velké instance SAP HANA. Hello zákazníky, kteří nainstalovali hlášených skvělé vylepšení svých aplikací SAP systémem SAP HANA pomocí Instance HANA velké jednotky. Nicméně byste měli otestovat své obchodní procesy důkladně v Azure HANA velké instancí.
 
V pořadí latence sítě deterministickou tooprovide mezi virtuálními počítači Azure a velké Instance HANA je nezbytné hello volbu hello skladová položka brány virtuální sítě Azure. Na rozdíl od hello vzory přenosů dat mezi místní a virtuální počítače Azure můžete vyvíjet hello vzor provoz mezi virtuálními počítači Azure a velké instancí HANA malé ale vysoké shluky požadavky a toobe svazky dat přenášených. V pořadí toohave takové shluky zpracovává dobře, důrazně doporučujeme hello využití hello UltraPerformance skladová položka brány. Pro hello typu II třídu HANA velké Instance SKU využití hello hello UltraPerformance brány SKU jako brána virtuální sítě Azure je povinné.  

> [!IMPORTANT] 
> Zadaný hello celkové síťový provoz mezi vrstvy databáze a aplikace SAP hello pouze hello HighPerformance nebo UltraPerformance SKU pro virtuální sítě brány je podporovaná pro připojení tooSAP HANA v Azure (velké instance). Pro velké HANA instance typu II SKU je podporováno pouze hello skladová položka brány UltraPerformance jako brána virtuální sítě Azure.



### <a name="single-sap-system"></a>Jednoho systému SAP

Hello na místní infrastrukturu výše uvedeném připojené prostřednictvím ExpressRoute do Azure a hello okruh ExpressRoute připojí do Microsoft Enterprise Edge směrovač (MSEE) (viz [technický přehled ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Po vytvoření danou trasu připojí do páteřní hello Microsoft Azure, a všechny oblasti Azure dostupná.

> [!NOTE] 
> SAP krajiny běžící v Azure, připojte toohello MSEE toohello nejbližší oblast Azure z šířku SAP hello. Azure velké Instance razítka jsou propojeny pomocí vyhrazené MSEE zařízení toominimize latence sítě mezi virtuálními počítači Azure v Azure IaaS a velké Instance razítka.

Hello brány virtuální sítě pro virtuální počítače Azure, který je hostitelem instance aplikace SAP, hello je připojených toothat okruh ExpressRoute a hello stejnou virtuální síť je připojená tooa samostatné směrovači MSEE vyhrazené tooconnecting tooLarge Instance razítka.

Toto je jednoduchý příklad jednoho systému SAP, kde hello SAP aplikační vrstvu je hostovaná v Azure a databázi SAP HANA hello běží na SAP HANA v Azure (velké instance). předpokládá Hello je šířku pásma bránu VNet hello 2 GB/s nebo 10 GB/s propustnost nepředstavuje úzkým místem.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Více systémů SAP nebo velké systémy SAP

Pokud více systémů SAP nebo velké systémy SAP nasazených připojení tooSAP HANA v Azure (velké instance), jeho & č. 39; s přiměřené tooassume hello propustnosti brány virtuální sítě hello může stát úzkým místem. V takovém případě musíte toosplit hello aplikačními vrstvami do více virtuálních sítí Azure. Toto nastavení také může být recommendable toocreate speciální virtuální sítě, který je připojen tooHANA velké instance pro případy:

- Provádění záloh přímo z hello HANA instancí v tooa HANA velké instancí virtuálního počítače v Azure který je hostitelem sdílených složek systému souborů NFS
- Kopírování velkých zálohy nebo další soubory z Instance HANA velké jednotky toodisk prostoru spravované v Azure.

Pomocí samostatné virtuální sítě, že hostitel virtuálních počítačů, které spravovat úložiště hello zabraňuje dopad na velkých souborů nebo přenos dat z velké instancí HANA tooAzure na hello brány virtuální sítě, která slouží hello virtuální počítače se systémem hello SAP aplikační vrstvu. 

Pro více škálovatelná architektura sítě:

- Využívejte více virtuálními sítěmi Azure pro jeden, větší SAP aplikační vrstvu.
- Nasaďte jeden samostatný virtuální síť Azure pro každý SAP systém nasadí, porovnání toocombining tyto systémy SAP v oddělené podsítě v části hello stejnou virtuální síť.

 Větší škálovatelnost sítě architektura pro SAP HANA v Azure (velké instance):

![Nasazení SAP aplikační vrstvu nad více virtuálními sítěmi Azure](./media/hana-overview-architecture/image4-networking-architecture.png)

Nasazení hello SAP aplikační vrstvu nebo součásti, prostřednictvím více virtuálními sítěmi Azure, jako v příkladu nahoře, zavedl nevyhnutelné latence režie, které došlo při komunikaci mezi aplikacemi hello hostované v těchto virtuálních sítí Azure. Ve výchozím nastavení hello síťový provoz mezi virtuálními počítači Azure nachází v různých virtuálních sítí směrovat přes hello směrovači MSEE v této konfiguraci. Nicméně od září 2016 tento směrování lze optimalizovat. Hello způsob toooptimize a omezit hello latence při komunikaci mezi dvěma virtuálními sítěmi je partnerského vztahu Azure virtuální sítě v rámci hello stejné oblasti. I když tyto virtuální sítě jsou v různých předplatných. Pomocí Azure VNet peering –, hello komunikace mezi virtuálními počítači ve dvou různých virtuálních sítí Azure můžete použít hello síť Azure páteřní toodirectly vzájemně komunikovat. Tím podobnou latenci zobrazující jakoby hello virtuálních počítačů bude v hello stejnou virtuální síť. Zatímco provoz, adresování rozsahy IP adres, které jsou připojené prostřednictvím hello brány virtuální sítě Azure, je směrován přes hello jednotlivých Brána virtuální sítě hello virtuální sítě. Můžete získat podrobnosti o službě Azure VNet partnerský vztah v článku hello [VNet peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Směrování v Azure

Existují dva aspekty směrování důležité sítě pro SAP HANA v Azure (velké instance):

1. SAP HANA v Azure (velké instance) můžete přistupovat pouze virtuální počítače Azure v hello vyhrazené připojení ExpressRoute; Ne přímo z místní. Někteří klienti správy a všechny aplikace, které vyžadují přímý přístup, jako je správce řešení SAP spuštěného na místě, se nemůže připojit databázi SAP HANA toohello.

2. SAP HANA na jednotkách Azure (velké instance) mají přiřazenou IP adresu z fondu serverů IP adresy hello rozsahu můžete jako zákazník hello odeslána (viz [infrastruktury SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnosti).  Tato IP adresa je přístupná prostřednictvím hello Azure odběry a ExpressRoute, který se připojuje sítě Azure Vnet tooHANA v Azure (velké instance). Hello přiřazené mimo tento rozsah adres fondu IP serveru IP adresy je přímo přiřazena toohello hardwaru jednotka a není NAT'ed už byla hello v takovém případě hello první nasazení tohoto řešení. 

> [!NOTE] 
> Pokud potřebujete tooconnect tooSAP HANA v Azure (velké instance) ve _datového skladu_ scénář, kde aplikace nebo koncoví uživatelé musí databázi SAP HANA toohello tooconnect (spuštěna přímo), musí být jiné síťové součásti použít: data tooroute reverznímu proxy serveru, tooand z. Například F5 BIG-IP, NGINX s Traffic Manager nasazené v Azure jako virtuální brány firewall nebo provoz směrování řešení.

### <a name="internet-connectivity-of-hana-large-instances"></a>Připojení k Internetu velké instancí HANA
Velké instancí HANA nemají přímé připojení k Internetu. Toto je omezení vaše možnosti, například zaregistrovat hello bitovou kopii operačního systému přímo s dodavatelem hello operačního systému. Proto může být nutné toowork místní server SLES SMT nebo RHEL odběr Manager

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Šifrování dat mezi virtuálními počítači Azure a velké instancí HANA
Není zašifrovaná data přenesená mezi instancemi velké HANA a virtuálních počítačích Azure. Můžete však výhradně pro výměnu hello mezi hello straně HANA databázového systému a aplikací na základě JDBC nebo rozhraní ODBC povolit šifrování přenosů. Referenční dokumentace [této dokumentace podle SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Pomocí HANA velké jednotky Instance v několika oblastech

Můžete mít další důvody toodeploy SAP HANA v Azure (velké instance) v několika oblastech Azure, kromě zotavení po havárii. Můžete chtít tooaccess HANA velké instancí z každé hello virtuálních počítačích nasazených v hello různých virtuálních sítí v oblastech hello. Vzhledem k tomu, že hello IP adresy přiřazené toohello různých instancí HANA velké jednotky nebudou rozšířeny nad rámec hello virtuální sítě Azure (které jsou přímo připojené prostřednictvím jejich toohello instance brány), je lehká změnit toohello návrh virtuální sítě zavedená výše: Služba Azure gateway virtuální síť dokáže zpracovat čtyři jiné okruhy ExpressRoute mimo jiné Msee a každé virtuální sítě, který je připojený tooone hello velké Instance razítek, která může být připojené toohello velké Instance razítka v jiné oblasti Azure.

![Virtuálních sítí Azure připojené tooAzure velké Instance razítka v různých oblastech Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

Hello výše obrázek ukazuje jak hello různých virtuálních sítí Azure v obou oblastí jsou připojené tootwo jiné okruhy ExpressRoute, které jsou používané tooconnect tooSAP HANA v Azure (velké instance) v obou oblastech Azure. Hello nově zavedené připojení jsou hello obdélníková červené čáry. Tato připojení mimo hello virtuálních sítí Azure, hello virtuální počítače spuštěné v jednom z těchto virtuálních sítí mohou mít přístup ke každé z hello různých instancí HANA velké jednotky nasazené ve dvou oblastech hello. Jak vidíte v hello grafiky výše uvedené, se předpokládá, že máte dvě připojení ExpressRoute z místní toohello dvou oblastech Azure; Doporučujeme důvodů zotavení po havárii.

> [!IMPORTANT] 
> Pokud se používají více okruhů ExpressRoute, by měl být předřazení AS Path a místní BGP předvoleb nastavení používané tooensure správné směrování provozu.


