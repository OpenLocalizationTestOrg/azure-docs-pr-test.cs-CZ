---
title: "osvědčené postupy Site Recovery aaaAzure | Microsoft Docs"
description: "Tento článek popisuje osvědčené postupy pro nasazení Azure Site Recovery."
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
ms.openlocfilehash: 288df858a0e1c1f5ad96dbe8b9dd0dc69d8f56ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-ready-toodeploy-azure-site-recovery"></a>Získat připravené toodeploy Azure Site Recovery

Tento článek popisuje, jak tooprepare pro nasazení Azure Site Recovery.

## <a name="protecting-hyper-v-virtual-machines"></a>Ochrana virtuálních počítačů Hyper-V

Pro ochranu virtuálních počítačů máte na výběr z několika možností nasazení. Můžete provádět replikaci technologie Hyper-VMs tooAzure místní nebo tooa sekundárního datacentra. Každé nasazení má specifické požadavky.

**Požadavek** | **Replikovat tooAzure (s nástrojem VMM)** | **Replikovat tooAzure virtuálních počítačů Hyper-V (bez VMM)** | **Replikovat virtuální počítače Hyper-V lokality toosecondary (s nástrojem VMM)** | **Podrobnosti**
---|---|---|---|---
**VMM** | Nástroj VMM běžící na řešení System Center 2012 R2 <br/><br/>Alespoň jeden cloud VMM obsahující minimálně jednu skupinu hostitelů VMM | Není k dispozici | Servery VMM v hello primární a sekundární weby spuštěnými na alespoň System Center 2012 SP1 s nejnovějšími aktualizacemi. <br/><br/> Alespoň jeden cloud na každém serveru VMM. Cloudy musí mít nastaven profil pro kapacity hello technologie Hyper-V.<br/><br/> Hello zdrojový cloud by měl mít aspoň jednu skupinu hostitelů VMM. | Volitelné. Nepotřebujete toohave, které jsou nasazené v pořadí tooreplicate technologie Hyper-V virtuální počítače tooAzure System Center VMM, ale v takovém případě budete potřebovat toomake se, že hello VMM server je správně nastavený. Který zahrnuje a ujistěte se, že používáte správnou verzi VMM hello a nastavit cloudy.
**Hyper-V** | Nejméně jeden hostitelský server Hyper-V v hello místní lokality, kde běží Windows Server 2012 R2 nebo novější | Alespoň jeden server technologie Hyper-V v hello zdrojové a cílové lokality běží aspoň Windows Server 2012 s nejnovějšími aktualizacemi hello nainstalovaný a připojený toohello Internetu.<br/><br/> Hello servery Hyper-V musí být v hostitelské skupině v cloudu VMM. | Alespoň jeden server technologie Hyper-V v hello zdrojové a cílové lokality běží aspoň Windows Server 2012 s nejnovějšími aktualizacemi hello nainstalovaný a připojený toohello Internetu.<br/><br/> servery Hyper-V Hello se musí nacházet ve skupině hostitelů, v cloudu VMM. |
**Virtual Machines** | Alespoň jeden virtuální počítač na serveru hostitele hello zdroj technologie Hyper-V | Alespoň jeden virtuální počítač na serveru hostitele hello technologie Hyper-V ve zdroji hello cloudu VMM | Alespoň jeden virtuální počítač na serveru hostitele hello technologie Hyper-V ve zdroji hello cloudu VMM. |  Virtuální počítače replikace tooAzure musí odpovídat požadavky virtuálního počítače Azure
**Účet Azure** | Budete potřebovat účet Azure a předplatné toohello služba Site Recovery. | Budete potřebovat účet Azure a předplatné toohello služba Site Recovery. | Není k dispozici | Pokud účet nemáte, začněte s bezplatnou zkušební verzí.
**Úložiště Azure** | Budete potřebovat předplatné pro účet úložiště Azure, které má povolenou geografickou replikací. | Budete potřebovat předplatné pro účet úložiště Azure, které má povolenou geografickou replikací. | Není k dispozici | Hello účet by měl být ve stejné oblasti jako trezor Azure Site Recovery hello hello a možné přidružit k hello stejného předplatného.
**Sítě** | Nastavení tooensure mapování sítě, aby všechny počítače, které se Překlopí na hello stejnou síť Azure můžete připojit tooeach jiných, bez ohledu na to, jsou v plánu obnovení. Kromě toho pokud bránu sítě je konfigurovat na cíli hello síť Azure, můžete virtuální počítače připojit tooother místní virtuální počítače. Pokud jste si nastavili sítě mapování pouze počítače, které převzetí služeb při selhání v hello, ke kterému se můžete připojit stejného plánu obnovení. | Není k dispozici |  <br/><br/>Nastavte tooensure mapování sítě, že virtuální počítače jsou připojené tooappropriate sítě po převzetí služeb při selhání a že jsou virtuální počítače repliky optimálně umístěny na hostitelských serverech technologie Hyper-V. Pokud nenakonfigurujete sítě mapování replikované počítače nebudou připojené tooany síť virtuálních počítačů po převzetí služeb při selhání. |  tooset si mapování sítě s VMM budete potřebovat toomake se, zda jsou správně nakonfigurovány VMM logické a sítě virtuálních počítačů.
**Zprostředkovatelé a agenti** | Během nasazování nainstalujete hello zprostředkovatele Azure Site Recovery na serverech VMM. Na serverech Hyper-V v cloudech VMM nainstalujete agenta služeb zotavení Azure hello. | Během nasazení budete instalovat hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure hello na hostitelském serveru Hyper-V hello nebo v clusteru| Během nasazování nainstalujete hello zprostředkovatele Azure Site Recovery na serverech VMM. Na serverech Hyper-V v cloudech VMM nainstalujete agenta služeb zotavení Azure hello. | Zprostředkovatelé a agenty připojit tooSite obnovení přes hello Internetu při použití šifrované připojení HTTPS. Není nutné tooadd výjimky brány firewall nebo vytvořte konkrétní proxy server pro připojení poskytovatele hello.
**Připojení k Internetu** | Pouze servery VMM hello potřebovat připojení k Internetu | Pouze hello servery hostitele technologie Hyper-V vyžadovat připojení k Internetu | Připojení k internetu je potřeba pouze na serverech VMM. | Virtuální počítače, není nutné nic na nich být nainstalovaný a Nepřipojovat přímo toohello Internetu.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>Ochrana virtuálních počítačů VMware nebo fyzických serverů

Pro ochranu virtuálních počítačů VMware nebo fyzických serverů s Windows/Linuxem máte na výběr z několika možností nasazení. Můžete je replikaci tooAzure nebo tooa sekundárního datacentra. Každé nasazení má specifické požadavky.

**Požadavek** | **Replikovat VMware virtuální počítače/fyzické servery tooAzure)** | * **Replikovat virtuální počítače VMware nebo fyzické servery toosecondary lokality**  
---|---|---
**Primární lokalita** | **Procesový server:** Vyhrazený server s Windows (fyzický nebo virtuální) | **Procesový server:** Vyhrazený server s Windows (fyzický server nebo virtuální počítač VMware)<br/><br/>  
**Místní sekundární lokalita** | Není k dispozici | **Konfigurační server:** Vyhrazený server s Windows (fyzický nebo virtuální) <br/><br/> **Hlavní cílový server:** Vyhrazený server (fyzický nebo virtuální) Konfigurace počítače s Windows tooprotect Windows nebo Linux tooprotect Linux.
**Azure** | **Předplatné**: budete potřebovat předplatné pro hello služba Site Recovery. <br/><br/> **Účet úložiště:** Budete potřebovat účet úložiště s povolenou geografickou replikací. Hello účet by měl být ve stejné oblasti jako trezor Site Recovery hello hello a možné přidružit k hello stejného předplatného. <br/><br/> **Konfigurační server**: budete potřebovat tooset až konfigurační server hello jako virtuální počítač Azure <br/><br/> **Hlavní cílový server**: budete potřebovat tooset hello hlavní cílový server jako virtuální počítač Azure <br/><br/> Konfigurace počítače s Windows tooprotect Windows nebo Linux tooprotect Linux.<br/><br/> **Virtuální síť Azure**: budete potřebovat virtuální síť Azure na které hello se nasadí konfigurační server a hlavní cílový server. Musí být v hello stejném předplatném, oblasti jako trezor Azure Site Recovery hello | Není k dispozici  
**Virtuální počítače/fyzické servery** | Alespoň jeden virtuální počítač VMware nebo fyzický server s Windows nebo Linuxem.<br/><br/>Během nasazení se nainstaluje hello služba Mobility na každý počítač| Alespoň jeden virtuální počítač VMware nebo fyzický server s Windows nebo Linuxem.<br/><br/> Během nasazení se instaluje hello Unified agent na každém počítači.




## <a name="azure-virtual-machine-requirements"></a>Požadavky na virtuální počítače Azure

Můžete nasadit Site Recovery tooreplicate virtuální počítače a fyzické servery se systémem žádný operační systém nepodporuje v Azure. To zahrnuje většinu verzí systému Windows a Linux. Budete potřebovat toomake opravdu který místně, že virtuální počítače, které chcete tooprotect v souladu s požadavky na Azure.


## <a name="optimizing-your-deployment"></a>Optimalizace nasazení

Použijte hello následující tipy toohelp optimalizace a škálovat vaše nasazení.

- **Velikost svazku operačního systému**: když replikujete hello tooAzure virtuální počítač operační systémový svazek musí být menší než 1 TB. Pokud máte víc svazků než to můžete ručně je přesunout tooa jiný disk před zahájením nasazení.
- **Kapacita disku data**: Pokud replikujete tooAzure může mít až too32 datové disky na virtuálním počítači, každý s maximálně 1 TB. Efektivně můžete replikovat a přebírat služby při selhání virtuálního počítače o velikosti přibližně 32 TB.
- **Omezení plánu obnovení**: Site Recovery můžete škálovat toothousands virtuálních počítačů. Plány obnovení jsou navržené tak, jako model pro aplikace, které by měl převzetí služeb při selhání společně, jsme omezit hello počet počítačů v too50 plánu obnovení.
- **Omezení služby Azure:** Každé předplatné Azure má přednastavena výchozí omezení počtu jader, cloudových služeb atd. Doporučujeme spustit testovací převzetí služeb při selhání toovalidate hello dostupnosti prostředků ve vašem předplatném. Tato omezení můžete upravit prostřednictvím podpory Azure.
- **Plánování kapacity:** Plánování škálování a výkonu.
- **Šířka pásma replikace:** Pokud nemáte dostatečnou šířku pásma replikace, všimněte si, že:
    - **ExpressRoute:** Site Recovery spolupracuje s Azure ExpressRoute a nástroji pro optimalizaci sítě WAN, jako je Riverbed.
    - **Provoz replikace**: používá Site Recovery provede inteligentní počáteční replikaci s využitím jenom datové bloky a hello není celý VHD. Během probíhající replikace se replikují jenom změny.
    - **Přenos v síti**: můžete řídit přenos v síti používá pro replikaci nastavení Windows QoS pomocí zásad na základě hello cílové IP adresy a portu.  Kromě toho pokud replikujete tooAzure Site Recovery pomocí agenta Azure Backup hello. můžete pro takového agenta nakonfigurovat omezování.
- **RTO**: Pokud chcete, aby toomeasure hello cíli času obnovení (RTO) můžete očekávat službou Site Recovery, doporučujeme spustit testovací převzetí služeb a zobrazení tooanalyze úlohy Site Recovery hello kolik času trvá toocomplete hello operace. Pokud jste selhání tooAzure, pro hello nejlepší RTO, doporučujeme vám, že je všechny ručně prováděné akce automatizovat integrací s Azure automation a obnovení plány.
- **Plánovaný bod obnovení**: Site Recovery podporuje cíl bodu téměř synchronní obnovení (RPO) při replikaci tooAzure. Předpokladem je dostatečná šířka pásma mezi vaším datacentrem a Azure.
