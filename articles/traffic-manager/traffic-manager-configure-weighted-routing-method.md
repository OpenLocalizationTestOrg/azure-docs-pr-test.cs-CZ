---
title: "Konfigurace metody směrování provozu vážené kruhové dotazování pomocí Azure Traffic Manageru | Microsoft Docs"
description: "Tento článek vysvětluje, jak načíst vyrovnávání přenosů pomocí jiné metody kruhového dotazování v Traffic Manageru"
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
ms.openlocfilehash: 7aa4c9120d44ff1b3e59a57090ea04e3f8021fc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a><span data-ttu-id="663af-103">Konfigurace metody směrování vyvážené provoz v Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="663af-103">Configure the weighted traffic routing method in Traffic Manager</span></span>

<span data-ttu-id="663af-104">Běžné vzor metoda směrování provozu je poskytují sadu identické koncové body, které zahrnují cloudové služby a weby, a odesílá data na každý kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="663af-104">A common traffic routing method pattern is to provide a set of identical endpoints, which include cloud services and websites, and send traffic to each in a round-robin fashion.</span></span> <span data-ttu-id="663af-105">Následující kroky popisují postup konfigurace tento typ metodu směrování provozu.</span><span class="sxs-lookup"><span data-stu-id="663af-105">The following steps outline how to configure this type of traffic routing method.</span></span>

> [!NOTE]
> <span data-ttu-id="663af-106">Weby Azure již poskytují funkce pro weby v rámci datového centra (označované také jako oblast) pro vyrovnávání zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="663af-106">Azure Websites already provide round-robin load balancing functionality for websites within a datacenter (also known as a region).</span></span> <span data-ttu-id="663af-107">Traffic Manager umožňuje určit metodu směrování provozu kruhovým dotazováním pro weby v různých datových centrech.</span><span class="sxs-lookup"><span data-stu-id="663af-107">Traffic Manager allows you to specify round-robin traffic routing method for websites in different datacenters.</span></span>

## <a name="to-configure-the-weighted-traffic-routing-method"></a><span data-ttu-id="663af-108">Nakonfigurovat metodu směrování provozu vyvážené</span><span class="sxs-lookup"><span data-stu-id="663af-108">To configure the weighted traffic routing method</span></span>

1. <span data-ttu-id="663af-109">V prohlížeči se přihlaste k webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="663af-109">From a browser, sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="663af-110">Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="663af-110">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="663af-111">V panelu vyhledávání na portálu, vyhledejte **profily Traffic Manageru** a pak klikněte na název profilu, který chcete nakonfigurovat metodu směrování.</span><span class="sxs-lookup"><span data-stu-id="663af-111">In the portal’s search bar, search for the **Traffic Manager profiles** and then click the profile name that you want to configure the routing method for.</span></span>
3. <span data-ttu-id="663af-112">V **profil služby Traffic Manager** okno, ověřte, zda cloudové služby i weby, které chcete zahrnout do vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="663af-112">In the **Traffic Manager profile** blade, verify that both the cloud services and websites that you want to include in your configuration are present.</span></span>
4. <span data-ttu-id="663af-113">V **nastavení** klikněte na tlačítko **konfigurace**a v **konfigurace** okně dokončení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="663af-113">In the **Settings** section, click **Configuration**, and in the **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="663af-114">Pro **nastavení metodu směrování provozu**, ověřte, zda je metodu směrování provozu **vážená**.</span><span class="sxs-lookup"><span data-stu-id="663af-114">For **traffic routing method settings**, verify that the traffic routing method is **Weighted**.</span></span> <span data-ttu-id="663af-115">Pokud není, klikněte na tlačítko **vážená** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="663af-115">If it is not, click **Weighted** from the dropdown list.</span></span>
    2. <span data-ttu-id="663af-116">Nastavte **nastavení monitorování koncového bodu** stejné pro všechny každý koncový bod v tomto profilu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="663af-116">Set the **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="663af-117">Vyberte odpovídající **protokol**a zadejte **Port** číslo.</span><span class="sxs-lookup"><span data-stu-id="663af-117">Select the appropriate **Protocol**, and specify the **Port** number.</span></span> 
        2. <span data-ttu-id="663af-118">Pro **cesta** zadejte lomítkem  */* .</span><span class="sxs-lookup"><span data-stu-id="663af-118">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="663af-119">Chcete-li monitorovat koncových bodů, musíte zadat cestu a název souboru.</span><span class="sxs-lookup"><span data-stu-id="663af-119">To monitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="663af-120">A dopředné lomítko "/" je platná položka pro relativní cestu a znamená, že soubor je v kořenovém adresáři (výchozí).</span><span class="sxs-lookup"><span data-stu-id="663af-120">A forward slash "/" is a valid entry for the relative path and implies that the file is in the root directory (default).</span></span>
        3. <span data-ttu-id="663af-121">V horní části stránky klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="663af-121">At the top of the page, click **Save**.</span></span>
5. <span data-ttu-id="663af-122">Testovací změny v konfiguraci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="663af-122">Test the changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="663af-123">V panelu vyhledávání na portálu, vyhledejte název profilu Traffic Manageru a klikněte na profil služby Traffic Manager ve výsledcích, zobrazené.</span><span class="sxs-lookup"><span data-stu-id="663af-123">In the portal’s search bar, search for the Traffic Manager profile name and click the Traffic Manager profile in the results that the displayed.</span></span>
    2.  <span data-ttu-id="663af-124">V **Traffic Manager** profilu okno, klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="663af-124">In the **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="663af-125">**Profil služby Traffic Manager** zobrazuje název DNS vašeho nově vytvořený profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="663af-125">The **Traffic Manager profile** blade displays the DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="663af-126">Tímto lze všechny klienty (například tak, že přejdete do ní pomocí webového prohlížeče) k získání směruje na pravý koncový bod jako Určuje typ směrování.</span><span class="sxs-lookup"><span data-stu-id="663af-126">This can be used by any clients (for example,by navigating to it using a web browser) to get routed to the right endpoint as determined by the routing type.</span></span> <span data-ttu-id="663af-127">V takovém případě jsou směrovat všechny požadavky každý koncový bod v kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="663af-127">In this case all requests are routed each endpoint in a round-robin fashion.</span></span>
6. <span data-ttu-id="663af-128">Jakmile váš profil služby Traffic Manager funguje, upravte záznam DNS na autoritativního serveru DNS pro název domény vaší společnosti přejděte na název domény Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="663af-128">Once your Traffic Manager profile is working, edit the DNS record on your authoritative DNS server to point your company domain name to the Traffic Manager domain name.</span></span>

![Konfigurace metody směrování vyvážené provozu pomocí Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="663af-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="663af-130">Next steps</span></span>

- <span data-ttu-id="663af-131">Další informace o [metoda směrování provozu s prioritou](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="663af-131">Learn about [priority traffic routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="663af-132">Další informace o [metoda směrování provozu výkonu](traffic-manager-configure-performance-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="663af-132">Learn about [performance traffic routing method](traffic-manager-configure-performance-routing-method.md).</span></span>
- <span data-ttu-id="663af-133">Další informace o [geografické metody směrování](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="663af-133">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="663af-134">Zjistěte, jak [Traffic Manager nastavení testů](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="663af-134">Learn how to [test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
