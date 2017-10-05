---
title: "Kurz služby Azure Analysis Services – Lekce 12: Analýza v aplikaci Excel | Dokumentace Microsoftu"
description: "Popisuje, jak používat funkci Analýza v aplikaci Excel v projektu Kurz služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: 6f47de43ff8d94de22f8b7c12fa0707a8d7ffbbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-12-analyze-in-excel"></a><span data-ttu-id="580b9-103">Lekce 12: Analýza v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="580b9-103">Lesson 12: Analyze in Excel</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="580b9-104">V této lekci pomocí funkce Analýza v aplikaci Excel otevřete aplikaci Microsoft Excel, automaticky vytvoříte připojení k pracovnímu prostoru modelu a automaticky do listu přidáte kontingenční tabulku.</span><span class="sxs-lookup"><span data-stu-id="580b9-104">In this lesson, you use the Analyze in Excel feature to open Microsoft Excel, automatically create a connection to the model workspace, and automatically add a PivotTable to the worksheet.</span></span> <span data-ttu-id="580b9-105">Smyslem funkce Analýza v aplikaci Excel je poskytnout rychlý a snadný způsob, jak otestovat efektivnost návrhu modelu před jeho nasazením.</span><span class="sxs-lookup"><span data-stu-id="580b9-105">The Analyze in Excel feature is meant to provide a quick and easy way to test the efficacy of your model design prior to deploying your model.</span></span> <span data-ttu-id="580b9-106">V této lekci nebudete provádět žádné analýzy dat.</span><span class="sxs-lookup"><span data-stu-id="580b9-106">You do not perform any data analysis in this lesson.</span></span> <span data-ttu-id="580b9-107">Účelem této lekce je seznámit vás jako autora modelu s nástroji, pomocí kterých můžete otestovat návrh modelu.</span><span class="sxs-lookup"><span data-stu-id="580b9-107">The purpose of this lesson is to familiarize you, the model author, with the tools you can use to test your model design.</span></span>   
  
<span data-ttu-id="580b9-108">K dokončení této lekce je nutné, aby byla aplikace Excel nainstalována na stejném počítači jako SSDT.</span><span class="sxs-lookup"><span data-stu-id="580b9-108">To complete this lesson, Excel must be installed on the same computer as SSDT.</span></span>
  
<span data-ttu-id="580b9-109">Odhadovaný čas dokončení této lekce: **5 minut**</span><span class="sxs-lookup"><span data-stu-id="580b9-109">Estimated time to complete this lesson: **Five minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="580b9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="580b9-110">Prerequisites</span></span>  
<span data-ttu-id="580b9-111">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="580b9-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="580b9-112">Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 11: Vytvoření rolí](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="580b9-112">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 11: Create roles](../tutorials/aas-lesson-11-create-roles.md).</span></span>  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a><span data-ttu-id="580b9-113">Procházení s využitím výchozí perspektivy a perspektivy Internet Sales</span><span class="sxs-lookup"><span data-stu-id="580b9-113">Browse using the Default and Internet Sales perspectives</span></span>  
<span data-ttu-id="580b9-114">V rámci těchto prvních úkolů budete procházet model s využitím výchozí perspektivy, která zahrnuje všechny objekty modelu, a také s využitím perspektivy Internet Sales, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="580b9-114">In these first tasks, you browse your model by using both the default perspective, which includes all model objects, and also by using the Internet Sales perspective you earlier.</span></span> <span data-ttu-id="580b9-115">Perspektiva Internet Sales nezahrnuje objekt tabulky se zákazníky.</span><span class="sxs-lookup"><span data-stu-id="580b9-115">The Internet Sales perspective excludes the Customer table object.</span></span>  
  
#### <a name="to-browse-by-using-the-default-perspective"></a><span data-ttu-id="580b9-116">Procházení s využitím výchozí perspektivy</span><span class="sxs-lookup"><span data-stu-id="580b9-116">To browse by using the Default perspective</span></span>  
  
1.  <span data-ttu-id="580b9-117">Klikněte na nabídku **Model** > **Analyzovat v aplikaci Excel**.</span><span class="sxs-lookup"><span data-stu-id="580b9-117">Click the **Model** menu > **Analyze in Excel**.</span></span>  
  
2.  <span data-ttu-id="580b9-118">V dialogovém okně **Analýza v aplikaci Excel** klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="580b9-118">In the **Analyze in Excel** dialog box, click **OK**.</span></span>  
  
    <span data-ttu-id="580b9-119">Otevře se aplikace Excel s novým sešitem.</span><span class="sxs-lookup"><span data-stu-id="580b9-119">Excel opens with a new workbook.</span></span> <span data-ttu-id="580b9-120">Vytvoří se připojení ke zdroji dat pomocí aktuálního uživatelského účtu a k definování zobrazitelných polí se použije výchozí perspektiva.</span><span class="sxs-lookup"><span data-stu-id="580b9-120">A data source connection is created using the current user account and the Default perspective is used to define viewable fields.</span></span> <span data-ttu-id="580b9-121">Do listu se automaticky přidá kontingenční tabulka.</span><span class="sxs-lookup"><span data-stu-id="580b9-121">A PivotTable is automatically added to the worksheet.</span></span>  
  
3.  <span data-ttu-id="580b9-122">V aplikaci Excel si všimněte, že v části **Seznam polí kontingenční tabulky** se zobrazí skupiny měr **DimDate** a **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="580b9-122">In Excel, in the **PivotTable Field List**, notice the **DimDate** and **FactInternetSales** measure groups appear.</span></span> <span data-ttu-id="580b9-123">Zobrazí se také tabulky **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** a **FactInternetSales** s příslušnými sloupci.</span><span class="sxs-lookup"><span data-stu-id="580b9-123">The **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales** tables with their respective columns also appear.</span></span>  
  
4.  <span data-ttu-id="580b9-124">Zavřete aplikaci Excel bez ukládání sešitu.</span><span class="sxs-lookup"><span data-stu-id="580b9-124">Close Excel without saving the workbook.</span></span>  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a><span data-ttu-id="580b9-125">Procházení s využitím perspektivy Internet Sales</span><span class="sxs-lookup"><span data-stu-id="580b9-125">To browse by using the Internet Sales perspective</span></span>  
  
1.  <span data-ttu-id="580b9-126">Klikněte na nabídku **Model** a potom na **Analyzovat v aplikaci Excel**.</span><span class="sxs-lookup"><span data-stu-id="580b9-126">Click the **Model** menu, and then click **Analyze in Excel**.</span></span>  
  
2.  <span data-ttu-id="580b9-127">V dialogovém okně **Analýza v aplikaci Excel** ponechte vybranou možnost **Aktuální uživatel systému Windows**, v rozevíracím seznamu **Perspektiva** vyberte **Internet Sales** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="580b9-127">In the **Analyze in Excel** dialog box, leave **Current Windows User** selected, then in the **Perspective** drop-down listbox, select **Internet Sales**, and then click **OK**.</span></span> 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  <span data-ttu-id="580b9-129">V aplikaci Excel si všimněte, že v části **Pole kontingenční tabulky** není tabulka DimCustomer zahrnuta do seznamu polí.</span><span class="sxs-lookup"><span data-stu-id="580b9-129">In Excel, in **PivotTable Fields**, notice the DimCustomer table is excluded from the field list.</span></span>  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  <span data-ttu-id="580b9-131">Zavřete aplikaci Excel bez ukládání sešitu.</span><span class="sxs-lookup"><span data-stu-id="580b9-131">Close Excel without saving the workbook.</span></span>  
  
## <a name="browse-by-using-roles"></a><span data-ttu-id="580b9-132">Procházení s využitím rolí</span><span class="sxs-lookup"><span data-stu-id="580b9-132">Browse by using roles</span></span>  
<span data-ttu-id="580b9-133">Role jsou důležitou součástí každého tabelárního modelu.</span><span class="sxs-lookup"><span data-stu-id="580b9-133">Roles are an important part of any tabular model.</span></span> <span data-ttu-id="580b9-134">Pokud nemáte alespoň jednu roli, do které se uživatelé přidávají jako členové, pak uživatelé nebudou mít přístup k datům a nebudou je moci analyzovat pomocí vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="580b9-134">Without at least one role to which users are added as members, users cannot access and analyze data using your model.</span></span> <span data-ttu-id="580b9-135">Funkce Analýza v aplikaci Excel nabízí způsob, jak otestovat role, které jste definovali.</span><span class="sxs-lookup"><span data-stu-id="580b9-135">The Analyze in Excel feature provides a way for you to test the roles you have defined.</span></span>  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a><span data-ttu-id="580b9-136">Procházení s využitím role uživatele Manažer prodeje</span><span class="sxs-lookup"><span data-stu-id="580b9-136">To browse by using the Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="580b9-137">V SSDT klikněte na nabídku **Model** a potom na **Analyzovat v aplikaci Excel**.</span><span class="sxs-lookup"><span data-stu-id="580b9-137">In SSDT, click the **Model** menu, and then click **Analyze in Excel**.</span></span>  
  
2.  <span data-ttu-id="580b9-138">V části **Zadejte uživatelské jméno nebo roli pro připojení k modelu** vyberte **Role**, v rozevíracím seznamu vyberte **Manažer prodeje** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="580b9-138">In **Specify the user name or role to use to connect to the model**, select **Role**, and then in the drop-down listbox, select **Sales Manager**, and then click **OK**.</span></span>  
  
    <span data-ttu-id="580b9-139">Otevře se aplikace Excel s novým sešitem.</span><span class="sxs-lookup"><span data-stu-id="580b9-139">Excel opens with a new workbook.</span></span> <span data-ttu-id="580b9-140">Automaticky se vytvoří kontingenční tabulka.</span><span class="sxs-lookup"><span data-stu-id="580b9-140">A PivotTable is automatically created.</span></span> <span data-ttu-id="580b9-141">Seznam polí kontingenční tabulky zahrnuje všechna datová pole, která jsou v novém modelu k dispozici.</span><span class="sxs-lookup"><span data-stu-id="580b9-141">The Pivot Table Field List includes all the data fields available in your new model.</span></span>  
      
3.  <span data-ttu-id="580b9-142">Zavřete aplikaci Excel bez ukládání sešitu.</span><span class="sxs-lookup"><span data-stu-id="580b9-142">Close Excel without saving the workbook.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="580b9-143">Co dále?</span><span class="sxs-lookup"><span data-stu-id="580b9-143">What's next?</span></span>
<span data-ttu-id="580b9-144">Přejděte na další lekci: [Lekce 13: Nasazení](../tutorials/aas-lesson-13-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="580b9-144">Go to the next lesson: [Lesson 13: Deploy](../tutorials/aas-lesson-13-deploy.md).</span></span>

  
  
  
