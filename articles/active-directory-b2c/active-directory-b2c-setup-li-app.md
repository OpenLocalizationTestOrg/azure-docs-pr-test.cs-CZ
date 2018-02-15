---
title: 'Azure Active Directory B2C: Konfigurace LinkedIn | Microsoft Docs'
description: "Zadejte registrace a přihlášení k příjemce s účty LinkedIn v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 860aa90c391604924850a00cf2137d59fa4a1b53
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a><span data-ttu-id="38c99-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty LinkedIn</span><span class="sxs-lookup"><span data-stu-id="38c99-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with LinkedIn accounts</span></span>
## <a name="create-a-linkedin-application"></a><span data-ttu-id="38c99-104">Vytvoření aplikace LinkedIn</span><span class="sxs-lookup"><span data-stu-id="38c99-104">Create a LinkedIn application</span></span>
<span data-ttu-id="38c99-105">Pokud chcete použít LinkedIn jako zprostředkovatele identity v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci LinkedIn a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="38c99-105">To use LinkedIn as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a LinkedIn application and supply it with the right parameters.</span></span> <span data-ttu-id="38c99-106">Potřebujete účet LinkedIn k tomu.</span><span class="sxs-lookup"><span data-stu-id="38c99-106">You need a LinkedIn account to do this.</span></span> <span data-ttu-id="38c99-107">Pokud nemáte, můžete ho na získat [https://www.linkedin.com/](https://www.linkedin.com/).</span><span class="sxs-lookup"><span data-stu-id="38c99-107">If you don’t have one, you can get it at [https://www.linkedin.com/](https://www.linkedin.com/).</span></span>

1. <span data-ttu-id="38c99-108">Přejděte na [LinkedIn vývojáři webu](https://www.developer.linkedin.com/) a přihlaste se pomocí přihlašovacích údajů účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="38c99-108">Go to the [LinkedIn Developers website](https://www.developer.linkedin.com/) and sign in with your LinkedIn account credentials.</span></span>
2. <span data-ttu-id="38c99-109">Klikněte na tlačítko **Moje aplikace** v horní nabídce panelu a pak klikněte na tlačítko **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="38c99-109">Click **My Apps** in the top menu bar and then click **Create Application**.</span></span>
   
    ![LinkedIn - nové aplikace](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. <span data-ttu-id="38c99-111">V **vytvořte novou aplikaci** formuláři, zadejte příslušné informace (**název společnosti**, **název**, **popis**, **Adresa URL aplikace Logo**, **využívání aplikací**, **adresu URL webu**, **e-mailová adresa** a **Telefon do zaměstnání**).</span><span class="sxs-lookup"><span data-stu-id="38c99-111">In the **Create a New Application** form, fill in the relevant information (**Company Name**, **Name**, **Description**, **Application Logo URL**, **Application Use**, **Website URL**, **Business Email** and **Business Phone**).</span></span>
4. <span data-ttu-id="38c99-112">Souhlas s **LinkedIn API podmínky použití** a klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="38c99-112">Agree to the **LinkedIn API Terms of Use** and click **Submit**.</span></span>
   
    ![LinkedIn – registrace aplikace](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. <span data-ttu-id="38c99-114">Zkopírujte hodnoty z **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="38c99-114">Copy the values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="38c99-115">(Je najdete v části **ověřovací klíče**.) Budete potřebovat oba dva konfigurace LinkedIn jako zprostředkovatele identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="38c99-115">(You can find them under **Authentication Keys**.) You will need both of them to configure LinkedIn as an identity provider in your tenant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="38c99-116">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="38c99-116">**Client Secret** is an important security credential.</span></span>
   > 
   > 
6. <span data-ttu-id="38c99-117">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **oprávnění adres URL pro přesměrování** pole (v části **OAuth 2.0**).</span><span class="sxs-lookup"><span data-stu-id="38c99-117">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized Redirect URLs** field (under **OAuth 2.0**).</span></span> <span data-ttu-id="38c99-118">Nahraďte **{klient}** s názvem vašeho klienta (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="38c99-118">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="38c99-119">Klikněte na tlačítko **přidat**a potom klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="38c99-119">Click **Add**, and then click **Update**.</span></span> <span data-ttu-id="38c99-120">**{Klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="38c99-120">The **{tenant}** value is case-sensitive.</span></span>
   
    ![LinkedIn – instalační program aplikace](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="38c99-122">Konfigurace LinkedIn jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="38c99-122">Configure LinkedIn as an identity provider in your tenant</span></span>
1. <span data-ttu-id="38c99-123">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="38c99-123">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="38c99-124">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="38c99-124">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="38c99-125">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="38c99-125">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="38c99-126">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="38c99-126">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="38c99-127">Zadejte například "LI".</span><span class="sxs-lookup"><span data-stu-id="38c99-127">For example, enter "LI".</span></span>
5. <span data-ttu-id="38c99-128">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **LinkedIn**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="38c99-128">Click **Identity provider type**, select **LinkedIn**, and click **OK**.</span></span>
6. <span data-ttu-id="38c99-129">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte ID klienta a tajný klíč klienta LinkedIn aplikace, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="38c99-129">Click **Set up this identity provider** and enter the client ID and client secret of the LinkedIn application that you created earlier.</span></span>
7. <span data-ttu-id="38c99-130">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložte konfiguraci LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="38c99-130">Click **OK** and then click **Create** to save your LinkedIn configuration.</span></span>

