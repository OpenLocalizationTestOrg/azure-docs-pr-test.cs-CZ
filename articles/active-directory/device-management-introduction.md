---
title: "aaaIntroduction toodevice správy ve službě Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak Správa zařízení vám může pomoct tooget kontrolu nad hello zařízení, která přistupují k prostředkům ve vašem prostředí."
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
ms.openlocfilehash: e2fc0a3e8d00dc69cf01db9074e34427e396cfcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodevice-management-in-azure-active-directory"></a><span data-ttu-id="7ddd7-103">Úvod toodevice správy ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ddd7-103">Introduction toodevice management in Azure Active Directory</span></span>

<span data-ttu-id="7ddd7-104">První mobilní, cloudové první světě Azure Active Directory (Azure AD) umožňuje toodevices přihlašování, aplikacím a službám odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-104">In a mobile-first, cloud-first world, Azure Active Directory (Azure AD) enables single sign-on toodevices, apps, and services from anywhere.</span></span> <span data-ttu-id="7ddd7-105">S hello jak narůstá počet zařízení – PŘINESTE si vlastní zařízení (), včetně Odborníci v oblasti IT potýkají s dva dosáhnout cíle:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-105">With hello proliferation of devices - including Bring Your Own Device (BYOD), IT professionals are faced with two opposing goals:</span></span>

- <span data-ttu-id="7ddd7-106">Posílení produktivity toobe koncoví uživatelé hello kdykoli a kdekoli</span><span class="sxs-lookup"><span data-stu-id="7ddd7-106">Empower hello end users toobe productive wherever and whenever</span></span>
- <span data-ttu-id="7ddd7-107">Ochrana podnikových prostředků hello kdykoli</span><span class="sxs-lookup"><span data-stu-id="7ddd7-107">Protect hello corporate assets at any time</span></span>

<span data-ttu-id="7ddd7-108">Pomocí zařízení jsou vaši uživatelé získávání přístupu tooyour podnikových prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-108">Through devices, your users are getting access tooyour corporate assets.</span></span> <span data-ttu-id="7ddd7-109">tooprotect vaše podnikové prostředky, jako správce IT je vhodné toohave řízení těchto zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-109">tooprotect your corporate assets, as an IT administrator, you want toohave control over these devices.</span></span> <span data-ttu-id="7ddd7-110">To vám umožní toomake se, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-110">This enables you toomake sure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="7ddd7-111">Správa zařízení je také hello základ pro [podmíněného přístupu na základě zařízení](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="7ddd7-111">Device management is also hello foundation for [device-based conditional access](active-directory-conditional-access-policy-connected-applications.md).</span></span> <span data-ttu-id="7ddd7-112">Pomocí podmíněného přístupu podle zařízení můžete zajistit, aby přístupu, kterou tooresources ve vašem prostředí je možné jenom s důvěryhodných zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-112">With device-based conditional access, you can ensure that access tooresources in your environment is only possible with trusted devices.</span></span>   

<span data-ttu-id="7ddd7-113">Toto téma vysvětluje, jak funguje správa zařízení ve službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-113">This topic explains how device management in Azure Active Directory works.</span></span>

## <a name="getting-devices-under-hello-control-of-azure-ad"></a><span data-ttu-id="7ddd7-114">Získávání zařízení pod kontrolou hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ddd7-114">Getting devices under hello control of Azure AD</span></span>

<span data-ttu-id="7ddd7-115">tooget zařízení pod kontrolou hello Azure AD, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-115">tooget a device under hello control of Azure AD, you have two options:</span></span>

- <span data-ttu-id="7ddd7-116">Registrace</span><span class="sxs-lookup"><span data-stu-id="7ddd7-116">Registering</span></span> 
- <span data-ttu-id="7ddd7-117">Připojení</span><span class="sxs-lookup"><span data-stu-id="7ddd7-117">Joining</span></span>

<span data-ttu-id="7ddd7-118">**Registrace** zařízení tooAzure AD umožňuje vám toomanage identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-118">**Registering** a device tooAzure AD enables you toomanage a device’s identity.</span></span> <span data-ttu-id="7ddd7-119">Když je zařízení registrováno, poskytne mu nástroj registrace zařízení služby Azure AD hello zařízení s identitou, která je použité tooauthenticate hello zařízení při tooAzure přihlášení uživatele AD.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-119">When a device is registered, Azure AD device registration provides hello device with an identity that is used tooauthenticate hello device when a user signs-in tooAzure AD.</span></span> <span data-ttu-id="7ddd7-120">Můžete použít hello identity tooenable nebo zakázat zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-120">You can use hello identity tooenable or disable a device.</span></span>

<span data-ttu-id="7ddd7-121">V kombinaci s řešením správy mobilních zařízení, jako je například Microsoft Intune, hello atributy zařízení ve službě Azure AD jsou aktualizované o další informace o zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-121">When combined with a mobile device management(MDM) solution such as Microsoft Intune, hello device attributes in Azure AD are updated with additional information about hello device.</span></span> <span data-ttu-id="7ddd7-122">Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-122">This allows you toocreate conditional access rules that enforce access from devices toomeet your standards for security and compliance.</span></span> <span data-ttu-id="7ddd7-123">Další informace o registraci zařízení v Microsoft Intune najdete v tématu registrace zařízení pro správu v Intune.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-123">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune .</span></span>

<span data-ttu-id="7ddd7-124">**Připojení** zařízení je rozšíření tooregistering zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-124">**Joining** a device is an extension tooregistering a device.</span></span> <span data-ttu-id="7ddd7-125">To znamená, poskytne vám registrace zařízení a přidání toothis všechny výhody hello, změní také hello místní stav zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-125">This means, it provides you with all hello benefits of registering a device and in addition toothis, it also changes hello local state of a device.</span></span> <span data-ttu-id="7ddd7-126">Změna stavu místní hello umožňuje tooa toosign v zařízení uživatele pomocí organizace pracovní nebo školní účet místo osobní účet.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-126">Changing hello local state enables your users toosign-in tooa device using an organizational work or school account instead of a personal account.</span></span>

## <a name="azure-ad-registered-devices"></a><span data-ttu-id="7ddd7-127">Azure AD registrované zařízení</span><span class="sxs-lookup"><span data-stu-id="7ddd7-127">Azure AD registered devices</span></span>   

<span data-ttu-id="7ddd7-128">Hello cílem zařízení zaregistrovat Azure AD je s podporovaných pro hello tooprovide **PŘINESTE si vlastní zařízení ()** scénář.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-128">hello goal of Azure AD registered devices is tooprovide you with support for hello **Bring Your Own Device (BYOD)** scenario.</span></span> <span data-ttu-id="7ddd7-129">V tomto scénáři může uživatel přístup k prostředkům vaší organizace Azure Active Directory řídí použití osobních zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-129">In this scenario, a user can access your organization’s Azure Active Directory controlled resources using a personal device.</span></span>  

![Azure AD registrované zařízení](./media/device-management-introduction/03.png)

<span data-ttu-id="7ddd7-131">Hello přístup je založen na pracovní nebo školní účet, který byl zadán v zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-131">hello access is based on a work or school account that has been entered on hello device.</span></span>  
<span data-ttu-id="7ddd7-132">Například Windows 10 umožňuje uživatelům tooadd pracovní nebo školní účet tooa osobní počítače, tabletu nebo telefonu.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-132">For example, Windows 10 enables users tooadd a work or school account tooa personal computer, tablet, or phone.</span></span>  
<span data-ttu-id="7ddd7-133">Když uživatel přidal pracovního nebo školního účtu, zařízení hello je zaregistrován s Azure AD a volitelně zaregistrované v systému pro správu (MDM) mobilních zařízení hello nakonfigurovaný vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-133">When a user has added a work or school account, hello device is registered with Azure AD and optionally enrolled in hello mobile device management (MDM) system that your organization has configured.</span></span> <span data-ttu-id="7ddd7-134">Uživatelé vaší organizace můžete přidat pracovní nebo školní účet tooa osobní zařízení pohodlně:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-134">Your organization’s users can add a work or school account tooa personal device conveniently:</span></span>

- <span data-ttu-id="7ddd7-135">Při přístupu k aplikaci pracovní pro hello nejprve čas.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-135">When accessing a work application for hello first time</span></span>
- <span data-ttu-id="7ddd7-136">Ručně prostřednictvím hello **nastavení** nabídky v případě hello Windows 10</span><span class="sxs-lookup"><span data-stu-id="7ddd7-136">Manually via hello **Settings** menu in hello case of Windows 10</span></span> 

<span data-ttu-id="7ddd7-137">Pro Windows 10, iOS, Android a systému macOS můžete konfigurovat Azure AD, které registrované zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-137">You can configure Azure AD registered devices for Windows 10, iOS, Android and macOS.</span></span>

## <a name="azure-ad-joined-devices"></a><span data-ttu-id="7ddd7-138">Zařízení připojená k Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ddd7-138">Azure AD joined devices</span></span>

<span data-ttu-id="7ddd7-139">cílem Hello zařízení připojených k Azure AD je toosimplify:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-139">hello goal of Azure AD joined devices is toosimplify:</span></span>

- <span data-ttu-id="7ddd7-140">Zařízení ve vlastnictví pracovní nasazení systému Windows</span><span class="sxs-lookup"><span data-stu-id="7ddd7-140">Windows deployments of work-owned devices</span></span> 
- <span data-ttu-id="7ddd7-141">Přístup k tooorganizational aplikacím a prostředkům z libovolného zařízení Windows</span><span class="sxs-lookup"><span data-stu-id="7ddd7-141">Access tooorganizational apps and resources from any Windows device</span></span>

![Azure AD registrované zařízení](./media/device-management-introduction/02.png)


<span data-ttu-id="7ddd7-143">Jsou těchto cílů dosáhnout tím, že poskytuje uživatelům samoobslužné služby prostředí pro získání zařízení ve vlastnictví pracovní pod kontrolou hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-143">These goals are accomplished by providing your users with a self-service experience for getting work-owned devices under hello control of Azure AD.</span></span>  
<span data-ttu-id="7ddd7-144">**Azure AD Join** je určená pro organizace, které jsou cloudu první / jenom pro cloud.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-144">**Azure AD Join** is intended for organizations that are cloud-first / cloud-only.</span></span> <span data-ttu-id="7ddd7-145">Obvykle jsou to malé a střední firmy, které nemají místní infrastrukturu Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-145">These are typically small- and medium-sized businesses that do not have an on-premises Windows Server Active Directory infrastructure.</span></span> 

<span data-ttu-id="7ddd7-146">Implementace služby Azure AD, které jsou připojené k zařízení vám poskytne hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-146">Implementing Azure AD joined devices provides you with hello following benefits:</span></span>

- <span data-ttu-id="7ddd7-147">**Jednotného přihlašování (SSO)** tooyour Azure spravované SaaS aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-147">**Single-Sign-On (SSO)** tooyour Azure managed SaaS apps and services.</span></span> <span data-ttu-id="7ddd7-148">Vaši uživatelé nevidíte další ověřování výzvy při přístupu k pracovním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-148">Your users don’t see additional authentication prompts when accessing work resources.</span></span> <span data-ttu-id="7ddd7-149">Hello funkce jednotného přihlašování je i když nejsou připojené toohello doménové síti dostupné.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-149">hello SSO functionality is even when they are not connected toohello domain network available.</span></span>

- <span data-ttu-id="7ddd7-150">**Enterprise kompatibilní roaming** uživatelská nastavení napříč připojená.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-150">**Enterprise compliant roaming** of user settings across joined devices.</span></span> <span data-ttu-id="7ddd7-151">Uživatelé nepotřebují tooconnect nastavení toosee účtu (například Hotmail) Microsoft mezi zařízeními.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-151">Users don’t need tooconnect a Microsoft account (for example, Hotmail) toosee settings across devices.</span></span>

- <span data-ttu-id="7ddd7-152">**Přístup k tooWindows Store pro firmy** účtem AD.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-152">**Access tooWindows Store for Business** using AD account.</span></span> <span data-ttu-id="7ddd7-153">Uživatele můžete vybrat z inventář aplikací předem vybraná hello organizací.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-153">Your users can choose from an inventory of applications pre-selected by hello organization.</span></span>

- <span data-ttu-id="7ddd7-154">**Windows Hello** podpora pro přístup k zabezpečení a pohodlný toowork prostředkům.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-154">**Windows Hello** support for secure and convenient access toowork resources.</span></span>

- <span data-ttu-id="7ddd7-155">**Omezení přístupu** tooapps z jenom zařízení, které splňují zásady dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-155">**Restriction of access** tooapps from only devices that meet compliance policy.</span></span>

<span data-ttu-id="7ddd7-156">Při připojení k Azure AD je primárně určený pro organizace, které nemají místní infrastrukturu Windows Server Active Directory, jistě můžete také použít ve scénářích kde:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-156">While Azure AD join is primarily intended for organizations that do not have an on-premises Windows Server Active Directory infrastructure, you can certainly also use it in scenarios where:</span></span>

- <span data-ttu-id="7ddd7-157">Připojení k místní doméně nelze použít například, pokud potřebujete tooget mobilní zařízení, jako jsou tablety a telefony pod kontrolou.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-157">You can’t use an on-premises domain join, for example, if you need tooget mobile devices such as tablets and phones under control.</span></span>

- <span data-ttu-id="7ddd7-158">Uživatelé potřebovat především tooaccess Office 365 nebo jiné aplikace SaaS integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-158">Your users primarily need tooaccess Office 365 or other SaaS apps integrated with Azure AD.</span></span>

- <span data-ttu-id="7ddd7-159">Chcete toomanage skupinu uživatelů ve službě Azure AD místo ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-159">You want toomanage a group of users in Azure AD instead of in Active Directory.</span></span> <span data-ttu-id="7ddd7-160">To můžete použít například pracovníci tooseasonal, dodavatelů nebo studenty.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-160">This can apply, for example, tooseasonal workers, contractors, or students.</span></span>

- <span data-ttu-id="7ddd7-161">Chcete tooprovide spojující možnosti tooworkers ve firemní pobočky ve vzdálených pobočkách s omezenou na místní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-161">You want tooprovide joining capabilities tooworkers in remote branch offices with limited on-premises infrastructure.</span></span>

<span data-ttu-id="7ddd7-162">Můžete nakonfigurovat zařízení připojených k Azure AD pro zařízení s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-162">You can configure Azure AD joined devices for Windows 10 devices.</span></span>


## <a name="hybrid-azure-ad-joined-devices"></a><span data-ttu-id="7ddd7-163">Zařízení připojená k hybridní Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ddd7-163">Hybrid Azure AD joined devices</span></span>

<span data-ttu-id="7ddd7-164">Pro více než deset použili řada organizací hello domény spojení tootheir místní služby Active Directory tooenable:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-164">For more than a decade, many organizations have used hello domain join tootheir on-premises Active Directory tooenable:</span></span>

- <span data-ttu-id="7ddd7-165">IT oddělení toomanage pracovní zařízení ve vlastnictví z centrálního umístění.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-165">IT departments toomanage work-owned devices from a central location.</span></span>

- <span data-ttu-id="7ddd7-166">Uživatelé toosign v tootheir zařízení s jejich služby Active Directory pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-166">Users toosign in tootheir devices with their Active Directory work or school accounts.</span></span> 

<span data-ttu-id="7ddd7-167">Obvykle organizace s nároky místní spoléhají na zařízení tooprovision metody pro zpracování obrázků a často používají **System Center Configuration Manager (SCCM)** nebo **zásady (zásady skupiny) skupiny** toomanage je.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-167">Typically, organizations with an on-premises footprint rely on imaging methods tooprovision devices, and they often use **System Center Configuration Manager (SCCM)** or **group policy (GP)** toomanage them.</span></span>

<span data-ttu-id="7ddd7-168">Pokud vaše prostředí disponuje místní AD nároky a vy chcete také výhody z hello možnosti poskytované službou Azure Active Directory, můžete implementovat hybridní Azure AD, které jsou připojené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-168">If your environment has an on-premises AD footprint and you also want benefit from hello capabilities provided by Azure Active Directory, you can implement hybrid Azure AD joined devices.</span></span> <span data-ttu-id="7ddd7-169">Jedná se o zařízení, které jsou obě, připojený k tooyour místní služby Active Directory a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-169">These are devices that are both, joined tooyour on-premises Active Directory and your Azure Active Directory.</span></span>

![Azure AD registrované zařízení](./media/device-management-introduction/01.png)


<span data-ttu-id="7ddd7-171">Pokud byste měli používat Azure AD hybridní připojené k zařízení:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-171">You should use Azure AD hybrid joined devices if:</span></span>

- <span data-ttu-id="7ddd7-172">Máte Win32 aplikace nasazené toothese zařízení, které používají protokol NTLM nebo Kerberos.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-172">You have Win32 apps deployed toothese devices that use NTLM / Kerberos.</span></span>

- <span data-ttu-id="7ddd7-173">Vyžadujete, zásady skupiny nebo SCCM nebo DCM toomanage zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-173">You require GP or SCCM / DCM toomanage devices.</span></span>

- <span data-ttu-id="7ddd7-174">Chcete toocontinue toouse vytváření bitové kopie řešení tooconfigure zařízení pro vaši zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-174">You want toocontinue toouse imaging solutions tooconfigure devices for your employees.</span></span>

<span data-ttu-id="7ddd7-175">Můžete nakonfigurovat hybridní Azure AD pro Windows 10 a zařízení nižší úrovně, jako jsou Windows 8 a Windows 7 připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ddd7-175">You can configure Hybrid Azure AD joined devices for Windows 10 and down-level devices such as Windows 8 and Windows 7.</span></span>

## <a name="summary"></a><span data-ttu-id="7ddd7-176">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7ddd7-176">Summary</span></span>

<span data-ttu-id="7ddd7-177">Správa zařízení ve službě Azure AD můžete:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-177">With device management in Azure AD, you can:</span></span> 

- <span data-ttu-id="7ddd7-178">Zjednodušení hello uvedení zařízení pod kontrolou hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ddd7-178">Simplify hello process of bringing devices under hello control of Azure AD</span></span>

- <span data-ttu-id="7ddd7-179">Poskytovat uživatelům snadno toouse přístup tooyour organizace cloudové prostředky</span><span class="sxs-lookup"><span data-stu-id="7ddd7-179">Provide your users with an easy toouse access tooyour organization’s cloud-based resources</span></span>

<span data-ttu-id="7ddd7-180">Jako pravidlo Flash měli byste použít:</span><span class="sxs-lookup"><span data-stu-id="7ddd7-180">As a rule of a thumb, you should use:</span></span>

- <span data-ttu-id="7ddd7-181">Azure AD registrované zařízení pro osobní zařízení</span><span class="sxs-lookup"><span data-stu-id="7ddd7-181">Azure AD registered devices for personal devices</span></span>

- <span data-ttu-id="7ddd7-182">Azure AD připojené zařízení pro zařízení, které nejsou připojené k tooan místní AD</span><span class="sxs-lookup"><span data-stu-id="7ddd7-182">Azure AD joined devices for devices that are not joined tooan on-premises AD</span></span> 

- <span data-ttu-id="7ddd7-183">Hybridní Azure AD připojené zařízení pro zařízení, které jsou připojené k tooan místní AD</span><span class="sxs-lookup"><span data-stu-id="7ddd7-183">Hybrid Azure AD joined devices for devices that are joined tooan on-premises AD</span></span>     




## <a name="next-steps"></a><span data-ttu-id="7ddd7-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ddd7-184">Next steps</span></span>

- <span data-ttu-id="7ddd7-185">tooget přehled o tom, jak hello toomanage zařízení v Azure portálu, najdete v tématu [Správa zařízení pomocí portálu Azure hello](device-management-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7ddd7-185">tooget an overview of how toomanage device in hello Azure portal, see [managing devices using hello Azure portal](device-management-azure-portal.md)</span></span>

- <span data-ttu-id="7ddd7-186">toolearn Další informace o podmíněného přístupu podle zařízení, najdete v části [konfigurovat zásady podmíněného přístupu na základě zařízení služby Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="7ddd7-186">toolearn more about device-based conditional access, see [configure Azure Active Directory device-based conditional access policies](active-directory-conditional-access-policy-connected-applications.md).</span></span>

- <span data-ttu-id="7ddd7-187">toosetup hybridní Azure AD, které jsou připojené k zařízení, najdete v části [jak zařízení připojená k hybridní tooconfigure Azure Active Directory](device-management-hybrid-azuread-joined-devices-setup.md).</span><span class="sxs-lookup"><span data-stu-id="7ddd7-187">toosetup hybrid Azure AD joined devices, see [how tooconfigure hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md).</span></span>


