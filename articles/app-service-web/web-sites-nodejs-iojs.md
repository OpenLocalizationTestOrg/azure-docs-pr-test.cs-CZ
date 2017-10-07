---
title: aaaHow toouse io.js s Azure App Service Web Apps
description: "Zjistěte, jak toouse webové aplikace v Azure App Service přes io.js."
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
ms.openlocfilehash: 5dfdac37546b36bc91ab43d9e0a39c2126b4fa9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-iojs-with-azure-app-service-web-apps"></a>Jak toouse io.js s Azure App Service Web Apps
Hello oblíbených uzlu rozvětvení [io.js] funkce projektu různé rozdíly tooJoyent Node.js, včetně více otevřete modelu zásad správného řízení, rychlejší cyklus vydání a rychlejší přijetí nové a povolenými experimentálními funkcemi JavaScriptu.

Při [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace má mnoho Node.js verze předinstalována, umožňuje také Node.js binárního souboru zadaný uživatelem. Tento článek popisuje dvě metody povolení hello použití io.js v App Service Web Apps: hello použití rozšířeného nasazení skriptu, který automaticky nakonfiguruje Azure toouse hello nejnovější verzi k dispozici io.js, jakož i hello ruční odeslání io.js binární. 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>Pomocí skriptu nasazení
Při nasazení aplikace Node.js spustí App Service Web Apps počet malých příkazy, které tooensure, který hello prostředí je správně nakonfigurován. Pomocí skriptu nasazení, tento proces může být vlastní tooinclude hello stažení a konfiguraci io.js.

Hello [io.js skript nasazení](https://github.com/felixrieseberg/iojs-azure) je dostupná na Githubu. tooenable io.js na vaší webové aplikace, jednoduše zkopírovat **.deployment**, **deploy.cmd** a **IISNode.yml** toohello kořenové složky vaší aplikace a nasazovat aplikace tooWeb.  

první soubor Hello, **.deployment**, dá pokyn webové aplikace toorun **deploy.cmd** po nasazení. Tento skript spustí všechny hello obvyklé kroky pro aplikaci Node.js, ale také stáhne nejnovější verzi io.js hello. Nakonec **IISNode.yml** nakonfiguruje Web Apps toouse jenom hello stáhnout io.js binární místo předem nainstalovaná binární Node.js.

> [!NOTE]
> tooupdate hello používá io.js binární, právě znovu nasaďte aplikaci – hello skript stáhne novou verzi io.js, které je každá aplikace hello jednorázově nasadili.
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>Pomocí ruční instalace
Ruční instalace Hello vlastní io.js verze obsahuje pouze dva kroky. Nejprve stáhnout hello **win-x64** binární přímo z hello [io.js distribuční]. Vyžaduje se dva soubory - **iojs.exe** a **iojs.lib**. Uložit oba soubory tooa složky uvnitř vaší webové aplikaci, například v **bin/iojs**.

Webové aplikace toouse tooconfigure **iojs.exe** místo předem nainstalovaná verze uzlu, vytvořte **IISNode.yml** souboru v kořenovém adresáři aplikace hello a přidejte následující řádek hello.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak zadat toouse io.js s App Service Web Apps, jak pomocí skriptů nasazení a také ruční instalaci. 

> [!NOTE]
> IO.js se velkou vývoj a aktualizován častěji než Node.js. Počet modulů Node.js nemusí pracovat io.js - prosím naleznete [io.js na Githubu] pro řešení potíží.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[io.js]: https://iojs.org
[io.js distribuční]: https://iojs.org/dist/
[io.js na Githubu]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
