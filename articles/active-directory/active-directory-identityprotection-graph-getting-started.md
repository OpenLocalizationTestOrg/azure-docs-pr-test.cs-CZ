---
title: "Začínáme s Azure Active Directory Identity Protection a Microsoft Graph | Microsoft Docs"
description: "Poskytuje úvod do dotazů Microsoft Graph seznam rizikových událostech a přidružené informace ze služby Azure Active Directory."
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
ms.openlocfilehash: 9b01ff86da6a1fd4a439a6ba59ea15ed6480cdad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a><span data-ttu-id="b1d58-104">Začínáme s Azure Active Directory Identity Protection a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="b1d58-104">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>
<span data-ttu-id="b1d58-105">Microsoft Graph je Microsoft unified koncový bod rozhraní API a součástí [Azure Active Directory Identity Protection](active-directory-identityprotection.md) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b1d58-105">Microsoft Graph is the Microsoft unified API endpoint and the home of [Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs.</span></span> <span data-ttu-id="b1d58-106">Rozhraní API first **identityRiskEvents**, umožňuje dotazování Microsoft Graph seznam [rizik události](active-directory-identityprotection-risk-events-types.md) a související informace.</span><span class="sxs-lookup"><span data-stu-id="b1d58-106">The first API, **identityRiskEvents**, allows you to query Microsoft Graph for a list of [risk events](active-directory-identityprotection-risk-events-types.md) and associated information.</span></span> <span data-ttu-id="b1d58-107">Tento článek vám pomůže začít dotazování toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b1d58-107">This article gets you started querying this API.</span></span> <span data-ttu-id="b1d58-108">Podrobný úvod, úplnou dokumentaci a přístup do Průzkumníka grafu naleznete v tématu [Microsoft Graph lokality](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="b1d58-108">For an in-depth introduction, full documentation, and access to the Graph Explorer, see the [Microsoft Graph site](https://graph.microsoft.io/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b1d58-109">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="b1d58-109">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span>

<span data-ttu-id="b1d58-110">Přístup k datům ochrany identit pomocí Microsoft Graph tři kroky:</span><span class="sxs-lookup"><span data-stu-id="b1d58-110">There are three steps to accessing Identity Protection data through Microsoft Graph:</span></span>

1. <span data-ttu-id="b1d58-111">Přidáte aplikaci s tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="b1d58-111">Add an application with a client secret.</span></span> 
2. <span data-ttu-id="b1d58-112">Tento tajný klíč a několik další požadované informace můžete použijte k ověření do aplikace Microsoft Graph, kterou budete dostávat ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="b1d58-112">Use this secret and a few other pieces of information to authenticate to Microsoft Graph, where you receive an authentication token.</span></span> 
3. <span data-ttu-id="b1d58-113">Pomocí tohoto tokenu provádět požadavky na koncový bod rozhraní API a vrátit data pro ochranu Identity.</span><span class="sxs-lookup"><span data-stu-id="b1d58-113">Use this token to make requests to the API endpoint and get Identity Protection data back.</span></span>

<span data-ttu-id="b1d58-114">Než začnete, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="b1d58-114">Before you get started, you’ll need:</span></span>

* <span data-ttu-id="b1d58-115">Oprávnění správce k vytvoření aplikace ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1d58-115">Administrator privileges to create the application in Azure AD</span></span>
* <span data-ttu-id="b1d58-116">Název domény vašeho klienta (například contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="b1d58-116">The name of your tenant's domain (for example, contoso.onmicrosoft.com)</span></span>

## <a name="add-an-application-with-a-client-secret"></a><span data-ttu-id="b1d58-117">Přidat aplikaci s tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="b1d58-117">Add an application with a client secret</span></span>
1. <span data-ttu-id="b1d58-118">[Přihlaste se](https://manage.windowsazure.com) na portálu Azure classic jako správce.</span><span class="sxs-lookup"><span data-stu-id="b1d58-118">[Sign in](https://manage.windowsazure.com) to your Azure classic portal as an administrator.</span></span> 
2. <span data-ttu-id="b1d58-119">V klikněte v levém navigačním podokně na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-119">On on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. <span data-ttu-id="b1d58-121">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="b1d58-121">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
4. <span data-ttu-id="b1d58-122">V nabídce v horní části, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-122">In the menu on the top, click **Applications**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. <span data-ttu-id="b1d58-124">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="b1d58-124">Click **Add** at the bottom of the page.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. <span data-ttu-id="b1d58-126">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-126">On the **What do you want to do** dialog, click **Add an application my organization is developing**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. <span data-ttu-id="b1d58-128">Na **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1d58-128">On the **Tell us about your application** dialog, perform the following steps:</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    <span data-ttu-id="b1d58-130">a.</span><span class="sxs-lookup"><span data-stu-id="b1d58-130">a.</span></span> <span data-ttu-id="b1d58-131">V **název** textovému poli, zadejte název vaší aplikace (např: aplikace AADIP riziko událostí rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="b1d58-131">In the **Name** textbox, type a name for your application (e.g.: AADIP Risk Event API Application).</span></span>
   
    <span data-ttu-id="b1d58-132">b.</span><span class="sxs-lookup"><span data-stu-id="b1d58-132">b.</span></span> <span data-ttu-id="b1d58-133">Jako **typ**, vyberte **webové aplikace nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-133">As **Type**, select **Web Application And / Or Web API**.</span></span>
   
    <span data-ttu-id="b1d58-134">c.</span><span class="sxs-lookup"><span data-stu-id="b1d58-134">c.</span></span> <span data-ttu-id="b1d58-135">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-135">Click **Next**.</span></span>
8. <span data-ttu-id="b1d58-136">Na **vlastností aplikace** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1d58-136">On the **App properties** dialog, perform the following steps:</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    <span data-ttu-id="b1d58-138">a.</span><span class="sxs-lookup"><span data-stu-id="b1d58-138">a.</span></span> <span data-ttu-id="b1d58-139">V **přihlašovací adresa URL** textovému poli, typ `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="b1d58-139">In the **Sign-On URL** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="b1d58-140">b.</span><span class="sxs-lookup"><span data-stu-id="b1d58-140">b.</span></span> <span data-ttu-id="b1d58-141">V **identifikátor ID URI aplikace** textovému poli, typ `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="b1d58-141">In the **App ID URI** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="b1d58-142">c.</span><span class="sxs-lookup"><span data-stu-id="b1d58-142">c.</span></span> <span data-ttu-id="b1d58-143">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-143">Click **Complete**.</span></span>

<span data-ttu-id="b1d58-144">Můžete teď konfigurovat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1d58-144">Your can now configure your application.</span></span>

![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a><span data-ttu-id="b1d58-146">Udělit oprávnění vaše aplikace k používání rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b1d58-146">Grant your application permission to use the API</span></span>
1. <span data-ttu-id="b1d58-147">Na stránce vaší aplikace, v nabídce v horní části klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-147">On your application's page, in the menu on the top, click **Configure**.</span></span> 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. <span data-ttu-id="b1d58-149">V **oprávnění k ostatním aplikacím** klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-149">In the **permissions to other applications** section, click **Add application**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. <span data-ttu-id="b1d58-151">Na **oprávnění k ostatním aplikacím** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1d58-151">On the **permissions to other applications** dialog, perform the following steps:</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    <span data-ttu-id="b1d58-153">a.</span><span class="sxs-lookup"><span data-stu-id="b1d58-153">a.</span></span> <span data-ttu-id="b1d58-154">Vyberte **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-154">Select **Microsoft Graph**.</span></span>
   
    <span data-ttu-id="b1d58-155">b.</span><span class="sxs-lookup"><span data-stu-id="b1d58-155">b.</span></span> <span data-ttu-id="b1d58-156">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-156">Click **Complete**.</span></span>
4. <span data-ttu-id="b1d58-157">Klikněte na tlačítko **oprávnění aplikací: 0**a potom vyberte **čtení informací o události riziko všechny identity**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-157">Click **Application Permissions: 0**, and then select **Read all identity risk event information**.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. <span data-ttu-id="b1d58-159">V dolní části stránky klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-159">Click **Save** at the bottom of the page.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a><span data-ttu-id="b1d58-161">Získání přístupového klíče</span><span class="sxs-lookup"><span data-stu-id="b1d58-161">Get an access key</span></span>
1. <span data-ttu-id="b1d58-162">Na stránce vaší aplikace v **klíče** vyberte 1 rok jako doba trvání.</span><span class="sxs-lookup"><span data-stu-id="b1d58-162">On your application's page, in the **keys** section, select 1 year as duration.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. <span data-ttu-id="b1d58-164">V dolní části stránky klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b1d58-164">Click **Save** at the bottom of the page.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. <span data-ttu-id="b1d58-166">v části klíče hodnotu nově vytvořený klíč zkopírujte a vložte jej do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="b1d58-166">in the keys section, copy the value of your newly created key, and then paste it into a safe location.</span></span>
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > <span data-ttu-id="b1d58-168">Pokud tento klíč ztratíte, budete muset vraťte do této části a vytvořte nový klíč.</span><span class="sxs-lookup"><span data-stu-id="b1d58-168">If you lose this key, you will have to return to this section and create a new key.</span></span> <span data-ttu-id="b1d58-169">Zachovat tento klíč tajný klíč: každý, kdo je přístup ke svým datům.</span><span class="sxs-lookup"><span data-stu-id="b1d58-169">Keep this key a secret: anyone who has it can access your data.</span></span>
   > 
   > 
4. <span data-ttu-id="b1d58-170">V **vlastnosti** část, zkopírujte **ID klienta**a pak ji vložit do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="b1d58-170">In the **properties** section, copy the **Client ID**, and then paste it into a safe location.</span></span> 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a><span data-ttu-id="b1d58-171">Ověřování do aplikace Microsoft Graph a dotaz rozhraní API Identity riziko události</span><span class="sxs-lookup"><span data-stu-id="b1d58-171">Authenticate to Microsoft Graph and query the Identity Risk Events API</span></span>
<span data-ttu-id="b1d58-172">V tomto okamžiku byste měli mít:</span><span class="sxs-lookup"><span data-stu-id="b1d58-172">At this point, you should have:</span></span>

* <span data-ttu-id="b1d58-173">ID klienta, které jste zkopírovali výše</span><span class="sxs-lookup"><span data-stu-id="b1d58-173">The client ID you copied above</span></span>
* <span data-ttu-id="b1d58-174">Klíč, který jste zkopírovali výše</span><span class="sxs-lookup"><span data-stu-id="b1d58-174">The key you copied above</span></span>
* <span data-ttu-id="b1d58-175">Název domény vašeho klienta</span><span class="sxs-lookup"><span data-stu-id="b1d58-175">The name of your tenant's domain</span></span>

<span data-ttu-id="b1d58-176">K ověření, odeslat požadavek post do `https://login.microsoft.com` s následujícími parametry v textu:</span><span class="sxs-lookup"><span data-stu-id="b1d58-176">To authenticate, send a post request to `https://login.microsoft.com` with the following parameters in the body:</span></span>

* <span data-ttu-id="b1d58-177">grant_type: "**client_credentials**"</span><span class="sxs-lookup"><span data-stu-id="b1d58-177">grant_type: “**client_credentials**”</span></span>
* <span data-ttu-id="b1d58-178">prostředek: "**https://graph.microsoft.com**"</span><span class="sxs-lookup"><span data-stu-id="b1d58-178">resource: “**https://graph.microsoft.com**”</span></span>
* <span data-ttu-id="b1d58-179">client_id:<your client ID></span><span class="sxs-lookup"><span data-stu-id="b1d58-179">client_id: <your client ID></span></span>
* <span data-ttu-id="b1d58-180">tajný klíč client_secret:<your key></span><span class="sxs-lookup"><span data-stu-id="b1d58-180">client_secret: <your key></span></span>

> [!NOTE]
> <span data-ttu-id="b1d58-181">Je nutné zadat hodnoty **client_id** a **tajný klíč client_secret** parametr.</span><span class="sxs-lookup"><span data-stu-id="b1d58-181">You need to provide values for the **client_id** and the **client_secret** parameter.</span></span>
> 
> 

<span data-ttu-id="b1d58-182">V případě úspěchu vrátí ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="b1d58-182">If successful, this returns an authentication token.</span></span>  
<span data-ttu-id="b1d58-183">Pro volání rozhraní API, vytvoří hlavičku s následující parametr:</span><span class="sxs-lookup"><span data-stu-id="b1d58-183">To call the API, create a header with the following parameter:</span></span>

    `Authorization`=”<token_type> <access_token>"


<span data-ttu-id="b1d58-184">Při ověřování, můžete najít typ tokenu a přístupový token v vrácený tokenu.</span><span class="sxs-lookup"><span data-stu-id="b1d58-184">When authenticating, you can find the token type and access token in the returned token.</span></span>

<span data-ttu-id="b1d58-185">Odeslat tuto hlavičku jako požadavek na následující adresu URL rozhraní API:`https://graph.microsoft.com/beta/identityRiskEvents`</span><span class="sxs-lookup"><span data-stu-id="b1d58-185">Send this header as a request to the following API URL: `https://graph.microsoft.com/beta/identityRiskEvents`</span></span>

<span data-ttu-id="b1d58-186">Odpověď, a to v případě úspěchu je kolekce identity rizikových událostech a přidružená data ve formátu OData JSON, který lze analyzovat a zpracovávají podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="b1d58-186">The response, if successful, is a collection of identity risk events and associated data in the OData JSON format, which can be parsed and handled as see fit.</span></span>

<span data-ttu-id="b1d58-187">Tady je ukázkový kód pro ověřování a volání rozhraní API pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="b1d58-187">Here’s sample code for authenticating and calling the API using Powershell.</span></span>  
<span data-ttu-id="b1d58-188">Stačí přidat vaše ID klienta, klíče a klienta domény.</span><span class="sxs-lookup"><span data-stu-id="b1d58-188">Just add your client ID, key, and tenant domain.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="b1d58-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1d58-189">Next steps</span></span>
<span data-ttu-id="b1d58-190">Blahopřejeme, jste právě provedli vaše první volání Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b1d58-190">Congratulations, you just made your first call to Microsoft Graph!</span></span>  
<span data-ttu-id="b1d58-191">Nyní můžete dotaz identity rizikových událostech a použije data, ale svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="b1d58-191">Now you can query identity risk events and use the data however you see fit.</span></span>

<span data-ttu-id="b1d58-192">Další informace o Microsoft Graph a jak vytvářet aplikace, které používají rozhraní Graph API, podívejte se [dokumentace](https://graph.microsoft.io/docs) zdaleka na [Microsoft Graph lokality](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="b1d58-192">To learn more about Microsoft Graph and how to build applications using the Graph API, check out the [documentation](https://graph.microsoft.io/docs) and much more on the [Microsoft Graph site](https://graph.microsoft.io/).</span></span> <span data-ttu-id="b1d58-193">Také si nezapomeňte bookmark [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) stránky, která obsahuje seznam všech dostupných v grafu Identity ochrany pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b1d58-193">Also, make sure to bookmark the [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) page that lists all of the Identity Protection APIs available in Graph.</span></span> <span data-ttu-id="b1d58-194">Jak jsme přidat nové způsoby, jak pracovat s ochranou Identity prostřednictvím rozhraní API, uvidíte je na této stránce.</span><span class="sxs-lookup"><span data-stu-id="b1d58-194">As we add new ways to work with Identity Protection via API, you’ll see them on that page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1d58-195">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b1d58-195">Additional resources</span></span>
* [<span data-ttu-id="b1d58-196">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b1d58-196">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
* [<span data-ttu-id="b1d58-197">Typy rizikových událostí detekovaných službou Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="b1d58-197">Types of risk events detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-risk-events-types.md)
* [<span data-ttu-id="b1d58-198">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="b1d58-198">Microsoft Graph</span></span>](https://graph.microsoft.io/)
* [<span data-ttu-id="b1d58-199">Přehled Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="b1d58-199">Overview of Microsoft Graph</span></span>](https://graph.microsoft.io/docs)
* [<span data-ttu-id="b1d58-200">Kořenovém adresáři služby Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="b1d58-200">Azure AD Identity Protection Service Root</span></span>](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

