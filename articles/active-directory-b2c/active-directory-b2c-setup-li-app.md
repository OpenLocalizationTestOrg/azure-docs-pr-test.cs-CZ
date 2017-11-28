---
title: 'Azure Active Directory B2C: Konfigurace LinkedIn | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytnout LinkedIn účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6c5233ef48b24968fd6383f470b5d8a969a78ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-linkedin-accounts"></a><span data-ttu-id="2952c-103">Azure Active Directory B2C: Poskytnout účty LinkedIn tooconsumers registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="2952c-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with LinkedIn accounts</span></span>
## <a name="create-a-linkedin-application"></a><span data-ttu-id="2952c-104">Vytvoření aplikace LinkedIn</span><span class="sxs-lookup"><span data-stu-id="2952c-104">Create a LinkedIn application</span></span>
<span data-ttu-id="2952c-105">toouse LinkedIn jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate LinkedIn aplikace a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="2952c-105">toouse LinkedIn as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a LinkedIn application and supply it with hello right parameters.</span></span> <span data-ttu-id="2952c-106">Tuto funkci potřebujete účet toodo LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="2952c-106">You need a LinkedIn account toodo this.</span></span> <span data-ttu-id="2952c-107">Pokud nemáte, můžete ho na získat [https://www.linkedin.com/](https://www.linkedin.com/).</span><span class="sxs-lookup"><span data-stu-id="2952c-107">If you don’t have one, you can get it at [https://www.linkedin.com/](https://www.linkedin.com/).</span></span>

1. <span data-ttu-id="2952c-108">Přejděte toohello [LinkedIn vývojáři webu](https://www.developer.linkedin.com/) a přihlaste se pomocí přihlašovacích údajů účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="2952c-108">Go toohello [LinkedIn Developers website](https://www.developer.linkedin.com/) and sign in with your LinkedIn account credentials.</span></span>
2. <span data-ttu-id="2952c-109">Klikněte na tlačítko **Moje aplikace** v hello horní nabídce a potom klikněte na **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="2952c-109">Click **My Apps** in hello top menu bar and then click **Create Application**.</span></span>
   
    ![LinkedIn - nové aplikace](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. <span data-ttu-id="2952c-111">V hello **vytvořte novou aplikaci** formuláři, zadejte příslušné informace hello (**název společnosti**, **název**, **popis**, **Adresa URL aplikace Logo**, **využívání aplikací**, **adresu URL webu**, **e-mailová adresa** a **Telefon do zaměstnání**).</span><span class="sxs-lookup"><span data-stu-id="2952c-111">In hello **Create a New Application** form, fill in hello relevant information (**Company Name**, **Name**, **Description**, **Application Logo URL**, **Application Use**, **Website URL**, **Business Email** and **Business Phone**).</span></span>
4. <span data-ttu-id="2952c-112">Souhlasím toohello **LinkedIn API podmínky použití** a klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="2952c-112">Agree toohello **LinkedIn API Terms of Use** and click **Submit**.</span></span>
   
    ![LinkedIn – registrace aplikace](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. <span data-ttu-id="2952c-114">Zkopírujte hodnoty hello **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="2952c-114">Copy hello values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="2952c-115">(Je najdete v části **ověřovací klíče**.) Budete potřebovat oba dva tooconfigure LinkedIn jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="2952c-115">(You can find them under **Authentication Keys**.) You will need both of them tooconfigure LinkedIn as an identity provider in your tenant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2952c-116">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="2952c-116">**Client Secret** is an important security credential.</span></span>
   > 
   > 
6. <span data-ttu-id="2952c-117">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **oprávnění adres URL pro přesměrování** pole (v části **OAuth 2.0**).</span><span class="sxs-lookup"><span data-stu-id="2952c-117">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized Redirect URLs** field (under **OAuth 2.0**).</span></span> <span data-ttu-id="2952c-118">Nahraďte **{klient}** s názvem vašeho klienta (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="2952c-118">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="2952c-119">Klikněte na tlačítko **přidat**a potom klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="2952c-119">Click **Add**, and then click **Update**.</span></span> <span data-ttu-id="2952c-120">Hello **{klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2952c-120">hello **{tenant}** value is case-sensitive.</span></span>
   
    ![LinkedIn – instalační program aplikace](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="2952c-122">Konfigurace LinkedIn jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="2952c-122">Configure LinkedIn as an identity provider in your tenant</span></span>
1. <span data-ttu-id="2952c-123">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2952c-123">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="2952c-124">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="2952c-124">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="2952c-125">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="2952c-125">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="2952c-126">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="2952c-126">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="2952c-127">Zadejte například "LI".</span><span class="sxs-lookup"><span data-stu-id="2952c-127">For example, enter "LI".</span></span>
5. <span data-ttu-id="2952c-128">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **LinkedIn**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2952c-128">Click **Identity provider type**, select **LinkedIn**, and click **OK**.</span></span>
6. <span data-ttu-id="2952c-129">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello ID a klienta tajný klíč klienta z hello LinkedIn aplikace, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="2952c-129">Click **Set up this identity provider** and enter hello client ID and client secret of hello LinkedIn application that you created earlier.</span></span>
7. <span data-ttu-id="2952c-130">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave konfiguraci LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="2952c-130">Click **OK** and then click **Create** toosave your LinkedIn configuration.</span></span>

