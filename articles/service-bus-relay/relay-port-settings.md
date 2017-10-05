---
title: "Nastavení portu Azure předávání | Microsoft Docs"
description: "Údaje o předávání přes Azure hodnoty portů."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: 5906495c565dad583e74a43b2e5eed57e0c68df1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-relay-port-settings"></a><span data-ttu-id="a5840-103">Nastavení portu Azure předávání</span><span class="sxs-lookup"><span data-stu-id="a5840-103">Azure Relay port settings</span></span>

<span data-ttu-id="a5840-104">Následující tabulka popisuje požadované konfigurace pro hodnoty portů pro předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="a5840-104">The following table describes the required configuration for port values for Azure Relay.</span></span>

## <a name="hybrid-connections"></a><span data-ttu-id="a5840-105">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="a5840-105">Hybrid Connections</span></span>
<span data-ttu-id="a5840-106">Hybridní připojení používá jako podkladový přenosový mechanismus, který používá **HTTPS** pouze.</span><span class="sxs-lookup"><span data-stu-id="a5840-106">Hybrid Connections uses WebSockets as the underlying transport mechanism, which uses **HTTPS** only.</span></span> 

## <a name="wcf-relays"></a><span data-ttu-id="a5840-107">Přenosy WCF</span><span class="sxs-lookup"><span data-stu-id="a5840-107">WCF Relays</span></span>
  
|<span data-ttu-id="a5840-108">Vazba</span><span class="sxs-lookup"><span data-stu-id="a5840-108">Binding</span></span>|<span data-ttu-id="a5840-109">Zabezpečení přenosu</span><span class="sxs-lookup"><span data-stu-id="a5840-109">Transport Security</span></span>|<span data-ttu-id="a5840-110">Port</span><span class="sxs-lookup"><span data-stu-id="a5840-110">Port</span></span>|  
|-------------|------------------------|----------|  
|<span data-ttu-id="a5840-111">[Třída BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (klient)</span><span class="sxs-lookup"><span data-stu-id="a5840-111">[BasicHttpRelayBinding Class](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (client)</span></span>|<span data-ttu-id="a5840-112">Ano</span><span class="sxs-lookup"><span data-stu-id="a5840-112">Yes</span></span>|<span data-ttu-id="a5840-113">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5840-113">HTTPS</span></span>| 
| |<span data-ttu-id="a5840-114">"</span><span class="sxs-lookup"><span data-stu-id="a5840-114">"</span></span> |<span data-ttu-id="a5840-115">Ne</span><span class="sxs-lookup"><span data-stu-id="a5840-115">No</span></span>|<span data-ttu-id="a5840-116">HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-116">HTTP</span></span>|  
|<span data-ttu-id="a5840-117">[Třída BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (služba)</span><span class="sxs-lookup"><span data-stu-id="a5840-117">[BasicHttpRelayBinding Class](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (service)</span></span>|<span data-ttu-id="a5840-118">Buď</span><span class="sxs-lookup"><span data-stu-id="a5840-118">Either</span></span>|<span data-ttu-id="a5840-119">9351/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-119">9351/HTTP</span></span>|  
|<span data-ttu-id="a5840-120">[Třída NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (klient)</span><span class="sxs-lookup"><span data-stu-id="a5840-120">[NetEventRelayBinding Class](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (client)</span></span>|<span data-ttu-id="a5840-121">Ano</span><span class="sxs-lookup"><span data-stu-id="a5840-121">Yes</span></span>|<span data-ttu-id="a5840-122">9351 NEBO HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5840-122">9351/HTTPS</span></span>|  
||<span data-ttu-id="a5840-123">"</span><span class="sxs-lookup"><span data-stu-id="a5840-123">"</span></span> |<span data-ttu-id="a5840-124">Ne</span><span class="sxs-lookup"><span data-stu-id="a5840-124">No</span></span>|<span data-ttu-id="a5840-125">9350/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-125">9350/HTTP</span></span>|  
|<span data-ttu-id="a5840-126">[Třída NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (služba)</span><span class="sxs-lookup"><span data-stu-id="a5840-126">[NetEventRelayBinding Class](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (service)</span></span>|<span data-ttu-id="a5840-127">Buď</span><span class="sxs-lookup"><span data-stu-id="a5840-127">Either</span></span>|<span data-ttu-id="a5840-128">9351/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-128">9351/HTTP</span></span>|  
|<span data-ttu-id="a5840-129">[Třída NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (klienta nebo služba)</span><span class="sxs-lookup"><span data-stu-id="a5840-129">[NetTcpRelayBinding Class](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (client/service)</span></span>|<span data-ttu-id="a5840-130">Buď</span><span class="sxs-lookup"><span data-stu-id="a5840-130">Either</span></span>|<span data-ttu-id="a5840-131">5671/9352/HTTP (9352/9353 Pokud používáte hybridní)</span><span class="sxs-lookup"><span data-stu-id="a5840-131">5671/9352/HTTP (9352/9353 if using hybrid)</span></span>|  
|<span data-ttu-id="a5840-132">[Třída NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (klient)</span><span class="sxs-lookup"><span data-stu-id="a5840-132">[NetOnewayRelayBinding Class](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (client)</span></span>|<span data-ttu-id="a5840-133">Ano</span><span class="sxs-lookup"><span data-stu-id="a5840-133">Yes</span></span>|<span data-ttu-id="a5840-134">9351 NEBO HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5840-134">9351/HTTPS</span></span>|  
||<span data-ttu-id="a5840-135">"</span><span class="sxs-lookup"><span data-stu-id="a5840-135">"</span></span> |<span data-ttu-id="a5840-136">Ne</span><span class="sxs-lookup"><span data-stu-id="a5840-136">No</span></span>|<span data-ttu-id="a5840-137">9350/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-137">9350/HTTP</span></span>|  
|<span data-ttu-id="a5840-138">[Třída NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (služba)</span><span class="sxs-lookup"><span data-stu-id="a5840-138">[NetOnewayRelayBinding Class](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (service)</span></span>|<span data-ttu-id="a5840-139">Buď</span><span class="sxs-lookup"><span data-stu-id="a5840-139">Either</span></span>|<span data-ttu-id="a5840-140">9351/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-140">9351/HTTP</span></span>|  
|<span data-ttu-id="a5840-141">[Třída WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (klient)</span><span class="sxs-lookup"><span data-stu-id="a5840-141">[WebHttpRelayBinding Class](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (client)</span></span>|<span data-ttu-id="a5840-142">Ano</span><span class="sxs-lookup"><span data-stu-id="a5840-142">Yes</span></span>|<span data-ttu-id="a5840-143">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5840-143">HTTPS</span></span>|  
||<span data-ttu-id="a5840-144">"</span><span class="sxs-lookup"><span data-stu-id="a5840-144">"</span></span> |<span data-ttu-id="a5840-145">Ne</span><span class="sxs-lookup"><span data-stu-id="a5840-145">No</span></span>|<span data-ttu-id="a5840-146">HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-146">HTTP</span></span>|  
|<span data-ttu-id="a5840-147">[Třída WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (služba)</span><span class="sxs-lookup"><span data-stu-id="a5840-147">[WebHttpRelayBinding Class](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (service)</span></span>|<span data-ttu-id="a5840-148">Buď</span><span class="sxs-lookup"><span data-stu-id="a5840-148">Either</span></span>|<span data-ttu-id="a5840-149">9351/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-149">9351/HTTP</span></span>|  
|<span data-ttu-id="a5840-150">[Třída WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (klient)</span><span class="sxs-lookup"><span data-stu-id="a5840-150">[WS2007HttpRelayBinding Class](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (client)</span></span>|<span data-ttu-id="a5840-151">Ano</span><span class="sxs-lookup"><span data-stu-id="a5840-151">Yes</span></span>|<span data-ttu-id="a5840-152">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5840-152">HTTPS</span></span>|  
||<span data-ttu-id="a5840-153">"</span><span class="sxs-lookup"><span data-stu-id="a5840-153">"</span></span> |<span data-ttu-id="a5840-154">Ne</span><span class="sxs-lookup"><span data-stu-id="a5840-154">No</span></span>|<span data-ttu-id="a5840-155">HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-155">HTTP</span></span>|  
|<span data-ttu-id="a5840-156">[Třída WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (služba)</span><span class="sxs-lookup"><span data-stu-id="a5840-156">[WS2007HttpRelayBinding Class](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (service)</span></span>|<span data-ttu-id="a5840-157">Buď</span><span class="sxs-lookup"><span data-stu-id="a5840-157">Either</span></span>|<span data-ttu-id="a5840-158">9351/HTTP</span><span class="sxs-lookup"><span data-stu-id="a5840-158">9351/HTTP</span></span>|

## <a name="next-steps"></a><span data-ttu-id="a5840-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5840-159">Next steps</span></span>
<span data-ttu-id="a5840-160">Další informace o předávání přes Azure, najdete pomocí těchto odkazů:</span><span class="sxs-lookup"><span data-stu-id="a5840-160">To learn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="a5840-161">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="a5840-161">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="a5840-162">Přenos – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a5840-162">Relay FAQ</span></span>](relay-faq.md)