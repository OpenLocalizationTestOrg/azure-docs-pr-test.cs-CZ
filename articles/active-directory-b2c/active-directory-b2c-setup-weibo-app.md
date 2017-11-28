---
title: 'Azure Active Directory B2C: Konfigurace Weibo | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte Weibo účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: c0648620f318046c1d9d24feb92a0c5f19c6a91a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-weibo-accounts"></a><span data-ttu-id="15af9-103">Azure Active Directory B2C: Poskytnout účty Weibo tooconsumers registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="15af9-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Weibo accounts</span></span>

> [!NOTE]
> <span data-ttu-id="15af9-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="15af9-104">This feature is in preview.</span></span> <span data-ttu-id="15af9-105">Nepoužívejte tohoto zprostředkovatele identity v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="15af9-105">Do not use this identity provider in your production environment.</span></span>
> 

## <a name="create-a-weibo-application"></a><span data-ttu-id="15af9-106">Vytvoření aplikace Weibo</span><span class="sxs-lookup"><span data-stu-id="15af9-106">Create a Weibo application</span></span>

<span data-ttu-id="15af9-107">toouse Weibo jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate Weibo aplikace a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="15af9-107">toouse Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Weibo application and supply it with hello right parameters.</span></span> <span data-ttu-id="15af9-108">Tuto funkci potřebujete toodo Weibo účtu.</span><span class="sxs-lookup"><span data-stu-id="15af9-108">You need a Weibo account toodo this.</span></span> <span data-ttu-id="15af9-109">Pokud nemáte, můžete jej získat v [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span><span class="sxs-lookup"><span data-stu-id="15af9-109">If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span></span>

### <a name="register-for-hello-weibo-developer-program"></a><span data-ttu-id="15af9-110">Registrace pro programu pro vývojáře Weibo hello</span><span class="sxs-lookup"><span data-stu-id="15af9-110">Register for hello Weibo developer program</span></span>

1. <span data-ttu-id="15af9-111">Přejděte toohello [portál pro vývojáře Weibo](http://open.weibo.com/) a přihlaste se pomocí přihlašovacích údajů účtu Weibo.</span><span class="sxs-lookup"><span data-stu-id="15af9-111">Go toohello [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.</span></span>
2. <span data-ttu-id="15af9-112">Po přihlášení, klikněte na název zobrazení v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="15af9-112">After signing in, click on your display name in hello top-right corner.</span></span>
3. <span data-ttu-id="15af9-113">V rozevírací nabídce hello, vyberte**编辑开发者信息**(Upravit informace pro vývojáře).</span><span class="sxs-lookup"><span data-stu-id="15af9-113">In hello dropdown, select **编辑开发者信息** (edit developer information).</span></span>
4. <span data-ttu-id="15af9-114">Zadejte do formuláře hello hello požadované informace a klikněte na tlačítko**提交**(Odeslat).</span><span class="sxs-lookup"><span data-stu-id="15af9-114">Enter hello required information into hello form and click **提交** (submit).</span></span>
5. <span data-ttu-id="15af9-115">Dokončení procesu ověření e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="15af9-115">Complete hello email verification process.</span></span>
6. <span data-ttu-id="15af9-116">Přejděte toohello [stránky ověření identit](http://open.weibo.com/developers/identity/edit).</span><span class="sxs-lookup"><span data-stu-id="15af9-116">Go toohello [identity verification page](http://open.weibo.com/developers/identity/edit).</span></span>
7. <span data-ttu-id="15af9-117">Zadejte do formuláře hello hello požadované informace a klikněte na tlačítko**提交**(Odeslat).</span><span class="sxs-lookup"><span data-stu-id="15af9-117">Enter hello required information into hello form and click **提交** (submit).</span></span>

### <a name="register-a-weibo-application"></a><span data-ttu-id="15af9-118">Registrace aplikace Weibo</span><span class="sxs-lookup"><span data-stu-id="15af9-118">Register a Weibo application</span></span>

1. <span data-ttu-id="15af9-119">Přejděte toohello [novou stránku registrace aplikace Weibo](http://open.weibo.com/apps/new).</span><span class="sxs-lookup"><span data-stu-id="15af9-119">Go toohello [new Weibo app registration page](http://open.weibo.com/apps/new).</span></span>
2. <span data-ttu-id="15af9-120">Zadejte informace potřebné aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="15af9-120">Enter hello necessary application information.</span></span>
3. <span data-ttu-id="15af9-121">Klikněte na tlačítko**创建**(vytvořit).</span><span class="sxs-lookup"><span data-stu-id="15af9-121">Click **创建** (create).</span></span>
4. <span data-ttu-id="15af9-122">Zkopírujte hodnoty hello **klíč aplikace** a **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="15af9-122">Copy hello values of **App Key** and **App Secret**.</span></span> <span data-ttu-id="15af9-123">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="15af9-123">You will need this later.</span></span>
5. <span data-ttu-id="15af9-124">Nahrání hello požadované fotografie a zadejte nezbytné informace hello.</span><span class="sxs-lookup"><span data-stu-id="15af9-124">Upload hello required photos and enter hello necessary information.</span></span>
6. <span data-ttu-id="15af9-125">Klikněte na tlačítko**保存以上信息**(Uložit).</span><span class="sxs-lookup"><span data-stu-id="15af9-125">Click **保存以上信息** (save).</span></span>
7. <span data-ttu-id="15af9-126">Klikněte na tlačítko**高级信息**(rozšířené informace).</span><span class="sxs-lookup"><span data-stu-id="15af9-126">Click **高级信息** (advanced information).</span></span>
8. <span data-ttu-id="15af9-127">Klikněte na tlačítko**编辑**(Upravit) další toohello pole pro OAuth2.0**授权设置**(přesměrování URL adresy).</span><span class="sxs-lookup"><span data-stu-id="15af9-127">Click **编辑** (edit) next toohello field for OAuth2.0 **授权设置** (redirect URL).</span></span>
9. <span data-ttu-id="15af9-128">Zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` pro OAuth2.0**授权设置**(přesměrování URL adresy).</span><span class="sxs-lookup"><span data-stu-id="15af9-128">Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL).</span></span> <span data-ttu-id="15af9-129">Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, toobe adresu URL sady hello `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="15af9-129">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
10. <span data-ttu-id="15af9-130">Klikněte na tlačítko**提交**(Odeslat).</span><span class="sxs-lookup"><span data-stu-id="15af9-130">Click **提交** (submit).</span></span>  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="15af9-131">Konfigurace Weibo jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="15af9-131">Configure Weibo as an identity provider in your tenant</span></span>
1. <span data-ttu-id="15af9-132">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15af9-132">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="15af9-133">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="15af9-133">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="15af9-134">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="15af9-134">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="15af9-135">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="15af9-135">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="15af9-136">Zadejte například "Weibo".</span><span class="sxs-lookup"><span data-stu-id="15af9-136">For example, enter "Weibo".</span></span>
5. <span data-ttu-id="15af9-137">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Weibo**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="15af9-137">Click **Identity provider type**, select **Weibo**, and click **OK**.</span></span>
6. <span data-ttu-id="15af9-138">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**</span><span class="sxs-lookup"><span data-stu-id="15af9-138">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="15af9-139">Zadejte hello **klíč aplikace** který jste zkopírovali dříve jako hello **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="15af9-139">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="15af9-140">Zadejte hello **tajný klíč aplikace** který jste zkopírovali dříve jako hello **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="15af9-140">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="15af9-141">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave Weibo konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="15af9-141">Click **OK** and then click **Create** toosave your Weibo configuration.</span></span>

