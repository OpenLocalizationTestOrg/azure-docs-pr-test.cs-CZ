---
title: "nižší úrovně zařízení připojená k hybridní aaaTroubleshooting Azure Active Directory | Microsoft Docs"
description: "Řešení potíží s hybridní Azure Active Directory připojené zařízení nižší úrovně."
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
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: edd56b89579fac6b427732902284ad9c568b87b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a><span data-ttu-id="25103-103">Nižší úrovně zařízení připojená k řešení potíží s hybridní Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="25103-103">Troubleshooting hybrid Azure Active Directory joined down-level devices</span></span> 

<span data-ttu-id="25103-104">Toto téma se vztahuje pouze toohello následující zařízení:</span><span class="sxs-lookup"><span data-stu-id="25103-104">This topic is applicable only toohello following devices:</span></span> 

- <span data-ttu-id="25103-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="25103-105">Windows 7</span></span> 
- <span data-ttu-id="25103-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="25103-106">Windows 8.1</span></span> 
- <span data-ttu-id="25103-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="25103-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="25103-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="25103-108">Windows Server 2012</span></span> 
- <span data-ttu-id="25103-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="25103-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="25103-110">Pro Windows 10 nebo Windows Server 2016, najdete v části [hybridní Poradce při potížích s Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).</span><span class="sxs-lookup"><span data-stu-id="25103-110">For Windows 10 or Windows Server 2016, see [Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices](device-management-troubleshoot-hybrid-join-windows-current.md).</span></span>

<span data-ttu-id="25103-111">Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="25103-111">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello following scenarios:</span></span>

- <span data-ttu-id="25103-112">Podmíněný přístup využívající zařízení</span><span class="sxs-lookup"><span data-stu-id="25103-112">Device-based conditional access</span></span>

- [<span data-ttu-id="25103-113">Enterprise cestovní nastavení</span><span class="sxs-lookup"><span data-stu-id="25103-113">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="25103-114">Windows Hello pro firmy</span><span class="sxs-lookup"><span data-stu-id="25103-114">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md) 





<span data-ttu-id="25103-115">Toto téma poskytuje pokyny o tom, jak tooresolve potenciální problémy při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="25103-115">This topic provides you with troubleshooting guidance on how tooresolve potential issues.</span></span>  

<span data-ttu-id="25103-116">**Co byste měli vědět:**</span><span class="sxs-lookup"><span data-stu-id="25103-116">**What you should know:**</span></span> 

- <span data-ttu-id="25103-117">maximální počet zařízení na uživatele Hello je zaměřená na zařízení.</span><span class="sxs-lookup"><span data-stu-id="25103-117">hello maximum number of devices per user is device-centric.</span></span> <span data-ttu-id="25103-118">Například pokud *jdoe* a *jharnett* přihlášení tooa zařízení samostatných registrací (DeviceID) se vytvoří pro každý z nich v hello **uživatele** informace o kartě.</span><span class="sxs-lookup"><span data-stu-id="25103-118">For example, if *jdoe* and *jharnett* sign-in tooa device, a separate registration (DeviceID) is created for each of them in hello **USER** info tab.</span></span>  

- <span data-ttu-id="25103-119">Hello počáteční registrace / join zařízení je nakonfigurované tooperform k pokusu o přihlášení nebo zámku a odemknout.</span><span class="sxs-lookup"><span data-stu-id="25103-119">hello initial registration / join of devices is configured tooperform an attempt at either logon or lock / unlock.</span></span> <span data-ttu-id="25103-120">Může dojít k 5 minut zpoždění aktivovány úloh služby Plánovač úloh.</span><span class="sxs-lookup"><span data-stu-id="25103-120">There could be 5-minute delay triggered by a task scheduler task.</span></span> 

- <span data-ttu-id="25103-121">Přeinstalujte hello operačního systému nebo ruční unregister a zopakujte registraci na Azure AD může vytvořit nové registrace a má za následek více položek kartě údaje uživatele hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="25103-121">A reinstall of hello operating system or a manual unregister and re-register may create a new registration on Azure AD and results in multiple entries under hello USER info tab in hello Azure portal.</span></span> 


## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="25103-122">Krok 1: Načíst stav registrace hello</span><span class="sxs-lookup"><span data-stu-id="25103-122">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="25103-123">**Stav registrace tooverify hello:**</span><span class="sxs-lookup"><span data-stu-id="25103-123">**tooverify hello registration status:**</span></span>  

1. <span data-ttu-id="25103-124">Hello otevřete příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="25103-124">Open hello command prompt as an administrator</span></span> 

2. <span data-ttu-id="25103-125">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="25103-125">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="25103-126">Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="25103-126">This command displays a dialog box that provides you with more details about hello join status.</span></span>

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-hybrid-azure-ad-join-status"></a><span data-ttu-id="25103-128">Krok 2: Posouzení stavu připojení k Azure AD hybridní hello</span><span class="sxs-lookup"><span data-stu-id="25103-128">Step 2: Evaluate hello hybrid Azure AD join status</span></span> 

<span data-ttu-id="25103-129">Pokud hello hybridní Azure AD join nebyla úspěšná, dialogové okno hello poskytuje podrobnosti o hello problém, který došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="25103-129">If hello hybrid Azure AD join was not successful, hello dialog box provides you with details about hello issue that has occurred.</span></span>

<span data-ttu-id="25103-130">**Většina běžných potíží s Hello jsou:**</span><span class="sxs-lookup"><span data-stu-id="25103-130">**hello most common issues are:**</span></span>

- <span data-ttu-id="25103-131">Konfigurace služby AD FS nebo služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="25103-131">A misconfigured AD FS or Azure AD</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="25103-133">Nejsou přihlášeni jako uživatel domény</span><span class="sxs-lookup"><span data-stu-id="25103-133">You are not signed on as a domain user</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="25103-135">Bylo dosaženo kvóty</span><span class="sxs-lookup"><span data-stu-id="25103-135">A quota has been reached</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="25103-137">Služba Hello neodpovídá</span><span class="sxs-lookup"><span data-stu-id="25103-137">hello service is not responding</span></span> 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="25103-139">Informace o stavu hello můžete také najít v protokolu událostí hello v části **aplikace a služby Log\Microsoft-pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="25103-139">You can also find hello status information in hello event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="25103-140">**Hello nejběžnější příčiny selhání hybridní spojení Azure AD jsou:**</span><span class="sxs-lookup"><span data-stu-id="25103-140">**hello most common causes for a failed hybrid Azure AD join are:**</span></span> 

- <span data-ttu-id="25103-141">Váš počítač není v interní síti organizace hello nebo síť VPN bez připojení tooan místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="25103-141">Your computer is not on hello organization’s internal network or a VPN without connection tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="25103-142">Jste přihlášeni tooyour počítači pomocí účtu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="25103-142">You are logged on tooyour computer with a local computer account.</span></span> 

- <span data-ttu-id="25103-143">Problémy s konfigurací služby:</span><span class="sxs-lookup"><span data-stu-id="25103-143">Service configuration issues:</span></span> 

  - <span data-ttu-id="25103-144">Hello federační server, musí být nakonfigurované toosupport **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="25103-144">hello federation server has been configured toosupport **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="25103-145">Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do.</span><span class="sxs-lookup"><span data-stu-id="25103-145">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to.</span></span>

  - <span data-ttu-id="25103-146">Uživatel dosáhl limitu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="25103-146">A user has reached hello limit of devices.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="25103-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25103-147">Next steps</span></span>

<span data-ttu-id="25103-148">Máte dotazy, viz hello [nejčastější dotazy ke správě zařízení](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="25103-148">For questions, see hello [device management FAQ](device-management-faq.md)</span></span>  
