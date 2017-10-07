---
title: "zabezpečení na úrovni aaaRow s Power BI Embedded"
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
ms.openlocfilehash: 384f78826ecc710cf8f101b251ae68b074f3e98b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a><span data-ttu-id="fbdc2-103">Zabezpečení na úrovni řádků v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="fbdc2-103">Row level security with Power BI Embedded</span></span>

<span data-ttu-id="fbdc2-104">Zabezpečení na úrovni řádků (RLS) lze použít toorestrict uživatel přístup tooparticular data v rámci sestavu nebo datovou sadu, které umožní více různým uživatelům toouse hello stejné sestavě při všechny vidět různá data.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-104">Row level security (RLS) can be used toorestrict user access tooparticular data within a report or dataset, allowing for multiple different users toouse hello same report while all seeing different data.</span></span> <span data-ttu-id="fbdc2-105">Power BI Embedded teď podporuje datové sady, které jsou nakonfigurované s RLS.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-105">Power BI Embedded now supports datasets configured with RLS.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

<span data-ttu-id="fbdc2-106">V pořadí tootake výhod RLS je důležité, že rozumíte tři hlavní koncepty; Uživatelé, role a pravidla.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-106">In order tootake advantage of RLS, it’s important you understand three main concepts; Users, Roles, and Rules.</span></span> <span data-ttu-id="fbdc2-107">Podívejme bližší pohled na každý:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-107">Let’s take a closer look at each:</span></span>

<span data-ttu-id="fbdc2-108">**Uživatelé** – tyto prohlížíte skutečné koncoví uživatelé hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-108">**Users** –  These are hello actual end-users viewing reports.</span></span> <span data-ttu-id="fbdc2-109">V Power BI Embedded uživatelé se identifikují podle vlastnosti hello uživatelské jméno v tokenu aplikace.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-109">In Power BI Embedded, users are identified by hello username property in an App Token.</span></span>

<span data-ttu-id="fbdc2-110">**Role** – uživatelé patří tooroles.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-110">**Roles** – Users belong tooroles.</span></span> <span data-ttu-id="fbdc2-111">Role je kontejner pro pravidla a může mít název něco jako "Vedoucí prodeje" nebo "Obchodního zástupce".</span><span class="sxs-lookup"><span data-stu-id="fbdc2-111">A role is a container for rules and can be named something like “Sales Manager” or “Sales Rep”.</span></span> <span data-ttu-id="fbdc2-112">V Power BI Embedded uživatelé se identifikují podle vlastnosti rolí hello v tokenu aplikace.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-112">In Power BI Embedded, users are identified by hello roles property in an App Token.</span></span>

<span data-ttu-id="fbdc2-113">**Pravidla** – role mají pravidla a tato pravidla jsou hello skutečné filtry, které budou data toohello toobe použít.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-113">**Rules** – Roles have rules, and those rules are hello actual filters that are going toobe applied toohello data.</span></span> <span data-ttu-id="fbdc2-114">To může být stejně jednoduché jako "země = USA" nebo něco víc dynamické.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-114">This could be as simple as “Country = USA” or something much more dynamic.</span></span>

### <a name="example"></a><span data-ttu-id="fbdc2-115">Příklad</span><span class="sxs-lookup"><span data-stu-id="fbdc2-115">Example</span></span>

<span data-ttu-id="fbdc2-116">Pro hello zbývající části tohoto článku poskytujeme příklad vytváření RLS a využívání, v rámci aplikace embedded.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-116">For hello rest of this article, we’ll provide an example of authoring RLS, and then consuming that within an embedded application.</span></span> <span data-ttu-id="fbdc2-117">Naše Ukázka používá hello [prodejní analýzy ukázka](http://go.microsoft.com/fwlink/?LinkID=780547) soubor PBIX.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-117">Our example uses hello [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX file.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

<span data-ttu-id="fbdc2-118">Naše ukázka prodejní analýzy prodejů pro všechny hello úložiště v určité maloobchodní řetězec.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-118">Our Retail Analysis sample shows sales for all hello stores in a particular retail chain.</span></span> <span data-ttu-id="fbdc2-119">Bez RLS bez ohledu na to, které oblastní správce přihlásí a zobrazení hello sestavy, se zobrazí hello stejná data.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-119">Without RLS, no matter which district manager signs in and views hello report, they’ll see hello same data.</span></span> <span data-ttu-id="fbdc2-120">Vedoucí pracovníci bylo zjištěno, že každý správce oblastní byste měli vidět jenom hello prodeje pro hello úložiště, které spravují a toodo to, můžeme použít RLS.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-120">Senior management has determined each district manager should only see hello sales for hello stores they manage, and toodo this, we can use RLS.</span></span>

<span data-ttu-id="fbdc2-121">RLS je vytvořené v Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-121">RLS is authored in Power BI Desktop.</span></span> <span data-ttu-id="fbdc2-122">Po otevření hello datovou sadu a sestavu jsme přepínači toodiagram zobrazení toosee hello schématu:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-122">When hello dataset and report are opened, we can switch toodiagram view toosee hello schema:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

<span data-ttu-id="fbdc2-123">Tady je několik věcí toonotice s toto schéma:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-123">Here are a few things toonotice with this schema:</span></span>

* <span data-ttu-id="fbdc2-124">Všechny míry, jako například **celkový prodej**, jsou uloženy v hello **prodej** tabulky faktů.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-124">All measures, like **Total Sales**, are stored in hello **Sales** fact table.</span></span>
* <span data-ttu-id="fbdc2-125">Existují čtyři další dimenze související tabulky: **položky**, **čas**, **úložiště**, a **oblastní**.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-125">There are four additional related dimension tables: **Item**, **Time**, **Store**, and **District**.</span></span>
* <span data-ttu-id="fbdc2-126">šipky Hello na řádky vztah hello označují toho, jak filtry můžete toku z jedné tabulky tooanother.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-126">hello arrows on hello relationship lines indicate which way filters can flow from one table tooanother.</span></span> <span data-ttu-id="fbdc2-127">Například, pokud filtr je umístěn na **čas [datum]**, v aktuálním schématu hello bude pouze filtrovat dolů hodnoty v hello **prodej** tabulky.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-127">For example, if a filter is placed on **Time[Date]**, in hello current schema it would only filter down values in hello **Sales** table.</span></span> <span data-ttu-id="fbdc2-128">Žádné jiné tabulky by ovlivnila tento filtr od všech hello šipek hello vztah řádky bodu toohello prodeje tabulky a není rychle.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-128">No other tables would be affected by this filter since all of hello arrows on hello relationship lines point toohello sales table and not away.</span></span>
* <span data-ttu-id="fbdc2-129">Hello **oblastní** tabulka udává, který správce hello je pro každou oblast:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-129">hello **District** table indicates who hello manager is for each district:</span></span>
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

<span data-ttu-id="fbdc2-130">Založené na tomto schématu v případě, že jsme použít filtr toohello **Manager oblastní** sloupec v hello oblastní tabulky, a pokud tento filtr odpovídá hello uživatele zobrazení hello sestavy, bude tento filtr také filtrovat dolů hello **úložiště**a **prodej** tabulky tooonly zobrazit data pro tento konkrétní oblastní správce.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-130">Based on this schema, if we apply a filter toohello **District Manager** column in hello District table, and if that filter matches hello user viewing hello report, that filter will also filter down hello **Store** and **Sales** tables tooonly show data for that particular district manager.</span></span>

<span data-ttu-id="fbdc2-131">Tady je jak:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-131">Here’s how:</span></span>

1. <span data-ttu-id="fbdc2-132">Na kartě hello modelování, klikněte na tlačítko **spravovat role**.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-132">On hello Modeling tab, click **Manage Roles**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. <span data-ttu-id="fbdc2-133">Vytvoření nové role s názvem **Manager**.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-133">Create a new role called **Manager**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. <span data-ttu-id="fbdc2-134">V hello **oblastní** tabulky zadejte následující výraz jazyka DAX hello: **[oblastní Manager] = USERNAME()**</span><span class="sxs-lookup"><span data-stu-id="fbdc2-134">In hello **District** table enter hello following DAX expression: **[District Manager] = USERNAME()**</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. <span data-ttu-id="fbdc2-135">zda pravidla hello toomake pracují na hello **modelování** , klikněte na **zobrazení jako role**a potom zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-135">toomake sure hello rules are working, on hello **Modeling** tab, click **View as Roles**, and then enter hello following:</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   <span data-ttu-id="fbdc2-136">Hello sestavy se nyní zobrazí data jako kdyby byly přihlášení jako **Andrew Ma**.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-136">hello reports will now show data as if you were signed in as **Andrew Ma**.</span></span>

<span data-ttu-id="fbdc2-137">Použití filtru hello, způsob hello jsme se zde bude filtrovat dolů všechny záznamy v hello **oblastní**, **úložiště**, a **prodej** tabulky.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-137">Applying hello filter, hello way we did here, will filter down all records in hello **District**, **Store**, and **Sales** tables.</span></span> <span data-ttu-id="fbdc2-138">Ale kvůli hello směr filtru na hello vztahy mezi **prodej** a **čas**, **prodej** a **položky**a **Položky** a **čas** tabulky se nebudou filtrovat dolů.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-138">However, because of hello filter direction on hello relationships between **Sales** and **Time**, **Sales** and **Item**, and **Item** and **Time** tables will not be filtered down.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

<span data-ttu-id="fbdc2-139">Které mohou být ok pro tento požadavek, ale pokud jsme nechcete, aby správci toosee položky, pro které nemají žádné prodeje, jsme může zapněte obousměrné křížové filtrování hello vztah a toku hello zabezpečení filtr v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-139">That may be ok for this requirement, however, if we don’t want managers toosee items for which they don’t have any sales, we could turn on bidirectional cross-filtering for hello relationship and flow hello security filter in both directions.</span></span> <span data-ttu-id="fbdc2-140">To lze provést tak, že upravíte hello vztah mezi **prodej** a **položky**, podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-140">This can be done by editing hello relationship between **Sales** and **Item**, like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

<span data-ttu-id="fbdc2-141">Teď, může také toku filtry z hello prodej tabulky toohello **položky** tabulky:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-141">Now, filters can also flow from hello Sales table toohello **Item** table:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> <span data-ttu-id="fbdc2-142">Pokud používáte režim DirectQuery pro data, budete potřebovat tooenable obousměrného křížové filtrování výběrem tyto dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-142">If you're using DirectQuery mode for your data, you will need tooenable bidirectional-cross filtering by selecting these two options:</span></span>

1. <span data-ttu-id="fbdc2-143">**Soubor** -> **možnosti a nastavení** -> **funkce verze Preview** -> **povolit křížové filtrování v obou směrech DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-143">**File** -> **Options and Settings** -> **Preview Features** -> **Enable cross filtering in both directions for DirectQuery**.</span></span>
2. <span data-ttu-id="fbdc2-144">**Soubor** -> **možnosti a nastavení** -> **DirectQuery** -> **Povolit neomezené míry v režimu DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-144">**File** -> **Options and Settings** -> **DirectQuery** -> **Allow unrestricted measure in DirectQuery mode**.</span></span>

<span data-ttu-id="fbdc2-145">Další informace o obousměrné křížové filtrování, stažení hello toolearn [obousměrné křížové filtrování v SQL Server Analysis Services 2016 a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) dokument White Paper.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-145">toolearn more about bidirectional cross-filtering, download hello [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) whitepaper.</span></span>

<span data-ttu-id="fbdc2-146">Tím se zabalí do všech hello práci, kterou toobe v Power BI Desktop, ale existuje jeden další část práce, která potřebuje provést toobe toomake hello RLS pravidla jsme definovali pracovní v Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-146">This wraps up all hello work that needs toobe done in Power BI Desktop, but there’s one more piece of work that needs toobe done toomake hello RLS rules we defined work in Power BI Embedded.</span></span> <span data-ttu-id="fbdc2-147">Uživatelé se ověří a autorizuje vaše aplikace a tokeny aplikací jsou použité toogrant tohoto uživatele tooa konkrétní Power BI Embedded sestavu přístupu.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-147">Users are authenticated and authorized by your application and App tokens are used toogrant that user access tooa specific Power BI Embedded report.</span></span> <span data-ttu-id="fbdc2-148">Power BI Embedded nemá žádné konkrétní informace, na který je vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-148">Power BI Embedded doesn’t have any specific information on who your user is.</span></span> <span data-ttu-id="fbdc2-149">Pro RLS toowork budete potřebovat toopass některé další kontext jako součást tokenu vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-149">For RLS toowork, you’ll need toopass some additional context as part of your app token:</span></span>

* <span data-ttu-id="fbdc2-150">**uživatelské jméno** (volitelné) – používá se s RLS Toto je řetězec, který lze použít toohelp identifikovat hello uživatele při použití pravidla RLS.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-150">**username** (optional) – Used with RLS this is a string that can be used toohelp identify hello user when applying RLS rules.</span></span> <span data-ttu-id="fbdc2-151">V části používání řádek zabezpečení na úrovni s Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="fbdc2-151">See Using Row Level Security with Power BI Embedded</span></span>
* <span data-ttu-id="fbdc2-152">**role** – řetězec obsahující hello role tooselect při použití pravidla zabezpečení na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-152">**roles** – A string containing hello roles tooselect when applying Row Level Security rules.</span></span> <span data-ttu-id="fbdc2-153">Pokud předávání víc než jedné role, musí být předán jako pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-153">If passing more than one role, they should be passed as a string array.</span></span>

<span data-ttu-id="fbdc2-154">Vytvoření tokenu hello pomocí hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metoda.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-154">You create hello token by using hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) method.</span></span> <span data-ttu-id="fbdc2-155">Pokud vlastnost username hello je k dispozici, je třeba alespoň jednu hodnotu předat také v rolích.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-155">If hello username property is present, you must also pass at least one value in roles.</span></span>

<span data-ttu-id="fbdc2-156">Můžete například změnit hello EmbedSample.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-156">For example, you could change hello EmbedSample.</span></span> <span data-ttu-id="fbdc2-157">Mohlo dojít k aktualizaci řádku DashboardController 55 z</span><span class="sxs-lookup"><span data-stu-id="fbdc2-157">DashboardController line 55 could be updated from</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

<span data-ttu-id="fbdc2-158">na</span><span class="sxs-lookup"><span data-stu-id="fbdc2-158">to</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

<span data-ttu-id="fbdc2-159">tokenu úplné aplikace Hello bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="fbdc2-159">hello full app token will look something like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

<span data-ttu-id="fbdc2-160">Nyní se všechny kusy hello společně, při přihlášení do našich tooview aplikace této sestavy pouze budou mít toosee hello data, mohou toosee, podle definice naše zabezpečení na úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="fbdc2-160">Now, with all hello pieces together, when someone logs into our application tooview this report, they’ll only be able toosee hello data that they are allowed toosee, as defined by our row-level security.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a><span data-ttu-id="fbdc2-161">Viz také</span><span class="sxs-lookup"><span data-stu-id="fbdc2-161">See also</span></span>

[<span data-ttu-id="fbdc2-162">Zabezpečení na úrovni řádků (RLS) s výkonem</span><span class="sxs-lookup"><span data-stu-id="fbdc2-162">Row-level security (RLS) with Power</span></span>](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[<span data-ttu-id="fbdc2-163">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="fbdc2-163">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="fbdc2-164">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="fbdc2-164">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="fbdc2-165">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="fbdc2-165">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="fbdc2-166">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="fbdc2-166">More questions?</span></span> [<span data-ttu-id="fbdc2-167">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="fbdc2-167">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

