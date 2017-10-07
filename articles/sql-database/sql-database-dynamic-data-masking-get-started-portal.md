---
title: "Portál Azure: maskování dynamická data databáze SQL | Microsoft Docs"
description: "Jak tooget pracovat s dynamickými daty SQL Database maskování v hello portálu Azure"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 5c5f74682a15d42eceb78c3ca0705401e1ac0ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-hello-azure-portal"></a><span data-ttu-id="ba9be-103">Začínáme s dynamickými daty SQL Database maskování s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ba9be-103">Get started with SQL Database dynamic data masking with hello Azure Portal</span></span>

<span data-ttu-id="ba9be-104">Toto téma ukazuje, jak tooimplement [dynamická data maskování](sql-database-dynamic-data-masking-get-started.md) s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba9be-104">This topic shows you how tooimplement [dynamic data masking](sql-database-dynamic-data-masking-get-started.md) with hello Azure portal.</span></span> <span data-ttu-id="ba9be-105">Můžete taky implementovat pomocí maskování dynamická data [rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) nebo hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba9be-105">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>


## <a name="set-up-dynamic-data-masking-for-your-database-using-hello-azure-portal"></a><span data-ttu-id="ba9be-106">Nastavit maskování dynamická data pro vaši databázi pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ba9be-106">Set up dynamic data masking for your database using hello Azure Portal</span></span>
1. <span data-ttu-id="ba9be-107">Spuštění hello Azure Portal na [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ba9be-107">Launch hello Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ba9be-108">Přejděte okno nastavení toohello hello databáze, která zahrnuje hello citlivá data, která chcete toomask.</span><span class="sxs-lookup"><span data-stu-id="ba9be-108">Navigate toohello settings blade of hello database that includes hello sensitive data you want toomask.</span></span>
3. <span data-ttu-id="ba9be-109">Klikněte na tlačítko hello **dynamického maskování dat** dlaždice, který spouští hello **dynamického maskování dat** okno konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ba9be-109">Click hello **Dynamic Data Masking** tile which launches hello **Dynamic Data Masking** configuration blade.</span></span>
   
   * <span data-ttu-id="ba9be-110">Alternativně můžete přejděte dolů toohello **operace** části a klikněte na tlačítko **dynamického maskování dat**.</span><span class="sxs-lookup"><span data-stu-id="ba9be-110">Alternatively, you can scroll down toohello **Operations** section and click **Dynamic Data Masking**.</span></span>
     
     ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. <span data-ttu-id="ba9be-112">V hello **dynamického maskování dat** okno konfigurace, mohou se zobrazit některé sloupce databáze tento motor doporučení hello má označeny k maskování.</span><span class="sxs-lookup"><span data-stu-id="ba9be-112">In hello **Dynamic Data Masking** configuration blade you may see some database columns that hello recommendations engine has flagged for masking.</span></span> <span data-ttu-id="ba9be-113">V pořadí tooaccept hello doporučení, stačí kliknout na **přidat masku** pro jeden nebo více sloupců a masky bude vytvořena podle hello výchozí typ daného sloupce.</span><span class="sxs-lookup"><span data-stu-id="ba9be-113">In order tooaccept hello recommendations, just click **Add Mask** for one or more columns and a mask will be created based on hello default type for this column.</span></span> <span data-ttu-id="ba9be-114">Můžete změnit hello funkci maskování tak, že kliknete na pravidlo hello úpravy hello maskování pole formátu tooa jiného formátu podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="ba9be-114">You can change hello masking function by clicking on hello masking rule and editing hello masking field format tooa different format of your choice.</span></span> <span data-ttu-id="ba9be-115">Být jisti tooclick **Uložit** toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba9be-115">Be sure tooclick **Save** toosave your settings.</span></span>
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. <span data-ttu-id="ba9be-117">tooadd masku pro všechny sloupce v databázi, hello horní části hello **dynamického maskování dat** konfigurace okna klikněte na tlačítko **přidat masku** tooopen hello **přidat pravidlo maskování** okno Konfigurace</span><span class="sxs-lookup"><span data-stu-id="ba9be-117">tooadd a mask for any column in your database, at hello top of hello **Dynamic Data Masking** configuration blade click **Add Mask** tooopen hello **Add Masking Rule** configuration blade</span></span>
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. <span data-ttu-id="ba9be-119">Vyberte hello **schématu**, **tabulky** a **sloupec** toodefine hello určené pole, které se maskování.</span><span class="sxs-lookup"><span data-stu-id="ba9be-119">Select hello **Schema**, **Table** and **Column** toodefine hello designated field that will be masked.</span></span>
7. <span data-ttu-id="ba9be-120">Vyberte **maskování formátu pole** hello seznamu citlivých dat maskování kategorií.</span><span class="sxs-lookup"><span data-stu-id="ba9be-120">Choose a **Masking Field Format** from hello list of sensitive data masking categories.</span></span>
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. <span data-ttu-id="ba9be-122">Klikněte na tlačítko **Uložit** v datech hello maskování pravidlo okno tooupdate hello sadu maskování pravidla v dynamické maskování zásad dat hello.</span><span class="sxs-lookup"><span data-stu-id="ba9be-122">Click **Save** in hello data masking rule blade tooupdate hello set of masking rules in hello dynamic data masking policy.</span></span>
9. <span data-ttu-id="ba9be-123">Uživatelé SQL hello typu nebo AAD identity, které mají být vyloučeny z maskování a mají přístup toohello odmaskováno citlivá data.</span><span class="sxs-lookup"><span data-stu-id="ba9be-123">Type hello SQL users or AAD identities that should be excluded from masking, and have access toohello unmasked sensitive data.</span></span> <span data-ttu-id="ba9be-124">To by měl být středníky oddělený seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ba9be-124">This should be a semicolon-separated list of users.</span></span> <span data-ttu-id="ba9be-125">Všimněte si, že uživatelé s oprávněním správce vždy mají přístup toohello původní odmaskovaná data.</span><span class="sxs-lookup"><span data-stu-id="ba9be-125">Note that users with administrator privileges always have access toohello original unmasked data.</span></span>
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > <span data-ttu-id="ba9be-127">toomake ji proto hello aplikační vrstvu můžete zobrazit citlivá data pro uživatele privilegovaný aplikací, přidejte hello uživatele SQL nebo AAD identity hello aplikace používá tooquery hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ba9be-127">toomake it so hello application layer can display sensitive data for application privileged users, add hello SQL user or AAD identity hello application uses tooquery hello database.</span></span> <span data-ttu-id="ba9be-128">Důrazně doporučujeme, aby tento seznam obsahovat minimální počet mohou uživatelé s oprávněním toominimize ohrožení citlivých dat hello.</span><span class="sxs-lookup"><span data-stu-id="ba9be-128">It is highly recommended that this list contain a minimal number of privileged users toominimize exposure of hello sensitive data.</span></span>
   > 
   > 
10. <span data-ttu-id="ba9be-129">Klikněte na tlačítko **Uložit** v datech hello maskování konfigurace okna toosave hello maskování nové nebo aktualizované zásady.</span><span class="sxs-lookup"><span data-stu-id="ba9be-129">Click **Save** in hello data masking configuration blade toosave hello new or updated masking policy.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ba9be-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba9be-130">Next steps</span></span>

* <span data-ttu-id="ba9be-131">Přehled maskování dynamickými daty naleznete v tématu [dynamická data maskování](sql-database-dynamic-data-masking-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ba9be-131">For an overview of dynamic data masking, see [dynamic data masking](sql-database-dynamic-data-masking-get-started.md).</span></span>
* <span data-ttu-id="ba9be-132">Můžete taky implementovat pomocí maskování dynamická data [rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) nebo hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba9be-132">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>
