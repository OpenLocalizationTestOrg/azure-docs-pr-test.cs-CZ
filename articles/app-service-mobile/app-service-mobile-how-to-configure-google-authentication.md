---
title: "ověřování Google tooconfigure aaaHow pro vaši aplikaci aplikační služby"
description: "Zjistěte, jak ověřování Google tooconfigure pro vaši aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 9175c40b78c859e9e191504c41cd0bb9a3380ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-google-login"></a><span data-ttu-id="3640e-103">Jak tooconfigure přihlašování Google toouse aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="3640e-103">How tooconfigure your App Service application toouse Google login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="3640e-104">Toto téma ukazuje, jak Azure App Service toouse tooconfigure jako zprostředkovatel ověřování Google.</span><span class="sxs-lookup"><span data-stu-id="3640e-104">This topic shows you how tooconfigure Azure App Service toouse Google as an authentication provider.</span></span>

<span data-ttu-id="3640e-105">toocomplete hello postup v tomto tématu, musíte mít účet Google s ověřenou e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="3640e-105">toocomplete hello procedure in this topic, you must have a Google account that has a verified email address.</span></span> <span data-ttu-id="3640e-106">toocreate nový účet Google, přejděte příliš[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="3640e-106">toocreate a new Google account, go too[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>

## <span data-ttu-id="3640e-107"><a name="register"></a>Registrace vaší aplikace službou Google</span><span class="sxs-lookup"><span data-stu-id="3640e-107"><a name="register"> </a>Register your application with Google</span></span>
1. <span data-ttu-id="3640e-108">Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="3640e-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="3640e-109">Kopie vašeho **URL**, které použijete novější tooconfigure aplikace Google.</span><span class="sxs-lookup"><span data-stu-id="3640e-109">Copy your **URL**, which you use later tooconfigure your Google app.</span></span>
2. <span data-ttu-id="3640e-110">Přejděte toohello [rozhraní Google API](http://go.microsoft.com/fwlink/p/?LinkId=268303) klikněte na web, přihlaste se pomocí svých přihlašovacích údajů účtu Google **vytvoření projektu**, poskytovat **název projektu**, pak klikněte na tlačítko  **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="3640e-110">Navigate toohello [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, sign in with your Google account credentials, click **Create Project**, provide a **Project name**, then click **Create**.</span></span>
3. <span data-ttu-id="3640e-111">V části **sociálních rozhraní API** klikněte na tlačítko **rozhraní API Google +** a potom **povolit**.</span><span class="sxs-lookup"><span data-stu-id="3640e-111">Under **Social APIs** click **Google+ API** and then **Enable**.</span></span>
4. <span data-ttu-id="3640e-112">V levé navigaci, hello **pověření** > **obrazovky souhlas OAuth**, pak vyberte vaše **e-mailová adresa**, zadejte **název produktu**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3640e-112">In hello left navigation, **Credentials** > **OAuth consent screen**, then select your **Email address**,  enter a **Product Name**, and click **Save**.</span></span>
5. <span data-ttu-id="3640e-113">V hello **pověření** , klikněte na **vytvořit přihlašovací údaje** > **ID klienta OAuth**, pak vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3640e-113">In hello **Credentials** tab, click **Create credentials** > **OAuth client ID**, then select **Web application**.</span></span>
6. <span data-ttu-id="3640e-114">Vložení hello služby App Service **URL** jste zkopírovali dříve do **oprávnění zdroje JavaScript**, vložte vaší přesměrování URI do **oprávnění identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="3640e-114">Paste hello App Service **URL** you copied earlier into **Authorized JavaScript Origins**, then paste your redirect URI into **Authorized Redirect URI**.</span></span> <span data-ttu-id="3640e-115">Hello hello adresu URL aplikace připojí s cestou hello je identifikátor URI přesměrování */.auth/login/google/callback*.</span><span class="sxs-lookup"><span data-stu-id="3640e-115">hello redirect URI is hello URL of your application appended with hello path, */.auth/login/google/callback*.</span></span> <span data-ttu-id="3640e-116">Například, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span><span class="sxs-lookup"><span data-stu-id="3640e-116">For example, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span></span> <span data-ttu-id="3640e-117">Ujistěte se, že používáte hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3640e-117">Make sure that you are using hello HTTPS scheme.</span></span> <span data-ttu-id="3640e-118">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3640e-118">Then click **Create**.</span></span>
7. <span data-ttu-id="3640e-119">Na další obrazovce hello si poznamenejte hodnoty hello klienta hello ID a klienta tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="3640e-119">On hello next screen, make a note of hello values of hello client ID and client secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3640e-120">tajný klíč klienta Hello je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="3640e-120">hello client secret is an important security credential.</span></span> <span data-ttu-id="3640e-121">S kýmkoli sdílet tento tajný klíč nebo distribuovat v rámci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="3640e-121">Do not share this secret with anyone or distribute it within a client application.</span></span>


## <span data-ttu-id="3640e-122"><a name="secrets"></a>Přidat Google informace tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="3640e-122"><a name="secrets"> </a>Add Google information tooyour application</span></span>
1. <span data-ttu-id="3640e-123">Zpět v hello [portál Azure], přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="3640e-123">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="3640e-124">Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="3640e-124">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="3640e-125">Pokud hello ověřování / autorizace funkce není povolena, zapněte přepínač hello příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="3640e-125">If hello Authentication / Authorization feature is not enabled, turn hello switch too**On**.</span></span>
3. <span data-ttu-id="3640e-126">Klikněte na tlačítko **Google**.</span><span class="sxs-lookup"><span data-stu-id="3640e-126">Click **Google**.</span></span> <span data-ttu-id="3640e-127">Vložte hello ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve a volitelně povolte všechny obory, které vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="3640e-127">Paste in hello App ID and App Secret values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="3640e-128">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3640e-128">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="3640e-129">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3640e-129">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="3640e-130">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="3640e-130">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="3640e-131">(Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Google, nastavte **tootake akce, když požadavek nebude ověřený** příliš**Google**.</span><span class="sxs-lookup"><span data-stu-id="3640e-131">(Optional) toorestrict access tooyour site tooonly users authenticated by Google, set **Action tootake when request is not authenticated** too**Google**.</span></span> <span data-ttu-id="3640e-132">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooGoogle pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="3640e-132">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooGoogle for authentication.</span></span>
5. <span data-ttu-id="3640e-133">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3640e-133">Click **Save**.</span></span>

<span data-ttu-id="3640e-134">Jste nyní připraven toouse Google pro ověřování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3640e-134">You are now ready toouse Google for authentication in your app.</span></span>

## <span data-ttu-id="3640e-135"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="3640e-135"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[portál Azure]: https://portal.azure.com/

