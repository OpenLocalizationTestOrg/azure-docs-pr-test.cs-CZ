---
title: "Omezení v Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Popisuje omezení verze preview ve službě Azure Database pro databázi MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c61d70897b66c2ffee819ac98c38ab75000db907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a><span data-ttu-id="ca474-103">Omezení v Azure databáze pro databázi MySQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="ca474-103">Limitations in Azure Database for MySQL (Preview)</span></span>
<span data-ttu-id="ca474-104">Databáze Azure pro službu MySQL je ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="ca474-104">The Azure Database for MySQL service is in public preview.</span></span> <span data-ttu-id="ca474-105">Následující části popisují kapacitu a funkční omezení ve službě databáze.</span><span class="sxs-lookup"><span data-stu-id="ca474-105">The following sections describe capacity and functional limits in the database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="ca474-106">Maximální hodnoty úroveň služby</span><span class="sxs-lookup"><span data-stu-id="ca474-106">Service Tier Maximums</span></span>
<span data-ttu-id="ca474-107">Azure databáze MySQL, má více úrovní služeb, které lze vybírat při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="ca474-107">Azure Database for MySQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="ca474-108">Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="ca474-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="ca474-109">Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb ve verzi Preview služby následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ca474-109">There is a maximum number of connections, compute units, and storage in each service tier during the service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="ca474-110">**Maximální počet připojení**</span><span class="sxs-lookup"><span data-stu-id="ca474-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="ca474-111">Základní 50 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="ca474-112">50 připojení</span><span class="sxs-lookup"><span data-stu-id="ca474-112">50 connections</span></span>    |
| <span data-ttu-id="ca474-113">Základní 100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="ca474-114">připojení 100</span><span class="sxs-lookup"><span data-stu-id="ca474-114">100 connections</span></span>   |
| <span data-ttu-id="ca474-115">Standardní 100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="ca474-116">200 připojení</span><span class="sxs-lookup"><span data-stu-id="ca474-116">200 connections</span></span>   |
| <span data-ttu-id="ca474-117">Standardní 200 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="ca474-118">300 připojení</span><span class="sxs-lookup"><span data-stu-id="ca474-118">300 connections</span></span>   |
| <span data-ttu-id="ca474-119">Standardní 400 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="ca474-120">400 připojení</span><span class="sxs-lookup"><span data-stu-id="ca474-120">400 connections</span></span>   |
| <span data-ttu-id="ca474-121">Standardní 800 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="ca474-122">připojení 500</span><span class="sxs-lookup"><span data-stu-id="ca474-122">500 connections</span></span>   |
| <span data-ttu-id="ca474-123">**Maximální počet výpočetní jednotky**</span><span class="sxs-lookup"><span data-stu-id="ca474-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="ca474-124">Úroveň služeb Basic</span><span class="sxs-lookup"><span data-stu-id="ca474-124">Basic service tier</span></span>         | <span data-ttu-id="ca474-125">100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-125">100 Compute Units</span></span> |
| <span data-ttu-id="ca474-126">Úroveň služeb Standard</span><span class="sxs-lookup"><span data-stu-id="ca474-126">Standard service tier</span></span>      | <span data-ttu-id="ca474-127">800 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="ca474-127">800 Compute Units</span></span> |
| <span data-ttu-id="ca474-128">**Maximální počet úložiště**</span><span class="sxs-lookup"><span data-stu-id="ca474-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="ca474-129">Úroveň služeb Basic</span><span class="sxs-lookup"><span data-stu-id="ca474-129">Basic service tier</span></span>         | <span data-ttu-id="ca474-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="ca474-130">1 TB</span></span>              |
| <span data-ttu-id="ca474-131">Úroveň služeb Standard</span><span class="sxs-lookup"><span data-stu-id="ca474-131">Standard service tier</span></span>      | <span data-ttu-id="ca474-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="ca474-132">1 TB</span></span>              |

<span data-ttu-id="ca474-133">Když se dosáhne příliš mnoha připojení, může se zobrazit chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="ca474-133">When too many connections are reached, you may receive the following error:</span></span>
> <span data-ttu-id="ca474-134">Chyba 1040 (08004): Příliš mnoho připojení</span><span class="sxs-lookup"><span data-stu-id="ca474-134">ERROR 1040 (08004): Too many connections</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="ca474-135">Funkční omezení verze Preview:</span><span class="sxs-lookup"><span data-stu-id="ca474-135">Preview functional limitations:</span></span>
### <a name="scale-operations"></a><span data-ttu-id="ca474-136">Operace škálování:</span><span class="sxs-lookup"><span data-stu-id="ca474-136">Scale operations:</span></span>
1.  <span data-ttu-id="ca474-137">Dynamické škálování serverů mezi úrovně služeb není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ca474-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="ca474-138">To znamená přepínání mezi úrovně služeb Basic a Standard.</span><span class="sxs-lookup"><span data-stu-id="ca474-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="ca474-139">Dynamické zvětšování na vyžádání úložiště na předem vytvořené serveru není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ca474-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="ca474-140">Zmenšuje velikost úložiště serveru není podporována.</span><span class="sxs-lookup"><span data-stu-id="ca474-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="ca474-141">Upgrady verze serveru:</span><span class="sxs-lookup"><span data-stu-id="ca474-141">Server version upgrades:</span></span>
- <span data-ttu-id="ca474-142">Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.</span><span class="sxs-lookup"><span data-stu-id="ca474-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="ca474-143">Správa předplatného:</span><span class="sxs-lookup"><span data-stu-id="ca474-143">Subscription management:</span></span>
- <span data-ttu-id="ca474-144">Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="ca474-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="ca474-145">Bod v čas – obnovení:</span><span class="sxs-lookup"><span data-stu-id="ca474-145">Point-in-time-restore:</span></span>
1.  <span data-ttu-id="ca474-146">Obnovení na jinou službu vrstvy nebo výpočetní jednotky a velikost úložiště není povoleno.</span><span class="sxs-lookup"><span data-stu-id="ca474-146">Restoring to different service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="ca474-147">Obnovení vynechaných server není podporováno.</span><span class="sxs-lookup"><span data-stu-id="ca474-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca474-148">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="ca474-148">Next Steps:</span></span>
<span data-ttu-id="ca474-149">[Co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md)
[podporované verze databáze MySQL](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="ca474-149">[What’s available in each service tier](concepts-service-tiers.md)
[Supported MySQL Database Versions](concepts-supported-versions.md)</span></span>
