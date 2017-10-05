---
title: "Úvod do správy zařízení v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak Správa zařízení vám může pomoct získat kontrolu nad zařízení, která přistupují k prostředkům ve vašem prostředí."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: bebbdddf6b591ea7e36cbac38b568bce614bb335
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-device-management-in-azure-active-directory"></a><span data-ttu-id="1bd0a-103">Úvod do správy zařízení v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1bd0a-103">Introduction to device management in Azure Active Directory</span></span>

<span data-ttu-id="1bd0a-104">První mobilní, cloudové první světě Azure Active Directory (Azure AD) umožňuje jednotné přihlašování k zařízení, aplikacím a službám odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-104">In a mobile-first, cloud-first world, Azure Active Directory (Azure AD) enables single sign-on to devices, apps, and services from anywhere.</span></span> <span data-ttu-id="1bd0a-105">S jak narůstá počet zařízení – PŘINESTE si vlastní zařízení (), včetně Odborníci v oblasti IT potýkají s dva dosáhnout cíle:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-105">With the proliferation of devices - including Bring Your Own Device (BYOD), IT professionals are faced with two opposing goals:</span></span>

- <span data-ttu-id="1bd0a-106">Umožnit koncovým uživatelům k dosažení produktivity. kdykoli a kdekoli</span><span class="sxs-lookup"><span data-stu-id="1bd0a-106">Empower the end users to be productive wherever and whenever</span></span>
- <span data-ttu-id="1bd0a-107">Ochrana podnikových prostředků kdykoli</span><span class="sxs-lookup"><span data-stu-id="1bd0a-107">Protect the corporate assets at any time</span></span>

<span data-ttu-id="1bd0a-108">Pomocí zařízení jsou vaši uživatelé získání přístupu k firemním majetkem.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-108">Through devices, your users are getting access to your corporate assets.</span></span> <span data-ttu-id="1bd0a-109">Pokud chcete ochránit vaše podnikové prostředky, jako správce IT, budete chtít mít řízení těchto zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-109">To protect your corporate assets, as an IT administrator, you want to have control over these devices.</span></span> <span data-ttu-id="1bd0a-110">Díky tomu můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-110">This enables you to make sure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="1bd0a-111">Správa zařízení je také základem pro [podmíněného přístupu na základě zařízení](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="1bd0a-111">Device management is also the foundation for [device-based conditional access](active-directory-conditional-access-policy-connected-applications.md).</span></span> <span data-ttu-id="1bd0a-112">Pomocí podmíněného přístupu podle zařízení můžete zajistit, že přístup k prostředkům ve vašem prostředí je možné pouze s důvěryhodnými zařízeními.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-112">With device-based conditional access, you can ensure that access to resources in your environment is only possible with trusted devices.</span></span>   

<span data-ttu-id="1bd0a-113">Toto téma vysvětluje, jak funguje správa zařízení ve službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-113">This topic explains how device management in Azure Active Directory works.</span></span>

## <a name="getting-devices-under-the-control-of-azure-ad"></a><span data-ttu-id="1bd0a-114">Získávání zařízení pod kontrolou Azure AD</span><span class="sxs-lookup"><span data-stu-id="1bd0a-114">Getting devices under the control of Azure AD</span></span>

<span data-ttu-id="1bd0a-115">Chcete-li získat zařízení pod kontrolou Azure AD, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-115">To get a device under the control of Azure AD, you have two options:</span></span>

- <span data-ttu-id="1bd0a-116">Registrace</span><span class="sxs-lookup"><span data-stu-id="1bd0a-116">Registering</span></span> 
- <span data-ttu-id="1bd0a-117">Připojení</span><span class="sxs-lookup"><span data-stu-id="1bd0a-117">Joining</span></span>

<span data-ttu-id="1bd0a-118">**Registrace** zařízení do služby Azure AD umožňuje spravovat identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-118">**Registering** a device to Azure AD enables you to manage a device’s identity.</span></span> <span data-ttu-id="1bd0a-119">Když je zařízení registrováno, poskytne mu nástroj registrace zařízení služby Azure AD zařízení s identitou, která se používá k ověření zařízení, když se uživatel přihlásí ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-119">When a device is registered, Azure AD device registration provides the device with an identity that is used to authenticate the device when a user signs-in to Azure AD.</span></span> <span data-ttu-id="1bd0a-120">Identity můžete použít k povolení nebo zakázání zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-120">You can use the identity to enable or disable a device.</span></span>

<span data-ttu-id="1bd0a-121">V kombinaci s řešením správy mobilních zařízení, jako je například Microsoft Intune, budou atributy zařízení ve službě Azure AD jsou aktualizované o další informace o zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-121">When combined with a mobile device management(MDM) solution such as Microsoft Intune, the device attributes in Azure AD are updated with additional information about the device.</span></span> <span data-ttu-id="1bd0a-122">To vám umožňuje vytvořit pravidla podmíněného přístupu, která vynucují, aby přístup měla pouze taková zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů. </span><span class="sxs-lookup"><span data-stu-id="1bd0a-122">This allows you to create conditional access rules that enforce access from devices to meet your standards for security and compliance.</span></span> <span data-ttu-id="1bd0a-123">Další informace o registraci zařízení v Microsoft Intune najdete v tématu registrace zařízení pro správu v Intune.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-123">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune .</span></span>

<span data-ttu-id="1bd0a-124">**Připojení** zařízení je rozšíření pro registraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-124">**Joining** a device is an extension to registering a device.</span></span> <span data-ttu-id="1bd0a-125">To znamená, se poskytuje veškeré výhody registrace zařízení a kromě toho také změní stav místní zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-125">This means, it provides you with all the benefits of registering a device and in addition to this, it also changes the local state of a device.</span></span> <span data-ttu-id="1bd0a-126">Změna stavu místní umožňuje uživatelům přihlásit se do zařízení pomocí organizace pracovní nebo školní účet místo osobní účet.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-126">Changing the local state enables your users to sign-in to a device using an organizational work or school account instead of a personal account.</span></span>

## <a name="azure-ad-registered-devices"></a><span data-ttu-id="1bd0a-127">Azure AD registrované zařízení</span><span class="sxs-lookup"><span data-stu-id="1bd0a-127">Azure AD registered devices</span></span>   

<span data-ttu-id="1bd0a-128">Cílem zařízení zaregistrovat Azure AD je poskytnout podporu **PŘINESTE si vlastní zařízení ()** scénář.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-128">The goal of Azure AD registered devices is to provide you with support for the **Bring Your Own Device (BYOD)** scenario.</span></span> <span data-ttu-id="1bd0a-129">V tomto scénáři může uživatel přístup k prostředkům vaší organizace Azure Active Directory řídí použití osobních zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-129">In this scenario, a user can access your organization’s Azure Active Directory controlled resources using a personal device.</span></span>  

![Azure AD registrované zařízení](./media/device-management-introduction/03.png)

<span data-ttu-id="1bd0a-131">Přístup je založena na pracovní nebo školní účet, který byl zadán v zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-131">The access is based on a work or school account that has been entered on the device.</span></span>  
<span data-ttu-id="1bd0a-132">Například Windows 10 umožňuje uživatelům přidat pracovní nebo školní účet osobní počítače, tabletu nebo telefonu.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-132">For example, Windows 10 enables users to add a work or school account to a personal computer, tablet, or phone.</span></span>  
<span data-ttu-id="1bd0a-133">Když uživatel přidal pracovní nebo školní účet, zařízení je zaregistrované s Azure AD a volitelně zaregistrované v systému pro správu (MDM) mobilních zařízení, který byl nakonfigurován vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-133">When a user has added a work or school account, the device is registered with Azure AD and optionally enrolled in the mobile device management (MDM) system that your organization has configured.</span></span> <span data-ttu-id="1bd0a-134">Uživatelé vaší organizace můžete přidat pracovní nebo školní účet osobní zařízení pohodlně:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-134">Your organization’s users can add a work or school account to a personal device conveniently:</span></span>

- <span data-ttu-id="1bd0a-135">Při přístupu k aplikaci pracovní první</span><span class="sxs-lookup"><span data-stu-id="1bd0a-135">When accessing a work application for the first time</span></span>
- <span data-ttu-id="1bd0a-136">Ručně pomocí **nastavení** nabídky v případě systému Windows 10</span><span class="sxs-lookup"><span data-stu-id="1bd0a-136">Manually via the **Settings** menu in the case of Windows 10</span></span> 

<span data-ttu-id="1bd0a-137">Pro Windows 10, iOS, Android a systému macOS můžete konfigurovat Azure AD, které registrované zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-137">You can configure Azure AD registered devices for Windows 10, iOS, Android and macOS.</span></span>

## <a name="azure-ad-joined-devices"></a><span data-ttu-id="1bd0a-138">Zařízení připojená k Azure AD</span><span class="sxs-lookup"><span data-stu-id="1bd0a-138">Azure AD joined devices</span></span>

<span data-ttu-id="1bd0a-139">Pro zjednodušení je cílem Azure AD, které jsou připojené k zařízení:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-139">The goal of Azure AD joined devices is to simplify:</span></span>

- <span data-ttu-id="1bd0a-140">Zařízení ve vlastnictví pracovní nasazení systému Windows</span><span class="sxs-lookup"><span data-stu-id="1bd0a-140">Windows deployments of work-owned devices</span></span> 
- <span data-ttu-id="1bd0a-141">Přístup k organizační aplikacím a prostředkům z libovolného zařízení Windows</span><span class="sxs-lookup"><span data-stu-id="1bd0a-141">Access to organizational apps and resources from any Windows device</span></span>

![Azure AD registrované zařízení](./media/device-management-introduction/02.png)


<span data-ttu-id="1bd0a-143">Jsou těchto cílů dosáhnout tím, že poskytuje uživatelům samoobslužné služby prostředí pro získání zařízení ve vlastnictví pracovní řídí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-143">These goals are accomplished by providing your users with a self-service experience for getting work-owned devices under the control of Azure AD.</span></span>  
<span data-ttu-id="1bd0a-144">**Azure AD Join** je určená pro organizace, které jsou cloudu první / jenom pro cloud.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-144">**Azure AD Join** is intended for organizations that are cloud-first / cloud-only.</span></span> <span data-ttu-id="1bd0a-145">Obvykle jsou to malé a střední firmy, které nemají místní infrastrukturu Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-145">These are typically small- and medium-sized businesses that do not have an on-premises Windows Server Active Directory infrastructure.</span></span> 

<span data-ttu-id="1bd0a-146">Implementace zařízení připojených k Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-146">Implementing Azure AD joined devices provides you with the following benefits:</span></span>

- <span data-ttu-id="1bd0a-147">**Jednotného přihlašování (SSO)** vaše Azure spravovaných SaaS aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-147">**Single-Sign-On (SSO)** to your Azure managed SaaS apps and services.</span></span> <span data-ttu-id="1bd0a-148">Vaši uživatelé nevidíte další ověřování výzvy při přístupu k pracovním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-148">Your users don’t see additional authentication prompts when accessing work resources.</span></span> <span data-ttu-id="1bd0a-149">Funkce jednotného přihlašování je i když nejsou připojeni k síťové doméně, která je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-149">The SSO functionality is even when they are not connected to the domain network available.</span></span>

- <span data-ttu-id="1bd0a-150">**Enterprise kompatibilní roaming** uživatelská nastavení napříč připojená.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-150">**Enterprise compliant roaming** of user settings across joined devices.</span></span> <span data-ttu-id="1bd0a-151">Uživatelé nepotřebují k připojení k účtu Microsoft (například Hotmail) zobrazíte nastavení mezi zařízeními.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-151">Users don’t need to connect a Microsoft account (for example, Hotmail) to see settings across devices.</span></span>

- <span data-ttu-id="1bd0a-152">**Přístup k Windows Store pro firmy** účtem AD.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-152">**Access to Windows Store for Business** using AD account.</span></span> <span data-ttu-id="1bd0a-153">Uživatele můžete vybrat z inventář aplikací předem vybraná organizace.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-153">Your users can choose from an inventory of applications pre-selected by the organization.</span></span>

- <span data-ttu-id="1bd0a-154">**Windows Hello** podporu pro zabezpečení a pohodlný přístup k pracovním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-154">**Windows Hello** support for secure and convenient access to work resources.</span></span>

- <span data-ttu-id="1bd0a-155">**Omezení přístupu** k aplikacím z jenom zařízení, které splňují zásady dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-155">**Restriction of access** to apps from only devices that meet compliance policy.</span></span>

<span data-ttu-id="1bd0a-156">Při připojení k Azure AD je primárně určený pro organizace, které nemají místní infrastrukturu Windows Server Active Directory, jistě můžete také použít ve scénářích kde:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-156">While Azure AD join is primarily intended for organizations that do not have an on-premises Windows Server Active Directory infrastructure, you can certainly also use it in scenarios where:</span></span>

- <span data-ttu-id="1bd0a-157">Pokud potřebujete získat mobilní zařízení, jako jsou tablety a telefony pod kontrolou nelze například použít připojení k místní doméně.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-157">You can’t use an on-premises domain join, for example, if you need to get mobile devices such as tablets and phones under control.</span></span>

- <span data-ttu-id="1bd0a-158">Uživatelé se primárně potřebujete přístup k Office 365 nebo jiné aplikace SaaS integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-158">Your users primarily need to access Office 365 or other SaaS apps integrated with Azure AD.</span></span>

- <span data-ttu-id="1bd0a-159">Chcete spravovat skupinu uživatelů ve službě Azure AD místo ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-159">You want to manage a group of users in Azure AD instead of in Active Directory.</span></span> <span data-ttu-id="1bd0a-160">To můžete použít, například sezónních pracovníků, dodavatelů nebo studenty.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-160">This can apply, for example, to seasonal workers, contractors, or students.</span></span>

- <span data-ttu-id="1bd0a-161">Chcete poskytovat spojující možnosti pracovní procesy ve firemní pobočky ve vzdálených pobočkách s omezenou na místní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-161">You want to provide joining capabilities to workers in remote branch offices with limited on-premises infrastructure.</span></span>

<span data-ttu-id="1bd0a-162">Můžete nakonfigurovat zařízení připojených k Azure AD pro zařízení s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-162">You can configure Azure AD joined devices for Windows 10 devices.</span></span>


## <a name="hybrid-azure-ad-joined-devices"></a><span data-ttu-id="1bd0a-163">Zařízení připojená k hybridní Azure AD</span><span class="sxs-lookup"><span data-stu-id="1bd0a-163">Hybrid Azure AD joined devices</span></span>

<span data-ttu-id="1bd0a-164">Pro více než deset řada organizací použili umožňující připojení k doméně do svojí místní službě Active Directory:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-164">For more than a decade, many organizations have used the domain join to their on-premises Active Directory to enable:</span></span>

- <span data-ttu-id="1bd0a-165">Oddělení IT spravovat zařízení vlastněná pracovní z centrálního umístění.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-165">IT departments to manage work-owned devices from a central location.</span></span>

- <span data-ttu-id="1bd0a-166">Uživatelé budou moct přihlašovat ke svým zařízením s jejich služby Active Directory pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-166">Users to sign in to their devices with their Active Directory work or school accounts.</span></span> 

<span data-ttu-id="1bd0a-167">Obvykle organizace s nároky místní spoléhají na imaging metody zřízení zařízení a často používají **System Center Configuration Manager (SCCM)** nebo **zásady (zásady skupiny) skupiny** ke správě je.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-167">Typically, organizations with an on-premises footprint rely on imaging methods to provision devices, and they often use **System Center Configuration Manager (SCCM)** or **group policy (GP)** to manage them.</span></span>

<span data-ttu-id="1bd0a-168">Pokud vaše prostředí disponuje místní AD nároky a vy chcete také výhody z možnosti poskytované službou Azure Active Directory, můžete implementovat hybridní Azure AD, které jsou připojené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-168">If your environment has an on-premises AD footprint and you also want benefit from the capabilities provided by Azure Active Directory, you can implement hybrid Azure AD joined devices.</span></span> <span data-ttu-id="1bd0a-169">Jedná se o zařízení, které jsou obě, připojený k místní službě Active Directory a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-169">These are devices that are both, joined to your on-premises Active Directory and your Azure Active Directory.</span></span>

![Azure AD registrované zařízení](./media/device-management-introduction/01.png)


<span data-ttu-id="1bd0a-171">Pokud byste měli používat Azure AD hybridní připojené k zařízení:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-171">You should use Azure AD hybrid joined devices if:</span></span>

- <span data-ttu-id="1bd0a-172">Máte Win32 aplikace nasazené do těchto zařízení, které používají protokol NTLM nebo Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-172">You have Win32 apps deployed to these devices that use NTLM / Kerberos.</span></span>

- <span data-ttu-id="1bd0a-173">Vyžadujete, zásady skupiny nebo SCCM nebo DCM ke správě zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-173">You require GP or SCCM / DCM to manage devices.</span></span>

- <span data-ttu-id="1bd0a-174">Chcete nadále používat pro vytváření bitových kopií řešení pro konfiguraci zařízení pro vaši zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-174">You want to continue to use imaging solutions to configure devices for your employees.</span></span>

<span data-ttu-id="1bd0a-175">Můžete nakonfigurovat hybridní Azure AD pro Windows 10 a zařízení nižší úrovně, jako jsou Windows 8 a Windows 7 připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="1bd0a-175">You can configure Hybrid Azure AD joined devices for Windows 10 and down-level devices such as Windows 8 and Windows 7.</span></span>

## <a name="summary"></a><span data-ttu-id="1bd0a-176">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1bd0a-176">Summary</span></span>

<span data-ttu-id="1bd0a-177">Správa zařízení ve službě Azure AD můžete:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-177">With device management in Azure AD, you can:</span></span> 

- <span data-ttu-id="1bd0a-178">Zjednodušit proces přináší zařízení pod kontrolou Azure AD</span><span class="sxs-lookup"><span data-stu-id="1bd0a-178">Simplify the process of bringing devices under the control of Azure AD</span></span>

- <span data-ttu-id="1bd0a-179">Poskytovat uživatelům snadno použitelný přístup k prostředkům vaší organizace založená na cloudu</span><span class="sxs-lookup"><span data-stu-id="1bd0a-179">Provide your users with an easy to use access to your organization’s cloud-based resources</span></span>

<span data-ttu-id="1bd0a-180">Jako pravidlo Flash měli byste použít:</span><span class="sxs-lookup"><span data-stu-id="1bd0a-180">As a rule of a thumb, you should use:</span></span>

- <span data-ttu-id="1bd0a-181">Azure AD registrované zařízení pro osobní zařízení</span><span class="sxs-lookup"><span data-stu-id="1bd0a-181">Azure AD registered devices for personal devices</span></span>

- <span data-ttu-id="1bd0a-182">Zařízení pro zařízení, které nejsou připojené k místní připojená k Azure AD AD</span><span class="sxs-lookup"><span data-stu-id="1bd0a-182">Azure AD joined devices for devices that are not joined to an on-premises AD</span></span> 

- <span data-ttu-id="1bd0a-183">Hybridní Azure AD připojené zařízení pro zařízení, které jsou připojeny k místní AD</span><span class="sxs-lookup"><span data-stu-id="1bd0a-183">Hybrid Azure AD joined devices for devices that are joined to an on-premises AD</span></span>     




## <a name="next-steps"></a><span data-ttu-id="1bd0a-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1bd0a-184">Next steps</span></span>

- <span data-ttu-id="1bd0a-185">Chcete-li získat přehled o tom, jak spravovat zařízení na portálu Azure, přečtěte si téma [Správa zařízení pomocí portálu Azure](device-management-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="1bd0a-185">To get an overview of how to manage device in the Azure portal, see [managing devices using the Azure portal](device-management-azure-portal.md)</span></span>

- <span data-ttu-id="1bd0a-186">Další informace o podmíněného přístupu podle zařízení, najdete v části [konfigurovat zásady podmíněného přístupu na základě zařízení služby Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="1bd0a-186">To learn more about device-based conditional access, see [configure Azure Active Directory device-based conditional access policies](active-directory-conditional-access-policy-connected-applications.md).</span></span>

- <span data-ttu-id="1bd0a-187">Nastavení hybridní Azure AD zařízení připojená k najdete v tématu [jak ke konfiguraci hybridního připojená k Azure Active Directory zařízení](device-management-hybrid-azuread-joined-devices-setup.md).</span><span class="sxs-lookup"><span data-stu-id="1bd0a-187">To setup hybrid Azure AD joined devices, see [how to configure hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md).</span></span>


