---
title: "databáze škálovatelné cloudové aaaBuilding | Microsoft Docs"
description: "Vývoj aplikací pro škálovatelná databáze .NET pomocí klientské knihovny pro elastické databáze hello"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 1f11c52d-13c1-4994-b9b1-5b1ae2f9255f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: ca34eff2078c0700dee1bc587f264bbfca979eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="building-scalable-cloud-databases"></a>Vytváření škálovatelných cloudových databází
Škálování databáze dá snadno dosáhnout pomocí funkcí a nástrojů pro škálovatelná pro databázi SQL Azure. Konkrétně můžete hello **klientské knihovny pro elastické databáze** toocreate a spravovat upraveným databáze. Tato funkce vám umožní snadno vyvíjet horizontálně dělené aplikace, které používají stovky – nebo dokonce tisíce – databází Azure SQL. [Elastické úlohy](sql-database-elastic-jobs-powershell.md) lze poté použít toohelp usnadňují správu těchto databází.

tooinstall hello knihovně přejděte příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="documentation"></a>Dokumentace
1. [Začínáme s nástroji Elastic Database](sql-database-elastic-scale-get-started.md)
2. [Funkce elastické databáze](sql-database-elastic-scale-introduction.md)
3. [Správa mapování horizontálních oddílů](sql-database-elastic-scale-shard-map-management.md)
4. [Migrovat existující databáze tooscale na více systémů](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
6. [Víc horizontálních dotazy](sql-database-elastic-scale-multishard-querying.md)
7. [Přidání horizontálního oddílu pomocí nástroje elastické databáze](sql-database-elastic-scale-add-a-shard.md)
8. [Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [Upgrade klienta knihovny aplikace](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Přehled elastické dotazy](sql-database-elastic-query-overview.md)
11. [Glosář nástrojů elastické databáze](sql-database-elastic-scale-glossary.md)
12. [Klientská knihovna pro elastické databáze s platformou Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Klientská knihovna pro elastické databáze s Dapper](sql-database-elastic-scale-working-with-dapper.md)
14. [Nástroj rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Čítače výkonu pro správce mapování horizontálních oddílů](sql-database-elastic-database-client-library.md) 
16. [Nejčastější dotazy týkající se nástroje elastické databáze](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Možnosti klienta
Škálování aplikací s použitím *horizontálního dělení* představuje výzvy pro vývojáře hello jak hello správce. Hello klientské knihovny zjednodušíte úlohy správy hello tím, že poskytuje nástroje, které umožní obou vývojáři a správci spravovat upraveným databáze. V Typickým příkladem existuje mnoho databází, označuje jako "horizontálních oddílů," toomanage. Zákazníci ve společném umístění hello stejné databáze, a není jednu databázi na jednu zákazníků (schéma jednoho klienta). Klientská knihovna pro Hello zahrnuje tyto funkce:

- **Správa mapování horizontálních**: speciální databáze názvem hello "horizontálního oddílu mapa správce" byla vytvořena. Správa mapování horizontálních je hello schopnost aplikaci toomanage metadata o jeho horizontálních oddílů. Vývojáři můžou použít tuto funkci tooregister databáze jako horizontálních oddílů, popisují mapování jednotlivých horizontálního dělení klíče nebo klíče rozsahy toothose databáze a zachování tato metadata hello číslo a složení databází zpracovaní tooreflect kapacity změny. Bez hello elastické databáze klientské knihovny bude třeba toospend mnoho času psaní kódu správu hello při implementaci horizontálního dělení. Podrobnosti najdete v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md).

- **Data závislé směrování**: Představte si žádost, než dorazí do aplikace hello. Na základě hello horizontálního dělení klíče hodnoty hello požadavku, aplikace hello musí toodetermine hello správné databáze založená na hodnotě klíče hello. Otevře se pak požadavek připojení toohello databáze tooprocess hello. Závislé směrování dat poskytuje možnost hello tooopen připojení na základě jednoho volání snadno do mapy hello horizontálního oddílu aplikace hello. Data závislé směrování se jiné oblasti infrastruktury kód, který je nyní předmětem funkce v klientské knihovny pro elastické databáze hello. Podrobnosti najdete v tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md).
- **Víc horizontálních dotazy (MSQ)**: dotazování víc horizontálních funguje, když požadavek zahrnuje několik (nebo všechny) horizontálních oddílů. Dotaz s více horizontálních provede hello stejný kód T-SQL na všechny horizontálních oddílů nebo sadu horizontálních oddílů. Hello výsledky z hello účasti horizontálních oddílů jsou sloučeny do celkový výsledek nastavit pomocí sémantiky UNION ALL. Hello funkce jako vystavenou přes hello klientské knihovny zpracovává mnoho úloh, včetně: Správa připojení, správu přístup z více vláken, selhání zpracování a zpracování mezilehlých výsledků. MSQ můžete dotazovat až toohundreds horizontálních oddílů. Podrobnosti najdete v tématu [víc horizontálních dotazování](sql-database-elastic-scale-multishard-querying.md).

Obecně platí zákazníků týkající se použití nástroje elastické databáze můžete očekávat tooget plnou funkčnost T-SQL, při odesílání operace horizontálního oddílu místní jako názvem na rozdíl od toocross horizontálních operace, které mají své vlastní sémantiku.

## <a name="next-steps"></a>Další kroky
Zkuste hello [ukázkovou aplikaci](sql-database-elastic-scale-get-started.md) která představuje funkce klienta hello. 

tooinstall hello knihovně přejděte příliš[klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Pokyny k používání nástroje hello rozdělení sloučení, naleznete hello [nástroji pro sloučení rozdělení přehled](sql-database-elastic-scale-overview-split-and-merge.md).

[Klientská knihovna pro elastické databáze je nyní otevřete z domácích zdrojů!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Použití [elastické dotazy](sql-database-elastic-query-overview.md).

Hello knihovně je k dispozici jako software s otevřeným zdrojem na [Githubu](https://github.com/Azure/elastic-db-tools). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png

