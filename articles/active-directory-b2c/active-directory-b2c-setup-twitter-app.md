---
title: "Azure Active Directory B2C: Konfigurace služby Twitter | Microsoft Docs"
description: "Registrace a přihlášení tooconsumers poskytněte Twitter účty v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 275c5c73fd5e8e5075e77fee942cbc1b5e1586cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-twitter-accounts"></a><span data-ttu-id="eeae5-103">Azure Active Directory B2C: Zadejte tooconsumers registrace a přihlášení s účty služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="eeae5-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Twitter accounts</span></span>

> [!NOTE]
> <span data-ttu-id="eeae5-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="eeae5-104">This feature is in preview.</span></span>
> 

## <a name="create-a-twitter-application"></a><span data-ttu-id="eeae5-105">Vytvořit aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="eeae5-105">Create a Twitter application</span></span>
<span data-ttu-id="eeae5-106">toouse Twitter jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba toocreate aplikace služby Twitter a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="eeae5-106">toouse Twitter as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Twitter application and supply it with hello right parameters.</span></span> <span data-ttu-id="eeae5-107">Tuto funkci potřebujete toodo účtu vývojáře Twitter.</span><span class="sxs-lookup"><span data-stu-id="eeae5-107">You need a Twitter developer account toodo this.</span></span> <span data-ttu-id="eeae5-108">Pokud nemáte, můžete ho na získat [https://dev.twitter.com/](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="eeae5-108">If you don’t have one, you can get it at [https://dev.twitter.com/](https://dev.twitter.com/).</span></span>

1. <span data-ttu-id="eeae5-109">Přejděte toohello [Twitter webu pro vývojáře](https://dev.twitter.com/) a přihlaste se pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="eeae5-109">Go toohello [Twitter developer's website](https://dev.twitter.com/) and sign in with your credentials.</span></span>
2. <span data-ttu-id="eeae5-110">Klikněte na tlačítko **Moje aplikace** pod **nástroje & podporu** a pak klikněte na **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-110">Click **My apps** under **Tools & Support** and then click **Create New App**.</span></span> 
3. <span data-ttu-id="eeae5-111">Ve formuláři hello, zadejte hodnotu pro hello **název**, **popis**, a **webu**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-111">In hello form, provide a value for hello **Name**, **Description**, and **Website**.</span></span>
4. <span data-ttu-id="eeae5-112">Pro hello **adresu URL zpětné volání**, zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="eeae5-112">For hello **Callback URL**, enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span></span> <span data-ttu-id="eeae5-113">Ujistěte se, že tooreplace **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="eeae5-113">Make sure tooreplace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
5. <span data-ttu-id="eeae5-114">Zkontrolujte hello pole tooagree toohello **vývojáře smlouvy** a klikněte na tlačítko **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-114">Check hello box tooagree toohello **Developer Agreement** and click **Create your Twitter application**.</span></span>
6. <span data-ttu-id="eeae5-115">Po vytvoření aplikace hello klikněte na tlačítko **klíče a přístupové tokeny**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-115">Once hello app is created, click **Keys and Access Tokens**.</span></span>
7. <span data-ttu-id="eeae5-116">Zkopírujte hodnotu hello **uživatelský klíč** a **uživatelský tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-116">Copy hello value of **Consumer Key** and **Consumer Secret**.</span></span> <span data-ttu-id="eeae5-117">Budete potřebovat oba dva tooconfigure Twitter jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="eeae5-117">You will need both of them tooconfigure Twitter as an identity provider in your tenant.</span></span>

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="eeae5-118">Konfigurace služby Twitter jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="eeae5-118">Configure Twitter as an identity provider in your tenant</span></span>
1. <span data-ttu-id="eeae5-119">Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eeae5-119">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="eeae5-120">V okně funkce hello B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-120">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="eeae5-121">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="eeae5-121">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="eeae5-122">Zadejte popisný **název** pro konfiguraci poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="eeae5-122">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="eeae5-123">Zadejte například "Twitter".</span><span class="sxs-lookup"><span data-stu-id="eeae5-123">For example, enter "Twitter".</span></span>
5. <span data-ttu-id="eeae5-124">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Twitter**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-124">Click **Identity provider type**, select **Twitter**, and click **OK**.</span></span>
6. <span data-ttu-id="eeae5-125">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte hello Twitter **uživatelský klíč** pro hello **id klienta** a hello Twitter **uživatelský tajný klíč**pro hello **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="eeae5-125">Click **Set up this identity provider** and enter hello Twitter **Consumer Key** for hello **Client id** and hello Twitter **Consumer Secret** for hello **Client secret**.</span></span>
7. <span data-ttu-id="eeae5-126">Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** toosave konfiguraci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="eeae5-126">Click **OK**, and then click **Create** toosave your Twitter configuration.</span></span>

