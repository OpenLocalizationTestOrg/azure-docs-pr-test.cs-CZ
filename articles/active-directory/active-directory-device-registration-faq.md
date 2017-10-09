---
title: "aaaAzure služby Active Directory Automatická registrace zařízení – nejčastější dotazy | Microsoft Docs"
description: "Automatická registrace zařízení s Azure Active Directory, – nejčastější dotazy."
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
ms.openlocfilehash: ba7f113fd3bc310def001a1f44d938b0be71dba8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-automatic-device-registration-faq"></a><span data-ttu-id="f067d-103">Nejčastější dotazy ohledně automatické registrace zařízení v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f067d-103">Azure Active Directory automatic device registration FAQ</span></span>

<span data-ttu-id="f067d-104">**Otázka: I nedávno zaregistrovaná zařízení hello. Proč nevidím hello zařízení v části Moje informace o uživateli v hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="f067d-104">**Q: I registered hello device recently. Why can’t I see hello device under my user info in hello Azure portal?**</span></span>

<span data-ttu-id="f067d-105">**Odpověď:** zařízení s Windows 10, které jsou připojené k doméně se automatická registrace zařízení se nezobrazí v části informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="f067d-105">**A:** Windows 10 devices that are domain-joined with automatic device registration do not show up under hello USER info.</span></span>
<span data-ttu-id="f067d-106">Je nutné toouse prostředí PowerShell toosee všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="f067d-106">You need toouse PowerShell toosee all devices.</span></span> 

<span data-ttu-id="f067d-107">Pouze hello následující zařízení jsou uvedené v části hello informace o uživateli:</span><span class="sxs-lookup"><span data-stu-id="f067d-107">Only hello following devices are listed under hello USER info:</span></span>

- <span data-ttu-id="f067d-108">Všechny osobní zařízení, které nejsou připojené k enterprise</span><span class="sxs-lookup"><span data-stu-id="f067d-108">All personal devices that are not enterprise joined</span></span> 
- <span data-ttu-id="f067d-109">Všechny bez – Windows 10 nebo Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="f067d-109">All non-Windows 10 / Windows Server 2016</span></span> 
- <span data-ttu-id="f067d-110">Všechna zařízení jiný systém než Windows</span><span class="sxs-lookup"><span data-stu-id="f067d-110">All non-Windows devices</span></span> 

---

<span data-ttu-id="f067d-111">**Otázka: Proč nevidím všechna hello zařízení zaregistrované v Azure Active Directory v hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="f067d-111">**Q: Why can I not see all hello devices registered in Azure Active Directory in hello Azure portal?**</span></span> 

<span data-ttu-id="f067d-112">**Odpověď:** v současné době neexistuje žádný způsob, jak toosee všech registrovaných zařízení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f067d-112">**A:** Currently, there is no way toosee all registered devices in hello Azure portal.</span></span> <span data-ttu-id="f067d-113">Můžete vytvořit prostředí Azure PowerShell toofind všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="f067d-113">You can use Azure PowerShell toofind all devices.</span></span> <span data-ttu-id="f067d-114">Další podrobnosti najdete v tématu hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="f067d-114">For more details, see hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span></span>

--- 

<span data-ttu-id="f067d-115">**Otázka: jak lze zjistit, jaké stav hello zařízení registrace klienta hello je?**</span><span class="sxs-lookup"><span data-stu-id="f067d-115">**Q: How do I know what hello device registration state of hello client is?**</span></span>

<span data-ttu-id="f067d-116">**Odpověď:** stav registrace zařízení hello závisí na:</span><span class="sxs-lookup"><span data-stu-id="f067d-116">**A:** hello device registration state depends on:</span></span>

- <span data-ttu-id="f067d-117">Jaká zařízení hello</span><span class="sxs-lookup"><span data-stu-id="f067d-117">What hello device is</span></span>
- <span data-ttu-id="f067d-118">Jak byl zaregistrován</span><span class="sxs-lookup"><span data-stu-id="f067d-118">How it was registered</span></span> 
- <span data-ttu-id="f067d-119">Všechny podrobnosti související s tooit.</span><span class="sxs-lookup"><span data-stu-id="f067d-119">Any details related tooit.</span></span> 
 

---

<span data-ttu-id="f067d-120">**Otázka: Proč je zařízení I odstranili v hello Azure portálu nebo pomocí prostředí Windows PowerShell pořád objevuje as registrované?**</span><span class="sxs-lookup"><span data-stu-id="f067d-120">**Q: Why is a device I have deleted in hello Azure portal or using Windows PowerShell still listed as registered?**</span></span>

<span data-ttu-id="f067d-121">**Odpověď:** toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="f067d-121">**A:** This is by design.</span></span> <span data-ttu-id="f067d-122">Hello zařízení nebude mít přístup tooresources v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="f067d-122">hello device will not have access tooresources in hello cloud.</span></span> <span data-ttu-id="f067d-123">Pokud chcete tooremove hello zařízení a znovu zaregistrovat, musí být manuální akce toobe pořízené hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="f067d-123">If you want tooremove hello device and register again, a manual action must be toobe taken on hello device.</span></span> 

<span data-ttu-id="f067d-124">Pro Windows 10 a Windows Server 2016, které jsou místní AD připojené k doméně:</span><span class="sxs-lookup"><span data-stu-id="f067d-124">For Windows 10 and Windows Server 2016 that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="f067d-125">Otevřete hello příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="f067d-125">Open hello command prompt as an administrator.</span></span>

2.  <span data-ttu-id="f067d-126">Typ`dsregcmd.exe /debug /leave`</span><span class="sxs-lookup"><span data-stu-id="f067d-126">Type `dsregcmd.exe /debug /leave`</span></span>

3.  <span data-ttu-id="f067d-127">Odhlásit se a přihlaste se hello tootrigger naplánované se úloha, která znovu zaregistruje zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f067d-127">Sign out and sign in tootrigger hello scheduled task that registers hello device again.</span></span> 

<span data-ttu-id="f067d-128">Pro jiné platformy Windows, které jsou místní AD připojené k doméně:</span><span class="sxs-lookup"><span data-stu-id="f067d-128">For other Windows platforms that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="f067d-129">Otevřete hello příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="f067d-129">Open hello command prompt as an administrator.</span></span>
2.  <span data-ttu-id="f067d-130">Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span><span class="sxs-lookup"><span data-stu-id="f067d-130">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span></span>
3.  <span data-ttu-id="f067d-131">Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span><span class="sxs-lookup"><span data-stu-id="f067d-131">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span></span>

---

<span data-ttu-id="f067d-132">**Otázka: Proč vidí položky duplicitní zařízení na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="f067d-132">**Q: Why do I see duplicate device entries in Azure portal?**</span></span>

<span data-ttu-id="f067d-133">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="f067d-133">**A:**</span></span>

-   <span data-ttu-id="f067d-134">Pro Windows 10 a Windows Server 2016, pokud jsou opakovaných pokusech toounjoin a znova připojit hello stejného zařízení, může být duplicitní položky.</span><span class="sxs-lookup"><span data-stu-id="f067d-134">For Windows 10 and Windows Server 2016, if they are repeated attempts toounjoin and re-join hello same device, there might be duplicate entries.</span></span> 

-   <span data-ttu-id="f067d-135">Pokud jste použili přidat pracovní nebo školní účet, každý uživatel systému windows, který používá přidat pracovní nebo školní účet se vytvoří nový záznam zařízení s hello stejný název zařízení.</span><span class="sxs-lookup"><span data-stu-id="f067d-135">If you have used Add Work or School Account, each windows user who uses Add Work or School Account will create a new device record with hello same device name.</span></span>

-   <span data-ttu-id="f067d-136">Jiné platformy Windows, které jsou místní AD připojené k doméně pomocí automatické registrace vytvoří nový záznam zařízení s hello stejný název zařízení pro každého uživatele domény, který se přihlásí do zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f067d-136">Other Windows platforms that are on-premises AD domain-joined using automatic registration will create a new device record with hello same device name for each domain user who logs into hello device.</span></span> 

-   <span data-ttu-id="f067d-137">AADJ počítač, který bylo vymazáno, přeinstalovat a znovu spojena s hello stejný název, se zobrazí jako jiný záznam se hello stejný název zařízení.</span><span class="sxs-lookup"><span data-stu-id="f067d-137">An AADJ machine that has been wiped, re-installed and re-joined with hello same name, will show up as another record with hello same device name.</span></span>

---

<span data-ttu-id="f067d-138">**Otázka: Proč může uživatel i nadále přístup k prostředkům ze zařízení, které I zakázali v hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="f067d-138">**Q: Why can a user still access resources from a device I have disabled in hello Azure portal?**</span></span>

<span data-ttu-id="f067d-139">**Odpověď:** může trvat až hodinu tooan odvolat toobe, použít.</span><span class="sxs-lookup"><span data-stu-id="f067d-139">**A:** It can take up tooan hour for a revoke toobe applied.</span></span>

>[!Note] 
><span data-ttu-id="f067d-140">Pro zařízení, ke ztrátě doporučujeme vymazání hello zařízení tooensure, že uživatelé nemají přístup k zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f067d-140">For lost devices, we recommend wiping hello device tooensure that users cannot access hello device.</span></span> <span data-ttu-id="f067d-141">Další podrobnosti najdete v tématu [registrovat zařízení pro správu v Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="f067d-141">For more details, see [Enroll devices for management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span></span> 


---

<span data-ttu-id="f067d-142">**Otázka: Proč Moji uživatelé vidí "Tam nedostanete odsud"?**</span><span class="sxs-lookup"><span data-stu-id="f067d-142">**Q: Why do my users see “You can’t get there from here”?**</span></span>

<span data-ttu-id="f067d-143">**Odpověď:** Pokud jste nakonfigurovali určité toorequire pravidla podmíněného přístupu stavu konkrétní zařízení a zařízení hello nesplňuje kritéria hello, uživatelé se zablokuje a zobrazí tato zpráva.</span><span class="sxs-lookup"><span data-stu-id="f067d-143">**A:** If you have configured certain conditional access rules toorequire a specific device state and hello device does not meet hello criteria, users are blocked and see this message.</span></span> <span data-ttu-id="f067d-144">Prosím vyhodnocení hello pravidel a ujistěte se, že zařízení hello je možné toomeet hello kritéria tooavoid tuto zprávu.</span><span class="sxs-lookup"><span data-stu-id="f067d-144">Please evaluate hello rules and ensure that hello device is able toomeet hello criteria tooavoid this message.</span></span>

---


<span data-ttu-id="f067d-145">**Otázka: I najdete v části hello zařízení záznam zařízení v části hello informace o uživateli v hello portál Azure a můžete zobrazit stav hello as registrované v klientovi hello. Se, že správně nastavit pro použití podmíněného přístupu?**</span><span class="sxs-lookup"><span data-stu-id="f067d-145">**Q: I see hello device record under hello USER info in hello Azure portal and can see hello state as registered on hello client. Am I setup correctly for using conditional access?**</span></span>

<span data-ttu-id="f067d-146">**Odpověď:** záznam zařízení hello (deviceID) a stav na portálu Azure hello musí odpovídat hello klienta a splnění libovolného kritéria hodnocení pro podmíněný přístup.</span><span class="sxs-lookup"><span data-stu-id="f067d-146">**A:** hello device record (deviceID) and state on hello Azure portal must match hello client and meet any evaluation criteria for conditional access.</span></span> <span data-ttu-id="f067d-147">Další podrobnosti najdete v tématu [Začínáme s Azure Active Directory Device Registration](active-directory-device-registration.md).</span><span class="sxs-lookup"><span data-stu-id="f067d-147">For more details, see [Get started with Azure Active Directory Device Registration](active-directory-device-registration.md).</span></span>

---

<span data-ttu-id="f067d-148">**Otázka: Proč se zobrazila zpráva "uživatelské jméno nebo heslo je chybné." pro zařízení I právě připojené tooAzure AD?**</span><span class="sxs-lookup"><span data-stu-id="f067d-148">**Q: Why do I get a "username or password is incorrect" message for a device I have just joined tooAzure AD?**</span></span>

<span data-ttu-id="f067d-149">**Odpověď:** běžných příčin pro tento scénář jsou:</span><span class="sxs-lookup"><span data-stu-id="f067d-149">**A:** Common reasons for this scenario are:</span></span>

- <span data-ttu-id="f067d-150">Pověření uživatele již není platný.</span><span class="sxs-lookup"><span data-stu-id="f067d-150">Your user credentials are no longer valid.</span></span>

- <span data-ttu-id="f067d-151">Váš počítač je nelze toocommunicate s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f067d-151">Your computer is unable toocommunicate with Azure Active Directory.</span></span> <span data-ttu-id="f067d-152">Zkontrolujte všechny problémy se síťovým připojením.</span><span class="sxs-lookup"><span data-stu-id="f067d-152">Check for any network connectivity issues.</span></span>

- <span data-ttu-id="f067d-153">nebyly splněny Azure AD Join požadavky Hello.</span><span class="sxs-lookup"><span data-stu-id="f067d-153">hello Azure AD Join pre-requisites were not met.</span></span> <span data-ttu-id="f067d-154">Zkontrolujte, že jste provedli kroky hello [rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f067d-154">Please ensure that you have followed hello steps for [Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span>  

- <span data-ttu-id="f067d-155">Federované přihlášení vyžaduje váš federační server toosupport WS-Trust aktivní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f067d-155">Federated logins requires your federation server toosupport a WS-Trust active endpoint.</span></span> 

---

<span data-ttu-id="f067d-156">**Otázka: Proč se zobrazuje hello "Oops... došlo k chybě!" dialogové okno při připojení k počítači?**</span><span class="sxs-lookup"><span data-stu-id="f067d-156">**Q: Why do I see hello “Oops… an error occurred!" dialog when I try do join my PC?**</span></span>

<span data-ttu-id="f067d-157">**Odpověď:** jedná o výsledek nastavení registrace Azure Active Directory s Intune.</span><span class="sxs-lookup"><span data-stu-id="f067d-157">**A:** This is a result of setting up Azure Active Directory enrollment with Intune.</span></span> <span data-ttu-id="f067d-158">Další podrobnosti najdete v tématu [nastavení správy pro zařízení Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span><span class="sxs-lookup"><span data-stu-id="f067d-158">For more details, see [Set up Windows device management](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span></span>  

---

<span data-ttu-id="f067d-159">**Otázka: Proč můj pokus o toojoin počítači nezdaří, i když se mi nepřišel. veškeré informace o chybě?**</span><span class="sxs-lookup"><span data-stu-id="f067d-159">**Q: Why did my attempt toojoin a PC fail although I didn't get any error information?**</span></span>

<span data-ttu-id="f067d-160">**Odpověď:** pravděpodobnou příčinou je, že hello uživatel je přihlášen toohello zařízení pomocí hello předdefinovaný účet správce.</span><span class="sxs-lookup"><span data-stu-id="f067d-160">**A:** A likely cause is that hello user is logged in toohello device using hello built-in administrator account.</span></span> <span data-ttu-id="f067d-161">Před použitím Azure Active Directory Join instalace hello toocomplete vytvořte jiný místní účet.</span><span class="sxs-lookup"><span data-stu-id="f067d-161">Please create a different local account before using Azure Active Directory Join toocomplete hello setup.</span></span> 

---

<span data-ttu-id="f067d-162">**Otázka: kde najdete pokyny k instalaci hello Automatická registrace zařízení?**</span><span class="sxs-lookup"><span data-stu-id="f067d-162">**Q: Where can I find instructions for hello setup of automatic device registration?**</span></span>

<span data-ttu-id="f067d-163">**Odpověď:** podrobné pokyny najdete v tématu [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="f067d-163">**A:** For detailed instructions, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

---

<span data-ttu-id="f067d-164">**Otázka: kde můžete najít řešení potíží s informace o hello Automatická registrace zařízení?**</span><span class="sxs-lookup"><span data-stu-id="f067d-164">**Q: Where can I find troubleshooting information about hello automatic device registration?**</span></span>

<span data-ttu-id="f067d-165">**Odpověď:** informace o odstraňování potíží, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f067d-165">**A:** For troubleshooting information, see:</span></span>

- [<span data-ttu-id="f067d-166">Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD – Windows 10 a Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="f067d-166">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)

- [<span data-ttu-id="f067d-167">Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD pro klienty nižší úrovně systému Windows</span><span class="sxs-lookup"><span data-stu-id="f067d-167">Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

