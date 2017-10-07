---
title: "pomocí Azure Traffic Manager metodu směrování provozu aaaConfigure vážené kruhové dotazování | Microsoft Docs"
description: "Tento článek vysvětluje, jak tooload vyrovnávat přenosy pomocí jiné metody kruhového dotazování v Traffic Manageru"
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
ms.openlocfilehash: 7e2866ead0b2b653845435dd420a763c5e175f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-weighted-traffic-routing-method-in-traffic-manager"></a><span data-ttu-id="72875-103">Konfigurace metody směrování provozu hello vážené v Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="72875-103">Configure hello weighted traffic routing method in Traffic Manager</span></span>

<span data-ttu-id="72875-104">Běžné vzor metoda směrování provozu je tooprovide sadu identické koncové body, které zahrnují cloudové služby a weby a odeslat tooeach provoz v kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="72875-104">A common traffic routing method pattern is tooprovide a set of identical endpoints, which include cloud services and websites, and send traffic tooeach in a round-robin fashion.</span></span> <span data-ttu-id="72875-105">Hello následující kroky popisují, jak tooconfigure tento typ metoda směrování provozu.</span><span class="sxs-lookup"><span data-stu-id="72875-105">hello following steps outline how tooconfigure this type of traffic routing method.</span></span>

> [!NOTE]
> <span data-ttu-id="72875-106">Weby Azure již poskytují funkce pro weby v rámci datového centra (označované také jako oblast) pro vyrovnávání zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="72875-106">Azure Websites already provide round-robin load balancing functionality for websites within a datacenter (also known as a region).</span></span> <span data-ttu-id="72875-107">Traffic Manager umožňuje toospecify metodu směrování provozu kruhovým dotazováním pro weby v různých datových centrech.</span><span class="sxs-lookup"><span data-stu-id="72875-107">Traffic Manager allows you toospecify round-robin traffic routing method for websites in different datacenters.</span></span>

## <a name="tooconfigure-hello-weighted-traffic-routing-method"></a><span data-ttu-id="72875-108">tooconfigure hello vážené metodu směrování provozu</span><span class="sxs-lookup"><span data-stu-id="72875-108">tooconfigure hello weighted traffic routing method</span></span>

1. <span data-ttu-id="72875-109">V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="72875-109">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="72875-110">Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="72875-110">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="72875-111">V panelu vyhledávání hello portál, vyhledejte hello **profily Traffic Manageru** a pak klikněte na název profilu hello, který chcete tooconfigure hello metody směrování pro.</span><span class="sxs-lookup"><span data-stu-id="72875-111">In hello portal’s search bar, search for hello **Traffic Manager profiles** and then click hello profile name that you want tooconfigure hello routing method for.</span></span>
3. <span data-ttu-id="72875-112">V hello **profil služby Traffic Manager** okno, ověřte, že oba hello cloudové služby a nejsou weby, že chcete tooinclude ve vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="72875-112">In hello **Traffic Manager profile** blade, verify that both hello cloud services and websites that you want tooinclude in your configuration are present.</span></span>
4. <span data-ttu-id="72875-113">V hello **nastavení** klikněte na tlačítko **konfigurace**a v hello **konfigurace** okně dokončení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="72875-113">In hello **Settings** section, click **Configuration**, and in hello **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="72875-114">Pro **nastavení metodu směrování provozu**, ověřte, zda text hello metodu směrování provozu je **vážená**.</span><span class="sxs-lookup"><span data-stu-id="72875-114">For **traffic routing method settings**, verify that hello traffic routing method is **Weighted**.</span></span> <span data-ttu-id="72875-115">Pokud není, klikněte na tlačítko **vážená** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="72875-115">If it is not, click **Weighted** from hello dropdown list.</span></span>
    2. <span data-ttu-id="72875-116">Sada hello **nastavení monitorování koncového bodu** stejné pro všechny každý koncový bod v tomto profilu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="72875-116">Set hello **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="72875-117">Vyberte odpovídající hello **protokol**a zadejte hello **Port** číslo.</span><span class="sxs-lookup"><span data-stu-id="72875-117">Select hello appropriate **Protocol**, and specify hello **Port** number.</span></span> 
        2. <span data-ttu-id="72875-118">Pro **cesta** zadejte lomítkem  */* .</span><span class="sxs-lookup"><span data-stu-id="72875-118">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="72875-119">toomonitor koncových bodů, musíte zadat cestu a název souboru.</span><span class="sxs-lookup"><span data-stu-id="72875-119">toomonitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="72875-120">A dopředné lomítko "/" je platná položka pro hello relativní cestu a znamená, že soubor hello je v kořenovém adresáři hello (výchozí).</span><span class="sxs-lookup"><span data-stu-id="72875-120">A forward slash "/" is a valid entry for hello relative path and implies that hello file is in hello root directory (default).</span></span>
        3. <span data-ttu-id="72875-121">V horní části hello hello stránky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="72875-121">At hello top of hello page, click **Save**.</span></span>
5. <span data-ttu-id="72875-122">Testovací hello změny v konfiguraci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="72875-122">Test hello changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="72875-123">V panelu vyhledávání hello portál vyhledejte název profilu Traffic Manageru hello a klikněte na profil služby Traffic Manager hello ve výsledcích hello této hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="72875-123">In hello portal’s search bar, search for hello Traffic Manager profile name and click hello Traffic Manager profile in hello results that hello displayed.</span></span>
    2.  <span data-ttu-id="72875-124">V hello **Traffic Manager** profilu okno, klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="72875-124">In hello **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="72875-125">Hello **profil služby Traffic Manager** zobrazuje název DNS hello vaše nově vytvořený profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="72875-125">hello **Traffic Manager profile** blade displays hello DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="72875-126">Toho můžete využít všechny klienty (například přechodem tooit pomocí webového prohlížeče) tooget směrovány toohello pravý koncový bod určeného typ směrování hello.</span><span class="sxs-lookup"><span data-stu-id="72875-126">This can be used by any clients (for example,by navigating tooit using a web browser) tooget routed toohello right endpoint as determined by hello routing type.</span></span> <span data-ttu-id="72875-127">V takovém případě jsou směrovat všechny požadavky každý koncový bod v kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="72875-127">In this case all requests are routed each endpoint in a round-robin fashion.</span></span>
6. <span data-ttu-id="72875-128">Po váš profil služby Traffic Manager funguje, upravte záznam DNS hello na vaše autoritativní server toopoint DNS domény název toohello Traffic Manager název domény vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="72875-128">Once your Traffic Manager profile is working, edit hello DNS record on your authoritative DNS server toopoint your company domain name toohello Traffic Manager domain name.</span></span>

![Konfigurace metody směrování vyvážené provozu pomocí Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="72875-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72875-130">Next steps</span></span>

- <span data-ttu-id="72875-131">Další informace o [metoda směrování provozu s prioritou](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="72875-131">Learn about [priority traffic routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="72875-132">Další informace o [metoda směrování provozu výkonu](traffic-manager-configure-performance-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="72875-132">Learn about [performance traffic routing method](traffic-manager-configure-performance-routing-method.md).</span></span>
- <span data-ttu-id="72875-133">Další informace o [geografické metody směrování](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="72875-133">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="72875-134">Zjistěte, jak příliš[Traffic Manager nastavení testů](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="72875-134">Learn how too[test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
