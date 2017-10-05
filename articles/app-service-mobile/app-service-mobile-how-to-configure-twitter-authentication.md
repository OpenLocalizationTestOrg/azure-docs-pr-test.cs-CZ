---
title: "Jak nakonfigurovat ověřování služby Twitter pro vaši aplikaci aplikační služby"
description: "Informace o konfiguraci ověřování služby Twitter pro vaši aplikaci aplikační služby."
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
ms.openlocfilehash: afde020b7817dc58ecea24eb4a09cf93d0986eb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a><span data-ttu-id="67bdd-103">Postup konfigurace aplikace služby App Service pomocí přihlášení služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="67bdd-103">How to configure your App Service application to use Twitter login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="67bdd-104">Toto téma ukazuje, jak nakonfigurovat služby Azure App Service pomocí služby Twitter jako zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="67bdd-104">This topic shows you how to configure Azure App Service to use Twitter as an authentication provider.</span></span>

<span data-ttu-id="67bdd-105">Dokončete postup v tomto tématu, musí mít účet služby Twitter, který má ověřenou e-mailovou adresu a telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="67bdd-105">To complete the procedure in this topic, you must have a Twitter account that has a verified email address and phone number.</span></span> <span data-ttu-id="67bdd-106">Chcete-li vytvořit nový účet služby Twitter, přejděte na <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span><span class="sxs-lookup"><span data-stu-id="67bdd-106">To create a new Twitter account, go to <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span></span>

## <span data-ttu-id="67bdd-107"><a name="register"></a>Aplikaci zaregistrovat u služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="67bdd-107"><a name="register"> </a>Register your application with Twitter</span></span>
1. <span data-ttu-id="67bdd-108">Přihlaste se na [portál Azure]a přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67bdd-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="67bdd-109">Kopie vašeho **URL**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-109">Copy your **URL**.</span></span> <span data-ttu-id="67bdd-110">To použijete ke konfiguraci vaší aplikace služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="67bdd-110">You will use this to configure your Twitter app.</span></span>
2. <span data-ttu-id="67bdd-111">Přejděte na [Twitter vývojáři] web, přihlaste se pomocí přihlašovacích údajů účtu služby Twitter a klikněte na **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-111">Navigate to the [Twitter Developers] website, sign in with your Twitter account credentials, and click **Create New App**.</span></span>
3. <span data-ttu-id="67bdd-112">Zadejte **název** a **popis** pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67bdd-112">Type in the **Name** and a **Description** for your new app.</span></span> <span data-ttu-id="67bdd-113">Vložení ve vaší aplikaci **URL** pro **webu** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="67bdd-113">Paste in your application's **URL** for the **Website** value.</span></span> <span data-ttu-id="67bdd-114">Potom **adresu URL zpětné volání**, vložte **adresu URL zpětné volání** jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="67bdd-114">Then, for the **Callback URL**, paste the **Callback URL** you copied earlier.</span></span> <span data-ttu-id="67bdd-115">Toto je vaše mobilní aplikace brána připojena cesta, */.auth/login/twitter/callback*.</span><span class="sxs-lookup"><span data-stu-id="67bdd-115">This is your Mobile App gateway appended with the path, */.auth/login/twitter/callback*.</span></span> <span data-ttu-id="67bdd-116">Například, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="67bdd-116">For example, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span></span> <span data-ttu-id="67bdd-117">Ujistěte se, že používáte schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="67bdd-117">Make sure that you are using the HTTPS scheme.</span></span>
4. <span data-ttu-id="67bdd-118">V dolní části stránky přečtěte si a přijměte podmínky.</span><span class="sxs-lookup"><span data-stu-id="67bdd-118">At the bottom the page, read and accept the terms.</span></span> <span data-ttu-id="67bdd-119">Pak klikněte na tlačítko **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-119">Then click **Create your Twitter application**.</span></span> <span data-ttu-id="67bdd-120">Takovém postupu zaregistruje aplikace zobrazí podrobnosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67bdd-120">This registers the app displays the application details.</span></span>
5. <span data-ttu-id="67bdd-121">Klikněte na tlačítko **nastavení** zkontrolujte **povolit této aplikace, který se má použít k přihlášení pomocí služby Twitter**, pak klikněte na tlačítko **nastavení aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-121">Click the **Settings** tab, check **Allow this application to be used to sign in with Twitter**, then click **Update Settings**.</span></span>
6. <span data-ttu-id="67bdd-122">Vyberte **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="67bdd-122">Select the **Keys and Access Tokens** tab.</span></span> <span data-ttu-id="67bdd-123">Poznamenejte si hodnoty **uživatelský klíč (klíč rozhraní API)** a **uživatelský tajný klíč (tajný klíč rozhraní API)**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-123">Make a note of the values of **Consumer Key (API Key)** and **Consumer secret (API Secret)**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="67bdd-124">Uživatelský tajný klíč je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="67bdd-124">The consumer secret is an important security credential.</span></span> <span data-ttu-id="67bdd-125">S kýmkoli sdílet tento tajný klíč nebo distribuovat s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="67bdd-125">Do not share this secret with anyone or distribute it with your app.</span></span>
   > 
   > 

## <span data-ttu-id="67bdd-126"><a name="secrets"></a>Twitter přidat informace do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="67bdd-126"><a name="secrets"> </a>Add Twitter information to your application</span></span>
1. <span data-ttu-id="67bdd-127">Zpět v [portál Azure], přejděte k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67bdd-127">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="67bdd-128">Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-128">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="67bdd-129">Pokud ověřování / autorizace funkce není povolena, zapněte přepínač k **na**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-129">If the Authentication / Authorization feature is not enabled, turn the switch to **On**.</span></span>
3. <span data-ttu-id="67bdd-130">Klikněte na tlačítko **Twitter**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-130">Click **Twitter**.</span></span> <span data-ttu-id="67bdd-131">Vložte v ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="67bdd-131">Paste in the App ID and App Secret values which you obtained previously.</span></span> <span data-ttu-id="67bdd-132">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-132">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="67bdd-133">Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje autorizovaný přístup k obsahu webu a rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="67bdd-133">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="67bdd-134">Je nutné autorizovat uživatele v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="67bdd-134">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="67bdd-135">(Volitelné) Chcete-li omezit přístup k webu jenom na uživatele ověřeného službou Twitter, nastavte **akci provést, když požadavek nebude ověřený** k **Twitter**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-135">(Optional) To restrict access to your site to only users authenticated by Twitter, set **Action to take when request is not authenticated** to **Twitter**.</span></span> <span data-ttu-id="67bdd-136">To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují do služby Twitter pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="67bdd-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Twitter for authentication.</span></span>
5. <span data-ttu-id="67bdd-137">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="67bdd-137">Click **Save**.</span></span>

<span data-ttu-id="67bdd-138">Nyní jste připraveni pro ověřování ve vaší aplikaci pomocí služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="67bdd-138">You are now ready to use Twitter for authentication in your app.</span></span>

## <span data-ttu-id="67bdd-139"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="67bdd-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

<span data-ttu-id="67bdd-140">[Twitter vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268300</span><span class="sxs-lookup"><span data-stu-id="67bdd-140">[Twitter Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300</span></span>
<span data-ttu-id="67bdd-141">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="67bdd-141">[Azure portal]: https://portal.azure.com/</span></span>
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
