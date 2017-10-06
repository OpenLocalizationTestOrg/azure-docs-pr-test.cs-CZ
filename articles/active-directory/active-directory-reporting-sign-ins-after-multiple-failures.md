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
# <a name="sign-ins-after-multiple-failures"></a><span data-ttu-id="a55e5-103">Přihlášení po několika neúspěších</span><span class="sxs-lookup"><span data-stu-id="a55e5-103">Sign-ins after multiple failures</span></span>
<span data-ttu-id="a55e5-104">Tato sestava označuje uživatele, kteří mají úspěšném přihlášení po několika po sobě jdoucích neúspěšných přihlášení pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a55e5-104">This report indicates users who have successfully signed in after multiple consecutive failed sign in attempts.</span></span> <span data-ttu-id="a55e5-105">Možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="a55e5-105">Possible causes include:</span></span>

* <span data-ttu-id="a55e5-106">Uživatel měl zapomněli svoje heslo</span><span class="sxs-lookup"><span data-stu-id="a55e5-106">User had forgotten their password</span></span></li><li><span data-ttu-id="a55e5-107">Uživatel je obětí hello za účelem uhodnutí útoku hrubou silou úspěšné hesla</span><span class="sxs-lookup"><span data-stu-id="a55e5-107">User is hello victim of a successful password guessing brute force attack</span></span>

<span data-ttu-id="a55e5-108">Výsledky z této sestavy se zobrazí hello počet po sobě jdoucích neúspěšných pokusů o přihlášení provedených předchozí toohello úspěšného přihlášení a časové razítko přidružené hello prvního úspěšného přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a55e5-108">Results from this report will show you hello number of consecutive failed sign-in attempts made prior toohello successful sign-in and a timestamp associated with hello first successful sign-in.</span></span>

<span data-ttu-id="a55e5-109">**Sestavy nastavení**: můžete nakonfigurovat hello minimální počet po sobě jdoucích neúspěšných přihlášení během pokusů, které musíte udělat předtím, než ji bylo možné zobrazit v sestavě hello.</span><span class="sxs-lookup"><span data-stu-id="a55e5-109">**Report Settings**: You can configure hello minimum number of consecutive failed sign in attempts that must occur before it can be displayed in hello report.</span></span> <span data-ttu-id="a55e5-110">Pokud provedete změny, které toothis jeho nastavení je důležité toonote, tyto změny nebudou použité tooany existující neúspěšných přihlášení, momentálně se objeví v existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="a55e5-110">When you make changes toothis setting it is important toonote that these changes will not be applied tooany existing failed sign ins that currently show up in your existing report.</span></span> <span data-ttu-id="a55e5-111">Však budou použité tooall budoucí přihlášení. Sestava změn toothis může provést pouze správce licencovanou.</span><span class="sxs-lookup"><span data-stu-id="a55e5-111">However, they will be applied tooall future sign-ins. Changes toothis report can only be made by licensed admins.</span></span>

![Přihlášení po několika selháních](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

