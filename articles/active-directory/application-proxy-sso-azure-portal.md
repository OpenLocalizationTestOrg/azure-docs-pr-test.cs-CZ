---
title: "aaaSingle přihlašování tooapps s proxy aplikace služby Azure AD | Microsoft Docs"
description: "Zapněte jednotné přihlašování pro aplikace publikované místní s Azure AD Application Proxy v hello portálu Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5ff288d36163b74215677d9e34c93c985ac33d54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a><span data-ttu-id="b135f-103">Heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="b135f-103">Password vaulting for single sign-on with Application Proxy</span></span>

<span data-ttu-id="b135f-104">Azure Proxy aplikace Active Directory vám pomůže zlepšit produktivitu tím, že publikujete místní aplikace tak, aby vzdálení zaměstnanci mít bezpečný přístup, je příliš.</span><span class="sxs-lookup"><span data-stu-id="b135f-104">Azure Active Directory Application Proxy helps you improve productivity by publishing on-premises applications so that remote employees can securely access them, too.</span></span> <span data-ttu-id="b135f-105">V hello portálu Azure můžete také nastavit jeden přihlašování (SSO) toothese aplikace.</span><span class="sxs-lookup"><span data-stu-id="b135f-105">In hello Azure portal, you can also set up single sign-on (SSO) toothese apps.</span></span> <span data-ttu-id="b135f-106">Uživatelé potřebují pouze tooauthenticate s Azure AD a přístupem podniková aplikace bez nutnosti toosign akci.</span><span class="sxs-lookup"><span data-stu-id="b135f-106">Your users only need tooauthenticate with Azure AD, and they can access your enterprise application without having toosign in again.</span></span>

<span data-ttu-id="b135f-107">Proxy aplikací podporuje několik [jednotné přihlašování režimy](application-proxy-sso-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b135f-107">Application Proxy supports several [single sign-on modes](application-proxy-sso-overview.md).</span></span> <span data-ttu-id="b135f-108">Založené na heslech přihlášení je určený pro aplikace, které používají kombinace uživatelského jména a hesla pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="b135f-108">Password-based sign-on is intended for applications that use a username/password combination for authentication.</span></span> <span data-ttu-id="b135f-109">Když konfigurujete založené na heslech přihlašování pro aplikace, uživatelé, aby toosign v jednou toohello místní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b135f-109">When you configure password-based sign-on for your application, your users have toosign in toohello on-premises application once.</span></span> <span data-ttu-id="b135f-110">Poté Azure Active Directory ukládá přihlašovací informace hello a automaticky poskytuje ji aplikace toohello Pokud vaši uživatelé k němu přístup vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="b135f-110">After that, Azure Active Directory stores hello sign-in information and automatically provides it toohello application when your users access it remotely.</span></span> 

<span data-ttu-id="b135f-111">Má už máte publikována a testování vaší aplikace pomocí Proxy aplikací.</span><span class="sxs-lookup"><span data-stu-id="b135f-111">You should already have published and tested your app with Application Proxy.</span></span> <span data-ttu-id="b135f-112">Pokud ne, postupujte podle kroků hello v [publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md) pak se vraťte se sem.</span><span class="sxs-lookup"><span data-stu-id="b135f-112">If not, follow hello steps in [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md) then come back here.</span></span> 

## <a name="set-up-password-vaulting-for-your-application"></a><span data-ttu-id="b135f-113">Nastavit heslo vaulting pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="b135f-113">Set up password vaulting for your application</span></span>

1. <span data-ttu-id="b135f-114">Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="b135f-114">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="b135f-115">Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b135f-115">Select **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="b135f-116">Hello seznamu vyberte hello aplikace, které chcete tooset si pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b135f-116">From hello list, select hello app that you want tooset up with SSO.</span></span>  
4. <span data-ttu-id="b135f-117">Vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b135f-117">Select **Single sign-on**.</span></span>

   ![Vybrat jednotné přihlašování](./media/application-proxy-sso-azure-portal/select-sso.png)

5. <span data-ttu-id="b135f-119">Režim jednotné přihlašování hello zvolte **založené na heslech přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b135f-119">For hello SSO mode, choose **Password-based Sign-on**.</span></span>
6. <span data-ttu-id="b135f-120">Hello přihlašování adresy URL zadejte adresu URL hello pro stránku hello, kde uživatelé zadat svoje uživatelské jméno a heslo toosign tooyour aplikace mimo podnikovou síť hello.</span><span class="sxs-lookup"><span data-stu-id="b135f-120">For hello Sign-on URL, enter hello URL for hello page where users enter their username and password toosign in tooyour app outside of hello corporate network.</span></span> <span data-ttu-id="b135f-121">To může být hello externí adresu URL, kterou jste vytvořili při publikování aplikace hello prostřednictvím Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="b135f-121">This may be hello External URL that you created when you published hello app through Application Proxy.</span></span> 

   ![Zvolte založené na heslech přihlašování a zadejte adresu URL](./media/application-proxy-sso-azure-portal/password-sso.png)

7. <span data-ttu-id="b135f-123">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b135f-123">Select **Save**.</span></span>

<!-- Need toorepro?
7. hello page should tell you that a sign-in form was successfully detected at hello provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow hello instructions toopoint out where hello sign-in credentials go. 
-->

## <a name="test-your-app"></a><span data-ttu-id="b135f-124">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="b135f-124">Test your app</span></span>

<span data-ttu-id="b135f-125">Přejděte tooexternal adresu URL, kterou jste nakonfigurovali pro aplikace tooyour vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="b135f-125">Go tooexternal URL that you configured for remote access tooyour application.</span></span> <span data-ttu-id="b135f-126">Přihlaste se pomocí přihlašovacích údajů pro tuto aplikaci (nebo hello pověření pro účet testů, které jste nastavili s přístupem).</span><span class="sxs-lookup"><span data-stu-id="b135f-126">Sign in with your credentials for that app (or hello credentials for a test account that you set up with access).</span></span> <span data-ttu-id="b135f-127">Po přihlášení úspěšně, musí být schopný tooleave hello aplikace a vraťte bez opětovného zadávání přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b135f-127">Once you sign in successfully, you should be able tooleave hello app and come back without entering your credentials again.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b135f-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b135f-128">Next steps</span></span>

- <span data-ttu-id="b135f-129">Přečtěte si informace o dalších způsobů tooimplement [jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b135f-129">Read about other ways tooimplement [Single sign-on with Application Proxy](application-proxy-sso-overview.md)</span></span>
- <span data-ttu-id="b135f-130">Další informace o [důležité informace o zabezpečení pro přístup k aplikacím vzdáleně pomocí proxy aplikace služby Azure AD](application-proxy-security-considerations.md)</span><span class="sxs-lookup"><span data-stu-id="b135f-130">Learn about [Security considerations for accessing apps remotely with Azure AD Application Proxy](application-proxy-security-considerations.md)</span></span>