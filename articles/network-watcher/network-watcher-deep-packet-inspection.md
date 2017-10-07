---
title: "aaaPacket kontroly pomocí sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak se shromažďují toouse sledovací proces sítě tooperform hloubkové kontroly paketů z virtuálního počítače"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4aeddcd482edc4df3d63e87b5c4b0788c540850b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Kontroly paketů s sledovací proces sítě Azure

Pomocí funkce zachytávání paketů hello sledovací proces sítě, můžete zahájit a spravovat relace zachycení na virtuální počítače Azure z hello portálu, prostředí PowerShell, rozhraní příkazového řádku a programově prostřednictvím hello SDK a REST API. Zachytáváním paketů umožňuje tooaddress scénáře, které vyžadují dat na úrovni paketů tím, že poskytuje hello informace ve snadno použitelného formátu. Využití dat hello tooinspect volně dostupných nástrojů, můžete prozkoumat komunikace tooand odeslaný virtuální počítače a proniknout do vašeho síťového provozu. Některé příklad použití paketu zachycení dat patří: příčin problémů sítě nebo aplikace, zjišťování sítě pokusy o zneužití a narušení nebo udržování dodržování legislativní předpisů. V tomto článku jsme ukazují, jak tooopen souboru zachytávání paketů poskytuje sledovací proces sítě pomocí nástroje populární open source. Poskytujeme také příklady, jak identifikovat neobvyklé provoz toocalculate latence připojení, a zkontrolujte síťových statistiky.

## <a name="before-you-begin"></a>Než začnete

Tento článek probírá některé scénáře předem nakonfigurovaná na zachytáváním paketů, která byla dříve spuštěna. Tyto scénáře ukazují možnosti, které jsou přístupné kontrolou zachytáváním paketů. Tento scénář používá [WireShark](https://www.wireshark.org/) zachytáváním paketů tooinspect hello.

Tento scénář předpokládá, že jste již spustili zachytáváním paketů na virtuálním počítači. jak toocreate zachytáváním paketů navštivte toolearn [zachytávali spravovat pakety hello portál](network-watcher-packet-capture-manage-portal.md) nebo se zbytkem navštivte stránky [Správa Zachytávali pakety REST API](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Scénář

V tomto scénáři můžete:

* Zkontrolujte zachytávání paketů

## <a name="calculate-network-latency"></a>Vypočítat latence sítě

V tomto scénáři ukážeme, jak tooview hello počáteční odezvy čas požadavku protokolu TCP (Transmission Control) konverzace, ke kterým dochází mezi dva koncové body.

Po vytvoření připojení protokolu TCP, hello první tři paketů odeslaných za hello připojení postupujte podle vzoru obvykle označuje tooas hello třícestný metody handshake. Prověřením hello první dva paketů odeslaných za této metody handshake, úvodního požadavku od klienta hello a odpověď ze serveru hello jsme můžete vypočítat hello latence při toto připojení bylo vytvořeno. Tato čekací doba je odkazované tooas hello doba odezvy (požadavku). Další informace o protokolu TCP hello a hello třícestné najdete v části toohello následujících prostředků. https://support.microsoft.com/en-US/Help/172983/Explanation-of-the-three-way-handshake-VIA-TCP-IP

### <a name="step-1"></a>Krok 1

Spusťte WireShark

### <a name="step-2"></a>Krok 2

Zatížení hello **CAP** soubor z vaší zachytáváním paketů. Tento soubor nachází v objektu blob hello se uloží do našich místně na virtuální počítač hello, v závislosti na tom, jak jste nakonfigurovali.

### <a name="step-3"></a>Krok 3

tooview hello počáteční odezvy čas požadavku v TCP konverzace, jsme bude pouze vidí první dva pakety hello zahrnutých v hello TCP handshake. Použijeme hello první dva pakety hello třícestné, kde jsou hello [SYN], [SYN, ACK] paketů. Že jsou pojmenované pro nastaveny v hlavičce protokolu TCP hello příznaky. v tomto scénáři se nepoužijí Hello poslední paketů v hello handshake, hello [ACK] paketů. hello klient odešle paket Hello [SYN]. Po přijetí je hello server odešle paket hello [ACK] jako potvrzení přijetí hello SYN z klienta hello. Využití hello fakt, že odpověď serveru hello vyžaduje velmi malé nároky, jsme vypočítat hello odeslání požadavku odečtením hello době hello [SYN, ACK] paketu byla přijata klientem hello hello čas [SYN] paket klientem hello.

Pomocí WireShark tato hodnota je vypočítána pro nás.

první dva pakety hello toomore snadno zobrazit v hello TCP třícestné, jsme, že bude využívat hello filtrování schopnosti poskytované WireShark.

Filtr hello tooapply v WireShark, rozbalte hello "Transmission Control Protocol" Segment paketu [SYN] na vašem snímku a prozkoumejte nastaveny v hlavičce protokolu TCP hello příznaky hello.

Vzhledem k tomu, že jsme hledáte toofilter na všechny [SYN] a [SYN, ACK] paketů, v části Příznaky cofirm, že je hello Syn bit nastavený too1, a potom klikněte pravým tlačítkem na hello Syn bit -> použít jako filtr -> vybrané.

![Obrázek 7][7]

### <a name="step-4"></a>Krok 4

Teď, když jste provedli filtrování hello okno tooonly najdete pakety s nastaven bit hello [SYN], můžete snadno vybrat konverzace, které vás zajímají tooview hello počáteční požadavku. Jednoduchý způsob tooview hello požadavku v WireShark jednoduše klikněte na rozevírací hello označena analysis "SEQ/ACK". Zobrazí se text hello, zobrazí požadavku. V takovém případě hello požadavku byla 0.0022114 sekund nebo 2.211 ms.

![Obrázek 8][8]

## <a name="unwanted-protocols"></a>Nežádoucí protokoly

Můžete mít mnoho aplikací běžících na instanci virtuálního počítače, které jste nasadili v Azure. Mnoho z těchto aplikací komunikují přes síť hello, možná bez explicitního oprávnění. Pomocí paketu zachycení toostore síťové komunikace, jsme prozkoumejte jak aplikace mluvíme v síti hello a vyhledejte všechny problémy.

V tomto příkladu jsme Zkontrolujte předchozí spustili zachytáváním paketů pro nežádoucí protokoly, které můžou signalizovat neověřené komunikace z aplikace běžící na vašem počítači.

### <a name="step-1"></a>Krok 1

Pomocí hello stejné zachycení v hello předchozí scénář, klikněte na tlačítko **statistiky** > **protokol hierarchie**

![protokol hierarchie nabídky][2]

Zobrazí se okno hierarchie protokol Hello. Toto zobrazení obsahuje seznam všech hello protokolů, které byly používány během relace zachytávání hello a hello počet paketů odeslaných a přijatých pomocí protokolů hello. Toto zobrazení může být užitečné pro hledání nežádoucí síťový provoz na virtuální počítače nebo v síti.

![protokol hierarchie otevřít][3]

Jak vidíte v hello následující snímek obrazovky, se přenosů dat pomocí hello BitTorrent protokol, který se používá pro sdílení souborů toopeer partnera. Jako správce neočekáváte toosee BitTorrent provozu na tomto konkrétní virtuální počítače. Nyní si vědom tento provoz odebráním hello sdílené toopeer software, který nainstalovaný na tento virtuální počítač nebo hello bloku přenosů pomocí skupin zabezpečení sítě nebo brána Firewall. Kromě toho může zvolit toorun paketu zachycení podle plánu, tak hello použití protokolu na virtuálních počítačích můžete zkontrolovat pravidelně. Příklad na jak tooautomate sítě úlohy v azure naleznete [monitorování síťových prostředků pomocí azure automation](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>Hledání hlavní cíle a porty

Principy typů hello provozu, hello koncových bodů a přenášená přes porty hello je důležité při sledování nebo řešení potíží s aplikací a prostředkům v síti. Použití souboru zachytávání paketů z výše, jsme můžete rychle zjistěte hello Nejoblíbenější cíle naše virtuální počítač komunikuje s a hello porty využívání.

### <a name="step-1"></a>Krok 1

Pomocí hello stejné zachycení v hello předchozí scénář, klikněte na tlačítko **statistiky** > **IPv4 statistiky** > **cíle a porty**

![okno zachytávání paketů][4]

### <a name="step-2"></a>Krok 2

Jako podíváme prostřednictvím hello výsledky vystupoval řádek, nebyly k dispozici více připojení na portu 111. Hello nejvíce používá port byla 3389, což je Vzdálená plocha a zbývající hello jsou dynamické porty RPC.

Tento provoz může to znamenat nic, je port, který byl použit pro mnoho připojení a neznámé toohello oprávnění správce.

![Obrázek 5][5]

### <a name="step-3"></a>Krok 3

Teď, když jsme určili out of místní port jsme můžete filtrovat naší zachycení na hello port.

Filtr Hello v tomto scénáři by byl:

```
tcp.port == 111
```

Jsme zadejte text filtru hello z výše v textovém poli filtru hello a stisknutí klávesy enter.

![Obrázek 6][6]

Z výsledků hello uvidíme veškerý provoz hello pochází z místní virtuální počítač na hello stejné podsíti. Pokud stále nemáte Chápeme, proč dochází k tento provoz, jsme můžete prohlédnout další toodetermine pakety hello Proč v provádění těchto volání na portu 111. Tyto informace budeme moci provést vhodnou akci hello.

## <a name="next-steps"></a>Další kroky

Další informace o hello jiné diagnostické funkce sledovací proces sítě navštivte stránky [Přehled monitorování sítě Azure](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













