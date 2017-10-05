---
title: 'Azure Active Directory B2C: Konfigurace Weibo | Microsoft Docs'
description: "Zadejte registrace a přihlášení k příjemce s Weibo účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
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
ms.openlocfilehash: 00c5d3781455c80b33bdbb4c872ae354531baf3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a><span data-ttu-id="7f61c-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s Weibo účty</span><span class="sxs-lookup"><span data-stu-id="7f61c-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Weibo accounts</span></span>

> [!NOTE]
> <span data-ttu-id="7f61c-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="7f61c-104">This feature is in preview.</span></span> <span data-ttu-id="7f61c-105">Nepoužívejte tohoto zprostředkovatele identity v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f61c-105">Do not use this identity provider in your production environment.</span></span>
> 

## <a name="create-a-weibo-application"></a><span data-ttu-id="7f61c-106">Vytvoření aplikace Weibo</span><span class="sxs-lookup"><span data-stu-id="7f61c-106">Create a Weibo application</span></span>

<span data-ttu-id="7f61c-107">Pokud chcete použít Weibo jako zprostředkovatele identity v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci Weibo a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="7f61c-107">To use Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Weibo application and supply it with the right parameters.</span></span> <span data-ttu-id="7f61c-108">Potřebujete účet Weibo k tomu.</span><span class="sxs-lookup"><span data-stu-id="7f61c-108">You need a Weibo account to do this.</span></span> <span data-ttu-id="7f61c-109">Pokud nemáte, můžete jej získat v [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span><span class="sxs-lookup"><span data-stu-id="7f61c-109">If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span></span>

### <a name="register-for-the-weibo-developer-program"></a><span data-ttu-id="7f61c-110">Registrace pro Weibo programu pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="7f61c-110">Register for the Weibo developer program</span></span>

1. <span data-ttu-id="7f61c-111">Přejděte na [portál pro vývojáře Weibo](http://open.weibo.com/) a přihlaste se pomocí přihlašovacích údajů účtu Weibo.</span><span class="sxs-lookup"><span data-stu-id="7f61c-111">Go to the [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.</span></span>
2. <span data-ttu-id="7f61c-112">Po přihlášení, klikněte na název zobrazení v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="7f61c-112">After signing in, click on your display name in the top-right corner.</span></span>
3. <span data-ttu-id="7f61c-113">V rozevírací nabídce, a vyberte**编辑开发者信息**(Upravit informace pro vývojáře).</span><span class="sxs-lookup"><span data-stu-id="7f61c-113">In the dropdown, select **编辑开发者信息** (edit developer information).</span></span>
4. <span data-ttu-id="7f61c-114">Zadejte požadované informace do formuláře a klikněte na tlačítko**提交**(Odeslat).</span><span class="sxs-lookup"><span data-stu-id="7f61c-114">Enter the required information into the form and click **提交** (submit).</span></span>
5. <span data-ttu-id="7f61c-115">Dokončete proces ověření e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7f61c-115">Complete the email verification process.</span></span>
6. <span data-ttu-id="7f61c-116">Přejděte na [stránky ověření identit](http://open.weibo.com/developers/identity/edit).</span><span class="sxs-lookup"><span data-stu-id="7f61c-116">Go to the [identity verification page](http://open.weibo.com/developers/identity/edit).</span></span>
7. <span data-ttu-id="7f61c-117">Zadejte požadované informace do formuláře a klikněte na tlačítko**提交**(Odeslat).</span><span class="sxs-lookup"><span data-stu-id="7f61c-117">Enter the required information into the form and click **提交** (submit).</span></span>

### <a name="register-a-weibo-application"></a><span data-ttu-id="7f61c-118">Registrace aplikace Weibo</span><span class="sxs-lookup"><span data-stu-id="7f61c-118">Register a Weibo application</span></span>

1. <span data-ttu-id="7f61c-119">Přejděte na [novou stránku registrace aplikace Weibo](http://open.weibo.com/apps/new).</span><span class="sxs-lookup"><span data-stu-id="7f61c-119">Go to the [new Weibo app registration page](http://open.weibo.com/apps/new).</span></span>
2. <span data-ttu-id="7f61c-120">Zadejte informace potřebné aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f61c-120">Enter the necessary application information.</span></span>
3. <span data-ttu-id="7f61c-121">Klikněte na tlačítko**创建**(vytvořit).</span><span class="sxs-lookup"><span data-stu-id="7f61c-121">Click **创建** (create).</span></span>
4. <span data-ttu-id="7f61c-122">Zkopírujte hodnoty z **klíč aplikace** a **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f61c-122">Copy the values of **App Key** and **App Secret**.</span></span> <span data-ttu-id="7f61c-123">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="7f61c-123">You will need this later.</span></span>
5. <span data-ttu-id="7f61c-124">Nahrání požadované fotografie a zadejte nezbytné informace.</span><span class="sxs-lookup"><span data-stu-id="7f61c-124">Upload the required photos and enter the necessary information.</span></span>
6. <span data-ttu-id="7f61c-125">Klikněte na tlačítko**保存以上信息**(Uložit).</span><span class="sxs-lookup"><span data-stu-id="7f61c-125">Click **保存以上信息** (save).</span></span>
7. <span data-ttu-id="7f61c-126">Klikněte na tlačítko**高级信息**(rozšířené informace).</span><span class="sxs-lookup"><span data-stu-id="7f61c-126">Click **高级信息** (advanced information).</span></span>
8. <span data-ttu-id="7f61c-127">Klikněte na tlačítko**编辑**(Upravit) vedle pole pro OAuth2.0**授权设置**(přesměrování URL adresy).</span><span class="sxs-lookup"><span data-stu-id="7f61c-127">Click **编辑** (edit) next to the field for OAuth2.0 **授权设置** (redirect URL).</span></span>
9. <span data-ttu-id="7f61c-128">Zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` pro OAuth2.0**授权设置**(přesměrování URL adresy).</span><span class="sxs-lookup"><span data-stu-id="7f61c-128">Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL).</span></span> <span data-ttu-id="7f61c-129">Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, nastavte adresu URL jako `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="7f61c-129">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
10. <span data-ttu-id="7f61c-130">Klikněte na tlačítko**提交**(Odeslat).</span><span class="sxs-lookup"><span data-stu-id="7f61c-130">Click **提交** (submit).</span></span>  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="7f61c-131">Konfigurace Weibo jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="7f61c-131">Configure Weibo as an identity provider in your tenant</span></span>
1. <span data-ttu-id="7f61c-132">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7f61c-132">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="7f61c-133">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="7f61c-133">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="7f61c-134">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="7f61c-134">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="7f61c-135">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="7f61c-135">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="7f61c-136">Zadejte například "Weibo".</span><span class="sxs-lookup"><span data-stu-id="7f61c-136">For example, enter "Weibo".</span></span>
5. <span data-ttu-id="7f61c-137">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Weibo**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f61c-137">Click **Identity provider type**, select **Weibo**, and click **OK**.</span></span>
6. <span data-ttu-id="7f61c-138">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**</span><span class="sxs-lookup"><span data-stu-id="7f61c-138">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="7f61c-139">Zadejte **klíč aplikace** který jste zkopírovali dříve, jako **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="7f61c-139">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="7f61c-140">Zadejte **tajný klíč aplikace** který jste zkopírovali dříve, jako **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="7f61c-140">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="7f61c-141">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložte konfiguraci Weibo.</span><span class="sxs-lookup"><span data-stu-id="7f61c-141">Click **OK** and then click **Create** to save your Weibo configuration.</span></span>

