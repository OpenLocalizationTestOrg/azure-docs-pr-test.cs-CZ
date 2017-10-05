---
title: "Řešení potíží s synchronizace hesel s Azure AD Connect sync | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak řešit potíže heslo."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 33fa6a8867764975a57b8727e7705529d1d7506a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a><span data-ttu-id="51c84-103">Řešení potíží s synchronizace hesel s synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="51c84-103">Troubleshoot password synchronization with Azure AD Connect sync</span></span>
<span data-ttu-id="51c84-104">Toto téma popisuje kroky pro řešení potíží se synchronizací hesla.</span><span class="sxs-lookup"><span data-stu-id="51c84-104">This topic provides steps for how to troubleshoot issues with password synchronization.</span></span> <span data-ttu-id="51c84-105">Pokud hesla se nesynchronizují podle očekávání, může být buď pro podmnožinu uživatelů, nebo pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="51c84-105">If passwords are not synchronizing as expected, it can be either for a subset of users or for all users.</span></span> <span data-ttu-id="51c84-106">Pro Azure Active Directory (Azure AD) připojit nasazení s verzí 1.1.524.0 nebo novější, je nyní k dispozici diagnostické rutina, můžete použít k řešení potíží se synchronizací hesla:</span><span class="sxs-lookup"><span data-stu-id="51c84-106">For Azure Active Directory (Azure AD) Connect deployment with version 1.1.524.0 or later, there is now a diagnostic cmdlet that you can use to troubleshoot password synchronization issues:</span></span>

* <span data-ttu-id="51c84-107">Pokud máte potíže, kde jsou synchronizovány žádná hesla, podívejte se na [bez hesla se synchronizují: řešení potíží pomocí diagnostiky rutina](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) části.</span><span class="sxs-lookup"><span data-stu-id="51c84-107">If you have an issue where no passwords are synchronized, refer to the [No passwords are synchronized: troubleshoot by using the diagnostic cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

* <span data-ttu-id="51c84-108">Pokud máte potíže s jednotlivé objekty, podívejte se na [jeden objekt není synchronizace hesel: řešení potíží pomocí diagnostiky rutina](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) části.</span><span class="sxs-lookup"><span data-stu-id="51c84-108">If you have an issue with individual objects, refer to the [One object is not synchronizing passwords: troubleshoot by using the diagnostic cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

<span data-ttu-id="51c84-109">Pro starší verze aplikace Azure AD Connect nasazení:</span><span class="sxs-lookup"><span data-stu-id="51c84-109">For older versions of Azure AD Connect deployment:</span></span>

* <span data-ttu-id="51c84-110">Pokud máte potíže, kde jsou synchronizovány žádná hesla, přečtěte si [bez hesla se synchronizují: Ruční postup řešení potíží](#no-passwords-are-synchronized-manual-troubleshooting-steps) části.</span><span class="sxs-lookup"><span data-stu-id="51c84-110">If you have an issue where no passwords are synchronized, refer to the [No passwords are synchronized: manual troubleshooting steps](#no-passwords-are-synchronized-manual-troubleshooting-steps) section.</span></span>

* <span data-ttu-id="51c84-111">Pokud máte potíže s jednotlivé objekty, podívejte se na [jeden objekt není synchronizace hesel: Ruční postup řešení potíží](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) části.</span><span class="sxs-lookup"><span data-stu-id="51c84-111">If you have an issue with individual objects, refer to the [One object is not synchronizing passwords: manual troubleshooting steps](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) section.</span></span>

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a><span data-ttu-id="51c84-112">Žádná hesla se synchronizují: řešení potíží pomocí rutiny diagnostiky</span><span class="sxs-lookup"><span data-stu-id="51c84-112">No passwords are synchronized: troubleshoot by using the diagnostic cmdlet</span></span>
<span data-ttu-id="51c84-113">Můžete použít `Invoke-ADSyncDiagnostics` rutiny zjistěte, proč jsou synchronizovány žádná hesla.</span><span class="sxs-lookup"><span data-stu-id="51c84-113">You can use the `Invoke-ADSyncDiagnostics` cmdlet to figure out why no passwords are synchronized.</span></span>

> [!NOTE]
> <span data-ttu-id="51c84-114">`Invoke-ADSyncDiagnostics` Rutina je dostupné pouze pro verze služby Azure AD Connect 1.1.524.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="51c84-114">The `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-the-diagnostics-cmdlet"></a><span data-ttu-id="51c84-115">Spusťte rutinu diagnostiky</span><span class="sxs-lookup"><span data-stu-id="51c84-115">Run the diagnostics cmdlet</span></span>
<span data-ttu-id="51c84-116">Řešení problémů, kde jsou synchronizovány žádná hesla:</span><span class="sxs-lookup"><span data-stu-id="51c84-116">To troubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="51c84-117">Otevřete novou relaci prostředí Windows PowerShell na serveru s Azure AD Connect **spustit jako správce** možnost.</span><span class="sxs-lookup"><span data-stu-id="51c84-117">Open a new Windows PowerShell session on your Azure AD Connect server with the **Run as Administrator** option.</span></span>

2. <span data-ttu-id="51c84-118">Spustit `Set-ExecutionPolicy RemoteSigned` nebo `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="51c84-118">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="51c84-119">Spusťte `Import-Module ADSyncDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="51c84-119">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="51c84-120">Spusťte `Invoke-ADSyncDiagnostics -PasswordSync`.</span><span class="sxs-lookup"><span data-stu-id="51c84-120">Run `Invoke-ADSyncDiagnostics -PasswordSync`.</span></span>

### <a name="understand-the-results-of-the-cmdlet"></a><span data-ttu-id="51c84-121">Pochopení výsledky rutiny</span><span class="sxs-lookup"><span data-stu-id="51c84-121">Understand the results of the cmdlet</span></span>
<span data-ttu-id="51c84-122">Diagnostické rutina provede následující kontroly:</span><span class="sxs-lookup"><span data-stu-id="51c84-122">The diagnostic cmdlet performs the following checks:</span></span>

* <span data-ttu-id="51c84-123">Ověří, že funkce Synchronizace hesla je povolen pro vašeho tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51c84-123">Validates that the password synchronization feature is enabled for your Azure AD tenant.</span></span>

* <span data-ttu-id="51c84-124">Ověří, zda není server Azure AD Connect v pracovním režimu.</span><span class="sxs-lookup"><span data-stu-id="51c84-124">Validates that the Azure AD Connect server is not in staging mode.</span></span>

* <span data-ttu-id="51c84-125">Pro každý existující místní konektor služby Active Directory (který odpovídá existující doménové struktury služby Active Directory):</span><span class="sxs-lookup"><span data-stu-id="51c84-125">For each existing on-premises Active Directory connector (which corresponds to an existing Active Directory forest):</span></span>

   * <span data-ttu-id="51c84-126">Ověří, že je povolena funkce Synchronizace hesla.</span><span class="sxs-lookup"><span data-stu-id="51c84-126">Validates that the password synchronization feature is enabled.</span></span>
   
   * <span data-ttu-id="51c84-127">Vyhledá události prezenční signál synchronizace hesel v protokolech událostí aplikace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="51c84-127">Searches for password synchronization heartbeat events in the Windows Application Event logs.</span></span>

   * <span data-ttu-id="51c84-128">Pro každou doménu služby Active Directory v rámci konektoru služby Active Directory v místě:</span><span class="sxs-lookup"><span data-stu-id="51c84-128">For each Active Directory domain under the on-premises Active Directory connector:</span></span>

      * <span data-ttu-id="51c84-129">Ověří, že doména je dostupný ze serveru, Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="51c84-129">Validates that the domain is reachable from the Azure AD Connect server.</span></span>

      * <span data-ttu-id="51c84-130">Ověří, že má správné uživatelské jméno, heslo a oprávněních pro synchronizaci hesel účtů služby Active Directory Domain Services (AD DS) používaných místní konektor služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="51c84-130">Validates that the Active Directory Domain Services (AD DS) accounts used by the on-premises Active Directory connector has the correct username, password, and permissions required for password synchronization.</span></span>

<span data-ttu-id="51c84-131">Následující diagram znázorňuje výsledky rutiny pro topologie s jednou doménou, místní služby Active Directory:</span><span class="sxs-lookup"><span data-stu-id="51c84-131">The following diagram illustrates the results of the cmdlet for a single-domain, on-premises Active Directory topology:</span></span>

![Výstup diagnostiky pro synchronizace hesel](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

<span data-ttu-id="51c84-133">Zbývající část tohoto oddílu popisuje konkrétní výsledky, které se vrátí pomocí rutiny a odpovídající problémy.</span><span class="sxs-lookup"><span data-stu-id="51c84-133">The rest of this section describes specific results that are returned by the cmdlet and corresponding issues.</span></span>

#### <a name="password-synchronization-feature-isnt-enabled"></a><span data-ttu-id="51c84-134">Funkce Synchronizace hesla není povoleno.</span><span class="sxs-lookup"><span data-stu-id="51c84-134">Password synchronization feature isn't enabled</span></span>
<span data-ttu-id="51c84-135">Pokud jste nepovolili synchronizace hesel pomocí Průvodce službou Azure AD Connect, je vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-135">If you haven't enabled password synchronization by using the Azure AD Connect wizard, the following error is returned:</span></span>

![Synchronizace hesla není povoleno.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a><span data-ttu-id="51c84-137">Server Azure AD Connect je v pracovním režimu</span><span class="sxs-lookup"><span data-stu-id="51c84-137">Azure AD Connect server is in staging mode</span></span>
<span data-ttu-id="51c84-138">Pokud je server Azure AD Connect v pracovním režimu, synchronizace hesel je dočasně zakázána a vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-138">If the Azure AD Connect server is in staging mode, password synchronization is temporarily disabled, and the following error is returned:</span></span>

![Server Azure AD Connect je v pracovním režimu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a><span data-ttu-id="51c84-140">Žádné události prezenční signál synchronizace hesel</span><span class="sxs-lookup"><span data-stu-id="51c84-140">No password synchronization heartbeat events</span></span>
<span data-ttu-id="51c84-141">Každý konektor služby Active Directory v místě má svou vlastní heslo synchronizačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="51c84-141">Each on-premises Active Directory connector has its own password synchronization channel.</span></span> <span data-ttu-id="51c84-142">Při kanál synchronizace hesla je vytvořen, a nejsou k dispozici žádné změny hesla k synchronizaci, prezenční signál události (EventId 654) generuje každých 30 minut v protokolu událostí aplikace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="51c84-142">When the password synchronization channel is established and there aren't any password changes to be synchronized, a heartbeat event (EventId 654) is generated once every 30 minutes under the Windows Application Event Log.</span></span> <span data-ttu-id="51c84-143">Pro každý konektor služby Active Directory v místě rutina vyhledá odpovídající události prezenčního signálu v posledních tří hodin.</span><span class="sxs-lookup"><span data-stu-id="51c84-143">For each on-premises Active Directory connector, the cmdlet searches for corresponding heartbeat events in the past three hours.</span></span> <span data-ttu-id="51c84-144">Pokud se nenajde žádný prezenční signál události, je vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-144">If no heartbeat event is found, the following error is returned:</span></span>

![Žádné vysílat synchronizace hesla porazit událostí](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a><span data-ttu-id="51c84-146">AD DS účet nemá správná oprávnění</span><span class="sxs-lookup"><span data-stu-id="51c84-146">AD DS account does not have correct permissions</span></span>
<span data-ttu-id="51c84-147">Pokud účet služby AD DS, který se používá místní konektor služby Active Directory k synchronizaci hodnot hash hesel nemá příslušná oprávnění, je vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-147">If the AD DS account that's used by the on-premises Active Directory connector to synchronize password hashes does not have the appropriate permissions, the following error is returned:</span></span>

![Nesprávná pověření](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a><span data-ttu-id="51c84-149">Nesprávné uživatelské jméno účtu služby AD DS nebo heslo</span><span class="sxs-lookup"><span data-stu-id="51c84-149">Incorrect AD DS account username or password</span></span>
<span data-ttu-id="51c84-150">Pokud účet služby AD DS používá místní konektor služby Active Directory k synchronizaci hodnot hash hesel má nesprávné uživatelské jméno nebo heslo, je vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-150">If the AD DS account used by the on-premises Active Directory connector to synchronize password hashes has an incorrect username or password, the following error is returned:</span></span>

![Nesprávná pověření](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a><span data-ttu-id="51c84-152">Jeden objekt není synchronizace hesel: řešení potíží pomocí rutiny diagnostiky</span><span class="sxs-lookup"><span data-stu-id="51c84-152">One object is not synchronizing passwords: troubleshoot by using the diagnostic cmdlet</span></span>
<span data-ttu-id="51c84-153">Můžete použít `Invoke-ADSyncDiagnostics` rutiny zjistit, proč se jeden objekt synchronizace hesel.</span><span class="sxs-lookup"><span data-stu-id="51c84-153">You can use the `Invoke-ADSyncDiagnostics` cmdlet to determine why one object is not synchronizing passwords.</span></span>

> [!NOTE]
> <span data-ttu-id="51c84-154">`Invoke-ADSyncDiagnostics` Rutina je dostupné pouze pro verze služby Azure AD Connect 1.1.524.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="51c84-154">The `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-the-diagnostics-cmdlet"></a><span data-ttu-id="51c84-155">Spusťte rutinu diagnostiky</span><span class="sxs-lookup"><span data-stu-id="51c84-155">Run the diagnostics cmdlet</span></span>
<span data-ttu-id="51c84-156">Řešení problémů, kde jsou synchronizovány žádná hesla:</span><span class="sxs-lookup"><span data-stu-id="51c84-156">To troubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="51c84-157">Otevřete novou relaci prostředí Windows PowerShell na serveru s Azure AD Connect **spustit jako správce** možnost.</span><span class="sxs-lookup"><span data-stu-id="51c84-157">Open a new Windows PowerShell session on your Azure AD Connect server with the **Run as Administrator** option.</span></span>

2. <span data-ttu-id="51c84-158">Spustit `Set-ExecutionPolicy RemoteSigned` nebo `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="51c84-158">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="51c84-159">Spusťte `Import-Module ADSyncDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="51c84-159">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="51c84-160">Spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="51c84-160">Run the following cmdlet:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   <span data-ttu-id="51c84-161">Například:</span><span class="sxs-lookup"><span data-stu-id="51c84-161">For example:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-the-results-of-the-cmdlet"></a><span data-ttu-id="51c84-162">Pochopení výsledky rutiny</span><span class="sxs-lookup"><span data-stu-id="51c84-162">Understand the results of the cmdlet</span></span>
<span data-ttu-id="51c84-163">Diagnostické rutina provede následující kontroly:</span><span class="sxs-lookup"><span data-stu-id="51c84-163">The diagnostic cmdlet performs the following checks:</span></span>

* <span data-ttu-id="51c84-164">Prozkoumá stav objektu služby Active Directory v prostoru konektoru služby Active Directory, úložiště Metaverse a Azure AD prostoru konektoru.</span><span class="sxs-lookup"><span data-stu-id="51c84-164">Examines the state of the Active Directory object in the Active Directory connector space, Metaverse, and Azure AD connector space.</span></span>

* <span data-ttu-id="51c84-165">Ověří, že jsou pravidla synchronizace s synchronizace hesel povolena a u objektu služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="51c84-165">Validates that there are synchronization rules with password synchronization enabled and applied to the Active Directory object.</span></span>

* <span data-ttu-id="51c84-166">Pokusí se načíst a zobrazit výsledky poslední pokus o synchronizaci hesla pro objekt.</span><span class="sxs-lookup"><span data-stu-id="51c84-166">Attempts to retrieve and display the results of the last attempt to synchronize the password for the object.</span></span>

<span data-ttu-id="51c84-167">Následující diagram znázorňuje výsledky rutiny při řešení potíží s synchronizace hesel pro jediný objekt:</span><span class="sxs-lookup"><span data-stu-id="51c84-167">The following diagram illustrates the results of the cmdlet when troubleshooting password synchronization for a single object:</span></span>

![Výstup diagnostiky pro synchronizaci hesel - jednoho objektu.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

<span data-ttu-id="51c84-169">Zbývající část tohoto oddílu popisuje konkrétní výsledky vrácené rutiny a odpovídající problémy.</span><span class="sxs-lookup"><span data-stu-id="51c84-169">The rest of this section describes specific results returned by the cmdlet and corresponding issues.</span></span>

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a><span data-ttu-id="51c84-170">Objekt služby Active Directory není exportovány do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="51c84-170">The Active Directory object isn't exported to Azure AD</span></span>
<span data-ttu-id="51c84-171">Synchronizace hesel pro tento účet místní služby Active Directory se nezdaří, protože neexistuje žádný odpovídající objektů v klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51c84-171">Password synchronization for this on-premises Active Directory account fails because there is no corresponding object in the Azure AD tenant.</span></span> <span data-ttu-id="51c84-172">Je vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-172">The following error is returned:</span></span>

![Chybí objekt Azure AD](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a><span data-ttu-id="51c84-174">Uživatel má dočasné heslo.</span><span class="sxs-lookup"><span data-stu-id="51c84-174">User has a temporary password</span></span>
<span data-ttu-id="51c84-175">Azure AD Connect v současné době nepodporuje synchronizaci dočasná hesla s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51c84-175">Currently, Azure AD Connect does not support synchronizing temporary passwords with Azure AD.</span></span> <span data-ttu-id="51c84-176">Heslo se považuje za dočasnou Pokud **při dalším přihlášení změnit heslo** je možnost nastavena na místní uživatele služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="51c84-176">A password is considered to be temporary if the **Change password at next logon** option is set on the on-premises Active Directory user.</span></span> <span data-ttu-id="51c84-177">Je vrácena následující chyba:</span><span class="sxs-lookup"><span data-stu-id="51c84-177">The following error is returned:</span></span>

![Dočasné heslo není exportovali.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a><span data-ttu-id="51c84-179">Výsledky poslední pokus o synchronizaci hesla nejsou k dispozici</span><span class="sxs-lookup"><span data-stu-id="51c84-179">Results of last attempt to synchronize password aren't available</span></span>
<span data-ttu-id="51c84-180">Ve výchozím nastavení Azure AD Connect ukládá výsledky pokusů o zadání synchronizace hesla sedm dní.</span><span class="sxs-lookup"><span data-stu-id="51c84-180">By default, Azure AD Connect stores the results of password synchronization attempts for seven days.</span></span> <span data-ttu-id="51c84-181">Pokud žádné výsledky nejsou dostupné pro vybraný objekt služby Active Directory, se vrátí následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="51c84-181">If there are no results available for the selected Active Directory object, the following warning is returned:</span></span>

![Výstup diagnostiky pro jediný objekt - žádná historie synchronizace hesla](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a><span data-ttu-id="51c84-183">Žádná hesla se synchronizují: Ruční postup řešení potíží</span><span class="sxs-lookup"><span data-stu-id="51c84-183">No passwords are synchronized: manual troubleshooting steps</span></span>
<span data-ttu-id="51c84-184">Postupujte podle těchto kroků, chcete-li zjistit, proč jsou synchronizovány žádná hesla:</span><span class="sxs-lookup"><span data-stu-id="51c84-184">Follow these steps to determine why no passwords are synchronized:</span></span>

1. <span data-ttu-id="51c84-185">Je server Connect v [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode)?</span><span class="sxs-lookup"><span data-stu-id="51c84-185">Is the Connect server in [staging mode](active-directory-aadconnectsync-operations.md#staging-mode)?</span></span> <span data-ttu-id="51c84-186">Server v pracovní režimu nebyla synchronizována všechna hesla.</span><span class="sxs-lookup"><span data-stu-id="51c84-186">A server in staging mode does not synchronize any passwords.</span></span>

2. <span data-ttu-id="51c84-187">Spusťte skript [získat stav nastavení synchronizace hesla](#get-the-status-of-password-sync-settings) části.</span><span class="sxs-lookup"><span data-stu-id="51c84-187">Run the script in the [Get the status of password sync settings](#get-the-status-of-password-sync-settings) section.</span></span> <span data-ttu-id="51c84-188">Poskytuje přehled o konfiguraci synchronizace hesel.</span><span class="sxs-lookup"><span data-stu-id="51c84-188">It gives you an overview of the password sync configuration.</span></span>  

    ![Výstup skriptu prostředí PowerShell z nastavení synchronizace hesla](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. <span data-ttu-id="51c84-190">Pokud není povolená funkce ve službě Azure AD, nebo pokud není povolené kanál stav synchronizace, spusťte Průvodce instalací služby Connect.</span><span class="sxs-lookup"><span data-stu-id="51c84-190">If the feature is not enabled in Azure AD or if the sync channel status is not enabled, run the Connect installation wizard.</span></span> <span data-ttu-id="51c84-191">Vyberte **přizpůsobit možnosti synchronizace**a zrušte výběr synchronizaci hesel. Tato změna dočasně zakáže funkci.</span><span class="sxs-lookup"><span data-stu-id="51c84-191">Select **Customize synchronization options**, and unselect password sync. This change temporarily disables the feature.</span></span> <span data-ttu-id="51c84-192">Potom spusťte znovu Průvodce a znovu povolte synchronizaci hesla. Spuštění skriptu znovu k ověření, že konfigurace je správná.</span><span class="sxs-lookup"><span data-stu-id="51c84-192">Then run the wizard again and re-enable password sync. Run the script again to verify that the configuration is correct.</span></span>

4. <span data-ttu-id="51c84-193">Vyhledejte v protokolu událostí pro chyby.</span><span class="sxs-lookup"><span data-stu-id="51c84-193">Look in the event log for errors.</span></span> <span data-ttu-id="51c84-194">Podívejte se na následující události, které by mohly ukazovat na problém:</span><span class="sxs-lookup"><span data-stu-id="51c84-194">Look for the following events, which would indicate a problem:</span></span>
    * <span data-ttu-id="51c84-195">Zdroje: ID "Synchronizace adresáře": 0, 611, 652, 655 těchto událostí došlo k potížím. připojení.</span><span class="sxs-lookup"><span data-stu-id="51c84-195">Source: "Directory synchronization" ID: 0, 611, 652, 655 If you see these events, you have a connectivity problem.</span></span> <span data-ttu-id="51c84-196">Zprávy protokolu událostí obsahuje informace doménové struktuře, kde došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="51c84-196">The event log message contains forest information where you have a problem.</span></span> <span data-ttu-id="51c84-197">Další informace najdete v tématu [problém s připojením k](#connectivity problem).</span><span class="sxs-lookup"><span data-stu-id="51c84-197">For more information, see [Connectivity problem](#connectivity problem).</span></span>

5. <span data-ttu-id="51c84-198">Pokud se neobjevil žádný prezenční signál nebo nic jiného fungovala, spusťte [spustit úplnou synchronizaci hesel všech](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="51c84-198">If you see no heartbeat or if nothing else worked, run [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span> <span data-ttu-id="51c84-199">Skript spusťte jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="51c84-199">Run the script only once.</span></span>

6. <span data-ttu-id="51c84-200">Najdete v článku [řešení potíží s jeden objekt, který není synchronizace hesel](#one-object-is-not-synchronizing-passwords) části.</span><span class="sxs-lookup"><span data-stu-id="51c84-200">See the [Troubleshoot one object that is not synchronizing passwords](#one-object-is-not-synchronizing-passwords) section.</span></span>

### <a name="connectivity-problems"></a><span data-ttu-id="51c84-201">Potíže s připojením k</span><span class="sxs-lookup"><span data-stu-id="51c84-201">Connectivity problems</span></span>

<span data-ttu-id="51c84-202">Máte připojení k Azure AD?</span><span class="sxs-lookup"><span data-stu-id="51c84-202">Do you have connectivity with Azure AD?</span></span>

<span data-ttu-id="51c84-203">Má účet požadovaná oprávnění ke čtení hodnot hash hesel ve všech doménách?</span><span class="sxs-lookup"><span data-stu-id="51c84-203">Does the account have required permissions to read the password hashes in all domains?</span></span> <span data-ttu-id="51c84-204">Pokud jste nainstalovali Connect pomocí expresního nastavení, oprávnění by měl být již správné.</span><span class="sxs-lookup"><span data-stu-id="51c84-204">If you installed Connect by using Express settings, the permissions should already be correct.</span></span> 

<span data-ttu-id="51c84-205">Pokud jste použili vlastní instalaci, nastavte oprávnění ručně následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="51c84-205">If you used custom installation, set the permissions manually by doing the following:</span></span>
    
1. <span data-ttu-id="51c84-206">Chcete-li najít má účet používaný službou konektoru služby Active Directory, spusťte **Synchronization Service Manager**.</span><span class="sxs-lookup"><span data-stu-id="51c84-206">To find the account used by the Active Directory connector, start **Synchronization Service Manager**.</span></span> 
 
2. <span data-ttu-id="51c84-207">Přejděte na **konektory**a poté vyhledejte doménové struktury služby Active Directory místní řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="51c84-207">Go to **Connectors**, and then search for the on-premises Active Directory forest you are troubleshooting.</span></span> 
 
3. <span data-ttu-id="51c84-208">Vyberte konektor a potom klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="51c84-208">Select the connector, and then click **Properties**.</span></span> 
 
4. <span data-ttu-id="51c84-209">Přejděte na **připojit k doménové struktuře služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="51c84-209">Go to **Connect to Active Directory Forest**.</span></span>  
    
    ![Účet používaný službou konektoru služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    <span data-ttu-id="51c84-211">Všimněte si, uživatelské jméno a doménu, kde je umístěn účet.</span><span class="sxs-lookup"><span data-stu-id="51c84-211">Note the username and the domain where the account is located.</span></span>
    
5. <span data-ttu-id="51c84-212">Spustit **Active Directory Users and Computers**a potom ověřte, zda má účet jste našli dříve oprávnění postupujte podle kroků nastavte v kořenovém adresáři všechny domény v doménové struktuře:</span><span class="sxs-lookup"><span data-stu-id="51c84-212">Start **Active Directory Users and Computers**, and then verify that the account you found earlier has the follow permissions set at the root of all domains in your forest:</span></span>
    * <span data-ttu-id="51c84-213">Replikovat změny adresáře</span><span class="sxs-lookup"><span data-stu-id="51c84-213">Replicate Directory Changes</span></span>
    * <span data-ttu-id="51c84-214">Replikace adresáře všechny změny</span><span class="sxs-lookup"><span data-stu-id="51c84-214">Replicate Directory Changes All</span></span>

6. <span data-ttu-id="51c84-215">Jsou dostupné přes Azure AD Connect řadiče domény?</span><span class="sxs-lookup"><span data-stu-id="51c84-215">Are the domain controllers reachable by Azure AD Connect?</span></span> <span data-ttu-id="51c84-216">Pokud server připojit se nelze připojit na všechny řadiče domény, nakonfigurujte **používat jenom upřednostňovaného řadiče domény**.</span><span class="sxs-lookup"><span data-stu-id="51c84-216">If the Connect server cannot connect to all domain controllers, configure **Only use preferred domain controller**.</span></span>  
    
    ![Řadič domény používané konektorem služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. <span data-ttu-id="51c84-218">Přejděte zpět na **Synchronization Service Manager** a **konfigurace oddílu adresáře**.</span><span class="sxs-lookup"><span data-stu-id="51c84-218">Go back to **Synchronization Service Manager** and **Configure Directory Partition**.</span></span> 
 
8. <span data-ttu-id="51c84-219">Vyberte doménu v **Vybrat oddíly adresářů**, vyberte **používat jenom řadiče domény upřednostňované** zaškrtněte políčko a potom klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="51c84-219">Select your domain in **Select directory partitions**, select the **Only use preferred domain controllers** check box, and then click **Configure**.</span></span> 

9. <span data-ttu-id="51c84-220">V seznamu zadejte řadiče domény, které by měly používat Connect pro synchronizaci hesel. Stejný seznam se používá pro import a export také.</span><span class="sxs-lookup"><span data-stu-id="51c84-220">In the list, enter the domain controllers that Connect should use for password sync. The same list is used for import and export as well.</span></span> <span data-ttu-id="51c84-221">Proveďte tyto kroky pro všechny domény.</span><span class="sxs-lookup"><span data-stu-id="51c84-221">Do these steps for all your domains.</span></span>

10. <span data-ttu-id="51c84-222">Pokud skript ukazuje, že neexistuje žádný prezenční signál, spusťte skript v [spustit úplnou synchronizaci hesel všech](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="51c84-222">If the script shows that there is no heartbeat, run the script in [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span>

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a><span data-ttu-id="51c84-223">Jeden objekt není synchronizace hesel: Ruční postup řešení potíží</span><span class="sxs-lookup"><span data-stu-id="51c84-223">One object is not synchronizing passwords: manual troubleshooting steps</span></span>
<span data-ttu-id="51c84-224">Můžete snadno potíže heslo synchronizace kontrolou stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="51c84-224">You can easily troubleshoot password synchronization issues by reviewing the status of an object.</span></span>

1. <span data-ttu-id="51c84-225">V **Active Directory Users and Computers**, vyhledejte uživatele a ověřte, že **musí uživatel změnit heslo při příštím přihlášení** zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="51c84-225">In **Active Directory Users and Computers**, search for the user, and then verify that the **User must change password at next logon** check box is cleared.</span></span>  

    ![Produktivní hesla služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    <span data-ttu-id="51c84-227">Pokud je zaškrtnuté políčko, požádejte uživatele přihlásit a změnit heslo.</span><span class="sxs-lookup"><span data-stu-id="51c84-227">If the check box is selected, ask the user to sign in and change the password.</span></span> <span data-ttu-id="51c84-228">Dočasná hesla nejsou synchronizovány s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51c84-228">Temporary passwords are not synchronized with Azure AD.</span></span>

2. <span data-ttu-id="51c84-229">Pokud ve službě Active Directory správné heslo, postupujte podle uživatele v synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="51c84-229">If the password looks correct in Active Directory, follow the user in the sync engine.</span></span> <span data-ttu-id="51c84-230">Pomocí následujících uživatele z místní služby Active Directory do Azure AD, se zobrazí, zda je u objektu popisný chyba.</span><span class="sxs-lookup"><span data-stu-id="51c84-230">By following the user from on-premises Active Directory to Azure AD, you can see whether there is a descriptive error on the object.</span></span>

    <span data-ttu-id="51c84-231">a.</span><span class="sxs-lookup"><span data-stu-id="51c84-231">a.</span></span> <span data-ttu-id="51c84-232">Spuštění [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="51c84-232">Start the [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span></span>

    <span data-ttu-id="51c84-233">b.</span><span class="sxs-lookup"><span data-stu-id="51c84-233">b.</span></span> <span data-ttu-id="51c84-234">Klikněte na tlačítko **konektory**.</span><span class="sxs-lookup"><span data-stu-id="51c84-234">Click **Connectors**.</span></span>

    <span data-ttu-id="51c84-235">c.</span><span class="sxs-lookup"><span data-stu-id="51c84-235">c.</span></span> <span data-ttu-id="51c84-236">Vyberte **konektor služby Active Directory** kde se uživatel nachází.</span><span class="sxs-lookup"><span data-stu-id="51c84-236">Select the **Active Directory Connector** where the user is located.</span></span>

    <span data-ttu-id="51c84-237">d.</span><span class="sxs-lookup"><span data-stu-id="51c84-237">d.</span></span> <span data-ttu-id="51c84-238">Vyberte **hledání prostoru konektoru**.</span><span class="sxs-lookup"><span data-stu-id="51c84-238">Select **Search Connector Space**.</span></span>

    <span data-ttu-id="51c84-239">e.</span><span class="sxs-lookup"><span data-stu-id="51c84-239">e.</span></span> <span data-ttu-id="51c84-240">V **oboru** vyberte **rozlišující název nebo ukotvení**a pak zadejte úplný název DN uživatele řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="51c84-240">In the **Scope** box, select **DN or Anchor**, and then enter the full DN of the user you are troubleshooting.</span></span>

    ![Vyhledejte uživatele v prostoru konektoru s rozlišujícím názvem](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    <span data-ttu-id="51c84-242">f.</span><span class="sxs-lookup"><span data-stu-id="51c84-242">f.</span></span> <span data-ttu-id="51c84-243">Vyhledejte uživatele, které hledáte a potom klikněte na **vlastnosti** zobrazíte všechny atributy.</span><span class="sxs-lookup"><span data-stu-id="51c84-243">Locate the user you are looking for, and then click **Properties** to see all the attributes.</span></span> <span data-ttu-id="51c84-244">Pokud uživatel není ve výsledcích hledání, ověřte vaší [pravidla filtrování](active-directory-aadconnectsync-configure-filtering.md) a ujistěte se, že spustíte [použít a ověřte změny](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) pro uživatele, než se objeví v připojení.</span><span class="sxs-lookup"><span data-stu-id="51c84-244">If the user is not in the search result, verify your [filtering rules](active-directory-aadconnectsync-configure-filtering.md) and make sure that you run [Apply and verify changes](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) for the user to appear in Connect.</span></span>

    <span data-ttu-id="51c84-245">g.</span><span class="sxs-lookup"><span data-stu-id="51c84-245">g.</span></span> <span data-ttu-id="51c84-246">Chcete-li zobrazit podrobnosti synchronizace hesla objektu za uplynulý týden, klikněte na tlačítko **protokolu**.</span><span class="sxs-lookup"><span data-stu-id="51c84-246">To see the password sync details of the object for the past week, click **Log**.</span></span>  

    ![Podrobnosti protokolu objektu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    <span data-ttu-id="51c84-248">Pokud protokol objektu je prázdná, Azure AD Connect nelze číst hodnotu hash hesla ze služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="51c84-248">If the object log is empty, Azure AD Connect has been unable to read the password hash from Active Directory.</span></span> <span data-ttu-id="51c84-249">Pokračovat v odstraňování problémů s [k chybám připojení](#connectivity-errors).</span><span class="sxs-lookup"><span data-stu-id="51c84-249">Continue your troubleshooting with [Connectivity Errors](#connectivity-errors).</span></span> <span data-ttu-id="51c84-250">Pokud se zobrazí jakoukoli jinou hodnotu než **úspěch**, naleznete v tabulce v [protokolu synchronizace hesla](#password-sync-log).</span><span class="sxs-lookup"><span data-stu-id="51c84-250">If you see any other value than **success**, refer to the table in [Password sync log](#password-sync-log).</span></span>

    <span data-ttu-id="51c84-251">h.</span><span class="sxs-lookup"><span data-stu-id="51c84-251">h.</span></span> <span data-ttu-id="51c84-252">Vyberte **rodokmenu** kartě a ujistěte se, že aspoň jeden synchronizační pravidlo v **PasswordSync** sloupec je **True**.</span><span class="sxs-lookup"><span data-stu-id="51c84-252">Select the **lineage** tab, and make sure that at least one sync rule in the **PasswordSync** column is **True**.</span></span> <span data-ttu-id="51c84-253">Ve výchozím nastavení je název pravidla synchronizace **v ze služby Active Directory - uživatele AccountEnabled**.</span><span class="sxs-lookup"><span data-stu-id="51c84-253">In the default configuration, the name of the sync rule is **In from AD - User AccountEnabled**.</span></span>  

    ![Rodokmenu informací o uživateli](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    <span data-ttu-id="51c84-255">i.</span><span class="sxs-lookup"><span data-stu-id="51c84-255">i.</span></span> <span data-ttu-id="51c84-256">Klikněte na tlačítko **vlastnosti objektu úložiště Metaverse** zobrazíte seznam atributů uživatele.</span><span class="sxs-lookup"><span data-stu-id="51c84-256">Click **Metaverse Object Properties** to display a list of user attributes.</span></span>  

    ![Informace o úložiště Metaverse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    <span data-ttu-id="51c84-258">Ověřte, zda je žádné **cloudFiltered** atribut přítomen.</span><span class="sxs-lookup"><span data-stu-id="51c84-258">Verify that there is no **cloudFiltered** attribute present.</span></span> <span data-ttu-id="51c84-259">Ujistěte se, že domény atributy (domainFQDN a domainNetBios) mají očekávaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="51c84-259">Make sure that the domain attributes (domainFQDN and domainNetBios) have the expected values.</span></span>

    <span data-ttu-id="51c84-260">j.</span><span class="sxs-lookup"><span data-stu-id="51c84-260">j.</span></span> <span data-ttu-id="51c84-261">Klikněte **konektory** kartě. Ujistěte se, najdete v části konektory pro místní služby Active Directory a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51c84-261">Click the **Connectors** tab. Make sure that you see connectors to both on-premises Active Directory and Azure AD.</span></span>

    ![Informace o úložiště Metaverse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    <span data-ttu-id="51c84-263">kB.</span><span class="sxs-lookup"><span data-stu-id="51c84-263">k.</span></span> <span data-ttu-id="51c84-264">Vyberte řádek, který představuje Azure AD, klikněte na tlačítko **vlastnosti**a klikněte **rodokmenu** kartě. Objekt konektoru místa měli odchozí pravidlo **PasswordSync** sloupec nastavený na **True**.</span><span class="sxs-lookup"><span data-stu-id="51c84-264">Select the row that represents Azure AD, click **Properties**, and then click the **Lineage** tab. The connector space object should have an outbound rule in the **PasswordSync** column set to **True**.</span></span> <span data-ttu-id="51c84-265">Ve výchozím nastavení je název pravidla synchronizace **Out aad - připojení uživatele k**.</span><span class="sxs-lookup"><span data-stu-id="51c84-265">In the default configuration, the name of the sync rule is **Out to AAD - User Join**.</span></span>  

    ![Dialogové okno Vlastnosti objektu prostoru konektoru](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a><span data-ttu-id="51c84-267">Protokol synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="51c84-267">Password sync log</span></span>
<span data-ttu-id="51c84-268">Ve sloupci stav může mít následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="51c84-268">The status column can have the following values:</span></span>

| <span data-ttu-id="51c84-269">Status</span><span class="sxs-lookup"><span data-stu-id="51c84-269">Status</span></span> | <span data-ttu-id="51c84-270">Popis</span><span class="sxs-lookup"><span data-stu-id="51c84-270">Description</span></span> |
| --- | --- |
| <span data-ttu-id="51c84-271">Úspěch</span><span class="sxs-lookup"><span data-stu-id="51c84-271">Success</span></span> |<span data-ttu-id="51c84-272">Heslo se úspěšně synchronizoval.</span><span class="sxs-lookup"><span data-stu-id="51c84-272">Password has been successfully synchronized.</span></span> |
| <span data-ttu-id="51c84-273">FilteredByTarget</span><span class="sxs-lookup"><span data-stu-id="51c84-273">FilteredByTarget</span></span> |<span data-ttu-id="51c84-274">Heslo je nastaveno na **musí uživatel změnit heslo při příštím přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="51c84-274">Password is set to **User must change password at next logon**.</span></span> <span data-ttu-id="51c84-275">Heslo nebyl synchronizován.</span><span class="sxs-lookup"><span data-stu-id="51c84-275">Password has not been synchronized.</span></span> |
| <span data-ttu-id="51c84-276">NoTargetConnection</span><span class="sxs-lookup"><span data-stu-id="51c84-276">NoTargetConnection</span></span> |<span data-ttu-id="51c84-277">Žádný objekt v úložišti metaverse nebo v prostoru konektoru služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51c84-277">No object in the metaverse or in the Azure AD connector space.</span></span> |
| <span data-ttu-id="51c84-278">SourceConnectorNotPresent</span><span class="sxs-lookup"><span data-stu-id="51c84-278">SourceConnectorNotPresent</span></span> |<span data-ttu-id="51c84-279">Žádný objekt v prostoru konektoru místní služby Active Directory nalezen.</span><span class="sxs-lookup"><span data-stu-id="51c84-279">No object found in the on-premises Active Directory connector space.</span></span> |
| <span data-ttu-id="51c84-280">TargetNotExportedToDirectory</span><span class="sxs-lookup"><span data-stu-id="51c84-280">TargetNotExportedToDirectory</span></span> |<span data-ttu-id="51c84-281">Objekt v prostoru konektoru služby Azure AD nebyl dosud byla exportována.</span><span class="sxs-lookup"><span data-stu-id="51c84-281">The object in the Azure AD connector space has not yet been exported.</span></span> |
| <span data-ttu-id="51c84-282">MigratedCheckDetailsForMoreInfo</span><span class="sxs-lookup"><span data-stu-id="51c84-282">MigratedCheckDetailsForMoreInfo</span></span> |<span data-ttu-id="51c84-283">Položka protokolu byl vytvořen ještě před sestavení 1.0.9125.0 a je zobrazen v stav starší verze.</span><span class="sxs-lookup"><span data-stu-id="51c84-283">Log entry was created before build 1.0.9125.0 and is shown in its legacy state.</span></span> |
| <span data-ttu-id="51c84-284">Chyba</span><span class="sxs-lookup"><span data-stu-id="51c84-284">Error</span></span> |<span data-ttu-id="51c84-285">Služba vrátila neznámou chybu.</span><span class="sxs-lookup"><span data-stu-id="51c84-285">Service returned an unknown error.</span></span> |
| <span data-ttu-id="51c84-286">Neznámý</span><span class="sxs-lookup"><span data-stu-id="51c84-286">Unknown</span></span> |<span data-ttu-id="51c84-287">Došlo k chybě při pokusu o zpracování dávky hodnot hash hesel.</span><span class="sxs-lookup"><span data-stu-id="51c84-287">An error occurred while trying to process a batch of password hashes.</span></span>  |
| <span data-ttu-id="51c84-288">MissingAttribute</span><span class="sxs-lookup"><span data-stu-id="51c84-288">MissingAttribute</span></span> |<span data-ttu-id="51c84-289">Konkrétní atributy (například hodnota hash protokolu Kerberos) vyžaduje Azure AD Domain Services nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="51c84-289">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services are not available.</span></span> |
| <span data-ttu-id="51c84-290">RetryRequestedByTarget</span><span class="sxs-lookup"><span data-stu-id="51c84-290">RetryRequestedByTarget</span></span> |<span data-ttu-id="51c84-291">Konkrétní atributy (například hodnota hash protokolu Kerberos) vyžaduje Azure AD Domain Services nebyly k dispozici dříve.</span><span class="sxs-lookup"><span data-stu-id="51c84-291">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services were not available previously.</span></span> <span data-ttu-id="51c84-292">Je proveden pokus o opakovanou synchronizaci hodnoty hash hesla uživatele.</span><span class="sxs-lookup"><span data-stu-id="51c84-292">An attempt to resynchronize the user's password hash is made.</span></span> |

## <a name="scripts-to-help-troubleshooting"></a><span data-ttu-id="51c84-293">Skripty, které pomůžou řešení potíží</span><span class="sxs-lookup"><span data-stu-id="51c84-293">Scripts to help troubleshooting</span></span>

### <a name="get-the-status-of-password-sync-settings"></a><span data-ttu-id="51c84-294">Načíst stav nastavení synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="51c84-294">Get the status of password sync settings</span></span>
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a><span data-ttu-id="51c84-295">Spustit úplnou synchronizaci všech hesel</span><span class="sxs-lookup"><span data-stu-id="51c84-295">Trigger a full sync of all passwords</span></span>
> [!NOTE]
> <span data-ttu-id="51c84-296">Tento skript spusťte jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="51c84-296">Run this script only once.</span></span> <span data-ttu-id="51c84-297">Pokud potřebujete spustit více než jednou, je něco jiného problému.</span><span class="sxs-lookup"><span data-stu-id="51c84-297">If you need to run it more than once, something else is the problem.</span></span> <span data-ttu-id="51c84-298">K vyřešení tohoto problému, kontaktujte podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="51c84-298">To troubleshoot the problem, contact Microsoft support.</span></span>

<span data-ttu-id="51c84-299">Pomocí následujícího skriptu můžete aktivovat úplnou synchronizaci všech hesel:</span><span class="sxs-lookup"><span data-stu-id="51c84-299">You can trigger a full sync of all passwords by using the following script:</span></span>

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a><span data-ttu-id="51c84-300">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51c84-300">Next steps</span></span>
* [<span data-ttu-id="51c84-301">Implementace synchronizace hesel s synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="51c84-301">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md)
* [<span data-ttu-id="51c84-302">Azure AD Connect Sync: Přizpůsobení možnosti synchronizace</span><span class="sxs-lookup"><span data-stu-id="51c84-302">Azure AD Connect Sync: Customizing synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="51c84-303">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="51c84-303">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
