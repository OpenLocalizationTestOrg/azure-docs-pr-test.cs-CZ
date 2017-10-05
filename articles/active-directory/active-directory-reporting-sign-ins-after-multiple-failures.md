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
# <a name="sign-ins-after-multiple-failures"></a><span data-ttu-id="afa5d-103">Přihlášení po několika neúspěších</span><span class="sxs-lookup"><span data-stu-id="afa5d-103">Sign-ins after multiple failures</span></span>
<span data-ttu-id="afa5d-104">Tato sestava označuje uživatele, kteří mají úspěšném přihlášení po několika po sobě jdoucích neúspěšných přihlášení pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="afa5d-104">This report indicates users who have successfully signed in after multiple consecutive failed sign in attempts.</span></span> <span data-ttu-id="afa5d-105">Možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="afa5d-105">Possible causes include:</span></span>

* <span data-ttu-id="afa5d-106">Uživatel měl zapomněli svoje heslo</span><span class="sxs-lookup"><span data-stu-id="afa5d-106">User had forgotten their password</span></span></li><li><span data-ttu-id="afa5d-107">Uživatel je obětí za účelem uhodnutí hrubou úspěšné hesla vynutit útoku</span><span class="sxs-lookup"><span data-stu-id="afa5d-107">User is the victim of a successful password guessing brute force attack</span></span>

<span data-ttu-id="afa5d-108">Počet po sobě jdoucích neúspěšných pokusů o přihlášení provedené před úspěšné přihlášení a časové razítko přidružené prvním úspěšném přihlášení se zobrazí výsledky z této sestavy.</span><span class="sxs-lookup"><span data-stu-id="afa5d-108">Results from this report will show you the number of consecutive failed sign-in attempts made prior to the successful sign-in and a timestamp associated with the first successful sign-in.</span></span>

<span data-ttu-id="afa5d-109">**Sestavy nastavení**: můžete nakonfigurovat minimální počet po sobě jdoucích neúspěšných přihlášení během pokusů, které musíte udělat předtím, než ji lze zobrazit v sestavě.</span><span class="sxs-lookup"><span data-stu-id="afa5d-109">**Report Settings**: You can configure the minimum number of consecutive failed sign in attempts that must occur before it can be displayed in the report.</span></span> <span data-ttu-id="afa5d-110">Pokud provedete změny tohoto nastavení je důležité si uvědomit, že tyto změny se nepoužije pro jakékoli existující neúspěšných přihlášení in které aktuálně zobrazí v existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="afa5d-110">When you make changes to this setting it is important to note that these changes will not be applied to any existing failed sign ins that currently show up in your existing report.</span></span> <span data-ttu-id="afa5d-111">Ale budou se použijí na všechny budoucí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="afa5d-111">However, they will be applied to all future sign-ins.</span></span> <span data-ttu-id="afa5d-112">Změny do této sestavy můžete provést pouze správce licencovanou.</span><span class="sxs-lookup"><span data-stu-id="afa5d-112">Changes to this report can only be made by licensed admins.</span></span>

![Přihlášení po několika selháních](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

