---
title: "Běžné FabricClient výjimky vydané | Microsoft Docs"
description: "Popisuje běžné výjimek a chyb, k nimž může být vyvolána rozhraní API FabricClient při provádění aplikace a operace správy clusterů."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: f8a4d7573f0d256b562056ba52939ff5e66de15c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a><span data-ttu-id="aaae9-103">Běžné výjimek a chyb při práci s rozhraními API sady FabricClient</span><span class="sxs-lookup"><span data-stu-id="aaae9-103">Common exceptions and errors when working with the FabricClient APIs</span></span>
<span data-ttu-id="aaae9-104">[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) rozhraní API umožňují cluster a aplikace správcům umožňuje provádět úlohy správy na aplikace, služby nebo clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="aaae9-104">The [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) APIs enable cluster and application administrators to perform administrative tasks on a Service Fabric application, service, or cluster.</span></span> <span data-ttu-id="aaae9-105">Například nasazení aplikace, upgrade a odebrání, kontrola stavu clusteru nebo testování služby.</span><span class="sxs-lookup"><span data-stu-id="aaae9-105">For example, application deployment, upgrade, and removal, checking the health a cluster, or testing a service.</span></span> <span data-ttu-id="aaae9-106">Vývojáři aplikací a Správce clusteru můžete použít rozhraní API FabricClient vyvíjet nástroje pro správu clusteru Service Fabric a aplikace.</span><span class="sxs-lookup"><span data-stu-id="aaae9-106">Application developers and cluster administrators can use the FabricClient APIs to develop tools for managing the Service Fabric cluster and applications.</span></span>

<span data-ttu-id="aaae9-107">Existuje mnoho různých typů operací, které je možné provést pomocí FabricClient.</span><span class="sxs-lookup"><span data-stu-id="aaae9-107">There are many different types of operations which can be performed using FabricClient.</span></span>  <span data-ttu-id="aaae9-108">Každá metoda může vyvolat výjimky pro chyby z důvodu nesprávné vstup, chyby za běhu nebo problémů s přechodnou infrastrukturou.</span><span class="sxs-lookup"><span data-stu-id="aaae9-108">Each method can throw exceptions for errors due to incorrect input, runtime errors, or transient infrastructure issues.</span></span>  <span data-ttu-id="aaae9-109">V tématu referenční dokumentace rozhraní API k vyhledání výjimek, které jsou vyvolány pomocí konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="aaae9-109">See the API reference documentation to find which exceptions are thrown by a specific method.</span></span> <span data-ttu-id="aaae9-110">Existují některé výjimky, ale to může být vyvolána mnoho různých [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="aaae9-110">There are some exceptions, however, which can be thrown by many different [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) APIs.</span></span> <span data-ttu-id="aaae9-111">Následující tabulka uvádí výjimky, které jsou společné pro rozhraní API FabricClient.</span><span class="sxs-lookup"><span data-stu-id="aaae9-111">The following table lists the exceptions that are common across the FabricClient APIs.</span></span>

| <span data-ttu-id="aaae9-112">Výjimka</span><span class="sxs-lookup"><span data-stu-id="aaae9-112">Exception</span></span> | <span data-ttu-id="aaae9-113">Při vyvolání</span><span class="sxs-lookup"><span data-stu-id="aaae9-113">Thrown when</span></span> |
| --- |:--- |
| [<span data-ttu-id="aaae9-114">System.Fabric.FabricObjectClosedException</span><span class="sxs-lookup"><span data-stu-id="aaae9-114">System.Fabric.FabricObjectClosedException</span></span>](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |<span data-ttu-id="aaae9-115">[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objekt je v uzavřeném stavu.</span><span class="sxs-lookup"><span data-stu-id="aaae9-115">The [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) object is in a closed state.</span></span> <span data-ttu-id="aaae9-116">Odstranění [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) používáte a vytvořit novou instanci objektu [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objektu.</span><span class="sxs-lookup"><span data-stu-id="aaae9-116">Dispose of the [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) object you are using and instantiate a new [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) object.</span></span> |
| [<span data-ttu-id="aaae9-117">System.TimeoutException</span><span class="sxs-lookup"><span data-stu-id="aaae9-117">System.TimeoutException</span></span>](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |<span data-ttu-id="aaae9-118">Vypršel časový limit operace.</span><span class="sxs-lookup"><span data-stu-id="aaae9-118">The operation timed out.</span></span> <span data-ttu-id="aaae9-119">[OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) je vrácena, pokud operace trvá déle než jako MaxOperationTimeout k dokončení.</span><span class="sxs-lookup"><span data-stu-id="aaae9-119">[OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) is returned when the operation takes more than MaxOperationTimeout to complete.</span></span> |
| [<span data-ttu-id="aaae9-120">System.UnauthorizedAccessException</span><span class="sxs-lookup"><span data-stu-id="aaae9-120">System.UnauthorizedAccessException</span></span>](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |<span data-ttu-id="aaae9-121">Kontrola přístupu pro operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="aaae9-121">The access check for the operation failed.</span></span> <span data-ttu-id="aaae9-122">E_ACCESSDENIED je vrácen.</span><span class="sxs-lookup"><span data-stu-id="aaae9-122">E_ACCESSDENIED is returned.</span></span> |
| [<span data-ttu-id="aaae9-123">System.Fabric.FabricException</span><span class="sxs-lookup"><span data-stu-id="aaae9-123">System.Fabric.FabricException</span></span>](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |<span data-ttu-id="aaae9-124">Při provádění operace došlo k chybě za běhu.</span><span class="sxs-lookup"><span data-stu-id="aaae9-124">A runtime error occurred while performing the operation.</span></span> <span data-ttu-id="aaae9-125">Libovolná z metod FabricClient potenciálně throw [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) vlastnost určuje přesné příčině výjimky.</span><span class="sxs-lookup"><span data-stu-id="aaae9-125">Any of the FabricClient methods can potentially throw [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), the [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) property indicates the exact cause of the exception.</span></span> <span data-ttu-id="aaae9-126">Kódy chyb jsou definovány v [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) výčtu.</span><span class="sxs-lookup"><span data-stu-id="aaae9-126">Error codes are defined in the [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) enumeration.</span></span> |
| [<span data-ttu-id="aaae9-127">System.Fabric.FabricTransientException</span><span class="sxs-lookup"><span data-stu-id="aaae9-127">System.Fabric.FabricTransientException</span></span>](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |<span data-ttu-id="aaae9-128">Operace se nezdařila z důvodu přechodného chybový stav určitého druhu.</span><span class="sxs-lookup"><span data-stu-id="aaae9-128">The operation failed due to a transient error condition of some kind.</span></span> <span data-ttu-id="aaae9-129">Například operace může selhat, protože kvorum repliky není dočasně dostupná.</span><span class="sxs-lookup"><span data-stu-id="aaae9-129">For example, an operation may fail because a quorum of replicas is temporarily not reachable.</span></span> <span data-ttu-id="aaae9-130">Přechodný výjimky odpovídají neúspěšné operace, které můžete zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="aaae9-130">Transient exceptions correspond to failed operations that can be retried.</span></span> |

<span data-ttu-id="aaae9-131">Některé běžné [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) chyb, které mohou být vráceny v [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):</span><span class="sxs-lookup"><span data-stu-id="aaae9-131">Some common [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) errors that can be returned in a [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):</span></span>

| <span data-ttu-id="aaae9-132">Chyba</span><span class="sxs-lookup"><span data-stu-id="aaae9-132">Error</span></span> | <span data-ttu-id="aaae9-133">Podmínka</span><span class="sxs-lookup"><span data-stu-id="aaae9-133">Condition</span></span> |
| --- |:--- |
| <span data-ttu-id="aaae9-134">CommunicationError</span><span class="sxs-lookup"><span data-stu-id="aaae9-134">CommunicationError</span></span> |<span data-ttu-id="aaae9-135">Komunikační chyba způsobila se operace nezdaří, zkuste operaci zopakovat.</span><span class="sxs-lookup"><span data-stu-id="aaae9-135">A communication error caused the operation to fail, retry the operation.</span></span> |
| <span data-ttu-id="aaae9-136">InvalidCredentialType</span><span class="sxs-lookup"><span data-stu-id="aaae9-136">InvalidCredentialType</span></span> |<span data-ttu-id="aaae9-137">Tento typ přihlašovacích údajů je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-137">The credential type is invalid.</span></span> |
| <span data-ttu-id="aaae9-138">InvalidX509FindType</span><span class="sxs-lookup"><span data-stu-id="aaae9-138">InvalidX509FindType</span></span> |<span data-ttu-id="aaae9-139">X509FindType je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-139">The X509FindType is invalid.</span></span> |
| <span data-ttu-id="aaae9-140">InvalidX509StoreLocation</span><span class="sxs-lookup"><span data-stu-id="aaae9-140">InvalidX509StoreLocation</span></span> |<span data-ttu-id="aaae9-141">X509 umístění úložiště je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-141">The X509 store location is invalid.</span></span> |
| <span data-ttu-id="aaae9-142">InvalidX509StoreName</span><span class="sxs-lookup"><span data-stu-id="aaae9-142">InvalidX509StoreName</span></span> |<span data-ttu-id="aaae9-143">X509 úložiště název je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-143">The X509 store name is invalid.</span></span> |
| <span data-ttu-id="aaae9-144">InvalidX509Thumbprint</span><span class="sxs-lookup"><span data-stu-id="aaae9-144">InvalidX509Thumbprint</span></span> |<span data-ttu-id="aaae9-145">X509 řetězec kryptografický otisk certifikátu je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-145">The X509 certificate thumbprint string is invalid.</span></span> |
| <span data-ttu-id="aaae9-146">InvalidProtectionLevel</span><span class="sxs-lookup"><span data-stu-id="aaae9-146">InvalidProtectionLevel</span></span> |<span data-ttu-id="aaae9-147">Úroveň ochrany je neplatná.</span><span class="sxs-lookup"><span data-stu-id="aaae9-147">The protection level is invalid.</span></span> |
| <span data-ttu-id="aaae9-148">InvalidX509Store</span><span class="sxs-lookup"><span data-stu-id="aaae9-148">InvalidX509Store</span></span> |<span data-ttu-id="aaae9-149">X509 nelze otevřít úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="aaae9-149">The X509 certificate store cannot be opened.</span></span> |
| <span data-ttu-id="aaae9-150">InvalidSubjectName</span><span class="sxs-lookup"><span data-stu-id="aaae9-150">InvalidSubjectName</span></span> |<span data-ttu-id="aaae9-151">Název předmětu je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-151">The subject name is invalid.</span></span> |
| <span data-ttu-id="aaae9-152">InvalidAllowedCommonNameList</span><span class="sxs-lookup"><span data-stu-id="aaae9-152">InvalidAllowedCommonNameList</span></span> |<span data-ttu-id="aaae9-153">Formát řetězce seznamu běžný název je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaae9-153">The format of common name list string is invalid.</span></span> <span data-ttu-id="aaae9-154">To by měl být čárkami oddělený seznam.</span><span class="sxs-lookup"><span data-stu-id="aaae9-154">It should be a comma-separated list.</span></span> |
