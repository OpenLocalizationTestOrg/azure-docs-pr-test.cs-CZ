---
title: "Správa jednotného přihlašování pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Další informace o základní informace o jednotné přihlašování pomocí Proxy aplikace"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1deb3d91049d45fe26791783e13bd23e0a7d9f95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a><span data-ttu-id="81eae-103">Jak Azure AD Application Proxy poskytovat jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="81eae-103">How does Azure AD Application Proxy provide single sign-on?</span></span>

<span data-ttu-id="81eae-104">Jednotné přihlašování je klíčovým prvkem proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="81eae-104">Single sign-on is a key element of Azure AD Application Proxy.</span></span>  <span data-ttu-id="81eae-105">Poskytuje nejlepších výsledků, protože pouze mají vaši uživatelé se přihlásit k Azure Active Directory v cloudu.</span><span class="sxs-lookup"><span data-stu-id="81eae-105">It provides the best user experience because your users only have to sign in to Azure Active Directory in the cloud.</span></span> <span data-ttu-id="81eae-106">Po ověření do Azure Active Directory, konektor Proxy aplikace zpracovává ověřování do aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="81eae-106">Once they authenticate to Azure Active Directory, the Application Proxy connector handles the authentication to the on-premises application.</span></span> <span data-ttu-id="81eae-107">Back-end aplikace nelze zjistit rozdíl mezi vzdáleného uživatele přihlásit pomocí Proxy aplikací a regulární použití na zařízení připojeném k doméně.</span><span class="sxs-lookup"><span data-stu-id="81eae-107">The backend application can't tell the difference between a remote user signing in through Application Proxy and a regular use on a domain-joined device.</span></span> 

<span data-ttu-id="81eae-108">Používání služby Azure Active Directory pro jednotné přihlašování pro vaše aplikace, budete muset vybrat možnost **Azure Active Directory** jako metoda předběžného ověřování.</span><span class="sxs-lookup"><span data-stu-id="81eae-108">To use Azure Active Directory for single sign-on to your applications, you need to select **Azure Active Directory** as the pre-authentication method.</span></span> <span data-ttu-id="81eae-109">Pokud vyberete **průchozí** pak nemáte ověření do Azure Active Directory na všechny uživatele, ale jsou směrované přímo k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81eae-109">If you select **Passthrough** then your users don't authenticate to Azure Active Directory at all, but are directed straight to the application.</span></span> <span data-ttu-id="81eae-110">Toto nastavení můžete nakonfigurovat při prvním publikování aplikace, nebo přejděte na aplikaci na portálu Azure a upravit nastavení Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="81eae-110">You can configure this setting when you first publish an application, or navigate to your application in the Azure portal and edit the Application Proxy settings.</span></span> 

<span data-ttu-id="81eae-111">Pokud chcete zobrazit vaše volby jednotného přihlašování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="81eae-111">To see your single sign-on options, follow these steps:</span></span>

1. <span data-ttu-id="81eae-112">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="81eae-112">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="81eae-113">Přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="81eae-113">Navigate to **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="81eae-114">Vyberte aplikaci, jejíž přihlašování volby jednotného chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="81eae-114">Select the app whose single sign-on options you want to manage.</span></span>
4. <span data-ttu-id="81eae-115">Vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="81eae-115">Select **Single sign-on**.</span></span>

   ![Jednotné přihlašování rozevírací nabídce](./media/application-proxy-sso-overview/single-sign-on-mode.png)

<span data-ttu-id="81eae-117">V rozevírací nabídce jsou pět možnosti pro jednotné přihlašování k vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="81eae-117">The dropdown menu shows five options for single sign-on to your application:</span></span>

* <span data-ttu-id="81eae-118">Azure AD jednotné přihlašování zakázáno</span><span class="sxs-lookup"><span data-stu-id="81eae-118">Azure AD single sign-on disabled</span></span>
* <span data-ttu-id="81eae-119">Založené na heslech přihlášení</span><span class="sxs-lookup"><span data-stu-id="81eae-119">Password-based sign-on</span></span>
* <span data-ttu-id="81eae-120">Propojené přihlášení</span><span class="sxs-lookup"><span data-stu-id="81eae-120">Linked sign-on</span></span>
* <span data-ttu-id="81eae-121">Integrované ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="81eae-121">Integrated Windows Authentication</span></span>
* <span data-ttu-id="81eae-122">Na základě záhlaví přihlášení</span><span class="sxs-lookup"><span data-stu-id="81eae-122">Header-based sign-on</span></span>

## <a name="azure-ad-single-sign-on-disabled"></a><span data-ttu-id="81eae-123">Azure AD jednotné přihlašování zakázáno</span><span class="sxs-lookup"><span data-stu-id="81eae-123">Azure AD single sign-on disabled</span></span>

<span data-ttu-id="81eae-124">Pokud nechcete použít integraci služby Azure Active Directory pro jednotné přihlašování k vaší aplikaci, vyberte **Azure AD jednotné přihlašování zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="81eae-124">If you don't want to use Azure Active Directory integration for single sign-on to your application, choose **Azure AD single sign-on disabled**.</span></span> <span data-ttu-id="81eae-125">Tuto možnost mohou uživatelé ověřit dvakrát.</span><span class="sxs-lookup"><span data-stu-id="81eae-125">With this option selected, your users may authenticate twice.</span></span> <span data-ttu-id="81eae-126">Nejprve ověření do Azure Active Directory a potom se přihlaste do vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="81eae-126">First, they authenticate to Azure Active Directory and then sign in to the application itself.</span></span> 

<span data-ttu-id="81eae-127">Tato možnost je vhodná, pokud vaše místní aplikace nevyžaduje uživatele bude možné ověřit, ale chcete přidat Azure Active Directory jako vrstva zabezpečení pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="81eae-127">This option is a good choice if your on-premises application doesn't require users to authenticate, but you want to add Azure Active Directory as a layer of security for remote access.</span></span> 

## <a name="password-based-sign-on"></a><span data-ttu-id="81eae-128">Založené na heslech přihlášení</span><span class="sxs-lookup"><span data-stu-id="81eae-128">Password-based sign-on</span></span>

<span data-ttu-id="81eae-129">Pokud chcete použít Azure Active Directory jako služba vault heslo pro místní aplikace, vyberte **založené na heslech přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="81eae-129">If you want to use Azure Active Directory as a password vault for your on-premises applications, choose **Password-based sign-on**.</span></span> <span data-ttu-id="81eae-130">Tato možnost je vhodná, pokud vaše aplikace ověří uživatelské jméno a heslo pole se seznamem místo přístupové tokeny nebo hlavičky.</span><span class="sxs-lookup"><span data-stu-id="81eae-130">This option is a good choice if your application authenticates with a username/password combo instead of access tokens or headers.</span></span> <span data-ttu-id="81eae-131">S založené na heslech přihlašování uživatelé potřebovat pro přihlášení k aplikaci první čas přístup.</span><span class="sxs-lookup"><span data-stu-id="81eae-131">With password-based sign-on, your users need to sign in to the application the first time they access it.</span></span> <span data-ttu-id="81eae-132">Potom Azure Active Directory poskytuje uživatelské jméno a heslo jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="81eae-132">After that, Azure Active Directory supplies the username and password on behalf of the user.</span></span> 

<span data-ttu-id="81eae-133">Informace o nastavení založené na heslech přihlašování najdete v tématu [heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="81eae-133">For information about setting up password-based sign-on, see [Password vaulting for single sign-on with Application Proxy](application-proxy-sso-azure-portal.md).</span></span>

## <a name="linked-sign-on"></a><span data-ttu-id="81eae-134">Propojené přihlášení</span><span class="sxs-lookup"><span data-stu-id="81eae-134">Linked sign-on</span></span>

<span data-ttu-id="81eae-135">Pokud již máte jeden řešení přihlašování nastavení pro místních identit, zvolte **propojené přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="81eae-135">If you already have a single sign-on solution set up for your on-premises identities, choose **Linked sign-on**.</span></span> <span data-ttu-id="81eae-136">Tato možnost umožňuje využít existující řešení jednotného přihlašování k Azure Active Directory, ale stále bude uživatelům vzdálený přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81eae-136">This option enables Azure Active Directory to leverage existing SSO solutions, but still gives your users remote access to the application.</span></span> 

<span data-ttu-id="81eae-137">Informace o propojené přihlášení (dříve označované jako existující jednotné přihlašování) najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span><span class="sxs-lookup"><span data-stu-id="81eae-137">For information about linked sign-on (formally known as existing single sign-on), see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span></span>

## <a name="integrated-windows-authentication"></a><span data-ttu-id="81eae-138">Integrované ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="81eae-138">Integrated Windows Authentication</span></span>

<span data-ttu-id="81eae-139">Pokud používáte integrované Authentication(IWA) Windows místním aplikacím nebo pokud chcete použít protokol Kerberos vynuceným delegování použitím (KCD) pro jednotné přihlašování, zvolte **integrované ověřování systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="81eae-139">If your on-premises applications use Integrated Windows Authentication(IWA) or if you want to use Kerberos Constrained Delegation (KCD) for single sign-on, choose **Integrated Windows Authentication**.</span></span> <span data-ttu-id="81eae-140">Tato možnost uživatelé potřebují pouze k ověření služby Azure Active Directory a potom konektor Proxy aplikace zosobňuje uživatele získat token protokolu Kerberos a přihlaste se k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81eae-140">With this option, your users only need to authenticate to Azure Active Directory, and then the Application Proxy connector impersonates the user to get a Kerberos token and sign in to the application.</span></span> 

<span data-ttu-id="81eae-141">Informace o nastavení integrované ověřování systému Windows najdete v tématu [omezené delegování Kerberos pro jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="81eae-141">For information about setting up Integrated Windows Authentication, see [Kerberos Constrained Delegation for single sign-on with Application Proxy](active-directory-application-proxy-sso-using-kcd.md).</span></span>

## <a name="header-based-sign-on"></a><span data-ttu-id="81eae-142">Na základě záhlaví přihlášení</span><span class="sxs-lookup"><span data-stu-id="81eae-142">Header-based sign-on</span></span> 

<span data-ttu-id="81eae-143">Pokud vaše aplikace používat hlavičky pro ověřování, zvolte **na základě záhlaví přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="81eae-143">If your applications use headers for authentication, choose **Header-based sign-on**.</span></span> <span data-ttu-id="81eae-144">Tato možnost uživatelé třeba pouze ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="81eae-144">With this option, your users only need to authentication the Azure Active Directory.</span></span> <span data-ttu-id="81eae-145">Partneři společnosti Microsoft službou ověřování třetích stran volá PingAccess, který přeložit přístupový token služby Azure Active Directory na formát hlavičky pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81eae-145">Microsoft partners with a third-party authentication service called PingAccess, which translated the Azure Active Directory access token into a header format for the application.</span></span> 

<span data-ttu-id="81eae-146">Informace o nastavení ověřování na základě záhlaví najdete v tématu [ověřování na základě záhlaví pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-ping-access.md).</span><span class="sxs-lookup"><span data-stu-id="81eae-146">For information about setting up header-based authentication, see [Header-based authentication for single sign-on with Application Proxy](application-proxy-ping-access.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="81eae-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81eae-147">Next steps</span></span>

- [<span data-ttu-id="81eae-148">Heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="81eae-148">Password vaulting for single sign-on with Application Proxy</span></span>](application-proxy-sso-azure-portal.md)
- [<span data-ttu-id="81eae-149">Omezené delegování Kerberos pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="81eae-149">Kerberos Constrained Delegation for single sign-on with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
- [<span data-ttu-id="81eae-150">Ověřování na základě záhlaví pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="81eae-150">Header-based authentication for single sign-on with Application Proxy</span></span>](application-proxy-ping-access.md) 
