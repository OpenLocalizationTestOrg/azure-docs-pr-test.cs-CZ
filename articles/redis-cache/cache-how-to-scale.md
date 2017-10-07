---
title: aaaHow tooScale Azure Redis Cache | Microsoft Docs
description: "Zjistěte, jak tooscale vaší služby Azure Redis instancích mezipaměti"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
ms.openlocfilehash: 8d7c015a539f872913056392aa080bf3f445bd03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-azure-redis-cache"></a>Jak tooScale Azure mezipaměti Redis
Azure Redis Cache má jiný mezipaměti nabídky, které poskytují flexibilitu při výběru hello velikost mezipaměti a funkcí. Po vytvoření mezipaměti můžete škálovat hello velikost a hello cenová úroveň mezipaměti hello, pokud se změní hello požadavky vaší aplikace. Tento článek ukazuje, jak tooscale svoji mezipaměť hello portál Azure a pomocí nástroje například Azure PowerShell a rozhraní příkazového řádku Azure.

## <a name="when-tooscale"></a>Když tooscale
Můžete použít hello [monitorování](cache-how-to-monitor.md) funkce Azure Redis Cache toomonitor hello stav a výkon vaší mezipaměti a pomůže vám určit, kdy tooscale hello mezipaměti. 

Můžete monitorovat následující hello metriky toohelp určete, zda je nutné tooscale.

* Zatížení serveru redis
* Využití paměti
* Šířka pásma sítě
* Využití procesoru

Pokud zjistíte, že mezipaměť již splňuje požadavky vaší aplikace, je možné škálovat tooa větší nebo menší mezipaměti cenovou úroveň, která je pro vaši aplikaci nejvhodnější. Další informace o určení, které mezipaměti cenová úroveň toouse najdete v tématu [jaké mezipaměť Redis nabídky a velikosti použít](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Změnit velikost mezipaměti
tooscale vaší mezipaměti [procházet mezipaměti toohello](cache-configure.md#configure-redis-cache-settings) v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **škálování** z hello **prostředků nabídky**.

![Měřítko](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Vyberte hello potřeby cenová úroveň z hello **vyberte cenová úroveň** a klikněte na **vyberte**.

![Cenová úroveň][redis-cache-pricing-tier-blade]


Je možné škálovat tooa jinou cenovou úroveň s hello následující omezení:

* Nelze škálovat z vyšší cenová úroveň tooa nižší cenová úroveň.
  * Nelze škálovat od **Premium** mezipaměti dolů tooa **standardní** nebo **základní** mezipaměti.
  * Nelze škálovat od **standardní** mezipaměti dolů tooa **základní** mezipaměti.
* Je možné škálovat od **základní** mezipaměti tooa **standardní** mezipaměti, ale nemůže změnit velikost hello v hello stejnou dobu. Pokud potřebujete jinou velikost, můžete provést následné velikost toohello potřeby operace škálování.
* Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti. Musí škálování z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následných škálování operace.
* Nelze škálovat z větší velikost dolů toohello **C0 (250 MB)** velikost.
 
Při hello mezipaměti se škáluje toohello nové cenovou úroveň, **škálování** stav se zobrazí v hello **Redis Cache** okno.

![Škálování][redis-cache-scaling]

Po dokončení škálování hello stav se změní z **škálování** příliš**systémem**.

## <a name="how-tooautomate-a-scaling-operation"></a>Jak tooautomate škálování operace
V přidání tooscaling hello vaše instance mezipaměti v portálu Azure, můžete škálovat pomocí rutin prostředí PowerShell, rozhraní příkazového řádku Azure a pomocí hello Microsoft Azure Management knihovny (MAML). 

* [Škálování pomocí prostředí PowerShell](#scale-using-powershell)
* [Škálování pomocí rozhraní příkazového řádku Azure](#scale-using-azure-cli)
* [Škálování pomocí MAML](#scale-using-maml)

### <a name="scale-using-powershell"></a>Škálování pomocí prostředí PowerShell
Je možné škálovat vaše instance služby Azure Redis Cache pomocí prostředí PowerShell pomocí hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) rutiny při hello `Size`, `Sku`, nebo `ShardCount` jsou upraveny vlastnosti. Hello následující příklad ukazuje, jak tooscale mezipaměti s názvem `myCache` tooa 2,5 GB mezipaměti. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Další informace o škálování pomocí prostředí PowerShell najdete v tématu [tooscale Redis mezipaměti pomocí prostředí Powershell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Škálování pomocí rozhraní příkazového řádku Azure
volání vaší instancí Azure Redis Cache pomocí rozhraní příkazového řádku Azure, tooscale hello `azure rediscache set` příkazu a předejte jí hello potřeby změny konfigurace, které zahrnují nové velikosti, sku nebo velikost clusteru, v závislosti na hello potřeby operace škálování.

Další informace o škálování s Azure CLI, najdete v části [změnit nastavení existujícího Redis Cache](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Škálování pomocí MAML
tooscale vaší instancí Azure Redis Cache pomocí hello [Microsoft Azure Management knihovny (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), volání hello `IRedisOperations.CreateOrUpdate` metoda a předejte jí hello novou velikost hello `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting hello access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // tooscale, set a new size for hello redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Další informace najdete v tématu hello [spravovat Redis Cache pomocí MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) ukázka.

## <a name="scaling-faq"></a>Škálování – nejčastější dotazy
Hello následující seznam obsahuje odpovědi toocommonly kladené dotazy týkající se Azure Redis Cache škálování.

* [Možné škálovat na, z a do mezipaměti Premium?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Po škálování, je nutné provést toochange mé klíče název nebo přístupu k mezipaměti?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Jak funguje škálování](#how-does-scaling-work)
* [Bude možné ztrátě dat z mé mezipaměti při škálování?](#will-i-lose-data-from-my-cache-during-scaling)
* [Je databází. vlastní nastavení ovlivněných během změny měřítka?](#is-my-custom-databases-setting-affected-during-scaling)
* [Moje mezipaměti bude k dispozici během změny měřítka?](#will-my-cache-be-available-during-scaling)
* [Operace, které nejsou podporovány](#operations-that-are-not-supported)
* [Jak dlouho škálování trvá?](#how-long-does-scaling-take)
* [Jak poznám, škálování po dokončení?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Možné škálovat na, z a do mezipaměti Premium?
* Nelze škálovat od **Premium** mezipaměti dolů tooa **základní** nebo **standardní** cenová úroveň.
* Je možné škálovat od jednoho **Premium** cenová úroveň tooanother mezipaměti.
* Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti. Je nutné nejprve škálovat z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následné operace škálování.
* Pokud jste povolili clustering při vytváření vašeho **Premium** mezipaměti, můžete [změnit velikost clusteru hello](cache-how-to-premium-clustering.md#cluster-size). Pokud vaše mezipaměť byl vytvořen bez clusteringu povoleno, nelze nakonfigurovat clustering později.
  
  Další informace najdete v tématu [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-toochange-my-cache-name-or-access-keys"></a>Po škálování, je nutné provést toochange mé klíče název nebo přístupu k mezipaměti?
Ne, název mezipaměti a klíče jsou beze změny během operace škálování.

### <a name="how-does-scaling-work"></a>Jak funguje škálování
* Když **základní** mezipaměti bude změněno měřítko tooa různou velikost je vypnutý a zřizovat nové mezipaměti pomocí hello novou velikost. Během této doby hello mezipaměti není k dispozici a dojde ke ztrátě všech dat v mezipaměti hello.
* Když **základní** mezipaměť je škálovat tooa **standardní** mezipaměti repliky mezipaměti je zřízený a hello data budou zkopírována z hello primární mezipaměti toohello repliky mezipaměť. Hello mezipaměti zůstává k dispozici během hello škálování procesu.
* Když **standardní** mezipaměť je škálovat tooa jinou velikost nebo tooa **Premium** mezipaměti, jednu z replik hello se vypne a znovu zřídit toohello nová velikost a hello data přenosu a hello jiných repliky provede převzetí služeb při selhání, než je znovu zřízený, podobný proces toohello, ke kterému dochází při selhání jednoho z uzlů mezipaměti hello.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Bude možné ztrátě dat z mé mezipaměti při škálování?
* Když **základní** mezipaměti je škálovat tooa novou velikost, všechna data se ztratí a hello mezipaměti není k dispozici během hello operace škálování.
* Když **základní** mezipaměť je škálovat tooa **standardní** mezipaměti, hello data v mezipaměti hello je obvykle zachovat.
* Když **standardní** mezipaměť je škálovat tooa větší velikost nebo vrstvy, nebo **Premium** mezipaměti bude změněno měřítko tooa větší velikost, všechna data je obvykle zachovaná. Při zvětšení velikosti **standardní** nebo **Premium** mezipaměti dolů tooa menší velikost, data mohou být ztraceny v závislosti na tom, kolik dat je v mezipaměti hello související s novou velikost toohello, když bude změněno měřítko. Pokud dojde ke ztrátě dat při škálování směrem dolů, klíče jsou vyřazování pomocí hello [allkeys lru](http://redis.io/topics/lru-cache) zásady vyřazení. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Je databází. vlastní nastavení ovlivněných během změny měřítka?
Některé cenové úrovně mají různé [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při škálování směrem dolů v případě nakonfigurovat vlastní hodnotu hello `databases` nastavení během vytváření mezipaměti.

* Při příjmu tooa cenová úroveň s nižší `databases` limit než současná vrstva hello:
  * Pokud používáte výchozí číslo hello `databases` tedy 16 pro všechny cenové úrovně, všechna data budou zachována.
  * Pokud používáte vlastní počet `databases` který spadá do hello limity pro toowhich vrstvy hello jsou změny velikosti, to `databases` se zachovává nastavení a dojde ke ztrátě žádná data.
  * Pokud používáte vlastní počet `databases` který překračuje omezení hello hello novou vrstvu, hello `databases` nastavení je snížený toohello omezení hello novou vrstvu a dojde ke ztrátě všech dat v databázích hello odebrat.
* Při příjmu tooa cenová úroveň s hello stejná nebo vyšší `databases` limit než současná vrstva hello vaší `databases` se zachovává nastavení a dojde ke ztrátě žádná data.

Všimněte si, že když Standard a Premium mezipamětí má SLA 99,9 % dostupnosti, bez ztráty dat smlouvy SLA.

### <a name="will-my-cache-be-available-during-scaling"></a>Moje mezipaměti bude k dispozici během změny měřítka?
* **Standardní** a **Premium** mezipamětí zůstanou k dispozici během hello operace škálování.
* **Základní** jsou offline během změny měřítka operations tooa jinou velikost mezipaměti, ale zůstanou dostupné při škálování z **základní** příliš**standardní**.

### <a name="operations-that-are-not-supported"></a>Operace, které nejsou podporovány
* Nelze škálovat z vyšší cenová úroveň tooa nižší cenová úroveň.
  * Nelze škálovat od **Premium** mezipaměti dolů tooa **standardní** nebo **základní** mezipaměti.
  * Nelze škálovat od **standardní** mezipaměti dolů tooa **základní** mezipaměti.
* Je možné škálovat od **základní** mezipaměti tooa **standardní** mezipaměti, ale nemůže změnit velikost hello v hello stejnou dobu. Pokud potřebujete jinou velikost, můžete provést následné velikost toohello potřeby operace škálování.
* Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti. Je nutné nejprve škálovat z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následné operace škálování.
* Nelze škálovat z větší velikost dolů toohello **C0 (250 MB)** velikost.

Pokud škálování operace selže, hello služba se pokusí toorevert hello operace a hello mezipaměti bude používat původní velikost toohello.

### <a name="how-long-does-scaling-take"></a>Jak dlouho škálování trvá?
Škálování trvá přibližně 20 minut v závislosti na tom, kolik dat je v mezipaměti hello.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Jak poznám, škálování po dokončení?
V portálu Azure hello se zobrazí hello škálování operace probíhá. Po dokončení škálování hello hello mezipaměti změně stavu příliš**systémem**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



