---
title: "Replikovat pomocí Azure Site Recovery nasazení Dynamics AX vícevrstvé | Microsoft Docs"
description: "Tento článek popisuje, jak se budou replikovat a chránit pomocí Azure Site Recovery Dynamics AX"
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
ms.openlocfilehash: 03127c8f4841b67436c4819628319705af0b2cd5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a>Replikovat vícevrstvé aplikace Dynamics AX pomocí Azure Site Recovery

## <a name="overview"></a>Přehled


Aplikace Microsoft Dynamics AX je jedním z nejoblíbenějších řešení ERP mezi podnikům standardizované proces v umístění, spravovat prostředky a zjednodušit dodržování předpisů. Considering aplikace je důležité pro organizaci obchodní je velmi důležité mít jistotu, že pokud žádné po havárii, aplikace musí být spuštěná ve minimální hodnota času.

Dnes Microsoft Dynamics AX neposkytuje žádné možnosti obnovení po havárii se na pole. Aplikace Microsoft Dynamics AX se skládá z mnoha součásti serveru jako aplikace objektu, Active Directory (AD), SQL databázový Server, SharePoint Server sestav serveru atd. Ke správě po havárii je obnovení každou z těchto součástí ručně pouze nákladná, ale také k chybám.

Tento článek podrobně vysvětluje o tom, jak můžete vytvořit řešení zotavení po havárii pro aplikaci Dynamics AX pomocí [Azure Site Recovery](site-recovery-overview.md). Také vysvětluje plánované/neplánované nebo testovací převzetí služeb při selhání pomocí plánu obnovení jedním kliknutím, podporované konfigurace a požadavky.
Řešení zotavení po havárii Azure Site Recovery na základě plně otestovat, certifikaci a doporučení Microsoft Dynamics AX.



## <a name="prerequisites"></a>Požadavky

Implementace zotavení po havárii pro aplikaci Dynamics AX pomocí Azure Site Recovery vyžaduje následující požadavky byla dokončena.

• Nasazení Dynamics AX místní byla nastavena

• Služeb azure Site Recovery vault byla vytvořena v rámci předplatného Microsoft Azure

• Pokud je váš web obnovení Azure spustit nástroj hodnocení připravenosti virtuální počítač Azure na virtuálních počítačích zajistit, aby byly kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery


## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

Pro účely vytváření tohoto článku, používaly virtuální počítače VMware s 2012R3 Dynamics AX na Windows Server 2012 R2 Enterprise. Replikace obnovení lokality je nezávislá na aplikace, doporučení uvedená tady se očekává počkejte také následující scénáře.

### <a name="source-and-target"></a>Zdroj a cíl

**Scénář** | **Sekundární lokality** | **Do Azure**
--- | --- | ---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano
**Fyzický server** | Ano | Ano

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a>Povolit zotavení po Havárii Dynamics AX aplikace pomocí Azure Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Ochrana aplikace Dynamics AX
Jednotlivé komponenty Dynamics AX je potřeba chránit povolit aplikace dokončena a replikace a obnovení. Tato část zahrnuje:

**1. Ochrana služby Active Directory**

**2. Ochrana SQL vrstvy**

**3. Ochrana a vrstvy webové aplikace**

**4. Konfigurace sítě**

**5. Plán obnovení**

### <a name="1-setup-ad-and-dns-replication"></a>1. Instalační program AD a replikaci DNS

Na webu zotavení po Havárii pro aplikaci Dynamics AX do funkce se vyžaduje služby Active Directory. Existují dvě možnosti doporučené podle složitosti zákazníka v místním prostředí.

**Možnost 1**

Pokud zákazník má malý počet aplikací a jeden řadič domény pro jeho celý místní lokality a bude selhání celý web společně, pak doporučujeme používat automatické obnovení systému replikace pro replikaci počítače řadiče domény do sekundární lokality (platí pro jak na lokalitu a lokality do Azure).

**Možnost 2**

Pokud zákazník má velký počet aplikací a běží doménové struktury služby Active Directory a bude převzetí služeb při selhání-několik aplikací najednou, pak doporučujeme nastavení dalšího řadiče domény v lokalitě zotavení po Havárii (sekundární lokality nebo v Azure).

Naleznete [doprovodná příručka na zpřístupnění řadič domény v lokalitě zotavení po Havárii](site-recovery-active-directory.md). Pro zbývající část tohoto dokumentu budeme předpokládat, že řadič domény k dispozici na webu zotavení po Havárii.

### <a name="2-setup-sql-server-replication"></a>2. Nastavení replikace systému SQL Server
Doprovodná příručka podrobné technické informace na doporučené možnosti pro ochranu naleznete [vrstvy SQL](site-recovery-sql.md).

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a>3. Povolit ochranu pro Dynamics AX klienta a AOS virtuální počítače
Provést příslušné konfigurace Azure Site Recovery na tom, zda jsou virtuální počítače nasazené na základě [technologie Hyper-V](site-recovery-hyper-v-site-to-azure.md) nebo na [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Doporučená konzistentní frekvence havárií konfigurace je 15 minut.
>

Následující snímek zobrazuje stav ochrany virtuálních počítačů, Dynamics součásti v případě ochrany lokalita VMware do Azure.
![Chráněné položky](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Konfigurace sítí
Konfigurace virtuálního počítače výpočty a síť nastavení

Pro klienta AX a AOS virtuálních počítačů nakonfigurovat nastavení sítě v Azure Site Recovery, aby získat sítě virtuálních počítačů připojený k správného síťového zotavení po Havárii po převzetí služeb při selhání. Zkontrolujte, zda síť zotavení po Havárii pro tyto vrstvy směrovat vrstvě SQL.

Virtuální počítač můžete vybrat v replikované položky konfigurace nastavení sítě, jak je znázorněno níže snímku.

* Pro servery AOS vyberte skupiny dostupnosti správný.

* Pokud používáte statickou IP adresu můžete zadat IP adresa, která má virtuální počítač má provést **cílová IP adresa** pole ![nastavení sítě](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)



### <a name="5-creating-a-recovery-plan"></a>5. Vytvoření plánu obnovení

Ve službě Azure Site Recovery pro automatizaci procesu převzetí služeb při selhání můžete vytvořit plán obnovení. Přidáte aplikaci vrstvy a webovou vrstvu v plánu obnovení. Pořadí je v různých skupinách, aby vrstvy front-end vypnutí před aplikací.

1)  Vyberte trezor Azure Site Recovery ve vašem předplatném a klikněte na dlaždici plány obnovení.

2)  Klikněte na ' + plánování a zadat název obnovení.

3)  Vyberte 'Source' a "Target". Cílem může být Azure nebo sekundární lokality. V případě, že zvolíte Azure, je nutné zadat model nasazení

![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  Vyberte AOS a klientských virtuálních počítačů do plánu obnovení a klikněte na tlačítko ✓.
![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/selectvms.png)


![Plán obnovení](./media/site-recovery-dynamics-ax/recoveryplan.png)

Plán obnovení pro aplikaci Dynamics AX můžete přizpůsobit přidáním různé kroky podle níže uvedeného postupu. Výše uvedený snímek zobrazuje plán dokončení obnovení po přidání všech kroků.

*Pomocí následujících kroků:*

*1. SQL Server převzít kroky*

Odkazovat na ['Řešení zotavení po Havárii serveru SQL'](site-recovery-sql.md) doprovodná příručka podrobné informace o krocích obnovení konkrétní k systému SQL server.

*2. Převzetí služeb při selhání skupina 1: Převzetí služeb při selhání AOS virtuální počítače*

Ujistěte se, že je vybraný bod obnovení co nejblíže k databázi PIT, ale není dopředu.

*3. Skript: Nástroj pro vyrovnávání zatížení přidat (pouze E-A)* přidat skript (prostřednictvím služby Azure automation) po skupiny virtuálních počítačů AOS zobrazí, můžete do ní přidejte služby Vyrovnávání zatížení. K provedení této úlohy můžete použít skript. Najdete v článku [postup přidání služby Vyrovnávání zatížení pro zotavení po Havárii vícevrstvé aplikace](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)

*4. Skupiny převzetí služeb při selhání 2: Převzít služby AX klientské virtuální počítače.*
Při selhání se webová úroveň virtuální počítače jako součást plánu obnovení.


### <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb

Viz 'Řešení zotavení po Havárii AD' a 'Řešení zotavení po Havárii serveru SQL' doprovodné příručky informace o konkrétní do služby AD a SQL server v uvedeném pořadí během testovacího převzetí služeb při selhání.

1.  Přejděte na portál Azure a vyberte svůj trezor Site Recovery.
2.  Kliknutím na vytvořit pro Dynamics AX plánu obnovení.
3.  Klikněte na "Testovací převzetí služeb".
4.  Vyberte virtuální síť, ke spuštění procesu testovací převzetí služeb při selhání.
5.  Jakmile sekundární prostředí zapnutý, můžete provést vaše ověření.
6.  Po dokončení se k ověření, můžete vybrat ověření dokončení a bude se vyčistit testovací převzetí služeb při selhání prostředí.

Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) provedete testovací převzetí služeb.

### <a name="doing-a-failover"></a>Převzetím služeb

1.  Přejděte na portál Azure a vyberte svůj trezor Site Recovery.
2.  Kliknutím na vytvořit pro Dynamics AX plánu obnovení.
3.  Klikněte na 'převzetí služeb při selhání a vyberte 'Převzetí služeb při selhání'.
4.  Vyberte cílová síť a klikněte na ✓ zahájíte proces převzetí služeb při selhání.

Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

### <a name="perform-a-failback"></a>Provést navrácení služeb po obnovení

Doprovodná příručka 'řešení zotavení po Havárii serveru SQL, informace o konkrétní k systému SQL server naleznete během navrácení služeb po obnovení.

1.  Přejděte na portál Azure a vyberte svůj trezor Site Recovery.
2.  Kliknutím na vytvořit pro Dynamics AX plánu obnovení.
3.  Klikněte na 'převzetí služeb při selhání a vyberte převzetí služeb při selhání.
4.  Klikněte na 'Změnu Direction'.
5.  Vyberte požadované možnosti - synchronizace dat a možností vytvoření virtuálního počítače
6.  Klikněte na tlačítko ✓ ke spuštění procesu, navrácení služeb po obnovení'.


Postupujte podle [v tomto návodu](site-recovery-failback-azure-to-vmware.md) při dělají navrácení služeb po obnovení.

##<a name="summary"></a>Souhrn
Pomocí Azure Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro vaši aplikaci Dynamics AX. Můžete spustit převzetí služeb při selhání během několika sekund z libovolného místa v případě přerušení a získání aplikace fungovaly v minutách.

## <a name="next-steps"></a>Další kroky
Čtení [jaké úlohy mohu ochránit?](site-recovery-workload.md) Další informace o ochraně podnikové úlohy s Azure Site Recovery.
