---
title: "aaaWhat je předávání přes Azure a proč ji používat přehled | Microsoft Docs"
description: "Přehled služby Azure Relay"
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 4cfd77048210a435c446b908b7896737cad0edbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-relay"></a>Co je Azure Relay?

Hello předávání přes Azure service usnadňuje hybridní aplikace povolením toosecurely můžete vystavit služby, které se nacházejí v podnikové síti toohello veřejného cloudu, bez nutnosti tooopen připojení brány firewall, nebo vyžadovat nežádoucí změny tooa podnikové síťové infrastruktury. Služba Relay podporuje různé přenosové protokoly a standardy webových služeb.

Hello předávací služba podporuje tradiční jednosměrný, požadavků a odpovědí a provoz peer-to-peer. Také podporuje distribuci událostí na úrovni Internetu tooenable publikování a přihlášení k odběru scénáře a obousměrnou soketovou komunikací pro zvýšenou point-to-point efektivitu. 

V hello přes předávací službu vzorek přenos dat k místní službě toohello předávací služba se připojuje přes odchozí port a vytvoří obousměrný soket pro komunikaci vázanou tooa konkrétní potkávací adresu. Hello Klient pak může komunikovat s místní službou hello odesláním provoz toohello předávací služba cílení hello potkávací adresa. Hello předávací služba pak "předává" místní služba toohello data prostřednictvím klienta vyhrazené tooeach obousměrný soket. Hello klient nepotřebuje přímé spojení toohello místní služby, není požadovaná tooknow kde hello služba nachází, a hello lokální služba nepotřebuje mít žádné příchozí porty otevřít v bráně firewall hello.

elementy Hello klíče schopnosti poskytované předávání jsou obousměrný, bez vyrovnávací paměti komunikaci napříč síťovými hranicemi s TCP jako omezení, koncový bod zjišťování, stavu připojení a jako překryvný obrázek zabezpečení koncového bodu. funkce předávání Hello se liší od technologie integrace na úrovni sítě, například ze sítě VPN, v této předávání může být koncový bod vymezená tooa jednu aplikaci na jednom počítači, technologie VPN je mnohem více obtěžující, jako je závislé na změnu hello síťového prostředí .

Azure Relay má dvě funkce:

1. [Hybridní připojení](#hybrid-connections) – používá hello otevřete standardní webové sokety povolení s více platformami.
2. [Předávací službu WCF](#wcf-relays) -používá Windows Communication Foundation (WCF) tooenable vzdáleného volání procedur. Předávání WCF je starší verze přenosového hello nabídky, které již využívají mnoho zákazníků s jejich WCF programovací modely.

Hybridní připojení a předávací službu WCF umožňují tooassets zabezpečené připojení, který neexistuje v podnikové síti. Použití jednoho přes hello jiných je závislá na konkrétních potřeb, jak je popsáno v následující tabulce hello:

|  | WCF Relay | Hybridní připojení |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS** | |x |
| **Otevřený protokol založený na standardech** | |x |
| **Několik programovacích modelů protokolu RPC** | |x |

## <a name="hybrid-connections"></a>Hybridní připojení

Hello [Azure předávání hybridní připojení](relay-hybrid-connections-protocol.md) funkce je zabezpečené, otevřete protokol vývoj hello existující funkce předávání, které se dají implementovat na jakékoli platformě a v libovolném jazyce, který má základní funkce protokolu WebSocket, který explicitně zahrnuje hello WebSocket API v běžné webových prohlížečů. Hybridní připojení jsou založená na protokolech HTTP a WebSocket.

### <a name="service-history"></a>Historie služby

Hybridní připojení supplants hello dřívějším, podobně jako s názvem "BizTalk Services" funkce, který byl postavený na hello předávání přes Azure Service Bus WCF. Nová funkce hybridních připojení Hello doplňuje stávající funkce předávání WCF hello a možnosti tyto dvě služby existují souběžného ve službě předávání přes Azure hello. Sdílejí sice společnou bránu, jinak se ale jedná o rozdílné implementace.

## <a name="wcf-relays"></a>Přenosy WCF

Hello WCF předávání funguje pro hello úplné rozhraní .NET Framework (NETFX) a pro WCF. Zahájení hello připojení mezi místní službou a předávací službou vytvoříte hello pomocí skupiny "předávání" vazeb WCF. Pozadí se děje hello hello předávací vazby mapují toonew přenosu vazby prvky určené toocreate komponentů kanálu WCF které se integrují se službou Service Bus v cloudu hello.

## <a name="architecture-processing-of-incoming-relay-requests"></a>Architektura: Zpracování příchozích požadavků na předání
Když klient odešle požadavek toohello [předávání přes Azure](/azure/service-bus-relay/) služby pro vyrovnávání zatížení Azure hello směruje tooany uzlů brány hello. Pokud hello jedná o požadavek na poslech, uzel brány hello vytvoří nové propojení. Pokud je požadavek hello konkrétnímu propojení tooa žádost o připojení, hello uzel brány předá hello připojení žádost toohello uzel brány, který vlastní hello propojení. Hello uzel brány, který vlastní hello propojení, odešle potkávací požadavek toohello naslouchání, toocreate hello naslouchací proces s dotazem, uzel brány toohello dočasný kanál, který obdržel požadavek na připojení hello.

Když se vytvoří předávací spojení hello, hello klienti si můžou vyměňovat zprávy přes uzel brány hello, který se používá pro potkávací hello.

![Zpracování příchozích událostí požadavků na předání WCF](./media/relay-what-is-it/ic690645.png)

## <a name="next-steps"></a>Další kroky

* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)

