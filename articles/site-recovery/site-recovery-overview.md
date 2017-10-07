---
title: aaaWhat je Azure Site Recovery? | Dokumentace Microsoftu
description: "Poskytuje přehled hello služba Azure Site Recovery a shrnuje, scénáře nasazení."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: da6755654b8036a03314ec836f014b64428d5518
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-site-recovery"></a>Co je Site Recovery?

Služba Azure Site Recovery toohello Vítejte! Tento článek obsahuje rychlý přehled služby hello.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Provozní kontinuita a zotavení po havárii (BCDR) pomocí služby Azure Recovery Services

Organizace, je nutné toofigure na tom, jak budete tookeep bezpečné data a aplikace/úlohy běžící při plánovaných a neplánovaných výpadků provádějí.

Azure Recovery Services přispívat strategie BCDR tooyour:

- **Služba Site Recovery:** Site Recovery pomáhá zajistit kontinuitu podnikových procesů tím, že zajistí provoz aplikací na virtuálních počítačích a dostupnost fyzických serverů v případě, že je lokalita mimo provoz. Site Recovery replikuje úlohy běžící na virtuálních počítačích a fyzických serverů, aby zůstaly dostupné v sekundární umístění Pokud hello primární lokalita není dostupná. Obnoví primární lokalitu pro úlohy toohello po ji a znovu spustit.
- **Služba zálohování**: Kromě toho hello [Azure Backup](https://docs.microsoft.com/azure/backup/) služby zajišťuje dat bezpečnost a obnovitelnost zálohováním tooAzure.

Site Recovery může spravovat replikaci pro:

- Virtuální počítače Azure replikované mezi oblastmi Azure.
- Místní virtuální počítače a fyzické servery replikaci tooAzure nebo tooa sekundární lokality.


## <a name="what-does-site-recovery-provide"></a>Co Site Recovery poskytuje?

**Funkce** | **Podrobnosti**
--- | ---
**Nasazení jednoduchého řešení BCDR** | Pomocí Site Recovery, můžete nastavit a spravovat replikace, převzetí služeb při selhání a navrácení služeb po obnovení z jednoho umístění v hello portálu Azure.
**Replikace virtuálních počítačů Azure** | Můžete nastavit strategii BCDR tak, že se virtuální počítače Azure budou replikovat mezi oblastmi Azure.
**Replikace místních virtuálních počítačů mimo lokalitu** | Můžete replikovat místní virtuální počítače a fyzické servery tooAzure nebo tooa sekundární místní umístění. Replikace tooAzure eliminuje hello nákladů a složitých zachování do sekundárního datacentra.
**Replikace jakékoli úlohy** | Můžete replikovat libovolné úlohy spuštěné na podporovaných virtuálních počítačích Azure, místních virtuálních počítačích Hyper-V a VMware a na fyzických serverech s Windows nebo Linuxem.
**Zajištění odolnosti a zabezpečení dat** | Site Recovery orchestruje replikaci bez zachycování dat aplikací. Replikovaná data jsou uložena v úložišti Azure, s hello odolnost, která poskytuje. Když dojde k převzetí služeb při selhání, jsou vytvořen na základě dat hello replikovat virtuální počítače Azure.
**Splnění RTO a RPO** | Udržujte cíle plánované doby obnovení (RTO) a plánovaných bodů obnovení (RPO) v mezích limitů organizace. Site Recovery poskytuje průběžnou replikaci virtuálních počítačů Azure a VMware a umožňuje frekvenci replikací pouhých 30 sekund. Plánovanou dobu obnovení (RTO) můžete ještě snížit integrací se službou [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Zachování konzistence aplikací při převzetí služeb při selhání** | Můžete nakonfigurovat body obnovení se snímky konzistentními vzhledem k aplikacím. Snímky konzistentní vzhledem k aplikacím zachycují data na disku, veškerá data v paměti a všechny probíhající transakce.
**Testování bez výpadků** | Můžete snadno spustit testovací převzetí služeb při selhání toosupport projde obnovení po havárii, aniž by to ovlivnilo probíhající replikace.
**Spouštění flexibilních převzetí služeb při selhání** | Pro očekávané výpadky můžete spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak můžou proběhnout neplánovaná převzetí služeb při selhání s minimálními ztrátami dat (v závislosti na frekvenci replikací). Primární lokalita back tooyour můžete snadno úspěšná, když je opět k dispozici.
**Vytváření plánů obnovení** | Pomocí plánů obnovení můžete přizpůsobit a nastavit pořadí převzetí služeb při selhání a zotavení vícevrstvých aplikací na několika virtuálních počítačích. V rámci plánů seskupujete počítače a přidáváte skripty a ručně prováděné akce. Plány obnovení je možné integrovat s runbooky služby Azure Automation.
**Integrace s existujícími technologiemi BCDR** | Site Recovery se integruje s dalšími technologiemi BCDR. Můžete například použít Site Recovery tooprotect hello systému SQL Server back-end u firemních úloh, včetně nativní podpory pro SQL Server AlwaysOn, toomanage hello převzetí služeb při selhání skupiny dostupnosti.
**Integrovat knihovna automation hello** | Bohatá knihovna Azure Automation poskytuje skripty specifické pro aplikace a připravené pro produkční prostředí, které je možné stáhnout a integrovat se Site Recovery.
**Správa síťových nastavení** | Site Recovery se integruje s Azure a poskytuje jednoduchou správu aplikační sítě, včetně rezervace IP adres, konfigurace nástrojů pro vyrovnávání zatížení a integrace služby Azure Traffic Manager pro efektivní přepínání sítí.


## <a name="what-can-i-replicate"></a>Co mohu replikovat?

**Podporuje se** | **Podrobnosti**
--- | ---
**Co můžu replikovat?** | Virtuální počítače Azure mezi oblastmi Azure (ve verzi Preview)<br/><br/>  Místní virtuální počítače VMware, virtuální počítače Hyper-V, tooAzure fyzických serverů (Windows a Linux) < br /<br/> Místní virtuální počítače VMware, virtuální počítače Hyper-V, sekundární lokality tooa fyzických serverů. Pro virtuální počítače Hyper-V replikace tooa sekundární lokality je podporována pouze v případě hostitelů Hyper-V spravuje nástroj System Center VMM.
**Které oblasti jsou podporovány pro Site Recovery?** | [Podporované oblasti](https://azure.microsoft.com/regions/services/) |
**Které operační systémy replikované počítače potřebují?** | [Požadavky na virtuální počítač Azure](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)<br></br>[Požadavky na virtuální počítač VMware](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Pro virtuální počítače Hyper-V se podporuje každý [hostovaný operační systém](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows), který podporuje Azure a Hyper-V.<br/><br/> [Požadavky na fyzický server](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Jaké servery nebo hostitele VMware potřebuji?** | Virtuální počítače VMware můžou být umístěné na [podporovaných hostitelích vSphere nebo serverech vCenter](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
**Jaké úlohy můžu replikovat?** | Můžete replikovat jakoukoli úlohu běžící v podporovaném počítači replikace. Kromě toho hello Site Recovery team provedli testování specifické pro aplikace [počet aplikace](site-recovery-workload.md#workload-summary).


## <a name="azure-portal-considerations"></a>Důležité informace o webu Azure Portal

* Site Recovery je možné nasadit v hello [portál Azure](https://portal.azure.com).
* V hello portál Azure classic které můžete spravovat pomocí modelu správy hello classic služby Site Recovery.
- portál classic Hello by měly být jenom využité toomaintain existující nasazení Site Recovery. Nelze vytvořit nové trezory portálu classic hello.

## <a name="next-steps"></a>Další kroky
* Další informace o [podpoře úloh](site-recovery-workload.md)
* Začínáme s [replikace virtuálního počítače Azure mezi oblastmi](site-recovery-azure-to-azure.md), [tooAzure replikace VMware](vmware-walkthrough-overview.md), nebo [tooAzure replikace technologie Hyper-V](hyper-v-site-walkthrough-overview.md).
