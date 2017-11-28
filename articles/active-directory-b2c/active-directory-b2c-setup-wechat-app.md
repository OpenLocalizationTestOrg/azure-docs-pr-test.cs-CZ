---
title: 'Azure Active Directory B2C: Konfigurace WeChat | Microsoft Docs'
description: "Registrace a přihlášení tooconsumers poskytněte WeChat účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 92cc3579d818d2379a503ccc695138b33a34466d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-wechat-accounts"></a><span data-ttu-id="ffa84-103">Azure Active Directory B2C: Poskytnout účty WeChat tooconsumers registrace a přihlášení</span><span class="sxs-lookup"><span data-stu-id="ffa84-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with WeChat accounts</span></span>

> [!NOTE]
> <span data-ttu-id="ffa84-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="ffa84-104">This feature is in preview.</span></span>
> 

## <a name="create-a-wechat-application"></a><span data-ttu-id="ffa84-105">Vytvoření aplikace WeChat</span><span class="sxs-lookup"><span data-stu-id="ffa84-105">Create a WeChat application</span></span>

<span data-ttu-id="ffa84-106">toouse WeChat jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate WeChat aplikace a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="ffa84-106">toouse WeChat as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a WeChat application and supply it with hello right parameters.</span></span> <span data-ttu-id="ffa84-107">Tuto funkci potřebujete toodo WeChat účtu.</span><span class="sxs-lookup"><span data-stu-id="ffa84-107">You need a WeChat account toodo this.</span></span> <span data-ttu-id="ffa84-108">Pokud nemáte, můžete jej získat registrací prostřednictvím jednoho z jejich mobilních aplikací nebo pomocí své QQ číslo.</span><span class="sxs-lookup"><span data-stu-id="ffa84-108">If you don’t have one, you can get one by signing up through one of their mobile apps or by using your QQ number.</span></span> <span data-ttu-id="ffa84-109">Potom získáte vašeho účtu zaregistrovat pomocí programu pro vývojáře WeChat hello.</span><span class="sxs-lookup"><span data-stu-id="ffa84-109">After that, get your account registered with hello WeChat developer program.</span></span> <span data-ttu-id="ffa84-110">Další informace můžete najít [zde](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span><span class="sxs-lookup"><span data-stu-id="ffa84-110">You can find more information [here](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span></span>

### <a name="register-a-wechat-application"></a><span data-ttu-id="ffa84-111">Registrace aplikace WeChat</span><span class="sxs-lookup"><span data-stu-id="ffa84-111">Register a WeChat application</span></span>

1. <span data-ttu-id="ffa84-112">Přejděte příliš[https://open.weixin.qq.com/](https://open.weixin.qq.com/) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="ffa84-112">Go too[https://open.weixin.qq.com/](https://open.weixin.qq.com/) and log in.</span></span>
2. <span data-ttu-id="ffa84-113">Klikněte na**管理中心**(management center).</span><span class="sxs-lookup"><span data-stu-id="ffa84-113">Click on **管理中心** (management center).</span></span>
3. <span data-ttu-id="ffa84-114">Postupujte podle hello potřebné kroky tooregister novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ffa84-114">Follow hello necessary steps tooregister a new application.</span></span>
4. <span data-ttu-id="ffa84-115">Pro**授权回调域**(zpětného volání URL), zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="ffa84-115">For **授权回调域** (callback URL), enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="ffa84-116">Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, toobe adresu URL sady hello `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="ffa84-116">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
5. <span data-ttu-id="ffa84-117">Najít a zkopírujte hello **ID aplikace** a **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ffa84-117">Find and copy hello **APP ID** and **APP KEY**.</span></span> <span data-ttu-id="ffa84-118">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="ffa84-118">You will need these later.</span></span>

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="ffa84-119">Konfigurace WeChat jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="ffa84-119">Configure WeChat as an identity provider in your tenant</span></span>
1. <span data-ttu-id="ffa84-120">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ffa84-120">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="ffa84-121">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="ffa84-121">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="ffa84-122">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="ffa84-122">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="ffa84-123">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="ffa84-123">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="ffa84-124">Zadejte například "WeChat".</span><span class="sxs-lookup"><span data-stu-id="ffa84-124">For example, enter "WeChat".</span></span>
5. <span data-ttu-id="ffa84-125">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **WeChat**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffa84-125">Click **Identity provider type**, select **WeChat**, and click **OK**.</span></span>
6. <span data-ttu-id="ffa84-126">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**</span><span class="sxs-lookup"><span data-stu-id="ffa84-126">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="ffa84-127">Zadejte hello **klíč aplikace** který jste zkopírovali dříve jako hello **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="ffa84-127">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="ffa84-128">Zadejte hello **tajný klíč aplikace** který jste zkopírovali dříve jako hello **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="ffa84-128">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="ffa84-129">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** toosave WeChat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ffa84-129">Click **OK** and then click **Create** toosave your WeChat configuration.</span></span>

