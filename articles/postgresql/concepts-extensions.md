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
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Rozšíření PostgreSQL v Azure databázi PostgreSQL
PostgreSQL poskytuje hello možnost tooextend hello funkcí databáze pomocí rozšíření. Rozšíření povolit pro více souvisejících toobe objektů SQL sdruženy do jednoho balíčku a dají se načíst nebo odebrat z databáze pomocí jednoho příkazu. Rozšíření jednou načíst do databáze hello, můžou fungovat stejně jako funkce, které jsou součástí. Další informace o rozšíření PostgreSQL najdete v tématu [balení související objekty do rozšíření](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-toouse-postgresql-extensions"></a>Jak toouse PostgreSQL rozšíření?
Rozšíření PostgreSQL musí toobe nainstalován pro vaši databázi, abyste mohli používat. tooinstall konkrétní rozšíření, spusťte [vytvoření rozšíření](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) příkaz z psql nástroj tooload hello zabalené objekty do databáze.

Azure databázi PostgreSQL podporuje podmnožinu klíče rozšíření, jak je uvedeno v tomto poli. Nad rámec hello uvedené nejsou podporovány další rozšíření. Nelze vytvořit vlastní rozšíření s Azure databáze pro službu PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Rozšíření nepodporuje Azure databáze pro PostgreSQL
Hello následující tabulky obsahují seznam hello standardní PostgreSQL rozšíření, která jsou aktuálně podporovány Azure databáze PostgreSQL. Tyto informace můžete získat také pomocí dotazu pg\_k dispozici\_rozšíření. 

### <a name="data-types-extensions"></a>Datové typy rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Poskytuje typu string velká a malá písmena znak |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Poskytuje datový typ pro ukládání sady páry klíč – hodnota |

### <a name="functions-extensions"></a>Funkce rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Poskytuje několik funkcí toodetermine podobnosti a vzdálenost mezi řetězce. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Poskytuje funkce a operátory pro manipulaci s hodnotou null bez pole celých čísel. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Poskytuje kryptografické funkce |
| [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Spravuje dělené tabulky čas nebo ID |
| [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Poskytuje funkce a operátory pro určení hello podobnosti alfanumerické textu na základě shody se trigram |
| [UUID ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Generovat univerzálně jedinečné identifikátory (identifikátory UUID) |

### <a name="full-text-search-extensions"></a>Fulltextové vyhledávání rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Slovník hledání textu, který odebere lexemes akcenty (diakritiky znaky). |

### <a name="index-types-extensions"></a>Index typy rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [Struktura b-stromu\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Poskytuje ukázkové GIN operátor třídy, které implementují B-stromu jako chování u určitých datových typů. |
| [Struktura b-stromu\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Poskytuje GiST index operátor třídy, které implementují B-stromu. |

### <a name="language-extensions"></a>Jazyková rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL načíst procedurální jazyk |

### <a name="miscellaneous-extensions"></a>Ostatní rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [PG\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Poskytuje způsob kontroly, co se děje v mezipaměti hello sdílené vyrovnávací paměti v reálném čase. |
| [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Poskytuje způsob tooload vztah dat do mezipaměti hello vyrovnávací paměti. |
| [PG\_stat\_příkazy](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Poskytuje prostředky pro sledování spuštění statistiky všechny příkazy SQL, které jsou spuštěny serveru. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Obálka cizí dat používá tooaccess dat uložených v externích serverů PostgreSQL |

### <a name="postgis"></a>PostGIS

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal | Prostorová a geografické objekty pro PostgreSQL. |
| Adresa\_standardizer, adresa\_standardizer\_data\_nám | Použít tooparse adresu do základních elementů. Použít toosupport geografické kódování adresu normalizaci krok. |
| [pgrouting](http://pgrouting.org/) | Rozšiřuje hello PostGIS / PostgreSQL geoprostorové databáze tooprovide geoprostorové směrování funkce. |

## <a name="next-steps"></a>Další kroky
Nevidíte rozšíření chcete toouse? Dejte nám vědět. Hlasovat pro existující požadavky nebo vytvořit nový zpětnou vazbu a přání v našem [fóru pro zpětnou vazbu zákazníka](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
