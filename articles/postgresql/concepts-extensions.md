---
title: "Pomocí rozšíření PostgreSQL v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Popisuje možnost rozšířit funkce vaší databáze pomocí rozšíření v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.openlocfilehash: 755d1cf1a921f6be8f28a4a8ae515db08d904fcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a><span data-ttu-id="bb6b2-103">Rozšíření PostgreSQL v Azure databázi PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="bb6b2-103">PostgreSQL Extensions in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="bb6b2-104">PostgreSQL poskytuje možnost rozšířit funkce vaší databáze pomocí rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-104">PostgreSQL provides the ability to extend the functionality of your database using extensions.</span></span> <span data-ttu-id="bb6b2-105">Rozšíření umožňují více příbuzných objektů SQL být sdruženy do jednoho balíčku a může načíst nebo odebrání z databáze pomocí jednoho příkazu.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-105">Extensions allow for multiple related SQL objects to be bundled together in a single package and can be loaded or removed from your database with a single command.</span></span> <span data-ttu-id="bb6b2-106">Rozšíření jednou načíst do databáze, můžou fungovat stejně jako funkce, které jsou součástí.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-106">Extensions once loaded into the database can function just like features that are built in.</span></span> <span data-ttu-id="bb6b2-107">Další informace o rozšíření PostgreSQL najdete v tématu [balení související objekty do rozšíření](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span><span class="sxs-lookup"><span data-stu-id="bb6b2-107">For more information on PostgreSQL extensions, see [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span></span>

## <a name="how-to-use-postgresql-extensions"></a><span data-ttu-id="bb6b2-108">Jak používat rozšíření PostgreSQL?</span><span class="sxs-lookup"><span data-stu-id="bb6b2-108">How to use PostgreSQL extensions?</span></span>
<span data-ttu-id="bb6b2-109">PostgreSQL rozšíření je potřeba nainstalovat pro vaši databázi, abyste mohli používat.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-109">PostgreSQL extensions need to be installed for your database before you can use them.</span></span> <span data-ttu-id="bb6b2-110">Pokud chcete nainstalovat konkrétní rozšíření, spusťte [vytvoření rozšíření](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) příkaz psql nástroj pro načítání zabalené objektů do databáze.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-110">To install a particular extension, run the [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) command from psql tool to load the packaged objects into your database.</span></span>

<span data-ttu-id="bb6b2-111">Azure databázi PostgreSQL podporuje podmnožinu klíče rozšíření, jak je uvedeno v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-111">Azure Database for PostgreSQL supports a subset of key extensions as listed here.</span></span> <span data-ttu-id="bb6b2-112">Nad rámec jsou uvedené další rozšíření nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-112">Beyond the ones listed, other extensions are not supported.</span></span> <span data-ttu-id="bb6b2-113">Nelze vytvořit vlastní rozšíření s Azure databáze pro službu PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-113">You cannot create your own extension with Azure Database for PostgreSQL service.</span></span>

## <a name="extensions-supported-by-azure-database-for-postgresql"></a><span data-ttu-id="bb6b2-114">Rozšíření nepodporuje Azure databáze pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="bb6b2-114">Extensions supported by Azure Database for PostgreSQL</span></span>
<span data-ttu-id="bb6b2-115">V následujících tabulkách najdete standardní PostgreSQL rozšíření, které jsou aktuálně podporovány Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-115">The following tables list the standard PostgreSQL extensions that are currently supported by Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bb6b2-116">Tyto informace můžete získat také pomocí dotazu pg\_k dispozici\_rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-116">You can also obtain this information by querying pg\_available\_extensions.</span></span> 

### <a name="data-types-extensions"></a><span data-ttu-id="bb6b2-117">Datové typy rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb6b2-117">Data types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-118">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-118">**Extension**</span></span> | <span data-ttu-id="bb6b2-119">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-119">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="bb6b2-120">citext</span><span class="sxs-lookup"><span data-stu-id="bb6b2-120">citext</span></span>](https://www.postgresql.org/docs/9.6/static/citext.html) | <span data-ttu-id="bb6b2-121">Poskytuje typu string velká a malá písmena znak</span><span class="sxs-lookup"><span data-stu-id="bb6b2-121">Provides a case-insensitive character string type</span></span> |
| [<span data-ttu-id="bb6b2-122">hstore</span><span class="sxs-lookup"><span data-stu-id="bb6b2-122">hstore</span></span>](https://www.postgresql.org/docs/9.6/static/hstore.html) | <span data-ttu-id="bb6b2-123">Poskytuje datový typ pro ukládání sady páry klíč – hodnota</span><span class="sxs-lookup"><span data-stu-id="bb6b2-123">Provides data type for storing sets of key/value pairs</span></span> |

### <a name="functions-extensions"></a><span data-ttu-id="bb6b2-124">Funkce rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb6b2-124">Functions extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-125">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-125">**Extension**</span></span> | <span data-ttu-id="bb6b2-126">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-126">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="bb6b2-127">fuzzystrmatch</span><span class="sxs-lookup"><span data-stu-id="bb6b2-127">fuzzystrmatch</span></span>](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | <span data-ttu-id="bb6b2-128">Poskytuje několik funkcí k určení podobnosti a vzdálenost mezi řetězce.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-128">Provides several functions to determine similarities and distance between strings.</span></span> |
| [<span data-ttu-id="bb6b2-129">intarray</span><span class="sxs-lookup"><span data-stu-id="bb6b2-129">intarray</span></span>](https://www.postgresql.org/docs/9.6/static/intarray.html) | <span data-ttu-id="bb6b2-130">Poskytuje funkce a operátory pro manipulaci s hodnotou null bez pole celých čísel.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-130">Provides functions and operators for manipulating null-free arrays of integers.</span></span> |
| [<span data-ttu-id="bb6b2-131">pgcrypto</span><span class="sxs-lookup"><span data-stu-id="bb6b2-131">pgcrypto</span></span>](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | <span data-ttu-id="bb6b2-132">Poskytuje kryptografické funkce</span><span class="sxs-lookup"><span data-stu-id="bb6b2-132">Provides cryptographic functions</span></span> |
| [<span data-ttu-id="bb6b2-133">PG\_partman</span><span class="sxs-lookup"><span data-stu-id="bb6b2-133">pg\_partman</span></span>](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | <span data-ttu-id="bb6b2-134">Spravuje dělené tabulky čas nebo ID</span><span class="sxs-lookup"><span data-stu-id="bb6b2-134">Manages partitioned tables by time or ID</span></span> |
| [<span data-ttu-id="bb6b2-135">PG\_trgm</span><span class="sxs-lookup"><span data-stu-id="bb6b2-135">pg\_trgm</span></span>](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | <span data-ttu-id="bb6b2-136">Poskytuje funkce a operátory pro určení podobnosti alfanumerické text na základě shody se trigram</span><span class="sxs-lookup"><span data-stu-id="bb6b2-136">Provides functions and operators for determining the similarity of alphanumeric text based on trigram matching</span></span> |
| [<span data-ttu-id="bb6b2-137">UUID ossp</span><span class="sxs-lookup"><span data-stu-id="bb6b2-137">uuid-ossp</span></span>](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | <span data-ttu-id="bb6b2-138">Generovat univerzálně jedinečné identifikátory (identifikátory UUID)</span><span class="sxs-lookup"><span data-stu-id="bb6b2-138">Generate universally unique identifiers (UUIDs)</span></span> |

### <a name="full-text-search-extensions"></a><span data-ttu-id="bb6b2-139">Fulltextové vyhledávání rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb6b2-139">Full-text Search extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-140">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-140">**Extension**</span></span> | <span data-ttu-id="bb6b2-141">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-141">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="bb6b2-142">unaccent</span><span class="sxs-lookup"><span data-stu-id="bb6b2-142">unaccent</span></span>](https://www.postgresql.org/docs/9.6/static/unaccent.html) | <span data-ttu-id="bb6b2-143">Slovník hledání textu, který odebere lexemes akcenty (diakritiky znaky).</span><span class="sxs-lookup"><span data-stu-id="bb6b2-143">A text search dictionary that removes accents (diacritic signs) from lexemes.</span></span> |

### <a name="index-types-extensions"></a><span data-ttu-id="bb6b2-144">Index typy rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb6b2-144">Index Types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-145">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-145">**Extension**</span></span> | <span data-ttu-id="bb6b2-146">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-146">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="bb6b2-147">Struktura b-stromu\_gin</span><span class="sxs-lookup"><span data-stu-id="bb6b2-147">btree\_gin</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | <span data-ttu-id="bb6b2-148">Poskytuje ukázkové GIN operátor třídy, které implementují B-stromu jako chování u určitých datových typů.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-148">Provides sample GIN operator classes that implement B-tree like behavior for certain data types.</span></span> |
| [<span data-ttu-id="bb6b2-149">Struktura b-stromu\_gist</span><span class="sxs-lookup"><span data-stu-id="bb6b2-149">btree\_gist</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | <span data-ttu-id="bb6b2-150">Poskytuje GiST index operátor třídy, které implementují B-stromu.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-150">Provides GiST index operator classes that implement B-tree.</span></span> |

### <a name="language-extensions"></a><span data-ttu-id="bb6b2-151">Jazyková rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb6b2-151">Language extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-152">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-152">**Extension**</span></span> | <span data-ttu-id="bb6b2-153">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-153">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="bb6b2-154">plpgsql</span><span class="sxs-lookup"><span data-stu-id="bb6b2-154">plpgsql</span></span>](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | <span data-ttu-id="bb6b2-155">PL/pgSQL načíst procedurální jazyk</span><span class="sxs-lookup"><span data-stu-id="bb6b2-155">PL/pgSQL loadable procedural language</span></span> |

### <a name="miscellaneous-extensions"></a><span data-ttu-id="bb6b2-156">Ostatní rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb6b2-156">Miscellaneous extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-157">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-157">**Extension**</span></span> | <span data-ttu-id="bb6b2-158">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-158">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="bb6b2-159">PG\_buffercache</span><span class="sxs-lookup"><span data-stu-id="bb6b2-159">pg\_buffercache</span></span>](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | <span data-ttu-id="bb6b2-160">Poskytuje způsob kontroly, co se děje v mezipaměti sdílené vyrovnávací paměti v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-160">Provides a means for examining what's happening in the shared buffer cache in real time.</span></span> |
| [<span data-ttu-id="bb6b2-161">PG\_prewarm</span><span class="sxs-lookup"><span data-stu-id="bb6b2-161">pg\_prewarm</span></span>](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | <span data-ttu-id="bb6b2-162">Poskytuje způsob, jak načíst data relace do mezipaměti vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-162">Provides a way to load relation data into the buffer cache.</span></span> |
| [<span data-ttu-id="bb6b2-163">PG\_stat\_příkazy</span><span class="sxs-lookup"><span data-stu-id="bb6b2-163">pg\_stat\_statements</span></span>](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | <span data-ttu-id="bb6b2-164">Poskytuje prostředky pro sledování spuštění statistiky všechny příkazy SQL, které jsou spuštěny serveru.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-164">Provides a means for tracking execution statistics of all SQL statements executed by a server.</span></span> |
| [<span data-ttu-id="bb6b2-165">postgres\_fdw</span><span class="sxs-lookup"><span data-stu-id="bb6b2-165">postgres\_fdw</span></span>](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | <span data-ttu-id="bb6b2-166">Obálka cizí dat používá pro přístup k dat uložených v externích serverů PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="bb6b2-166">Foreign-data wrapper used to access data stored in external PostgreSQL servers</span></span> |

### <a name="postgis"></a><span data-ttu-id="bb6b2-167">PostGIS</span><span class="sxs-lookup"><span data-stu-id="bb6b2-167">PostGIS</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="bb6b2-168">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-168">**Extension**</span></span> | <span data-ttu-id="bb6b2-169">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bb6b2-169">**Description**</span></span> |
|---|---|
| <span data-ttu-id="bb6b2-170">[PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal</span><span class="sxs-lookup"><span data-stu-id="bb6b2-170">[PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal</span></span> | <span data-ttu-id="bb6b2-171">Prostorová a geografické objekty pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-171">Spatial and geographic objects for PostgreSQL.</span></span> |
| <span data-ttu-id="bb6b2-172">Adresa\_standardizer, adresa\_standardizer\_data\_nám</span><span class="sxs-lookup"><span data-stu-id="bb6b2-172">address\_standardizer, address\_standardizer\_data\_us</span></span> | <span data-ttu-id="bb6b2-173">Použít k analýze adresu do základní elementy.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-173">Used to parse an address into constituent elements.</span></span> <span data-ttu-id="bb6b2-174">Použít pro podporu určování zeměpisných souřadnic adresu normalizaci krok.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-174">Used to support geocoding address normalization step.</span></span> |
| [<span data-ttu-id="bb6b2-175">pgrouting</span><span class="sxs-lookup"><span data-stu-id="bb6b2-175">pgrouting</span></span>](http://pgrouting.org/) | <span data-ttu-id="bb6b2-176">Rozšiřuje PostGIS / PostgreSQL geoprostorové databázi, aby zajistil geoprostorové směrování funkce.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-176">Extends the PostGIS / PostgreSQL geospatial database to provide geospatial routing functionality.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bb6b2-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb6b2-177">Next steps</span></span>
<span data-ttu-id="bb6b2-178">Nevidíte rozšíření, které chcete použít?</span><span class="sxs-lookup"><span data-stu-id="bb6b2-178">Don't see an extension you'd like to use?</span></span> <span data-ttu-id="bb6b2-179">Dejte nám vědět.</span><span class="sxs-lookup"><span data-stu-id="bb6b2-179">Please let us know.</span></span> <span data-ttu-id="bb6b2-180">Hlasovat pro existující požadavky nebo vytvořit nový zpětnou vazbu a přání v našem [fóru pro zpětnou vazbu zákazníka](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span><span class="sxs-lookup"><span data-stu-id="bb6b2-180">Vote for existing requests or create new feedback and wishes in our [Customer feedback forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span></span>
