---
title: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro Windows 10 a Windows Server 2016 | Microsoft Docs"
description: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro Windows 10 a Windows Server 2016."
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
ms.openlocfilehash: 5b7f95f302f716d9221b5fae59aa2df5c956a524
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a><span data-ttu-id="4c959-103">Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD – Windows 10 a Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="4c959-103">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>

<span data-ttu-id="4c959-104">Toto téma se vztahuje na následující klienty:</span><span class="sxs-lookup"><span data-stu-id="4c959-104">This topic is applicable to the following clients:</span></span>

-   <span data-ttu-id="4c959-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="4c959-105">Windows 10</span></span>
-   <span data-ttu-id="4c959-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="4c959-106">Windows Server 2016</span></span>

<span data-ttu-id="4c959-107">Pro ostatní klienty systému Windows najdete v části [řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD pro klienty nižší úrovně Windows](active-directory-device-registration-troubleshoot-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="4c959-107">For other Windows clients, see [Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients](active-directory-device-registration-troubleshoot-windows-legacy.md).</span></span>

<span data-ttu-id="4c959-108">Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-device-registration-get-started.md) k podporují následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="4c959-108">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md) to support the following scenarios:</span></span>

- [<span data-ttu-id="4c959-109">Podmíněný přístup využívající zařízení</span><span class="sxs-lookup"><span data-stu-id="4c959-109">Device-based conditional access</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)

- [<span data-ttu-id="4c959-110">Enterprise cestovní nastavení</span><span class="sxs-lookup"><span data-stu-id="4c959-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="4c959-111">Windows Hello pro firmy</span><span class="sxs-lookup"><span data-stu-id="4c959-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="4c959-112">Tento dokument obsahuje pokyny k odstraňování problémů, o tom, jak vyřešit potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="4c959-112">This document provides troubleshooting guidance on how to resolve potential issues.</span></span> 

<span data-ttu-id="4c959-113">Registrace je podporována v systému Windows 10. listopadu 2015 aktualizace a vyšší.</span><span class="sxs-lookup"><span data-stu-id="4c959-113">The registration is supported in the Windows 10 November 2015 Update and above.</span></span>  
<span data-ttu-id="4c959-114">Doporučujeme používat výročí aktualizace pro povolení výše uvedených scénářích.</span><span class="sxs-lookup"><span data-stu-id="4c959-114">We recommend using the Anniversary Update for enabling the scenarios above.</span></span>

## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="4c959-115">Krok 1: Načíst stav registrace</span><span class="sxs-lookup"><span data-stu-id="4c959-115">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="4c959-116">**Načíst stav registrace:**</span><span class="sxs-lookup"><span data-stu-id="4c959-116">**To retrieve the registration status:**</span></span>

1. <span data-ttu-id="4c959-117">Otevřete příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="4c959-117">Open the command prompt as an administrator.</span></span>

2. <span data-ttu-id="4c959-118">Typ   **/dsregcmd status**</span><span class="sxs-lookup"><span data-stu-id="4c959-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="4c959-119">+----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="4c959-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined : YES
     <span data-ttu-id="4c959-120">EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatele kryptografických platformy společnosti Microsoft: Ano KeySignTest:: musí spouštět se zvýšenými oprávněními k testování.</span><span class="sxs-lookup"><span data-stu-id="4c959-120">EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated to test.</span></span>
                  <span data-ttu-id="4c959-121">Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ano DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="4c959-121">Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO</span></span>
    
    <span data-ttu-id="4c959-122">+----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="4c959-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    <span data-ttu-id="4c959-123">WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano</span><span class="sxs-lookup"><span data-stu-id="4c959-123">WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES</span></span>



## <a name="step-2-evaluate-the-registration-status"></a><span data-ttu-id="4c959-124">Krok 2: Vyhodnoťte stav registrace</span><span class="sxs-lookup"><span data-stu-id="4c959-124">Step 2: Evaluate the registration status</span></span> 

<span data-ttu-id="4c959-125">Zkontrolujte následující pole a ujistěte se, že mají očekávaných hodnot:</span><span class="sxs-lookup"><span data-stu-id="4c959-125">Review the following fields and make sure that they have the expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="4c959-126">AzureAdJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="4c959-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="4c959-127">Toto pole ukazuje, jestli je zařízení zaregistrované v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c959-127">This field shows whether the device is registered with Azure AD.</span></span> <span data-ttu-id="4c959-128">Pokud je hodnota zobrazena jako "Ne", registrace nebyla dokončena.</span><span class="sxs-lookup"><span data-stu-id="4c959-128">If the value shows as ‘NO’, registration has not completed.</span></span> 

<span data-ttu-id="4c959-129">**Možné příčiny:**</span><span class="sxs-lookup"><span data-stu-id="4c959-129">**Possible causes:**</span></span>

- <span data-ttu-id="4c959-130">Ověřování počítače pro registraci se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4c959-130">Authentication of the computer for registration failed.</span></span>

- <span data-ttu-id="4c959-131">V organizaci, která nemůže být zjištěny počítače je proxy server HTTP</span><span class="sxs-lookup"><span data-stu-id="4c959-131">There is an HTTP proxy in the organization that cannot be discovered by the computer</span></span>

- <span data-ttu-id="4c959-132">Počítač se nemůže připojit, Azure AD pro ověřování nebo Azure DRS pro registraci</span><span class="sxs-lookup"><span data-stu-id="4c959-132">The computer cannot reach Azure AD for authentication or Azure DRS for registration</span></span>

- <span data-ttu-id="4c959-133">Počítač nemusí být v interní síti organizace nebo na VPN s přímé směrem pohledu na místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="4c959-133">The computer may not be on the organization’s internal network or on VPN with direct line of sight to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="4c959-134">Pokud má počítač čip TPM, je ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="4c959-134">If the computer has a TPM, it may be in a bad state.</span></span>

- <span data-ttu-id="4c959-135">Je možné chybnou konfigurací v služby v dokumentu si předtím poznamenali, budete muset znovu ověřit.</span><span class="sxs-lookup"><span data-stu-id="4c959-135">There may be a misconfiguration in services noted in the document earlier that you will need to verify again.</span></span> <span data-ttu-id="4c959-136">Běžných příkladů:</span><span class="sxs-lookup"><span data-stu-id="4c959-136">Common examples are:</span></span>

    - <span data-ttu-id="4c959-137">Federační server nemá WS-Trust koncových bodů povoleno</span><span class="sxs-lookup"><span data-stu-id="4c959-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="4c959-138">Federační server nemusí umožňovat příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4c959-138">Your federation server may not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="4c959-139">Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, kam počítač patří do</span><span class="sxs-lookup"><span data-stu-id="4c959-139">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="4c959-140">DomainJoined: Ano</span><span class="sxs-lookup"><span data-stu-id="4c959-140">DomainJoined : YES</span></span>  

<span data-ttu-id="4c959-141">Toto pole ukazuje, zda je připojení zařízení do služby Active Directory v místě nebo ne.</span><span class="sxs-lookup"><span data-stu-id="4c959-141">This field shows whether the device is joined to an on-premises Active Directory or not.</span></span> <span data-ttu-id="4c959-142">Pokud je hodnota zobrazena jako **ne**, zařízení nelze automaticky registrace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c959-142">If the value shows as **NO**, the device cannot auto-register with Azure AD.</span></span> <span data-ttu-id="4c959-143">Nejdřív zkontrolujte, že zařízení připojí ke službě Active Directory v místě, než můžete zaregistrovat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c959-143">Check first that the device joins to the on-premises Active Directory before it can register with Azure AD.</span></span> <span data-ttu-id="4c959-144">Pokud hledáte připojování počítače k Azure AD přímo, přejděte na další informace o možnostech Azure Active Directory Join.</span><span class="sxs-lookup"><span data-stu-id="4c959-144">If you are looking for joining the computer to Azure AD directly, please go to Learn about capabilities of Azure Active Directory Join.</span></span>

---

### <a name="workplacejoined--no"></a><span data-ttu-id="4c959-145">WorkplaceJoined: Ne</span><span class="sxs-lookup"><span data-stu-id="4c959-145">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="4c959-146">Toto pole ukazuje, jestli je zařízení zaregistrované s Azure AD, ale jako osobní zařízení (označen jako připojené k síti na pracovišti).</span><span class="sxs-lookup"><span data-stu-id="4c959-146">This field shows whether the device is registered with Azure AD but as a personal device (marked as ‘Workplace Joined’).</span></span> <span data-ttu-id="4c959-147">Pokud tato hodnota by měla zobrazit jako "Ne" pro počítači připojeném k doméně zaregistrován s Azure AD, ale pokud se zobrazí jako Ano znamená, že pracovní nebo školní účet byl přidán před počítače dokončení registrace.</span><span class="sxs-lookup"><span data-stu-id="4c959-147">If this value should show as ‘NO’ for a domain joined computer registered with Azure AD, however if it shows as YES it means that a work or school account was added prior to the computer completing registration.</span></span> <span data-ttu-id="4c959-148">Účet v tomto případě bude ignorován, pokud používáte verzi výročí aktualizace Windows 10 (1607 při spuštění WinVer příkazu v okně "Spustit" nebo okna příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="4c959-148">In this case the account will be ignored if using the Anniversary Update version of Windows 10 (1607 when running the WinVer command in the ‘Run’ window or a command prompt window).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="4c959-149">WamDefaultSet: Ano a AzureADPrt: Ano</span><span class="sxs-lookup"><span data-stu-id="4c959-149">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="4c959-150">Tato pole ukazují, že uživatel byl úspěšně ověřen do služby Azure AD při přihlášení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="4c959-150">These fields show that the user has successfully authenticated to Azure AD upon signing in to the device.</span></span> <span data-ttu-id="4c959-151">Pokud se zobrazí "Ne", že toto jsou možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="4c959-151">If they show ‘NO’ the following are possible causes:</span></span>

- <span data-ttu-id="4c959-152">Klíč chybný úložiště (STK) v čipu TPM, které jsou přidružené k zařízení na základě zápisu (Kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).</span><span class="sxs-lookup"><span data-stu-id="4c959-152">Bad storage key (STK) in TPM associated with the device upon registration (check the KeySignTest while running elevated).</span></span>

- <span data-ttu-id="4c959-153">Alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="4c959-153">Alternate Login ID</span></span>

- <span data-ttu-id="4c959-154">Server Proxy protokolu HTTP nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="4c959-154">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c959-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c959-155">Next steps</span></span>

<span data-ttu-id="4c959-156">Další informace najdete v tématu [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="4c959-156">For more information, see the [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 