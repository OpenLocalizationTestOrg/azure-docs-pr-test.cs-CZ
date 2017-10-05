---
title: "Postup ladění webové aplikace Node.js ve službě Azure App Service"
description: "Postup ladění webové aplikace Node.js ve službě Azure App Service."
tags: azure-portal
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: a48f906c-1a3e-43bc-ae84-7d2dde175b15
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 5e302a4c58a171d40e43a22c34c724e868019ec8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Postup ladění webové aplikace Node.js ve službě Azure App Service
Azure poskytuje integrované diagnostiky vám pomůže při ladění aplikace Node.js ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace. V tomto článku se dozvíte, jak povolit protokolování stdout a stderr, v prohlížeči zobrazit informace o chybě a jak stáhnout a zobrazit soubory protokolu.

Diagnostika pro aplikace Node.js hostované v Azure poskytuje [IISNode]. Když tento článek popisuje nejběžnější nastavení pro shromažďování diagnostické informace, neposkytuje úplný referenční pro práci s modulu IISNode. Další informace o práci s modulu IISNode, najdete v článku [modulu IISNode Readme] na Githubu.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Povolit protokolování
Ve výchozím nastavení webové aplikace služby App Service zaznamená pouze diagnostické informace o nasazení, například když nasadíte webovou aplikaci pomocí Git. Tyto informace jsou užitečné, pokud máte potíže při nasazení, například selhání při instalaci modulu, kterou se odkazuje v **package.json**, nebo pokud používáte vlastní nasazení skriptu.

Chcete-li povolit protokolování stdout a stderr datové proudy, musíte vytvořit **IISNode.yml** souboru v kořenovém adresáři aplikace Node.js a přidejte následující:

    loggingEnabled: true

To umožňuje protokolování stderr a stdout z aplikace Node.js.

**IISNode.yml** soubor lze použít také na ovládací prvek, zda popisný chyby nebo vývojáře se vrátí do prohlížeče při dojde k chybě. Povolit vývojáře chyby, přidejte následující řádek na **IISNode.yml** souboru:

    devErrorsEnabled: true

Jakmile povolíte tuto možnost, modul IISNode vrátí poslední 64 kB informace odesílané do stderr místo popisný chyby, jako je "o vnitřní chybu serveru došlo k chybě".

> [!NOTE]
> Zatímco devErrorsEnabled je užitečné při diagnostice problémů během vývoje, povolení v produkčním prostředí může vést k chybám vývoj odesílány koncovým uživatelům.
> 
> 

Pokud **IISNode.yml** soubor v rámci vaší aplikace již neexistoval, je nutné restartovat vaší webové aplikace po publikování aktualizované aplikace. Pokud chcete změnit nastavení v existující jednoduše **IISNode.yml** soubor, který se dříve publikoval, není nutné žádné restartování.

> [!NOTE]
> Pokud vaše webová aplikace byla vytvořena pomocí nástroje příkazového řádku Azure nebo rutin prostředí Azure PowerShell, výchozí **IISNode.yml** soubor se automaticky vytvoří.
> 
> 

Restartování webové aplikace, vyberte webovou aplikaci v [portálu Azure](https://portal.azure.com)a potom klikněte na **RESTARTUJTE** tlačítko:

![Restartujte tlačítko][restart-button]

Pokud nástroje příkazového řádku Azure jsou nainstalovány ve vašem vývojovém prostředí, můžete tyto příkazy k restartování webové aplikace:

    azure site restart [sitename]

> [!NOTE]
> LoggingEnabled a devErrorsEnabled jsou nejčastěji používané IISNode.yml možnosti konfigurace pro shromažďování diagnostických informací, IISNode.yml slouží ke konfiguraci celou řadu možností pro hostitelské prostředí. Úplný seznam možností konfigurace, najdete v článku [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) souboru.
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a>Přístup k protokoly
Diagnostické protokoly jsou přístupné třemi způsoby; Pomocí protokol FTP (File Transfer), stahování archivu Zip, nebo jako živé aktualizovat datového proudu protokolu (také označované jako tail). Stahování archivu Zip souborů protokolu nebo zobrazení živý datový proud vyžadují nástroje příkazového řádku Azure. Lze je instalovat pomocí následujícího příkazu:

    npm install azure-cli -g

Po instalaci nástroje je přístupný pomocí příkazu "azure". Nástroje příkazového řádku musí nejdřív nakonfigurovat na používání vašeho předplatného Azure. Informace o tom, jak se dá tento úkol provést, najdete v tématu **stahování a import nastavení publikování** části [postup použití nástroje příkazového řádku Azure](../xplat-cli-connect.md) článku.

### <a name="ftp"></a>FTP
Pro přístup k diagnostické informace pomocí služby FTP, přejděte [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci a pak vyberte **řídicí panel**. V **rychlé odkazy** části **diagnostické protokoly FTP** a **FTPS diagnostické protokoly** odkazy poskytují přístup k protokolům pomocí protokolu FTP.

> [!NOTE]
> Pokud jste předtím nenakonfigurovali uživatelské jméno a heslo pro nasazení nebo FTP, můžete tak učinit z **rychlý Start** stránce management výběrem **nastavit přihlašovací údaje nasazení**.
> 
> 

Adresa URL FTP, vrátí se v řídicím panelu **LogFiles** adresáři, který bude obsahovat následující dílčí adresáře:

* [Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, se vytvoří adresář se stejným názvem a bude obsahovat informace týkající se nasazení.
* nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)

### <a name="zip-archive"></a>Archivu ZIP
Chcete-li stáhnout archivu Zip diagnostických protokolů, použijte následující příkaz z příkazového řádku nástroje Azure:

    azure site log download [sitename]

To bude stahovat **diagnostics.zip** v aktuálním adresáři. Archivu obsahuje následující adresářovou strukturu:

* nasazení – protokolu informace o nasazení aplikace
* LogFiles
  
  * [Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, se vytvoří adresář se stejným názvem a bude obsahovat informace týkající se nasazení.
  * nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)

### <a name="live-stream-tail"></a>Živý datový proud (tail)
Chcete-li zobrazit živý datový proud diagnostických informací, protokolu, použijte následující příkaz z příkazového řádku nástroje Azure:

    azure site log tail [sitename]

Tato možnost vrátí datový proud protokolu událostí, které jsou aktualizované, kdy k nim dojde na serveru. Tento datový proud vrátí informace o nasazení a také stdout a stderr informace (Pokud loggingEnabled hodnotu true.)

<a id="nextsteps"></a>

## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak povolit a přístup k diagnostické informace pro Azure. Když tyto informace jsou užitečné v Principy problémy, které se aplikace, mohou odkazovat problém s modulem používáte nebo verze Node.js používané App Service Web Apps je jiný než ten, který používá v prostředí pro nasazení.

Informace v práci s moduly v Azure najdete v tématu [používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md).

Informace o určení verze Node.js pro vaši aplikaci najdete v tématu [určení verze Node.js v aplikaci Azure].

Další informace naleznete také [středisku pro vývojáře Node.js](/develop/nodejs/).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[modulu IISNode Readme]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[určení verze Node.js v aplikaci Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

