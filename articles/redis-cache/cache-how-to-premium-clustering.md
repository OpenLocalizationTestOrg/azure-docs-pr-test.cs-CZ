---
title: "aaaHow tooconfigure Redis clusteringu pro mezipaměť Azure Redis Cache Premium | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat Redis clusteringu pro vaše úroveň Premium instance služby Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: 44d520facb9d1af145b69f1b58f082aabb655d4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-redis-clustering-for-a-premium-azure-redis-cache"></a>Jak tooconfigure Redis clusteringu pro mezipaměť Azure Redis Cache Premium
Azure Redis Cache má jiný mezipaměti nabídky, které poskytují flexibilitu při výběru hello velikost mezipaměti a funkcí, včetně funkce úrovně Premium, jako je clustering, trvalosti a podpory služby virtual network. Tento článek popisuje, jak tooconfigure clusteringu prémiový Azure mezipaměti Redis instance.

Informace o dalších funkcí mezipaměti premium najdete v tématu [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Co je Redis clusteru?
Azure Redis Cache nabízí clusteru Redis jako [implementované v Redis](http://redis.io/topics/cluster-tutorial). S clusterem Redis získat hello následující výhody: 

* možnost tooautomatically Hello rozdělení datovou sadu mezi více uzly. 
* Hello možnost toocontinue operace při podmnožina uzlů hello dochází k selhání nebo jsou nelze toocommunicate se zbytkem hello hello clusteru. 
* Další propustnost: zvyšuje propustnost lineárně zvýšit hello počet horizontálních oddílů. 
* Další velikost paměti: lineárně zvyšuje zvýšit hello počet horizontálních oddílů.  

Další informace o velikosti, propustnosti a šířky pásma u prémiových mezipamětí najdete v tématu [jaké mezipaměť Redis nabídky a velikosti použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

Ve službě Azure Redis clusteru je poskytován jako primární/replika modelu, kde každý horizontálního oddílu má pár primární/replika s replikací, kde je replikace hello spravované službou Azure Redis Cache. 

## <a name="clustering"></a>Clustering
Clustering je zapnuta hello **nová mezipaměť Redis** okno při vytvoření mezipaměti. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Clustering je nakonfigurovaná na hello **Redis clusteru** okno.

![Clustering][redis-cache-clustering]

Může obsahovat maximálně too10 horizontálních oddílů v clusteru hello. Klikněte na tlačítko **povoleno** a posuňte jezdec hello nebo zadejte číslo mezi 1 a 10 pro **počet horizontálních** a klikněte na tlačítko **OK**.

Každý horizontálního oddílu je pár mezipaměti primární/replika spravuje Azure a hello celková velikost mezipaměti hello se počítá vynásobením hello počet horizontálních oddílů velikost mezipaměti hello vybraný v hello cenová úroveň. 

![Clustering][redis-cache-clustering-selected]

Po vytvoření mezipaměti hello připojení tooit a použijte ho stejně jako mezipaměť bez clusterů a Redis distribuuje hello data v mezipaměti hello horizontálních oddílů. Pokud je diagnostiky [povoleno](cache-how-to-monitor.md#enable-cache-diagnostics), metriky zaznamenání samostatně pro každou horizontálního oddílu a může být [zobrazit](cache-how-to-monitor.md) v okně hello Redis Cache. 

> [!NOTE]
> 
> Existují některé malé rozdíly při clustering je nakonfigurovaný vyžaduje v klientské aplikace. Další informace najdete v tématu [potřebuji toomake žádné změny toomy klienta aplikace toouse clustering?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Ukázkový kód pro práci s clusteringem s klienta StackExchange.Redis hello, najdete v části hello [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) část hello [Hello, World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukázka.

<a name="cluster-size"></a>

## <a name="change-hello-cluster-size-on-a-running-premium-cache"></a>Změnit velikost clusteru hello na spuštěný premium mezipaměti
velikost clusteru hello toochange na spuštěné premium mezipaměti s clustering povoleno, klikněte na tlačítko **Redis velikost clusteru** z hello **prostředků nabídky**.

> [!NOTE]
> Při hello Azure Redis Cache Premium vrstvy bylo vydání tooGeneral dostupnosti, hello Redis velikost clusteru funkce je aktuálně ve verzi preview.
> 
> 

![Velikost clusteru redis][redis-cache-redis-cluster-size]

velikost clusteru hello toochange, použijte hello posuvníku nebo zadejte číslo mezi 1 a 10 hello **počet horizontálních** textového pole a klikněte na tlačítko **OK** toosave.

> [!NOTE]
> Škálování clusteru spustí hello [MIGRACÍ](https://redis.io/commands/migrate) příkaz, který je nákladné příkaz, takže na minimální, vezměte v úvahu spuštěním této operace v době mimo špičku. Během procesu migrace hello uvidíte Špička při zatížení serveru. Škálování clusteru běží s dlouhým proces a hello množství doba závisí na hello počet klíčů a velikosti hello hodnoty související s těmito klíči.
> 
> 

## <a name="clustering-faq"></a>Clustering – nejčastější dotazy
Hello následující seznam obsahuje odpovědi toocommonly kladené dotazy týkající se clusteringu Azure Redis Cache.

* [Je nutné toomake všechny změny toomy klienta aplikace toouse clustering?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Klíče rozdělení v clusteru?](#how-are-keys-distributed-in-a-cluster)
* [Co je hello největší velikost mezipaměti, které lze vytvořit?](#what-is-the-largest-cache-size-i-can-create)
* [Všichni klienti Redis podporují clustering?](#do-all-redis-clients-support-clustering)
* [Jak se při clustering je povoleno připojovat toomy mezipaměti?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Můžete přímo připojit jednotlivých horizontálních oddílů toohello Moje mezipaměti?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Můžete nakonfigurovat clustering pro dříve vytvořenou mezipaměti?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Můžete nakonfigurovat clustering pro základní nebo standardní úroveň mezipaměti?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Můžete použít clustering zprostředkovatelům hello stavu relace ASP.NET Redis a ukládání výstupu do mezipaměti?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [Zobrazuje PŘESUNUTÍ výjimky při používání StackExchange.Redis a clusteringu, co dělat?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-toomake-any-changes-toomy-client-application-toouse-clustering"></a>Je nutné toomake všechny změny toomy klienta aplikace toouse clustering?
* Pokud je povoleno clustering, je k dispozici pouze databázi 0. Pokud vaše aplikace klienta používá více databází a se pokusí o tooread nebo zápisu databáze tooa než 0, hello následující je vyvolána výjimka. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch toodatabase: 6`
  
  Další informace najdete v tématu [specifikace Cluster Redis - implementovaná podmnožina](http://redis.io/topics/cluster-spec#implemented-subset).
* Pokud používáte [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), je nutné použít 1.0.481 nebo novější. Připojit toohello mezipaměti pomocí stejné hello [koncové body, porty a klíčů](cache-configure.md#properties) používat při připojování tooa mezipaměti, který nemá povoleným clusteringem. Hello jediným rozdílem je, že všechny čtení a zápisu musí být provedeno toodatabase 0.
  
  * Ostatní klienty může mít jiné požadavky. V tématu [všech klientů Redis podporují clustering?](#do-all-redis-clients-support-clustering)
* Pokud vaše aplikace používá více operace s klíčem zpracovat v dávce do jednoho příkazu, všechny klíče se musí nacházet v hello stejné ID horizontálního oddílu. toolocate klíče v hello stejné ID horizontálního oddílu najdete v tématu [klíče rozdělení v clusteru?](#how-are-keys-distributed-in-a-cluster)
* Pokud používáte poskytovatele stavu relace ASP.NET Redis musí používat 2.0.1 nebo vyšší. V tématu [použitím clustering zprostředkovatelům hello stavu relace ASP.NET Redis a ukládání výstupu do mezipaměti?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Klíče rozdělení v clusteru?
Za hello Redis [modelu distribučního klíče](http://redis.io/topics/cluster-spec#keys-distribution-model) dokumentace: hello klíče místo je rozděleno do 16384 sloty. Každý klíč je použita hodnota hash a přiřadit tooone tyto sloty, které jsou rozmístěny v hello uzly clusteru hello. Můžete nakonfigurovat, které součástí hello klíče je hash tooensure, které více klíčů jsou umístěny ve stejné ID horizontálního oddílu pomocí značek hash hello.

* Klíče s hash značkou - li libovolná součást hello klíč je uzavřena v `{` a `}`, rozdělí pouze část hello klíč pro účely hello určení hello slotu hash klíče. Například následující 3 klíče hello umístěna na hello stejné ID horizontálního oddílu: `{key}1`, `{key}2`, a `{key}3` od pouze hello `key` součástí názvu hello je použita hodnota hash. Úplný seznam klíčů hash značky specifikace, najdete v části [klíče hash značky](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Klíče bez značky hash - hello celý název klíče se používá k výpočtu hodnoty hash. Výsledkem je statisticky rovnoměrné rozdělení napříč horizontálních oddílů hello hello mezipaměti.

Pro nejlepší výkon a propustnost doporučujeme distribuci klíčů hello rovnoměrně. Pokud používáte s značkou hash klíče je, že rovnoměrně aplikace hello odpovědnost tooensure hello klíče.

Další informace najdete v tématu [modelu distribučního klíče](http://redis.io/topics/cluster-spec#keys-distribution-model), [horizontálního dělení dat Redis clusteru](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), a [klíče hash značky](http://redis.io/topics/cluster-spec#keys-hash-tags).

Ukázkový kód pro práci s clustering a vyhledáním klíčů v hello stejné ID horizontálního oddílu s klienta StackExchange.Redis hello, najdete v části hello [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) část hello [Hello, World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukázka.

### <a name="what-is-hello-largest-cache-size-i-can-create"></a>Co je hello největší velikost mezipaměti, které lze vytvořit?
Hello největší velikost mezipaměti premium je 53 GB. Můžete vytvořit až too10 horizontálních oddílů, která poskytuje maximální velikost 530 GB. Pokud potřebujete větší velikost můžete [požádat o další](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Další informace najdete v tématu [Azure Redis Cache ceny](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Všichni klienti Redis podporují clustering?
V hello aktuální čas podporu klientů Redis clustering. StackExchange.Redis je ten, který podporuje pro ni. Další informace o jiných klientů najdete v tématu hello [přehrávání s clusterem hello](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) části hello [kurzu cluster Redis](http://redis.io/topics/cluster-tutorial).

> [!NOTE]
> Pokud používáte StackExchange.Redis jako váš klient, zkontrolujte, že používáte nejnovější verzi hello [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 nebo novější pro clustering toowork správně. Pokud máte problémy s výjimkami přesunutí, přečtěte si téma [přesun výjimek](#move-exceptions) Další informace.
> 
> 

### <a name="how-do-i-connect-toomy-cache-when-clustering-is-enabled"></a>Jak se při clustering je povoleno připojovat toomy mezipaměti?
Tooyour mezipaměti se můžete připojit pomocí stejné hello [koncové body](cache-configure.md#properties), [porty](cache-configure.md#properties), a [klíče](cache-configure.md#access-keys) používat při připojování tooa mezipaměti, který nemá povoleným clusteringem. Redis spravuje hello clustering pro back-end hello, takže není nutné toomanage z vašeho klienta.

### <a name="can-i-directly-connect-toohello-individual-shards-of-my-cache"></a>Můžete přímo připojit jednotlivých horizontálních oddílů toohello Moje mezipaměti?
Toto není oficiálně podporován. S třídou uvedená každý horizontálního oddílu se skládá z mezipaměti pár primární/replika souhrnně označované jako instance mezipaměti. Můžete připojit pomocí rozhraní příkazového řádku redis nástroje hello systému hello instance mezipaměti toothese [nestabilním](http://redis.io/download) větve hello Redis úložiště v Githubu. Tato verze implementuje základní podpora při spuštění s hello `-c` přepínače. Další informace najdete v části [přehrávání s clusterem hello](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) na [http://redis.io](http://redis.io) v hello [kurzu cluster Redis](http://redis.io/topics/cluster-tutorial).

Bez ssl použijte následující příkazy hello.

    Redis-cli.exe –h <<cachename>> -p 13000 (tooconnect tooinstance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (tooconnect tooinstance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (tooconnect tooinstance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (tooconnect tooinstance N)

Pro protokol ssl, nahraďte `1300N` s `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Můžete nakonfigurovat clustering pro dříve vytvořenou mezipaměti?
Aktuálně lze povolit pouze clustering při vytvoření mezipaměti. Velikost clusteru hello můžete změnit po hello mezipaměti je vytvořen, avšak nelze přidat clustering tooa premium mezipaměti nebo odebrání clustering z mezipaměti premium po vytvoření mezipaměti hello. Cache ve verzi premium s povoleným clusteringem a jedinou horizontálních se liší od premium mezipaměť hello stejná velikost s bez clusteringu.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Můžete nakonfigurovat clustering pro základní nebo standardní úroveň mezipaměti?
Clustering je dostupná jenom pro prémiových mezipamětí.

### <a name="can-i-use-clustering-with-hello-redis-aspnet-session-state-and-output-caching-providers"></a>Můžete použít clustering zprostředkovatelům hello stavu relace ASP.NET Redis a ukládání výstupu do mezipaměti?
* **Poskytovatel výstupní mezipaměti redis** – bez nutnosti změn.
* **Zprostředkovatel stavu relace redis** -toouse clustering, je nutné použít [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 nebo vyšší nebo výjimku, je vyvolána. Toto je narušující změně; Další informace najdete v části [v2.0.0 nejnovější podrobností o změně](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Zobrazuje PŘESUNUTÍ výjimky při používání StackExchange.Redis a clusteringu, co dělat?
Pokud používáte StackExchange.Redis a přijímat `MOVE` výjimky při použití clusteringu, ujistěte se, že používáte [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) nebo novější. Pokyny ke konfiguraci vaší toouse aplikace .NET StackExchange.Redis najdete v tématu [konfigurace klientů mezipaměti hello](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Další kroky
Zjistěte, jak toouse více premium mezipaměti funkce.

* [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







