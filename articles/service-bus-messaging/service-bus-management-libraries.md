---
title: "knihovny správy aaaAzure Service Bus | Microsoft Docs"
description: "Správa oborů názvů Service Bus a entity z rozhraní .NET pro zasílání zpráv."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: 9e4ad91f22815ca0838e6e4647a3606109b2b441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-management-libraries"></a><span data-ttu-id="74891-103">Správa knihovny Service Bus</span><span class="sxs-lookup"><span data-stu-id="74891-103">Service Bus management libraries</span></span>

<span data-ttu-id="74891-104">knihovny Azure Service Bus správy Hello můžete dynamicky zajišťují obory názvů Service Bus a entity.</span><span class="sxs-lookup"><span data-stu-id="74891-104">hello Azure Service Bus management libraries can dynamically provision Service Bus namespaces and entities.</span></span> <span data-ttu-id="74891-105">To umožňuje komplexní nasazení a scénáře zasílání zpráv a umožňuje tooprogrammatically určit, jaké tooprovision entity.</span><span class="sxs-lookup"><span data-stu-id="74891-105">This enables complex deployments and messaging scenarios, and makes it possible tooprogrammatically determine what entities tooprovision.</span></span> <span data-ttu-id="74891-106">Tyto knihovny jsou aktuálně dostupné pro rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="74891-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="74891-107">Podporované funkce</span><span class="sxs-lookup"><span data-stu-id="74891-107">Supported functionality</span></span>

* <span data-ttu-id="74891-108">Namespace vytváření, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="74891-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="74891-109">Vytvoření fronty, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="74891-109">Queue creation, update, deletion</span></span>
* <span data-ttu-id="74891-110">Téma vytváření, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="74891-110">Topic creation, update, deletion</span></span>
* <span data-ttu-id="74891-111">Vytvoření odběru, aktualizaci, odstranění</span><span class="sxs-lookup"><span data-stu-id="74891-111">Subscription creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74891-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74891-112">Prerequisites</span></span>

<span data-ttu-id="74891-113">tooget pomocí knihovny management Service Bus hello spuštěna, je třeba ověřit pomocí hello služby Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="74891-113">tooget started using hello Service Bus management libraries, you must authenticate with hello Azure Active Directory (AAD) service.</span></span> <span data-ttu-id="74891-114">AAD vyžaduje ověřování jako hlavní název služby, který poskytuje přístup tooyour prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="74891-114">AAD requires that you authenticate as a service principal, which provides access tooyour Azure resources.</span></span> <span data-ttu-id="74891-115">Informace o vytvoření objektu služby najdete v jednom z těchto článků:</span><span class="sxs-lookup"><span data-stu-id="74891-115">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="74891-116">Pomocí aplikace hello portálu toocreate Azure Active Directory a objektu služby, které mají přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="74891-116">Use hello Azure portal toocreate Active Directory application and service principal that can access resources</span></span>](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [<span data-ttu-id="74891-117">Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="74891-117">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="74891-118">Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="74891-118">Use Azure CLI toocreate a service principal tooaccess resources</span></span>](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

<span data-ttu-id="74891-119">Tyto kurzy vám poskytují `AppId` (ID klienta), `TenantId`, a `ClientSecret` (ověřovací klíč), které se používají pro ověřování pomocí knihovny správy hello.</span><span class="sxs-lookup"><span data-stu-id="74891-119">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by hello management libraries.</span></span> <span data-ttu-id="74891-120">Musíte mít **vlastníka** oprávnění pro skupinu prostředků hello, na kterém chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="74891-120">You must have **Owner** permissions for hello resource group on which you wish toorun.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="74891-121">Vzor programování</span><span class="sxs-lookup"><span data-stu-id="74891-121">Programming pattern</span></span>

<span data-ttu-id="74891-122">Hello toomanipulate vzor odpovídá jakémukoli prostředku, Service Bus společný protokol:</span><span class="sxs-lookup"><span data-stu-id="74891-122">hello pattern toomanipulate any Service Bus resource follows a common protocol:</span></span>

1. <span data-ttu-id="74891-123">Získání tokenu z Azure Active Directory pomocí hello **Microsoft.IdentityModel.Clients.ActiveDirectory** knihovny.</span><span class="sxs-lookup"><span data-stu-id="74891-123">Obtain a token from Azure Active Directory using hello **Microsoft.IdentityModel.Clients.ActiveDirectory** library.</span></span>
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. <span data-ttu-id="74891-124">Vytvoření hello `ServiceBusManagementClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="74891-124">Create hello `ServiceBusManagementClient` object.</span></span>

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. <span data-ttu-id="74891-125">Sada hello `CreateOrUpdate` tooyour parametry zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="74891-125">Set hello `CreateOrUpdate` parameters tooyour specified values.</span></span>

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. <span data-ttu-id="74891-126">Spusťte volání hello.</span><span class="sxs-lookup"><span data-stu-id="74891-126">Execute hello call.</span></span>

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a><span data-ttu-id="74891-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74891-127">Next steps</span></span>
* [<span data-ttu-id="74891-128">Ukázka správy rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="74891-128">.NET management sample</span></span>](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [<span data-ttu-id="74891-129">Odkaz na Microsoft.Azure.Management.ServiceBus rozhraní API</span><span class="sxs-lookup"><span data-stu-id="74891-129">Microsoft.Azure.Management.ServiceBus API reference</span></span>](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
