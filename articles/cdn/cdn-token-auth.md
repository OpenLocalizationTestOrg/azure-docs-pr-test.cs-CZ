---
title: "Zabezpečení prostředků Azure CDN pomocí tokenu ověřování | Microsoft Docs"
description: "Zabezpečený přístup k prostředkům vaší Azure CDN pomocí ověření tokenu."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: 42b182c314795b1ebf69639ec7ac5583208dc7c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a><span data-ttu-id="521b2-103">Zabezpečení prostředků Azure CDN se ověření pomocí tokenu</span><span class="sxs-lookup"><span data-stu-id="521b2-103">Securing Azure CDN assets with token authentication</span></span>

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a><span data-ttu-id="521b2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="521b2-104">Overview</span></span>

<span data-ttu-id="521b2-105">Ověření pomocí tokenu je mechanismus, který umožňuje zabránit obsluhující prostředky klientům neoprávněným Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="521b2-105">Token authentication is a mechanism that allows you to prevent Azure CDN from serving assets to unauthorized clients.</span></span>  <span data-ttu-id="521b2-106">To se obvykle provádí aby "hotlinking" obsahu, kde používá jiný web, často zpráva Tabule, vaše prostředky bez povolení.</span><span class="sxs-lookup"><span data-stu-id="521b2-106">This is typically done to prevent "hotlinking" of content, where a different website, often a message board, uses your assets without permission.</span></span>  <span data-ttu-id="521b2-107">To může mít vliv na vaše náklady na doručování obsahu.</span><span class="sxs-lookup"><span data-stu-id="521b2-107">This can have an impact on your content delivery costs.</span></span> <span data-ttu-id="521b2-108">Povolením této funkce na CDN se ověřit požadavky edge CDN POP před doručením obsah.</span><span class="sxs-lookup"><span data-stu-id="521b2-108">By enabling this feature on CDN, requests will be authenticated by CDN edge POPs before delivering the content.</span></span> 

## <a name="how-it-works"></a><span data-ttu-id="521b2-109">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="521b2-109">How it works</span></span>

<span data-ttu-id="521b2-110">Ověření pomocí tokenu ověřuje, že požadavky generované jako důvěryhodný web tím, že požadavky na obsahovat hodnotu tokenu obsahující kódovaného informace o žadatel.</span><span class="sxs-lookup"><span data-stu-id="521b2-110">Token authentication verifies requests are generated by a trusted site by requiring requests to contain a token value containing encoded information about the requester.</span></span> <span data-ttu-id="521b2-111">Kódovaného informace splňovat požadavky se obsah jenom zpracuje žadatel, v opačném případě požadavky budou odepřeny.</span><span class="sxs-lookup"><span data-stu-id="521b2-111">Content will only be served to requester when the encoded information meet the requirements, otherwise requests will be denied.</span></span> <span data-ttu-id="521b2-112">Můžete nastavit požadavek pomocí jednoho nebo více parametrů níže.</span><span class="sxs-lookup"><span data-stu-id="521b2-112">You can set up the requirement using one or multiple parameters below.</span></span>

- <span data-ttu-id="521b2-113">Země: Povolit nebo odmítnout požadavky, které pocházely ze zadaného zemích.</span><span class="sxs-lookup"><span data-stu-id="521b2-113">Country: allow or deny requests that originated from specified countries.</span></span>  [<span data-ttu-id="521b2-114">Seznam platných kódů.</span><span class="sxs-lookup"><span data-stu-id="521b2-114">List of valid country codes.</span></span>](https://msdn.microsoft.com/library/mt761717.aspx) 
- <span data-ttu-id="521b2-115">Adresa URL: Povolte jenom daný prostředek nebo cesta k požadavku.</span><span class="sxs-lookup"><span data-stu-id="521b2-115">URL: only allow specified asset or path to request.</span></span>  
- <span data-ttu-id="521b2-116">Hostitele: Povolit nebo odepřít požadavky pomocí zadaní hostitelé v hlavičce žádosti.</span><span class="sxs-lookup"><span data-stu-id="521b2-116">Host: allow or deny requests using specified hosts in the request header.</span></span>
- <span data-ttu-id="521b2-117">Odkazující server: Povolí nebo zakáže zadaný odkazující server můžete požádat.</span><span class="sxs-lookup"><span data-stu-id="521b2-117">Referrer: allow or deny specified referrer to request.</span></span>
- <span data-ttu-id="521b2-118">IP adresa: pouze povolit požadavky, které pochází z konkrétní IP adresu nebo podsíť protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="521b2-118">IP address: only allow requests that originated from specific IP address or IP subnet.</span></span>
- <span data-ttu-id="521b2-119">Protokol: Povolit nebo blokovat požadavky založené na protokolu slouží k vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="521b2-119">Protocol: allow or block requests based on the protocol used to request the content.</span></span>
- <span data-ttu-id="521b2-120">Čas vypršení platnosti: přiřadit datum a čas období zajistit, že odkaz pouze zůstává platná po omezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="521b2-120">Expiration time: assign a date and time period to ensure that a link only remains valid for a limited time period.</span></span>

<span data-ttu-id="521b2-121">Podívejte se na příklad podrobnou konfiguraci jednotlivých parametrů.</span><span class="sxs-lookup"><span data-stu-id="521b2-121">See detailed configuration example of each parameter.</span></span>

## <a name="reference-architecture"></a><span data-ttu-id="521b2-122">Referenční architektura</span><span class="sxs-lookup"><span data-stu-id="521b2-122">Reference architecture</span></span>

<span data-ttu-id="521b2-123">Najdete níže referenční architektura nastavení ověření pomocí tokenu na CDN pro práci s vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="521b2-123">See below a reference architecture of setting up token authentication on CDN to work with your Web App.</span></span>

![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a><span data-ttu-id="521b2-125">Ověření tokenu logiky na koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="521b2-125">Token validation logic on CDN endpoint</span></span>
    
<span data-ttu-id="521b2-126">Tento graf popisuje, jak Azure CDN ověří požadavek klienta po ověření tokenu je nakonfigurovaná na koncový bod CDN.</span><span class="sxs-lookup"><span data-stu-id="521b2-126">This chart describes how Azure CDN validates client request when token authentication is configured on CDN endpoint.</span></span>

![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a><span data-ttu-id="521b2-128">Nastavení ověření pomocí tokenu</span><span class="sxs-lookup"><span data-stu-id="521b2-128">Setting up token authentication</span></span>

1. <span data-ttu-id="521b2-129">Z [portál Azure](https://portal.azure.com), přejděte na svůj profil CDN a klikněte **spravovat** tlačítko spustit na doplňkovém portálu.</span><span class="sxs-lookup"><span data-stu-id="521b2-129">From the [Azure portal](https://portal.azure.com), browse to your CDN profile, and then click the **Manage** button to launch the supplemental portal.</span></span>

    ![Tlačítko Spravovat okno profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)

2. <span data-ttu-id="521b2-131">Pozastavte ukazatel myši nad **HTTP velké**a potom klikněte na **tokenu ověřování** v plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="521b2-131">Hover over **HTTP Large**, and then click **Token Auth** in the flyout.</span></span> <span data-ttu-id="521b2-132">Nastavíte šifrovací klíč a parametry šifrování na této kartě.</span><span class="sxs-lookup"><span data-stu-id="521b2-132">You will set up encryption key and encryption parameters in this tab.</span></span>

    1. <span data-ttu-id="521b2-133">Zadejte jedinečný šifrovací klíč pro **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="521b2-133">Enter a unique encryption key for **Primary Key**.</span></span>  <span data-ttu-id="521b2-134">Zadejte jinou pro **zálohování klíče**</span><span class="sxs-lookup"><span data-stu-id="521b2-134">Enter another for **Backup Key**</span></span>

        ![CDN tokenu ověřování Instalační klíč](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. <span data-ttu-id="521b2-136">Nastavit parametry šifrování nástrojem šifrování (povolit nebo odepřít požadavky na základě čas vypršení platnosti, země, odkazující server, protokolu, IP adresa klienta.</span><span class="sxs-lookup"><span data-stu-id="521b2-136">Set up encryption parameters with encryption tool (allow or deny requests based on expiration time, country, referrer, protocol, client IP.</span></span> <span data-ttu-id="521b2-137">Můžete použít libovolnou kombinaci.)</span><span class="sxs-lookup"><span data-stu-id="521b2-137">You can use any combination.)</span></span>

        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - <span data-ttu-id="521b2-139">ES-vyprší: přiřadí čas vypršení platnosti tokenu po zadaném časovém období.</span><span class="sxs-lookup"><span data-stu-id="521b2-139">ec-expire: assigns an expiration time of a token after a specified time period.</span></span> <span data-ttu-id="521b2-140">Požadavky na odeslání po čas vypršení platnosti budou odepřeny.</span><span class="sxs-lookup"><span data-stu-id="521b2-140">Requests submitted after the expiration time will be denied.</span></span> <span data-ttu-id="521b2-141">Tento parametr používá časové razítko systému Unix (podle počet sekund od standardní epoch 1/1/1970 00:00:00 GMT.</span><span class="sxs-lookup"><span data-stu-id="521b2-141">This parameter uses Unix timestamp (based on seconds since standard epoch of 1/1/1970 00:00:00 GMT.</span></span> <span data-ttu-id="521b2-142">Online nástroje můžete použít k poskytování převod mezi (běžný čas) a časem v systému Unix.)  Například, pokud chcete nastavit tokenu vyprší na 12/31. prosinci 2016 12:00:00 GMT, použijte Unix čas: 1483185600 jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="521b2-142">You can use online tools to provide conversion between standard time and Unix time.)  For example, If you want to set up the token to be expired at 12/31/2016 12:00:00 GMT, use the Unix time:1483185600 as below:</span></span>
    
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - <span data-ttu-id="521b2-144">Povolit adresu url ES: umožňuje přizpůsobit tokenů pro konkrétní prostředek nebo cestu.</span><span class="sxs-lookup"><span data-stu-id="521b2-144">ec-url-allow: allows you to tailor tokens to a particular asset or path.</span></span> <span data-ttu-id="521b2-145">Omezuje přístup na požadavky, jejichž adresa URL začínat konkrétní relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="521b2-145">It restricts access to requests whose URL start with a specific relative path.</span></span> <span data-ttu-id="521b2-146">Můžete zadat více cest jednotlivé cesty oddělte čárkou.</span><span class="sxs-lookup"><span data-stu-id="521b2-146">You can input multiple paths separating each path with a comma.</span></span> <span data-ttu-id="521b2-147">URL – adresy rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="521b2-147">URLs are case-sensitive.</span></span> <span data-ttu-id="521b2-148">V závislosti na požadavek můžete nastavit jinou hodnotu poskytují různé úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="521b2-148">Depending on the requirement, you can set up different value to provide different level of access.</span></span> <span data-ttu-id="521b2-149">Níže je několik scénářů:</span><span class="sxs-lookup"><span data-stu-id="521b2-149">Below are a couple of scenarios:</span></span>
        
            <span data-ttu-id="521b2-150">Pokud máte adresu URL: http://www.mydomain.com/pictures/city/strasbourg.png.</span><span class="sxs-lookup"><span data-stu-id="521b2-150">If you have a URL: http://www.mydomain.com/pictures/city/strasbourg.png.</span></span> <span data-ttu-id="521b2-151">Vstupní hodnota najdete v části "" a jeho přístup na úrovni odpovídajícím způsobem</span><span class="sxs-lookup"><span data-stu-id="521b2-151">See input value "" and its access level accordingly</span></span>

            1. <span data-ttu-id="521b2-152">Vstupní hodnota "/": všechny požadavky budou povoleny.</span><span class="sxs-lookup"><span data-stu-id="521b2-152">Input value "/": all requests will be allowed</span></span>
            2. <span data-ttu-id="521b2-153">Vstupní hodnota "/ obrázků": všechny tyto požadavky budou povolit</span><span class="sxs-lookup"><span data-stu-id="521b2-153">Input value "/pictures": all the following requests will be allow</span></span>
            
                - <span data-ttu-id="521b2-154">http://www.mydomain.com/Pictures.PNG</span><span class="sxs-lookup"><span data-stu-id="521b2-154">http://www.mydomain.com/pictures.png</span></span>
                - <span data-ttu-id="521b2-155">http://www.mydomain.com/Pictures/City/strasbourg.PNG</span><span class="sxs-lookup"><span data-stu-id="521b2-155">http://www.mydomain.com/pictures/city/strasbourg.png</span></span>
                - <span data-ttu-id="521b2-156">http://www.mydomain.com/picturesnew/City/strasbourgh.PNG</span><span class="sxs-lookup"><span data-stu-id="521b2-156">http://www.mydomain.com/picturesnew/city/strasbourgh.png</span></span>
            3. <span data-ttu-id="521b2-157">Vstupní hodnota "/ obrázky /": pouze požadavky pro /pictures/ bude možné.</span><span class="sxs-lookup"><span data-stu-id="521b2-157">Input value "/pictures/": only requests for /pictures/ will be allowed</span></span>
            4. <span data-ttu-id="521b2-158">Vstupní hodnota "/ pictures/city/strasbourg.png": pouze umožňuje požadavku pro tento prostředek</span><span class="sxs-lookup"><span data-stu-id="521b2-158">Input value "/pictures/city/strasbourg.png": only allows request for this asset</span></span>
    
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - <span data-ttu-id="521b2-160">Povolit země ES: umožňuje pouze požadavky, které pocházejí z jedné nebo více zadaný zemí.</span><span class="sxs-lookup"><span data-stu-id="521b2-160">ec-country-allow: only allows requests that originate from one or more specified countries.</span></span> <span data-ttu-id="521b2-161">Požadavky, které pocházejí z jiných zemí budou odepřeny.</span><span class="sxs-lookup"><span data-stu-id="521b2-161">Requests that originate from all other countries will be denied.</span></span> <span data-ttu-id="521b2-162">Nastavit parametry pomocí kód země a každý kód země oddělíte čárkou.</span><span class="sxs-lookup"><span data-stu-id="521b2-162">Use country code to set up the parameters and separating each country code with a comma.</span></span> <span data-ttu-id="521b2-163">Například pokud chcete povolit přístup ze Spojených státech amerických a Francie, zadejte nám FR ve sloupci jako níže.</span><span class="sxs-lookup"><span data-stu-id="521b2-163">For example, If you want to allow access from United States and France, input US, FR in the column as below.</span></span>  
        
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - <span data-ttu-id="521b2-165">Odepřít země ES: odmítne požadavky, které pochází z jedné nebo více zadaný zemí.</span><span class="sxs-lookup"><span data-stu-id="521b2-165">ec-country-deny: denies requests that originated from one or more specified countries.</span></span> <span data-ttu-id="521b2-166">Požadavky, které pocházejí z jiných zemí bude možné.</span><span class="sxs-lookup"><span data-stu-id="521b2-166">Requests that originate from all other countries will be allowed.</span></span> <span data-ttu-id="521b2-167">Nastavit parametry pomocí kód země a každý kód země oddělíte čárkou.</span><span class="sxs-lookup"><span data-stu-id="521b2-167">Use country code to set up the parameters and separating each country code with a comma.</span></span> <span data-ttu-id="521b2-168">Například pokud chcete odepřít přístup ze Spojených státech amerických a Francie, zadejte nám FR ve sloupci.</span><span class="sxs-lookup"><span data-stu-id="521b2-168">For example, If you want to deny access from United States and France, input US, FR in the column.</span></span>
    
        - <span data-ttu-id="521b2-169">Povolit ref ES: umožňuje pouze požadavky z odkazující zadaný server.</span><span class="sxs-lookup"><span data-stu-id="521b2-169">ec-ref-allow: only allows requests from specified referrer.</span></span> <span data-ttu-id="521b2-170">Odkazující server identifikuje adresu URL webové stránky, která propojené s tímto zdrojem požadováno.</span><span class="sxs-lookup"><span data-stu-id="521b2-170">A referrer identifies the URL of the web page that linked to the resource being requested.</span></span> <span data-ttu-id="521b2-171">Hodnota parametru odkazující server nesmí obsahovat protokol.</span><span class="sxs-lookup"><span data-stu-id="521b2-171">The referrer parameter value shouldn't include the protocol.</span></span> <span data-ttu-id="521b2-172">Můžete zadat název hostitele nebo na konkrétní cestu na tento název hostitele.</span><span class="sxs-lookup"><span data-stu-id="521b2-172">You can input a hostname and/or a particular path on that hostname.</span></span> <span data-ttu-id="521b2-173">Můžete také přidat více odkazujících serverů v rámci jeden parametr oddělení každé z nich oddělujte čárkami.</span><span class="sxs-lookup"><span data-stu-id="521b2-173">You can also add multiple referrers within a single parameter separating each one with a comma.</span></span> <span data-ttu-id="521b2-174">Pokud jste zadali hodnotu odkazující server, ale odkazující server informace nebudou odeslány v požadavek, protože některé konfigurace prohlížeče, bude ve výchozím nastavení zamítnutý tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="521b2-174">If you have specified a referrer value, but the referrer information is not sent in the request due to some browser configuration, these requests will be denied by default.</span></span> <span data-ttu-id="521b2-175">Můžete přiřadit "Chybí" nebo prázdná hodnota v parametru, aby tyto požadavky s chybějícími informacemi o odkazující server.</span><span class="sxs-lookup"><span data-stu-id="521b2-175">You can assign "Missing" or a blank value in the parameter to allow these requests with missing referrer information.</span></span> <span data-ttu-id="521b2-176">Můžete také použít "*. consoto.com" Povolit všechny subdomény consoto.com.</span><span class="sxs-lookup"><span data-stu-id="521b2-176">You can also use "*.consoto.com" to allow all subdomains of consoto.com.</span></span>  <span data-ttu-id="521b2-177">Například pokud chcete povolit přístup pro žádosti od www.consoto.com, všem dílčím doménám consoto2.com a erquests s prázdné nebo chybějící reffers, zadejte hodnotu níže:</span><span class="sxs-lookup"><span data-stu-id="521b2-177">For example, if you want to allow access for requests from www.consoto.com, all subdomains under consoto2.com and erquests with blank or missing reffers, input value below:</span></span>
        
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - <span data-ttu-id="521b2-179">Odepřít ref ES: odmítne požadavky z odkazující zadaný server.</span><span class="sxs-lookup"><span data-stu-id="521b2-179">ec-ref-deny: denies requests from specified referrer.</span></span> <span data-ttu-id="521b2-180">Viz podrobnosti a příklad v parametru "ES ref povolit".</span><span class="sxs-lookup"><span data-stu-id="521b2-180">Refer to details and example in "ec-ref-allow" parameter.</span></span>
         
        - <span data-ttu-id="521b2-181">umožňují proto – ES: pouze povoluje požadavky z zadaný protokol.</span><span class="sxs-lookup"><span data-stu-id="521b2-181">ec-proto-allow: only allows requests from specified protocol.</span></span> <span data-ttu-id="521b2-182">Například http nebo https.</span><span class="sxs-lookup"><span data-stu-id="521b2-182">For example, http or https.</span></span>
        
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - <span data-ttu-id="521b2-184">ES proto – odepřít: odmítne požadavky z zadaný protokol.</span><span class="sxs-lookup"><span data-stu-id="521b2-184">ec-proto-deny: denies requests from specified protocol.</span></span> <span data-ttu-id="521b2-185">Například http nebo https.</span><span class="sxs-lookup"><span data-stu-id="521b2-185">For example, http or https.</span></span>
    
        - <span data-ttu-id="521b2-186">Když ES: omezuje přístup k zadané žadatele IP adresu.</span><span class="sxs-lookup"><span data-stu-id="521b2-186">ec-clientip: restricts access to specified requester's IP address.</span></span> <span data-ttu-id="521b2-187">Jsou podporovány adresy IPV4 a IPV6.</span><span class="sxs-lookup"><span data-stu-id="521b2-187">Both IPV4 and IPV6 are supported.</span></span> <span data-ttu-id="521b2-188">Můžete zadat jednu žádost IP adresu nebo podsíť protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="521b2-188">You can specify single request IP address or IP subnet.</span></span>
            
        ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. <span data-ttu-id="521b2-190">Můžete otestovat váš token pomocí nástroje dešifrování.</span><span class="sxs-lookup"><span data-stu-id="521b2-190">You can test your token with the decription tool.</span></span>

    4. <span data-ttu-id="521b2-191">Můžete také upravit typ odpovědi, který bude vrácen pro uživatele, když požadavek je odepřen.</span><span class="sxs-lookup"><span data-stu-id="521b2-191">You can also customize the type of response that will be returned to user when request is denied.</span></span> <span data-ttu-id="521b2-192">Ve výchozím nastavení používáme 403.</span><span class="sxs-lookup"><span data-stu-id="521b2-192">By default we use 403.</span></span>

3. <span data-ttu-id="521b2-193">Klikněte na tlačítko **stroj pravidel** v části **HTTP velké**.</span><span class="sxs-lookup"><span data-stu-id="521b2-193">Now click **Rules Engine** tab under **HTTP Large**.</span></span> <span data-ttu-id="521b2-194">Použijete-li na této kartě můžete definovat cesty použít funkci, povolte funkci ověření pomocí tokenu a povolit další ověření tokenu související možnosti.</span><span class="sxs-lookup"><span data-stu-id="521b2-194">You will use this tab to define paths to apply the feature, enable the token authentication feature, and enable additional token authentication related capabilities.</span></span>

    - <span data-ttu-id="521b2-195">Použijte sloupec "Pokud" k definování asset nebo cestu, kterou chcete použít ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="521b2-195">Use "IF" column to define asset or path that you want to apply token authentication.</span></span> 
    - <span data-ttu-id="521b2-196">Kliknutím lze přidat "Tokenu ověřování" z rozevíracího seznamu funkce povolit ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="521b2-196">Click to add "Token Auth" from the feature dropdown to enable token authentication.</span></span>
        
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. <span data-ttu-id="521b2-198">V **stroj pravidel** se nachází několik dalších možností můžete povolit.</span><span class="sxs-lookup"><span data-stu-id="521b2-198">In the **Rules Engine** tab, there are a few additional capabilities you can enable.</span></span>
    
    - <span data-ttu-id="521b2-199">Token denial kód ověřování: Určuje typ odpovědi, který bude vrácen pro uživatele, když požadavek je odepřen.</span><span class="sxs-lookup"><span data-stu-id="521b2-199">Token auth denial code: determines the type of response that will be returned to user when a request is denied.</span></span> <span data-ttu-id="521b2-200">Pravidla nastavení tady potlačí nastavení kódu útok na kartě tokenu ověřování.</span><span class="sxs-lookup"><span data-stu-id="521b2-200">Rules set up here will override the denial code setting in the token auth tab.</span></span>
    - <span data-ttu-id="521b2-201">Ignorovat tokenu ověřování: Určuje, zda bude mít adresu URL, které slouží k ověření tokenu velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="521b2-201">Token auth ignore: determines whether URL used to validate token will be case sensitive.</span></span>
    - <span data-ttu-id="521b2-202">Parametr tokenu ověřování: Přejmenovat tokenu ověřování parametru řetězce dotazu zobrazující v požadovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="521b2-202">Token auth parameter: rename the token auth query string parameter showing in the requested URL.</span></span> 
        
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-token-auth/cdn-rules-engine2.png)

5. <span data-ttu-id="521b2-204">Váš token zabezpečení, které je aplikace, která generuje token pro funkce na základě tokenu ověřování můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="521b2-204">You can customize your token which is an application that generates token for Token-based authentication features.</span></span> <span data-ttu-id="521b2-205">Zdrojový kód je přístupná zde v [Githubu](https://github.com/VerizonDigital/ectoken).</span><span class="sxs-lookup"><span data-stu-id="521b2-205">Source code can be accessed here in [GitHub](https://github.com/VerizonDigital/ectoken).</span></span>
<span data-ttu-id="521b2-206">Dostupné jazyky patří:</span><span class="sxs-lookup"><span data-stu-id="521b2-206">Available languages include:</span></span>
    
    - <span data-ttu-id="521b2-207">C</span><span class="sxs-lookup"><span data-stu-id="521b2-207">C</span></span>
    - <span data-ttu-id="521b2-208">C#</span><span class="sxs-lookup"><span data-stu-id="521b2-208">C#</span></span>
    - <span data-ttu-id="521b2-209">PHP</span><span class="sxs-lookup"><span data-stu-id="521b2-209">PHP</span></span>
    - <span data-ttu-id="521b2-210">Perl</span><span class="sxs-lookup"><span data-stu-id="521b2-210">Perl</span></span>
    - <span data-ttu-id="521b2-211">Java</span><span class="sxs-lookup"><span data-stu-id="521b2-211">Java</span></span>
    - <span data-ttu-id="521b2-212">Python</span><span class="sxs-lookup"><span data-stu-id="521b2-212">Python</span></span>    


## <a name="azure-cdn-features-and-provider-pricing"></a><span data-ttu-id="521b2-213">Azure CDN funkce a zprostředkovatele ceny</span><span class="sxs-lookup"><span data-stu-id="521b2-213">Azure CDN features and provider pricing</span></span>

<span data-ttu-id="521b2-214">Najdete v článku [přehled CDN](cdn-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="521b2-214">See the [CDN Overview](cdn-overview.md) topic.</span></span>