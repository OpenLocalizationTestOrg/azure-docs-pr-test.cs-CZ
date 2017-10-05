---
title: "Začínáme s Azure CDN | Dokumentace Microsoftu"
description: "Toto téma ukazuje, jak povolit Azure Content Delivery Network (CDN). Kurz vás provede vytvořením nového profilu a koncového bodu CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d263e911d0d0b3cdc1e48e300a3c8a0994b38c39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-cdn"></a><span data-ttu-id="f835b-104">Začínáme s Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f835b-104">Getting started with Azure CDN</span></span>
<span data-ttu-id="f835b-105">Toto téma vás provede povolením Azure CDN prostřednictvím vytvoření nového profilu a koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-105">This topic walks through enabling Azure CDN by creating a new CDN profile and endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f835b-106">Úvod do fungování CDN a seznam funkcí najdete v tématu [Přehled CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f835b-106">For an introduction to how CDN works, as well as a list of features, see the [CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="f835b-107">Vytvoření nového profilu CDN</span><span class="sxs-lookup"><span data-stu-id="f835b-107">Create a new CDN profile</span></span>
<span data-ttu-id="f835b-108">Profil CDN je kolekcí koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-108">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="f835b-109">Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-109">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="f835b-110">Můžete použít více profilů a uspořádat koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.</span><span class="sxs-lookup"><span data-stu-id="f835b-110">You may wish to use multiple profiles to organize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="f835b-111">Ve výchozím nastavení je jedno předplatné Azure omezeno na osm profilů CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-111">By default, a single Azure subscription is limited to eight CDN profiles.</span></span> <span data-ttu-id="f835b-112">Jednotlivé profily CDN jsou omezené na deset koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-112">Each CDN profile is limited to ten CDN endpoints.</span></span>
> 
> <span data-ttu-id="f835b-113">Ceny CDN se uplatní na úrovni profilu CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-113">CDN pricing is applied at the CDN profile level.</span></span> <span data-ttu-id="f835b-114">Pokud chcete použít kombinaci cenových úrovní Azure CDN, budete potřebovat víc profilů CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-114">If you wish to use a mix of Azure CDN pricing tiers, you will need multiple CDN profiles.</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="f835b-115">Vytvoření nového koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="f835b-115">Create a new CDN endpoint</span></span>
<span data-ttu-id="f835b-116">**Postup vytvoření nového koncového bodu CDN**</span><span class="sxs-lookup"><span data-stu-id="f835b-116">**To create a new CDN endpoint**</span></span>

1. <span data-ttu-id="f835b-117">Na [webu Azure Portal](https://portal.azure.com) přejděte na svůj profil CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-117">In the [Azure Portal](https://portal.azure.com), navigate to your CDN profile.</span></span>  <span data-ttu-id="f835b-118">Je možné, že jste si ho v předchozím kroku připnuli k řídicímu panelu.</span><span class="sxs-lookup"><span data-stu-id="f835b-118">You may have pinned it to the dashboard in the previous step.</span></span>  <span data-ttu-id="f835b-119">Pokud ne, najdete ho kliknutím na položku **Procházet**, poté položku **Profily CDN** a nakonec kliknutím na profil, ke kterému plánujete přidat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f835b-119">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on the profile you plan to add your endpoint to.</span></span>
   
    <span data-ttu-id="f835b-120">Otevře se okno Profil CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-120">The CDN profile blade appears.</span></span>
   
    ![Profil CDN][cdn-profile-settings]
2. <span data-ttu-id="f835b-122">Klikněte na tlačítko **Přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="f835b-122">Click the **Add Endpoint** button.</span></span>
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    <span data-ttu-id="f835b-124">Otevře se okno **Přidání koncového bodu**.</span><span class="sxs-lookup"><span data-stu-id="f835b-124">The **Add an endpoint** blade appears.</span></span>
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. <span data-ttu-id="f835b-126">Zadejte **Název** tohoto koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-126">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="f835b-127">Tento název se použije pro přístup k prostředkům v mezipaměti v doméně `<endpointname>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="f835b-127">This name will be used to access your cached resources at the domain `<endpointname>.azureedge.net`.</span></span>
4. <span data-ttu-id="f835b-128">V rozevíracím seznamu **Typ původu** vyberte typ původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-128">In the **Origin type** dropdown, select your origin type.</span></span>  <span data-ttu-id="f835b-129">V případě účtu Azure Storage vyberte položku **Storage**, v případě cloudové služby Azure vyberte položku **Cloudová služba**, v případě webové aplikace Azure vyberte položku **Webová aplikace** a v případě jakéhokoli jiného veřejně přístupného původu webového serveru (hostovaného v Azure nebo jinde) vyberte položku **Vlastní původ**.</span><span class="sxs-lookup"><span data-stu-id="f835b-129">Select **Storage** for an Azure Storage account, **Cloud service** for an Azure Cloud Service, **Web App** for an Azure Web App, or **Custom origin** for any other publicly accessible web server origin (hosted in Azure or elsewhere).</span></span>
   
    ![Typ původu CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. <span data-ttu-id="f835b-131">V rozevíracím seznamu **Název hostitele původu** vyberte nebo zadejte doménu původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-131">In the **Origin hostname** dropdown, select or type your origin domain.</span></span>  <span data-ttu-id="f835b-132">Tento rozevírací seznam bude obsahovat všechny dostupné původy typu, který jste zadali v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="f835b-132">The dropdown will list all available origins of the type you specified in step 4.</span></span>  <span data-ttu-id="f835b-133">Pokud jste jako **Typ původu** vybrali možnost *Vlastní původ*, zadáte doménu vlastního původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-133">If you selected *Custom origin* as your **Origin type**, you will type in the domain of your custom origin.</span></span>
6. <span data-ttu-id="f835b-134">Do textového pole **Cesta původu** zadejte cestu k prostředkům, které chcete uložit do mezipaměti, nebo ponechte pole prázdné, čímž povolíte, aby byl do mezipaměti uložen libovolný prostředek v doméně zadané v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="f835b-134">In the **Origin path** text box, enter the path to the resources you want to cache, or leave blank to allow cache any resource at the domain you specified in step 5.</span></span>
7. <span data-ttu-id="f835b-135">Do pole **Hlavička hostitele původu** zadejte hlavičku hostitele, kterou má CDN odeslat spolu s každou žádostí, nebo ponechte výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="f835b-135">In the **Origin host header**, enter the host header you want the CDN to send with each request, or leave the default.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="f835b-136">Některé typy původu (například Azure Storage a Web Apps) vyžadují, aby se hlavička hostitele shodovala s doménou původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-136">Some types of origins, such as Azure Storage and Web Apps, require the host header to match the domain of the origin.</span></span> <span data-ttu-id="f835b-137">Pokud nemáte původ, který vyžaduje hlavičku hostitele odlišnou od své domény, je vhodné ponechat výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f835b-137">Unless you have an origin that requires a host header different from its domain, you should leave the default value.</span></span>
   > 
   > 
8. <span data-ttu-id="f835b-138">V polích **Protokol** a **Port původu** určete protokoly a porty sloužící k přístupu k prostředkům v původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-138">For **Protocol** and **Origin port**, specify the protocols and ports used to access your resources at the origin.</span></span>  <span data-ttu-id="f835b-139">Je nutné vybrat alespoň jeden protokol (HTTP nebo HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f835b-139">At least one protocol (HTTP or HTTPS) must be selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f835b-140">**Port původu** má vliv pouze na port použitý koncovým bodem k načtení informací z původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-140">The **Origin port** only affects what port the endpoint uses to retrieve information from the origin.</span></span>  <span data-ttu-id="f835b-141">Koncový bod jako takový bude dostupný pouze koncovým klientům na výchozích portech HTTP a HTTPS (80 a 443), a to bez ohledu na **Port původu**.</span><span class="sxs-lookup"><span data-stu-id="f835b-141">The endpoint itself will only be available to end clients on the default HTTP and HTTPS ports (80 and 443), regardless of the **Origin port**.</span></span>  
   > 
   > <span data-ttu-id="f835b-142">Koncové body **Azure CDN společnosti Akamai** neumožňují použít u původu plný rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="f835b-142">**Azure CDN from Akamai** endpoints do not allow the full TCP port range for origins.</span></span>  <span data-ttu-id="f835b-143">Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).</span><span class="sxs-lookup"><span data-stu-id="f835b-143">For a list of origin ports that are not allowed, see [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx).</span></span>  
   > 
   > <span data-ttu-id="f835b-144">Přístup k obsahu CDN pomocí protokolu HTTPS má tato omezení:</span><span class="sxs-lookup"><span data-stu-id="f835b-144">Accessing CDN content using HTTPS has the following constraints:</span></span>
   > 
   > * <span data-ttu-id="f835b-145">Je nutné použít certifikát SSL poskytnutý systémem CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-145">You must use the SSL certificate provided by the CDN.</span></span> <span data-ttu-id="f835b-146">Certifikáty třetích stran nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="f835b-146">Third party certificates are not supported.</span></span>
   > * <span data-ttu-id="f835b-147">Pro přístup k obsahu HTTPS je nutné použít doménu poskytnutou systémem CDN (`<endpointname>.azureedge.net`).</span><span class="sxs-lookup"><span data-stu-id="f835b-147">You must use the CDN-provided domain (`<endpointname>.azureedge.net`) to access HTTPS content.</span></span> <span data-ttu-id="f835b-148">U vlastních názvů domén (záznamů CNAME) není dostupná podpora protokolu HTTPS, protože CDN aktuálně nepodporuje vlastní certifikáty.</span><span class="sxs-lookup"><span data-stu-id="f835b-148">HTTPS support is not available for custom domain names (CNAMEs) since the CDN does not support custom certificates at this time.</span></span>
   > 
   > 
9. <span data-ttu-id="f835b-149">Kliknutím na tlačítko **Přidat** vytvořte nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f835b-149">Click the **Add** button to create the new endpoint.</span></span>
10. <span data-ttu-id="f835b-150">Jakmile je koncový bod vytvořený, zobrazí se v seznamu koncových bodů daného profilu.</span><span class="sxs-lookup"><span data-stu-id="f835b-150">Once the endpoint is created, it appears in a list of endpoints for the profile.</span></span> <span data-ttu-id="f835b-151">Zobrazení seznamu zobrazuje adresu URL, kterou je nutné použít k přístupu k obsahu v mezipaměti, a také doménu původu.</span><span class="sxs-lookup"><span data-stu-id="f835b-151">The list view shows the URL to use to access cached content, as well as the origin domain.</span></span>
    
    ![Koncový bod CDN][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > <span data-ttu-id="f835b-153">Koncový bod nebude hned dostupný pro použití, protože chvíli trvá, než se registrace rozšíří v rámci CDN.</span><span class="sxs-lookup"><span data-stu-id="f835b-153">The endpoint will not immediately be available for use, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="f835b-154">V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.</span><span class="sxs-lookup"><span data-stu-id="f835b-154">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="f835b-155">V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.</span><span class="sxs-lookup"><span data-stu-id="f835b-155">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
    > 
    > <span data-ttu-id="f835b-156">Pokud se uživatel pokusí použít název domény CDN dřív, než se konfigurace koncového bodu rozšíří do bodů POP, obdrží kódy odpovědí protokolu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="f835b-156">Users who try to use the CDN domain name before the endpoint configuration has propagated to the POPs will receive HTTP 404 response codes.</span></span>  <span data-ttu-id="f835b-157">Pokud jste vytvořili koncový bod před několika hodinami, a přesto obdržíte kód odpovědi 404, prostudujte prosím téma [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="f835b-157">If it's been several hours since you created your endpoint and you're still receiving 404 responses, please see [Troubleshooting CDN endpoints returning 404 statuses](cdn-troubleshoot-endpoint.md).</span></span>
    > 
    > 

## <a name="see-also"></a><span data-ttu-id="f835b-158">Viz také</span><span class="sxs-lookup"><span data-stu-id="f835b-158">See Also</span></span>
* [<span data-ttu-id="f835b-159">Řízení chování ukládání do mezipaměti u žádostí s řetězci dotazu</span><span class="sxs-lookup"><span data-stu-id="f835b-159">Controlling caching behavior of requests with query strings</span></span>](cdn-query-string.md)
* [<span data-ttu-id="f835b-160">Postup mapování obsahu CDN do vlastní domény</span><span class="sxs-lookup"><span data-stu-id="f835b-160">How to Map CDN Content to a Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="f835b-161">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f835b-161">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="f835b-162">Vyprázdnění koncového bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f835b-162">Purge an Azure CDN Endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="f835b-163">Řešení problémů s koncovými body CDN, které vracejí stav 404</span><span class="sxs-lookup"><span data-stu-id="f835b-163">Troubleshooting CDN endpoints returning 404 statuses</span></span>](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
