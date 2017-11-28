---
title: "aaaGetting začít s Azure CDN | Microsoft Docs"
description: "Toto téma ukazuje, jak tooenable hello Azure Content Delivery Network (CDN). kurz Hello provede hello vytvoření nového profilu CDN a koncového bodu."
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
ms.openlocfilehash: 0ce9802bfd7b60e70a9a62330f5593fc17ea07d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-cdn"></a><span data-ttu-id="dc534-104">Začínáme s Azure CDN</span><span class="sxs-lookup"><span data-stu-id="dc534-104">Getting started with Azure CDN</span></span>
<span data-ttu-id="dc534-105">Toto téma vás provede povolením Azure CDN prostřednictvím vytvoření nového profilu a koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-105">This topic walks through enabling Azure CDN by creating a new CDN profile and endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc534-106">Úvod toohow CDN funguje, jakož i seznam funkcí, najdete v části hello [přehled CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dc534-106">For an introduction toohow CDN works, as well as a list of features, see hello [CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="dc534-107">Vytvoření nového profilu CDN</span><span class="sxs-lookup"><span data-stu-id="dc534-107">Create a new CDN profile</span></span>
<span data-ttu-id="dc534-108">Profil CDN je kolekcí koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-108">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="dc534-109">Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-109">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="dc534-110">Můžete toouse více profilů tooorganize koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.</span><span class="sxs-lookup"><span data-stu-id="dc534-110">You may wish toouse multiple profiles tooorganize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="dc534-111">Ve výchozím nastavení je jedno předplatné Azure je omezený tooeight profilů CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-111">By default, a single Azure subscription is limited tooeight CDN profiles.</span></span> <span data-ttu-id="dc534-112">Jednotlivé profily CDN jsou omezené tooten koncových bodů CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-112">Each CDN profile is limited tooten CDN endpoints.</span></span>
> 
> <span data-ttu-id="dc534-113">Ceny CDN se uplatní na úrovni profilu CDN hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-113">CDN pricing is applied at hello CDN profile level.</span></span> <span data-ttu-id="dc534-114">Pokud chcete toouse směs Azure CDN cenové úrovně, budete potřebovat víc profilů CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-114">If you wish toouse a mix of Azure CDN pricing tiers, you will need multiple CDN profiles.</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="dc534-115">Vytvoření nového koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="dc534-115">Create a new CDN endpoint</span></span>
<span data-ttu-id="dc534-116">**toocreate nový koncový bod CDN**</span><span class="sxs-lookup"><span data-stu-id="dc534-116">**toocreate a new CDN endpoint**</span></span>

1. <span data-ttu-id="dc534-117">V hello [portálu Azure](https://portal.azure.com), přejděte tooyour profil CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-117">In hello [Azure Portal](https://portal.azure.com), navigate tooyour CDN profile.</span></span>  <span data-ttu-id="dc534-118">Je může mít připnutý toohello řídicí panel v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-118">You may have pinned it toohello dashboard in hello previous step.</span></span>  <span data-ttu-id="dc534-119">Pokud není, můžete najdete ho kliknutím na **Procházet**, pak **profilů CDN**, a kliknutím na profil hello máte v plánu tooadd koncový bod.</span><span class="sxs-lookup"><span data-stu-id="dc534-119">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on hello profile you plan tooadd your endpoint to.</span></span>
   
    <span data-ttu-id="dc534-120">Otevře se okno profil CDN Hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-120">hello CDN profile blade appears.</span></span>
   
    ![Profil CDN][cdn-profile-settings]
2. <span data-ttu-id="dc534-122">Klikněte na tlačítko hello **přidat koncový bod** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dc534-122">Click hello **Add Endpoint** button.</span></span>
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    <span data-ttu-id="dc534-124">Hello **přidání koncového bodu** se zobrazí okno.</span><span class="sxs-lookup"><span data-stu-id="dc534-124">hello **Add an endpoint** blade appears.</span></span>
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. <span data-ttu-id="dc534-126">Zadejte **Název** tohoto koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-126">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="dc534-127">Tento název bude použité tooaccess prostředkům v mezipaměti v doméně hello `<endpointname>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="dc534-127">This name will be used tooaccess your cached resources at hello domain `<endpointname>.azureedge.net`.</span></span>
4. <span data-ttu-id="dc534-128">V hello **typ původu** rozevíracího seznamu, vyberte typ původu.</span><span class="sxs-lookup"><span data-stu-id="dc534-128">In hello **Origin type** dropdown, select your origin type.</span></span>  <span data-ttu-id="dc534-129">V případě účtu Azure Storage vyberte položku **Storage**, v případě cloudové služby Azure vyberte položku **Cloudová služba**, v případě webové aplikace Azure vyberte položku **Webová aplikace** a v případě jakéhokoli jiného veřejně přístupného původu webového serveru (hostovaného v Azure nebo jinde) vyberte položku **Vlastní původ**.</span><span class="sxs-lookup"><span data-stu-id="dc534-129">Select **Storage** for an Azure Storage account, **Cloud service** for an Azure Cloud Service, **Web App** for an Azure Web App, or **Custom origin** for any other publicly accessible web server origin (hosted in Azure or elsewhere).</span></span>
   
    ![Typ původu CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. <span data-ttu-id="dc534-131">V hello **název počátečního hostitele** rozevíracího seznamu, vyberte nebo zadejte doménu původu.</span><span class="sxs-lookup"><span data-stu-id="dc534-131">In hello **Origin hostname** dropdown, select or type your origin domain.</span></span>  <span data-ttu-id="dc534-132">rozevírací Hello se zobrazí seznam všech dostupných zdroje hello typu, který jste zadali v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="dc534-132">hello dropdown will list all available origins of hello type you specified in step 4.</span></span>  <span data-ttu-id="dc534-133">Pokud jste vybrali *vlastní původ* jako vaše **typ původu**, zadáte doménu vlastního původu hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-133">If you selected *Custom origin* as your **Origin type**, you will type in hello domain of your custom origin.</span></span>
6. <span data-ttu-id="dc534-134">V hello **cesty ke zdroji** textové pole, zadejte hello cesta toohello prostředky chcete toocache, nebo nechte prázdné tooallow mezipaměti libovolný prostředek v doméně hello zadané v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="dc534-134">In hello **Origin path** text box, enter hello path toohello resources you want toocache, or leave blank tooallow cache any resource at hello domain you specified in step 5.</span></span>
7. <span data-ttu-id="dc534-135">V hello **hlavičky hostitele počátku**, zadejte hlavičku hostitele hello chcete hello CDN toosend spolu s každou žádostí, nebo ponechte výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-135">In hello **Origin host header**, enter hello host header you want hello CDN toosend with each request, or leave hello default.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="dc534-136">Některé typy původu, například Azure Storage a Web Apps, vyžadují hello hostitele záhlaví toomatch hello domény hello původu.</span><span class="sxs-lookup"><span data-stu-id="dc534-136">Some types of origins, such as Azure Storage and Web Apps, require hello host header toomatch hello domain of hello origin.</span></span> <span data-ttu-id="dc534-137">Pokud nemáte původ, který vyžaduje hlavičku hostitele odlišnou od své domény, nechte hello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dc534-137">Unless you have an origin that requires a host header different from its domain, you should leave hello default value.</span></span>
   > 
   > 
8. <span data-ttu-id="dc534-138">Pro **protokol** a **port původu**, zadejte hello protokoly a porty používané tooaccess k prostředkům v původu hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-138">For **Protocol** and **Origin port**, specify hello protocols and ports used tooaccess your resources at hello origin.</span></span>  <span data-ttu-id="dc534-139">Je nutné vybrat alespoň jeden protokol (HTTP nebo HTTPS).</span><span class="sxs-lookup"><span data-stu-id="dc534-139">At least one protocol (HTTP or HTTPS) must be selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dc534-140">Hello **port původu** ovlivňuje pouze co koncový bod hello port používá tooretrieve informace z počátku hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-140">hello **Origin port** only affects what port hello endpoint uses tooretrieve information from hello origin.</span></span>  <span data-ttu-id="dc534-141">Hello koncový bod jako takový bude pouze klienti k dispozici tooend na hello výchozí porty HTTP a HTTPS (80 a 443), bez ohledu na to hello **port původu**.</span><span class="sxs-lookup"><span data-stu-id="dc534-141">hello endpoint itself will only be available tooend clients on hello default HTTP and HTTPS ports (80 and 443), regardless of hello **Origin port**.</span></span>  
   > 
   > <span data-ttu-id="dc534-142">**Azure CDN společnosti Akamai** koncové body neumožňují hello plný rozsah portů pro zdroje.</span><span class="sxs-lookup"><span data-stu-id="dc534-142">**Azure CDN from Akamai** endpoints do not allow hello full TCP port range for origins.</span></span>  <span data-ttu-id="dc534-143">Seznam nepovolených portů původu najdete v tématu [Povolené porty původu Azure CDN společnosti Akamai](https://msdn.microsoft.com/library/mt757337.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc534-143">For a list of origin ports that are not allowed, see [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx).</span></span>  
   > 
   > <span data-ttu-id="dc534-144">Přístup k CDN obsah pomocí protokolu HTTPS má hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="dc534-144">Accessing CDN content using HTTPS has hello following constraints:</span></span>
   > 
   > * <span data-ttu-id="dc534-145">Je nutné použít certifikát SSL hello poskytované hello CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-145">You must use hello SSL certificate provided by hello CDN.</span></span> <span data-ttu-id="dc534-146">Certifikáty třetích stran nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="dc534-146">Third party certificates are not supported.</span></span>
   > * <span data-ttu-id="dc534-147">Je nutné použít doménu poskytnutou CDN hello (`<endpointname>.azureedge.net`) tooaccess obsahu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dc534-147">You must use hello CDN-provided domain (`<endpointname>.azureedge.net`) tooaccess HTTPS content.</span></span> <span data-ttu-id="dc534-148">Podpora protokolu HTTPS není k dispozici pro názvy vlastních domén (záznamů CNAME), protože hello CDN v tuto chvíli nepodporuje vlastní certifikáty.</span><span class="sxs-lookup"><span data-stu-id="dc534-148">HTTPS support is not available for custom domain names (CNAMEs) since hello CDN does not support custom certificates at this time.</span></span>
   > 
   > 
9. <span data-ttu-id="dc534-149">Klikněte na tlačítko hello **přidat** tlačítko toocreate hello nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="dc534-149">Click hello **Add** button toocreate hello new endpoint.</span></span>
10. <span data-ttu-id="dc534-150">Po vytvoření koncového bodu hello se zobrazí v seznamu koncových bodů pro profil hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-150">Once hello endpoint is created, it appears in a list of endpoints for hello profile.</span></span> <span data-ttu-id="dc534-151">zobrazení seznamu Hello ukazuje, že hello URL toouse tooaccess do mezipaměti obsah, a také doménu původu hello.</span><span class="sxs-lookup"><span data-stu-id="dc534-151">hello list view shows hello URL toouse tooaccess cached content, as well as hello origin domain.</span></span>
    
    ![Koncový bod CDN][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > <span data-ttu-id="dc534-153">Hello koncový bod nebude hned dostupný pro použití, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.</span><span class="sxs-lookup"><span data-stu-id="dc534-153">hello endpoint will not immediately be available for use, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="dc534-154">V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.</span><span class="sxs-lookup"><span data-stu-id="dc534-154">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="dc534-155">V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.</span><span class="sxs-lookup"><span data-stu-id="dc534-155">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
    > 
    > <span data-ttu-id="dc534-156">Uživatelů, kteří chtějí název domény CDN hello toouse před hello konfigurace koncového bodu má rozšíří toohello bodů POP, obdrží kódy odpovědí HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="dc534-156">Users who try toouse hello CDN domain name before hello endpoint configuration has propagated toohello POPs will receive HTTP 404 response codes.</span></span>  <span data-ttu-id="dc534-157">Pokud jste vytvořili koncový bod před několika hodinami, a přesto obdržíte kód odpovědi 404, prostudujte prosím téma [Řešení problémů s koncovými body CDN, které vracejí stav 404](cdn-troubleshoot-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="dc534-157">If it's been several hours since you created your endpoint and you're still receiving 404 responses, please see [Troubleshooting CDN endpoints returning 404 statuses](cdn-troubleshoot-endpoint.md).</span></span>
    > 
    > 

## <a name="see-also"></a><span data-ttu-id="dc534-158">Viz také</span><span class="sxs-lookup"><span data-stu-id="dc534-158">See Also</span></span>
* [<span data-ttu-id="dc534-159">Řízení chování ukládání do mezipaměti u žádostí s řetězci dotazu</span><span class="sxs-lookup"><span data-stu-id="dc534-159">Controlling caching behavior of requests with query strings</span></span>](cdn-query-string.md)
* [<span data-ttu-id="dc534-160">Jak tooMap obsahu CDN tooa vlastní domény</span><span class="sxs-lookup"><span data-stu-id="dc534-160">How tooMap CDN Content tooa Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="dc534-161">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="dc534-161">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="dc534-162">Vyprázdnění koncového bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="dc534-162">Purge an Azure CDN Endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="dc534-163">Řešení problémů s koncovými body CDN, které vracejí stav 404</span><span class="sxs-lookup"><span data-stu-id="dc534-163">Troubleshooting CDN endpoints returning 404 statuses</span></span>](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
