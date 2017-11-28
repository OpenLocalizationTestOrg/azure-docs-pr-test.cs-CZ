---
title: "aaaLimitations ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Popisuje omezení verze preview ve službě Azure Database pro databázi MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 9c877c592bf640f62182d8761c9c51363882d706
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a><span data-ttu-id="93d10-103">Omezení v Azure databáze pro databázi MySQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="93d10-103">Limitations in Azure Database for MySQL (Preview)</span></span>
<span data-ttu-id="93d10-104">Hello databáze Azure pro službu MySQL je ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="93d10-104">hello Azure Database for MySQL service is in public preview.</span></span> <span data-ttu-id="93d10-105">Hello následující části popisují kapacitu a funkční omezení v databázi služby hello.</span><span class="sxs-lookup"><span data-stu-id="93d10-105">hello following sections describe capacity and functional limits in hello database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="93d10-106">Maximální hodnoty úroveň služby</span><span class="sxs-lookup"><span data-stu-id="93d10-106">Service Tier Maximums</span></span>
<span data-ttu-id="93d10-107">Azure databáze MySQL, má více úrovní služeb, které lze vybírat při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="93d10-107">Azure Database for MySQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="93d10-108">Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="93d10-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="93d10-109">Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb během verze preview služby hello, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93d10-109">There is a maximum number of connections, compute units, and storage in each service tier during hello service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="93d10-110">**Maximální počet připojení**</span><span class="sxs-lookup"><span data-stu-id="93d10-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="93d10-111">Základní 50 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="93d10-112">50 připojení</span><span class="sxs-lookup"><span data-stu-id="93d10-112">50 connections</span></span>    |
| <span data-ttu-id="93d10-113">Základní 100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="93d10-114">připojení 100</span><span class="sxs-lookup"><span data-stu-id="93d10-114">100 connections</span></span>   |
| <span data-ttu-id="93d10-115">Standardní 100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="93d10-116">200 připojení</span><span class="sxs-lookup"><span data-stu-id="93d10-116">200 connections</span></span>   |
| <span data-ttu-id="93d10-117">Standardní 200 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="93d10-118">300 připojení</span><span class="sxs-lookup"><span data-stu-id="93d10-118">300 connections</span></span>   |
| <span data-ttu-id="93d10-119">Standardní 400 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="93d10-120">400 připojení</span><span class="sxs-lookup"><span data-stu-id="93d10-120">400 connections</span></span>   |
| <span data-ttu-id="93d10-121">Standardní 800 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="93d10-122">připojení 500</span><span class="sxs-lookup"><span data-stu-id="93d10-122">500 connections</span></span>   |
| <span data-ttu-id="93d10-123">**Maximální počet výpočetní jednotky**</span><span class="sxs-lookup"><span data-stu-id="93d10-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="93d10-124">Úroveň služeb Basic</span><span class="sxs-lookup"><span data-stu-id="93d10-124">Basic service tier</span></span>         | <span data-ttu-id="93d10-125">100 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-125">100 Compute Units</span></span> |
| <span data-ttu-id="93d10-126">Úroveň služeb Standard</span><span class="sxs-lookup"><span data-stu-id="93d10-126">Standard service tier</span></span>      | <span data-ttu-id="93d10-127">800 výpočetní jednotky</span><span class="sxs-lookup"><span data-stu-id="93d10-127">800 Compute Units</span></span> |
| <span data-ttu-id="93d10-128">**Maximální počet úložiště**</span><span class="sxs-lookup"><span data-stu-id="93d10-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="93d10-129">Úroveň služeb Basic</span><span class="sxs-lookup"><span data-stu-id="93d10-129">Basic service tier</span></span>         | <span data-ttu-id="93d10-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="93d10-130">1 TB</span></span>              |
| <span data-ttu-id="93d10-131">Úroveň služeb Standard</span><span class="sxs-lookup"><span data-stu-id="93d10-131">Standard service tier</span></span>      | <span data-ttu-id="93d10-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="93d10-132">1 TB</span></span>              |

<span data-ttu-id="93d10-133">Když se dosáhne příliš mnoha připojení, může se zobrazit hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="93d10-133">When too many connections are reached, you may receive hello following error:</span></span>
> <span data-ttu-id="93d10-134">Chyba 1040 (08004): Příliš mnoho připojení</span><span class="sxs-lookup"><span data-stu-id="93d10-134">ERROR 1040 (08004): Too many connections</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="93d10-135">Funkční omezení verze Preview:</span><span class="sxs-lookup"><span data-stu-id="93d10-135">Preview functional limitations:</span></span>
### <a name="scale-operations"></a><span data-ttu-id="93d10-136">Operace škálování:</span><span class="sxs-lookup"><span data-stu-id="93d10-136">Scale operations:</span></span>
1.  <span data-ttu-id="93d10-137">Dynamické škálování serverů mezi úrovně služeb není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="93d10-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="93d10-138">To znamená přepínání mezi úrovně služeb Basic a Standard.</span><span class="sxs-lookup"><span data-stu-id="93d10-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="93d10-139">Dynamické zvětšování na vyžádání úložiště na předem vytvořené serveru není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="93d10-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="93d10-140">Zmenšuje velikost úložiště serveru není podporována.</span><span class="sxs-lookup"><span data-stu-id="93d10-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="93d10-141">Upgrady verze serveru:</span><span class="sxs-lookup"><span data-stu-id="93d10-141">Server version upgrades:</span></span>
- <span data-ttu-id="93d10-142">Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.</span><span class="sxs-lookup"><span data-stu-id="93d10-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="93d10-143">Správa předplatného:</span><span class="sxs-lookup"><span data-stu-id="93d10-143">Subscription management:</span></span>
- <span data-ttu-id="93d10-144">Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="93d10-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="93d10-145">Bod v čas – obnovení:</span><span class="sxs-lookup"><span data-stu-id="93d10-145">Point-in-time-restore:</span></span>
1.  <span data-ttu-id="93d10-146">Obnovení vrstvy služby toodifferent nebo výpočetní jednotky a velikost úložiště není povoleno.</span><span class="sxs-lookup"><span data-stu-id="93d10-146">Restoring toodifferent service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="93d10-147">Obnovení vynechaných server není podporováno.</span><span class="sxs-lookup"><span data-stu-id="93d10-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93d10-148">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="93d10-148">Next Steps:</span></span>
<span data-ttu-id="93d10-149">[Co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md)
[podporované verze databáze MySQL](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="93d10-149">[What’s available in each service tier](concepts-service-tiers.md)
[Supported MySQL Database Versions](concepts-supported-versions.md)</span></span>
