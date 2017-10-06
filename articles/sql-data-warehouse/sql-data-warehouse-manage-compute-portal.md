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
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure)
> [!div class="op_single_selector"]
> * [Přehled](sql-data-warehouse-manage-compute-overview.md)
> * [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Škálování výpočetní výkon
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

toochange výpočetní prostředky:

1. Otevřete hello [portál Azure][Azure portal], otevřete databázi a klikněte na tlačítko **škálování**.

    ![Klikněte na škálování][1]
2. V okně škálování hello hello posuvník doleva nebo pravým nastavení DWU toochange hello.

    ![Posuvník][2]
3. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** tooconfirm nebo **žádné** toocancel.

    ![Kliknutí na Uložit][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetní
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

toopause databáze:

1. Otevřete hello [portál Azure] [ Azure portal] a otevřete vaší databáze. Všimněte si, že hello stav je **Online**.

    ![Online stav][6]
2. Klikněte na tlačítko toosuspend paměťovou a výpočetní prostředky, **pozastavení**, a pak se zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** tooconfirm nebo **žádné** toocancel.

    ![Potvrďte pozastavení][7]
3. Při spouštění hello databáze SQL Data Warehouse, je stav hello **pozastaveno**.
4. Pokud je stav hello **pozastaveno**, se provádí operace pozastavení hello a jsou už se vám účtována Dwu.

    ![Pozastavení stav][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Obnovit výpočetní
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

tooresume databáze:

1. Otevřete hello [portál Azure] [ Azure portal] a otevřete vaší databáze. Všimněte si, že hello stav je **pozastaveno**.

    ![Pozastavení databáze][4]
2. Klikněte na databázi hello tooresume **spustit**, a pak se zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** tooconfirm nebo **žádné** toocancel.

    ![Potvrďte obnovení][5]
3. Při spouštění hello databáze SQL Data Warehouse, je stav hello "Obnovování".
4. Pokud je stav hello **online**, hello databáze je připravena.

    ![Online stav][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [přehled správy][Management overview].

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
