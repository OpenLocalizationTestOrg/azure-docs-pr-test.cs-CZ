---
title: "aaaTroubleshoot nástroj pro vyrovnávání zatížení Azure | Microsoft Docs"
description: "Řešení známých problémů s nástrojem pro vyrovnávání zatížení Azure"
services: load-balancer
documentationcenter: na
author: RamanDhillon
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/10/2017
ms.author: kumud
ms.openlocfilehash: 56b73fcbf0bbf18cedfd113a280cfafa2a3dc9f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-load-balancer"></a>Řešení potíží pro vyrovnávání zatížení Azure

Tato stránka obsahuje informace o odstraňování potíží pro běžné otázky nástroj pro vyrovnávání zatížení Azure. Když není k dispozici hello připojením ke službě Vyrovnávání zatížení, hello nejběžnější příznaky jsou následující: 
- Virtuální počítače za hello nástroj pro vyrovnávání zatížení nereagují toohealth testy 
- Virtuální počítače za hello nástroj pro vyrovnávání zatížení nereagují toohello přenosy na portu hello nakonfigurované

## <a name="symptom-vms-behind-hello-load-balancer-are-not-responding-toohealth-probes"></a>Příznak: Virtuální počítače za hello nástroj pro vyrovnávání zatížení nereagují toohealth testy
Pro hello back-end serverů tooparticipate sady hello nástroje pro vyrovnávání zatížení se musí projít hello testu kontroly. Další informace o sondy stavu najdete v tématu [sondy nástroje pro vyrovnávání zatížení Principy](load-balancer-custom-probe-overview.md). 

fond back-end pro vyrovnávání zatížení Hello virtuálních počítačů nemusí odpovídat toohello sondy kvůli tooany hello následujících důvodů: 
- Fond back-end pro vyrovnávání zatížení virtuálního počítače není v pořádku 
- Načíst back-endový fond vyrovnávání, které se virtuální počítač nenaslouchá na portu testu hello 
- Brána firewall nebo skupinu zabezpečení sítě je blokování hello port hello back-endový fond Vyrovnávání zatížení virtuálních počítačů 
- Další chybné konfigurace pro vyrovnávání zatížení

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>Příčina 1: Back-endový fond Vyrovnávání zatížení virtuálního počítače není v pořádku 

**Ověření a řešení**

tooresolve tento problém, přihlaste se toohello účasti virtuální počítače a zkontrolujte, jestli hello stav virtuálního počítače je v pořádku a může odpovědět příliš**Pspingu** nebo **TCPing** z jiného virtuálního počítače ve fondu hello. Pokud hello virtuálního počítače není v pořádku nebo je nelze toorespond toohello testu, musíte opravit problém hello a získat hello virtuálních počítačů zpět tooa stavu v pořádku, než se mohl účastnit vyrovnávání zatížení.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-hello-probe-port"></a>Příčina 2: Back-endový fond Vyrovnávání zatížení virtuální počítač nenaslouchá na portu testu hello
Pokud hello virtuální počítač je v pořádku, ale neodpovídá toohello testu, jedním z možných důvodů může být port testu hello není otevřen v hello účasti virtuálních počítačů, nebo není na tomto portu naslouchá hello virtuálních počítačů.

**Ověření a řešení**

1. Přihlaste se toohello back-end virtuálních počítačů. 
2. Otevřete příkazový řádek a spusťte následující příkaz toovalidate aplikace naslouchá na portu testu hello hello:   
            netstat - an
3. Pokud stav portu hello není uvedena jako **NASLOUCHAJÍCÍ**, nakonfigurujte hello správný port. 
4. Nebo můžete vybrat jiný port, který je uveden jako **NASLOUCHAJÍCÍ**a aktualizovat konfiguraci nástroje pro vyrovnávání zatížení odpovídajícím způsobem.              

###<a name="cause-3-firewall-or-a-network-security-group-is-blocking-hello-port-on-hello-load-balancer-backend-pool-vms"></a>Příčina 3: Brána Firewall nebo skupinu zabezpečení sítě je blokování hello port na back-endový fond Vyrovnávání zatížení hello virtuální počítače  
Pokud brána firewall hello na hello virtuálního počítače je blokování port testu hello nebo jeden nebo více skupiny zabezpečení sítě nakonfigurované na podsíť hello nebo hello virtuální počítač, není povolení port hello tooreach testu hello, hello virtuálního počítače je test stavu nelze toorespond toohello.          

**Ověření a řešení**

* Pokud je zapnutá brána firewall hello, zkontrolujte, jestli je konfigurovaný port testu tooallow hello. Pokud ne, nakonfigurujte hello brány firewall tooallow provozu na port testu hello a vyzkoušejte znovu. 
* Hello seznamu skupin zabezpečení sítě zkontrolujte, jestli má hello příchozí nebo odchozí přenosy na portu testu hello narušení. 
* Zkontrolujte také, pokud **Odepřít všechny** skupin zabezpečení sítě pravidla u hello seskupování hello virtuálního počítače nebo hello podsítě, který má vyšší prioritu než hello výchozí pravidlo, které umožňuje LB sondy & provoz (skupiny zabezpečení sítě musí umožňovat IP nástroje pro vyrovnávání zatížení 168.63.129.16). 
* Pokud žádné z těchto pravidel, blokují přenosy hello testu, odebrat a překonfigurujte hello pravidla tooallow hello testu provoz.  
* Otestujte, pokud hello virtuálního počítače teď zahájil reagovat, že testy toohello stavu. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>4 příčina: Ostatní chybné konfigurace pro vyrovnávání zatížení
Pokud všechny hello předchozí příčiny zdá se, že toobe ověřit a analyzovat správně a back-end hello virtuální počítač stále nepodporuje reagovat toohello test stavu, pak ručně testování pro připojení k síti a shromažďovat některé trasování toounderstand hello připojení.

**Ověření a řešení**

* Použití **Pspingu** z jednoho z hello hello ostatních virtuálních počítačů v rámci virtuální sítě tootest hello testu port odpovědi (Příklad: 10.0.0.4:3389.\psping.exe -t) a zaznamenejte výsledky. 
* Použití **TCPing** z jednoho z hello hello ostatních virtuálních počítačů v rámci virtuální sítě tootest hello testu port odpovědi (Příklad:.\tcping.exe 10.0.0.4 3389) a zaznamenejte výsledky. 
* Pokud je v těchto testech ping pak přijata žádná odpověď
    - Spuštění souběžných Netsh trasování na hello cílového back-end fondu virtuálních počítačů a jiné testovací virtuální počítač z hello stejné virtuální síti. Nyní spuštění testu Pspingu nějakou dobu, shromažďovat některé trasování sítě a potom zastavení testu hello. 
    - Analýza hello zachycení dat ze sítě a zda jsou oba příchozí a odchozí pakety související toohello ping dotazu. 
        - Pokud je možné vysledovat žádné příchozí pakety hello back-end fondu virtuálních počítačů, je potenciálně skupin zabezpečení sítě nebo UDR chybná konfigurace blokování hello přenosů. 
        - Pokud žádná odchozí pakety jsou dodržovány ve fondu back-end hello virtuálních počítačů, hello virtuálních počítačů musí toobe zaškrtnutí pro potíže, které nejsou (pro eample, port testu hello blokování aplikací). 
    - Ověřte, pokud pakety hello test probíhá vynucené tooanother cílové (případně prostřednictvím nastavení UDR) dříve, než dorazila hello nástroj pro vyrovnávání zatížení. To může způsobit hello provoz toonever reach hello back-end virtuálních počítačů. 
* Změnit typ testu hello (například tooTCP HTTP) a nakonfigurujte hello odpovídající port ve skupinách zabezpečení sítě seznamy řízení přístupu a brány firewall toovalidate Pokud hello problém s konfigurací hello testu odpovědi. Další informace o Konfigurace testu stavu najdete v tématu [koncový bod Vyrovnávání zatížení Konfigurace testu stavu](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-tootraffic-on-hello-configured-data-port"></a>Příznak: Virtuální počítače za službou Vyrovnávání zatížení nereagují tootraffic na portu hello nakonfigurované dat

Pokud back-end fondu virtuálních počítačů je uveden jako v pořádku a odpovídá toohello sondy stavu, ale stále neúčastní hello Vyrovnávání zatížení nebo neodpovídá toohello přenos dat, může být kvůli tooany hello následujících důvodů: 
* Načíst fond back-end pro vyrovnávání, který počítač nenaslouchá na portu data hello 
* Skupina zabezpečení sítě je blokování hello port hello back-endový fond Vyrovnávání zatížení virtuálních počítačů  
* Přístup k hello nástroj pro vyrovnávání zatížení z hello stejného virtuálního počítače a síťový adaptér 
* Přístup k hello VIP pro vyrovnávání zatížení Internetu z hello účasti back-endový fond Vyrovnávání zatížení virtuálních počítačů 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-hello-data-port"></a>Příčina 1: Back-endový fond Vyrovnávání zatížení virtuální počítač nenaslouchá na portu data hello 
Pokud virtuální počítač neodpovídá toohello přenos dat, může to být protože hello cílový port není otevřen v hello účasti virtuálních počítačů, nebo, není na tomto portu naslouchá hello virtuálních počítačů. 

**Ověření a řešení**

1. Přihlaste se toohello back-end virtuálních počítačů. 
2. Otevřete příkazový řádek a spusťte následující příkaz toovalidate aplikace naslouchá na portu data hello hello:  
            netstat - an 
3. Pokud není hello port uvedené se stavem "Naslouchá", nakonfigurovat port naslouchacího procesu správné hello 
4. Pokud hello port je označen jako naslouchající, zkontrolujte hello cílová aplikace na tomto portu pro všechny možné problémy. 

### <a name="cause-2-network-security-group-is-blocking-hello-port-on-hello-load-balancer-backend-pool-vm"></a>Příčina 2: Skupina zabezpečení sítě je blokování hello port hello back-endový fond Vyrovnávání zatížení virtuálních počítačů  

Pokud jeden nebo více skupin zabezpečení, které jsou nakonfigurované v podsíti hello nebo na hello virtuálních počítačů sítě, je blokování hello zdrojové IP adresy nebo portu, potom hello virtuální počítač nelze toorespond.

* Zobrazí seznam skupin zabezpečení sítě hello nakonfigurované na back-end hello virtuálních počítačů. Další informace naleznete v tématu:
    -  [Správa skupin zabezpečení sítě pomocí hello portálu](../virtual-network/virtual-network-manage-nsg-arm-portal.md)
    -  [Správa skupin zabezpečení sítě pomocí PowerShellu](../virtual-network/virtual-network-manage-nsg-arm-ps.md)
* Hello seznamu skupin zabezpečení sítě zkontrolujte, zda:
    - Hello příchozí nebo odchozí přenosy na portu hello dat má narušení. 
    - **Odepřít všechny** pravidla skupiny zabezpečení sítě na hello síťový adaptér z virtuálního počítače nebo hello podsítě hello, která má vyšší prioritu, že hello výchozí pravidlo, které umožňuje sondy nástroj pro vyrovnávání zatížení a provoz (skupiny zabezpečení sítě musí umožňovat IP nástroje pro vyrovnávání zatížení 168.63.129.16, který je port testu) 
* Pokud žádné z pravidel hello, blokují přenosy hello, odebrat a překonfigurujte přenos dat hello tooallow těchto pravidel.  
* Otestujte, pokud hello virtuálního počítače teď zahájil sondy stavu toohello toorespond.

### <a name="cause-3-accessing-hello-load-balancer-from-hello-same-vm-and-network-interface"></a>Příčina 3: Přístup k hello nástroj pro vyrovnávání zatížení z hello stejné rozhraní virtuálního počítače a sítě 

Pokud vaše aplikace hostované na back-end hello virtuálních počítačů služby Vyrovnávání zatížení se pokouší tooaccess jiná aplikace, které jsou hostované v hello stejnou back-end virtuálních počítačů přes hello stejné síti rozhraní, se o nepodporovaný scénář a se nezdaří. 

**Řešení** vyřešíte tento problém prostřednictvím jednu z následujících metod hello:
* Nakonfigurujte samostatné back-end fondu virtuálních počítačů na aplikaci. 
* Nakonfigurujte aplikace hello ve virtuálních počítačích duální síťovou kartu, tak, aby byla každá aplikace pomocí vlastní rozhraní sítě a IP adresu. 

### <a name="cause-4-accessing-hello-internal-load-balancer-vip-from-hello-participating-load-balancer-backend-pool-vm"></a>4 příčina: Přístup k hello interní VIP pro vyrovnávání zatížení z hello účasti back-endový fond Vyrovnávání zatížení virtuálních počítačů

Pokud je nakonfigurovaný ILB VIP uvnitř virtuální sítě, a jeden z hello účastnické back-end virtuálních počítačů se pokouší tooaccess hello interní VIP pro vyrovnávání zatížení, jejímž výsledkem chyba. Tento scénář se nepodporuje.
**Řešení** vyhodnotit aplikační bránou nebo jiné servery proxy (například nginx nebo haproxy) toosupport který druh scénáře. Další informace o Application Gateway najdete v tématu [Přehled služby Application Gateway](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>Dalších síťových přenosů
Pokud se rozhodnete tooopen případu podpory, shromážděte následující informace o řešení rychlejší hello. Zvolte hello tooperform virtuálního počítače jeden back-end následující testy:
- Pomocí Pspingu z jednoho z hello back-end virtuálních počítačů v rámci hello virtuální síť tootest hello testu port odpovědi (Příklad: pspingu 10.0.0.4:3389) a zaznamenejte výsledky. 
- Použít TCPing z jednoho z hello back-end virtuálních počítačů v rámci hello virtuální síť tootest hello testu port odpovědi (Příklad: pspingu 10.0.0.4:3389) a zaznamenejte výsledky.
- Pokud je v těchto testech ping žádná odpověď, spusťte souběžných příkazu Netsh trace na back-end hello virtuálních počítačů a hello virtuální síť testovací virtuální počítač při spuštění Pspingu pak zastavit příkazu Netsh trace hello. 
  
## <a name="next-steps"></a>Další kroky

Pokud předchozí kroky hello nevyřeší hello problém, otevřete [lístek podpory](https://azure.microsoft.com/support/options/).

