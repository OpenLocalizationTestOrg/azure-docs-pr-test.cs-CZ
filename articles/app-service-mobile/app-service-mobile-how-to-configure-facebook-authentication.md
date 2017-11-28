---
title: "aaaHow tooconfigure ověřování Facebook pro aplikaci aplikační služby"
description: "Zjistěte, jak tooconfigure ověřování Facebook pro aplikaci aplikační služby."
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
ms.openlocfilehash: 53d03445a2ad17de1d2f69f5e770d14385b48ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-facebook-login"></a><span data-ttu-id="bafbb-103">Jak tooconfigure přihlašování Facebook toouse aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="bafbb-103">How tooconfigure your App Service application toouse Facebook login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="bafbb-104">Toto téma ukazuje, jak Azure App Service toouse tooconfigure Facebook jako zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="bafbb-104">This topic shows you how tooconfigure Azure App Service toouse Facebook as an authentication provider.</span></span>

<span data-ttu-id="bafbb-105">toocomplete hello postup v tomto tématu, musíte mít účet Facebook, který má ověřenou e-mailovou adresu a číslo mobilního telefonu.</span><span class="sxs-lookup"><span data-stu-id="bafbb-105">toocomplete hello procedure in this topic, you must have a Facebook account that has a verified email address and a mobile phone number.</span></span> <span data-ttu-id="bafbb-106">toocreate nový účet Facebook, přejděte příliš[facebook.com].</span><span class="sxs-lookup"><span data-stu-id="bafbb-106">toocreate a new Facebook account, go too[facebook.com].</span></span>

## <span data-ttu-id="bafbb-107"><a name="register"></a>Registrace vaší aplikace se službou Facebook.</span><span class="sxs-lookup"><span data-stu-id="bafbb-107"><a name="register"> </a>Register your application with Facebook</span></span>
1. <span data-ttu-id="bafbb-108">Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="bafbb-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="bafbb-109">Kopie vašeho **URL**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-109">Copy your **URL**.</span></span> <span data-ttu-id="bafbb-110">Použijete tento tooconfigure aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="bafbb-110">You will use this tooconfigure your Facebook app.</span></span>
2. <span data-ttu-id="bafbb-111">V jiném okně prohlížeče přejděte toohello [Facebook vývojáři] webu a přihlaste se pomocí vaší sítě Facebook přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="bafbb-111">In another browser window, navigate toohello [Facebook Developers] website and sign-in with your Facebook account credentials.</span></span>
3. <span data-ttu-id="bafbb-112">(Volitelné) Pokud jste ještě nezaregistrovali, klikněte na tlačítko **aplikace** > **zaregistrujte se jako vývojář**, přijměte zásady hello a postupujte podle kroků registrace hello.</span><span class="sxs-lookup"><span data-stu-id="bafbb-112">(Optional) If you have not already registered, click **Apps** > **Register as a Developer**, then accept hello policy and follow hello registration steps.</span></span>
4. <span data-ttu-id="bafbb-113">Klikněte na tlačítko **Moje aplikace** > **přidejte novou aplikaci** > **webu** > **přeskočit a vytvořit ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-113">Click **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.</span></span> 
5. <span data-ttu-id="bafbb-114">V **zobrazovaný název**, zadejte jedinečný název pro vaši aplikaci, typ vaše **e-mailu kontaktujte**, vyberte **kategorie** pro vaši aplikaci, pak klikněte na tlačítko **vytvoření ID aplikace**a dokončit kontrolu zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="bafbb-114">In **Display Name**, type a unique name for your app, type your **Contact Email**, choose a **Category** for your app, then click **Create App ID** and complete hello security check.</span></span> <span data-ttu-id="bafbb-115">Tím přejdete toohello vývojáře řídicí panel pro novou aplikaci Facebook.</span><span class="sxs-lookup"><span data-stu-id="bafbb-115">This takes you toohello developer dashboard for your new Facebook app.</span></span>
6. <span data-ttu-id="bafbb-116">V části "Facebook Login", klikněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-116">Under "Facebook Login," click **Get Started**.</span></span> <span data-ttu-id="bafbb-117">Přidejte svoji aplikaci **identifikátor URI pro přesměrování** příliš**identifikátory URI přesměrování platný OAuth**, pak klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-117">Add your application's **Redirect URI** too**Valid OAuth redirect URIs**, then click **Save Changes**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="bafbb-118">Vaše přesměrování identifikátor URI je adresa URL hello aplikace připojí s cestou hello */.auth/login/facebook/callback*.</span><span class="sxs-lookup"><span data-stu-id="bafbb-118">Your redirect URI is hello URL of your application appended with hello path, */.auth/login/facebook/callback*.</span></span> <span data-ttu-id="bafbb-119">Například, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span><span class="sxs-lookup"><span data-stu-id="bafbb-119">For example, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span></span> <span data-ttu-id="bafbb-120">Ujistěte se, že používáte hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bafbb-120">Make sure that you are using hello HTTPS scheme.</span></span>
   > 
   > 
7. <span data-ttu-id="bafbb-121">V levém navigačním hello, klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-121">In hello left-hand navigation, click **Settings**.</span></span> <span data-ttu-id="bafbb-122">Na hello **tajný klíč aplikace** pole, klikněte na tlačítko **zobrazit**, zadejte heslo, pokud požadovaný, poznamenejte si hodnoty hello **ID aplikace** a **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-122">On hello **App Secret** field, click **Show**, provide your password if requested, then make a note of hello values of **App ID** and **App Secret**.</span></span> <span data-ttu-id="bafbb-123">Použijte tyto novější tooconfigure vaší aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="bafbb-123">You use these later tooconfigure your application in Azure.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="bafbb-124">tajný klíč aplikace Hello je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="bafbb-124">hello app secret is an important security credential.</span></span> <span data-ttu-id="bafbb-125">S kýmkoli sdílet tento tajný klíč nebo distribuovat v rámci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="bafbb-125">Do not share this secret with anyone or distribute it within a client application.</span></span>
   > 
   > 
8. <span data-ttu-id="bafbb-126">Hello účtu sítě Facebook, která byla aplikace hello použité tooregister je správcem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bafbb-126">hello Facebook account which was used tooregister hello application is an administrator of hello app.</span></span> <span data-ttu-id="bafbb-127">V tomto okamžiku se jenom správci moct přihlásit do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="bafbb-127">At this point, only administrators can sign into this application.</span></span> <span data-ttu-id="bafbb-128">tooauthenticate klikněte na tlačítko Další účty Facebook **revize aplikace** a povolte **veřejné zkontrolujte <-název_aplikace >** tooenable obecné veřejný přístup pomocí sítě Facebook ověřování.</span><span class="sxs-lookup"><span data-stu-id="bafbb-128">tooauthenticate other Facebook accounts, click **App Review** and enable **Make <your-app-name> public** tooenable general public access using Facebook authentication.</span></span>

## <span data-ttu-id="bafbb-129"><a name="secrets"></a>Aplikace tooyour informace o přidání Facebook</span><span class="sxs-lookup"><span data-stu-id="bafbb-129"><a name="secrets"> </a>Add Facebook information tooyour application</span></span>
1. <span data-ttu-id="bafbb-130">Zpět v hello [portál Azure], přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="bafbb-130">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="bafbb-131">Klikněte na tlačítko **nastavení** > **ověřování / autorizace**a ujistěte se, že **ověřování služby aplikace** je **na**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-131">Click **Settings** > **Authentication / Authorization**, and make sure that **App Service Authentication** is **On**.</span></span>
2. <span data-ttu-id="bafbb-132">Klikněte na tlačítko **Facebook**, vložte hello ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve, můžete také povolit všechny obory, které vyžaduje vaše aplikace a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-132">Click **Facebook**, paste in hello App ID and App Secret values which you obtained previously, optionally enable any scopes needed by your application, then click **OK**.</span></span>
   
    ![][0]
   
    <span data-ttu-id="bafbb-133">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bafbb-133">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="bafbb-134">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="bafbb-134">You must authorize users in your app code.</span></span>
3. <span data-ttu-id="bafbb-135">(Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Facebook, nastavte **tootake akce, když požadavek nebude ověřený** příliš**Facebook**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-135">(Optional) toorestrict access tooyour site tooonly users authenticated by Facebook, set **Action tootake when request is not authenticated** too**Facebook**.</span></span> <span data-ttu-id="bafbb-136">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooFacebook pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="bafbb-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooFacebook for authentication.</span></span>
4. <span data-ttu-id="bafbb-137">Po dokončení konfigurace ověřování, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bafbb-137">When done configuring authentication, click **Save**.</span></span>

<span data-ttu-id="bafbb-138">Jste nyní připraven toouse Facebook pro ověření ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bafbb-138">You are now ready toouse Facebook for authentication in your app.</span></span>

## <span data-ttu-id="bafbb-139"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="bafbb-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[portál Azure]: https://portal.azure.com/
