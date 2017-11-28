---
title: "pomocí Azure Traffic Manager metodu směrování provozu aaaConfigure výkonu | Microsoft Docs"
description: "Tento článek vysvětluje, jak tooconfigure Traffic Manager tooroute provozu toohello koncový bod s nejnižší latencí"
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
ms.openlocfilehash: d0ccd4c9de411844c6f36068859265244f4aa52b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-performance-traffic-routing-method"></a><span data-ttu-id="bc01f-103">Konfigurace metody směrování provozu hello výkonu</span><span class="sxs-lookup"><span data-stu-id="bc01f-103">Configure hello performance traffic routing method</span></span>

<span data-ttu-id="bc01f-104">Hello výkonu metodu směrování provozu umožňuje toodirect provoz toohello koncový bod s nejnižší latencí hello z hello klienta sítě.</span><span class="sxs-lookup"><span data-stu-id="bc01f-104">hello Performance traffic routing method allows you toodirect traffic toohello endpoint with hello lowest latency from hello client's network.</span></span> <span data-ttu-id="bc01f-105">Hello datacenter s nejnižší latencí hello je obvykle hello nejbližší v zeměpisné vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="bc01f-105">Typically, hello datacenter with hello lowest latency is hello closest in geographic distance.</span></span> <span data-ttu-id="bc01f-106">Tuto metodu směrování provozu nelze účet v reálném čase změny v konfiguraci sítě nebo zatížení.</span><span class="sxs-lookup"><span data-stu-id="bc01f-106">This traffic routing method cannot account for real-time changes in network configuration or load.</span></span>

##  <a name="tooconfigure-performance-routing-method"></a><span data-ttu-id="bc01f-107">metody směrování podle výkonu tooconfigure</span><span class="sxs-lookup"><span data-stu-id="bc01f-107">tooconfigure performance routing method</span></span>

1. <span data-ttu-id="bc01f-108">V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bc01f-108">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="bc01f-109">Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="bc01f-109">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="bc01f-110">V panelu vyhledávání hello portál, vyhledejte hello **profily Traffic Manageru** a pak klikněte na název profilu hello, který chcete tooconfigure hello metody směrování pro.</span><span class="sxs-lookup"><span data-stu-id="bc01f-110">In hello portal’s search bar, search for hello **Traffic Manager profiles** and then click hello profile name that you want tooconfigure hello routing method for.</span></span>
3. <span data-ttu-id="bc01f-111">V hello **profil služby Traffic Manager** okno, ověřte, že oba hello cloudové služby a nejsou weby, že chcete tooinclude ve vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="bc01f-111">In hello **Traffic Manager profile** blade, verify that both hello cloud services and websites that you want tooinclude in your configuration are present.</span></span>
4. <span data-ttu-id="bc01f-112">V hello **nastavení** klikněte na tlačítko **konfigurace**a v hello **konfigurace** okně dokončení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bc01f-112">In hello **Settings** section, click **Configuration**, and in hello **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="bc01f-113">Pro **nastavení metodu směrování provozu**, pro **metoda směrování** vyberte **výkonu**.</span><span class="sxs-lookup"><span data-stu-id="bc01f-113">For **traffic routing method settings**, for **Routing method** select **Performance**.</span></span>
    2. <span data-ttu-id="bc01f-114">Sada hello **nastavení monitorování koncového bodu** stejné pro všechny každý koncový bod v tomto profilu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bc01f-114">Set hello **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="bc01f-115">Vyberte odpovídající hello **protokol**a zadejte hello **Port** číslo.</span><span class="sxs-lookup"><span data-stu-id="bc01f-115">Select hello appropriate **Protocol**, and specify hello **Port** number.</span></span> 
        2. <span data-ttu-id="bc01f-116">Pro **cesta** zadejte lomítkem  */* .</span><span class="sxs-lookup"><span data-stu-id="bc01f-116">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="bc01f-117">toomonitor koncových bodů, musíte zadat cestu a název souboru.</span><span class="sxs-lookup"><span data-stu-id="bc01f-117">toomonitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="bc01f-118">A dopředné lomítko "/" je platná položka pro hello relativní cestu a znamená, že soubor hello je v kořenovém adresáři hello (výchozí).</span><span class="sxs-lookup"><span data-stu-id="bc01f-118">A forward slash "/" is a valid entry for hello relative path and implies that hello file is in hello root directory (default).</span></span>
        3. <span data-ttu-id="bc01f-119">V horní části hello hello stránky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bc01f-119">At hello top of hello page, click **Save**.</span></span>
5.  <span data-ttu-id="bc01f-120">Testovací hello změny v konfiguraci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bc01f-120">Test hello changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="bc01f-121">V panelu vyhledávání hello portál vyhledejte název profilu Traffic Manageru hello a klikněte na profil služby Traffic Manager hello ve výsledcích hello této hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bc01f-121">In hello portal’s search bar, search for hello Traffic Manager profile name and click hello Traffic Manager profile in hello results that hello displayed.</span></span>
    2.  <span data-ttu-id="bc01f-122">V hello **Traffic Manager** profilu okno, klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="bc01f-122">In hello **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="bc01f-123">Hello **profil služby Traffic Manager** zobrazuje název DNS hello vaše nově vytvořený profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="bc01f-123">hello **Traffic Manager profile** blade displays hello DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="bc01f-124">Toho můžete využít všechny klienty (například přechodem tooit pomocí webového prohlížeče) tooget směrovány toohello pravý koncový bod určeného typ směrování hello.</span><span class="sxs-lookup"><span data-stu-id="bc01f-124">This can be used by any clients (for example, by navigating tooit using a web browser) tooget routed toohello right endpoint as determined by hello routing type.</span></span> <span data-ttu-id="bc01f-125">V tomto případě jsou všechny žádosti směrované toohello koncový bod s nejnižší latenci hello z hello klienta sítě.</span><span class="sxs-lookup"><span data-stu-id="bc01f-125">In this case all requests are routed toohello endpoint with hello lowest latency from hello client's network.</span></span>
6. <span data-ttu-id="bc01f-126">Po váš profil služby Traffic Manager funguje, upravte záznam DNS hello na vaše autoritativní server toopoint DNS domény název toohello Traffic Manager název domény vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="bc01f-126">Once your Traffic Manager profile is working, edit hello DNS record on your authoritative DNS server toopoint your company domain name toohello Traffic Manager domain name.</span></span>

![Konfigurace metody směrování výkonu provozu pomocí Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="bc01f-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc01f-128">Next steps</span></span>

- <span data-ttu-id="bc01f-129">Další informace o [vážené metodu směrování provozu](traffic-manager-configure-weighted-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="bc01f-129">Learn about [weighted traffic routing method](traffic-manager-configure-weighted-routing-method.md).</span></span>
- <span data-ttu-id="bc01f-130">Další informace o [metody směrování s prioritou](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="bc01f-130">Learn about [priority routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="bc01f-131">Další informace o [geografické metody směrování](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="bc01f-131">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="bc01f-132">Zjistěte, jak příliš[Traffic Manager nastavení testů](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="bc01f-132">Learn how too[test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png