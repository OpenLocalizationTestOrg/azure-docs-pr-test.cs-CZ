---
title: "aaaContainer monitorování řešení v Azure Log Analytics | Microsoft Docs"
description: "Hello monitorování kontejneru řešení v analýzy protokolů umožňuje zobrazení a správa Docker a Windows hostitele kontejneru na jednom místě."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte;banders
ms.openlocfilehash: 2eed1dd81c22faef78a375fca3ebece9e5300c09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a>Řešení monitorování kontejneru v analýzy protokolů

![Kontejnery – symbol](./media/log-analytics-containers/containers-symbol.png)

Tento článek popisuje, jak tooset nahoru a používání hello monitorování kontejneru řešení v analýzy protokolů, která slouží k zobrazení a správa Docker a Windows hostitele kontejneru na jednom místě. Docker je použit systém virtualizační software toocreate kontejnery, které automatizují tootheir nasazení softwaru IT infrastruktury.

Hello řešení zobrazuje, který kontejnery běží, jaké kontejneru bitové kopie, že používáte systém, a kde kontejnery běží. Můžete zobrazit podrobné informace o auditování zobrazující příkazy používané s kontejnery. A kontejnery můžete vyřešit tak, že zobrazení a hledání centralizované protokoly bez nutnosti tooremotely zobrazení Docker nebo hostitelů systému Windows. Můžete najít kontejnery, které může být aktivní nebo využívání nadbytečné prostředky na hostiteli. A lze je zobrazit centralizované procesoru, paměti, úložiště a využití a výkonu informace o síti pro kontejnery. V počítačích se systémem Windows, můžete centralizovat a porovnání protokoly ze systému Windows Server, technologie Hyper-V a Docker kontejnerů. řešení Hello podporuje hello následující orchestrators kontejneru:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift


Hello následující diagram znázorňuje hello vztahy mezi různými hostiteli kontejneru a agentů v OMS.

![Diagram kontejnery](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Požadavky na systém

Než začnete, zkontrolujte následující podrobnosti tooverify splňujete požadavky hello hello.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Podpora řešení monitorování kontejneru pro platformy Docker Orchestrator a verze operačního systému
Hello následující tabulka obsahuje přehled hello Docker orchestration a operační systém monitorování podporu kontejneru inventáře, výkonu a protokoly s analýzy protokolů.   

| | ACS | Linux | Windows | Kontejner<br>Inventáře | Image<br>Inventáře | Node<br>Inventáře | Kontejner<br>Výkon | Kontejner<br>Událost | Událost<br>Protokol | Kontejner<br>Protokol |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Služba<br>Prostředky infrastruktury | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Otevřete Red Hat<br>Posunutí | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(samostatně) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux Server<br>(samostatně) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


### <a name="docker-versions-supported-on-linux"></a>Verze docker podporované v systému Linux

- Docker 1.11 too1.13
- V17.06 docker CE a EE

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 Linuxových distribucích podporovány jako hostitele kontejneru

- Ubuntu 14.04 LTS a 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2 a 7.3
- SLES 12
- RHEL 7.2 a 7.3
- Red Hat OpenShift kontejneru platformy (OCP) 3.4 a 3.5
- Too1.8.8 DC/OS 1.7.3 Mesosphere služby ACS
- Služba ACS Kubernetes 1.4.5 too1.6
- Služba ACS Docker Swarm

### <a name="supported-windows-operating-system"></a>Podporovaný operační systém Windows

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional nebo Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Verze docker podporované v systému Windows

- Docker 1.12 a 1.13
- Docker 17.03.0 a novější

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

1. Přidat hello monitorování kontejneru řešení tooyour pracovním prostorem OMS z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).

2. Nainstalovat a používat Docker s agentem OMS.  V závislosti na použitém operačním systému, můžete vybrat z hello následující metody:

  * Na podporovaných operačních systémů Linux, nainstalujte a spusťte Docker a poté nainstalujte a nakonfigurujte hello [OMS agenta pro Linux](log-analytics-agent-linux.md).  
  * Na jádro operačního systému nelze spustit hello OMS agenta pro Linux. Místo toho spustíte kontejnerizované verzi hello OMS agenta pro Linux. Zkontrolujte [Linux kontejneru hostitele, včetně CoreOS](#for-all-linux-container-hosts-including-coreos) nebo [hostitele kontejner Azure Government Linux včetně CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) při práci s kontejnery v cloudu Azure Government.
  * V systému Windows Server 2016 a Windows 10 instalaci hello modulu Docker a klienta pak připojit agenta toogather informace a odeslat tooLog Analytics.  

### <a name="container-services"></a>Služby kontejneru

- Pokud máte Red Hat OpenShift prostředí, přečtěte si [konfigurace agenta OMS pro Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).
- Pokud máte cluster Kubernetes pomocí hello Azure Container Service, přečtěte si [monitorování clusteru Azure Container Service s Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).
- Pokud máte cluster Azure Container Service DC/OS, přečtěte si informace v [monitorování clusteru Azure Container Service DC/OS s Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
- Pokud máte prostředí režimu Docker Swarm, další informace v [konfigurace agenta OMS pro Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
- Pokud používáte kontejnery s Service Fabric, další informace v [přehled Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Zkontrolujte hello [modulu Docker v systému Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) Další informace o tom, najdete v článku tooinstall a konfigurace vašeho Docker moduly v počítačích se systémem Windows.

> [!IMPORTANT]
> Docker musí být spuštěna **před** nainstalujete hello [OMS agenta pro Linux](log-analytics-agent-linux.md) v hostitelích kontejneru. Pokud jste již nainstalovali agenta hello před instalací Docker, musíte tooreinstall hello OMS agenta pro Linux. Další informace o Docker najdete v tématu hello [Docker webu](https://www.docker.com).


## <a name="linux-container-hosts"></a>Linux kontejneru hostitele

Po instalaci Docker, použijte následující nastavení pro agenta kontejneru hostitele tooconfigure hello pro použití s nástrojem Docker hello. Je třeba nejprve vaše OMS ID a klíč, který můžete najít v hello portálu Azure. V pracovním prostoru, klikněte na tlačítko **rychlý Start** > **počítače** tooview vaše **ID pracovního prostoru** a **primární klíč**.  Zkopírujte a vložte do vašeho oblíbeného editoru.

### <a name="for-all-linux-container-hosts-except-coreos"></a>Pro všechny hostitele kontejneru Linux s výjimkou CoreOS

- Další informace a kroky na tom, jak tooinstall hello OMS agenta pro Linux najdete v tématu [připojit vaše počítače se systémem Linux tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Pro všechny hostitele Linux kontejneru, včetně CoreOS

Spusťte kontejner hello OMS, které chcete toomonitor. Upravit a použít hello následující ukázka:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>Pro všechny hostitele kontejner Azure Government Linux včetně CoreOS

Spusťte kontejner hello OMS, které chcete toomonitor. Upravit a použít hello následující ukázka:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-tooone-in-a-container"></a>Přepínání z pomocí nainstalovaného agenta tooone Linux v kontejneru
Pokud dříve používá hello přímo nainstalovat agenta a chcete tooinstead použijte agenta spuštěného v kontejneru, musíte nejdřív odebrat hello OMS agenta pro Linux. V tématu [hello odinstalace agenta OMS pro Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand, jak odinstalovat toosuccessfully hello agenta.  

### <a name="configure-an-oms-agent-for-docker-swarm"></a>Konfigurace agenta OMS pro Docker Swarm

Jako globální služby můžete spustit hello agenta OMS na Docker Swarm. Použijte následující informace toocreate služby agenta OMS hello. Je třeba tooinsert ID pracovního prostoru OMS a primární klíč.

- Spusťte následující hello na hlavní uzel hello.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Konfigurace agenta OMS pro OpenShift Red Hat
Existují tři způsoby tooadd hello agenta OMS tooRed Hat OpenShift toostart shromažďování kontejner dat monitorování.

* [Instalace hello OMS agenta pro Linux](log-analytics-agent-linux.md) přímo na každém uzlu OpenShift  
* [Povolit rozšíření virtuálního počítače Log Analytics](log-analytics-azure-vm-extension.md) na každém uzlu OpenShift umístěných v Azure  
* Nainstalovat agenta OMS hello jako OpenShift démon sadu  

V této části nabídneme hello kroky požadované tooinstall hello agenta OMS jako démon OpenShift-set.  

1. Přihlášení toohello OpenShift hlavní uzel a zkopírujte hello yaml soubor [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) z Githubu tooyour hlavní uzel a měnit hodnotu hello s vaše ID pracovního prostoru OMS a primární klíč.
2. Spusťte následující příkazy toocreate hello projektu pro OMS a nastavte hello uživatelský účet.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. toodeploy hello démon set, spusťte následující hello:

    `oc create -f ocp-omsagent.yaml`

5. tooverify, který je nakonfigurován a že fungují správně, zadejte následující hello:

    `oc describe daemonset omsagent`  

    a měl by vypadat jako výstup hello:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Pokud chcete toouse tajné klíče toosecure ID pracovního prostoru OMS a primární klíč při použití souboru démon set yaml agenta OMS hello, proveďte následující kroky hello.

1. Přihlášení toohello OpenShift hlavní uzel a zkopírujte hello yaml soubor [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) a tajný klíč generování skriptu [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) z Githubu.  Tento skript vygeneruje soubor yaml hello tajné klíče pro ID pracovního prostoru OMS a primární klíč toosecure vaše secrete informace.  
2. Spusťte následující příkazy toocreate hello projektu pro OMS a nastavte hello uživatelský účet. tajný klíč Hello generování skriptu požádá o vaše ID pracovního prostoru OMS <WSID> a primární klíč <KEY> a po dokončení zpracování se vytvoří soubor ocp secret.yaml hello.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Nasaďte soubor tajný hello spuštěním hello následující:

    `oc create -f ocp-secret.yaml`

5. Ověření nasazení tak, že spustíte následující hello:

    `oc describe secret omsagent-secret`  

    a měl by vypadat jako výstup hello:  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. Nasaďte soubor démon set yaml agenta OMS hello spuštěním hello následující:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Ověření nasazení tak, že spustíte následující hello:

    `oc describe ds oms`

    a měl by vypadat jako výstup hello:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a>Zabezpečit vaše tajné informace pro Docker Swarm a Kubernetes

Pro Docker Swarm a Kubernetes kontejneru služby můžete zabezpečit tajný ID pracovního prostoru OMS a primární klíče.

#### <a name="secure-secrets-for-docker-swarm"></a>Zabezpečené tajné klíče pro Docker Swarm

Pro Docker Swarm, jakmile se vytvoří hello tajný klíč pro ID pracovního prostoru a primární klíč, můžete spustit hello vytvořte službu Docker hello OMSagent. Použijte následující informace toocreate hello tajné informace.

1. Spusťte následující hello na hlavní uzel hello.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Ověřte, že tajné klíče byly vytvořeny správně.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Spuštění hello následující příkaz toomount hello tajné klíče toohello kontejnerizované OMS Agent.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Zabezpečené tajné klíče pro Kubernetes s yaml soubory

Pro Kubernetes použijete soubor skriptu toogenerate hello tajné klíče yaml ID pracovního prostoru a primární klíč. V hello [OMS Docker Kubernetes Githubu](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) stránky, jsou soubory, které můžete použít s nebo bez tajné informace.

- Hello výchozí DaemonSet agenta OMS nemá tajné informace (omsagent.yaml)
- soubor yaml DaemonSet agenta OMS Hello používá tajné informace (omsagent – ds-secrets.yaml) s tajný generování skriptů toogenerate hello tajné klíče yaml (omsagentsecret.yaml) souboru.

Můžete zvolit toocreate omsagent DaemonSets s nebo bez tajných klíčů.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Výchozí soubor yaml OMSagent DaemonSet bez tajné klíče

- Hello výchozí DaemonSet agenta OMS yaml souboru, nahraďte hello `<WSID>` a `<KEY>` tooyour WSID a klíč. Zkopírujte hlavní uzel tooyour hello souboru a spuštění hello následující:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Výchozí OMSagent DaemonSet yaml soubor obsahující tajné údaje

1. toouse DaemonSet agenta OMS tajné informace, pomocí tajných klíčů hello nejprve vytvořit.
    1. Zkopírujte skript hello a soubor tajný šablony a ujistěte se, že jsou na hello stejný adresář.
        - Generování skriptu - tajný klíč gen.sh tajný klíč
        - Šablona tajné – template.yaml tajný klíč
    2. Spusťte skript hello, jako je hello následující ukázka. skript Hello požádá o hello ID pracovního prostoru OMS a primární klíč a po zadání je skript hello vytvoří soubor tajný yaml, můžete ji spustit.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Vytvořte hello tajné klíče pod spuštěním hello následující:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. tooverify spustit hello následující:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Výstup by měl vypadat takto:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Výstup by měl vypadat takto:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Vytvoření vaší omsagent démon set spuštěním``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Ověřte, že hello, které běží DaemonSet agenta OMS, podobně jako toohello následující:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Pro Kubernetes použijte soubor skriptu toogenerate hello tajné klíče yaml pro ID pracovního prostoru a primární klíč. Hello použijte následující informace z příkladu s hello [omsagent yaml soubor](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure tajné informace.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a>Hostitelé kontejneru systému Windows

### <a name="preparation-before-installing-windows-agents"></a>Příprava před instalací agentů v systému Windows

Před instalací agentů do počítačů se systémem Windows, musíte tooconfigure hello Docker služby. Konfigurace Hello umožňuje hello Windows agenta nebo hello analýzy protokolů virtuálního počítače rozšíření toouse hello soketu Docker TCP tak, aby hello agentů můžete přistupovat vzdáleně hello Docker démon a toocapture data monitorování.

#### <a name="toostart-docker-and-verify-its-configuration"></a>toostart Docker a ověřte jeho konfiguraci

Existují kroky tooset až TCP pojmenovaný kanál pro systém Windows Server:

1. V prostředí Windows PowerShell povolte kanálu TCP a pojmenovaný kanál.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Nakonfigurujte Docker hello konfiguračního souboru pro TCP kanálu a pojmenovaný kanál. Hello konfigurační soubor se nachází v C:\ProgramData\docker\config\daemon.json.

    V souboru daemon.json hello budete potřebovat následující hello:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Další informace o konfiguraci démon Docker hello používá s kontejnery Windows najdete v tématu [modulu Docker v systému Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Instalace agentů v systému Windows

tooenable Windows a monitorování, technologie Hyper-V kontejneru nainstalovat na počítačích s Windows, které jsou hostiteli kontejneru hello Microsoft Monitoring Agent (MMA). Pro počítače se systémem Windows v místním prostředí, najdete v části [tooLog počítače připojit Windows Analytics](log-analytics-windows-agents.md). Pro virtuální počítače běžící v Azure, připojte je tooLog analýzy pomocí hello [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).

Můžete monitorovat kontejnery Windows spuštěné v Service Fabric. Ale pouze [virtuální počítače běžící v Azure](log-analytics-azure-vm-extension.md) a [počítačů se systémem Windows v místním prostředí](log-analytics-windows-agents.md) jsou aktuálně podporovány pro Service Fabric.

Můžete ověřit, jestli je že tento hello řešením pro monitorování kontejneru správně nastavený pro systém Windows. toocheck zda hello management pack nebyla stažení správně, vyhledejte *ContainerManagement.xxx*. Hello soubory musí být ve složce C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management SP hello.


## <a name="solution-components"></a>Součásti řešení

Pokud používáte agenty se systémem Windows, potom hello následující sady management pack je nainstalována na každém počítači s agentem při přidání tohoto řešení. Je požadován pro sadu management pack hello žádná konfigurace nebo údržby.

- *ContainerManagement.xxx* nainstalované v C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management SP

## <a name="container-data-collection-details"></a>Kontejner dat kolekce podrobnosti
Hello řešením pro monitorování kontejneru shromažďuje různé metriky a protokolu údaje o výkonu z kontejneru hostitelů a kontejnery pomocí agentů, které povolíte.

Shromažďuje data každé tři minuty hello následující typy agenta.

- [OMS agenta pro Linux](log-analytics-linux-agents.md)
- [Agent služby Windows](log-analytics-windows-agents.md)
- [Rozšíření virtuálního počítače analýzy protokolů](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Záznamů kontejneru

Hello následující tabulka uvádí příklady záznamů shromážděných řešením pro monitorování kontejneru hello a hello datové typy, které se zobrazí ve výsledcích hledání protokolu.

| Datový typ | Datový typ v hledání protokolů | Pole |
| --- | --- | --- |
| Výkon pro hostitele a kontejnery | `Type=Perf` | Počítač, ObjectName, název_čítače &#40; % času procesoru, Disk načte MB, zapíše MB, MB využití paměti, disku sítě přijatých bajtů, síti odesílat bajtů, procesor doba využití sítě &#41; přepočtené, TimeGenerated, Cesta_k_čítači, SourceSystem |
| Kontejner inventáře | `Type=ContainerInventory` | TimeGenerated, počítače a název kontejneru, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, příkazu, CreatedTime, StartedTime, FinishedTime, SourceSystem, identifikátor ContainerID, ID obrázku |
| Kontejner image inventáře | `Type=ContainerImageInventory` | TimeGenerated, počítače, Image, ImageTag, ImageSize, VirtualSize, spuštění, pozastavena, zastavit, se nezdařilo, SourceSystem, ID obrázku, TotalContainer |
| Kontejner protokolu | `Type=ContainerLog` | TimeGenerated, počítač, ID bitové kopie, název kontejneru, LogEntrySource, LogEntry, SourceSystem, identifikátor ContainerID |
| Protokol služby kontejneru | `Type=ContainerServiceLog`  | TimeGenerated, počítače, TimeOfCommand, Image, příkazu, SourceSystem, identifikátor ContainerID |
| Uzel inventáře kontejneru | `Type=ContainerNodeInventory_CL`| TimeGenerated, počítače, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes inventáře | `Type=KubePodInventory_CL` | TimeGenerated, počítače, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Proces kontejneru | `Type=ContainerProcess_CL` | TimeGenerated, počítače, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes události | `Type=KubeEvents_CL` | TimeGenerated, počítače, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, zprávy |

Popisky připojeno příliš*PodLabel* datové typy jsou vlastní štítky. Hello připojením PodLabel popisky uvedené v tabulce hello jsou příklady. Ano `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` se liší v sadě dat vaše prostředí a obecně vypadat jako `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Monitorování kontejnery
Až budete mít povoleno na portálu OMS hello řešení hello, hello **kontejnery** dlaždice se zobrazí souhrnné informace o kontejneru hostitelů a spuštěné v hostitelích kontejnery hello.

![Dlaždice kontejnery](./media/log-analytics-containers/containers-title.png)

dlaždice Hello ukazuje přehled o tom, kolik kontejnery, které máte v prostředí hello a jestli se nezdařila, spuštěná nebo zastavená.

### <a name="using-hello-containers-dashboard"></a>Pomocí řídicího panelu hello kontejnery
Klikněte na tlačítko hello **kontejnery** dlaždici. Zde se zobrazí zobrazení uspořádané podle:

- **Události kontejneru** -zobrazuje stav kontejneru a počítače s kontejnery se nezdařilo.
- **Kontejner protokoly** – zobrazuje graf soubory protokolu kontejneru vygeneroval přes čas a seznam počítačů s hello nejvyšší počet souborů protokolu.
- **Události Kubernetes** – zobrazuje graf Kubernetes události generované přes čas a seznamu hello důvodů, proč pracovními stanicemi soustředěnými kolem vygeneruje hello události. *Tato datová sada se používá jenom v prostředích Linux.*
- **Kubernetes Namespace inventáře** – zobrazuje počet hello obory názvů a pracovními stanicemi soustředěnými kolem a zobrazí jejich hierarchie. *Tato datová sada se používá jenom v prostředích Linux.*
- **Uzel inventáře kontejneru** – zobrazuje počet hello orchestration typy používané v kontejneru uzly nebo hostitele. počítač Hello uzly nebo hostuje jsou také uvedeny podle hello počet kontejnerů. *Tato datová sada se používá jenom v prostředích Linux.*
- **Kontejner Imagí inventáře** -zobrazuje celkový počet hello kontejneru obrázků použitých a počet typy obrázků. Hello počet bitových kopií, jsou také uvedené podle značky obrázku hello.
- **Kontejnery stav** -zobrazuje celkový počet kontejneru hello uzly nebo hostitelské počítače, které mají spuštěných kontejnerů. Počítače jsou také uvedeny podle hello počet spuštěných hostitele.
- **Kontejner proces** -zobrazuje spojnicový graf kontejneru procesů spuštěných v čase. Kontejnery také uvádí spuštěním příkazu/procesu v rámci kontejnerů. *Tato datová sada se používá jenom v prostředích Linux.*
- **Výkon procesoru kontejneru** – zobrazuje graf řádku hello průměrné využití procesoru v čase pro počítač uzly nebo hostitele. Také seznamy hello počítače uzly nebo hostitelů na základě průměrné využití procesoru.
- **Kontejner výkonu paměti** -znázorňuje spojnicový graf využití paměti v průběhu času. Také uvádí na název instance na základě využití paměti počítače.
- **Výkon počítače** -znázorňuje spojnicových grafů hello procent výkonu procesoru v čase, procentuální využití paměti nad čas a MB volného místa na disku v průběhu času. Můžete ukazatel myši přesunete kterýkoli řádek v grafu tooview další podrobnosti.


Každou oblast hello řídicí panel je vizuální reprezentace hledání, které běží na shromážděná data.

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash01.png)

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash02.png)

V hello **stav kontejneru** oblast, klikněte na tlačítko hello hlavní oblasti, jak je uvedeno níže.

![Stav kontejnery](./media/log-analytics-containers/containers-status.png)

Protokol hledání se otevře, zobrazení informací o stavu hello kontejnerů.

![Hledání protokolů pro kontejnery](./media/log-analytics-containers/containers-log-search.png)

Zde můžete upravit hello vyhledávání dotazu toomodify ho toofind hello konkrétní informace, co vás zajímá. Další informace o protokolu hledání najdete v tématu [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Řešení potíží s tak, že najdete kontejner se nezdařilo

Analýzy protokolů označí kontejneru jako **se nezdařilo** Pokud byl ukončen s nenulový ukončovací kód. Zobrazí se přehled hello chyb nebo selhání v prostředí hello hello **se nezdařilo kontejnery** oblasti.

### <a name="toofind-failed-containers"></a>kontejnery toofind se nezdařilo
1. Klikněte na tlačítko hello **stav kontejneru** oblasti.  
   ![Stav kontejnery](./media/log-analytics-containers/containers-status.png)
2. Protokol hledání se zobrazí stav hello kontejnerů, podobně jako toohello následující.  
   ![kontejnery stavu](./media/log-analytics-containers/containers-log-search.png)
3. Klikněte na tlačítko hello agregovat hodnotu neúspěšné kontejnery tooview Další informace. Rozbalte položku **zobrazit další** ID tooview hello obrázku.  
   ![Neúspěšné kontejnery](./media/log-analytics-containers/containers-state-failed.png)  
4. Potom zadejte následující hello hello vyhledávací dotaz. `Type=ContainerInventory <ImageID>`toosee údaje o hello image, jako je například velikost bitové kopie a počet zastaven a k selhání bitové kopie.  
   ![Neúspěšné kontejnery](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Hledání protokoly pro kontejner dat
Pokud se řešení potíží s konkrétní chyby, může pomoct toosee, kde je vznikl ve vašem prostředí. následující typy protokolu Hello vám pomůže vytvořit dotazy tooreturn hello informace, které chcete.


- **ContainerImageInventory** – tento typ použijte, když se pokoušíte toofind informace uspořádané podle bitové kopie a tooview informace o obrázku jako ID bitové kopie nebo velikosti.
- **ContainerInventory** – tento typ použijte, pokud chcete informace o umístění kontejneru, jaké jsou jejich názvy a co bitové kopie, že používáte.
- **ContainerLog** – tento typ použijte, pokud chcete informace o protokolu konkrétní chybě toofind a položky.
- **ContainerNodeInventory_CL** tento typ můžete použít hello informace o uzlu hostitele nebo kde je umístěný kontejnery. Poskytuje Docker verze, typ orchestration, úložiště a informace o síti.
- **ContainerProcess_CL** použít tento typ tooquickly najdete v části hello proces, který běží v rámci kontejneru hello.
- **ContainerServiceLog** – tento typ použijte, pokud se pokoušíte toofind informace protokolu auditu pro hello Docker démona, jako je například spuštění, zastavení, odstraňovat nebo příkazy pro vyžádání obsahu.
- **KubeEvents_CL** použít tento typ toosee hello Kubernetes události.
- **KubePodInventory_CL** tento typ použijte, pokud chcete informace o hierarchii toounderstand hello clusteru.


### <a name="toosearch-logs-for-container-data"></a>toosearch protokoly pro kontejner dat
* Vyberte obrázek, který znáte selhával a vyhledání protokolů chyb hello pro ni. Začněte tím, že název kontejneru, který běží této bitové kopie s hledání **ContainerInventory** vyhledávání. Například vyhledejte`Type=ContainerInventory ubuntu Failed`  
    ![Hledat kontejnery Ubuntu](./media/log-analytics-containers/search-ubuntu.png)

  Hello název kontejneru hello vedle příliš**název**a vyhledejte tyto protokoly. V tomto příkladu je to `Type=ContainerLog cranky_stonebreaker`.

**Informace o zobrazení výkonu**

Pokud jste od tooconstruct dotazy, může pomoct toosee co je možné nejprve. Například toosee, které vyhledávání všechny údaje o výkonu, zkuste široký dotazu zadáním hello následující dotaz.

```
Type=Perf
```

![kontejnery výkonu](./media/log-analytics-containers/containers-perf01.png)

Data výkonu hello se zobrazuje specifický kontejner tooa zadáním názvu hello je toohello napravo od dotazu, můžete určit obor.

```
Type=Perf <containerName>
```

Který zobrazuje hello seznam metriky výkonu, které se shromažďují pro jednotlivé kontejneru.

![kontejnery výkonu](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Příklad protokolu vyhledávací dotazy
Často užitečné toobuild dotazuje počínaje příklad nebo dva a úpravou těchto toofit prostředí. Jako počáteční bod, můžete experimentovat s hello **ukázkové dotazy** oblasti toohelp sestavení složitější dotazy.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Kontejnery dotazy](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Uložení protokolu vyhledávacích dotazů
Uložení dotazů je standardní funkce v analýzy protokolů. Je uložíte, budete mít ty, které jste najít užitečné užitečný pro budoucí použití.

Jakmile vytvoříte dotaz, který pro vás užitečné, uložte kliknutím na **Oblíbené** hello horní části stránky hledání protokolu hello. Potom můžete snadno k němu přístup později z hello **vlastní řídicí panel** stránky.

## <a name="next-steps"></a>Další kroky
* [V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné záznamy dat kontejneru.
