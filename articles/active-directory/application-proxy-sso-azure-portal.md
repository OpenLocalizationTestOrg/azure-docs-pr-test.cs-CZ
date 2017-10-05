---
title: "Jednotné přihlašování pro aplikace s proxy aplikace služby Azure AD | Microsoft Docs"
description: "Zapněte jedním přihlašování pro aplikace publikované místní s Azure AD Application Proxy na portálu Azure."
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
ms.openlocfilehash: 9ddc0c1bd5f2cbb24f6761cfd041b820ee6464b8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a><span data-ttu-id="47405-103">Heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="47405-103">Password vaulting for single sign-on with Application Proxy</span></span>

<span data-ttu-id="47405-104">Azure Proxy aplikace Active Directory vám pomůže zlepšit produktivitu tím, že publikujete místní aplikace tak, aby vzdálení zaměstnanci mít bezpečný přístup, je příliš.</span><span class="sxs-lookup"><span data-stu-id="47405-104">Azure Active Directory Application Proxy helps you improve productivity by publishing on-premises applications so that remote employees can securely access them, too.</span></span> <span data-ttu-id="47405-105">Na portálu Azure můžete také nastavíte jednotné přihlašování (SSO) pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="47405-105">In the Azure portal, you can also set up single sign-on (SSO) to these apps.</span></span> <span data-ttu-id="47405-106">Uživatelé potřebují pouze k ověření s Azure AD a přístupem podniková aplikace bez nutnosti znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="47405-106">Your users only need to authenticate with Azure AD, and they can access your enterprise application without having to sign in again.</span></span>

<span data-ttu-id="47405-107">Proxy aplikací podporuje několik [jednotné přihlašování režimy](application-proxy-sso-overview.md).</span><span class="sxs-lookup"><span data-stu-id="47405-107">Application Proxy supports several [single sign-on modes](application-proxy-sso-overview.md).</span></span> <span data-ttu-id="47405-108">Založené na heslech přihlášení je určený pro aplikace, které používají kombinace uživatelského jména a hesla pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="47405-108">Password-based sign-on is intended for applications that use a username/password combination for authentication.</span></span> <span data-ttu-id="47405-109">Když konfigurujete založené na heslech přihlašování pro aplikace, uživatelé musí přihlásit k aplikaci místně jednou.</span><span class="sxs-lookup"><span data-stu-id="47405-109">When you configure password-based sign-on for your application, your users have to sign in to the on-premises application once.</span></span> <span data-ttu-id="47405-110">Potom Azure Active Directory ukládá přihlašovací informace a automaticky poskytuje k aplikaci při vaši uživatelé k němu přístup vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="47405-110">After that, Azure Active Directory stores the sign-in information and automatically provides it to the application when your users access it remotely.</span></span> 

<span data-ttu-id="47405-111">Má už máte publikována a testování vaší aplikace pomocí Proxy aplikací.</span><span class="sxs-lookup"><span data-stu-id="47405-111">You should already have published and tested your app with Application Proxy.</span></span> <span data-ttu-id="47405-112">Pokud ne, postupujte podle kroků v [publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md) pak se vraťte se sem.</span><span class="sxs-lookup"><span data-stu-id="47405-112">If not, follow the steps in [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md) then come back here.</span></span> 

## <a name="set-up-password-vaulting-for-your-application"></a><span data-ttu-id="47405-113">Nastavit heslo vaulting pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="47405-113">Set up password vaulting for your application</span></span>

1. <span data-ttu-id="47405-114">Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="47405-114">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="47405-115">Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="47405-115">Select **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="47405-116">Ze seznamu vyberte aplikaci, kterou chcete nastavit pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="47405-116">From the list, select the app that you want to set up with SSO.</span></span>  
4. <span data-ttu-id="47405-117">Vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="47405-117">Select **Single sign-on**.</span></span>

   ![Vybrat jednotné přihlašování](./media/application-proxy-sso-azure-portal/select-sso.png)

5. <span data-ttu-id="47405-119">Pro režim jednotné přihlašování, zvolte **založené na heslech přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="47405-119">For the SSO mode, choose **Password-based Sign-on**.</span></span>
6. <span data-ttu-id="47405-120">Přihlášení adresy URL zadejte adresu URL pro stránku, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení do aplikace mimo firemní síť.</span><span class="sxs-lookup"><span data-stu-id="47405-120">For the Sign-on URL, enter the URL for the page where users enter their username and password to sign in to your app outside of the corporate network.</span></span> <span data-ttu-id="47405-121">To může být externí adresu URL, kterou jste vytvořili při publikování aplikace prostřednictvím Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="47405-121">This may be the External URL that you created when you published the app through Application Proxy.</span></span> 

   ![Zvolte založené na heslech přihlašování a zadejte adresu URL](./media/application-proxy-sso-azure-portal/password-sso.png)

7. <span data-ttu-id="47405-123">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="47405-123">Select **Save**.</span></span>

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a><span data-ttu-id="47405-124">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="47405-124">Test your app</span></span>

<span data-ttu-id="47405-125">Přejděte na externí adresu URL, kterou jste nakonfigurovali pro vzdálený přístup k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="47405-125">Go to external URL that you configured for remote access to your application.</span></span> <span data-ttu-id="47405-126">Přihlaste se pomocí přihlašovacích údajů pro tuto aplikaci (nebo pověření pro účet testů, které jste nastavili s přístupem).</span><span class="sxs-lookup"><span data-stu-id="47405-126">Sign in with your credentials for that app (or the credentials for a test account that you set up with access).</span></span> <span data-ttu-id="47405-127">Jakmile se přihlásíte úspěšně, byste měli mít opuštění aplikace a vraťte bez opětovného zadávání přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="47405-127">Once you sign in successfully, you should be able to leave the app and come back without entering your credentials again.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="47405-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47405-128">Next steps</span></span>

- <span data-ttu-id="47405-129">Přečtěte si informace o další způsoby, jak implementovat [jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-overview.md)</span><span class="sxs-lookup"><span data-stu-id="47405-129">Read about other ways to implement [Single sign-on with Application Proxy](application-proxy-sso-overview.md)</span></span>
- <span data-ttu-id="47405-130">Další informace o [důležité informace o zabezpečení pro přístup k aplikacím vzdáleně pomocí proxy aplikace služby Azure AD](application-proxy-security-considerations.md)</span><span class="sxs-lookup"><span data-stu-id="47405-130">Learn about [Security considerations for accessing apps remotely with Azure AD Application Proxy](application-proxy-security-considerations.md)</span></span>