---
title: 'Azure Active Directory B2C: Google + konfigurace | Microsoft Docs'
description: "Zadejte registrace a přihlášení k příjemce s účty Google + v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 93589352094fdd556811ba906ee27e7b8ac1d8b5
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a><span data-ttu-id="8623d-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s Google + účty</span><span class="sxs-lookup"><span data-stu-id="8623d-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Google+ accounts</span></span>
## <a name="create-a-google-application"></a><span data-ttu-id="8623d-104">Vytvoření aplikace Google +</span><span class="sxs-lookup"><span data-stu-id="8623d-104">Create a Google+ application</span></span>
<span data-ttu-id="8623d-105">Pokud chcete používat Google + jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci Google + a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="8623d-105">To use Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Google+ application and supply it with the right parameters.</span></span> <span data-ttu-id="8623d-106">Potřebujete účet Google + k tomu.</span><span class="sxs-lookup"><span data-stu-id="8623d-106">You need a Google+ account to do this.</span></span> <span data-ttu-id="8623d-107">Pokud nemáte, můžete ho na získat [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span><span class="sxs-lookup"><span data-stu-id="8623d-107">If you don’t have one, you can get it at [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span></span>

1. <span data-ttu-id="8623d-108">Přejděte na [konzole pro vývojáře Google](https://console.developers.google.com/) a přihlaste se pomocí přihlašovací údaje účtu Google +.</span><span class="sxs-lookup"><span data-stu-id="8623d-108">Go to the [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2. <span data-ttu-id="8623d-109">Klikněte na tlačítko **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8623d-109">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>
   
    ![Google + – Začínáme](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + – nový projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. <span data-ttu-id="8623d-112">Klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření** v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="8623d-112">Click **API Manager** and then click **Credentials** in the left navigation.</span></span>
4. <span data-ttu-id="8623d-113">Klikněte **obrazovky souhlas OAuth** v horní části.</span><span class="sxs-lookup"><span data-stu-id="8623d-113">Click the **OAuth consent screen** tab at the top.</span></span>
   
    ![Google + - přihlašovací údaje](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. <span data-ttu-id="8623d-115">Vyberte nebo zadejte platný **e-mailová adresa**, poskytovat **název produktu**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8623d-115">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. <span data-ttu-id="8623d-117">Klikněte na tlačítko **nové přihlašovací údaje** a potom zvolte **ID klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="8623d-117">Click **New credentials** and then choose **OAuth client ID**.</span></span>
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. <span data-ttu-id="8623d-119">V části **typ aplikace**, vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8623d-119">Under **Application type**, select **Web application**.</span></span>
   
    ![Google + - obrazovky souhlas OAuth](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. <span data-ttu-id="8623d-121">Zadejte **název** pro vaši aplikaci, zadejte `https://login.microsoftonline.com` v **zdroje oprávnění JavaScript** pole, a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **autorizováno přesměrování identifikátory URI** pole.</span><span class="sxs-lookup"><span data-stu-id="8623d-121">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in the **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized redirect URIs** field.</span></span> <span data-ttu-id="8623d-122">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="8623d-122">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="8623d-123">**{Klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="8623d-123">The **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="8623d-124">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8623d-124">Click **Create**.</span></span>
   
    ![Google + - vytvořit ID klienta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. <span data-ttu-id="8623d-126">Zkopírujte hodnoty z **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="8623d-126">Copy the values of **Client ID** and **Client secret**.</span></span> <span data-ttu-id="8623d-127">Budete potřebovat oba dva konfigurace Google + jako zprostředkovatele identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="8623d-127">You will need both of them to configure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="8623d-128">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="8623d-128">**Client secret** is an important security credential.</span></span>
   
    ![Google + - tajný klíč klienta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="8623d-130">Konfigurace Google + jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="8623d-130">Configure Google+ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="8623d-131">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8623d-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="8623d-132">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="8623d-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="8623d-133">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="8623d-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="8623d-134">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="8623d-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="8623d-135">Zadejte například "G +".</span><span class="sxs-lookup"><span data-stu-id="8623d-135">For example, enter "G+".</span></span>
5. <span data-ttu-id="8623d-136">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Google**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8623d-136">Click **Identity provider type**, select **Google**, and click **OK**.</span></span>
6. <span data-ttu-id="8623d-137">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte ID klienta a tajný klíč klienta aplikace Google +, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="8623d-137">Click **Set up this identity provider** and enter the client ID and client secret of the Google+ application that you created earlier.</span></span>
7. <span data-ttu-id="8623d-138">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložte konfiguraci Google +.</span><span class="sxs-lookup"><span data-stu-id="8623d-138">Click **OK** and then click **Create** to save your Google+ configuration.</span></span>

