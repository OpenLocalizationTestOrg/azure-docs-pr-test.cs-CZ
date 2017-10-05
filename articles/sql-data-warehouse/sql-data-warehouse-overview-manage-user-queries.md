---
title: "Monitorování uživatelských dotazů v Azure SQL Data Warehouse | Microsoft Docs"
description: "Přehled důležité informace, osvědčené postupy a úlohy pro monitorování uživatelských dotazů v Azure SQL Data Warehouse"
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
ms.openlocfilehash: 65509a65c2b34553822cc02d7a7fa5a614adc57f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-user-queries-in-azure-sql-data-warehouse"></a><span data-ttu-id="06475-103">Monitorování uživatelských dotazů v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="06475-103">Monitor user queries in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="06475-104">Přehled důležité informace, osvědčené postupy a úlohy pro monitorování uživatelských dotazů v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="06475-104">Overview of the considerations, best practices, and tasks for monitoring user queries in SQL Data Warehouse.</span></span>

| <span data-ttu-id="06475-105">Kategorie</span><span class="sxs-lookup"><span data-stu-id="06475-105">Category</span></span> | <span data-ttu-id="06475-106">Úlohu nebo aspekt</span><span class="sxs-lookup"><span data-stu-id="06475-106">Task or consideration</span></span> | <span data-ttu-id="06475-107">Popis</span><span class="sxs-lookup"><span data-stu-id="06475-107">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="06475-108">Nízký výkon</span><span class="sxs-lookup"><span data-stu-id="06475-108">Slow performance</span></span> |<span data-ttu-id="06475-109">Najít dlouho běžící dotaz na uživatele</span><span class="sxs-lookup"><span data-stu-id="06475-109">Find a long-running user query</span></span> |<span data-ttu-id="06475-110">[Najít dlouho běžící dotazy][Find long-running queries]</span><span class="sxs-lookup"><span data-stu-id="06475-110">[Find long-running queries][Find long-running queries]</span></span> |
| <span data-ttu-id="06475-111">Souběžnost</span><span class="sxs-lookup"><span data-stu-id="06475-111">Concurrency</span></span> |<span data-ttu-id="06475-112">Přidělování prostředků s souběžných uživatelů na dotazy</span><span class="sxs-lookup"><span data-stu-id="06475-112">Assign concurrent resources to user queries</span></span> |<span data-ttu-id="06475-113">[Souběžnost a úlohy správy][Concurrency and workload management]</span><span class="sxs-lookup"><span data-stu-id="06475-113">[Concurrency and workload management][Concurrency and workload management]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="06475-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06475-114">Next steps</span></span>
<span data-ttu-id="06475-115">Pro další správu tipy, přejděte na [přehled správy][Management overview].</span><span class="sxs-lookup"><span data-stu-id="06475-115">For more management tips, head over to the [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Find long-running queries]: sql-data-warehouse-manage-monitor.md
[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md
[Management overview]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->


<!--Other Web references-->
