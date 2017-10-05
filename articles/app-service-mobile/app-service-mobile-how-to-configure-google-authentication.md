---
title: "Postup konfigurace ověřování Google pro vaši aplikaci aplikační služby"
description: "Informace o konfiguraci ověřování Google pro vaši aplikaci aplikační služby."
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
ms.openlocfilehash: d6c1707f67d986487e5a45e76ffc9a02ddf16eb1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a><span data-ttu-id="7db5b-103">Postup konfigurace aplikace služby App Service pomocí Google přihlášení</span><span class="sxs-lookup"><span data-stu-id="7db5b-103">How to configure your App Service application to use Google login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="7db5b-104">Toto téma ukazuje, jak nakonfigurovat služby Azure App Service, pokud chcete použít jako zprostředkovatel ověřování Google.</span><span class="sxs-lookup"><span data-stu-id="7db5b-104">This topic shows you how to configure Azure App Service to use Google as an authentication provider.</span></span>

<span data-ttu-id="7db5b-105">Dokončete postup v tomto tématu, musí mít účet Google s ověřenou e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="7db5b-105">To complete the procedure in this topic, you must have a Google account that has a verified email address.</span></span> <span data-ttu-id="7db5b-106">Nový účet Google si můžete vytvořit na stránce [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="7db5b-106">To create a new Google account, go to [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>

## <span data-ttu-id="7db5b-107"><a name="register"></a>Registrace vaší aplikace službou Google</span><span class="sxs-lookup"><span data-stu-id="7db5b-107"><a name="register"> </a>Register your application with Google</span></span>
1. <span data-ttu-id="7db5b-108">Přihlaste se na [portál Azure]a přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7db5b-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="7db5b-109">Kopie vašeho **URL**, které můžete použít ke konfiguraci vaší aplikace Google později.</span><span class="sxs-lookup"><span data-stu-id="7db5b-109">Copy your **URL**, which you use later to configure your Google app.</span></span>
2. <span data-ttu-id="7db5b-110">Přejděte na [rozhraní Google API](http://go.microsoft.com/fwlink/p/?LinkId=268303) klikněte na web, přihlaste se pomocí svých přihlašovacích údajů účtu Google **vytvoření projektu**, poskytovat **název projektu**, pak klikněte na tlačítko  **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-110">Navigate to the [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, sign in with your Google account credentials, click **Create Project**, provide a **Project name**, then click **Create**.</span></span>
3. <span data-ttu-id="7db5b-111">V části **sociálních rozhraní API** klikněte na tlačítko **rozhraní API Google +** a potom **povolit**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-111">Under **Social APIs** click **Google+ API** and then **Enable**.</span></span>
4. <span data-ttu-id="7db5b-112">V levém navigačním **pověření** > **obrazovky souhlas OAuth**, pak vyberte vaše **e-mailová adresa**, zadejte **název produktu**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-112">In the left navigation, **Credentials** > **OAuth consent screen**, then select your **Email address**,  enter a **Product Name**, and click **Save**.</span></span>
5. <span data-ttu-id="7db5b-113">V **pověření** , klikněte na **vytvořit přihlašovací údaje** > **ID klienta OAuth**, pak vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-113">In the **Credentials** tab, click **Create credentials** > **OAuth client ID**, then select **Web application**.</span></span>
6. <span data-ttu-id="7db5b-114">Vložte App Service **URL** jste zkopírovali dříve do **oprávnění zdroje JavaScript**, vložte vaší přesměrování URI do **oprávnění identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-114">Paste the App Service **URL** you copied earlier into **Authorized JavaScript Origins**, then paste your redirect URI into **Authorized Redirect URI**.</span></span> <span data-ttu-id="7db5b-115">Přesměrování identifikátor URI je adresa URL aplikace připojí s cestou, */.auth/login/google/callback*.</span><span class="sxs-lookup"><span data-stu-id="7db5b-115">The redirect URI is the URL of your application appended with the path, */.auth/login/google/callback*.</span></span> <span data-ttu-id="7db5b-116">Například, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span><span class="sxs-lookup"><span data-stu-id="7db5b-116">For example, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span></span> <span data-ttu-id="7db5b-117">Ujistěte se, že používáte schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7db5b-117">Make sure that you are using the HTTPS scheme.</span></span> <span data-ttu-id="7db5b-118">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-118">Then click **Create**.</span></span>
7. <span data-ttu-id="7db5b-119">Poznamenejte si hodnoty ID klienta a tajný klíč klienta na další obrazovce.</span><span class="sxs-lookup"><span data-stu-id="7db5b-119">On the next screen, make a note of the values of the client ID and client secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7db5b-120">Tajný klíč klienta je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="7db5b-120">The client secret is an important security credential.</span></span> <span data-ttu-id="7db5b-121">S kýmkoli sdílet tento tajný klíč nebo distribuovat v rámci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="7db5b-121">Do not share this secret with anyone or distribute it within a client application.</span></span>


## <span data-ttu-id="7db5b-122"><a name="secrets"></a>Google přidat informace do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7db5b-122"><a name="secrets"> </a>Add Google information to your application</span></span>
1. <span data-ttu-id="7db5b-123">Zpět v [portál Azure], přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7db5b-123">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="7db5b-124">Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-124">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="7db5b-125">Pokud ověřování / autorizace funkce není povolena, zapněte přepínač k **na**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-125">If the Authentication / Authorization feature is not enabled, turn the switch to **On**.</span></span>
3. <span data-ttu-id="7db5b-126">Klikněte na tlačítko **Google**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-126">Click **Google**.</span></span> <span data-ttu-id="7db5b-127">Vložte hodnoty ID aplikace a tajný klíč aplikace, které jste získali dříve a volitelně povolte všechny obory, které vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="7db5b-127">Paste in the App ID and App Secret values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="7db5b-128">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-128">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="7db5b-129">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7db5b-129">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="7db5b-130">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="7db5b-130">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="7db5b-131">(Volitelné) Chcete-li omezit přístup k webu jenom na uživatele ověřeného službou Google, nastavte **akci provést, když požadavek nebude ověřený** k **Google**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-131">(Optional) To restrict access to your site to only users authenticated by Google, set **Action to take when request is not authenticated** to **Google**.</span></span> <span data-ttu-id="7db5b-132">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do Google pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="7db5b-132">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Google for authentication.</span></span>
5. <span data-ttu-id="7db5b-133">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7db5b-133">Click **Save**.</span></span>

<span data-ttu-id="7db5b-134">Nyní jste připraveni pro ověřování ve vaší aplikaci pomocí služby Google.</span><span class="sxs-lookup"><span data-stu-id="7db5b-134">You are now ready to use Google for authentication in your app.</span></span>

## <span data-ttu-id="7db5b-135"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="7db5b-135"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[portál Azure]: https://portal.azure.com/

