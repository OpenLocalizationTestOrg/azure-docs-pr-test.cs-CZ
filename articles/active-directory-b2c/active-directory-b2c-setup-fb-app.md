---
title: "Azure Active Directory B2C: Konfigurace sítě Facebook | Microsoft Docs"
description: "Registrace a přihlášení tooconsumers poskytněte Facebook účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 4c3b79c7248bd1e789bf33fc467abb27d0170edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-facebook-accounts"></a><span data-ttu-id="95071-103">Azure Active Directory B2C: Zadejte tooconsumers registrace a přihlášení s účty služby Facebook</span><span class="sxs-lookup"><span data-stu-id="95071-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Facebook accounts</span></span>
## <a name="create-a-facebook-application"></a><span data-ttu-id="95071-104">Vytvořit aplikaci pro Facebook</span><span class="sxs-lookup"><span data-stu-id="95071-104">Create a Facebook application</span></span>
<span data-ttu-id="95071-105">toouse Facebook jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate aplikace Facebook a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="95071-105">toouse Facebook as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Facebook application and supply it with hello right parameters.</span></span> <span data-ttu-id="95071-106">Tuto funkci potřebujete toodo účet Facebooku.</span><span class="sxs-lookup"><span data-stu-id="95071-106">You need a Facebook account toodo this.</span></span> <span data-ttu-id="95071-107">Pokud nemáte, můžete ho na získat [https://www.facebook.com/](https://www.facebook.com/).</span><span class="sxs-lookup"><span data-stu-id="95071-107">If you don’t have one, you can get it at [https://www.facebook.com/](https://www.facebook.com/).</span></span>

1. <span data-ttu-id="95071-108">Přejděte toohello [Facebook pro vývojáře](https://developers.facebook.com/) webu a přihlaste se pomocí vaší sítě Facebook přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="95071-108">Go toohello [Facebook for developers](https://developers.facebook.com/) website and sign in with your Facebook account credentials.</span></span>
2. <span data-ttu-id="95071-109">Pokud jste tak již neučinili, musíte tooregister jako vývojář Facebook.</span><span class="sxs-lookup"><span data-stu-id="95071-109">If you have not already done so, you need tooregister as a Facebook developer.</span></span> <span data-ttu-id="95071-110">toodo tento, klikněte na tlačítko **zaregistrovat** (na hello pravém horním rohu stránky hello), přijměte zásady pro Facebook a kroky registrace hello.</span><span class="sxs-lookup"><span data-stu-id="95071-110">toodo this, click **Register** (on hello upper-right corner of hello page), accept Facebook's policies, and complete hello registration steps.</span></span>
3. <span data-ttu-id="95071-111">Klikněte na tlačítko **Moje aplikace** a pak klikněte na **přidejte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="95071-111">Click **My Apps** and then click **Add a New App**.</span></span> 
4. <span data-ttu-id="95071-112">Ve formuláři hello poskytují **zobrazovaný název** platné **e-mailu kontaktujte**.</span><span class="sxs-lookup"><span data-stu-id="95071-112">In hello form, provide a **Display Name** and a valid **Contact Email**.</span></span>
5. <span data-ttu-id="95071-113">Klikněte na tlačítko **vytvoření ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95071-113">Click **Create App ID**.</span></span> <span data-ttu-id="95071-114">To může vyžadovat tooaccept Facebook platformy zásady a dokončit kontrolu zabezpečení online.</span><span class="sxs-lookup"><span data-stu-id="95071-114">This may require you tooaccept Facebook platform policies and complete an online security check.</span></span>
6. <span data-ttu-id="95071-115">V levém sloupci hello, klikněte na **nastavení** a pak vyberte **základní** Pokud již není vybrán.</span><span class="sxs-lookup"><span data-stu-id="95071-115">In hello left column, click **Settings** and then select **Basic** if not selected already.</span></span>
7. <span data-ttu-id="95071-116">Vyberte **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="95071-116">Select a **Category**.</span></span> 
8. <span data-ttu-id="95071-117">Klikněte na tlačítko **+ přidejte platformu** a vyberte **webu**.</span><span class="sxs-lookup"><span data-stu-id="95071-117">Click **+ Add Platform** and select **Website**.</span></span>
   
    ![Facebook – nastavení](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook – nastavení – Web](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. <span data-ttu-id="95071-120">Zadejte `https://login.microsoftonline.com/` v hello **adresa URL webu** pole a pak klikněte na **uložit změny** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="95071-120">Enter `https://login.microsoftonline.com/` in hello **Site URL** field and then click **Save Changes** at hello bottom of hello page.</span></span>
   
    ![Facebook – adresa URL webu](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. <span data-ttu-id="95071-122">Zkopírujte hodnotu hello **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95071-122">Copy hello value of **App ID**.</span></span> <span data-ttu-id="95071-123">Klikněte na tlačítko **zobrazit** a zkopírujte hodnotu hello **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95071-123">Click **Show** and copy hello value of **App Secret**.</span></span> <span data-ttu-id="95071-124">Budete potřebovat oba dva tooconfigure Facebook jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="95071-124">You will need both of them tooconfigure Facebook as an identity provider in your tenant.</span></span> <span data-ttu-id="95071-125">**Tajný klíč aplikace** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="95071-125">**App Secret** is an important security credential.</span></span>
   
    ![Facebook - ID aplikace a tajný klíč aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. <span data-ttu-id="95071-127">Klikněte na tlačítko **+ přidat produkt** hello levé navigaci a potom hello **nastavit až** tlačítko pro **Facebook přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="95071-127">Click **+ Add Product** on hello left navigation and then hello **Set Up** button for **Facebook Login**.</span></span>
   
    ![Facebook - Facebook přihlášení](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. <span data-ttu-id="95071-129">Klikněte na tlačítko **nastavení** na správné nav hello pod **Facebook přihlášení**</span><span class="sxs-lookup"><span data-stu-id="95071-129">Click **Settings** on hello right nav under **Facebook Login**</span></span>

    ![Facebook – nastavení přihlášení Facebook](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. <span data-ttu-id="95071-131">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **identifikátory URI přesměrování platný OAuth** pole hello **nastavení klienta OAuth** části.</span><span class="sxs-lookup"><span data-stu-id="95071-131">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Valid OAuth redirect URIs** field in hello **Client OAuth Settings** section.</span></span> <span data-ttu-id="95071-132">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="95071-132">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="95071-133">Klikněte na tlačítko **uložit změny** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="95071-133">Click **Save Changes** at hello bottom of hello page.</span></span>
    
    ![Facebook – identifikátor URI přesměrování OAuth](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. <span data-ttu-id="95071-135">toomake aplikace Facebook použitelné pomocí Azure AD B2C, musíte toomake ho veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="95071-135">toomake your Facebook application usable by Azure AD B2C, you need toomake it publicly available.</span></span> <span data-ttu-id="95071-136">To provedete kliknutím na **revize aplikace** na hello levé navigační a zapnutí hello přepínač hello horní části stránky hello příliš**Ano** a kliknutím na **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="95071-136">You can do this by clicking **App Review** on hello left navigation and by turning hello switch at hello top of hello page too**YES** and clicking **Confirm**.</span></span>
    
    ![Facebook - veřejné aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="95071-138">Konfigurovat jako zprostředkovatel identity ve vašem klientovi sítě Facebook</span><span class="sxs-lookup"><span data-stu-id="95071-138">Configure Facebook as an identity provider in your tenant</span></span>
1. <span data-ttu-id="95071-139">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="95071-139">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="95071-140">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="95071-140">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="95071-141">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="95071-141">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="95071-142">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="95071-142">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="95071-143">Zadejte například "Facebook".</span><span class="sxs-lookup"><span data-stu-id="95071-143">For example, enter "Facebook".</span></span>
5. <span data-ttu-id="95071-144">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Facebook**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="95071-144">Click **Identity provider type**, select **Facebook**, and click **OK**.</span></span>
6. <span data-ttu-id="95071-145">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello app ID a aplikace tajný klíč (hello aplikace Facebook, který jste vytvořili dříve) v hello **ID klienta** a **tajný klíč klienta**polí v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="95071-145">Click **Set up this identity provider** and enter hello app ID and app secret (of hello Facebook application that you created earlier) in hello **Client ID** and **Client secret** fields respectively.</span></span>
7. <span data-ttu-id="95071-146">Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** toosave konfiguraci sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="95071-146">Click **OK**, and then click **Create** toosave your Facebook configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="95071-147">Přidání **zprostředkovatele Identity** tooyour klienta nedojde ke změně existující zásady.</span><span class="sxs-lookup"><span data-stu-id="95071-147">Adding an **Identity provider** tooyour tenant does not modify your existing policies.</span></span> <span data-ttu-id="95071-148">Mějte na paměti tooupdate zásad zahrnutím hello zprostředkovatele identity, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="95071-148">Remember tooupdate your policies by including hello identity provider you just created.</span></span>
>