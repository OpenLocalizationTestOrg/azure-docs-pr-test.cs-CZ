---
title: "aaaInstall SAP HANA na SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Jak tooinstall SAP HANA na SAP HANA v Azure (velké Instance)."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
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
ms.openlocfilehash: b2fe242270a1166cabcfae2f9249a8dd70ff3b93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-sap-hana-large-instances-on-azure"></a>Jak tooinstall a konfigurace SAP HANA (velké instance) na Azure

Toto jsou některé důležité definice tooknow před čtením této příručky. V [přehled SAP HANA (velké instance) a architektura v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) zavedli jsme dvěma různými třídami velké Instance HANA jednotek s:

- S72, S72m, S144, S144m, S192 a S192m, které označujeme tooas hello "Typ I třídy" z jednotky SKU.
- S384, S384m, S384xm, S576, S768 a S960, které označujeme tooas hello "Typu II třídou" SKU.

specifikátor třídy Hello je toobe probíhající používaných v celém hello HANA velké Instance tooeventually dokumentaci najdete toodifferent možnosti a požadavky založené na HANA velké Instance SKU.

Další definice, které jsme používané jsou:
- **Velké Instance razítka:** zásobníku infrastruktury hardwaru, který je SAP HANA TDI certifikaci a vyhrazené instance SAP HANA toorun v rámci Azure.
- **SAP HANA v Azure (velké instance):** oficiální název hello nabídka v Azure toorun HANA instance v na SAP HANA TDI certifikaci hardwaru, která je nasazena v velké Instance razítka v různých oblastech Azure. Hello související termín **HANA velké Instance** je zkratka pro SAP HANA v Azure (velké instance) a je široce používá tato příručka technického nasazení.


instalace Hello SAP HANA je vaší povinností a hello aktivitu můžete spustit po předání nové SAP HANA na serveru Azure (velké instance). A po tu navázání hello připojení mezi Azure VNet(s) a hello jednotka HANA velké Instance. 

> [!Note]
> Podle zásad SAP musí provést instalaci hello SAP HANA osoby certifikované tooperform SAP HANA instalace. Osoba, která byla úspěšná hello certifikované přidružit technologie SAP zkoušku, SAP HANA instalace zkouška, nebo certifikaci SAP systémový integrátor (SI).

Zkontrolujte znovu, zejména v případě, že plánování tooinstall HANA 2.0 [SAP podporu Poznámka #2235581 - SAP HANA: podporované operační systémy](https://launchpad.support.sap.com/#/notes/2235581/E) v pořadí toomake jistotu, že hello operačního systému je podporovaná s hello SAP HANA verze je se rozhodli tooinstall. Zjistíte, že hello podporovaný operační systém pro HANA 2.0 je omezenější než hello operačního systému pro HANA 1.0 podporována. 

## <a name="first-steps-after-receiving-hello-hana-large-instance-units"></a>První kroky po přijetí hello HANA velké Instance jednotka

**První krok** po přijetí hello HANA velké Instance a vytvoření instancí toohello přístupu a připojení je tooregister hello OS instance hello u svého poskytovatele operačního systému. Tento krok bude zahrnovat registrace operačním systému SUSE Linux v instanci SMT SUSE, kterou potřebujete toohave nasazené ve virtuálním počítači v Azure. Hello velké Instance HANA jednotku můžete připojit toothis SMT instance (viz dále v této dokumentaci). Nebo operačním systému Red Hat musí toobe zaregistrována hello Red Hat odběr Manager potřebujete tooconnect k. Viz také remarks v tomto [dokumentu](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tento krok je také nutné toobe možné toopatch hello operačního systému. Úloha, která je v hello zodpovědností zákazníků hello. Pro SUSE, najít dokumentaci tooinstall a nakonfigurujte SMT [zde](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**Druhé krok** je toocheck pro nové aktualizace a opravy hello konkrétní operační systém nebo verzi. Zkontrolujte, zda úroveň opravy hello hello HANA velké Instance je na nejnovější stav hello. Podle časování na opravy nebo verze a změny toohello image OS, které můžete nasadit Microsoft, mohou existovat případech, kdy nejnovější opravy hello nemusí být zahrnuty. Proto je povinný krok po převzetí jednotce velké Instance HANA toocheck zda opravy, které jsou relevantní pro zabezpečení, funkce, dostupnosti a výkonu uvolnila mezitím hello konkrétní Linux dodavatele a potřebovat toobe použít.

**Třetí krok** je toocheck out hello relevantní SAP poznámky pro instalaci a konfiguraci SAP HANA na hello konkrétní operační systém nebo verzi. Z důvodu toochanging doporučení nebo změny tooSAP poznámky nebo konfigurace, které jsou závislé na jednotlivé instalace scénáře, společnost Microsoft vždy nebude možné toohave velké Instance HANA jednotku perfektně nakonfigurované. Proto je povinné pro vás jako zákazník, tooread hello SAP poznámky související tooSAP HANA na vaše přesnou verzi systému Linux. Také zkontrolujte hello konfigurace hello operační systém nebo verzi potřebné a použít nastavení konfigurace hello kde neudělali.

V konkrétní zkontrolujte hello následující parametry a nakonec upravený tak, aby:

- NET.Core.rmem_max = 16777216
- NET.Core.wmem_max = 16777216
- NET.Core.rmem_default = 16777216
- NET.Core.wmem_default = 16777216
- NET.Core.optmem_max = 16777216
- NET.IPv4.tcp_rmem = 65536 16777216 16777216
- NET.IPv4.tcp_wmem = 65536 16777216 16777216

Od verze SLES12 SP1 a RHEL 7.2, musí tyto parametry nastavit v konfiguračním souboru v adresáři /etc/sysctl.d hello. Například musíte vytvořit konfigurační soubor s názvem hello 91-NetApp-HANA.conf. Pro starší verze SLES a RHEL musí být tyto parametry in/etc/sysctl.conf sady.

Pro všechny RHEL uvolní a počínaje SLES12, hello 
- sunrpc.tcp_slot_table_entries = 128

Parametr musí být nastaven in/etc/modprobe.d/sunrpc-local.conf. Pokud hello soubor neexistuje, musí nejdřív vytvořit přidáním hello následující položky: 
- Možnosti sunrpc tcp_max_slot_table_entries = 128

**Čtvrtý krok** je toocheck hello systémového času HANA velké jednotky Instance. instance Hello se nasadí s systémovým časovým pásmem, která představují hello umístění hello hello oblast Azure, HANA velké Instance razítka je umístěno v. Jste volné toochange hello systémového času nebo časové pásmo hello instancí, které vlastníte. Díky tomu a řazení více instancí do vašeho klienta, připravené, je nutné, aby tooadapt hello časové pásmo hello nově doručit instancí. Microsoft operations mít žádné přehledy hello systémovým časovým pásmem, že nastavíte s instancí hello po předání hello. Proto nově nasazené instancí nemusí být nastavena v hello stejné časové pásmo jako jeden změněn na hello. V důsledku toho je vaší povinností jako zákazník toocheck a v případě potřeby přizpůsobit hello časové pásmo předá instance hello. 

**První krok** toocheck atd nebo hostitele. Jak získat předá hello oken, mají různé IP adresy přiřazené pro různé účely (viz další část). Zkontrolujte soubor etc/hosts hello. V případech, kde jsou jednotky přidané do existujícího klienta neočekávané toohave atd nebo hostitelů systémů hello nově nasazené správně udržovaná hello IP adresy dříve doručené systémů. Proto je na vás jako zákazník toocheck hello správné nastavení tak, že nově nasazená instance spolupracovat a překlad názvů hello dříve nasazené jednotek ve vašem klientovi. 

## <a name="networking"></a>Sítě
Předpokládáme, že jste udělali hello doporučení při navrhování sítě Azure Vnet a připojení u velkých instancí virtuálních sítí toohello HANA, jak je popsáno v těchto dokumentů:

- [Přehled SAP HANA (velké Instance) a architektura v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infrastruktura SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Existují některé podrobnosti vhodné toomention o možnostech sítě hello hello jedné jednotky. Každá jednotka HANA velké Instance obsahuje dvě nebo tři IP adresy, které jsou přiřazeny tootwo nebo tři porty NIC hello jednotky. Tři IP adresy se používají v HANA konfigurace Škálováním na více systémů a scénář replikaci systému HANA hello. Jeden z hello IP adresy přiřazené toohello síťový adaptér hello jednotky je mimo hello fond IP adresy serveru, který je popsáno v hello [přehled SAP HANA (velké Instance) a architektura v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Hello distribuce pro jednotky s přiřazeny dvě IP adresy by měl vypadat podobně jako:

- eth0.xx by měl mít IP adresu přiřadit, které je mimo rozsah adres fondu IP serveru odeslané tooMicrosoft hello. Tato IP adresa se použije pro zachování v/etc/hosts Dobrý den operačního systému.
- eth1.xx by měl mít IP adresu přiřadit, který se používá pro komunikaci tooNFS. Proto se tyto adresy **není** potřebovat toobe udržuje v atd nebo hostitele v pořadí tooallow instance tooinstance provoz v rámci klienta hello.

Pro nasazení případech replikaci systému HANA nebo HANA škálování není vhodný okno Konfigurace s přiřazeny dvě IP adresy. Pokud s dvě IP adresy, které jsou přiřazeny pouze a podmínka mimo toodeploy konfigurace, obraťte se na SAP HANA na Azure Service Management tooget třetí IP adresu ve třetí přiřazené sítě VLAN. Pro velké Instance HANA jednotky s tři IP adresy přiřazené na tři porty NIC hello využití platí následující pravidla:

- eth0.xx by měl mít IP adresu přiřadit, které je mimo rozsah adres fondu IP serveru odeslané tooMicrosoft hello. Proto tuto IP adresu se nepoužijí pro zachování v/etc/hosts Dobrý den operačního systému.
- eth1.xx by měl mít IP adresu přiřadit, který se používá pro komunikaci tooNFS úložiště. Tento typ adresy proto by nemělo být udržovány v etc/hosts.
- eth2.xx musí být výhradně použité toobe udržuje v atd nebo hostitele pro komunikaci mezi různými instancemi hello. Tyto adresy by také hello IP adresy, které je třeba toobe udržovat v konfiguracích HANA škálování jako IP adresy, kterou HANA používá pro konfiguraci hello mezi uzly.



## <a name="storage"></a>Úložiště

Hello rozložení úložiště pro SAP HANA v Azure (velké instance) se nakonfiguruje prostřednictvím SAP HANA na Azure Service Management prostřednictvím SAP doporučená Průvodce řádky, jak je uvedeno v [požadavky na úložiště SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) dokumentu white paper. Hello hrubý velikosti hello jiné svazky s hello různé instance SKU velké HANA tu popsané v [přehled SAP HANA (velké Instance) a architektura v Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

zásady vytváření názvů Hello hello svazků úložiště jsou uvedeny v hello následující tabulka:

| Využití úložiště | Název připojení | Název svazku | 
| --- | --- | ---|
| HANA data | /Hana/data/SID/mnt0000<m> | Úložiště IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA protokolu | /Hana/log/SID/mnt0000<m> | Úložiště IP: / hana_log_SID_mnt00001_tenant_vol |
| Záloha protokolu HANA | /Hana/log/backups | Úložiště IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| Sdílené HANA | /Hana/Shared/SID | Úložiště IP: / hana_shared_SID_mnt00001_tenant_vol/sdílené |
| USR/sap | /USR/SAP/SID | Úložiště IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Kde SID = hello HANA instance ID systému 

A klient = interní výčet operací při nasazování klienta.

Jak vidíte, jsou sdílení HANA sdílené a usr/sap hello stejný svazek. klasifikace Hello přípojné body hello nezahrnuje hello ID systému hello HANA instancí, jakož i číslo přípojného hello. V nasazení škálování jenom je jeden přípojný, jako je mnt00001. Zatímco v nasazení s více instancemi zobrazí se jako v mnoha připojení zařízení, jako, máte uzly pracovního procesu a hlavní server. Pro prostředí hello Škálováním na více systémů, dat, protokolu jsou záložní svazky protokolu sdílené a připojené tooeach uzel v konfiguraci hello Škálováním na více systémů. Pro konfigurace spuštěním několika instancí SAP jinou sadu svazků je jednotka velké Instance HANU toohello vytvořené a připojené.

Při čtení dokumentu hello a vypadat Instance HANA velké jednotky, zjistíte, že hello jednotky dodávány spolu s místo štědrém diskový svazek pro HANA nebo data a že obsahuje svazek HANA/log/zálohování. Hello důvod, proč jsme velikost hello HANA nebo data tak velký, je, že hello hello úložiště snímků, které nabízí pro jako zákazník používáte stejný svazek disku. Znamená to hello další úložiště snímků, které můžete provádět, hello, které využívá víc místa snímky v přiřazené úložiště svazků. Hello HANA/log/zálohování je svazek není myšlenku toobe hello tooput zálohování databáze v. Je velikostí toobe používat jako záložní svazek pro zálohování transakčního protokolu hello HANA. V budoucích verzích hello úložiště snímku samoobslužné služby, že jsme se zaměří na tuto konkrétní svazek toohave další časté snímky. A s třídou další časté lokalitě obnovení po havárii toohello replikace li toooption-in pro fungování obnovy po havárii hello poskytované hello velké Instance HANA infrastruktury. Zobrazit podrobnosti v [SAP HANA (velké instance) vysokou dostupnost a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Kromě toho toohello úložiště k dispozici, si můžete zakoupit dodatečné úložiště kapacity v přírůstcích po 1 TB. Toto dodatečné úložiště se dá přidat jako nové svazky tooa HANA velké instancí.

Během registrace s SAP HANA na Azure Service Management hello zákazníka určuje ID uživatele (UID) a ID skupiny (GID) pro skupiny uživatelů a sapsys sidadm hello (např: 1000,500) je nutné při instalaci hello systému SAP HANA, používají tyto stejné hodnoty. Jak chcete toodeploy více instancí HANA na jednotce, získáte více sad svazky (jednu sadu pro každou instanci). V době nasazení, je třeba v důsledku toho toodefine:

- Hello SID různých instancí HANA hello (sidadm je odvozený z jeho).
- Velikosti paměti HANA instancí jiné hello. Vzhledem k tomu, že velikost paměti hello za instance definuje hello velikost svazků hello v každé sadě jednotlivých svazku.

Na základě doporučení zprostředkovatele úložiště hello následující možnosti připojení jsou nakonfigurované pro všechny připojené svazky (nezahrnuje spouštěcí logickou jednotku):

- systém souborů NFS rw vladače = 4, pevné, timeo = 600, parametru rsize = 1048576 wsize = 1048576, intr, noatime, uzamčení 0 0

Tyto přípojné body, které jsou konfigurovány v fstab/etc/jako uvedené v hello následující grafiky:

![fstab připojených svazků v jednotce velké Instance HANA](./media/hana-installation/image1_fstab.PNG)

výstup Hello hello příkaz df – h na jednotce S72m HANA velké Instance by vypadat podobně jako:

![fstab připojených svazků v jednotce velké Instance HANA](./media/hana-installation/image2_df_output.PNG)


Řadič úložiště Hello a uzly v hello velké Instance razítka nejsou synchronizované tooNTP servery. S vámi synchronizaci hello SAP HANA jednotek Azure (velké instance) a virtuální počítače Azure proti serveru NTP měla by existovat žádné významné čas odlišily situaci mezi hello infrastruktury a hello výpočetní jednotky v Azure nebo velké Instance razítka.

V pořadí toooptimize SAP HANA toohello úložiště používané pod je třeba nastavit také následující konfigurační parametry SAP HANA hello:

- max_parallel_io_requests 128
- async_read_submit na
- async_write_submit_active na
- všechny async_write_submit_blocks
 
Verze SAP HANA 1.0 až tooSPS12, tyto parametry lze nastavit během instalace hello hello databázi SAP HANA, jak je popsáno v [SAP Poznámka #2267798 - konfigurace hello databázi SAP HANA](https://launchpad.support.sap.com/#/notes/2267798)

Také můžete nakonfigurovat parametry hello po instalaci databáze SAP HANA hello pomocí hello hdbparam framework. 

S SAP HANA 2.0 je zastaralá hello hdbparam framework. V důsledku hello parametry musí být nastavena pomocí příkazů SQL. Podrobnosti najdete v tématu [&#2399079; Poznámka SAP: odstranění hdbparam v HANA 2](https://launchpad.support.sap.com/#/notes/2399079).


## <a name="operating-system"></a>Operační systém

Velikosti odkládacího souboru Dobrý den doručeny bitové kopie operačního systému je nastaven podle toohello GB too2 [SAP podporu Poznámka #1999997 – nejčastější dotazy: paměti SAP HANA](https://launchpad.support.sap.com/#/notes/1999997/E). Žádné jiné nastavíte požadované vyžaduje toobe nastavíte vy jako zákazník.

[SUSE Linux Enterprise Server 12 SP1 pro aplikace SAP](https://www.suse.com/products/sles-for-sap/hana) je hello distribuce systému Linux nainstalován pro SAP HANA v Azure (velké instance). Tuto konkrétní distribuci poskytuje funkce specifické pro SAP &quot;předinstalované hello&quot; (včetně předem nastavené parametry pro SAP systémem SLES efektivně).

V tématu [dokumenty prostředků knihovny nebo White Paper](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) na webu SUSE hello a [SAP na SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) na hello oznámení změny SAP komunity sítě (stavu), pro několik užitečné zdroje související s toodeploying SAP HANA na SLES (včetně hello nastavení Vysoká dostupnost, operace konkrétní tooSAP posílení zabezpečení a další).

Další a užitečné SAP SUSE související odkazy:

- [SAP HANA v lokalitě SUSE Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Nejvhodnější pro SAP: replikace zařazování – SAP NetWeaver na operačního systému SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – SLES antivirovou ochranu pro SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (včetně SLES 12 pro aplikace SAP).

SAP podporu poznámky platí tooimplementing SAP HANA na SLES 12:

- [Podpora Poznámka SAP #1944799 – SAP HANA pokyny pro instalaci operačního systému SLES](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [Podpora Poznámka SAP #2205917 – SAP HANA DB doporučené nastavení operačního systému pro SLES 12 pro aplikace SAP](https://launchpad.support.sap.com/#/notes/2205917/E).
- [Podpory Poznámka SAP #1984787 – SUSE Linux Enterprise Server 12: Poznámky k instalaci](https://launchpad.support.sap.com/#/notes/1984787).
- [Podpora Poznámka SAP #171356 – SAP softwaru v systému Linux: Obecné informace](https://launchpad.support.sap.com/#/notes/1984787).
- [Podpora Poznámka SAP #1391070 – řešení Linux UUID](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux pro SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) je jinou nabídku pro spouštění na HANA velké instance SAP HANA. Verze RHEL 6.7 a 7.2 jsou k dispozici. 

Další a užitečné SAP Red Hat související odkazy:
- [SAP HANA na Red Hat Linux lokality](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP podporu poznámky platí tooimplementing SAP HANA na Red Hat:

- [Podpora Poznámka SAP #2009879 - SAP HANA pokyny pro operační systém Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879/E).
- [Podporu Poznámka SAP #2292690 - SAP HANA DB: OS doporučená nastavení pro RHEL 7](https://launchpad.support.sap.com/#/notes/2292690).
- [Podporu Poznámka SAP #2247020 - SAP HANA DB: OS doporučená nastavení pro RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [Podpora Poznámka SAP #1391070 – řešení Linux UUID](https://launchpad.support.sap.com/#/notes/1391070).
- [Podpora Poznámka SAP #2228351 - Linux: SAP HANA databáze aktualizace Service Pack 11 revize 110 (nebo vyšší) v systému RHEL 6 nebo SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [Podpora Poznámka SAP #2397039 – nejčastější dotazy: SAP na RHEL](https://launchpad.support.sap.com/#/notes/2397039).
- [Podpora Poznámka SAP #1496410 - Red Hat Enterprise Linux 6.x: instalace a Upgrade](https://launchpad.support.sap.com/#/notes/1496410).
- [Podpora Poznámka SAP #2002167 - Red Hat Enterprise Linux 7.x: instalace a Upgrade](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Synchronizaci času

SAP aplikace založené na hello SAP NetWeaver architektura jsou citlivé na čas rozdíly pro hello různé součásti, které tvoří hello systému SAP. Výpisy paměti SAP ABAP krátké s hello chyba název ZDATE\_velké\_čas\_Rozdílové jsou pravděpodobně známý, jak tyto krátké výpisy zobrazí při hello je příliš daleko od sebe plovoucí systémového času jiné servery nebo virtuální počítače.

Pro SAP HANA v Azure (velké instance), časové synchronizace provádí v Azure nemá & č. 39; použít t toohello výpočetní jednotky v hello velké Instance razítka. Tato synchronizace se nedá použít pro spouštění aplikací SAP v nativní virtuálních počítačích Azure, jak Azure zajistí systému & č. 39; s čas je správně synchronizovaný. V důsledku toho samostatné čas, který server musí být nastaven, mohou být využívána SAP aplikačních serverů se systémem na virtuálních počítačích Azure a hello SAP HANA databáze instancí spuštěných na velké instance HANA. infrastruktura úložiště Hello ve velkých Instance razítka je časově synchronizované s NTP servery.

## <a name="setting-up-smt-server-for-suse-linux"></a>Nastavení serveru SMT SUSE Linux
Velké instance SAP HANA nemají toohello přímé připojení k Internetu. Proto není jednoduchý proces tooregister jednotku s poskytovatelem hello operačního systému a toodownload a používat opravy. V případě hello systému SUSE Linux může být jedno řešení tooset až serveru SMT ve virtuálním počítači Azure. Zatímco hello virtuálního počítače Azure musí toobe hostované v Azure VNet, která je připojená toohello HANA velké Instance. Pomocí těchto SMT serveru může hello velké Instance HANA jednotka zaregistrovat a stažení oprav. 

SUSE poskytuje většího Průvodce na jejich [nástroj pro správu předplatného pro SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Jako předběžnou podmínku pro instalaci hello splňující hello úloh pro velké instanci HANA SMT serveru musíte:

- Virtuální síť Azure, který je připojený toohello HANA velké Instance ER okruhu.
- SUSE účet, který je přidružen organizaci. Zatímco hello organizace potřebovat toohave některé platné předplatné SUSE.

### <a name="installation-of-smt-server-on-azure-vm"></a>Instalace serveru SMT na virtuální počítač Azure

V tomto kroku instalujete server SMT hello virtuální počítač Azure. první měr Hello je toolog v toohello [SUSE zákazníka Center](https://scc.suse.com/)

Protože jste přihlášeni, přejděte tooOrganization--> přihlašovacích údajů organizace. V této části by měl zjistit hello přihlašovací údaje, které jsou nezbytné tooset hello SMT server.

třetí krok Hello je tooinstall virtuálního počítače s Linuxem SUSE v hello virtuální síť Azure. toodeploy hello virtuálních počítačů, trvat bitovou kopii SLES 12 SP2 Galerie Azure. V procesu nasazení hello nemusíte definovat název DNS a nepoužívejte statické IP adresy, jak je vidět v tento snímek obrazovky

![nasazení virtuálního počítače pro SMT server](./media/hana-installation/image3_vm_deployment.png)

Hello nasazených virtuálních počítačů byla menší virtuálního počítače a obdržel hello interní IP adresu ve službě Azure VNet 10.34.1.4 hello. Název hello virtuálního počítače byl smtserver. Po instalaci hello byla zaškrtnutá hello připojení toohello HANA velké jednotky instance. Závisí na uspořádání překlad může být nutné tooconfigure rozlišení hello velké Instance HANA jednotky v atd na hostitele hello virtuálního počítače Azure. Přidáte toohello další disk virtuálního počítače, který se bude, že toobe používá toohold hello opravy. spouštěcí disk Hello samotné může být příliš malá. V případě hello ukázán tu hello disku připojeného příliš/srv/www/htdocs, jak ukazuje následující snímek obrazovky hello. Měla by stačit 100 GB místa na disku.

![nasazení virtuálního počítače pro SMT server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Přihlaste toohello velké Instance HANA jednotka, Udržovat/etc/hosts a zkontrolujte, zda mohou mít přístup hello virtuálního počítače Azure, který by měl toorun hello SMT serveru přes síť hello.

Po tato kontrola probíhá úspěšně, je třeba toolog v toohello virtuálního počítače Azure, který se má spustit hello SMT server. Pokud používáte putty toolog toohello virtuálních počítačů, je třeba tooexecute této sekvence příkazů v okně bash:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Po provedení těchto příkazů, restartujte nastavení hello tooactivate bash. Spusťte YAST.

V YAST přejděte tooSoftware údržby a vyhledejte smt. Vyberte smt, který tooyast2 smt přepne automaticky, jak je uvedeno níže

![SMT v yast](./media/hana-installation/image5_smt_in_yast.PNG)


Přijměte hello hodnotu pro instalaci na hello smtserver. Po instalaci, přejděte konfigurace serveru SMT toohello a zadejte hello firemní přihlašovací údaje z hello SUSE zákazníka Center, který jste získali dříve. Váš název hostitele virtuálního počítače Azure můžete také zadejte jako hello SMT adresa URL serveru. V této ukázce bylo https://smtserver, jak je uvedeno v další grafiky hello.

![Konfigurace serveru SMT](./media/hana-installation/image6_configuration_of_smtserver1.png)

Jako další krok je nutné tootest, zda funguje hello připojení toohello SUSE zákazníka Center. Jak vidíte v hello následující grafiky, v případě ukázkový hello fungovat.

![Testovací připojení tooSUSE zákazníka Center](./media/hana-installation/image7_test_connect.png)

Jednou hello SMT instalační program spustí, je nutné tooprovide heslo k databázi. Vzhledem k tomu, že jde o novou instalaci, musíte toodefine toto heslo, jak je znázorněno v další grafiky hello.

![Zadejte heslo pro databázi](./media/hana-installation/image8_define_db_passwd.PNG)

Další interakce Hello, měli byste je, když se vytvoří certifikát. Přejděte prostřednictvím dialogu hello, jak ukazuje následující a hello krok by měly pokračovat.

![Vytvoření certifikátu pro SMT server](./media/hana-installation/image9_certificate_creation.PNG)

Může být několik minut stráví v kroku hello "Spustit synchronizaci kontrolou" na konci hello hello konfigurace. Po hello instalaci a konfiguraci serveru SMT hello, byste měli najít hello directory úložišti pod hello přípojného bodu /srv/www/htdocs/plus některé dílčí adresáře v úložišti. 

Restartujte hello SMT server a její služby související s těmito příkazy.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Stahování balíčky do serveru SMT

Po všech hello restartování služby, vyberte hello odpovídajících balíčků ve správě SMT pomocí Yast. Výběr balíčku Hello závisí na bitovou kopii operačního systému hello hello HANA velké Instance serveru a ne na hello SLES verzi nebo verzi serveru spuštěné hello SMT hello virtuálního počítače. Níže je uveden příklad obrazovky výběru hello.

![Vyberte balíčky](./media/hana-installation/image10_select_packages.PNG)

Jakmile budete hotovi s výběrem hello balíčku, musíte toostart hello počáteční kopii hello vyberte balíčky toohello SMT serveru, kterou vytvoříte. Tato kopie se spustí v prostředí hello pomocí hello příkaz smt zrcadlení jak je uvedeno níže


![Stáhnout balíčky tooSMT serveru](./media/hana-installation/image11_download_packages.PNG)

Jak můžete vidět výše, by měl získat hello balíčky zkopírován do adresáře hello vytvořil v rámci hello přípojného bodu /srv/www/htdocs. Tento proces může chvíli trvat. Závisí na tom, kolik balíčky, které jste vybrali, může trvat až tooone hodinu nebo déle.
Když tento proces dokončí, je nutné toomove toohello SMT klientské instalace. 

### <a name="set-up-hello-smt-client-on-hana-large-instance-units"></a>Nastavení klienta SMT hello na velké Instance HANA jednotky

Hello mailového klienta, jsou v tomto případě hello velké Instance HANA jednotky. Instalační program serveru SMT Hello hello skriptu clientSetup4SMT.sh zkopírují do hello virtuálního počítače Azure. Zkopírujte tento skript přes toohello velké Instance HANA jednotku, tooconnect tooyour SMT serveru. Spusťte skript hello s -h hello a dejte mu jako název parametru hello SMT serveru. V této smtserver příklad.

![Konfigurace klienta SMT](./media/hana-installation/image12_configure_client.PNG)

Mohou existovat scénáře, kde hello zatížení hello certifikát ze serveru hello klientem hello byl úspěšný, avšak hello registrace se nezdařila, jak je uvedeno níže.

![Neúspěšné registrace klienta](./media/hana-installation/image13_registration_failed.PNG)

Pokud registrace hello se nezdařila, přečtěte si to [SUSE podporu dokumentu](https://www.suse.com/de-de/support/kb/doc/?id=7006024) a provedení hello postupu existuje.

> [!IMPORTANT] 
> Jako název serveru je třeba název hello tooprovide hello virtuálních počítačů, v této případu smtserver bez hello plně kvalifikovaný název domény. Právě hello virtuálních počítačů název funguje. 

Po provedení těchto kroků musíte tooexecute hello následující příkaz na jednotce velké Instance HANA hello

```
SUSEConnect –cleanup
```

> [!Note] 
> V našich testech jsme vždy měli toowait pár minut po provedení tohoto kroku. Hello clientSetup4SMT.sh okamžité spuštění po hello opravná opatření popsané v článku SUSE hello skončila s zprávy, že tento certifikát hello nebude dosud platný. Čeká se obvykle 5 až 10 minut a provádění clientSetup4SMT.sh skončila v konfiguraci klienta úspěšné.

Pokud jste spustili do hello problém, který potřeby toofix podle kroků hello hello SUSE článku, toorestart clientSetup4SMT.sh na jednotce velké Instance HANA hello je nutné znovu. Nyní je třeba dokončit úspěšně, jak je uvedeno níže.

![Registrace klienta proběhla úspěšně.](./media/hana-installation/image14_finish_client_config.PNG)

Pro tento krok nakonfigurovat klienta SMT hello hello velké Instance HANA jednotky tooconnect proti hello SMT serveru, který jste nainstalovali v hello virtuálního počítače Azure. Nyní můžete trvat 'zypper nahoru' nebo 'zypper v' tooinstall OS opravy tooHANA velké instance nebo nainstalovat další balíčky. Předpokládá se, že pouze můžete získat opravy, které jste stáhli před na serveru SMT hello.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Příklad instalace SAP HANA na velké instancí HANA
Tato část ukazuje, jak tooinstall SAP HANA na jednotce, velké Instance HANA. Počáteční stav Hello jsme vypadat:

- Poskytuje Microsoft všechna data toodeploy hello je velké Instance SAP HANA.
- Hello velké Instance SAP HANA jste dostali od společnosti Microsoft.
- Vytvořili jste virtuální síť Azure, který je připojený tooyour místní sítě.
- Připojené hello ExpressRotue okruhu pro velké instancí HANA toohello stejnou virtuální síť Azure.
- Nainstalovali jste virtuální počítač Azure používat jako pole přechod pro velké instance HANA.
- Je ověřeno, zda se můžete připojit z hello přechod pole tooyour velké Instance HANA jednotky a naopak.
- Můžete zkontrolovat, zda všechny hello potřebné balíčky a opravy jsou nainstalovány.
- Si přečíst poznámky SAP hello a dokumentace týkající se instalace HANA na hello operačního systému používáte a zkontrolujte, že hello HANA verzi volba je podporována v verze hello operačního systému.

Informace zobrazené v další pořadí hello je ke stažení hello hello HANA instalace balíčků toohello přechod pole virtuálních počítačů, v takovém případě systémem operačního systému Windows hello kopii hello balíčky toohello velké Instance HANA jednotky a pořadí hello instalačního programu hello.

### <a name="download-of-hello-sap-hana-installation-bits"></a>Stahování bits instalace hello SAP HANA
Vzhledem k tomu, že hello velké Instance HANA jednotky nemají toohello přímé připojení k Internetu, nelze stáhnout přímo hello instalační balíčky z toohello SAP HANA velké Instance virtuálních počítačů. tooovercome hello chybějící přímé připojení k Internetu, musíte hello přechod pole. Si stáhnout hello balíčky toohello přechod pole virtuálních počítačů.

Pořadí toodownload hello HANA instalačních balíčků je nutné SAP uživatelem S nebo jiného uživatele, což umožňuje vám tooaccess hello SAP Marketplace. Projděte této posloupnosti obrazovky po přihlášení:

Přejděte příliš[Marketplace služby SAP](https://support.sap.com/en/index.html) > klikněte na tlačítko Stáhnout Software > Instalace a Upgrade > podle abecední Index > v části H – SAP HANA platformy edice > SAP HANA platformy verze 2.0 > instalace > stažení hello Následující soubory

![Stažení a instalace HANA](./media/hana-installation/image16_download_hana.PNG)

V případě ukázkový text hello, stáhli jsme SAP HANA 2.0 instalační balíčky. Na pole Azure přechod hello virtuálních počítačů rozbalte samorozbalující archivy hello do adresáře hello, jak je uvedeno níže.

![Extrahování HANA instalace](./media/hana-installation/image17_extract_hana.PNG)

Při extrahování hello archivy zkopírujte hello adresář vytvořený hello extrakce, v případě hello výše 51052030 toohello HANA velké jednotky instance do svazku /hana/shared hello do adresáře, kterou jste vytvořili.

> [!Important]
> Zkopírujte do kořenové hello hello instalační balíčky nebo spouštěcí logické jednotky, protože místo je omezený a musí používat jinými procesy také toobe.


### <a name="install-sap-hana-on-hello-hana-large-instance-unit"></a>Nainstalovat na jednotce hello HANA velké Instance SAP HANA
Pořadí tooinstall SAP HANA je nutné toolog v jako kořenové uživatele. Pouze kořenové má dost oprávnění tooinstall SAP HANA.
První věc Hello potřebujete toodo je tooset oprávnění hello adresář, který jste zkopírovali přes do/hana/sdílené. oprávnění Hello potřebovat tooset jako

```
chmod –R 744 <Installation bits folder>
```

Pokud chcete, aby tooinstall SAP HANA pomocí hello grafické instalační program, hello gtk2 balíčku musí toobe nainstalovaná na hello HANA velké instancí. Zkontrolujte, zda je nainstalována pomocí příkazu hello

```
rpm –qa | grep gtk2
```

V části Další kroky jsme jsou demonstraci hello SAP HANA instalací s grafickým uživatelským rozhraním hello. Jako další krok přejděte do instalačního adresáře hello a přejděte do adresáře hello HDB_LCM_LINUX_X86_64. Start

```
./hdblcmgui 
```
z tohoto adresáře. Nyní získávání projdete posloupnost obrazovky kde potřebujete tooprovide hello data pro instalaci hello. V případě hello ukázán jsme instalujete hello SAP HANA databázového serveru a klientské komponenty nástroje hello SAP HANA. Proto naše výběr je 'Databázi SAP HANA', jak je uvedeno níže

![Vyberte HANA v instalaci](./media/hana-installation/image18_hana_selection.PNG)

Na další obrazovce hello zvolíte možnost hello nainstalovat nový systém

![Vyberte nové instalace HANA](./media/hana-installation/image19_select_new.PNG)

Po provedení tohoto kroku je nutné tooselect mezi několik dalších součástí, které mohou být nainstalovány kromě toohello SAP HANA databázový server.

![Vyberte další součásti HANA](./media/hana-installation/image20_select_components.PNG)

Za účelem hello této dokumentace jsme zvolili hello SAP HANA klienta a hello SAP HANA Studio. Také jsme nainstalovali instanci škálování. proto hello na další obrazovce, je nutné toochoose "systém jednoho hostitele. 

![Vyberte možnost instalace škálování](./media/hana-installation/image21_single_host.PNG)

Na další obrazovce hello budete potřebovat tooprovide data

![Zadejte identifikátor SID SAP HANA](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Jako HANA systému ID (SID), je nutné tooprovide hello stejným identifikátorem SID, jako jste zadali Microsoft při řazení hello velké Instance HANA nasazení. Výběr jiný identifikátor SID umožňuje instalaci hello zdařit kvůli tooaccess oprávnění problémy v různých svazcích hello

Jako instalační adresář můžete použít hello /hana/shared adresáře. V dalším kroku hello potřebujete tooprovide hello umístění pro hello HANA datové soubory a soubory protokolu HANA hello


![Zadejte umístění protokolu HANA](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Měli byste jako dat a souborů protokolu hello svazky, které již byly dodány s hello přípojné body, které obsahují hello SID jste zvolili v hello obrazovce výběr před tuto obrazovku. Pokud hello SID došlo k neshodě s hello jeden jste zadali, hello obrazovka – předtím, přejděte zpět a upravit hello toohello hodnotu SID, které máte na hello přípojné body.

V dalším kroku hello zkontrolujte název hostitele hello a nakonec opravte ji. 

![Zkontrolujte název hostitele](./media/hana-installation/image24_review_host_name.PNG)

V dalším kroku hello je také nutné tooretrieve data dáte tooMicrosoft při řazení hello velké Instance HANA nasazení. 

![Zadejte uživatele systému UID a GID](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Je třeba tooprovide hello stejné ID uživatele systému a ID skupiny uživatelů, jako jste zadali Microsoft jako pořadí hello jednotky nasazení. Pokud selže toogive hello velmi stejná ID, hello instalaci SAP HANA na jednotce velké Instance HANA hello se nezdaří.

V následujících dvou obrazovky hello, které nezobrazují se všechna v této dokumentaci, je třeba tooprovide hello heslo pro uživatele systému hello databázi SAP HANA hello a hello heslo hello sapadm uživatele, který se používá pro hello Agent hostitele SAP, která je nainstalována jako součást instance databáze SAP HANA Hello.

Po definování hello heslo, se zobrazuje obrazovka s potvrzením, protože. Zkontrolujte všechna data hello uvedena v seznamu a pokračujte v instalaci hello. Probíhá na obrazovce, která dokumenty hello průběh instalace, například hello jeden níže

![Zkontrolovat průběh instalace](./media/hana-installation/image27_show_progress.PNG)

Když hello instalace dokončí, měli byste obrázek jako hello následující jeden

![Po dokončení instalace](./media/hana-installation/image28_install_finished.PNG)

V tomto okamžiku instance SAP HANA hello by to být až a spuštěná a připravená na použití. Musí být schopný tooconnect tooit ze SAP HANA Studio. Také zajistěte, aby kontrolovat hello systému SAP HANA nejnovějších oprav a používat tyto opravy.
























































 







 




