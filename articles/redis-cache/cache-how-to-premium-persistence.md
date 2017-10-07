---
title: "aaaHow tooconfigure data trvalosti pro mezipaměť Azure Redis Cache Premium"
description: "Zjistěte, jak tooconfigure a spravovat vaše instance Azure Redis Cache Premium vrstvu trvalosti dat"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: sdanie
ms.openlocfilehash: 62feb6f5522e0270487f045eb303bf852434143d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-persistence-for-a-premium-azure-redis-cache"></a>Jak tooconfigure trvalosti dat pro mezipaměť Azure Redis Cache Premium
Azure Redis Cache má jiný mezipaměti nabídky, které poskytují flexibilitu při výběru hello velikost mezipaměti a funkcí, včetně funkce úrovně Premium, jako je clustering, trvalosti a podpory služby virtual network. Tento článek popisuje, jak tooconfigure trvalost v prémiový Azure mezipaměti Redis instance.

Informace o dalších funkcí mezipaměti premium najdete v tématu [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Co je trvalosti dat?
[Trvalost redis](https://redis.io/topics/persistence) vám umožní toopersist data uložená v Redis. Můžete také pořizovat snímky a zálohovat hello data, která budete moct načíst v případě selhání hardwaru. Toto je obrovskou výhodu přes Basic nebo standardní úroveň, kde všechny hello data jsou uloženy v paměti a může být potenciální ztrátě dat v případě selhání, kde jsou uzly mezipaměti dolů. 

Azure Redis Cache nabízí trvalosti Redis pomocí hello následující modely:

* **Trvalost RDB** -trvalost při RDB (databáze Redis) je nakonfigurován, Azure Redis Cache potrvají snímek hello mezipaměti Redis v Redis binární formát toodisk podle konfigurovat četnost zálohování. Pokud dojde k závažné události, zakazující hello primární a mezipaměti repliky, hello mezipaměti je znovu vytvořena, pomocí hello poslední snímek. Další informace o hello [výhody](https://redis.io/topics/persistence#rdb-advantages) a [nevýhody](https://redis.io/topics/persistence#rdb-disadvantages) z trvalost RDB.
* **Trvalost AOF** -trvalost při AOF (pouze připojit soubor) je nakonfigurován, Azure Redis Cache uloží každých protokolu tooa operace zápisu, který je uložený aspoň jednou za sekundu do účtu Azure Storage. Pokud dojde k závažné události, zakazující hello primární a mezipaměti repliky, hello mezipaměti je znovu vytvořena, pomocí operace zápisu hello uložené. Další informace o hello [výhody](https://redis.io/topics/persistence#aof-advantages) a [nevýhody](https://redis.io/topics/persistence#aof-disadvantages) z AOF trvalost.

Trvalost je nakonfigurovaný z hello **nová mezipaměť Redis** okno při vytvoření mezipaměti a na hello **prostředků nabídky** existující premium ukládá do mezipaměti.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Jakmile je vybraná cenová úroveň premium, klikněte na možnost **trvalosti Redis**.

![Trvalost redis][redis-cache-persistence]

Hello kroky v hello další části popisují, jak tooconfigure trvalosti Redis na nové mezipaměti premium. Po nakonfigurování trvalosti Redis klikněte na tlačítko **vytvořit** toocreate vaší nové premium mezipaměti s trvalosti Redis.

## <a name="enable-redis-persistence"></a>Povolit trvalost Redis

Redis na hello je povolena trvalost **trvalosti dat Redis** okna tak, že buď zvolíte **RDB** nebo **AOF** trvalost. U nových mezipamětí je přístup toto okno při procesu vytváření mezipaměti hello, jak je popsáno v předchozí části hello. Pro existující mezipamětí hello **trvalosti dat Redis** okno je k němu přistupovat z hello **prostředků nabídky** ke svojí mezipaměti.

![Nastavení redis][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>Konfigurace trvalosti RDB

trvalost RDB tooenable, klikněte na tlačítko **RDB**. trvalost RDB toodisable na dříve povoleno premium mezipaměti, klikněte na tlačítko **zakázané**.

![RDB trvalosti redis][redis-cache-rdb-persistence]

tooconfigure hello interval zálohování, vyberte **četnost zálohování** z rozevíracího seznamu hello. Vybrat možnost **15 minut**, **30 minut**, **60 minut**, **6 hodin**, **12 hodin**a **24 hodin**. Tento interval spustí počítání po úspěšném dokončení předchozí operace zálohování hello a po jeho uplynutí se zahájí novou zálohu.

Klikněte na tlačítko **účet úložiště** tooselect hello toouse účet úložiště a vyberte buď hello **primární klíč** nebo **sekundární klíč** toouse z hello **úložiště Klíč** rozevíracího seznamu. Musíte zvolit účet úložiště v hello stejné oblasti jako hello mezipaměti a **Storage úrovně Premium** účet je doporučená, protože úložiště premium je vyšší propustnost. 

> [!IMPORTANT]
> Pokud se znovu vygeneruje klíč hello úložiště pro váš účet trvalost, je nutné překonfigurovat hello požadovaný klíč hello **klíč úložiště** rozevíracího seznamu.
> 
> 

Klikněte na tlačítko **OK** toosave hello trvalost konfigurace.

Další zálohování Hello (nebo první zálohy u nových mezipamětí) je zahájena po uplynutí intervalu hello četnost zálohování.

## <a name="configure-aof-persistence"></a>Konfigurace trvalosti AOF

trvalost AOF tooenable, klikněte na tlačítko **AOF**. trvalost AOF toodisable na dříve povoleno premium mezipaměti, klikněte na tlačítko **zakázané**.

![AOF trvalosti redis][redis-cache-aof-persistence]

trvalost AOF tooconfigure, zadejte **první účet úložiště**. Tento účet úložiště musí být v hello stejné oblasti jako hello mezipaměti a **Storage úrovně Premium** účet je doporučená, protože úložiště premium je vyšší propustnost. Volitelně můžete nakonfigurovat účet úložiště s názvem **druhého účtu úložiště**. Pokud je nakonfigurovaný druhý účet úložiště, hello zápisy toohello repliky mezipaměti, zapíšou se toothis druhého účtu úložiště. U každého účtu úložiště, vyberte buď hello **primární klíč** nebo **sekundární klíč** toouse z hello **klíč úložiště** rozevíracího seznamu. 

> [!IMPORTANT]
> Pokud se znovu vygeneruje klíč hello úložiště pro váš účet trvalost, je nutné překonfigurovat hello požadovaný klíč hello **klíč úložiště** rozevíracího seznamu.
> 
> 

Když je povolena trvalost AOF, zápisu operace toohello mezipaměti se ukládají toohello určený účet úložiště (nebo účty, pokud jste nakonfigurovali druhého účtu úložiště). V případě hello závažné selhání, trvá dolů obě hello primární a repliky mezipaměti hello uložené AOF protokol je použít toorebuild hello mezipaměti.

## <a name="persistence-faq"></a>Trvalost – nejčastější dotazy
Hello následující seznam obsahuje odpovědi toocommonly kladené dotazy týkající se Azure Redis Cache trvalost.

* [Můžete povolit trvalost na dříve vytvořenou mezipaměti?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Můžete povolit trvalost AOF a RDB v hello současně?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Zvolte trvalost modelu](#which-persistence-model-should-i-choose)
* [Co se stane, pokud mají I škálovat tooa jinou velikost a která byla vytvořená před hello škálování operaci obnovení zálohy?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>Trvalost RDB
* [Můžete změnit po vytvoření mezipaměti hello četnost zálohování RDB hello?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Proč Pokud četnost zálohování v RDB 60 minut je víc než 60 minut mezi zálohování?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Co se stane starší zálohy RDB toohello při novou zálohu?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>Trvalost AOF
* [Kdy je vhodné použít druhého účtu úložiště?](#when-should-i-use-a-second-storage-account)
* [Nemá vliv trvalost AOF v celé, latenci nebo výkonu Moje mezipaměti?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [Jak můžete odebrat hello druhého účtu úložiště?](#how-can-i-remove-the-second-storage-account)
* [Co je přepisování a jak se týká Moje mezipaměti?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Co lze očekávat při škálování mezipaměť s AOF povolené?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [Uspořádání AOF data v úložišti](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Můžete povolit trvalost na dříve vytvořenou mezipaměti?
Ano, trvalosti Redis lze nakonfigurovat při vytváření mezipaměti i na existující prémiových mezipamětí.

### <a name="can-i-enable-aof-and-rdb-persistence-at-hello-same-time"></a>Můžete povolit trvalost AOF a RDB v hello současně?

Ne, můžete povolit pouze RDB nebo AOF, ale ne obojí v hello stejnou dobu.

### <a name="which-persistence-model-should-i-choose"></a>Zvolte trvalost modelu

Trvalost AOF uloží každých zápisu tooa protokol, který má určitý dopad na propustnost, porovnat s RDB trvalost, které ukládá zálohování podle hello nakonfigurované interval zálohování s minimálním dopadem na výkon. Trvalost AOF zvolte, pokud primární cílem je toominimize ztrátě dat a může zpracovat pokles v propustnost pro mezipaměť. Pokud chcete toomaintain optimální propustnosti v mezipaměti, ale stále chcete mechanismus pro obnovení dat, vyberte trvalost RDB.

* Další informace o hello [výhody](https://redis.io/topics/persistence#rdb-advantages) a [nevýhody](https://redis.io/topics/persistence#rdb-disadvantages) z trvalost RDB.
* Další informace o hello [výhody](https://redis.io/topics/persistence#aof-advantages) a [nevýhody](https://redis.io/topics/persistence#aof-disadvantages) z AOF trvalost.

Další informace o výkonu při použití AOF trvalosti najdete v tématu [AOF nemá vliv trvalost v celé, latenci nebo výkonu Moje mezipaměti?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-tooa-different-size-and-a-backup-is-restored-that-was-made-before-hello-scaling-operation"></a>Co se stane, pokud mají I škálovat tooa jinou velikost a která byla vytvořená před hello škálování operaci obnovení zálohy?

RDB a AOF trvalosti:

* Při změně měřítka větší velikost tooa neexistuje žádný vliv.
* Pokud máte škálovat tooa menší velikost, a máte vlastní [databáze](cache-configure.md#databases) nastavení, která je větší než hello [databáze limit](cache-configure.md#databases) pro novou velikost, není obnovit data v těchto databází. Další informace najdete v tématu [je vlastní databází. nastavení ovlivněných během změny měřítka?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Pokud máte škálovat tooa menší velikost, a v hello menší velikost toohold všechna data hello z poslední zálohy klíčů hello bude vyloučena během procesu obnovení hello není dostatek místa, obvykle pomocí hello [allkeys lru](http://redis.io/topics/lru-cache) vyřazení zásady.

### <a name="can-i-change-hello-rdb-backup-frequency-after-i-create-hello-cache"></a>Můžete změnit po vytvoření mezipaměti hello četnost zálohování RDB hello?
Ano, můžete změnit četnost zálohování hello RDB trvalosti na hello **trvalosti dat Redis** okno. Pokyny najdete v tématu [konfigurace Redis trvalost](#configure-redis-persistence).

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Proč Pokud četnost zálohování v RDB 60 minut je víc než 60 minut mezi zálohování?
Hello RDB trvalost četnost zálohování interval se nespustí, dokud proces zálohování předchozí hello byla úspěšně dokončena. Pokud četnost zálohování hello je 60 minut a jak dlouho trvá toosuccessfully proces zálohování 15 minut, dokončení, hello další zálohování se nespustí, dokud 75 minut po hello počáteční čas hello předchozí zálohy.

### <a name="what-happens-toohello-old-rdb-backups-when-a-new-backup-is-made"></a>Co se stane starší zálohy RDB toohello při novou zálohu?
Všechny zálohy trvalost RDB s výjimkou hello nejnovějšího se automaticky odstraní. Odstranění nemusí dojít okamžitě, ale nejsou starší zálohy trvalé bez omezení.


### <a name="when-should-i-use-a-second-storage-account"></a>Kdy je vhodné použít druhého účtu úložiště?

Pokud se domníváte, že máte vyšší než očekávaný množinové operace u hello mezipaměti, používejte druhého účtu úložiště AOF trvalosti.  Nastavení účtu sekundární úložiště hello pomáhá zajistit, že mezipaměť nebude dosáhnout omezení šířky pásma úložiště.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>Nemá vliv trvalost AOF v celé, latenci nebo výkonu Moje mezipaměti?

Trvalost AOF ovlivní propustnost podle přibližně 15 % – 20 % při hello mezipaměti je menší než maximální zatížení (procesoru a Server načíst i v části 90 %). By neměl být problémy s latencí při hello mezipaměť je v rámci těchto mezních hodnot. Hello mezipaměti bude tyto limity však dříve dosáhnout s AOF povolena.

### <a name="how-can-i-remove-hello-second-storage-account"></a>Jak můžete odebrat hello druhého účtu úložiště?

Můžete odebrat účet sekundární úložiště trvalosti AOF hello nastavením hello druhého toobe hello stejné jako první účet úložiště hello účtu úložiště. Pokyny najdete v tématu [AOF konfigurace trvalosti](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Co je přepisování a jak se týká Moje mezipaměti?

Když se soubor AOF hello stane dostatečně velké na to, přepisování je automaticky ve frontě na hello mezipaměti. soubor AOF Hello přepisování změní velikost hello hello minimální sadu potřebných operací toocreate hello aktuální datové sady. Během přepisů očekávat dříve tooreach omezení výkonu, zejména při plánování práce s velkými datovými sadami. Přepisů docházet k méně často hello AOF souboru se změní na větší, ale bude trvat poměrně dlouhou dobu, kdy se stane.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Co lze očekávat při škálování mezipaměť s AOF povolené?

Pokud je soubor AOF hello v době hello škálování výrazně velký, pak očekávejte tootake operace škálování hello déle, než se očekávalo vzhledem k tomu, že se bude znovu načíst soubor hello po dokončení škálování.

Další informace o škálování najdete v tématu [co se stane, když I mít škálovat tooa jinou velikost a která byla vytvořená před hello škálování operaci obnovení zálohy?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>Uspořádání AOF data v úložišti

Data uložená v souborech AOF je rozdělen do více objekty BLOB stránky na uzlu tooincrease výkon ukládání dat toostorage hello. Hello následující tabulka zobrazuje kolik objekty BLOB stránky se používají u jednotlivých cenových úrovní:

| Úroveň Premium | Objekty blob |
|--------------|-------|
| P1           | 4 na horizontální oddíl    |
| P2           | 8 na horizontální oddíl    |
| P3           | 16 na horizontální oddíl   |
| P4           | 20 na horizontální oddíl   |

Pokud je povoleno clustering, každý horizontálního oddílu v mezipaměti hello má vlastní sadu objektů BLOB stránky, jak je uvedeno v předchozí tabulce hello. Například P2 mezipaměť tři horizontálních oddílů rozděluje jeho AOF souboru na 24 objekty BLOB stránky (8 objekty BLOB na horizontální oddíl s 3 horizontálních oddílů).

Po přepsání dvě sady AOF soubory existují v úložišti. Přepisů dojít hello pozadí a připojte toohello první sadu souborů, zatímco množinové operace, které se odesílají toohello mezipaměti při přepisování hello připojit toohello druhé sadě. Zálohy se dočasně ukládají během přepisů v případě selhání, ale bude po dokončení přepište okamžitě odstraněna.


## <a name="next-steps"></a>Další kroky
Zjistěte, jak toouse více premium mezipaměti funkce.

* [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
