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
# <a name="sign-ins-from-multiple-geographies"></a>Přihlášení z více geografických poloh
Tato sestava obsahuje úspěšné přihlášení od uživatele, kde dvě přihlášení pocházela z různých oblastech a čas mezi přihlášení znemožňuje uživatele, pro kterou mají urazit mezi těmito oblastmi. Možné příčiny:

* Uživatel sdílí svoje heslo s ostatními uživateli
* Uživatel je pomocí vzdálené plochy se spustit webový prohlížeč pro přihlášení
* Se hacker se přihlásil k účtu uživatele z jiné země
* Uživatel používá sítě VPN nebo proxy server
* Uživatel je přihlášený z více zařízení ve stejnou dobu, například stolní počítač a mobilní telefon, a IP adresu mobilního telefonu neobvyklé.

Výsledky z této sestavy se mezi těmito oblastmi zobrazí úspěšné přihlášení události, společně s čas mezi přihlášení, oblasti, kde přihlášení pocházela z a odhadovanou cesta času. Cesta čas, zobrazí se pouze o odhad a může lišit od skutečné cesta čas mezi umístění.

![Přihlášení z několika zeměpisných oblastí](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

