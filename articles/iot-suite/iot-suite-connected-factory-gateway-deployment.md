---
title: "aaaDeploy sady Azure IoT Suite připojen objekt pro vytváření brány | Microsoft Docs"
description: "Jak toodeploy brány na systému Windows nebo Linux toohello připojení tooenable připojen objekt pro vytváření předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 72436dec60eda0de20143f362fe740b0c4412f36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-hello-connected-factory-preconfigured-solution"></a>Nasazení brány v systému Windows nebo Linux hello připojen objekt pro vytváření předkonfigurovaného řešení

Hello softwaru požadované toodeploy a brány pro objekt pro vytváření předkonfigurovaného řešení hello připojené má dvě součásti:

* Hello *OPC Proxy* vytváří tooIoT připojení rozbočovače. Hello *OPC Proxy* potom počká pro příkazy a ovládání zprávy z hello integrované OPC prohlížeč, který běží v portálu řešení připojených factory hello.
* Hello *OPC vydavatele* připojí tooexisting místní OPC UA servery a předává telemetrické zprávy z nich tooIoT rozbočovače.

Obě součásti jsou open source a jsou k dispozici jako zdroj na Githubu a jako kontejnery Docker:

| GitHubu | DockerHub |
| ------ | --------- |
| [Vydavatel OPC][lnk-publisher-github] | [Vydavatel OPC][lnk-publisher-docker] |
| [OPC Proxy][lnk-proxy-github] | [OPC Proxy][lnk-proxy-docker] |

Žádné veřejnou IP adresu nebo díry ve hello brány firewall jsou požadovány pro žádné komponenty. Hello OPC Proxy a OPC vydavatele používat pouze odchozí porty 443 a 5671, 8883.

Hello kroky v tomto článku vám ukážou, jak toodeploy a bránu pomocí Docker na buď [Windows](#windows-deployment) nebo [Linux](#linux-deployment). Brána Hello umožňuje připojení k připojené toohello objekt pro vytváření předkonfigurovaného řešení.

> [!NOTE]
> Hello brány software, který běží v kontejner Docker hello [Azure IoT Edge].

## <a name="windows-deployment"></a>Nasazení systému Windows

> [!NOTE]
> Pokud ještě nemáte zařízení brány, Microsoft doporučuje že zakoupit komerční brány na jednom z našich partnerů. Navštivte hello [katalogu zařízení Azure IoT] seznam zařízení brány, které jsou kompatibilní s hello připojeného objektu pro vytváření řešení. Postupujte podle pokynů hello, které jsou součástí hello zařízení tooset bránu hello. Můžete taky použijte následující pokyny toomanually nastavení jedné z vašich bran existující hello.

### <a name="install-docker"></a>Nainstalujte Docker

Nainstalujte [Docker pro systém Windows] na zařízení brány se systémem Windows. Během instalace systému Windows Docker vyberte jednotku na váš počítač tooshare hostitele s Docker. Následující snímek obrazovky ukazuje sdílení hello D jednotky v systému Windows Hello:

![Nainstalujte Docker][img-install-docker]

Pak vytvořte složku s názvem **docker** hello kořenové hello sdílené jednotky.
Můžete také provést tento krok po instalaci docker z hello **nastavení** nabídky.

### <a name="configure-hello-gateway"></a>Konfigurace brány hello

1. Je třeba hello **iothubowner** připojovací řetězec sady Azure IoT Suite připojen objekt pro vytváření nasazení toocomplete hello brány nasazení. V hello [portál Azure], přejděte tooyour IoT Hub ve skupině prostředků hello vytvořili při nasazení hello připojen objekt pro vytváření řešení. Klikněte na tlačítko **zásady sdíleného přístupu** tooaccess hello **iothubowner** připojovací řetězec:

    ![Najde hello připojovací řetězec služby IoT Hub][img-hub-connection]

    Kopírování hello **připojovací řetězec – primární klíč** hodnotu.

1. Konfigurace brány hello služby IoT hub spuštěním hello dvě brány moduly **po** z příkazového řádku pomocí:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  je hello název toogive tooyour OPC UA vydavatele ve formátu hello **vydavatele.&lt; váš plně kvalifikovaný název domény&gt;**. Například, pokud objekt pro vytváření sítě se nazývá **myfactorynetwork.com**, hello **ApplicationName** hodnota je **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  je hello **iothubowner** připojovacího řetězce, které jste zkopírovali v předchozím kroku hello. Tento připojovací řetězec se používá pouze v tomto kroku, nebudete ho potřebovat v hello následující kroky:

    Používáte hello namapované D:\\docker složky (hello `-v` argument) novější toopersist hello hello brány modulů použít dva certifikáty X.509.

### <a name="run-hello-gateway"></a>Spustit bránu hello

1. Restartujte bránu hello pomocí hello následující příkazy:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Z bezpečnostních důvodů hello dva certifikáty X.509 uchovávané v hello D:\\docker složka obsahovat hello privátní klíč. Omezit přístup toothis složky toohello pověření (obvykle **správci**) použijete kontejner Docker toorun hello. Klikněte pravým tlačítkem na hello D:\\docker složky, vyberte **vlastnosti**, pak **zabezpečení**a potom **upravit**. Poskytnout **správci** úplné řízení a odeberte všechny ostatní:

    ![Udělení oprávnění tooDocker sdílené složky][img-docker-share]

1. Ověřte připojení k síti. Z příkazového řádku, zadejte příkaz hello `ping publisher.<your fully qualified domain name>` tooping bránu. Pokud hello cíl nedostupný, přidejte hello IP adresu a název souboru hostitelů toohello brány na bránu. soubor hosts Hello nachází v hello **Windows\\System32\\ovladače\\atd** složky.

1. Zkuste tooconnect toohello vydavateli pomocí místního klienta OPC UA systémem hello brány. Hello OPC UA koncový bod je adresa URL `opc.tcp://publisher.<your fully qualified domain name>:62222`. Pokud nemáte klientem OPC UA, můžete stáhnout a použít [open-source OPC UA klienta].

1. Po úspěšném dokončení tyto místní testy procházet toohello **připojit vlastní OPC UA Server** stránku hello připojené vytváření řešení portálu. Zadejte adresu URL koncového bodu vydavatele hello (`tcp://publisher.<your fully qualified domain name>:62222`) a klikněte na tlačítko **Connect**. Získat varování týkající se certifikátu a potom klikněte na **pokračovat.** Další dojde k chybě této hello vydavatel není důvěřovat hello uživatelský Agent webového klienta. tooresolve tato chyba, kopie hello **uživatelský Agent webového klienta** certifikát z hello **D:\\docker\\odmítl certifikáty\\certifikátů** složky toohello **D:\\docker\\uživatelský Agent aplikace\\certifikátů** složky na hello brány. Není nutné toorestart hello brány. Tento krok opakujte. Nyní můžete přihlásit toohello brány z cloudu hello a jsou připravené tooadd OPC UA servery toohello řešení.

### <a name="add-your-opc-ua-servers"></a>Přidání serverů OPC UA

1. Procházet toohello **připojit vlastní OPC UA Server** stránku hello připojené vytváření řešení portálu. Postupujte podle kroků stejná jako v předcházející části tooestablish hello vztah důvěryhodnosti mezi hello připojené factory portál a hello OPC UA server hello. Tento krok sestavuje vzájemné důvěryhodnosti hello certifikátů hello připojené portal objekt pro vytváření a hello OPC UA serveru a vytvoří připojení.

1. Procházet hello OPC UA uzlů stromu OPC UA serveru, klikněte pravým tlačítkem na hello OPC uzly a vyberte **publikování**. Pro publikování toowork tímto způsobem, hello OPC UA serveru a hello vydavatele musí být na hello stejné síti. Jinými slovy, pokud hello plně kvalifikovaný název domény hello vydavatele je **publisher.mydomain.com** pak hello plně kvalifikovaný název domény musí být server OPC UA, například hello **myopcuaserver.mydomain.com**. Pokud vaše instalace se liší, můžete ručně přidat uzly toohello publishesnodes.json soubor ve hello **D:\\docker** složky. Hello publishesnodes.json souboru se automaticky vygeneroval na hello nejprve úspěšné publikování OPC uzlu.

1. Telemetrická data jsou nyní z hello zařízení brány. Hello telemetrie si můžete prohlédnout v hello **umístění objektu pro vytváření** zobrazení hello připojené factory portálu pod **nový objekt pro vytváření**.

## <a name="linux-deployment"></a>Linux nasazení

> [!NOTE]
> Pokud ještě nemáte zařízení brány, Microsoft doporučuje že zakoupit komerční brány na jednom z našich partnerů. Navštivte hello [katalogu zařízení Azure IoT] seznam zařízení brány, které jsou kompatibilní s hello připojeného objektu pro vytváření řešení. Postupujte podle pokynů hello, které jsou součástí hello zařízení tooset bránu hello. Můžete taky použijte následující pokyny toomanually nastavení jedné z vašich bran existující hello.

[Nainstalujte Docker] na vaše zařízení brány Linux.

### <a name="configure-hello-gateway"></a>Konfigurace brány hello

1. Je třeba hello **iothubowner** připojovací řetězec sady Azure IoT Suite připojen objekt pro vytváření nasazení toocomplete hello brány nasazení. V hello [portál Azure], přejděte tooyour IoT Hub ve skupině prostředků hello vytvořili při nasazení hello připojen objekt pro vytváření řešení. Klikněte na tlačítko **zásady sdíleného přístupu** tooaccess hello **iothubowner** připojovací řetězec:

    ![Najde hello připojovací řetězec služby IoT Hub][img-hub-connection]

    Kopírování hello **připojovací řetězec – primární klíč** hodnotu.

1. Konfigurace brány hello služby IoT hub spuštěním hello dvě brány moduly **po** z prostředí s:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  je název hello hello OPC UA aplikace hello brány vytvoří ve formátu hello **vydavatele.&lt; váš plně kvalifikovaný název domény&gt;**. Například **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  je hello **iothubowner** připojovacího řetězce, které jste zkopírovali v předchozím kroku hello. Tento připojovací řetězec se používá pouze v tomto kroku, nebudete ho potřebovat v hello následující kroky:

    Použít hello **/ sdílené** složky (hello `-v` argument) novější toopersist hello hello brány modulů použít dva certifikáty X.509.

### <a name="run-hello-gateway"></a>Spustit bránu hello

1. Restartujte bránu hello pomocí hello následující příkazy:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 –-rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Z bezpečnostních důvodů hello dva certifikáty X.509 uchovávané v hello **/ sdílené** složka obsahovat hello privátní klíč. Omezení přístupu toothis složky toohello přihlašovací údaje použijete toorun hello kontejner Docker. tooset hello oprávnění pro **kořenové** použít pouze, hello `chmod` prostředí shell příkaz na složku hello.

1. Ověřte připojení k síti. Z prostředí, zadejte příkaz hello `ping publisher.<your fully qualified domain name>` tooping bránu. Pokud hello cíl nedostupný, přidejte hello IP adresu a název souboru hostitelů tooyour brány na bránu. soubor hosts Hello nachází v hello **/atd** složky.

1. Zkuste tooconnect toohello vydavateli pomocí místního klienta OPC UA systémem hello brány. Hello OPC UA koncový bod je adresa URL `opc.tcp://publisher.<your fully qualified domain name>:62222`. Pokud nemáte klientem OPC UA, můžete stáhnout a použít [open-source OPC UA klienta].

1. Po úspěšném dokončení tyto místní testy procházet toohello **připojit vlastní OPC UA Server** stránku hello připojené vytváření řešení portálu. Zadejte adresu URL koncového bodu vydavatele hello (`tcp://publisher.<your fully qualified domain name>:62222`) a klikněte na tlačítko **Connect**. Získat varování týkající se certifikátu a potom klikněte na **pokračovat.** Další dojde k chybě této hello vydavatel není důvěřovat hello uživatelský Agent webového klienta. tooresolve tato chyba, kopie hello **uživatelský Agent webového klienta** certifikát z hello **/sdílené/zamítnutý certifikáty nebo certifikáty** složky toohello **/shared/UA aplikací/certifikátů** složka v bráně hello. Není nutné toorestart hello brány. Tento krok opakujte. Nyní můžete přihlásit toohello brány z cloudu hello a jsou připravené tooadd OPC UA servery toohello řešení.

### <a name="add-your-opc-ua-servers"></a>Přidání serverů OPC UA

1. Procházet toohello **připojit vlastní OPC UA Server** stránku hello připojené vytváření řešení portálu. Postupujte podle kroků stejná jako v předcházející části tooestablish hello vztah důvěryhodnosti mezi hello připojené factory portál a hello OPC UA server hello. Tento krok sestavuje vzájemné důvěryhodnosti hello certifikátů hello připojené portal objekt pro vytváření a hello OPC UA serveru a vytvoří připojení.

1. Procházet hello OPC UA uzlů stromu OPC UA serveru, klikněte pravým tlačítkem na hello OPC uzly a vyberte **publikování**. Pro publikování toowork tímto způsobem, hello OPC UA serveru a hello vydavatele musí být na hello stejné síti. Jinými slovy, pokud hello plně kvalifikovaný název domény hello vydavatele je **publisher.mydomain.com** pak hello plně kvalifikovaný název domény musí být server OPC UA, například hello **myopcuaserver.mydomain.com**. Pokud vaše instalace se liší, můžete ručně přidat uzly toohello publishesnodes.json soubor ve hello **/ sdílené** složky. Hello publishesnodes.json se automaticky vygeneroval na hello nejprve úspěšné publikování OPC uzlu.

1. Telemetrická data jsou nyní z hello zařízení brány. Hello telemetrie si můžete prohlédnout v hello **umístění objektu pro vytváření** zobrazení hello připojené factory portálu pod **nový objekt pro vytváření**.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o architektuře hello hello připojeného objektu pro vytváření předkonfigurovaného řešení najdete v tématu [připojené factory návod pro předkonfigurované řešení][lnk-walkthrough].

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Docker pro systém Windows]: https://www.docker.com/docker-windows
[katalogu zařízení Azure IoT]: https://catalog.azureiotsuite.com/?q=opc
[portál Azure]: http://portal.azure.com/
[open-source OPC UA klienta]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Nainstalujte Docker]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IoT Edge]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy