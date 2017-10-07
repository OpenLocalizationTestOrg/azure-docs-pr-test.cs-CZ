---
title: "aaaManage jednotné přihlašování pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Další informace o hello základy jednotné přihlašování pomocí Proxy aplikace"
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
ms.openlocfilehash: a278751a5cb1bf98c970a4e5d2eb3edc3b784096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a><span data-ttu-id="d6489-103">Jak Azure AD Application Proxy poskytovat jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="d6489-103">How does Azure AD Application Proxy provide single sign-on?</span></span>

<span data-ttu-id="d6489-104">Jednotné přihlašování je klíčovým prvkem proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d6489-104">Single sign-on is a key element of Azure AD Application Proxy.</span></span>  <span data-ttu-id="d6489-105">Poskytuje hello nejlepších výsledků, protože mají vaši uživatelé jenom toosign v tooAzure služby Active Directory v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="d6489-105">It provides hello best user experience because your users only have toosign in tooAzure Active Directory in hello cloud.</span></span> <span data-ttu-id="d6489-106">Po ověření tooAzure služby Active Directory, konektor Proxy aplikace hello zpracovává hello ověřování toohello místní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6489-106">Once they authenticate tooAzure Active Directory, hello Application Proxy connector handles hello authentication toohello on-premises application.</span></span> <span data-ttu-id="d6489-107">back-end aplikace Hello nelze zjistit hello rozdíl mezi vzdáleného uživatele přihlásit pomocí Proxy aplikací a regulární použití na zařízení připojeném k doméně.</span><span class="sxs-lookup"><span data-stu-id="d6489-107">hello backend application can't tell hello difference between a remote user signing in through Application Proxy and a regular use on a domain-joined device.</span></span> 

<span data-ttu-id="d6489-108">toouse Azure Active Directory pro tooyour přihlášení aplikace, budete potřebovat tooselect **Azure Active Directory** jako metoda předběžného ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="d6489-108">toouse Azure Active Directory for single sign-on tooyour applications, you need tooselect **Azure Active Directory** as hello pre-authentication method.</span></span> <span data-ttu-id="d6489-109">Pokud vyberete **průchozí** pak uživatelé nejsou vůbec ověření tooAzure služby Active Directory, ale jsou řízené přímých toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6489-109">If you select **Passthrough** then your users don't authenticate tooAzure Active Directory at all, but are directed straight toohello application.</span></span> <span data-ttu-id="d6489-110">Toto nastavení můžete nakonfigurovat při prvním publikování aplikace, nebo přejděte tooyour aplikace hello portál Azure a upravit nastavení Proxy aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d6489-110">You can configure this setting when you first publish an application, or navigate tooyour application in hello Azure portal and edit hello Application Proxy settings.</span></span> 

<span data-ttu-id="d6489-111">toosee jedné možnosti přihlašování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d6489-111">toosee your single sign-on options, follow these steps:</span></span>

1. <span data-ttu-id="d6489-112">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d6489-112">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d6489-113">Přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d6489-113">Navigate too**Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="d6489-114">Vyberte hello aplikace, jejichž jednotné přihlašování možnosti, můžete chtít toomanage.</span><span class="sxs-lookup"><span data-stu-id="d6489-114">Select hello app whose single sign-on options you want toomanage.</span></span>
4. <span data-ttu-id="d6489-115">Vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d6489-115">Select **Single sign-on**.</span></span>

   ![Jednotné přihlašování rozevírací nabídce](./media/application-proxy-sso-overview/single-sign-on-mode.png)

<span data-ttu-id="d6489-117">Hello rozevírací nabídce ukazuje pět možnosti pro aplikaci tooyour přihlašování:</span><span class="sxs-lookup"><span data-stu-id="d6489-117">hello dropdown menu shows five options for single sign-on tooyour application:</span></span>

* <span data-ttu-id="d6489-118">Azure AD jednotné přihlašování zakázáno</span><span class="sxs-lookup"><span data-stu-id="d6489-118">Azure AD single sign-on disabled</span></span>
* <span data-ttu-id="d6489-119">Založené na heslech přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6489-119">Password-based sign-on</span></span>
* <span data-ttu-id="d6489-120">Propojené přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6489-120">Linked sign-on</span></span>
* <span data-ttu-id="d6489-121">Integrované ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="d6489-121">Integrated Windows Authentication</span></span>
* <span data-ttu-id="d6489-122">Na základě záhlaví přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6489-122">Header-based sign-on</span></span>

## <a name="azure-ad-single-sign-on-disabled"></a><span data-ttu-id="d6489-123">Azure AD jednotné přihlašování zakázáno</span><span class="sxs-lookup"><span data-stu-id="d6489-123">Azure AD single sign-on disabled</span></span>

<span data-ttu-id="d6489-124">Pokud nechcete, aby integrace služby Active Directory Azure toouse pro tooyour přihlašování v aplikaci, vyberte **Azure AD jednotné přihlašování zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="d6489-124">If you don't want toouse Azure Active Directory integration for single sign-on tooyour application, choose **Azure AD single sign-on disabled**.</span></span> <span data-ttu-id="d6489-125">Tuto možnost mohou uživatelé ověřit dvakrát.</span><span class="sxs-lookup"><span data-stu-id="d6489-125">With this option selected, your users may authenticate twice.</span></span> <span data-ttu-id="d6489-126">Nejprve ověřit tooAzure služby Active Directory a potom se přihlaste toohello vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6489-126">First, they authenticate tooAzure Active Directory and then sign in toohello application itself.</span></span> 

<span data-ttu-id="d6489-127">Tato možnost je vhodná, pokud vaše místní aplikace nevyžaduje tooauthenticate uživatele, ale chcete tooadd Azure Active Directory jako úroveň zabezpečení pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="d6489-127">This option is a good choice if your on-premises application doesn't require users tooauthenticate, but you want tooadd Azure Active Directory as a layer of security for remote access.</span></span> 

## <a name="password-based-sign-on"></a><span data-ttu-id="d6489-128">Založené na heslech přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6489-128">Password-based sign-on</span></span>

<span data-ttu-id="d6489-129">Pokud chcete toouse Azure Active Directory jako služba vault heslo pro místní aplikace, vyberte **založené na heslech přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d6489-129">If you want toouse Azure Active Directory as a password vault for your on-premises applications, choose **Password-based sign-on**.</span></span> <span data-ttu-id="d6489-130">Tato možnost je vhodná, pokud vaše aplikace ověří uživatelské jméno a heslo pole se seznamem místo přístupové tokeny nebo hlavičky.</span><span class="sxs-lookup"><span data-stu-id="d6489-130">This option is a good choice if your application authenticates with a username/password combo instead of access tokens or headers.</span></span> <span data-ttu-id="d6489-131">S založené na heslech přihlašování uživatelé potřebovat toosign v toohello aplikace hello poprvé přístup.</span><span class="sxs-lookup"><span data-stu-id="d6489-131">With password-based sign-on, your users need toosign in toohello application hello first time they access it.</span></span> <span data-ttu-id="d6489-132">Potom poskytuje Azure Active Directory hello uživatelské jméno a heslo jménem uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d6489-132">After that, Azure Active Directory supplies hello username and password on behalf of hello user.</span></span> 

<span data-ttu-id="d6489-133">Informace o nastavení založené na heslech přihlašování najdete v tématu [heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d6489-133">For information about setting up password-based sign-on, see [Password vaulting for single sign-on with Application Proxy](application-proxy-sso-azure-portal.md).</span></span>

## <a name="linked-sign-on"></a><span data-ttu-id="d6489-134">Propojené přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6489-134">Linked sign-on</span></span>

<span data-ttu-id="d6489-135">Pokud již máte jeden řešení přihlašování nastavení pro místních identit, zvolte **propojené přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="d6489-135">If you already have a single sign-on solution set up for your on-premises identities, choose **Linked sign-on**.</span></span> <span data-ttu-id="d6489-136">Tato možnost umožňuje Azure Active Directory tooleverage existující jednotného přihlašování k řešení, ale stále bude uživatelům aplikace toohello vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="d6489-136">This option enables Azure Active Directory tooleverage existing SSO solutions, but still gives your users remote access toohello application.</span></span> 

<span data-ttu-id="d6489-137">Informace o propojené přihlášení (dříve označované jako existující jednotné přihlašování) najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span><span class="sxs-lookup"><span data-stu-id="d6489-137">For information about linked sign-on (formally known as existing single sign-on), see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span></span>

## <a name="integrated-windows-authentication"></a><span data-ttu-id="d6489-138">Integrované ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="d6489-138">Integrated Windows Authentication</span></span>

<span data-ttu-id="d6489-139">Pokud vaše místní aplikace používat integrované Authentication(IWA) Windows nebo pokud chcete toouse použitím (KCD Kerberos omezeného delegování) pro jednotné přihlašování, zvolte **integrované ověřování systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="d6489-139">If your on-premises applications use Integrated Windows Authentication(IWA) or if you want toouse Kerberos Constrained Delegation (KCD) for single sign-on, choose **Integrated Windows Authentication**.</span></span> <span data-ttu-id="d6489-140">Tato možnost uživatelé potřebují pouze tooauthenticate tooAzure služby Active Directory a potom konektor Proxy aplikace hello zosobňuje uživatele tooget hello token protokolu Kerberos a přihlaste se toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6489-140">With this option, your users only need tooauthenticate tooAzure Active Directory, and then hello Application Proxy connector impersonates hello user tooget a Kerberos token and sign in toohello application.</span></span> 

<span data-ttu-id="d6489-141">Informace o nastavení integrované ověřování systému Windows najdete v tématu [omezené delegování Kerberos pro jednotné přihlašování pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="d6489-141">For information about setting up Integrated Windows Authentication, see [Kerberos Constrained Delegation for single sign-on with Application Proxy](active-directory-application-proxy-sso-using-kcd.md).</span></span>

## <a name="header-based-sign-on"></a><span data-ttu-id="d6489-142">Na základě záhlaví přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6489-142">Header-based sign-on</span></span> 

<span data-ttu-id="d6489-143">Pokud vaše aplikace používat hlavičky pro ověřování, zvolte **na základě záhlaví přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d6489-143">If your applications use headers for authentication, choose **Header-based sign-on**.</span></span> <span data-ttu-id="d6489-144">Tato možnost uživatelé potřebovat pouze tooauthentication hello Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d6489-144">With this option, your users only need tooauthentication hello Azure Active Directory.</span></span> <span data-ttu-id="d6489-145">Partneři společnosti Microsoft službou ověřování třetích stran volá PingAccess, který přeložit hello Azure Active Directory přístupový token na formát hlavičky pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="d6489-145">Microsoft partners with a third-party authentication service called PingAccess, which translated hello Azure Active Directory access token into a header format for hello application.</span></span> 

<span data-ttu-id="d6489-146">Informace o nastavení ověřování na základě záhlaví najdete v tématu [ověřování na základě záhlaví pro jednotné přihlašování pomocí Proxy aplikace](application-proxy-ping-access.md).</span><span class="sxs-lookup"><span data-stu-id="d6489-146">For information about setting up header-based authentication, see [Header-based authentication for single sign-on with Application Proxy](application-proxy-ping-access.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6489-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6489-147">Next steps</span></span>

- [<span data-ttu-id="d6489-148">Heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d6489-148">Password vaulting for single sign-on with Application Proxy</span></span>](application-proxy-sso-azure-portal.md)
- [<span data-ttu-id="d6489-149">Omezené delegování Kerberos pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d6489-149">Kerberos Constrained Delegation for single sign-on with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
- [<span data-ttu-id="d6489-150">Ověřování na základě záhlaví pro jednotné přihlašování pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d6489-150">Header-based authentication for single sign-on with Application Proxy</span></span>](application-proxy-ping-access.md) 
