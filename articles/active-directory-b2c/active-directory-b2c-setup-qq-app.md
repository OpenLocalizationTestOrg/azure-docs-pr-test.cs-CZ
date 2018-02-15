---
title: 'Azure Active Directory B2C: Konfigurace QQ | Microsoft Docs'
description: "Zadejte registrace a přihlášení k příjemce s QQ účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: d4cc26d4f206baf9137feae0825b1f9fa5a7c8d6
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a><span data-ttu-id="e6492-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s QQ účty</span><span class="sxs-lookup"><span data-stu-id="e6492-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with QQ accounts</span></span>

> [!NOTE]
> <span data-ttu-id="e6492-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="e6492-104">This feature is in preview.</span></span>
> 

## <a name="create-a-qq-application"></a><span data-ttu-id="e6492-105">Vytvoření aplikace QQ</span><span class="sxs-lookup"><span data-stu-id="e6492-105">Create a QQ application</span></span>

<span data-ttu-id="e6492-106">Pokud chcete použít QQ jako zprostředkovatele identity v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci QQ a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="e6492-106">To use QQ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a QQ application and supply it with the right parameters.</span></span> <span data-ttu-id="e6492-107">Potřebujete účet QQ k tomu.</span><span class="sxs-lookup"><span data-stu-id="e6492-107">You need a QQ account to do this.</span></span> <span data-ttu-id="e6492-108">Pokud nemáte, můžete jej získat v [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span><span class="sxs-lookup"><span data-stu-id="e6492-108">If you don’t have one, you can get one at [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span></span>

### <a name="register-for-the-qq-developer-program"></a><span data-ttu-id="e6492-109">Registrace pro QQ programu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="e6492-109">Register for the QQ developer program</span></span>

1. <span data-ttu-id="e6492-110">Přejděte na [portál pro vývojáře QQ](http://open.qq.com) a přihlaste se pomocí přihlašovacích údajů účtu QQ.</span><span class="sxs-lookup"><span data-stu-id="e6492-110">Go to the [QQ developer portal](http://open.qq.com) and sign in with your QQ account credentials.</span></span>
2. <span data-ttu-id="e6492-111">Po přihlášení, přejděte na [http://open.qq.com/reg](http://open.qq.com/reg) sami zaregistrovat jako vývojář.</span><span class="sxs-lookup"><span data-stu-id="e6492-111">After signing in, go to [http://open.qq.com/reg](http://open.qq.com/reg) to register yourself as a developer.</span></span>
3. <span data-ttu-id="e6492-112">V nabídce vyberte**个人**(jednotlivé vývojáře).</span><span class="sxs-lookup"><span data-stu-id="e6492-112">In the menu, select **个人** (individual developer).</span></span>
4. <span data-ttu-id="e6492-113">Zadejte požadované informace do formuláře a klikněte na tlačítko**下一步**(Další krok).</span><span class="sxs-lookup"><span data-stu-id="e6492-113">Enter the required information into the form and click **下一步** (next step).</span></span>
5. <span data-ttu-id="e6492-114">Dokončete proces ověření e-mailu.</span><span class="sxs-lookup"><span data-stu-id="e6492-114">Complete the email verification process.</span></span>

> [!NOTE]
> <span data-ttu-id="e6492-115">Musíte se za několik dní po registraci jako vývojář schválení.</span><span class="sxs-lookup"><span data-stu-id="e6492-115">You will need to wait a few days to be approved after registering as a developer.</span></span> 

### <a name="register-a-qq-application"></a><span data-ttu-id="e6492-116">Registrace aplikace QQ</span><span class="sxs-lookup"><span data-stu-id="e6492-116">Register a QQ application</span></span>

1. <span data-ttu-id="e6492-117">Go to [https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span><span class="sxs-lookup"><span data-stu-id="e6492-117">Go to [https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span></span>
2. <span data-ttu-id="e6492-118">Klikněte na**应用管理**(Správa aplikací).</span><span class="sxs-lookup"><span data-stu-id="e6492-118">Click on **应用管理** (app management).</span></span>
3. <span data-ttu-id="e6492-119">Klikněte na**创建应用**(vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="e6492-119">Click on **创建应用** (create app).</span></span>
4. <span data-ttu-id="e6492-120">Zadejte informace potřebné aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6492-120">Enter the necessary app information.</span></span>
5. <span data-ttu-id="e6492-121">Klikněte na**创建应用**(vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="e6492-121">Click on **创建应用** (create app).</span></span>
6. <span data-ttu-id="e6492-122">Zadejte požadované informace.</span><span class="sxs-lookup"><span data-stu-id="e6492-122">Enter the required information.</span></span>
7. <span data-ttu-id="e6492-123">Pro**授权回调域**(zpětného volání URL) zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="e6492-123">For the **授权回调域** (callback URL) field, enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="e6492-124">Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, nastavte adresu URL jako `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="e6492-124">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
8. <span data-ttu-id="e6492-125">Klikněte na**创建应用**(vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="e6492-125">Click on **创建应用** (create app).</span></span>
9. <span data-ttu-id="e6492-126">Na stránce pro potvrzení klikněte na**应用管理**(Správa aplikací) se vrátíte na stránku správy aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6492-126">On the confirmation page, click on **应用管理** (app management) to return to the app management page.</span></span>
10. <span data-ttu-id="e6492-127">Klikněte na**查看**(Zobrazit) vedle aplikace, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e6492-127">Click on **查看** (view) next to the app you just created.</span></span>
11. <span data-ttu-id="e6492-128">Klikněte na**修改**(Upravit).</span><span class="sxs-lookup"><span data-stu-id="e6492-128">Click on **修改** (edit).</span></span>
12. <span data-ttu-id="e6492-129">Z horní části stránky, zkopírujte **ID aplikace** a **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e6492-129">From the top of the page, copy the **APP ID** and **APP KEY**.</span></span>

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="e6492-130">Konfigurace QQ jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="e6492-130">Configure QQ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="e6492-131">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e6492-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="e6492-132">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="e6492-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="e6492-133">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="e6492-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="e6492-134">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="e6492-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="e6492-135">Zadejte například "QQ".</span><span class="sxs-lookup"><span data-stu-id="e6492-135">For example, enter "QQ".</span></span>
5. <span data-ttu-id="e6492-136">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **QQ**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6492-136">Click **Identity provider type**, select **QQ**, and click **OK**.</span></span>
6. <span data-ttu-id="e6492-137">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**</span><span class="sxs-lookup"><span data-stu-id="e6492-137">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="e6492-138">Zadejte **klíč aplikace** který jste zkopírovali dříve, jako **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="e6492-138">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="e6492-139">Zadejte **tajný klíč aplikace** který jste zkopírovali dříve, jako **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="e6492-139">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="e6492-140">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložte konfiguraci QQ.</span><span class="sxs-lookup"><span data-stu-id="e6492-140">Click **OK** and then click **Create** to save your QQ configuration.</span></span>

