---
title: "aaaSign in po několika selháních"
description: "Sestava, která označuje, uživatelé, kteří mají úspěšném přihlášení po několika po sobě jdoucích neúspěšných přihlášení pokusů o přihlášení."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: femila
editor: 
ms.assetid: e4ec1a39-9c20-418f-8a75-6497d0117176
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 48d137dc3abf65287cb3b9ba8a6ff10340f6741f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-ins-after-multiple-failures"></a>Přihlášení po několika neúspěších
Tato sestava označuje uživatele, kteří mají úspěšném přihlášení po několika po sobě jdoucích neúspěšných přihlášení pokusů o přihlášení. Možné příčiny:

* Uživatel měl zapomněli svoje heslo</li><li>Uživatel je obětí hello za účelem uhodnutí útoku hrubou silou úspěšné hesla

Výsledky z této sestavy se zobrazí hello počet po sobě jdoucích neúspěšných pokusů o přihlášení provedených předchozí toohello úspěšného přihlášení a časové razítko přidružené hello prvního úspěšného přihlášení.

**Sestavy nastavení**: můžete nakonfigurovat hello minimální počet po sobě jdoucích neúspěšných přihlášení během pokusů, které musíte udělat předtím, než ji bylo možné zobrazit v sestavě hello. Pokud provedete změny, které toothis jeho nastavení je důležité toonote, tyto změny nebudou použité tooany existující neúspěšných přihlášení, momentálně se objeví v existující sestavy. Však budou použité tooall budoucí přihlášení. Sestava změn toothis může provést pouze správce licencovanou.

![Přihlášení po několika selháních](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

