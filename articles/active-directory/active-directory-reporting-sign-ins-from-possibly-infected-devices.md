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
# <a name="sign-ins-from-possibly-infected-devices"></a>Přihlášení z pravděpodobně nakažených zařízení
Tato sestava se pokusí identifikovat zařízení vašich uživatelů, které mají být nakažené a jsou teď součástí botnet. Jsme korelovat IP adresy přihlášení uživatele vůči IP adresy, které jsme si vědomi být v kontaktu s botnet servery.

Doporučení: Tato sestava příznaky IP adresy, nikoli zařízením uživatele. Doporučujeme, abyste Kontaktujte uživatele a kontrolovat všechny uživatele zařízení k určité zařízení. Je také možné, že je nakažená osobní zařízení uživatele nebo že má někdo než uživateli, který používal stejnou IP adresu jako uživatel, nakažených zařízení.

Další informace o tom, jak adresu napadení malwarem, najdete v článku [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).

![Přihlášení z pravděpodobně nakažených zařízení](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

