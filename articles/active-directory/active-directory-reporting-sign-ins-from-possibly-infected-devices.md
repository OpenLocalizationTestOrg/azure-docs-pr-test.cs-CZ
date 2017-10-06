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
# <a name="sign-ins-from-possibly-infected-devices"></a>Přihlášení z pravděpodobně nakažených zařízení
Tato sestava pokusí tooidentify zařízení vašich uživatelů, které mají být nakažené a jsou teď součástí botnet. Jsme korelovat IP adresy přihlášení uživatele vůči IP adresám, abychom věděli, toobe v kontaktu s botnet servery.

Doporučení: Tato sestava příznaky IP adresy, nikoli zařízením uživatele. Doporučujeme kontaktovat uživatele hello a kontrolovat všechny hello uživatelské zařízení toobe určité. Je také možné, že je nakažená osobní zařízení uživatele, nebo někoho jiného, než hello uživatele, který používal text hello stejnou IP adresu jako hello uživatel má nakažených zařízení.

Další informace o tom, tooaddress napadení malwarem, najdete v části hello [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).

![Přihlášení z pravděpodobně nakažených zařízení](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

