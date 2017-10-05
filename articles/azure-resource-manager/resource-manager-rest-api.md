---
title: "Rozhraní REST API Resource Manager | Microsoft Docs"
description: "Přehled příklady ověřování a použití rozhraní REST API Resource Manager"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-rest-apis"></a><span data-ttu-id="3f3f5-103">Rozhraní REST API pro Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="3f3f5-103">Resource Manager REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f3f5-104">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f3f5-104">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="3f3f5-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3f3f5-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="3f3f5-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3f3f5-106">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="3f3f5-107">REST API</span><span class="sxs-lookup"><span data-stu-id="3f3f5-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="3f3f5-108">Za každé volání do Azure Resource Manager, za každou nasazené šablony za každý účet úložiště nejsou jeden nebo více volání rozhraní RESTful API Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-108">Behind every call to Azure Resource Manager, behind every deployed template, behind every configured storage account there are one or more calls to the Azure Resource Manager's RESTful API.</span></span> <span data-ttu-id="3f3f5-109">Toto téma je věnována těchto rozhraní API a jak je můžete volat bez použití vůbec žádné sady SDK.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-109">This topic is devoted to those APIs and how you can call them without using any SDK at all.</span></span> <span data-ttu-id="3f3f5-110">Tento přístup je užitečné, pokud chcete plnou kontrolu nad požadavky do Azure, nebo pokud sada SDK pro preferovaný jazyk není k dispozici nebo nepodporuje operace, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-110">This approach is useful if you want full control of requests to Azure, or if the SDK for your preferred language is not available or doesn't support the operations you need.</span></span>

<span data-ttu-id="3f3f5-111">Tento článek neprochází každé rozhraní API, které je vystaven v Azure, ale spíš používá některé operace, jako příklady, jak se připojíte k nim.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-111">This article does not go through every API that is exposed in Azure, but rather uses some operations as examples of how you connect to them.</span></span> <span data-ttu-id="3f3f5-112">Jakmile porozumíte základům, můžete si přečíst [referenci rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) naleznete podrobné informace o tom, jak používat zbytek rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-112">After you understand the basics, you can read the [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) to find detailed information on how to use the rest of the APIs.</span></span>

## <a name="authentication"></a><span data-ttu-id="3f3f5-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="3f3f5-113">Authentication</span></span>
<span data-ttu-id="3f3f5-114">Ověřování pro službu správce prostředků se zpracovává pomocí Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-114">Authentication for Resource Manager is handled by Azure Active Directory (AD).</span></span> <span data-ttu-id="3f3f5-115">Pro připojení k jakéhokoli rozhraní API, musíte nejprve ověřit pomocí služby Azure AD pro příjem ověřovací token, který můžete předat každou žádost.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-115">To connect to any API, you first need to authenticate with Azure AD to receive an authentication token that you can pass on to every request.</span></span> <span data-ttu-id="3f3f5-116">Jak jsme jsou popisující přímo čistý volání rozhraní REST API, předpokládáme, že nechcete ověřit tak, že se zobrazí výzva k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-116">As we are describing a pure call directly to the REST APIs, we assume that you don't want to authenticate by being prompted for a username and password.</span></span> <span data-ttu-id="3f3f5-117">Také předpokládáme, že nepoužíváte dvou mechanismů Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-117">We also assume you are not using two factor authentication mechanisms.</span></span> <span data-ttu-id="3f3f5-118">Proto vytvoříme, co se nazývá aplikaci Azure AD a službu objektu zabezpečení, které se používají k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-118">Therefore, we create what is called an Azure AD Application and a service principal that are used to log in.</span></span> <span data-ttu-id="3f3f5-119">Ale mějte na paměti, který Azure AD podporuje několik postupů ověřování, a všechny z nich může získat token tuto ověřování, které potřebujeme pro následné žádosti rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-119">But remember that Azure AD support several authentication procedures and all of them could be used to retrieve that authentication token that we need for subsequent API requests.</span></span>
<span data-ttu-id="3f3f5-120">Postupujte podle [aplikace vytvořit Azure AD a instanční objekt](resource-group-create-service-principal-portal.md) pokyny krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-120">Follow [Create Azure AD Application and Service Principal](resource-group-create-service-principal-portal.md) for step by step instructions.</span></span>

### <a name="generating-an-access-token"></a><span data-ttu-id="3f3f5-121">Generování Token přístupu</span><span class="sxs-lookup"><span data-stu-id="3f3f5-121">Generating an Access Token</span></span>
<span data-ttu-id="3f3f5-122">Ověřování služby Azure AD se provádí volání do služby Azure AD, nachází v login.microsoftonline.com.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-122">Authentication against Azure AD is done by calling out to Azure AD, located at login.microsoftonline.com.</span></span> <span data-ttu-id="3f3f5-123">K ověření, musíte mít následující informace:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-123">To authenticate, you need to have the following information:</span></span>

* <span data-ttu-id="3f3f5-124">ID klienta Azure AD (název se použije k přihlášení, často stejný jako vaší společnosti, ale není nezbytné služby Azure AD)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-124">Azure AD Tenant ID (the name of that Azure AD you are using to log in, often the same as your company but not necessary)</span></span>
* <span data-ttu-id="3f3f5-125">ID aplikace (prováděné během kroku vytvoření aplikace Azure AD)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-125">Application ID (taken during the Azure AD application creation step)</span></span>
* <span data-ttu-id="3f3f5-126">Heslo (který jste vybrali při vytváření aplikace Azure AD)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-126">Password (that you selected while creating the Azure AD Application)</span></span>

<span data-ttu-id="3f3f5-127">V následující požadavek HTTP nezapomeňte nahradit správné hodnoty "ID klienta Azure AD", "ID aplikace" a "Password".</span><span class="sxs-lookup"><span data-stu-id="3f3f5-127">In the following HTTP request, make sure to replace "Azure AD Tenant ID", "Application ID" and "Password" with the correct values.</span></span>

<span data-ttu-id="3f3f5-128">**Obecná žádost HTTP:**</span><span class="sxs-lookup"><span data-stu-id="3f3f5-128">**Generic HTTP Request:**</span></span>

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

<span data-ttu-id="3f3f5-129">... (Pokud je ověřování úspěšné) bude mít za následek odpověď podobná následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-129">... will (if authentication succeeds) result in a response similar to the following response:</span></span>

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
<span data-ttu-id="3f3f5-130">(Access_token v předchozí odpovědi byla zkrácena zvýšit čitelnost)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-130">(The access_token in the preceding response have been shortened to increase readability)</span></span>

<span data-ttu-id="3f3f5-131">**Generování přístupového tokenu pomocí Bash:**</span><span class="sxs-lookup"><span data-stu-id="3f3f5-131">**Generating access token using Bash:**</span></span>

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

<span data-ttu-id="3f3f5-132">**Generování přístupového tokenu pomocí prostředí PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="3f3f5-132">**Generating access token using PowerShell:**</span></span>

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

<span data-ttu-id="3f3f5-133">Odpověď obsahuje přístupový token, informace o tom, jak dlouho je tento token platný a informace o jaké prostředků můžete použít tento token pro.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-133">The response contains an access token, information about how long that token is valid, and information about what resource you can use that token for.</span></span>
<span data-ttu-id="3f3f5-134">Přístupový token, který jste obdrželi v předchozích volání protokolu HTTP, musí být předán v pro všechny žádosti do rozhraní API Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-134">The access token you received in the previous HTTP call must be passed in for all request to the Resource Manager API.</span></span> <span data-ttu-id="3f3f5-135">Předejte ji jako hodnotu záhlaví s názvem "Autorizace" s hodnotou "Nosiče YOUR_ACCESS_TOKEN".</span><span class="sxs-lookup"><span data-stu-id="3f3f5-135">You pass it as a header value named "Authorization" with the value "Bearer YOUR_ACCESS_TOKEN".</span></span> <span data-ttu-id="3f3f5-136">Všimněte si, mezeru mezi "Nosiče" a přístupový token.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-136">Notice the space between "Bearer" and your access token.</span></span>

<span data-ttu-id="3f3f5-137">Jak vidíte z výše uvedených výsledku HTTP, pro určitou dobu dobu, během které by měly ukládat do mezipaměti a opakovaně používat tento stejný token je platný token.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-137">As you can see from the above HTTP Result, the token is valid for a specific period of time during which you should cache and reuse that same token.</span></span> <span data-ttu-id="3f3f5-138">I když je možné k ověřování na základě Azure AD u jednotlivých volání API, by bylo velmi neefektivní.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-138">Even if it is possible to authenticate against Azure AD for each API call, it would be highly inefficient.</span></span>

## <a name="calling-resource-manager-rest-apis"></a><span data-ttu-id="3f3f5-139">Rozhraní API REST volání Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3f3f5-139">Calling Resource Manager REST APIs</span></span>
<span data-ttu-id="3f3f5-140">Toto téma pouze pomocí několik rozhraní API vysvětlují základní použití operace REST.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-140">This topic only uses a few APIs to explain the basic usage of the REST operations.</span></span> <span data-ttu-id="3f3f5-141">Informace o všech operacích, najdete v tématu [rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-141">For information about all the operations, see [Azure Resource Manager REST APIs](https://docs.microsoft.com/rest/api/resources/).</span></span>

### <a name="list-all-subscriptions"></a><span data-ttu-id="3f3f5-142">Zobrazí seznam všech odběrů</span><span class="sxs-lookup"><span data-stu-id="3f3f5-142">List all subscriptions</span></span>
<span data-ttu-id="3f3f5-143">Jedním z nejjednodušší operace, které můžete provést je do seznamu dostupných předplatných, kterým můžete přistupovat.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-143">One of the simplest operations you can do is to list the available subscriptions that you can access.</span></span> <span data-ttu-id="3f3f5-144">V následující žádosti uvidíte, jak přístupový token je předána jako hlavičku:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-144">In the following request, you see how the access token is passed in as a header:</span></span>

<span data-ttu-id="3f3f5-145">(Nahraďte YOUR_ACCESS_TOKEN vaše skutečné přístup k tokenu.)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-145">(Replace YOUR_ACCESS_TOKEN with your actual Access Token.)</span></span>

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="3f3f5-146">... a v důsledku toho získat seznam předplatná, která tento objekt zabezpečení služby je povolen přístup k</span><span class="sxs-lookup"><span data-stu-id="3f3f5-146">... and as a result, you get a list of subscriptions that this service principal is allowed to access</span></span>

<span data-ttu-id="3f3f5-147">(ID odběru byla zkrácena čitelnější)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-147">(Subscription IDs have been shortened for readability)</span></span>

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a><span data-ttu-id="3f3f5-148">Zobrazí seznam všech skupin prostředků v konkrétní předplatné</span><span class="sxs-lookup"><span data-stu-id="3f3f5-148">List all resource groups in a specific subscription</span></span>
<span data-ttu-id="3f3f5-149">Všechny prostředky, které jsou k dispozici rozhraní API Resource Manager jsou vnořeny uvnitř skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-149">All resources available with the Resource Manager APIs are nested inside a resource group.</span></span> <span data-ttu-id="3f3f5-150">Resource Manager můžete dotazovat pro existující skupiny prostředků ve vašem předplatném pomocí následující požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-150">You can query Resource Manager for existing resource groups in your subscription using the following HTTP GET request.</span></span> <span data-ttu-id="3f3f5-151">Všimněte si, jak ID předplatného se předávají v jako část adresy URL této doby.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-151">Notice how the subscription ID is passed in as part of the URL this time.</span></span>

<span data-ttu-id="3f3f5-152">(Nahraďte YOUR_ACCESS_TOKEN a ID_ODBĚRU ID skutečného přístupu token a předplatného)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-152">(Replace YOUR_ACCESS_TOKEN and SUBSCRIPTION_ID with your actual access token and subscription ID)</span></span>

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="3f3f5-153">Odpovědi můžete získat závisí na tom, jestli máte definované žádné skupiny prostředků a pokud ano, kolik.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-153">The response you get depends on whether you have any resource groups defined and if so, how many.</span></span>

<span data-ttu-id="3f3f5-154">(ID odběru byla zkrácena čitelnější)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-154">(Subscription IDs have been shortened for readability)</span></span>

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a><span data-ttu-id="3f3f5-155">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3f3f5-155">Create a resource group</span></span>
<span data-ttu-id="3f3f5-156">Zatím jsme jste pouze byla dotaz na rozhraní API Resource Manager informace.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-156">So far, we've only been querying the Resource Manager APIs for information.</span></span> <span data-ttu-id="3f3f5-157">Je čas vytvoření některé prostředky a Začněme tím nejjednodušší ze všech, skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-157">It's time we create some resources, and let's start by the simplest of them all, a resource group.</span></span> <span data-ttu-id="3f3f5-158">Následující požadavek HTTP vytvoří skupinu prostředků v oblast nebo umístění podle vaší volby a přidá značku na ni.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-158">The following HTTP request creates a resource group in a region/location of your choice, and adds a tag to it.</span></span>

<span data-ttu-id="3f3f5-159">(Nahraďte YOUR_ACCESS_TOKEN, ID_ODBĚRU, RESOURCE_GROUP_NAME se skutečné tokenu přístupu, ID předplatného a název skupiny prostředků, kterou chcete vytvořit)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-159">(Replace YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME with your actual Access Token, Subscription ID and name of the Resource Group you want to create)</span></span>

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

<span data-ttu-id="3f3f5-160">Pokud bylo úspěšné, můžete získat odpovědi, která je podobná následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="3f3f5-160">If successful, you get a response that is similar to the following response:</span></span>

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<span data-ttu-id="3f3f5-161">Úspěšně jste vytvořili skupinu prostředků v Azure.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-161">You've successfully created a resource group in Azure.</span></span> <span data-ttu-id="3f3f5-162">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3f3f5-162">Congratulations!</span></span>

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a><span data-ttu-id="3f3f5-163">Nasadit prostředky do skupiny prostředků pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="3f3f5-163">Deploy resources to a resource group using a Resource Manager Template</span></span>
<span data-ttu-id="3f3f5-164">S Resource Managerem, můžete nasadit své prostředky pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-164">With Resource Manager, you can deploy your resources using templates.</span></span> <span data-ttu-id="3f3f5-165">Šablona definuje několik prostředků a jejich závislosti.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-165">A template defines several resources and their dependencies.</span></span> <span data-ttu-id="3f3f5-166">Pro tento oddíl předpokladu, že jste se seznámili s šablony Resource Manageru a jsme právě ukazují, jak můžete využít ke spuštění nasazení volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-166">For this section, we assume you are familiar with Resource Manager templates, and we just show you how to make the API call to start deployment.</span></span> <span data-ttu-id="3f3f5-167">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-167">For more information about constructing templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="3f3f5-168">Nasazení šablony není lišit na tom, jak volat jiná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-168">Deployment of a template doesn't differ much to how you call other APIs.</span></span> <span data-ttu-id="3f3f5-169">Jeden aspekt důležité je, že nasazení šablony může trvat poměrně dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-169">One important aspect is that deployment of a template can take quite a long time.</span></span> <span data-ttu-id="3f3f5-170">Volání rozhraní API právě vrátí, a je to pro vás jako vývojáři dotaz na stav nasazení a zjistěte, pokud se provádí nasazení.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-170">The API call just returns, and it's up to you as developer to query for status of the deployment to find out when the deployment is done.</span></span> <span data-ttu-id="3f3f5-171">Další informace najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-171">For more information, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>

<span data-ttu-id="3f3f5-172">V tomto příkladu používáme k dispozici na veřejně vystavené šablonu [Githubu](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-172">For this example, we use a publicly exposed template available on [GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="3f3f5-173">Šablona, kterou používáme nasadí virtuální počítač s Linuxem do oblasti západní USA.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-173">The template we use deploys a Linux VM to the West US region.</span></span> <span data-ttu-id="3f3f5-174">I když tento příklad používá šablonu k dispozici v veřejného úložiště jako GitHub, můžete místo toho předat kompletní šablonou jako součást požadavku.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-174">Even though this example uses a template available in a public repository like GitHub, you can instead pass the full template as part of the request.</span></span> <span data-ttu-id="3f3f5-175">Všimněte si, že jsme zadejte hodnoty parametrů v žádosti, které se používají v šabloně nasazené.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-175">Note that we provide parameter values in the request that are used inside the deployed template.</span></span>

<span data-ttu-id="3f3f5-176">(Náhrada ID_ODBĚRU, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD a DNS_NAME_FOR_PUBLIC_IP hodnoty, které jsou vhodné pro vaši žádost o)</span><span class="sxs-lookup"><span data-stu-id="3f3f5-176">(Replace SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD and DNS_NAME_FOR_PUBLIC_IP to values appropriate for your request)</span></span>

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

<span data-ttu-id="3f3f5-177">Dlouhé JSON odpovědi na tento požadavek se vynechá ke zlepšení čitelnosti této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-177">The long JSON response for this request has been omitted to improve readability of this documentation.</span></span> <span data-ttu-id="3f3f5-178">Odpověď obsahuje informace o podle šablony nasazení, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3f3f5-178">The response contains information about the templated deployment that you created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f3f5-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f3f5-179">Next steps</span></span>

- <span data-ttu-id="3f3f5-180">Další informace o zpracování asynchronní operace REST najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="3f3f5-180">To learn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
