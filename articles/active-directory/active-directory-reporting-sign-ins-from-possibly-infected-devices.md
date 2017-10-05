---
title: "Přihlášení z pravděpodobně nakažených zařízení"
description: "Sestava, která obsahuje znak v pokusů, které byly provedeny ze zařízení, na které může používat některé malwaru (škodlivého softwaru)."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: d0361662-d970-4a66-8eb3-72e9f8824dcf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 3809e20937d8d9829675e20f893101cb849dcea2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-from-possibly-infected-devices"></a><span data-ttu-id="31fe8-103">Přihlášení z pravděpodobně nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="31fe8-103">Sign ins from possibly infected devices</span></span>
<span data-ttu-id="31fe8-104">Tato sestava se pokusí identifikovat zařízení vašich uživatelů, které mají být nakažené a jsou teď součástí botnet.</span><span class="sxs-lookup"><span data-stu-id="31fe8-104">This report attempts to identify your users' devices that that have become infected and are now part of a botnet.</span></span> <span data-ttu-id="31fe8-105">Jsme korelovat IP adresy přihlášení uživatele vůči IP adresy, které jsme si vědomi být v kontaktu s botnet servery.</span><span class="sxs-lookup"><span data-stu-id="31fe8-105">We correlate IP addresses of users' sign-ins against IP addresses that we know to be in contact with botnet servers.</span></span>

<span data-ttu-id="31fe8-106">Doporučení: Tato sestava příznaky IP adresy, nikoli zařízením uživatele.</span><span class="sxs-lookup"><span data-stu-id="31fe8-106">Recommendation: This report flags IP addresses, not user devices.</span></span> <span data-ttu-id="31fe8-107">Doporučujeme, abyste Kontaktujte uživatele a kontrolovat všechny uživatele zařízení k určité zařízení.</span><span class="sxs-lookup"><span data-stu-id="31fe8-107">We recommend that you contact the user and scan all the user's devices to be certain.</span></span> <span data-ttu-id="31fe8-108">Je také možné, že je nakažená osobní zařízení uživatele nebo že má někdo než uživateli, který používal stejnou IP adresu jako uživatel, nakažených zařízení.</span><span class="sxs-lookup"><span data-stu-id="31fe8-108">It is also possible that a user's personal device is infected, or that someone other than the user, who was using the same IP address as the user, has an infected device.</span></span>

<span data-ttu-id="31fe8-109">Další informace o tom, jak adresu napadení malwarem, najdete v článku [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span><span class="sxs-lookup"><span data-stu-id="31fe8-109">For more information about how to address malware infections, see the [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span></span>

![Přihlášení z pravděpodobně nakažených zařízení](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

