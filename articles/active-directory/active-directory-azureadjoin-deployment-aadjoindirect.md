---
title: "Scénáře využití a aspekty nasazení pro připojení ke službě Azure AD | Microsoft Docs"
description: "Vysvětluje, jak mohou správci nastavit službu Azure AD Join pro své koncové uživatele (zaměstnanci, studenty, ostatní uživatelé). Popisuje také různé reálných scénářů pro použití připojení ke službě Azure AD."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: fd0aab1a14bbd324e734e5efe8fe101e8a8dfefa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="d2097-104">Scénáře využití a aspekty nasazení pro Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="d2097-104">Usage scenarios and deployment considerations for Azure AD Join</span></span>
## <a name="usage-scenarios-for-azure-ad-join"></a><span data-ttu-id="d2097-105">Scénáře použití pro připojení ke službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2097-105">Usage scenarios for Azure AD Join</span></span>
### <a name="scenario-1-businesses-largely-in-the-cloud"></a><span data-ttu-id="d2097-106">Scénář 1: Podnikům do značné míry v cloudu</span><span class="sxs-lookup"><span data-stu-id="d2097-106">Scenario 1: Businesses largely in the cloud</span></span>
<span data-ttu-id="d2097-107">Azure Active Directory Join (Azure AD Join) vám může hodit Pokud aktuálně fungovat a Správa identit pro vaši firmu v cloudu nebo se brzy přechodu do cloudu.</span><span class="sxs-lookup"><span data-stu-id="d2097-107">Azure Active Directory Join (Azure AD Join) can benefit you if you currently operate and manage identities for your business in the cloud or are moving to the cloud soon.</span></span> <span data-ttu-id="d2097-108">Můžete použít účet, který jste vytvořili ve službě Azure AD pro přihlášení k Windows 10.</span><span class="sxs-lookup"><span data-stu-id="d2097-108">You can use an account that you have created in Azure AD to sign in to Windows 10.</span></span> <span data-ttu-id="d2097-109">Prostřednictvím [během prvního spuštění prostředí procesu (FRX)](active-directory-azureadjoin-user-frx.md), nebo díky připojení ke službě Azure AD z [nabídky nastavení](active-directory-azureadjoin-user-upgrade.md), uživatelé se můžete zapojit své počítače ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2097-109">Through [the first run experience (FRX) process](active-directory-azureadjoin-user-frx.md), or by joining Azure AD from [the settings menu](active-directory-azureadjoin-user-upgrade.md), your users can join their machines to Azure AD.</span></span>  <span data-ttu-id="d2097-110">Uživatelé mohou také získejte (jeden přihlašování SSO) přístup k prostředkům cloudu třeba Office 365 ve svém prohlížeči zakázanou nebo v aplikacích Office.</span><span class="sxs-lookup"><span data-stu-id="d2097-110">Your users can also enjoy single sign-on (SSO) access to  cloud resources like Office 365, either in their browsers or in Office applications.</span></span>

### <a name="scenario-2-educational-institutions"></a><span data-ttu-id="d2097-111">Scénář 2: Vzdělávací instituce</span><span class="sxs-lookup"><span data-stu-id="d2097-111">Scenario 2: Educational institutions</span></span>
<span data-ttu-id="d2097-112">Vzdělávací instituce obvykle mají dva typy uživatelů: zaměstnance školy a studenti.</span><span class="sxs-lookup"><span data-stu-id="d2097-112">Educational institutions usually have two user types: faculty and students.</span></span> <span data-ttu-id="d2097-113">Zaměstnance školy členové jsou považovány za dlouhodobější členy organizace.</span><span class="sxs-lookup"><span data-stu-id="d2097-113">Faculty members are considered longer-term members of the organization.</span></span> <span data-ttu-id="d2097-114">Vytváření místních účtů pro ně je žádoucí.</span><span class="sxs-lookup"><span data-stu-id="d2097-114">Creating on-premises accounts for them is desirable.</span></span> <span data-ttu-id="d2097-115">Ale studenti, kteří jsou členy shorter-term organizace a je možné spravovat své účty ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2097-115">But students are shorter-term members of the organization and  their accounts can be managed in Azure AD.</span></span> <span data-ttu-id="d2097-116">To znamená, že můžete directory škálování vloží do cloudu namísto uloženého místně.</span><span class="sxs-lookup"><span data-stu-id="d2097-116">This means that directory scale can be pushed to the cloud instead of being stored on-premises.</span></span> <span data-ttu-id="d2097-117">Taky to znamená, že studenti, kteří budou moci přihlásit do systému Windows pomocí svých účtů Azure AD a získat přístup k prostředkům v Office 365 v aplikacích Office.</span><span class="sxs-lookup"><span data-stu-id="d2097-117">It also means that students  will be able to sign in to Windows with their Azure AD accounts and get access to Office 365 resources in Office applications.</span></span>

### <a name="scenario-3-retail-businesses"></a><span data-ttu-id="d2097-118">Scénář 3: Prodejní firmách</span><span class="sxs-lookup"><span data-stu-id="d2097-118">Scenario 3: Retail businesses</span></span>
<span data-ttu-id="d2097-119">Prodejní firmy mají sezónních pracovníků a dlouhodobé zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="d2097-119">Retail businesses have seasonal workers and long-term employees.</span></span> <span data-ttu-id="d2097-120">Obvykle vytvoříte místní účty a připojený k doméně počítače použít pro dlouhodobější zaměstnance na plný úvazek.</span><span class="sxs-lookup"><span data-stu-id="d2097-120">You typically create on-premises accounts and use domain-joined machines for longer-term full-time employees.</span></span> <span data-ttu-id="d2097-121">Ale sezónních pracovníků shorter-term členy organizace, a je třeba spravovat své účty, kde uživatelské licence můžete snadno přesouvat.</span><span class="sxs-lookup"><span data-stu-id="d2097-121">But seasonal workers are shorter-term members of the organization, and it's desirable to manage their accounts where user licenses can be more easily moved around.</span></span> <span data-ttu-id="d2097-122">Když vytvoříte své uživatelské účty v cloudu s licencemi Office 365, těmto uživatelům získat výhody přihlášení k Windows a Office aplikace pomocí účtu Azure AD, při zachování větší flexibilitu s jejich licencí po opustí.</span><span class="sxs-lookup"><span data-stu-id="d2097-122">When you create their user accounts in the cloud with Office 365 licenses, these users get the benefits of signing in to Windows and Office applications with an Azure AD account, while you maintain more flexibility with their licenses after they leave.</span></span>

### <a name="scenario-4-additional-scenarios"></a><span data-ttu-id="d2097-123">Scénář 4: Další scénáře</span><span class="sxs-lookup"><span data-stu-id="d2097-123">Scenario 4: Additional scenarios</span></span>
<span data-ttu-id="d2097-124">Společně s výhody už jsme probírali výše, můžete využívat museli uživatelé kvůli zjednodušená spojující prostředí, efektivní správu zařízení, registrace správu automatické mobilních zařízení a jednotné přihlašování do služby Azure AD připojit svá zařízení do služby Azure AD a místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="d2097-124">Along with the benefits discussed earlier, you  benefit from having your users join their devices to Azure AD because of a simplified joining experience, efficient device management, automatic mobile device management enrollment, and single sign-on to Azure AD and on-premises resources.</span></span>  

## <a name="deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="d2097-125">Informace o nasazení pro Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="d2097-125">Deployment considerations for Azure AD Join</span></span>
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a><span data-ttu-id="d2097-126">Povolit uživatelům připojení k zařízení ve vlastnictví společnosti přímo do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2097-126">Enable your users to join a company-owned device directly to Azure AD</span></span>
<span data-ttu-id="d2097-127">Podniky můžou poskytnout jen cloudové účty partnerských společností a organizace.</span><span class="sxs-lookup"><span data-stu-id="d2097-127">Enterprises can provide cloud-only accounts to partner companies and organizations.</span></span> <span data-ttu-id="d2097-128">Těchto partnerů můžete pak snadný přístup k firemním aplikacím a prostředkům pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2097-128">These partners can then easily access company apps and resources with single sign-on.</span></span> <span data-ttu-id="d2097-129">Tento scénář se vztahuje na uživatele, kteří přístup k prostředkům především v cloudu, jako je například Office 365 nebo SaaS aplikací, které jsou závislé na službě Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="d2097-129">This scenario is applicable to users who access resources primarily in the cloud, such as Office 365 or SaaS apps that rely on Azure AD for authentication.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d2097-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2097-130">Prerequisites</span></span>
<span data-ttu-id="d2097-131">**Na úrovni organizace (správce)**</span><span class="sxs-lookup"><span data-stu-id="d2097-131">**At the enterprise level (administrator)**</span></span>

* <span data-ttu-id="d2097-132">Předplatné Azure s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2097-132">Azure subscription with Azure Active Directory</span></span>  

<span data-ttu-id="d2097-133">**Na úrovni uživatele**</span><span class="sxs-lookup"><span data-stu-id="d2097-133">**At the user level**</span></span>

* <span data-ttu-id="d2097-134">Windows 10 (edice Professional a Enterprise)</span><span class="sxs-lookup"><span data-stu-id="d2097-134">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="d2097-135">Úlohy správce</span><span class="sxs-lookup"><span data-stu-id="d2097-135">Administrator tasks</span></span>
* [<span data-ttu-id="d2097-136">Nastavení registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="d2097-136">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="d2097-137">Úlohy uživatele</span><span class="sxs-lookup"><span data-stu-id="d2097-137">User tasks</span></span>
* [<span data-ttu-id="d2097-138">Nastavit nové zařízení Windows 10 s Azure AD během instalace</span><span class="sxs-lookup"><span data-stu-id="d2097-138">Set up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md)
* [<span data-ttu-id="d2097-139">Nastavení Windows 10 zařízení s Azure AD v nabídce nastavení</span><span class="sxs-lookup"><span data-stu-id="d2097-139">Set up a Windows 10 device with Azure AD from the settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="d2097-140">Připojení osobního zařízení Windows 10 pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="d2097-140">Join a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a><span data-ttu-id="d2097-141">Povolit model BYOD ve vaší organizaci pro Windows 10</span><span class="sxs-lookup"><span data-stu-id="d2097-141">Enable BYOD in your organization for Windows 10</span></span>
<span data-ttu-id="d2097-142">Můžete nastavit uživatele a zaměstnanci používat svá osobní zařízení se systémem Windows (BYOD) přístup k podnikovým aplikacím a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="d2097-142">You can set up your users and employees to use their personal Windows devices (BYOD) to access company apps and resources.</span></span> <span data-ttu-id="d2097-143">Vaši uživatelé můžete přidat osobní zařízení se systémem Windows pro přístup k prostředkům způsobem zabezpečení a dodržování své účty Azure AD (pracovní nebo školní účty).</span><span class="sxs-lookup"><span data-stu-id="d2097-143">Your users can add their Azure AD accounts (work or school accounts) to a personal Windows device to access resources in a secure and compliant fashion.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d2097-144">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2097-144">Prerequisites</span></span>
<span data-ttu-id="d2097-145">**Na úrovni organizace (správce)**</span><span class="sxs-lookup"><span data-stu-id="d2097-145">**At the enterprise level (administrator)**</span></span>

* <span data-ttu-id="d2097-146">Předplatné Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2097-146">Azure AD subscription</span></span>

<span data-ttu-id="d2097-147">**Na úrovni uživatele**</span><span class="sxs-lookup"><span data-stu-id="d2097-147">**At the user level**</span></span>

* <span data-ttu-id="d2097-148">Windows 10 (edice Professional a Enterprise)</span><span class="sxs-lookup"><span data-stu-id="d2097-148">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="d2097-149">Úlohy správce</span><span class="sxs-lookup"><span data-stu-id="d2097-149">Administrator tasks</span></span>
* [<span data-ttu-id="d2097-150">Nastavení registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="d2097-150">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="d2097-151">Úlohy uživatele</span><span class="sxs-lookup"><span data-stu-id="d2097-151">User tasks</span></span>
* [<span data-ttu-id="d2097-152">Připojení osobního zařízení Windows 10 pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="d2097-152">Join a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a><span data-ttu-id="d2097-153">Další informace</span><span class="sxs-lookup"><span data-stu-id="d2097-153">Additional information</span></span>
* [<span data-ttu-id="d2097-154">Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="d2097-154">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="d2097-155">Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="d2097-155">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="d2097-156">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="d2097-156">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="d2097-157">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="d2097-157">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="d2097-158">Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe</span><span class="sxs-lookup"><span data-stu-id="d2097-158">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="d2097-159">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="d2097-159">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

