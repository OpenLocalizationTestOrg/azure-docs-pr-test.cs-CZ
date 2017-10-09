---
title: "osvědčené postupy pro vzdálené aplikace RemoteApp aaaAzure | Microsoft Docs"
description: "Osvědčené postupy pro konfiguraci a použití Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b851865b-bec4-4f29-82c0-7b9770c1a520
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f4d09ef30816eaebb74b69f26f3242c69ea27591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a><span data-ttu-id="f7bdc-103">Osvědčené postupy pro konfiguraci a použití Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="f7bdc-103">Best practices for configuring and using Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f7bdc-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f7bdc-105">Čtení hello [oznámení](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-105">Read hello [announcement](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) for details.</span></span>
> 
> 

<span data-ttu-id="f7bdc-106">Hello následující informace vám může pomoct konfigurovat a používat Azure RemoteApp efektivní.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-106">hello following information can help you configure and use Azure RemoteApp productively.</span></span>

## <a name="connectivity"></a><span data-ttu-id="f7bdc-107">Připojení</span><span class="sxs-lookup"><span data-stu-id="f7bdc-107">Connectivity</span></span>
* <span data-ttu-id="f7bdc-108">Vždy používejte nejnovější verzi klienta hello.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-108">Always use hello latest client version.</span></span> <span data-ttu-id="f7bdc-109">Použití starší klienty může způsobit problémy s připojením a dalších činnostech sníženou.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-109">Using older clients might result in connectivity issues and other degraded experiences.</span></span> <span data-ttu-id="f7bdc-110">Povolení automatické aktualizace aplikace pro zařízení se ujistěte se, že tento klient nejnovější hello je vždy nainstalován.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-110">Enabling automatic application updates for your device will ensure that hello latest client is always installed.</span></span>
* <span data-ttu-id="f7bdc-111">Vždy používejte hello nejvíce stabilní a spolehlivá internetové připojení k dispozici tooyou.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-111">Always use hello most stable and reliable internet connection available tooyou.</span></span>  
* <span data-ttu-id="f7bdc-112">Použití podporuje připojení proxy server jenom pro připojení optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-112">Use only supported proxy connections for optimal connectivity performance.</span></span>  <span data-ttu-id="f7bdc-113">Hello SOCKS proxy není podporována.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-113">hello SOCKS proxy is not supported.</span></span>

## <a name="applications"></a><span data-ttu-id="f7bdc-114">Aplikace</span><span class="sxs-lookup"><span data-stu-id="f7bdc-114">Applications</span></span>
* <span data-ttu-id="f7bdc-115">Uložte a zavřete aplikace RemoteApp, když jste hotovi s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-115">Save and close RemoteApp applications when you are done with hello application.</span></span> <span data-ttu-id="f7bdc-116">Není zavření aplikace hello může způsobit ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-116">Not closing hello application might result in data loss.</span></span>
* <span data-ttu-id="f7bdc-117">Ověření vlastních aplikací před jejich používání v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-117">Validate custom applications before using them in Azure RemoteApp.</span></span> <span data-ttu-id="f7bdc-118">To zahrnuje kontrolu jejich fungovat na platformě více relací a nemáte využívat nepotřebné prostředkům, například paměti a procesoru, který může omezují jiného uživatele hello stejné kolekci.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-118">This includes ensuring they work on a multi-session platform and don’t consume unnecessary resources such as memory and CPU that might starve another user in hello same collection.</span></span> <span data-ttu-id="f7bdc-119">Informace, stáhnout a revidovat hello [aplikace kompatibility osvědčené postupy pro služby Vzdálená plocha](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span><span class="sxs-lookup"><span data-stu-id="f7bdc-119">For information, download and review hello [Application Compatibility Best Practices for Remote Desktop Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span></span>

## <a name="configuration-and-management"></a><span data-ttu-id="f7bdc-120">Konfigurace a Správa</span><span class="sxs-lookup"><span data-stu-id="f7bdc-120">Configuration and management</span></span>
* <span data-ttu-id="f7bdc-121">Zachovat vaší Image šablony až toodate, instalace aktualizace softwaru a další důležité opravy podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-121">Keep your template images up toodate, installing software updates and other critical fixes as needed.</span></span> <span data-ttu-id="f7bdc-122">Tím se zajistí, že jako Azure RemoteApp auto měřítka toomeet kapacitu, každá instance je opravit.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-122">This ensures that as Azure RemoteApp auto-scales toomeet your capacity, each instance is patched.</span></span>  
* <span data-ttu-id="f7bdc-123">Zajistěte, aby že vaše nasazení služby Active Directory Federation Services (AD FS) je zabezpečený a spolehlivý.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-123">Make sure your Active Directory Federation Services (AD FS) deployment is secure and reliable.</span></span> <span data-ttu-id="f7bdc-124">V opačném případě může selhat ověřování klienta, brání uživatelům v přístupu k Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-124">Otherwise client authentications might fail, preventing users from accessing Azure RemoteApp.</span></span>
* <span data-ttu-id="f7bdc-125">Nakonfigurujte Image šablony nainstalovaných aplikací, role nebo funkce tak, aby byly bezstavové.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-125">Configure template images with installed applications, roles, or features such that they are stateless.</span></span> <span data-ttu-id="f7bdc-126">Se neměli spoléhat na všechny instance hello virtuální počítače ve službě Vzdálené aplikace RemoteApp je ve stavu trvalé.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-126">They should not rely on any instances of hello virtual machines in a RemoteApp service being in a persistent state.</span></span>
  * <span data-ttu-id="f7bdc-127">Ukládání dat všech uživatelů v uživatelských profilů nebo jiné služby externí toohello umístění úložiště, jako je místní sdílené složky nebo OneDrive.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-127">Store all user data in user profiles or other storage locations external toohello service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="f7bdc-128">Úložiště sdílených dat v modulu snap-in Služba externí toohello umístění úložiště, jako je místní sdílené složky nebo OneDrive.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-128">Store shared data in storage locations external toohello service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="f7bdc-129">Nakonfigurujte všechna nastavení systémového v image šablony hello a nikoli na jednotlivé virtuální počítače ve službě.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-129">Configure any system-wide settings in hello template image rather than on individual virtual machines in a service.</span></span>
  * <span data-ttu-id="f7bdc-130">Zakázat automatické aktualizace softwaru pro publikování aplikace – místo toho použít je ručně image šablony toohello a otestovat je před nasazením z šablony hello.</span><span class="sxs-lookup"><span data-stu-id="f7bdc-130">Disable automatic software updates for published applications - instead apply them manually toohello template image and test them before you deploy  from hello template.</span></span>

