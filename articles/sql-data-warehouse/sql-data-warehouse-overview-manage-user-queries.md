---
title: "dotazy aaaMonitor uživatele v Azure SQL Data Warehouse | Microsoft Docs"
description: "Přehled aspektů hello, osvědčené postupy a úlohy pro monitorování uživatelských dotazů v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 1d0960db-5dcf-4a08-b1dc-6c5d3d5a616d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 67639e81b04635452e1ed844fe2d7245aa96a4fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-user-queries-in-azure-sql-data-warehouse"></a><span data-ttu-id="186a6-103">Monitorování uživatelských dotazů v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="186a6-103">Monitor user queries in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="186a6-104">Přehled hello aspekty, osvědčené postupy a úlohy pro monitorování uživatelských dotazů v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="186a6-104">Overview of hello considerations, best practices, and tasks for monitoring user queries in SQL Data Warehouse.</span></span>

| <span data-ttu-id="186a6-105">Kategorie</span><span class="sxs-lookup"><span data-stu-id="186a6-105">Category</span></span> | <span data-ttu-id="186a6-106">Úlohu nebo aspekt</span><span class="sxs-lookup"><span data-stu-id="186a6-106">Task or consideration</span></span> | <span data-ttu-id="186a6-107">Popis</span><span class="sxs-lookup"><span data-stu-id="186a6-107">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="186a6-108">Nízký výkon</span><span class="sxs-lookup"><span data-stu-id="186a6-108">Slow performance</span></span> |<span data-ttu-id="186a6-109">Najít dlouho běžící dotaz na uživatele</span><span class="sxs-lookup"><span data-stu-id="186a6-109">Find a long-running user query</span></span> |<span data-ttu-id="186a6-110">[Najít dlouho běžící dotazy][Find long-running queries]</span><span class="sxs-lookup"><span data-stu-id="186a6-110">[Find long-running queries][Find long-running queries]</span></span> |
| <span data-ttu-id="186a6-111">Souběžnost</span><span class="sxs-lookup"><span data-stu-id="186a6-111">Concurrency</span></span> |<span data-ttu-id="186a6-112">Přidělování prostředků s souběžných dotazů toouser</span><span class="sxs-lookup"><span data-stu-id="186a6-112">Assign concurrent resources toouser queries</span></span> |<span data-ttu-id="186a6-113">[Souběžnost a úlohy správy][Concurrency and workload management]</span><span class="sxs-lookup"><span data-stu-id="186a6-113">[Concurrency and workload management][Concurrency and workload management]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="186a6-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="186a6-114">Next steps</span></span>
<span data-ttu-id="186a6-115">Pro další správu tipy zamiřte toohello [přehled správy][Management overview].</span><span class="sxs-lookup"><span data-stu-id="186a6-115">For more management tips, head over toohello [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Find long-running queries]: sql-data-warehouse-manage-monitor.md
[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md
[Management overview]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->


<!--Other Web references-->
