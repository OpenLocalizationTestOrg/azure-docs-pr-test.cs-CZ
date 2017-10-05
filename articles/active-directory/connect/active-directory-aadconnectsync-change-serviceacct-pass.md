---
title: "Synchronizace Azure AD Connect: Změna účtu služby Azure AD Connect Sync | Microsoft Docs"
description: "Tento dokument téma popisuje šifrovací klíč a jak ho ukončil po změně hesla."
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
ms.openlocfilehash: bf6234d0810f870909957ee1c1e33c225a4922b9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a><span data-ttu-id="8583e-104">Změna hesla účtu služby synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="8583e-104">Changing the Azure AD Connect sync service account password</span></span>
<span data-ttu-id="8583e-105">Pokud změníte heslo účtu služby synchronizace Azure AD Connect, synchronizační služby nebude možné spustit správně opuštění šifrovací klíč a heslo účtu služby Azure AD Connect sync znovu inicializován.</span><span class="sxs-lookup"><span data-stu-id="8583e-105">If you change the  Azure AD Connect sync service account password, the Synchronization Service will not be able start correctly until you have abandoned the encryption key and reinitialized the Azure AD Connect sync service account password.</span></span> 

<span data-ttu-id="8583e-106">Azure AD Connect: jako součást synchronizační služby používá šifrovací klíč k uložení hesla účtů služby AD DS a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8583e-106">Azure AD Connect, as part of the Synchronization Services uses an encryption key to store the passwords of the AD DS and Azure AD service accounts.</span></span>  <span data-ttu-id="8583e-107">Tyto účty jsou zašifrované, než jsou uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="8583e-107">These accounts are encrypted before they are stored in the database.</span></span> 

<span data-ttu-id="8583e-108">Šifrovací klíč použít zabezpečené pomocí [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span><span class="sxs-lookup"><span data-stu-id="8583e-108">The encryption key used is secured using [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span></span> <span data-ttu-id="8583e-109">Rozhraní DPAPI chrání klíč šifrování pomocí **heslo účtu služby Azure AD Connect sync**.</span><span class="sxs-lookup"><span data-stu-id="8583e-109">DPAPI protects the encryption key using the **password of the Azure AD Connect sync service account**.</span></span> 

<span data-ttu-id="8583e-110">Pokud potřebujete změnit heslo účtu služby můžete použít postupy v [zrušení šifrovací klíč Azure AD Connect Sync](#abandoning-the-azure-ad-connect-sync-encryption-key) toho chcete dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="8583e-110">If you need to change the service account password you can use the procedures in [Abandoning the Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) to accomplish this.</span></span>  <span data-ttu-id="8583e-111">Tyto postupy by měla být používána Pokud potřebujete abandon šifrovacího klíče z jakéhokoli důvodu.</span><span class="sxs-lookup"><span data-stu-id="8583e-111">These procedures should also be used if you need to abandon the encryption key for any reason.</span></span>

##<a name="issues-that-arise-from-changing-the-password"></a><span data-ttu-id="8583e-112">Problémy, které vznikají z Změna hesla</span><span class="sxs-lookup"><span data-stu-id="8583e-112">Issues that arise from changing the password</span></span>
<span data-ttu-id="8583e-113">Existují dvě věci, které je třeba provést, pokud změníte heslo účtu služby.</span><span class="sxs-lookup"><span data-stu-id="8583e-113">There are two things that need to be done when you change the service account password.</span></span>

<span data-ttu-id="8583e-114">Je třeba nejprve, chcete-li změnit heslo v části správce řízení služeb systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8583e-114">First, you need to change the password under the Windows Service Control Manager.</span></span>  <span data-ttu-id="8583e-115">Dokud nebude tento problém vyřešen. zobrazí se následující chyby:</span><span class="sxs-lookup"><span data-stu-id="8583e-115">Until this issue is resolved you will see following errors:</span></span>


- <span data-ttu-id="8583e-116">Pokud se pokusíte spustit synchronizační službu v modulu snap-in Správce řízení služeb systému Windows, se zobrazí chyba "**Windows v místním počítači nelze spustit službu Microsoft Azure AD Sync**".</span><span class="sxs-lookup"><span data-stu-id="8583e-116">If you try to start the Synchronization Service in Windows Service Control Manager, you receive the error "**Windows could not start the Microsoft Azure AD Sync service on Local Computer**".</span></span> <span data-ttu-id="8583e-117">**Chyba. 1069: Službu se nepodařilo spustit z důvodu selhání přihlášení.** "</span><span class="sxs-lookup"><span data-stu-id="8583e-117">**Error 1069: The service did not start due to a logon failure.**"</span></span>
- <span data-ttu-id="8583e-118">V prohlížeči událostí systému Windows v protokolu událostí systému obsahuje chybu s **7038 ID události** a zpráva "**ADSync service nebylo možné se přihlásit jako se se současně nakonfigurovaným heslem z důvodu následující chyby: uživatel jméno nebo heslo není správné.** "</span><span class="sxs-lookup"><span data-stu-id="8583e-118">Under Windows Event Viewer, the system event log contains an error with **Event ID 7038** and message “**The ADSync service was unable to log on as with the currently configured password due to the following error: The user name or password is incorrect.**"</span></span>

<span data-ttu-id="8583e-119">Druhý pro určité podmínky, pokud je aktualizovat heslo, služba synchronizace už načíst šifrovací klíč pomocí rozhraní DPAPI.</span><span class="sxs-lookup"><span data-stu-id="8583e-119">Second, under specific conditions, if the password is updated, the Synchronization Service can no longer retrieve the encryption key via DPAPI.</span></span> <span data-ttu-id="8583e-120">Bez šifrovací klíč nemůže dešifrovat služba synchronizace hesel vyžadovaných k synchronizaci z místní AD a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8583e-120">Without the encryption key, the Synchronization Service cannot decrypt the passwords required to synchronize to/from on-premises AD and Azure AD.</span></span>
<span data-ttu-id="8583e-121">Zobrazí se chyby, jako:</span><span class="sxs-lookup"><span data-stu-id="8583e-121">You will see errors such as:</span></span>

- <span data-ttu-id="8583e-122">V části správce řízení služeb systému Windows Pokud pokusu o spuštění synchronizační službu a nemůže získat šifrovací klíč, se nezdaří s chybou "** Windows se nepodařilo spustit Microsoft Azure AD Sync v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="8583e-122">Under Windows Service Control Manager, if you try to start the Synchronization Service and it cannot retrieve the encryption key, it fails with error “**Windows could not start the Microsoft Azure AD Sync on Local Computer.</span></span> <span data-ttu-id="8583e-123">Další informace vyhledejte v protokolu událostí systému.</span><span class="sxs-lookup"><span data-stu-id="8583e-123">For more information, review the System Event log.</span></span> <span data-ttu-id="8583e-124">Pokud je služba jiného výrobce než Microsoftu, obraťte se na dodavatele služby a pročtěte si kód chyby-**-21451857952 ***. "</span><span class="sxs-lookup"><span data-stu-id="8583e-124">If this is a non-Microsoft service, contact the service vendor, and refer to service-specific error code **-21451857952****.”</span></span>
- <span data-ttu-id="8583e-125">V prohlížeči událostí systému Windows v protokolu událostí aplikace obsahuje chybu s **6028 ID události** a chybovou zprávou *"**šifrovacího klíče serveru nelze získat přístup.* *"*</span><span class="sxs-lookup"><span data-stu-id="8583e-125">Under Windows Event Viewer, the application event log contains an error with **Event ID 6028** and error message *“**The server encryption key cannot be accessed.**”*</span></span>

<span data-ttu-id="8583e-126">Aby se tyto chyby neobdrží, postupujte podle pokynů v [zrušení šifrovací klíč Azure AD Connect Sync](#abandoning-the-azure-ad-connect-sync-encryption-key) při změně hesla.</span><span class="sxs-lookup"><span data-stu-id="8583e-126">To ensure that you do not receive these errors, follow the procedures in [Abandoning the Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) when changing the password.</span></span>
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a><span data-ttu-id="8583e-127">Zrušení šifrovací klíč Azure AD Connect Sync</span><span class="sxs-lookup"><span data-stu-id="8583e-127">Abandoning the Azure AD Connect Sync encryption key</span></span>
>[!IMPORTANT]
><span data-ttu-id="8583e-128">Následující postupy lze použít pouze na Azure AD Connect sestavení 1.1.443.0 nebo starší.</span><span class="sxs-lookup"><span data-stu-id="8583e-128">The following procedures only apply to Azure AD Connect build 1.1.443.0 or older.</span></span>

<span data-ttu-id="8583e-129">Pomocí následujících postupů ukončil šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="8583e-129">Use the following procedures to abandon the encryption key.</span></span>

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a><span data-ttu-id="8583e-130">Co dělat, když potřebujete abandon šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="8583e-130">What to do if you need to abandon the encryption key</span></span>

<span data-ttu-id="8583e-131">Pokud potřebujete abandon šifrovací klíč, použijte k tomu následující postupy.</span><span class="sxs-lookup"><span data-stu-id="8583e-131">If you need to abandon the encryption key, use the following procedures to accomplish this.</span></span>

1. [<span data-ttu-id="8583e-132">Zrušte existující šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="8583e-132">Abandon the existing encryption key</span></span>](#abandon-the-existing-encryption-key)

2. [<span data-ttu-id="8583e-133">Zadejte heslo účtu služby AD DS</span><span class="sxs-lookup"><span data-stu-id="8583e-133">Provide the password of the AD DS account</span></span>](#provide-the-password-of-the-ad-ds-account)

3. [<span data-ttu-id="8583e-134">Znovu inicializovat heslo účtu služby Azure AD sync</span><span class="sxs-lookup"><span data-stu-id="8583e-134">Reinitialize the password of the Azure AD sync account</span></span>](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [<span data-ttu-id="8583e-135">Spusťte službu synchronizace</span><span class="sxs-lookup"><span data-stu-id="8583e-135">Start the Synchronization Service</span></span>](#start-the-synchronization-service)

#### <a name="abandon-the-existing-encryption-key"></a><span data-ttu-id="8583e-136">Zrušte existující šifrovací klíč</span><span class="sxs-lookup"><span data-stu-id="8583e-136">Abandon the existing encryption key</span></span>
<span data-ttu-id="8583e-137">Zrušte existující šifrovací klíč, že nový šifrovací klíč lze vytvořit:</span><span class="sxs-lookup"><span data-stu-id="8583e-137">Abandon the existing encryption key so that new encryption key can be created:</span></span>

1. <span data-ttu-id="8583e-138">Přihlaste se k Azure AD Connect serveru jako správce.</span><span class="sxs-lookup"><span data-stu-id="8583e-138">Log in to your Azure AD Connect Server as administrator.</span></span>

2. <span data-ttu-id="8583e-139">Spusťte novou relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8583e-139">Start a new PowerShell session.</span></span>

3. <span data-ttu-id="8583e-140">Přejděte do složky:`$env:Program Files\Microsoft Azure AD Sync\bin\`</span><span class="sxs-lookup"><span data-stu-id="8583e-140">Navigate to folder: `$env:Program Files\Microsoft Azure AD Sync\bin\`</span></span>

4. <span data-ttu-id="8583e-141">Spusťte příkaz:`./miiskmu.exe /a`</span><span class="sxs-lookup"><span data-stu-id="8583e-141">Run the command: `./miiskmu.exe /a`</span></span>

![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a><span data-ttu-id="8583e-143">Zadejte heslo účtu služby AD DS</span><span class="sxs-lookup"><span data-stu-id="8583e-143">Provide the password of the AD DS account</span></span>
<span data-ttu-id="8583e-144">Jak existující hesel uložených v databázi již nebude možné dešifrovat, musíte poskytnout synchronizační služby heslo účtu služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="8583e-144">As the existing passwords stored inside the database can no longer be decrypted, you need to provide the Synchronization Service with the password of the AD DS account.</span></span> <span data-ttu-id="8583e-145">Služba synchronizace šifruje pomocí nového klíče šifrování hesla:</span><span class="sxs-lookup"><span data-stu-id="8583e-145">The Synchronization Service encrypts the passwords using the new encryption key:</span></span>

1. <span data-ttu-id="8583e-146">Spusťte Synchronization Service Manager (Služba synchronizace → START).</span><span class="sxs-lookup"><span data-stu-id="8583e-146">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="8583e-147">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="8583e-147">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  
2. <span data-ttu-id="8583e-148">Přejděte na **konektory** kartě.</span><span class="sxs-lookup"><span data-stu-id="8583e-148">Go to the **Connectors** tab.</span></span>
3. <span data-ttu-id="8583e-149">Vyberte **AD Connector.** odpovídající místní AD.</span><span class="sxs-lookup"><span data-stu-id="8583e-149">Select the **AD Connector** that corresponds to your on-premises AD.</span></span> <span data-ttu-id="8583e-150">Pokud máte více než jeden konektor AD, opakujte tyto kroky pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="8583e-150">If you have more than one AD connector, repeat the following steps for each of them.</span></span>
4. <span data-ttu-id="8583e-151">V části **akce**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8583e-151">Under **Actions**, select **Properties**.</span></span>
5. <span data-ttu-id="8583e-152">V dialogovém okně automaticky otevírané okno Vyberte **připojit k doménové struktuře služby Active Directory**:</span><span class="sxs-lookup"><span data-stu-id="8583e-152">In the pop-up dialog, select **Connect to Active Directory Forest**:</span></span>
6. <span data-ttu-id="8583e-153">Zadejte heslo účtu služby AD DS v **heslo** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8583e-153">Enter the password of the AD DS account in the **Password** textbox.</span></span> <span data-ttu-id="8583e-154">Pokud neznáte jeho heslo, musí ho nastavit na hodnotu známé před provedením tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="8583e-154">If you do not know its password, you must set it to a known value before performing this step.</span></span>
7. <span data-ttu-id="8583e-155">Klikněte na tlačítko **OK** nové heslo uložte a zavřete dialogové okno automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="8583e-155">Click **OK** to save the new password and close the pop-up dialog.</span></span>
<span data-ttu-id="8583e-156">![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="8583e-156">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a><span data-ttu-id="8583e-157">Znovu inicializovat heslo účtu služby Azure AD sync</span><span class="sxs-lookup"><span data-stu-id="8583e-157">Reinitialize the password of the Azure AD sync account</span></span>
<span data-ttu-id="8583e-158">Pro synchronizační službu nelze přímo zadejte heslo účtu služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8583e-158">You cannot directly provide the password of the Azure AD service account to the Synchronization Service.</span></span> <span data-ttu-id="8583e-159">Místo toho musíte použít rutinu **přidat ADSyncAADServiceAccount** znovu inicializovat účtu služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8583e-159">Instead, you need to use the cmdlet **Add-ADSyncAADServiceAccount** to reinitialize the Azure AD service account.</span></span> <span data-ttu-id="8583e-160">Rutina resetuje heslo k účtu a je k dispozici služba synchronizace:</span><span class="sxs-lookup"><span data-stu-id="8583e-160">The cmdlet resets the account password and makes it available to the Synchronization Service:</span></span>

1. <span data-ttu-id="8583e-161">Spusťte novou relaci prostředí PowerShell na server Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="8583e-161">Start a new PowerShell session on the Azure AD Connect server.</span></span>
2. <span data-ttu-id="8583e-162">Spusťte rutinu `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="8583e-162">Run cmdlet `Add-ADSyncAADServiceAccount`.</span></span>
3. <span data-ttu-id="8583e-163">V dialogovém okně automaticky otevírané okno Zadejte přihlašovací údaje Azure AD globálního správce pro vašeho tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8583e-163">In the pop-up dialog, provide the Azure AD Global admin credentials for your Azure AD tenant.</span></span>
<span data-ttu-id="8583e-164">![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key7.png)</span><span class="sxs-lookup"><span data-stu-id="8583e-164">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key7.png)</span></span>
4. <span data-ttu-id="8583e-165">Pokud neproběhne úspěšně, zobrazí se příkazový řádek Powershellu.</span><span class="sxs-lookup"><span data-stu-id="8583e-165">If it is successful, you will see the PowerShell command prompt.</span></span>

#### <a name="start-the-synchronization-service"></a><span data-ttu-id="8583e-166">Spusťte službu synchronizace</span><span class="sxs-lookup"><span data-stu-id="8583e-166">Start the Synchronization Service</span></span>
<span data-ttu-id="8583e-167">Teď, když služba synchronizace má přístup k šifrovací klíč a všechna hesla, které potřebuje, můžete službu restartovat správce řízení služeb systému Windows:</span><span class="sxs-lookup"><span data-stu-id="8583e-167">Now that the Synchronization Service has access to the encryption key and all the passwords it needs, you can restart the service in the Windows Service Control Manager:</span></span>


1. <span data-ttu-id="8583e-168">Přejděte do Windows správce řízení služeb (počáteční → služeb).</span><span class="sxs-lookup"><span data-stu-id="8583e-168">Go to Windows Service Control Manager (START → Services).</span></span>
2. <span data-ttu-id="8583e-169">Vyberte **Microsoft Azure AD Sync** a klikněte na tlačítko Restartovat.</span><span class="sxs-lookup"><span data-stu-id="8583e-169">Select **Microsoft Azure AD Sync** and click Restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8583e-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8583e-170">Next steps</span></span>
<span data-ttu-id="8583e-171">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="8583e-171">**Overview topics**</span></span>

* [<span data-ttu-id="8583e-172">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="8583e-172">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="8583e-173">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8583e-173">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
