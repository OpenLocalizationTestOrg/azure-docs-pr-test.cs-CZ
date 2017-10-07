---
title: "rozšíření PostgreSQL aaaUsing v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Popisuje hello možnost tooextend hello funkcí databáze pomocí rozšíření v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.openlocfilehash: af2462d7a923b934bc0329153be7079ba86e8856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a><span data-ttu-id="48cae-103">Rozšíření PostgreSQL v Azure databázi PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="48cae-103">PostgreSQL Extensions in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="48cae-104">PostgreSQL poskytuje hello možnost tooextend hello funkcí databáze pomocí rozšíření.</span><span class="sxs-lookup"><span data-stu-id="48cae-104">PostgreSQL provides hello ability tooextend hello functionality of your database using extensions.</span></span> <span data-ttu-id="48cae-105">Rozšíření povolit pro více souvisejících toobe objektů SQL sdruženy do jednoho balíčku a dají se načíst nebo odebrat z databáze pomocí jednoho příkazu.</span><span class="sxs-lookup"><span data-stu-id="48cae-105">Extensions allow for multiple related SQL objects toobe bundled together in a single package and can be loaded or removed from your database with a single command.</span></span> <span data-ttu-id="48cae-106">Rozšíření jednou načíst do databáze hello, můžou fungovat stejně jako funkce, které jsou součástí.</span><span class="sxs-lookup"><span data-stu-id="48cae-106">Extensions once loaded into hello database can function just like features that are built in.</span></span> <span data-ttu-id="48cae-107">Další informace o rozšíření PostgreSQL najdete v tématu [balení související objekty do rozšíření](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span><span class="sxs-lookup"><span data-stu-id="48cae-107">For more information on PostgreSQL extensions, see [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span></span>

## <a name="how-toouse-postgresql-extensions"></a><span data-ttu-id="48cae-108">Jak toouse PostgreSQL rozšíření?</span><span class="sxs-lookup"><span data-stu-id="48cae-108">How toouse PostgreSQL extensions?</span></span>
<span data-ttu-id="48cae-109">Rozšíření PostgreSQL musí toobe nainstalován pro vaši databázi, abyste mohli používat.</span><span class="sxs-lookup"><span data-stu-id="48cae-109">PostgreSQL extensions need toobe installed for your database before you can use them.</span></span> <span data-ttu-id="48cae-110">tooinstall konkrétní rozšíření, spusťte [vytvoření rozšíření](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) příkaz z psql nástroj tooload hello zabalené objekty do databáze.</span><span class="sxs-lookup"><span data-stu-id="48cae-110">tooinstall a particular extension, run the [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) command from psql tool tooload hello packaged objects into your database.</span></span>

<span data-ttu-id="48cae-111">Azure databázi PostgreSQL podporuje podmnožinu klíče rozšíření, jak je uvedeno v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="48cae-111">Azure Database for PostgreSQL supports a subset of key extensions as listed here.</span></span> <span data-ttu-id="48cae-112">Nad rámec hello uvedené nejsou podporovány další rozšíření.</span><span class="sxs-lookup"><span data-stu-id="48cae-112">Beyond hello ones listed, other extensions are not supported.</span></span> <span data-ttu-id="48cae-113">Nelze vytvořit vlastní rozšíření s Azure databáze pro službu PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="48cae-113">You cannot create your own extension with Azure Database for PostgreSQL service.</span></span>

## <a name="extensions-supported-by-azure-database-for-postgresql"></a><span data-ttu-id="48cae-114">Rozšíření nepodporuje Azure databáze pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="48cae-114">Extensions supported by Azure Database for PostgreSQL</span></span>
<span data-ttu-id="48cae-115">Hello následující tabulky obsahují seznam hello standardní PostgreSQL rozšíření, která jsou aktuálně podporovány Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="48cae-115">hello following tables list hello standard PostgreSQL extensions that are currently supported by Azure Database for PostgreSQL.</span></span> <span data-ttu-id="48cae-116">Tyto informace můžete získat také pomocí dotazu pg\_k dispozici\_rozšíření.</span><span class="sxs-lookup"><span data-stu-id="48cae-116">You can also obtain this information by querying pg\_available\_extensions.</span></span> 

### <a name="data-types-extensions"></a><span data-ttu-id="48cae-117">Datové typy rozšíření</span><span class="sxs-lookup"><span data-stu-id="48cae-117">Data types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-118">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-118">**Extension**</span></span> | <span data-ttu-id="48cae-119">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-119">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="48cae-120">citext</span><span class="sxs-lookup"><span data-stu-id="48cae-120">citext</span></span>](https://www.postgresql.org/docs/9.6/static/citext.html) | <span data-ttu-id="48cae-121">Poskytuje typu string velká a malá písmena znak</span><span class="sxs-lookup"><span data-stu-id="48cae-121">Provides a case-insensitive character string type</span></span> |
| [<span data-ttu-id="48cae-122">hstore</span><span class="sxs-lookup"><span data-stu-id="48cae-122">hstore</span></span>](https://www.postgresql.org/docs/9.6/static/hstore.html) | <span data-ttu-id="48cae-123">Poskytuje datový typ pro ukládání sady páry klíč – hodnota</span><span class="sxs-lookup"><span data-stu-id="48cae-123">Provides data type for storing sets of key/value pairs</span></span> |

### <a name="functions-extensions"></a><span data-ttu-id="48cae-124">Funkce rozšíření</span><span class="sxs-lookup"><span data-stu-id="48cae-124">Functions extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-125">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-125">**Extension**</span></span> | <span data-ttu-id="48cae-126">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-126">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="48cae-127">fuzzystrmatch</span><span class="sxs-lookup"><span data-stu-id="48cae-127">fuzzystrmatch</span></span>](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | <span data-ttu-id="48cae-128">Poskytuje několik funkcí toodetermine podobnosti a vzdálenost mezi řetězce.</span><span class="sxs-lookup"><span data-stu-id="48cae-128">Provides several functions toodetermine similarities and distance between strings.</span></span> |
| [<span data-ttu-id="48cae-129">intarray</span><span class="sxs-lookup"><span data-stu-id="48cae-129">intarray</span></span>](https://www.postgresql.org/docs/9.6/static/intarray.html) | <span data-ttu-id="48cae-130">Poskytuje funkce a operátory pro manipulaci s hodnotou null bez pole celých čísel.</span><span class="sxs-lookup"><span data-stu-id="48cae-130">Provides functions and operators for manipulating null-free arrays of integers.</span></span> |
| [<span data-ttu-id="48cae-131">pgcrypto</span><span class="sxs-lookup"><span data-stu-id="48cae-131">pgcrypto</span></span>](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | <span data-ttu-id="48cae-132">Poskytuje kryptografické funkce</span><span class="sxs-lookup"><span data-stu-id="48cae-132">Provides cryptographic functions</span></span> |
| [<span data-ttu-id="48cae-133">PG\_partman</span><span class="sxs-lookup"><span data-stu-id="48cae-133">pg\_partman</span></span>](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | <span data-ttu-id="48cae-134">Spravuje dělené tabulky čas nebo ID</span><span class="sxs-lookup"><span data-stu-id="48cae-134">Manages partitioned tables by time or ID</span></span> |
| [<span data-ttu-id="48cae-135">PG\_trgm</span><span class="sxs-lookup"><span data-stu-id="48cae-135">pg\_trgm</span></span>](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | <span data-ttu-id="48cae-136">Poskytuje funkce a operátory pro určení hello podobnosti alfanumerické textu na základě shody se trigram</span><span class="sxs-lookup"><span data-stu-id="48cae-136">Provides functions and operators for determining hello similarity of alphanumeric text based on trigram matching</span></span> |
| [<span data-ttu-id="48cae-137">UUID ossp</span><span class="sxs-lookup"><span data-stu-id="48cae-137">uuid-ossp</span></span>](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | <span data-ttu-id="48cae-138">Generovat univerzálně jedinečné identifikátory (identifikátory UUID)</span><span class="sxs-lookup"><span data-stu-id="48cae-138">Generate universally unique identifiers (UUIDs)</span></span> |

### <a name="full-text-search-extensions"></a><span data-ttu-id="48cae-139">Fulltextové vyhledávání rozšíření</span><span class="sxs-lookup"><span data-stu-id="48cae-139">Full-text Search extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-140">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-140">**Extension**</span></span> | <span data-ttu-id="48cae-141">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-141">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="48cae-142">unaccent</span><span class="sxs-lookup"><span data-stu-id="48cae-142">unaccent</span></span>](https://www.postgresql.org/docs/9.6/static/unaccent.html) | <span data-ttu-id="48cae-143">Slovník hledání textu, který odebere lexemes akcenty (diakritiky znaky).</span><span class="sxs-lookup"><span data-stu-id="48cae-143">A text search dictionary that removes accents (diacritic signs) from lexemes.</span></span> |

### <a name="index-types-extensions"></a><span data-ttu-id="48cae-144">Index typy rozšíření</span><span class="sxs-lookup"><span data-stu-id="48cae-144">Index Types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-145">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-145">**Extension**</span></span> | <span data-ttu-id="48cae-146">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-146">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="48cae-147">Struktura b-stromu\_gin</span><span class="sxs-lookup"><span data-stu-id="48cae-147">btree\_gin</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | <span data-ttu-id="48cae-148">Poskytuje ukázkové GIN operátor třídy, které implementují B-stromu jako chování u určitých datových typů.</span><span class="sxs-lookup"><span data-stu-id="48cae-148">Provides sample GIN operator classes that implement B-tree like behavior for certain data types.</span></span> |
| [<span data-ttu-id="48cae-149">Struktura b-stromu\_gist</span><span class="sxs-lookup"><span data-stu-id="48cae-149">btree\_gist</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | <span data-ttu-id="48cae-150">Poskytuje GiST index operátor třídy, které implementují B-stromu.</span><span class="sxs-lookup"><span data-stu-id="48cae-150">Provides GiST index operator classes that implement B-tree.</span></span> |

### <a name="language-extensions"></a><span data-ttu-id="48cae-151">Jazyková rozšíření</span><span class="sxs-lookup"><span data-stu-id="48cae-151">Language extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-152">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-152">**Extension**</span></span> | <span data-ttu-id="48cae-153">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-153">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="48cae-154">plpgsql</span><span class="sxs-lookup"><span data-stu-id="48cae-154">plpgsql</span></span>](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | <span data-ttu-id="48cae-155">PL/pgSQL načíst procedurální jazyk</span><span class="sxs-lookup"><span data-stu-id="48cae-155">PL/pgSQL loadable procedural language</span></span> |

### <a name="miscellaneous-extensions"></a><span data-ttu-id="48cae-156">Ostatní rozšíření</span><span class="sxs-lookup"><span data-stu-id="48cae-156">Miscellaneous extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-157">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-157">**Extension**</span></span> | <span data-ttu-id="48cae-158">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-158">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="48cae-159">PG\_buffercache</span><span class="sxs-lookup"><span data-stu-id="48cae-159">pg\_buffercache</span></span>](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | <span data-ttu-id="48cae-160">Poskytuje způsob kontroly, co se děje v mezipaměti hello sdílené vyrovnávací paměti v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="48cae-160">Provides a means for examining what's happening in hello shared buffer cache in real time.</span></span> |
| [<span data-ttu-id="48cae-161">PG\_prewarm</span><span class="sxs-lookup"><span data-stu-id="48cae-161">pg\_prewarm</span></span>](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | <span data-ttu-id="48cae-162">Poskytuje způsob tooload vztah dat do mezipaměti hello vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="48cae-162">Provides a way tooload relation data into hello buffer cache.</span></span> |
| [<span data-ttu-id="48cae-163">PG\_stat\_příkazy</span><span class="sxs-lookup"><span data-stu-id="48cae-163">pg\_stat\_statements</span></span>](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | <span data-ttu-id="48cae-164">Poskytuje prostředky pro sledování spuštění statistiky všechny příkazy SQL, které jsou spuštěny serveru.</span><span class="sxs-lookup"><span data-stu-id="48cae-164">Provides a means for tracking execution statistics of all SQL statements executed by a server.</span></span> |
| [<span data-ttu-id="48cae-165">postgres\_fdw</span><span class="sxs-lookup"><span data-stu-id="48cae-165">postgres\_fdw</span></span>](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | <span data-ttu-id="48cae-166">Obálka cizí dat používá tooaccess dat uložených v externích serverů PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="48cae-166">Foreign-data wrapper used tooaccess data stored in external PostgreSQL servers</span></span> |

### <a name="postgis"></a><span data-ttu-id="48cae-167">PostGIS</span><span class="sxs-lookup"><span data-stu-id="48cae-167">PostGIS</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="48cae-168">**Rozšíření**</span><span class="sxs-lookup"><span data-stu-id="48cae-168">**Extension**</span></span> | <span data-ttu-id="48cae-169">**Popis**</span><span class="sxs-lookup"><span data-stu-id="48cae-169">**Description**</span></span> |
|---|---|
| <span data-ttu-id="48cae-170">[PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal</span><span class="sxs-lookup"><span data-stu-id="48cae-170">[PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal</span></span> | <span data-ttu-id="48cae-171">Prostorová a geografické objekty pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="48cae-171">Spatial and geographic objects for PostgreSQL.</span></span> |
| <span data-ttu-id="48cae-172">Adresa\_standardizer, adresa\_standardizer\_data\_nám</span><span class="sxs-lookup"><span data-stu-id="48cae-172">address\_standardizer, address\_standardizer\_data\_us</span></span> | <span data-ttu-id="48cae-173">Použít tooparse adresu do základních elementů.</span><span class="sxs-lookup"><span data-stu-id="48cae-173">Used tooparse an address into constituent elements.</span></span> <span data-ttu-id="48cae-174">Použít toosupport geografické kódování adresu normalizaci krok.</span><span class="sxs-lookup"><span data-stu-id="48cae-174">Used toosupport geocoding address normalization step.</span></span> |
| [<span data-ttu-id="48cae-175">pgrouting</span><span class="sxs-lookup"><span data-stu-id="48cae-175">pgrouting</span></span>](http://pgrouting.org/) | <span data-ttu-id="48cae-176">Rozšiřuje hello PostGIS / PostgreSQL geoprostorové databáze tooprovide geoprostorové směrování funkce.</span><span class="sxs-lookup"><span data-stu-id="48cae-176">Extends hello PostGIS / PostgreSQL geospatial database tooprovide geospatial routing functionality.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="48cae-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48cae-177">Next steps</span></span>
<span data-ttu-id="48cae-178">Nevidíte rozšíření chcete toouse?</span><span class="sxs-lookup"><span data-stu-id="48cae-178">Don't see an extension you'd like toouse?</span></span> <span data-ttu-id="48cae-179">Dejte nám vědět.</span><span class="sxs-lookup"><span data-stu-id="48cae-179">Please let us know.</span></span> <span data-ttu-id="48cae-180">Hlasovat pro existující požadavky nebo vytvořit nový zpětnou vazbu a přání v našem [fóru pro zpětnou vazbu zákazníka](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span><span class="sxs-lookup"><span data-stu-id="48cae-180">Vote for existing requests or create new feedback and wishes in our [Customer feedback forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span></span>
