---
title: "aaaOverview zpětné DNS v Azure | Microsoft Docs"
description: "Zjistěte, jak zpětné funguje DNS a jeho použití v Azure"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 687663fb83469ab8e696bb714649d0856915bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Přehled zpětné DNS a podpory v Azure

Tento článek poskytuje přehled o tom, jak zpětné funguje DNS a hello zpětné DNS scénáře podporované v Azure.

## <a name="what-is-reverse-dns"></a>Co je zpětné DNS?

Konvenční záznamy DNS povolte mapování z DNS název (např 'www.contoso.com) tooan IP adresu (například 64.4.6.100).  Zpětné DNS umožňuje hello překlad IP adres (64.4.6.100) back tooa názvu (www.contoso.com).

Zpětné záznamy DNS se používají v různých situacích. Zpětné záznamy DNS se často používá v boje proti spamu e-mailu kontrolou hello odesílatele e-mailové zprávy.  Hello příjmu e-mailu serveru načte hello zpětné záznam hello odesílání IP adresu serveru DNS a ověřuje, pokud hostitele, který je autorizovaný toosend e-mailu z hello pocházející domény. 

## <a name="how-reverse-dns-works"></a>Jak zpětné funguje DNS

Zpětné záznamy DNS jsou hostované v speciální zóny DNS, označuje jako 'ARPA' zóny.  Tyto zóny formuláři samostatné hierarchie DNS paralelně s normální hierarchie hello hostování domény, jako je například "contoso.com".

Například hello záznam DNS "www.contoso.com" je implementovaná pomocí záznam DNS "A" s název hello "www" v zóně hello "contoso.com".  Tento záznam A body toohello odpovídající IP adresy v tomto případě 64.4.6.100.  Hello zpětného vyhledávání je implementovaná samostatně pomocí záznamu, PTR, s názvem '100' v zóně hello '6.4.64.in-addr.arpa' (Upozorňujeme, že jsou v zóny ARPA obrácený IP adresy.)  Tento záznam PTR, pokud byla nakonfigurována správně, body toohello název "www.contoso.com".

Pokud organizace přiřazen blok IP adres, získat jejich hello správné toomanage hello odpovídající ARPA zónu. Hello zóny ARPA odpovídající toohello IP adresu bloky, na které se používají v Azure jsou hostované a spravované společností Microsoft. Poskytovatel internetových služeb může hostovat hello zóny ARPA pro vlastní IP adresy pro vás, nebo může povolíte tooyou hostitele zóny ARPA hello ve službě DNS podle vaší volby, například Azure DNS.

> [!NOTE]
> Dopředného vyhledávání DNS a zpětného vyhledávání DNS jsou implementované v samostatné, paralelní hierarchiích DNS. Hello zpětného vyhledávání pro "www.contoso.com" je **není** hostované v hello zóně "contoso.com", spíše je hostovaná v hello zóny ARPA pro hello odpovídající blok IP adres. Samostatné zóny jsou používány pro bloky adres IPv4 a IPv6.

### <a name="ipv4"></a>IPv4

Název Hello zóny zpětného vyhledávání IPv4 by měl být ve formátu hello: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Například při vytváření zóny zpětného vyhledávání toohost záznamů pro hostitele pomocí IP adresy, které jsou v hello 192.0.2.0/24 předponu, název zóny hello by se vytvořily tak, že izoluje hello sítě předponu adresy hello (192.0.2) a pak obrácení směru hello (2.0.192) a přidání hello přípona `.in-addr.arpa`.

|Podsíť – třída|Předpona sítě  |Předpona odstínech sítě  |Standardní příponu  |Název zóny zpětného vyhledávání |
|-------|----------------|------------|-----------------|---------------------------|
|Třída A|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|Třída B|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|Třída C|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Notace delegování IPv4

V některých případech je menší než třídy C hello rozsah IP adres přidělené tooan organizace (/ 24) rozsahu. V takovém případě rozsah IP hello nespadá na hranici zóny v rámci hello `.in-addr.arpa` zón hierarchii a proto není možné delegovat jako podřízenou zónu.

Místo toho se používá jiný mechanismus řízení tootransfer jednotlivých zpětného vyhledávání (PTR) zaznamenává tooa vyhrazené zóny DNS. Tento mechanismus deleguje podřízenou zónu pro každý rozsah IP adres a potom mapy jednotlivých IP adres v hello rozsahu jednotlivě toothat podřízenou zónu pomocí záznamů CNAME.

Předpokládejme například, že organizace je udělují 192.0.2.128/26 rozsah IP hello jeho poskytovatele internetových služeb. To představuje 64 IP adresy, od 192.0.2.128 too192.0.2.191. Zpětné DNS pro tento rozsah je implementována následujícím způsobem:
- organizace Hello vytvoří zónu zpětného vyhledávání názvem 128-26.2.0.192.in-addr.arpa. Předpona Hello ' 128-26' představuje hello sítě segment přiřazený toohello organizace v rámci hello třídy C (/ 24) rozsahu.
- Hello poskytovatele internetových služeb vytvoří tooset záznamy NS až hello delegování DNS pro hello výše zóny z hello třídy C nadřazené zóny. Vytvoří také záznamy CNAME v zóně zpětného vyhledávání hello nadřazené (třída C), mapování každou IP adresu v hello IP rozsah toohello nové zóny vytvořené hello organizace:

```
$ORIGIN 2.0.192.in-addr.arpa
; Delegate child zone
128-26    NS       <name server 1 for 128-26.2.0.192.in-addr.arpa>
128-26    NS       <name server 2 for 128-26.2.0.192.in-addr.arpa>
; CNAME records for each IP address
129       CNAME    129.128-26.2.0.192.in-addr.arpa
130       CNAME    130.128-26.2.0.192.in-addr.arpa
131       CNAME    131.128-26.2.0.192.in-addr.arpa
; etc
```
- potom Hello organizace spravuje hello jednotlivých záznamů PTR v rámci jejich podřízené zóny.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
Zpětné vyhledávání pro dotazy na IP adresu, 192.0.2.129' hello pro záznam PTR s názvem '129.2.0.192.in-addr.arpa'. Tento dotaz přeloží prostřednictvím hello CNAME v záznam PTR hello nadřazené zóny toohello v hello podřízenou zónu.

### <a name="ipv6"></a>IPv6

Název Hello zóny zpětného vyhledávání IPv6 by měl být v hello následující formulář:`<IPv6 network prefix in reverse order>.ip6.arpa`

Například. Při vytváření zóny zpětného vyhledávání toohost záznamů pro hostitele pomocí IP adresy, které jsou v hello 2001:db8:1000:abdc:: / 64 předpona, název zóny hello by se vytvořily tak, že izoluje hello sítě předponu adresy hello (2001:db8:abdc::). Potom rozbalte hello IPv6 sítě předponu tooremove [nula komprese](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), pokud se jednalo o předpona adresy používané tooshorten hello IPv6 (2001:0db8:abdc:0000::). Reverse hello pořadí, použití tečky jako hello oddělovač mezi každou šestnáctkové číslo v hello předponu, toobuild hello změněn na opačný předpony sítě (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) a přidejte příponu hello `.ip6.arpa`.


|Předpona sítě  |Předpona rozšířené a odstínech sítě |Standardní příponu |Název zóny zpětného vyhledávání  |
|---------|---------|---------|---------|
|2001:DB8:abdc:: / 64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | . ip6.arpa naleznete        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:DB8:1000:9102:: / 64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | . ip6.arpa naleznete        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Podpora Azure pro zpětné DNS

Azure podporuje dva samostatné scénáře týkající se tooreverse DNS:

**Hostování hello zpětného vyhledávání zóny odpovídající tooyour blok IP adres.**
Azure DNS je možné příliš[hostovat vaše zóny zpětného vyhledávání a spravovat záznamy hello PTR pro každý zpětného vyhledávání DNS](dns-reverse-dns-hosting.md)pro oba protokoly IPv4 a IPv6.  Hello proces vytváření zóny zpětného vyhledávání (ARPA) hello, hello delegování, nastavení a konfigurace PTR záznamů je hello stejné jako u regulární zóny DNS.  Hello pouze rozdíly mezi nimi jsou hello delegování musí být nakonfigurované přes vašeho poskytovatele internetových služeb, nikoli registrátora DNS, a slouží pouze hello záznamů typu PTR.

**Nakonfigurujte hello zpětné záznam DNS pro adresu IP hello přiřazenou tooyour služby Azure.** Azure umožňuje příliš[konfigurovat hello zpětného vyhledávání pro hello IP adresy přidělené tooyour služba Azure](dns-reverse-dns-for-azure-services.md).  Tato zpětného vyhledávání je nakonfigurovaný v Azure jako záznam PTR v odpovídající zóny ARPA hello.  Tyto zóny ARPA odpovídající rozsahy IP hello tooall používají v Azure, jsou hostovaná společností Microsoft

## <a name="next-steps"></a>Další kroky

Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Zjistěte, jak příliš[zóny zpětného vyhledávání hello hostitele pro rozsah vašeho poskytovatele internetových služeb přiřadit IP v Azure DNS](dns-reverse-dns-for-azure-services.md).
<br>
Zjistěte, jak příliš[zpětné záznamy DNS pro služeb Azure spravovat](dns-reverse-dns-for-azure-services.md).

