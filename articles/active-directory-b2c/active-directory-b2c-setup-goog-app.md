---
title: 'Azure Active Directory B2C: Google + konfigurace | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte Google + účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ef66eb17777acd95b5f4745ed6097c77e37663b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-google-accounts"></a><span data-ttu-id="f45db-103">Azure Active Directory B2C: Poskytnout Google + účty tooconsumers registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="f45db-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Google+ accounts</span></span>
## <a name="create-a-google-application"></a><span data-ttu-id="f45db-104">Vytvoření aplikace Google +</span><span class="sxs-lookup"><span data-stu-id="f45db-104">Create a Google+ application</span></span>
<span data-ttu-id="f45db-105">toouse Google + jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate aplikace Google + a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="f45db-105">toouse Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Google+ application and supply it with hello right parameters.</span></span> <span data-ttu-id="f45db-106">Tuto funkci potřebujete účet toodo Google +.</span><span class="sxs-lookup"><span data-stu-id="f45db-106">You need a Google+ account toodo this.</span></span> <span data-ttu-id="f45db-107">Pokud nemáte, můžete ho na získat [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span><span class="sxs-lookup"><span data-stu-id="f45db-107">If you don’t have one, you can get it at [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span></span>

1. <span data-ttu-id="f45db-108">Přejděte toohello [konzole pro vývojáře Google](https://console.developers.google.com/) a přihlaste se pomocí přihlašovací údaje účtu Google +.</span><span class="sxs-lookup"><span data-stu-id="f45db-108">Go toohello [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2. <span data-ttu-id="f45db-109">Klikněte na tlačítko **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f45db-109">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>
   
    ![Google + – Začínáme](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + – nový projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. <span data-ttu-id="f45db-112">Klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření** v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="f45db-112">Click **API Manager** and then click **Credentials** in hello left navigation.</span></span>
4. <span data-ttu-id="f45db-113">Klikněte na tlačítko hello **obrazovky souhlas OAuth** karty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="f45db-113">Click hello **OAuth consent screen** tab at hello top.</span></span>
   
    ![Google + - přihlašovací údaje](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. <span data-ttu-id="f45db-115">Vyberte nebo zadejte platný **e-mailová adresa**, poskytovat **název produktu**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f45db-115">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. <span data-ttu-id="f45db-117">Klikněte na tlačítko **nové přihlašovací údaje** a potom zvolte **ID klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="f45db-117">Click **New credentials** and then choose **OAuth client ID**.</span></span>
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. <span data-ttu-id="f45db-119">V části **typ aplikace**, vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f45db-119">Under **Application type**, select **Web application**.</span></span>
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. <span data-ttu-id="f45db-121">Zadejte **název** pro vaši aplikaci, zadejte `https://login.microsoftonline.com` v hello **zdroje oprávnění JavaScript** pole, a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **autorizováno přesměrování identifikátory URI**pole.</span><span class="sxs-lookup"><span data-stu-id="f45db-121">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in hello **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized redirect URIs** field.</span></span> <span data-ttu-id="f45db-122">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="f45db-122">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="f45db-123">Hello **{klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f45db-123">hello **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="f45db-124">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f45db-124">Click **Create**.</span></span>
   
    ![Google + - vytvořit ID klienta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. <span data-ttu-id="f45db-126">Zkopírujte hodnoty hello **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="f45db-126">Copy hello values of **Client ID** and **Client secret**.</span></span> <span data-ttu-id="f45db-127">Budete potřebovat oba dva tooconfigure Google + jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="f45db-127">You will need both of them tooconfigure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="f45db-128">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="f45db-128">**Client secret** is an important security credential.</span></span>
   
    ![Google + - tajný klíč klienta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="f45db-130">Konfigurace Google + jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="f45db-130">Configure Google+ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="f45db-131">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f45db-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="f45db-132">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="f45db-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="f45db-133">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="f45db-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="f45db-134">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="f45db-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="f45db-135">Zadejte například "G +".</span><span class="sxs-lookup"><span data-stu-id="f45db-135">For example, enter "G+".</span></span>
5. <span data-ttu-id="f45db-136">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Google**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f45db-136">Click **Identity provider type**, select **Google**, and click **OK**.</span></span>
6. <span data-ttu-id="f45db-137">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello ID a klienta tajný klíč klienta z hello aplikace Google +, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="f45db-137">Click **Set up this identity provider** and enter hello client ID and client secret of hello Google+ application that you created earlier.</span></span>
7. <span data-ttu-id="f45db-138">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave konfiguraci Google +.</span><span class="sxs-lookup"><span data-stu-id="f45db-138">Click **OK** and then click **Create** toosave your Google+ configuration.</span></span>

