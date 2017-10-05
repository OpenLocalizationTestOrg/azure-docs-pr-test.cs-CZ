---
title: "Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure) | Microsoft Docs"
description: "Azure úkoly portálu pro správu výpočetního výkonu. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a><span data-ttu-id="5460e-105">Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure)</span><span class="sxs-lookup"><span data-stu-id="5460e-105">Manage compute power in Azure SQL Data Warehouse (Azure portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5460e-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="5460e-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="5460e-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5460e-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="5460e-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5460e-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="5460e-109">REST</span><span class="sxs-lookup"><span data-stu-id="5460e-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="5460e-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="5460e-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a><span data-ttu-id="5460e-111">Škálování výpočetní výkon</span><span class="sxs-lookup"><span data-stu-id="5460e-111">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="5460e-112">Chcete-li změnit výpočetní prostředky:</span><span class="sxs-lookup"><span data-stu-id="5460e-112">To change compute resources:</span></span>

1. <span data-ttu-id="5460e-113">Otevřete [portál Azure][Azure portal], otevřete databázi a klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="5460e-113">Open the [Azure portal][Azure portal], open your database, and click **Scale**.</span></span>

    ![Klikněte na škálování][1]
2. <span data-ttu-id="5460e-115">V okně škálování nastavte posuvník doleva nebo doprava, chcete-li změnit nastavení DWU.</span><span class="sxs-lookup"><span data-stu-id="5460e-115">In the Scale blade, move the slider left or right to change the DWU setting.</span></span>

    ![Posuvník][2]
3. <span data-ttu-id="5460e-117">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5460e-117">Click **Save**.</span></span> <span data-ttu-id="5460e-118">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="5460e-118">A confirmation message appears.</span></span> <span data-ttu-id="5460e-119">Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.</span><span class="sxs-lookup"><span data-stu-id="5460e-119">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Kliknutí na Uložit][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="5460e-121">Pozastavit výpočetní</span><span class="sxs-lookup"><span data-stu-id="5460e-121">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="5460e-122">Chcete-li pozastavit databázi:</span><span class="sxs-lookup"><span data-stu-id="5460e-122">To pause a database:</span></span>

1. <span data-ttu-id="5460e-123">Otevřete [portál Azure] [ Azure portal] a otevřete vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="5460e-123">Open the [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="5460e-124">Všimněte si, že je stav **Online**.</span><span class="sxs-lookup"><span data-stu-id="5460e-124">Notice that the Status is **Online**.</span></span>

    ![Online stav][6]
2. <span data-ttu-id="5460e-126">Chcete-li pozastavit paměťovou a výpočetní prostředky, klikněte na tlačítko **pozastavení**, a pak se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="5460e-126">To suspend compute and memory resources, click **Pause**, and then a confirmation message appears.</span></span> <span data-ttu-id="5460e-127">Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.</span><span class="sxs-lookup"><span data-stu-id="5460e-127">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Potvrďte pozastavení][7]
3. <span data-ttu-id="5460e-129">Během spouštění databáze SQL Data Warehouse, je stav **pozastaveno**.</span><span class="sxs-lookup"><span data-stu-id="5460e-129">While SQL Data Warehouse is starting the database, the status is **Pausing**.</span></span>
4. <span data-ttu-id="5460e-130">Pokud je stav **pozastaveno**, se provádí operace pozastavení a jsou už se vám účtována Dwu.</span><span class="sxs-lookup"><span data-stu-id="5460e-130">When the status is **Paused**, the pause operation is done and you are no longer being charged for DWUs.</span></span>

    ![Pozastavení stav][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="5460e-132">Obnovit výpočetní</span><span class="sxs-lookup"><span data-stu-id="5460e-132">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="5460e-133">Chcete-li obnovit databázi:</span><span class="sxs-lookup"><span data-stu-id="5460e-133">To resume a database:</span></span>

1. <span data-ttu-id="5460e-134">Otevřete [portál Azure] [ Azure portal] a otevřete vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="5460e-134">Open the [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="5460e-135">Všimněte si, že je stav **pozastaveno**.</span><span class="sxs-lookup"><span data-stu-id="5460e-135">Notice that the Status is **Paused**.</span></span>

    ![Pozastavení databáze][4]
2. <span data-ttu-id="5460e-137">Obnovit databázi, klikněte na tlačítko **spustit**, a pak se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="5460e-137">To resume the database click **Start**, and then a confirmation message appears.</span></span> <span data-ttu-id="5460e-138">Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.</span><span class="sxs-lookup"><span data-stu-id="5460e-138">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Potvrďte obnovení][5]
3. <span data-ttu-id="5460e-140">Během spouštění databáze SQL Data Warehouse, je stav "Obnovování".</span><span class="sxs-lookup"><span data-stu-id="5460e-140">While SQL Data Warehouse is starting the database, the status is "Resuming".</span></span>
4. <span data-ttu-id="5460e-141">Pokud je stav **online**, databáze je připravena.</span><span class="sxs-lookup"><span data-stu-id="5460e-141">When the status is **online**, the database is ready.</span></span>

    ![Online stav][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="5460e-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5460e-143">Next steps</span></span>
<span data-ttu-id="5460e-144">Další informace najdete v tématu [přehled správy][Management overview].</span><span class="sxs-lookup"><span data-stu-id="5460e-144">For more information, see [Management overview][Management overview].</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
