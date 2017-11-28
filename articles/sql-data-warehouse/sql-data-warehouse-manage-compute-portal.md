---
title: "aaaManage výpočetní výkon v Azure SQL Data Warehouse (portál Azure) | Microsoft Docs"
description: "Portál Azure úlohy toomanage výpočetní výkon. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení toosave náklady na výpočetní prostředky."
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
ms.openlocfilehash: b2e84b3763e97ce88c190eecfb64b2d06f727229
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a><span data-ttu-id="3d31c-105">Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure)</span><span class="sxs-lookup"><span data-stu-id="3d31c-105">Manage compute power in Azure SQL Data Warehouse (Azure portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3d31c-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="3d31c-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="3d31c-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3d31c-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="3d31c-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3d31c-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="3d31c-109">REST</span><span class="sxs-lookup"><span data-stu-id="3d31c-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="3d31c-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="3d31c-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a><span data-ttu-id="3d31c-111">Škálování výpočetní výkon</span><span class="sxs-lookup"><span data-stu-id="3d31c-111">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="3d31c-112">toochange výpočetní prostředky:</span><span class="sxs-lookup"><span data-stu-id="3d31c-112">toochange compute resources:</span></span>

1. <span data-ttu-id="3d31c-113">Otevřete hello [portál Azure][Azure portal], otevřete databázi a klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="3d31c-113">Open hello [Azure portal][Azure portal], open your database, and click **Scale**.</span></span>

    ![Klikněte na škálování][1]
2. <span data-ttu-id="3d31c-115">V okně škálování hello hello posuvník doleva nebo pravým nastavení DWU toochange hello.</span><span class="sxs-lookup"><span data-stu-id="3d31c-115">In hello Scale blade, move hello slider left or right toochange hello DWU setting.</span></span>

    ![Posuvník][2]
3. <span data-ttu-id="3d31c-117">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3d31c-117">Click **Save**.</span></span> <span data-ttu-id="3d31c-118">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="3d31c-118">A confirmation message appears.</span></span> <span data-ttu-id="3d31c-119">Klikněte na tlačítko **Ano** tooconfirm nebo **žádné** toocancel.</span><span class="sxs-lookup"><span data-stu-id="3d31c-119">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Kliknutí na Uložit][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="3d31c-121">Pozastavit výpočetní</span><span class="sxs-lookup"><span data-stu-id="3d31c-121">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="3d31c-122">toopause databáze:</span><span class="sxs-lookup"><span data-stu-id="3d31c-122">toopause a database:</span></span>

1. <span data-ttu-id="3d31c-123">Otevřete hello [portál Azure] [ Azure portal] a otevřete vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="3d31c-123">Open hello [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="3d31c-124">Všimněte si, že hello stav je **Online**.</span><span class="sxs-lookup"><span data-stu-id="3d31c-124">Notice that hello Status is **Online**.</span></span>

    ![Online stav][6]
2. <span data-ttu-id="3d31c-126">Klikněte na tlačítko toosuspend paměťovou a výpočetní prostředky, **pozastavení**, a pak se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="3d31c-126">toosuspend compute and memory resources, click **Pause**, and then a confirmation message appears.</span></span> <span data-ttu-id="3d31c-127">Klikněte na tlačítko **Ano** tooconfirm nebo **žádné** toocancel.</span><span class="sxs-lookup"><span data-stu-id="3d31c-127">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Potvrďte pozastavení][7]
3. <span data-ttu-id="3d31c-129">Při spouštění hello databáze SQL Data Warehouse, je stav hello **pozastaveno**.</span><span class="sxs-lookup"><span data-stu-id="3d31c-129">While SQL Data Warehouse is starting hello database, hello status is **Pausing**.</span></span>
4. <span data-ttu-id="3d31c-130">Pokud je stav hello **pozastaveno**, se provádí operace pozastavení hello a jsou už se vám účtována Dwu.</span><span class="sxs-lookup"><span data-stu-id="3d31c-130">When hello status is **Paused**, hello pause operation is done and you are no longer being charged for DWUs.</span></span>

    ![Pozastavení stav][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="3d31c-132">Obnovit výpočetní</span><span class="sxs-lookup"><span data-stu-id="3d31c-132">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="3d31c-133">tooresume databáze:</span><span class="sxs-lookup"><span data-stu-id="3d31c-133">tooresume a database:</span></span>

1. <span data-ttu-id="3d31c-134">Otevřete hello [portál Azure] [ Azure portal] a otevřete vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="3d31c-134">Open hello [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="3d31c-135">Všimněte si, že hello stav je **pozastaveno**.</span><span class="sxs-lookup"><span data-stu-id="3d31c-135">Notice that hello Status is **Paused**.</span></span>

    ![Pozastavení databáze][4]
2. <span data-ttu-id="3d31c-137">Klikněte na databázi hello tooresume **spustit**, a pak se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="3d31c-137">tooresume hello database click **Start**, and then a confirmation message appears.</span></span> <span data-ttu-id="3d31c-138">Klikněte na tlačítko **Ano** tooconfirm nebo **žádné** toocancel.</span><span class="sxs-lookup"><span data-stu-id="3d31c-138">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Potvrďte obnovení][5]
3. <span data-ttu-id="3d31c-140">Při spouštění hello databáze SQL Data Warehouse, je stav hello "Obnovování".</span><span class="sxs-lookup"><span data-stu-id="3d31c-140">While SQL Data Warehouse is starting hello database, hello status is "Resuming".</span></span>
4. <span data-ttu-id="3d31c-141">Pokud je stav hello **online**, hello databáze je připravena.</span><span class="sxs-lookup"><span data-stu-id="3d31c-141">When hello status is **online**, hello database is ready.</span></span>

    ![Online stav][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="3d31c-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d31c-143">Next steps</span></span>
<span data-ttu-id="3d31c-144">Další informace najdete v tématu [přehled správy][Management overview].</span><span class="sxs-lookup"><span data-stu-id="3d31c-144">For more information, see [Management overview][Management overview].</span></span>

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
