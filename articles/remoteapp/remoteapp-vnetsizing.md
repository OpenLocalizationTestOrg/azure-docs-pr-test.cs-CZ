---
title: "Změna velikosti informace pro virtuální síť v Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích na IP adresu pro virtuální síť s Azure RemoteApp"
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
ms.openlocfilehash: 9375981db64ec4a1ae523e958423b5f5787cec33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a><span data-ttu-id="9dc35-103">Informace o nastavení velikosti pro virtuální sítě v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="9dc35-103">Sizing information for a VNET in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9dc35-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="9dc35-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="9dc35-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="9dc35-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="9dc35-106">Pokud používáte Azure RemoteApp s virtuální sítí (VNET), vzdálené aplikace RemoteApp používá IP adresy v rámci podsítě.</span><span class="sxs-lookup"><span data-stu-id="9dc35-106">When you use Azure RemoteApp with a virtual network (VNET), RemoteApp uses IP addresses within the subnet.</span></span> <span data-ttu-id="9dc35-107">Závislosti na rozsahu vaší služby vzdálené aplikace RemoteApp, musíte zajistit, aby podsíť není dost IP adres, které jsou k dispozici pro virtuální počítače vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="9dc35-107">Based on the scale of your RemoteApp service, you need to ensure that your subnet has enough IP addresses available for RemoteApp virtual machines.</span></span> <span data-ttu-id="9dc35-108">Při této pokyny k dimenzování není ideální daného jak vzdálené aplikace RemoteApp dynamicky otáčí nahoru a dolů virtuálních počítačů v rámci kolekce otáčí, pomůže vám odhad vaší rozsahu podsítě.</span><span class="sxs-lookup"><span data-stu-id="9dc35-108">While this sizing guidance isn’t perfect given how RemoteApp dynamically spins up and spins down virtual machines within a collection, it will help you estimate your subnet range.</span></span> <span data-ttu-id="9dc35-109">To je obzvláště důležité, protože po služby vzdálené aplikace RemoteApp je umístěn ve virtuální síti, nelze zvýšit velikost podsítě bez odebrání vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="9dc35-109">This is especially important as, once a RemoteApp service is placed in a VNET, you cannot increase the subnet size without removing RemoteApp.</span></span>

<span data-ttu-id="9dc35-110">Pro každou kolekci vzdálené aplikace RemoteApp, která se má spustit při maximální kapacitě měli byste mít 100 IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9dc35-110">For each RemoteApp collection that you want to run at maximum capacity, you should have 100 IP addresses available.</span></span> <span data-ttu-id="9dc35-111">Například pokud máte v plánu Standard jednu kolekci vzdálené aplikace RemoteApp a budete chtít mít maximální 500 uživatelů, byste měli mít 100 IP adresy pro tuto kolekci.</span><span class="sxs-lookup"><span data-stu-id="9dc35-111">For example, if you have one RemoteApp collection in the Standard plan and you want to have the maximum 500 users, you should have 100 IP addresses for that collection.</span></span> <span data-ttu-id="9dc35-112">Podobně platí musíte 100 IP adresy pro kolekci vzdálené aplikace RemoteApp v základní plán, který má 800 uživatele.</span><span class="sxs-lookup"><span data-stu-id="9dc35-112">Similarly, you need 100 IP addresses for a RemoteApp collection in the Basic plan that has 800 users.</span></span> <span data-ttu-id="9dc35-113">Pokud budete chtít mít méně uživatele (je menší než maximální), můžete snížit IP adresy, potřebuje na kolekci.</span><span class="sxs-lookup"><span data-stu-id="9dc35-113">If you plan to have fewer users (less than the maximum), you can reduce the IP addresses needed per collection.</span></span> <span data-ttu-id="9dc35-114">Požadavek na minimální podsíť velikost je 30 IP adres (/ 27).</span><span class="sxs-lookup"><span data-stu-id="9dc35-114">The minimum subnet size requirement is 30 IP addresses (/27).</span></span>

<span data-ttu-id="9dc35-115">Podívejte se na následující informace a ujistěte se virtuální síť je konfigurována a funkční propertly:</span><span class="sxs-lookup"><span data-stu-id="9dc35-115">Check out the following information to make sure your VNET is configured and working propertly:</span></span>

* [<span data-ttu-id="9dc35-116">Migrace z osobní virtuální sítě do Azure VNET</span><span class="sxs-lookup"><span data-stu-id="9dc35-116">Migrate from a personal VNET to an Azure VNET</span></span>](remoteapp-migratevnet.md)
* [<span data-ttu-id="9dc35-117">Ověření virtuální síť Azure pro použití s Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="9dc35-117">Validate the Azure VNET to use with Azure RemoteApp</span></span>](remoteapp-vnet.md)

