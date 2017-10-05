---
title: "Postup konfigurace ověřování Facebook pro aplikaci aplikační služby"
description: "Informace o konfiguraci ověřování Facebook pro aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: c1b4c91d384c56c4f55bf8d31ced250f51c0d837
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a><span data-ttu-id="05746-103">Konfigurace aplikace App Service pro použití přihlášení k Facebooku</span><span class="sxs-lookup"><span data-stu-id="05746-103">How to configure your App Service application to use Facebook login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="05746-104">Toto téma ukazuje, jak nakonfigurovat služby Azure App Service použít k ověřování sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="05746-104">This topic shows you how to configure Azure App Service to use Facebook as an authentication provider.</span></span>

<span data-ttu-id="05746-105">Dokončete postup v tomto tématu, musí mít účet Facebook, který má ověřenou e-mailovou adresu a číslo mobilního telefonu.</span><span class="sxs-lookup"><span data-stu-id="05746-105">To complete the procedure in this topic, you must have a Facebook account that has a verified email address and a mobile phone number.</span></span> <span data-ttu-id="05746-106">Chcete-li vytvořit nový účet Facebook, přejděte na [facebook.com].</span><span class="sxs-lookup"><span data-stu-id="05746-106">To create a new Facebook account, go to [facebook.com].</span></span>

## <span data-ttu-id="05746-107"><a name="register"></a>Registrace vaší aplikace se službou Facebook.</span><span class="sxs-lookup"><span data-stu-id="05746-107"><a name="register"> </a>Register your application with Facebook</span></span>
1. <span data-ttu-id="05746-108">Přihlaste se na [portál Azure]a přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05746-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="05746-109">Kopie vašeho **URL**.</span><span class="sxs-lookup"><span data-stu-id="05746-109">Copy your **URL**.</span></span> <span data-ttu-id="05746-110">To bude používat ke konfiguraci aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="05746-110">You will use this to configure your Facebook app.</span></span>
2. <span data-ttu-id="05746-111">V jiném okně prohlížeče, přejděte na [Facebook vývojáři] webu a přihlaste se pomocí vaší sítě Facebook přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="05746-111">In another browser window, navigate to the [Facebook Developers] website and sign-in with your Facebook account credentials.</span></span>
3. <span data-ttu-id="05746-112">(Volitelné) Pokud jste ještě nezaregistrovali, klikněte na tlačítko **aplikace** > **zaregistrujte se jako vývojář**, přijměte zásady a postupujte podle kroků registrace.</span><span class="sxs-lookup"><span data-stu-id="05746-112">(Optional) If you have not already registered, click **Apps** > **Register as a Developer**, then accept the policy and follow the registration steps.</span></span>
4. <span data-ttu-id="05746-113">Klikněte na tlačítko **Moje aplikace** > **přidejte novou aplikaci** > **webu** > **přeskočit a vytvořit ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="05746-113">Click **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.</span></span> 
5. <span data-ttu-id="05746-114">V **zobrazovaný název**, zadejte jedinečný název pro vaši aplikaci, typ vaše **e-mailu kontaktujte**, vyberte **kategorie** pro vaši aplikaci, pak klikněte na tlačítko **vytvoření ID aplikace** a dokončit kontrolu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05746-114">In **Display Name**, type a unique name for your app, type your **Contact Email**, choose a **Category** for your app, then click **Create App ID** and complete the security check.</span></span> <span data-ttu-id="05746-115">Tím přejdete na řídicí panel pro vývojáře pro novou aplikaci Facebook.</span><span class="sxs-lookup"><span data-stu-id="05746-115">This takes you to the developer dashboard for your new Facebook app.</span></span>
6. <span data-ttu-id="05746-116">V části "Facebook Login", klikněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="05746-116">Under "Facebook Login," click **Get Started**.</span></span> <span data-ttu-id="05746-117">Přidejte svoji aplikaci **identifikátor URI pro přesměrování** k **identifikátory URI přesměrování platný OAuth**, pak klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="05746-117">Add your application's **Redirect URI** to **Valid OAuth redirect URIs**, then click **Save Changes**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="05746-118">Vaše přesměrování identifikátor URI je adresa URL aplikace připojí s cestou, */.auth/login/facebook/callback*.</span><span class="sxs-lookup"><span data-stu-id="05746-118">Your redirect URI is the URL of your application appended with the path, */.auth/login/facebook/callback*.</span></span> <span data-ttu-id="05746-119">Například, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span><span class="sxs-lookup"><span data-stu-id="05746-119">For example, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span></span> <span data-ttu-id="05746-120">Ujistěte se, že používáte schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="05746-120">Make sure that you are using the HTTPS scheme.</span></span>
   > 
   > 
7. <span data-ttu-id="05746-121">V levém navigačním panelu, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="05746-121">In the left-hand navigation, click **Settings**.</span></span> <span data-ttu-id="05746-122">Na **tajný klíč aplikace** pole, klikněte na tlačítko **zobrazit**, zadejte heslo, pokud požadovaný, poznamenejte si hodnoty **ID aplikace** a **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="05746-122">On the **App Secret** field, click **Show**, provide your password if requested, then make a note of the values of **App ID** and **App Secret**.</span></span> <span data-ttu-id="05746-123">Můžete použít později ke konfiguraci vaší aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="05746-123">You use these later to configure your application in Azure.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="05746-124">Tajný klíč aplikace je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="05746-124">The app secret is an important security credential.</span></span> <span data-ttu-id="05746-125">S kýmkoli sdílet tento tajný klíč nebo distribuovat v rámci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="05746-125">Do not share this secret with anyone or distribute it within a client application.</span></span>
   > 
   > 
8. <span data-ttu-id="05746-126">Účet Facebook, který se používá k registraci aplikace je správce aplikace.</span><span class="sxs-lookup"><span data-stu-id="05746-126">The Facebook account which was used to register the application is an administrator of the app.</span></span> <span data-ttu-id="05746-127">V tomto okamžiku se jenom správci moct přihlásit do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="05746-127">At this point, only administrators can sign into this application.</span></span> <span data-ttu-id="05746-128">K ověření jiné účty Facebook, klikněte na tlačítko **revize aplikace** a povolte **zkontrolujte <-název_aplikace > veřejné** a povolte obecné veřejný přístup pomocí ověřování Facebook.</span><span class="sxs-lookup"><span data-stu-id="05746-128">To authenticate other Facebook accounts, click **App Review** and enable **Make <your-app-name> public** to enable general public access using Facebook authentication.</span></span>

## <span data-ttu-id="05746-129"><a name="secrets"></a>Facebook přidat informace do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="05746-129"><a name="secrets"> </a>Add Facebook information to your application</span></span>
1. <span data-ttu-id="05746-130">Zpět v [portál Azure], přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05746-130">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="05746-131">Klikněte na tlačítko **nastavení** > **ověřování / autorizace**a ujistěte se, že **ověřování služby aplikace** je **na**.</span><span class="sxs-lookup"><span data-stu-id="05746-131">Click **Settings** > **Authentication / Authorization**, and make sure that **App Service Authentication** is **On**.</span></span>
2. <span data-ttu-id="05746-132">Klikněte na tlačítko **Facebook**, vložte hodnoty ID aplikace a tajný klíč aplikace, které jste získali dříve, můžete také povolit všechny obory, které vyžaduje vaše aplikace a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="05746-132">Click **Facebook**, paste in the App ID and App Secret values which you obtained previously, optionally enable any scopes needed by your application, then click **OK**.</span></span>
   
    ![][0]
   
    <span data-ttu-id="05746-133">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="05746-133">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="05746-134">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="05746-134">You must authorize users in your app code.</span></span>
3. <span data-ttu-id="05746-135">(Volitelné) Chcete-li omezit přístup k webu jenom na uživatele ověřeného službou Facebook, nastavte **akci provést, když požadavek nebude ověřený** k **Facebook**.</span><span class="sxs-lookup"><span data-stu-id="05746-135">(Optional) To restrict access to your site to only users authenticated by Facebook, set **Action to take when request is not authenticated** to **Facebook**.</span></span> <span data-ttu-id="05746-136">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do sítě Facebook pro ověření.</span><span class="sxs-lookup"><span data-stu-id="05746-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Facebook for authentication.</span></span>
4. <span data-ttu-id="05746-137">Po dokončení konfigurace ověřování, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="05746-137">When done configuring authentication, click **Save**.</span></span>

<span data-ttu-id="05746-138">Nyní jste připraveni používat sítě Facebook pro ověření ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05746-138">You are now ready to use Facebook for authentication in your app.</span></span>

## <span data-ttu-id="05746-139"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="05746-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
<span data-ttu-id="05746-140">[Facebook vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268286</span><span class="sxs-lookup"><span data-stu-id="05746-140">[Facebook Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268286</span></span>
<span data-ttu-id="05746-141">[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285</span><span class="sxs-lookup"><span data-stu-id="05746-141">[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285</span></span>
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
<span data-ttu-id="05746-142">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="05746-142">[Azure portal]: https://portal.azure.com/</span></span>
