---
title: "ovládací prvky stránky aaaAzure API Management | Microsoft Docs"
description: "Další informace o hello ovládací prvky stránky k dispozici pro použití v šablonách portálu vývojáře ve službě Azure API Management."
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
ms.openlocfilehash: 1a16a6fce197c0a2e14807ac21e81a9a73b515b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-page-controls"></a><span data-ttu-id="5d9d4-103">Ovládací prvky stránky Azure API Management</span><span class="sxs-lookup"><span data-stu-id="5d9d4-103">Azure API Management page controls</span></span>
<span data-ttu-id="5d9d4-104">Azure API Management obsahuje následující ovládací prvky pro použití v šablonách portálu vývojáře hello hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-104">Azure API Management provides hello following controls for use in hello developer portal templates.</span></span>  
  
 <span data-ttu-id="5d9d4-105">toouse ovládacího prvku ji umístíte do hello požadovaného umístění v šabloně portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-105">toouse a control, place it in hello desired location in hello developer portal template.</span></span> <span data-ttu-id="5d9d4-106">Některé ovládací prvky, jako je například hello [akcí aplikace](#app-actions) řízení, musí mít parametry, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-106">Some controls, such as hello [app-actions](#app-actions) control, have parameters, as shown in hello following example.</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 <span data-ttu-id="5d9d4-107">Hello hodnoty parametrů hello jsou předána jako součást hello datový model pro šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-107">hello values for hello parameters are passed in as part of hello data model for hello template.</span></span> <span data-ttu-id="5d9d4-108">Ve většině případů můžete jednoduše vložit hello poskytuje příklad pro každý ovládací prvek pro něj toowork správně.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-108">In most cases, you can simply paste in hello provided example for each control for it toowork correctly.</span></span> <span data-ttu-id="5d9d4-109">Další informace o hello hodnoty parametrů uvidíte oddíl modelu dat hello ke každé šabloně, ve kterém může použít ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-109">For more information on hello parameter values, you can see hello data model section for each template in which a control may be used.</span></span>  
  
 <span data-ttu-id="5d9d4-110">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="5d9d4-110">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
## <a name="developer-portal-template-page-controls"></a><span data-ttu-id="5d9d4-111">Ovládací prvky stránky šablony portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-111">Developer portal template page controls</span></span>  
  
-   [<span data-ttu-id="5d9d4-112">Akce aplikace</span><span class="sxs-lookup"><span data-stu-id="5d9d4-112">app-actions</span></span>](#app-actions)  
  
-   [<span data-ttu-id="5d9d4-113">Basic přihlášení</span><span class="sxs-lookup"><span data-stu-id="5d9d4-113">basic-signin</span></span>](#basic-signin)  
  
-   [<span data-ttu-id="5d9d4-114">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="5d9d4-114">paging-control</span></span>](#paging-control)  
  
-   [<span data-ttu-id="5d9d4-115">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="5d9d4-115">providers</span></span>](#providers)  
  
-   [<span data-ttu-id="5d9d4-116">ovládací prvek vyhledávání</span><span class="sxs-lookup"><span data-stu-id="5d9d4-116">search-control</span></span>](#search-control)  
  
-   [<span data-ttu-id="5d9d4-117">registrace</span><span class="sxs-lookup"><span data-stu-id="5d9d4-117">sign-up</span></span>](#sign-up)  
  
-   [<span data-ttu-id="5d9d4-118">přihlášení k odběru tlačítko</span><span class="sxs-lookup"><span data-stu-id="5d9d4-118">subscribe-button</span></span>](#subscribe-button)  
  
-   [<span data-ttu-id="5d9d4-119">zrušení předplatného</span><span class="sxs-lookup"><span data-stu-id="5d9d4-119">subscription-cancel</span></span>](#subscription-cancel)  
  
##  <span data-ttu-id="5d9d4-120"><a name="app-actions"></a>Akce aplikace</span><span class="sxs-lookup"><span data-stu-id="5d9d4-120"><a name="app-actions"></a> app-actions</span></span>  
 <span data-ttu-id="5d9d4-121">Hello `app-actions` řízení poskytuje uživatelské rozhraní pro interakci s aplikací na stránce profilu uživatele hello v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-121">hello `app-actions` control provides a user interface for interacting with applications on hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="5d9d4-122">![aplikace & č. 45; akce řízení](./media/api-management-page-controls/APIM-app-actions-control.png "APIM akcí aplikace řízení")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-122">![app&#45;actions control](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app-actions control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-123">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-123">Usage</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-124">Parameters</span></span>  
  
|<span data-ttu-id="5d9d4-125">Parametr</span><span class="sxs-lookup"><span data-stu-id="5d9d4-125">Parameter</span></span>|<span data-ttu-id="5d9d4-126">Popis</span><span class="sxs-lookup"><span data-stu-id="5d9d4-126">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="5d9d4-127">appId</span><span class="sxs-lookup"><span data-stu-id="5d9d4-127">appId</span></span>|<span data-ttu-id="5d9d4-128">Hello id aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-128">hello id of hello application.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-129">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-129">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-130">Hello `app-actions` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-130">hello `app-actions` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-131">Aplikace</span><span class="sxs-lookup"><span data-stu-id="5d9d4-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
  
##  <span data-ttu-id="5d9d4-132"><a name="basic-signin"></a>Basic přihlášení</span><span class="sxs-lookup"><span data-stu-id="5d9d4-132"><a name="basic-signin"></a> basic-signin</span></span>  
 <span data-ttu-id="5d9d4-133">Hello `basic-signin` řízení poskytuje ovládací prvek pro shromažďování přihlášení uživatele informace v hello přihlašovací stránku v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-133">hello `basic-signin` control provides a control for collecting user sign in information in hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="5d9d4-134">![Basic & č. 45; přihlášení řízení](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic přihlášení řízení")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-134">![basic&#45;signin control](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-135">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-135">Usage</span></span>  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-136">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-136">Parameters</span></span>  
 <span data-ttu-id="5d9d4-137">Žádné.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-137">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-138">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-138">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-139">Hello `basic-signin` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-139">hello `basic-signin` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-140">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="5d9d4-140">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="5d9d4-141"><a name="paging-control"></a>ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="5d9d4-141"><a name="paging-control"></a> paging-control</span></span>  
 <span data-ttu-id="5d9d4-142">Hello `paging-control` poskytuje funkce stránkování vývojáře portálu stránky zobrazující seznam položek.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-142">hello `paging-control` provides paging functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="5d9d4-143">![stránkování řízení](./media/api-management-page-controls/APIM-paging-control.png "APIM stránkování ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-143">![paging control](./media/api-management-page-controls/APIM-paging-control.png "APIM paging control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-144">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-144">Usage</span></span>  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-145">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-145">Parameters</span></span>  
 <span data-ttu-id="5d9d4-146">Žádné.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-146">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-147">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-147">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-148">Hello `paging-control` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-148">hello `paging-control` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-149">Rozhraní API seznamu</span><span class="sxs-lookup"><span data-stu-id="5d9d4-149">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="5d9d4-150">Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="5d9d4-150">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
  
-   [<span data-ttu-id="5d9d4-151">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="5d9d4-151">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="5d9d4-152"><a name="providers"></a>Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="5d9d4-152"><a name="providers"></a> providers</span></span>  
 <span data-ttu-id="5d9d4-153">Hello `providers` řízení poskytuje ovládací prvek pro výběr zprostředkovatelů ověřování v hello přihlašovací stránku v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-153">hello `providers` control provides a control for selection of authentication providers in hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="5d9d4-154">![poskytovatelé řízení](./media/api-management-page-controls/APIM-providers-control.png "APIM poskytovatelé řízení")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-154">![providers control](./media/api-management-page-controls/APIM-providers-control.png "APIM providers control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-155">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-155">Usage</span></span>  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-156">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-156">Parameters</span></span>  
 <span data-ttu-id="5d9d4-157">Žádné.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-157">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-158">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-158">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-159">Hello `providers` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-159">hello `providers` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-160">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="5d9d4-160">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="5d9d4-161"><a name="search-control"></a>ovládací prvek vyhledávání</span><span class="sxs-lookup"><span data-stu-id="5d9d4-161"><a name="search-control"></a> search-control</span></span>  
 <span data-ttu-id="5d9d4-162">Hello `search-control` poskytuje funkce hledání vývojáře portálu stránky zobrazující seznam položek.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-162">hello `search-control` provides search functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="5d9d4-163">![hledání řízení](./media/api-management-page-controls/APIM-search-control.png "APIM vyhledávání řízení")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-163">![search control](./media/api-management-page-controls/APIM-search-control.png "APIM search control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-164">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-164">Usage</span></span>  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-165">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-165">Parameters</span></span>  
 <span data-ttu-id="5d9d4-166">Žádné.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-166">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-167">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-167">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-168">Hello `search-control` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-168">hello `search-control` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-169">Rozhraní API seznamu</span><span class="sxs-lookup"><span data-stu-id="5d9d4-169">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="5d9d4-170">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="5d9d4-170">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="5d9d4-171"><a name="sign-up"></a>registrace</span><span class="sxs-lookup"><span data-stu-id="5d9d4-171"><a name="sign-up"></a> sign-up</span></span>  
 <span data-ttu-id="5d9d4-172">Hello `sign-up` řízení poskytuje ovládací prvek pro shromažďování informací o profilu uživatele hello registrační stránku v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-172">hello `sign-up` control provides a control for collecting user profile information in hello sign up page in hello developer portal.</span></span>  
  
 <span data-ttu-id="5d9d4-173">![přihlašovací & č. 45; až řízení](./media/api-management-page-controls/APIM-sign-up-control.png "APIM registrace ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-173">![sign&#45;up control](./media/api-management-page-controls/APIM-sign-up-control.png "APIM sign-up control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-174">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-174">Usage</span></span>  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-175">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-175">Parameters</span></span>  
 <span data-ttu-id="5d9d4-176">Žádné.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-176">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-177">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-177">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-178">Hello `sign-up` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-178">hello `sign-up` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-179">Registrace</span><span class="sxs-lookup"><span data-stu-id="5d9d4-179">Sign up</span></span>](api-management-page-templates.md#SignUp)  
  
##  <span data-ttu-id="5d9d4-180"><a name="subscribe-button"></a>přihlášení k odběru tlačítko</span><span class="sxs-lookup"><span data-stu-id="5d9d4-180"><a name="subscribe-button"></a> subscribe-button</span></span>  
 <span data-ttu-id="5d9d4-181">Hello `subscribe-button` poskytuje ovládací prvek pro přihlášení k odběru produktu tooa uživatele.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-181">hello `subscribe-button` provides a control for subscribing a user tooa product.</span></span>  
  
 <span data-ttu-id="5d9d4-182">![přihlášení k odběru & č. 45; tlačítko – ovládací prvek](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM přihlášení k odběru tlačítko – ovládací prvek")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-182">![subscribe&#45;button control](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM subscribe-button control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-183">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-183">Usage</span></span>  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-184">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-184">Parameters</span></span>  
 <span data-ttu-id="5d9d4-185">Žádné.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-185">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-186">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-186">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-187">Hello `subscribe-button` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-187">hello `subscribe-button` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-188">Produktu</span><span class="sxs-lookup"><span data-stu-id="5d9d4-188">Product</span></span>](api-management-product-templates.md#Product)  
  
##  <span data-ttu-id="5d9d4-189"><a name="subscription-cancel"></a>zrušení předplatného</span><span class="sxs-lookup"><span data-stu-id="5d9d4-189"><a name="subscription-cancel"></a> subscription-cancel</span></span>  
 <span data-ttu-id="5d9d4-190">Hello `subscription-cancel` řízení poskytuje ovládací prvek pro zrušení předplatného produktu tooa v hello profilu uživatele na stránce portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-190">hello `subscription-cancel` control provides a control for cancelling a subscription tooa product in hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="5d9d4-191">![předplatné & č. 45; zrušit řízení](./media/api-management-page-controls/APIM-subscription-cancel-control.png "řízení APIM zrušit předplatné")</span><span class="sxs-lookup"><span data-stu-id="5d9d4-191">![subscription&#45;cancel control](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM subscription-cancel control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="5d9d4-192">Využití</span><span class="sxs-lookup"><span data-stu-id="5d9d4-192">Usage</span></span>  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a><span data-ttu-id="5d9d4-193">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d9d4-193">Parameters</span></span>  
  
|<span data-ttu-id="5d9d4-194">Parametr</span><span class="sxs-lookup"><span data-stu-id="5d9d4-194">Parameter</span></span>|<span data-ttu-id="5d9d4-195">Popis</span><span class="sxs-lookup"><span data-stu-id="5d9d4-195">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="5d9d4-196">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="5d9d4-196">subscriptionId</span></span>|<span data-ttu-id="5d9d4-197">Hello id předplatného toocancel hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-197">hello id of hello subscription toocancel.</span></span>|  
|<span data-ttu-id="5d9d4-198">cancelUrl</span><span class="sxs-lookup"><span data-stu-id="5d9d4-198">cancelUrl</span></span>|<span data-ttu-id="5d9d4-199">Hello předplatné zrušit adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-199">hello subscription cancel URL.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="5d9d4-200">Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="5d9d4-200">Developer portal templates</span></span>  
 <span data-ttu-id="5d9d4-201">Hello `subscription-cancel` řízení je možné použít následující šablony na portálu vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="5d9d4-201">hello `subscription-cancel` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="5d9d4-202">Produktu</span><span class="sxs-lookup"><span data-stu-id="5d9d4-202">Product</span></span>](api-management-product-templates.md#Product)

## <a name="next-steps"></a><span data-ttu-id="5d9d4-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d9d4-203">Next steps</span></span>
<span data-ttu-id="5d9d4-204">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5d9d4-204">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
