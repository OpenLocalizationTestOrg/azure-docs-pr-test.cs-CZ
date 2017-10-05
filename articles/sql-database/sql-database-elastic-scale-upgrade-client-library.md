---
title: "Upgrade na nejnovější klientské knihovny elastické databáze | Microsoft Docs"
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
ms.openlocfilehash: 7e28d128645e856c0efa6e4f78d8f9f36982a76c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a><span data-ttu-id="774aa-103">Upgrade aplikace používat nejnovější klientské knihovny elastické databáze</span><span class="sxs-lookup"><span data-stu-id="774aa-103">Upgrade an app to use the latest elastic database client library</span></span>
<span data-ttu-id="774aa-104">Nové verze [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) jsou k dispozici prostřednictvím NuGetand rozhraní NuGetPackage správce v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="774aa-104">New versions of the [Elastic Database client library](sql-database-elastic-database-client-library.md) are  available through NuGetand the NuGetPackage Manager interface in Visual Studio.</span></span> <span data-ttu-id="774aa-105">Upgrady obsahovat oprav chyb a podporu pro nové funkce knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="774aa-105">Upgrades contain bug fixes and support for new capabilities of the client library.</span></span>

<span data-ttu-id="774aa-106">**Nejnovější verzi:** přejít na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="774aa-106">**For the latest version:** Go to [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span>

<span data-ttu-id="774aa-107">Znovu sestavte aplikaci s novou knihovnu, a také změnit vaše stávající správce mapy horizontálního oddílu metadata uložená v databázích SQL Azure pro podporu nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="774aa-107">Rebuild your application with the new library, as well as change your existing Shard Map Manager metadata stored in your Azure SQL Databases to support new features.</span></span>

<span data-ttu-id="774aa-108">Provedením těchto kroků v pořadí zajistí staré verze klientské knihovny už existuje ve vašem prostředí při aktualizaci metadat objektů, což znamená, že objekty starý verze metadat nevytvoří po upgradu.</span><span class="sxs-lookup"><span data-stu-id="774aa-108">Performing these steps in order ensures that old versions of the client library are no longer present in your environment when metadata objects are updated, which means that old-version metadata objects won’t be created after upgrade.</span></span>   

## <a name="upgrade-steps"></a><span data-ttu-id="774aa-109">Postup upgradu</span><span class="sxs-lookup"><span data-stu-id="774aa-109">Upgrade steps</span></span>
<span data-ttu-id="774aa-110">**1. Upgrade aplikace.**</span><span class="sxs-lookup"><span data-stu-id="774aa-110">**1. Upgrade your applications.**</span></span> <span data-ttu-id="774aa-111">V sadě Visual Studio stáhněte a odkazovat na nejnovější verzi knihovny klienta do všech vašich projektů vývoj, které používají knihovnu; poté znovu vytvořit a nasadit.</span><span class="sxs-lookup"><span data-stu-id="774aa-111">In Visual Studio, download and reference the latest client library version into all of your development projects that use the library; then rebuild and deploy.</span></span> 

* <span data-ttu-id="774aa-112">V řešení sady Visual Studio vyberte **nástroje** --> **Správce balíčků NuGet** -->  **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="774aa-112">In your Visual Studio solution, select **Tools** --> **NuGet Package Manager** -->  **Manage NuGet Packages for Solution**.</span></span> 
* <span data-ttu-id="774aa-113">(Visual Studio 2013) V levém panelu, vyberte **aktualizace**a pak vyberte **aktualizace** tlačítko na balíček **Azure SQL Database elastické škálování klientské knihovny** které se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="774aa-113">(Visual Studio 2013) In the left panel, select **Updates**, and then select the **Update** button on the package **Azure SQL Database Elastic Scale Client Library** that appears in the window.</span></span>
* <span data-ttu-id="774aa-114">(Visual Studio 2015) Nastavte filtr na **Upgrade k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="774aa-114">(Visual Studio 2015) Set the Filter box to **Upgrade available**.</span></span> <span data-ttu-id="774aa-115">Vyberte balíček, který chcete aktualizovat a klikněte na **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="774aa-115">Select the package to update, and click the **Update** button.</span></span>
* <span data-ttu-id="774aa-116">(Visual Studio 2017) V horní části okna vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="774aa-116">(Visual Studio 2017) At the top of the dialog, select **Updates**.</span></span> <span data-ttu-id="774aa-117">Vyberte balíček, který chcete aktualizovat a klikněte na **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="774aa-117">Select the package to update, and click the **Update** button.</span></span>
* <span data-ttu-id="774aa-118">Sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="774aa-118">Build and Deploy.</span></span> 

<span data-ttu-id="774aa-119">**2. Upgrade skripty.**</span><span class="sxs-lookup"><span data-stu-id="774aa-119">**2. Upgrade your scripts.**</span></span> <span data-ttu-id="774aa-120">Pokud používáte **prostředí PowerShell** skripty pro správu horizontálních oddílů, [stáhněte si novou verzi knihovny](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) a zkopírujte jej do adresáře, ze kterého můžete spustit skripty.</span><span class="sxs-lookup"><span data-stu-id="774aa-120">If you are using **PowerShell** scripts to manage shards, [download the new library version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) and copy it into the directory from which you execute scripts.</span></span> 

<span data-ttu-id="774aa-121">**3. Upgrade služby rozdělení sloučení.**</span><span class="sxs-lookup"><span data-stu-id="774aa-121">**3. Upgrade your split-merge service.**</span></span> <span data-ttu-id="774aa-122">Pokud použijete nástroj rozdělení sloučení elastické databáze reorganizace horizontálně dělená data [stáhnout a nasadit nejnovější verzi nástroje](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span><span class="sxs-lookup"><span data-stu-id="774aa-122">If you use the elastic database split-merge tool to reorganize sharded data, [download and deploy the latest version of the tool](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span></span> <span data-ttu-id="774aa-123">Podrobné kroky upgradu naleznete službu [zde](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="774aa-123">Detailed upgrade steps for the Service can be found [here](sql-database-elastic-scale-overview-split-and-merge.md).</span></span> 

<span data-ttu-id="774aa-124">**4. Upgrade vašich databází správce mapy horizontálního oddílu**.</span><span class="sxs-lookup"><span data-stu-id="774aa-124">**4. Upgrade your Shard Map Manager databases**.</span></span> <span data-ttu-id="774aa-125">Upgrade metadat podpora vaší mapy horizontálního oddílu v Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="774aa-125">Upgrade the metadata supporting your Shard Maps in Azure SQL Database.</span></span>  <span data-ttu-id="774aa-126">Existují dva způsoby, můžete to provést, pomocí prostředí PowerShell nebo C#.</span><span class="sxs-lookup"><span data-stu-id="774aa-126">There are two ways you can accomplish this, using PowerShell or C#.</span></span> <span data-ttu-id="774aa-127">Níže jsou uvedeny obě možnosti.</span><span class="sxs-lookup"><span data-stu-id="774aa-127">Both options are shown below.</span></span>

<span data-ttu-id="774aa-128">***Možnost 1: Upgrade metadat pomocí prostředí PowerShell***</span><span class="sxs-lookup"><span data-stu-id="774aa-128">***Option 1: Upgrade metadata using PowerShell***</span></span>

1. <span data-ttu-id="774aa-129">Stáhněte si nejnovější nástroj příkazového řádku pro NuGet z [zde](http://nuget.org/nuget.exe) a uložte do složky.</span><span class="sxs-lookup"><span data-stu-id="774aa-129">Download the latest command-line utility for NuGet from [here](http://nuget.org/nuget.exe) and save to a folder.</span></span> 
2. <span data-ttu-id="774aa-130">Otevřete příkazový řádek, přejděte do stejné složky a vydejte příkaz:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span><span class="sxs-lookup"><span data-stu-id="774aa-130">Open a Command Prompt, navigate to the same folder, and issue the command: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span></span>
3. <span data-ttu-id="774aa-131">Přejděte do podsložky, která obsahuje nové verze knihoven DLL klienta, který jste právě stáhli, například:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span><span class="sxs-lookup"><span data-stu-id="774aa-131">Navigate to the subfolder containing the new client DLL version you have just downloaded, for example: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span></span>
4. <span data-ttu-id="774aa-132">Stáhnout upgradu skriptlet elastické databáze klienta z [centra skriptů](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)a uložte ho do stejné složky, která obsahuje knihovnu DLL.</span><span class="sxs-lookup"><span data-stu-id="774aa-132">Download the elastic database client upgrade scriptlet from the [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), and save it into the same folder containing the DLL.</span></span>
5. <span data-ttu-id="774aa-133">Z této složky spusťte z příkazového řádku "PowerShell.\upgrade.ps1" a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="774aa-133">From that folder, run “PowerShell .\upgrade.ps1” from the command prompt and follow the prompts.</span></span>

<span data-ttu-id="774aa-134">***Možnost 2: Upgrade metadat pomocí jazyka C#***</span><span class="sxs-lookup"><span data-stu-id="774aa-134">***Option 2: Upgrade metadata using C#***</span></span>

<span data-ttu-id="774aa-135">Můžete taky vytvořit aplikaci Visual Studio, která otevře vaše ShardMapManager iteruje nad všechny horizontálních oddílů a provede upgrade metadat voláním metody [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) a [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="774aa-135">Alternatively, create a Visual Studio application that opens your ShardMapManager, iterates over all shards, and performs the metadata upgrade by calling the methods [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) and [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) as in this example:</span></span> 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

<span data-ttu-id="774aa-136">Tyto postupy pro metadata upgrady je možné použít více než jednou. bez škodu.</span><span class="sxs-lookup"><span data-stu-id="774aa-136">These techniques for metadata upgrades can be applied multiple times without harm.</span></span> <span data-ttu-id="774aa-137">Například pokud starší verze klienta nechtěně vytvoří horizontálního oddílu po již aktualizaci, můžete spustit upgrade znovu napříč všechny horizontálních oddílů zajistit, že je nainstalována v celé infrastruktuře nejnovější verze metadat.</span><span class="sxs-lookup"><span data-stu-id="774aa-137">For example, if an older client version inadvertently creates a shard after you have already updated, you can run upgrade again across all shards to ensure that the latest metadata version is present throughout your infrastructure.</span></span> 

<span data-ttu-id="774aa-138">**Poznámka:** nové verze klientské knihovny publikovaná-datum pokračovat v práci s předchozím verzím metadaty správce mapy horizontálního oddílu na Azure SQL DB a naopak.</span><span class="sxs-lookup"><span data-stu-id="774aa-138">**Note:**  New versions of the client library published to-date continue to work with prior versions of the Shard Map Manager metadata on Azure SQL DB, and vice-versa.</span></span>   <span data-ttu-id="774aa-139">Ale pokud chcete využít výhod některé z nových funkcí v nejnovější verzi klienta, je nutno aktualizovat metadata.</span><span class="sxs-lookup"><span data-stu-id="774aa-139">However to take advantage of some of the new features in the latest client, metadata needs to be upgraded.</span></span>   <span data-ttu-id="774aa-140">Všimněte si, že metadata upgrady nebude mít vliv na všechna uživatelská data nebo data specifické pro aplikaci, pouze objekty vytvořené a pomocí správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="774aa-140">Note that metadata upgrades will not affect any user-data or application-specific data, only objects created and used by the Shard Map Manager.</span></span>  <span data-ttu-id="774aa-141">A aplikace i nadále fungovat prostřednictvím pořadí upgradu popsané výše.</span><span class="sxs-lookup"><span data-stu-id="774aa-141">And applications continue to operate through the upgrade sequence described above.</span></span> 

## <a name="elastic-database-client-version-history"></a><span data-ttu-id="774aa-142">Historie verzí klienta elastické databáze</span><span class="sxs-lookup"><span data-stu-id="774aa-142">Elastic database client version history</span></span>
<span data-ttu-id="774aa-143">Historie verzí najdete na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span><span class="sxs-lookup"><span data-stu-id="774aa-143">For version history, go to [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

