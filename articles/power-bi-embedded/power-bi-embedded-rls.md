---
title: "Zabezpečení na úrovni řádků s Power BI Embedded"
description: "Podrobnosti o zabezpečení na úrovni řádků s Power BI Embedded"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 7936ade5-2c75-435b-8314-ea7ca815867a
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 1cde5b9ee4c716af07d427d4d0eb3f0775d456ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a><span data-ttu-id="727d0-103">Zabezpečení na úrovni řádků v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="727d0-103">Row level security with Power BI Embedded</span></span>

<span data-ttu-id="727d0-104">Zabezpečení na úrovni řádků (RLS) slouží k omezení přístupu uživatelů k konkrétní dat v rámci sestavu nebo datovou sadu, povolení pro několik různých uživatelům používat stejné sestavě při všech dat vidět jiné.</span><span class="sxs-lookup"><span data-stu-id="727d0-104">Row level security (RLS) can be used to restrict user access to particular data within a report or dataset, allowing for multiple different users to use the same report while all seeing different data.</span></span> <span data-ttu-id="727d0-105">Power BI Embedded teď podporuje datové sady, které jsou nakonfigurované s RLS.</span><span class="sxs-lookup"><span data-stu-id="727d0-105">Power BI Embedded now supports datasets configured with RLS.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

<span data-ttu-id="727d0-106">Aby bylo možné využít výhod RLS, je důležité, že rozumíte tři hlavní koncepty; Uživatelé, role a pravidla.</span><span class="sxs-lookup"><span data-stu-id="727d0-106">In order to take advantage of RLS, it’s important you understand three main concepts; Users, Roles, and Rules.</span></span> <span data-ttu-id="727d0-107">Podívejme bližší pohled na každý:</span><span class="sxs-lookup"><span data-stu-id="727d0-107">Let’s take a closer look at each:</span></span>

<span data-ttu-id="727d0-108">**Uživatelé** – tyto jsou skutečné koncovým uživatelům prohlížení sestav.</span><span class="sxs-lookup"><span data-stu-id="727d0-108">**Users** –  These are the actual end-users viewing reports.</span></span> <span data-ttu-id="727d0-109">V Power BI Embedded uživatelé se identifikují podle vlastnosti uživatelské jméno v tokenu aplikace.</span><span class="sxs-lookup"><span data-stu-id="727d0-109">In Power BI Embedded, users are identified by the username property in an App Token.</span></span>

<span data-ttu-id="727d0-110">**Role** – uživatelé patří do role.</span><span class="sxs-lookup"><span data-stu-id="727d0-110">**Roles** – Users belong to roles.</span></span> <span data-ttu-id="727d0-111">Role je kontejner pro pravidla a může mít název něco jako "Vedoucí prodeje" nebo "Obchodního zástupce".</span><span class="sxs-lookup"><span data-stu-id="727d0-111">A role is a container for rules and can be named something like “Sales Manager” or “Sales Rep”.</span></span> <span data-ttu-id="727d0-112">V Power BI Embedded uživatelé se identifikují podle vlastnosti rolí v tokenu aplikace.</span><span class="sxs-lookup"><span data-stu-id="727d0-112">In Power BI Embedded, users are identified by the roles property in an App Token.</span></span>

<span data-ttu-id="727d0-113">**Pravidla** – role mají pravidla a tato pravidla jsou skutečné filtry, které se chystáte použít k datům.</span><span class="sxs-lookup"><span data-stu-id="727d0-113">**Rules** – Roles have rules, and those rules are the actual filters that are going to be applied to the data.</span></span> <span data-ttu-id="727d0-114">To může být stejně jednoduché jako "země = USA" nebo něco víc dynamické.</span><span class="sxs-lookup"><span data-stu-id="727d0-114">This could be as simple as “Country = USA” or something much more dynamic.</span></span>

### <a name="example"></a><span data-ttu-id="727d0-115">Příklad</span><span class="sxs-lookup"><span data-stu-id="727d0-115">Example</span></span>

<span data-ttu-id="727d0-116">Pro zbývající část tohoto článku poskytujeme příklad vytváření RLS a využívání, v rámci aplikace embedded.</span><span class="sxs-lookup"><span data-stu-id="727d0-116">For the rest of this article, we’ll provide an example of authoring RLS, and then consuming that within an embedded application.</span></span> <span data-ttu-id="727d0-117">Naše Ukázka používá [prodejní analýzy ukázka](http://go.microsoft.com/fwlink/?LinkID=780547) soubor PBIX.</span><span class="sxs-lookup"><span data-stu-id="727d0-117">Our example uses the [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX file.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

<span data-ttu-id="727d0-118">Naše ukázka prodejní analýzy prodejů pro všechny úložiště v určité maloobchodní řetězec.</span><span class="sxs-lookup"><span data-stu-id="727d0-118">Our Retail Analysis sample shows sales for all the stores in a particular retail chain.</span></span> <span data-ttu-id="727d0-119">Bez RLS, bez ohledu na to, které oblastní správce přihlásí a zobrazení sestavy, uvidí stejná data.</span><span class="sxs-lookup"><span data-stu-id="727d0-119">Without RLS, no matter which district manager signs in and views the report, they’ll see the same data.</span></span> <span data-ttu-id="727d0-120">Vedoucí pracovníci určil každý oblastní manager byste měli vidět jenom prodeje pro úložiště, které spravují a k tomu můžeme použít RLS.</span><span class="sxs-lookup"><span data-stu-id="727d0-120">Senior management has determined each district manager should only see the sales for the stores they manage, and to do this, we can use RLS.</span></span>

<span data-ttu-id="727d0-121">RLS je vytvořené v Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="727d0-121">RLS is authored in Power BI Desktop.</span></span> <span data-ttu-id="727d0-122">Po otevření datovou sadu a sestavu jsme můžete přepnout do zobrazení diagramu zobrazíte schéma:</span><span class="sxs-lookup"><span data-stu-id="727d0-122">When the dataset and report are opened, we can switch to diagram view to see the schema:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

<span data-ttu-id="727d0-123">Zde je několik věcí Všimněte s toto schéma:</span><span class="sxs-lookup"><span data-stu-id="727d0-123">Here are a few things to notice with this schema:</span></span>

* <span data-ttu-id="727d0-124">Všechny míry, jako například **celkový prodej**, jsou uloženy v **prodej** tabulky faktů.</span><span class="sxs-lookup"><span data-stu-id="727d0-124">All measures, like **Total Sales**, are stored in the **Sales** fact table.</span></span>
* <span data-ttu-id="727d0-125">Existují čtyři další dimenze související tabulky: **položky**, **čas**, **úložiště**, a **oblastní**.</span><span class="sxs-lookup"><span data-stu-id="727d0-125">There are four additional related dimension tables: **Item**, **Time**, **Store**, and **District**.</span></span>
* <span data-ttu-id="727d0-126">Šipky v řádcích vztah znamenat toho, jak filtry můžete toku z jedné tabulky do jiné.</span><span class="sxs-lookup"><span data-stu-id="727d0-126">The arrows on the relationship lines indicate which way filters can flow from one table to another.</span></span> <span data-ttu-id="727d0-127">Například, pokud filtr je umístěn na **čas [datum]**, v aktuálním schématu bude pouze filtrovat dolů hodnoty **prodej** tabulky.</span><span class="sxs-lookup"><span data-stu-id="727d0-127">For example, if a filter is placed on **Time[Date]**, in the current schema it would only filter down values in the **Sales** table.</span></span> <span data-ttu-id="727d0-128">Žádné jiné tabulky by ovlivnila tento filtr, protože všechny šipek na řádcích vztah bod registrace k tabulce prodeje a není rychle.</span><span class="sxs-lookup"><span data-stu-id="727d0-128">No other tables would be affected by this filter since all of the arrows on the relationship lines point to the sales table and not away.</span></span>
* <span data-ttu-id="727d0-129">**Oblastní** tabulka udává, který správce je pro každou oblast:</span><span class="sxs-lookup"><span data-stu-id="727d0-129">The **District** table indicates who the manager is for each district:</span></span>
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

<span data-ttu-id="727d0-130">Založené na tomto schématu v případě, že jsme použít filtr pro **oblastní Manager** sloupce v tabulce oblastní, a pokud tento filtr odpovídá uživatele zobrazení sestavy, bude tento filtr také filtrovat dolů **úložiště** a **prodej** tabulky jenom zobrazit data pro tento konkrétní oblastní správce.</span><span class="sxs-lookup"><span data-stu-id="727d0-130">Based on this schema, if we apply a filter to the **District Manager** column in the District table, and if that filter matches the user viewing the report, that filter will also filter down the **Store** and **Sales** tables to only show data for that particular district manager.</span></span>

<span data-ttu-id="727d0-131">Tady je jak:</span><span class="sxs-lookup"><span data-stu-id="727d0-131">Here’s how:</span></span>

1. <span data-ttu-id="727d0-132">Na kartě modelování klikněte na **spravovat role**.</span><span class="sxs-lookup"><span data-stu-id="727d0-132">On the Modeling tab, click **Manage Roles**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. <span data-ttu-id="727d0-133">Vytvoření nové role s názvem **Manager**.</span><span class="sxs-lookup"><span data-stu-id="727d0-133">Create a new role called **Manager**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. <span data-ttu-id="727d0-134">V **oblastní** tabulky zadejte následující výraz DAX: **[oblastní Manager] = USERNAME()**</span><span class="sxs-lookup"><span data-stu-id="727d0-134">In the **District** table enter the following DAX expression: **[District Manager] = USERNAME()**</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. <span data-ttu-id="727d0-135">Abyste měli jistotu, pravidla pracují na **modelování** , klikněte na **zobrazení jako role**a potom zadejte následující:</span><span class="sxs-lookup"><span data-stu-id="727d0-135">To make sure the rules are working, on the **Modeling** tab, click **View as Roles**, and then enter the following:</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   <span data-ttu-id="727d0-136">Sestavy se nyní zobrazí data, jako kdyby byly přihlášení jako **Andrew Ma**.</span><span class="sxs-lookup"><span data-stu-id="727d0-136">The reports will now show data as if you were signed in as **Andrew Ma**.</span></span>

<span data-ttu-id="727d0-137">Použití filtru způsob, jakým jsme se zde bude filtrovat dolů všechny záznamy v **oblastní**, **úložiště**, a **prodej** tabulky.</span><span class="sxs-lookup"><span data-stu-id="727d0-137">Applying the filter, the way we did here, will filter down all records in the **District**, **Store**, and **Sales** tables.</span></span> <span data-ttu-id="727d0-138">Ale kvůli směr filtru na vztahy mezi **prodej** a **čas**, **prodej** a **položky**, a **položky** a **čas** tabulky se nebudou filtrovat dolů.</span><span class="sxs-lookup"><span data-stu-id="727d0-138">However, because of the filter direction on the relationships between **Sales** and **Time**, **Sales** and **Item**, and **Item** and **Time** tables will not be filtered down.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

<span data-ttu-id="727d0-139">Které mohou být ok pro tento požadavek, ale pokud jsme nechcete, aby správce, kteří mají zobrazovat položky, pro které nemají žádné prodeje, jsme může zapněte obousměrné křížové filtrování pro relaci a toku zabezpečení filtr v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="727d0-139">That may be ok for this requirement, however, if we don’t want managers to see items for which they don’t have any sales, we could turn on bidirectional cross-filtering for the relationship and flow the security filter in both directions.</span></span> <span data-ttu-id="727d0-140">To lze provést tak, že upravíte vztah mezi **prodej** a **položky**, podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="727d0-140">This can be done by editing the relationship between **Sales** and **Item**, like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

<span data-ttu-id="727d0-141">Teď, může také toku filtry z tabulky prodeje pro **položky** tabulky:</span><span class="sxs-lookup"><span data-stu-id="727d0-141">Now, filters can also flow from the Sales table to the **Item** table:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> <span data-ttu-id="727d0-142">Pokud používáte režim DirectQuery pro data, musíte povolit obousměrné křížové filtrování výběrem tyto dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="727d0-142">If you're using DirectQuery mode for your data, you will need to enable bidirectional-cross filtering by selecting these two options:</span></span>

1. <span data-ttu-id="727d0-143">**Soubor** -> **možnosti a nastavení** -> **funkce verze Preview** -> **povolit křížové filtrování v obou směrech DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="727d0-143">**File** -> **Options and Settings** -> **Preview Features** -> **Enable cross filtering in both directions for DirectQuery**.</span></span>
2. <span data-ttu-id="727d0-144">**Soubor** -> **možnosti a nastavení** -> **DirectQuery** -> **Povolit neomezené míry v režimu DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="727d0-144">**File** -> **Options and Settings** -> **DirectQuery** -> **Allow unrestricted measure in DirectQuery mode**.</span></span>

<span data-ttu-id="727d0-145">Další informace o obousměrné křížové filtrování, stáhněte si [obousměrné křížové filtrování v SQL Server Analysis Services 2016 a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) dokument White Paper.</span><span class="sxs-lookup"><span data-stu-id="727d0-145">To learn more about bidirectional cross-filtering, download the [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) whitepaper.</span></span>

<span data-ttu-id="727d0-146">Tím se zabalí do všech práci, kterou je třeba provést v Power BI Desktop, ale je, že jeden další část práce, která je potřeba zajistit, aby RLS pravidel, že jsme definovali pracovní v Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="727d0-146">This wraps up all the work that needs to be done in Power BI Desktop, but there’s one more piece of work that needs to be done to make the RLS rules we defined work in Power BI Embedded.</span></span> <span data-ttu-id="727d0-147">Uživatelé se ověří a autorizuje vaše aplikace a tokeny aplikací slouží k udělení tohoto přístupu uživatelů k konkrétní sestavy Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="727d0-147">Users are authenticated and authorized by your application and App tokens are used to grant that user access to a specific Power BI Embedded report.</span></span> <span data-ttu-id="727d0-148">Power BI Embedded nemá žádné konkrétní informace, na který je vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="727d0-148">Power BI Embedded doesn’t have any specific information on who your user is.</span></span> <span data-ttu-id="727d0-149">Pro RLS pro práci budete muset předat některé další kontext jako součást tokenu vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="727d0-149">For RLS to work, you’ll need to pass some additional context as part of your app token:</span></span>

* <span data-ttu-id="727d0-150">**uživatelské jméno** (volitelné) – používá se s RLS Toto je řetězec, který slouží k identifikaci uživatele při použití pravidla RLS.</span><span class="sxs-lookup"><span data-stu-id="727d0-150">**username** (optional) – Used with RLS this is a string that can be used to help identify the user when applying RLS rules.</span></span> <span data-ttu-id="727d0-151">V části používání řádek zabezpečení na úrovni s Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="727d0-151">See Using Row Level Security with Power BI Embedded</span></span>
* <span data-ttu-id="727d0-152">**role** – řetězec obsahující rolí vyberte při použití pravidla zabezpečení na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="727d0-152">**roles** – A string containing the roles to select when applying Row Level Security rules.</span></span> <span data-ttu-id="727d0-153">Pokud předávání víc než jedné role, musí být předán jako pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="727d0-153">If passing more than one role, they should be passed as a string array.</span></span>

<span data-ttu-id="727d0-154">Vytvoření tohoto tokenu pomocí [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metoda.</span><span class="sxs-lookup"><span data-stu-id="727d0-154">You create the token by using the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) method.</span></span> <span data-ttu-id="727d0-155">Pokud se vlastnost username nachází, je třeba alespoň jednu hodnotu předat také v rolích.</span><span class="sxs-lookup"><span data-stu-id="727d0-155">If the username property is present, you must also pass at least one value in roles.</span></span>

<span data-ttu-id="727d0-156">Například můžete změnit EmbedSample.</span><span class="sxs-lookup"><span data-stu-id="727d0-156">For example, you could change the EmbedSample.</span></span> <span data-ttu-id="727d0-157">Mohlo dojít k aktualizaci řádku DashboardController 55 z</span><span class="sxs-lookup"><span data-stu-id="727d0-157">DashboardController line 55 could be updated from</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

<span data-ttu-id="727d0-158">na</span><span class="sxs-lookup"><span data-stu-id="727d0-158">to</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

<span data-ttu-id="727d0-159">Tokenu úplné aplikace bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="727d0-159">The full app token will look something like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

<span data-ttu-id="727d0-160">Nyní s všechny části společně, při přihlášení do aplikace pro zobrazení této sestavy pouze budou moci zobrazit data, která mohou zobrazit, jak jsou definovány pomocí našich zabezpečení na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="727d0-160">Now, with all the pieces together, when someone logs into our application to view this report, they’ll only be able to see the data that they are allowed to see, as defined by our row-level security.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a><span data-ttu-id="727d0-161">Viz také</span><span class="sxs-lookup"><span data-stu-id="727d0-161">See also</span></span>

[<span data-ttu-id="727d0-162">Zabezpečení na úrovni řádků (RLS) s výkonem</span><span class="sxs-lookup"><span data-stu-id="727d0-162">Row-level security (RLS) with Power</span></span>](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[<span data-ttu-id="727d0-163">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="727d0-163">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="727d0-164">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="727d0-164">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="727d0-165">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="727d0-165">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="727d0-166">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="727d0-166">More questions?</span></span> [<span data-ttu-id="727d0-167">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="727d0-167">Try the Power BI Community</span></span>](http://community.powerbi.com/)

