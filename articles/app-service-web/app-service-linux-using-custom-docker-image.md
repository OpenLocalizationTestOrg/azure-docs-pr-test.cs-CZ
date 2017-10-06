---
title: "aaaHow toouse vlastní image Docker pro webové aplikace Azure v systému Linux | Microsoft Docs"
description: "Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux."
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
ms.openlocfilehash: 8853095d0e1067cfea4297bbd23b622fe4a0d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Použití vlastní image Docker pro webové aplikace Azure v systému Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Služba App Service poskytuje zásobníky předem definované aplikací v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5. Aplikační služby v systému Linux používá Docker kontejnery toohost, tyto předem integrované zásobníky aplikace. Můžete také použít vlastní toodeploy image Docker vaší webové aplikace tooan zásobník aplikací v Azure, již není definovaná. Vlastní imagí Dockeru může být hostitelem buď veřejných nebo privátních Docker úložiště.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Postupy: nastavení vlastní image Docker pro webovou aplikaci
Můžete nastavit hello vlastní image Docker pro obě nové a stávající aplikace a weby. Při vytváření webové aplikace v systému Linux v hello [portál Azure](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klikněte na tlačítko **kontejneru konfigurace** tooset vlastní image Docker:

![Vlastní Image Docker pro novou webovou aplikaci v systému Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Postupy: použití vlastní image Docker z úložiště Docker Hub ##
toouse vlastní image Docker z úložiště Docker Hub:

1. V hello [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.

2.  Vyberte **úložiště Docker Hub** jako hello **zdroj bitové kopie**, pak klikněte buď na **veřejné** nebo **privátní** a typ hello **bitové kopie a název značky volitelné**, jako například `node:4.5`. Hello **spuštění příkazu** je sada automaticky na základě co je definována v souboru bitové kopie Docker hello, ale můžete nastavit vlastní příkazy.  

    ![Obrázek veřejného úložiště Docker Hub konfigurace][2]

    Když bitové kopie z privátní úložiště, musíte taky přihlašovací údaje úložiště Docker Hub hello tooenter jako (**uživatelské jméno přihlášení** a **heslo**) pro privátní úložiště Docker Hub hello.

    ![Obrázek privátní úložiště Docker Hub konfigurace][3]

3. Po nakonfigurování hello kontejner, klikněte na tlačítko **Uložit**.

## <a name="how-toouse-a-docker-image-from-a-private-image-registry"></a>Jak toouse Docker obrázek z registru privátní bitové kopie ##
toouse vlastní image Docker z registru privátní bitové kopie:

1. V hello [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.

2.  Klikněte na tlačítko **privátní registru** jako hello **zdroj bitové kopie**. Zadejte hello **bitové kopie a název značky volitelné**, **adresa URL serveru** pro privátní registru hello spolu s přihlašovací údaje hello (**uživatelské jméno přihlášení** a **heslo** ). Klikněte na **Uložit**.

    ![Obrázek Docker z registru privátní konfigurace][4]


## <a name="how-to-set-hello-port-used-by-your-docker-image"></a>Postupy: nastavení portu hello používá Docker image ##

Pokud používáte vlastní image Docker pro vaši webovou aplikaci, můžete použít hello `WEBSITES_PORT` proměnné prostředí v váš soubor Docker, který získá přidat toohello generované kontejneru. Vezměte v úvahu následující ukázka soubor docker pro poznámky Ruby aplikace hello:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

Na poslední řádek hello příkazu uvidíte, že tuto proměnnou prostředí WEBSITES_PORT hello předaný za běhu. Mějte na paměti, že v příkazech záleží velká a malá písmena.

Dřív byla hello platformě pomocí `PORT` aplikace nastavení, jsme plánování toodeprecate hello použití této aplikace, nastavení a přesunout toousing `WEBSITES_PORT` výhradně.

Pokud používáte stávající image Docker vytvořené někdo jiný, musíte toospecify jiný port než port 80 pro aplikaci hello. tooconfigure hello port, přidání aplikace nastavení s názvem `WEBSITES_PORT` hodnotou hello, jak je uvedeno níže:

![Konfigurace nastavení portu aplikace pro vlastní obrázek Docker][6]

## <a name="how-to-set-hello-startup-time-for-your-docker-image"></a>Postupy: nastavení času spuštění hello pro bitové kopie Docker ##

Ve výchozím nastavení Pokud vaše kontejneru nespustí před 230 sekund, restartuje hello platforma vašeho kontejneru. Pokud vaše vlastní image Docker spustí ve více než 230 sekund, můžete použít hello `WEBSITES_CONTAINER_START_TIME_LIMIT` nastavení aplikace hello hodnota tohoto nastavení je v sekundách, to vám umožní zachovat platformy hello vašeho kontejneru spuštěné před jej restartovat. Hello výchozí hodnota je 230 sekund a hello maximální povolená hodnota je 600 sekund.

## <a name="how-to-unmount-hello-platform-provided-storage"></a>Postupy: odpojení hello platforma poskytovaná úložiště ##

Ve výchozím nastavení, bude hello platformy připojit trvalého úložiště sdílené složky toohello `\home\` adresáře. Pokud je image kontejneru nemusí trvalé sdílené složky, můžete zakázat připojení tohoto úložiště pomocí nastavení hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` aplikace nastavení příliš`false`. Z lokality scm hello budete mít stále přístup k úložišti toothat a všechny protokoly Docker (Pokud je povoleno) se zapíšou toohello souborů protokolu vygenerovaných hello platformy.

## <a name="how-to-switch-back-toousing-a-built-in-image"></a>Postupy: přepínač zpět toousing integrované bitové kopie ##

tooswitch pomocí vlastní image toousing integrované bitové kopie:

1. V hello [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **služby App Service**.

2. Vyberte vaše **Runtime zásobníku** toouse hello integrované bitové kopie, pak klikněte na **Uložit**. 

![Konfigurace předdefinovaných Docker image][5]


## <a name="troubleshooting"></a>Řešení potíží ##

Pokud vaše aplikace nezdaří toostart s vlastní bitovou kopii Docker, zkontrolujte hello Docker protokolů v adresáři LogFiles hello. Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.
toolog hello `stdout` a `stderr` z kontejneru, je nutné tooenable **kontejner Docker protokolování** pod **protokolů diagnostiky**.

![Povolení protokolování][8]

![Pomocí modulu Kudu tooview Docker protokolů][7]

Dostanete hello SCM lokality z **Rozšířené nástroje** v hello **nástroje pro vývoj** nabídky.

## <a name="next-steps"></a>Další kroky ##

Postupujte podle hello následující odkazy tooget začít s webovou aplikaci v systému Linux.   

* [Úvod tooAzure webové aplikace v systému Linux](./app-service-linux-intro.md)
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
