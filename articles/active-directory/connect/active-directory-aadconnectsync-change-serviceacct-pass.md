---
title: "Synchronizace Azure AD Connect: Změna účtu služby hello Azure AD Connect Sync | Microsoft Docs"
description: "Tento dokument téma popisuje hello šifrovací klíč a jak tooabandon po hello heslo je změnit."
services: active-directory
keywords: "Účtu synchronizační služby Azure AD, heslo"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 11948ac4662f722e4f684ef6c9b9ccdc6387e60f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-azure-ad-connect-sync-service-account-password"></a><span data-ttu-id="bc50b-104">Změna hesla účtu služby synchronizace Azure AD Connect hello</span><span class="sxs-lookup"><span data-stu-id="bc50b-104">Changing hello Azure AD Connect sync service account password</span></span>
<span data-ttu-id="bc50b-105">Pokud změníte heslo účtu služby synchronizace Azure AD Connect hello, hello synchronizační služba nebude možné spustit správně opuštění hello šifrovací klíč a heslo účtu služby synchronizace Azure AD Connect hello znovu inicializován.</span><span class="sxs-lookup"><span data-stu-id="bc50b-105">If you change hello  Azure AD Connect sync service account password, hello Synchronization Service will not be able start correctly until you have abandoned hello encryption key and reinitialized hello Azure AD Connect sync service account password.</span></span> 

<span data-ttu-id="bc50b-106">Azure AD Connect, jako součást hello synchronizační služby používá šifrování klíče toostore hello hesla hello služby AD DS a účty služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc50b-106">Azure AD Connect, as part of hello Synchronization Services uses an encryption key toostore hello passwords of hello AD DS and Azure AD service accounts.</span></span>  <span data-ttu-id="bc50b-107">Tyto účty jsou zašifrované, než jsou uloženy v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="bc50b-107">These accounts are encrypted before they are stored in hello database.</span></span> 

<span data-ttu-id="bc50b-108">Hello šifrovací klíč použít zabezpečené pomocí [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc50b-108">hello encryption key used is secured using [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span></span> <span data-ttu-id="bc50b-109">Rozhraní DPAPI chrání hello šifrovací klíč pomocí hello **heslo účtu služby sync hello Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="bc50b-109">DPAPI protects hello encryption key using hello **password of hello Azure AD Connect sync service account**.</span></span> 

<span data-ttu-id="bc50b-110">Pokud potřebujete heslo účtu služby hello toochange můžete použít postupy hello v [Abandoning hello Azure AD Connect Sync šifrovací klíč](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish to.</span><span class="sxs-lookup"><span data-stu-id="bc50b-110">If you need toochange hello service account password you can use hello procedures in [Abandoning hello Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish this.</span></span>  <span data-ttu-id="bc50b-111">Tyto postupy by měla být používána Pokud z jakéhokoli důvodu potřebujete tooabandon hello šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="bc50b-111">These procedures should also be used if you need tooabandon hello encryption key for any reason.</span></span>

##<a name="issues-that-arise-from-changing-hello-password"></a><span data-ttu-id="bc50b-112">Problémy, které vznikají z Změna hesla hello</span><span class="sxs-lookup"><span data-stu-id="bc50b-112">Issues that arise from changing hello password</span></span>
<span data-ttu-id="bc50b-113">Existují dvě věci, které je třeba provést, pokud změníte heslo účtu služby hello toobe.</span><span class="sxs-lookup"><span data-stu-id="bc50b-113">There are two things that need toobe done when you change hello service account password.</span></span>

<span data-ttu-id="bc50b-114">Nejprve je třeba heslo hello toochange pod hello správce řízení služeb systému Windows.</span><span class="sxs-lookup"><span data-stu-id="bc50b-114">First, you need toochange hello password under hello Windows Service Control Manager.</span></span>  <span data-ttu-id="bc50b-115">Dokud nebude tento problém vyřešen. zobrazí se následující chyby:</span><span class="sxs-lookup"><span data-stu-id="bc50b-115">Until this issue is resolved you will see following errors:</span></span>


- <span data-ttu-id="bc50b-116">Pokud se pokusíte toostart hello synchronizační služba v modulu snap-in Správce řízení služeb systému Windows, zobrazí se chyba hello "**Windows nelze spustit službu Microsoft Azure AD Sync hello v místním počítači**".</span><span class="sxs-lookup"><span data-stu-id="bc50b-116">If you try toostart hello Synchronization Service in Windows Service Control Manager, you receive hello error "**Windows could not start hello Microsoft Azure AD Sync service on Local Computer**".</span></span> <span data-ttu-id="bc50b-117">**Chyba. 1069: hello služby se nepodařilo spustit z důvodu tooa přihlášení se nezdařilo.** "</span><span class="sxs-lookup"><span data-stu-id="bc50b-117">**Error 1069: hello service did not start due tooa logon failure.**"</span></span>
- <span data-ttu-id="bc50b-118">V prohlížeči událostí systému Windows, protokolu událostí systému hello obsahuje chybu s **7038 ID události** a zpráva "**hello ADSync service byla nelze toolog na stejně jako u hello aktuálně nakonfigurované heslo z důvodu toohello následující chyba: Hello uživatelské jméno nebo heslo není správné.** "</span><span class="sxs-lookup"><span data-stu-id="bc50b-118">Under Windows Event Viewer, hello system event log contains an error with **Event ID 7038** and message “**hello ADSync service was unable toolog on as with hello currently configured password due toohello following error: hello user name or password is incorrect.**"</span></span>

<span data-ttu-id="bc50b-119">Druhý pro určité podmínky, pokud se aktualizuje hello heslo, hello synchronizační služba už načíst hello šifrovací klíč pomocí rozhraní DPAPI.</span><span class="sxs-lookup"><span data-stu-id="bc50b-119">Second, under specific conditions, if hello password is updated, hello Synchronization Service can no longer retrieve hello encryption key via DPAPI.</span></span> <span data-ttu-id="bc50b-120">Bez hello šifrovací klíč hello synchronizační služby nelze dešifrovat hello hesla vyžaduje toosynchronize do a z místní AD a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc50b-120">Without hello encryption key, hello Synchronization Service cannot decrypt hello passwords required toosynchronize to/from on-premises AD and Azure AD.</span></span>
<span data-ttu-id="bc50b-121">Zobrazí se chyby, jako:</span><span class="sxs-lookup"><span data-stu-id="bc50b-121">You will see errors such as:</span></span>

- <span data-ttu-id="bc50b-122">V části správce řízení služeb systému Windows Pokud zkuste toostart hello synchronizační službu a nemůže získat hello šifrovací klíč, se nezdaří s chybou "** Windows se nepodařilo spustit hello Microsoft Azure AD Sync v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="bc50b-122">Under Windows Service Control Manager, if you try toostart hello Synchronization Service and it cannot retrieve hello encryption key, it fails with error “**Windows could not start hello Microsoft Azure AD Sync on Local Computer.</span></span> <span data-ttu-id="bc50b-123">Další informace najdete v tématu hello protokolu událostí systému.</span><span class="sxs-lookup"><span data-stu-id="bc50b-123">For more information, review hello System Event log.</span></span> <span data-ttu-id="bc50b-124">Pokud je služba jiného výrobce než Microsoftu, obraťte se na dodavatele služby hello a odkazovat kód chyby specifický tooservice **-21451857952 ***. "</span><span class="sxs-lookup"><span data-stu-id="bc50b-124">If this is a non-Microsoft service, contact hello service vendor, and refer tooservice-specific error code **-21451857952****.”</span></span>
- <span data-ttu-id="bc50b-125">V prohlížeči událostí systému Windows, protokolu událostí aplikace hello obsahuje chybu s **6028 ID události** a chybovou zprávou *"**hello serveru šifrovací klíč je nepřístupný.* *"*</span><span class="sxs-lookup"><span data-stu-id="bc50b-125">Under Windows Event Viewer, hello application event log contains an error with **Event ID 6028** and error message *“**hello server encryption key cannot be accessed.**”*</span></span>

<span data-ttu-id="bc50b-126">tooensure neobdrží tyto chyby, postupujte podle pokynů hello v [Abandoning hello Azure AD Connect Sync šifrovací klíč](#abandoning-the-azure-ad-connect-sync-encryption-key) při změně hesla hello.</span><span class="sxs-lookup"><span data-stu-id="bc50b-126">tooensure that you do not receive these errors, follow hello procedures in [Abandoning hello Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) when changing hello password.</span></span>
 
## <a name="abandoning-hello-azure-ad-connect-sync-encryption-key"></a><span data-ttu-id="bc50b-127">Zrušení hello Azure AD Connect Sync šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="bc50b-127">Abandoning hello Azure AD Connect Sync encryption key</span></span>
>[!IMPORTANT]
><span data-ttu-id="bc50b-128">Hello následující postupy platí pouze pro tooAzure AD Connect sestavení 1.1.443.0 nebo starší.</span><span class="sxs-lookup"><span data-stu-id="bc50b-128">hello following procedures only apply tooAzure AD Connect build 1.1.443.0 or older.</span></span>

<span data-ttu-id="bc50b-129">Použijte následující postupy tooabandon hello šifrovací klíč hello.</span><span class="sxs-lookup"><span data-stu-id="bc50b-129">Use hello following procedures tooabandon hello encryption key.</span></span>

### <a name="what-toodo-if-you-need-tooabandon-hello-encryption-key"></a><span data-ttu-id="bc50b-130">Jaké toodo, pokud potřebujete tooabandon hello šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="bc50b-130">What toodo if you need tooabandon hello encryption key</span></span>

<span data-ttu-id="bc50b-131">Pokud potřebujete tooabandon hello šifrovací klíč, použijte následující postupy tooaccomplish hello.</span><span class="sxs-lookup"><span data-stu-id="bc50b-131">If you need tooabandon hello encryption key, use hello following procedures tooaccomplish this.</span></span>

1. [<span data-ttu-id="bc50b-132">Zrušte hello existující šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="bc50b-132">Abandon hello existing encryption key</span></span>](#abandon-the-existing-encryption-key)

2. [<span data-ttu-id="bc50b-133">Zadejte heslo hello hello účet služby AD DS</span><span class="sxs-lookup"><span data-stu-id="bc50b-133">Provide hello password of hello AD DS account</span></span>](#provide-the-password-of-the-ad-ds-account)

3. [<span data-ttu-id="bc50b-134">Znovu inicializovat hello heslo hello účet synchronizační služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc50b-134">Reinitialize hello password of hello Azure AD sync account</span></span>](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [<span data-ttu-id="bc50b-135">Spustit hello synchronizační služby</span><span class="sxs-lookup"><span data-stu-id="bc50b-135">Start hello Synchronization Service</span></span>](#start-the-synchronization-service)

#### <a name="abandon-hello-existing-encryption-key"></a><span data-ttu-id="bc50b-136">Zrušte hello existující šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="bc50b-136">Abandon hello existing encryption key</span></span>
<span data-ttu-id="bc50b-137">Zrušte hello existující šifrovací klíč, že nový šifrovací klíč lze vytvořit:</span><span class="sxs-lookup"><span data-stu-id="bc50b-137">Abandon hello existing encryption key so that new encryption key can be created:</span></span>

1. <span data-ttu-id="bc50b-138">Přihlaste se tooyour připojení serveru služby Azure AD jako správce.</span><span class="sxs-lookup"><span data-stu-id="bc50b-138">Log in tooyour Azure AD Connect Server as administrator.</span></span>

2. <span data-ttu-id="bc50b-139">Spusťte novou relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc50b-139">Start a new PowerShell session.</span></span>

3. <span data-ttu-id="bc50b-140">Přejděte toofolder:`$env:Program Files\Microsoft Azure AD Sync\bin\`</span><span class="sxs-lookup"><span data-stu-id="bc50b-140">Navigate toofolder: `$env:Program Files\Microsoft Azure AD Sync\bin\`</span></span>

4. <span data-ttu-id="bc50b-141">Spusťte příkaz hello:`./miiskmu.exe /a`</span><span class="sxs-lookup"><span data-stu-id="bc50b-141">Run hello command: `./miiskmu.exe /a`</span></span>

![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-hello-password-of-hello-ad-ds-account"></a><span data-ttu-id="bc50b-143">Zadejte heslo hello hello účet služby AD DS</span><span class="sxs-lookup"><span data-stu-id="bc50b-143">Provide hello password of hello AD DS account</span></span>
<span data-ttu-id="bc50b-144">Jak hello stávající hesla uložená v databázi hello již nebude možné dešifrovat, musíte tooprovide hello synchronizační služba s hello heslo účtu hello služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="bc50b-144">As hello existing passwords stored inside hello database can no longer be decrypted, you need tooprovide hello Synchronization Service with hello password of hello AD DS account.</span></span> <span data-ttu-id="bc50b-145">Hello synchronizační služba šifruje hello hesla pomocí hello nový šifrovací klíč:</span><span class="sxs-lookup"><span data-stu-id="bc50b-145">hello Synchronization Service encrypts hello passwords using hello new encryption key:</span></span>

1. <span data-ttu-id="bc50b-146">Spusťte hello Synchronization Service Manager (START → synchronizace Service).</span><span class="sxs-lookup"><span data-stu-id="bc50b-146">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="bc50b-147">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="bc50b-147">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  
2. <span data-ttu-id="bc50b-148">Přejděte toohello **konektory** kartě.</span><span class="sxs-lookup"><span data-stu-id="bc50b-148">Go toohello **Connectors** tab.</span></span>
3. <span data-ttu-id="bc50b-149">Vyberte hello **AD Connector.** odpovídající tooyour místní AD.</span><span class="sxs-lookup"><span data-stu-id="bc50b-149">Select hello **AD Connector** that corresponds tooyour on-premises AD.</span></span> <span data-ttu-id="bc50b-150">Pokud máte více než jeden konektor AD, opakujte hello následující kroky pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="bc50b-150">If you have more than one AD connector, repeat hello following steps for each of them.</span></span>
4. <span data-ttu-id="bc50b-151">V části **akce**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="bc50b-151">Under **Actions**, select **Properties**.</span></span>
5. <span data-ttu-id="bc50b-152">V místním dialogovém okně hello, vyberte **připojit doménové struktury Directory tooActive**:</span><span class="sxs-lookup"><span data-stu-id="bc50b-152">In hello pop-up dialog, select **Connect tooActive Directory Forest**:</span></span>
6. <span data-ttu-id="bc50b-153">Zadejte heslo hello účtu hello služby AD DS v hello **heslo** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bc50b-153">Enter hello password of hello AD DS account in hello **Password** textbox.</span></span> <span data-ttu-id="bc50b-154">Pokud neznáte jeho heslo, je nutné ji nastavit tooa známé hodnoty před provedením tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="bc50b-154">If you do not know its password, you must set it tooa known value before performing this step.</span></span>
7. <span data-ttu-id="bc50b-155">Klikněte na tlačítko **OK** toosave hello nové heslo a automaticky otevíraný dialog zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="bc50b-155">Click **OK** toosave hello new password and close hello pop-up dialog.</span></span>
<span data-ttu-id="bc50b-156">![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="bc50b-156">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>

#### <a name="reinitialize-hello-password-of-hello-azure-ad-sync-account"></a><span data-ttu-id="bc50b-157">Znovu inicializovat hello heslo hello účet synchronizační služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc50b-157">Reinitialize hello password of hello Azure AD sync account</span></span>
<span data-ttu-id="bc50b-158">Nemůžete přímo zadat heslo hello hello Azure AD služba účet toohello synchronizační služba.</span><span class="sxs-lookup"><span data-stu-id="bc50b-158">You cannot directly provide hello password of hello Azure AD service account toohello Synchronization Service.</span></span> <span data-ttu-id="bc50b-159">Místo toho musíte rutinu hello toouse **přidat ADSyncAADServiceAccount** tooreinitialize hello účet služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc50b-159">Instead, you need toouse hello cmdlet **Add-ADSyncAADServiceAccount** tooreinitialize hello Azure AD service account.</span></span> <span data-ttu-id="bc50b-160">rutina Hello resetuje heslo účtu hello a je k dispozici toohello synchronizační služba:</span><span class="sxs-lookup"><span data-stu-id="bc50b-160">hello cmdlet resets hello account password and makes it available toohello Synchronization Service:</span></span>

1. <span data-ttu-id="bc50b-161">Na server Azure AD Connect hello spusťte novou relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc50b-161">Start a new PowerShell session on hello Azure AD Connect server.</span></span>
2. <span data-ttu-id="bc50b-162">Spusťte rutinu `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="bc50b-162">Run cmdlet `Add-ADSyncAADServiceAccount`.</span></span>
3. <span data-ttu-id="bc50b-163">V místním dialogovém okně hello zadejte přihlašovací údaje hello Azure AD globálního správce pro vašeho tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc50b-163">In hello pop-up dialog, provide hello Azure AD Global admin credentials for your Azure AD tenant.</span></span>
<span data-ttu-id="bc50b-164">![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key7.png)</span><span class="sxs-lookup"><span data-stu-id="bc50b-164">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key7.png)</span></span>
4. <span data-ttu-id="bc50b-165">Pokud neproběhne úspěšně, zobrazí se příkazový řádek Powershellu hello.</span><span class="sxs-lookup"><span data-stu-id="bc50b-165">If it is successful, you will see hello PowerShell command prompt.</span></span>

#### <a name="start-hello-synchronization-service"></a><span data-ttu-id="bc50b-166">Spustit hello synchronizační služby</span><span class="sxs-lookup"><span data-stu-id="bc50b-166">Start hello Synchronization Service</span></span>
<span data-ttu-id="bc50b-167">Teď, když hello synchronizační služba má přístup toohello šifrovací klíč a všechny hello hesla je nutné, můžete restartovat službu hello v hello správce řízení služeb systému Windows:</span><span class="sxs-lookup"><span data-stu-id="bc50b-167">Now that hello Synchronization Service has access toohello encryption key and all hello passwords it needs, you can restart hello service in hello Windows Service Control Manager:</span></span>


1. <span data-ttu-id="bc50b-168">Přejděte tooWindows správce řízení služeb (počáteční → služeb).</span><span class="sxs-lookup"><span data-stu-id="bc50b-168">Go tooWindows Service Control Manager (START → Services).</span></span>
2. <span data-ttu-id="bc50b-169">Vyberte **Microsoft Azure AD Sync** a klikněte na tlačítko Restartovat.</span><span class="sxs-lookup"><span data-stu-id="bc50b-169">Select **Microsoft Azure AD Sync** and click Restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc50b-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc50b-170">Next steps</span></span>
<span data-ttu-id="bc50b-171">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="bc50b-171">**Overview topics**</span></span>

* [<span data-ttu-id="bc50b-172">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="bc50b-172">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="bc50b-173">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc50b-173">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
