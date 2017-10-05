---
title: "Použití io.js s aplikacemi Azure App Service Web Apps"
description: "Naučte se používat s io.js webové aplikace v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 4800504e1939a46842d15e8c9d4279a4b9cae787
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Použití io.js s aplikacemi Azure App Service Web Apps
Oblíbené rozvětvení uzlu [io.js] funkce různých rozdíly do projektu na Joyent Node.js, včetně více otevřete modelu zásad správného řízení, rychlejší cyklus vydání a rychlejší přijetí nové a povolenými experimentálními funkcemi JavaScriptu.

Při [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace má mnoho Node.js verze předinstalována, umožňuje také Node.js binárního souboru zadaný uživatelem. Tento článek popisuje dvě metody, které umožňuje použití io.js v App Service Web Apps: použití rozšířeného nasazení skriptu, který automaticky nakonfiguruje Azure a použít nejnovější verzi k dispozici io.js, jakož i ruční odesílání io.js binární. 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>Pomocí skriptu nasazení
Při nasazení aplikace Node.js spustí App Service Web Apps počet malých příkazů prostředí je třeba nastavit správně. Pomocí skriptu nasazení, tento proces můžete upravit, aby obsahoval stažení a konfiguraci io.js.

[Io.js skript nasazení](https://github.com/felixrieseberg/iojs-azure) je dostupná na Githubu. Pokud chcete povolit io.js na vaší webové aplikace, jednoduše zkopírovat **.deployment**, **deploy.cmd** a **IISNode.yml** kořenové složky vaší aplikace a nasazení do webové aplikace.  

První soubor **.deployment**, dá pokyn ke spuštění webové aplikace **deploy.cmd** po nasazení. Tento skript spustí všechny obvyklé kroky pro aplikaci Node.js, ale také soubory ke stažení nejnovější verze io.js. Nakonec **IISNode.yml** nakonfiguruje webové aplikace mohly používat jenom stažené io.js binární místo předem nainstalovaná binární Node.js.

> [!NOTE]
> Aktualizovat binární použité io.js, právě znovu nasaďte aplikaci - stáhne skript novou verzi io.js každých jednorázově, které je aplikace nasazená.
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>Pomocí ruční instalace
Ruční instalace verze vlastní io.js obsahuje pouze dva kroky. Nejprve stáhnout **win-x64** binární přímo z [io.js distribuční]. Vyžaduje se dva soubory - **iojs.exe** a **iojs.lib**. Uložit oba soubory do složky uvnitř vaší webové aplikaci, například v **bin/iojs**.

Konfigurace webové aplikace mohly používat **iojs.exe** místo předem nainstalovaná verze uzlu, vytvoření **IISNode.yml** souboru v kořenovém adresáři aplikace a přidejte následující řádek.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak používat k dispozici io.js s App Service Web Apps, jak pomocí skriptů nasazení také ruční instalaci. 

> [!NOTE]
> IO.js se velkou vývoj a aktualizován častěji než Node.js. Počet modulů Node.js nemusí pracovat io.js - prosím naleznete [io.js na Githubu] pro řešení potíží.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[io.js]: https://iojs.org
[io.js distribuční]: https://iojs.org/dist/
[io.js na Githubu]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
