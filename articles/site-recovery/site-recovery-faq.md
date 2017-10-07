---
title: "Azure Site Recovery: Časté otázky | Microsoft Docs"
description: "Tento článek popisuje oblíbených otázky o Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5cdc4bcd-b4fe-48c7-8be1-1db39bd9c078
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/22/2017
ms.author: raynew
ms.openlocfilehash: 6d0bd2475466e5745e1f084bd2267d954d624ebd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: Časté otázky (FAQ)
Tento článek obsahuje nejčastější dotazy týkající se Azure Site Recovery. Pokud po přečtení tohoto článku máte dotazy, odešlete je na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## <a name="general"></a>Obecné
### <a name="what-does-site-recovery-do"></a>K čemu Site Recovery slouží?
Site Recovery přispívá tooyour provozní kontinuitu a po havárii (BCDR) strategie, tím, že orchestruje a automatizuje replikaci virtuálních počítačů Azure mezi oblastí, místní virtuální počítače a fyzické servery tooAzure a místní tooa počítače sekundárního datacentra. [Další informace](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Co můžete chránit pomocí Site Recovery?
* **Virtuální počítače Azure**: Site Recovery dokáže replikovat jakoukoli úlohu spuštěnou na podporovaném virtuálním počítači Azure
* **Virtuální počítače Hyper-V**: jakoukoli úlohu spuštěnou na virtuálním počítači technologie Hyper-V můžete chránit pomocí Site Recovery.
* **Fyzické servery**: fyzické servery se systémem Windows nebo Linux můžete chránit pomocí Site Recovery.
* **Virtuální počítače VMware**: jakoukoli úlohu spuštěnou ve virtuálním počítači VMware můžete chránit pomocí Site Recovery.

### <a name="does-site-recovery-support-hello-azure-resource-manager-model"></a>Podporuje Site Recovery hello modelu Azure Resource Manager?
Site Recovery je k dispozici v hello portál Azure s podporou pro Resource Manager. Site Recovery podporuje starší verze nasazení v hello portál Azure classic. Nelze vytvořit nové trezory portálu classic hello a nové funkce nejsou podporovány.

### <a name="can-i-replicate-azure-vms"></a>Můžete replikovat virtuální počítače Azure?
Ano, můžete provádět replikaci mezi oblastmi Azure podporovaných virtuálních počítačích Azure. [Další informace](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-tooorchestrate-replication-with-site-recovery"></a>Co potřebuji v Hyper-V tooorchestrate replikaci pomocí Site Recovery?
Pro hostitelský server Hyper-V hello co potřebujete, závisí na scénáři nasazení hello. Podívejte se na hello požadavky technologie Hyper-V v:

* [Replikace virtuálních počítačů technologie Hyper-V (bez VMM) tooAzure](site-recovery-hyper-v-site-to-azure.md)
* [Replikace virtuálních počítačů Hyper-V (s nástrojem VMM) tooAzure](site-recovery-vmm-to-azure.md)
* [Replikace virtuálních počítačů Hyper-V tooa sekundárního datacentra](site-recovery-vmm-to-vmm.md)
* Pokud replikujete tooa sekundárního datového centra, přečtěte si informace o [podporované hostované operační systémy pro virtuální počítače Hyper-V](https://technet.microsoft.com/library/mt126277.aspx).
* Pokud replikujete tooAzure, Site Recovery podporuje všechny hello hostované operační systémy, které jsou [nepodporuje v Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Může chránit virtuální počítače, když běží technologie Hyper-V na klientský operační systém?
Ne, virtuální počítače se musí nacházet na hostitelském serveru Hyper-V, který běží na podporovaném serveru s Windows. Pokud potřebujete tooprotect klientský počítač, můžete jej replikovat jako fyzický počítač příliš[Azure](site-recovery-vmware-to-azure.md) nebo [sekundárního datacentra](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Jaké úlohy mohu ochránit pomocí Site Recovery?
Většina úloh běžících na podporovaném virtuálním počítači nebo fyzickém serveru, můžete použít tooprotect Site Recovery. Site Recovery poskytuje podporu pro aplikace s cílem replikace, takže aplikace můžou být obnovena tooan použitelného stavu. Se integruje s aplikacemi Microsoftu, například SharePoint, Exchange, Dynamics, SQL Server a služby Active Directory a úzce spolupracuje s hlavními dodavateli včetně Oracle, SAP, IBM a Red Hat. [Další informace](site-recovery-workload.md) o ochraně úloh.

### <a name="do-hyper-v-hosts-need-toobe-in-vmm-clouds"></a>Mají hostitelů Hyper-V v cloudech VMM toobe?
Pokud chcete tooreplicate tooa sekundárního datacentra, pak musí být virtuální počítače Hyper-V na Hyper-V hostuje servery umístěné v cloudu VMM. Pokud chcete tooreplicate tooAzure, můžete replikovat virtuální počítače na hostitelských serverech technologie Hyper-V s cloudy VMM i bez. [Přečtěte si další informace](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Mohu nasadit Site Recovery s VMM, když mám jen jeden server VMM?

Ano. Můžete buď replikovat virtuální počítače na serverech Hyper-V v tooAzure cloudu VMM hello nebo můžete replikovat mezi cloudy VMM na hello stejný server. Pro replikaci mezi lokálními tooon místní doporučujeme mít VMM server v obou hello primárních a sekundárních lokalit.  

### <a name="what-physical-servers-can-i-protect"></a>Jaké fyzické servery mohu ochránit?
Můžete replikovat fyzické servery se systémem Windows a Linux tooAzure nebo tooa sekundární lokality. [Další informace o](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) požadavky na operační systém.  Dobrý den, platí stejné požadavky zda replikujete tooAzure fyzických serverech nebo tooa sekundární lokality.


Všimněte si, že fyzické servery se spustí jako virtuální počítače v Azure případě výpadku na místním serveru. Navrácení služeb po obnovení tooan místní fyzický server není aktuálně podporována. Pro chráněný jako fyzický počítač můžete pouze navrácení služeb po obnovení tooa virtuálního počítače VMware.

### <a name="what-vmware-vms-can-i-protect"></a>Jaké virtuální počítače VMware mohu ochránit?

tooprotect virtuální počítače VMware budete potřebovat vSphere hypervisor a virtuální počítače běžící nástroje VMware. Doporučujeme také, že máte hello hypervisory VMware vCenter server toomanage. [Další informace](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) o přesných požadavcích na replikaci serverů VMware a virtuálních počítačů tooAzure nebo tooa sekundární lokality.


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Mohu pomocí Site Recovery spravovat zotavení po havárii pro pobočky?
Ano. Pokud používáte Site Recovery tooorchestrate replikace a převzetí služeb při selhání ve firemních pobočkách, získáte jednotná orchestration a zobrazit všechny úlohy poboček v centrálním umístění. Snadno můžete spustit převzetí služeb při selhání a spravovat zotavení po havárii všechny větve z centrály, aniž byste museli navštívit hello větve.

## <a name="pricing"></a>Ceny

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>Jaké poplatky dojít při použití Azure Site Recovery?
Pokud používáte Site Recovery, platit poplatky hello Site Recovery licence, úložiště Azure, transakce úložiště a odchozí přenosy dat. [Další informace](https://azure.microsoft.com/pricing/details/site-recovery).

licence Hello Site Recovery je na chráněný instanci, kde je instance virtuálního počítače nebo fyzického serveru.

- Pokud disk virtuálního počítače replikuje tooa standardní účet úložiště, je hello poplatků úložiště Azure pro hello spotřebu úložiště. Například pokud je velikost disku zdroje hello se používá 1 TB a 400 GB, Site Recovery vytvoří 1 TB virtuální pevný disk v Azure, ale úložiště hello účtovat je 400 GB (plus hello množství prostoru úložiště pro protokoly replikace).
- Pokud disk virtuálního počítače replikuje tooa prémiový účet úložiště, je hello poplatků úložiště Azure pro velikost úložiště hello zřízený, zaokrouhlí pro hello nejbližší možnost disk úložiště premium. Pokud velikost zdrojového disku hello 50 GB, Site Recovery vytvoří 50 GB disk v Azure a Azure mapuje tento toohello nejbližší disku úložiště premium (P10).  Počítá náklady na P10 a ne na velikost disku hello 50 GB.  [Další informace](https://aka.ms/premium-storage-pricing).  Pokud používáte storage úrovně premium, účet standardního úložiště pro replikaci protokolování je také nutný a také se fakturuje hello množství místa standardní úložiště pro tyto protokoly.
- Žádné disky jsou vytvořeny do testovací převzetí služeb nebo převzetí služeb při selhání. Ve stavu replikace hello, úložiště poplatky v kategorii hello "Objekt blob stránky a disku" podle hello [cenové kalkulačky úložiště](https://azure.microsoft.com/en-in/pricing/calculator/) se vám neúčtují. Tyto poplatky jsou založené na hello úložiště typu premium nebo standard a hello redundanci dat zadejte - LRS, GRS, RA-GRS atd.
- Pokud hello možnost toouse spravovaných disků na je vybraná převzetí služeb při selhání, [poplatky za spravované disky](https://azure.microsoft.com/en-in/pricing/details/managed-disks/) použije po převzetí služeb při selhání nebo testovací převzetí služeb. Spravovat disky, které poplatky neuplatnění během replikace.
- Pokud není vybraná hello možnost toouse spravovaných disků na převzetí služeb při selhání, úložiště poplatky v kategorii hello "Objekt blob stránky a disku" podle hello [cenové kalkulačky úložiště](https://azure.microsoft.com/en-in/pricing/calculator/) se vám neúčtují po převzetí služeb při selhání. Tyto poplatky jsou založené na hello úložiště typu premium nebo standard a hello redundanci dat zadejte - LRS, GRS, RA-GRS atd.
- Transakce úložiště po selhání účtujeme během stabilního stavu replikace a pro standardních operací virtuálního počítače nebo testovací převzetí služeb při selhání. Ale tyto poplatky jsou nepatrné.

Náklady jsou také vzniklých během převzetí služeb při selhání, kde bude použito náklady na virtuální počítač, úložiště, výstupní a úložiště transakce hello.



## <a name="security"></a>Zabezpečení
### <a name="is-replication-data-sent-toohello-site-recovery-service"></a>Posílají se replikační data toohello služba Site Recovery?
Ne, Site Recovery nemá nezachycuje replikovaná data a nemá žádné informace o co běží na virtuálních počítačích nebo fyzických serverů.
Replikační data se vyměňují mezi lokálními hostiteli Hyper-V, hypervisory VMware nebo fyzickými servery a úložištěm Azure nebo sekundární lokalitou. Site Recovery nemá žádná možnost toointercept tato data. Pouze metadata hello potřeby tooorchestrate replikace a převzetí služeb při selhání je odeslána toohello služba Site Recovery.  

Site Recovery je ISO 27001: 2013, 27018, HIPAA, DPA certifikaci a je v procesu hello SOC2 a FedRAMP JAB hodnocení.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-hello-same-geographic-region-can-site-recovery-help-us"></a>Kvůli dodržování předpisů musí zůstat i naše metadata místně v rámci hello stejné zeměpisné oblasti. Site Recovery, budeme moct snadněji?
Ano. Když vytvoříte trezor Site Recovery v oblasti, je zajištěno, že všechna metadata, která budeme potřebovat tooenable a orchestraci replikace a převzetí služeb při selhání zůstávají uvnitř této oblasti je geografické hranice.

### <a name="does-site-recovery-encrypt-replication"></a>Šifruje Site Recovery replikaci?
Pro virtuální počítače a fyzické servery je podporováno replikaci mezi místními lokalitami šifrování během přenosu. Pro virtuální počítače a fyzické servery replikace tooAzure, jak šifrování během přenosu a [šifrování na rest (v Azure)](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) jsou podporovány.

## <a name="replication"></a>Replikace

### <a name="can-i-replicate-over-a-site-to-site-vpn-tooazure"></a>Mohu replikovat přes tooAzure VPN site-to-site?
Azure Site Recovery replikuje data účtu úložiště Azure tooan, přes veřejný koncový bod. Replikace není přes síť site-to-site VPN. Síť site-to-site VPN, můžete vytvořit pomocí virtuální sítě Azure. To není v konfliktu se Site Recovery replikace.

### <a name="can-i-use-expressroute-tooreplicate-virtual-machines-tooazure"></a>Můžete použít tooAzure virtuální počítače tooreplicate ExpressRoute?
Ano, ExpressRoute může být tooAzure použité tooreplicate virtuálních počítačů. Azure Site Recovery replikuje data tooan účet úložiště Azure přes veřejný koncový bod. Je třeba tooset až [veřejného partnerského vztahu](../expressroute/expressroute-circuit-peerings.md#public-peering) toouse ExpressRoute pro replikaci Site Recovery. Po selhaly hello virtuálních počítačů přes tooan virtuální síť Azure se dostanete pomocí hello [soukromého partnerského vztahu](../expressroute/expressroute-circuit-peerings.md#private-peering) nastavení s hello virtuální síť Azure.

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-tooazure"></a>Jsou všechny požadavky na replikaci virtuálních počítačů tooAzure?
Virtuální počítače, které chcete tooreplicate tooAzure, by měly splňovat [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-tooazure"></a>Můžete replikovat tooAzure virtuální počítače 2. generace technologie Hyper-V?
Ano. Site Recovery převede z toogeneration 2. generace 1 během převzetí služeb při selhání. Při navrácení služeb po obnovení hello počítač je převeden zpět toogeneration 2. [Přečtěte si další informace](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-tooazure-how-do-i-pay-for-azure-vms"></a>Pokud replikuji tooAzure, jak je platit pro virtuální počítače Azure?
Během běžné replikace se data jsou replikované toogeo redundantního úložiště Azure a nepotřebujete toopay žádné poplatky za virtuální počítače Azure IaaS poskytuje významné výhody. Při spuštění tooAzure převzetí služeb při selhání, Site Recovery automaticky vytvoří virtuální počítače Azure IaaS a následně se vám budou fakturovat pro hello výpočetní prostředky, které v Azure spotřebujete.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Můžete automatizovat scénáře obnovení lokality pomocí sady SDK?
Ano. Je možné automatizovat postupy workflow Site Recovery pomocí hello Rest API, Powershellu nebo hello Azure SDK. Aktuálně podporované scénáře pro nasazení Site Recovery pomocí prostředí PowerShell:

* [Replikace virtuálních počítačů technologie Hyper-V v VMMs cloudy tooAzure správce prostředků PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
* [Replikace virtuálních počítačů technologie Hyper-V bez tooAzure VMM správce prostředků PowerShell](site-recovery-deploy-with-powershell-resource-manager.md)

### <a name="if-i-replicate-tooazure-what-kind-of-storage-account-do-i-need"></a>Pokud replikuji tooAzure jaký typ účtu úložiště potřebuji?
* **Portál Azure classic**: Pokud nasazujete Site Recovery v hello portál Azure classic, budete potřebovat [standardním geograficky redundantním úložištěm účet](../storage/common/storage-redundancy.md#geo-redundant-storage). Storage úrovně Premium se v tuto chvíli nepodporuje. Hello účet musí být v hello stejné oblasti jako trezor Site Recovery hello.
* **Portál Azure**: Pokud nasazujete hello portálu Azure Site Recovery, budete potřebovat účet úložiště LRS nebo GRS. Doporučujeme, abyste GRS, aby byla zajištěna odolnost dat, pokud oblastního výpadku nebo pokud není možné obnovit primární oblast hello. Hello účet musí být v hello stejné oblasti jako hello trezoru služeb zotavení. Storage úrovně Premium se teď podporuje pro virtuální počítač VMware, virtuální počítač Hyper-V a replikaci fyzický server, když nasadíte v hello portálu Azure Site Recovery.

### <a name="how-often-can-i-replicate-data"></a>Jak často je možné replikovat data?
* **Technologie Hyper-V:** virtuálních počítačů Hyper-V je možné replikovat každých 30 sekund (s výjimkou storage úrovně premium), 5 minut nebo 15 minut. Pokud jste nastavili replikaci sítě SAN je replikace synchronní.
* **VMware a fyzické servery:** Četnost replikací zde není relevantní. Replikace je souvislý.

### <a name="can-i-extend-replication-from-existing-recovery-site-tooanother-tertiary-site"></a>Můžete rozšířit replikaci z existující lokality tooanother terciární lokality pro obnovení?
Rozšířená nebo zřetězená replikace není podporována. Tato funkce v žádosti o [fóru pro zpětnou vazbu](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-replication-hello-first-time-i-replicate-tooazure"></a>Mohu replikaci offline hello poprvé replikaci tooAzure?
Toto není podporováno. Žádosti o tuto funkci v hello [fóru pro zpětnou vazbu](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Mohu z replikace vyloučit konkrétní disky?
To je podporováno, když jste [replikovat virtuální počítače VMware a virtuálních počítačů Hyper-V](site-recovery-exclude-disk.md) tooAzure pomocí hello portálu Azure.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Můžete replikovat virtuální počítače s dynamickými disky?
Dynamické disky jsou podporovány, pokud se provádí replikace virtuálních počítačů Hyper-V. Podporovány jsou i při replikaci virtuálních počítačů VMware a fyzické počítače tooAzure. Hello disk operačního systému musí být základní disk.

### <a name="can-i-add-a-new-machine-tooan-existing-replication-group"></a>Můžete přidat nové počítače tooan existující replikační skupiny?
Přidání nové skupiny replikace tooexisting počítače je podporováno. toodo tedy vyberte hello replikační skupiny (v okně "Replikované položky") a klikněte pravým tlačítkem nebo vyberte kontextovou nabídku na hello replikační skupinu a vyberte příslušnou možnost hello.

![Přidat skupinu tooreplication](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Je možné omezovat šířku pásma vyhrazenou pro přenosy replikace Hyper-V?
Ano. Další informace o omezování šířky pásma v článcích hello nasazení:

* [Plánování kapacity pro replikaci virtuálních počítačů VMware a fyzické servery](site-recovery-plan-capacity-vmware.md)
* [Plánování kapacity pro replikaci virtuálních počítačů Hyper-V v cloudech VMM](site-recovery-vmm-to-azure.md#capacity-planning)
* [Plánování kapacity pro replikaci virtuálních počítačů technologie Hyper-V bez VMM](site-recovery-hyper-v-site-to-azure.md)

## <a name="failover"></a>Převzetí služeb při selhání
### <a name="if-im-failing-over-tooazure-how-do-i-access-hello-azure-virtual-machines-after-failover"></a>Pokud se I selhání tooAzure, jak lze získat přístup hello Azure virtuální počítače po převzetí služeb při selhání?
Můžete přistupovat hello virtuálních počítačích Azure přes zabezpečené internetové připojení, přes síť site-to-site VPN nebo přes Azure ExpressRoute. Budete potřebovat tooprepare počet akcí v pořadí tooconnect. [Další informace](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-tooazure-how-does-azure-make-sure-my-data-is-resilient"></a>Pokud I převzetí služeb při selhání tooAzure jak Azure Ujistěte se, je odolný svá data?
Služba Azure je pro odolnost navržena. Obnovení lokality je již vytvořena pro převzetí služeb při selhání tooa sekundární datacentrum Azure, v souladu s hello, ke které dojde smlouva Azure SLA. Pokud potřebujete hello. Pokud se to stane, zajišťujeme, že metadata a trezory zůstávají ve hello stejné zeměpisné oblasti, kterou jste zvolili pro svůj trezor.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Pokud replikuji mezi dvěma datovými centry co se stane, když dojde k nečekanému výpadku mé primární datové centrum?
Můžete aktivovat neplánované převzetí služeb při selhání z hello sekundární lokality. Site Recovery nepotřebuje připojení z hello primární lokality tooperform hello převzetí služeb při selhání.

### <a name="is-failover-automatic"></a>Je převzetí služeb při selhání automatické?
Převzetí služeb při selhání není automatické. Zahajte převzetí služeb při selhání s jedním kliknutím hello portálu, nebo můžete použít [PowerShell obnovení lokality](/powershell/module/azurerm.siterecovery) tootrigger převzetí služeb při selhání. Navrácení služeb po obnovení je jednoduchý akce na portálu Site Recovery hello.

tooautomate, můžete pomocí místního Orchestratoru nebo Operations Manageru toodetect selhání virtuálního počítače, a potom pomocí převzetí služeb při selhání hello aktivační událost hello SDK.

* [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.
* [Další informace](site-recovery-failover.md) o převzetí služeb při selhání.
* [Další informace](site-recovery-failback-azure-to-vmware.md) o selhání zálohovat virtuální počítače VMware a fyzické servery

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-tooa-different-host"></a>Pokud není Moje místní hostitel neodpovídá nebo zhroucené, se dá převzetí služeb při selhání jiného hostitele back tooa?
Ano, můžete použít hello alternativní umístění pro obnovení toofailback tooa jiného hostitele z Azure. Číst informace o možnostech hello v hello pod odkazy pro virtuální počítače VMware a Hyper-v.

* [U virtuálních počítačů VMware](site-recovery-how-to-failback-azure-to-vmware.md#fail-back-to-the-original-or-alternate-location)
* [Pro virtuální počítače Hyper-v](site-recovery-failback-from-azure-to-hyper-v.md#failback-to-an-alternate-location)

## <a name="service-providers"></a>Poskytovatelé služeb
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Jsem poskytovatel služeb. Funguje Site Recovery pro vyhrazené tak sdílené modely infrastruktury?
Ano, Site Recovery podporuje jak vyhrazené, tak sdílené modely infrastruktury.

### <a name="for-a-service-provider-is-hello-identity-of-my-tenant-shared-with-hello-site-recovery-service"></a>Pro poskytovatele služeb je identita hello klienta sdílet s hello služba Site Recovery?
Ne. Zůstane anonymní identity klienta. Vaši klienti nepotřebují přístup k portálu Site Recovery toohello. Pouze hello správce poskytovatele služeb pracuje s hello portálu.

### <a name="will-tenant-application-data-ever-go-tooazure"></a>Data aplikací klienta někdy přejde tooAzure?
Při replikaci mezi lokalitami ve vlastnictví poskytovatele služeb, data aplikací nikdy nepřenášejí tooAzure. Data se šifrují během přenosu a replikují se přímo mezi lokalitami poskytovatele služeb hello.

Pokud replikujete tooAzure, data aplikací se posílají tooAzure úložiště, ale není toohello služba Site Recovery. Data jsou šifrovaná na cestě a v Azure zůstávají zašifrovaná.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Obdrží mí klienti fakturu za služby Azure?
Ne. Azure má fakturační vztah přímo s poskytovatele služeb hello je. Za generování konkrétních faktur pro své klienty má plnou odpovědnost poskytovatel služeb.

### <a name="if-im-replicating-tooazure-do-we-need-toorun-virtual-machines-in-azure-at-all-times"></a>Pokud replikuji tooAzure, potřebujeme toorun virtuálních počítačů v Azure za všech okolností?
Ne, Data jsou replikované tooan účtu úložiště Azure v rámci vašeho předplatného. Když provedete testovací převzetí služeb při selhání (rutina pro zotavení po havárii) nebo skutečné převzetí, Site Recovery ve vašem předplatném automaticky vytvoří virtuální počítače.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-tooazure"></a>Zajistíte izolaci na úrovni klienta při replikaci tooAzure?
Ano.

### <a name="what-platforms-do-you-currently-support"></a>Jaké platformy aktuálně podporujete?
Podporujeme sady Azure Cloud Platform System a System Center na základě nasazení (2012 a vyšší). [Další informace](https://technet.microsoft.com/library/dn850370.aspx) informace o integraci sady Azure a Site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Podporujete nasazení s jediným Azure Packem a jediným serverem VMM?
Ano, můžete replikovat tooAzure virtuální počítače Hyper-V, nebo mezi lokalitami poskytovatele služeb.  Všimněte si, že pokud budete replikovat mezi lokalitami poskytovatele služeb, integrace runbooku Azure není k dispozici.

## <a name="next-steps"></a>Další kroky
* Čtení hello [přehled Site Recovery](site-recovery-overview.md)
* Seznamte se s [architekturou Site Recovery](site-recovery-components.md).  
