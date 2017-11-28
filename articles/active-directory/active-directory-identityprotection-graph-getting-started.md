---
title: "aaaGet začít s Azure Active Directory Identity Protection a Microsoft Graph | Microsoft Docs"
description: "Poskytuje úvod tooquery Microsoft Graph seznam rizikových událostech a přidružené informace ze služby Azure Active Directory."
services: active-directory
keywords: "ochrany identit Azure active directory, riziko událostí, ohrožení zabezpečení, zásady zabezpečení, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 75b8b7629a0120d8101f6fde0d9163122503d276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a><span data-ttu-id="39261-104">Začínáme s Azure Active Directory Identity Protection a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="39261-104">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>
<span data-ttu-id="39261-105">Microsoft Graph je hello Microsoft unified koncový bod rozhraní API a hello domácí z [Azure Active Directory Identity Protection](active-directory-identityprotection.md) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39261-105">Microsoft Graph is hello Microsoft unified API endpoint and hello home of [Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs.</span></span> <span data-ttu-id="39261-106">Hello prvního rozhraní API, **identityRiskEvents**, vám umožní tooquery Microsoft Graph seznam z [rizik události](active-directory-identityprotection-risk-events-types.md) a související informace.</span><span class="sxs-lookup"><span data-stu-id="39261-106">hello first API, **identityRiskEvents**, allows you tooquery Microsoft Graph for a list of [risk events](active-directory-identityprotection-risk-events-types.md) and associated information.</span></span> <span data-ttu-id="39261-107">Tento článek vám pomůže začít dotazování toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39261-107">This article gets you started querying this API.</span></span> <span data-ttu-id="39261-108">Podrobný úvod, úplnou dokumentaci a přístup toohello grafu Explorer najdete v tématu hello [Microsoft Graph lokality](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="39261-108">For an in-depth introduction, full documentation, and access toohello Graph Explorer, see hello [Microsoft Graph site](https://graph.microsoft.io/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="39261-109">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="39261-109">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span>

<span data-ttu-id="39261-110">Existují tři kroky tooaccessing Identity Protection data prostřednictvím Microsoft Graph:</span><span class="sxs-lookup"><span data-stu-id="39261-110">There are three steps tooaccessing Identity Protection data through Microsoft Graph:</span></span>

1. <span data-ttu-id="39261-111">Přidáte aplikaci s tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="39261-111">Add an application with a client secret.</span></span> 
2. <span data-ttu-id="39261-112">Použijte tento tajný klíč a několik dalších informace tooauthenticate tooMicrosoft grafu, kterou budete dostávat ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="39261-112">Use this secret and a few other pieces of information tooauthenticate tooMicrosoft Graph, where you receive an authentication token.</span></span> 
3. <span data-ttu-id="39261-113">Použijte tento koncový bod tokenu toomake požadavky toohello rozhraní API a vrátit data pro ochranu Identity.</span><span class="sxs-lookup"><span data-stu-id="39261-113">Use this token toomake requests toohello API endpoint and get Identity Protection data back.</span></span>

<span data-ttu-id="39261-114">Než začnete, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="39261-114">Before you get started, you’ll need:</span></span>

* <span data-ttu-id="39261-115">Správce oprávnění toocreate hello aplikace ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="39261-115">Administrator privileges toocreate hello application in Azure AD</span></span>
* <span data-ttu-id="39261-116">Hello název domény vašeho klienta (například contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="39261-116">hello name of your tenant's domain (for example, contoso.onmicrosoft.com)</span></span>

## <a name="add-an-application-with-a-client-secret"></a><span data-ttu-id="39261-117">Přidat aplikaci s tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="39261-117">Add an application with a client secret</span></span>
1. <span data-ttu-id="39261-118">[Přihlaste se](https://manage.windowsazure.com) tooyour portál Azure classic jako správce.</span><span class="sxs-lookup"><span data-stu-id="39261-118">[Sign in](https://manage.windowsazure.com) tooyour Azure classic portal as an administrator.</span></span> 
2. <span data-ttu-id="39261-119">V klikněte v levém navigačním podokně hello na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="39261-119">On on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. <span data-ttu-id="39261-121">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="39261-121">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
4. <span data-ttu-id="39261-122">V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="39261-122">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. <span data-ttu-id="39261-124">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="39261-124">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. <span data-ttu-id="39261-126">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="39261-126">On hello **What do you want toodo** dialog, click **Add an application my organization is developing**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. <span data-ttu-id="39261-128">Na hello **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="39261-128">On hello **Tell us about your application** dialog, perform hello following steps:</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    <span data-ttu-id="39261-130">a.</span><span class="sxs-lookup"><span data-stu-id="39261-130">a.</span></span> <span data-ttu-id="39261-131">V hello **název** textovému poli, zadejte název vaší aplikace (např: aplikace AADIP riziko událostí rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="39261-131">In hello **Name** textbox, type a name for your application (e.g.: AADIP Risk Event API Application).</span></span>
   
    <span data-ttu-id="39261-132">b.</span><span class="sxs-lookup"><span data-stu-id="39261-132">b.</span></span> <span data-ttu-id="39261-133">Jako **typ**, vyberte **webové aplikace nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="39261-133">As **Type**, select **Web Application And / Or Web API**.</span></span>
   
    <span data-ttu-id="39261-134">c.</span><span class="sxs-lookup"><span data-stu-id="39261-134">c.</span></span> <span data-ttu-id="39261-135">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="39261-135">Click **Next**.</span></span>
8. <span data-ttu-id="39261-136">Na hello **vlastností aplikace** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="39261-136">On hello **App properties** dialog, perform hello following steps:</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    <span data-ttu-id="39261-138">a.</span><span class="sxs-lookup"><span data-stu-id="39261-138">a.</span></span> <span data-ttu-id="39261-139">V hello **přihlašovací adresa URL** textovému poli, typ `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="39261-139">In hello **Sign-On URL** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="39261-140">b.</span><span class="sxs-lookup"><span data-stu-id="39261-140">b.</span></span> <span data-ttu-id="39261-141">V hello **identifikátor ID URI aplikace** textovému poli, typ `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="39261-141">In hello **App ID URI** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="39261-142">c.</span><span class="sxs-lookup"><span data-stu-id="39261-142">c.</span></span> <span data-ttu-id="39261-143">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="39261-143">Click **Complete**.</span></span>

<span data-ttu-id="39261-144">Můžete teď konfigurovat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="39261-144">Your can now configure your application.</span></span>

![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-toouse-hello-api"></a><span data-ttu-id="39261-146">Udělení oprávnění vaší aplikace toouse hello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="39261-146">Grant your application permission toouse hello API</span></span>
1. <span data-ttu-id="39261-147">Na stránce vaší aplikace, v nabídce hello hello nahoře, klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="39261-147">On your application's page, in hello menu on hello top, click **Configure**.</span></span> 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. <span data-ttu-id="39261-149">V hello **oprávnění tooother aplikace** klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="39261-149">In hello **permissions tooother applications** section, click **Add application**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. <span data-ttu-id="39261-151">Na hello **oprávnění tooother aplikace** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="39261-151">On hello **permissions tooother applications** dialog, perform hello following steps:</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    <span data-ttu-id="39261-153">a.</span><span class="sxs-lookup"><span data-stu-id="39261-153">a.</span></span> <span data-ttu-id="39261-154">Vyberte **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="39261-154">Select **Microsoft Graph**.</span></span>
   
    <span data-ttu-id="39261-155">b.</span><span class="sxs-lookup"><span data-stu-id="39261-155">b.</span></span> <span data-ttu-id="39261-156">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="39261-156">Click **Complete**.</span></span>
4. <span data-ttu-id="39261-157">Klikněte na tlačítko **oprávnění aplikací: 0**a potom vyberte **čtení informací o události riziko všechny identity**.</span><span class="sxs-lookup"><span data-stu-id="39261-157">Click **Application Permissions: 0**, and then select **Read all identity risk event information**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. <span data-ttu-id="39261-159">Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="39261-159">Click **Save** at hello bottom of hello page.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a><span data-ttu-id="39261-161">Získání přístupového klíče</span><span class="sxs-lookup"><span data-stu-id="39261-161">Get an access key</span></span>
1. <span data-ttu-id="39261-162">Na stránce vaší aplikace v hello **klíče** vyberte 1 rok jako doba trvání.</span><span class="sxs-lookup"><span data-stu-id="39261-162">On your application's page, in hello **keys** section, select 1 year as duration.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. <span data-ttu-id="39261-164">Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="39261-164">Click **Save** at hello bottom of hello page.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. <span data-ttu-id="39261-166">v části klíče hello hello hodnotu nově vytvořený klíč zkopírujte a vložte jej do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="39261-166">in hello keys section, copy hello value of your newly created key, and then paste it into a safe location.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > <span data-ttu-id="39261-168">Pokud tento klíč ztratíte, bude mít tooreturn toothis části a vytvořte nový klíč.</span><span class="sxs-lookup"><span data-stu-id="39261-168">If you lose this key, you will have tooreturn toothis section and create a new key.</span></span> <span data-ttu-id="39261-169">Zachovat tento klíč tajný klíč: každý, kdo je přístup ke svým datům.</span><span class="sxs-lookup"><span data-stu-id="39261-169">Keep this key a secret: anyone who has it can access your data.</span></span>
   > 
   > 
4. <span data-ttu-id="39261-170">V hello **vlastnosti** část, kopie hello **ID klienta**a pak ji vložit do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="39261-170">In hello **properties** section, copy hello **Client ID**, and then paste it into a safe location.</span></span> 

## <a name="authenticate-toomicrosoft-graph-and-query-hello-identity-risk-events-api"></a><span data-ttu-id="39261-171">Ověření tooMicrosoft grafu a dotaz hello Identity riziko události rozhraní API</span><span class="sxs-lookup"><span data-stu-id="39261-171">Authenticate tooMicrosoft Graph and query hello Identity Risk Events API</span></span>
<span data-ttu-id="39261-172">V tomto okamžiku byste měli mít:</span><span class="sxs-lookup"><span data-stu-id="39261-172">At this point, you should have:</span></span>

* <span data-ttu-id="39261-173">ID klienta Hello, které jste zkopírovali výše</span><span class="sxs-lookup"><span data-stu-id="39261-173">hello client ID you copied above</span></span>
* <span data-ttu-id="39261-174">Hello klíče, který jste zkopírovali výše</span><span class="sxs-lookup"><span data-stu-id="39261-174">hello key you copied above</span></span>
* <span data-ttu-id="39261-175">Hello název domény vašeho klienta</span><span class="sxs-lookup"><span data-stu-id="39261-175">hello name of your tenant's domain</span></span>

<span data-ttu-id="39261-176">tooauthenticate, odeslání a post požadavku příliš`https://login.microsoft.com` s hello následující parametry v textu hello:</span><span class="sxs-lookup"><span data-stu-id="39261-176">tooauthenticate, send a post request too`https://login.microsoft.com` with hello following parameters in hello body:</span></span>

* <span data-ttu-id="39261-177">grant_type: "**client_credentials**"</span><span class="sxs-lookup"><span data-stu-id="39261-177">grant_type: “**client_credentials**”</span></span>
* <span data-ttu-id="39261-178">prostředek: "**https://graph.microsoft.com**"</span><span class="sxs-lookup"><span data-stu-id="39261-178">resource: “**https://graph.microsoft.com**”</span></span>
* <span data-ttu-id="39261-179">client_id:<your client ID></span><span class="sxs-lookup"><span data-stu-id="39261-179">client_id: <your client ID></span></span>
* <span data-ttu-id="39261-180">tajný klíč client_secret:<your key></span><span class="sxs-lookup"><span data-stu-id="39261-180">client_secret: <your key></span></span>

> [!NOTE]
> <span data-ttu-id="39261-181">Potřebujete tooprovide hodnoty pro hello **client_id** a hello **tajný klíč client_secret** parametr.</span><span class="sxs-lookup"><span data-stu-id="39261-181">You need tooprovide values for hello **client_id** and hello **client_secret** parameter.</span></span>
> 
> 

<span data-ttu-id="39261-182">V případě úspěchu vrátí ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="39261-182">If successful, this returns an authentication token.</span></span>  
<span data-ttu-id="39261-183">hello toocall rozhraní API, vytvoří hlavičku s hello následující parametr:</span><span class="sxs-lookup"><span data-stu-id="39261-183">toocall hello API, create a header with hello following parameter:</span></span>

    `Authorization`=”<token_type> <access_token>"


<span data-ttu-id="39261-184">Při ověřování, můžete najít typ tokenu hello a přístupový token v hello vrátil tokenu.</span><span class="sxs-lookup"><span data-stu-id="39261-184">When authenticating, you can find hello token type and access token in hello returned token.</span></span>

<span data-ttu-id="39261-185">Odesílat hlavičku jako požadavek toohello, následující adresy URL rozhraní API:`https://graph.microsoft.com/beta/identityRiskEvents`</span><span class="sxs-lookup"><span data-stu-id="39261-185">Send this header as a request toohello following API URL: `https://graph.microsoft.com/beta/identityRiskEvents`</span></span>

<span data-ttu-id="39261-186">odpověď Hello, pokud bylo úspěšné, je kolekce identity rizikových událostech a související data ve formátu OData JSON, který lze analyzovat a zpracovávají podle svých potřeb hello.</span><span class="sxs-lookup"><span data-stu-id="39261-186">hello response, if successful, is a collection of identity risk events and associated data in hello OData JSON format, which can be parsed and handled as see fit.</span></span>

<span data-ttu-id="39261-187">Tady je ukázkový kód pro ověřování a volání rozhraní API hello pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="39261-187">Here’s sample code for authenticating and calling hello API using Powershell.</span></span>  
<span data-ttu-id="39261-188">Stačí přidat vaše ID klienta, klíče a klienta domény.</span><span class="sxs-lookup"><span data-stu-id="39261-188">Just add your client ID, key, and tenant domain.</span></span>

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a><span data-ttu-id="39261-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39261-189">Next steps</span></span>
<span data-ttu-id="39261-190">Blahopřejeme, jste právě provedli první volání tooMicrosoft grafu.</span><span class="sxs-lookup"><span data-stu-id="39261-190">Congratulations, you just made your first call tooMicrosoft Graph!</span></span>  
<span data-ttu-id="39261-191">Nyní můžete dotazovat identity rizikových událostech a používat hello data, ale svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="39261-191">Now you can query identity risk events and use hello data however you see fit.</span></span>

<span data-ttu-id="39261-192">toolearn Další informace o Microsoft Graph a jak hello toobuild aplikací pomocí rozhraní Graph API, projděte si hello [dokumentace](https://graph.microsoft.io/docs) zdaleka na hello [Microsoft Graph lokality](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="39261-192">toolearn more about Microsoft Graph and how toobuild applications using hello Graph API, check out hello [documentation](https://graph.microsoft.io/docs) and much more on hello [Microsoft Graph site](https://graph.microsoft.io/).</span></span> <span data-ttu-id="39261-193">Zajistěte také, zda text hello toobookmark [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) stránky, která obsahuje seznam všech hello API ochrany Identity k dispozici v grafu.</span><span class="sxs-lookup"><span data-stu-id="39261-193">Also, make sure toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) page that lists all of hello Identity Protection APIs available in Graph.</span></span> <span data-ttu-id="39261-194">Jak jsme přidat nové způsoby toowork s ochranou Identity prostřednictvím rozhraní API, uvidíte je na této stránce.</span><span class="sxs-lookup"><span data-stu-id="39261-194">As we add new ways toowork with Identity Protection via API, you’ll see them on that page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39261-195">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="39261-195">Additional resources</span></span>
* [<span data-ttu-id="39261-196">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39261-196">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
* [<span data-ttu-id="39261-197">Typy rizikových událostí detekovaných službou Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="39261-197">Types of risk events detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-risk-events-types.md)
* [<span data-ttu-id="39261-198">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="39261-198">Microsoft Graph</span></span>](https://graph.microsoft.io/)
* [<span data-ttu-id="39261-199">Přehled Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="39261-199">Overview of Microsoft Graph</span></span>](https://graph.microsoft.io/docs)
* [<span data-ttu-id="39261-200">Kořenovém adresáři služby Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="39261-200">Azure AD Identity Protection Service Root</span></span>](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

