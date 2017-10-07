---
title: "aaaConfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat podpory služby Virtual Network pro vaše instance služby Azure Redis Cache úrovně Premium"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: sdanie
ms.openlocfilehash: fab715f4d9365ee4c2f8b89d2e2e58768c25b671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Jak tooconfigure podpora virtuální sítě pro mezipaměť Azure Redis Cache Premium
Azure Redis Cache má jiný mezipaměti nabídky, které poskytují flexibilitu při výběru hello velikost mezipaměti a funkcí, včetně funkce úrovně Premium, jako je clustering, trvalosti a podpory služby virtual network. Virtuální síť je privátní síť v cloudu hello. Pokud instanci služby Azure Redis Cache je konfigurován s virtuální síť, není veřejně adresovatelné a můžete přistupovat pouze z virtuálních počítačů a aplikací v rámci hello virtuální sítě. Tento článek popisuje, jak podporují tooconfigure virtuální sítě pro instanci služby Azure Redis Cache premium.

> [!NOTE]
> Azure Redis Cache podporuje obě classic a Resource Manager virtuální sítě.
> 
> 

Informace o dalších funkcí mezipaměti premium najdete v tématu [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Proč virtuální síť?
[Virtuální síť Azure (VNet)](https://azure.microsoft.com/services/virtual-network/) nasazení poskytuje lepší zabezpečení a izolaci pro vaši Azure Redis Cache, stejně jako podsítě, zásady řízení přístupu a další funkce toofurther omezení přístupu.

## <a name="virtual-network-support"></a>Podpora virtuální sítě
Podpora služby Virtual Network (VNet) je nakonfigurován na hello **nová mezipaměť Redis** okno při vytvoření mezipaměti. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Jakmile vyberete cenová úroveň premium integrace Redis virtuální sítě můžete nakonfigurovat tak, že vyberete virtuální síť, která je v hello stejném předplatném a umístění jako mezipaměť. toouse nové sítě VNet, je nejprve vytvořte pomocí následujících kroků hello v [vytvoření virtuální sítě pomocí portálu Azure hello](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [vytvoření virtuální sítě (klasické) pomocí portálu Azure hello](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) a pak se vraťte toohello **Nová mezipaměť Redis** toocreate okna a konfiguraci mezipaměti premium.

tooconfigure hello virtuální sítě pro nové mezipaměti, klikněte na tlačítko **virtuální sítě** na hello **nová mezipaměť Redis** okno a vyberte hello požadovaného virtuální sítě z rozevíracího seznamu hello.

![Virtuální síť][redis-cache-vnet]

Vyberte hello požadované podsíť z hello **podsíť** rozevíracího seznamu a zadejte hello potřeby **statická IP adresa**. Pokud používáte klasickou virtuální síť hello **statická IP adresa** pole je volitelné, a pokud není zadaný žádný, jedna je vybrána z hello vybrané podsítě.

> [!IMPORTANT]
> Při nasazení Azure Redis Cache tooa virtuální sítě Resource Manageru, hello mezipaměti musí být ve vyhrazené podsíť, která obsahuje žádné další prostředky s výjimkou instance služby Azure Redis Cache. Pokud je proveden pokus o toodeploy Azure Redis Cache tooa tooa podsíť virtuální sítě Resource Manageru, která obsahuje jiné prostředky, hello nasazení se nezdaří.
> 
> 

![Virtuální síť][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure si vyhrazuje některé IP adresy v rámci každé podsítě a tyto adresy nelze použít. Hello první a poslední IP adres podsítě hello jsou vyhrazené pro protokol shoda, společně s tři další adresy používané pro služby Azure. Další informace najdete v tématu [existují nějaká omezení na pomocí IP adresy v rámci těchto podsítí?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Přidání toohello IP adresy používané hello infrastruktury virtuální sítě Azure každý Redis instance v hello podsíť používá dvě IP adresy na horizontální oddíl a jednu IP adresu další nástroje pro vyrovnávání zatížení hello. Mezipaměť vypojené z clusteru se považuje za jeden horizontálního oddílu toohave.
> 
> 

Po vytvoření mezipaměti hello hello konfiguraci pro hello virtuální sítě můžete zobrazit kliknutím na **virtuální sítě** z hello **prostředků nabídky**.

![Virtuální síť][redis-cache-vnet-info]

tooconnect tooyour Azure Redis cache instance při použití virtuální síť, zadejte název hostitele hello svojí mezipaměti hello připojovacího řetězce, jak je znázorněno v hello následující ukázka:

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>VNet Azure Redis Cache – nejčastější dotazy
Hello následující seznam obsahuje odpovědi toocommonly kladené dotazy týkající se hello škálování Azure Redis Cache.

* [Jaké jsou některé běžné chybné konfigurace problémy s Azure Redis Cache a virtuální sítě?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [Jak můžete ověřit, že moje mezipaměti funguje ve virtuální síti?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [Můžete použít virtuální sítě s standardní nebo základní mezipaměti?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Proč vytvoření mezipaměti Redis selhání v některé podsítě a jiné ne?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Jaké jsou požadavky na místo na hello podsíť adres?](#what-are-the-subnet-address-space-requirements)
* [Fungují všechny funkce mezipaměti při hostování mezipaměti ve virtuální síti?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Jaké jsou některé běžné chybné konfigurace problémy s Azure Redis Cache a virtuální sítě?
Pokud Azure Redis Cache je hostovaná ve virtuální síti, se použijí hello porty hello následující tabulky. 

>[!IMPORTANT]
>Pokud jsou zablokované hello porty hello následující tabulky, mezipaměti hello nemusí správně fungovat. Nejméně jeden z těchto portů blokované je hello nejběžnější chybné konfigurace problém při použití Azure Redis Cache ve virtuální síti.
> 
> 

- [Požadavky na odchozí port](#outbound-port-requirements)
- [Požadavky na portu pro příchozí spojení](#inbound-port-requirements)

### <a name="outbound-port-requirements"></a>Požadavky na odchozí port

Existuj sedm požadavků odchozí port.

- V případě potřeby všechny toohello odchozí připojení, které internet můžete provedeny prostřednictvím klienta místní auditování zařízení.
- Tři porty hello směrovat provoz tooAzure koncové body údržby Azure Storage a Azure DNS.
- Hello zbývající rozsahy portů a pro interní komunikaci podsítě Redis. Pravidla NSG žádné podsítě jsou požadovány pro interní komunikaci podsítě Redis.

| Port(y) pro | Směr | Přenosový protokol | Účel | Místní IP | Vzdálené IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Odchozí |TCP |Redis závislosti na Azure Storage nebo infrastruktury veřejných KLÍČŮ (Internet) | (Redis podsíť) |* |
| 53 |Odchozí |TCP/UDP |Redis závislosti ve službě DNS (Internet nebo VNet) | (Redis podsíť) |* |
| 8443 |Odchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) | (Redis podsíť) |
| 10221-10231 |Odchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) | (Redis podsíť) |
| 20226 |Odchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) |(Redis podsíť) |
| 13000-13999 |Odchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) |(Redis podsíť) |
| 15000-15999 |Odchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) |(Redis podsíť) |


### <a name="inbound-port-requirements"></a>Požadavky na portu pro příchozí spojení

Nejsou osm požadavky rozsah portu pro příchozí spojení. Příchozí požadavky do tohoto rozsahu buď jsou příchozí z jiné služby hostované v hello stejnou virtuální síť nebo interní toohello Redis podsíť komunikace.

| Port(y) pro | Směr | Přenosový protokol | Účel | Místní IP | Vzdálené IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Příchozí |TCP |Vyrovnávání zatížení Azure tooRedis komunikaci klienta, | (Redis podsíť) |Virtuální síť, nástroj pro vyrovnávání zatížení Azure |
| 8443 |Příchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) |(Redis podsíť) |
| 8500 |Příchozí |TCP/UDP |Vyrovnávání zatížení Azure | (Redis podsíť) |Nástroj pro vyrovnávání zatížení Azure |
| 10221-10231 |Příchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) |(Redis podsíť), nástroj pro vyrovnávání zatížení Azure |
| 13000-13999 |Příchozí |TCP |Klientské komunikace tooRedis clustery Vyrovnávání zatížení Azure | (Redis podsíť) |Virtuální síť, nástroj pro vyrovnávání zatížení Azure |
| 15000-15999 |Příchozí |TCP |Vyrovnávání zatížení clusterů, Azure tooRedis komunikace klienta | (Redis podsíť) |Virtuální síť, nástroj pro vyrovnávání zatížení Azure |
| 16001 |Příchozí |TCP/UDP |Vyrovnávání zatížení Azure | (Redis podsíť) |Nástroj pro vyrovnávání zatížení Azure |
| 20226 |Příchozí |TCP |Interní komunikaci pro Redis | (Redis podsíť) |(Redis podsíť) |

### <a name="additional-vnet-network-connectivity-requirements"></a>Další požadavky na připojení sítě VNET

Existují požadavky na síťové připojení pro Azure Redis Cache, nemusí být splněny původně ve virtuální síti. Azure Redis Cache vyžaduje že všechny hello následující položky toofunction správně, pokud se používá v rámci virtuální sítě.

* Odchozí připojení tooAzure úložiště koncových bodů sítě po celém světě. To zahrnuje koncové body, které jsou umístěné v hello stejné oblasti jako hello instanci Azure Redis Cache, stejně jako koncové body úložiště, které jsou umístěné v **jiných** oblastech Azure. Koncové body Azure úložiště vyřešit v rámci hello následující domény DNS: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*a *file.core.windows.net*. 
* Odchozí připojení k síti příliš*ocsp.msocsp.com*, *mscrl.microsoft.com*, a *crl.microsoft.com*. Toto připojení je potřebné toosupport SSL funkce.
* Hello konfiguraci DNS pro virtuální síť hello musí být schopen překladu hello koncové body a domény uvedený v hello starší body. Zajištěním platný infrastruktura DNS je nakonfigurován a udržovat hello virtuální sítě můžete splnit tyto požadavky na DNS.
* Odchozí síťové připojení toohello následující Azure Monitoring koncových bodů, které řešení v části hello následující domény DNS: shoebox2 black.shoebox2.metrics.nsatc.net, severní prod2.prod2.metrics.nsatc.net azglobal-black.azglobal.metrics.nsatc.net, shoebox2-red.shoebox2.metrics.nsatc.net prod2.prod2.metrics.nsatc.net – východ, azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Jak můžete ověřit, že moje mezipaměti funguje ve virtuální síti?

>[!IMPORTANT]
>Při připojování tooan instanci Azure Redis Cache, který je hostován ve virtuální síti, hello mezipaměti, které klienti musí být ve stejné virtuální síti, včetně všech testovací aplikace a diagnostické nástroje příkaz ping.
>
>

Po nakonfigurování hello port požadavky popsané v předchozí části hello můžete ověřit, že mezipaměť funguje tak, že provedete následující kroky hello.

- [Restartovat](cache-administration.md#reboot) všechny hello mezipaměti uzly. Podle potřeby všechny hello závislosti mezipaměti, není dosažitelná (jak je uvedeno v [příchozí požadavky na porty](cache-how-to-premium-vnet.md#inbound-port-requirements) a [odchozí port požadavky](cache-how-to-premium-vnet.md#outbound-port-requirements)), hello mezipaměť nebude možné toorestart úspěšně.
- Po restartování uzly mezipaměti hello (vykazované hello mezipaměti stav v hello portál Azure), můžete provést hello následující testy:
  - příkaz ping hello koncový bod mezipaměti (pomocí portu 6380) z počítače, který je v rámci hello stejnou virtuální síť jako hello mezipaměti, pomocí [tcping](https://www.elifulkerson.com/projects/tcping.php). Například:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Pokud hello `tcping` nástroj hlásí, že je otevřený hello port, hello mezipaměti je k dispozici pro připojení z klientů ve hello virtuální sítě.

  - Tootest jiný způsob, jak je toocreate testovacího mezipaměti klienta (které by mohly být jednoduchou konzolovou aplikaci pomocí StackExchange.Redis), připojuje toohello mezipaměti a přidá a načte některé položky z mezipaměti hello. Nainstalujte hello vzorku klientskou aplikaci na virtuální počítač, který je v hello stejnou virtuální síť jako hello mezipaměti a potom ho spusťte tooverify připojení toohello mezipaměti.


### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Můžete použít virtuální sítě s standardní nebo základní mezipaměti?
Virtuální sítě můžete použít pouze u prémiových mezipamětí.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Proč vytvoření mezipaměti Redis selhání v některé podsítě a jiné ne?
Pokud provádíte nasazení Azure Redis Cache tooa virtuální sítě Resource Manageru, hello mezipaměti musí být ve vyhrazené podsíť, která obsahuje jiný typ prostředku. Pokud je proveden pokus o toodeploy Azure Redis Cache tooa podsíť virtuální sítě Resource Manageru, který obsahuje jiné prostředky, hello nasazení se nezdaří. Před vytvořením nové mezipaměti Redis, je nutné odstranit existující prostředky hello uvnitř hello podsítě.

Můžete nasadit více typů z prostředků tooa klasické virtuální síti, dokud máte dost IP adres k dispozici.

### <a name="what-are-hello-subnet-address-space-requirements"></a>Jaké jsou požadavky na místo na hello podsíť adres?
Azure si vyhrazuje některé IP adresy v rámci každé podsítě a tyto adresy nelze použít. Hello první a poslední IP adres podsítě hello jsou vyhrazené pro protokol shoda, společně s tři další adresy používané pro služby Azure. Další informace najdete v tématu [existují nějaká omezení na pomocí IP adresy v rámci těchto podsítí?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Přidání toohello IP adresy používané hello infrastruktury virtuální sítě Azure každý Redis instance v hello podsíť používá dvě IP adresy na horizontální oddíl a jednu IP adresu další nástroje pro vyrovnávání zatížení hello. Mezipaměť vypojené z clusteru se považuje za jeden horizontálního oddílu toohave.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Fungují všechny funkce mezipaměti při hostování mezipaměti ve virtuální síti?
Pokud vaše mezipaměť je součástí virtuální sítě, můžete přístup jenom pro klienty v hello virtuální síť hello mezipaměti. V důsledku toho hello následující funkce správy mezipaměti nefungují v tuto chvíli.

* Konzola redis – vzhledem k tomu konzola Redis běží v prohlížeči místní, což je mimo hello virtuální sítě, se nemůže připojit tooyour mezipaměti.

## <a name="use-expressroute-with-azure-redis-cache"></a>Pomocí ExpressRoute s Azure Redis Cache
Zákazníci mohou připojit [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) okruhu tootheir virtuální síťové infrastruktury, tak rozšířit jejich místní sítě tooAzure. 

Ve výchozím nastavení, nově vytvořený okruh ExpressRoute neprovádí vynucené tunelování (inzerování výchozí trasy, 0.0.0.0/0) na virtuální síť. V důsledku toho odchozí připojení k Internetu je povoleno přímo ze hello virtuální sítě a klientských aplikací jsou možné tooconnect tooother Azure včetně Azure Redis Cache koncové body.

Obvyklé konfigurace zákazníka je ale toouse vynuceného tunelování (Inzerovat výchozí trasu) který vynutí odchozí internetové přenosy tooinstead toku místně. Tento tok provozu rozdělí připojení s Azure Redis Cache, pokud je odchozí přenosy hello potom blokovaný místně, tak, aby hello instanci Azure Redis Cache není možné toocommunicate s jeho závislé součásti.

řešení Hello je toodefine jeden (nebo více) trasy definované uživatelem (udr) v podsíti hello, který obsahuje hello Azure Redis Cache. UDR definuje tras konkrétní podsítě, které bude dodržení místo hello výchozí trasu.

Pokud je to možné doporučujeme toouse hello následující konfigurace:

* Konfigurace ExpressRoute Hello inzeruje 0.0.0.0/0 a ve výchozím nastavení vynucené tunelových propojení všechny odchozí přenosy na místě.
* Hello UDR použít toohello podsíť obsahující hello Azure Redis Cache definuje 0.0.0.0/0 s pracovní postup pro toohello provoz protokolu TCP/IP veřejného Internetu; například podle nastavení hello dalšího směrování too'Internet typu '.

Hello celkové požadavky tyto kroky je úrovni podsítě hello UDR má přednost před hello ExpressRoute vynuceného tunelování, čímž zajišťuje odchozí přístup k Internetu z hello Azure Redis Cache.

Připojování tooan Azure Redis Cache instance z místní aplikace pomocí služby ExpressRoute není scénáři typické použití kvůli tooperformance důvodů (pro nejlepší výkon Azure Redis Cache musí být klienti ve hello stejné oblasti jako hello Azure Redis Cache).

>[!IMPORTANT] 
>Hello trasy definované v UDR **musí** být dost konkrétní tootake přednost přes všechny trasy inzerované konfigurace ExpressRoute hello. Hello následující příklad používá rozsah adres široký 0.0.0.0/0 hello a jako takový se dá potenciálně omylem přepsat inzerování tras pomocí podrobnější rozsahy adres.

>[!WARNING]  
>Azure Redis Cache není podporován s konfigurací ExpressRoute, **nesprávně Inzerovat trasy z hello cesty veřejného partnerského vztahu cesta toohello soukromého partnerského vztahu mezi**. Konfigurace ExpressRoute, které mají veřejné partnerské vztahy nakonfigurované, obdrží inzerování trasy od společnosti Microsoft pro velké sady rozsahů adres Microsoft Azure IP. Pokud tyto rozsahy adres nesprávně ohlášené mezi na hello cestou soukromého partnerského vztahu, hello výsledkem je, že jsou všechny odchozí síťových paketů z podsítě instanci Azure Redis Cache hello nesprávně tunelovým propojením force tooa zákazníka do místní sítě infrastruktura. Tento tok sítě dělí Azure Redis Cache. je ale problém toothis řešení Hello směrování mezi – reklamu toostop z hello cesty veřejného partnerského vztahu cesta toohello soukromého partnerského vztahu.


Základní informace o trasy definované uživatelem je k dispozici v tomto [přehled](../virtual-network/virtual-networks-udr-overview.md).

Další informace o ExpressRoute najdete v tématu [technický přehled ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Další kroky
Zjistěte, jak toouse více premium mezipaměti funkce.

* [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

