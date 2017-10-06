---
title: "aaaIntroduction toohello Azure Redis Cache na úrovni Premium | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat trvalost Redis, Redis clustering a podpora virtuální sítě pro vaše instance služby Azure Redis Cache úrovně Premium"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: 5b58a03647fbf1198509ac6f1acd04f1b682ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-redis-cache-premium-tier"></a>Úvod toohello Azure Redis Cache na úrovni Premium
Azure Redis Cache je mezipaměť distribuovaná, kterou spravuje vám usnadní vytváření vysoce škálovatelné a dobře reagovaly aplikací tím, že poskytuje extrémně rychlého přístup k datům tooyour. 

Hello novou úroveň Premium je připraven vrstvou Enterprise, který zahrnuje všechny funkce úrovně Standard hello a informace, jako je lepší výkon, větší zatížení, zotavení po havárii, importu a exportu a rozšířené zabezpečení. Další informace o dalších funkcí hello hello Premium mezipaměti vrstvy čtení toolearn pokračujte.

## <a name="better-performance-compared-toostandard-or-basic-tier"></a>Lepší výkon ve srovnání tooStandard nebo úroveň Basic
**Vyšší výkon přes standardní nebo základní vrstvě.** Mezipaměti v úrovni Premium hello jsou nasazeny na hardware, který má rychlejších procesorů a nabízí lepší výkon ve srovnání toohello základní nebo standardní úroveň. Premium úroveň mezipaměti mají vyšší propustnost a nižší latenci. 

**Propustnost pro hello je stejné velikostí mezipaměti jako porovnání tooStandard vrstvy vyšší na úrovni Premium.** Například hello propustnost 53 GB P4 (Premium) mezipaměť je 250 TIS požadavků za sekundu jako porovnání too150K pro C6 (Standard).

Další informace o velikosti, propustnosti a šířky pásma u prémiových mezipamětí najdete v tématu [Azure Redis Cache – nejčastější dotazy](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Trvalosti dat redis
úroveň Premium Hello umožňuje toopersist hello ukládat data v účtu Azure Storage. V mezipaměti Basic nebo Standard všechna data hello uloženo pouze v paměti. V případě základní infrastruktura může být problémy existuje potenciální ztrátě dat. Doporučujeme používat funkce trvalosti dat Redis hello v hello Premium vrstvy tooincrease odolnost proti ztrátě dat. Azure Redis Cache nabízí RDB a AOF (už brzy) možnosti v [trvalosti Redis](http://redis.io/topics/persistence). 

Pokyny týkající se konfigurace trvalosti najdete v tématu [jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Cluster redis
Chcete-li toocreate mezipamětí větší než 53 GB nebo mají tooshard data mezi různými uzly Redis, můžete použít clustering, která je dostupná v úrovni Premium hello Redis. Každý uzel se skládá z dvojice primární/replika mezipaměti spravuje Azure pro vysokou dostupnost. 

**Redis clustering poskytuje maximální měřítko a propustnosti.** Propustnost zvyšuje lineárně zvýšit hello počet horizontálních oddílů (uzlů) v clusteru hello. Např. Pokud vytvoříte cluster P4 10 horizontálními oddíly, pak hello k dispozici propustnost je 250 TIS * 10 = 2,5 milionu požadavků za sekundu. Najdete v tématu hello [Azure Redis Cache – nejčastější dotazy](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) další podrobnosti o velikosti, propustnosti a šířky pásma u prémiových mezipamětí.

tooget začít s clustering, najdete v části [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Vyšší míra zabezpečení a izolace
Jsou přístupné na mezipamětí vytvořené v úroveň Basic nebo Standard hello hello veřejného Internetu. Přístup k toohello mezipaměti je omezená na základě hello přístupový klíč. S hello úroveň Premium můžete další ujistěte, že jenom pro klienty v zadané síti přístup hello mezipaměti. Můžete nasadit Redis Cache v [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). Všechny funkce hello virtuální sítě můžete použít, například podsítě, zásady řízení přístupu a další funkce toofurther omezit přístup tooRedis.

Další informace najdete v tématu [jak podporují tooconfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/export
Import a Export je Azure Redis Cache data management operace, která vám umožní tooimport data do Azure Redis Cache nebo exportu dat z Azure Redis Cache pomocí import a Export snímku databáze Redis Cache (RDB) ze stránky premium mezipaměti tooa blob v Azure Účet úložiště. To vám umožní toomigrate mezi různými instancemi Azure Redis Cache nebo naplňte hello mezipaměť s daty před použitím.

Import se dá použít toobring Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších. Import dat se snadný způsob toocreate mezipaměti s předem vyplněná daty. Během procesu importu hello Azure Redis Cache načte hello RDB soubory z úložiště Azure do paměti a poté vloží hello klíčů do mezipaměti hello.

Export umožňuje tooexport hello data uložená v Azure Redis Cache tooRedis kompatibilní RDB soubory. Můžete použít tuto funkci toomove data tooanother instanci Azure Redis Cache či tooanother serveru Redis. Během procesu exportu hello dočasný soubor vytvořen na hello virtuálních počítačů, hostitelů hello instanci serveru Azure Redis Cache a soubor hello je nahrané toohello určený účet úložiště. Po dokončení operace exportu hello se stavem úspěch nebo selhání hello dočasný soubor bude odstraněn.

Další informace najdete v tématu [jak tooimport dat do a exportu dat z Azure Redis Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Restartování
úroveň premium Hello vám umožní tooreboot jeden nebo více uzlů vaší mezipaměti na vyžádání. To vám umožní tootest aplikace pro odolnost v případě hello selhání. Můžete restartovat hello následující uzly.

* Hlavní uzel vaší mezipaměti
* Podřízený uzel vaší mezipaměti
* Hlavní a podřízené uzly mezipaměti
* Při použití s clusteringem cache ve verzi premium, můžete restartovat hello master, podřízený nebo oba uzly pro jednotlivé horizontálních oddílů v mezipaměti hello

Další informace najdete v tématu [restartovat](cache-administration.md#reboot) a [nejčastější dotazy týkající se restartovat](cache-administration.md#reboot-faq).

>[!NOTE]
>Restartovat funkce je teď zapnutá pro všechny úrovně Azure Redis Cache.
>
>

## <a name="schedule-updates"></a>Aktualizace plánu
Hello plánovaných aktualizací funkce vám umožní toodesignate údržby pro mezipaměť. Pokud je zadána hello údržby, všechny aktualizace serveru Redis probíhají během této doby. toodesignate údržby, vyberte požadovaného hello dnů a určete hodina spouštění údržby okno hello za každý den. Všimněte si, že hello časového období údržby se ve standardu UTC. 

Další informace najdete v tématu [naplánovat aktualizace](cache-administration.md#schedule-updates) a [naplánovat aktualizace – nejčastější dotazy](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Pouze Redis serveru, které jsou provedeny aktualizace během plánované údržby hello. Hello údržby doporučení se netýká tooAzure aktualizací nebo aktualizací toohello virtuálních počítačů operačního systému.
> 
> 

## <a name="geo-replication"></a>Geografická replikace

**Geografická replikace** poskytuje mechanismus pro propojení dvě instance vrstvy Azure Redis Cache Premium. Jeden mezipaměti je určený jako primární propojené mezipaměti hello a hello jiných jako sekundární propojené mezipaměti hello. Hello sekundární propojené mezipaměti se stane, jen pro čtení a data napsané toohello primární mezipaměť je replikují toohello sekundární propojené mezipaměti. Tuto funkci lze použít tooreplicate mezipaměti nad oblastmi Azure.

Další informace najdete v tématu [jak tooconfigure geografická replikace pro Azure Redis Cache](cache-how-to-geo-replication.md).


## <a name="tooscale-toohello-premium-tier"></a>úroveň premium toohello tooscale
úroveň premium toohello tooscale, jednoduše vyberte jednu z úrovní premium hello v hello **změna cenové úrovně** okno. Také je možné škálovat úrovně premium toohello vaší mezipaměti pomocí prostředí PowerShell a rozhraní příkazového řádku. Podrobné pokyny najdete v tématu [jak tooScale Azure mezipaměti Redis](cache-how-to-scale.md) a [jak tooautomate škálování operaci](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Další kroky
Vytvoření mezipaměti a prozkoumejte hello nové funkce úrovně premium.

* [Jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md)
* [Jak podporuje tooconfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md)
* [Jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md)
* [Jak tooimport dat do a exportu dat z Azure Redis Cache](cache-how-to-import-export-data.md)
* [Jak tooadminister Azure mezipaměti Redis](cache-administration.md)

