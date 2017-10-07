---
title: "aaaScheduler odchozí ověření"
description: "Odchozí ověření scheduleru"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 6707f82b-7e32-401b-a960-02aae7bb59cc
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2016
ms.author: deli
ms.openlocfilehash: ef713f4770b48d0a9176415e87c1042a823582e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-outbound-authentication"></a><span data-ttu-id="6e05a-103">Odchozí ověření scheduleru</span><span class="sxs-lookup"><span data-stu-id="6e05a-103">Scheduler Outbound Authentication</span></span>
<span data-ttu-id="6e05a-104">Plánovač úloh může být nutné toocall out tooservices, které vyžadují ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-104">Scheduler jobs may need toocall out tooservices that require authentication.</span></span> <span data-ttu-id="6e05a-105">Tímto způsobem názvem služby můžete určit Pokud hello plánovače úloh můžete přistupovat k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="6e05a-105">This way, a called service can determine if hello Scheduler job can access its resources.</span></span> <span data-ttu-id="6e05a-106">Některé z těchto služeb zahrnují dalším službám Azure, Salesforce.com, Facebook a zabezpečené vlastní weby.</span><span class="sxs-lookup"><span data-stu-id="6e05a-106">Some of these services include other Azure services, Salesforce.com, Facebook, and secure custom websites.</span></span>

## <a name="adding-and-removing-authentication"></a><span data-ttu-id="6e05a-107">Přidávání a odebírání ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-107">Adding and Removing Authentication</span></span>
<span data-ttu-id="6e05a-108">Přidání ověřování tooa Plánovač úloh je jednoduchá – přidání podřízený element JSON `authentication` toohello `request` element při vytváření nebo aktualizaci úlohy.</span><span class="sxs-lookup"><span data-stu-id="6e05a-108">Adding authentication tooa Scheduler job is simple – add a JSON child element `authentication` toohello `request` element when creating or updating a job.</span></span> <span data-ttu-id="6e05a-109">Tajné klíče předá služba Plánovač toohello v požadavku PUT, PATCH nebo POST – jako součást hello `authentication` objektu – nikdy jsou vráceny v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6e05a-109">Secrets passed toohello Scheduler service in a PUT, PATCH, or POST request – as part of hello `authentication` object – are never returned in responses.</span></span> <span data-ttu-id="6e05a-110">V odpovědi tajné informace není nastaven toonull nebo může mít veřejné token, který představuje entitu hello ověření.</span><span class="sxs-lookup"><span data-stu-id="6e05a-110">In responses, secret information is set toonull or may have a public token that represents hello authenticated entity.</span></span>

<span data-ttu-id="6e05a-111">tooremove ověřování, PUT nebo PATCH hello úlohy explicitně, nastavení hello `authentication` objektu toonull.</span><span class="sxs-lookup"><span data-stu-id="6e05a-111">tooremove authentication, PUT or PATCH hello job explicitly, setting hello `authentication` object toonull.</span></span> <span data-ttu-id="6e05a-112">Neuvidíte žádné vlastnosti ověřování zpět v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6e05a-112">You will not see any authentication properties back in response.</span></span>

<span data-ttu-id="6e05a-113">V současné době hello podporovány pouze typy ověřování jsou hello `ClientCertificate` modelu (pro použití klientské certifikáty SSL/TLS hello), hello `Basic` modelu (pro základní ověřování) a hello `ActiveDirectoryOAuth` (pro Active Directory OAuth ověřování.)</span><span class="sxs-lookup"><span data-stu-id="6e05a-113">Currently, hello only supported authentication types are hello `ClientCertificate` model (for using hello SSL/TLS client certificates), hello `Basic` model (for Basic authentication), and hello `ActiveDirectoryOAuth` model (for Active Directory OAuth authentication.)</span></span>

## <a name="request-body-for-clientcertificate-authentication"></a><span data-ttu-id="6e05a-114">Text žádosti pro ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-114">Request Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="6e05a-115">Při přidávání ověřování pomocí hello `ClientCertificate` model, zadejte následující další prvky v textu žádosti hello hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-115">When adding authentication using hello `ClientCertificate` model, specify hello following additional elements in hello request body.</span></span>  

| <span data-ttu-id="6e05a-116">Element</span><span class="sxs-lookup"><span data-stu-id="6e05a-116">Element</span></span> | <span data-ttu-id="6e05a-117">Popis</span><span class="sxs-lookup"><span data-stu-id="6e05a-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6e05a-118">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="6e05a-118">*authentication (parent element)*</span></span> |<span data-ttu-id="6e05a-119">Objekt ověřování pro použití klientský certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="6e05a-119">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="6e05a-120">*Typ*</span><span class="sxs-lookup"><span data-stu-id="6e05a-120">*type*</span></span> |<span data-ttu-id="6e05a-121">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-121">Required.</span></span> <span data-ttu-id="6e05a-122">Typ ověřování. Pro klientské certifikáty protokolu SSL, musí být hodnota hello `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="6e05a-122">Type of authentication.For SSL client certificates, hello value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="6e05a-123">*soubor PFX*</span><span class="sxs-lookup"><span data-stu-id="6e05a-123">*pfx*</span></span> |<span data-ttu-id="6e05a-124">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-124">Required.</span></span> <span data-ttu-id="6e05a-125">Kódování Base64 obsah souboru PFX hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-125">Base64-encoded contents of hello PFX file.</span></span> |
| <span data-ttu-id="6e05a-126">*heslo*</span><span class="sxs-lookup"><span data-stu-id="6e05a-126">*password*</span></span> |<span data-ttu-id="6e05a-127">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-127">Required.</span></span> <span data-ttu-id="6e05a-128">Heslo souboru PFX hello tooaccess.</span><span class="sxs-lookup"><span data-stu-id="6e05a-128">Password tooaccess hello PFX file.</span></span> |

## <a name="response-body-for-clientcertificate-authentication"></a><span data-ttu-id="6e05a-129">Text odpovědi pro ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-129">Response Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="6e05a-130">Když přijde požadavek je s informacemi ověřování, hello odpověď obsahuje hello následující elementy související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-130">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="6e05a-131">Element</span><span class="sxs-lookup"><span data-stu-id="6e05a-131">Element</span></span> | <span data-ttu-id="6e05a-132">Popis</span><span class="sxs-lookup"><span data-stu-id="6e05a-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6e05a-133">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="6e05a-133">*authentication (parent element)*</span></span> |<span data-ttu-id="6e05a-134">Objekt ověřování pro použití klientský certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="6e05a-134">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="6e05a-135">*Typ*</span><span class="sxs-lookup"><span data-stu-id="6e05a-135">*type*</span></span> |<span data-ttu-id="6e05a-136">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-136">Type of authentication.</span></span> <span data-ttu-id="6e05a-137">Pro klientské certifikáty protokolu SSL, je hodnota hello `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="6e05a-137">For SSL client certificates, hello value is `ClientCertificate`.</span></span> |
| <span data-ttu-id="6e05a-138">*certificateThumbprint*</span><span class="sxs-lookup"><span data-stu-id="6e05a-138">*certificateThumbprint*</span></span> |<span data-ttu-id="6e05a-139">Hello kryptografický otisk certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-139">hello thumbprint of hello certificate.</span></span> |
| <span data-ttu-id="6e05a-140">*certificateSubjectName*</span><span class="sxs-lookup"><span data-stu-id="6e05a-140">*certificateSubjectName*</span></span> |<span data-ttu-id="6e05a-141">Hello rozlišující název subjektu certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-141">hello subject distinguished name of hello certificate.</span></span> |
| <span data-ttu-id="6e05a-142">*certificateExpiration*</span><span class="sxs-lookup"><span data-stu-id="6e05a-142">*certificateExpiration*</span></span> |<span data-ttu-id="6e05a-143">Hello datum vypršení platnosti certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-143">hello expiration date of hello certificate.</span></span> |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a><span data-ttu-id="6e05a-144">Ukázka REST žádost o ověření ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="6e05a-144">Sample REST Request for ClientCertificate Authentication</span></span>
```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a><span data-ttu-id="6e05a-145">Ukázková odpověď REST pro ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-145">Sample REST Response for ClientCertificate Authentication</span></span>
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a><span data-ttu-id="6e05a-146">Text žádosti pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-146">Request Body for Basic Authentication</span></span>
<span data-ttu-id="6e05a-147">Při přidávání ověřování pomocí hello `Basic` model, zadejte následující další prvky v textu žádosti hello hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-147">When adding authentication using hello `Basic` model, specify hello following additional elements in hello request body.</span></span>

| <span data-ttu-id="6e05a-148">Element</span><span class="sxs-lookup"><span data-stu-id="6e05a-148">Element</span></span> | <span data-ttu-id="6e05a-149">Popis</span><span class="sxs-lookup"><span data-stu-id="6e05a-149">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6e05a-150">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="6e05a-150">*authentication (parent element)*</span></span> |<span data-ttu-id="6e05a-151">Objekt ověřování pro použití základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-151">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="6e05a-152">*Typ*</span><span class="sxs-lookup"><span data-stu-id="6e05a-152">*type*</span></span> |<span data-ttu-id="6e05a-153">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-153">Required.</span></span> <span data-ttu-id="6e05a-154">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-154">Type of authentication.</span></span> <span data-ttu-id="6e05a-155">Pro základní ověřování, musí být hodnota hello `Basic`.</span><span class="sxs-lookup"><span data-stu-id="6e05a-155">For Basic authentication, hello value must be `Basic`.</span></span> |
| <span data-ttu-id="6e05a-156">*uživatelské jméno*</span><span class="sxs-lookup"><span data-stu-id="6e05a-156">*username*</span></span> |<span data-ttu-id="6e05a-157">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-157">Required.</span></span> <span data-ttu-id="6e05a-158">Tooauthenticate uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="6e05a-158">Username tooauthenticate.</span></span> |
| <span data-ttu-id="6e05a-159">*heslo*</span><span class="sxs-lookup"><span data-stu-id="6e05a-159">*password*</span></span> |<span data-ttu-id="6e05a-160">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-160">Required.</span></span> <span data-ttu-id="6e05a-161">Tooauthenticate heslo.</span><span class="sxs-lookup"><span data-stu-id="6e05a-161">Password tooauthenticate.</span></span> |

## <a name="response-body-for-basic-authentication"></a><span data-ttu-id="6e05a-162">Text odpovědi pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-162">Response Body for Basic Authentication</span></span>
<span data-ttu-id="6e05a-163">Když přijde požadavek je s informacemi ověřování, hello odpověď obsahuje hello následující elementy související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-163">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="6e05a-164">Element</span><span class="sxs-lookup"><span data-stu-id="6e05a-164">Element</span></span> | <span data-ttu-id="6e05a-165">Popis</span><span class="sxs-lookup"><span data-stu-id="6e05a-165">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6e05a-166">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="6e05a-166">*authentication (parent element)*</span></span> |<span data-ttu-id="6e05a-167">Objekt ověřování pro použití základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-167">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="6e05a-168">*Typ*</span><span class="sxs-lookup"><span data-stu-id="6e05a-168">*type*</span></span> |<span data-ttu-id="6e05a-169">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-169">Type of authentication.</span></span> <span data-ttu-id="6e05a-170">Pro základní ověřování, je hodnota hello `Basic`.</span><span class="sxs-lookup"><span data-stu-id="6e05a-170">For Basic authentication, hello value is `Basic`.</span></span> |
| <span data-ttu-id="6e05a-171">*uživatelské jméno*</span><span class="sxs-lookup"><span data-stu-id="6e05a-171">*username*</span></span> |<span data-ttu-id="6e05a-172">Hello ověřit uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="6e05a-172">hello authenticated username.</span></span> |

## <a name="sample-rest-request-for-basic-authentication"></a><span data-ttu-id="6e05a-173">Ukázková žádost REST pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-173">Sample REST Request for Basic Authentication</span></span>
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a><span data-ttu-id="6e05a-174">Ukázková odpověď REST pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-174">Sample REST Response for Basic Authentication</span></span>
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="6e05a-175">Text žádosti pro ActiveDirectoryOAuth ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-175">Request Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="6e05a-176">Při přidávání ověřování pomocí hello `ActiveDirectoryOAuth` model, zadejte následující další prvky v textu žádosti hello hello.</span><span class="sxs-lookup"><span data-stu-id="6e05a-176">When adding authentication using hello `ActiveDirectoryOAuth` model, specify hello following additional elements in hello request body.</span></span>

| <span data-ttu-id="6e05a-177">Element</span><span class="sxs-lookup"><span data-stu-id="6e05a-177">Element</span></span> | <span data-ttu-id="6e05a-178">Popis</span><span class="sxs-lookup"><span data-stu-id="6e05a-178">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6e05a-179">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="6e05a-179">*authentication (parent element)*</span></span> |<span data-ttu-id="6e05a-180">Objekt ověřování pro použití ActiveDirectoryOAuth ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-180">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="6e05a-181">*Typ*</span><span class="sxs-lookup"><span data-stu-id="6e05a-181">*type*</span></span> |<span data-ttu-id="6e05a-182">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-182">Required.</span></span> <span data-ttu-id="6e05a-183">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-183">Type of authentication.</span></span> <span data-ttu-id="6e05a-184">ActiveDirectoryOAuth ověřování, musí být hodnota hello `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="6e05a-184">For ActiveDirectoryOAuth authentication, hello value must be `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="6e05a-185">*klienta*</span><span class="sxs-lookup"><span data-stu-id="6e05a-185">*tenant*</span></span> |<span data-ttu-id="6e05a-186">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-186">Required.</span></span> <span data-ttu-id="6e05a-187">Hello identifikátor klienta pro klienta hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e05a-187">hello tenant identifier for hello Azure AD tenant.</span></span> |
| <span data-ttu-id="6e05a-188">*Cílová skupina*</span><span class="sxs-lookup"><span data-stu-id="6e05a-188">*audience*</span></span> |<span data-ttu-id="6e05a-189">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-189">Required.</span></span> <span data-ttu-id="6e05a-190">Toto nastavení toohttps://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="6e05a-190">This is set toohttps://management.core.windows.net/.</span></span> |
| <span data-ttu-id="6e05a-191">*clientId*</span><span class="sxs-lookup"><span data-stu-id="6e05a-191">*clientId*</span></span> |<span data-ttu-id="6e05a-192">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-192">Required.</span></span> <span data-ttu-id="6e05a-193">Zadejte identifikátor klienta hello hello aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e05a-193">Provide hello client identifier for hello Azure AD application.</span></span> |
| <span data-ttu-id="6e05a-194">*tajný klíč*</span><span class="sxs-lookup"><span data-stu-id="6e05a-194">*secret*</span></span> |<span data-ttu-id="6e05a-195">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="6e05a-195">Required.</span></span> <span data-ttu-id="6e05a-196">Tajný klíč klienta hello, který požaduje hello token.</span><span class="sxs-lookup"><span data-stu-id="6e05a-196">Secret of hello client that is requesting hello token.</span></span> |

### <a name="determining-your-tenant-identifier"></a><span data-ttu-id="6e05a-197">Určení ID vašeho klienta</span><span class="sxs-lookup"><span data-stu-id="6e05a-197">Determining your Tenant Identifier</span></span>
<span data-ttu-id="6e05a-198">Identifikátor hello klienta pro klienta hello Azure AD můžete najít spuštěním `Get-AzureAccount` v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e05a-198">You can find hello tenant identifier for hello Azure AD tenant by running `Get-AzureAccount` in Azure PowerShell.</span></span>

## <a name="response-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="6e05a-199">Text odpovědi pro ActiveDirectoryOAuth ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-199">Response Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="6e05a-200">Když přijde požadavek je s informacemi ověřování, hello odpověď obsahuje hello následující elementy související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-200">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="6e05a-201">Element</span><span class="sxs-lookup"><span data-stu-id="6e05a-201">Element</span></span> | <span data-ttu-id="6e05a-202">Popis</span><span class="sxs-lookup"><span data-stu-id="6e05a-202">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="6e05a-203">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="6e05a-203">*authentication (parent element)*</span></span> |<span data-ttu-id="6e05a-204">Objekt ověřování pro použití ActiveDirectoryOAuth ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-204">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="6e05a-205">*Typ*</span><span class="sxs-lookup"><span data-stu-id="6e05a-205">*type*</span></span> |<span data-ttu-id="6e05a-206">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="6e05a-206">Type of authentication.</span></span> <span data-ttu-id="6e05a-207">Pro ověřování ActiveDirectoryOAuth hello hodnota je `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="6e05a-207">For ActiveDirectoryOAuth authentication, hello value is `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="6e05a-208">*klienta*</span><span class="sxs-lookup"><span data-stu-id="6e05a-208">*tenant*</span></span> |<span data-ttu-id="6e05a-209">Hello identifikátor klienta pro klienta hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e05a-209">hello tenant identifier for hello Azure AD tenant.</span></span> |
| <span data-ttu-id="6e05a-210">*Cílová skupina*</span><span class="sxs-lookup"><span data-stu-id="6e05a-210">*audience*</span></span> |<span data-ttu-id="6e05a-211">Toto nastavení toohttps://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="6e05a-211">This is set toohttps://management.core.windows.net/.</span></span> |
| <span data-ttu-id="6e05a-212">*clientId*</span><span class="sxs-lookup"><span data-stu-id="6e05a-212">*clientId*</span></span> |<span data-ttu-id="6e05a-213">Hello identifikátor klienta pro aplikaci hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e05a-213">hello client identifier for hello Azure AD application.</span></span> |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a><span data-ttu-id="6e05a-214">Ukázka REST žádost o ověření ActiveDirectoryOAuth</span><span class="sxs-lookup"><span data-stu-id="6e05a-214">Sample REST Request for ActiveDirectoryOAuth Authentication</span></span>
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a><span data-ttu-id="6e05a-215">Ukázková odpověď REST pro ActiveDirectoryOAuth ověřování</span><span class="sxs-lookup"><span data-stu-id="6e05a-215">Sample REST Response for ActiveDirectoryOAuth Authentication</span></span>
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a><span data-ttu-id="6e05a-216">Viz také</span><span class="sxs-lookup"><span data-stu-id="6e05a-216">See Also</span></span>
 [<span data-ttu-id="6e05a-217">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="6e05a-217">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="6e05a-218">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="6e05a-218">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="6e05a-219">Začněte používat plánovače v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e05a-219">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="6e05a-220">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="6e05a-220">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="6e05a-221">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="6e05a-221">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="6e05a-222">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="6e05a-222">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="6e05a-223">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="6e05a-223">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="6e05a-224">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="6e05a-224">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

