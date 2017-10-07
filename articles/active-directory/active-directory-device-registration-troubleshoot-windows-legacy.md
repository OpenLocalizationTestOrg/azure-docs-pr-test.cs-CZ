---
title: "aaaTroubleshooting hello automatickou registraci Azure AD domény k počítačům připojeným k pro klienty nižší úrovně Windows | Microsoft Docs"
description: "Řešení potíží s hello automatickou registraci Azure AD domény k počítačům připojeným k pro klienty nižší úrovně systému Windows."
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
ms.openlocfilehash: 84fe666576f13de09d1eaa5692517d45a4dbeebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad-for-windows-down-level-clients"></a><span data-ttu-id="11ab0-103">Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD pro klienty nižší úrovně systému Windows</span><span class="sxs-lookup"><span data-stu-id="11ab0-103">Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients</span></span> 

<span data-ttu-id="11ab0-104">Toto téma je použít jenom toohello následující klienty:</span><span class="sxs-lookup"><span data-stu-id="11ab0-104">This topic is applicable only toohello following clients:</span></span> 

- <span data-ttu-id="11ab0-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="11ab0-105">Windows 7</span></span> 
- <span data-ttu-id="11ab0-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="11ab0-106">Windows 8.1</span></span> 
- <span data-ttu-id="11ab0-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="11ab0-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="11ab0-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="11ab0-108">Windows Server 2012</span></span> 
- <span data-ttu-id="11ab0-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="11ab0-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="11ab0-110">Pro Windows 10 nebo Windows Server 2016, najdete v části [řešení potíží s automatickou registraci domény připojené počítače tooAzure AD – Windows 10 a Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span><span class="sxs-lookup"><span data-stu-id="11ab0-110">For Windows 10 or Windows Server 2016, see [Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span></span>

<span data-ttu-id="11ab0-111">Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-device-registration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="11ab0-111">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md).</span></span>
 
<span data-ttu-id="11ab0-112">Toto téma poskytuje pokyny o tom, jak tooresolve potenciální problémy při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="11ab0-112">This topic provides you with troubleshooting guidance on how tooresolve potential issues.</span></span>  
<span data-ttu-id="11ab0-113">Pro úspěšné výstupy některé toonote věcí:</span><span class="sxs-lookup"><span data-stu-id="11ab0-113">Some things toonote for successful outcomes:</span></span> 

- <span data-ttu-id="11ab0-114">Registrace těchto klientů na Azure AD je na uživatele a zařízení.</span><span class="sxs-lookup"><span data-stu-id="11ab0-114">Registration of these clients on Azure AD is per user/device.</span></span> <span data-ttu-id="11ab0-115">Jako příklad: Pokud jdoe a jharnett přihlásit toothis zařízení, samostatné registrace (DeviceID) se vytvoří pro každou z těchto uživatelů hello uživatelské informace o kartě.</span><span class="sxs-lookup"><span data-stu-id="11ab0-115">As an example: If jdoe and jharnett log in toothis device, a separate registration (DeviceID) is created for each of these users in hello USER info tab.</span></span>  

- <span data-ttu-id="11ab0-116">Registrace těchto klientů předinstalované hello je nakonfigurované tootry při přihlášení a uzamčení a může být zpoždění 5 minut, zda tento stav je aktivován pomocí úlohy plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="11ab0-116">Registration of these clients out of hello box is configured tootry at either logon or lock/unlock and there could be 5-minute delay that this is triggered using a Task Scheduler task.</span></span> 

- <span data-ttu-id="11ab0-117">Znovu nainstalujte hello operačního systému nebo Ruční zrušení registrace a znovu zaregistrovat může vytvořit novou registraci na Azure AD a bude mít za následek více položek kartě údaje uživatele hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="11ab0-117">A re-install of hello operating system or a manual un-register and re-register may create a new registration on Azure AD and will result in multiple entries under hello USER info tab in hello Azure portal.</span></span> 


## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="11ab0-118">Krok 1: Načíst stav registrace hello</span><span class="sxs-lookup"><span data-stu-id="11ab0-118">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="11ab0-119">**Stav registrace tooverify hello:**</span><span class="sxs-lookup"><span data-stu-id="11ab0-119">**tooverify hello registration status:**</span></span>  

1. <span data-ttu-id="11ab0-120">Hello otevřete příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="11ab0-120">Open hello command prompt as an administrator</span></span> 

2. <span data-ttu-id="11ab0-121">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="11ab0-121">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="11ab0-122">Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="11ab0-122">This command displays a dialog box that provides you with more details about hello join status.</span></span>

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-registration-status"></a><span data-ttu-id="11ab0-124">Krok 2: Vyhodnoťte stav registrace hello</span><span class="sxs-lookup"><span data-stu-id="11ab0-124">Step 2: Evaluate hello registration status</span></span> 

<span data-ttu-id="11ab0-125">Pokud hello spojení nebyla úspěšná, dialogové okno hello poskytuje podrobnosti o hello problém, který došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="11ab0-125">If hello join was not successful, hello dialog box provides you with details about hello issue that has occured.</span></span>

<span data-ttu-id="11ab0-126">**Většina běžných potíží s Hello jsou:**</span><span class="sxs-lookup"><span data-stu-id="11ab0-126">**hello most common issues are:**</span></span>

- <span data-ttu-id="11ab0-127">Konfigurace služby AD FS nebo služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="11ab0-127">A misconfigured AD FS or Azure AD</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="11ab0-129">Nejsou přihlášeni jako uživatel domény</span><span class="sxs-lookup"><span data-stu-id="11ab0-129">You are not signed on as a domain user</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="11ab0-131">Bylo dosaženo kvóty</span><span class="sxs-lookup"><span data-stu-id="11ab0-131">A quota has been reached</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="11ab0-133">Služba Hello neodpovídá</span><span class="sxs-lookup"><span data-stu-id="11ab0-133">hello service is not responding</span></span> 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="11ab0-135">Informace o stavu hello můžete také najít v protokolu událostí hello v části **aplikace a služby Log\Microsoft-pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="11ab0-135">You can also find hello status information in hello event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="11ab0-136">**Hello nejběžnější příčiny selhání registrace jsou:**</span><span class="sxs-lookup"><span data-stu-id="11ab0-136">**hello most common causes for a failed registration are:**</span></span> 

- <span data-ttu-id="11ab0-137">Váš počítač není v interní síti organizace hello nebo síť VPN bez připojení tooan místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="11ab0-137">Your computer is not on hello organization’s internal network or a VPN without connection tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="11ab0-138">Jste přihlášeni tooyour počítači pomocí účtu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="11ab0-138">You are logged on tooyour computer with a local computer account.</span></span> 

- <span data-ttu-id="11ab0-139">Problémy s konfigurací služby:</span><span class="sxs-lookup"><span data-stu-id="11ab0-139">Service configuration issues:</span></span> 

  - <span data-ttu-id="11ab0-140">Hello federační server, musí být nakonfigurované toosupport **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="11ab0-140">hello federation server has been configured toosupport **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="11ab0-141">Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do.</span><span class="sxs-lookup"><span data-stu-id="11ab0-141">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to.</span></span>

  - <span data-ttu-id="11ab0-142">Uživatel dosáhl limitu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="11ab0-142">A user has reached hello limit of devices.</span></span> <span data-ttu-id="11ab0-143">Podrobnosti viz Začínáme s Azure Active Directory Device Registration.</span><span class="sxs-lookup"><span data-stu-id="11ab0-143">Please see Get Started with Azure Active Directory Device Registration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11ab0-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11ab0-144">Next steps</span></span>

<span data-ttu-id="11ab0-145">Další informace najdete v tématu hello [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="11ab0-145">For more information, see hello [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
