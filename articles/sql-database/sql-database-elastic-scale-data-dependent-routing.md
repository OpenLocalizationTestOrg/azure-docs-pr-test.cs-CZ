---
title: "aaaData závislé směrování s Azure SQL Database | Microsoft Docs"
description: "Jak toouse hello ShardMapManager třídy v aplikacích .NET pro data závislé na směrování, funkce horizontálně dělené databází ve službě Azure SQL Database"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 34014508ae01905686791fe096bb275cb84f53b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-dependent-routing"></a>Směrování závislé na datech
**Data závislé směrování** je hello možnost toouse hello data v dotazu tooroute hello požadavek tooan příslušné databázi. Toto je základní vzor při práci s horizontálně dělené databáze. kontext požadavku Hello může být také použít tooroute hello požadavku, zvlášť pokud hello horizontálního dělení klíč není součástí hello dotazu. Každé specifického dotazu nebo transakce v aplikaci pomocí závislé směrování dat je omezená tooaccessing jednu databázi na základě požadavku. Azure SQL Database elastické nástroje hello, tento směrování se provádí s hello  **[ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  v aplikacích ADO.NET.

aplikace Hello nemusí tootrack různé připojovací řetězce nebo DB umístění přidružené k jiné řezy dat v horizontálně dělené prostředí hello. Místo toho hello [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md) otevře připojení toohello správné databází v případě potřeby na základě hello dat hello horizontálního oddílu mapy a hello hodnota hello horizontálního dělení klíče, který je cílem hello žádostí na aplikace hello. klíč Hello je obvykle hello *customer_id*, *tenant_id*, *date_key*, nebo některé konkrétní identifikátor, který je základní parametr požadavek databáze hello). 

Další informace najdete v tématu [škálování SQL serveru se Škálováním s závislé směrování dat](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-hello-client-library"></a>Stáhnout klientskou knihovnu hello
Třída hello tooget, instalace hello [klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Použití ShardMapManager v závislou aplikaci dat směrování
Aplikace by měl vytvořit instanci hello **ShardMapManager** během inicializace pomocí volání objektu pro vytváření hello  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. V tomto příkladu jak **ShardMapManager** a konkrétní **ShardMap** ji obsahující jsou inicializovány. Tento příklad ukazuje hello GetSqlShardMapManager a [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metody.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-hello-shard-map"></a>Použít pověření nejnižší oprávnění možné pro získání hello horizontálního oddílu mapy
Pokud aplikace není manipulace s samotná mapa hello horizontálního oddílu, hello přihlašovací údaje použité metoda factory hello musí mít oprávnění stačí jen pro čtení na hello **globální horizontálního oddílu mapy** databáze. Tyto přihlašovací údaje se obvykle liší od přihlašovací údaje používané tooopen připojení toohello horizontálního oddílu mapa správce. Viz také [použita pověření tooaccess hello elastické databáze klientské knihovny](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-hello-openconnectionforkey-method"></a>Volání metody OpenConnectionForKey hello
Hello  **[ShardMap.OpenConnectionForKey metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  vrátí připojení k ADO.Net připravené k vydávání příkazů toohello příslušné databáze na základě hodnoty hello hello **klíč**parametr. Informace ID horizontálního oddílu jsou uloženy v mezipaměti v aplikaci hello hello **ShardMapManager**, takže tyto požadavky nejsou obvykle zahrnují vyhledávání v databázi proti hello **globální horizontálního oddílu mapy** databáze. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* Hello **klíč** parametr se používá jako klíč vyhledávání do hello horizontálního oddílu mapy toodetermine hello příslušné databáze pro požadavek hello. 
* Hello **connectionString** je použité toopass pouze hello uživatelská pověření pro připojení hello potřeby. Žádný název databáze nebo název serveru jsou zahrnuté v tomto *connectionString* vzhledem k tomu, že metoda hello určí hello databáze a serveru pomocí hello **ShardMap**. 
* Hello  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  by mělo být nastavené příliš**ConnectionOptions.Validate** Pokud prostředí, kde horizontálního oddílu mapuje může změnit a řádky může přesunutí databází tooother jako výsledek operace rozdělení nebo sloučení. To zahrnuje mapu místní horizontálních toohello stručný dotaz na hello cílová databáze (ne toohello globální horizontálních map) před hello připojení doručuje toohello aplikace. 

Pokud hello ověření vůči hello místní horizontálních mapování nezdaří, (což znamená, že mezipaměť hello je nesprávná), bude hello správce mapy horizontálního oddílu dotaz hello globální horizontálních mapy tooobtain hello nové správnou hodnotu pro vyhledávání hello aktualizace mezipaměti hello a získat a vrátí hello připojení k příslušné databázi. 

Použití **ConnectionOptions.None** pouze když změny mapování horizontálních nejsou očekáván aplikace je online. V takovém případě hello mezipaměti hodnoty dá se předpokládat tooalways být správná, a hello velmi odezvy ověření volání toohello cílová databáze mohou být bezpečně přeskočeny. Který snižuje zatížení databáze. Hello **connectionOptions** také lze nastavit prostřednictvím hodnotou v souboru tooindicate konfigurace horizontálního dělení změny se očekává, nebo není během v časovém intervalu.  

Tento příklad používá hello hodnotu celé číslo klíče **CustomerID**pomocí **ShardMap** objekt s názvem **customerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect toohello shard for that customer ID. No need toocall a SqlConnection 
    // constructor followed by hello Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

Hello **OpenConnectionForKey** metoda vrátí novou databázi správné toohello již otevřené připojení. Připojení v tímto způsobem nadále využívat všech výhod ADO.Net sdružování připojení. Tak dlouho, dokud transakce a požadavky lze splnit jednu horizontálního oddílu současně, to by měl být hello pouze změny potřebné v aplikaci už používá ADO.Net. 

Hello  **[OpenConnectionForKeyAsync metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  je také dostupná v případě, že vaše aplikace umožňuje použití asynchronní programování s ADO.Net. Její chování je závislé směrování ekvivalentní ADO hello data. NET na  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metoda.

## <a name="integrating-with-transient-fault-handling"></a>Integrace s přechodná chyba zpracování
Osvědčeným postupem při vývoji aplikací přístup dat v cloudu hello je tooensure že přechodných zachytila hello aplikaci a že hello operace jsou několikrát opakována před vyvolání k chybě. Přechodná chyba zpracování pro cloudové aplikace je popsána v [přechodných chyb](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Přechodná chyba zpracování mohou existovat vedle sebe přirozeně pomocí vzoru Data závislé směrování hello. Hello klíčovým požadavkem je tooretry hello celého datového žádost o přístup včetně hello **pomocí** blok, který získat hello závislé na data směrování připojení. výše uvedený příklad Hello by mohla být přepsána následujícím způsobem (Všimněte si zvýrazněné změny). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Příklad: závislé směrování s přechodná chyba zpracování dat
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect toohello shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Balíčky, které jsou nezbytné tooimplement přechodná chyba zpracování nestahuje automaticky při vytváření hello elastické databáze ukázkovou aplikaci. Balíčky jsou k dispozici samostatně na [Enterprise Library - přechodné chyby zpracování bloku aplikace](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). Použijte verzi 6.0 nebo novější. 

## <a name="transactional-consistency"></a>Transakční konzistence
Transakční vlastnosti jsou pro všechny operace místní tooa horizontálních zaručená. Například transakce odeslat prostřednictvím závislé na data směrování spustit hello rozsahu horizontálního oddílu hello cíl pro hello připojení. V současné době neexistují žádné možnosti zadaná pro zapsání několik připojení do transakce a proto nejsou žádné záruky transakcí pro operace prováděné napříč horizontálních oddílů.

## <a name="next-steps"></a>Další kroky
toodetach horizontálního oddílu nebo tooreattach horizontálního oddílu, najdete v tématu [pomocí hello RecoveryManager třída toofix horizontálního oddílu mapy problémy](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

