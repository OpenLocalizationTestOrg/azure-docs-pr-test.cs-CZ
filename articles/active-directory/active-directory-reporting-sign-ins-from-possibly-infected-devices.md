---
title: "aaaSign in z pravděpodobně nakažených zařízení"
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
ms.openlocfilehash: 8d973701d6833f748de443f96cf7ed1d060202e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-ins-from-possibly-infected-devices"></a><span data-ttu-id="31ea2-103">Přihlášení z pravděpodobně nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="31ea2-103">Sign ins from possibly infected devices</span></span>
<span data-ttu-id="31ea2-104">Tato sestava pokusí tooidentify zařízení vašich uživatelů, které mají být nakažené a jsou teď součástí botnet.</span><span class="sxs-lookup"><span data-stu-id="31ea2-104">This report attempts tooidentify your users' devices that that have become infected and are now part of a botnet.</span></span> <span data-ttu-id="31ea2-105">Jsme korelovat IP adresy přihlášení uživatele vůči IP adresám, abychom věděli, toobe v kontaktu s botnet servery.</span><span class="sxs-lookup"><span data-stu-id="31ea2-105">We correlate IP addresses of users' sign-ins against IP addresses that we know toobe in contact with botnet servers.</span></span>

<span data-ttu-id="31ea2-106">Doporučení: Tato sestava příznaky IP adresy, nikoli zařízením uživatele.</span><span class="sxs-lookup"><span data-stu-id="31ea2-106">Recommendation: This report flags IP addresses, not user devices.</span></span> <span data-ttu-id="31ea2-107">Doporučujeme kontaktovat uživatele hello a kontrolovat všechny hello uživatelské zařízení toobe určité.</span><span class="sxs-lookup"><span data-stu-id="31ea2-107">We recommend that you contact hello user and scan all hello user's devices toobe certain.</span></span> <span data-ttu-id="31ea2-108">Je také možné, že je nakažená osobní zařízení uživatele, nebo někoho jiného, než hello uživatele, který používal text hello stejnou IP adresu jako hello uživatel má nakažených zařízení.</span><span class="sxs-lookup"><span data-stu-id="31ea2-108">It is also possible that a user's personal device is infected, or that someone other than hello user, who was using hello same IP address as hello user, has an infected device.</span></span>

<span data-ttu-id="31ea2-109">Další informace o tom, tooaddress napadení malwarem, najdete v části hello [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span><span class="sxs-lookup"><span data-stu-id="31ea2-109">For more information about how tooaddress malware infections, see hello [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span></span>

![Přihlášení z pravděpodobně nakažených zařízení](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

