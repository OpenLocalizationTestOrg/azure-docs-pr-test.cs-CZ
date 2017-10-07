---
title: "aaaUsing správce obnovení toofix horizontálních mapování problémy | Microsoft Docs"
description: "Použít hello RecoveryManager třída toosolve problémy s horizontálního oddílu mapy"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 45520ca3-6903-4b39-88ba-1d41b22da9fe
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: ddove
ms.openlocfilehash: 2218fb15122f1df466e65483480461e366317f2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-recoverymanager-class-toofix-shard-map-problems"></a>Pomocí hello RecoveryManager třída toofix horizontálního oddílu mapy problémy
Hello [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) třída poskytuje ADO.Net aplikace hello možnost tooeasily zjistit a opravit nekonzistence mezi hello globální horizontálních mapy (GSM) a hello místní horizontálních mapy (LSM) v prostředí s horizontálně dělené databáze. 

Hello GSM a LSM sledovat hello mapování jednotlivých databází v horizontálně dělené prostředí. V některých případech zalomení probíhá mezi hello GSM a hello LSM. V takovém případě použijte hello RecoveryManager třída toodetect a opravit hello přerušení.

Hello RecoveryManager třída je součástí hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md). 

![Mapování horizontálních][1]

Definice podmínek, najdete v části [Glosář nástroje elastické databáze](sql-database-elastic-scale-glossary.md). jak hello toounderstand **ShardMapManager** je použité toomanage data v horizontálně dělené řešení, najdete v části [správu mapování horizontálních](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-hello-recovery-manager"></a>Proč používat Správce hello obnovení?
V prostředí s horizontálně dělené databáze není jednoho klienta na databázi a mnoho databází na serveru. V prostředí hello může být také mnoho serverů. Každou databázi je namapována v mapě horizontálního oddílu hello, takže volání může být směrované toohello správný server a databáze. Databáze se sledují podle tooa **horizontálního dělení klíč**, a je mu přiřazená každý horizontálního oddílu **rozsah hodnot klíče**. Například klíč horizontálního dělení může představovat hello zákazníka názvy z "D" příliš "F." Hello mapování horizontálních oddílů všechny (neboli databáze) a jejich mapování rozsahy jsou obsaženy v hello **globální horizontálních mapy (GSM)**. Každá databáze také obsahuje mapu rozsahů hello obsažených v hello horizontálního oddílu, který se označuje jako hello **místní horizontálních mapy (LSM)**. Když se aplikace připojí tooa horizontálního oddílu, hello mapování se uloží do mezipaměti s hello aplikací pro rychlé načítání. Hello LSM je dat použité toovalidate do mezipaměti. 

Hello GSM a LSM se mohou stát synchronizována pro hello následujících důvodů:

1. odstranění Hello horizontálních, jejichž rozsah předpokládá toono delší být používán nebo přejmenování horizontálního oddílu. Odstranění horizontálního oddílu vede **osamocené mapování horizontálních**. Podobně přejmenovat databázi, může způsobit mapování u osamocených horizontálního oddílu. V závislosti na hello záměr hello změny hello horizontálních může být nutné toobe odebrat nebo umístění horizontálního oddílu hello musí toobe aktualizovat. toorecover odstraněné databáze najdete v části [obnovit odstraněnou databázi](sql-database-recovery-using-backups.md).
2. Dojde k události geo-převzetí služeb při selhání. toocontinue, jeden musíte aktualizovat hello název serveru a název databáze správce mapy horizontálního oddílu v hello aplikaci a pak podrobné informace o aktualizaci hello horizontálního oddílu mapování pro všechny horizontálních oddílů v mapě horizontálního oddílu. Pokud dojde k selhání geograficky, by je možné automatizovat tuto logiku obnovení v rámci pracovního postupu hello převzetí služeb při selhání. Automatizace akce obnovení umožňuje hladký možnosti správy databází s povolenou geografickou a zabraňuje ruční lidského akce. toolearn o toorecover možnosti databáze při výpadku datacentra, najdete v části [kontinuity podnikových procesů](sql-database-business-continuity.md) a [zotavení po havárii](sql-database-disaster-recovery.md).
3. Buď horizontálního oddílu, nebo hello ShardMapManager databáze je obnovený tooan předchozí stav bodu. toolearn o bodu obnovení pomocí záloh, najdete v části [obnovení pomocí záloh](sql-database-recovery-using-backups.md).

Další informace o nástroje elastické databáze pro databázi SQL Azure, geografická replikace a obnovení najdete v tématu hello následující: 

* [Přehled: Cloudu obchodní kontinuitu a databáze zotavení po havárii s databází SQL](sql-database-business-continuity.md) 
* [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md)  
* [ShardMap správy](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Načítání RecoveryManager z ShardMapManager
prvním krokem Hello je toocreate RecoveryManager instance. Hello [GetRecoveryManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) vrátí hello správce obnovení pro aktuální hello [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) instance. mapování nekonzistencím v hello horizontálních tooaddress, musí nejdřív načíst hello RecoveryManager pro mapu konkrétní horizontálních hello. 

   ```
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 
   ```

V tomto příkladu je hello RecoveryManager inicializován ze hello ShardMapManager. Hello ShardMapManager obsahující ShardMap také již byla inicializována. 

Vzhledem k tomu, že tento kód aplikace zpracovává samotná mapa hello horizontálního oddílu, hello přihlašovací údaje použité metoda factory hello (v předchozím příkladu hello smmConnectionString) by měla být přihlašovací údaje, které mají oprávnění pro čtení a zápis v databázi GSM hello odkazuje hello připojovací řetězec. Tyto přihlašovací údaje se obvykle liší od připojení tooopen pověření použitá pro směrování závislé na data. Další informace najdete v tématu [pomocí přihlašovacích údajů v klientovi elastické databáze hello](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-hello-shardmap-after-a-shard-is-deleted"></a>Odebrání horizontálního oddílu z hello ShardMap po odstranění horizontálního oddílu
Hello [DetachShard metoda](https://msdn.microsoft.com/library/azure/dn842083.aspx) odpojí hello zadané ID horizontálního oddílu z mapování horizontálních hello a odstraní mapování přidružené hello horizontálního oddílu.  

* Parametr umístění Hello je hello horizontálního oddílu umístění, konkrétně název serveru a název databáze, hello horizontálních odpojuje. 
* Parametr shardMapName Hello je název mapování horizontálních hello. Toto je pouze při více mapami horizontálního oddílu spravuje hello vyžaduje správce mapy stejné ID horizontálního oddílu. Volitelné. 


> [!IMPORTANT]
> Tento postup použijte pouze v případě, že jste si jisti, že hello rozsah pro mapování hello aktualizovat je prázdný. metody Hello výše Neprovádět kontrolu pro rozsah hello přesouvání dat, proto je vhodné tooinclude zkontroluje ve vašem kódu.
>

V tomto příkladu odebere hello horizontálního oddílu mapy horizontálních oddílů. 

   ```
   rm.DetachShard(s.Location, customerMap);
   ``` 

mapování horizontálních Hello odráží hello horizontálního oddílu umístění v hello GSM před hello odstranění hello horizontálního oddílu. Protože hello horizontálních byla odstraněna, předpokládá se, bylo záměrné, hello horizontálního dělení klíče rozsah je již používán. Pokud ne, můžete provést obnovení bodu v době. toorecover horizontálního oddílu hello z starší v daném okamžiku. (V takovém případě zkontrolujte hello následující části toodetect horizontálního oddílu nekonzistence.) toorecover, najdete v části [bodu v době obnovení](sql-database-recovery-using-backups.md).

Vzhledem k tomu, že se předpokládá, že byl odstraněn databáze hello záměrně, hello konečné správu čištění akce je toodelete hello položka toohello horizontálních ve Správci mapy hello horizontálního oddílu. To zabraňuje aplikace hello z nechtěně zápis informace tooa rozsah, který není očekáván.

## <a name="toodetect-mapping-differences"></a>rozdíly toodetect mapování
Hello [DetectMappingDifferences metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) vybere a vrátí jednu z hello horizontálního oddílu mapy (místní nebo globální) jako zdroj hello založená a sloučí mapování na obou horizontálního oddílu mapy (GSM a LSM).

   ```
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* Hello *umístění* určuje hello název serveru a název databáze. 
* Hello *shardMapName* parametr je název mapování horizontálních hello. Používá se pouze požadované Pokud více mapami horizontálního oddílu spravuje hello správce mapy stejné ID horizontálního oddílu. Volitelné. 

## <a name="tooresolve-mapping-differences"></a>rozdíly tooresolve mapování
Hello [ResolveMappingDifferences metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) vybere jeden hello horizontálního oddílu map (místní nebo globální) jako zdroj hello založená a sloučí mapování na obou horizontálního oddílu mapy (GSM a LSM).

   ```
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* Hello *RecoveryToken* parametr zobrazí rozdíly hello hello mapování mezi hello GSM a hello LSM pro konkrétní horizontálního oddílu hello. 
* Hello [MappingDifferenceResolution výčtu](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) je použité tooindicate hello metodu pro překlad hello rozdíl mezi mapování horizontálních hello. 
* **MappingDifferenceResolution.KeepShardMapping** se doporučuje, když hello LSM obsahuje mapování hello přesné, a proto by měl použít hello mapování horizontálních hello. To je obvykle případ hello, pokud dojde převzetí služeb při selhání: horizontálního oddílu hello se nyní nachází na nový server. Vzhledem k tomu, že hello horizontálního oddílu musí být nejprve odebrány z hello GSM (pomocí metody RecoveryManager.DetachShard hello), mapování na hello GSM už existuje. Proto hello LSM musí být použité toore-vytvořit mapování horizontálních hello.

## <a name="attach-a-shard-toohello-shardmap-after-a-shard-is-restored"></a>Připojte horizontálního oddílu toohello ShardMap po obnovení horizontálního oddílu
Hello [AttachShard metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) připojí hello dané horizontálních toohello horizontálního oddílu mapy. Potom zjistí nekonzistencím mapy horizontálního oddílu a aktualizuje hello mapování toomatch hello horizontálních okamžiku hello hello horizontálního oddílu obnovení. Předpokládá se, že hello databáze je také přejmenovat tooreflect hello původní název databáze (dříve, než byla obnovena hello horizontálního oddílu), od bodu v době obnovení hello výchozí tooa novou databázi spolu s časovým razítkem hello. 

   ```
   rm.AttachShard(location, shardMapName)
   ``` 

* Hello *umístění* parametr je hello název serveru a název databáze, připojovaný horizontálních hello. 
* Hello *shardMapName* parametr je název mapování horizontálních hello. Toto je pouze při více mapami horizontálního oddílu spravuje hello vyžaduje správce mapy stejné ID horizontálního oddílu. Volitelné. 

Tento příklad přidá horizontálního oddílu toohello horizontálního oddílu mapu, která byla nedávno obnovena z předchozí stav bodu v. Vzhledem k tomu, že byla obnovena horizontálních hello (konkrétně mapování horizontálních hello v hello LSM hello), je potenciálně nekonzistentní s položkou hello horizontálního oddílu v hello GSM. Mimo tento příklad kódu hello horizontálních byla obnovena a přejmenovat toohello původní název databáze hello. Vzhledem k tomu, že ho byla obnovena, předpokládá se, že je mapování hello v hello LSM hello důvěryhodné mapování. 

   ```
   rm.AttachShard(s.Location, customerMap); 
   var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-hello-shards"></a>Aktualizace umístění horizontálního oddílu po geo-převzetí služeb při selhání (obnovení) horizontálních oddílů hello
Pokud dojde k selhání geograficky, hello sekundární databáze se provádí zápis přístupný a že bude hello nové primární databáze. Hello název hello serveru a potenciálně hello databáze (v závislosti na konfiguraci) se může lišit od původní primární hello. Proto hello položky mapování horizontálních hello v hello GSM a LSM musí být vyřešeny. Podobně pokud hello databáze je obnovený tooa jiný název nebo umístění nebo tooan dřívějšího bodu v čase, to může způsobit nekonzistence v rámci služby maps hello horizontálního oddílu. Hello horizontálního oddílu mapy Manager zpracovává hello distribuční databáze správný toohello otevřená připojení. Distribuce je založena na datech hello v mapě hello horizontálního oddílu a hodnota hello hello horizontálního dělení klíče, který je cílem hello hello aplikace požadavku. Po geo-převzetí služeb při selhání tyto informace musí být aktualizovány hello přesný název, název databáze a serveru mapování horizontálních hello obnovené databáze. 

## <a name="best-practices"></a>Osvědčené postupy
GEO-převzetí služeb při selhání a obnovení jsou operace, které jsou obvykle spravuje správce cloudu aplikace hello záměrně využitím jedna z funkcí kontinuity obchodních databází SQL Azure. Plánování kontinuity obchodních vyžaduje procesy, postupy a tooensure míry, který provoz firmy můžete pokračovat bez přerušení. Hello metody dostupné jako část hello RecoveryManager třída by měla být použita v rámci této pracovní tok tooensure hello GSM a LSM budou kopírovány podle hello obnovení akce. Je pět základní kroky tooproperly zajištění hello GSM a LSM reflektoval přesné informace o hello po události převzetí služeb při selhání. Hello tooexecute kód aplikace, které tyto kroky můžete integrovat do existující nástroje a pracovní postup. 

1. Načtení hello RecoveryManager z hello ShardMapManager. 
2. Odpojte staré horizontálního oddílu hello z mapování horizontálních hello.
3. Připojte hello nové horizontálního oddílu toohello horizontálního oddílu mapování, včetně nové umístění horizontálního oddílu hello.
4. Rozpoznat nekonzistence v mapování mezi hello GSM a LSM hello. 
5. Rozdíly mezi hello GSM a hello LSM, důvěřující hello LSM vyřešte. 

Tento příklad provádí hello následující kroky:

1. Odebere hello horizontálního oddílu mapu, která odráží horizontálního oddílu umístění před převzetím služeb při selhání hello horizontálních oddílů.
2. Připojí horizontálních oddílů toohello horizontálního oddílu mapy odrážející hello nová horizontálního oddílu umístění (hello parametr "Configuration.SecondaryServer" je nový název serveru hello ale hello stejný název databáze).
3. Získá tokeny obnovení hello zjišťuje rozdíly mapování mezi hello GSM a hello LSM pro každý horizontálního oddílu. 
4. Přeloží hello nekonzistence mapováním důvěřující hello z hello LSM každý horizontálního oddílu. 
   
   ```
   var shards = smm.GetShards(); 
   foreach (shard s in shards) 
   { 
     if (s.Location.Server == Configuration.PrimaryServer) 
   
         { 
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database); 
   
          rm.DetachShard(s.Location); 
   
          rm.AttachShard(slNew); 
   
          var gs = rm.DetectMappingDifferences(slNew); 
   
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 
   ```

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png

