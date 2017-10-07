---
title: "aaaReplicate vícevrstvé aplikace služby SharePoint pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate vícevrstvé aplikace služby SharePoint pomocí funkcí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: sutalasi
ms.openlocfilehash: d856034ac2a3c95b0c1f0cf85e62c4e7a5a3210f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Replikovat vícevrstvé aplikace služby SharePoint pro zotavení po havárii pomocí Azure Site Recovery

Tento článek podrobně popisuje, jak tooprotect aplikace služby SharePoint pomocí [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Přehled

Microsoft SharePoint je výkonné aplikace, která může pomoci skupinu nebo oddělení organizace, spolupracovat a sdílet informace. SharePoint může poskytnout intranetu portálů, dokument a správa souborů, spolupráce, sociálních sítí, extranetu, weby, hledání enterprise a business intelligence. Je také integrace systému, proces integrace a možnosti automatizace pracovního postupu. Obvykle organizace považuje za jako úrovně 1 aplikace citlivá data a toodowntime ztrátu.

Dnes Microsoft SharePoint neposkytuje žádné možnosti obnovení po havárii se na pole. Bez ohledu na typ hello a škále havárie obnovení zahrnuje použití hello pohotovostní datového centra, kterou můžete obnovit hello farmy. Pohotovostní datových centrech jsou požadovány pro scénáře, kde místní redundantní systémy a zálohování nelze obnovit z hello výpadku v hello primární datové centrum.

Řešení pro zotavení po havárii dobrý musí umožňovat modelování plány obnovení kolem architektury komplexní aplikace hello například SharePoint. Musí být také hello možnost tooadd přizpůsobit kroky toohandle aplikace mapování mezi různé úrovně a proto poskytování nižší RTO v případě havárie hello selhání jedním kliknutím.

Tento článek podrobně popisuje, jak tooprotect aplikace služby SharePoint pomocí [Azure Site Recovery](site-recovery-overview.md). Tento článek se zabývá osvědčené postupy pro replikaci tooAzure aplikací služby SharePoint tři vrstvy, jak můžete provést postupu zotavení po havárii a jak můžete tooAzure aplikace hello převzetí služeb při selhání.

Podívejte se na hello níže uvedené video o obnovení tooAzure více vrstvy aplikace.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že rozumíte hello následující:

1. [Replikaci virtuálního počítače tooAzure](site-recovery-vmware-to-azure.md)
2. Jak příliš[návrh k síti pro obnovení](site-recovery-network-design.md)
3. [Provádění tooAzure testovací převzetí služeb při selhání](site-recovery-test-failover-to-azure.md)
4. [Provádění tooAzure převzetí služeb při selhání](site-recovery-failover.md)
5. Jak příliš[replikace řadiče domény](site-recovery-active-directory.md)
6. Jak příliš[replikaci systému SQL Server](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>Architektura služby SharePoint

Služby SharePoint můžete nasadit na jeden nebo více serverů pomocí vrstvené topologie a tooimplement role serveru farmy návrh, který splňuje specifické cíle a cílů. Typické velkých, vysoce vyžádání farmy služby SharePoint server podporující velký počet souběžných uživatelů a velký počet položek obsahu pomocí služby seskupování v rámci své strategie škálovatelnost. Tento postup zahrnuje službami na vyhrazené servery, seskupování tyto služby a škálování hello servery jako skupina. Hello následující topologie znázorňuje hello služby a server seskupení pro vrstvu tři serverové farmy služby SharePoint. Podrobné informace o různých topologiích SharePoint naleznete tooSharePoint dokumentace a architektury řádku produktu. Můžete najít další podrobnosti o nasazení služby SharePoint 2013 v [tento dokument](https://technet.microsoft.com/en-us/library/cc303422.aspx).



![Vzor nasazení 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

Pro vytvoření tohoto článku, používaly virtuální počítače VMware s Windows Server 2012 R2 Enterprise. Byly použity SharePoint 2013 Enterprise edition a SQL server 2014 Enterprise edition. Site Recovery replikace je nezávislá na aplikace, hello doporučení zadané tady jsou očekávané toohold na pro následující scénáře také.

### <a name="source-and-target"></a>Zdroj a cíl

**Scénář** | **tooa sekundární lokality** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano
**Fyzický server** | Ano | Ano

### <a name="sharepoint-versions"></a>Verze služby SharePoint
jsou podporovány následující verze serveru SharePoint Hello.

* SharePoint server 2013 standardní
* SharePoint server 2013 Enterprise
* SharePoint server 2016 standardní
* SharePoint server 2016 Enterprise

### <a name="things-tookeep-in-mind"></a>Tookeep věcí v paměti

Pokud používáte sdíleného disku clusteru jako libovolné úrovně ve vaší aplikaci, pak není budete mít možnost toouse Site Recovery replikaci tooreplicate tyto virtuální počítače. Můžete použít nativní replikaci poskytované hello aplikace a pak použít [plán obnovení](site-recovery-create-recovery-plans.md) toofailover všechny úrovně.

## <a name="replicating-virtual-machines"></a>Replikace virtuálních počítačů

Postupujte podle [v tomto návodu](site-recovery-vmware-to-azure.md) toostart replikace tooAzure hello virtuálního počítače.

* Po dokončení replikace hello zkontrolujte, zda přejít tooeach virtuálního počítače každé vrstvy a vyberte stejné skupině dostupnosti, replikované položky > Nastavení > Vlastnosti > výpočty a síť ". Například pokud vaše webová vrstva 3 virtuální počítače, ujistěte se, že všechny hello, které jsou 3 virtuální počítače nakonfigurované toobe součástí stejné dostupnosti v Azure.

    ![Sada sady dostupnosti.](./media/site-recovery-sharepoint/select-av-set.png)

* Pokyny o ochraně Active Directory a DNS, najdete v tématu příliš[chránit Active Directory a DNS](site-recovery-active-directory.md) dokumentu.

* Pokyny k ochraně databázové vrstvy systémem SQL server, najdete v části příliš[chránit SQL Server](site-recovery-active-directory.md) dokumentu.

## <a name="networking-configuration"></a>Konfigurace sítě

### <a name="network-properties"></a>Vlastnosti sítě

* Hello aplikaci a webovou vrstvu virtuálních počítačů nakonfigurujte nastavení sítě na portálu Azure tak, aby virtuální počítače hello získání správného síťového zotavení po Havárii připojené toohello po převzetí služeb při selhání.

    ![Vyberte síť](./media/site-recovery-sharepoint/select-network.png)


* Pokud používáte statickou IP adresu, zadejte hello IP, který chcete hello tootake virtuálního počítače v hello **cílová IP adresa** pole

    ![Nastavit statickou IP adresu](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS a směrování provozu

Pro internetové weby [vytvořit profil Traffic Manageru typu "Priority"](../traffic-manager/traffic-manager-create-profile.md) v hello předplatného Azure. A pak nakonfigurovat DNS a Traffic Manager profilu v hello následujícím způsobem.


| **Kde** | **Zdroj** | **Cíl**|
| --- | --- | --- |
| Veřejné služby DNS | Veřejné služby DNS pro weby služby SharePoint <br/><br/> Například: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.NET |
| Místní DNS | sharepointonprem.contoso.com | Veřejná IP adresa na farmy místního hello |


V hello profil služby Traffic Manager [vytvoření hello primárními a obnovovacími koncových bodů](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Použijte hello externí koncový bod pro místní koncový bod a veřejnou IP adresu pro koncový bod Azure. Ověřte, zda text hello priority vyšší tooon místní koncový bod.

Hostitele zkušební stránku na určitém portu (například 800) v hello SharePoint webovou vrstvu v pořadí pro tooautomatically Traffic Manager zjišťovat dostupnost post převzetí služeb při selhání. Toto je alternativní řešení, v případě, že na všech webů služby SharePoint nelze povolit anonymní ověřování.

[Konfigurace profilu Správce provozu hello](../traffic-manager/traffic-manager-configure-priority-routing-method.md) s hello níže nastavení.

* Metody směrování - 'Priority.
* DNS čas toolive (TTL) – 30 sekund.
* Nastavení monitorování koncového bodu – Pokud povolíte anonymní ověřování, můžete udělit koncový bod konkrétní web. Nebo můžete použít zkušební stránku na určitém portu (například 800).

## <a name="creating-a-recovery-plan"></a>Vytvoření plánu obnovení

Plán obnovení umožňuje pořadí převzetí služeb při selhání hello v různých vrstvách vícevrstvé aplikace, proto zachování konzistence aplikace. Postupujte podle následujících kroků hello při vytváření plánu obnovení vícevrstvých webových aplikací. [Další informace o vytváření plánu obnovení](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-toofailover-groups"></a>Přidání skupin toofailover virtuální počítače

1. Vytvořte plán obnovení tak, že přidání hello aplikaci a webovou vrstvu virtuálních počítačů.
2. Klikněte na "Upravit" toogroup hello virtuálních počítačů. Ve výchozím nastavení všechny virtuální počítače jsou součástí skupiny 1, který.

    ![Přizpůsobení RP](./media/site-recovery-sharepoint/rp-groups.png)

3. Vytvořte další skupinu (skupiny 2) a přesuňte hello webovou vrstvu virtuálních počítačů do nové skupiny hello. Vaše aplikace vrstvy virtuální počítače by měly být součástí skupiny 1, a webovou vrstvu virtuálních počítačů musí být součástí 'Skupiny 2'. Toto je tooensure, který hello virtuální počítače vrstvy spustit nejprve následuje virtuální počítače vrstvy webové aplikace.


### <a name="adding-scripts-toohello-recovery-plan"></a>Přidávání skriptů toohello plánu obnovení

Skripty hello nejčastěji používaná Azure Site Recovery můžete nasadit do vašeho účtu Automation, kliknutím na tlačítko hello 'Nasazení tooAzure' níže uvedené tlačítko. Při použití všech publikovaných skriptů, ujistěte se, postupujte podle pokynů hello ve skriptu hello.

[![Nasazení tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Přidejte skript předběžné akce too'Group 1 toofailover SQL skupině dostupnosti. Pomocí skriptu, automatické obnovení systému SQL FailoverAG' hello publikované v hello ukázkové skripty. Ujistěte se, postupujte podle pokynů hello ve skriptu hello a proveďte hello požadované změny v skriptu hello správně.

    ![Přidat AG-skriptu krok-1](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Přidat AG-skriptu krok-2](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Přidejte tooattach post akce skriptu Vyrovnávání zatížení na hello převzal virtuální počítače webová vrstva (2. skupina). Pomocí skriptu, automatické obnovení systému-AddSingleLoadBalancer"hello publikované v hello ukázkové skripty. Ujistěte se, postupujte podle pokynů hello ve skriptu hello a proveďte hello požadované změny v skriptu hello správně.

    ![Přidat LB-skriptu krok-1](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Přidat LB-skriptu krok-2](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Manuální krok tooupdate hello DNS záznamy toopoint toohello nové farmy přidáte v Azure.

    * Pro internetové weby nejsou žádné aktualizace DNS požadované post převzetí služeb při selhání. Postupujte podle kroků hello popsaných v hello 'Sítě pokyny' části tooconfigure Traffic Manager. Pokud hello profil služby Traffic Manager nastavena tak jak je popsáno v předchozí části hello, přidejte skript tooopen fiktivní port (800 v příkladu hello) na hello virtuálního počítače Azure.

    * Pro interní weby přidejte IP nástroje pro vyrovnávání zatížení manuální krok tooupdate hello DNS záznamů toopoint toohello nové webové vrstvy Virtuálního počítače.

4. Přidání aplikace vyhledávání toorestore manuální krok ze zálohy nebo spuštění nové služby vyhledávání.

5. Pro aplikaci služby vyhledávání obnovení ze zálohy, postupujte podle následujících kroků.

    * Tato metoda předpokládá, že zálohy hello aplikace Vyhledávací služby byla provedena před hello závažné události a zálohování hello je k dispozici v lokalitě hello zotavení po Havárii.
    * To lze snadno dosáhnout plánování zálohování hello (například, jednou denně) a pomocí záložní kopie postup tooplace hello v lokalitě hello zotavení po Havárii. Kopírování postupů může obsahovat skriptované programy jako AzCopy (kopie Azure) nebo nastavení služby DFSR (distribuované služby replikace souborů).
    * Teď tento hello SharePoint farmy běží, přejděte hello centrální správy, zálohování a obnovení' a vyberte obnovení. Hello obnovení interrogates umístění zálohy hello zadaný (může být zapotřebí tooupdate hello hodnotu). Vyberte zálohování aplikace Vyhledávací služby hello chcete toorestore.
    * Hledání je obnoven. Mějte na paměti, která hello obnovení očekává toofind hello stejné topologie (stejný počet serverů) a stejná pevný disk písmena přiřazené toothose servery. Další informace najdete v tématu [obnovit vyhledávání služby aplikace v SharePoint 2013](https://technet.microsoft.com/library/ee748654.aspx) dokumentu.


6. Pro spuštění s novou aplikaci Vyhledávací služby, postupujte podle následujících kroků.

    * Tato metoda předpokládá, že zálohy databáze "Správa vyhledávání" hello je k dispozici v lokalitě hello zotavení po Havárii.
    * Vzhledem k tomu, že hello jiné databáze aplikace služby vyhledávání nejsou replikovány, potřebují toobe znovu vytvořit. toodo tedy přejděte tooCentral správy a odstraňte hello aplikace Vyhledávací služby. Na všechny servery, které hostitele hello indexu vyhledávání, odstraňte soubory indexu hello.
    * Vytvořte znovu hello aplikace Vyhledávací služby a tato databáze hello znovu vytvoří. Je doporučeno toohave připravené skript, který znovu vytvoří tato aplikace služby vzhledem k tomu, že není možné tooperform všechny akce prostřednictvím hello grafickým uživatelským rozhraním. Například nastavení umístění disku hello index a konfiguraci topologie vyhledávání hello jsou možná jenom pomocí rutin prostředí PowerShell služby SharePoint. Použijte rutinu prostředí Windows PowerShell hello obnovení SPEnterpriseSearchServiceApplication a zadejte hello zasílání protokolů a replikované databáze správy vyhledávání, Search_Service__DB. Tato rutina poskytuje hello hledat konfiguraci, schéma, spravované vlastnosti, pravidla a zdroje a vytvoří výchozí sadu hello ostatní součásti.
    * Jakmile hello aplikace Vyhledávací služby je potřeba znovu vytvořit, je nutné spustit úplné procházení pro každý zdroj obsahu toorestore hello službu vyhledávání. Přijdete o některé informace analytics z farmy místního hello, jako je například vyhledávání doporučení.

7. Po dokončení všech kroků hello, uložení plánu obnovení hello a plán obnovení poslední hello bude vypadat podobně jako následující.

    ![Uložené RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb
Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) toodo testovací převzetí služeb.

1.  Přejděte tooAzure portál a vyberte svůj trezor služeb zotavení.
2.  Klikněte na plán obnovení hello vytvořit pro aplikaci služby SharePoint.
3.  Klikněte na "Testovací převzetí služeb".
4.  Vyberte bod obnovení a virtuální síť Azure toostart hello testovací převzetí služeb při selhání procesu.
5.  Jakmile sekundární prostředí hello zapnutý, můžete provést vaše ověření.
6.  Po dokončení ověření hello se můžete kliknout na 'Vyčistit testovací převzetí služeb při selhání' v plánu obnovení hello a vyčištění hello testovací převzetí služeb při selhání prostředí.

Pokyny k provádění převzetí služeb při selhání pro AD a DNS, příliš[testovací převzetí služeb při selhání aspekty AD a DNS](site-recovery-active-directory.md#test-failover-considerations) dokumentu.

Pokyny k provádění převzetí služeb při selhání pro SQL vždy na skupiny dostupnosti, najdete v části příliš[provádění testovací převzetí služeb při selhání pro SQL serveru Always On](site-recovery-sql.md#steps-to-do-a-test-failover) dokumentu.

## <a name="doing-a-failover"></a>Převzetím služeb
Postupujte podle [v tomto návodu](site-recovery-failover.md) pro převzetím služeb.

1.  Přejděte tooAzure portál a vyberte svůj trezor služeb zotavení.
2.  Klikněte na plán obnovení hello vytvořit pro aplikaci služby SharePoint.
3.  Klikněte na 'Převzetí služeb při selhání'.
4.  Vyberte proces obnovení bodu toostart hello převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
Další informace o [replikace jiné aplikace](site-recovery-workload.md) pomocí Site Recovery.
