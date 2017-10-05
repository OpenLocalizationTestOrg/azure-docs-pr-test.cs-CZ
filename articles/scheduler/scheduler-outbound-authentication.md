---
title: "Odchozí ověření scheduleru"
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
ms.openlocfilehash: e345b2e22daae5b24c23645f7d2636f66df630ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-outbound-authentication"></a><span data-ttu-id="14fa8-103">Odchozí ověření scheduleru</span><span class="sxs-lookup"><span data-stu-id="14fa8-103">Scheduler Outbound Authentication</span></span>
<span data-ttu-id="14fa8-104">Plánovač úloh možná muset volat na služby, které vyžadují ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-104">Scheduler jobs may need to call out to services that require authentication.</span></span> <span data-ttu-id="14fa8-105">Tímto způsobem názvem služby můžete určit Pokud úlohy plánovače může přistupovat k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="14fa8-105">This way, a called service can determine if the Scheduler job can access its resources.</span></span> <span data-ttu-id="14fa8-106">Některé z těchto služeb zahrnují dalším službám Azure, Salesforce.com, Facebook a zabezpečené vlastní weby.</span><span class="sxs-lookup"><span data-stu-id="14fa8-106">Some of these services include other Azure services, Salesforce.com, Facebook, and secure custom websites.</span></span>

## <a name="adding-and-removing-authentication"></a><span data-ttu-id="14fa8-107">Přidávání a odebírání ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-107">Adding and Removing Authentication</span></span>
<span data-ttu-id="14fa8-108">Přidání ověřování do úlohy plánovače je jednoduchá – přidání podřízený element JSON `authentication` k `request` element při vytváření nebo aktualizaci úlohy.</span><span class="sxs-lookup"><span data-stu-id="14fa8-108">Adding authentication to a Scheduler job is simple – add a JSON child element `authentication` to the `request` element when creating or updating a job.</span></span> <span data-ttu-id="14fa8-109">Tajné klíče předaný služby plánovače v požadavku PUT, PATCH nebo POST – jako součást `authentication` objektu – nikdy jsou vráceny v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="14fa8-109">Secrets passed to the Scheduler service in a PUT, PATCH, or POST request – as part of the `authentication` object – are never returned in responses.</span></span> <span data-ttu-id="14fa8-110">V odpovědi, tajné informace nastavena na hodnotu null, nebo může mít veřejné token, který představuje ověřený entitu.</span><span class="sxs-lookup"><span data-stu-id="14fa8-110">In responses, secret information is set to null or may have a public token that represents the authenticated entity.</span></span>

<span data-ttu-id="14fa8-111">Pokud chcete odebrat ověřování, PUT nebo PATCH úlohu explicitně, nastavení `authentication` objekt, který má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="14fa8-111">To remove authentication, PUT or PATCH the job explicitly, setting the `authentication` object to null.</span></span> <span data-ttu-id="14fa8-112">Neuvidíte žádné vlastnosti ověřování zpět v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="14fa8-112">You will not see any authentication properties back in response.</span></span>

<span data-ttu-id="14fa8-113">V současné době jsou typy podporované jenom ověřování `ClientCertificate` (pro použití protokolu SSL/TLS klientské certifikáty), `Basic` (pro základní ověřování) a `ActiveDirectoryOAuth` (pro ověřování Active Directory OAuth.)</span><span class="sxs-lookup"><span data-stu-id="14fa8-113">Currently, the only supported authentication types are the `ClientCertificate` model (for using the SSL/TLS client certificates), the `Basic` model (for Basic authentication), and the `ActiveDirectoryOAuth` model (for Active Directory OAuth authentication.)</span></span>

## <a name="request-body-for-clientcertificate-authentication"></a><span data-ttu-id="14fa8-114">Text žádosti pro ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-114">Request Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="14fa8-115">Při přidávání ověřování pomocí `ClientCertificate` model, zadejte následující další prvky v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="14fa8-115">When adding authentication using the `ClientCertificate` model, specify the following additional elements in the request body.</span></span>  

| <span data-ttu-id="14fa8-116">Element</span><span class="sxs-lookup"><span data-stu-id="14fa8-116">Element</span></span> | <span data-ttu-id="14fa8-117">Popis</span><span class="sxs-lookup"><span data-stu-id="14fa8-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14fa8-118">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="14fa8-118">*authentication (parent element)*</span></span> |<span data-ttu-id="14fa8-119">Objekt ověřování pro použití klientský certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="14fa8-119">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="14fa8-120">*Typ*</span><span class="sxs-lookup"><span data-stu-id="14fa8-120">*type*</span></span> |<span data-ttu-id="14fa8-121">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-121">Required.</span></span> <span data-ttu-id="14fa8-122">Typ ověřování. Pro klientské certifikáty protokolu SSL, hodnota musí být `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="14fa8-122">Type of authentication.For SSL client certificates, the value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="14fa8-123">*soubor PFX*</span><span class="sxs-lookup"><span data-stu-id="14fa8-123">*pfx*</span></span> |<span data-ttu-id="14fa8-124">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-124">Required.</span></span> <span data-ttu-id="14fa8-125">Kódování Base64 obsah souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="14fa8-125">Base64-encoded contents of the PFX file.</span></span> |
| <span data-ttu-id="14fa8-126">*heslo*</span><span class="sxs-lookup"><span data-stu-id="14fa8-126">*password*</span></span> |<span data-ttu-id="14fa8-127">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-127">Required.</span></span> <span data-ttu-id="14fa8-128">Heslo pro přístup k souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="14fa8-128">Password to access the PFX file.</span></span> |

## <a name="response-body-for-clientcertificate-authentication"></a><span data-ttu-id="14fa8-129">Text odpovědi pro ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-129">Response Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="14fa8-130">Když přijde požadavek je s informacemi ověřování, odpověď obsahuje následující prvky souvisejících s ověřováním.</span><span class="sxs-lookup"><span data-stu-id="14fa8-130">When a request is sent with authentication info, the response contains the following authentication-related elements.</span></span>

| <span data-ttu-id="14fa8-131">Element</span><span class="sxs-lookup"><span data-stu-id="14fa8-131">Element</span></span> | <span data-ttu-id="14fa8-132">Popis</span><span class="sxs-lookup"><span data-stu-id="14fa8-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14fa8-133">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="14fa8-133">*authentication (parent element)*</span></span> |<span data-ttu-id="14fa8-134">Objekt ověřování pro použití klientský certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="14fa8-134">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="14fa8-135">*Typ*</span><span class="sxs-lookup"><span data-stu-id="14fa8-135">*type*</span></span> |<span data-ttu-id="14fa8-136">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-136">Type of authentication.</span></span> <span data-ttu-id="14fa8-137">Pro klientské certifikáty protokolu SSL, je hodnota `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="14fa8-137">For SSL client certificates, the value is `ClientCertificate`.</span></span> |
| <span data-ttu-id="14fa8-138">*certificateThumbprint*</span><span class="sxs-lookup"><span data-stu-id="14fa8-138">*certificateThumbprint*</span></span> |<span data-ttu-id="14fa8-139">Kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="14fa8-139">The thumbprint of the certificate.</span></span> |
| <span data-ttu-id="14fa8-140">*certificateSubjectName*</span><span class="sxs-lookup"><span data-stu-id="14fa8-140">*certificateSubjectName*</span></span> |<span data-ttu-id="14fa8-141">Rozlišující název subjektu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="14fa8-141">The subject distinguished name of the certificate.</span></span> |
| <span data-ttu-id="14fa8-142">*certificateExpiration*</span><span class="sxs-lookup"><span data-stu-id="14fa8-142">*certificateExpiration*</span></span> |<span data-ttu-id="14fa8-143">Datum vypršení platnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="14fa8-143">The expiration date of the certificate.</span></span> |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a><span data-ttu-id="14fa8-144">Ukázka REST žádost o ověření ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="14fa8-144">Sample REST Request for ClientCertificate Authentication</span></span>
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

## <a name="sample-rest-response-for-clientcertificate-authentication"></a><span data-ttu-id="14fa8-145">Ukázková odpověď REST pro ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-145">Sample REST Response for ClientCertificate Authentication</span></span>
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

## <a name="request-body-for-basic-authentication"></a><span data-ttu-id="14fa8-146">Text žádosti pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-146">Request Body for Basic Authentication</span></span>
<span data-ttu-id="14fa8-147">Při přidávání ověřování pomocí `Basic` model, zadejte následující další prvky v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="14fa8-147">When adding authentication using the `Basic` model, specify the following additional elements in the request body.</span></span>

| <span data-ttu-id="14fa8-148">Element</span><span class="sxs-lookup"><span data-stu-id="14fa8-148">Element</span></span> | <span data-ttu-id="14fa8-149">Popis</span><span class="sxs-lookup"><span data-stu-id="14fa8-149">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14fa8-150">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="14fa8-150">*authentication (parent element)*</span></span> |<span data-ttu-id="14fa8-151">Objekt ověřování pro použití základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-151">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="14fa8-152">*Typ*</span><span class="sxs-lookup"><span data-stu-id="14fa8-152">*type*</span></span> |<span data-ttu-id="14fa8-153">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-153">Required.</span></span> <span data-ttu-id="14fa8-154">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-154">Type of authentication.</span></span> <span data-ttu-id="14fa8-155">Pro základní ověřování, musí být hodnota `Basic`.</span><span class="sxs-lookup"><span data-stu-id="14fa8-155">For Basic authentication, the value must be `Basic`.</span></span> |
| <span data-ttu-id="14fa8-156">*uživatelské jméno*</span><span class="sxs-lookup"><span data-stu-id="14fa8-156">*username*</span></span> |<span data-ttu-id="14fa8-157">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-157">Required.</span></span> <span data-ttu-id="14fa8-158">Uživatelské jméno k ověření.</span><span class="sxs-lookup"><span data-stu-id="14fa8-158">Username to authenticate.</span></span> |
| <span data-ttu-id="14fa8-159">*heslo*</span><span class="sxs-lookup"><span data-stu-id="14fa8-159">*password*</span></span> |<span data-ttu-id="14fa8-160">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-160">Required.</span></span> <span data-ttu-id="14fa8-161">Heslo k ověření.</span><span class="sxs-lookup"><span data-stu-id="14fa8-161">Password to authenticate.</span></span> |

## <a name="response-body-for-basic-authentication"></a><span data-ttu-id="14fa8-162">Text odpovědi pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-162">Response Body for Basic Authentication</span></span>
<span data-ttu-id="14fa8-163">Když přijde požadavek je s informacemi ověřování, odpověď obsahuje následující prvky souvisejících s ověřováním.</span><span class="sxs-lookup"><span data-stu-id="14fa8-163">When a request is sent with authentication info, the response contains the following authentication-related elements.</span></span>

| <span data-ttu-id="14fa8-164">Element</span><span class="sxs-lookup"><span data-stu-id="14fa8-164">Element</span></span> | <span data-ttu-id="14fa8-165">Popis</span><span class="sxs-lookup"><span data-stu-id="14fa8-165">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14fa8-166">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="14fa8-166">*authentication (parent element)*</span></span> |<span data-ttu-id="14fa8-167">Objekt ověřování pro použití základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-167">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="14fa8-168">*Typ*</span><span class="sxs-lookup"><span data-stu-id="14fa8-168">*type*</span></span> |<span data-ttu-id="14fa8-169">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-169">Type of authentication.</span></span> <span data-ttu-id="14fa8-170">Pro základní ověřování, je hodnota `Basic`.</span><span class="sxs-lookup"><span data-stu-id="14fa8-170">For Basic authentication, the value is `Basic`.</span></span> |
| <span data-ttu-id="14fa8-171">*uživatelské jméno*</span><span class="sxs-lookup"><span data-stu-id="14fa8-171">*username*</span></span> |<span data-ttu-id="14fa8-172">Ověřené uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="14fa8-172">The authenticated username.</span></span> |

## <a name="sample-rest-request-for-basic-authentication"></a><span data-ttu-id="14fa8-173">Ukázková žádost REST pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-173">Sample REST Request for Basic Authentication</span></span>
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

## <a name="sample-rest-response-for-basic-authentication"></a><span data-ttu-id="14fa8-174">Ukázková odpověď REST pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-174">Sample REST Response for Basic Authentication</span></span>
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

## <a name="request-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="14fa8-175">Text žádosti pro ActiveDirectoryOAuth ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-175">Request Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="14fa8-176">Při přidávání ověřování pomocí `ActiveDirectoryOAuth` model, zadejte následující další prvky v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="14fa8-176">When adding authentication using the `ActiveDirectoryOAuth` model, specify the following additional elements in the request body.</span></span>

| <span data-ttu-id="14fa8-177">Element</span><span class="sxs-lookup"><span data-stu-id="14fa8-177">Element</span></span> | <span data-ttu-id="14fa8-178">Popis</span><span class="sxs-lookup"><span data-stu-id="14fa8-178">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14fa8-179">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="14fa8-179">*authentication (parent element)*</span></span> |<span data-ttu-id="14fa8-180">Objekt ověřování pro použití ActiveDirectoryOAuth ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-180">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="14fa8-181">*Typ*</span><span class="sxs-lookup"><span data-stu-id="14fa8-181">*type*</span></span> |<span data-ttu-id="14fa8-182">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-182">Required.</span></span> <span data-ttu-id="14fa8-183">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-183">Type of authentication.</span></span> <span data-ttu-id="14fa8-184">ActiveDirectoryOAuth ověřování, musí být hodnota `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="14fa8-184">For ActiveDirectoryOAuth authentication, the value must be `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="14fa8-185">*klienta*</span><span class="sxs-lookup"><span data-stu-id="14fa8-185">*tenant*</span></span> |<span data-ttu-id="14fa8-186">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-186">Required.</span></span> <span data-ttu-id="14fa8-187">Identifikátor klienta pro klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14fa8-187">The tenant identifier for the Azure AD tenant.</span></span> |
| <span data-ttu-id="14fa8-188">*Cílová skupina*</span><span class="sxs-lookup"><span data-stu-id="14fa8-188">*audience*</span></span> |<span data-ttu-id="14fa8-189">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-189">Required.</span></span> <span data-ttu-id="14fa8-190">To je nastavena na https://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="14fa8-190">This is set to https://management.core.windows.net/.</span></span> |
| <span data-ttu-id="14fa8-191">*clientId*</span><span class="sxs-lookup"><span data-stu-id="14fa8-191">*clientId*</span></span> |<span data-ttu-id="14fa8-192">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-192">Required.</span></span> <span data-ttu-id="14fa8-193">Zadejte identifikátor klienta aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14fa8-193">Provide the client identifier for the Azure AD application.</span></span> |
| <span data-ttu-id="14fa8-194">*tajný klíč*</span><span class="sxs-lookup"><span data-stu-id="14fa8-194">*secret*</span></span> |<span data-ttu-id="14fa8-195">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="14fa8-195">Required.</span></span> <span data-ttu-id="14fa8-196">Tajný klíč klienta, který vyžaduje token.</span><span class="sxs-lookup"><span data-stu-id="14fa8-196">Secret of the client that is requesting the token.</span></span> |

### <a name="determining-your-tenant-identifier"></a><span data-ttu-id="14fa8-197">Určení ID vašeho klienta</span><span class="sxs-lookup"><span data-stu-id="14fa8-197">Determining your Tenant Identifier</span></span>
<span data-ttu-id="14fa8-198">Identifikátor klienta pro klienta služby Azure AD můžete najít spuštěním `Get-AzureAccount` v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="14fa8-198">You can find the tenant identifier for the Azure AD tenant by running `Get-AzureAccount` in Azure PowerShell.</span></span>

## <a name="response-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="14fa8-199">Text odpovědi pro ActiveDirectoryOAuth ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-199">Response Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="14fa8-200">Když přijde požadavek je s informacemi ověřování, odpověď obsahuje následující prvky souvisejících s ověřováním.</span><span class="sxs-lookup"><span data-stu-id="14fa8-200">When a request is sent with authentication info, the response contains the following authentication-related elements.</span></span>

| <span data-ttu-id="14fa8-201">Element</span><span class="sxs-lookup"><span data-stu-id="14fa8-201">Element</span></span> | <span data-ttu-id="14fa8-202">Popis</span><span class="sxs-lookup"><span data-stu-id="14fa8-202">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="14fa8-203">*ověřování (nadřazený element)*</span><span class="sxs-lookup"><span data-stu-id="14fa8-203">*authentication (parent element)*</span></span> |<span data-ttu-id="14fa8-204">Objekt ověřování pro použití ActiveDirectoryOAuth ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-204">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="14fa8-205">*Typ*</span><span class="sxs-lookup"><span data-stu-id="14fa8-205">*type*</span></span> |<span data-ttu-id="14fa8-206">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="14fa8-206">Type of authentication.</span></span> <span data-ttu-id="14fa8-207">Pro ověřování ActiveDirectoryOAuth hodnota je `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="14fa8-207">For ActiveDirectoryOAuth authentication, the value is `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="14fa8-208">*klienta*</span><span class="sxs-lookup"><span data-stu-id="14fa8-208">*tenant*</span></span> |<span data-ttu-id="14fa8-209">Identifikátor klienta pro klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14fa8-209">The tenant identifier for the Azure AD tenant.</span></span> |
| <span data-ttu-id="14fa8-210">*Cílová skupina*</span><span class="sxs-lookup"><span data-stu-id="14fa8-210">*audience*</span></span> |<span data-ttu-id="14fa8-211">To je nastavena na https://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="14fa8-211">This is set to https://management.core.windows.net/.</span></span> |
| <span data-ttu-id="14fa8-212">*clientId*</span><span class="sxs-lookup"><span data-stu-id="14fa8-212">*clientId*</span></span> |<span data-ttu-id="14fa8-213">Identifikátor klienta aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14fa8-213">The client identifier for the Azure AD application.</span></span> |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a><span data-ttu-id="14fa8-214">Ukázka REST žádost o ověření ActiveDirectoryOAuth</span><span class="sxs-lookup"><span data-stu-id="14fa8-214">Sample REST Request for ActiveDirectoryOAuth Authentication</span></span>
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

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a><span data-ttu-id="14fa8-215">Ukázková odpověď REST pro ActiveDirectoryOAuth ověřování</span><span class="sxs-lookup"><span data-stu-id="14fa8-215">Sample REST Response for ActiveDirectoryOAuth Authentication</span></span>
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

## <a name="see-also"></a><span data-ttu-id="14fa8-216">Viz také</span><span class="sxs-lookup"><span data-stu-id="14fa8-216">See Also</span></span>
 [<span data-ttu-id="14fa8-217">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="14fa8-217">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="14fa8-218">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="14fa8-218">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="14fa8-219">Úvod do používání Scheduleru na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="14fa8-219">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="14fa8-220">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="14fa8-220">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="14fa8-221">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="14fa8-221">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="14fa8-222">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="14fa8-222">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="14fa8-223">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="14fa8-223">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="14fa8-224">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="14fa8-224">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

