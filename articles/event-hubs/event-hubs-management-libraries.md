---
title: "knihovny správy aaaAzure Event Hubs | Microsoft Docs"
description: "Správa oborů názvů Event Hubs a entity z rozhraní .NET"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b7db0077f6f31397ae46e926c3c28630a157824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-management-libraries"></a><span data-ttu-id="77db6-103">Knihovny správy centra událostí</span><span class="sxs-lookup"><span data-stu-id="77db6-103">Event Hubs management libraries</span></span>

<span data-ttu-id="77db6-104">knihovny správy Hello Event Hubs můžete dynamicky zajišťují Event Hubs obory názvů a entity.</span><span class="sxs-lookup"><span data-stu-id="77db6-104">hello Event Hubs management libraries can dynamically provision Event Hubs namespaces and entities.</span></span> <span data-ttu-id="77db6-105">To umožňuje komplexní nasazení a scénáře zasílání zpráv, tak, aby prostřednictvím kódu programu můžete určit, jaké tooprovision entity.</span><span class="sxs-lookup"><span data-stu-id="77db6-105">This enables complex deployments and messaging scenarios, so that you can programmatically determine what entities tooprovision.</span></span> <span data-ttu-id="77db6-106">Tyto knihovny jsou aktuálně dostupné pro rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="77db6-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="77db6-107">Podporované funkce</span><span class="sxs-lookup"><span data-stu-id="77db6-107">Supported functionality</span></span>

* <span data-ttu-id="77db6-108">Namespace vytváření, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="77db6-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="77db6-109">Vytvoření centra událostí, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="77db6-109">Event Hubs creation, update, deletion</span></span>
* <span data-ttu-id="77db6-110">Vytvoření skupiny uživatelů, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="77db6-110">Consumer Group creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77db6-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="77db6-111">Prerequisites</span></span>

<span data-ttu-id="77db6-112">tooget pomocí knihovny správy Event Hubs hello spuštěna, je třeba ověřit s Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="77db6-112">tooget started using hello Event Hubs management libraries, you must authenticate with Azure Active Directory (AAD).</span></span> <span data-ttu-id="77db6-113">AAD vyžaduje ověřování jako hlavní název služby, který poskytuje přístup tooyour prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="77db6-113">AAD requires that you authenticate as a service principal, which provides access tooyour Azure resources.</span></span> <span data-ttu-id="77db6-114">Informace o vytvoření objektu služby najdete v jednom z těchto článků:</span><span class="sxs-lookup"><span data-stu-id="77db6-114">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="77db6-115">Pomocí aplikace hello portálu toocreate Azure Active Directory a objektu služby, které mají přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="77db6-115">Use hello Azure portal toocreate Active Directory application and service principal that can access resources</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="77db6-116">Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="77db6-116">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="77db6-117">Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="77db6-117">Use Azure CLI toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

<span data-ttu-id="77db6-118">Tyto kurzy vám poskytují `AppId` (ID klienta), `TenantId`, a `ClientSecret` (ověřovací klíč), které se používají pro ověřování pomocí knihovny správy hello.</span><span class="sxs-lookup"><span data-stu-id="77db6-118">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by hello management libraries.</span></span> <span data-ttu-id="77db6-119">Musíte mít **vlastníka** oprávnění pro skupinu prostředků hello, na kterém chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="77db6-119">You must have **Owner** permissions for hello resource group on which you want toorun.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="77db6-120">Vzor programování</span><span class="sxs-lookup"><span data-stu-id="77db6-120">Programming pattern</span></span>

<span data-ttu-id="77db6-121">Hello toomanipulate vzor odpovídá jakémukoli prostředku, Event Hubs společný protokol:</span><span class="sxs-lookup"><span data-stu-id="77db6-121">hello pattern toomanipulate any Event Hubs resource follows a common protocol:</span></span>

1. <span data-ttu-id="77db6-122">Získání tokenu z AAD pomocí hello `Microsoft.IdentityModel.Clients.ActiveDirectory` knihovny.</span><span class="sxs-lookup"><span data-stu-id="77db6-122">Obtain a token from AAD using hello `Microsoft.IdentityModel.Clients.ActiveDirectory` library.</span></span>
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. <span data-ttu-id="77db6-123">Vytvoření hello `EventHubManagementClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="77db6-123">Create hello `EventHubManagementClient` object.</span></span>
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. <span data-ttu-id="77db6-124">Sada hello `CreateOrUpdate` tooyour parametry zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="77db6-124">Set hello `CreateOrUpdate` parameters tooyour specified values.</span></span>
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. <span data-ttu-id="77db6-125">Spusťte volání hello.</span><span class="sxs-lookup"><span data-stu-id="77db6-125">Execute hello call.</span></span>
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a><span data-ttu-id="77db6-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77db6-126">Next steps</span></span>
* [<span data-ttu-id="77db6-127">Ukázka správy rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="77db6-127">.NET Management sample</span></span>](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [<span data-ttu-id="77db6-128">Odkaz na Microsoft.Azure.Management.EventHub</span><span class="sxs-lookup"><span data-stu-id="77db6-128">Microsoft.Azure.Management.EventHub Reference</span></span>](/dotnet/api/Microsoft.Azure.Management.EventHub) 
