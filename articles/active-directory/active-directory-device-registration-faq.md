---
title: "Azure Active Directory Automatická registrace zařízení – nejčastější dotazy | Microsoft Docs"
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
ms.openlocfilehash: 29751239ae2a26cd7b07ddd0d8a8e706d4056b68
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-automatic-device-registration-faq"></a><span data-ttu-id="93390-103">Nejčastější dotazy ohledně automatické registrace zařízení v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="93390-103">Azure Active Directory automatic device registration FAQ</span></span>

<span data-ttu-id="93390-104">**Otázka: I nedávno zaregistrovaná zařízení. Proč nevidím zařízení v části Moje informace o uživateli na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="93390-104">**Q: I registered the device recently. Why can’t I see the device under my user info in the Azure portal?**</span></span>

<span data-ttu-id="93390-105">**Odpověď:** zařízení s Windows 10, které jsou připojené k doméně se automatická registrace zařízení se nezobrazí v části informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="93390-105">**A:** Windows 10 devices that are domain-joined with automatic device registration do not show up under the USER info.</span></span>
<span data-ttu-id="93390-106">Budete muset použít PowerShell zobrazíte všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-106">You need to use PowerShell to see all devices.</span></span> 

<span data-ttu-id="93390-107">Následující zařízení jsou uvedené v části informace o uživateli:</span><span class="sxs-lookup"><span data-stu-id="93390-107">Only the following devices are listed under the USER info:</span></span>

- <span data-ttu-id="93390-108">Všechny osobní zařízení, které nejsou připojené k enterprise</span><span class="sxs-lookup"><span data-stu-id="93390-108">All personal devices that are not enterprise joined</span></span> 
- <span data-ttu-id="93390-109">Všechny bez – Windows 10 nebo Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="93390-109">All non-Windows 10 / Windows Server 2016</span></span> 
- <span data-ttu-id="93390-110">Všechna zařízení jiný systém než Windows</span><span class="sxs-lookup"><span data-stu-id="93390-110">All non-Windows devices</span></span> 

---

<span data-ttu-id="93390-111">**Otázka: Proč nevidím všechna zařízení zaregistrované v Azure Active Directory na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="93390-111">**Q: Why can I not see all the devices registered in Azure Active Directory in the Azure portal?**</span></span> 

<span data-ttu-id="93390-112">**Odpověď:** v současné době neexistuje žádný způsob, jak zobrazit všech registrovaných zařízení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="93390-112">**A:** Currently, there is no way to see all registered devices in the Azure portal.</span></span> <span data-ttu-id="93390-113">Prostředí Azure PowerShell můžete použít k vyhledání všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-113">You can use Azure PowerShell to find all devices.</span></span> <span data-ttu-id="93390-114">Další podrobnosti najdete v tématu [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="93390-114">For more details, see the [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span></span>

--- 

<span data-ttu-id="93390-115">**Otázka: Jak poznám, jaký je stav registrace zařízení klienta?**</span><span class="sxs-lookup"><span data-stu-id="93390-115">**Q: How do I know what the device registration state of the client is?**</span></span>

<span data-ttu-id="93390-116">**Odpověď:** stav registrace zařízení závisí na:</span><span class="sxs-lookup"><span data-stu-id="93390-116">**A:** The device registration state depends on:</span></span>

- <span data-ttu-id="93390-117">Co je zařízení</span><span class="sxs-lookup"><span data-stu-id="93390-117">What the device is</span></span>
- <span data-ttu-id="93390-118">Jak byl zaregistrován</span><span class="sxs-lookup"><span data-stu-id="93390-118">How it was registered</span></span> 
- <span data-ttu-id="93390-119">Všechny údaje s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="93390-119">Any details related to it.</span></span> 
 

---

<span data-ttu-id="93390-120">**Otázka: Proč je zařízení odstraněný na portálu Azure nebo pomocí prostředí Windows PowerShell pořád objevuje as registrované?**</span><span class="sxs-lookup"><span data-stu-id="93390-120">**Q: Why is a device I have deleted in the Azure portal or using Windows PowerShell still listed as registered?**</span></span>

<span data-ttu-id="93390-121">**Odpověď:** toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="93390-121">**A:** This is by design.</span></span> <span data-ttu-id="93390-122">Zařízení nebude mít přístup k prostředkům v cloudu.</span><span class="sxs-lookup"><span data-stu-id="93390-122">The device will not have access to resources in the cloud.</span></span> <span data-ttu-id="93390-123">Pokud chcete zařízení odebrat a znovu zaregistrovat, musí být manuální akce mají být provedeny v zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-123">If you want to remove the device and register again, a manual action must be to be taken on the device.</span></span> 

<span data-ttu-id="93390-124">Pro Windows 10 a Windows Server 2016, které jsou místní AD připojené k doméně:</span><span class="sxs-lookup"><span data-stu-id="93390-124">For Windows 10 and Windows Server 2016 that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="93390-125">Otevřete příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="93390-125">Open the command prompt as an administrator.</span></span>

2.  <span data-ttu-id="93390-126">Typ`dsregcmd.exe /debug /leave`</span><span class="sxs-lookup"><span data-stu-id="93390-126">Type `dsregcmd.exe /debug /leave`</span></span>

3.  <span data-ttu-id="93390-127">Odhlásit se a přihlaste se k aktivaci naplánovanou úlohu, která znovu zaregistruje zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-127">Sign out and sign in to trigger the scheduled task that registers the device again.</span></span> 

<span data-ttu-id="93390-128">Pro jiné platformy Windows, které jsou místní AD připojené k doméně:</span><span class="sxs-lookup"><span data-stu-id="93390-128">For other Windows platforms that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="93390-129">Otevřete příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="93390-129">Open the command prompt as an administrator.</span></span>
2.  <span data-ttu-id="93390-130">Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span><span class="sxs-lookup"><span data-stu-id="93390-130">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span></span>
3.  <span data-ttu-id="93390-131">Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span><span class="sxs-lookup"><span data-stu-id="93390-131">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span></span>

---

<span data-ttu-id="93390-132">**Otázka: Proč vidí položky duplicitní zařízení na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="93390-132">**Q: Why do I see duplicate device entries in Azure portal?**</span></span>

<span data-ttu-id="93390-133">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="93390-133">**A:**</span></span>

-   <span data-ttu-id="93390-134">Pro Windows 10 a Windows Server 2016 Pokud jsou opakovaných pokusech o zrušení služby a znova připojit do stejného zařízení, může být duplicitní položky.</span><span class="sxs-lookup"><span data-stu-id="93390-134">For Windows 10 and Windows Server 2016, if they are repeated attempts to unjoin and re-join the same device, there might be duplicate entries.</span></span> 

-   <span data-ttu-id="93390-135">Pokud jste použili přidat pracovní nebo školní účet, bude každý uživatel systému windows, který používá přidat pracovní nebo školní účet vytvořit nový záznam zařízení se stejným názvem zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-135">If you have used Add Work or School Account, each windows user who uses Add Work or School Account will create a new device record with the same device name.</span></span>

-   <span data-ttu-id="93390-136">Jiné platformy Windows, které jsou místní AD připojené k doméně pomocí automatické registrace vytvoří nový záznam zařízení s stejný název zařízení pro každého uživatele domény, který se přihlásí do zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-136">Other Windows platforms that are on-premises AD domain-joined using automatic registration will create a new device record with the same device name for each domain user who logs into the device.</span></span> 

-   <span data-ttu-id="93390-137">AADJ počítač, který vymaže, přeinstalovat a znovu připojený se stejným názvem, se zobrazí jako jiný záznam se stejným názvem zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-137">An AADJ machine that has been wiped, re-installed and re-joined with the same name, will show up as another record with the same device name.</span></span>

---

<span data-ttu-id="93390-138">**Otázka: Proč může uživatel i nadále přístup k prostředkům ze zařízení, které I zakázali na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="93390-138">**Q: Why can a user still access resources from a device I have disabled in the Azure portal?**</span></span>

<span data-ttu-id="93390-139">**Odpověď:** může trvat až jednu hodinu pro odvolání má být použita.</span><span class="sxs-lookup"><span data-stu-id="93390-139">**A:** It can take up to an hour for a revoke to be applied.</span></span>

>[!Note] 
><span data-ttu-id="93390-140">Pro zařízení, ke ztrátě doporučujeme vymazání obsahu zařízení k zajištění, že uživatelé nemají přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="93390-140">For lost devices, we recommend wiping the device to ensure that users cannot access the device.</span></span> <span data-ttu-id="93390-141">Další podrobnosti najdete v tématu [registrovat zařízení pro správu v Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="93390-141">For more details, see [Enroll devices for management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span></span> 


---

<span data-ttu-id="93390-142">**Otázka: Proč Moji uživatelé vidí "Tam nedostanete odsud"?**</span><span class="sxs-lookup"><span data-stu-id="93390-142">**Q: Why do my users see “You can’t get there from here”?**</span></span>

<span data-ttu-id="93390-143">**Odpověď:** Pokud jste nakonfigurovali určitá pravidla podmíněného přístupu tak, aby vyžadovala stavu konkrétní zařízení a zařízení nesplňuje kritéria, uživatelé se zablokuje a zobrazí tato zpráva.</span><span class="sxs-lookup"><span data-stu-id="93390-143">**A:** If you have configured certain conditional access rules to require a specific device state and the device does not meet the criteria, users are blocked and see this message.</span></span> <span data-ttu-id="93390-144">Vyhodnocení pravidla a zajistěte, aby byla zařízení moct splňují kritéria, zobrazí se zpráva.</span><span class="sxs-lookup"><span data-stu-id="93390-144">Please evaluate the rules and ensure that the device is able to meet the criteria to avoid this message.</span></span>

---


<span data-ttu-id="93390-145">**Otázka: I najdete v části zařízení záznam zařízení v části informace o uživateli na webu Azure portal a můžete zobrazit stav as registrované na straně klienta. Se, že správně nastavit pro použití podmíněného přístupu?**</span><span class="sxs-lookup"><span data-stu-id="93390-145">**Q: I see the device record under the USER info in the Azure portal and can see the state as registered on the client. Am I setup correctly for using conditional access?**</span></span>

<span data-ttu-id="93390-146">**Odpověď:** stav na portálu Azure a záznam o zařízení (deviceID) musí odpovídat klienta a splnění libovolného kritéria hodnocení pro podmíněný přístup.</span><span class="sxs-lookup"><span data-stu-id="93390-146">**A:** The device record (deviceID) and state on the Azure portal must match the client and meet any evaluation criteria for conditional access.</span></span> <span data-ttu-id="93390-147">Další podrobnosti najdete v tématu [Začínáme s Azure Active Directory Device Registration](active-directory-device-registration.md).</span><span class="sxs-lookup"><span data-stu-id="93390-147">For more details, see [Get started with Azure Active Directory Device Registration](active-directory-device-registration.md).</span></span>

---

<span data-ttu-id="93390-148">**Otázka: Proč se zobrazila zpráva "uživatelské jméno nebo heslo je chybné." pro zařízení, které I právě připojené ke službě Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="93390-148">**Q: Why do I get a "username or password is incorrect" message for a device I have just joined to Azure AD?**</span></span>

<span data-ttu-id="93390-149">**Odpověď:** běžných příčin pro tento scénář jsou:</span><span class="sxs-lookup"><span data-stu-id="93390-149">**A:** Common reasons for this scenario are:</span></span>

- <span data-ttu-id="93390-150">Pověření uživatele již není platný.</span><span class="sxs-lookup"><span data-stu-id="93390-150">Your user credentials are no longer valid.</span></span>

- <span data-ttu-id="93390-151">Počítač je schopen komunikovat s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="93390-151">Your computer is unable to communicate with Azure Active Directory.</span></span> <span data-ttu-id="93390-152">Zkontrolujte všechny problémy se síťovým připojením.</span><span class="sxs-lookup"><span data-stu-id="93390-152">Check for any network connectivity issues.</span></span>

- <span data-ttu-id="93390-153">Azure AD Join požadavky nebyly splněny.</span><span class="sxs-lookup"><span data-stu-id="93390-153">The Azure AD Join pre-requisites were not met.</span></span> <span data-ttu-id="93390-154">Zkontrolujte, že jste provedli kroky pro [rozšíření možností cloudu k zařízení s Windows 10 prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="93390-154">Please ensure that you have followed the steps for [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span>  

- <span data-ttu-id="93390-155">Federované přihlášení vyžaduje federačním serveru pro podporu WS-Trust aktivní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="93390-155">Federated logins requires your federation server to support a WS-Trust active endpoint.</span></span> 

---

<span data-ttu-id="93390-156">**Otázka: Proč se zobrazuje "Oops... došlo k chybě!" dialogové okno při připojení k počítači?**</span><span class="sxs-lookup"><span data-stu-id="93390-156">**Q: Why do I see the “Oops… an error occurred!" dialog when I try do join my PC?**</span></span>

<span data-ttu-id="93390-157">**Odpověď:** jedná o výsledek nastavení registrace Azure Active Directory s Intune.</span><span class="sxs-lookup"><span data-stu-id="93390-157">**A:** This is a result of setting up Azure Active Directory enrollment with Intune.</span></span> <span data-ttu-id="93390-158">Další podrobnosti najdete v tématu [nastavení správy pro zařízení Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span><span class="sxs-lookup"><span data-stu-id="93390-158">For more details, see [Set up Windows device management](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span></span>  

---

<span data-ttu-id="93390-159">**Otázka: Proč moje pokus o připojení k počítači nezdaří i když se mi nepřišel. veškeré informace o chybě?**</span><span class="sxs-lookup"><span data-stu-id="93390-159">**Q: Why did my attempt to join a PC fail although I didn't get any error information?**</span></span>

<span data-ttu-id="93390-160">**Odpověď:** pravděpodobnou příčinou je, že je uživatel přihlášen do zařízení pomocí předdefinovaného účtu správce.</span><span class="sxs-lookup"><span data-stu-id="93390-160">**A:** A likely cause is that the user is logged in to the device using the built-in administrator account.</span></span> <span data-ttu-id="93390-161">Před použitím Azure Active Directory Join k dokončení instalace vytvořte jiný místní účet.</span><span class="sxs-lookup"><span data-stu-id="93390-161">Please create a different local account before using Azure Active Directory Join to complete the setup.</span></span> 

---

<span data-ttu-id="93390-162">**Otázka: kde najdete pokyny pro nastavení automatické registrace zařízení?**</span><span class="sxs-lookup"><span data-stu-id="93390-162">**Q: Where can I find instructions for the setup of automatic device registration?**</span></span>

<span data-ttu-id="93390-163">**Odpověď:** podrobné pokyny najdete v tématu [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="93390-163">**A:** For detailed instructions, see [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

---

<span data-ttu-id="93390-164">**Otázka: kde můžete najít řešení potíží s informace o registraci automatického zařízení?**</span><span class="sxs-lookup"><span data-stu-id="93390-164">**Q: Where can I find troubleshooting information about the automatic device registration?**</span></span>

<span data-ttu-id="93390-165">**Odpověď:** informace o odstraňování potíží, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="93390-165">**A:** For troubleshooting information, see:</span></span>

- [<span data-ttu-id="93390-166">Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD – Windows 10 a Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="93390-166">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)

- [<span data-ttu-id="93390-167">Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD pro klienty nižší úrovně systému Windows</span><span class="sxs-lookup"><span data-stu-id="93390-167">Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

