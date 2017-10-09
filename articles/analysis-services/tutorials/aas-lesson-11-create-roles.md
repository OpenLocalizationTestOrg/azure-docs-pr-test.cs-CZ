---
<span data-ttu-id="140b6-101">Title: aaa "Azure Analysis Services kurz lekce 11: vytvoření role | Microsoft Docs"Popis: Popisuje, jak hello toocreate rolí v kurzu projektu Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="140b6-101">title: aaa"Azure Analysis Services tutorial lesson 11: Create roles | Microsoft Docs" description: Describes how toocreate roles in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="140b6-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="140b6-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="140b6-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="140b6-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-11-create-roles"></a><span data-ttu-id="140b6-104">Lekce 11: Vytvoření rolí</span><span class="sxs-lookup"><span data-stu-id="140b6-104">Lesson 11: Create roles</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="140b6-105">V této lekci vytvoříte role.</span><span class="sxs-lookup"><span data-stu-id="140b6-105">In this lesson, you create roles.</span></span> <span data-ttu-id="140b6-106">Role poskytují modelu zabezpečení objektů a dat databázi omezením přístupu tooonly uživatelům, kteří jsou členy role.</span><span class="sxs-lookup"><span data-stu-id="140b6-106">Roles provide model database object and data security by limiting access tooonly those users that are role members.</span></span> <span data-ttu-id="140b6-107">Každá role je definována s jediným oprávněním: Žádné, Čtení, Čtení a zpracování, Zpracování nebo Správce.</span><span class="sxs-lookup"><span data-stu-id="140b6-107">Each role is defined with a single permission: None, Read, Read and Process, Process, or Administrator.</span></span> <span data-ttu-id="140b6-108">Role je možné definovat při vytváření modelu pomocí Správce rolí.</span><span class="sxs-lookup"><span data-stu-id="140b6-108">Roles can be defined during model authoring by using Role Manager.</span></span> <span data-ttu-id="140b6-109">Po nasazení modelu můžete role spravovat pomocí aplikace SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="140b6-109">After a model has been deployed, you can manage roles by using SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="140b6-110">Další, najdete v části toolearn [role](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="140b6-110">toolearn more, see [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span></span>
  
> [!NOTE]  
> <span data-ttu-id="140b6-111">Vytváření rolí není nutné toocomplete v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="140b6-111">Creating roles is not necessary toocomplete this tutorial.</span></span> <span data-ttu-id="140b6-112">Ve výchozím nastavení hello účet, který jste právě přihlášeni, má oprávnění správce na hello modelu.</span><span class="sxs-lookup"><span data-stu-id="140b6-112">By default, hello account you are currently logged in with has Administrator privileges on hello model.</span></span> <span data-ttu-id="140b6-113">Ale pro ostatní uživatelé ve vaší organizaci toobrowse pomocí sestav klienta, musíte vytvořit alespoň jednu roli čtení oprávnění a tyto uživatele přidejte jako členy.</span><span class="sxs-lookup"><span data-stu-id="140b6-113">However, for other users in your organization toobrowse by using a reporting client, you must create at least one role with Read permissions and add those users as members.</span></span>  
  
<span data-ttu-id="140b6-114">Vytvoříte tři role:</span><span class="sxs-lookup"><span data-stu-id="140b6-114">You create three roles:</span></span>  
  
-   <span data-ttu-id="140b6-115">**Vedoucí prodeje** – tato role může obsahovat uživatele ve vaší organizaci, pro které chcete objekty modelu tooall oprávnění pro čtení toohave a data.</span><span class="sxs-lookup"><span data-stu-id="140b6-115">**Sales Manager** – This role can include users in your organization for which you want toohave Read permission tooall model objects and data.</span></span>  
  
-   <span data-ttu-id="140b6-116">**Prodejní analytik USA** – tato role může obsahovat uživatele ve vaší organizaci, pro které chcete jenom data možné toobrowse toobe související toosales ve Spojených státech amerických hello.</span><span class="sxs-lookup"><span data-stu-id="140b6-116">**Sales Analyst US** – This role can include users in your organization for which you want only toobe able toobrowse data related toosales in hello United States.</span></span> <span data-ttu-id="140b6-117">Pro tuto roli použijete toodefine vzorce DAX *řádkový filtr*, což omezuje členy toobrowse dat pouze pro hello Spojených státech amerických.</span><span class="sxs-lookup"><span data-stu-id="140b6-117">For this role, you use a DAX formula toodefine a *Row Filter*, which restricts members toobrowse data only for hello United States.</span></span>  
  
-   <span data-ttu-id="140b6-118">**Správce** – tato role může obsahovat uživatele, pro které chcete toohave oprávnění správce, které umožňuje neomezený přístup a oprávnění tooperform úlohy správy na hello modelové databáze.</span><span class="sxs-lookup"><span data-stu-id="140b6-118">**Administrator** – This role can include users for which you want toohave Administrator permission, which allows unlimited access and permissions tooperform administrative tasks on hello model database.</span></span>  
  
<span data-ttu-id="140b6-119">Protože jsou jedinečné účty uživatelů a skupin systému Windows ve vaší organizaci, můžete přidat účty z vaší organizace konkrétní toomembers.</span><span class="sxs-lookup"><span data-stu-id="140b6-119">Because Windows user and group accounts in your organization are unique, you can add accounts from your particular organization toomembers.</span></span> <span data-ttu-id="140b6-120">Ale pro účely tohoto kurzu můžete také ponechat hello členy prázdné.</span><span class="sxs-lookup"><span data-stu-id="140b6-120">However, for this tutorial, you can also leave hello members blank.</span></span> <span data-ttu-id="140b6-121">Testování účinku hello každou roli později v lekce 12: analyzovat v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="140b6-121">You test hello effect of each role later in Lesson 12: Analyze in Excel.</span></span>  
  
<span data-ttu-id="140b6-122">Odhadovaný čas toocomplete této lekci: **15 minut**</span><span class="sxs-lookup"><span data-stu-id="140b6-122">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="140b6-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="140b6-123">Prerequisites</span></span>  
<span data-ttu-id="140b6-124">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="140b6-124">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="140b6-125">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 10: vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="140b6-125">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span>  
  
## <a name="create-roles"></a><span data-ttu-id="140b6-126">Vytvoření rolí</span><span class="sxs-lookup"><span data-stu-id="140b6-126">Create roles</span></span>  
  
#### <a name="toocreate-a-sales-manager-user-role"></a><span data-ttu-id="140b6-127">toocreate roli uživatele správce prodeje</span><span class="sxs-lookup"><span data-stu-id="140b6-127">toocreate a Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="140b6-128">V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Role** > **Role**.</span><span class="sxs-lookup"><span data-stu-id="140b6-128">In Tabular Model Explorer, right-click **Roles** > **Roles**.</span></span>  
  
2.  <span data-ttu-id="140b6-129">Ve Správci rolí klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="140b6-129">In Role Manager, click **New**.</span></span>  
  
3.  <span data-ttu-id="140b6-130">Klikněte na tlačítko Nová role hello a potom v hello **název** sloupce, přejmenujte roli hello příliš**vedoucí prodeje**.</span><span class="sxs-lookup"><span data-stu-id="140b6-130">Click hello new role, and then in hello **Name** column, rename hello role too**Sales Manager**.</span></span>  
  
4.  <span data-ttu-id="140b6-131">V hello **oprávnění** sloupce, klikněte na tlačítko hello rozevíracího seznamu a pak vyberte hello **čtení** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="140b6-131">In hello **Permissions** column, click hello dropdown list, and then select hello **Read** permission.</span></span> 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  <span data-ttu-id="140b6-133">Volitelné: Klikněte na tlačítko hello **členy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="140b6-133">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="140b6-134">V hello **vybrat uživatele nebo skupiny** dialogové okno, zadejte uživatele Windows hello nebo skupiny z vaší organizace mají tooinclude v roli hello.</span><span class="sxs-lookup"><span data-stu-id="140b6-134">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span>  
  
#### <a name="toocreate-a-sales-analyst-us-user-role"></a><span data-ttu-id="140b6-135">toocreate roli uživatele analytik prodej USA</span><span class="sxs-lookup"><span data-stu-id="140b6-135">toocreate a Sales Analyst US user role</span></span>  
  
1.  <span data-ttu-id="140b6-136">Ve Správci rolí klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="140b6-136">In Role Manager, click **New**.</span></span>    
  
2.  <span data-ttu-id="140b6-137">Přejmenujte roli hello příliš**prodej analytik USA**.</span><span class="sxs-lookup"><span data-stu-id="140b6-137">Rename hello role too**Sales Analyst US**.</span></span>  
  
3.  <span data-ttu-id="140b6-138">Přidělte této roli oprávnění **Čtení**.</span><span class="sxs-lookup"><span data-stu-id="140b6-138">Give this role **Read** permission.</span></span>  
  
4.  <span data-ttu-id="140b6-139">Klikněte na kartu filtry řádek hello a pak pro hello **DimGeography** tabulky, sloupce filtru DAX hello, typ hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="140b6-139">Click hello Row Filters tab, and then for hello **DimGeography** table only, in hello DAX Filter column, type hello following formula:</span></span>  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    <span data-ttu-id="140b6-140">Vzorec A řádkový filtr je třeba vyřešit tooa hodnota logická hodnota (TRUE/FALSE).</span><span class="sxs-lookup"><span data-stu-id="140b6-140">A Row Filter formula must resolve tooa Boolean (TRUE/FALSE) value.</span></span> <span data-ttu-id="140b6-141">Pomocí tohoto vzorce určíte, že pouze řádky s hello kód země/oblasti hodnotu "US" jsou viditelné toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="140b6-141">With this formula, you are specifying that only rows with hello Country Region Code value of “US” are visible toohello user.</span></span>  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  <span data-ttu-id="140b6-143">Volitelné: Klikněte na tlačítko hello **členy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="140b6-143">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="140b6-144">V hello **vybrat uživatele nebo skupiny** dialogové okno, zadejte uživatele Windows hello nebo skupiny z vaší organizace mají tooinclude v roli hello.</span><span class="sxs-lookup"><span data-stu-id="140b6-144">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span>  
  
#### <a name="toocreate-an-administrator-user-role"></a><span data-ttu-id="140b6-145">toocreate role uživatele správce</span><span class="sxs-lookup"><span data-stu-id="140b6-145">toocreate an Administrator user role</span></span>  
  
1.  <span data-ttu-id="140b6-146">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="140b6-146">Click **New**.</span></span>  
  
2.  <span data-ttu-id="140b6-147">Přejmenujte roli hello příliš**správce**.</span><span class="sxs-lookup"><span data-stu-id="140b6-147">Rename hello role too**Administrator**.</span></span>  
  
3.  <span data-ttu-id="140b6-148">Přidělte této roli oprávnění **Správce**.</span><span class="sxs-lookup"><span data-stu-id="140b6-148">Give this role **Administrator** permission.</span></span>  
  
4.  <span data-ttu-id="140b6-149">Volitelné: Klikněte na tlačítko hello **členy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="140b6-149">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="140b6-150">V hello **vybrat uživatele nebo skupiny** dialogové okno, zadejte uživatele Windows hello nebo skupiny z vaší organizace mají tooinclude v roli hello.</span><span class="sxs-lookup"><span data-stu-id="140b6-150">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span> 
  
  
## <a name="whats-next"></a><span data-ttu-id="140b6-151">Co dále?</span><span class="sxs-lookup"><span data-stu-id="140b6-151">What's next?</span></span>
<span data-ttu-id="140b6-152">[Lekce 12: Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)</span><span class="sxs-lookup"><span data-stu-id="140b6-152">[Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>

  
  
