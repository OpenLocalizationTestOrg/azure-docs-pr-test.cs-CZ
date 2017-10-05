---
title: "Rychlý úvodní kurz pro Azure AD Graph API | Microsoft Docs"
description: "Azure Active Directory Graph API zajišťují programový přístup ke službě Azure AD prostřednictvím koncové body OData REST API. Aplikace můžete použít rozhraní Graph API k provedení vytvářet, číst, aktualizovat a odstraňovat operace na data adresáře a objekty."
services: active-directory
documentationcenter: n/a
author: viv-liu
manager: mbaldwin
editor: 
tags: 
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: fad5c315a247673b7a2ad52b4a78b49c567a997a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quickstart-for-the-azure-ad-graph-api"></a><span data-ttu-id="1410d-104">Rychlý úvodní kurz pro Azure AD Graph API</span><span class="sxs-lookup"><span data-stu-id="1410d-104">Quickstart for the Azure AD Graph API</span></span>
<span data-ttu-id="1410d-105">Rozhraní Graph API Azure Active Directory (AD) zajišťují programový přístup ke službě Azure AD prostřednictvím koncové body OData REST API.</span><span class="sxs-lookup"><span data-stu-id="1410d-105">The Azure Active Directory (AD) Graph API provides programmatic access to Azure AD through OData REST API endpoints.</span></span> <span data-ttu-id="1410d-106">Aplikace můžete použít rozhraní Graph API k provedení vytvářet, číst, aktualizovat a odstraňovat operace na data adresáře a objekty.</span><span class="sxs-lookup"><span data-stu-id="1410d-106">Applications can use the Graph API to perform create, read, update, and delete (CRUD) operations on directory data and objects.</span></span> <span data-ttu-id="1410d-107">Například můžete použít rozhraní Graph API k vytvoření nového uživatele, zobrazit nebo aktualizovat vlastnosti uživatele, změnit heslo uživatele, zkontrolovat členství ve skupinách pro přístup na základě rolí, zakázat nebo odstranit uživatele.</span><span class="sxs-lookup"><span data-stu-id="1410d-107">For example, you can use the Graph API to create a new user, view or update user’s properties, change user’s password, check group membership for role-based access, disable, or delete the user.</span></span> <span data-ttu-id="1410d-108">Další informace o funkcích rozhraní Graph API a scénáře aplikací najdete v tématu [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) a [Azure AD Graph API požadavky](https://msdn.microsoft.com/library/hh974476.aspx).</span><span class="sxs-lookup"><span data-stu-id="1410d-108">For more information on the Graph API features and application scenarios, see [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) and [Azure AD Graph API Prerequisites](https://msdn.microsoft.com/library/hh974476.aspx).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="1410d-109">Důrazně doporučujeme pro přístup k prostředkům Azure Active Directory použít [Microsoft Graph](https://developer.microsoft.com/graph) místo Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="1410d-109">We strongly recommend that you use [Microsoft Graph](https://developer.microsoft.com/graph) instead of Azure AD Graph API to access Azure Active Directory resources.</span></span> <span data-ttu-id="1410d-110">Náš vývojový program se nyní soustředí na Microsoft Graph a pro Azure AD Graph API nejsou plánovaná žádná další vylepšení.</span><span class="sxs-lookup"><span data-stu-id="1410d-110">Our development efforts are now concentrated on Microsoft Graph and no further enhancements are planned for Azure AD Graph API.</span></span> <span data-ttu-id="1410d-111">Existuje velmi omezený počet scénářů, pro které může být Azure AD Graph API stále vhodné. Další informace najdete v příspěvku [Microsoft Graph nebo Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blogu na webu Office Dev Center.</span><span class="sxs-lookup"><span data-stu-id="1410d-111">There are a very limited number of scenarios for which Azure AD Graph API might still be appropriate; for more information, see the [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog post in the Office Dev Center.</span></span>
> 
> 

## <a name="how-to-construct-a-graph-api-url"></a><span data-ttu-id="1410d-112">Jak vytvořit adresu URL rozhraní API grafu</span><span class="sxs-lookup"><span data-stu-id="1410d-112">How to construct a Graph API URL</span></span>
<span data-ttu-id="1410d-113">V rozhraní Graph API přístup k datům adresáře a objekty (jinými slovy, prostředky nebo entity), u kterých chcete provádět operace CRUD, můžete použít adresy URL založené na protokolu (OData Open Data).</span><span class="sxs-lookup"><span data-stu-id="1410d-113">In Graph API, to access directory data and objects (in other words, resources or entities) against which you want to perform CRUD operations, you can use URLs based on the Open Data (OData) Protocol.</span></span> <span data-ttu-id="1410d-114">Adresy URL používá v rozhraní Graph API obsahovat čtyři hlavní části: služby root, identifikátor klienta, cesta prostředku a možnosti řetězec dotazu: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`.</span><span class="sxs-lookup"><span data-stu-id="1410d-114">The URLs used in Graph API consist of four main parts: service root, tenant identifier, resource path, and query string options: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`.</span></span> <span data-ttu-id="1410d-115">Provést jako příklad následující adresu URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="1410d-115">Take the example of the following URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.</span></span>

* <span data-ttu-id="1410d-116">**Služba kořenové**: V Azure AD Graph API, vždycky je kořenový adresář https://graph.windows.net.</span><span class="sxs-lookup"><span data-stu-id="1410d-116">**Service Root**: In Azure AD Graph API, the service root is always https://graph.windows.net.</span></span>
* <span data-ttu-id="1410d-117">**Identifikátor klienta**: v této části může být název ověřené domény (registrovaný), v předchozím příkladu contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1410d-117">**Tenant identifier**: This section can be a verified (registered) domain name, in the preceding example, contoso.com.</span></span> <span data-ttu-id="1410d-118">Lze ji ID objektu klienta nebo "TatoOrganizace" nebo "ME." alias.</span><span class="sxs-lookup"><span data-stu-id="1410d-118">It can also be a tenant object ID or the “myorganization” or “me” alias.</span></span> <span data-ttu-id="1410d-119">Další informace najdete v tématu [adresování entity a operace v rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).</span><span class="sxs-lookup"><span data-stu-id="1410d-119">For more information, see [Addressing Entities and Operations in the Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).</span></span>
* <span data-ttu-id="1410d-120">**Cesta prostředku**: Tato část adresy URL identifikuje prostředek na serveru (uživatelé, skupiny, určitého uživatele, nebo konkrétní skupiny atd.) V předchozím příkladu je nejvyšší úrovně "skupiny" adresu, která prostředek nastaven.</span><span class="sxs-lookup"><span data-stu-id="1410d-120">**Resource path**: This section of a URL identifies the resource to be interacted with (users, groups, a particular user, or a particular group, etc.) In the example above, it is the top level “groups” to address that resource set.</span></span> <span data-ttu-id="1410d-121">Můžete také vyřešit konkrétní entitu, například "uživatelé / {objectId}" nebo "uživatelé nebo userPrincipalName".</span><span class="sxs-lookup"><span data-stu-id="1410d-121">You can also address a specific entity, for example “users/{objectId}” or “users/userPrincipalName”.</span></span>
* <span data-ttu-id="1410d-122">**Parametrů dotazu**: otazník (?) odděluje část cesty prostředku z části parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="1410d-122">**Query parameters**: A question mark (?) separates the resource path section from the query parameters section.</span></span> <span data-ttu-id="1410d-123">Parametr dotazu "api-version" je požadován u všech požadavků v rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="1410d-123">The “api-version” query parameter is required on all requests in the Graph API.</span></span> <span data-ttu-id="1410d-124">Rozhraní Graph API také podporuje následující možnosti dotazu OData: **$filter**, **$orderby**, **$expand**, **$top**, a **$ Formát**.</span><span class="sxs-lookup"><span data-stu-id="1410d-124">The Graph API also supports the following OData query options: **$filter**, **$orderby**, **$expand**, **$top**, and **$format**.</span></span> <span data-ttu-id="1410d-125">Následující možnosti dotazu nejsou aktuálně podporovány: **$count**, **$inlinecount**, a **$skip**.</span><span class="sxs-lookup"><span data-stu-id="1410d-125">The following query options are not currently supported: **$count**, **$inlinecount**, and **$skip**.</span></span> <span data-ttu-id="1410d-126">Další informace najdete v tématu [podporované dotazy, filtrů a možností stránkování v Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).</span><span class="sxs-lookup"><span data-stu-id="1410d-126">For more information, see [Supported Queries, Filters, and Paging Options in Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).</span></span>

## <a name="graph-api-versions"></a><span data-ttu-id="1410d-127">Verze rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="1410d-127">Graph API versions</span></span>
<span data-ttu-id="1410d-128">Verze pro žádost o rozhraní Graph API určíte v parametru dotazu "api-version".</span><span class="sxs-lookup"><span data-stu-id="1410d-128">You specify the version for a Graph API request in the “api-version” query parameter.</span></span> <span data-ttu-id="1410d-129">Pro verze 1.5 a novější použijte hodnotu numerické verze; rozhraní API-version = 1.6.</span><span class="sxs-lookup"><span data-stu-id="1410d-129">For version 1.5 and later, you use a numerical version value; api-version=1.6.</span></span> <span data-ttu-id="1410d-130">U starších verzí použijte datum řetězec, který dodržuje formát rrrr-MM-DD; například rozhraní api-version = 2013. 11 08.</span><span class="sxs-lookup"><span data-stu-id="1410d-130">For earlier versions, you use a date string that adheres to the format YYYY-MM-DD; for example, api-version=2013-11-08.</span></span> <span data-ttu-id="1410d-131">Pro funkce verze preview použijte řetězec "beta"; například rozhraní api-version = beta.</span><span class="sxs-lookup"><span data-stu-id="1410d-131">For preview features, use the string “beta”; for example, api-version=beta.</span></span> <span data-ttu-id="1410d-132">Další informace o rozdílech mezi verzemi rozhraní Graph API najdete v tématu [Správa verzí Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).</span><span class="sxs-lookup"><span data-stu-id="1410d-132">For more information about differences between Graph API versions, see [Azure AD Graph API Versioning](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).</span></span>

## <a name="graph-api-metadata"></a><span data-ttu-id="1410d-133">Graf metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1410d-133">Graph API metadata</span></span>
<span data-ttu-id="1410d-134">Vrátí soubor metadat rozhraní Graph API, přidejte segment "$metadata" po identifikátor klienta v adrese URL pro příkladu, následující adresu URL vrátí metadata pro ukázkové společnosti: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="1410d-134">To return the Graph API metadata file, add the “$metadata” segment after the tenant-identifier in the URL For example, the following URL returns metadata for a demo company: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`.</span></span> <span data-ttu-id="1410d-135">Můžete zadat tuto adresu URL do adresního řádku webového prohlížeče zobrazíte metadata.</span><span class="sxs-lookup"><span data-stu-id="1410d-135">You can enter this URL in the address bar of a web browser to see the metadata.</span></span> <span data-ttu-id="1410d-136">Dokument metadat CSDL vrátil popisuje entity a komplexní typy, jejich vlastnosti a funkce a vystavené verzi rozhraní Graph API požadovaná akce.</span><span class="sxs-lookup"><span data-stu-id="1410d-136">The CSDL metadata document returned describes the entities and complex types, their properties, and the functions and actions exposed by the version of Graph API you requested.</span></span> <span data-ttu-id="1410d-137">Vynechání parametru verze rozhraní api vrátí metadata pro nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="1410d-137">Omitting the api-version parameter returns metadata for the most recent version.</span></span>

## <a name="common-queries"></a><span data-ttu-id="1410d-138">Běžné dotazy</span><span class="sxs-lookup"><span data-stu-id="1410d-138">Common queries</span></span>
<span data-ttu-id="1410d-139">[Azure AD Graph API běžné dotazy](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) jsou uvedeny běžné dotazy, které lze použít s Azure AD Graph, včetně dotazy, které lze použít pro přístup k prostředkům nejvyšší úrovně ve vašem adresáři a k provádění operací ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="1410d-139">[Azure AD Graph API Common Queries](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) lists common queries that can be used with the Azure AD Graph, including queries that can be used to access top-level resources in your directory and queries to perform operations in your directory.</span></span>

<span data-ttu-id="1410d-140">Například `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` vrátí společnosti informace pro adresář contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1410d-140">For example, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` returns company information for directory contoso.com.</span></span>

<span data-ttu-id="1410d-141">Nebo `https://graph.windows.net/contoso.com/users?api-version=1.6` jsou uvedené všechny uživatelské objekty v adresáři contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1410d-141">Or `https://graph.windows.net/contoso.com/users?api-version=1.6` lists all user objects in the directory contoso.com.</span></span>

## <a name="using-the-graph-explorer"></a><span data-ttu-id="1410d-142">Pomocí Průzkumníka grafu</span><span class="sxs-lookup"><span data-stu-id="1410d-142">Using the Graph Explorer</span></span>
<span data-ttu-id="1410d-143">K dotazování dat adresáře, jak sestavit aplikaci, můžete pomocí Průzkumníka grafu pro Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="1410d-143">You can use the Graph Explorer for the Azure AD Graph API to query the directory data as you build your application.</span></span>

<span data-ttu-id="1410d-144">Výstupu, zobrazí se, pokud byste chtěli přejděte do Průzkumníka grafu, přihlaste se a zadejte `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` zobrazíte všechny uživatele v adresáři přihlášeného uživatele:</span><span class="sxs-lookup"><span data-stu-id="1410d-144">The following is the output you would see if you were to navigate to the Graph Explorer, sign in, and enter `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` to display all the users in the signed-in user's directory:</span></span>

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

<span data-ttu-id="1410d-146">**Načíst graf Explorer**: Chcete-li načíst nástroj, přejděte na [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="1410d-146">**Load the Graph Explorer**: To load the tool, navigate to [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span></span> <span data-ttu-id="1410d-147">Klikněte na tlačítko **přihlášení** a přihlaste se pomocí svých přihlašovacích údajů účtu Azure AD a spusťte Průzkumníka grafu vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="1410d-147">Click **Login** and sign-in with your Azure AD account credentials to run the Graph Explorer against your tenant.</span></span> <span data-ttu-id="1410d-148">Pokud spustíte Průzkumníka grafu proti vlastního klienta, vy nebo váš správce musí vyjádřit souhlas. během přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1410d-148">If you run Graph Explorer against your own tenant, either you or your administrator needs to consent during sign-in.</span></span> <span data-ttu-id="1410d-149">Pokud máte předplatné služeb Office 365, máte automaticky klient služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1410d-149">If you have an Office 365 subscription, you automatically have an Azure AD tenant.</span></span> <span data-ttu-id="1410d-150">Přihlašovací údaje, které používáte k přihlášení do služeb Office 365 jsou ve skutečnosti účty Azure AD, a můžete použít tyto přihlašovací údaje pomocí Průzkumníka grafu.</span><span class="sxs-lookup"><span data-stu-id="1410d-150">The credentials you use to sign in to Office 365 are, in fact, Azure AD accounts, and you can use these credentials with Graph Explorer.</span></span>

<span data-ttu-id="1410d-151">**Spuštění dotazu**: ke spuštění dotazu, zadejte dotaz do textového pole žádost a klikněte na tlačítko **získat** nebo klikněte na tlačítko **zadejte** klíč.</span><span class="sxs-lookup"><span data-stu-id="1410d-151">**Run a query**: To run a query, type your query in the request text box and click **GET** or click the **enter** key.</span></span> <span data-ttu-id="1410d-152">Výsledky jsou zobrazeny v poli odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1410d-152">The results are displayed in the response box.</span></span> <span data-ttu-id="1410d-153">Například `https://graph.windows.net/myorganization/groups?api-version=1.6` zobrazí seznam všech objektů skupiny v adresáři přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="1410d-153">For example, `https://graph.windows.net/myorganization/groups?api-version=1.6` lists all group objects in the signed-in user's directory.</span></span>

<span data-ttu-id="1410d-154">Poznámka: následující vlastnosti a omezení Průzkumníku grafu:</span><span class="sxs-lookup"><span data-stu-id="1410d-154">Note the following features and limitations of the Graph Explorer:</span></span>

* <span data-ttu-id="1410d-155">Nastaví možnost automatického dokončování na prostředek.</span><span class="sxs-lookup"><span data-stu-id="1410d-155">Autocomplete capability on resource sets.</span></span> <span data-ttu-id="1410d-156">Pokud chcete zobrazit tuto funkci, klikněte na textového pole požadavku (kde adresa URL společnosti se zobrazí).</span><span class="sxs-lookup"><span data-stu-id="1410d-156">To see this functionality, click on the request text box (where the company URL appears).</span></span> <span data-ttu-id="1410d-157">Můžete vybrat prostředek nastavit z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1410d-157">You can select a resource set from the dropdown list.</span></span>
* <span data-ttu-id="1410d-158">Podporuje "mě" a "TatoOrganizace" adresování aliasy.</span><span class="sxs-lookup"><span data-stu-id="1410d-158">Supports the “me” and “myorganization” addressing aliases.</span></span> <span data-ttu-id="1410d-159">Například můžete použít `https://graph.windows.net/me?api-version=1.6` vrátit objekt uživatele přihlášeného uživatele nebo `https://graph.windows.net/myorganization/users?api-version=1.6` vrátit všechny uživatele v aktuálním adresáři.</span><span class="sxs-lookup"><span data-stu-id="1410d-159">For example, you can use `https://graph.windows.net/me?api-version=1.6` to return the user object of the signed-in user or `https://graph.windows.net/myorganization/users?api-version=1.6` to return all users in the current directory.</span></span>
* <span data-ttu-id="1410d-160">Oddíl hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1410d-160">A response headers section.</span></span> <span data-ttu-id="1410d-161">V této části můžete použít k řešení potíží, ke kterým dochází při spuštění dotazů.</span><span class="sxs-lookup"><span data-stu-id="1410d-161">This section can be used to help troubleshoot issues that occur when running queries.</span></span>
* <span data-ttu-id="1410d-162">Prohlížečem JSON pro odpověď s rozbalení a sbalení možnosti.</span><span class="sxs-lookup"><span data-stu-id="1410d-162">A JSON viewer for the response with expand and collapse capabilities.</span></span>
* <span data-ttu-id="1410d-163">Žádná podpora pro zobrazení miniaturu fotografie.</span><span class="sxs-lookup"><span data-stu-id="1410d-163">No support for displaying a thumbnail photo.</span></span>

## <a name="using-fiddler-to-write-to-the-directory"></a><span data-ttu-id="1410d-164">Pomocí Fiddler k zápisu do adresáře</span><span class="sxs-lookup"><span data-stu-id="1410d-164">Using Fiddler to write to the directory</span></span>
<span data-ttu-id="1410d-165">Pro účely tohoto průvodce rychlý start můžete použít aplikaci Fiddler ladicí program Web na postup provádění zápisu operace u svého adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1410d-165">For the purposes of this Quickstart guide, you can use the Fiddler Web Debugger to practice performing ‘write’ operations against your Azure AD directory.</span></span> <span data-ttu-id="1410d-166">Další informace a nainstalovat aplikaci Fiddler, najdete v části [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="1410d-166">For more information and to install Fiddler, see [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).</span></span>

<span data-ttu-id="1410d-167">V následujícím příkladu použijete webovou aplikaci Fiddler ladicí program vytvořit novou skupinu zabezpečení, MyTestGroup' v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1410d-167">In the example below, you use Fiddler Web Debugger to create a new security group ‘MyTestGroup’ in your Azure AD directory.</span></span>

<span data-ttu-id="1410d-168">**Získat přístupový token**: přístup k Azure AD Graph, jsou klienti vyžadovaný k úspěšně nejprve ověření Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1410d-168">**Obtain an access token**: To access Azure AD Graph, clients are required to successfully authenticate to Azure AD first.</span></span> <span data-ttu-id="1410d-169">Další informace najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="1410d-169">For more information, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).</span></span>

<span data-ttu-id="1410d-170">**Napište a spuštění dotazu**: pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1410d-170">**Compose and run a query**: Complete the following steps:</span></span>

1. <span data-ttu-id="1410d-171">Otevřete webovou aplikaci Fiddler ladicího programu a přepnout **autora** kartě.</span><span class="sxs-lookup"><span data-stu-id="1410d-171">Open Fiddler Web Debugger and switch to the **Composer** tab.</span></span>
2. <span data-ttu-id="1410d-172">Vzhledem k tomu, že chcete vytvořit novou skupinu zabezpečení, vyberte **Post** jako metodu protokolu HTTP v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="1410d-172">Since you want to create a new security group, select **Post** as the HTTP method from the pull-down menu.</span></span> <span data-ttu-id="1410d-173">Další informace o operacích a oprávnění na objekt skupiny najdete v tématu [skupiny](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) v rámci [Azure AD Graph REST API – referenční informace](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).</span><span class="sxs-lookup"><span data-stu-id="1410d-173">For more information about operations and permissions on a group object, see [Group](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) within the [Azure AD Graph REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).</span></span>
3. <span data-ttu-id="1410d-174">V poli vedle **Post**, zadejte následující kroky jako adresu URL požadavku: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="1410d-174">In the field next to **Post**, type in the following as the request URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1410d-175">Je třeba nahradit mytenantdomain s názvem domény adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1410d-175">You must substitute mytenantdomain with the domain name of your own Azure AD directory.</span></span>
   > 
   > 
4. <span data-ttu-id="1410d-176">Do pole přímo pod rozevírací Post zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1410d-176">In the field directly below Post pull-down, type the following:</span></span>
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > <span data-ttu-id="1410d-177">SUBSTITUTE vaše &lt;přístupový token&gt; s tímto tokenem přístupu pro váš adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1410d-177">Substitute your &lt;your access token&gt; with the access token for your Azure AD directory.</span></span>
   > 
   > 
5. <span data-ttu-id="1410d-178">V **text žádosti** pole, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1410d-178">In the **Request body** field, type the following:</span></span>
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    <span data-ttu-id="1410d-179">Další informace o vytváření skupin najdete v tématu [vytvořit skupinu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).</span><span class="sxs-lookup"><span data-stu-id="1410d-179">For more information about creating groups, see [Create Group](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).</span></span>

<span data-ttu-id="1410d-180">Další informace o Azure AD entity a typy, které jsou vystavené grafu a informace o operacích, které lze provést na nich grafu naleznete v tématu [Azure AD Graph REST API – referenční informace](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).</span><span class="sxs-lookup"><span data-stu-id="1410d-180">For more information on Azure AD entities and types that are exposed by Graph and information about the operations that can be performed on them with Graph, see [Azure AD Graph REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1410d-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1410d-181">Next steps</span></span>
* <span data-ttu-id="1410d-182">Další informace o [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)</span><span class="sxs-lookup"><span data-stu-id="1410d-182">Learn more about the [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)</span></span>
* <span data-ttu-id="1410d-183">Další informace o [Azure AD Graph API oprávnění oborů](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)</span><span class="sxs-lookup"><span data-stu-id="1410d-183">Learn more about [Azure AD Graph API Permission Scopes](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)</span></span>
