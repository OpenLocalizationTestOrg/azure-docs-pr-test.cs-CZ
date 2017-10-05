---
title: "Konfigurace metody směrování výkonu provozu pomocí Azure Traffic Manageru | Microsoft Docs"
description: "Tento článek vysvětluje, jak nakonfigurovat Traffic Manager přesměrovat provoz na koncový bod s nejnižší latencí"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 014aa646459cd64fca7c697419324caa3edaeeea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-performance-traffic-routing-method"></a><span data-ttu-id="80334-103">Konfigurace metody směrování provozu výkonu</span><span class="sxs-lookup"><span data-stu-id="80334-103">Configure the performance traffic routing method</span></span>

<span data-ttu-id="80334-104">Metodu směrování provozu výkonu umožňuje směrovat provoz na koncový bod s nejnižší latencí z klienta sítě.</span><span class="sxs-lookup"><span data-stu-id="80334-104">The Performance traffic routing method allows you to direct traffic to the endpoint with the lowest latency from the client's network.</span></span> <span data-ttu-id="80334-105">Datového centra s nejnižší latencí je obvykle na nejbližší v zeměpisné vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="80334-105">Typically, the datacenter with the lowest latency is the closest in geographic distance.</span></span> <span data-ttu-id="80334-106">Tuto metodu směrování provozu nelze účet v reálném čase změny v konfiguraci sítě nebo zatížení.</span><span class="sxs-lookup"><span data-stu-id="80334-106">This traffic routing method cannot account for real-time changes in network configuration or load.</span></span>

##  <a name="to-configure-performance-routing-method"></a><span data-ttu-id="80334-107">Konfigurace metody směrování podle výkonu</span><span class="sxs-lookup"><span data-stu-id="80334-107">To configure performance routing method</span></span>

1. <span data-ttu-id="80334-108">V prohlížeči se přihlaste k webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="80334-108">From a browser, sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="80334-109">Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="80334-109">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="80334-110">V panelu vyhledávání na portálu, vyhledejte **profily Traffic Manageru** a pak klikněte na název profilu, který chcete nakonfigurovat metodu směrování.</span><span class="sxs-lookup"><span data-stu-id="80334-110">In the portal’s search bar, search for the **Traffic Manager profiles** and then click the profile name that you want to configure the routing method for.</span></span>
3. <span data-ttu-id="80334-111">V **profil služby Traffic Manager** okno, ověřte, zda cloudové služby i weby, které chcete zahrnout do vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="80334-111">In the **Traffic Manager profile** blade, verify that both the cloud services and websites that you want to include in your configuration are present.</span></span>
4. <span data-ttu-id="80334-112">V **nastavení** klikněte na tlačítko **konfigurace**a v **konfigurace** okně dokončení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="80334-112">In the **Settings** section, click **Configuration**, and in the **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="80334-113">Pro **nastavení metodu směrování provozu**, pro **metoda směrování** vyberte **výkonu**.</span><span class="sxs-lookup"><span data-stu-id="80334-113">For **traffic routing method settings**, for **Routing method** select **Performance**.</span></span>
    2. <span data-ttu-id="80334-114">Nastavte **nastavení monitorování koncového bodu** stejné pro všechny každý koncový bod v tomto profilu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="80334-114">Set the **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="80334-115">Vyberte odpovídající **protokol**a zadejte **Port** číslo.</span><span class="sxs-lookup"><span data-stu-id="80334-115">Select the appropriate **Protocol**, and specify the **Port** number.</span></span> 
        2. <span data-ttu-id="80334-116">Pro **cesta** zadejte lomítkem  */* .</span><span class="sxs-lookup"><span data-stu-id="80334-116">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="80334-117">Chcete-li monitorovat koncových bodů, musíte zadat cestu a název souboru.</span><span class="sxs-lookup"><span data-stu-id="80334-117">To monitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="80334-118">A dopředné lomítko "/" je platná položka pro relativní cestu a znamená, že soubor je v kořenovém adresáři (výchozí).</span><span class="sxs-lookup"><span data-stu-id="80334-118">A forward slash "/" is a valid entry for the relative path and implies that the file is in the root directory (default).</span></span>
        3. <span data-ttu-id="80334-119">V horní části stránky klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="80334-119">At the top of the page, click **Save**.</span></span>
5.  <span data-ttu-id="80334-120">Testovací změny v konfiguraci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="80334-120">Test the changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="80334-121">V panelu vyhledávání na portálu, vyhledejte název profilu Traffic Manageru a klikněte na profil služby Traffic Manager ve výsledcích, zobrazené.</span><span class="sxs-lookup"><span data-stu-id="80334-121">In the portal’s search bar, search for the Traffic Manager profile name and click the Traffic Manager profile in the results that the displayed.</span></span>
    2.  <span data-ttu-id="80334-122">V **Traffic Manager** profilu okno, klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="80334-122">In the **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="80334-123">**Profil služby Traffic Manager** zobrazuje název DNS vašeho nově vytvořený profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="80334-123">The **Traffic Manager profile** blade displays the DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="80334-124">Tímto lze všechny klienty (například tak, že přejdete do ní pomocí webového prohlížeče) k získání směruje na pravý koncový bod jako Určuje typ směrování.</span><span class="sxs-lookup"><span data-stu-id="80334-124">This can be used by any clients (for example, by navigating to it using a web browser) to get routed to the right endpoint as determined by the routing type.</span></span> <span data-ttu-id="80334-125">V takovém případě všechny požadavky směrují na koncový bod s nejnižší latencí z klienta sítě.</span><span class="sxs-lookup"><span data-stu-id="80334-125">In this case all requests are routed to the endpoint with the lowest latency from the client's network.</span></span>
6. <span data-ttu-id="80334-126">Jakmile váš profil služby Traffic Manager funguje, upravte záznam DNS na autoritativního serveru DNS pro název domény vaší společnosti přejděte na název domény Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="80334-126">Once your Traffic Manager profile is working, edit the DNS record on your authoritative DNS server to point your company domain name to the Traffic Manager domain name.</span></span>

![Konfigurace metody směrování výkonu provozu pomocí Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="80334-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80334-128">Next steps</span></span>

- <span data-ttu-id="80334-129">Další informace o [vážené metodu směrování provozu](traffic-manager-configure-weighted-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="80334-129">Learn about [weighted traffic routing method](traffic-manager-configure-weighted-routing-method.md).</span></span>
- <span data-ttu-id="80334-130">Další informace o [metody směrování s prioritou](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="80334-130">Learn about [priority routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="80334-131">Další informace o [geografické metody směrování](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="80334-131">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="80334-132">Zjistěte, jak [Traffic Manager nastavení testů](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="80334-132">Learn how to [test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png