---
title: "aaaUsage scénáře a aspekty nasazení pro připojení ke službě Azure AD | Microsoft Docs"
description: "Vysvětluje, jak mohou správci nastavit službu Azure AD Join pro své koncové uživatele (zaměstnanci, studenty, ostatní uživatelé). Popisuje také hello různých reálných scénářů pro použití připojení ke službě Azure AD."
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
ms.openlocfilehash: 7e57971481aa312ebf8a69999d194f9dcc3d4708
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="1aba4-104">Scénáře využití a aspekty nasazení pro Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="1aba4-104">Usage scenarios and deployment considerations for Azure AD Join</span></span>
## <a name="usage-scenarios-for-azure-ad-join"></a><span data-ttu-id="1aba4-105">Scénáře použití pro připojení ke službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="1aba4-105">Usage scenarios for Azure AD Join</span></span>
### <a name="scenario-1-businesses-largely-in-hello-cloud"></a><span data-ttu-id="1aba4-106">Scénář 1: Podnikům do značné míry v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="1aba4-106">Scenario 1: Businesses largely in hello cloud</span></span>
<span data-ttu-id="1aba4-107">Azure Active Directory Join (Azure AD Join) vám může hodit Pokud aktuálně fungovat a Správa identit pro vaši firmu v cloudu hello nebo brzy přesouváte toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="1aba4-107">Azure Active Directory Join (Azure AD Join) can benefit you if you currently operate and manage identities for your business in hello cloud or are moving toohello cloud soon.</span></span> <span data-ttu-id="1aba4-108">Můžete použít účet, který jste vytvořili v Azure AD toosign v tooWindows 10.</span><span class="sxs-lookup"><span data-stu-id="1aba4-108">You can use an account that you have created in Azure AD toosign in tooWindows 10.</span></span> <span data-ttu-id="1aba4-109">Prostřednictvím [hello prvním spuštění procesu prostředí (FRX)](active-directory-azureadjoin-user-frx.md), nebo díky připojení ke službě Azure AD z [nabídky nastavení hello](active-directory-azureadjoin-user-upgrade.md), uživatelé se můžete zapojit do jejich počítačů tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1aba4-109">Through [hello first run experience (FRX) process](active-directory-azureadjoin-user-frx.md), or by joining Azure AD from [hello settings menu](active-directory-azureadjoin-user-upgrade.md), your users can join their machines tooAzure AD.</span></span>  <span data-ttu-id="1aba4-110">Uživatelé mohou také získejte jednotné přihlašování (SSO) přístup příliš cloudové prostředky jako je Office 365 ve svém prohlížeči zakázanou nebo v aplikacích Office.</span><span class="sxs-lookup"><span data-stu-id="1aba4-110">Your users can also enjoy single sign-on (SSO) access too cloud resources like Office 365, either in their browsers or in Office applications.</span></span>

### <a name="scenario-2-educational-institutions"></a><span data-ttu-id="1aba4-111">Scénář 2: Vzdělávací instituce</span><span class="sxs-lookup"><span data-stu-id="1aba4-111">Scenario 2: Educational institutions</span></span>
<span data-ttu-id="1aba4-112">Vzdělávací instituce obvykle mají dva typy uživatelů: zaměstnance školy a studenti.</span><span class="sxs-lookup"><span data-stu-id="1aba4-112">Educational institutions usually have two user types: faculty and students.</span></span> <span data-ttu-id="1aba4-113">Zaměstnance školy členové jsou považovány za dlouhodobější členy hello organizace.</span><span class="sxs-lookup"><span data-stu-id="1aba4-113">Faculty members are considered longer-term members of hello organization.</span></span> <span data-ttu-id="1aba4-114">Vytváření místních účtů pro ně je žádoucí.</span><span class="sxs-lookup"><span data-stu-id="1aba4-114">Creating on-premises accounts for them is desirable.</span></span> <span data-ttu-id="1aba4-115">Ale studenti, kteří jsou členy shorter-term hello organizace a je možné spravovat své účty ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aba4-115">But students are shorter-term members of hello organization and  their accounts can be managed in Azure AD.</span></span> <span data-ttu-id="1aba4-116">To znamená, že adresář škálování mohou poslat toohello cloudu namísto uloženého místně.</span><span class="sxs-lookup"><span data-stu-id="1aba4-116">This means that directory scale can be pushed toohello cloud instead of being stored on-premises.</span></span> <span data-ttu-id="1aba4-117">Také znamená, že studenty bude možné toosign v tooWindows s účty služby Azure AD a získání přístupu v aplikacích Office 365 prostředky tooOffice.</span><span class="sxs-lookup"><span data-stu-id="1aba4-117">It also means that students  will be able toosign in tooWindows with their Azure AD accounts and get access tooOffice 365 resources in Office applications.</span></span>

### <a name="scenario-3-retail-businesses"></a><span data-ttu-id="1aba4-118">Scénář 3: Prodejní firmách</span><span class="sxs-lookup"><span data-stu-id="1aba4-118">Scenario 3: Retail businesses</span></span>
<span data-ttu-id="1aba4-119">Prodejní firmy mají sezónních pracovníků a dlouhodobé zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="1aba4-119">Retail businesses have seasonal workers and long-term employees.</span></span> <span data-ttu-id="1aba4-120">Obvykle vytvoříte místní účty a připojený k doméně počítače použít pro dlouhodobější zaměstnance na plný úvazek.</span><span class="sxs-lookup"><span data-stu-id="1aba4-120">You typically create on-premises accounts and use domain-joined machines for longer-term full-time employees.</span></span> <span data-ttu-id="1aba4-121">Ale sezónních pracovníků shorter-term členy hello organizace, a je žádoucí toomanage své účty, kde uživatelské licence můžete snadno přesouvat.</span><span class="sxs-lookup"><span data-stu-id="1aba4-121">But seasonal workers are shorter-term members of hello organization, and it's desirable toomanage their accounts where user licenses can be more easily moved around.</span></span> <span data-ttu-id="1aba4-122">Když vytvoříte své uživatelské účty v cloudu hello s licencemi Office 365, tito uživatelé získat výhody hello podepisování tooWindows a v aplikacích Office pomocí účtu Azure AD, při zachování větší flexibilitu s jejich licencí po opustí.</span><span class="sxs-lookup"><span data-stu-id="1aba4-122">When you create their user accounts in hello cloud with Office 365 licenses, these users get hello benefits of signing in tooWindows and Office applications with an Azure AD account, while you maintain more flexibility with their licenses after they leave.</span></span>

### <a name="scenario-4-additional-scenarios"></a><span data-ttu-id="1aba4-123">Scénář 4: Další scénáře</span><span class="sxs-lookup"><span data-stu-id="1aba4-123">Scenario 4: Additional scenarios</span></span>
<span data-ttu-id="1aba4-124">Společně s hello výhody už jsme probírali výše můžete využívat s kvůli zjednodušené spojující prostředí, Správa efektivní zařízení, registrace správu automatické mobilních zařízení a tooAzure přihlášení připojit své zařízení tooAzure AD uživatelům AD a místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="1aba4-124">Along with hello benefits discussed earlier, you  benefit from having your users join their devices tooAzure AD because of a simplified joining experience, efficient device management, automatic mobile device management enrollment, and single sign-on tooAzure AD and on-premises resources.</span></span>  

## <a name="deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="1aba4-125">Informace o nasazení pro Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="1aba4-125">Deployment considerations for Azure AD Join</span></span>
### <a name="enable-your-users-toojoin-a-company-owned-device-directly-tooazure-ad"></a><span data-ttu-id="1aba4-126">Povolit vaši uživatelé toojoin zařízení ve vlastnictví společnosti přímo tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="1aba4-126">Enable your users toojoin a company-owned device directly tooAzure AD</span></span>
<span data-ttu-id="1aba4-127">Podniky můžete zadat jen cloudové účty toopartner společnosti a organizace.</span><span class="sxs-lookup"><span data-stu-id="1aba4-127">Enterprises can provide cloud-only accounts toopartner companies and organizations.</span></span> <span data-ttu-id="1aba4-128">Těchto partnerů můžete pak snadný přístup k firemním aplikacím a prostředkům pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1aba4-128">These partners can then easily access company apps and resources with single sign-on.</span></span> <span data-ttu-id="1aba4-129">Tento scénář je použít toousers, kdo přístup k prostředkům především v cloudu hello, jako je například Office 365 nebo SaaS aplikací, které jsou závislé na službě Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="1aba4-129">This scenario is applicable toousers who access resources primarily in hello cloud, such as Office 365 or SaaS apps that rely on Azure AD for authentication.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1aba4-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1aba4-130">Prerequisites</span></span>
<span data-ttu-id="1aba4-131">**Na úrovni podniku hello (správce)**</span><span class="sxs-lookup"><span data-stu-id="1aba4-131">**At hello enterprise level (administrator)**</span></span>

* <span data-ttu-id="1aba4-132">Předplatné Azure s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1aba4-132">Azure subscription with Azure Active Directory</span></span>  

<span data-ttu-id="1aba4-133">**Na úrovni uživatele hello**</span><span class="sxs-lookup"><span data-stu-id="1aba4-133">**At hello user level**</span></span>

* <span data-ttu-id="1aba4-134">Windows 10 (edice Professional a Enterprise)</span><span class="sxs-lookup"><span data-stu-id="1aba4-134">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="1aba4-135">Úlohy správce</span><span class="sxs-lookup"><span data-stu-id="1aba4-135">Administrator tasks</span></span>
* [<span data-ttu-id="1aba4-136">Nastavení registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="1aba4-136">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="1aba4-137">Úlohy uživatele</span><span class="sxs-lookup"><span data-stu-id="1aba4-137">User tasks</span></span>
* [<span data-ttu-id="1aba4-138">Nastavit nové zařízení Windows 10 s Azure AD během instalace</span><span class="sxs-lookup"><span data-stu-id="1aba4-138">Set up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md)
* [<span data-ttu-id="1aba4-139">Nastavit v nabídce nastavení hello zařízením s Windows 10 s Azure AD</span><span class="sxs-lookup"><span data-stu-id="1aba4-139">Set up a Windows 10 device with Azure AD from hello settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="1aba4-140">Připojení osobní organizaci tooyour zařízení Windows 10</span><span class="sxs-lookup"><span data-stu-id="1aba4-140">Join a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a><span data-ttu-id="1aba4-141">Povolit model BYOD ve vaší organizaci pro Windows 10</span><span class="sxs-lookup"><span data-stu-id="1aba4-141">Enable BYOD in your organization for Windows 10</span></span>
<span data-ttu-id="1aba4-142">Vaše uživatele a zaměstnanci toouse můžete nastavit jejich osobní Windows zařízení (BYOD) tooaccess firemním aplikacím a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="1aba4-142">You can set up your users and employees toouse their personal Windows devices (BYOD) tooaccess company apps and resources.</span></span> <span data-ttu-id="1aba4-143">Vaši uživatelé můžete přidat svých prostředků Azure AD účty (pracovní nebo školní účty) tooa osobní Windows zařízení tooaccess způsobem zabezpečení a dodržování.</span><span class="sxs-lookup"><span data-stu-id="1aba4-143">Your users can add their Azure AD accounts (work or school accounts) tooa personal Windows device tooaccess resources in a secure and compliant fashion.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1aba4-144">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1aba4-144">Prerequisites</span></span>
<span data-ttu-id="1aba4-145">**Na úrovni podniku hello (správce)**</span><span class="sxs-lookup"><span data-stu-id="1aba4-145">**At hello enterprise level (administrator)**</span></span>

* <span data-ttu-id="1aba4-146">Předplatné Azure AD</span><span class="sxs-lookup"><span data-stu-id="1aba4-146">Azure AD subscription</span></span>

<span data-ttu-id="1aba4-147">**Na úrovni uživatele hello**</span><span class="sxs-lookup"><span data-stu-id="1aba4-147">**At hello user level**</span></span>

* <span data-ttu-id="1aba4-148">Windows 10 (edice Professional a Enterprise)</span><span class="sxs-lookup"><span data-stu-id="1aba4-148">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="1aba4-149">Úlohy správce</span><span class="sxs-lookup"><span data-stu-id="1aba4-149">Administrator tasks</span></span>
* [<span data-ttu-id="1aba4-150">Nastavení registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="1aba4-150">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="1aba4-151">Úlohy uživatele</span><span class="sxs-lookup"><span data-stu-id="1aba4-151">User tasks</span></span>
* [<span data-ttu-id="1aba4-152">Připojení osobní organizaci tooyour zařízení Windows 10</span><span class="sxs-lookup"><span data-stu-id="1aba4-152">Join a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a><span data-ttu-id="1aba4-153">Další informace</span><span class="sxs-lookup"><span data-stu-id="1aba4-153">Additional information</span></span>
* [<span data-ttu-id="1aba4-154">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="1aba4-154">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="1aba4-155">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="1aba4-155">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="1aba4-156">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="1aba4-156">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="1aba4-157">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="1aba4-157">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="1aba4-158">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="1aba4-158">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="1aba4-159">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="1aba4-159">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

