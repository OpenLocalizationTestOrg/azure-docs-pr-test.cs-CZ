---
title: "Azure Active Directory B2C: Konfigurace účtu Microsoft | Microsoft Docs"
description: "Zadejte registrace a přihlášení k příjemce s účty Microsoft v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
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
ms.openlocfilehash: 59879dc0b3fc1d7af3e2a1f67f1701f451de9126
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a><span data-ttu-id="d3fd7-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty Microsoft</span><span class="sxs-lookup"><span data-stu-id="d3fd7-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Microsoft accounts</span></span>
## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="d3fd7-104">Vytvoření aplikace účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="d3fd7-104">Create a Microsoft account application</span></span>
<span data-ttu-id="d3fd7-105">Pokud chcete používat účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci účtu Microsoft a zadejte se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-105">To use Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Microsoft account application and supply it with the right parameters.</span></span> <span data-ttu-id="d3fd7-106">Potřebujete účet Microsoft k tomu.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-106">You need a Microsoft account to do this.</span></span> <span data-ttu-id="d3fd7-107">Pokud nemáte, můžete ho na získat [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="d3fd7-107">If you don’t have one, you can get it at [https://www.live.com/](https://www.live.com/).</span></span>

1. <span data-ttu-id="d3fd7-108">Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-108">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2. <span data-ttu-id="d3fd7-109">Klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-109">Click **Add an app**.</span></span>
   
    ![Microsoft účet – přidání nové aplikace](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. <span data-ttu-id="d3fd7-111">Zadejte **název** pro aplikaci a klikněte na tlačítko **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-111">Provide a **Name** for your application and click **Create application**.</span></span>
   
    ![Účet Microsoft - název aplikace.](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. <span data-ttu-id="d3fd7-113">Zkopírujte hodnotu **Id aplikace**. Je nutné ho nakonfigurovat účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-113">Copy the value of **Application Id**. You will need it to configure Microsoft account as an identity provider in your tenant.</span></span>
   
    ![Účet Microsoft - Id aplikace](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. <span data-ttu-id="d3fd7-115">Klikněte na **přidat platformy** a zvolte **webové**.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-115">Click on **Add platform** and choose **Web**.</span></span>
   
    ![Microsoft účet – přidejte platformu](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Účet Microsoft – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. <span data-ttu-id="d3fd7-118">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **identifikátory URI přesměrování** pole.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-118">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Redirect URIs** field.</span></span> <span data-ttu-id="d3fd7-119">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="d3fd7-119">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
   
    ![Účet Microsoft - adresy URL pro přesměrování](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. <span data-ttu-id="d3fd7-121">Klikněte na **generovat nové heslo** pod **tajné klíče aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-121">Click on **Generate New Password** under the **Application Secrets** section.</span></span> <span data-ttu-id="d3fd7-122">Zkopírujte nové heslo, které jsou zobrazené na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-122">Copy the new password displayed on screen.</span></span> <span data-ttu-id="d3fd7-123">Je nutné ho nakonfigurovat účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-123">You will need it to configure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="d3fd7-124">Toto heslo je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-124">This password is an important security credential.</span></span>
   
    ![Microsoft účet - vytvořit nové heslo](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Účet Microsoft - nové heslo](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. <span data-ttu-id="d3fd7-127">Zaškrtněte políčko, která uvádí, že **Live SDK podporu** pod **pokročilé možnosti** části.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-127">Check the box that says **Live SDK support** under the **Advanced Options** section.</span></span> <span data-ttu-id="d3fd7-128">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-128">Click **Save**.</span></span>
   
    ![Účet Microsoft - Live SDK podpory](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="d3fd7-130">Nakonfigurujte účet Microsoft jako zprostředkovatel identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="d3fd7-130">Configure Microsoft account as an identity provider in your tenant</span></span>
1. <span data-ttu-id="d3fd7-131">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="d3fd7-132">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="d3fd7-133">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="d3fd7-134">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="d3fd7-135">Zadejte například "Účet spravované služby".</span><span class="sxs-lookup"><span data-stu-id="d3fd7-135">For example, enter "MSA".</span></span>
5. <span data-ttu-id="d3fd7-136">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **účtu Microsoft**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-136">Click **Identity provider type**, select **Microsoft account**, and click **OK**.</span></span>
6. <span data-ttu-id="d3fd7-137">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte Id aplikace a heslo aplikace účtu Microsoft, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-137">Click **Set up this identity provider** and enter the Application Id and password of the Microsoft account application that you created earlier.</span></span>
7. <span data-ttu-id="d3fd7-138">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložit konfiguraci vašeho účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d3fd7-138">Click **OK** and then click **Create** to save your Microsoft account configuration.</span></span>

