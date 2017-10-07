---
title: "Azure Active Directory B2C: Konfigurace účtu Microsoft | Microsoft Docs"
description: "Registrace a přihlášení tooconsumers poskytněte účty Microsoft v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: bec4777f003c459030f68c35b24f0e4bcddf84ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-microsoft-accounts"></a><span data-ttu-id="e88cb-103">Azure Active Directory B2C: Zadejte tooconsumers registrace a přihlášení s účty Microsoft</span><span class="sxs-lookup"><span data-stu-id="e88cb-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Microsoft accounts</span></span>
## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="e88cb-104">Vytvoření aplikace účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="e88cb-104">Create a Microsoft account application</span></span>
<span data-ttu-id="e88cb-105">toouse účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba toocreate k aplikaci účtu Microsoft a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="e88cb-105">toouse Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Microsoft account application and supply it with hello right parameters.</span></span> <span data-ttu-id="e88cb-106">Tuto funkci potřebujete toodo účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e88cb-106">You need a Microsoft account toodo this.</span></span> <span data-ttu-id="e88cb-107">Pokud nemáte, můžete ho na získat [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="e88cb-107">If you don’t have one, you can get it at [https://www.live.com/](https://www.live.com/).</span></span>

1. <span data-ttu-id="e88cb-108">Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e88cb-108">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2. <span data-ttu-id="e88cb-109">Klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="e88cb-109">Click **Add an app**.</span></span>
   
    ![Microsoft účet – přidání nové aplikace](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. <span data-ttu-id="e88cb-111">Zadejte **název** pro aplikaci a klikněte na tlačítko **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="e88cb-111">Provide a **Name** for your application and click **Create application**.</span></span>
   
    ![Účet Microsoft - název aplikace.](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. <span data-ttu-id="e88cb-113">Zkopírujte hodnotu hello **Id aplikace**. Je nutné ji tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="e88cb-113">Copy hello value of **Application Id**. You will need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span>
   
    ![Účet Microsoft - Id aplikace](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. <span data-ttu-id="e88cb-115">Klikněte na **přidat platformy** a zvolte **webové**.</span><span class="sxs-lookup"><span data-stu-id="e88cb-115">Click on **Add platform** and choose **Web**.</span></span>
   
    ![Microsoft účet – přidejte platformu](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Účet Microsoft – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. <span data-ttu-id="e88cb-118">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **identifikátory URI přesměrování** pole.</span><span class="sxs-lookup"><span data-stu-id="e88cb-118">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Redirect URIs** field.</span></span> <span data-ttu-id="e88cb-119">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="e88cb-119">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
   
    ![Účet Microsoft - adresy URL pro přesměrování](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. <span data-ttu-id="e88cb-121">Klikněte na **generovat nové heslo** pod hello **tajné klíče aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="e88cb-121">Click on **Generate New Password** under hello **Application Secrets** section.</span></span> <span data-ttu-id="e88cb-122">Zkopírujte hello nové heslo zobrazené na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="e88cb-122">Copy hello new password displayed on screen.</span></span> <span data-ttu-id="e88cb-123">Je nutné ji tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="e88cb-123">You will need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="e88cb-124">Toto heslo je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="e88cb-124">This password is an important security credential.</span></span>
   
    ![Microsoft účet - vytvořit nové heslo](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Účet Microsoft - nové heslo](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. <span data-ttu-id="e88cb-127">Zaškrtávací políčko hello, která uvádí, že **Live SDK podporu** pod hello **pokročilé možnosti** části.</span><span class="sxs-lookup"><span data-stu-id="e88cb-127">Check hello box that says **Live SDK support** under hello **Advanced Options** section.</span></span> <span data-ttu-id="e88cb-128">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e88cb-128">Click **Save**.</span></span>
   
    ![Účet Microsoft - Live SDK podpory](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="e88cb-130">Nakonfigurujte účet Microsoft jako zprostředkovatel identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="e88cb-130">Configure Microsoft account as an identity provider in your tenant</span></span>
1. <span data-ttu-id="e88cb-131">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e88cb-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="e88cb-132">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="e88cb-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="e88cb-133">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="e88cb-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="e88cb-134">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="e88cb-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="e88cb-135">Zadejte například "Účet spravované služby".</span><span class="sxs-lookup"><span data-stu-id="e88cb-135">For example, enter "MSA".</span></span>
5. <span data-ttu-id="e88cb-136">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **účtu Microsoft**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e88cb-136">Click **Identity provider type**, select **Microsoft account**, and click **OK**.</span></span>
6. <span data-ttu-id="e88cb-137">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello Id aplikace a heslo hello aplikace účtu Microsoft, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e88cb-137">Click **Set up this identity provider** and enter hello Application Id and password of hello Microsoft account application that you created earlier.</span></span>
7. <span data-ttu-id="e88cb-138">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave účet Microsoft konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e88cb-138">Click **OK** and then click **Create** toosave your Microsoft account configuration.</span></span>

