---
title: "Azure Active Directory ověřování prostřednictvím certifikátu v systému iOS | Microsoft Docs"
description: "Další informace o podporované scénáře a požadavky na konfiguraci ověřování pomocí certifikátů v řešeních s zařízení s iOS"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: 26a6fc54-0153-44fb-b970-9b432c99e9f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: c781f3f054fad5c5092fed5058c932fd4e97cf35
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a><span data-ttu-id="2d297-103">Azure Active Directory ověřování prostřednictvím certifikátu v systému iOS</span><span class="sxs-lookup"><span data-stu-id="2d297-103">Azure Active Directory certificate-based authentication on iOS</span></span>

<span data-ttu-id="2d297-104">Ověřování pomocí certifikátů (CBA) umožňuje službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online:</span><span class="sxs-lookup"><span data-stu-id="2d297-104">Certificate-based authentication (CBA) enables you to be authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="2d297-105">Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="2d297-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="2d297-106">Klienti Exchange ActiveSync (EAS)</span><span class="sxs-lookup"><span data-stu-id="2d297-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="2d297-107">Konfigurace tato funkce eliminuje potřebu zadejte kombinace uživatelského jména a hesla do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="2d297-107">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="2d297-108">Toto téma poskytuje požadavky a podporované scénáře konfigurace CBA pro zařízení s iOS(Android) pro uživatele klientů v Office 365 Enterprise, Business, Education, US Government, Čína, a plány Německu.</span><span class="sxs-lookup"><span data-stu-id="2d297-108">This topic provides you with the requirements and the supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>

<span data-ttu-id="2d297-109">Tato funkce je dostupná ve verzi preview v Office 365 US Government obrany a federální plány.</span><span class="sxs-lookup"><span data-stu-id="2d297-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>




## <a name="office-mobile-applications-support"></a><span data-ttu-id="2d297-110">Podpora mobilních aplikacích Office</span><span class="sxs-lookup"><span data-stu-id="2d297-110">Office mobile applications support</span></span>

| <span data-ttu-id="2d297-111">Aplikace</span><span class="sxs-lookup"><span data-stu-id="2d297-111">Apps</span></span> | <span data-ttu-id="2d297-112">Podpora</span><span class="sxs-lookup"><span data-stu-id="2d297-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="2d297-113">Azure Information Protection aplikace</span><span class="sxs-lookup"><span data-stu-id="2d297-113">Azure Information Protection app</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="2d297-115">Microsoft Teams</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="2d297-117">OneNote</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="2d297-119">OneDrive</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="2d297-121">Outlook</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-123">Mobilních aplikací Power BI</span><span class="sxs-lookup"><span data-stu-id="2d297-123">Power BI mobile apps</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-125">Skype pro firmy</span><span class="sxs-lookup"><span data-stu-id="2d297-125">Skype for Business</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-127">Aplikace Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="2d297-127">Word / Excel / PowerPoint</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="2d297-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="2d297-129">Yammer</span></span> |![Zaškrtnout][1] |


## <a name="requirements"></a><span data-ttu-id="2d297-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2d297-131">Requirements</span></span> 

<span data-ttu-id="2d297-132">Verze operačního systému zařízení musí být iOS 9 a novějším</span><span class="sxs-lookup"><span data-stu-id="2d297-132">The device OS version must be iOS 9 and above</span></span> 

<span data-ttu-id="2d297-133">Federační server musí být nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="2d297-133">A federation server must be configured.</span></span>  

<span data-ttu-id="2d297-134">Microsoft Authenticator je vyžadována pro aplikace Office v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="2d297-134">Microsoft Authenticator is required for Office applications on iOS.</span></span>  

<span data-ttu-id="2d297-135">Pro Azure Active Directory k odvolání certifikátu klienta musí mít tokenu služby AD FS následující deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="2d297-135">For Azure Active Directory to revoke a client certificate, the ADFS token must have the following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="2d297-136">(Sériové číslo certifikátu klienta)</span><span class="sxs-lookup"><span data-stu-id="2d297-136">(The serial number of the client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="2d297-137">(String pro vystavitele certifikátu klienta)</span><span class="sxs-lookup"><span data-stu-id="2d297-137">(The string for the issuer of the client certificate)</span></span> 

<span data-ttu-id="2d297-138">Azure Active Directory přidá tyto deklarace do tokenu obnovení, pokud jsou k dispozici v tokenu služby AD FS (nebo jiné tokenu SAML).</span><span class="sxs-lookup"><span data-stu-id="2d297-138">Azure Active Directory adds these claims to the refresh token if they are available in the ADFS token (or any other SAML token).</span></span> <span data-ttu-id="2d297-139">Pokud token obnovení, musí se ověřit, tyto informace slouží ke kontrole odvolání.</span><span class="sxs-lookup"><span data-stu-id="2d297-139">When the refresh token needs to be validated, this information is used to check the revocation.</span></span> 

<span data-ttu-id="2d297-140">Jako osvědčený postup by měl aktualizovat chybové stránky služby AD FS s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="2d297-140">As a best practice, you should update the ADFS error pages with the following:</span></span>

* <span data-ttu-id="2d297-141">Požadavek na instalaci Microsoft Authenticator v systému iOS</span><span class="sxs-lookup"><span data-stu-id="2d297-141">The requirement for installing the Microsoft Authenticator on iOS</span></span>
* <span data-ttu-id="2d297-142">Pokyny, jak získat certifikát uživatele.</span><span class="sxs-lookup"><span data-stu-id="2d297-142">Instructions on how to get a user certificate.</span></span> 

<span data-ttu-id="2d297-143">Další podrobnosti najdete v tématu [přizpůsobení stránek přihlášení AD FS](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="2d297-143">For more details, see [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>

<span data-ttu-id="2d297-144">Odeslat některé aplikace Office (s povolené moderní ověřování),*řádku = přihlášení*se do služby Azure AD v jejich požadavku.</span><span class="sxs-lookup"><span data-stu-id="2d297-144">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ to Azure AD in their request.</span></span> <span data-ttu-id="2d297-145">Ve výchozím nastavení, Azure AD to převede v požadavku na služby AD FS pro "*wauth = usernamepassworduri*' (požádá ADFS udělat ověřování U/P) a '*wfresh = 0*' (požádá ADFS ignorovat stavu jednotné přihlašování a provádět ověřování obnovit).</span><span class="sxs-lookup"><span data-stu-id="2d297-145">By default, Azure AD translates this in the request to ADFS to ‘*wauth=usernamepassworduri*’ (asks ADFS to do U/P auth) and ‘*wfresh=0*’ (asks ADFS to ignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="2d297-146">Pokud chcete povolit ověřování pomocí certifikátů pro tyto aplikace, budete muset upravit výchozí chování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d297-146">If you want to enable certificate-based authentication for these apps, you need to modify the default Azure AD behavior.</span></span> <span data-ttu-id="2d297-147">Stačí nastavit "*PromptLoginBehavior*"v nastavení federované domény k'*zakázané*'.</span><span class="sxs-lookup"><span data-stu-id="2d297-147">Just set the ‘*PromptLoginBehavior*’ in your federated domain settings to ‘*Disabled*‘.</span></span> <span data-ttu-id="2d297-148">Můžete použít [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) rutina k provedení této úlohy:</span><span class="sxs-lookup"><span data-stu-id="2d297-148">You can use the [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet to perform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="2d297-149">Podporu klientů protokolu Exchange ActiveSync</span><span class="sxs-lookup"><span data-stu-id="2d297-149">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="2d297-150">V systému iOS 9 nebo novější je podporovaná nativní aplikace pro iOS e-mailového klienta.</span><span class="sxs-lookup"><span data-stu-id="2d297-150">On iOS 9 or later, the native iOS mail client is supported.</span></span> <span data-ttu-id="2d297-151">Pro všechny ostatní aplikace Exchange ActiveSync Pokud chcete zjistit, pokud je tato funkce podporována, obraťte se na vývojáře vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d297-151">For all other Exchange ActiveSync applications, to determine if this feature is supported, contact your application developer.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="2d297-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d297-152">Next steps</span></span>

<span data-ttu-id="2d297-153">Pokud chcete nakonfigurovat ověřování pomocí certifikátů ve vašem prostředí, přečtěte si téma [Začínáme s ověřováním na základě certifikátu v systému Android](active-directory-certificate-based-authentication-get-started.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="2d297-153">If you want to configure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
