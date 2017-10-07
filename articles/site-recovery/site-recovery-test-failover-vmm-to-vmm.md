---
title: "aaaTest převzetí služeb při selhání (VMM tooVMM) ve službě Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje hello replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání tooAzure nebo do sekundárního datacentra."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.openlocfilehash: 6b4f65ab692cbb0665102c4f51ea0694151cd3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-failover-vmm-toovmm-in-site-recovery"></a>Testovací převzetí služeb při selhání (VMM tooVMM) ve službě Site Recovery


Tento článek obsahuje informace a pokyny pro provádění testovací převzetí služeb nebo přechod zotavení po havárii virtuálních počítačů (VM) a fyzické servery, které jsou chráněné službou Azure Site Recovery. System Center Virtual Machine Manager VMM spravovat místní lokality budete používat jako hello obnovení lokality.

Spustit testovací převzetí služeb při selhání toovalidate strategie replikace nebo provést zotavení po Havárii procházení bez ztráty dat nebo výpadek. Testovací převzetí služeb nemá žádný vliv na probíhající replikace hello nebo produkční prostředí. Můžete ji spustit virtuální počítač nebo [plán obnovení](site-recovery-create-recovery-plans.md). Když se aktivuje testovací převzetí služeb, je nutné toospecify hello síť, která hello testovací virtuální počítače se budou připojovat k. Můžete sledovat průběh hello hello testovací převzetí služeb při selhání na hello **úlohy** stránky.  

Pokud máte jakékoli dotazy nebo připomínky, odešlete je na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hello-infrastructure-for-test-failover"></a>Příprava infrastruktury hello testovací převzetí služeb při selhání
Pokud chcete toorun testovací převzetí služeb pomocí stávající síť, Příprava služby Active Directory, DHCP a DNS v síti.

Pokud chcete toorun testovací převzetí služeb pomocí sítě virtuálních počítačů toocreate hello možnost automaticky, přidejte manuální krok před skupiny-1 v plánu obnovení hello budete toouse hello testovací převzetí služeb při selhání. Pak přidejte toohello prostředky infrastruktury hello automaticky vytvoří síť před spuštěním hello testovací převzetí služeb při selhání.

### <a name="things-toonote"></a>Toonote věcí
Pokud replikujete tooa sekundární lokality, hello typ sítě, že hello repliky počítač používá nepotřebuje toomatch hello typ logické sítě používané pro testovací převzetí služeb při selhání, ale některé kombinace nemusí fungovat. Pokud replika hello používá DHCP a izolace založené na síti VLAN, nepotřebuje hello síť virtuálních počítačů pro repliku hello fond statických adres IP. Proto pomocí virtualizace sítě systému Windows hello testovací převzetí služeb při selhání nebude fungovat, protože nejsou k dispozici žádné fondy adres. 

Kromě toho hello testovací převzetí služeb při selhání nebude fungovat, pokud síť repliky hello používá bez izolace a testovací síti hello používá virtualizace sítě systému Windows. To je proto síti bez izolace hello nemá požadované toocreate hello podsítě síť virtualizace sítě systému Windows.

Jak virtuální počítače repliky jsou sítě virtuálních počítačů připojených toomapped po převzetí služeb při selhání závisí na konfiguraci sítě virtuálních počítačů hello v konzole VMM hello.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Síť virtuálních počítačů konfigurovanou bez izolace nebo izolace sítě VLAN
Pokud DHCP je definována pro síť virtuálních počítačů hello, je virtuální počítač repliky hello připojené toohello ID sítě VLAN prostřednictvím hello nastavení, které jsou určené pro hello síťová lokalita v hello přidruženu logickou síť. Hello virtuální počítač přijímá jeho IP adresu ze serveru DHCP k dispozici hello. 

Nepotřebujete toodefine fond statických adres IP pro síť virtuálních počítačů cíl hello. Pokud fond statických adres IP se používá pro síť virtuálních počítačů hello, je virtuální počítač repliky hello připojené toohello ID sítě VLAN prostřednictvím hello nastavení, které jsou určené pro hello síťová lokalita v hello přidruženu logickou síť.

Hello virtuální počítač přijímá jeho IP adresu z fondu hello, která je definována pro síť virtuálních počítačů hello. Pokud fond statických adres IP není definována v síti virtuálních počítačů hello cíl, přidělení IP adresy se nezdaří. Vytvořte fond IP adres hello na obou hello zdrojové a cílové VMM serverech, které budete používat pro ochranu a obnovení.

#### <a name="vm-network-with-windows-network-virtualization"></a>Síť virtuálních počítačů s virtualizací sítě systému Windows
Pokud je síť virtuálních počítačů nakonfigurované pomocí virtualizace sítě systému Windows, byste měli definovat fond statických pro hello cílový virtuální počítačové sítě, bez ohledu na to, jestli je hello zdrojové síti virtuálních počítačů nakonfigurované toouse DHCP nebo fond statických adres IP. 

Pokud definujete DHCP, hello cílovém serveru VMM funguje jako DHCP server a poskytuje IP adresu z fondu hello, která je definována pro síť virtuálních počítačů cíl hello. Pokud se používá fond statických IP adres je definována pro zdrojový server hello, hello cílovém serveru VMM přidělí IP adresu z fondu hello. V obou případech přidělování IP adres se nezdaří, pokud fond statických adres IP není definován.


### <a name="prepare-dhcp"></a>Příprava DHCP
Pokud součástí hello virtuálních počítačů testovací převzetí služeb při selhání používal protokol DHCP, vytvořit testovací server DHCP v rámci hello izolované sítě za účelem hello testovací převzetí služeb při selhání.

### <a name="prepare-active-directory"></a>Příprava služby Active Directory
toorun testovací převzetí služeb při selhání pro testování aplikace, musíte kopii prostředí služby Active Directory produkční hello v testovacím prostředí. Další informace najdete v tématu hello [testovací převzetí služeb při selhání důležité informace týkající se služby Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>Příprava DNS
Připravte DNS server pro hello testovací převzetí služeb při selhání následujícím způsobem:

* **DHCP**: Pokud virtuální počítače používat službu DHCP, IP adresa hello hello testu DNS by měl být aktualizovány na serveru DHCP testovací hello. Pokud používáte typ sítě virtualizace sítě systému Windows, hello VMM server funguje jako hello DHCP server. Proto je třeba aktualizovat hello IP adresu DNS v hello testovací převzetí služeb při selhání sítě. V takovém případě hello virtuální počítače zaregistrovat sami toohello příslušný server DNS.
* **Statická adresa**: Pokud virtuální počítače používat statickou IP adresu, hello IP adresu serveru DNS by měly být aktualizovány v testovací síti převzetí služeb při selhání testu hello. Může být nutné tooupdate DNS s IP adresou hello hello testovací virtuální počítače. Můžete použít následující ukázkový skript pro tento účel hello:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Tento postup popisuje, jak toorun testovací převzetí služeb při selhání pro obnovení plánu. Alternativně můžete spustit hello převzetí služeb při selhání pro jeden virtuální počítač na hello **virtuální počítače** kartě.

![Okno test převzetí služeb při selhání](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **převzetí služeb při selhání** > **testovací převzetí služeb při selhání**.
1. Na hello **testovací převzetí služeb při selhání** okno, zadejte, jak mají být připojené toonetworks po převzetí služeb při selhání hello testovací virtuální počítače. Další informace najdete v tématu hello [sítě možnosti](#network-options-in-site-recovery).
1. Sledovat průběh převzetí služeb při selhání na hello **úlohy** kartě.
1. Po dokončení převzetí služeb při selhání ověřte, že hello virtuální počítače úspěšně spustí.
1. Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tento krok odstraní hello virtuální počítače a sítě, které byly vytvořeny během testovacího převzetí služeb při selhání.


## <a name="network-options-in-site-recovery"></a>Možnosti sítě v Site Recovery

Při spuštění testu převzetí služeb, zobrazí se dotaz tooselect nastavení sítě pro testovací počítače repliky. Máte několik možností.  

| **Možnost testovací převzetí služeb při selhání** | **Popis** | **Kontrola převzetí služeb při selhání** | **Podrobnosti** |
| --- | --- | --- | --- |
| **Selhání tooa sekundární lokalita VMM – bez sítě** |Nemáte vyberte síť virtuálních počítačů. |Převzetí služeb při selhání ověří, že jsou vytvořeny testovacích počítačů.<br/><br/>Hello testovací virtuální počítač se vytvoří na hello hostiteli, kde existuje virtuální počítač repliky hello. Není přidáno toohello cloudu, kde je umístěn virtuální počítač repliky hello. |<p>Hello při selhání počítač není připojený tooany sítě.<br/><br/>Hello počítač může být síť virtuálních počítačů připojených tooa po jeho vytvoření. |
| **Selhání tooa sekundární lokalita VMM – se sítí** |Vyberte stávající síť VM. |Převzetí služeb při selhání ověří, že byly vytvořeny virtuální počítače. |Hello testovací virtuální počítač se vytvoří na hello hostiteli, kde existuje virtuální počítač repliky hello. Není přidáno toohello cloudu, kde je umístěn virtuální počítač repliky hello.<br/><br/>Vytvořte síť virtuálních počítačů, která bude izolovaná od produkční sítě.<br/><br/>Pokud používáte síť založená na síti VLAN, doporučujeme vytvořit samostatnou logickou síť (nepoužívá se v produkčním prostředí) v nástroji VMM pro tento účel. Tato logická síť je použité toocreate sítě virtuálních počítačů pro testovací převzetí služeb při selhání.<br/><br/>Hello logické sítě musí být přidružen alespoň jeden ze síťových adaptérů hello ve všech serverech hello technologie Hyper-V, které jsou hostiteli virtuálních počítačů.<br/><br/>Pro logické sítě VLAN, hello síťové lokality, abyste přidali toohello logické sítě by měl být izolované.<br/><br/>Pokud používáte logické sítě na základě virtualizace sítě systému Windows, Azure Site Recovery automaticky vytvoří izolované sítě virtuálních počítačů. |
| **Selhání přes tooa sekundární lokalita VMM – vytvoření sítě** |Dočasné testovací síti se vytvoří automaticky v závislosti na nastavení hello, který určíte v **logické sítě** a její související síťové lokality. |Převzetí služeb při selhání ověří, že byly vytvořeny virtuální počítače. |Tuto možnost použijte, pokud plán obnovení hello používá více než jedna síť virtuálních počítačů. Pokud používáte Windows virtualizace sítě, tato možnost umožňuje automatické vytvoření sítě virtuálních počítačů s hello stejné nastavení (podsítí a fondy IP adres) v síti hello hello repliky virtuálního počítače. Tyto sítě virtuálních počítačů se automaticky vyčistí po dokončení hello testovací převzetí služeb při selhání.</p><p>Hello testovací virtuální počítač se vytvoří na hello hostiteli, kde existuje virtuální počítač repliky hello. Není přidáno toohello cloudu, kde je umístěn virtuální počítač repliky hello. |

> [!TIP]
> Hello IP adresa poskytnutý tooa virtuálního počítače během testovacího převzetí služeb při selhání je hello stejnou IP adresu, která hello virtuální počítač by přijímat plánovaném nebo neplánovaném převzetí služeb při selhání (za předpokladu, že hello IP adresa je k dispozici v hello testovací převzetí služeb při selhání sítě). Pokud hello stejnou IP adresu není k dispozici v hello testovací převzetí služeb při selhání sítě, virtuální počítač hello obdrží jinou IP adresu, která je k dispozici v hello testovací převzetí služeb při selhání sítě.
>
>


## <a name="test-failover-tooa-production-network-on-a-recovery-site"></a>Testovací převzetí služeb při selhání tooa produkční sítě v recovery site
Doporučujeme, pokud jste to testovací převzetí služeb, abyste zvolili síť, která se liší od síti produkční obnovení lokality, který jste zadali v [mapování sítě](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Ale pokud chcete připojení k síti začátku do konce toovalidate v rámci virtuálního počítače při selhání, mějte na paměti následující body hello:

* Ujistěte se, že aby hello primární virtuální počítač vypnutý, když provádíte hello testovací převzetí služeb při selhání. Pokud to neuděláte, dvou virtuálních počítačů se stejnou identitou bude spuštěna v hello stejné sítě v hello hello stejný čas. Tato situace může způsobit tooundesired důsledky.
* Při čištění hello testování převzetí služeb při selhání virtuálního počítače budou ztraceny všechny změny, abyste vytvořili toohello testovací převzetí služeb při selhání virtuálního počítače. Tyto změny nejsou replikované back toohello primárního virtuálního počítače.
* Tento způsob plnění testování vede toodowntime pro produkční aplikace. Požádejte uživatele aplikace hello není toouse, že pokud přejdete hello zotavení po Havárii aplikace hello právě probíhá.  


## <a name="next-steps"></a>Další kroky
Poté, co byl úspěšně spuštěn převzetí služeb při selhání, můžete zkusit [převzetí služeb při selhání](site-recovery-failover.md).
