---
title: "Správa knihovny Azure Event Hubs | Microsoft Docs"
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
ms.openlocfilehash: 0d659cb860a6c98342b548212820efe046decfcc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-management-libraries"></a><span data-ttu-id="d8c25-103">Knihovny správy centra událostí</span><span class="sxs-lookup"><span data-stu-id="d8c25-103">Event Hubs management libraries</span></span>

<span data-ttu-id="d8c25-104">Knihovny správy Event Hubs můžete dynamicky zajišťují Event Hubs obory názvů a entity.</span><span class="sxs-lookup"><span data-stu-id="d8c25-104">The Event Hubs management libraries can dynamically provision Event Hubs namespaces and entities.</span></span> <span data-ttu-id="d8c25-105">To umožňuje komplexní nasazení a scénáře zasílání zpráv, tak, aby prostřednictvím kódu programu můžete určit, jaké entity zřídit.</span><span class="sxs-lookup"><span data-stu-id="d8c25-105">This enables complex deployments and messaging scenarios, so that you can programmatically determine what entities to provision.</span></span> <span data-ttu-id="d8c25-106">Tyto knihovny jsou aktuálně dostupné pro rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="d8c25-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="d8c25-107">Podporované funkce</span><span class="sxs-lookup"><span data-stu-id="d8c25-107">Supported functionality</span></span>

* <span data-ttu-id="d8c25-108">Namespace vytváření, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="d8c25-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="d8c25-109">Vytvoření centra událostí, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="d8c25-109">Event Hubs creation, update, deletion</span></span>
* <span data-ttu-id="d8c25-110">Vytvoření skupiny uživatelů, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="d8c25-110">Consumer Group creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8c25-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8c25-111">Prerequisites</span></span>

<span data-ttu-id="d8c25-112">Abyste mohli začít používat knihovny správy Event Hubs, je třeba ověřit s Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="d8c25-112">To get started using the Event Hubs management libraries, you must authenticate with Azure Active Directory (AAD).</span></span> <span data-ttu-id="d8c25-113">AAD vyžaduje ověřování jako hlavní název služby, která poskytuje přístup k prostředkům Azure.</span><span class="sxs-lookup"><span data-stu-id="d8c25-113">AAD requires that you authenticate as a service principal, which provides access to your Azure resources.</span></span> <span data-ttu-id="d8c25-114">Informace o vytvoření objektu služby najdete v jednom z těchto článků:</span><span class="sxs-lookup"><span data-stu-id="d8c25-114">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="d8c25-115">Pomocí portálu Azure k vytvoření aplikace Active Directory a objektu služby, které mají přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="d8c25-115">Use the Azure portal to create Active Directory application and service principal that can access resources</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="d8c25-116">Vytvoření instančního objektu pro přístup k prostředkům pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="d8c25-116">Use Azure PowerShell to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="d8c25-117">Vytvoření instančního objektu pro přístup k prostředkům pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d8c25-117">Use Azure CLI to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

<span data-ttu-id="d8c25-118">Tyto kurzy vám poskytují `AppId` (ID klienta), `TenantId`, a `ClientSecret` (ověřovací klíč), které se používají pro ověřování pomocí knihovny správy.</span><span class="sxs-lookup"><span data-stu-id="d8c25-118">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by the management libraries.</span></span> <span data-ttu-id="d8c25-119">Musíte mít **vlastníka** oprávnění pro skupinu prostředků, na kterém chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="d8c25-119">You must have **Owner** permissions for the resource group on which you want to run.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="d8c25-120">Vzor programování</span><span class="sxs-lookup"><span data-stu-id="d8c25-120">Programming pattern</span></span>

<span data-ttu-id="d8c25-121">Vzor k manipulaci s jakýmikoli prostředky služby Event Hubs dodržuje společný protokol:</span><span class="sxs-lookup"><span data-stu-id="d8c25-121">The pattern to manipulate any Event Hubs resource follows a common protocol:</span></span>

1. <span data-ttu-id="d8c25-122">Získat token pomocí AAD `Microsoft.IdentityModel.Clients.ActiveDirectory` knihovny.</span><span class="sxs-lookup"><span data-stu-id="d8c25-122">Obtain a token from AAD using the `Microsoft.IdentityModel.Clients.ActiveDirectory` library.</span></span>
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. <span data-ttu-id="d8c25-123">Vytvořte `EventHubManagementClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="d8c25-123">Create the `EventHubManagementClient` object.</span></span>
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. <span data-ttu-id="d8c25-124">Nastavte `CreateOrUpdate` parametry pro zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d8c25-124">Set the `CreateOrUpdate` parameters to your specified values.</span></span>
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. <span data-ttu-id="d8c25-125">Spusťte volání.</span><span class="sxs-lookup"><span data-stu-id="d8c25-125">Execute the call.</span></span>
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a><span data-ttu-id="d8c25-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8c25-126">Next steps</span></span>
* [<span data-ttu-id="d8c25-127">Ukázka správy rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="d8c25-127">.NET Management sample</span></span>](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [<span data-ttu-id="d8c25-128">Odkaz na Microsoft.Azure.Management.EventHub</span><span class="sxs-lookup"><span data-stu-id="d8c25-128">Microsoft.Azure.Management.EventHub Reference</span></span>](/dotnet/api/Microsoft.Azure.Management.EventHub) 
