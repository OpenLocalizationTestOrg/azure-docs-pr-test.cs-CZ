---
title: "aaaLoad vyrovnávání vlastní testy paměti a stav monitorování | Microsoft Docs"
description: "Zjistěte, jak toouse vlastní testy pro nástroj pro vyrovnávání zatížení Azure toomonitor instance za službou Vyrovnávání zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3dfcfcd2d5cffa58b160cb38d63acffbd997d452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-load-balancer-probes"></a>Porozumění testům nástroje pro vyrovnávání zatížení

Azure nástroj pro vyrovnávání zatížení nabízí hello schopností toomonitor hello stav instance serveru pomocí sondy. Pokud sondu nezdaří toorespond, nástroj pro vyrovnávání zatížení zastaví odesílání novou instanci není v pořádku toohello připojení. ovlivněné nejsou Hello existující připojení a nová připojení posílají toohealthy instance.

Cloudové služby rolí (role pracovního procesu a webové role) pomocí agenta hosta pro test monitorování. Při použití virtuálních počítačů za službou Vyrovnávání zatížení musí být nakonfigurované TCP nebo HTTP vlastní testy paměti.

## <a name="understand-probe-count-and-timeout"></a>Pochopení počet test a vypršení časového limitu

Postup kontroly závisí na:

* Hello počet úspěšné sondy, které umožňují instanci toobe označený jako zapnutý.
* Hello počet neúspěšných sondy, které způsobí instanci toobe označené jako dolů.

Hello časový limit a četnost hodnotu nastavenou v SuccessFailCount určit, zda je instance potvrzené toobe spuštěna nebo není spuštěna. V hello portálu Azure časový limit hello nastaven tootwo časy hello hodnota frekvence hello.

Konfigurace testu Hello všech instancí Vyrovnávání zatížení sítě pro koncový bod (to znamená, Vyrovnávání zatížení sítě sady) musí být hello stejné. To znamená, že nemůže mít konfiguraci různých testu pro každou instanci role nebo virtuální počítač v hello stejné hostované služby pro konkrétní koncový bod kombinaci. Například každá instance musí mít identické místní porty a vypršení časových limitů.

> [!IMPORTANT]
> Sondu nástroje pro vyrovnávání zatížení používá hello IP adresu 168.63.129.16. Tato veřejná IP adresa zajišťuje komunikaci toointernal platformy prostředky pro hello přineste vaše vlastníte IP Azure virtuální síť. Hello virtuální veřejnou IP adresu 168.63.129.16 se používá ve všech oblastech a nedojde ke změně. Doporučujeme povolit tuto IP adresu na všechny zásady místní brány firewall. Nemělo by se používat bezpečnostní riziko protože zdrojem může být pouze hello vnitřní platformy Azure zprávy z této adresy. Pokud není to uděláte, bude neočekávanému chování v různých scénářích, jako jsou konfigurace hello stejného rozsahu IP adres 168.63.129.16 a s duplicitní IP adresy.

## <a name="learn-about-hello-types-of-probes"></a>Další informace o typech hello sondy

### <a name="guest-agent-probe"></a>Test agenta hosta

Tento test je jenom pro cloudové služby Azure k dispozici. Nástroj pro vyrovnávání zatížení, využívá agent hosta hello uvnitř hello virtuálního počítače a pak naslouchá a odpoví odpověď HTTP 200 OK jenom v případě hello instance je ve stavu Připraveno hello (to znamená, není v jiném stavu například zaneprázdněn recyklace nebo ukončení).

Další informace najdete v tématu [konfigurace hello souboru definice služby (csdef) pro sondy stavu](https://msdn.microsoft.com/library/azure/ee758710.aspx) nebo [začínáte s vytvářením Vyrovnávání zatížení straně Internetu pro cloudové služby](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Díky sondu agenta hosta označit instance jako není v pořádku?

V případě selhání toorespond s HTTP 200 OK agenta hosta hello značky nástroje pro vyrovnávání zatížení hello hello instance jako reagovat a zastaví odesílání přenosů toothat instance. Nástroj pro vyrovnávání zatížení Hello pokračuje tooping hello instance. Pokud agenta hosta hello odpoví HTTP 200, odešle nástroj pro vyrovnávání zatížení hello provoz toothat instance znovu.

Pokud používáte webovou roli, kód webu hello obvykle běží v w3wp.exe, který není monitorován pomocí hello prostředků infrastruktury Azure nebo agenta hosta. To znamená, že selhání v w3wp.exe (například odpovědi HTTP 500) nebude agent hosta hlášené toohello a nástroj pro vyrovnávání zatížení hello nebude tato instance mimo otočení.

### <a name="http-custom-probe"></a>Vlastní test paměti HTTP

Hello vlastní test paměti pro vyrovnávání zatížení HTTP přepíše hello výchozí hostovaného agenta kontroly, což znamená, že můžete vytvořit vlastní vlastní logiky toodetermine hello stav hello role instance. Nástroj pro vyrovnávání zatížení Hello sondy váš koncový bod každých 15 sekund, ve výchozím nastavení. Hello instance je považován za toobe v otočení nástroje pro vyrovnávání zatížení hello, pokud ji odpoví HTTP 200 v rámci hello časový limit (ve výchozím nastavení je 31 sekund).

To může být užitečné, pokud chcete tooimplement vlastní logiky tooremove instancí z otočení nástroje pro vyrovnávání zatížení. Může například rozhodnout tooremove instance, pokud je vyšší než 90 % využití procesoru a vrátí stav bez 200. Pokud máte webové role, které používají w3wp.exe, to také znamená získáte automatické monitorování vašeho webu, protože selhání ve vašem kódu webu vrátí sondu nástroje pro vyrovnávání zatížení toohello stav bez 200.

> [!NOTE]
> vlastní test paměti Hello HTTP podporuje pouze protokoly HTTP a relativní cesty. Není podporován protokol HTTPS.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Díky HTTP vlastní test paměti označit instance jako není v pořádku?

* Hello HTTP aplikace vrátí kód odpovědi HTTP než 200 (například 403, 404 nebo 500). Toto je kladné potvrzení, která hello aplikace instance by se dostala mimo službu hned.
* Hello HTTP server neodpovídá vůbec po hello časový limit. V závislosti na hodnotě hello vypršení časového limitu, který je nastavený, to může znamenat, že více testu požadavků přejděte nezodpovězená před hello testu získá označeno není spuštěna (který je před SuccessFailCount sondy odesílají).
* Hello server zavře hello připojení přes TCP resetování.

### <a name="tcp-custom-probe"></a>Vlastní test paměti TCP

Sondy TCP iniciovat připojení k provádění třícestné s hello definovaný port.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Díky vlastní test paměti TCP označit instance jako není v pořádku?

* Hello protokolu TCP serveru neodpovídá vůbec po hello časový limit. Hello testu, když je označena jako neběží, závisí na hello počet selhání testu požadavky, které byly nakonfigurované toogo nezodpovězená před označením hello testu jako neběží.
* Test Hello obdrží TCP, obnovit z hello role instance.

Další informace o konfiguraci test stavu protokolu HTTP nebo TCP testu najdete v tématu [začít vytvářet ve Správci prostředků pomocí prostředí PowerShell k pro vyrovnávání zatížení internetového](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Přidání pořádku instancí zpět do otočení nástroje pro vyrovnávání zatížení

Sondy TCP a protokolu HTTP jsou považovány za v pořádku a označit instanci role hello za v pořádku, pokud:

* Nástroj pro vyrovnávání zatížení Hello získá kladné testu hello první čas hello, které se spustí počítač.
* číslo Hello SuccessFailCount (popsanou výš) definuje hodnotu hello úspěšné sondy, které jsou požadované toomark hello instance role jako v pořádku. Pokud role instance byla odebrána, hello počet úspěšný, následných sondy musí rovnat nebo vyšší než hodnota hello SuccessFailCount toomark hello role instance jako spuštěná.

> [!NOTE]
> Pokud je kolísal hello stav instanci role, nástroj pro vyrovnávání zatížení hello již čeká před přepnutím hello role instance zpět ve stavu v pořádku hello. To se provádí prostřednictvím hello infrastruktury a zásad tooprotect hello uživatelů.

## <a name="use-log-analytics-for-load-balancer"></a>Pomocí analýzy protokolů pro vyrovnávání zatížení

Můžete použít [protokolu analýzy nástroje pro vyrovnávání zatížení](load-balancer-monitor-log.md) toocheck na počet hello test stavu stav a kontroly. Protokolování můžete použít s Power BI nebo statistice provozu Azure tooprovide statistiky o stav nástroj pro vyrovnávání zatížení.
