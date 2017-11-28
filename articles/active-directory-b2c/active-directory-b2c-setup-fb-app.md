---
title: "Azure Active Directory B2C: Konfigurace sítě Facebook | Microsoft Docs"
description: "Zadejte registrace a přihlášení k příjemce s účty sítě Facebook v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
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
ms.openlocfilehash: 8c2154fcf33537358b549395d15b4ba937371cd0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a><span data-ttu-id="0c9ce-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty služby Facebook</span><span class="sxs-lookup"><span data-stu-id="0c9ce-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Facebook accounts</span></span>
## <a name="create-a-facebook-application"></a><span data-ttu-id="0c9ce-104">Vytvořit aplikaci pro Facebook</span><span class="sxs-lookup"><span data-stu-id="0c9ce-104">Create a Facebook application</span></span>
<span data-ttu-id="0c9ce-105">Pokud chcete použít Facebook jako zprostředkovatele identity v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci pro Facebook a přiřaďte mu správné parametry.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-105">To use Facebook as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Facebook application and supply it with the right parameters.</span></span> <span data-ttu-id="0c9ce-106">Je nutné k tomu účtu sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-106">You need a Facebook account to do this.</span></span> <span data-ttu-id="0c9ce-107">Pokud nemáte, můžete ho na získat [https://www.facebook.com/](https://www.facebook.com/).</span><span class="sxs-lookup"><span data-stu-id="0c9ce-107">If you don’t have one, you can get it at [https://www.facebook.com/](https://www.facebook.com/).</span></span>

1. <span data-ttu-id="0c9ce-108">Přejděte na [Facebook pro vývojáře](https://developers.facebook.com/) webu a přihlaste se pomocí vaší sítě Facebook přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-108">Go to the [Facebook for developers](https://developers.facebook.com/) website and sign in with your Facebook account credentials.</span></span>
2. <span data-ttu-id="0c9ce-109">Pokud jste tak již neučinili, musíte zaregistrovat jako vývojář Facebook.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-109">If you have not already done so, you need to register as a Facebook developer.</span></span> <span data-ttu-id="0c9ce-110">Chcete-li to provést, klikněte na tlačítko **zaregistrovat** (v horním pravém horním rohu stránky), přijměte zásady pro Facebook a dokončete registraci.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-110">To do this, click **Register** (on the upper-right corner of the page), accept Facebook's policies, and complete the registration steps.</span></span>
3. <span data-ttu-id="0c9ce-111">Klikněte na tlačítko **Moje aplikace** a pak klikněte na **přidejte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-111">Click **My Apps** and then click **Add a New App**.</span></span> 
4. <span data-ttu-id="0c9ce-112">Ve formuláři, zadejte **zobrazovaný název** platné **e-mailu kontaktujte**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-112">In the form, provide a **Display Name** and a valid **Contact Email**.</span></span>
5. <span data-ttu-id="0c9ce-113">Klikněte na tlačítko **vytvoření ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-113">Click **Create App ID**.</span></span> <span data-ttu-id="0c9ce-114">To může vyžadovat přijměte zásady platformy Facebook a dokončit kontrolu zabezpečení online.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-114">This may require you to accept Facebook platform policies and complete an online security check.</span></span>
6. <span data-ttu-id="0c9ce-115">V levém sloupci klikněte na tlačítko **nastavení** a pak vyberte **základní** Pokud již není vybrán.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-115">In the left column, click **Settings** and then select **Basic** if not selected already.</span></span>
7. <span data-ttu-id="0c9ce-116">Vyberte **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-116">Select a **Category**.</span></span> 
8. <span data-ttu-id="0c9ce-117">Klikněte na tlačítko **+ přidejte platformu** a vyberte **webu**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-117">Click **+ Add Platform** and select **Website**.</span></span>
   
    ![Facebook – nastavení](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook – nastavení – Web](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. <span data-ttu-id="0c9ce-120">Zadejte `https://login.microsoftonline.com/` v **adresa URL webu** pole a pak klikněte na **uložit změny** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-120">Enter `https://login.microsoftonline.com/` in the **Site URL** field and then click **Save Changes** at the bottom of the page.</span></span>
   
    ![Facebook – adresa URL webu](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. <span data-ttu-id="0c9ce-122">Zkopírujte hodnotu **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-122">Copy the value of **App ID**.</span></span> <span data-ttu-id="0c9ce-123">Klikněte na tlačítko **zobrazit** a zkopírujte hodnotu **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-123">Click **Show** and copy the value of **App Secret**.</span></span> <span data-ttu-id="0c9ce-124">Budete potřebovat obou z nich nakonfigurovat jako zprostředkovatel identity ve vašem klientovi sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-124">You will need both of them to configure Facebook as an identity provider in your tenant.</span></span> <span data-ttu-id="0c9ce-125">**Tajný klíč aplikace** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-125">**App Secret** is an important security credential.</span></span>
   
    ![Facebook - ID aplikace a tajný klíč aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. <span data-ttu-id="0c9ce-127">Klikněte na tlačítko **+ přidat produkt** na levé straně a pak **nastavit až** tlačítko pro **Facebook přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-127">Click **+ Add Product** on the left navigation and then the **Set Up** button for **Facebook Login**.</span></span>
   
    ![Facebook - Facebook přihlášení](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. <span data-ttu-id="0c9ce-129">Klikněte na tlačítko **nastavení** na pravém navigaci v části **Facebook přihlášení**</span><span class="sxs-lookup"><span data-stu-id="0c9ce-129">Click **Settings** on the right nav under **Facebook Login**</span></span>

    ![Facebook – nastavení přihlášení Facebook](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. <span data-ttu-id="0c9ce-131">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **identifikátory URI přesměrování platný OAuth** pole **nastavení klienta OAuth** části.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-131">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Valid OAuth redirect URIs** field in the **Client OAuth Settings** section.</span></span> <span data-ttu-id="0c9ce-132">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="0c9ce-132">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="0c9ce-133">Klikněte na tlačítko **uložit změny** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-133">Click **Save Changes** at the bottom of the page.</span></span>
    
    ![Facebook – identifikátor URI přesměrování OAuth](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. <span data-ttu-id="0c9ce-135">Chcete-li aplikace Facebook použitelná pro Azure AD B2C, je třeba, aby veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-135">To make your Facebook application usable by Azure AD B2C, you need to make it publicly available.</span></span> <span data-ttu-id="0c9ce-136">To provedete kliknutím na **revize aplikace** na levé straně a vypnutím přepínače v horní části stránky **Ano** a kliknutím na **potvrdit**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-136">You can do this by clicking **App Review** on the left navigation and by turning the switch at the top of the page to **YES** and clicking **Confirm**.</span></span>
    
    ![Facebook - veřejné aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="0c9ce-138">Konfigurovat jako zprostředkovatel identity ve vašem klientovi sítě Facebook</span><span class="sxs-lookup"><span data-stu-id="0c9ce-138">Configure Facebook as an identity provider in your tenant</span></span>
1. <span data-ttu-id="0c9ce-139">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-139">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="0c9ce-140">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-140">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="0c9ce-141">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-141">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="0c9ce-142">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-142">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="0c9ce-143">Zadejte například "Facebook".</span><span class="sxs-lookup"><span data-stu-id="0c9ce-143">For example, enter "Facebook".</span></span>
5. <span data-ttu-id="0c9ce-144">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Facebook**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-144">Click **Identity provider type**, select **Facebook**, and click **OK**.</span></span>
6. <span data-ttu-id="0c9ce-145">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte ID aplikace a tajný klíč aplikace (of aplikace Facebook, který jste vytvořili dříve) v **ID klienta** a **tajný klíč klienta** polí v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-145">Click **Set up this identity provider** and enter the app ID and app secret (of the Facebook application that you created earlier) in the **Client ID** and **Client secret** fields respectively.</span></span>
7. <span data-ttu-id="0c9ce-146">Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** uložte konfiguraci sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-146">Click **OK**, and then click **Create** to save your Facebook configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="0c9ce-147">Přidání **zprostředkovatele Identity** ke klientovi neupravuje existující zásady.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-147">Adding an **Identity provider** to your tenant does not modify your existing policies.</span></span> <span data-ttu-id="0c9ce-148">Nezapomeňte aktualizovat zásady zahrnutím zprostředkovatele identity, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0c9ce-148">Remember to update your policies by including the identity provider you just created.</span></span>
>