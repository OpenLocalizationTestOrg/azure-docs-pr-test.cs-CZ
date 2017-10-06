---
title: "aaaHow toodebug webové aplikace Node.js ve službě Azure App Service"
description: "Zjistěte, jak toodebug Node.js webové aplikace v Azure App Service."
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
ms.openlocfilehash: 888ec5c3f92cfc3aeea4ea86005b9b6a0d1306ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-a-nodejs-web-app-in-azure-app-service"></a>Jak toodebug Node.js webové aplikace v Azure App Service
Azure poskytuje integrované diagnostiky tooassist s ladění aplikace Node.js ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace. V tomto článku se dozvíte, jak protokolování tooenable stdout a stderr, informace o chybě zobrazení v prohlížeči hello a jak toodownload a zobrazit soubory protokolu.

Diagnostika pro aplikace Node.js hostované v Azure poskytuje [IISNode]. Když tento článek popisuje hello nejběžnější nastavení pro shromažďování diagnostické informace, neposkytuje úplný referenční pro práci s modulu IISNode. Další informace o práci s modulu IISNode, najdete v části hello [modulu IISNode Readme] na Githubu.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Povolit protokolování
Ve výchozím nastavení webové aplikace služby App Service zaznamená pouze diagnostické informace o nasazení, například když nasadíte webovou aplikaci pomocí Git. Tyto informace jsou užitečné, pokud máte potíže při nasazení, například selhání při instalaci modulu, kterou se odkazuje v **package.json**, nebo pokud používáte vlastní nasazení skriptu.

tooenable hello protokolování stdout a stderr datové proudy, musíte vytvořit **IISNode.yml** souboru v kořenovém adresáři aplikace Node.js hello a přidejte následující hello:

    loggingEnabled: true

To umožňuje protokolování hello stderr a stdout z aplikace Node.js.

Hello **IISNode.yml** soubor může být také použít toocontrol zda popisný chyby nebo vývojáře jsou vráceny toohello prohlížeče, když dojde k chybě. chyby tooenable developer, přidejte následující řádek toohello hello **IISNode.yml** souboru:

    devErrorsEnabled: true

Jakmile povolíte tuto možnost, modul IISNode vrátí hello poslední 64 tisíc informací odesílaných toostderr místo popisný chyby, jako je "o vnitřní chybu serveru došlo k chybě".

> [!NOTE]
> Zatímco devErrorsEnabled je užitečné při diagnostice problémů během vývoje, povolení v produkčním prostředí může vést k chybám vývoj odesílány tooend uživatele.
> 
> 

Pokud hello **IISNode.yml** souboru v rámci vaší aplikace již neexistuje, je nutné restartovat po publikování aplikace hello aktualizovat vaši webovou aplikaci. Pokud chcete změnit nastavení v existující jednoduše **IISNode.yml** soubor, který se dříve publikoval, není nutné žádné restartování.

> [!NOTE]
> Pokud vaše webová aplikace byla vytvořena pomocí nástroje příkazového řádku Azure hello nebo rutin prostředí Azure PowerShell, výchozí **IISNode.yml** soubor se automaticky vytvoří.
> 
> 

toorestart hello webovou aplikaci, vyberte hello webové aplikace ve hello [portálu Azure](https://portal.azure.com)a potom klikněte na **RESTARTUJTE** tlačítko:

![Restartujte tlačítko][restart-button]

Pokud nástroje příkazového řádku Azure hello jsou nainstalovány ve vašem vývojovém prostředí, můžete použít následující příkaz toorestart hello webové aplikace hello:

    azure site restart [sitename]

> [!NOTE]
> LoggingEnabled a devErrorsEnabled jsou možnosti konfigurace IISNode.yml hello nejčastěji používaná pro shromažďování diagnostických informací, může být IISNode.yml použité tooconfigure celou řadu možností pro hostitelské prostředí. Úplný seznam možností konfigurace hello najdete v tématu hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) souboru.
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a>Přístup k protokoly
Diagnostické protokoly jsou přístupné třemi způsoby; Pomocí hello protokol FTP (File Transfer), stahování archivu Zip, nebo jako živé aktualizovat datový proud protokolu hello (také označované jako tail). Stahování archivu Zip hello hello protokolových souborů nebo zobrazení živý datový proud hello vyžadují hello nástroje příkazového řádku Azure. Lze je instalovat pomocí hello následující příkaz:

    npm install azure-cli -g

Po instalaci nástrojů hello je přístupný pomocí příkazu "azure" hello. Hello nástroje příkazového řádku musí být nakonfigurované toouse nejprve při vašeho předplatného Azure. Informace o tom, jak se tooaccomplish to úloh najdete v tématu hello **jak toodownload a import nastavení publikování** části hello [jak tooUse hello nástroje příkazového řádku Azure](../xplat-cli-connect.md) článku.

### <a name="ftp"></a>FTP
tooaccess hello diagnostických informací pomocí služby FTP, navštivte hello [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci a pak vyberte hello **řídicí panel**. V hello **rychlé odkazy** části hello **diagnostické protokoly FTP** a **FTPS diagnostické protokoly** odkazy poskytují přístup k protokolům toohello pomocí protokolu hello FTP.

> [!NOTE]
> Pokud jste předtím nenakonfigurovali uživatelské jméno a heslo pro nasazení nebo FTP, můžete tak učinit z hello **rychlý Start** stránce management výběrem **nastavit přihlašovací údaje nasazení**.
> 
> 

Hello adresy URL FTP, vrátí se v řídicím panelu hello je pro hello **LogFiles** adresáři, který bude obsahovat hello následující dílčí adresáře:

* [Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, adresář hello stejným názvem bude vytvořen a bude obsahují informace související s toodeployments.
* nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)

### <a name="zip-archive"></a>Archivu ZIP
toodownload archivu Zip hello diagnostických protokolů, hello použijte následující příkaz z nástroje příkazového řádku Azure hello:

    azure site log download [sitename]

To bude stahovat **diagnostics.zip** v aktuálním adresáři hello. Archivu obsahuje hello následující adresářovou strukturu:

* nasazení – protokolu informace o nasazení aplikace
* LogFiles
  
  * [Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, adresář hello stejným názvem bude vytvořen a bude obsahují informace související s toodeployments.
  * nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)

### <a name="live-stream-tail"></a>Živý datový proud (tail)
tooview živý datový proud protokolů diagnostiky informací, hello použijte následující příkaz z nástroje příkazového řádku Azure hello:

    azure site log tail [sitename]

Tato možnost vrátí datový proud protokolu událostí, které jsou aktualizované, kdy k nim dojde na serveru hello. Tento datový proud vrátí informace o nasazení a také stdout a stderr informace (Pokud loggingEnabled hodnotu true.)

<a id="nextsteps"></a>

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak tooenable a přístup diagnostické informace pro Azure. Když tyto informace jsou užitečné v Principy problémy, které ke kterým dochází u vaší aplikace, už může ukazovat tooa problém s modul, který používáte nebo že hello tato verze Node.js používané App Service Web Apps je jiný než hello používá v nasazení prostředí.

Informace v práci s moduly v Azure najdete v tématu [používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md).

Informace o určení verze Node.js pro vaši aplikaci najdete v tématu [určení verze Node.js v aplikaci Azure].

Další informace najdete v tématu taky hello [středisku pro vývojáře Node.js](/develop/nodejs/).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[modulu IISNode Readme]: https://github.com/tjanczuk/iisnode#readme
[How tooUse hello Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[určení verze Node.js v aplikaci Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

