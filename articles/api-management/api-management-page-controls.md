---
title: "Ovládací prvky stránky Azure API Management | Microsoft Docs"
description: "Další informace o ovládací prvky stránky, která je k dispozici pro použití v šablonách portálu vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1ce0657aebe34d093ae94281de208c929067742a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-page-controls"></a><span data-ttu-id="287d1-103">Ovládací prvky stránky Azure API Management</span><span class="sxs-lookup"><span data-stu-id="287d1-103">Azure API Management page controls</span></span>
<span data-ttu-id="287d1-104">Azure API Management poskytuje následující ovládací prvky pro použití v vývojář portálu šablony.</span><span class="sxs-lookup"><span data-stu-id="287d1-104">Azure API Management provides the following controls for use in the developer portal templates.</span></span>  
  
 <span data-ttu-id="287d1-105">Použití ovládacího prvku, umístěte ho na požadované místo v šabloně portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-105">To use a control, place it in the desired location in the developer portal template.</span></span> <span data-ttu-id="287d1-106">Některé ovládací prvky, jako například [akcí aplikace](#app-actions) řízení, musí mít parametry, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="287d1-106">Some controls, such as the [app-actions](#app-actions) control, have parameters, as shown in the following example.</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 <span data-ttu-id="287d1-107">Hodnoty parametrů jsou předána jako součást datový model pro šablonu.</span><span class="sxs-lookup"><span data-stu-id="287d1-107">The values for the parameters are passed in as part of the data model for the template.</span></span> <span data-ttu-id="287d1-108">Ve většině případů můžete jednoduše vložit zadaný příklad pro každý ovládací prvek pro mohl správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="287d1-108">In most cases, you can simply paste in the provided example for each control for it to work correctly.</span></span> <span data-ttu-id="287d1-109">Další informace o hodnoty parametrů uvidíte oddíl modelu dat ke každé šabloně, ve kterém může použít ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="287d1-109">For more information on the parameter values, you can see the data model section for each template in which a control may be used.</span></span>  
  
 <span data-ttu-id="287d1-110">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="287d1-110">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
## <a name="developer-portal-template-page-controls"></a><span data-ttu-id="287d1-111">Ovládací prvky stránky šablony portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-111">Developer portal template page controls</span></span>  
  
-   [<span data-ttu-id="287d1-112">Akce aplikace</span><span class="sxs-lookup"><span data-stu-id="287d1-112">app-actions</span></span>](#app-actions)  
  
-   [<span data-ttu-id="287d1-113">Basic přihlášení</span><span class="sxs-lookup"><span data-stu-id="287d1-113">basic-signin</span></span>](#basic-signin)  
  
-   [<span data-ttu-id="287d1-114">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="287d1-114">paging-control</span></span>](#paging-control)  
  
-   [<span data-ttu-id="287d1-115">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="287d1-115">providers</span></span>](#providers)  
  
-   [<span data-ttu-id="287d1-116">ovládací prvek vyhledávání</span><span class="sxs-lookup"><span data-stu-id="287d1-116">search-control</span></span>](#search-control)  
  
-   [<span data-ttu-id="287d1-117">registrace</span><span class="sxs-lookup"><span data-stu-id="287d1-117">sign-up</span></span>](#sign-up)  
  
-   [<span data-ttu-id="287d1-118">přihlášení k odběru tlačítko</span><span class="sxs-lookup"><span data-stu-id="287d1-118">subscribe-button</span></span>](#subscribe-button)  
  
-   [<span data-ttu-id="287d1-119">zrušení předplatného</span><span class="sxs-lookup"><span data-stu-id="287d1-119">subscription-cancel</span></span>](#subscription-cancel)  
  
##  <span data-ttu-id="287d1-120"><a name="app-actions"></a>Akce aplikace</span><span class="sxs-lookup"><span data-stu-id="287d1-120"><a name="app-actions"></a> app-actions</span></span>  
 <span data-ttu-id="287d1-121">`app-actions` Řízení poskytuje uživatelské rozhraní pro interakci s aplikacemi na stránce profilu uživatele na portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-121">The `app-actions` control provides a user interface for interacting with applications on the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="287d1-122">![aplikace & č. 45; akce řízení](./media/api-management-page-controls/APIM-app-actions-control.png "APIM akcí aplikace řízení")</span><span class="sxs-lookup"><span data-stu-id="287d1-122">![app&#45;actions control](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app-actions control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-123">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-123">Usage</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-124">Parameters</span></span>  
  
|<span data-ttu-id="287d1-125">Parametr</span><span class="sxs-lookup"><span data-stu-id="287d1-125">Parameter</span></span>|<span data-ttu-id="287d1-126">Popis</span><span class="sxs-lookup"><span data-stu-id="287d1-126">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="287d1-127">appId</span><span class="sxs-lookup"><span data-stu-id="287d1-127">appId</span></span>|<span data-ttu-id="287d1-128">Id aplikace.</span><span class="sxs-lookup"><span data-stu-id="287d1-128">The id of the application.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-129">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-129">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-130">`app-actions` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-130">The `app-actions` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-131">Aplikace</span><span class="sxs-lookup"><span data-stu-id="287d1-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
  
##  <span data-ttu-id="287d1-132"><a name="basic-signin"></a>Basic přihlášení</span><span class="sxs-lookup"><span data-stu-id="287d1-132"><a name="basic-signin"></a> basic-signin</span></span>  
 <span data-ttu-id="287d1-133">`basic-signin` Řízení poskytuje ovládací prvek pro shromažďování informací v přihlašovací stránku v portálu pro vývojáře přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="287d1-133">The `basic-signin` control provides a control for collecting user sign in information in the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="287d1-134">![Basic & č. 45; přihlášení řízení](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic přihlášení řízení")</span><span class="sxs-lookup"><span data-stu-id="287d1-134">![basic&#45;signin control](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-135">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-135">Usage</span></span>  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-136">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-136">Parameters</span></span>  
 <span data-ttu-id="287d1-137">Žádné.</span><span class="sxs-lookup"><span data-stu-id="287d1-137">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-138">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-138">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-139">`basic-signin` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-139">The `basic-signin` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-140">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="287d1-140">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="287d1-141"><a name="paging-control"></a>ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="287d1-141"><a name="paging-control"></a> paging-control</span></span>  
 <span data-ttu-id="287d1-142">`paging-control` Poskytuje funkce stránkování vývojáře portálu stránky zobrazující seznam položek.</span><span class="sxs-lookup"><span data-stu-id="287d1-142">The `paging-control` provides paging functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="287d1-143">![stránkování řízení](./media/api-management-page-controls/APIM-paging-control.png "APIM stránkování ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="287d1-143">![paging control](./media/api-management-page-controls/APIM-paging-control.png "APIM paging control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-144">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-144">Usage</span></span>  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-145">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-145">Parameters</span></span>  
 <span data-ttu-id="287d1-146">Žádné.</span><span class="sxs-lookup"><span data-stu-id="287d1-146">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-147">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-147">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-148">`paging-control` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-148">The `paging-control` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-149">Rozhraní API seznamu</span><span class="sxs-lookup"><span data-stu-id="287d1-149">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="287d1-150">Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="287d1-150">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
  
-   [<span data-ttu-id="287d1-151">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="287d1-151">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="287d1-152"><a name="providers"></a>Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="287d1-152"><a name="providers"></a> providers</span></span>  
 <span data-ttu-id="287d1-153">`providers` Řízení poskytuje ovládací prvek pro výběr zprostředkovatelů ověřování v stránku v portálu pro vývojáře pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="287d1-153">The `providers` control provides a control for selection of authentication providers in the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="287d1-154">![poskytovatelé řízení](./media/api-management-page-controls/APIM-providers-control.png "APIM poskytovatelé řízení")</span><span class="sxs-lookup"><span data-stu-id="287d1-154">![providers control](./media/api-management-page-controls/APIM-providers-control.png "APIM providers control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-155">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-155">Usage</span></span>  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-156">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-156">Parameters</span></span>  
 <span data-ttu-id="287d1-157">Žádné.</span><span class="sxs-lookup"><span data-stu-id="287d1-157">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-158">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-158">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-159">`providers` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-159">The `providers` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-160">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="287d1-160">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="287d1-161"><a name="search-control"></a>ovládací prvek vyhledávání</span><span class="sxs-lookup"><span data-stu-id="287d1-161"><a name="search-control"></a> search-control</span></span>  
 <span data-ttu-id="287d1-162">`search-control` Poskytuje funkce hledání vývojáře portálu stránky zobrazující seznam položek.</span><span class="sxs-lookup"><span data-stu-id="287d1-162">The `search-control` provides search functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="287d1-163">![hledání řízení](./media/api-management-page-controls/APIM-search-control.png "APIM vyhledávání řízení")</span><span class="sxs-lookup"><span data-stu-id="287d1-163">![search control](./media/api-management-page-controls/APIM-search-control.png "APIM search control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-164">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-164">Usage</span></span>  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-165">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-165">Parameters</span></span>  
 <span data-ttu-id="287d1-166">Žádné.</span><span class="sxs-lookup"><span data-stu-id="287d1-166">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-167">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-167">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-168">`search-control` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-168">The `search-control` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-169">Rozhraní API seznamu</span><span class="sxs-lookup"><span data-stu-id="287d1-169">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="287d1-170">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="287d1-170">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="287d1-171"><a name="sign-up"></a>registrace</span><span class="sxs-lookup"><span data-stu-id="287d1-171"><a name="sign-up"></a> sign-up</span></span>  
 <span data-ttu-id="287d1-172">`sign-up` Řízení poskytuje ovládací prvek pro shromažďování informací o profilu uživatele na přihlašovací stránku v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-172">The `sign-up` control provides a control for collecting user profile information in the sign up page in the developer portal.</span></span>  
  
 <span data-ttu-id="287d1-173">![přihlašovací & č. 45; až řízení](./media/api-management-page-controls/APIM-sign-up-control.png "APIM registrace ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="287d1-173">![sign&#45;up control](./media/api-management-page-controls/APIM-sign-up-control.png "APIM sign-up control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-174">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-174">Usage</span></span>  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-175">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-175">Parameters</span></span>  
 <span data-ttu-id="287d1-176">Žádné.</span><span class="sxs-lookup"><span data-stu-id="287d1-176">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-177">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-177">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-178">`sign-up` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-178">The `sign-up` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-179">Registrace</span><span class="sxs-lookup"><span data-stu-id="287d1-179">Sign up</span></span>](api-management-page-templates.md#SignUp)  
  
##  <span data-ttu-id="287d1-180"><a name="subscribe-button"></a>přihlášení k odběru tlačítko</span><span class="sxs-lookup"><span data-stu-id="287d1-180"><a name="subscribe-button"></a> subscribe-button</span></span>  
 <span data-ttu-id="287d1-181">`subscribe-button` Poskytuje ovládací prvek pro přihlášení k odběru uživatele k produktu.</span><span class="sxs-lookup"><span data-stu-id="287d1-181">The `subscribe-button` provides a control for subscribing a user to a product.</span></span>  
  
 <span data-ttu-id="287d1-182">![přihlášení k odběru & č. 45; tlačítko – ovládací prvek](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM přihlášení k odběru tlačítko – ovládací prvek")</span><span class="sxs-lookup"><span data-stu-id="287d1-182">![subscribe&#45;button control](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM subscribe-button control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-183">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-183">Usage</span></span>  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-184">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-184">Parameters</span></span>  
 <span data-ttu-id="287d1-185">Žádné.</span><span class="sxs-lookup"><span data-stu-id="287d1-185">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-186">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-186">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-187">`subscribe-button` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-187">The `subscribe-button` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-188">Produktu</span><span class="sxs-lookup"><span data-stu-id="287d1-188">Product</span></span>](api-management-product-templates.md#Product)  
  
##  <span data-ttu-id="287d1-189"><a name="subscription-cancel"></a>zrušení předplatného</span><span class="sxs-lookup"><span data-stu-id="287d1-189"><a name="subscription-cancel"></a> subscription-cancel</span></span>  
 <span data-ttu-id="287d1-190">`subscription-cancel` Řízení poskytuje ovládací prvek pro zrušení předplatného pro určitý produkt v profilu uživatele na stránce portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-190">The `subscription-cancel` control provides a control for cancelling a subscription to a product in the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="287d1-191">![předplatné & č. 45; zrušit řízení](./media/api-management-page-controls/APIM-subscription-cancel-control.png "řízení APIM zrušit předplatné")</span><span class="sxs-lookup"><span data-stu-id="287d1-191">![subscription&#45;cancel control](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM subscription-cancel control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="287d1-192">Využití</span><span class="sxs-lookup"><span data-stu-id="287d1-192">Usage</span></span>  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a><span data-ttu-id="287d1-193">Parametry</span><span class="sxs-lookup"><span data-stu-id="287d1-193">Parameters</span></span>  
  
|<span data-ttu-id="287d1-194">Parametr</span><span class="sxs-lookup"><span data-stu-id="287d1-194">Parameter</span></span>|<span data-ttu-id="287d1-195">Popis</span><span class="sxs-lookup"><span data-stu-id="287d1-195">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="287d1-196">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="287d1-196">subscriptionId</span></span>|<span data-ttu-id="287d1-197">Id předplatného zrušit.</span><span class="sxs-lookup"><span data-stu-id="287d1-197">The id of the subscription to cancel.</span></span>|  
|<span data-ttu-id="287d1-198">cancelUrl</span><span class="sxs-lookup"><span data-stu-id="287d1-198">cancelUrl</span></span>|<span data-ttu-id="287d1-199">Adresa URL zrušit předplatné.</span><span class="sxs-lookup"><span data-stu-id="287d1-199">The subscription cancel URL.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="287d1-200">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="287d1-200">Developer portal templates</span></span>  
 <span data-ttu-id="287d1-201">`subscription-cancel` Řízení je možné použít následující šablony portálu vývojáře.</span><span class="sxs-lookup"><span data-stu-id="287d1-201">The `subscription-cancel` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="287d1-202">Produktu</span><span class="sxs-lookup"><span data-stu-id="287d1-202">Product</span></span>](api-management-product-templates.md#Product)

## <a name="next-steps"></a><span data-ttu-id="287d1-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="287d1-203">Next steps</span></span>
<span data-ttu-id="287d1-204">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="287d1-204">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>