---
title: "aaaReplicate nasazení Dynamics AX vícevrstvé pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate a chránit pomocí Azure Site Recovery Dynamics AX"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: b974315ec50ab2ec43846b3d3f95c7de88b72fc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a>Replikovat vícevrstvé aplikace Dynamics AX pomocí Azure Site Recovery

## <a name="overview"></a>Přehled


Aplikace Microsoft Dynamics AX je jedním z hello nejoblíbenější řešení ERP mezi podniky toostandardized proces v umístění, spravovat prostředky a zjednodušit dodržování předpisů. Considering aplikace hello je obchodní kritické tooan organizace je velmi důležité, aby toobe, pokud žádné po havárii, aplikace by spuštěná minimální hodnota času.

Dnes Microsoft Dynamics AX neposkytuje žádné možnosti obnovení po havárii se na pole. Aplikace Microsoft Dynamics AX se skládá z mnoha součásti serveru jako aplikace objektu, Active Directory (AD), SQL databázový Server, SharePoint Server, vytváření sestav atd. Server toomanage hello zotavení po havárii každou z těchto součástí je ručně nejen nákladná, ale také k chybám.

Tento článek podrobně vysvětluje o tom, jak můžete vytvořit řešení zotavení po havárii pro aplikaci Dynamics AX pomocí [Azure Site Recovery](site-recovery-overview.md). Také vysvětluje plánované/neplánované nebo testovací převzetí služeb při selhání pomocí plánu obnovení jedním kliknutím, podporované konfigurace a požadavky.
Řešení zotavení po havárii Azure Site Recovery na základě plně otestovat, certifikaci a doporučení Microsoft Dynamics AX.



## <a name="prerequisites"></a>Požadavky

Implementace zotavení po havárii pro aplikaci Dynamics AX pomocí Azure Site Recovery vyžaduje hello následující předpoklady byla dokončena.

• Nasazení Dynamics AX místní byla nastavena

• Služeb azure Site Recovery vault byla vytvořena v rámci předplatného Microsoft Azure

• Pokud je váš web obnovení Azure hello posouzení připravenosti na virtuální počítač Azure v spusťte nástroj tooensure virtuálních počítačů, které jsou kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery


## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

Za účelem hello vytvoření v tomto článku používaly virtuální počítače VMware s 2012R3 Dynamics AX na Windows Server 2012 R2 Enterprise. Replikace obnovení lokality je nezávislá na aplikace, pokud hello doporučení tady jsou očekávané toohold také následujících scénářů.

### <a name="source-and-target"></a>Zdroj a cíl

**Scénář** | **tooa sekundární lokality** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano
**Fyzický server** | Ano | Ano

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a>Povolit zotavení po Havárii Dynamics AX aplikace pomocí Azure Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Ochrana aplikace Dynamics AX
Jednotlivé komponenty hello Dynamics AX potřebám toobe chráněný tooenable hello aplikace dokončena a replikace a obnovení. Tato část zahrnuje:

**1. Ochrana služby Active Directory**

**2. Ochrana SQL vrstvy**

**3. Ochrana a vrstvy webové aplikace**

**4. Konfigurace sítě**

**5. Plán obnovení**

### <a name="1-setup-ad-and-dns-replication"></a>1. Instalační program AD a replikaci DNS

Služby Active Directory je potřeba v lokalitě hello zotavení po Havárii pro toofunction aplikace Dynamics AX. Existují dvě možnosti doporučené podle složitosti hello hello zákazníka v místním prostředí.

**Možnost 1**

Pokud má zákazník hello malý počet aplikací a jeden řadič domény pro jeho celý místní lokality a budou se přebírání služeb při selhání celý web hello společně, pak doporučujeme používat automatické obnovení systému replikace tooreplicate hello řadič domény počítače toosecondary lokality ( platí pro tooSite lokality a lokality tooAzure).

**Možnost 2**

Pokud zákazník hello má velký počet aplikací a běží doménové struktury služby Active Directory a bude převzetí služeb při selhání-několik aplikací najednou, pak doporučujeme nastavení dalšího řadiče domény v lokalitě hello zotavení po Havárii (sekundární lokality nebo v Azure).

Naleznete příliš[doprovodná příručka na zpřístupnění řadič domény v lokalitě zotavení po Havárii](site-recovery-active-directory.md). Pro zbývající část tohoto dokumentu budeme předpokládat, že řadič domény k dispozici na webu zotavení po Havárii.

### <a name="2-setup-sql-server-replication"></a>2. Nastavení replikace systému SQL Server
Podrobné technické informace o hello doporučená možnost ochrany naleznete v příručce toocompanion [vrstvy SQL](site-recovery-sql.md).

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a>3. Povolit ochranu pro Dynamics AX klienta a AOS virtuální počítače
Provést příslušné konfigurace Azure Site Recovery na tom, jestli jsou hello virtuální počítače nasazené na základě [technologie Hyper-V](site-recovery-hyper-v-site-to-azure.md) nebo na [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Doporučené tooconfigure konzistentní frekvence havárií je 15 minut.
>

Hello níže snímku se zobrazuje stav ochrany hello Dynamics součásti virtuálních počítačů v případě ochrany 'VMware lokality tooAzure'.
![Chráněné položky](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Konfigurace sítí
Konfigurace virtuálního počítače výpočty a síť nastavení

Hello AX klienta a virtuální počítače AOS konfigurace nastavení sítě v Azure Site Recovery, sítě virtuálních počítačů hello získat správného síťového zotavení po Havárii připojené toohello po převzetí služeb při selhání. Zkontrolujte, zda síť hello zotavení po Havárii pro tyto vrstvy směrovatelné toohello SQL vrstvy.

Můžete vybrat hello virtuálních počítačů v hello replikované položky tooconfigure hello síťová nastavení, jak je znázorněno níže hello snímku.

* Pro servery AOS vyberte skupinu dostupnosti správné hello.

* Pokud používáte statickou IP adresu můžete zadat hello IP, který chcete hello tootake virtuálního počítače v hello **cílová IP adresa** pole ![nastavení sítě](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)



### <a name="5-creating-a-recovery-plan"></a>5. Vytvoření plánu obnovení

V Azure Site Recovery tooautomate hello převzetí služeb při selhání procesu můžete vytvořit plán obnovení. Přidáte aplikaci vrstvy a webovou vrstvu v hello plán obnovení. Pořadí je v různých skupinách, které hello front-end vypnutí před vrstvy aplikace.

1)  Vyberte hello trezoru Azure Site Recovery ve vašem předplatném a klikněte na dlaždici plány obnovení.

2)  Klikněte na ' + plánování a zadat název obnovení.

3)  Vyberte hello 'Source' a "Target". cíl Hello může být Azure nebo sekundární lokality. V případě, že zvolíte Azure, je nutné zadat model nasazení hello

![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  Vyberte hello AOS a plán obnovení toohello klientské virtuální počítače a klikněte na tlačítko ✓.
![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/selectvms.png)


![Plán obnovení](./media/site-recovery-dynamics-ax/recoveryplan.png)

Plán obnovení hello pro aplikaci Dynamics AX můžete přizpůsobit přidáním různé kroky podle níže uvedeného postupu. Hello výše snímku ukazuje hello plán dokončení obnovení po přidání všech kroků hello.

*Pomocí následujících kroků:*

*1. SQL Server převzít kroky*

Odkazovat příliš['Řešení zotavení po Havárii serveru SQL'](site-recovery-sql.md) doprovodná příručka podrobnosti o serveru konkrétní tooSQL kroky obnovení.

*2. Převzetí služeb při selhání skupiny 1: Převzít služby virtuálních počítačů AOS hello*

Ujistěte se, že je vybraný bod obnovení hello co možná toohello databáze PIT, ale není dopředu.

*3. Skript: Nástroj pro vyrovnávání zatížení přidat (pouze E-A)* přidat skript (prostřednictvím služby Azure automation) po skupině AOS virtuální počítač se zobrazí tooadd tooit nástroje pro vyrovnávání zatížení. Tento úkol můžete použít skript toodo. Najdete v článku [jak tooadd službu Vyrovnávání zatížení pro zotavení po Havárii vícevrstvé aplikace](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)

*4. Skupiny převzetí služeb při selhání 2: Převzít služby hello AX klientské virtuální počítače.*
Selhání hello webovou vrstvu virtuálních počítačů v rámci plánu obnovení hello.


### <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb

Odkazovat too'AD řešení zotavení po Havárii ' a 'Řešení zotavení po Havárii serveru SQL' doprovodných průvodců pro konkrétní tooAD aspekty a SQL server v uvedeném pořadí během testovacího převzetí služeb při selhání.

1.  Přejděte tooAzure portál a vyberte svůj trezor Site Recovery.
2.  Klikněte na plán obnovení hello vytvořené pro Dynamics AX.
3.  Klikněte na "Testovací převzetí služeb".
4.  Vyberte hello virtuální sítě toostart hello testovací převzetí služeb při selhání procesu.
5.  Jakmile sekundární prostředí hello zapnutý, můžete provést vaše ověření.
6.  Po dokončení ověření hello se můžete vybrat ověření dokončení a hello testovací převzetí služeb při selhání prostředí bude vyčištěna.

Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) toodo testovací převzetí služeb.

### <a name="doing-a-failover"></a>Převzetím služeb

1.  Přejděte tooAzure portál a vyberte svůj trezor Site Recovery.
2.  Klikněte na plán obnovení hello vytvořené pro Dynamics AX.
3.  Klikněte na 'převzetí služeb při selhání a vyberte 'Převzetí služeb při selhání'.
4.  Vyberte hello Cílová síť a klikněte na tlačítko ✓ toostart hello převzetí služeb při selhání procesu.

Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

### <a name="perform-a-failback"></a>Provést navrácení služeb po obnovení

Odkazovat too'SQL řešení zotavení po Havárii serveru, doprovodná Příručka pro server konkrétní tooSQL aspekty během navrácení služeb po obnovení.

1.  Přejděte tooAzure portál a vyberte svůj trezor Site Recovery.
2.  Klikněte na plán obnovení hello vytvořené pro Dynamics AX.
3.  Klikněte na 'převzetí služeb při selhání a vyberte převzetí služeb při selhání.
4.  Klikněte na 'Změnu Direction'.
5.  Vyberte požadované možnosti hello - synchronizace dat a možností vytvoření virtuálního počítače
6.  Klikněte na tlačítko ✓ toostart hello 'Navrácení služeb po obnovení' proces.


Postupujte podle [v tomto návodu](site-recovery-failback-azure-to-vmware.md) při dělají navrácení služeb po obnovení.

##<a name="summary"></a>Souhrn
Pomocí Azure Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro vaši aplikaci Dynamics AX. Můžete zahájit převzetí služeb při selhání hello během několika sekund z libovolného místa v hello událostí přerušení a zprovoznění aplikace hello v minutách.

## <a name="next-steps"></a>Další kroky
Čtení [jaké úlohy mohu ochránit?](site-recovery-workload.md) toolearn informace o ochraně podnikové úlohy s Azure Site Recovery.
