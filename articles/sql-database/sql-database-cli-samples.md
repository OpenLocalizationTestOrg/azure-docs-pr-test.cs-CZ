---
title: "Azure CLI příklady skriptu pro databázi SQL. | Microsoft Docs"
description: "Příklady skriptů Azure CLI k vytváření a správě serverů Azure SQL Database, elastické fondy, databáze a brány firewall."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 91b02d1099dc1683abb1042b3dc65cbee5aeae5b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-azure-sql-database"></a><span data-ttu-id="720b1-103">Ukázek Azure CLI pro databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="720b1-103">Azure CLI samples for Azure SQL Database</span></span>

<span data-ttu-id="720b1-104">Následující tabulka obsahuje odkazy na příklady skript příkazového řádku Azure CLI pro databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="720b1-104">The following table includes links to Azure CLI script examples for Azure SQL Database.</span></span>

| |  |
|---|---|
|<span data-ttu-id="720b1-105">**Vytvoření jedné databáze a fondu elastické databáze**</span><span class="sxs-lookup"><span data-stu-id="720b1-105">**Create a single database and an elastic pool**</span></span>||
| [<span data-ttu-id="720b1-106">Vytvořte jednu databázi a nakonfigurujte pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="720b1-106">Create a single database and configure a firewall rule</span></span>](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="720b1-107">Tento příklad skriptu rozhraní příkazového řádku vytvoří jednu databázi Azure SQL a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="720b1-107">This CLI script example creates a single Azure SQL database and configures a server-level firewall rule.</span></span> |
| [<span data-ttu-id="720b1-108">Vytvoření elastických fondů a přesunutí databází ve fondu</span><span class="sxs-lookup"><span data-stu-id="720b1-108">Create elastic pools and move pooled databases</span></span>](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="720b1-109">Tento příklad skriptu rozhraní příkazového řádku vytvoří SQL elastické fondy, přesune ve fondu databází Azure SQL a změny úrovně výkonu.</span><span class="sxs-lookup"><span data-stu-id="720b1-109">This CLI script example creates SQL elastic pools, and moves pooled Azure SQL databases, and changes performance levels.</span></span>|
|<span data-ttu-id="720b1-110">**Škálování jedné databáze a fondu elastické databáze**</span><span class="sxs-lookup"><span data-stu-id="720b1-110">**Scale a single database and an elastic pool**</span></span>||
| [<span data-ttu-id="720b1-111">Škálování jedné databáze</span><span class="sxs-lookup"><span data-stu-id="720b1-111">Scale a single database</span></span>](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="720b1-112">Tento příklad skriptu rozhraní příkazového řádku škáluje jedné databáze Azure SQL na úroveň výkonu různých po dotaz na informace o velikosti databáze.</span><span class="sxs-lookup"><span data-stu-id="720b1-112">This CLI script example scales a single Azure SQL database to a different performance level after querying the size information for the database.</span></span> |
| [<span data-ttu-id="720b1-113">Škálování fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="720b1-113">Scale an elastic pool</span></span>](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="720b1-114">Tento příklad skriptu rozhraní příkazového řádku škáluje elastický fond SQL na úroveň výkonu jiný.</span><span class="sxs-lookup"><span data-stu-id="720b1-114">This CLI script example scales a SQL elastic pool to a different performance level.</span></span>  |
|||
