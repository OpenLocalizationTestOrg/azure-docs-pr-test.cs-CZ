---
title: "ověřování služby Twitter tooconfigure aaaHow pro vaši aplikaci aplikační služby"
description: "Zjistěte, jak ověřováním služby Twitter tooconfigure pro vaši aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 0d16ac44d7b54e3567b793a904059d31ab1d8911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-twitter-login"></a><span data-ttu-id="a377a-103">Jak tooconfigure přihlašování Twitter toouse aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="a377a-103">How tooconfigure your App Service application toouse Twitter login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="a377a-104">Toto téma ukazuje, jak Azure App Service toouse tooconfigure Twitter jako zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="a377a-104">This topic shows you how tooconfigure Azure App Service toouse Twitter as an authentication provider.</span></span>

<span data-ttu-id="a377a-105">toocomplete hello postup v tomto tématu, musíte mít účet služby Twitter, který má ověřenou e-mailovou adresu a telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="a377a-105">toocomplete hello procedure in this topic, you must have a Twitter account that has a verified email address and phone number.</span></span> <span data-ttu-id="a377a-106">toocreate nový účet služby Twitter, přejděte příliš<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span><span class="sxs-lookup"><span data-stu-id="a377a-106">toocreate a new Twitter account, go too<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span></span>

## <span data-ttu-id="a377a-107"><a name="register"></a>Aplikaci zaregistrovat u služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="a377a-107"><a name="register"> </a>Register your application with Twitter</span></span>
1. <span data-ttu-id="a377a-108">Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="a377a-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="a377a-109">Kopie vašeho **URL**.</span><span class="sxs-lookup"><span data-stu-id="a377a-109">Copy your **URL**.</span></span> <span data-ttu-id="a377a-110">Tato tooconfigure použijete aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="a377a-110">You will use this tooconfigure your Twitter app.</span></span>
2. <span data-ttu-id="a377a-111">Přejděte toohello [Twitter vývojáři] web, přihlaste se pomocí přihlašovacích údajů účtu služby Twitter a klikněte na **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="a377a-111">Navigate toohello [Twitter Developers] website, sign in with your Twitter account credentials, and click **Create New App**.</span></span>
3. <span data-ttu-id="a377a-112">Typ v hello **název** a **popis** pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a377a-112">Type in hello **Name** and a **Description** for your new app.</span></span> <span data-ttu-id="a377a-113">Vložení ve vaší aplikaci **URL** pro hello **webu** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a377a-113">Paste in your application's **URL** for hello **Website** value.</span></span> <span data-ttu-id="a377a-114">Potom hello **adresu URL zpětné volání**, vložte hello **adresu URL zpětné volání** jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="a377a-114">Then, for hello **Callback URL**, paste hello **Callback URL** you copied earlier.</span></span> <span data-ttu-id="a377a-115">Toto je vaše mobilní aplikace brána připojena s cestou hello, */.auth/login/twitter/callback*.</span><span class="sxs-lookup"><span data-stu-id="a377a-115">This is your Mobile App gateway appended with hello path, */.auth/login/twitter/callback*.</span></span> <span data-ttu-id="a377a-116">Například, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="a377a-116">For example, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span></span> <span data-ttu-id="a377a-117">Ujistěte se, že používáte hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a377a-117">Make sure that you are using hello HTTPS scheme.</span></span>
4. <span data-ttu-id="a377a-118">Na stránce hello dolní hello přečtěte si a přijměte podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="a377a-118">At hello bottom hello page, read and accept hello terms.</span></span> <span data-ttu-id="a377a-119">Pak klikněte na tlačítko **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="a377a-119">Then click **Create your Twitter application**.</span></span> <span data-ttu-id="a377a-120">Tato aplikace hello registry zobrazí podrobnosti o aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a377a-120">This registers hello app displays hello application details.</span></span>
5. <span data-ttu-id="a377a-121">Klikněte na tlačítko hello **nastavení** zkontrolujte **povolit toosign toobe používá tato aplikace se přes Twitter**, pak klikněte na tlačítko **nastavení aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="a377a-121">Click hello **Settings** tab, check **Allow this application toobe used toosign in with Twitter**, then click **Update Settings**.</span></span>
6. <span data-ttu-id="a377a-122">Vyberte hello **klíče a přístupové tokeny** kartě. Poznamenejte si hodnoty hello **uživatelský klíč (klíč rozhraní API)** a **uživatelský tajný klíč (tajný klíč rozhraní API)**.</span><span class="sxs-lookup"><span data-stu-id="a377a-122">Select hello **Keys and Access Tokens** tab. Make a note of hello values of **Consumer Key (API Key)** and **Consumer secret (API Secret)**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a377a-123">Hello uživatelský tajný klíč je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="a377a-123">hello consumer secret is an important security credential.</span></span> <span data-ttu-id="a377a-124">S kýmkoli sdílet tento tajný klíč nebo distribuovat s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="a377a-124">Do not share this secret with anyone or distribute it with your app.</span></span>
   > 
   > 

## <span data-ttu-id="a377a-125"><a name="secrets"></a>Přidat Twitter informace tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="a377a-125"><a name="secrets"> </a>Add Twitter information tooyour application</span></span>
1. <span data-ttu-id="a377a-126">Zpět v hello [portál Azure], přejděte tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="a377a-126">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="a377a-127">Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="a377a-127">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="a377a-128">Pokud hello ověřování / autorizace funkce není povolena, zapněte přepínač hello příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="a377a-128">If hello Authentication / Authorization feature is not enabled, turn hello switch too**On**.</span></span>
3. <span data-ttu-id="a377a-129">Klikněte na tlačítko **Twitter**.</span><span class="sxs-lookup"><span data-stu-id="a377a-129">Click **Twitter**.</span></span> <span data-ttu-id="a377a-130">Vložte hello ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="a377a-130">Paste in hello App ID and App Secret values which you obtained previously.</span></span> <span data-ttu-id="a377a-131">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a377a-131">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="a377a-132">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a377a-132">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="a377a-133">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a377a-133">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="a377a-134">(Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Twitter, nastavte **tootake akce, když požadavek nebude ověřený** příliš**Twitter**.</span><span class="sxs-lookup"><span data-stu-id="a377a-134">(Optional) toorestrict access tooyour site tooonly users authenticated by Twitter, set **Action tootake when request is not authenticated** too**Twitter**.</span></span> <span data-ttu-id="a377a-135">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooTwitter pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a377a-135">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooTwitter for authentication.</span></span>
5. <span data-ttu-id="a377a-136">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a377a-136">Click **Save**.</span></span>

<span data-ttu-id="a377a-137">Jste nyní připraven toouse Twitter pro ověřování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a377a-137">You are now ready toouse Twitter for authentication in your app.</span></span>

## <span data-ttu-id="a377a-138"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="a377a-138"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[portál Azure]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
