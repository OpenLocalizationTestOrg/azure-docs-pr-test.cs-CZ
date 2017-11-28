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
# <a name="upgrade-an-app-toouse-hello-latest-elastic-database-client-library"></a><span data-ttu-id="c2662-103">Upgrade aplikaci toouse hello nejnovější elastické databáze klientské knihovny</span><span class="sxs-lookup"><span data-stu-id="c2662-103">Upgrade an app toouse hello latest elastic database client library</span></span>
<span data-ttu-id="c2662-104">Nové verze hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) jsou k dispozici prostřednictvím rozhraní správce NuGetPackage hello NuGetand v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2662-104">New versions of hello [Elastic Database client library](sql-database-elastic-database-client-library.md) are  available through NuGetand hello NuGetPackage Manager interface in Visual Studio.</span></span> <span data-ttu-id="c2662-105">Upgrady obsahovat oprav chyb a podporu pro nové funkce hello klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="c2662-105">Upgrades contain bug fixes and support for new capabilities of hello client library.</span></span>

<span data-ttu-id="c2662-106">**Nejnovější verzi hello:** přejděte příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="c2662-106">**For hello latest version:** Go too[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span>

<span data-ttu-id="c2662-107">Znovu sestavte aplikaci s novou knihovnu hello a také změnit vaše stávající správce mapy horizontálního oddílu metadata uložená v vaší databáze SQL Azure toosupport nové funkce.</span><span class="sxs-lookup"><span data-stu-id="c2662-107">Rebuild your application with hello new library, as well as change your existing Shard Map Manager metadata stored in your Azure SQL Databases toosupport new features.</span></span>

<span data-ttu-id="c2662-108">Provedením těchto kroků v pořadí zajistí, že staré verze klientské knihovny hello již neexistují ve vašem prostředí při aktualizaci metadat objektů, což znamená, že objekty starý verze metadat nevytvoří po upgradu.</span><span class="sxs-lookup"><span data-stu-id="c2662-108">Performing these steps in order ensures that old versions of hello client library are no longer present in your environment when metadata objects are updated, which means that old-version metadata objects won’t be created after upgrade.</span></span>   

## <a name="upgrade-steps"></a><span data-ttu-id="c2662-109">Postup upgradu</span><span class="sxs-lookup"><span data-stu-id="c2662-109">Upgrade steps</span></span>
<span data-ttu-id="c2662-110">**1. Upgrade aplikace.**</span><span class="sxs-lookup"><span data-stu-id="c2662-110">**1. Upgrade your applications.**</span></span> <span data-ttu-id="c2662-111">V sadě Visual Studio, stahování a referenční dokumentace hello nejnovější knihovny verze klienta do všech vašich projektů vývoj, které používají hello knihovně; poté znovu vytvořit a nasadit.</span><span class="sxs-lookup"><span data-stu-id="c2662-111">In Visual Studio, download and reference hello latest client library version into all of your development projects that use hello library; then rebuild and deploy.</span></span> 

* <span data-ttu-id="c2662-112">V řešení sady Visual Studio vyberte **nástroje** --> **Správce balíčků NuGet** -->  **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="c2662-112">In your Visual Studio solution, select **Tools** --> **NuGet Package Manager** -->  **Manage NuGet Packages for Solution**.</span></span> 
* <span data-ttu-id="c2662-113">(Visual Studio 2013) V levém panelu hello, vyberte **aktualizace**a potom vyberte hello **aktualizace** tlačítko na hello balíčku **Azure SQL Database elastické škálování klientské knihovny** které se zobrazí v hello okno.</span><span class="sxs-lookup"><span data-stu-id="c2662-113">(Visual Studio 2013) In hello left panel, select **Updates**, and then select hello **Update** button on hello package **Azure SQL Database Elastic Scale Client Library** that appears in hello window.</span></span>
* <span data-ttu-id="c2662-114">(Visual Studio 2015) Nastavte pole filtru hello příliš**Upgrade k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="c2662-114">(Visual Studio 2015) Set hello Filter box too**Upgrade available**.</span></span> <span data-ttu-id="c2662-115">Vyberte balíček tooupdate hello a klikněte na hello **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c2662-115">Select hello package tooupdate, and click hello **Update** button.</span></span>
* <span data-ttu-id="c2662-116">(Visual Studio 2017) V horní části hello hello dialogového okna, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="c2662-116">(Visual Studio 2017) At hello top of hello dialog, select **Updates**.</span></span> <span data-ttu-id="c2662-117">Vyberte balíček tooupdate hello a klikněte na hello **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c2662-117">Select hello package tooupdate, and click hello **Update** button.</span></span>
* <span data-ttu-id="c2662-118">Sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="c2662-118">Build and Deploy.</span></span> 

<span data-ttu-id="c2662-119">**2. Upgrade skripty.**</span><span class="sxs-lookup"><span data-stu-id="c2662-119">**2. Upgrade your scripts.**</span></span> <span data-ttu-id="c2662-120">Pokud používáte **prostředí PowerShell** skriptů toomanage horizontálních oddílů, [stáhněte si novou verzi knihovny hello](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) a zkopírujte jej do adresáře hello, ze kterého můžete spustit skripty.</span><span class="sxs-lookup"><span data-stu-id="c2662-120">If you are using **PowerShell** scripts toomanage shards, [download hello new library version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) and copy it into hello directory from which you execute scripts.</span></span> 

<span data-ttu-id="c2662-121">**3. Upgrade služby rozdělení sloučení.**</span><span class="sxs-lookup"><span data-stu-id="c2662-121">**3. Upgrade your split-merge service.**</span></span> <span data-ttu-id="c2662-122">Pokud používáte hello elastické databáze nástroji pro sloučení rozdělení tooreorganize horizontálně dělená data, [stáhnout a nasadit hello nejnovější verzi nástroje hello](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span><span class="sxs-lookup"><span data-stu-id="c2662-122">If you use hello elastic database split-merge tool tooreorganize sharded data, [download and deploy hello latest version of hello tool](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span></span> <span data-ttu-id="c2662-123">Podrobné kroky upgradu pro hello se dá služba najít [zde](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="c2662-123">Detailed upgrade steps for hello Service can be found [here](sql-database-elastic-scale-overview-split-and-merge.md).</span></span> 

<span data-ttu-id="c2662-124">**4. Upgrade vašich databází správce mapy horizontálního oddílu**.</span><span class="sxs-lookup"><span data-stu-id="c2662-124">**4. Upgrade your Shard Map Manager databases**.</span></span> <span data-ttu-id="c2662-125">Upgrade metadat hello podpora vaší mapy horizontálního oddílu v Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c2662-125">Upgrade hello metadata supporting your Shard Maps in Azure SQL Database.</span></span>  <span data-ttu-id="c2662-126">Existují dva způsoby, můžete to provést, pomocí prostředí PowerShell nebo C#.</span><span class="sxs-lookup"><span data-stu-id="c2662-126">There are two ways you can accomplish this, using PowerShell or C#.</span></span> <span data-ttu-id="c2662-127">Níže jsou uvedeny obě možnosti.</span><span class="sxs-lookup"><span data-stu-id="c2662-127">Both options are shown below.</span></span>

<span data-ttu-id="c2662-128">***Možnost 1: Upgrade metadat pomocí prostředí PowerShell***</span><span class="sxs-lookup"><span data-stu-id="c2662-128">***Option 1: Upgrade metadata using PowerShell***</span></span>

1. <span data-ttu-id="c2662-129">Stažení hello nejnovější nástroj příkazového řádku pro NuGet z [zde](http://nuget.org/nuget.exe) a tooa složku pro uložení.</span><span class="sxs-lookup"><span data-stu-id="c2662-129">Download hello latest command-line utility for NuGet from [here](http://nuget.org/nuget.exe) and save tooa folder.</span></span> 
2. <span data-ttu-id="c2662-130">Otevřete příkazový řádek, přejděte toohello stejné složce a problém hello příkaz:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span><span class="sxs-lookup"><span data-stu-id="c2662-130">Open a Command Prompt, navigate toohello same folder, and issue hello command: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span></span>
3. <span data-ttu-id="c2662-131">Přejděte toohello podsložky obsahující hello nová knihovna DLL verze klienta, který jste právě stáhli, například:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span><span class="sxs-lookup"><span data-stu-id="c2662-131">Navigate toohello subfolder containing hello new client DLL version you have just downloaded, for example: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span></span>
4. <span data-ttu-id="c2662-132">Stáhnout hello elastické databáze klienta upgradu skriptlet z hello [centra skriptů](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), a uložte ho do hello stejnou složku obsahující hello knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="c2662-132">Download hello elastic database client upgrade scriptlet from hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), and save it into hello same folder containing hello DLL.</span></span>
5. <span data-ttu-id="c2662-133">Z této složky spusťte z příkazového řádku hello "PowerShell.\upgrade.ps1" a budete postupovat podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="c2662-133">From that folder, run “PowerShell .\upgrade.ps1” from hello command prompt and follow hello prompts.</span></span>

<span data-ttu-id="c2662-134">***Možnost 2: Upgrade metadat pomocí jazyka C#***</span><span class="sxs-lookup"><span data-stu-id="c2662-134">***Option 2: Upgrade metadata using C#***</span></span>

<span data-ttu-id="c2662-135">Můžete taky vytvořit aplikaci Visual Studio, která otevře vaše ShardMapManager iteruje nad všechny horizontálních oddílů a provede upgrade metadat hello voláním metody hello [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) a [ UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c2662-135">Alternatively, create a Visual Studio application that opens your ShardMapManager, iterates over all shards, and performs hello metadata upgrade by calling hello methods [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) and [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) as in this example:</span></span> 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

<span data-ttu-id="c2662-136">Tyto postupy pro metadata upgrady je možné použít více než jednou. bez škodu.</span><span class="sxs-lookup"><span data-stu-id="c2662-136">These techniques for metadata upgrades can be applied multiple times without harm.</span></span> <span data-ttu-id="c2662-137">Například pokud starší verze klienta nechtěně vytvoří horizontálního oddílu po již aktualizaci, můžete spustit upgrade znovu napříč všechny tooensure horizontálních oddílů této hello nejnovější verze metadat se nachází v rámci vaší infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="c2662-137">For example, if an older client version inadvertently creates a shard after you have already updated, you can run upgrade again across all shards tooensure that hello latest metadata version is present throughout your infrastructure.</span></span> 

<span data-ttu-id="c2662-138">**Poznámka:** nové verze klientské knihovny hello publikovaná-datum pokračovat toowork s předchozím verzím hello metadata správce mapy horizontálního oddílu na Azure SQL DB a naopak.</span><span class="sxs-lookup"><span data-stu-id="c2662-138">**Note:**  New versions of hello client library published to-date continue toowork with prior versions of hello Shard Map Manager metadata on Azure SQL DB, and vice-versa.</span></span>   <span data-ttu-id="c2662-139">Ale tootake využít některé z nových funkcí hello v nejnovější verzi klienta hello metadata musí toobe upgradovat.</span><span class="sxs-lookup"><span data-stu-id="c2662-139">However tootake advantage of some of hello new features in hello latest client, metadata needs toobe upgraded.</span></span>   <span data-ttu-id="c2662-140">Všimněte si, že upgrady metadata nebude mít vliv na všechny uživatele – data nebo data specifické pro aplikaci, pouze u objektů vytvořených a používaných hello správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c2662-140">Note that metadata upgrades will not affect any user-data or application-specific data, only objects created and used by hello Shard Map Manager.</span></span>  <span data-ttu-id="c2662-141">A aplikace pokračovat toooperate prostřednictvím pořadí upgradu hello popsané výše.</span><span class="sxs-lookup"><span data-stu-id="c2662-141">And applications continue toooperate through hello upgrade sequence described above.</span></span> 

## <a name="elastic-database-client-version-history"></a><span data-ttu-id="c2662-142">Historie verzí klienta elastické databáze</span><span class="sxs-lookup"><span data-stu-id="c2662-142">Elastic database client version history</span></span>
<span data-ttu-id="c2662-143">Historie verzí najdete příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span><span class="sxs-lookup"><span data-stu-id="c2662-143">For version history, go too[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

