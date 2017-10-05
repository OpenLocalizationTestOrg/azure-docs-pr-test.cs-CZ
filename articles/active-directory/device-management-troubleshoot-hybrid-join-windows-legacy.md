---
title: "Řešení potíží s hybridní Azure Active Directory nižší úrovně zařízení připojená k | Microsoft Docs"
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
ms.openlocfilehash: 715fca79e488ae3759926181c244a42026f4a554
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a><span data-ttu-id="051a0-103">Nižší úrovně zařízení připojená k řešení potíží s hybridní Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="051a0-103">Troubleshooting hybrid Azure Active Directory joined down-level devices</span></span> 

<span data-ttu-id="051a0-104">Toto téma se vztahuje pouze na následujících zařízeních:</span><span class="sxs-lookup"><span data-stu-id="051a0-104">This topic is applicable only to the following devices:</span></span> 

- <span data-ttu-id="051a0-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="051a0-105">Windows 7</span></span> 
- <span data-ttu-id="051a0-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="051a0-106">Windows 8.1</span></span> 
- <span data-ttu-id="051a0-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="051a0-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="051a0-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="051a0-108">Windows Server 2012</span></span> 
- <span data-ttu-id="051a0-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="051a0-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="051a0-110">Pro Windows 10 nebo Windows Server 2016, najdete v části [hybridní Poradce při potížích s Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).</span><span class="sxs-lookup"><span data-stu-id="051a0-110">For Windows 10 or Windows Server 2016, see [Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices](device-management-troubleshoot-hybrid-join-windows-current.md).</span></span>

<span data-ttu-id="051a0-111">Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) k podporují následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="051a0-111">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) to support the following scenarios:</span></span>

- <span data-ttu-id="051a0-112">Podmíněný přístup využívající zařízení</span><span class="sxs-lookup"><span data-stu-id="051a0-112">Device-based conditional access</span></span>

- [<span data-ttu-id="051a0-113">Enterprise cestovní nastavení</span><span class="sxs-lookup"><span data-stu-id="051a0-113">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="051a0-114">Windows Hello pro firmy</span><span class="sxs-lookup"><span data-stu-id="051a0-114">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md) 





<span data-ttu-id="051a0-115">Toto téma poskytuje pokyny o tom, jak vyřešit potenciální problémy při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="051a0-115">This topic provides you with troubleshooting guidance on how to resolve potential issues.</span></span>  

<span data-ttu-id="051a0-116">**Co byste měli vědět:**</span><span class="sxs-lookup"><span data-stu-id="051a0-116">**What you should know:**</span></span> 

- <span data-ttu-id="051a0-117">Maximální počet zařízení na uživatele je zaměřená na zařízení.</span><span class="sxs-lookup"><span data-stu-id="051a0-117">The maximum number of devices per user is device-centric.</span></span> <span data-ttu-id="051a0-118">Například pokud *jdoe* a *jharnett* Přihlaste se do zařízení, samostatné registrace (DeviceID) se vytvoří pro každou z nich **uživatele** informace o kartě.</span><span class="sxs-lookup"><span data-stu-id="051a0-118">For example, if *jdoe* and *jharnett* sign-in to a device, a separate registration (DeviceID) is created for each of them in the **USER** info tab.</span></span>  

- <span data-ttu-id="051a0-119">Počáteční registrace / připojení k zařízení, je nakonfigurována se provést k pokusu o přihlášení nebo uzamčení nebo odemčení.</span><span class="sxs-lookup"><span data-stu-id="051a0-119">The initial registration / join of devices is configured to perform an attempt at either logon or lock / unlock.</span></span> <span data-ttu-id="051a0-120">Může dojít k 5 minut zpoždění aktivovány úloh služby Plánovač úloh.</span><span class="sxs-lookup"><span data-stu-id="051a0-120">There could be 5-minute delay triggered by a task scheduler task.</span></span> 

- <span data-ttu-id="051a0-121">Přeinstalujte operační systém nebo ruční unregister a zopakujte registraci na Azure AD může vytvořit nové registrace a má za následek více položek na kartě informace uživatele na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="051a0-121">A reinstall of the operating system or a manual unregister and re-register may create a new registration on Azure AD and results in multiple entries under the USER info tab in the Azure portal.</span></span> 


## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="051a0-122">Krok 1: Načíst stav registrace</span><span class="sxs-lookup"><span data-stu-id="051a0-122">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="051a0-123">**Chcete-li ověřit stav registrace:**</span><span class="sxs-lookup"><span data-stu-id="051a0-123">**To verify the registration status:**</span></span>  

1. <span data-ttu-id="051a0-124">Otevřete příkazový řádek jako správce</span><span class="sxs-lookup"><span data-stu-id="051a0-124">Open the command prompt as an administrator</span></span> 

2. <span data-ttu-id="051a0-125">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="051a0-125">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="051a0-126">Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="051a0-126">This command displays a dialog box that provides you with more details about the join status.</span></span>

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a><span data-ttu-id="051a0-128">Krok 2: Vyhodnoťte hybridní stav připojení k Azure AD</span><span class="sxs-lookup"><span data-stu-id="051a0-128">Step 2: Evaluate the hybrid Azure AD join status</span></span> 

<span data-ttu-id="051a0-129">Pokud připojení k Azure AD hybridní nebyla úspěšná, dialogové okno vám poskytuje podrobnosti o problému, který došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="051a0-129">If the hybrid Azure AD join was not successful, the dialog box provides you with details about the issue that has occurred.</span></span>

<span data-ttu-id="051a0-130">**Nejběžnějším problémům, jsou:**</span><span class="sxs-lookup"><span data-stu-id="051a0-130">**The most common issues are:**</span></span>

- <span data-ttu-id="051a0-131">Konfigurace služby AD FS nebo služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="051a0-131">A misconfigured AD FS or Azure AD</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="051a0-133">Nejsou přihlášeni jako uživatel domény</span><span class="sxs-lookup"><span data-stu-id="051a0-133">You are not signed on as a domain user</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="051a0-135">Bylo dosaženo kvóty</span><span class="sxs-lookup"><span data-stu-id="051a0-135">A quota has been reached</span></span>

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="051a0-137">Služba nereaguje.</span><span class="sxs-lookup"><span data-stu-id="051a0-137">The service is not responding</span></span> 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="051a0-139">Informace o stavu můžete také najít v protokolu událostí v části **aplikace a služby Log\Microsoft-pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="051a0-139">You can also find the status information in the event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="051a0-140">**Nejběžnější příčiny selhání hybridní Azure AD join jsou:**</span><span class="sxs-lookup"><span data-stu-id="051a0-140">**The most common causes for a failed hybrid Azure AD join are:**</span></span> 

- <span data-ttu-id="051a0-141">Váš počítač není v interní síti organizace nebo síť VPN bez připojení k místní řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="051a0-141">Your computer is not on the organization’s internal network or a VPN without connection to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="051a0-142">Jste přihlášeni k počítači pomocí účtu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="051a0-142">You are logged on to your computer with a local computer account.</span></span> 

- <span data-ttu-id="051a0-143">Problémy s konfigurací služby:</span><span class="sxs-lookup"><span data-stu-id="051a0-143">Service configuration issues:</span></span> 

  - <span data-ttu-id="051a0-144">Federační server nakonfigurovaný pro podporu **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="051a0-144">The federation server has been configured to support **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="051a0-145">Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, pokud je počítač členem.</span><span class="sxs-lookup"><span data-stu-id="051a0-145">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to.</span></span>

  - <span data-ttu-id="051a0-146">Uživatel byl dosažen maximální počet zařízení.</span><span class="sxs-lookup"><span data-stu-id="051a0-146">A user has reached the limit of devices.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="051a0-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="051a0-147">Next steps</span></span>

<span data-ttu-id="051a0-148">Otázky, najdete v článku [nejčastější dotazy ke správě zařízení](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="051a0-148">For questions, see the [device management FAQ](device-management-faq.md)</span></span>  
