---
title: "aaaProtect nasazení vícevrstvé SAP NetWeaver aplikace pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooprotect SAP NetWeaver nasazení aplikace pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: manayar
ms.openlocfilehash: 34651c7b14d23a44005372f4f923c401e0224231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Ochrana nasazení vícevrstvé SAP NetWeaver aplikace pomocí Azure Site Recovery

Většina nasazení SAP velké a střední má určitou formu řešení zotavení po havárii.  důležitost Hello robustní a možností intenzivního testování řešení zotavení po havárii zvýšilo další základní obchodní procesy jsou během pohybu tooapplications například SAP.  Azure Site Recovery byl otestovaný a integrované se službou aplikace SAP, překračuje hello možnosti většina řešení zotavení po havárii v místě, na nižší celkové náklady na vlastnictví (TCO) než konkurenční řešení.
S Azure Site Recovery můžete:
* Povolení ochrany SAP NetWeaver a NetWeaver produkční aplikací běžících na místních počítačích nastavením replikace tooAzure součásti.
* Povolení ochrany SAP NetWeaver a NetWeaver produkční aplikací běžících Azure, nastavením replikace součásti tooanother datového centra Azure.
* Zjednodušení migrace na cloud pomocí Site Recovery toomigrate vaše tooAzure nasazení SAP.
* Zjednodušení upgradů, testování a vytváření prototypů projektů SAP pomocí vytváření produkčního klonu na vyžádání pro účely testování aplikací SAP.

Tento článek popisuje, jak tooprotect SAP NetWeaver nasazení aplikací pomocí [Azure Site Recovery](site-recovery-overview.md). Tento článek se zabývá hello osvědčené postupy pro ochrana nasazení SAP NetWeaver třívrstvá v Azure nastavením replikace tooanother datové centrum Azure pomocí Azure Site Recovery hello Podporované scénáře a konfigurace, a jak se tooperform převzetí služeb při selhání i testování převzetí služeb při selhání (podrobněji obnovení po havárii) a skutečné převzetí služeb při selhání.


## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že rozumíte hello následující:

1. [Replikaci virtuálního počítače tooAzure](azure-to-azure-walkthrough-enable-replication.md)
2. Jak příliš[návrh k síti pro obnovení](site-recovery-azure-to-azure-networking-guidance.md)
3. [Provádění tooAzure testovací převzetí služeb při selhání](azure-to-azure-walkthrough-test-failover.md)
4. [Provádění tooAzure převzetí služeb při selhání](site-recovery-failover.md)
5. Jak příliš[replikace řadiče domény](site-recovery-active-directory.md)
6. Jak příliš[replikaci systému SQL Server](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Podporované scénáře
S Azure Site Recovery můžete implementovat řešení zotavení po havárii pro hello následující scénáře:
* SAP systémy s operačním systémem v jedné datové centrum Azure replikace tooanother datové centrum Azure (Azure do Azure DR), jak je navržen [zde](https://aka.ms/asr-a2a-architecture).
* SAP systémy s operačním systémem na serverech VMWare (nebo fyzický) na místní replikaci lokality tooa zotavení po Havárii v datovém centru Azure (VMware do Azure DR), který vyžaduje další součásti, jako je navržen [zde](https://aka.ms/asr-v2a-architecture).
* SAP systémy s operačním systémem v technologii Hyper-V na místní replikaci lokality tooa zotavení po Havárii v datovém centru Azure (Hyper-V-do Azure DR), který vyžaduje další součásti, jako je navržen [zde](https://aka.ms/asr-h2a-architecture).

Tento dokument používá možnosti obnovení po havárii SAP hello první případu - Azure-Azure DR - toodemonstrate Azure Site Recovery. Azure Site Recovery replikace je nezávislá na aplikace, je hello proces popsaný očekávané toohold i v jiných případech.

### <a name="required-foundation-services"></a>Požadované foundation services
Tento scénář dokumentace všechny byla nasazena s hello následující nasazenou foundation services:
* ExpressRoute nebo Site-to-Site virtuální privátní síť (VPN)
* Alespoň jeden řadič domény služby Active Directory a DNS serveru běží v Azure

Doporučuje se, že tuto infrastrukturu hello výše je navázáno předchozí toodeploying Azure Site Recovery.


## <a name="typical-sap-application-deployment"></a>Typické nasazení aplikace SAP
Velké SAP zákazníci obvykle nasazovat mezi 6 too20 jednotlivých aplikací SAP.  Většina těchto aplikací jsou založeny na moduly hello SAP NetWeaver ABAP nebo Java.  Podpora tyto základní NetWeaver aplikace jsou mnoho modulů menší samostatné konkrétní jiný - NetWeaver SAP a obvykle některé aplikace bez SAP.  

Je důležité tooinventory všechny hello SAP aplikace spuštěné v šířku a toodetermine hello nasazení režim (vrstvy 2 nebo 3 úrovně), verze, opravy, velikosti, změn rychlostí a trvalost na disku.

![Vzor nasazení](./media/site-recovery-sap/sap-typical-deployment.png)

vrstvu trvalosti databázi SAP Hello by měly být chráněné prostřednictvím hello nativního databázového systému nástrojů, jako je SQL Server AlwaysOn, Oracle DataGuard nebo HANA systému replikace. vrstva Hello klienta není také chráněný službou Azure Site Recovery, ale je důležité tooconsider témata, která mít vliv na tuto vrstvu například zpoždění šíření záznamů DNS, zabezpečení a vzdáleného přístupu toohello datacenter zotavení po Havárii.

Azure Site Recovery je doporučené řešení pro hello aplikační vrstvu, včetně (A) SCS hello. Jiné aplikace, jako je aplikace NetWeaver SAP a aplikace bez SAP součástí hello celkové SAP prostředí nasazení a taky by měly být chráněné službou Azure Site Recovery.

## <a name="replicate-virtual-machines"></a>Replikace virtuálních počítačů
Postupujte podle [v tomto návodu](azure-to-azure-walkthrough-enable-replication.md) toostart replikace všech hello SAP aplikace virtuální počítače toohello datové centrum Azure zotavení po Havárii.

Pokud používáte statickou IP adresu, můžete zadat hello IP, který chcete hello tootake virtuálního počítače v části o síťové rozhraní karty hello v nastavení výpočtů a sítě.

![Cílové IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Vytvoření plánu obnovení
Plán obnovení umožňuje pořadí převzetí služeb při selhání hello v různých vrstvách vícevrstvé aplikace, proto zachování konzistence aplikace. Pomocí postupu hello [sem](site-recovery-create-recovery-plans.md) při vytváření plánu obnovení vícevrstvých webových aplikací.

### <a name="adding-scripts-toohello-recovery-plan"></a>Přidávání skriptů toohello plánu obnovení
Může být nutné toodo některé operace na hello virtuální počítače Azure post převzetí služeb při selhání a testovací převzetí služeb při selhání pro vaše aplikace toofunction správně. Je možné automatizovat převzetí služeb při selhání operaci post hello například aktualizaci položky DNS a změna vazby a připojení, jak je popsáno v přidáním příslušných skriptů v plánu obnovení hello [v tomto článku](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>Aktualizace DNS.
Pokud hello DNS je nakonfigurován pro dynamické aktualizace DNS, pak po spuštění virtuálních počítačů obvykle aktualizovat hello DNS s novou IP Adresou hello. Pokud chcete tooadd explicitní krok tooupdate DNS s hello nové IP adresy virtuálních počítačů hello přidejte to [skriptu tooupdate IP ve službě DNS](https://aka.ms/asr-dns-update) jako akce post na skupiny plánu obnovení.  

## <a name="example-azure-to-azure-deployment"></a>Příklad nasazení Azure do Azure
V diagramu hello níže hello Azure Site Recovery Azure do Azure DR scénář je znázorněn:
* Hongkong (Azure východní Asie) je Hello primární datové centrum se Singapur (Azure jihovýchodní Asie) a hello datacenter zotavení po Havárii.  V tomto scénáři se tak, že dva virtuální počítače se systémem SQL Server AlwaysOn v synchronním režimu v Singapur poskytuje místní vysokou dostupnost.
* Hello ASC sdílené složky souboru lze použít tooprovide HA pro hello SAP jediný bod selhání systému. ASC sdílené složky souboru nevyžaduje sdíleného disku clusteru a aplikace, jako je SIOS nejsou požadovány.
* Ochrany DR pro vrstvu databázového systému hello je dosaženo pomocí asynchronní replikaci.
* Tento scénář popisuje "symetrický DR" – termín, který se používá toodescribe řešení zotavení po Havárii, které je repliku přesný provozních, proto hello řešení zotavení po Havárii SQL serveru má místní vysokou dostupnost. není povinné pro databázové vrstvě hello Hello použití symetrický zotavení po Havárii a flexibilitu hello cloudové nasazení toobuild místní vysoké dostupnosti uzlu mnoho zákazníků rychle využít po události zotavení po Havárii.
* Hello diagram znázorňuje hello SAP NetWeaver ASC a aplikační server vrstvu replikovat pomocí Azure Site Recovery.

![Scénář replikace](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb
Postupujte podle [v tomto návodu](azure-to-azure-walkthrough-test-failover.md) toodo testovací převzetí služeb.

1.  Přejděte tooAzure portál a vyberte svůj trezor služeb zotavení.
2.  Klikněte na plán obnovení hello vytvořili pro SAP aplikace (s).
3.  Klikněte na "Testovací převzetí služeb".
4.  Vyberte bod obnovení a virtuální síť Azure toostart hello testovací převzetí služeb při selhání procesu.
5.  Jakmile sekundární prostředí hello zapnutý, můžete provést vaše ověření.
6.  Po dokončení ověření hello se klikněte na 'vyčistit testovací převzetí služeb při selhání a převzetí služeb při selhání tooclean hello prostředí.

## <a name="doing-a-failover"></a>Převzetím služeb
Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

1.  Přejděte tooAzure portál a vyberte svůj trezor služeb zotavení.
2.  Klikněte na plán obnovení hello vytvořené pro SAP aplikace.
3.  Klikněte na 'Převzetí služeb při selhání'.
4.  Vyberte proces obnovení bodu toostart hello převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
Další informace o vytváření řešení zotavení po havárii pro nasazení SAP NetWeaver pomocí Azure Site Recovery v [tento dokument White Paper](http://aka.ms/asr-sap). dokument White Paper Hello také popisuje doporučení pro různé architektury SAP, jsou uvedené podporované aplikací a typů virtuálních počítačů pro SAP v Azure a popisuje možné testování plány pro vaše řešení pro zotavení po havárii.

Další informace o [replikace dalších zatížení](site-recovery-workload.md) pomocí Site Recovery.
