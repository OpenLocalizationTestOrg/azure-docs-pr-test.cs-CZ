---
title: "aaaCreate tabulkový model pomocí návrháře Azure Analysis Services – webové hello | Microsoft Docs"
description: "Zjistěte, jak toocreate tabulkový model pomocí Azure Analysis Services hello Návrhář na portálu Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: a37b326b76c84fc3a4300827bc1c8706b0584701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-model-in-azure-portal"></a><span data-ttu-id="e9bab-103">Vytvoření modelu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9bab-103">Create a model in Azure portal</span></span>

<span data-ttu-id="e9bab-104">Hello Azure Analysis Services webové návrháře (preview) funkce na portálu Azure poskytuje rychlý a snadný způsob toocreate a upravit tabulkové modely a dotaz modelu dat vpravo v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e9bab-104">hello Azure Analysis Services web designer (preview) feature in Azure portal provides a quick and easy way toocreate and edit tabular models and query model data right in your browser.</span></span> 

<span data-ttu-id="e9bab-105">Mějte na paměti, Návrhář hello **preview**.</span><span class="sxs-lookup"><span data-stu-id="e9bab-105">Keep in mind, hello web designer is **preview**.</span></span> <span data-ttu-id="e9bab-106">Když se přidává nové funkce všechny hello čas, ve verzi preview, funkce je omezena.</span><span class="sxs-lookup"><span data-stu-id="e9bab-106">While new functionality is being added all hello time, in preview, functionality is limited.</span></span> <span data-ttu-id="e9bab-107">Pro pokročilejší model vývoje a testování je nejlepší toouse Visual Studio (SSDT) a SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="e9bab-107">For more advanced model development and testing, it's best toouse Visual Studio (SSDT) and SQL Server Management Studio (SSMS).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9bab-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9bab-108">Prerequisites</span></span>

- <span data-ttu-id="e9bab-109">Serveru Azure Analysis Services na úroveň Standard nebo vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="e9bab-109">An Azure Analysis Services server at hello Standard or Developer tier.</span></span> <span data-ttu-id="e9bab-110">Nové modelů vytvořených pomocí návrháře webové hello jsou DirectQuery, podporuje pouze tyto vrstvy.</span><span class="sxs-lookup"><span data-stu-id="e9bab-110">New models created by using hello Web designer are DirectQuery, supported only by these tiers.</span></span>
- <span data-ttu-id="e9bab-111">Databáze SQL Azure, Azure SQL Data Warehouse nebo souboru Power BI Desktop (.pbix) jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="e9bab-111">An Azure SQL Database, Azure SQL Data Warehouse, or Power BI Desktop (.pbix) file as a datasource.</span></span> <span data-ttu-id="e9bab-112">Nové modely vytvořené z Power BI Desktop podporu soubory databáze SQL Azure, Azure SQL Data Warehouse, Oracle a Teradata datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="e9bab-112">New models created from Power BI Desktop files support Azure SQL Database, Azure SQL Data Warehouse, Oracle, and Teradata data sources.</span></span>
- <span data-ttu-id="e9bab-113">Účet systému SQL Server a heslo pro připojení zdroje dat tooAzure SQL Database nebo Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e9bab-113">A SQL Server account and password for connecting tooAzure SQL Database or Azure SQL Data Warehouse data sources.</span></span>

## <a name="toocreate-a-new-tabular-model"></a><span data-ttu-id="e9bab-114">toocreate nový tabulkový model.</span><span class="sxs-lookup"><span data-stu-id="e9bab-114">toocreate a new tabular model</span></span>

1. <span data-ttu-id="e9bab-115">Na vašem serveru **přehled** okno > **Návrhář**, klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="e9bab-115">In your server's **Overview** blade > **Web designer**, click **Open**.</span></span>

    ![Vytvoření modelu na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. <span data-ttu-id="e9bab-117">V **Návrhář** > **modely**, klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="e9bab-117">In **Web designer** > **Models**, click **+ Add**.</span></span>

    ![Vytvoření modelu na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. <span data-ttu-id="e9bab-119">V **nový model**, zadejte název modelu a potom vyberte zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="e9bab-119">In **New model**, type a model name, and then select a data source.</span></span>

    ![Dialogové okno Nový model na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. <span data-ttu-id="e9bab-121">V **Connect**, zadejte vlastnosti připojení hello.</span><span class="sxs-lookup"><span data-stu-id="e9bab-121">In **Connect**, enter hello connection properties.</span></span> <span data-ttu-id="e9bab-122">Uživatelské jméno a heslo musí být účet systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e9bab-122">Username and password must be a SQL Server account.</span></span>

     ![Připojit dialogové okno na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. <span data-ttu-id="e9bab-124">V **tabulek a zobrazení**, vyberte tooinclude hello tabulky v modelu a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e9bab-124">In **Tables and views**, select hello tables tooinclude in your model, and then click **Create**.</span></span> <span data-ttu-id="e9bab-125">Jsou automaticky vytvářet relace mezi tabulkami s pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="e9bab-125">Relationships are created automatically between tables with a key pair.</span></span>

     ![Vyberte tabulky a zobrazení](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

<span data-ttu-id="e9bab-127">Nový model se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e9bab-127">Your new model appears in your browser.</span></span> <span data-ttu-id="e9bab-128">Tady můžete:</span><span class="sxs-lookup"><span data-stu-id="e9bab-128">From here, you can:</span></span>   

- <span data-ttu-id="e9bab-129">Dotaz na data modelu tak, že Návrháře dotazů toohello pole přetažením přidáte filtry.</span><span class="sxs-lookup"><span data-stu-id="e9bab-129">Query model data by dragging fields toohello query designer and adding filters.</span></span>
- <span data-ttu-id="e9bab-130">Vytvořte nové míry v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="e9bab-130">Create new measures in tables.</span></span>
- <span data-ttu-id="e9bab-131">Úpravy metadat modelu s použitím hello json editor.</span><span class="sxs-lookup"><span data-stu-id="e9bab-131">Edit model metadata by using hello json editor.</span></span>
- <span data-ttu-id="e9bab-132">Visual Studio (SSDT), Power BI Desktop nebo aplikace Excel otevřít hello model.</span><span class="sxs-lookup"><span data-stu-id="e9bab-132">Open hello model in Visual Studio (SSDT), Power BI Desktop, or Excel.</span></span>

![Vyberte tabulky a zobrazení](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> <span data-ttu-id="e9bab-134">Když budete upravovat metadata modelu nebo vytvářet nové míry v prohlížeči, ukládáte modelu tooyour tyto změny v Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bab-134">When you edit model metadata or create new measures in your browser, you're saving those changes tooyour model in Azure.</span></span> <span data-ttu-id="e9bab-135">Také při práci na modelu SSDT, Power BI Desktop nebo aplikace Excel, můžete získat váš model synchronizován.</span><span class="sxs-lookup"><span data-stu-id="e9bab-135">If you're also working on your model in SSDT, Power BI Desktop, or Excel, your model can get out of sync.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e9bab-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9bab-136">Next steps</span></span> 
[<span data-ttu-id="e9bab-137">Spravovat role databáze a uživatele</span><span class="sxs-lookup"><span data-stu-id="e9bab-137">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="e9bab-138">Propojení s Excelem</span><span class="sxs-lookup"><span data-stu-id="e9bab-138">Connect with Excel</span></span>](analysis-services-connect-excel.md)  


