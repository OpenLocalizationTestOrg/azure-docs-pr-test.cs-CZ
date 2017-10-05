---
title: "Přihlášení po několika selháních"
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
ms.openlocfilehash: e55e0145adbdb1f41a8b8753d5555f20e96bf161
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-after-multiple-failures"></a>Přihlášení po několika neúspěších
Tato sestava označuje uživatele, kteří mají úspěšném přihlášení po několika po sobě jdoucích neúspěšných přihlášení pokusů o přihlášení. Možné příčiny:

* Uživatel měl zapomněli svoje heslo</li><li>Uživatel je obětí za účelem uhodnutí hrubou úspěšné hesla vynutit útoku

Počet po sobě jdoucích neúspěšných pokusů o přihlášení provedené před úspěšné přihlášení a časové razítko přidružené prvním úspěšném přihlášení se zobrazí výsledky z této sestavy.

**Sestavy nastavení**: můžete nakonfigurovat minimální počet po sobě jdoucích neúspěšných přihlášení během pokusů, které musíte udělat předtím, než ji lze zobrazit v sestavě. Pokud provedete změny tohoto nastavení je důležité si uvědomit, že tyto změny se nepoužije pro jakékoli existující neúspěšných přihlášení in které aktuálně zobrazí v existující sestavy. Ale budou se použijí na všechny budoucí přihlášení. Změny do této sestavy můžete provést pouze správce licencovanou.

![Přihlášení po několika selháních](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

