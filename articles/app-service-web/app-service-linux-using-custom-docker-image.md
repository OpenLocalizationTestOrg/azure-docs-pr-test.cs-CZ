---
title: "Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux | Microsoft Docs"
description: "Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux."
keywords: "služby Azure app service, webové aplikace, linux, docker, kontejneru"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 1458217a31c4781b28877c030a665f5b22819e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Použití vlastní image Docker pro webové aplikace Azure v systému Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Služba App Service poskytuje zásobníky předem definované aplikací v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5. Aplikační služby v systému Linux využívá Docker kontejnery k hostování těchto zásobníky předem sestavených aplikací. Vlastní image Docker můžete také použít k nasazení vaší webové aplikace do zásobník aplikací, které již nejsou definované v Azure. Vlastní imagí Dockeru může být hostitelem buď veřejných nebo privátních Docker úložiště.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Postupy: nastavení vlastní image Docker pro webovou aplikaci
Můžete nastavit vlastní obrázek Docker pro obě aplikace a weby nových nebo stávajících. Při vytváření webové aplikace v systému Linux v [portál Azure](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klikněte na tlačítko **kontejneru konfigurace** nastavit vlastní image Docker:

![Vlastní Image Docker pro novou webovou aplikaci v systému Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Postupy: použití vlastní image Docker z úložiště Docker Hub ##
Použití vlastní image Docker z úložiště Docker Hub:

1. V [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.

2.  Vyberte **úložiště Docker Hub** jako **zdroj bitové kopie**, pak klikněte buď na **veřejné** nebo **privátní** a zadejte **bitové kopie a název značky volitelné**, jako například `node:4.5`. **Spuštění příkazu** je sada automaticky na základě co je definována v souboru bitové kopie Docker, ale můžete nastavit vlastní příkazy.  

    ![Obrázek veřejného úložiště Docker Hub konfigurace][2]

    Když bitové kopie z privátní úložiště, musíte taky zadat přihlašovací údaje úložiště Docker Hub jako (**uživatelské jméno přihlášení** a **heslo**) pro privátní úložiště Docker Hub.

    ![Obrázek privátní úložiště Docker Hub konfigurace][3]

3. Po konfiguraci kontejneru, klikněte na tlačítko **Uložit**.

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a>Jak použít bitovou kopii Docker z registru privátní bitové kopie ##
Použití vlastní image Docker z registru privátní bitové kopie:

1. V [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.

2.  Klikněte na tlačítko **privátní registru** jako **zdroj bitové kopie**. Zadejte **bitové kopie a název značky volitelné**, **adresa URL serveru** pro privátní registru, spolu s přihlašovací údaje (**uživatelské jméno přihlášení** a **heslo**). Klikněte na **Uložit**.

    ![Obrázek Docker z registru privátní konfigurace][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a>Postupy: nastavení port je používán bitové kopie Docker ##

Pokud používáte vlastní image Docker pro vaši webovou aplikaci, můžete použít `WEBSITES_PORT` proměnné prostředí v váš soubor Docker, který získá přidat do kontejneru vygenerovaný. Podívejte se na následující příklad souboru docker pro poznámky Ruby aplikaci:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

Na poslední řádek příkazu se zobrazí, že proměnná prostředí WEBSITES_PORT předaný za běhu. Mějte na paměti, že v příkazech záleží velká a malá písmena.

Dřív používal platformou `PORT` aplikace nastavení, jsme plánování přestat používat použití této aplikace, nastavení a přesouvat pomocí `WEBSITES_PORT` výhradně.

Při použití stávající image Docker vytvořené někdo jiný, budete možná muset zadat jiný port než port 80 pro aplikace. Pokud chcete konfigurovat port, přidání aplikace nastavení s názvem `WEBSITES_PORT` s hodnotou, jak je uvedeno níže:

![Konfigurace nastavení portu aplikace pro vlastní obrázek Docker][6]

## <a name="how-to-set-the-startup-time-for-your-docker-image"></a>Postupy: nastavení času spuštění pro bitové kopie Docker ##

Ve výchozím nastavení Pokud vaše kontejneru nespustí před 230 sekund, restartuje platforma vašeho kontejneru. Pokud vaše vlastní image Docker spustí ve více než 230 sekund, můžete použít `WEBSITES_CONTAINER_START_TIME_LIMIT` aplikace nastavení, hodnota tohoto nastavení je v sekundách, to umožní zachovat platforma vašeho kontejneru spuštěné před jeho restartování. Výchozí hodnota je 230 sekund a maximální povolená hodnota je 600 sekund.

## <a name="how-to-unmount-the-platform-provided-storage"></a>Postupy: odpojení platforma poskytovaná úložiště ##

Ve výchozím nastavení, bude platformou připojit sdílenou složku trvalé úložiště k `\home\` adresáře. Pokud je image kontejneru nemusí trvalé sdílené složky, můžete zakázat připojení tohoto úložiště nastavením `WEBSITES_ENABLE_APP_SERVICE_STORAGE` nastavení aplikace nastavte na `false`. Budete mít stále přístup do tohoto úložiště z webu scm a všechny protokoly Docker (Pokud je povoleno) se zapíšou do protokolu soubory generované platformu.

## <a name="how-to-switch-back-to-using-a-built-in-image"></a>Postupy: Přejít zpět k používání předdefinovaných bitové kopie ##

Chcete-li přepnout pomocí vlastní image pomocí předdefinovaných bitové kopie:

1. V [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **služby App Service**.

2. Vyberte vaše **Runtime zásobníku** Pokud chcete použít integrované bitové kopie, pak klikněte na tlačítko **Uložit**. 

![Konfigurace předdefinovaných Docker image][5]


## <a name="troubleshooting"></a>Řešení potíží ##

Pokud vaše aplikace se nepodaří spustit s vlastní Docker bitovou kopii, zkontrolujte, že že docker protokolů v adresáři LogFiles. Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.
Do protokolu `stdout` a `stderr` z kontejneru, je nutné povolit **kontejner Docker protokolování** pod **protokolů diagnostiky**.

![Povolení protokolování][8]

![Pokud chcete zobrazit protokoly Docker pomocí modulu Kudu][7]

Můžete získat přístup k webu SCM z **Rozšířené nástroje** v **nástroje pro vývoj** nabídky.

## <a name="next-steps"></a>Další kroky ##

Postupujte podle následujících odkazů můžete začít pracovat s webové aplikace v systému Linux.   

* [Úvod do Azure webové aplikace v systému Linux](./app-service-linux-intro.md)
* [Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux](./app-service-linux-using-nodejs-pm2.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](app-service-linux-faq.md)

Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
