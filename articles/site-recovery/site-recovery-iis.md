---
title: "aaaReplicate vícevrstvé IIS na základě webové aplikace pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate služby IIS webová farma virtuálních počítačů pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 1974265b3cb05f6dc57049876306d2e08424bb97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Replikovat vícevrstvé aplikace webové služby IIS na základě pomocí Azure Site Recovery

## <a name="overview"></a>Přehled


Software, aplikace je modul hello firmy produktivity v organizaci. Různé webových aplikací můžete v organizaci slouží k jiným účelům. Některé z těchto jako mzdy zpracování, finanční aplikace a zákazníků, kterým čelí weby může být co nejvíce kritické pro organizaci. Je důležité pro organizaci toohave hello je nahoru a spuštěná na všechny časy tooprevent ke snížení produktivity a více je nejdůležitější zabránit žádný obrázek značky toohello poškození hello organizace.

Kritické webové aplikace jsou obvykle nastavit jako vícevrstvé aplikace s hello webový server, databáze a aplikací na různých vrstev. Kromě se šíří přes různých úrovní, hello aplikace může také používat více serverů v každé vrstvě tooload vyrovnávání hello přenosů. Kromě toho může být hello mapování mezi různými úrovněmi a na webovém serveru hello založené na statických IP adres. Na převzetí služeb při selhání některé z těchto mapování potřebovat toobe aktualizován, zejména, pokud máte více webů nakonfigurovaná na webovém serveru hello. V případě webových aplikací pomocí protokolu SSL bude nutné vazby certifikátů toobe aktualizovat.

Tradiční bez replikace na základě obnovení metody zahrnovat zálohování z různých souborů, konfigurace, nastavení registru, vazby, vlastní komponent (COM nebo rozhraní .NET), obsah a také certifikáty a se obnovované hello soubory pomocí sady ruční kroky. Tyto postupy jsou jasně náročnější, chyba není škálovatelné a náchylné k chybám. Je například možné snadno pro tooforget zálohování certifikátů a být ponecháno s žádná volba ale toobuy nové certifikáty pro hello server po převzetí služeb při selhání.

Řešení pro zotavení po havárii funkční, by měl Povolit modelování plány obnovení kolem hello výše architektury komplexní aplikace a také mít hello možnost tooadd přizpůsobit kroky toohandle aplikace mapování mezi různými úrovněmi proto poskytování jedním kliknutím zda snímek řešení v případě havárie úvodní tooa hello nižší RTO.


Tento článek popisuje, jak tooprotect službu IIS na základě, webové aplikace pomocí [Azure Site Recovery](site-recovery-overview.md). Tento článek se zabývá osvědčené postupy pro replikaci tři vrstvy služby IIS na základě tooAzure webové aplikace, jak můžete provést postupu zotavení po havárii a jak můžete tooAzure aplikace hello převzetí služeb při selhání.


## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že rozumíte hello následující:

1. [Replikaci virtuálního počítače tooAzure](site-recovery-vmware-to-azure.md)
1. Jak příliš[návrh k síti pro obnovení](site-recovery-network-design.md)
1. [Provádění tooAzure testovací převzetí služeb při selhání](./site-recovery-test-failover-to-azure.md)
1. [Provádění tooAzure převzetí služeb při selhání](site-recovery-failover.md)
1. Jak příliš[replikace řadiče domény](site-recovery-active-directory.md)
1. Jak příliš[replikaci systému SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Vzory pro nasazení
Webové aplikace služby IIS na základě obvykle zahrnuje jednu hello následující vzory nasazení:

** Nasazení vzor 1 ** služby IIS na základě webové farmy se Routing(ARR) požádat o aplikaci, Server služby IIS a Microsoft SQL Server.

![Vzor nasazení](./media/site-recovery-iis/deployment-pattern1.png)

**Vzor nasazení 2** služby IIS na základě webové farmy se Routing(ARR) požádat o aplikaci, Server služby IIS, aplikační Server a Microsoft SQL Server.


![Vzor nasazení](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

Vytváření virtuálních počítačů VMware Tento článek se serverem služby IIS verze 7.5 na Windows Server 2012 R2 Enterprise za účelem hello byly použity. Replikace obnovení lokality je nezávislá na aplikace, pokud hello doporučení tady jsou očekávané toohold také následujících scénářů a pro různé verze služby IIS.

### <a name="source-and-target"></a>Zdroj a cíl

**Scénář** | **tooa sekundární lokality** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano
**Fyzický server** | Ne | Ano

## <a name="replicate-virtual-machines"></a>Replikace virtuálních počítačů

Postupujte podle [v tomto návodu](site-recovery-vmware-to-azure.md) toostart replikace všech hello IIS webové farmy virtuálních počítačů tooAzure.

Pokud používáte statickou IP adresu můžete zadat hello IP, který chcete hello tootake virtuálního počítače v hello [ **cílová IP adresa** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) nastavení v nastavení výpočtů a sítě.

![Cílové IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Vytvoření plánu obnovení

Plán obnovení umožňuje pořadí převzetí služeb při selhání hello v různých vrstvách vícevrstvé aplikace, proto zachování konzistence aplikace. Postupujte podle následujících kroků hello při vytváření plánu obnovení vícevrstvých webových aplikací.  [Další informace o vytváření plánu obnovení](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-toofailover-groups"></a>Přidání skupin toofailover virtuální počítače
Typické vícevrstvé aplikace webové služby IIS budou tvořeny a databázové vrstvy s virtuálními počítači, hello webová vrstva zřízená serveru IIS a aplikační vrstvě SQL. Přidejte všechny tyto virtuální počítače toodifferent skupiny založené na vrstvě jako níže. [Další informace o přizpůsobení plán obnovení](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Vytvoření plánu obnovení. Přidáte virtuální počítače vrstvy hello databáze v 1. skupina tooensure, že jsou vypnutí poslední a první zapínají.

1. Přidáte virtuální počítače vrstvy hello aplikace ve skupině 2 tak, aby se zapínají po byla zapínají hello databázové vrstvy.

1. Přidáte hello webové vrstvy virtuální počítače do 3. skupina tak, aby se zapínají po byla zapínají hello aplikační vrstvě.

1. Přidejte virtuální počítače Vyrovnávání zatížení do skupiny 4 tak, aby se zapínají po byla zapínají hello webovou vrstvu.


### <a name="adding-scripts-toohello-recovery-plan"></a>Přidávání skriptů toohello plánu obnovení
Může být nutné toodo některých operací na virtuálních počítačích Azure post převzetí služeb při selhání a testovací převzetí služeb při selhání toomake IIS webové farmy funkce hello správně. Můžete automatizovat převzetí služeb při selhání operaci post hello jako aktualizaci položky DNS, změna vazby webu, změňte v připojovacím řetězci přidáním příslušných skriptů v plánu obnovení hello jak je uvedeno níže. [Další informace o přidání skript plánu obnovení](./site-recovery-create-recovery-plans.md#add-scripts).

#### <a name="dns-update"></a>Aktualizace DNS.
Pokud hello DNS je nakonfigurován pro dynamické aktualizace DNS, pak po spuštění virtuálních počítačů obvykle aktualizovat hello DNS s novou IP Adresou hello. Pokud chcete tooadd explicitní krok tooupdate DNS s hello nové IP adresy virtuálních počítačů hello přidejte to [skriptu tooupdate IP ve službě DNS](https://aka.ms/asr-dns-update) jako akce post na skupiny plánu obnovení.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Připojovací řetězec v souboru web.config aplikace
Hello připojovací řetězec Určuje, že komunikuje hello databázi, která hello webu.

Pokud hello připojovací řetězec představuje název hello hello databáze virtuálního počítače, bude žádné další kroky potřebné post převzetí služeb při selhání a aplikace hello se nebude moct komunikovat tooautomatically toohello DB. Navíc pokud se uchovávají hello IP adresu pro virtuální počítač hello databáze, nebude potřeba tooupdate hello připojovací řetězec. Pokud připojovací řetězec hello odkazuje toohello databázového virtuálního počítače pomocí IP adresy, je nutné aktualizovat toobe post převzetí služeb při selhání. Například Hello níže připojovací řetězec body toohello DB s IP Adresou 127.0.1.2

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Hello připojovací řetězec v webovou vrstvu můžete aktualizovat přidáním [skript aktualizace připojení služby IIS](https://aka.ms/asr-update-webtier-script-classic) po 3. skupina v plánu obnovení hello.

#### <a name="site-bindings-for-hello-application"></a>Vazby webu pro aplikaci hello
Každé lokalitě se skládá z vazby informace, které zahrnují hello typ vazby, hello IP adresu, na které hello naslouchá server služby IIS toohello požadavky na lokalitu hello, číslo portu hello a názvy hostitelů hello hello lokality. V době hello převzetí služeb při selhání může být třeba tyto vazby toobe aktualizováni, pokud dojde ke změně hello IP adresu s nimi spojených.

> [!NOTE]
>
> Pokud jste označili 'všechny nepřiřazené' pro vazbu webu hello jako v následujícím příkladu hello, nebude nutné tooupdate post převzetí této vazby. Navíc pokud hello IP adresy přidružené k lokalitě nedojde ke změně po převzetí služeb při selhání, vazbu webu hello nemusí být aktualizována (uchování hello IP adresu závisí na architekturu hello sítě a podsítě přiřazené toohello primárními a obnovovacími weby a proto může nebo nemusí být vhodný pro vaši organizaci.)

![Vazba SSL](./media/site-recovery-iis/sslbinding.png)

Pokud máte přidružené hello IP adresu s lokalitou, budete potřebovat tooupdate všechny vazby webu s hello novou IP adresu. Můžete přidat [webové vrstvy aktualizační skript](https://aka.ms/asr-web-tier-update-runbook-classic) po 3. skupina v vazby webu hello toochange plánu obnovení.


#### <a name="update-load-balancer-ip-address"></a>Aktualizujte IP adresa služby Vyrovnávání zatížení
Pokud máte virtuální počítač směrování žádostí na aplikace, přidejte [směrování žádostí na aplikace služby IIS převzetí služeb při selhání skriptu](https://aka.ms/asr-iis-arrtier-failover-script-classic) po 4 skupiny tooupdate hello IP adresu.

#### <a name="hello-ssl-cert-binding-for-an-https-connection"></a>Hello vazbu certifikátu SSL pro připojení https
Weby může mít přidružený certifikát SSL, který pomáhá zajistit zabezpečenou komunikaci mezi hello webový server a prohlížeče hello uživatele. Pokud má web hello připojení https a přidružené https toohello vazby lokality IP adresu serveru služby IIS hello s vazbou certifikátu SSL, bude nutné novou vazbu webu toobe přidat pro cert hello hello post IP hello IIS virtuálního počítače převzetí služeb při selhání.

certifikát SSL Hello může být vydaný pro-

(a) hello plně kvalifikovaný název domény webu hello<br>
b) název hello serveru hello<br>
c) certifikát se zástupným znakem pro název domény hello<br>
d) IP adresu – Pokud je hello certifikát SSL vydaný pro hello IP hello serveru služby IIS, jiné toobe musí certifikát SSL vydaný pro hello hello IP adresu serveru služby IIS na hello Azure site a další vazby SSL pro tento certifikát bude nutné vytvořit toobe. Proto je vhodné toonot použijte certifikát SSL vydaný pro IP. Toto je méně používaných možnost a bude brzy zastaralá podle nové změny fórum certifikační Autority nebo prohlížeče.

#### <a name="update-hello-dependency-between-hello-web-and-hello-application-tier"></a>Aktualizace hello závislosti mezi hello webové a aplikační vrstvě hello
Pokud máte konkrétní závislost aplikace na základě IP adresy hello hello virtuálních počítačů, musíte tooupdate převzetí této závislosti post.

## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb
Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) toodo testovací převzetí služeb.

1.  Přejděte tooAzure portál a vyberte svůj trezor služeb zotavení.
1.  Klikněte na hello plán obnovení pro webové farmy IIS vytvořit.
1.  Klikněte na "Testovací převzetí služeb".
1.  Vyberte bod obnovení a virtuální síť Azure toostart hello testovací převzetí služeb při selhání procesu.
1.  Jakmile sekundární prostředí hello zapnutý, můžete provést vaše ověření.
1.  Po dokončení ověření hello se můžete vybrat ověření dokončení a hello testovací převzetí služeb při selhání prostředí bude vyčištěna.

## <a name="doing-a-failover"></a>Převzetím služeb
Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

1.  Přejděte tooAzure portál a vyberte svůj trezor služeb zotavení.
1.  Klikněte na hello plán obnovení pro webové farmy IIS vytvořit.
1.  Klikněte na 'Převzetí služeb při selhání'.
1.  Vyberte proces obnovení bodu toostart hello převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
Další informace o [replikovat jiné aplikace](site-recovery-workload.md) pomocí Site Recovery.
