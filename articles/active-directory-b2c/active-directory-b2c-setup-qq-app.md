---
title: 'Azure Active Directory B2C: Konfigurace QQ | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte QQ účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 896d6221e01d15de1652a5717cf1f65619101e0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-qq-accounts"></a><span data-ttu-id="8f33d-103">Azure Active Directory B2C: Poskytnout účty QQ tooconsumers registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="8f33d-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with QQ accounts</span></span>

> [!NOTE]
> <span data-ttu-id="8f33d-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="8f33d-104">This feature is in preview.</span></span>
> 

## <a name="create-a-qq-application"></a><span data-ttu-id="8f33d-105">Vytvoření aplikace QQ</span><span class="sxs-lookup"><span data-stu-id="8f33d-105">Create a QQ application</span></span>

<span data-ttu-id="8f33d-106">toouse QQ jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate QQ aplikace a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="8f33d-106">toouse QQ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a QQ application and supply it with hello right parameters.</span></span> <span data-ttu-id="8f33d-107">Tuto funkci potřebujete toodo QQ účtu.</span><span class="sxs-lookup"><span data-stu-id="8f33d-107">You need a QQ account toodo this.</span></span> <span data-ttu-id="8f33d-108">Pokud nemáte, můžete jej získat v [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span><span class="sxs-lookup"><span data-stu-id="8f33d-108">If you don’t have one, you can get one at [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span></span>

### <a name="register-for-hello-qq-developer-program"></a><span data-ttu-id="8f33d-109">Registrace pro programu pro vývojáře QQ hello</span><span class="sxs-lookup"><span data-stu-id="8f33d-109">Register for hello QQ developer program</span></span>

1. <span data-ttu-id="8f33d-110">Přejděte toohello [portál pro vývojáře QQ](http://open.qq.com) a přihlaste se pomocí přihlašovacích údajů účtu QQ.</span><span class="sxs-lookup"><span data-stu-id="8f33d-110">Go toohello [QQ developer portal](http://open.qq.com) and sign in with your QQ account credentials.</span></span>
2. <span data-ttu-id="8f33d-111">Po přihlášení, přejděte příliš[http://open.qq.com/reg](http://open.qq.com/reg) tooregister sami jako vývojář.</span><span class="sxs-lookup"><span data-stu-id="8f33d-111">After signing in, go too[http://open.qq.com/reg](http://open.qq.com/reg) tooregister yourself as a developer.</span></span>
3. <span data-ttu-id="8f33d-112">V nabídce hello vyberte**个人**(jednotlivé vývojáře).</span><span class="sxs-lookup"><span data-stu-id="8f33d-112">In hello menu, select **个人** (individual developer).</span></span>
4. <span data-ttu-id="8f33d-113">Zadejte do formuláře hello hello požadované informace a klikněte na tlačítko**下一步**(Další krok).</span><span class="sxs-lookup"><span data-stu-id="8f33d-113">Enter hello required information into hello form and click **下一步** (next step).</span></span>
5. <span data-ttu-id="8f33d-114">Dokončení procesu ověření e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="8f33d-114">Complete hello email verification process.</span></span>

> [!NOTE]
> <span data-ttu-id="8f33d-115">Je nutné toowait několik dní toobe schválení po registraci jako vývojář.</span><span class="sxs-lookup"><span data-stu-id="8f33d-115">You will need toowait a few days toobe approved after registering as a developer.</span></span> 

### <a name="register-a-qq-application"></a><span data-ttu-id="8f33d-116">Registrace aplikace QQ</span><span class="sxs-lookup"><span data-stu-id="8f33d-116">Register a QQ application</span></span>

1. <span data-ttu-id="8f33d-117">Přejděte příliš[https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span><span class="sxs-lookup"><span data-stu-id="8f33d-117">Go too[https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span></span>
2. <span data-ttu-id="8f33d-118">Klikněte na**应用管理**(Správa aplikací).</span><span class="sxs-lookup"><span data-stu-id="8f33d-118">Click on **应用管理** (app management).</span></span>
3. <span data-ttu-id="8f33d-119">Klikněte na**创建应用**(vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="8f33d-119">Click on **创建应用** (create app).</span></span>
4. <span data-ttu-id="8f33d-120">Zadejte informace potřebné aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8f33d-120">Enter hello necessary app information.</span></span>
5. <span data-ttu-id="8f33d-121">Klikněte na**创建应用**(vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="8f33d-121">Click on **创建应用** (create app).</span></span>
6. <span data-ttu-id="8f33d-122">Zadejte informace požadované hello.</span><span class="sxs-lookup"><span data-stu-id="8f33d-122">Enter hello required information.</span></span>
7. <span data-ttu-id="8f33d-123">Pro hello**授权回调域**(zpětného volání URL) zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="8f33d-123">For hello **授权回调域** (callback URL) field, enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="8f33d-124">Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, toobe adresu URL sady hello `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="8f33d-124">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
8. <span data-ttu-id="8f33d-125">Klikněte na**创建应用**(vytvořit aplikaci).</span><span class="sxs-lookup"><span data-stu-id="8f33d-125">Click on **创建应用** (create app).</span></span>
9. <span data-ttu-id="8f33d-126">Na stránce pro potvrzení hello, klikněte na**应用管理**stránce management (Správa aplikací) tooreturn toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f33d-126">On hello confirmation page, click on **应用管理** (app management) tooreturn toohello app management page.</span></span>
10. <span data-ttu-id="8f33d-127">Klikněte na**查看**(zobrazení) další toohello aplikaci jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8f33d-127">Click on **查看** (view) next toohello app you just created.</span></span>
11. <span data-ttu-id="8f33d-128">Klikněte na**修改**(Upravit).</span><span class="sxs-lookup"><span data-stu-id="8f33d-128">Click on **修改** (edit).</span></span>
12. <span data-ttu-id="8f33d-129">Z hello horní části stránky hello, zkopírujte hello **ID aplikace** a **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f33d-129">From hello top of hello page, copy hello **APP ID** and **APP KEY**.</span></span>

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="8f33d-130">Konfigurace QQ jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="8f33d-130">Configure QQ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="8f33d-131">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8f33d-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="8f33d-132">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="8f33d-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="8f33d-133">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="8f33d-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="8f33d-134">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="8f33d-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="8f33d-135">Zadejte například "QQ".</span><span class="sxs-lookup"><span data-stu-id="8f33d-135">For example, enter "QQ".</span></span>
5. <span data-ttu-id="8f33d-136">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **QQ**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f33d-136">Click **Identity provider type**, select **QQ**, and click **OK**.</span></span>
6. <span data-ttu-id="8f33d-137">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**</span><span class="sxs-lookup"><span data-stu-id="8f33d-137">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="8f33d-138">Zadejte hello **klíč aplikace** který jste zkopírovali dříve jako hello **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="8f33d-138">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="8f33d-139">Zadejte hello **tajný klíč aplikace** který jste zkopírovali dříve jako hello **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="8f33d-139">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="8f33d-140">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave QQ konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8f33d-140">Click **OK** and then click **Create** toosave your QQ configuration.</span></span>

