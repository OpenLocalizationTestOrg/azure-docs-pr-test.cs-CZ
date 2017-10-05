---
title: "Řešení potíží s hybridní Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016 | Microsoft Docs"
description: "Řešení potíží s hybridní Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 51962c14a3c32bbfa9a613fa203cc48cfea50c0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a><span data-ttu-id="7d1a0-103">Řešení potíží s hybridní Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="7d1a0-103">Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices</span></span> 

<span data-ttu-id="7d1a0-104">Toto téma se vztahuje na následující klienty:</span><span class="sxs-lookup"><span data-stu-id="7d1a0-104">This topic is applicable to the following clients:</span></span>

-   <span data-ttu-id="7d1a0-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="7d1a0-105">Windows 10</span></span>
-   <span data-ttu-id="7d1a0-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="7d1a0-106">Windows Server 2016</span></span>

<span data-ttu-id="7d1a0-107">Pro ostatní klienty systému Windows najdete v části [nižší úrovně zařízení připojená k hybridní Poradce při potížích s Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="7d1a0-107">For other Windows clients, see [Troubleshooting hybrid Azure Active Directory joined down-level devices](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span></span>

<span data-ttu-id="7d1a0-108">Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) k podporují následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="7d1a0-108">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) to support the following scenarios:</span></span>

- <span data-ttu-id="7d1a0-109">Podmíněný přístup využívající zařízení</span><span class="sxs-lookup"><span data-stu-id="7d1a0-109">Device-based conditional access</span></span>

- [<span data-ttu-id="7d1a0-110">Enterprise cestovní nastavení</span><span class="sxs-lookup"><span data-stu-id="7d1a0-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="7d1a0-111">Windows Hello pro firmy</span><span class="sxs-lookup"><span data-stu-id="7d1a0-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="7d1a0-112">Tento dokument obsahuje pokyny k odstraňování problémů, o tom, jak vyřešit potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-112">This document provides troubleshooting guidance on how to resolve potential issues.</span></span> 


<span data-ttu-id="7d1a0-113">Pro Windows 10 a Windows Server 2016, hybridní připojení k Azure Active Directory podporuje Windows 10. listopadu 2015 aktualizace a vyšší.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-113">For Windows 10 and Windows Server 2016, hybrid Azure Active Directory join supports the Windows 10 November 2015 Update and above.</span></span> <span data-ttu-id="7d1a0-114">Doporučujeme používat výročí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-114">We recommend using the Anniversary update.</span></span>

## <a name="step-1-retrieve-the-join-status"></a><span data-ttu-id="7d1a0-115">Krok 1: Načíst stav spojení</span><span class="sxs-lookup"><span data-stu-id="7d1a0-115">Step 1: Retrieve the join status</span></span> 

<span data-ttu-id="7d1a0-116">**Načíst stav připojení:**</span><span class="sxs-lookup"><span data-stu-id="7d1a0-116">**To retrieve the join status:**</span></span>

1. <span data-ttu-id="7d1a0-117">Otevřete příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="7d1a0-117">Open the command prompt as an administrator</span></span>

2. <span data-ttu-id="7d1a0-118">Typ   **/dsregcmd status**</span><span class="sxs-lookup"><span data-stu-id="7d1a0-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="7d1a0-119">+----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="7d1a0-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined: YES
     <span data-ttu-id="7d1a0-120">EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatel kryptografických služeb platformy Microsoft: Ano KeySignTest:: musíte spustit zvýšenými k testování.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-120">EnterpriseJoined: NO DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: YES KeySignTest: : MUST Run elevated to test.</span></span>
                  <span data-ttu-id="7d1a0-121">Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Ano DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="7d1a0-121">Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn:ms-drs:enterpriseregistration.windows.net DomainJoined: YES DomainName: CONTOSO</span></span>
    
    <span data-ttu-id="7d1a0-122">+----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="7d1a0-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    <span data-ttu-id="7d1a0-123">WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano</span><span class="sxs-lookup"><span data-stu-id="7d1a0-123">WamDefaultAuthority: organizations         WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt: YES</span></span>



## <a name="step-2-evaluate-the-join-status"></a><span data-ttu-id="7d1a0-124">Krok 2: Vyhodnocení stavu připojení k</span><span class="sxs-lookup"><span data-stu-id="7d1a0-124">Step 2: Evaluate the join status</span></span> 

<span data-ttu-id="7d1a0-125">Zkontrolujte následující pole a ujistěte se, že mají očekávaných hodnot:</span><span class="sxs-lookup"><span data-stu-id="7d1a0-125">Review the following fields and make sure that they have the expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="7d1a0-126">AzureAdJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="7d1a0-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="7d1a0-127">Toto pole určuje, zda je připojení zařízení s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-127">This field indicates whether the device is joined with Azure AD.</span></span> <span data-ttu-id="7d1a0-128">Pokud je hodnota **ne**, nebyl ještě dokončen připojení ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-128">If the value is **NO**, the join to Azure AD has not completed yet.</span></span> 

<span data-ttu-id="7d1a0-129">**Možné příčiny:**</span><span class="sxs-lookup"><span data-stu-id="7d1a0-129">**Possible causes:**</span></span>

- <span data-ttu-id="7d1a0-130">Ověřování počítače, a připojte se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-130">Authentication of the computer for a join failed.</span></span>

- <span data-ttu-id="7d1a0-131">V organizaci, která nemůže být zjištěny počítače je proxy server HTTP</span><span class="sxs-lookup"><span data-stu-id="7d1a0-131">There is an HTTP proxy in the organization that cannot be discovered by the computer</span></span>

- <span data-ttu-id="7d1a0-132">Počítač se nemůže připojit k ověření Azure AD nebo Azure DRS pro registraci</span><span class="sxs-lookup"><span data-stu-id="7d1a0-132">The computer cannot reach Azure AD to authenticate or Azure DRS for registration</span></span>

- <span data-ttu-id="7d1a0-133">Počítač nemusí být v interní síti organizace nebo na VPN s přímé směrem pohledu na místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-133">The computer may not be on the organization’s internal network or on VPN with direct line of sight to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="7d1a0-134">Pokud má počítač čip TPM, může být ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-134">If the computer has a TPM, it can be in a bad state.</span></span>

- <span data-ttu-id="7d1a0-135">Může být chybnou konfigurací v služeb v dokumentu si předtím poznamenali, budete muset znovu ověřit.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-135">There might be a misconfiguration in the services noted in the document earlier that you will need to verify again.</span></span> <span data-ttu-id="7d1a0-136">Běžných příkladů:</span><span class="sxs-lookup"><span data-stu-id="7d1a0-136">Common examples are:</span></span>

    - <span data-ttu-id="7d1a0-137">Federační server nemá WS-Trust koncových bodů povoleno</span><span class="sxs-lookup"><span data-stu-id="7d1a0-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="7d1a0-138">Federační server nepovoluje příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-138">Your federation server does not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="7d1a0-139">Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, kam počítač patří do</span><span class="sxs-lookup"><span data-stu-id="7d1a0-139">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="7d1a0-140">DomainJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="7d1a0-140">DomainJoined : YES</span></span>  

<span data-ttu-id="7d1a0-141">Toto pole určuje, zda je připojení zařízení do služby Active Directory v místě nebo ne.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-141">This field indicates whether the device is joined to an on-premises Active Directory or not.</span></span> <span data-ttu-id="7d1a0-142">Pokud je hodnota **ne**, zařízení nelze provést připojení k hybridní Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-142">If the value is **NO**, the device cannot perform a hybrid Azure AD join.</span></span>  

---

### <a name="workplacejoined--no"></a><span data-ttu-id="7d1a0-143">WorkplaceJoined: Ne</span><span class="sxs-lookup"><span data-stu-id="7d1a0-143">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="7d1a0-144">Toto pole určuje, jestli je zařízení zaregistrované v Azure AD jako osobní zařízení (označen jako *připojené k síti na pracovišti*).</span><span class="sxs-lookup"><span data-stu-id="7d1a0-144">This field indicates whether the device is registered with Azure AD as a personal device (marked as *Workplace Joined*).</span></span> <span data-ttu-id="7d1a0-145">Tato hodnota by měla být **ne** pro počítač připojený k doméně, která je také připojené k hybridní Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-145">This value should be **NO** for a domain-joined computer that is also hybrid Azure AD joined.</span></span> <span data-ttu-id="7d1a0-146">Pokud je hodnota **Ano**, pracovní nebo školní účet byl přidán před dokončením připojení k hybridní Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-146">If the value is **YES**, a work or school account was added prior to the completion of the hybrid Azure AD join.</span></span> <span data-ttu-id="7d1a0-147">V takovém případě účtu se ignoruje při použití výročí aktualizovanou verzi Windows 10 (1607).</span><span class="sxs-lookup"><span data-stu-id="7d1a0-147">In this case, the account is ignored when using the Anniversary Update version of Windows 10 (1607).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="7d1a0-148">WamDefaultSet: Ano a AzureADPrt: Ano</span><span class="sxs-lookup"><span data-stu-id="7d1a0-148">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="7d1a0-149">Tato pole označuje, zda byl uživatel úspěšně ověřen do služby Azure AD při přihlášení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-149">These fields indicate whether the user has successfully authenticated to Azure AD when signing in to the device.</span></span> <span data-ttu-id="7d1a0-150">Pokud jsou hodnoty **ne**, může to být kvůli:</span><span class="sxs-lookup"><span data-stu-id="7d1a0-150">If the values are **NO**, it could be due:</span></span>

- <span data-ttu-id="7d1a0-151">Klíč chybný úložiště (STK) v čipu TPM, které jsou přidružené k zařízení na základě zápisu (Kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).</span><span class="sxs-lookup"><span data-stu-id="7d1a0-151">Bad storage key (STK) in TPM associated with the device upon registration (check the KeySignTest while running elevated).</span></span>

- <span data-ttu-id="7d1a0-152">Alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="7d1a0-152">Alternate Login ID</span></span>

- <span data-ttu-id="7d1a0-153">Server Proxy protokolu HTTP nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="7d1a0-153">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d1a0-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d1a0-154">Next steps</span></span>

<span data-ttu-id="7d1a0-155">Otázky, najdete v článku [nejčastější dotazy ke správě zařízení](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="7d1a0-155">For questions, see the [device management FAQ](device-management-faq.md)</span></span> 