---
title: "aaaHow tooconfigure Account Microsoft ověřování pro aplikaci aplikační služby"
description: "Zjistěte, jak tooconfigure Account Microsoft ověřování pro aplikaci aplikační služby."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d86d8dab26a189f4454082fc18e44e3fb6e0a01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-microsoft-account-login"></a><span data-ttu-id="86a91-103">Jak tooconfigure přihlašování Account Microsoft toouse aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="86a91-103">How tooconfigure your App Service application toouse Microsoft Account login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="86a91-104">Toto téma ukazuje, jak Azure App Service toouse tooconfigure Account Microsoft jako zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="86a91-104">This topic shows you how tooconfigure Azure App Service toouse Microsoft Account as an authentication provider.</span></span> 

## <span data-ttu-id="86a91-105"><a name="register-microsoft-account"></a>Svou aplikaci zaregistrovat pomocí účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="86a91-105"><a name="register-microsoft-account"> </a>Register your app with Microsoft Account</span></span>
1. <span data-ttu-id="86a91-106">Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="86a91-106">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="86a91-107">Kopie vašeho **URL**, které později použijete tooconfigure vaší aplikace s Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="86a91-107">Copy your **URL**, which later you use tooconfigure your app with Microsoft Account.</span></span>
2. <span data-ttu-id="86a91-108">Přejděte toohello [Moje aplikace] stránku hello Microsoft Account Developer Center a přihlaste se pomocí účtu Microsoft, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="86a91-108">Navigate toohello [My Applications] page in hello Microsoft Account Developer Center, and log on with your Microsoft account, if required.</span></span>
3. <span data-ttu-id="86a91-109">Klikněte na tlačítko **přidat aplikaci**, pak zadejte název aplikace a klikněte na **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="86a91-109">Click **Add an app**, then type an application name, and click **Create application**.</span></span>
4. <span data-ttu-id="86a91-110">Poznamenejte si hello **ID aplikace**, protože jej budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="86a91-110">Make a note of hello **Application ID**, as you will need it later.</span></span> 
5. <span data-ttu-id="86a91-111">V části "Platformy", klikněte na **přidejte platformu** a vyberte možnost "Web".</span><span class="sxs-lookup"><span data-stu-id="86a91-111">Under "Platforms," click **Add Platform** and select "Web".</span></span>
6. <span data-ttu-id="86a91-112">V části "Identifikátory URI přesměrování" koncový bod zadejte hello pro vaši aplikaci, pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="86a91-112">Under "Redirect URIs" supply hello endpoint for your application, then click **Save**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="86a91-113">Vaše přesměrování identifikátor URI je adresa URL hello aplikace připojí s cestou hello */.auth/login/microsoftaccount/callback*.</span><span class="sxs-lookup"><span data-stu-id="86a91-113">Your redirect URI is hello URL of your application appended with hello path, */.auth/login/microsoftaccount/callback*.</span></span> <span data-ttu-id="86a91-114">Například, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span><span class="sxs-lookup"><span data-stu-id="86a91-114">For example, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span></span>   
   > <span data-ttu-id="86a91-115">Ujistěte se, že používáte hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="86a91-115">Make sure that you are using hello HTTPS scheme.</span></span>
   
7. <span data-ttu-id="86a91-116">V části "Aplikace tajné údaje", klikněte na **generovat nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="86a91-116">Under "Application Secrets," click **Generate New Password**.</span></span> <span data-ttu-id="86a91-117">Poznamenejte si hello hodnotu, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="86a91-117">Make note of hello value that appears.</span></span> <span data-ttu-id="86a91-118">Jakmile hello stránku opustíte, nebudou zobrazeny znovu.</span><span class="sxs-lookup"><span data-stu-id="86a91-118">Once you leave hello page, it will not be displayed again.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="86a91-119">heslo Hello je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="86a91-119">hello password is an important security credential.</span></span> <span data-ttu-id="86a91-120">S kýmkoli sdílet hello heslo nebo distribuovat v rámci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="86a91-120">Do not share hello password with anyone or distribute it within a client application.</span></span>

## <span data-ttu-id="86a91-121"><a name="secrets"></a>Tooyour informace přidat účet Microsoft aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="86a91-121"><a name="secrets"> </a>Add Microsoft Account information tooyour App Service application</span></span>
1. <span data-ttu-id="86a91-122">Zpět v hello [portál Azure]přejděte tooyour aplikace, klikněte na tlačítko **nastavení** > **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="86a91-122">Back in hello [Azure portal], navigate tooyour application, click **Settings** > **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="86a91-123">Pokud hello ověřování / autorizace funkce není povolena, je přepínač **na**.</span><span class="sxs-lookup"><span data-stu-id="86a91-123">If hello Authentication / Authorization feature is not enabled, switch it **On**.</span></span>
3. <span data-ttu-id="86a91-124">Klikněte na tlačítko **účtu Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="86a91-124">Click **Microsoft Account**.</span></span> <span data-ttu-id="86a91-125">Vložte hodnoty hello ID aplikace a heslo, které jste získali dříve a volitelně povolte všechny obory, které vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="86a91-125">Paste in hello Application ID and Password values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="86a91-126">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="86a91-126">Then click **OK**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="86a91-127">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="86a91-127">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="86a91-128">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="86a91-128">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="86a91-129">(Volitelné) toorestrict přístup tooyour lokality tooonly uživatelé ověřit účet Microsoft nastavit **tootake akce, když požadavek nebude ověřený** příliš**Account Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="86a91-129">(Optional) toorestrict access tooyour site tooonly users authenticated by Microsoft account, set **Action tootake when request is not authenticated** too**Microsoft Account**.</span></span> <span data-ttu-id="86a91-130">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují tooMicrosoft účet pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="86a91-130">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooMicrosoft account for authentication.</span></span>
5. <span data-ttu-id="86a91-131">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="86a91-131">Click **Save**.</span></span>

<span data-ttu-id="86a91-132">Jste nyní připraven toouse Account Microsoft pro ověřování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86a91-132">You are now ready toouse Microsoft Account for authentication in your app.</span></span>

## <span data-ttu-id="86a91-133"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="86a91-133"><a name="related-content"> </a>Related content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[portál Azure]: https://portal.azure.com/
