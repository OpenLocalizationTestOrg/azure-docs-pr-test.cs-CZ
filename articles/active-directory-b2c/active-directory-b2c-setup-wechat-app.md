---
title: 'Azure Active Directory B2C: Konfigurace WeChat | Microsoft Docs'
description: "Zadejte registrace a přihlášení k příjemce s WeChat účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
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
ms.openlocfilehash: a54aec23d951610118246e9f70cdd27752ef39a6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a><span data-ttu-id="24e37-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s WeChat účty</span><span class="sxs-lookup"><span data-stu-id="24e37-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with WeChat accounts</span></span>

> [!NOTE]
> <span data-ttu-id="24e37-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="24e37-104">This feature is in preview.</span></span>
> 

## <a name="create-a-wechat-application"></a><span data-ttu-id="24e37-105">Vytvoření aplikace WeChat</span><span class="sxs-lookup"><span data-stu-id="24e37-105">Create a WeChat application</span></span>

<span data-ttu-id="24e37-106">Pokud chcete použít WeChat jako zprostředkovatele identity v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci WeChat a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="24e37-106">To use WeChat as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a WeChat application and supply it with the right parameters.</span></span> <span data-ttu-id="24e37-107">Potřebujete účet WeChat k tomu.</span><span class="sxs-lookup"><span data-stu-id="24e37-107">You need a WeChat account to do this.</span></span> <span data-ttu-id="24e37-108">Pokud nemáte, můžete jej získat registrací prostřednictvím jednoho z jejich mobilních aplikací nebo pomocí své QQ číslo.</span><span class="sxs-lookup"><span data-stu-id="24e37-108">If you don’t have one, you can get one by signing up through one of their mobile apps or by using your QQ number.</span></span> <span data-ttu-id="24e37-109">Potom získáte váš účet zaregistrována WeChat programu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="24e37-109">After that, get your account registered with the WeChat developer program.</span></span> <span data-ttu-id="24e37-110">Další informace můžete najít [zde](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span><span class="sxs-lookup"><span data-stu-id="24e37-110">You can find more information [here](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span></span>

### <a name="register-a-wechat-application"></a><span data-ttu-id="24e37-111">Registrace aplikace WeChat</span><span class="sxs-lookup"><span data-stu-id="24e37-111">Register a WeChat application</span></span>

1. <span data-ttu-id="24e37-112">Přejděte na [https://open.weixin.qq.com/](https://open.weixin.qq.com/) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="24e37-112">Go to [https://open.weixin.qq.com/](https://open.weixin.qq.com/) and log in.</span></span>
2. <span data-ttu-id="24e37-113">Klikněte na**管理中心**(management center).</span><span class="sxs-lookup"><span data-stu-id="24e37-113">Click on **管理中心** (management center).</span></span>
3. <span data-ttu-id="24e37-114">Postupujte podle potřeby kroky pro registraci novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="24e37-114">Follow the necessary steps to register a new application.</span></span>
4. <span data-ttu-id="24e37-115">Pro**授权回调域**(zpětného volání URL), zadejte `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="24e37-115">For **授权回调域** (callback URL), enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="24e37-116">Například pokud vaše `tenant_name` je contoso.onmicrosoft.com, nastavte adresu URL jako `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="24e37-116">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
5. <span data-ttu-id="24e37-117">Najít a zkopírovat **ID aplikace** a **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="24e37-117">Find and copy the **APP ID** and **APP KEY**.</span></span> <span data-ttu-id="24e37-118">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="24e37-118">You will need these later.</span></span>

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="24e37-119">Konfigurace WeChat jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="24e37-119">Configure WeChat as an identity provider in your tenant</span></span>
1. <span data-ttu-id="24e37-120">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="24e37-120">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="24e37-121">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="24e37-121">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="24e37-122">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="24e37-122">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="24e37-123">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="24e37-123">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="24e37-124">Zadejte například "WeChat".</span><span class="sxs-lookup"><span data-stu-id="24e37-124">For example, enter "WeChat".</span></span>
5. <span data-ttu-id="24e37-125">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **WeChat**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="24e37-125">Click **Identity provider type**, select **WeChat**, and click **OK**.</span></span>
6. <span data-ttu-id="24e37-126">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity**</span><span class="sxs-lookup"><span data-stu-id="24e37-126">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="24e37-127">Zadejte **klíč aplikace** který jste zkopírovali dříve, jako **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="24e37-127">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="24e37-128">Zadejte **tajný klíč aplikace** který jste zkopírovali dříve, jako **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="24e37-128">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="24e37-129">Klikněte na tlačítko **OK** a pak klikněte na **vytvořit** uložte konfiguraci WeChat.</span><span class="sxs-lookup"><span data-stu-id="24e37-129">Click **OK** and then click **Create** to save your WeChat configuration.</span></span>

