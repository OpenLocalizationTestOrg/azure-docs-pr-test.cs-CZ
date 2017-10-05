---
title: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro klienty nižší úrovně Windows | Microsoft Docs"
description: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro klienty nižší úrovně systému Windows."
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
ms.openlocfilehash: a7c8ef4c59c53c21258f0c61963d8f994a3946ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a><span data-ttu-id="a1768-103">Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD pro klienty nižší úrovně systému Windows</span><span class="sxs-lookup"><span data-stu-id="a1768-103">Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients</span></span> 

<span data-ttu-id="a1768-104">Toto téma se vztahuje pouze na následující klienty:</span><span class="sxs-lookup"><span data-stu-id="a1768-104">This topic is applicable only to the following clients:</span></span> 

- <span data-ttu-id="a1768-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="a1768-105">Windows 7</span></span> 
- <span data-ttu-id="a1768-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="a1768-106">Windows 8.1</span></span> 
- <span data-ttu-id="a1768-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="a1768-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="a1768-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="a1768-108">Windows Server 2012</span></span> 
- <span data-ttu-id="a1768-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="a1768-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="a1768-110">Pro Windows 10 nebo Windows Server 2016, najdete v části [řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD – Windows 10 a Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span><span class="sxs-lookup"><span data-stu-id="a1768-110">For Windows 10 or Windows Server 2016, see [Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span></span>

<span data-ttu-id="a1768-111">Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-device-registration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a1768-111">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md).</span></span>
 
<span data-ttu-id="a1768-112">Toto téma poskytuje pokyny o tom, jak vyřešit potenciální problémy při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="a1768-112">This topic provides you with troubleshooting guidance on how to resolve potential issues.</span></span>  
<span data-ttu-id="a1768-113">Pro úspěšné výstupy poznamenat některé věci:</span><span class="sxs-lookup"><span data-stu-id="a1768-113">Some things to note for successful outcomes:</span></span> 

- <span data-ttu-id="a1768-114">Registrace těchto klientů na Azure AD je na uživatele a zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1768-114">Registration of these clients on Azure AD is per user/device.</span></span> <span data-ttu-id="a1768-115">Jako příklad: Pokud jdoe a jharnett přihlásit s tímto zařízením, samostatných registrací (DeviceID) se vytvoří pro každou z těchto uživatelů na kartě uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="a1768-115">As an example: If jdoe and jharnett log in to this device, a separate registration (DeviceID) is created for each of these users in the USER info tab.</span></span>  

- <span data-ttu-id="a1768-116">Registrace těchto klientů ihned nakonfigurovaný tak, aby opakujte přihlášení nebo uzamčení a může být zpoždění 5 minut, zda tento stav je aktivován pomocí úlohy plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="a1768-116">Registration of these clients out of the box is configured to try at either logon or lock/unlock and there could be 5-minute delay that this is triggered using a Task Scheduler task.</span></span> 

- <span data-ttu-id="a1768-117">Znovu nainstalujte operační systém nebo Ruční zrušení registrace a znovu zaregistrovat může vytvořit novou registraci na Azure AD a bude mít za následek více položek na kartě informace uživatele na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a1768-117">A re-install of the operating system or a manual un-register and re-register may create a new registration on Azure AD and will result in multiple entries under the USER info tab in the Azure portal.</span></span> 


## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="a1768-118">Krok 1: Načíst stav registrace</span><span class="sxs-lookup"><span data-stu-id="a1768-118">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="a1768-119">**Chcete-li ověřit stav registrace:**</span><span class="sxs-lookup"><span data-stu-id="a1768-119">**To verify the registration status:**</span></span>  

1. <span data-ttu-id="a1768-120">Otevřete příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="a1768-120">Open the command prompt as an administrator</span></span> 

2. <span data-ttu-id="a1768-121">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="a1768-121">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="a1768-122">Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="a1768-122">This command displays a dialog box that provides you with more details about the join status.</span></span>

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a><span data-ttu-id="a1768-124">Krok 2: Vyhodnoťte stav registrace</span><span class="sxs-lookup"><span data-stu-id="a1768-124">Step 2: Evaluate the registration status</span></span> 

<span data-ttu-id="a1768-125">Pokud spojovací nebyla úspěšná, dialogové okno vám poskytuje podrobnosti o problému, který došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="a1768-125">If the join was not successful, the dialog box provides you with details about the issue that has occured.</span></span>

<span data-ttu-id="a1768-126">**Nejběžnějším problémům, jsou:**</span><span class="sxs-lookup"><span data-stu-id="a1768-126">**The most common issues are:**</span></span>

- <span data-ttu-id="a1768-127">Konfigurace služby AD FS nebo služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1768-127">A misconfigured AD FS or Azure AD</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="a1768-129">Nejsou přihlášeni jako uživatel domény</span><span class="sxs-lookup"><span data-stu-id="a1768-129">You are not signed on as a domain user</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="a1768-131">Bylo dosaženo kvóty</span><span class="sxs-lookup"><span data-stu-id="a1768-131">A quota has been reached</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="a1768-133">Služba nereaguje.</span><span class="sxs-lookup"><span data-stu-id="a1768-133">The service is not responding</span></span> 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="a1768-135">Informace o stavu můžete také najít v protokolu událostí v části **aplikace a služby Log\Microsoft-pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="a1768-135">You can also find the status information in the event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="a1768-136">**Nejběžnější příčiny selhání registrace jsou:**</span><span class="sxs-lookup"><span data-stu-id="a1768-136">**The most common causes for a failed registration are:**</span></span> 

- <span data-ttu-id="a1768-137">Váš počítač není v interní síti organizace nebo síť VPN bez připojení k místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="a1768-137">Your computer is not on the organization’s internal network or a VPN without connection to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="a1768-138">Jste přihlášeni k počítači pomocí účtu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="a1768-138">You are logged on to your computer with a local computer account.</span></span> 

- <span data-ttu-id="a1768-139">Problémy s konfigurací služby:</span><span class="sxs-lookup"><span data-stu-id="a1768-139">Service configuration issues:</span></span> 

  - <span data-ttu-id="a1768-140">Federační server nakonfigurovaný pro podporu **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="a1768-140">The federation server has been configured to support **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="a1768-141">Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, pokud je počítač členem.</span><span class="sxs-lookup"><span data-stu-id="a1768-141">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to.</span></span>

  - <span data-ttu-id="a1768-142">Uživatel byl dosažen maximální počet zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1768-142">A user has reached the limit of devices.</span></span> <span data-ttu-id="a1768-143">Podrobnosti viz Začínáme s Azure Active Directory Device Registration.</span><span class="sxs-lookup"><span data-stu-id="a1768-143">Please see Get Started with Azure Active Directory Device Registration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1768-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1768-144">Next steps</span></span>

<span data-ttu-id="a1768-145">Další informace najdete v tématu [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="a1768-145">For more information, see the [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
