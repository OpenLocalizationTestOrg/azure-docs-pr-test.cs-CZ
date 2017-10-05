---
title: "Azure Active Directory B2C: Konfigurace služby Twitter | Microsoft Docs"
description: "Zadejte registrace a přihlášení k příjemce s účty služby Twitter v aplikacích, které jsou zabezpečené službou Azure Active Directory B2C."
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
ms.openlocfilehash: 82a001dd53cdddcf3b360090f3250af593c96fbb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a><span data-ttu-id="4010d-103">Azure Active Directory B2C: Zadejte registrace a přihlášení k příjemce s účty služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="4010d-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Twitter accounts</span></span>

> [!NOTE]
> <span data-ttu-id="4010d-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="4010d-104">This feature is in preview.</span></span>
> 

## <a name="create-a-twitter-application"></a><span data-ttu-id="4010d-105">Vytvořit aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="4010d-105">Create a Twitter application</span></span>
<span data-ttu-id="4010d-106">Pokud chcete používat služby Twitter jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci služby Twitter a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="4010d-106">To use Twitter as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Twitter application and supply it with the right parameters.</span></span> <span data-ttu-id="4010d-107">Twitter vývojářského účtu k tomu potřebujete.</span><span class="sxs-lookup"><span data-stu-id="4010d-107">You need a Twitter developer account to do this.</span></span> <span data-ttu-id="4010d-108">Pokud nemáte, můžete ho na získat [https://dev.twitter.com/](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="4010d-108">If you don’t have one, you can get it at [https://dev.twitter.com/](https://dev.twitter.com/).</span></span>

1. <span data-ttu-id="4010d-109">Přejděte na [Twitter webu pro vývojáře](https://dev.twitter.com/) a přihlaste se pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="4010d-109">Go to the [Twitter developer's website](https://dev.twitter.com/) and sign in with your credentials.</span></span>
2. <span data-ttu-id="4010d-110">Klikněte na tlačítko **Moje aplikace** pod **nástroje & podporu** a pak klikněte na **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="4010d-110">Click **My apps** under **Tools & Support** and then click **Create New App**.</span></span> 
3. <span data-ttu-id="4010d-111">Ve formuláři, zadejte hodnotu **název**, **popis**, a **webu**.</span><span class="sxs-lookup"><span data-stu-id="4010d-111">In the form, provide a value for the **Name**, **Description**, and **Website**.</span></span>
4. <span data-ttu-id="4010d-112">Pro **adresu URL zpětné volání**, zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="4010d-112">For the **Callback URL**, enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span></span> <span data-ttu-id="4010d-113">Nezapomeňte nahradit **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="4010d-113">Make sure to replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
5. <span data-ttu-id="4010d-114">Zaškrtněte políčko s **vývojáře smlouvy** a klikněte na tlačítko **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="4010d-114">Check the box to agree to the **Developer Agreement** and click **Create your Twitter application**.</span></span>
6. <span data-ttu-id="4010d-115">Po vytvoření aplikace, klikněte na tlačítko **klíče a přístupové tokeny**.</span><span class="sxs-lookup"><span data-stu-id="4010d-115">Once the app is created, click **Keys and Access Tokens**.</span></span>
7. <span data-ttu-id="4010d-116">Zkopírujte hodnotu **uživatelský klíč** a **uživatelský tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="4010d-116">Copy the value of **Consumer Key** and **Consumer Secret**.</span></span> <span data-ttu-id="4010d-117">Budete potřebovat oba dva ke konfiguraci služby Twitter jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="4010d-117">You will need both of them to configure Twitter as an identity provider in your tenant.</span></span>

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="4010d-118">Konfigurace služby Twitter jako zprostředkovatele identity ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="4010d-118">Configure Twitter as an identity provider in your tenant</span></span>
1. <span data-ttu-id="4010d-119">Postupujte podle těchto kroků [přejděte do okna s funkcemi B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4010d-119">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="4010d-120">Na okno s funkcemi B2C, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="4010d-120">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="4010d-121">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="4010d-121">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="4010d-122">Zadejte popisný **název** pro konfiguraci poskytovatele identity.</span><span class="sxs-lookup"><span data-stu-id="4010d-122">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="4010d-123">Zadejte například "Twitter".</span><span class="sxs-lookup"><span data-stu-id="4010d-123">For example, enter "Twitter".</span></span>
5. <span data-ttu-id="4010d-124">Klikněte na tlačítko **typ zprostředkovatele Identity**, vyberte **Twitter**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4010d-124">Click **Identity provider type**, select **Twitter**, and click **OK**.</span></span>
6. <span data-ttu-id="4010d-125">Klikněte na tlačítko **nastavení tohoto zprostředkovatele identity** a zadejte Twitteru **uživatelský klíč** pro **id klienta** a Twitteru **uživatelský tajný klíč** pro **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="4010d-125">Click **Set up this identity provider** and enter the Twitter **Consumer Key** for the **Client id** and the Twitter **Consumer Secret** for the **Client secret**.</span></span>
7. <span data-ttu-id="4010d-126">Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** uložte konfiguraci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="4010d-126">Click **OK**, and then click **Create** to save your Twitter configuration.</span></span>

