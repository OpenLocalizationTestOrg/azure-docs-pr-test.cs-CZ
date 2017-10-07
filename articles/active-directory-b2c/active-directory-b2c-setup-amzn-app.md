---
title: 'Azure Active Directory B2C: Konfigurace Amazon | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte Amazon účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
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
ms.openlocfilehash: 60d7c4b76d9d3e86ed535765329abed07f1e5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-amazon-accounts"></a><span data-ttu-id="ad7cc-103">Azure Active Directory B2C: Poskytnout účty Amazon tooconsumers registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="ad7cc-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Amazon accounts</span></span>
## <a name="create-an-amazon-application"></a><span data-ttu-id="ad7cc-104">Vytvoření aplikace Amazon</span><span class="sxs-lookup"><span data-stu-id="ad7cc-104">Create an Amazon application</span></span>
<span data-ttu-id="ad7cc-105">toouse Amazon jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate Amazon aplikaci a zadat s hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-105">toouse Amazon as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate an Amazon application and supply it with hello right parameters.</span></span> <span data-ttu-id="ad7cc-106">Tuto funkci potřebujete toodo účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-106">You need an Amazon account toodo this.</span></span> <span data-ttu-id="ad7cc-107">Pokud nemáte, můžete ho na získat [http://www.amazon.com/](http://www.amazon.com/).</span><span class="sxs-lookup"><span data-stu-id="ad7cc-107">If you don’t have one, you can get it at [http://www.amazon.com/](http://www.amazon.com/).</span></span>

1. <span data-ttu-id="ad7cc-108">Přejděte toohello [středisku pro vývojáře Amazon](https://login.amazon.com/) a přihlaste se pomocí přihlašovacích údajů účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-108">Go toohello [Amazon Developer Center](https://login.amazon.com/) and sign in with your Amazon account credentials.</span></span>
2. <span data-ttu-id="ad7cc-109">Pokud jste tak již neučinili, klikněte na tlačítko **zaregistrovat**, postupujte podle kroků registrace vývojáře hello a přijměte zásady hello.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-109">If you have not already done so, click **Sign Up**, follow hello developer registration steps, and accept hello policy.</span></span>
3. <span data-ttu-id="ad7cc-110">Klikněte na tlačítko **zaregistrujte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-110">Click **Register new application**.</span></span>
   
    ![Registrace nové aplikace na webu Amazon hello](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. <span data-ttu-id="ad7cc-112">Zadejte informace o aplikaci (**název**, **popis**, a **URL oznámení o ochraně osobních údajů**) a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-112">Provide application information (**Name**, **Description**, and **Privacy Notice URL**) and click **Save**.</span></span>
   
    ![Poskytuje informace o aplikaci pro registraci novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. <span data-ttu-id="ad7cc-114">V hello **nastavení webové** část hodnoty hello kopie **ID klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-114">In hello **Web Settings** section, copy hello values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="ad7cc-115">(Potřebujete tooclick hello **zobrazit tajný klíč** tlačítko to toosee.) Budete potřebovat oba dva tooconfigure Amazon jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-115">(You need tooclick hello **Show Secret** button toosee this.) You need both of them tooconfigure Amazon as an identity provider in your tenant.</span></span> <span data-ttu-id="ad7cc-116">Klikněte na tlačítko **upravit** dolnímu hello hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-116">Click **Edit** at hello bottom of hello section.</span></span> <span data-ttu-id="ad7cc-117">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-117">**Client Secret** is an important security credential.</span></span>
   
    ![Poskytnutí ID klienta a tajný klíč klienta pro novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. <span data-ttu-id="ad7cc-119">Zadejte `https://login.microsoftonline.com` v hello **povolené zdroje JavaScript** pole a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **povoleno vrátit adresy URL** pole.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-119">Enter `https://login.microsoftonline.com` in hello **Allowed JavaScript Origins** field and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Allowed Return URLs** field.</span></span> <span data-ttu-id="ad7cc-120">Nahraďte **{klient}** s názvem vašeho klienta (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="ad7cc-120">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="ad7cc-121">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-121">Click **Save**.</span></span> <span data-ttu-id="ad7cc-122">Hello **{klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-122">hello **{tenant}** value is case-sensitive.</span></span>
   
    ![Poskytnutí zdroje JavaScript a vrátit adresy URL pro novou aplikaci v Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="ad7cc-124">Konfigurace Amazon jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="ad7cc-124">Configure Amazon as an identity provider in your tenant</span></span>
1. <span data-ttu-id="ad7cc-125">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-125">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="ad7cc-126">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-126">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="ad7cc-127">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-127">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="ad7cc-128">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-128">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="ad7cc-129">Zadejte například "Amzn".</span><span class="sxs-lookup"><span data-stu-id="ad7cc-129">For example, enter "Amzn".</span></span>
5. <span data-ttu-id="ad7cc-130">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Amazon**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-130">Click **Identity provider type**, select **Amazon**, and click **OK**.</span></span>
6. <span data-ttu-id="ad7cc-131">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello ID a klienta tajný klíč klienta z hello Amazon aplikace, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-131">Click **Set up this identity provider** and enter hello client ID and client secret of hello Amazon application that you created earlier.</span></span>
7. <span data-ttu-id="ad7cc-132">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave konfiguraci Amazon.</span><span class="sxs-lookup"><span data-stu-id="ad7cc-132">Click **OK** and then click **Create** toosave your Amazon configuration.</span></span>

