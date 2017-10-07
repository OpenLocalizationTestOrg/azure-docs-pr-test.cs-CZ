---
title: aaaHow funguje Site Recovery? | Dokumentace Microsoftu
description: "Tento článek přináší přehled architektury Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-azure-to-azure-architecture
ms.openlocfilehash: ff1580d0fe294148dc0c621728491e6119c3048a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-site-recovery-work-for-on-premises-infrastructure"></a>Jak Azure Site Recovery funguje u místní infrastruktury?

> [!div class="op_single_selector"]
> * [Replikace virtuálních počítačů Azure](site-recovery-azure-to-azure-architecture.md)
> * [Replikace místních počítačů](site-recovery-components.md)

Tento článek popisuje základní architekturu hello [Azure Site Recovery](site-recovery-overview.md) služby a hello komponent, které fungují pro replikaci z místní tooAzure úlohy.

Odeslat všechny komentáře dole hello v tomto článku, nebo v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="replicate-tooazure"></a>Replikovat tooAzure

Můžete replikovat a chránit hello následující na místní infrastrukturu tooAzure:

- **VMware:** Místní virtuální počítače VMware spuštěné na [podporovaném hostiteli](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Můžete replikovat virtuální počítače VMware s [podporovanými operačními systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions).
- **Hyper-V:** Místní virtuální počítače Hyper-V spuštěné na [podporovaných hostitelích](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- **Fyzické počítače:** Místní fyzické servery s Windows nebo Linuxem spuštěné v [podporovaných operačních systémech](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

## <a name="vmware-tooazure"></a>VMware tooAzure

Zde je, co potřebujete pro replikaci tooAzure virtuálních počítačů VMware.

Oblast | Komponenta | Podrobnosti
--- | --- | ---
**Konfigurační server** | Na jednom serveru pro správu (virtuální počítač VMware) jsou spuštěné všechny místní komponenty – konfigurační server, procesový server a hlavní cílový server. | konfigurační server Hello koordinuje komunikaci mezi místními a Azure a spravuje replikaci dat.
 **Procesový server:**  | Ve výchozím nastavení na konfiguračním serveru hello nainstalovaná. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odešle ji tooAzure úložiště.<br/><br/> procesový server Hello také obstará nabízenou instalaci počítače tooprotected služby Mobility hello a provádí automatického zjišťování virtuálních počítačů VMware.<br/><br/> S růstem nasazení můžete přidat další samostatný proces vyhrazené servery toohandle zvýšení svazky provoz replikace.
 **Hlavní cílový server** | Ve výchozím nastavení na konfiguračním serveru hello místně nainstalován. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Servery VMware** | Hostované virtuální počítače VMware na serverech vSphere ESXi a doporučujeme systému vCenter server toomanage hello hostitele. | Můžete přidat servery VMware, tooyour trezor služeb zotavení.<br/><br/>
**Replikované počítače** | Hello služba Mobility se nainstaluje na jednotlivých virtuálních počítačů má tooreplicate VMware. Ho může být nainstalován ručně na každém počítači, nebo pomocí nabízené instalace z hello procesový server.| -

**Obrázek 1: VMware tooAzure součásti**

![Komponenty](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Proces replikace

1. Nastavit hello nasazení, včetně Azure součásti a trezoru služeb zotavení. V trezoru hello zadejte hello replikace zdroj a cíl, nastavení konfigurace serveru hello, přidat servery VMware, vytvořte zásadu replikace, nasazení hello služby Mobility, povolit replikaci a spustit testovací převzetí služeb.
2.  Počítače zahájení replikace v souladu se zásadami replikace hello a počáteční kopii dat hello je tooAzure replikované úložiště.
4. Replikace rozdílové změny tooAzure se spustí po dokončení počáteční replikace hello. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Replikaci počítačů komunikovat s hello konfigurační server na portu 443 protokolu HTTPS příchozí pro správu replikace.
    - Replikaci počítače odesílají replikace dat toohello procesový server na portu HTTPS 9443 příchozí (může být nakonfigurován).
    - konfigurační server Hello orchestruje replikaci správy s Azure přes port HTTPS 443 odchozí.
    - Hello procesový server přijímá data ze zdrojového počítače, optimalizuje je zašifruje a odešle ji tooAzure úložiště přes port 443 odchozí.
    - Pokud povolíte více virtuálních počítačů konzistence, pak počítače v replikační skupině hello vzájemně komunikovat přes port 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud počítače běží stejné zatížení hello a je třeba toobe konzistentní.
5. Přenosy jsou replikované tooAzure úložiště veřejné koncové body, přes hello internet. Alternativně můžete použít [veřejný partnerský vztah](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) Azure ExpressRoute. Provoz replikace prostřednictvím VPN site-to-site místní lokalitě tooAzure není podporována.

**Obrázek 2: VMware tooAzure replikace**

![Rozšířené](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback"></a>Převzetí služeb při selhání a navrácení služeb po obnovení

1. Po ověření, že převzetí služeb při selhání funguje podle očekávání, můžete spustit neplánované převzetí služeb při selhání tooAzure podle potřeby. Plánované převzetí služeb není podporované.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md), toofail přes víc virtuálních počítačů.
3. V průběhu převzetí služeb při selhání se v Azure vytvoří replika virtuálního počítače. Potvrzení převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálního počítače Azure.
4. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Nastavení infrastruktury navrácení služeb po obnovení, zahájení replikace z primární toohello sekundární lokality hello hello počítač a spustit neplánované převzetí služeb při selhání z hello sekundární lokality. Po potvrzení toto převzetí služeb při selhání, data budou zpět na místní a budete potřebovat tooAzure tooenable replikace. [Další informace](site-recovery-failback-azure-to-vmware.md)

**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**

![Navrácení služeb po obnovení](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-tooazure"></a>Fyzické tooAzure

Když replikujete fyzický místní servery tooAzure, používá replikace také hello stejné součástech a procesech jako [VMware tooAzure](#vmware-replication-to-azure), ale tyto rozdíly:

- Fyzický server můžete použít pro hello konfigurační server, namísto virtuální počítače VMware
- Pro navrácení služeb po obnovení budete potřebovat místní infrastrukturu VMware. Fyzický počítač zpět tooa nelze nezdaří.

## <a name="hyper-v-tooazure"></a>TooAzure technologie Hyper-V

### <a name="replication-process"></a>Proces replikace

1. Nastavíte hello Azure součásti. Doporučujeme připravit účty úložiště a sítě ještě než začnete s nasazením Site Recovery.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:
    - Nastavení zdroje a cíle. Pokud nespravujete hostitelů Hyper-V v cloudu VMM, pro cíl hello můžete vytvořit kontejner lokality Hyper-V a přidejte tooit hostitele technologie Hyper-V. Pokud hostitele Hyper-V jsou spravovány v nástroji VMM, zdroj hello je hello cloudu VMM. cíl Hello je Azure.
    - Instalace hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Microsoft Azure hello. Pokud máte hello VMM se nainstaluje na něj zprostředkovatele a hello agent na každém hostiteli technologie Hyper-V. Pokud nemáte VMM, nenainstalují se hello zprostředkovatel a agent na každém hostiteli.
    - Můžete vytvořit zásadu replikace pro cloudového serveru VMM nebo Hyper-V Server hello. zásady Hello je použité tooall virtuální počítače umístěné na hostitelích na hello webu nebo v cloudu.
    - Povolíte replikaci pro virtuální počítače Hyper-V. V souladu s nastavením zásad replikace hello dojde k počáteční replikaci.
4. Se sledují změny dat a replikaci rozdílové změny tooAzure se spustí po dokončení počáteční replikace hello. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Při spuštění toomake testovací převzetí služeb při selhání, zda vše funguje.

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované, nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) z tooAzure místní virtuální počítače Hyper-V. Pokud spustíte plánované převzetí služeb při selhání, pak zdrojové virtuální počítače jsou vypnout tooensure nedošlo ke ztrátě dat.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) tooorchestrate převzetí služeb při selhání více počítačů.
4. Po spuštění hello převzetí služeb při selhání, musí být schopný toosee hello vytvoří replikované virtuální počítače v Azure. V případě potřeby můžete přiřadit toohello veřejnou adresu IP virtuálního počítače.
5. Potom potvrzení toostart hello převzetí služeb při selhání přístupu k hello zatížení z hello repliky virtuálního počítače Azure.
6. Až bude vaše místní lokalita opět dostupná, můžete [navrátit služby po obnovení](site-recovery-failback-from-azure-to-hyper-v.md). Ji plánované převzetí služeb při selhání z Azure toohello primární lokality. Plánované převzetí služeb, které můžete vyberte toofailback toohello stejného virtuálního počítače nebo tooan alternativní umístění a synchronizovat změny mezi Azure a místními, tooensure nedošlo ke ztrátě dat. Virtuální počítače vytvořené na místě, potvrzení převzetí služeb při selhání hello.

**Obrázek 4: Technologie Hyper-V lokality tooAzure replikace**

![Komponenty](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Obrázek 5: Technologie Hyper-V v nástroji VMM cloudů tooAzure replikace**

![Komponenty](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replicate-tooa-secondary-site"></a>Replikovat tooa sekundární lokality

Můžete replikovat hello následující tooyour sekundární lokality:

- **VMware:** Místní virtuální počítače VMware spuštěné na [podporovaném hostiteli](site-recovery-support-matrix-to-sec-site.md#on-premises-servers). Můžete replikovat virtuální počítače VMware s [podporovanými operačními systémy](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Fyzické počítače:** Místní fyzické servery s Windows nebo Linuxem spuštěné v [podporovaných operačních systémech](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Hyper-V:** Místní virtuální počítače Hyper-V spuštěné na [podporovaných hostitelích Hyper-V](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) spravované v cloudech VMM. [Podporovaní hostitelé](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="vmwarephysical-tooa-secondary-site"></a>Sekundární lokalita VMware nebo fyzický tooa

Můžete replikovat virtuální počítače VMware nebo fyzických serverů tooa sekundární lokality pomocí nástroje InMage Scout.

### <a name="components"></a>Komponenty

**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Procesový server** | Umístěný v primární lokalitě | Nasadíte ukládání do mezipaměti toohandle hello proces serveru, kompresi a optimalizaci data.<br/><br/> Také obstará nabízenou instalaci hello chcete tooprotect toomachines Unified Agent.
**Konfigurační server** | Umístěný v sekundární lokalitě | Hello konfigurační server spravuje, konfiguraci a monitorování vašeho nasazení – buď pomocí webu pro správu hello nebo konzole vContinuum hello.
**Server vContinuum** | Volitelné. Nainstalované v hello stejné umístění jako hello konfigurační server. | Poskytuje konzoli pro správu a monitorování chráněného prostředí.
**Hlavní cílový server** | V sekundární lokalitě hello | Hello hlavní cílový server uchovává replikovaná data. Přijímá data z hello procesového serveru, vytváří repliku počítače v sekundární lokalitě hello a obsahuje body pro uchovávání hello.<br/><br/> Hello počet hlavních cílových serverů, které potřebujete, závisí na hello počtu počítačů, které chcete chránit.<br/><br/> Pokud chcete toofail back toohello primární lokality, potřebujete hlavní cílový server existuje příliš. Hello Unified Agent je na tomto serveru nainstalován.
**VMware ESX/ESXi a server vCenter** |  Virtuální počítače jsou hostované na hostitelích ESX nebo ESXi. Hostitelé se spravují pomocí serveru vCenter. | Je nutné tooreplicate infrastruktury VMware virtuálních počítačů VMware.
**Virtuální počítače / fyzické servery** |  Unified Agent nainstalovaný na virtuální počítače VMware a fyzické servery, které chcete tooreplicate. | Hello agent funguje jako zprostředkovatel komunikace mezi všechny součásti hello.


### <a name="replication-process"></a>Proces replikace

1. Nastavit hello součástí serverů v každé lokalitě (konfigurace, proces, hlavní cílový) a nainstalujte hello Unified Agent na počítače, které chcete tooreplicate.
2. Po počáteční replikaci odesílá hello agent na každém počítači rozdílové replikační změny toohello procesový server.
3. Hello procesový server hello dat optimalizuje a přenese na sekundární lokalitě hello toohello hlavní cílový server. Hello konfigurační server spravuje replikační proces hello.

**Obrázek 6: VMware tooVMware replikace**

![VMware tooVMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-tooa-secondary-site"></a>Sekundární lokality tooa technologie Hyper-V

Zde je, co potřebujete pro replikaci virtuálních počítačů Hyper-V tooa sekundární lokality.


**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Server VMM** | Doporučujeme, abyste server VMM v primární lokalitě hello a po jednom v sekundární lokalitě hello | Každý server VMM musí být připojené toohello Internetu.<br/><br/> Každý server musí mít alespoň jeden privátní cloud VMM, sadou profil schopností hello technologie Hyper-V.<br/><br/> Hello zprostředkovatele Azure Site Recovery nainstalujete na hello VMM server. Hello zprostředkovatel koordinuje a orchestruje replikaci pomocí hello služba Site Recovery přes hello Internetu. Komunikace mezi hello zprostředkovatele a Azure je zabezpečená a šifrovaná.
**Server Hyper-V** |  Jeden nebo více servery Hyper-V host v hello primárních a sekundárních cloudech VMM.<br/><br/> Servery musí být připojené toohello Internetu.<br/><br/> Data se replikují mezi hello primární a sekundární servery Hyper-V hostiteli prostřednictvím hello LAN nebo VPN pomocí protokolu Kerberos nebo ověření certifikátu.  
**Virtuální počítače Hyper-V** | Umístěný na serveru hostitele technologie Hyper-V hello zdroje. | Hello zdrojový hostitelský server by měl mít aspoň jeden virtuální počítač, který má tooreplicate.

### <a name="replication-process"></a>Proces replikace

1. Nastavit hello účet Azure.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:

    - Hello replikace zdroj a cíl (primárních a sekundárních lokalit).
    - Instalace hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Microsoft Azure hello. Hello zprostředkovatele je nainstalován na serverech VMM a agenta hello na každém hostiteli technologie Hyper-V.
    - Vytvoříte zásady replikace pro zdrojový cloud VMM. zásady Hello je použité tooall virtuální počítače umístěné na hostitelích v cloudu hello.
    - Povolíte replikaci pro virtuální počítače Hyper-V. V souladu s nastavením zásad replikace hello dojde k počáteční replikaci.
4. Se sledují změny dat a replikace rozdílové změny toobegins po dokončení počáteční replikace hello. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Při spuštění toomake testovací převzetí služeb při selhání, zda vše funguje.

**Obrázek 7: VMM tooVMM replikace**

![Místní tooon místní](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback"></a>Převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánované převzetí služeb při selhání, pak zdrojové virtuální počítače jsou vypnout tooensure nedošlo ke ztrátě dat.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) tooorchestrate převzetí služeb při selhání více počítačů.
4. Pokud provádíte tooa sekundární lokality neplánované převzetí služeb při selhání, po převzetí služeb při selhání počítače hello v hello sekundárního umístění nejsou povolené pro ochranu nebo replikace. Pokud jste spustili plánované převzetí služeb při selhání, po převzetí služeb při selhání hello, jsou chráněné počítače v hello sekundárního umístění.
5. Poté potvrďte hello převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálních počítačů.
6. Při primární lokality je opět k dispozici, můžete zahájit tooreplicate zpětná replikace z primární toohello sekundární lokality hello. Zpětná replikace přináší hello virtuální počítače v chráněném stavu, ale hello sekundárního datového centra je stále aktivní umístění hello.
7. toomake hello primární lokalitu do umístění služby active hello znovu spustíte plánované převzetí služeb při selhání z sekundární tooprimary, za nímž následuje jiný zpětné replikace.


## <a name="next-steps"></a>Další kroky

- [Další informace](site-recovery-hyper-v-azure-architecture.md) o postupu replikace hello technologie Hyper-V.
- [Kontrola požadavků](site-recovery-prereq.md)
