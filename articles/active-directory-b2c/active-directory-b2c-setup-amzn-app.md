---
title: 'Azure Active Directory B2C: Konfigurace Amazon | Microsoft Docs'
description: "Zadejte registrace a přihlášení k příjemce s účty Amazon v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: dcc97e1b7f6287bd7692c52bf068950065a26572
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a><span data-ttu-id="cc42d-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty Amazon</span><span class="sxs-lookup"><span data-stu-id="cc42d-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Amazon accounts</span></span>
## <a name="create-an-amazon-application"></a><span data-ttu-id="cc42d-104">Vytvoření aplikace Amazon</span><span class="sxs-lookup"><span data-stu-id="cc42d-104">Create an Amazon application</span></span>
<span data-ttu-id="cc42d-105">Pokud chcete používat službu Amazon jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte k vytvoření aplikace Amazon a poskytne mu se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="cc42d-105">To use Amazon as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create an Amazon application and supply it with the right parameters.</span></span> <span data-ttu-id="cc42d-106">Je nutné k tomu účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="cc42d-106">You need an Amazon account to do this.</span></span> <span data-ttu-id="cc42d-107">Pokud nemáte, můžete ho na získat [http://www.amazon.com/](http://www.amazon.com/).</span><span class="sxs-lookup"><span data-stu-id="cc42d-107">If you don’t have one, you can get it at [http://www.amazon.com/](http://www.amazon.com/).</span></span>

1. <span data-ttu-id="cc42d-108">Přejděte na [středisku pro vývojáře Amazon](https://login.amazon.com/) a přihlaste se pomocí přihlašovacích údajů účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="cc42d-108">Go to the [Amazon Developer Center](https://login.amazon.com/) and sign in with your Amazon account credentials.</span></span>
2. <span data-ttu-id="cc42d-109">Pokud jste tak již neučinili, klikněte na tlačítko **zaregistrovat**, postupujte podle kroků registrace developer a přijměte zásady.</span><span class="sxs-lookup"><span data-stu-id="cc42d-109">If you have not already done so, click **Sign Up**, follow the developer registration steps, and accept the policy.</span></span>
3. <span data-ttu-id="cc42d-110">Klikněte na tlačítko **zaregistrujte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="cc42d-110">Click **Register new application**.</span></span>
   
    ![Registrace nové aplikace na webu Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. <span data-ttu-id="cc42d-112">Zadejte informace o aplikaci (**název**, **popis**, a **URL oznámení o ochraně osobních údajů**) a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cc42d-112">Provide application information (**Name**, **Description**, and **Privacy Notice URL**) and click **Save**.</span></span>
   
    ![Poskytuje informace o aplikaci pro registraci novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. <span data-ttu-id="cc42d-114">V **nastavení webové** část, zkopírujte hodnoty z **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="cc42d-114">In the **Web Settings** section, copy the values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="cc42d-115">(Je třeba kliknout na **zobrazit tajný klíč** tlačítko zobrazení tohoto.) Je nutné oba dva konfigurace Amazon jako zprostředkovatele identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="cc42d-115">(You need to click the **Show Secret** button to see this.) You need both of them to configure Amazon as an identity provider in your tenant.</span></span> <span data-ttu-id="cc42d-116">Klikněte na tlačítko **upravit** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="cc42d-116">Click **Edit** at the bottom of the section.</span></span> <span data-ttu-id="cc42d-117">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="cc42d-117">**Client Secret** is an important security credential.</span></span>
   
    ![Poskytnutí ID klienta a tajný klíč klienta pro novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. <span data-ttu-id="cc42d-119">Zadejte `https://login.microsoftonline.com` v **povolené zdroje JavaScript** pole a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **povoleno vrátit adresy URL** pole.</span><span class="sxs-lookup"><span data-stu-id="cc42d-119">Enter `https://login.microsoftonline.com` in the **Allowed JavaScript Origins** field and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Allowed Return URLs** field.</span></span> <span data-ttu-id="cc42d-120">Nahraďte **{klient}** s názvem vašeho klienta (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="cc42d-120">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="cc42d-121">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cc42d-121">Click **Save**.</span></span> <span data-ttu-id="cc42d-122">**{Klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="cc42d-122">The **{tenant}** value is case-sensitive.</span></span>
   
    ![Poskytnutí zdroje JavaScript a vrátit adresy URL pro novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="cc42d-124">Konfigurace Amazon jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="cc42d-124">Configure Amazon as an identity provider in your tenant</span></span>
1. <span data-ttu-id="cc42d-125">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cc42d-125">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="cc42d-126">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="cc42d-126">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="cc42d-127">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="cc42d-127">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="cc42d-128">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="cc42d-128">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="cc42d-129">Zadejte například "Amzn".</span><span class="sxs-lookup"><span data-stu-id="cc42d-129">For example, enter "Amzn".</span></span>
5. <span data-ttu-id="cc42d-130">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Amazon**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc42d-130">Click **Identity provider type**, select **Amazon**, and click **OK**.</span></span>
6. <span data-ttu-id="cc42d-131">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte ID klienta a tajný klíč klienta aplikace Amazon, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="cc42d-131">Click **Set up this identity provider** and enter the client ID and client secret of the Amazon application that you created earlier.</span></span>
7. <span data-ttu-id="cc42d-132">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložte konfiguraci Amazon.</span><span class="sxs-lookup"><span data-stu-id="cc42d-132">Click **OK** and then click **Create** to save your Amazon configuration.</span></span>

