---
title: "Nejčastější dotazy týkající se vytváření připojení aaaAzure IoT Suite | Microsoft Docs"
description: "Nejčastější dotazy pro připojené vytváření IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 4ae9beb0daf1b0578850cd652eaca7635b0d039d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Nejčastější dotazy pro IoT Suite připojené vytváření předkonfigurovaného řešení

Viz také, hello Obecné [– nejčastější dotazy](iot-suite-faq.md) pro IoT Suite.

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solution"></a>Kde najdu hello zdrojového kódu pro hello předkonfigurované řešení?

Hello zdrojového kódu se ukládají v hello následující úložiště GitHub:

* [Připojené objekt pro vytváření předkonfigurovaného řešení](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Co je OPC UA?

OPC Unified architektura (uživatelský Agent), vydané v 2008, je nezávislý na platformě, orientované na služby interoperabilita standardní. OPC UA používají různé průmyslových systémů a zařízení, jako jsou odvětví počítačů, plc a snímače. OPC UA hello funkce OPC Classic specifikace hello integruje do jednoho rozšiřitelná architektura integrované zabezpečení. Je standard, který doprovází hello OPC Foundation. Hello [OPC Foundation](http://opcfoundation.org/) je pro neziskové organizace s víc než 440 členy. cílem Hello hello organizace je OPC toouse specifikace toofacilitate více dodavatele, více platformami, zabezpečený a spolehlivý interoperabilita prostřednictvím:

* Infrastruktura
* Specifikace
* Technologie
* Procesy

### <a name="why-did-microsoft-choose-opc-ua-for-hello-connected-factory-preconfigured-solution"></a>Proč Microsoft zvolit, že OPC UA pro hello připojen objekt pro vytváření předkonfigurovaného řešení?

Microsoft zvolili OPC UA, protože je otevřený, bez chráněné, platformu nezávislé, oborových rozpoznána a osvědčené standardem. Je požadavek na Industrie 4.0 (RAMI4.0) referenční Architektura řešení zajistit interoperabilitu mezi širokou škálu výrobní procesy a vybavení. Microsoft setkává vyžádání z našich zákazníků toobuild Industrie 4.0 řešení. Podpora OPC UA pomáhá nižší hello bariéry pro zákazníky tooachieve svých cílů a poskytuje toothem okamžitou obchodní hodnotu.

### <a name="how-do-i-add-a-public-ip-address-toohello-simulation-vm"></a>Jak přidat veřejné simulace toohello IP adresu virtuálního počítače?

Máte dvě možnosti tooadd hello IP adresu:

* Pomocí skriptu prostředí PowerShell hello `Simulation/Factory/Add-SimulationPublicIp.ps1` v hello [úložiště](https://github.com/Azure/azure-iot-connected-factory). Název nasazení předat jako parametr. Pro místní nasazení použijte `<your username>ConnFactoryLocal`. skript Hello vytiskne hello IP adresu hello virtuálních počítačů.

* V hello portálu Azure vyhledejte skupinu prostředků hello vašeho nasazení. S výjimkou místní nasazení skupiny prostředků hello má hello název, který jste zadali jako řešení nebo název nasazení. Pro místní nasazení pomocí skriptu buildu hello hello název skupiny prostředků hello je `<your username>ConnFactoryLocal`. Nyní přidejte nový **veřejnou IP adresu** skupiny prostředků toohello prostředků.

> [!NOTE]
> V obou případech zajistěte instalaci nejnovějších oprav hello podle pokynů hello na hello [Ubuntu webu](https://wiki.ubuntu.com/Security/Upgrades). Zachovat hello instalace si toodate pro tak dlouho, dokud virtuální počítač je přístupný prostřednictvím veřejnou IP adresu.

### <a name="how-do-i-remove-hello-public-ip-address-toohello-simulation-vm"></a>Odebrání hello veřejnou IP adresu toohello simulace virtuálních počítačů

Máte dvě možnosti tooremove hello IP adresu:

* Pomocí skriptu prostředí PowerShell hello Simulation/Factory/Remove-SimulationPublicIp.ps1 hello [úložiště](https://github.com/Azure/azure-iot-connected-factory). Název nasazení předat jako parametr. Pro místní nasazení použijte `<your username>ConnFactoryLocal`. skript Hello vytiskne hello IP adresu hello virtuálních počítačů.

* V hello portálu Azure vyhledejte skupinu prostředků hello vašeho nasazení. S výjimkou místní nasazení skupiny prostředků hello má hello název, který jste zadali jako řešení nebo název nasazení. Pro místní nasazení pomocí skriptu buildu hello hello název skupiny prostředků hello je `<your username>ConnFactoryLocal`. Nyní odebrat hello **veřejnou IP adresu** prostředků ze skupiny prostředků hello.

### <a name="how-do-i-sign-in-toohello-simulation-vm"></a>Registrace v simulaci toohello virtuálních počítačů

Přihlášení toohello simulace virtuálního počítače je podporována, pouze pokud jste nasadili řešení pomocí skriptu prostředí PowerShell hello `build.ps1` v hello [úložiště](https://github.com/Azure/azure-iot-connected-factory).

Pokud jste nasadili hello řešení z www.azureiotsuite.com, nemůžete se přihlásit toohello virtuálních počítačů. Nemůžete se přihlásit, protože je náhodně vygenerované heslo hello a nelze ho obnovit.

1. Přidejte veřejný toohello adresu IP virtuálního počítače. V tématu [jak přidat veřejné simulace toohello IP adresu virtuálního počítače?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Vytvoření tooyour relace SSH virtuálních počítačů pomocí IP adresy hello hello virtuálních počítačů.
1. je uživatelské jméno toouse Hello: `docker`.
1. Hello heslo toouse závisí na verzi hello, které že jste použili toodeploy:
    * Pro řešení nasazuje pomocí skriptu build.ps1 hello před 1. června 2017, je heslo hello: `Passw0rd`.
    * Řešení nasazuje pomocí skriptu build.ps1 hello po 1. června 2017, můžete najít hello heslo v hello `<name of your deployment>.config.user` souboru. Hello heslo je uloženo v hello **VmAdminPassword** nastavení. Hello hesla je generována náhodně v době nasazení nezadáte pomocí hello `build.ps1` skript parametr`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-hello-simulation-vm"></a>Jak zastavení a spuštění všech procesů docker v simulaci hello virtuálního počítače?

1. Přihlaste se toohello simulace virtuálních počítačů. V tématu [jak přihlásím toohello simulace virtuálního počítače?](#how-do-i-sign-in-to-the-simulation-vm)
1. toocheck kontejnery, které jsou aktivní, spusťte: `docker ps`.
1. spustit všechny kontejnery simulace toostop: `./stopsimulation`.
1. toostart všechny kontejnery simulace:
    * Export proměnnou prostředí s názvem hello **IOTHUB_CONNECTIONSTRING**. Hello hodnotu hello **IotHubOwnerConnectionString** nastavení v hello `<name of your deployment>.config.user` souboru. Například:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Spusťte `./startsimulation`.

### <a name="how-do-i-update-hello-simulation-in-hello-vm"></a>Jak aktualizovat hello simulace v hello virtuálního počítače?

Pokud jste provedli změny toohello simulace, můžete použít skript prostředí PowerShell hello `build.ps1` v hello [úložiště](https://github.com/Azure/azure-iot-connected-factory) pomocí hello `updatedimulation` příkaz. Tento skript vytvoří všechny součásti hello simulace, zastaví hello simulace v hello virtuálních počítačů, odešle, nainstaluje a spustí je.

### <a name="how-do-i-find-out-hello-connection-string-of-hello-iot-hub-used-by-my-solution"></a>Jak zjistím hello připojovací řetězec služby IoT hub hello používá mém řešení?

Pokud jste nasadili řešení pomocí hello `build.ps1` skript v hello [úložiště](https://github.com/Azure/azure-iot-connected-factory), hello připojovací řetězec je hodnota hello **IotHubOwnerConnectionString** v hello `<name of your deployment>.config.user` souboru.

Můžete také získat hello připojovací řetězec pomocí hello portálu Azure. V hello IoT Hub prostředků ve skupině prostředků hello vašeho nasazení vyhledejte hello nastavení připojovacího řetězce.

### <a name="which-iot-hub-devices-does-hello-connected-factory-simulation-use"></a>Zařízení IoT Hub, která hello připojené factory simulace použít?

Hello simulace vlastní zaregistruje hello následující zařízení:

* proxy.Beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.Mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.Rio.corp.contoso
* proxy.SEATTLE.corp.contoso
* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.SEATTLE.corp.contoso

Pomocí hello [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/azure/iothub-explorer) nástroj, můžete zkontrolovat zařízení, která jsou zaregistrována hello IoT hub používá vaše řešení. Tyto nástroje toouse, musíte pro hello IoT hub ve vašem nasazení hello připojovací řetězec.

### <a name="how-can-i-get-log-data-from-hello-simulation-components"></a>Načtení dat protokolu z hello simulace součásti?

Všechny součásti v simulaci hello protokolování informací v souborech toolog. Tyto soubory naleznete v hello virtuálních počítačů ve složce hello `home/docker/Logs`. protokoly hello tooretrieve, můžete použít skript prostředí PowerShell hello `Simulation/Factory/Get-SimulationLogs.ps1` v hello [úložiště](https://github.com/Azure/azure-iot-connected-factory).

Tento skript musí toosign v toohello virtuálních počítačů. Může být nutné tooprovide přihlašovací údaje pro přihlášení hello. V tématu [jak přihlásím toohello simulace virtuální počítač?](#how-do-i-sign-in-to-the-simulation-vm) toofind hello pověření.

Hello skript přidá nebo odebere toohello veřejnou adresu IP virtuálního počítače, pokud ho ještě nemá a odstraní ji. skript Hello vloží všech souborů protokolů v rámci archivu a stáhne hello archivu tooyour pracovní stanici.

Můžete taky přihlásit toohello virtuálních počítačů pomocí protokolu SSH a zkontrolujte soubory protokolu hello za běhu.

### <a name="how-can-i-check-if-hello-simulation-is-sending-data-toohello-cloud"></a>Jak můžete zkontrolovat, pokud hello simulace odesílá toohello dat v cloudu?

S hello [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) nebo hello [iothub-explorer](https://github.com/azure/iothub-explorer) nástroj, si můžete prohlédnout hello data odeslaná tooIoT centra z určitých zařízení. Tyto nástroje toouse, musíte pro hello IoT hub ve vašem nasazení tooknow hello připojovací řetězec. V tématu [Jak zjistím hello připojovací řetězec služby IoT hub hello používá mém řešení?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Zkontrolujte hello data odeslaná pomocí jedné z hello vydavatele zařízení:

* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.SEATTLE.corp.contoso

Pokud se žádná data odeslaná tooIoT rozbočovače, je problém s hello simulace. Jako první krok analysis byste měli provést analýzu soubory protokolu hello součástí simulace hello. V tématu [načtení dat protokolu z hello simulace součásti?](#how-can-i-get-log-data-from-the-simulation-components) V dalším kroku zkuste toostop a spustit simulaci hello a pokud nebude probíhat žádná data odeslaná, aktualizovat hello simulace úplně. V tématu [jak aktualizovat hello simulace v hello virtuálního počítače?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Další kroky

Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby](iot-suite-predictive-overview.md)
* [Přehled připojené objekt pro vytváření předkonfigurovaného řešení](iot-suite-connected-factory-overview.md)
* [Zabezpečení IoT z hello pozadí](securing-iot-ground-up.md)
