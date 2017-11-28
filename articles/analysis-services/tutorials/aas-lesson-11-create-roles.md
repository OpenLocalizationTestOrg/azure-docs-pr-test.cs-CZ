---
title: "Kurz služby Azure Analysis Services – Lekce 11: Vytvoření rolí | Dokumentace Microsoftu"
description: "Popisuje, jak vytvořit role v projektu Kurz služby Azure Analysis Services."
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
ms.openlocfilehash: 085a36edd2a0e80123ac8754b438bceadfa6c0e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-11-create-roles"></a><span data-ttu-id="17497-103">Lekce 11: Vytvoření rolí</span><span class="sxs-lookup"><span data-stu-id="17497-103">Lesson 11: Create roles</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="17497-104">V této lekci vytvoříte role.</span><span class="sxs-lookup"><span data-stu-id="17497-104">In this lesson, you create roles.</span></span> <span data-ttu-id="17497-105">Role poskytují objekt databáze modelu a zabezpečení dat tím, že omezují přístup pouze na uživatele, kteří jsou členy dané role.</span><span class="sxs-lookup"><span data-stu-id="17497-105">Roles provide model database object and data security by limiting access to only those users that are role members.</span></span> <span data-ttu-id="17497-106">Každá role je definována s jediným oprávněním: Žádné, Čtení, Čtení a zpracování, Zpracování nebo Správce.</span><span class="sxs-lookup"><span data-stu-id="17497-106">Each role is defined with a single permission: None, Read, Read and Process, Process, or Administrator.</span></span> <span data-ttu-id="17497-107">Role je možné definovat při vytváření modelu pomocí Správce rolí.</span><span class="sxs-lookup"><span data-stu-id="17497-107">Roles can be defined during model authoring by using Role Manager.</span></span> <span data-ttu-id="17497-108">Po nasazení modelu můžete role spravovat pomocí aplikace SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="17497-108">After a model has been deployed, you can manage roles by using SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="17497-109">Další informace najdete v tématu [Role](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="17497-109">To learn more, see [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span></span>
  
> [!NOTE]  
> <span data-ttu-id="17497-110">Vytvoření rolí není nezbytné pro dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="17497-110">Creating roles is not necessary to complete this tutorial.</span></span> <span data-ttu-id="17497-111">Ve výchozím nastavení má účet, ke kterému jste právě přihlášeni, oprávnění správce modelu.</span><span class="sxs-lookup"><span data-stu-id="17497-111">By default, the account you are currently logged in with has Administrator privileges on the model.</span></span> <span data-ttu-id="17497-112">Aby však mohli data procházet pomocí klienta sestav ostatní uživatelé ve vaší organizaci, musíte vytvořit alespoň jednu roli s oprávněním Čtení a přidat tyto uživatele jako členy.</span><span class="sxs-lookup"><span data-stu-id="17497-112">However, for other users in your organization to browse by using a reporting client, you must create at least one role with Read permissions and add those users as members.</span></span>  
  
<span data-ttu-id="17497-113">Vytvoříte tři role:</span><span class="sxs-lookup"><span data-stu-id="17497-113">You create three roles:</span></span>  
  
-   <span data-ttu-id="17497-114">**Manažer prodeje** – Tato role může zahrnovat uživatele ve vaší organizaci, u kterých chcete, aby měli oprávnění Čtení ke všem objektům a datům modelu.</span><span class="sxs-lookup"><span data-stu-id="17497-114">**Sales Manager** – This role can include users in your organization for which you want to have Read permission to all model objects and data.</span></span>  
  
-   <span data-ttu-id="17497-115">**Analytik prodeje v USA** – Tato role může zahrnovat uživatele ve vaší organizaci, u kterých chcete, aby mohli procházet pouze data související s prodejem v USA.</span><span class="sxs-lookup"><span data-stu-id="17497-115">**Sales Analyst US** – This role can include users in your organization for which you want only to be able to browse data related to sales in the United States.</span></span> <span data-ttu-id="17497-116">Pro tuto roli pomocí vzorce DAX nadefinujete *Filtr řádků*, který členům umožní procházet pouze data pro USA.</span><span class="sxs-lookup"><span data-stu-id="17497-116">For this role, you use a DAX formula to define a *Row Filter*, which restricts members to browse data only for the United States.</span></span>  
  
-   <span data-ttu-id="17497-117">**Správce** – Tato role může zahrnovat uživatele, u kterých chcete, aby měli oprávnění Správce, což jim poskytne neomezený přístup a oprávnění k provádění úloh správy s databází modelu.</span><span class="sxs-lookup"><span data-stu-id="17497-117">**Administrator** – This role can include users for which you want to have Administrator permission, which allows unlimited access and permissions to perform administrative tasks on the model database.</span></span>  
  
<span data-ttu-id="17497-118">Protože jsou účty uživatelů a skupin systému Windows ve vaší organizaci jedinečné, můžete mezi členy přidat účty z konkrétní organizace.</span><span class="sxs-lookup"><span data-stu-id="17497-118">Because Windows user and group accounts in your organization are unique, you can add accounts from your particular organization to members.</span></span> <span data-ttu-id="17497-119">Pro účely tohoto kurzu však můžete členy také ponechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="17497-119">However, for this tutorial, you can also leave the members blank.</span></span> <span data-ttu-id="17497-120">Účinky jednotlivých rolí otestujete později v lekci 12: Analýza v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="17497-120">You test the effect of each role later in Lesson 12: Analyze in Excel.</span></span>  
  
<span data-ttu-id="17497-121">Odhadovaný čas dokončení této lekce: **15 minut**</span><span class="sxs-lookup"><span data-stu-id="17497-121">Estimated time to complete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="17497-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17497-122">Prerequisites</span></span>  
<span data-ttu-id="17497-123">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="17497-123">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="17497-124">Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 10: Vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="17497-124">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span>  
  
## <a name="create-roles"></a><span data-ttu-id="17497-125">Vytvoření rolí</span><span class="sxs-lookup"><span data-stu-id="17497-125">Create roles</span></span>  
  
#### <a name="to-create-a-sales-manager-user-role"></a><span data-ttu-id="17497-126">Vytvoření role uživatele Manažer prodeje</span><span class="sxs-lookup"><span data-stu-id="17497-126">To create a Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="17497-127">V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Role** > **Role**.</span><span class="sxs-lookup"><span data-stu-id="17497-127">In Tabular Model Explorer, right-click **Roles** > **Roles**.</span></span>  
  
2.  <span data-ttu-id="17497-128">Ve Správci rolí klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="17497-128">In Role Manager, click **New**.</span></span>  
  
3.  <span data-ttu-id="17497-129">Klikněte na novou roli a potom ji ve sloupci **Název** přejmenujte na **Manažer prodeje**.</span><span class="sxs-lookup"><span data-stu-id="17497-129">Click the new role, and then in the **Name** column, rename the role to **Sales Manager**.</span></span>  
  
4.  <span data-ttu-id="17497-130">Ve sloupci **Oprávnění** klikněte na rozevírací seznam a vyberte oprávnění **Čtení**.</span><span class="sxs-lookup"><span data-stu-id="17497-130">In the **Permissions** column, click the dropdown list, and then select the **Read** permission.</span></span> 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  <span data-ttu-id="17497-132">Volitelné: Klikněte na kartu **Členové** a pak na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="17497-132">Optional: Click the **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="17497-133">V dialogovém okně **Vybrat uživatele nebo skupiny** zadejte uživatele nebo skupiny systému Windows z vaší organizace, které chcete zahrnout do role.</span><span class="sxs-lookup"><span data-stu-id="17497-133">In the **Select Users or Groups** dialog box, enter the Windows users or groups from your organization you want to include in the role.</span></span>  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a><span data-ttu-id="17497-134">Vytvoření role uživatele Analytik prodej v USA</span><span class="sxs-lookup"><span data-stu-id="17497-134">To create a Sales Analyst US user role</span></span>  
  
1.  <span data-ttu-id="17497-135">Ve Správci rolí klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="17497-135">In Role Manager, click **New**.</span></span>    
  
2.  <span data-ttu-id="17497-136">Přejmenujte roli na **Analytik prodeje v USA**.</span><span class="sxs-lookup"><span data-stu-id="17497-136">Rename the role to **Sales Analyst US**.</span></span>  
  
3.  <span data-ttu-id="17497-137">Přidělte této roli oprávnění **Čtení**.</span><span class="sxs-lookup"><span data-stu-id="17497-137">Give this role **Read** permission.</span></span>  
  
4.  <span data-ttu-id="17497-138">Klikněte na kartu Filtry řádků a pak jenom pro tabulku **DimGeography** zadejte do sloupce Filtr DAX následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="17497-138">Click the Row Filters tab, and then for the **DimGeography** table only, in the DAX Filter column, type the following formula:</span></span>  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    <span data-ttu-id="17497-139">Vzorec filtru řádků se musí přeložit na logickou hodnotu (TRUE/FALSE).</span><span class="sxs-lookup"><span data-stu-id="17497-139">A Row Filter formula must resolve to a Boolean (TRUE/FALSE) value.</span></span> <span data-ttu-id="17497-140">Pomocí tohoto vzorce určujete, že pro uživatele budou viditelné pouze řádky, na kterých má kód země nebo oblasti hodnotu USA.</span><span class="sxs-lookup"><span data-stu-id="17497-140">With this formula, you are specifying that only rows with the Country Region Code value of “US” are visible to the user.</span></span>  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  <span data-ttu-id="17497-142">Volitelné: Klikněte na kartu **Členové** a pak na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="17497-142">Optional: Click the **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="17497-143">V dialogovém okně **Vybrat uživatele nebo skupiny** zadejte uživatele nebo skupiny systému Windows z vaší organizace, které chcete zahrnout do role.</span><span class="sxs-lookup"><span data-stu-id="17497-143">In the **Select Users or Groups** dialog box, enter the Windows users or groups from your organization you want to include in the role.</span></span>  
  
#### <a name="to-create-an-administrator-user-role"></a><span data-ttu-id="17497-144">Vytvoření role uživatele Správce</span><span class="sxs-lookup"><span data-stu-id="17497-144">To create an Administrator user role</span></span>  
  
1.  <span data-ttu-id="17497-145">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="17497-145">Click **New**.</span></span>  
  
2.  <span data-ttu-id="17497-146">Přejmenujte roli na **Správce**.</span><span class="sxs-lookup"><span data-stu-id="17497-146">Rename the role to **Administrator**.</span></span>  
  
3.  <span data-ttu-id="17497-147">Přidělte této roli oprávnění **Správce**.</span><span class="sxs-lookup"><span data-stu-id="17497-147">Give this role **Administrator** permission.</span></span>  
  
4.  <span data-ttu-id="17497-148">Volitelné: Klikněte na kartu **Členové** a pak na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="17497-148">Optional: Click the **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="17497-149">V dialogovém okně **Vybrat uživatele nebo skupiny** zadejte uživatele nebo skupiny systému Windows z vaší organizace, které chcete zahrnout do role.</span><span class="sxs-lookup"><span data-stu-id="17497-149">In the **Select Users or Groups** dialog box, enter the Windows users or groups from your organization you want to include in the role.</span></span> 
  
  
## <a name="whats-next"></a><span data-ttu-id="17497-150">Co dále?</span><span class="sxs-lookup"><span data-stu-id="17497-150">What's next?</span></span>
<span data-ttu-id="17497-151">[Lekce 12: Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)</span><span class="sxs-lookup"><span data-stu-id="17497-151">[Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>

  
  
