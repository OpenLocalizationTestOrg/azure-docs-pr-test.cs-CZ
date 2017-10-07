---
title: "aaaApp požadavky pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích hello pro aplikace, které chcete toouse v Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 4427eef6-288a-49e1-97eb-fee67d99f26a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 3fa2bcdaab457a6fbee8ac52a81d1c4154bbdce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-requirements"></a><span data-ttu-id="4e308-103">Požadavky aplikace</span><span class="sxs-lookup"><span data-stu-id="4e308-103">App requirements</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4e308-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="4e308-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4e308-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4e308-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4e308-106">Azure RemoteApp podporuje streamování 32bitové nebo 64bitové aplikace pro systém Windows z bitové kopie systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="4e308-106">Azure RemoteApp supports streaming 32-bit or 64-bit Windows-based applications from a Windows Server 2012 R2 image.</span></span> <span data-ttu-id="4e308-107">Většina existující 32bitové nebo 64bitové aplikace pro systém Windows spusťte "tak, jak je v Azure Remoteappu (služby Vzdálená plocha nebo dříve známé jako Terminálové služby) prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e308-107">Most existing 32-bit or 64-bit Windows-based applications run "as is" in Azure RemoteApp (Remote Desktop Services or formerly known as Terminal Services) environment.</span></span> <span data-ttu-id="4e308-108">Nicméně je rozdíl mezi spuštění a dobře – některé aplikace fungovat správně a provádět i, zatímco jiné nikoli.</span><span class="sxs-lookup"><span data-stu-id="4e308-108">However, there is a difference between running and running well - some applications function correctly and perform well, while others do not.</span></span> <span data-ttu-id="4e308-109">Následující informace Hello obsahuje pokyny pro vývoj aplikací v prostředí vzdálené plochy a testování kompatibility tooensure.</span><span class="sxs-lookup"><span data-stu-id="4e308-109">hello following information provides guidance for developing applications in a Remote Desktop Services environment and testing tooensure compatibility.</span></span>

<span data-ttu-id="4e308-110">Tip: Pracujeme na vytváření příklady pracovní aplikace za vás.</span><span class="sxs-lookup"><span data-stu-id="4e308-110">Tip: We're working on creating some working examples of apps for you.</span></span> <span data-ttu-id="4e308-111">Zobrazí se nová témata, které popisují pomocí aplikace Microsoft Access, QuickBooks a App-V v Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="4e308-111">You'll see new topics that discuss using Microsoft Access, QuickBooks, and App-V in RemoteApp.</span></span>

## <a name="requirements"></a><span data-ttu-id="4e308-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e308-112">Requirements</span></span>
<span data-ttu-id="4e308-113">Tyto tři požadavky, pokud dodrženy, pomoci aplikace spustit i v RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="4e308-113">These three requirements, if followed, help your application run well in RemoteApp:</span></span>

1. <span data-ttu-id="4e308-114">Aplikace, které splňují všechny [požadavky na certifikaci aplikací klasické pracovní plochy Windows](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) a dodržovat příliš[služby Vzdálená plocha programování pokyny](https://msdn.microsoft.com/library/aa383490.aspx) bude mít úplný kompatibilitu s vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="4e308-114">Applications that meet all [Certification requirements for Windows desktop apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) and adhere too[Remote Desktop Services programming guidelines](https://msdn.microsoft.com/library/aa383490.aspx) will have complete compatibility with RemoteApp.</span></span>
2. <span data-ttu-id="4e308-115">Aplikace by nikdy ukládat data místně v bitové kopii hello nebo vzdálené aplikace RemoteApp instancí, které mohou být ztracena.</span><span class="sxs-lookup"><span data-stu-id="4e308-115">Applications should never store data locally on hello image or RemoteApp instances that can be lost.</span></span>  <span data-ttu-id="4e308-116">Po vytvoření kolekce vzdálené aplikace RemoteApp hello instance jsou klonovat a jsou bezstavové a smí obsahovat pouze aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e308-116">After you create a RemoteApp collection, hello instances are cloned and are stateless and should only contain applications.</span></span> <span data-ttu-id="4e308-117">Uložit data do externího zdroje nebo v rámci profilu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4e308-117">Store data in an external source or within hello user's profile.</span></span>
3. <span data-ttu-id="4e308-118">Vlastní Image nikdy by mělo obsahovat data, která mohou být ztraceny.</span><span class="sxs-lookup"><span data-stu-id="4e308-118">Custom images should never contain data that can be lost.</span></span>  

## <a name="testing-your-apps"></a><span data-ttu-id="4e308-119">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="4e308-119">Testing your apps</span></span>
<span data-ttu-id="4e308-120">Použijte tyto kroky tootesting aplikace:</span><span class="sxs-lookup"><span data-stu-id="4e308-120">Use these steps tootesting applications:</span></span>

1. <span data-ttu-id="4e308-121">Instalace systému Windows Server 2012 R2 a aplikace</span><span class="sxs-lookup"><span data-stu-id="4e308-121">Install Windows Server 2012 R2 and your application</span></span>
2. <span data-ttu-id="4e308-122">Povolení Vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="4e308-122">Enable Remote Desktop</span></span>
3. <span data-ttu-id="4e308-123">Vytvořte dva uživatelské účty, uživatele a b, přidání skupiny zabezpečení toohello vzdálené plochy oba uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="4e308-123">Create two user accounts, UserA and UserB, adding both user accounts toohello Remote Desktop security group.</span></span>
4. <span data-ttu-id="4e308-124">Zkontrolujte kompatibilitu s více relace vytvořením dvou souběžných RDS relací toohello počítač při spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4e308-124">Check multi-session compatibility by establishing two simultaneous RDS sessions toohello PC while launching hello application.</span></span>
5. <span data-ttu-id="4e308-125">Ověření chování aplikace</span><span class="sxs-lookup"><span data-stu-id="4e308-125">Validate app behavior</span></span>

## <a name="application-development-guidelines"></a><span data-ttu-id="4e308-126">Pokyny pro vývoj aplikací</span><span class="sxs-lookup"><span data-stu-id="4e308-126">Application development guidelines</span></span>
<span data-ttu-id="4e308-127">Použijte následující pokyny pro vývoj aplikací pro vzdálenou aplikaci RemoteApp hello.</span><span class="sxs-lookup"><span data-stu-id="4e308-127">Use hello following guidelines for developing applications for RemoteApp.</span></span>

### <a name="multiple-users"></a><span data-ttu-id="4e308-128">Více uživatelů</span><span class="sxs-lookup"><span data-stu-id="4e308-128">Multiple users</span></span>
* <span data-ttu-id="4e308-129">Instalace [aplikace pro jednoho uživatele ](https://msdn.microsoft.com/library/aa380661.aspx)může způsobit problémy v prostředí s více uživateli.</span><span class="sxs-lookup"><span data-stu-id="4e308-129">Installing an [application for a single user ](https://msdn.microsoft.com/library/aa380661.aspx)can create problems in a multiuser environment.</span></span>
* <span data-ttu-id="4e308-130">Aplikace by měla [ukládat informace o uživateli](https://msdn.microsoft.com/library/aa383452.aspx) v umístění specifický pro uživatele, samostatně z globální informací, platí tooall uživatele.</span><span class="sxs-lookup"><span data-stu-id="4e308-130">Applications should [store user-specific information](https://msdn.microsoft.com/library/aa383452.aspx) in user-specific locations, separately from global information that applies tooall users.</span></span>
* <span data-ttu-id="4e308-131">Vzdálené aplikace RemoteApp používá více [obory názvů pro objekty jádra](https://msdn.microsoft.com/library/aa382954.aspx); globální obor názvů se používá hlavně službami v aplikacích klienta nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="4e308-131">RemoteApp uses multiple [namespaces for kernel objects](https://msdn.microsoft.com/library/aa382954.aspx); a global namespace is used primarily by services in client/server applications.</span></span>
* <span data-ttu-id="4e308-132">Není bezpečné tooassume, který hello názvu počítače nebo hello [IP adresu](https://msdn.microsoft.com/library/aa382942.aspx) přiřazené toohello počítači jsou přidružené jenom jednoho konkrétního uživatele, protože současně tooa hostitele relace vzdálené plochy (RD relace je přihlášeno více uživatelů Server hostitele).</span><span class="sxs-lookup"><span data-stu-id="4e308-132">It is not safe tooassume that hello computer name or hello [IP address](https://msdn.microsoft.com/library/aa382942.aspx) assigned toohello computer are associated with a single user because multiple users can be logged on simultaneously tooa Remote Desktop Session Host (RD Session Host) server.</span></span>

### <a name="performance"></a><span data-ttu-id="4e308-133">Výkon</span><span class="sxs-lookup"><span data-stu-id="4e308-133">Performance</span></span>
* <span data-ttu-id="4e308-134">Zakázat [grafické efekty](https://msdn.microsoft.com/library/aa380822.aspx) předtím, než přidáte tooRemoteApp vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e308-134">Disable [graphic effects](https://msdn.microsoft.com/library/aa380822.aspx) before you add your app tooRemoteApp.</span></span>
* <span data-ttu-id="4e308-135">toomaximize procesoru dostupnosti pro všechny uživatele, buď zakažte [úlohy na pozadí ](https://msdn.microsoft.com/library/aa380665.aspx) nebo vytvořit efektivní pozadí úlohy, které nejsou náročná.</span><span class="sxs-lookup"><span data-stu-id="4e308-135">toomaximize CPU availability for all users, either disable [background tasks ](https://msdn.microsoft.com/library/aa380665.aspx) or create efficient background tasks that are not resource-intensive.</span></span>
* <span data-ttu-id="4e308-136">Má ladit a vyvážení aplikace [vláken využití](https://msdn.microsoft.com/library/aa383520.aspx) pro prostředí s více uživateli, s více procesory.</span><span class="sxs-lookup"><span data-stu-id="4e308-136">You should tune and balance application [thread usage](https://msdn.microsoft.com/library/aa383520.aspx) for a multiuser, multiprocessor environment.</span></span>
* <span data-ttu-id="4e308-137">toooptimize výkon, je vhodné pro aplikace příliš[zjistit](https://msdn.microsoft.com/library/aa380798.aspx) tom, zda jsou spuštěny v relaci klienta.</span><span class="sxs-lookup"><span data-stu-id="4e308-137">toooptimize performance, it is good practice for applications too[detect](https://msdn.microsoft.com/library/aa380798.aspx) whether they are running in a client session.</span></span>

