---
title: "Přihlášení z několika zeměpisných oblastí"
description: "Sestavu, která označuje uživatele, jehož dvě přihlášení pocházela z různých oblastí, přičemž čas mezi znemožňuje pro uživatele do mají mezi těmito oblastmi přesunul."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: 79259c8a-2388-4747-b41e-c07434ea9a02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 1de57f64692ade442f9ef8d1e3b587ffee35d7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-from-multiple-geographies"></a><span data-ttu-id="0205b-103">Přihlášení z více geografických poloh</span><span class="sxs-lookup"><span data-stu-id="0205b-103">Sign-ins from multiple geographies</span></span>
<span data-ttu-id="0205b-104">Tato sestava obsahuje úspěšné přihlášení od uživatele, kde dvě přihlášení pocházela z různých oblastech a čas mezi přihlášení znemožňuje uživatele, pro kterou mají urazit mezi těmito oblastmi.</span><span class="sxs-lookup"><span data-stu-id="0205b-104">This report includes successful sign-ins from a user where two sign-ins appeared to originate from different regions and the time between the sign-ins makes it impossible for the user to have traveled between those regions.</span></span> <span data-ttu-id="0205b-105">Možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="0205b-105">Possible causes include:</span></span>

* <span data-ttu-id="0205b-106">Uživatel sdílí svoje heslo s ostatními uživateli</span><span class="sxs-lookup"><span data-stu-id="0205b-106">User is sharing their password with other users</span></span>
* <span data-ttu-id="0205b-107">Uživatel je pomocí vzdálené plochy se spustit webový prohlížeč pro přihlášení</span><span class="sxs-lookup"><span data-stu-id="0205b-107">User is using a remote desktop to launch a web browser for sign-in</span></span>
* <span data-ttu-id="0205b-108">Se hacker se přihlásil k účtu uživatele z jiné země</span><span class="sxs-lookup"><span data-stu-id="0205b-108">A hacker has signed in to the account of a user from a different country</span></span>
* <span data-ttu-id="0205b-109">Uživatel používá sítě VPN nebo proxy server</span><span class="sxs-lookup"><span data-stu-id="0205b-109">User is using a VPN or proxy</span></span>
* <span data-ttu-id="0205b-110">Uživatel je přihlášený z více zařízení ve stejnou dobu, například stolní počítač a mobilní telefon, a IP adresu mobilního telefonu neobvyklé.</span><span class="sxs-lookup"><span data-stu-id="0205b-110">User is signed in from multiple devices at the same time, such as a desktop and a mobile phone, and the IP address of the mobile phone is unusual.</span></span>

<span data-ttu-id="0205b-111">Výsledky z této sestavy se mezi těmito oblastmi zobrazí úspěšné přihlášení události, společně s čas mezi přihlášení, oblasti, kde přihlášení pocházela z a odhadovanou cesta času.</span><span class="sxs-lookup"><span data-stu-id="0205b-111">Results from this report will show you the successful sign-in events, together with the time between the sign-ins, the regions where the sign-ins appeared to originate from, and the estimated travel time between those regions.</span></span> <span data-ttu-id="0205b-112">Cesta čas, zobrazí se pouze o odhad a může lišit od skutečné cesta čas mezi umístění.</span><span class="sxs-lookup"><span data-stu-id="0205b-112">The travel time shown is only an estimate and may be different from the actual travel time between the locations.</span></span>

![Přihlášení z několika zeměpisných oblastí](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

