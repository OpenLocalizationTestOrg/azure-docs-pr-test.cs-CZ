---
title: Synchronizace hesel aaaTroubleshoot s Azure AD Connect sync | Microsoft Docs
description: "Tento článek obsahuje informace o tom, tootroubleshoot heslo synchronizace problémy."
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
ms.openlocfilehash: 390eafec792cb39251627c14cb754f8bb30035b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a><span data-ttu-id="59958-103">Řešení potíží s synchronizace hesel s synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="59958-103">Troubleshoot password synchronization with Azure AD Connect sync</span></span>
<span data-ttu-id="59958-104">Toto téma popisuje kroky pro jak tootroubleshoot problémy se synchronizace hesel.</span><span class="sxs-lookup"><span data-stu-id="59958-104">This topic provides steps for how tootroubleshoot issues with password synchronization.</span></span> <span data-ttu-id="59958-105">Pokud hesla se nesynchronizují podle očekávání, může být buď pro podmnožinu uživatelů, nebo pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="59958-105">If passwords are not synchronizing as expected, it can be either for a subset of users or for all users.</span></span> <span data-ttu-id="59958-106">Pro Azure Active Directory (Azure AD) připojit nasazení s verzí 1.1.524.0 nebo novější, je nyní k dispozici diagnostické rutina, které můžete použít tootroubleshoot heslo synchronizace problémy:</span><span class="sxs-lookup"><span data-stu-id="59958-106">For Azure Active Directory (Azure AD) Connect deployment with version 1.1.524.0 or later, there is now a diagnostic cmdlet that you can use tootroubleshoot password synchronization issues:</span></span>

* <span data-ttu-id="59958-107">Pokud máte potíže, kde jsou synchronizovány žádná hesla, přečtěte si téma toohello [bez hesla se synchronizují: řešení potíží pomocí diagnostiky rutiny hello](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) části.</span><span class="sxs-lookup"><span data-stu-id="59958-107">If you have an issue where no passwords are synchronized, refer toohello [No passwords are synchronized: troubleshoot by using hello diagnostic cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

* <span data-ttu-id="59958-108">Pokud máte potíže s jednotlivé objekty, přečtěte si téma toohello [jeden objekt není synchronizace hesel: řešení potíží pomocí diagnostiky rutiny hello](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) části.</span><span class="sxs-lookup"><span data-stu-id="59958-108">If you have an issue with individual objects, refer toohello [One object is not synchronizing passwords: troubleshoot by using hello diagnostic cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

<span data-ttu-id="59958-109">Pro starší verze aplikace Azure AD Connect nasazení:</span><span class="sxs-lookup"><span data-stu-id="59958-109">For older versions of Azure AD Connect deployment:</span></span>

* <span data-ttu-id="59958-110">Pokud máte potíže, kde jsou synchronizovány žádná hesla, přečtěte si téma toohello [bez hesla se synchronizují: Ruční postup řešení potíží](#no-passwords-are-synchronized-manual-troubleshooting-steps) části.</span><span class="sxs-lookup"><span data-stu-id="59958-110">If you have an issue where no passwords are synchronized, refer toohello [No passwords are synchronized: manual troubleshooting steps](#no-passwords-are-synchronized-manual-troubleshooting-steps) section.</span></span>

* <span data-ttu-id="59958-111">Pokud máte potíže s jednotlivé objekty, přečtěte si téma toohello [jeden objekt není synchronizace hesel: Ruční postup řešení potíží](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) části.</span><span class="sxs-lookup"><span data-stu-id="59958-111">If you have an issue with individual objects, refer toohello [One object is not synchronizing passwords: manual troubleshooting steps](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) section.</span></span>

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-hello-diagnostic-cmdlet"></a><span data-ttu-id="59958-112">Žádná hesla se synchronizují: řešení potíží pomocí diagnostiky rutiny hello</span><span class="sxs-lookup"><span data-stu-id="59958-112">No passwords are synchronized: troubleshoot by using hello diagnostic cmdlet</span></span>
<span data-ttu-id="59958-113">Můžete použít hello `Invoke-ADSyncDiagnostics` toofigure rutiny se proč bez hesla se synchronizují.</span><span class="sxs-lookup"><span data-stu-id="59958-113">You can use hello `Invoke-ADSyncDiagnostics` cmdlet toofigure out why no passwords are synchronized.</span></span>

> [!NOTE]
> <span data-ttu-id="59958-114">Hello `Invoke-ADSyncDiagnostics` rutina je dostupné pouze pro verze služby Azure AD Connect 1.1.524.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="59958-114">hello `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-hello-diagnostics-cmdlet"></a><span data-ttu-id="59958-115">Spusťte rutinu diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="59958-115">Run hello diagnostics cmdlet</span></span>
<span data-ttu-id="59958-116">tootroubleshoot problémy, kde jsou synchronizovány žádná hesla:</span><span class="sxs-lookup"><span data-stu-id="59958-116">tootroubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="59958-117">Otevřete novou relaci prostředí Windows PowerShell na serveru Azure AD Connect s hello **spustit jako správce** možnost.</span><span class="sxs-lookup"><span data-stu-id="59958-117">Open a new Windows PowerShell session on your Azure AD Connect server with hello **Run as Administrator** option.</span></span>

2. <span data-ttu-id="59958-118">Spustit `Set-ExecutionPolicy RemoteSigned` nebo `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="59958-118">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="59958-119">Spusťte `Import-Module ADSyncDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="59958-119">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="59958-120">Spusťte `Invoke-ADSyncDiagnostics -PasswordSync`.</span><span class="sxs-lookup"><span data-stu-id="59958-120">Run `Invoke-ADSyncDiagnostics -PasswordSync`.</span></span>

### <a name="understand-hello-results-of-hello-cmdlet"></a><span data-ttu-id="59958-121">Pochopení hello výsledky rutiny hello</span><span class="sxs-lookup"><span data-stu-id="59958-121">Understand hello results of hello cmdlet</span></span>
<span data-ttu-id="59958-122">diagnostické rutiny Hello provádí hello následující kontroly:</span><span class="sxs-lookup"><span data-stu-id="59958-122">hello diagnostic cmdlet performs hello following checks:</span></span>

* <span data-ttu-id="59958-123">Ověří, že hello funkce Synchronizace hesla je povolený pro vašeho klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59958-123">Validates that hello password synchronization feature is enabled for your Azure AD tenant.</span></span>

* <span data-ttu-id="59958-124">Ověří, že hello Azure AD Connect server není v pracovním režimu.</span><span class="sxs-lookup"><span data-stu-id="59958-124">Validates that hello Azure AD Connect server is not in staging mode.</span></span>

* <span data-ttu-id="59958-125">Pro každý existující místní konektor služby Active Directory (který odpovídá tooan existující doménové struktury služby Active Directory):</span><span class="sxs-lookup"><span data-stu-id="59958-125">For each existing on-premises Active Directory connector (which corresponds tooan existing Active Directory forest):</span></span>

   * <span data-ttu-id="59958-126">Ověří, že hello je povolena funkce Synchronizace hesla.</span><span class="sxs-lookup"><span data-stu-id="59958-126">Validates that hello password synchronization feature is enabled.</span></span>
   
   * <span data-ttu-id="59958-127">Vyhledá události prezenční signál synchronizace hesel v hello v protokolech událostí aplikace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="59958-127">Searches for password synchronization heartbeat events in hello Windows Application Event logs.</span></span>

   * <span data-ttu-id="59958-128">Pro každou doménu služby Active Directory v části konektor služby Active Directory v místě hello:</span><span class="sxs-lookup"><span data-stu-id="59958-128">For each Active Directory domain under hello on-premises Active Directory connector:</span></span>

      * <span data-ttu-id="59958-129">Ověří, že hello doména je dostupný ze serveru hello Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="59958-129">Validates that hello domain is reachable from hello Azure AD Connect server.</span></span>

      * <span data-ttu-id="59958-130">Ověří, zda má účty služby Active Directory Domain Services (AD DS) hello používá konektor služby Active Directory v místě hello hello správné uživatelské jméno, heslo a oprávněních pro synchronizaci hesel.</span><span class="sxs-lookup"><span data-stu-id="59958-130">Validates that hello Active Directory Domain Services (AD DS) accounts used by hello on-premises Active Directory connector has hello correct username, password, and permissions required for password synchronization.</span></span>

<span data-ttu-id="59958-131">Hello následující diagram znázorňuje hello výsledky hello rutiny pro topologie s jednou doménou, místní služby Active Directory:</span><span class="sxs-lookup"><span data-stu-id="59958-131">hello following diagram illustrates hello results of hello cmdlet for a single-domain, on-premises Active Directory topology:</span></span>

![Výstup diagnostiky pro synchronizace hesel](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

<span data-ttu-id="59958-133">Hello zbývající část tohoto oddílu popisuje konkrétní výsledky, které se vrátí pomocí rutiny hello a odpovídající problémy.</span><span class="sxs-lookup"><span data-stu-id="59958-133">hello rest of this section describes specific results that are returned by hello cmdlet and corresponding issues.</span></span>

#### <a name="password-synchronization-feature-isnt-enabled"></a><span data-ttu-id="59958-134">Funkce Synchronizace hesla není povoleno.</span><span class="sxs-lookup"><span data-stu-id="59958-134">Password synchronization feature isn't enabled</span></span>
<span data-ttu-id="59958-135">Pokud jste nepovolili synchronizace hesel pomocí Průvodce hello Azure AD Connect, je vrácena hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="59958-135">If you haven't enabled password synchronization by using hello Azure AD Connect wizard, hello following error is returned:</span></span>

![Synchronizace hesla není povoleno.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a><span data-ttu-id="59958-137">Server Azure AD Connect je v pracovním režimu</span><span class="sxs-lookup"><span data-stu-id="59958-137">Azure AD Connect server is in staging mode</span></span>
<span data-ttu-id="59958-138">Pokud server hello Azure AD Connect v pracovním režimu, synchronizace hesel je dočasně zakázána a hello vrácena následující chyba je:</span><span class="sxs-lookup"><span data-stu-id="59958-138">If hello Azure AD Connect server is in staging mode, password synchronization is temporarily disabled, and hello following error is returned:</span></span>

![Server Azure AD Connect je v pracovním režimu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a><span data-ttu-id="59958-140">Žádné události prezenční signál synchronizace hesel</span><span class="sxs-lookup"><span data-stu-id="59958-140">No password synchronization heartbeat events</span></span>
<span data-ttu-id="59958-141">Každý konektor služby Active Directory v místě má svou vlastní heslo synchronizačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="59958-141">Each on-premises Active Directory connector has its own password synchronization channel.</span></span> <span data-ttu-id="59958-142">Při kanál synchronizace hesla hello je vytvořen a nejsou k dispozici žádné toobe změny hesla synchronizován, prezenční signál události (EventId 654) generuje každých 30 minut v protokolu událostí aplikace systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="59958-142">When hello password synchronization channel is established and there aren't any password changes toobe synchronized, a heartbeat event (EventId 654) is generated once every 30 minutes under hello Windows Application Event Log.</span></span> <span data-ttu-id="59958-143">Pro každý konektor služby Active Directory v místě hello rutina vyhledá odpovídající události prezenčního signálu v hello posledních tří hodin.</span><span class="sxs-lookup"><span data-stu-id="59958-143">For each on-premises Active Directory connector, hello cmdlet searches for corresponding heartbeat events in hello past three hours.</span></span> <span data-ttu-id="59958-144">Pokud se nenajde žádný prezenční signál události, hello vrácena následující chyba je:</span><span class="sxs-lookup"><span data-stu-id="59958-144">If no heartbeat event is found, hello following error is returned:</span></span>

![Žádné vysílat synchronizace hesla porazit událostí](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a><span data-ttu-id="59958-146">AD DS účet nemá správná oprávnění</span><span class="sxs-lookup"><span data-stu-id="59958-146">AD DS account does not have correct permissions</span></span>
<span data-ttu-id="59958-147">Pokud účet hello služby AD DS, který je používán hello místně hodnot hash hesel služby Active Directory connector toosynchronize nemá příslušná oprávnění hello hello vrácena následující chyba je:</span><span class="sxs-lookup"><span data-stu-id="59958-147">If hello AD DS account that's used by hello on-premises Active Directory connector toosynchronize password hashes does not have hello appropriate permissions, hello following error is returned:</span></span>

![Nesprávná pověření](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a><span data-ttu-id="59958-149">Nesprávné uživatelské jméno účtu služby AD DS nebo heslo</span><span class="sxs-lookup"><span data-stu-id="59958-149">Incorrect AD DS account username or password</span></span>
<span data-ttu-id="59958-150">Pokud hello účet služby AD DS používá má nesprávné uživatelské jméno nebo heslo zprostředkovatele hello místní služby Active Directory connector toosynchronize hodnot hash hesel, hello vrácena následující chyba je:</span><span class="sxs-lookup"><span data-stu-id="59958-150">If hello AD DS account used by hello on-premises Active Directory connector toosynchronize password hashes has an incorrect username or password, hello following error is returned:</span></span>

![Nesprávná pověření](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-hello-diagnostic-cmdlet"></a><span data-ttu-id="59958-152">Jeden objekt není synchronizace hesel: řešení potíží pomocí diagnostiky rutiny hello</span><span class="sxs-lookup"><span data-stu-id="59958-152">One object is not synchronizing passwords: troubleshoot by using hello diagnostic cmdlet</span></span>
<span data-ttu-id="59958-153">Můžete použít hello `Invoke-ADSyncDiagnostics` rutiny toodetermine Proč není jeden objekt synchronizace hesel.</span><span class="sxs-lookup"><span data-stu-id="59958-153">You can use hello `Invoke-ADSyncDiagnostics` cmdlet toodetermine why one object is not synchronizing passwords.</span></span>

> [!NOTE]
> <span data-ttu-id="59958-154">Hello `Invoke-ADSyncDiagnostics` rutina je dostupné pouze pro verze služby Azure AD Connect 1.1.524.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="59958-154">hello `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-hello-diagnostics-cmdlet"></a><span data-ttu-id="59958-155">Spusťte rutinu diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="59958-155">Run hello diagnostics cmdlet</span></span>
<span data-ttu-id="59958-156">tootroubleshoot problémy, kde jsou synchronizovány žádná hesla:</span><span class="sxs-lookup"><span data-stu-id="59958-156">tootroubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="59958-157">Otevřete novou relaci prostředí Windows PowerShell na serveru Azure AD Connect s hello **spustit jako správce** možnost.</span><span class="sxs-lookup"><span data-stu-id="59958-157">Open a new Windows PowerShell session on your Azure AD Connect server with hello **Run as Administrator** option.</span></span>

2. <span data-ttu-id="59958-158">Spustit `Set-ExecutionPolicy RemoteSigned` nebo `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="59958-158">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="59958-159">Spusťte `Import-Module ADSyncDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="59958-159">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="59958-160">Spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="59958-160">Run hello following cmdlet:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   <span data-ttu-id="59958-161">Například:</span><span class="sxs-lookup"><span data-stu-id="59958-161">For example:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-hello-results-of-hello-cmdlet"></a><span data-ttu-id="59958-162">Pochopení hello výsledky rutiny hello</span><span class="sxs-lookup"><span data-stu-id="59958-162">Understand hello results of hello cmdlet</span></span>
<span data-ttu-id="59958-163">diagnostické rutiny Hello provádí hello následující kontroly:</span><span class="sxs-lookup"><span data-stu-id="59958-163">hello diagnostic cmdlet performs hello following checks:</span></span>

* <span data-ttu-id="59958-164">Prozkoumá hello stav objektu služby Active Directory hello hello prostoru konektoru služby Active Directory, úložiště Metaverse a Azure AD prostoru konektoru.</span><span class="sxs-lookup"><span data-stu-id="59958-164">Examines hello state of hello Active Directory object in hello Active Directory connector space, Metaverse, and Azure AD connector space.</span></span>

* <span data-ttu-id="59958-165">Ověří, že jsou synchronizační pravidla, jejichž synchronizace hesel povolena a použít toohello objektu služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="59958-165">Validates that there are synchronization rules with password synchronization enabled and applied toohello Active Directory object.</span></span>

* <span data-ttu-id="59958-166">Pokusy o tooretrieve a zobrazení výsledků hello hello poslední pokus o toosynchronize hello hesla pro objekt hello.</span><span class="sxs-lookup"><span data-stu-id="59958-166">Attempts tooretrieve and display hello results of hello last attempt toosynchronize hello password for hello object.</span></span>

<span data-ttu-id="59958-167">Hello následující diagram znázorňuje hello výsledky rutiny hello při řešení potíží s synchronizace hesel pro jediný objekt:</span><span class="sxs-lookup"><span data-stu-id="59958-167">hello following diagram illustrates hello results of hello cmdlet when troubleshooting password synchronization for a single object:</span></span>

![Výstup diagnostiky pro synchronizaci hesel - jednoho objektu.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

<span data-ttu-id="59958-169">Hello zbývající část tohoto oddílu popisuje konkrétní výsledků vrácených rutinou hello a odpovídající problémy.</span><span class="sxs-lookup"><span data-stu-id="59958-169">hello rest of this section describes specific results returned by hello cmdlet and corresponding issues.</span></span>

#### <a name="hello-active-directory-object-isnt-exported-tooazure-ad"></a><span data-ttu-id="59958-170">Hello objektu služby Active Directory není exportovaný tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="59958-170">hello Active Directory object isn't exported tooAzure AD</span></span>
<span data-ttu-id="59958-171">Synchronizace hesel pro tento účet místní služby Active Directory se nezdaří, protože neexistuje žádný odpovídající objektů v klientovi Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="59958-171">Password synchronization for this on-premises Active Directory account fails because there is no corresponding object in hello Azure AD tenant.</span></span> <span data-ttu-id="59958-172">se vrátí Hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="59958-172">hello following error is returned:</span></span>

![Chybí objekt Azure AD](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a><span data-ttu-id="59958-174">Uživatel má dočasné heslo.</span><span class="sxs-lookup"><span data-stu-id="59958-174">User has a temporary password</span></span>
<span data-ttu-id="59958-175">Azure AD Connect v současné době nepodporuje synchronizaci dočasná hesla s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59958-175">Currently, Azure AD Connect does not support synchronizing temporary passwords with Azure AD.</span></span> <span data-ttu-id="59958-176">Heslo je považován za toobe dočasné Pokud hello **při dalším přihlášení změnit heslo** je možnost nastavena na uživatele služby Active Directory v místě hello.</span><span class="sxs-lookup"><span data-stu-id="59958-176">A password is considered toobe temporary if hello **Change password at next logon** option is set on hello on-premises Active Directory user.</span></span> <span data-ttu-id="59958-177">se vrátí Hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="59958-177">hello following error is returned:</span></span>

![Dočasné heslo není exportovali.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-toosynchronize-password-arent-available"></a><span data-ttu-id="59958-179">Nejsou k dispozici, výsledky poslední pokus o toosynchronize heslo</span><span class="sxs-lookup"><span data-stu-id="59958-179">Results of last attempt toosynchronize password aren't available</span></span>
<span data-ttu-id="59958-180">Ve výchozím nastavení ukládá Azure AD Connect hello výsledky pokusů o zadání synchronizace hesla sedm dní.</span><span class="sxs-lookup"><span data-stu-id="59958-180">By default, Azure AD Connect stores hello results of password synchronization attempts for seven days.</span></span> <span data-ttu-id="59958-181">Pokud nejsou k dispozici pro objekt služby Active Directory hello vybrané žádné výsledky, je vrácena hello následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="59958-181">If there are no results available for hello selected Active Directory object, hello following warning is returned:</span></span>

![Výstup diagnostiky pro jediný objekt - žádná historie synchronizace hesla](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a><span data-ttu-id="59958-183">Žádná hesla se synchronizují: Ruční postup řešení potíží</span><span class="sxs-lookup"><span data-stu-id="59958-183">No passwords are synchronized: manual troubleshooting steps</span></span>
<span data-ttu-id="59958-184">Postupujte podle těchto kroků toodetermine, proč jsou synchronizovány žádná hesla:</span><span class="sxs-lookup"><span data-stu-id="59958-184">Follow these steps toodetermine why no passwords are synchronized:</span></span>

1. <span data-ttu-id="59958-185">Je server hello Connect v [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode)?</span><span class="sxs-lookup"><span data-stu-id="59958-185">Is hello Connect server in [staging mode](active-directory-aadconnectsync-operations.md#staging-mode)?</span></span> <span data-ttu-id="59958-186">Server v pracovní režimu nebyla synchronizována všechna hesla.</span><span class="sxs-lookup"><span data-stu-id="59958-186">A server in staging mode does not synchronize any passwords.</span></span>

2. <span data-ttu-id="59958-187">Spusťte skript hello hello [získat stav hello nastavení synchronizace hesla](#get-the-status-of-password-sync-settings) části.</span><span class="sxs-lookup"><span data-stu-id="59958-187">Run hello script in hello [Get hello status of password sync settings](#get-the-status-of-password-sync-settings) section.</span></span> <span data-ttu-id="59958-188">Poskytuje přehled o konfiguraci synchronizace hesel hello.</span><span class="sxs-lookup"><span data-stu-id="59958-188">It gives you an overview of hello password sync configuration.</span></span>  

    ![Výstup skriptu prostředí PowerShell z nastavení synchronizace hesla](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. <span data-ttu-id="59958-190">Pokud není povolená funkce hello ve službě Azure AD, nebo pokud není povolené stav kanál synchronizace hello, spusťte Průvodce instalací Connect hello.</span><span class="sxs-lookup"><span data-stu-id="59958-190">If hello feature is not enabled in Azure AD or if hello sync channel status is not enabled, run hello Connect installation wizard.</span></span> <span data-ttu-id="59958-191">Vyberte **přizpůsobit možnosti synchronizace**a zrušte výběr synchronizaci hesel. Tato změna dočasně zakáže funkci hello.</span><span class="sxs-lookup"><span data-stu-id="59958-191">Select **Customize synchronization options**, and unselect password sync. This change temporarily disables hello feature.</span></span> <span data-ttu-id="59958-192">Potom spusťte znovu Průvodce hello a znovu povolte synchronizaci hesla. Spusťte skript hello znovu tooverify, který hello konfigurace je správná.</span><span class="sxs-lookup"><span data-stu-id="59958-192">Then run hello wizard again and re-enable password sync. Run hello script again tooverify that hello configuration is correct.</span></span>

4. <span data-ttu-id="59958-193">Vyhledejte v protokolu událostí hello chyby.</span><span class="sxs-lookup"><span data-stu-id="59958-193">Look in hello event log for errors.</span></span> <span data-ttu-id="59958-194">Podívejte se na následující události, které by mohly ukazovat na problém hello:</span><span class="sxs-lookup"><span data-stu-id="59958-194">Look for hello following events, which would indicate a problem:</span></span>
    * <span data-ttu-id="59958-195">Zdroje: ID "Synchronizace adresáře": 0, 611, 652, 655 těchto událostí došlo k potížím. připojení.</span><span class="sxs-lookup"><span data-stu-id="59958-195">Source: "Directory synchronization" ID: 0, 611, 652, 655 If you see these events, you have a connectivity problem.</span></span> <span data-ttu-id="59958-196">zprávy protokolu událostí Hello obsahuje informace doménové struktury, kde došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="59958-196">hello event log message contains forest information where you have a problem.</span></span> <span data-ttu-id="59958-197">Další informace najdete v tématu [problém s připojením k](#connectivity problem).</span><span class="sxs-lookup"><span data-stu-id="59958-197">For more information, see [Connectivity problem](#connectivity problem).</span></span>

5. <span data-ttu-id="59958-198">Pokud se neobjevil žádný prezenční signál nebo nic jiného fungovala, spusťte [spustit úplnou synchronizaci hesel všech](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="59958-198">If you see no heartbeat or if nothing else worked, run [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span> <span data-ttu-id="59958-199">Spusťte skript hello pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="59958-199">Run hello script only once.</span></span>

6. <span data-ttu-id="59958-200">V tématu hello [řešení potíží s jeden objekt, který není synchronizace hesel](#one-object-is-not-synchronizing-passwords) části.</span><span class="sxs-lookup"><span data-stu-id="59958-200">See hello [Troubleshoot one object that is not synchronizing passwords](#one-object-is-not-synchronizing-passwords) section.</span></span>

### <a name="connectivity-problems"></a><span data-ttu-id="59958-201">Potíže s připojením k</span><span class="sxs-lookup"><span data-stu-id="59958-201">Connectivity problems</span></span>

<span data-ttu-id="59958-202">Máte připojení k Azure AD?</span><span class="sxs-lookup"><span data-stu-id="59958-202">Do you have connectivity with Azure AD?</span></span>

<span data-ttu-id="59958-203">Účet hello mají požadovaná oprávnění tooread hello hodnot hash hesel ve všech doménách?</span><span class="sxs-lookup"><span data-stu-id="59958-203">Does hello account have required permissions tooread hello password hashes in all domains?</span></span> <span data-ttu-id="59958-204">Pokud jste nainstalovali Connect pomocí expresního nastavení, musí být hello oprávnění již správné.</span><span class="sxs-lookup"><span data-stu-id="59958-204">If you installed Connect by using Express settings, hello permissions should already be correct.</span></span> 

<span data-ttu-id="59958-205">Pokud jste použili vlastní instalaci, nastavte oprávnění hello ručně pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="59958-205">If you used custom installation, set hello permissions manually by doing hello following:</span></span>
    
1. <span data-ttu-id="59958-206">toofind hello účet používaný službou hello konektor služby Active Directory, spuštění **Synchronization Service Manager**.</span><span class="sxs-lookup"><span data-stu-id="59958-206">toofind hello account used by hello Active Directory connector, start **Synchronization Service Manager**.</span></span> 
 
2. <span data-ttu-id="59958-207">Přejděte příliš**konektory**a poté vyhledejte místní doménové struktuře služby Active Directory hello řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="59958-207">Go too**Connectors**, and then search for hello on-premises Active Directory forest you are troubleshooting.</span></span> 
 
3. <span data-ttu-id="59958-208">Vyberte konektor hello a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="59958-208">Select hello connector, and then click **Properties**.</span></span> 
 
4. <span data-ttu-id="59958-209">Přejděte příliš**připojit doménové struktury Directory tooActive**.</span><span class="sxs-lookup"><span data-stu-id="59958-209">Go too**Connect tooActive Directory Forest**.</span></span>  
    
    ![Účet používaný službou konektoru služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    <span data-ttu-id="59958-211">Všimněte si hello uživatelské jméno a hello domény, kde je umístěn účet hello.</span><span class="sxs-lookup"><span data-stu-id="59958-211">Note hello username and hello domain where hello account is located.</span></span>
    
5. <span data-ttu-id="59958-212">Spustit **Active Directory Users and Computers**a potom ověřte, zda má účet hello jste našli dříve hello postupujte podle oprávněními nastavenými v kořenovém adresáři hello všechny domény v doménové struktuře:</span><span class="sxs-lookup"><span data-stu-id="59958-212">Start **Active Directory Users and Computers**, and then verify that hello account you found earlier has hello follow permissions set at hello root of all domains in your forest:</span></span>
    * <span data-ttu-id="59958-213">Replikovat změny adresáře</span><span class="sxs-lookup"><span data-stu-id="59958-213">Replicate Directory Changes</span></span>
    * <span data-ttu-id="59958-214">Replikace adresáře všechny změny</span><span class="sxs-lookup"><span data-stu-id="59958-214">Replicate Directory Changes All</span></span>

6. <span data-ttu-id="59958-215">Jsou hello řadiče domény dostupný přes Azure AD Connect?</span><span class="sxs-lookup"><span data-stu-id="59958-215">Are hello domain controllers reachable by Azure AD Connect?</span></span> <span data-ttu-id="59958-216">Pokud hello připojit server se nemůže připojit tooall řadiče domény, nakonfigurujte **používat jenom upřednostňovaného řadiče domény**.</span><span class="sxs-lookup"><span data-stu-id="59958-216">If hello Connect server cannot connect tooall domain controllers, configure **Only use preferred domain controller**.</span></span>  
    
    ![Řadič domény používané konektorem služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. <span data-ttu-id="59958-218">Přejděte zpět příliš**Synchronization Service Manager** a **konfigurace oddílu adresáře**.</span><span class="sxs-lookup"><span data-stu-id="59958-218">Go back too**Synchronization Service Manager** and **Configure Directory Partition**.</span></span> 
 
8. <span data-ttu-id="59958-219">Vyberte doménu v **Vybrat oddíly adresářů**, vyberte hello **používat jenom řadiče domény upřednostňované** zaškrtněte políčko a potom klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="59958-219">Select your domain in **Select directory partitions**, select hello **Only use preferred domain controllers** check box, and then click **Configure**.</span></span> 

9. <span data-ttu-id="59958-220">V seznamu hello zadejte hello řadiče domény, které by měly používat Connect pro synchronizaci hesel. Hello stejný seznam se používá pro import a export také.</span><span class="sxs-lookup"><span data-stu-id="59958-220">In hello list, enter hello domain controllers that Connect should use for password sync. hello same list is used for import and export as well.</span></span> <span data-ttu-id="59958-221">Proveďte tyto kroky pro všechny domény.</span><span class="sxs-lookup"><span data-stu-id="59958-221">Do these steps for all your domains.</span></span>

10. <span data-ttu-id="59958-222">Pokud hello skript je ukázkou, že neexistuje žádný prezenční signál, spusťte skript hello [spustit úplnou synchronizaci hesel všech](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="59958-222">If hello script shows that there is no heartbeat, run hello script in [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span>

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a><span data-ttu-id="59958-223">Jeden objekt není synchronizace hesel: Ruční postup řešení potíží</span><span class="sxs-lookup"><span data-stu-id="59958-223">One object is not synchronizing passwords: manual troubleshooting steps</span></span>
<span data-ttu-id="59958-224">Můžete snadno potíže heslo synchronizace kontrolou hello stavem objektu.</span><span class="sxs-lookup"><span data-stu-id="59958-224">You can easily troubleshoot password synchronization issues by reviewing hello status of an object.</span></span>

1. <span data-ttu-id="59958-225">V **Active Directory Users and Computers**, vyhledejte hello uživatele a potom ověřte, že hello **musí uživatel změnit heslo při příštím přihlášení** zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="59958-225">In **Active Directory Users and Computers**, search for hello user, and then verify that hello **User must change password at next logon** check box is cleared.</span></span>  

    ![Produktivní hesla služby Active Directory](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    <span data-ttu-id="59958-227">Pokud je zaškrtnuto políčko hello, požádejte uživatele toosign hello v a změnit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="59958-227">If hello check box is selected, ask hello user toosign in and change hello password.</span></span> <span data-ttu-id="59958-228">Dočasná hesla nejsou synchronizovány s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59958-228">Temporary passwords are not synchronized with Azure AD.</span></span>

2. <span data-ttu-id="59958-229">Pokud správná hello hesla ve službě Active Directory, postupujte podle hello uživatele v synchronizační modul hello.</span><span class="sxs-lookup"><span data-stu-id="59958-229">If hello password looks correct in Active Directory, follow hello user in hello sync engine.</span></span> <span data-ttu-id="59958-230">Podle následující hello uživatele z místní služby Active Directory tooAzure AD se zobrazí, zda je u objektu hello popisný chyba.</span><span class="sxs-lookup"><span data-stu-id="59958-230">By following hello user from on-premises Active Directory tooAzure AD, you can see whether there is a descriptive error on hello object.</span></span>

    <span data-ttu-id="59958-231">a.</span><span class="sxs-lookup"><span data-stu-id="59958-231">a.</span></span> <span data-ttu-id="59958-232">Spustit hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="59958-232">Start hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span></span>

    <span data-ttu-id="59958-233">b.</span><span class="sxs-lookup"><span data-stu-id="59958-233">b.</span></span> <span data-ttu-id="59958-234">Klikněte na tlačítko **konektory**.</span><span class="sxs-lookup"><span data-stu-id="59958-234">Click **Connectors**.</span></span>

    <span data-ttu-id="59958-235">c.</span><span class="sxs-lookup"><span data-stu-id="59958-235">c.</span></span> <span data-ttu-id="59958-236">Vyberte hello **konektor služby Active Directory** kde se nachází hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="59958-236">Select hello **Active Directory Connector** where hello user is located.</span></span>

    <span data-ttu-id="59958-237">d.</span><span class="sxs-lookup"><span data-stu-id="59958-237">d.</span></span> <span data-ttu-id="59958-238">Vyberte **hledání prostoru konektoru**.</span><span class="sxs-lookup"><span data-stu-id="59958-238">Select **Search Connector Space**.</span></span>

    <span data-ttu-id="59958-239">e.</span><span class="sxs-lookup"><span data-stu-id="59958-239">e.</span></span> <span data-ttu-id="59958-240">V hello **oboru** vyberte **rozlišující název nebo ukotvení**a pak zadejte úplný název DN hello hello uživatele řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="59958-240">In hello **Scope** box, select **DN or Anchor**, and then enter hello full DN of hello user you are troubleshooting.</span></span>

    ![Vyhledejte uživatele v prostoru konektoru s rozlišujícím názvem](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    <span data-ttu-id="59958-242">f.</span><span class="sxs-lookup"><span data-stu-id="59958-242">f.</span></span> <span data-ttu-id="59958-243">Vyhledejte uživatele hello hledáte a potom klikněte na **vlastnosti** toosee všechny hello atributy.</span><span class="sxs-lookup"><span data-stu-id="59958-243">Locate hello user you are looking for, and then click **Properties** toosee all hello attributes.</span></span> <span data-ttu-id="59958-244">Pokud není uživatel hello ve výsledku hledání hello, ověřte vaší [pravidla filtrování](active-directory-aadconnectsync-configure-filtering.md) a ujistěte se, že spustíte [použít a ověřte změny](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) pro uživatele tooappear hello v připojení.</span><span class="sxs-lookup"><span data-stu-id="59958-244">If hello user is not in hello search result, verify your [filtering rules](active-directory-aadconnectsync-configure-filtering.md) and make sure that you run [Apply and verify changes](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) for hello user tooappear in Connect.</span></span>

    <span data-ttu-id="59958-245">g.</span><span class="sxs-lookup"><span data-stu-id="59958-245">g.</span></span> <span data-ttu-id="59958-246">Klikněte na tlačítko toosee hello heslo synchronizace Podrobnosti objektu hello pro hello minulého týdne **protokolu**.</span><span class="sxs-lookup"><span data-stu-id="59958-246">toosee hello password sync details of hello object for hello past week, click **Log**.</span></span>  

    ![Podrobnosti protokolu objektu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    <span data-ttu-id="59958-248">Pokud objekt protokolu hello je prázdná, Azure AD Connect byla hodnoty hash hesla hello nelze tooread ze služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="59958-248">If hello object log is empty, Azure AD Connect has been unable tooread hello password hash from Active Directory.</span></span> <span data-ttu-id="59958-249">Pokračovat v odstraňování problémů s [k chybám připojení](#connectivity-errors).</span><span class="sxs-lookup"><span data-stu-id="59958-249">Continue your troubleshooting with [Connectivity Errors](#connectivity-errors).</span></span> <span data-ttu-id="59958-250">Pokud se zobrazí jakoukoli jinou hodnotu než **úspěch**, najdete v tabulce toohello v [protokolu synchronizace hesla](#password-sync-log).</span><span class="sxs-lookup"><span data-stu-id="59958-250">If you see any other value than **success**, refer toohello table in [Password sync log](#password-sync-log).</span></span>

    <span data-ttu-id="59958-251">h.</span><span class="sxs-lookup"><span data-stu-id="59958-251">h.</span></span> <span data-ttu-id="59958-252">Vyberte hello **rodokmenu** kartě a ujistěte se, že minimálně jeden synchronizační pravidlo v hello **PasswordSync** sloupec je **True**.</span><span class="sxs-lookup"><span data-stu-id="59958-252">Select hello **lineage** tab, and make sure that at least one sync rule in hello **PasswordSync** column is **True**.</span></span> <span data-ttu-id="59958-253">Ve výchozí konfiguraci hello hello název pravidla synchronizace hello je **v ze služby Active Directory - uživatele AccountEnabled**.</span><span class="sxs-lookup"><span data-stu-id="59958-253">In hello default configuration, hello name of hello sync rule is **In from AD - User AccountEnabled**.</span></span>  

    ![Rodokmenu informací o uživateli](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    <span data-ttu-id="59958-255">i.</span><span class="sxs-lookup"><span data-stu-id="59958-255">i.</span></span> <span data-ttu-id="59958-256">Klikněte na tlačítko **vlastnosti objektu úložiště Metaverse** toodisplay seznam atributů uživatele.</span><span class="sxs-lookup"><span data-stu-id="59958-256">Click **Metaverse Object Properties** toodisplay a list of user attributes.</span></span>  

    ![Informace o úložiště Metaverse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    <span data-ttu-id="59958-258">Ověřte, zda je žádné **cloudFiltered** atribut přítomen.</span><span class="sxs-lookup"><span data-stu-id="59958-258">Verify that there is no **cloudFiltered** attribute present.</span></span> <span data-ttu-id="59958-259">Ujistěte se, že hello domény atributy (domainFQDN a domainNetBios) obsahují hello očekávaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="59958-259">Make sure that hello domain attributes (domainFQDN and domainNetBios) have hello expected values.</span></span>

    <span data-ttu-id="59958-260">j.</span><span class="sxs-lookup"><span data-stu-id="59958-260">j.</span></span> <span data-ttu-id="59958-261">Klikněte na tlačítko hello **konektory** kartě. Ujistěte se, že vidíte konektory tooboth místní služby Active Directory a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59958-261">Click hello **Connectors** tab. Make sure that you see connectors tooboth on-premises Active Directory and Azure AD.</span></span>

    ![Informace o úložiště Metaverse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    <span data-ttu-id="59958-263">kB.</span><span class="sxs-lookup"><span data-stu-id="59958-263">k.</span></span> <span data-ttu-id="59958-264">Vyberte hello řádek, který představuje Azure AD, klikněte na tlačítko **vlastnosti**a potom klikněte na hello **rodokmenu** objektu prostoru konektoru hello kartu. měl by tam mít odchozí pravidlo hello **PasswordSync** sloupec nastavit příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="59958-264">Select hello row that represents Azure AD, click **Properties**, and then click hello **Lineage** tab. hello connector space object should have an outbound rule in hello **PasswordSync** column set too**True**.</span></span> <span data-ttu-id="59958-265">Ve výchozí konfiguraci hello hello název pravidla synchronizace hello je **Out tooAAD - připojení uživatele k**.</span><span class="sxs-lookup"><span data-stu-id="59958-265">In hello default configuration, hello name of hello sync rule is **Out tooAAD - User Join**.</span></span>  

    ![Dialogové okno Vlastnosti objektu prostoru konektoru](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a><span data-ttu-id="59958-267">Protokol synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="59958-267">Password sync log</span></span>
<span data-ttu-id="59958-268">ve sloupci Stav Hello může mít hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="59958-268">hello status column can have hello following values:</span></span>

| <span data-ttu-id="59958-269">Status</span><span class="sxs-lookup"><span data-stu-id="59958-269">Status</span></span> | <span data-ttu-id="59958-270">Popis</span><span class="sxs-lookup"><span data-stu-id="59958-270">Description</span></span> |
| --- | --- |
| <span data-ttu-id="59958-271">Úspěch</span><span class="sxs-lookup"><span data-stu-id="59958-271">Success</span></span> |<span data-ttu-id="59958-272">Heslo se úspěšně synchronizoval.</span><span class="sxs-lookup"><span data-stu-id="59958-272">Password has been successfully synchronized.</span></span> |
| <span data-ttu-id="59958-273">FilteredByTarget</span><span class="sxs-lookup"><span data-stu-id="59958-273">FilteredByTarget</span></span> |<span data-ttu-id="59958-274">Heslo se nastavuje příliš**musí uživatel změnit heslo při příštím přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="59958-274">Password is set too**User must change password at next logon**.</span></span> <span data-ttu-id="59958-275">Heslo nebyl synchronizován.</span><span class="sxs-lookup"><span data-stu-id="59958-275">Password has not been synchronized.</span></span> |
| <span data-ttu-id="59958-276">NoTargetConnection</span><span class="sxs-lookup"><span data-stu-id="59958-276">NoTargetConnection</span></span> |<span data-ttu-id="59958-277">Žádný objekt v úložišti metaverse hello nebo v prostoru konektoru hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59958-277">No object in hello metaverse or in hello Azure AD connector space.</span></span> |
| <span data-ttu-id="59958-278">SourceConnectorNotPresent</span><span class="sxs-lookup"><span data-stu-id="59958-278">SourceConnectorNotPresent</span></span> |<span data-ttu-id="59958-279">Žádný objekt v prostoru konektoru služby Active Directory v místě hello nalezen.</span><span class="sxs-lookup"><span data-stu-id="59958-279">No object found in hello on-premises Active Directory connector space.</span></span> |
| <span data-ttu-id="59958-280">TargetNotExportedToDirectory</span><span class="sxs-lookup"><span data-stu-id="59958-280">TargetNotExportedToDirectory</span></span> |<span data-ttu-id="59958-281">nebyl ještě exportoval Hello objekt v hello prostoru konektoru služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59958-281">hello object in hello Azure AD connector space has not yet been exported.</span></span> |
| <span data-ttu-id="59958-282">MigratedCheckDetailsForMoreInfo</span><span class="sxs-lookup"><span data-stu-id="59958-282">MigratedCheckDetailsForMoreInfo</span></span> |<span data-ttu-id="59958-283">Položka protokolu byl vytvořen ještě před sestavení 1.0.9125.0 a je zobrazen v stav starší verze.</span><span class="sxs-lookup"><span data-stu-id="59958-283">Log entry was created before build 1.0.9125.0 and is shown in its legacy state.</span></span> |
| <span data-ttu-id="59958-284">Chyba</span><span class="sxs-lookup"><span data-stu-id="59958-284">Error</span></span> |<span data-ttu-id="59958-285">Služba vrátila neznámou chybu.</span><span class="sxs-lookup"><span data-stu-id="59958-285">Service returned an unknown error.</span></span> |
| <span data-ttu-id="59958-286">Neznámý</span><span class="sxs-lookup"><span data-stu-id="59958-286">Unknown</span></span> |<span data-ttu-id="59958-287">Došlo k chybě při pokusu o tooprocess dávky hodnot hash hesel.</span><span class="sxs-lookup"><span data-stu-id="59958-287">An error occurred while trying tooprocess a batch of password hashes.</span></span>  |
| <span data-ttu-id="59958-288">MissingAttribute</span><span class="sxs-lookup"><span data-stu-id="59958-288">MissingAttribute</span></span> |<span data-ttu-id="59958-289">Konkrétní atributy (například hodnota hash protokolu Kerberos) vyžaduje Azure AD Domain Services nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="59958-289">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services are not available.</span></span> |
| <span data-ttu-id="59958-290">RetryRequestedByTarget</span><span class="sxs-lookup"><span data-stu-id="59958-290">RetryRequestedByTarget</span></span> |<span data-ttu-id="59958-291">Konkrétní atributy (například hodnota hash protokolu Kerberos) vyžaduje Azure AD Domain Services nebyly k dispozici dříve.</span><span class="sxs-lookup"><span data-stu-id="59958-291">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services were not available previously.</span></span> <span data-ttu-id="59958-292">Hodnota hash hesla pokusu o tooresynchronize hello uživatele Přišla žádost.</span><span class="sxs-lookup"><span data-stu-id="59958-292">An attempt tooresynchronize hello user's password hash is made.</span></span> |

## <a name="scripts-toohelp-troubleshooting"></a><span data-ttu-id="59958-293">Řešení potíží s toohelp skriptů</span><span class="sxs-lookup"><span data-stu-id="59958-293">Scripts toohelp troubleshooting</span></span>

### <a name="get-hello-status-of-password-sync-settings"></a><span data-ttu-id="59958-294">Získat stav hello nastavení synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="59958-294">Get hello status of password sync settings</span></span>
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
        Write-Warning "More than one Azure AD Connectors found. Please update hello script toouse hello appropriate Connector."
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

#### <a name="trigger-a-full-sync-of-all-passwords"></a><span data-ttu-id="59958-295">Spustit úplnou synchronizaci všech hesel</span><span class="sxs-lookup"><span data-stu-id="59958-295">Trigger a full sync of all passwords</span></span>
> [!NOTE]
> <span data-ttu-id="59958-296">Tento skript spusťte jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="59958-296">Run this script only once.</span></span> <span data-ttu-id="59958-297">Pokud potřebujete toorun je více než jednou, jiný problém hello je.</span><span class="sxs-lookup"><span data-stu-id="59958-297">If you need toorun it more than once, something else is hello problem.</span></span> <span data-ttu-id="59958-298">tootroubleshoot hello problému, kontaktujte podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="59958-298">tootroubleshoot hello problem, contact Microsoft support.</span></span>

<span data-ttu-id="59958-299">Úplnou synchronizaci všech hesel můžete aktivovat pomocí hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="59958-299">You can trigger a full sync of all passwords by using hello following script:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="59958-300">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59958-300">Next steps</span></span>
* [<span data-ttu-id="59958-301">Implementace synchronizace hesel s synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="59958-301">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md)
* [<span data-ttu-id="59958-302">Azure AD Connect Sync: Přizpůsobení možnosti synchronizace</span><span class="sxs-lookup"><span data-stu-id="59958-302">Azure AD Connect Sync: Customizing synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="59958-303">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="59958-303">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
