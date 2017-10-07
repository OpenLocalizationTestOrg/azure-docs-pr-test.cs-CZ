---
title: "čítače aaaPerformance pro správce horizontálního oddílu mapy"
description: "ShardMapManager třídy a data závislé směrování čítače výkonu"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: b090aba0-2e30-454c-96b3-dffa281f539a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: ddove
ms.openlocfilehash: d24198563d9fa88d12e6c464dbe89bc300e72ca0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-counters-for-shard-map-manager"></a>Čítače výkonu pro správce mapování horizontálních oddílů
Můžete zaznamenat hello výkon [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md), zvláště při používání [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md). Čítače jsou vytvořeny pomocí metod hello Microsoft.Azure.SqlDatabase.ElasticScale.Client třídy.  

Čítače jsou použité tootrack hello výkon [data závislé směrování](sql-database-elastic-scale-data-dependent-routing.md) operace. Tyto čítače jsou k dispozici v hello sledování výkonu v kategorii "Elastické databáze: ID horizontálního oddílu Správa" hello.

**Nejnovější verzi hello:** přejděte příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Viz také [upgradu aplikaci toouse hello nejnovější elastické databáze klientské knihovny](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Požadavky
* kategorie toocreate hello výkonu a čítače, hello uživatel musí být součástí místní hello **správci** skupina na počítači hello hostování aplikace hello.  
* toocreate výkonu čítače instance a aktualizovat hello čítače, hello uživatele musí být členem buď hello **správci** nebo **Performance Monitor Users** skupiny. 

## <a name="create-performance-category-and-counters"></a>Vytvoření kategorie výkonu a čítače
čítače hello toocreate, volejte metodu CreatePeformanceCategoryAndCounters hello Dobrý den [ShardMapManagmentFactory třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Metoda hello může spustit pouze správce: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Můžete také použít [to](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) metoda hello tooexecute skript prostředí PowerShell. Metoda Hello vytvoří hello následující čítače výkonu:  

* **V mezipaměti mapování**: počet mapování do mezipaměti pro mapu hello horizontálního oddílu.
* **Operace DDR za sekundu**: rychlost závislé směrování operací dat pro mapu hello horizontálního oddílu. Toto počítadlo je aktualizováno při volání příliš[OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) výsledkem horizontálního úspěšné připojení toohello cílového oddílu. 
* **Mapování vyhledávání přístupy do mezipaměti nebo s**: rychlost úspěšných mezipaměti vyhledávání operací pro mapování v mapě horizontálního oddílu hello. 
* **Mapování vyhledávání sekundu Neúspěšné přístupy do mezipaměti**: počet mezipaměti se nezdařila operace vyhledávání pro mapování v mapě hello horizontálního oddílu.
* **Mapování přidán nebo aktualizován v mezipaměti za sekundu**: frekvence, které mapování se přidávají nebo aktualizované v mezipaměti pro mapu hello horizontálního oddílu. 
* **Mapování odebrány z mezipaměti za sekundu**: míry, kdy probíhá odebírání mapování z mezipaměti pro mapu hello horizontálního oddílu. 

Čítače výkonu jsou vytvořeny pro každé mapování v mezipaměti horizontálních podle procesu.  

## <a name="notes"></a>Poznámky
Hello následující události aktivovat hello vytvoření hello čítače výkonu:  

* Inicializace hello [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) s [přes načítání](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), pokud hello ShardMapManager obsahuje všechny mapy horizontálního oddílu. Patří mezi ně hello [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) a hello [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) metody.
* Úspěšné vyhledávání horizontálního oddílu mapy (pomocí [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) nebo [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Úspěšné vytvoření mapy horizontálního oddílu pomocí CreateShardMap().

čítače výkonu Hello budou aktualizovány operacemi mezipaměti u hello horizontálního oddílu mapy a mapování. Úspěšné odstranění mapy hello horizontálního oddílu v odstranění instance čítače výkonu hello reults DeleteShardMap ().  

## <a name="best-practices"></a>Osvědčené postupy
* Vytvoření kategorie hello výkonu a čítače se provádí jenom jednou před vytvořením hello ShardMapManager objektu. Každé provedení příkazu hello CreatePerformanceCategoryAndCounters() vymaže hello předchozí čítače (ztráty dat, které jsou uvedeny všechny instance) a vytvoří nové.  
* Podle procesu vytváření instancí čítače výkonu. Všechny havárie aplikací nebo odebrání horizontálního oddílu mapy z mezipaměti hello způsobí odstranění instancí čítače výkonu hello.  

### <a name="see-also"></a>Viz také
[Přehled funkcí elastické databáze](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

