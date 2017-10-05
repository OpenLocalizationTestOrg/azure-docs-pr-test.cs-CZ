---
title: "Omezení v Azure databázi PostgreSQL | Microsoft Docs"
description: "Popisuje omezení v Azure databázi PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.openlocfilehash: 38988fc5c0dc05331ea078534cd1a05e9eca2493
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a><span data-ttu-id="ec7fd-103">Omezení v Azure databázi PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="ec7fd-103">Limitations in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="ec7fd-104">Databáze Azure pro PostgreSQL služby je ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-104">The Azure Database for PostgreSQL service is in public preview.</span></span> <span data-ttu-id="ec7fd-105">Následující části popisují kapacitu a funkční omezení ve službě databáze.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-105">The following sections describe capacity and functional limits in the database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="ec7fd-106">Maximální hodnoty úroveň služby</span><span class="sxs-lookup"><span data-stu-id="ec7fd-106">Service Tier Maximums</span></span>
<span data-ttu-id="ec7fd-107">Azure databáze PostgreSQL má více úrovní služeb, které lze vybírat při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-107">Azure Database for PostgreSQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="ec7fd-108">Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="ec7fd-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="ec7fd-109">Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb ve verzi Preview služby následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ec7fd-109">There is a maximum number of connections, compute units, and storage in each service tier during the service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="ec7fd-110">**Maximální počet připojení**</span><span class="sxs-lookup"><span data-stu-id="ec7fd-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="ec7fd-111">Základní 50 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="ec7fd-112">50 připojení</span><span class="sxs-lookup"><span data-stu-id="ec7fd-112">50 connections</span></span>    |
| <span data-ttu-id="ec7fd-113">Základní 100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="ec7fd-114">připojení 100</span><span class="sxs-lookup"><span data-stu-id="ec7fd-114">100 connections</span></span>   |
| <span data-ttu-id="ec7fd-115">Standardní 100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="ec7fd-116">200 připojení</span><span class="sxs-lookup"><span data-stu-id="ec7fd-116">200 connections</span></span>   |
| <span data-ttu-id="ec7fd-117">Standardní 200 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="ec7fd-118">300 připojení</span><span class="sxs-lookup"><span data-stu-id="ec7fd-118">300 connections</span></span>   |
| <span data-ttu-id="ec7fd-119">Standardní 400 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="ec7fd-120">400 připojení</span><span class="sxs-lookup"><span data-stu-id="ec7fd-120">400 connections</span></span>   |
| <span data-ttu-id="ec7fd-121">Standardní 800 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="ec7fd-122">připojení 500</span><span class="sxs-lookup"><span data-stu-id="ec7fd-122">500 connections</span></span>   |
| <span data-ttu-id="ec7fd-123">**Maximální počet výpočetní jednotky**</span><span class="sxs-lookup"><span data-stu-id="ec7fd-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="ec7fd-124">Úroveň služeb Basic</span><span class="sxs-lookup"><span data-stu-id="ec7fd-124">Basic service tier</span></span>         | <span data-ttu-id="ec7fd-125">100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-125">100 Compute Units</span></span> |
| <span data-ttu-id="ec7fd-126">Úroveň služeb Standard</span><span class="sxs-lookup"><span data-stu-id="ec7fd-126">Standard service tier</span></span>      | <span data-ttu-id="ec7fd-127">800 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-127">800 Compute Units</span></span> |
| <span data-ttu-id="ec7fd-128">**Maximální počet úložiště**</span><span class="sxs-lookup"><span data-stu-id="ec7fd-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="ec7fd-129">Úroveň služeb Basic</span><span class="sxs-lookup"><span data-stu-id="ec7fd-129">Basic service tier</span></span>         | <span data-ttu-id="ec7fd-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="ec7fd-130">1 TB</span></span>              |
| <span data-ttu-id="ec7fd-131">Úroveň služeb Standard</span><span class="sxs-lookup"><span data-stu-id="ec7fd-131">Standard service tier</span></span>      | <span data-ttu-id="ec7fd-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="ec7fd-132">1 TB</span></span>              |

<span data-ttu-id="ec7fd-133">Když se dosáhne příliš mnoha připojení, může se zobrazit chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="ec7fd-133">When too many connections are reached, you may receive the following error:</span></span>
> <span data-ttu-id="ec7fd-134">Závažná chyba: bohužel již příliš mnoho klientů</span><span class="sxs-lookup"><span data-stu-id="ec7fd-134">FATAL:  sorry, too many clients already</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="ec7fd-135">Funkční omezení verze Preview</span><span class="sxs-lookup"><span data-stu-id="ec7fd-135">Preview functional limitations</span></span>
### <a name="scale-operations"></a><span data-ttu-id="ec7fd-136">Operace škálování</span><span class="sxs-lookup"><span data-stu-id="ec7fd-136">Scale operations</span></span>
1.  <span data-ttu-id="ec7fd-137">Dynamické škálování serverů mezi úrovně služeb není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="ec7fd-138">To znamená přepínání mezi úrovně služeb Basic a Standard.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="ec7fd-139">Dynamické zvětšování na vyžádání úložiště na předem vytvořené serveru není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="ec7fd-140">Zmenšuje velikost úložiště serveru není podporována.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="ec7fd-141">Upgrady verze serveru</span><span class="sxs-lookup"><span data-stu-id="ec7fd-141">Server version upgrades</span></span>
- <span data-ttu-id="ec7fd-142">Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="ec7fd-143">Správa předplatného</span><span class="sxs-lookup"><span data-stu-id="ec7fd-143">Subscription management</span></span>
- <span data-ttu-id="ec7fd-144">Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="ec7fd-145">Obnovení do bodu v čase</span><span class="sxs-lookup"><span data-stu-id="ec7fd-145">Point-in-time-restore</span></span>
1.  <span data-ttu-id="ec7fd-146">Obnovení na jinou službu vrstvy nebo výpočetní jednotky a velikost úložiště není povoleno.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-146">Restoring to different service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="ec7fd-147">Obnovení vynechaných server není podporováno.</span><span class="sxs-lookup"><span data-stu-id="ec7fd-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec7fd-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec7fd-148">Next steps</span></span>
- <span data-ttu-id="ec7fd-149">Pochopení [co je k dispozici v jednotlivých cenových úrovní](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="ec7fd-149">Understand [What’s available in each pricing tier](concepts-service-tiers.md)</span></span>
- <span data-ttu-id="ec7fd-150">Pochopení [podporované verze databáze PostgreSQL](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="ec7fd-150">Understand [Supported PostgreSQL Database Versions](concepts-supported-versions.md)</span></span>
- <span data-ttu-id="ec7fd-151">Zkontrolujte [postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí portálu Azure](howto-restore-server-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ec7fd-151">Review [How To Back up and Restore a server in Azure Database for PostgreSQL using the Azure portal](howto-restore-server-portal.md)</span></span>
