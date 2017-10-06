---
title: "Přehled delegování DNS aaaAzure | Microsoft Docs"
description: "Pochopte, jak se toochange delegování domény a použití Azure DNS název, hostování domény tooprovide servery."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: eaf2d2e345004b4d631e8d81d548b8ca23277d05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delegation-of-dns-zones-with-azure-dns"></a>Delegování zón DNS s využitím Azure DNS

Azure DNS vám umožní toohost zóny DNS a spravovat hello záznamy DNS pro doménu v Azure. Aby dotazy DNS pro domény tooreach Azure DNS, má doména hello toobe delegovaná z nadřazené domény hello tooAzure DNS. Mějte na paměti Azure DNS není doménový Registrátor hello. Tento článek vysvětluje, jak funguje delegování domény a tooAzure toodelegate domén DNS.

## <a name="how-dns-delegation-works"></a>Jak funguje delegování DNS

### <a name="domains-and-zones"></a>Domény a zóny

Hello Domain Name System je hierarchie domén. Hello hierarchie začíná od domény "kořenový" hello, jejíž název je jednoduše "**.**'.  Následují domény nejvyšší úrovně, jako jsou „com“, „net“, „org“, „uk“ nebo „jp“.  Pod doménami nejvyšší úrovně jsou domény druhé úrovně, jako jsou „org.uk“ nebo „co.jp“.  A tak dále. Hello domén v hello hierarchii DNS jsou hostované pomocí oddělených zón DNS. Tyto zóny jsou globálně distribuované a hostované názvovými servery DNS po hello, world.

**Zóna DNS** -doména je jedinečný název v Domain Name systemu, například "contoso.com" hello. Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény. Například hello doména "contoso.com" může obsahovat několik záznamů DNS, třeba "mail.contoso.com" (pro poštovní server) a "www.contoso.com" (pro webový server).

**Doménový registrátor** – Doménový registrátor je společnost, která poskytuje názvy internetových domén. Jejich ověřte hello internetové domény. Chcete-li toouse je k dispozici a umožňují toopurchase ho. Po registraci názvu domény hello jste hello právní vlastníka pro název domény hello. Pokud již máte internetové domény, budete používat hello aktuální domény registrátora toodelegate tooAzure DNS.

Další informace na kdo vlastní určitý název domény nebo informace o tom toofind toobuy domény, najdete v části [Správa internetových domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### <a name="resolution-and-delegation"></a>Překládání a delegování

Existují dva typy serverů DNS:

* *Autoritativní* server DNS hostí zóny DNS. Odpovídá pouze na dotazy DNS pro záznamy v těchto zónách.
* *Rekurzivní* server DNS nehostuje zóny DNS. Odpovídá na všechny dotazy DNS voláním autoritativních DNS serverů toogather hello data, která potřebuje.

Azure DNS poskytuje autoritativní službu DNS.  Neposkytuje rekurzivní službu DNS. Cloudové služby a virtuálních počítačů v Azure jsou automaticky konfigurované toouse rekurzivní služba DNS, která je poskytována samostatně jako součást infrastruktury Azure a. Informace o tom, jak toochange tato nastavení DNS, najdete v části [překlad v Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

Klient DNS v počítačů nebo mobilních zařízení většinou volání tooperform serveru DNS rekurzivní všechny dotazy DNS klientské aplikace hello.

Pokud rekurzivní server DNS obdrží dotaz na záznam DNS, třeba "www.contoso.com", nejprve musí toofind hello název serveru hostingu hello zónu pro doménu "contoso.com" hello. toofind hello název serveru, začíná hello kořenových názvových serverů a vyhledá názvové servery hello hostují zónu "com" hello. Následně se dotazuje hello "com" název servery toofind hello názvové servery hostují zónu "contoso.com" hello.  Nakonec je možné tooquery tyto názvové servery pro "www.contoso.com".

Tento postup se nazývá překlad názvu DNS hello. Přesněji řečeno překlad DNS zahrnuje ještě další kroky, třeba sledování záznamů CNAME, ale není důležité toounderstanding jak funguje delegování DNS.

Jak nadřazená zóna "ukáže" toohello názvové servery pro podřízenou zónu? Používá k tomu speciální typ záznamu DNS, který se nazývá záznam NS (NS zastupuje „názvový server“). Například hello kořenová zóna obsahuje záznamy NS pro "com" a ukazuje hello názvové servery pro zónu "com" hello. Hello zónu "com" pak obsahuje záznamy NS pro "contoso.com", který ukazuje hello názvové servery pro zónu "contoso.com" hello. Nastavení hello záznamy NS pro podřízenou zónu v nadřazené zóně, se nazývá delegování domény hello.

Hello následující obrázek znázorňuje příklad dotazu DNS. Hello contoso.net a partners.contoso.net jsou Azure DNS zóny.

![Názvový server DNS](./media/dns-domain-delegation/image1.png)

1. požadavky na klienta Hello `www.partners.contoso.net` ze své místní server DNS.
1. místní server DNS Hello nemá hello záznam, umožňuje žádost tootheir kořenový server.
1. kořenový server Hello nemá hello záznam, ale zná adresu hello hello `.net` název serveru, poskytuje tento server DNS toohello adresa
1. Hello DNS odešle žádost toohello hello `.net` název serveru, nemá záznam hello však znát adresu hello hello contoso.net název serveru. V tomto případě je to zóna DNS hostovaná v Azure DNS.
1. zóny Hello `contoso.net` nemá hello záznam, ale zná hello název serveru pro `partners.contoso.net` a který odpoví. V tomto případě je to zóna DNS hostovaná v Azure DNS.
1. Hello DNS server žádost o adresu IP hello `partners.contoso.net` z hello `partners.contoso.net` zóny. Obsahuje záznam A hello a odpoví hello IP adresu.
1. Hello DNS server poskytuje hello IP adresu toohello klienta
1. Hello klient připojí toohello webu `www.partners.contoso.net`.

Každé delegování má ve skutečnosti dvě kopie záznamů NS hello; jednu v nadřazené zóně hello odkazující podřízené toohello a druhou v samotné podřízené zóně hello. zónu "contoso.net" Hello obsahuje záznamy hello NS pro "contoso.net" (v přidání toohello záznamů NS v "net"). Tyto záznamy se nazývají záznamy autoritativních NS a nacházejí se na vrcholu hello hello podřízenou zónu.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-delegate-domain-azure-dns.md)

