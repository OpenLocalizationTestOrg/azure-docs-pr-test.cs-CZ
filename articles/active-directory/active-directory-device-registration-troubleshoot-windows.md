---
title: "aaaTroubleshooting hello automatickou registraci Azure AD domény k počítačům připojeným k pro Windows 10 a Windows Server 2016 | Microsoft Docs"
description: "Řešení potíží s hello automatickou registraci Azure AD domény k počítačům připojeným k pro Windows 10 a Windows Server 2016."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3795323ce9392368b412b3e1208868431e59a74b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad--windows-10-and-windows-server-2016"></a><span data-ttu-id="8beff-103">Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD – Windows 10 a Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="8beff-103">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>

<span data-ttu-id="8beff-104">Toto téma se vztahuje toohello následující klienty:</span><span class="sxs-lookup"><span data-stu-id="8beff-104">This topic is applicable toohello following clients:</span></span>

-   <span data-ttu-id="8beff-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="8beff-105">Windows 10</span></span>
-   <span data-ttu-id="8beff-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="8beff-106">Windows Server 2016</span></span>

<span data-ttu-id="8beff-107">Pro ostatní klienty systému Windows najdete v části [řešení potíží s automatickou registraci domény připojené počítače tooAzure AD pro klienty nižší úrovně Windows](active-directory-device-registration-troubleshoot-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="8beff-107">For other Windows clients, see [Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients](active-directory-device-registration-troubleshoot-windows-legacy.md).</span></span>

<span data-ttu-id="8beff-108">Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-device-registration-get-started.md) hello toosupport následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="8beff-108">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md) toosupport hello following scenarios:</span></span>

- [<span data-ttu-id="8beff-109">Podmíněný přístup využívající zařízení</span><span class="sxs-lookup"><span data-stu-id="8beff-109">Device-based conditional access</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)

- [<span data-ttu-id="8beff-110">Enterprise cestovní nastavení</span><span class="sxs-lookup"><span data-stu-id="8beff-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="8beff-111">Windows Hello pro firmy</span><span class="sxs-lookup"><span data-stu-id="8beff-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="8beff-112">Tento dokument obsahuje pokyny k odstraňování problémů na tom, jak tooresolve potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="8beff-112">This document provides troubleshooting guidance on how tooresolve potential issues.</span></span> 

<span data-ttu-id="8beff-113">Hello registrace je podporované v systému Windows hello aktualizace 10 listopad 2015 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="8beff-113">hello registration is supported in hello Windows 10 November 2015 Update and above.</span></span>  
<span data-ttu-id="8beff-114">Doporučujeme používat hello výročí aktualizace pro povolení výše hello scénáře.</span><span class="sxs-lookup"><span data-stu-id="8beff-114">We recommend using hello Anniversary Update for enabling hello scenarios above.</span></span>

## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="8beff-115">Krok 1: Načíst stav registrace hello</span><span class="sxs-lookup"><span data-stu-id="8beff-115">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="8beff-116">**Stav registrace tooretrieve hello:**</span><span class="sxs-lookup"><span data-stu-id="8beff-116">**tooretrieve hello registration status:**</span></span>

1. <span data-ttu-id="8beff-117">Otevřete hello příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="8beff-117">Open hello command prompt as an administrator.</span></span>

2. <span data-ttu-id="8beff-118">Typ   **/dsregcmd status**</span><span class="sxs-lookup"><span data-stu-id="8beff-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="8beff-119">+----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="8beff-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined : YES
     <span data-ttu-id="8beff-120">EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatel kryptografických služeb platformy Microsoft: Ano KeySignTest:: musíte spustit zvýšenými tootest.</span><span class="sxs-lookup"><span data-stu-id="8beff-120">EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated tootest.</span></span>
                  <span data-ttu-id="8beff-121">Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ano DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="8beff-121">Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO</span></span>
    
    <span data-ttu-id="8beff-122">+----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="8beff-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    <span data-ttu-id="8beff-123">WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano</span><span class="sxs-lookup"><span data-stu-id="8beff-123">WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES</span></span>



## <a name="step-2-evaluate-hello-registration-status"></a><span data-ttu-id="8beff-124">Krok 2: Vyhodnoťte stav registrace hello</span><span class="sxs-lookup"><span data-stu-id="8beff-124">Step 2: Evaluate hello registration status</span></span> 

<span data-ttu-id="8beff-125">Prohlédněte si hello následující pole a ujistěte se, že mají hello očekávaných hodnot:</span><span class="sxs-lookup"><span data-stu-id="8beff-125">Review hello following fields and make sure that they have hello expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="8beff-126">AzureAdJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="8beff-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="8beff-127">Toto pole ukazuje, zda text hello zařízení zaregistrované v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8beff-127">This field shows whether hello device is registered with Azure AD.</span></span> <span data-ttu-id="8beff-128">Pokud hodnota hello zobrazí jako "Ne", registrace nebyla dokončena.</span><span class="sxs-lookup"><span data-stu-id="8beff-128">If hello value shows as ‘NO’, registration has not completed.</span></span> 

<span data-ttu-id="8beff-129">**Možné příčiny:**</span><span class="sxs-lookup"><span data-stu-id="8beff-129">**Possible causes:**</span></span>

- <span data-ttu-id="8beff-130">Ověřování počítače hello, registrace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="8beff-130">Authentication of hello computer for registration failed.</span></span>

- <span data-ttu-id="8beff-131">V organizaci hello, která nemůže být zjištěny hello počítače je proxy server HTTP</span><span class="sxs-lookup"><span data-stu-id="8beff-131">There is an HTTP proxy in hello organization that cannot be discovered by hello computer</span></span>

- <span data-ttu-id="8beff-132">Hello počítače nebudou moci připojit Azure AD pro ověřování nebo Azure DRS pro registraci</span><span class="sxs-lookup"><span data-stu-id="8beff-132">hello computer cannot reach Azure AD for authentication or Azure DRS for registration</span></span>

- <span data-ttu-id="8beff-133">Hello počítač nemusí být v interní síti organizace hello nebo na VPN s přímé směrem pohledu tooan místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="8beff-133">hello computer may not be on hello organization’s internal network or on VPN with direct line of sight tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="8beff-134">Pokud má počítač hello čip TPM, je ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="8beff-134">If hello computer has a TPM, it may be in a bad state.</span></span>

- <span data-ttu-id="8beff-135">Je možné chybnou konfigurací v služby v dokumentu hello si předtím poznamenali, budete potřebovat tooverify znovu.</span><span class="sxs-lookup"><span data-stu-id="8beff-135">There may be a misconfiguration in services noted in hello document earlier that you will need tooverify again.</span></span> <span data-ttu-id="8beff-136">Běžných příkladů:</span><span class="sxs-lookup"><span data-stu-id="8beff-136">Common examples are:</span></span>

    - <span data-ttu-id="8beff-137">Federační server nemá WS-Trust koncových bodů povoleno</span><span class="sxs-lookup"><span data-stu-id="8beff-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="8beff-138">Federační server nemusí umožňovat příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8beff-138">Your federation server may not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="8beff-139">Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do</span><span class="sxs-lookup"><span data-stu-id="8beff-139">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="8beff-140">DomainJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="8beff-140">DomainJoined : YES</span></span>  

<span data-ttu-id="8beff-141">Toto pole ukazuje, jestli hello zařízení je připojené k tooan místní služby Active Directory, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="8beff-141">This field shows whether hello device is joined tooan on-premises Active Directory or not.</span></span> <span data-ttu-id="8beff-142">Pokud hodnota hello zobrazí jako **ne**, hello zařízení nelze automaticky registrace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8beff-142">If hello value shows as **NO**, hello device cannot auto-register with Azure AD.</span></span> <span data-ttu-id="8beff-143">Nejdřív zkontrolujte, že hello zařízení spojení toohello místní služby Active Directory předtím, než můžete zaregistrovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8beff-143">Check first that hello device joins toohello on-premises Active Directory before it can register with Azure AD.</span></span> <span data-ttu-id="8beff-144">Pokud chcete pro připojení hello tooAzure počítače AD přímo, přejděte prosím tooLearn o možnostech Azure Active Directory Join.</span><span class="sxs-lookup"><span data-stu-id="8beff-144">If you are looking for joining hello computer tooAzure AD directly, please go tooLearn about capabilities of Azure Active Directory Join.</span></span>

---

### <a name="workplacejoined--no"></a><span data-ttu-id="8beff-145">WorkplaceJoined: Ne</span><span class="sxs-lookup"><span data-stu-id="8beff-145">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="8beff-146">Toto pole ukazuje, zda text hello zařízení je zaregistrované s Azure AD, ale jako osobní zařízení (označen jako připojené k síti na pracovišti).</span><span class="sxs-lookup"><span data-stu-id="8beff-146">This field shows whether hello device is registered with Azure AD but as a personal device (marked as ‘Workplace Joined’).</span></span> <span data-ttu-id="8beff-147">Pokud tato hodnota by měla zobrazit jako "Ne" pro počítač připojený k doméně zaregistrován s Azure AD, ale pokud se zobrazí na hodnotu Ano, znamená to, že pracovní nebo školní účet byl přidaný předchozí toohello počítač dokončuje registraci.</span><span class="sxs-lookup"><span data-stu-id="8beff-147">If this value should show as ‘NO’ for a domain joined computer registered with Azure AD, however if it shows as YES it means that a work or school account was added prior toohello computer completing registration.</span></span> <span data-ttu-id="8beff-148">Hello účet v tomto případě bude ignorován, pokud používáte verzi hello výročí aktualizace Windows 10 (1607 při spuštění příkazu hello WinVer hello "Spustit" okno nebo okna příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="8beff-148">In this case hello account will be ignored if using hello Anniversary Update version of Windows 10 (1607 when running hello WinVer command in hello ‘Run’ window or a command prompt window).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="8beff-149">WamDefaultSet: Ano a AzureADPrt: Ano</span><span class="sxs-lookup"><span data-stu-id="8beff-149">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="8beff-150">Tato pole ukazují, že tento uživatel hello úspěšně ověřen tooAzure AD při přihlášení toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8beff-150">These fields show that hello user has successfully authenticated tooAzure AD upon signing in toohello device.</span></span> <span data-ttu-id="8beff-151">Pokud se zobrazí "Žádný" hello Toto jsou možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="8beff-151">If they show ‘NO’ hello following are possible causes:</span></span>

- <span data-ttu-id="8beff-152">Klíč chybný úložiště (STK) v čipu TPM přidružené hello zařízení při registraci (hello kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).</span><span class="sxs-lookup"><span data-stu-id="8beff-152">Bad storage key (STK) in TPM associated with hello device upon registration (check hello KeySignTest while running elevated).</span></span>

- <span data-ttu-id="8beff-153">Alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="8beff-153">Alternate Login ID</span></span>

- <span data-ttu-id="8beff-154">Server Proxy protokolu HTTP nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="8beff-154">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="8beff-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8beff-155">Next steps</span></span>

<span data-ttu-id="8beff-156">Další informace najdete v tématu hello [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="8beff-156">For more information, see hello [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
