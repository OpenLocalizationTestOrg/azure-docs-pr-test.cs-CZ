---
title: "Windows 10 a Windows Server 2016 zařízení připojená k hybridní aaaTroubleshooting Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: cc252d1d0684d6632694afc8a367327794228c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a><span data-ttu-id="8c227-103">Řešení potíží s hybridní Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="8c227-103">Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices</span></span> 

<span data-ttu-id="8c227-104">Toto téma se vztahuje toohello následující klienty:</span><span class="sxs-lookup"><span data-stu-id="8c227-104">This topic is applicable toohello following clients:</span></span>

-   <span data-ttu-id="8c227-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="8c227-105">Windows 10</span></span>
-   <span data-ttu-id="8c227-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="8c227-106">Windows Server 2016</span></span>

<span data-ttu-id="8c227-107">Pro ostatní klienty systému Windows najdete v části [nižší úrovně zařízení připojená k hybridní Poradce při potížích s Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="8c227-107">For other Windows clients, see [Troubleshooting hybrid Azure Active Directory joined down-level devices](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span></span>

<span data-ttu-id="8c227-108">Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="8c227-108">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello following scenarios:</span></span>

- <span data-ttu-id="8c227-109">Podmíněný přístup využívající zařízení</span><span class="sxs-lookup"><span data-stu-id="8c227-109">Device-based conditional access</span></span>

- [<span data-ttu-id="8c227-110">Enterprise cestovní nastavení</span><span class="sxs-lookup"><span data-stu-id="8c227-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="8c227-111">Windows Hello pro firmy</span><span class="sxs-lookup"><span data-stu-id="8c227-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="8c227-112">Tento dokument obsahuje pokyny k odstraňování problémů na tom, jak tooresolve potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="8c227-112">This document provides troubleshooting guidance on how tooresolve potential issues.</span></span> 


<span data-ttu-id="8c227-113">Pro Windows 10 a Windows Server 2016, hybridní hello podporuje připojení k Azure Active Directory systému Windows 10. listopadu 2015 aktualizace a vyšší.</span><span class="sxs-lookup"><span data-stu-id="8c227-113">For Windows 10 and Windows Server 2016, hybrid Azure Active Directory join supports hello Windows 10 November 2015 Update and above.</span></span> <span data-ttu-id="8c227-114">Doporučujeme používat hello výročí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="8c227-114">We recommend using hello Anniversary update.</span></span>

## <a name="step-1-retrieve-hello-join-status"></a><span data-ttu-id="8c227-115">Krok 1: Načíst stav spojení hello</span><span class="sxs-lookup"><span data-stu-id="8c227-115">Step 1: Retrieve hello join status</span></span> 

<span data-ttu-id="8c227-116">**Stav připojení k tooretrieve hello:**</span><span class="sxs-lookup"><span data-stu-id="8c227-116">**tooretrieve hello join status:**</span></span>

1. <span data-ttu-id="8c227-117">Hello otevřete příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="8c227-117">Open hello command prompt as an administrator</span></span>

2. <span data-ttu-id="8c227-118">Typ   **/dsregcmd status**</span><span class="sxs-lookup"><span data-stu-id="8c227-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="8c227-119">+----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="8c227-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined: YES
     <span data-ttu-id="8c227-120">EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatel kryptografických služeb platformy Microsoft: Ano KeySignTest:: musíte spustit zvýšenými tootest.</span><span class="sxs-lookup"><span data-stu-id="8c227-120">EnterpriseJoined: NO DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: YES KeySignTest: : MUST Run elevated tootest.</span></span>
                  <span data-ttu-id="8c227-121">Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Ano DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="8c227-121">Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn:ms-drs:enterpriseregistration.windows.net DomainJoined: YES DomainName: CONTOSO</span></span>
    
    <span data-ttu-id="8c227-122">+----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="8c227-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    <span data-ttu-id="8c227-123">WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano</span><span class="sxs-lookup"><span data-stu-id="8c227-123">WamDefaultAuthority: organizations         WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt: YES</span></span>



## <a name="step-2-evaluate-hello-join-status"></a><span data-ttu-id="8c227-124">Krok 2: Vyhodnocení stavu připojení k hello</span><span class="sxs-lookup"><span data-stu-id="8c227-124">Step 2: Evaluate hello join status</span></span> 

<span data-ttu-id="8c227-125">Prohlédněte si hello následující pole a ujistěte se, že mají hello očekávaných hodnot:</span><span class="sxs-lookup"><span data-stu-id="8c227-125">Review hello following fields and make sure that they have hello expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="8c227-126">AzureAdJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="8c227-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="8c227-127">Toto pole určuje, zda text hello zařízení je spojen s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c227-127">This field indicates whether hello device is joined with Azure AD.</span></span> <span data-ttu-id="8c227-128">Pokud je hodnota hello **ne**, hello spojení tooAzure AD dosud nebyl dokončen.</span><span class="sxs-lookup"><span data-stu-id="8c227-128">If hello value is **NO**, hello join tooAzure AD has not completed yet.</span></span> 

<span data-ttu-id="8c227-129">**Možné příčiny:**</span><span class="sxs-lookup"><span data-stu-id="8c227-129">**Possible causes:**</span></span>

- <span data-ttu-id="8c227-130">Ověřování počítače hello spojení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="8c227-130">Authentication of hello computer for a join failed.</span></span>

- <span data-ttu-id="8c227-131">V organizaci hello, která nemůže být zjištěny hello počítače je proxy server HTTP</span><span class="sxs-lookup"><span data-stu-id="8c227-131">There is an HTTP proxy in hello organization that cannot be discovered by hello computer</span></span>

- <span data-ttu-id="8c227-132">Hello počítače nebudou moci připojit tooauthenticate Azure AD nebo Azure DRS pro registraci</span><span class="sxs-lookup"><span data-stu-id="8c227-132">hello computer cannot reach Azure AD tooauthenticate or Azure DRS for registration</span></span>

- <span data-ttu-id="8c227-133">Hello počítač nemusí být v interní síti organizace hello nebo na VPN s přímé směrem pohledu tooan místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="8c227-133">hello computer may not be on hello organization’s internal network or on VPN with direct line of sight tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="8c227-134">Pokud má počítač hello čip TPM, může být ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="8c227-134">If hello computer has a TPM, it can be in a bad state.</span></span>

- <span data-ttu-id="8c227-135">Může být chybnou konfigurací v hello služby v dokumentu hello si předtím poznamenali, budete potřebovat tooverify znovu.</span><span class="sxs-lookup"><span data-stu-id="8c227-135">There might be a misconfiguration in hello services noted in hello document earlier that you will need tooverify again.</span></span> <span data-ttu-id="8c227-136">Běžných příkladů:</span><span class="sxs-lookup"><span data-stu-id="8c227-136">Common examples are:</span></span>

    - <span data-ttu-id="8c227-137">Federační server nemá WS-Trust koncových bodů povoleno</span><span class="sxs-lookup"><span data-stu-id="8c227-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="8c227-138">Federační server nepovoluje příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8c227-138">Your federation server does not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="8c227-139">Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do</span><span class="sxs-lookup"><span data-stu-id="8c227-139">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="8c227-140">DomainJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="8c227-140">DomainJoined : YES</span></span>  

<span data-ttu-id="8c227-141">Toto pole určuje, zda text hello zařízení se připojilo k tooan místní služby Active Directory nebo ne.</span><span class="sxs-lookup"><span data-stu-id="8c227-141">This field indicates whether hello device is joined tooan on-premises Active Directory or not.</span></span> <span data-ttu-id="8c227-142">Pokud je hodnota hello **ne**, hello zařízení nelze provést připojení k hybridní Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c227-142">If hello value is **NO**, hello device cannot perform a hybrid Azure AD join.</span></span>  

---

### <a name="workplacejoined--no"></a><span data-ttu-id="8c227-143">WorkplaceJoined: Ne</span><span class="sxs-lookup"><span data-stu-id="8c227-143">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="8c227-144">Toto pole určuje, zda text hello zařízení zaregistrované v Azure AD jako osobní zařízení (označen jako *připojené k síti na pracovišti*).</span><span class="sxs-lookup"><span data-stu-id="8c227-144">This field indicates whether hello device is registered with Azure AD as a personal device (marked as *Workplace Joined*).</span></span> <span data-ttu-id="8c227-145">Tato hodnota by měla být **ne** pro počítač připojený k doméně, která je také připojené k hybridní Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c227-145">This value should be **NO** for a domain-joined computer that is also hybrid Azure AD joined.</span></span> <span data-ttu-id="8c227-146">Pokud je hodnota hello **Ano**, pracovní nebo školní účet se přidal dokončení předchozí toohello hello hybridní Azure AD join.</span><span class="sxs-lookup"><span data-stu-id="8c227-146">If hello value is **YES**, a work or school account was added prior toohello completion of hello hybrid Azure AD join.</span></span> <span data-ttu-id="8c227-147">V takovém případě hello účtu se ignoruje při použití hello výročí aktualizovanou verzi Windows 10 (1607).</span><span class="sxs-lookup"><span data-stu-id="8c227-147">In this case, hello account is ignored when using hello Anniversary Update version of Windows 10 (1607).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="8c227-148">WamDefaultSet: Ano a AzureADPrt: Ano</span><span class="sxs-lookup"><span data-stu-id="8c227-148">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="8c227-149">Tato pole označuje, zda uživatel hello byl úspěšně ověřen tooAzure AD při přihlášení toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8c227-149">These fields indicate whether hello user has successfully authenticated tooAzure AD when signing in toohello device.</span></span> <span data-ttu-id="8c227-150">Pokud jsou hodnoty hello **ne**, může to být kvůli:</span><span class="sxs-lookup"><span data-stu-id="8c227-150">If hello values are **NO**, it could be due:</span></span>

- <span data-ttu-id="8c227-151">Klíč chybný úložiště (STK) v čipu TPM přidružené hello zařízení při registraci (hello kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).</span><span class="sxs-lookup"><span data-stu-id="8c227-151">Bad storage key (STK) in TPM associated with hello device upon registration (check hello KeySignTest while running elevated).</span></span>

- <span data-ttu-id="8c227-152">Alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="8c227-152">Alternate Login ID</span></span>

- <span data-ttu-id="8c227-153">Server Proxy protokolu HTTP nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="8c227-153">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c227-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c227-154">Next steps</span></span>

<span data-ttu-id="8c227-155">Máte dotazy, viz hello [nejčastější dotazy ke správě zařízení](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="8c227-155">For questions, see hello [device management FAQ](device-management-faq.md)</span></span> 
