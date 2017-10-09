---
title: "informace o aaaSizing pro virtuální sítě v Azure Remoteappu | Microsoft Docs"
description: "Další informace o hello požadavky na IP adresu pro virtuální síť s Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b6e1c4ba-0236-42b2-bced-69353bf211be
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: f98b831af32c41740b258d122b3e18765be08d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a><span data-ttu-id="eb52f-103">Informace o nastavení velikosti pro virtuální sítě v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="eb52f-103">Sizing information for a VNET in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="eb52f-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="eb52f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="eb52f-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="eb52f-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="eb52f-106">Pokud používáte Azure RemoteApp s virtuální sítí (VNET), vzdálené aplikace RemoteApp používá IP adresy v rámci podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="eb52f-106">When you use Azure RemoteApp with a virtual network (VNET), RemoteApp uses IP addresses within hello subnet.</span></span> <span data-ttu-id="eb52f-107">Podle hello škálování služby vzdálené aplikace RemoteApp, je nutné tooensure že podsíť má dost IP adres, které jsou k dispozici pro virtuální počítače vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="eb52f-107">Based on hello scale of your RemoteApp service, you need tooensure that your subnet has enough IP addresses available for RemoteApp virtual machines.</span></span> <span data-ttu-id="eb52f-108">Při této pokyny k dimenzování není ideální daného jak vzdálené aplikace RemoteApp dynamicky otáčí nahoru a dolů virtuálních počítačů v rámci kolekce otáčí, pomůže vám odhad vaší rozsahu podsítě.</span><span class="sxs-lookup"><span data-stu-id="eb52f-108">While this sizing guidance isn’t perfect given how RemoteApp dynamically spins up and spins down virtual machines within a collection, it will help you estimate your subnet range.</span></span> <span data-ttu-id="eb52f-109">To je obzvláště důležité, protože po služby vzdálené aplikace RemoteApp je umístěn ve virtuální síti, nelze zvýšit velikost podsítě hello bez odebrání vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="eb52f-109">This is especially important as, once a RemoteApp service is placed in a VNET, you cannot increase hello subnet size without removing RemoteApp.</span></span>

<span data-ttu-id="eb52f-110">Pro každou kolekci vzdálené aplikace RemoteApp, že chcete toorun při maximální kapacitě měli byste mít 100 IP adresy.</span><span class="sxs-lookup"><span data-stu-id="eb52f-110">For each RemoteApp collection that you want toorun at maximum capacity, you should have 100 IP addresses available.</span></span> <span data-ttu-id="eb52f-111">Například pokud máte v plánu Standard hello jednu kolekci vzdálené aplikace RemoteApp a chcete toohave hello maximální 500 uživatelů, měli byste 100 IP adresy pro tuto kolekci.</span><span class="sxs-lookup"><span data-stu-id="eb52f-111">For example, if you have one RemoteApp collection in hello Standard plan and you want toohave hello maximum 500 users, you should have 100 IP addresses for that collection.</span></span> <span data-ttu-id="eb52f-112">Podobně platí musíte 100 IP adresy pro kolekci vzdálené aplikace RemoteApp v hello základní plán, který má 800 uživatele.</span><span class="sxs-lookup"><span data-stu-id="eb52f-112">Similarly, you need 100 IP addresses for a RemoteApp collection in hello Basic plan that has 800 users.</span></span> <span data-ttu-id="eb52f-113">Pokud máte v plánu toohave méně uživatelů (menší než maximální hello), můžete snížit hello IP adresy, které potřebuje na kolekci.</span><span class="sxs-lookup"><span data-stu-id="eb52f-113">If you plan toohave fewer users (less than hello maximum), you can reduce hello IP addresses needed per collection.</span></span> <span data-ttu-id="eb52f-114">požadavek na velikost minimální podsíť Hello je 30 IP adres (/ 27).</span><span class="sxs-lookup"><span data-stu-id="eb52f-114">hello minimum subnet size requirement is 30 IP addresses (/27).</span></span>

<span data-ttu-id="eb52f-115">Projděte si následující informace toomake se, že jsou vaše virtuální síť nakonfigurované a práci propertly hello:</span><span class="sxs-lookup"><span data-stu-id="eb52f-115">Check out hello following information toomake sure your VNET is configured and working propertly:</span></span>

* [<span data-ttu-id="eb52f-116">Migrace z osobní tooan virtuální sítě Azure VNET</span><span class="sxs-lookup"><span data-stu-id="eb52f-116">Migrate from a personal VNET tooan Azure VNET</span></span>](remoteapp-migratevnet.md)
* [<span data-ttu-id="eb52f-117">Ověření hello toouse virtuální síť Azure s Azure Remoteappem</span><span class="sxs-lookup"><span data-stu-id="eb52f-117">Validate hello Azure VNET toouse with Azure RemoteApp</span></span>](remoteapp-vnet.md)

