---
title: "Klientská knihovna pro elastické databáze aaaUsing s Dapper | Microsoft Docs"
description: "Klientská knihovna pro elastické databáze pomocí Dapper."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 463d2676-3b19-47c2-83df-f8c50492c9d2
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: c22ece2a977265e93850f0ad3f3ca48f0a8733ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Klientská knihovna pro elastické databáze pomocí Dapper
Tento dokument je pro vývojáře, které spoléhají na Dapper toobuild aplikace, ale také chcete tooembrace [nástroje elastické databáze](sql-database-elastic-scale-introduction.md) toocreate aplikace, které implementují horizontálního dělení tooscale-out jejich datové vrstvy.  Tento dokument ukazuje hello změny v Dapper aplikacím, které jsou nezbytné toointegrate nástroje elastické databáze. Soustředili je na skládání hello elastické databáze horizontálního oddílu správy a závisí na datech směrování s Dapper. 

**Ukázkový kód**: [nástroje elastické databáze pro databázi SQL Azure - Dapper integrace](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Integrace **Dapper** a **DapperExtensions** s hello je snadné klientské knihovny pro elastické databáze pro databázi SQL Azure. Aplikace může použít závislé směrování změnou hello vytváření a otevírání nových dat [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekty toouse hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) volat z hello [klienta Knihovna](http://msdn.microsoft.com/library/azure/dn765902.aspx). Toto nastavení omezuje změny ve vaší aplikace tooonly, kde jsou vytvořen a otevřít nové připojení. 

## <a name="dapper-overview"></a>Dapper – přehled
**Dapper** je objekt relační mapper. Mapuje objekty .NET z vaší aplikace tooa relační databáze (a naopak). první část Hello hello ukázkový kód ukazuje, jak integrovat hello klientské knihovny pro elastické databáze pomocí aplikace založené na Dapper. Druhá část Hello hello ukázkový kód ukazuje, jak toointegrate při používání Dapper a DapperExtensions.  

Funkce mapper Hello v Dapper poskytuje rozšiřující metody u připojení databáze, které zjednodušují odesílá příkazů T-SQL pro provádění nebo dotazování databáze hello. Například Dapper umožňuje snadno toomap mezi objekty .NET a hello parametrů příkazů SQL pro **Execute** volání nebo tooconsume hello výsledky vašich dotazů SQL na objekty .NET pomocí **dotazu**volání z Dapper. 

Při použití DapperExtensions, musíte už tooprovide hello SQL příkazy. Metody rozšíření, jako **GetList** nebo **vložit** přes připojení k databázi hello vytváření hello příkazů SQL pozadí hello.

Další výhodou Dapper a také DapperExtensions je, že ovládací prvky aplikace hello hello vytvoření připojení k databázi hello. To pomáhá interakci s hello elastické databáze klientskou knihovnu, která připojení na základě mapování hello z shardlets toodatabases databáze zprostředkovatelé.

tooget hello Dapper sestavení, najdete v části [Dapper tečka síť](http://www.nuget.org/packages/Dapper/). Hello Dapper rozšíření, najdete v části [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-hello-elastic-database-client-library"></a>Rychlý přehled klientské knihovny pro elastické databáze hello
S hello elastické databáze klientské knihovny, definujete oddíly data aplikací názvem *shardlets* , jejich namapování toodatabases a identifikovat pomocí *horizontálního dělení klíče*. Může mít libovolný počet databází, potřebujete a distribuovat vaše shardlets do těchto databází. mapování Hello horizontálního dělení hodnoty klíče toohello databází ukládá horizontálního oddílu mapu poskytované hello knihovně rozhraní API. Tato funkce je volána **horizontálního oddílu mapy správu**. mapování horizontálních Hello slouží také jako hello zprostředkovatele připojení k databázi pro požadavky, které zajišťují klíč horizontálního dělení. Tato funkce je odkazované tooas **závislé směrování dat**.

![Mapování horizontálních a závislé směrování dat][1]

správce mapy horizontálního oddílu Hello chrání uživatelé z nekonzistentní zobrazení do shardlet data, která může dojít, když operace správy souběžných shardlet se děje v databázích hello. toodo tedy hello horizontálního oddílu mapy zprostředkovatele hello databáze připojení pro aplikace vytvořené s nástroji hello knihovně. Při operacích správy horizontálního oddílu by mohlo mít vliv hello shardlet, tato funkce umožňuje hello horizontálního oddílu mapy funkce tooautomatically kill připojení k databázi. 

Místo použití hello tradičním způsobem, jakým toocreate připojení pro Dapper, potřebujeme toouse hello [OpenConnectionForKey metoda](http://msdn.microsoft.com/library/azure/dn824099.aspx). To zajišťuje, že všemi ověřovacími hello probíhá a připojení spravuje správně, pokud se přesune všechna data mezi horizontálních oddílů.

### <a name="requirements-for-dapper-integration"></a>Požadavky pro Dapper integrace
Při práci s klientské knihovny pro elastické databáze hello i hello Dapper rozhraní API, chceme tooretain hello následující vlastnosti:

* **Škálování**: jsme mají tooadd nebo odebrat databáze z hello datové vrstvy hello horizontálně dělené aplikace podle potřeby pro požadavky na kapacitu hello aplikace hello. 
* **Konzistence**: vzhledem k tomu, že naše aplikace je škálovat na více systémů pomocí horizontálního dělení, potřebujeme tooperform data závislé směrování. Chceme, aby tak toouse hello Data závislé směrování možnosti toodo hello knihovně. Zejména chceme tooretain hello ověření a konzistence zaručuje poskytované připojení, která jsou zprostředkované prostřednictvím správce mapy hello horizontálního oddílu v pořadí tooavoid poškození nebo výsledky nesprávný dotazu. To zajistí, že připojení tooa zadané shardlet jsou odmítl nebo zastavena (například) hello shardlet je aktuálně přesunutý tooa různých horizontálního oddílu pomocí rozhraní API rozdělení nebo Merge, pokud.
* **Mapování objektu**: chceme tooretain hello pohodlí hello mapování poskytované Dapper tootranslate mezi třídy v aplikaci hello a hello základní struktury databáze. 

Hello následující část obsahuje pokyny pro tyto požadavky pro aplikace na základě **Dapper** a **DapperExtensions**.

## <a name="technical-guidance"></a>Technické pokyny
### <a name="data-dependent-routing-with-dapper"></a>Závislé směrování s Dapper dat
S Dapper je obvykle zodpovědná za vytváření a otevírání hello připojení toohello základní databáze aplikace hello. Zadaný typ T hello aplikací, Dapper vrátí výsledky dotazu jako kolekcí .NET typu T. Dapper provede mapování hello z hello T-SQL výsledek řádky toohello objekty typu T. Podobně Dapper mapuje objekty .NET do hodnot SQL nebo parametry pro příkazy data manipulaci language (DML). Dapper nabízí tuto funkci prostřednictvím metody rozšíření na hello regulární [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekt z knihoven ADO .NET SQL Client hello. Hello připojení SQL vrácený hello elastické škálování rozhraní API pro DDR jsou také regulární [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekty. To umožňuje nám Dapper rozšíření použití toodirectly přes hello typ vrácený rozhraním API DDR hello klientské knihovny, protože je zároveň jednoduchého připojení klienta SQL.

Tyto připomínky díky přehledné toouse připojení pro Dapper zprostředkované pomocí klientské knihovny pro elastické databáze hello.

Tento příklad kódu (z hello doplňujícími ukázkové) znázorňuje hello přístup, kde je klíč horizontálního dělení hello poskytované hello aplikace toohello knihovny toobroker hello připojení toohello správné horizontálního oddílu.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Hello volání toohello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) rozhraní API nahrazuje hello výchozí vytváření a otevírání připojení klienta SQL. Hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) volání má hello argumenty, které jsou požadovány pro závislé směrování dat: 

* Hello horizontálního oddílu mapy tooaccess hello závislé směrování rozhraní dat
* Hello horizontálního dělení klíče tooidentify hello shardlet
* Hello horizontálního oddílu toohello tooconnect přihlašovací údaje (uživatelské jméno a heslo)

objekt map horizontálního oddílu Hello vytvoří horizontálních toohello připojení, která obsahuje hello shardlet pro zadaný klíč horizontálního dělení hello. rozhraní API klienta elastické databáze Hello také označovat tooimplement hello připojení, které zaručuje jeho konzistence. Od hello volání příliš[OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) vrátí regulární objekt připojení klienta SQL, hello následných volání toohello **Execute** metoda rozšíření z Dapper způsobem hello standardní Dapper postup.

Dotazy na pracovní velmi mnohem hello stejný způsobem - poprvé otevřete pomocí připojení hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) z klienta hello rozhraní API. Potom do objektů .NET pomocí hello regulární Dapper rozšíření metody toomap hello výsledky dotazu SQL:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Všimněte si, že hello **pomocí** blokovat pomocí oborů připojení DDR hello všechny databázové operace v rámci hello bloku toohello jeden horizontálních kde je udržována tenantId1. Hello dotaz vrátí jenom blogy uložené na aktuální horizontálního oddílu hello, ale není hello ty, které jsou uložené na jiné horizontálních oddílů. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Data závislé směrování s Dapper a DapperExtensions
Dapper se dodává s prostředí další rozšíření, které můžete zadat další pohodlí a abstrakce z databáze hello při vývoji aplikace databáze. DapperExtensions je příklad. 

Použití DapperExtensions v aplikaci se nezměnil způsob vytváření a spravovaná databázová připojení. Je stále aplikace hello odpovědnost tooopen připojení a regulární objekty připojení klienta SQL se očekává metodami rozšíření hello. Spoléháme na hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) jak je uvedeno výš. Jako hello zobrazit následující ukázky kódu hello jedinou změnou je, že jsme již nemáte toowrite hello T-SQL příkazy:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

A zde je ukázka kódu hello hello dotazu: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Zpracování přechodných chyb
Hello postupy společnosti Microsoft Patterns team publikované hello [přechodné chyby zpracování bloku aplikace](http://msdn.microsoft.com/library/hh680934.aspx) vývojáři aplikace toohelp zmírnit běžné podmínky přechodná chyba došlo při spuštění v cloudu hello. Další informace najdete v tématu [Perseverance, tajný klíč všechny vítězství: hello bloku přechodné chyby zpracování aplikace pomocí](http://msdn.microsoft.com/library/dn440719.aspx).

Ukázka kódu Hello spoléhá na hello přechodná chyba knihovny tooprotect proti přechodných. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** v hello výše uvedený kód je definovaný jako **SqlDatabaseTransientErrorDetectionStrategy** s počtem opakování 10 a 5 sekund čekací dobu mezi opakovanými pokusy. Pokud používáte transakce, ujistěte se, že váš rozsah opakování přejde zpět toohello zahájení transakce hello v případě hello přechodná chyba.

## <a name="limitations"></a>Omezení
přístupy Hello uvedených v tomto dokumentu za následek několik omezení:

* Hello ukázkový kód pro tento dokument není ukazují, jak toomanage schématu napříč horizontálních oddílů.
* Zadaný požadavek, předpokládáme, že jeho zpracování databáze je obsažena v jedné horizontálního oddílu určeno klíčem horizontálního dělení hello poskytované hello požadavku. Však tento předpoklad vždy nemá, například když není možné toomake horizontálního dělení klíč, který je k dispozici. tooaddress se hello klientské knihovny pro elastické databáze zahrnuje hello [MultiShardQuery třída](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Hello třída implementuje abstraktní připojení pro dotazování přes několik horizontálních oddílů. Pomocí MultiShardQuery v kombinaci s Dapper je nad rámec tohoto dokumentu hello.

## <a name="conclusion"></a>Závěr
Aplikace s použitím Dapper a DapperExtensions můžete snadno využívat nástroje elastické databáze pro databázi SQL Azure. Prostřednictvím hello kroků uvedených v tomto dokumentu, můžete tyto aplikace použít nástroj hello schopností závislé směrování změnou hello vytváření a otevírání nových dat [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekty toouse hello [ OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) volání hello klientské knihovny pro elastické databáze. Toto nastavení omezuje hello aplikace změny požadované toothose místech, kde nová připojení jsou vytvořeny a otevřít. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
