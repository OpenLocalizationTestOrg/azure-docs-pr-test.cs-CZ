---
title: "aaaUpgrade toohello nejnovější elastické databáze klientské knihovny | Microsoft Docs"
description: "Upgrade aplikace a knihoven pomocí Nuget"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: cc2c9179be4c53ca59cd24d832127cf277c6e695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-app-toouse-hello-latest-elastic-database-client-library"></a>Upgrade aplikaci toouse hello nejnovější elastické databáze klientské knihovny
Nové verze hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) jsou k dispozici prostřednictvím rozhraní správce NuGetPackage hello NuGetand v sadě Visual Studio. Upgrady obsahovat oprav chyb a podporu pro nové funkce hello klientské knihovny.

**Nejnovější verzi hello:** přejděte příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Znovu sestavte aplikaci s novou knihovnu hello a také změnit vaše stávající správce mapy horizontálního oddílu metadata uložená v vaší databáze SQL Azure toosupport nové funkce.

Provedením těchto kroků v pořadí zajistí, že staré verze klientské knihovny hello již neexistují ve vašem prostředí při aktualizaci metadat objektů, což znamená, že objekty starý verze metadat nevytvoří po upgradu.   

## <a name="upgrade-steps"></a>Postup upgradu
**1. Upgrade aplikace.** V sadě Visual Studio, stahování a referenční dokumentace hello nejnovější knihovny verze klienta do všech vašich projektů vývoj, které používají hello knihovně; poté znovu vytvořit a nasadit. 

* V řešení sady Visual Studio vyberte **nástroje** --> **Správce balíčků NuGet** -->  **spravovat balíčky NuGet pro řešení**. 
* (Visual Studio 2013) V levém panelu hello, vyberte **aktualizace**a potom vyberte hello **aktualizace** tlačítko na hello balíčku **Azure SQL Database elastické škálování klientské knihovny** které se zobrazí v hello okno.
* (Visual Studio 2015) Nastavte pole filtru hello příliš**Upgrade k dispozici**. Vyberte balíček tooupdate hello a klikněte na hello **aktualizace** tlačítko.
* (Visual Studio 2017) V horní části hello hello dialogového okna, vyberte **aktualizace**. Vyberte balíček tooupdate hello a klikněte na hello **aktualizace** tlačítko.
* Sestavení a nasazení. 

**2. Upgrade skripty.** Pokud používáte **prostředí PowerShell** skriptů toomanage horizontálních oddílů, [stáhněte si novou verzi knihovny hello](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) a zkopírujte jej do adresáře hello, ze kterého můžete spustit skripty. 

**3. Upgrade služby rozdělení sloučení.** Pokud používáte hello elastické databáze nástroji pro sloučení rozdělení tooreorganize horizontálně dělená data, [stáhnout a nasadit hello nejnovější verzi nástroje hello](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Podrobné kroky upgradu pro hello se dá služba najít [zde](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Upgrade vašich databází správce mapy horizontálního oddílu**. Upgrade metadat hello podpora vaší mapy horizontálního oddílu v Azure SQL Database.  Existují dva způsoby, můžete to provést, pomocí prostředí PowerShell nebo C#. Níže jsou uvedeny obě možnosti.

***Možnost 1: Upgrade metadat pomocí prostředí PowerShell***

1. Stažení hello nejnovější nástroj příkazového řádku pro NuGet z [zde](http://nuget.org/nuget.exe) a tooa složku pro uložení. 
2. Otevřete příkazový řádek, přejděte toohello stejné složce a problém hello příkaz:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Přejděte toohello podsložky obsahující hello nová knihovna DLL verze klienta, který jste právě stáhli, například:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Stáhnout hello elastické databáze klienta upgradu skriptlet z hello [centra skriptů](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), a uložte ho do hello stejnou složku obsahující hello knihovny DLL.
5. Z této složky spusťte z příkazového řádku hello "PowerShell.\upgrade.ps1" a budete postupovat podle pokynů hello.

***Možnost 2: Upgrade metadat pomocí jazyka C#***

Můžete taky vytvořit aplikaci Visual Studio, která otevře vaše ShardMapManager iteruje nad všechny horizontálních oddílů a provede upgrade metadat hello voláním metody hello [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) a [ UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) jako v následujícím příkladu: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Tyto postupy pro metadata upgrady je možné použít více než jednou. bez škodu. Například pokud starší verze klienta nechtěně vytvoří horizontálního oddílu po již aktualizaci, můžete spustit upgrade znovu napříč všechny tooensure horizontálních oddílů této hello nejnovější verze metadat se nachází v rámci vaší infrastruktury. 

**Poznámka:** nové verze klientské knihovny hello publikovaná-datum pokračovat toowork s předchozím verzím hello metadata správce mapy horizontálního oddílu na Azure SQL DB a naopak.   Ale tootake využít některé z nových funkcí hello v nejnovější verzi klienta hello metadata musí toobe upgradovat.   Všimněte si, že upgrady metadata nebude mít vliv na všechny uživatele – data nebo data specifické pro aplikaci, pouze u objektů vytvořených a používaných hello správce mapy horizontálního oddílu.  A aplikace pokračovat toooperate prostřednictvím pořadí upgradu hello popsané výše. 

## <a name="elastic-database-client-version-history"></a>Historie verzí klienta elastické databáze
Historie verzí najdete příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

