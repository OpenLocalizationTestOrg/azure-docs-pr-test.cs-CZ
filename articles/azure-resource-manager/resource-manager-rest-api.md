---
title: aaaResource Manager REST API | Microsoft Docs
description: "Přehled hello ověřování rozhraní REST API Resource Manager a příklady použití"
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
ms.openlocfilehash: 3ccc3575c5e06c41f2fdc5317711980fc6a2f649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-manager-rest-apis"></a><span data-ttu-id="f0b88-103">Rozhraní REST API pro Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="f0b88-103">Resource Manager REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0b88-104">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0b88-104">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="f0b88-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f0b88-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="f0b88-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f0b88-106">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="f0b88-107">REST API</span><span class="sxs-lookup"><span data-stu-id="f0b88-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="f0b88-108">Za každé volání tooAzure Resource Manager za každých nasazené šablony za každý účet úložiště nejsou jeden nebo více volání toohello Azure Resource Manager na rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="f0b88-108">Behind every call tooAzure Resource Manager, behind every deployed template, behind every configured storage account there are one or more calls toohello Azure Resource Manager's RESTful API.</span></span> <span data-ttu-id="f0b88-109">Toto téma je věnovaná toothose rozhraní API a jak je můžete volat bez použití vůbec žádné sady SDK.</span><span class="sxs-lookup"><span data-stu-id="f0b88-109">This topic is devoted toothose APIs and how you can call them without using any SDK at all.</span></span> <span data-ttu-id="f0b88-110">Tento přístup je užitečné, pokud chcete plnou kontrolu nad tooAzure požadavky, nebo pokud hello SDK pro preferovaný jazyk není k dispozici nebo nepodporuje hello operace, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="f0b88-110">This approach is useful if you want full control of requests tooAzure, or if hello SDK for your preferred language is not available or doesn't support hello operations you need.</span></span>

<span data-ttu-id="f0b88-111">Tento článek neprochází každé rozhraní API, které je vystaven v Azure, ale spíš používá jako příklady, jak připojit toothem některé operace.</span><span class="sxs-lookup"><span data-stu-id="f0b88-111">This article does not go through every API that is exposed in Azure, but rather uses some operations as examples of how you connect toothem.</span></span> <span data-ttu-id="f0b88-112">Jakmile porozumíte hello základy, si můžete přečíst hello [referenci rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) toofind podrobné informace o tom, jak toouse hello zbytku hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0b88-112">After you understand hello basics, you can read hello [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) toofind detailed information on how toouse hello rest of hello APIs.</span></span>

## <a name="authentication"></a><span data-ttu-id="f0b88-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="f0b88-113">Authentication</span></span>
<span data-ttu-id="f0b88-114">Ověřování pro službu správce prostředků se zpracovává pomocí Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="f0b88-114">Authentication for Resource Manager is handled by Azure Active Directory (AD).</span></span> <span data-ttu-id="f0b88-115">tooconnect tooany rozhraní API, musíte nejdřív tooauthenticate s Azure AD tooreceive ověřovací token, který můžete předat na tooevery požadavku.</span><span class="sxs-lookup"><span data-stu-id="f0b88-115">tooconnect tooany API, you first need tooauthenticate with Azure AD tooreceive an authentication token that you can pass on tooevery request.</span></span> <span data-ttu-id="f0b88-116">Jak jsme jsou popisující čistý volání přímo toohello rozhraní REST API, budeme předpokládat, že nechcete, aby tooauthenticate vyzváni k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="f0b88-116">As we are describing a pure call directly toohello REST APIs, we assume that you don't want tooauthenticate by being prompted for a username and password.</span></span> <span data-ttu-id="f0b88-117">Také předpokládáme, že nepoužíváte dvou mechanismů Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="f0b88-117">We also assume you are not using two factor authentication mechanisms.</span></span> <span data-ttu-id="f0b88-118">Proto vytvoříme, co se nazývá aplikaci Azure AD a instanční objekt, které jsou používané toolog v.</span><span class="sxs-lookup"><span data-stu-id="f0b88-118">Therefore, we create what is called an Azure AD Application and a service principal that are used toolog in.</span></span> <span data-ttu-id="f0b88-119">Mějte na paměti, že Azure AD podporují několik postupů ověřování a všechny z nich, ale může být použité tooretrieve Tento ověřovací token, které potřebujeme pro následné žádosti rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0b88-119">But remember that Azure AD support several authentication procedures and all of them could be used tooretrieve that authentication token that we need for subsequent API requests.</span></span>
<span data-ttu-id="f0b88-120">Postupujte podle [aplikace vytvořit Azure AD a instanční objekt](resource-group-create-service-principal-portal.md) pokyny krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="f0b88-120">Follow [Create Azure AD Application and Service Principal](resource-group-create-service-principal-portal.md) for step by step instructions.</span></span>

### <a name="generating-an-access-token"></a><span data-ttu-id="f0b88-121">Generování Token přístupu</span><span class="sxs-lookup"><span data-stu-id="f0b88-121">Generating an Access Token</span></span>
<span data-ttu-id="f0b88-122">Volání se nachází v login.microsoftonline.com tooAzure AD, je potřeba ověřování služby Azure AD. tooauthenticate, je třeba toohave hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="f0b88-122">Authentication against Azure AD is done by calling out tooAzure AD, located at login.microsoftonline.com. tooauthenticate, you need toohave hello following information:</span></span>

* <span data-ttu-id="f0b88-123">ID klienta Azure AD (hello název používáte toolog v služby Azure AD, často hello stejné jako vaší společnosti, ale není vyžadováno)</span><span class="sxs-lookup"><span data-stu-id="f0b88-123">Azure AD Tenant ID (hello name of that Azure AD you are using toolog in, often hello same as your company but not necessary)</span></span>
* <span data-ttu-id="f0b88-124">ID aplikace (prováděné během kroku vytváření aplikace hello Azure AD)</span><span class="sxs-lookup"><span data-stu-id="f0b88-124">Application ID (taken during hello Azure AD application creation step)</span></span>
* <span data-ttu-id="f0b88-125">Heslo (který jste vybrali při vytváření hello aplikace Azure AD)</span><span class="sxs-lookup"><span data-stu-id="f0b88-125">Password (that you selected while creating hello Azure AD Application)</span></span>

<span data-ttu-id="f0b88-126">V hello následující požadavek HTTP Ujistěte se, že tooreplace "Azure AD ID klienta", "ID aplikace" a "Password" s hello správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f0b88-126">In hello following HTTP request, make sure tooreplace "Azure AD Tenant ID", "Application ID" and "Password" with hello correct values.</span></span>

<span data-ttu-id="f0b88-127">**Obecná žádost HTTP:**</span><span class="sxs-lookup"><span data-stu-id="f0b88-127">**Generic HTTP Request:**</span></span>

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

<span data-ttu-id="f0b88-128">... (Pokud je ověřování úspěšné) bude mít za následek odpovědi podobné toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="f0b88-128">... will (if authentication succeeds) result in a response similar toohello following response:</span></span>

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
<span data-ttu-id="f0b88-129">(hello access_token v hello předcházející odpověď byla zkrácená tooincrease čitelnost)</span><span class="sxs-lookup"><span data-stu-id="f0b88-129">(hello access_token in hello preceding response have been shortened tooincrease readability)</span></span>

<span data-ttu-id="f0b88-130">**Generování přístupového tokenu pomocí Bash:**</span><span class="sxs-lookup"><span data-stu-id="f0b88-130">**Generating access token using Bash:**</span></span>

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

<span data-ttu-id="f0b88-131">**Generování přístupového tokenu pomocí prostředí PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="f0b88-131">**Generating access token using PowerShell:**</span></span>

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

<span data-ttu-id="f0b88-132">Hello odpovědi obsahuje přístupový token, informace o tom, jak dlouho tento token je platný a informace o jaké prostředků můžete použít tento token.</span><span class="sxs-lookup"><span data-stu-id="f0b88-132">hello response contains an access token, information about how long that token is valid, and information about what resource you can use that token for.</span></span>
<span data-ttu-id="f0b88-133">Hello přístupový token, který jste obdrželi v hello předchozí volání protokolu HTTP musí být předán v pro všechny žádosti o toohello rozhraní API Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0b88-133">hello access token you received in hello previous HTTP call must be passed in for all request toohello Resource Manager API.</span></span> <span data-ttu-id="f0b88-134">Předejte ji jako hodnotu záhlaví s názvem "Autorizace" s hodnotou hello "Nosiče YOUR_ACCESS_TOKEN".</span><span class="sxs-lookup"><span data-stu-id="f0b88-134">You pass it as a header value named "Authorization" with hello value "Bearer YOUR_ACCESS_TOKEN".</span></span> <span data-ttu-id="f0b88-135">Všimněte si hello mezery mezi "Nosiče" a přístupový token.</span><span class="sxs-lookup"><span data-stu-id="f0b88-135">Notice hello space between "Bearer" and your access token.</span></span>

<span data-ttu-id="f0b88-136">Jak je vidět z hello výše HTTP výsledek hello token je platný pro určitou dobu dobu, během které by měly ukládat do mezipaměti a opakovaně používat tento stejný token.</span><span class="sxs-lookup"><span data-stu-id="f0b88-136">As you can see from hello above HTTP Result, hello token is valid for a specific period of time during which you should cache and reuse that same token.</span></span> <span data-ttu-id="f0b88-137">I když je možné tooauthenticate služby Azure AD u jednotlivých volání API, by bylo velmi neefektivní.</span><span class="sxs-lookup"><span data-stu-id="f0b88-137">Even if it is possible tooauthenticate against Azure AD for each API call, it would be highly inefficient.</span></span>

## <a name="calling-resource-manager-rest-apis"></a><span data-ttu-id="f0b88-138">Rozhraní API REST volání Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f0b88-138">Calling Resource Manager REST APIs</span></span>
<span data-ttu-id="f0b88-139">Toto téma se používá jenom několik rozhraní API tooexplain hello základní použití operace REST hello.</span><span class="sxs-lookup"><span data-stu-id="f0b88-139">This topic only uses a few APIs tooexplain hello basic usage of hello REST operations.</span></span> <span data-ttu-id="f0b88-140">Informace o všech operacích hello najdete v tématu [rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="f0b88-140">For information about all hello operations, see [Azure Resource Manager REST APIs](https://docs.microsoft.com/rest/api/resources/).</span></span>

### <a name="list-all-subscriptions"></a><span data-ttu-id="f0b88-141">Zobrazí seznam všech odběrů</span><span class="sxs-lookup"><span data-stu-id="f0b88-141">List all subscriptions</span></span>
<span data-ttu-id="f0b88-142">Jedním z nejjednodušší operace hello, které můžete provést je hello toolist dostupných se předplatných, kterým můžete přistupovat.</span><span class="sxs-lookup"><span data-stu-id="f0b88-142">One of hello simplest operations you can do is toolist hello available subscriptions that you can access.</span></span> <span data-ttu-id="f0b88-143">V hello následující požadavek uvidíte, jak se předávají hello přístupový token v jako hlavičku:</span><span class="sxs-lookup"><span data-stu-id="f0b88-143">In hello following request, you see how hello access token is passed in as a header:</span></span>

<span data-ttu-id="f0b88-144">(Nahraďte YOUR_ACCESS_TOKEN vaše skutečné přístup k tokenu.)</span><span class="sxs-lookup"><span data-stu-id="f0b88-144">(Replace YOUR_ACCESS_TOKEN with your actual Access Token.)</span></span>

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="f0b88-145">... a v důsledku toho získat seznam předplatných, že je tento objekt zabezpečení služby povolené tooaccess</span><span class="sxs-lookup"><span data-stu-id="f0b88-145">... and as a result, you get a list of subscriptions that this service principal is allowed tooaccess</span></span>

<span data-ttu-id="f0b88-146">(ID odběru byla zkrácena čitelnější)</span><span class="sxs-lookup"><span data-stu-id="f0b88-146">(Subscription IDs have been shortened for readability)</span></span>

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

### <a name="list-all-resource-groups-in-a-specific-subscription"></a><span data-ttu-id="f0b88-147">Zobrazí seznam všech skupin prostředků v konkrétní předplatné</span><span class="sxs-lookup"><span data-stu-id="f0b88-147">List all resource groups in a specific subscription</span></span>
<span data-ttu-id="f0b88-148">Všechny prostředky, které jsou k dispozici hello rozhraní API Resource Manager jsou vnořeny uvnitř skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0b88-148">All resources available with hello Resource Manager APIs are nested inside a resource group.</span></span> <span data-ttu-id="f0b88-149">Resource Manager můžete dotazovat pro existující skupiny prostředků ve vašem předplatném pomocí hello následující požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f0b88-149">You can query Resource Manager for existing resource groups in your subscription using hello following HTTP GET request.</span></span> <span data-ttu-id="f0b88-150">Všimněte si, jak hello ID předplatného se předávají v jako součást hello URL této doby.</span><span class="sxs-lookup"><span data-stu-id="f0b88-150">Notice how hello subscription ID is passed in as part of hello URL this time.</span></span>

<span data-ttu-id="f0b88-151">(Nahraďte YOUR_ACCESS_TOKEN a ID_ODBĚRU ID skutečného přístupu token a předplatného)</span><span class="sxs-lookup"><span data-stu-id="f0b88-151">(Replace YOUR_ACCESS_TOKEN and SUBSCRIPTION_ID with your actual access token and subscription ID)</span></span>

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="f0b88-152">Hello odpovědi získáte závisí na tom, jestli máte definované žádné skupiny prostředků a pokud ano, kolik.</span><span class="sxs-lookup"><span data-stu-id="f0b88-152">hello response you get depends on whether you have any resource groups defined and if so, how many.</span></span>

<span data-ttu-id="f0b88-153">(ID odběru byla zkrácena čitelnější)</span><span class="sxs-lookup"><span data-stu-id="f0b88-153">(Subscription IDs have been shortened for readability)</span></span>

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

### <a name="create-a-resource-group"></a><span data-ttu-id="f0b88-154">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f0b88-154">Create a resource group</span></span>
<span data-ttu-id="f0b88-155">Zatím jsme jste pouze byla dotazování hello rozhraní API Resource Manager informace.</span><span class="sxs-lookup"><span data-stu-id="f0b88-155">So far, we've only been querying hello Resource Manager APIs for information.</span></span> <span data-ttu-id="f0b88-156">Je čas vytvoření některé prostředky a Začněme tím hello nejjednodušší ze všech, skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0b88-156">It's time we create some resources, and let's start by hello simplest of them all, a resource group.</span></span> <span data-ttu-id="f0b88-157">Hello následující požadavek HTTP vytvoří skupinu prostředků v oblast nebo umístění podle vaší volby a přidá tooit značky.</span><span class="sxs-lookup"><span data-stu-id="f0b88-157">hello following HTTP request creates a resource group in a region/location of your choice, and adds a tag tooit.</span></span>

<span data-ttu-id="f0b88-158">(Nahraďte YOUR_ACCESS_TOKEN, ID_ODBĚRU, RESOURCE_GROUP_NAME se skutečné tokenu přístupu, ID předplatného a název skupiny prostředků, které chcete toocreate hello)</span><span class="sxs-lookup"><span data-stu-id="f0b88-158">(Replace YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME with your actual Access Token, Subscription ID and name of hello Resource Group you want toocreate)</span></span>

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

<span data-ttu-id="f0b88-159">Pokud bylo úspěšné, můžete získat odpověď, který je podobný toohello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="f0b88-159">If successful, you get a response that is similar toohello following response:</span></span>

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

<span data-ttu-id="f0b88-160">Úspěšně jste vytvořili skupinu prostředků v Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b88-160">You've successfully created a resource group in Azure.</span></span> <span data-ttu-id="f0b88-161">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="f0b88-161">Congratulations!</span></span>

### <a name="deploy-resources-tooa-resource-group-using-a-resource-manager-template"></a><span data-ttu-id="f0b88-162">Nasazení skupiny prostředků tooa prostředky pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f0b88-162">Deploy resources tooa resource group using a Resource Manager Template</span></span>
<span data-ttu-id="f0b88-163">S Resource Managerem, můžete nasadit své prostředky pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="f0b88-163">With Resource Manager, you can deploy your resources using templates.</span></span> <span data-ttu-id="f0b88-164">Šablona definuje několik prostředků a jejich závislosti.</span><span class="sxs-lookup"><span data-stu-id="f0b88-164">A template defines several resources and their dependencies.</span></span> <span data-ttu-id="f0b88-165">Pro tento oddíl předpokladu, že jste se seznámili s šablony Resource Manageru a jsme právě ukazují, jak volat rozhraní API hello toomake toostart nasazení.</span><span class="sxs-lookup"><span data-stu-id="f0b88-165">For this section, we assume you are familiar with Resource Manager templates, and we just show you how toomake hello API call toostart deployment.</span></span> <span data-ttu-id="f0b88-166">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f0b88-166">For more information about constructing templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="f0b88-167">Nasazení šablony není liší množství toohow volání jiná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0b88-167">Deployment of a template doesn't differ much toohow you call other APIs.</span></span> <span data-ttu-id="f0b88-168">Jeden aspekt důležité je, že nasazení šablony může trvat poměrně dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="f0b88-168">One important aspect is that deployment of a template can take quite a long time.</span></span> <span data-ttu-id="f0b88-169">volání rozhraní API Hello právě vrátí, a je funkční tooyou jako tooquery vývojáře pro stav nasazení toofind hello se po dokončení nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="f0b88-169">hello API call just returns, and it's up tooyou as developer tooquery for status of hello deployment toofind out when hello deployment is done.</span></span> <span data-ttu-id="f0b88-170">Další informace najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f0b88-170">For more information, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>

<span data-ttu-id="f0b88-171">V tomto příkladu používáme k dispozici na veřejně vystavené šablonu [Githubu](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="f0b88-171">For this example, we use a publicly exposed template available on [GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="f0b88-172">Hello šablonu, kterou používáme nasadí oblast západní USA toohello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f0b88-172">hello template we use deploys a Linux VM toohello West US region.</span></span> <span data-ttu-id="f0b88-173">I když tento příklad používá šablonu k dispozici v veřejného úložiště jako GitHub, můžete místo toho předat kompletní šablonou hello jako součást požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="f0b88-173">Even though this example uses a template available in a public repository like GitHub, you can instead pass hello full template as part of hello request.</span></span> <span data-ttu-id="f0b88-174">Všimněte si, že jsme zadáním hodnot parametru v hello žádosti, které se používají v rámci hello nasadit šablonu.</span><span class="sxs-lookup"><span data-stu-id="f0b88-174">Note that we provide parameter values in hello request that are used inside hello deployed template.</span></span>

<span data-ttu-id="f0b88-175">(Nahraďte ID_ODBĚRU, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD a DNS_NAME_FOR_PUBLIC_IP toovalues vhodné pro vaši žádost o)</span><span class="sxs-lookup"><span data-stu-id="f0b88-175">(Replace SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD and DNS_NAME_FOR_PUBLIC_IP toovalues appropriate for your request)</span></span>

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

<span data-ttu-id="f0b88-176">Hello dlouho JSON odpovědi na tento požadavek byl čitelnější vynechání tooimprove této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="f0b88-176">hello long JSON response for this request has been omitted tooimprove readability of this documentation.</span></span> <span data-ttu-id="f0b88-177">Hello odpovědi obsahuje informace o hello podle šablony nasazení, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f0b88-177">hello response contains information about hello templated deployment that you created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0b88-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0b88-178">Next steps</span></span>

- <span data-ttu-id="f0b88-179">toolearn o zpracování asynchronní operace REST, najdete v části [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f0b88-179">toolearn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
