---
title: "ověřování pomocí certifikátů aaaAzure služby Active Directory v systému iOS | Microsoft Docs"
description: "Další informace o hello Podporované scénáře a hello požadavky pro konfiguraci ověřování pomocí certifikátů v řešení se zařízení s iOS"
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
ms.openlocfilehash: 4486ff5239c2897b3bc187053f31d74807430301
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a><span data-ttu-id="f487b-103">Azure Active Directory ověřování prostřednictvím certifikátu v systému iOS</span><span class="sxs-lookup"><span data-stu-id="f487b-103">Azure Active Directory certificate-based authentication on iOS</span></span>

<span data-ttu-id="f487b-104">Ověřování pomocí certifikátů (CBA) vám umožní toobe službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online:</span><span class="sxs-lookup"><span data-stu-id="f487b-104">Certificate-based authentication (CBA) enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="f487b-105">Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="f487b-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="f487b-106">Klienti Exchange ActiveSync (EAS)</span><span class="sxs-lookup"><span data-stu-id="f487b-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="f487b-107">Konfigurace tato funkce eliminuje hello nutné tooenter uživatelské jméno a heslo kombinaci do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="f487b-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="f487b-108">Toto téma poskytuje hello Podporované scénáře a požadavky na hello CBA konfigurace pro zařízení s iOS(Android) pro uživatele klientů v Office 365 Enterprise, Business, Education, US Government, Čína, a plány Německu.</span><span class="sxs-lookup"><span data-stu-id="f487b-108">This topic provides you with hello requirements and hello supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>

<span data-ttu-id="f487b-109">Tato funkce je dostupná ve verzi preview v Office 365 US Government obrany a federální plány.</span><span class="sxs-lookup"><span data-stu-id="f487b-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>




## <a name="office-mobile-applications-support"></a><span data-ttu-id="f487b-110">Podpora mobilních aplikacích Office</span><span class="sxs-lookup"><span data-stu-id="f487b-110">Office mobile applications support</span></span>

| <span data-ttu-id="f487b-111">Aplikace</span><span class="sxs-lookup"><span data-stu-id="f487b-111">Apps</span></span> | <span data-ttu-id="f487b-112">Podpora</span><span class="sxs-lookup"><span data-stu-id="f487b-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="f487b-113">Azure Information Protection aplikace</span><span class="sxs-lookup"><span data-stu-id="f487b-113">Azure Information Protection app</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="f487b-115">Microsoft Teams</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="f487b-117">OneNote</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="f487b-119">OneDrive</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="f487b-121">Outlook</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-123">Mobilních aplikací Power BI</span><span class="sxs-lookup"><span data-stu-id="f487b-123">Power BI mobile apps</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-125">Skype pro firmy</span><span class="sxs-lookup"><span data-stu-id="f487b-125">Skype for Business</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-127">Aplikace Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="f487b-127">Word / Excel / PowerPoint</span></span> |![Zaškrtnout][1] |
| <span data-ttu-id="f487b-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="f487b-129">Yammer</span></span> |![Zaškrtnout][1] |


## <a name="requirements"></a><span data-ttu-id="f487b-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f487b-131">Requirements</span></span> 

<span data-ttu-id="f487b-132">Hello verze operačního systému zařízení musí být iOS 9 a novějším</span><span class="sxs-lookup"><span data-stu-id="f487b-132">hello device OS version must be iOS 9 and above</span></span> 

<span data-ttu-id="f487b-133">Federační server musí být nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="f487b-133">A federation server must be configured.</span></span>  

<span data-ttu-id="f487b-134">Microsoft Authenticator je vyžadována pro aplikace Office v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="f487b-134">Microsoft Authenticator is required for Office applications on iOS.</span></span>  

<span data-ttu-id="f487b-135">Pro Azure Active Directory toorevoke klientský certifikát musí mít tokenu služby AD FS hello hello následující deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="f487b-135">For Azure Active Directory toorevoke a client certificate, hello ADFS token must have hello following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="f487b-136">(hello sériové číslo certifikátu klienta hello)</span><span class="sxs-lookup"><span data-stu-id="f487b-136">(hello serial number of hello client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="f487b-137">(hello řetězec hello vystavitele certifikátu klienta hello)</span><span class="sxs-lookup"><span data-stu-id="f487b-137">(hello string for hello issuer of hello client certificate)</span></span> 

<span data-ttu-id="f487b-138">Azure Active Directory přidá tyto deklarace identity toohello obnovovací token, pokud jsou k dispozici v tokenu služby AD FS hello (nebo jiné tokenu SAML).</span><span class="sxs-lookup"><span data-stu-id="f487b-138">Azure Active Directory adds these claims toohello refresh token if they are available in hello ADFS token (or any other SAML token).</span></span> <span data-ttu-id="f487b-139">Pokud token obnovení hello potřebuje toobe ověřit, tyto informace jsou použité toocheck hello odvolání.</span><span class="sxs-lookup"><span data-stu-id="f487b-139">When hello refresh token needs toobe validated, this information is used toocheck hello revocation.</span></span> 

<span data-ttu-id="f487b-140">Jako osvědčený postup by měl aktualizovat hello služby AD FS chybové stránky s hello následující:</span><span class="sxs-lookup"><span data-stu-id="f487b-140">As a best practice, you should update hello ADFS error pages with hello following:</span></span>

* <span data-ttu-id="f487b-141">Hello požadavek pro instalaci hello Microsoft Authenticator v systému iOS</span><span class="sxs-lookup"><span data-stu-id="f487b-141">hello requirement for installing hello Microsoft Authenticator on iOS</span></span>
* <span data-ttu-id="f487b-142">Pokyny, jak tooget certifikát uživatele.</span><span class="sxs-lookup"><span data-stu-id="f487b-142">Instructions on how tooget a user certificate.</span></span> 

<span data-ttu-id="f487b-143">Další podrobnosti najdete v tématu [přizpůsobení stránek přihlášení AD FS hello](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="f487b-143">For more details, see [Customizing hello AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>

<span data-ttu-id="f487b-144">Některé aplikace Office (s povolené moderní ověřování) odeslat '*řádku = přihlášení*' tooAzure AD v jejich požadavku.</span><span class="sxs-lookup"><span data-stu-id="f487b-144">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ tooAzure AD in their request.</span></span> <span data-ttu-id="f487b-145">Ve výchozím nastavení, Azure AD to převede v požadavku tooADFS hello příliš '*wauth = usernamepassworduri*' (požádá ověřování U/P toodo služby AD FS) a "*wfresh = 0*' (požádá stavu jednotného přihlašování k tooignore služby AD FS a provést novou ověřování) .</span><span class="sxs-lookup"><span data-stu-id="f487b-145">By default, Azure AD translates this in hello request tooADFS too‘*wauth=usernamepassworduri*’ (asks ADFS toodo U/P auth) and ‘*wfresh=0*’ (asks ADFS tooignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="f487b-146">Pokud chcete, aby tooenable ověřování pomocí certifikátů pro tyto aplikace, je třeba hello toomodify se výchozí chování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f487b-146">If you want tooenable certificate-based authentication for these apps, you need toomodify hello default Azure AD behavior.</span></span> <span data-ttu-id="f487b-147">Právě sadu hello '*PromptLoginBehavior*' v nastaveních federovanou doménu příliš '*zakázané*'.</span><span class="sxs-lookup"><span data-stu-id="f487b-147">Just set hello ‘*PromptLoginBehavior*’ in your federated domain settings too‘*Disabled*‘.</span></span> <span data-ttu-id="f487b-148">Můžete použít hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) tooperform rutiny tento úkol:</span><span class="sxs-lookup"><span data-stu-id="f487b-148">You can use hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="f487b-149">Podporu klientů protokolu Exchange ActiveSync</span><span class="sxs-lookup"><span data-stu-id="f487b-149">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="f487b-150">V systému iOS 9 nebo novější je podporována hello nativní aplikace pro iOS e-mailového klienta.</span><span class="sxs-lookup"><span data-stu-id="f487b-150">On iOS 9 or later, hello native iOS mail client is supported.</span></span> <span data-ttu-id="f487b-151">Pro všechny ostatní aplikace Exchange ActiveSync toodetermine Pokud je tato funkce podporována, obraťte se na vývojáře vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f487b-151">For all other Exchange ActiveSync applications, toodetermine if this feature is supported, contact your application developer.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="f487b-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f487b-152">Next steps</span></span>

<span data-ttu-id="f487b-153">Pokud chcete ověřování pomocí certifikátů tooconfigure ve vašem prostředí, najdete v části [Začínáme s ověřováním na základě certifikátu v systému Android](active-directory-certificate-based-authentication-get-started.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="f487b-153">If you want tooconfigure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
