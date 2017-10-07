---
title: aaaAzure aplikace Service Fabric oprava orchestration | Microsoft Docs
description: "Operační systém opravy na cluster Service Fabric tooautomate aplikace."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.openlocfilehash: fbb89aa2ea418181ee908a01850178c113c462fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="patch-hello-windows-operating-system-in-your-service-fabric-cluster"></a>Oprava operačního systému Windows hello v clusteru Service Fabric

Hello oprava orchestration aplikace je aplikace Azure Service Fabric, která automatizuje operačního systému opravy na cluster Service Fabric v Azure bez výpadků.

Hello oprava orchestration aplikaci poskytuje hello následující:

- **Automatická instalace operačního systému aktualizaci**. Aktualizace operačního systému se automaticky stáhnout a nainstalovat. Uzly se restartují podle potřeby a bez výpadku clusteru.

- **Clustery opravy a stavu integrace**. Při použití aktualizací, monitoruje hello oprava orchestration aplikace hello stav hello uzly clusteru. Upgradovaná jeden uzel nebo jednu upgradovací doménu jsou uzly clusteru současně. Pokud stav hello hello clusteru ocitne mimo provoz z důvodu toohello oprav procesu, opravy je zastaven tooprevent zhorší hello problém.

## <a name="internal-details-of-hello-app"></a>Interní podrobnosti aplikace hello

Hello oprava orchestration aplikace se skládá z hello následující dílčí součásti:

- **Služba Koordinátor**: Tato stavová služba je zodpovědná za:
    - Úloha aktualizace Windows hello na celý cluster hello spolupráci.
    - Ukládání hello výsledek dokončené operace aplikace Windows Update.
- **Služba agenta uzlu**: Tento bezstavové služby běží na všech uzlech clusteru Service Fabric. Hello služba je zodpovědná za:
    - Zavedení spouštěcího programu hello NTService agenta uzlu.
    - Monitorování hello NTService agenta uzlu.
- **Uzel agenta NTService**: Tento systém Windows NT služba běží na vyšší úrovni oprávnění (systém). Naproti tomu hello Služba agenta uzlu a hello Služba koordinátora spouštět na nižší úrovni oprávnění (síťová služba). Hello služba je zodpovědná za provádění následujících úloh aktualizace systému Windows na všech uzlech clusteru hello hello:
    - Vypnutí automatických aktualizací Windows hello uzlu.
    - Podle uživatele hello zásad toohello poskytl stahování a instalace služby Windows Update.
    - Restartování počítače post hello instalace služby Windows Update.
    - Odesílání hello výsledky toohello aktualizace systému Windows služby Coordinator.
    - Vytváření sestav stavu hlásí v případě, že operace se nezdařilo po jejím vyčerpání všechny opakování.

> [!NOTE]
> Hello oprava orchestration aplikace používá hello Service Fabric opravit toodisable služby správce systému nebo povolit hello uzlu a provádět kontroly stavu. Úloha opravy Hello vytvořené hello oprava orchestration aplikace sleduje hello průběh Windows Update pro každý uzel.

## <a name="prerequisites"></a>Požadavky

### <a name="minimum-supported-service-fabric-runtime-version"></a>Minimální podporovaná verze modulu runtime Service Fabric

#### <a name="azure-clusters"></a>Azure clustery
Hello oprava orchestration aplikace musí být spuštěn na Azure clustery, které mají v5.5 verzi modulu runtime Service Fabric nebo novější.

#### <a name="standalone-on-premises-clusters"></a>Samostatné místní clustery
Hello oprava orchestration aplikace musí být spuštěn na samostatné clustery, které mají verze 5.6 verzi modulu runtime Service Fabric nebo novější.

### <a name="enable-hello-repair-manager-service-if-its-not-running-already"></a>Povolení hello opravy Správce služby (Pokud již není spuštěn)

Hello oprava orchestration aplikace vyžaduje hello opravy správce systému služby toobe na hello clusteru povolena.

#### <a name="azure-clusters"></a>Azure clustery

Azure clustery ve vrstvě stříbrným odolnost hello mají hello opravit service manager ve výchozím nastavení povolené. Azure clustery ve vrstvě gold odolnost hello mohou nebo nemusí mít službu správce opravy hello povoleno, v závislosti na tom, kdy byly vytvořeny těchto clusterů. Azure clustery ve vrstvě hello bronzovým odolnost, ve výchozím nastavení, není nutné hello opravit service manager povolena. Pokud už je povolena služba hello, uvidíte jeho spuštění v části služby systému hello v hello Service Fabric Exploreru.

##### <a name="azure-portal"></a>portál Azure
Opravte manager z portálu Azure můžete povolit v době hello nastavení clusteru. Vyberte `Include Repair Manager` možnost pod `Add on features` během hello konfigurace clusteru.
![Image Manager opravy povolení z portálu Azure](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-template"></a>Šablona Azure Resource Manageru
Případně můžete také použít hello [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello opravy Správce služby v nových nebo stávajících clusterů Service Fabric. Získáte hello šablony pro hello clusteru, které chcete toodeploy. Můžete buď použít hello ukázkové šablony, nebo vytvořit vlastní šablony Resource Manageru. 

tooenable hello opravy Správce služby pomocí [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Nejdřív zkontrolujte, že hello `apiversion` je nastaven příliš`2017-07-01-preview` pro hello `Microsoft.ServiceFabric/clusters` prostředků, jak je znázorněno v následujícím fragmentu kódu hello. Pokud se liší, pak musíte tooupdate hello `apiVersion` toohello hodnotu `2017-07-01-preview`:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Teď povolit service manager hello opravy přidáním následující hello `addonFeatures` po provedení hello `fabricSettings` části:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. Po aktualizaci šablony clusteru se tyto změny použít, je a nechat hello upgrade dokončit. Nyní můžete vidět hello opravy správce systému služby běžící v clusteru. Je volána `fabric:/System/RepairManagerService` v části služby systému hello v hello Service Fabric Exploreru. 

### <a name="standalone-on-premises-clusters"></a>Samostatné místní clustery

Můžete použít hello [nastavení konfigurace pro samostatný cluster Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello opravy Správce služby v nových nebo stávajících clusteru Service Fabric.

Služba Správce oprava tooenable hello:

1. Nejdřív zkontrolujte, že hello `apiversion` v [konfigurace obecné clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) je nastaven příliš`04-2017` nebo vyšší:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Teď povolit service manager opravy přidáním následující hello `addonFeaturres` po provedení hello `fabricSettings` části, jak je uvedeno níže:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. Aktualizovat manifestu clusteru se tyto změny pomocí manifestu clusteru hello aktualizovat [vytvoření nového clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) nebo [konfigurace clusteru upgradu hello](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration). Jakmile hello clusteru je spuštěn s manifestu aktualizované clusteru, se zobrazí hello opravy správce systému služby běžící v clusteru, který se nazývá `fabric:/System/RepairManagerService`v systému služby v Service Fabric explorer hello části.

### <a name="disable-automatic-windows-update-on-all-nodes"></a>Zakázat automatické aktualizace systému Windows na všech uzlech

Automatické aktualizace systému Windows může vést ke ztrátě tooavailability, protože více uzlech clusteru můžete restartovat hello stejný čas. Hello oprava orchestration aplikace, ve výchozím nastavení pokusů toodisable hello automatických aktualizací Windows na všech uzlech clusteru. Pokud hello nastavení spravuje správce nebo zásad skupiny, ale doporučujeme hello nastavení zásad příliš "oznámit před stažení" explicitně služby Windows Update.

### <a name="optional-enable-azure-diagnostics"></a>Volitelné: Povolení Azure Diagnostics

Clustery se systémem verzi modulu runtime Service Fabric `5.6.220.9494` a výše shromažďování oprava orchestration aplikace protokoly jako součást Service Fabric protokoly.
Pokud váš cluster běží na verzi modulu runtime Service Fabric, můžete tento krok přeskočit `5.6.220.9494` a vyšší.

Pro clustery spuštěná verze modulu runtime Service Fabric menší než `5.6.220.9494`, na každém uzlu clusteru hello místně shromáždí protokoly pro aplikaci orchestration oprava hello.
Doporučujeme nakonfigurovat Azure Diagnostics tooupload protokoly ze všech uzlů tooa centrálního umístění.

Informace o povolení Azure Diagnostics najdete v tématu [shromažďování protokolů pomocí Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).

Protokoly pro hello oprava orchestration aplikace jsou generovány na následující pevný zprostředkovatele ID hello:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

V goto šablony Resource Manageru `EtwEventSourceProviderConfiguration` oddílu pod `WadCfg` a přidejte hello následující položky:

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> Pokud váš cluster Service Fabric má více typů uzlu, pak hello předchozí části, je třeba přidat k všechny hello `WadCfg` oddíly.

## <a name="download-hello-app-package"></a>Stáhněte si balíček aplikace hello

Stažení aplikace hello z hello [stáhnout odkaz](https://go.microsoft.com/fwlink/P/?linkid=849590).

## <a name="configure-hello-app"></a>Konfigurace aplikace hello

Hello chování hello oprava orchestration aplikace může být nakonfigurované toomeet vašim potřebám. Přepište výchozí hodnoty hello předávání v parametru aplikace hello při vytvoření aplikace nebo aktualizace. Parametry aplikačního lze zadat zadáním `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` nebo `New-ServiceFabricApplication` rutiny.

|**Parametr**        |**Typ**                          | **Podrobnosti**|
|:-|-|-|
|MaxResultsToCache    |Dlouhé                              | Maximální počet výsledků Windows Update, které by měl být mezipaměti. <br>Výchozí hodnota je 3000 za předpokladu, že: <br> -Počet uzlů je 20. <br> -Počet aktualizací děje na uzlu za měsíc je pět. <br> -Počet výsledků na operace může být 10. <br> -Výsledky pro hello posledních tří měsíců by měly být uložené. |
|TaskApprovalPolicy   |výčet <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy označuje hello zásady, které toobe používané aktualizace systému Windows tooinstall Služba koordinátora hello mezi uzly clusteru Service Fabric hello.<br>                         Povolené hodnoty jsou: <br>                                                           <b>NodeWise</b>. Windows Update je nainstalovaná jednoho uzlu současně. <br>                                                           <b>UpgradeDomainWise</b>. Windows Update je nainstalovaná jednu upgradovací doménu najednou. (Na maximální hello, můžete přejít všechny hello uzlech náležících tooan domény upgradu pro Windows Update.)
|LogsDiskQuotaInMB   |Dlouhé  <br> (Výchozí: 1024)               |Maximální velikost oprava orchestration aplikace přihlásí MB, který můžete nastavit jako trvalý, místně na uzlech.
| WUQuery               | Řetězec<br>(Výchozí: "IsInstalled = 0")                | Dotaz tooget aktualizace systému Windows. Další informace najdete v tématu [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| InstallWindowsOSOnlyUpdates | BOOL <br> (výchozí: True)                 | Tento příznak umožňuje toobe aktualizace systému nainstalován operační systém Windows.            |
| WUOperationTimeOutInMinutes | celá čísla <br>(Výchozí: 90).                   | Určuje časový limit hello pro všechny operace služby Windows Update (hledání nebo stáhnout nebo nainstalovat). Pokud hello operaci nelze dokončit v rámci zadaného časového limitu Dobrý den, byl přerušen.       |
| WURescheduleCount     | celá čísla <br> (Výchozí: 5).                  | Hello maximálním počtem pokusů hello přeplánuje hello služby Windows update v případě, že operace selže trvalé.          |
| WURescheduleTimeInMinutes | celá čísla <br>(Výchozí: 30). | Hello interval, ve které hello služby přeplánuje hello Windows update v případě, že chyba přetrvává. |
| WUFrequency           | Řetězce s hodnotami oddělenými čárkou (výchozí: "Každý týden, středu, 7:00:00")     | Hello četnost pro instalaci služby Windows Update. Hello formátu a možné hodnoty jsou: <br>-Měsíců, DD, hh, například každý měsíc, 5, 12: 22:32. <br> -Každý týden, den, hh: mm:, pro příklad, týdně, úterý, 12:22:32.  <br> -Denní, hh: mm:, např. denně, 12:22:32.  <br> -Žádný označuje, že by neměl Windows Update provést.  <br><br> Všimněte si, že jsou všechny časy hello v čase UTC.|
| AcceptWindowsUpdateEula | BOOL <br>(Výchozí: true) | Nastavením tohoto příznaku hello aplikace přijímá hello koncového uživatele licenční smlouvu pro Windows Update jménem vlastníka hello hello počítače.              |

> [!TIP]
> Pokud chcete okamžitě toohappen služby Windows Update, nastavte `WUFrequency` relativní toohello času nasazení aplikace. Například předpokládejme, že máte testovací pěti uzly clusteru a plán toodeploy hello aplikace v přibližně 5:00 PM UTC. Pokud budete předpokládat, že nasazení nebo upgradu aplikace hello trvá 30 minut v hello maximální, nastavte hello WUFrequency jako "Denně, 17:30:00."

## <a name="deploy-hello-app"></a>Nasazení aplikace hello

1. Dokončete všechny hello požadovaných krocích tooprepare hello clusteru.
2. Nasaďte aplikaci orchestration oprava hello stejně jako jiná aplikace Service Fabric. Hello aplikaci můžete nasadit pomocí prostředí PowerShell. Postupujte podle kroků hello v [nasazení a odeberte aplikací pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
3. aplikace hello tooconfigure v době nasazení, pass hello hello `ApplicationParamater` toohello `New-ServiceFabricApplication` rutiny. Pro usnadnění nabízíme hello skriptu Deploy.ps1 společně s aplikací hello. toouse hello skriptu:

    - Připojit cluster Service Fabric tooa pomocí `Connect-ServiceFabricCluster`.
    - Spustit skript prostředí PowerShell hello Deploy.ps1 s hello odpovídající `ApplicationParameter` hodnotu.

> [!NOTE]
> Mějte hello hello skript a složce aplikace hello PatchOrchestrationApplication stejný adresář.

## <a name="upgrade-hello-app"></a>Upgrade aplikace hello

tooupgrade stávající aplikace orchestration oprava pomocí prostředí PowerShell, postupujte podle kroků hello v [upgradu aplikace Service Fabric pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-hello-app"></a>Odeberte aplikaci hello

tooremove hello aplikace, postupujte podle kroků hello v [nasazení a odeberte aplikací pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).

Pro usnadnění nabízíme hello skriptu Undeploy.ps1 společně s aplikací hello. toouse hello skriptu:

  - Připojit cluster Service Fabric tooa pomocí ```Connect-ServiceFabricCluster```.

  - Spusťte skript prostředí PowerShell hello Undeploy.ps1.

> [!NOTE]
> Mějte hello hello skript a složce aplikace hello PatchOrchestrationApplication stejný adresář.

## <a name="view-hello-windows-update-results"></a>Zobrazení výsledků aktualizace Windows hello

Hello oprava orchestration aplikace zpřístupňuje rozhraní REST API toodisplay hello historických výsledky toohello uživatele. Příklad hello výsledku JSON:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of hello issues that are included in this update, see hello associated Microsoft Knowledge Base article. After you install this update, you may have toorestart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
Pokud žádná aktualizace je ještě naplánováno, výsledkem hello JSON je prázdný.

Přihlaste se tooquery toohello clusteru služby Windows Update výsledky. Poté zjistit adresu hello repliky pro primární hello Dobrý den Služba koordinátora a stiskněte tlačítko hello adresu URL z prohlížeče hello: http://&lt;REPLIKY IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.

koncový bod REST Hello pro hello Služba koordinátora má dynamický port. toocheck hello přesnou adresu URL, najdete v toohello Service Fabric Exploreru. Například jsou k dispozici na výsledky hello `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.

![Obrázek koncový bod REST](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Pokud je v clusteru hello povoleno hello reverzní proxy server, můžete přístup k adrese URL hello z mimo hello clusteru také.
Hello koncový bod, který potřebuje toobe podle je http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

tooenable hello reverzní proxy server na hello clusteru, postupujte podle kroků hello v [reverzní proxy server v Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Po dokončení konfigurace hello reverzní proxy server, jsou adresovatelné z mimo hello cluster všechny malých služby v hello clusteru, které zveřejňují koncový bod HTTP.

## <a name="diagnosticshealth-events"></a>Diagnostika stavu události

### <a name="collect-patch-orchestration-app-logs"></a>Protokolů shromažďování oprava orchestration aplikace

Oprava orchestration aplikace protokoly se shromažďují jako součást Service Fabric protokoly z modulu runtime verze `5.6.220.9494` a vyšší.
Pro clustery spuštěná verze modulu runtime Service Fabric menší než `5.6.220.9494`, pomocí jedné z následujících metod hello se můžou shromažďovat protokoly.

#### <a name="locally-on-each-node"></a>Místně na každém uzlu

Protokoly se shromažďují místně na každém uzlu clusteru Service Fabric Pokud verzi modulu runtime Service Fabric je menší než `5.6.220.9494`. je technologie Hello umístění tooaccess hello protokoly \[Service Fabric\_instalace\_jednotka\]:\\PatchOrchestrationApplication\\protokoly.

Například pokud Service Fabric je nainstalován na jednotce D, cesta hello je D:\\PatchOrchestrationApplication\\protokoly.

#### <a name="central-location"></a>Centrální umístění

Pokud Azure Diagnostics je nakonfigurovaný jako součást požadovaných krocích, protokoly pro hello oprava orchestration aplikace jsou dostupné ve službě Azure Storage.

### <a name="health-reports"></a>Sestavy o stavu

Hello oprava orchestration aplikace, publikuje také sestavy stavu proti hello Služba koordinátora nebo hello Služba agenta uzlu v následujících případech hello:

#### <a name="a-windows-update-operation-failed"></a>Windows Update operace se nezdařila.

Pokud na uzlu selže operace služby Windows Update, sestavy stavu se vygeneruje proti hello Služba agenta uzlu. Podrobnosti sestavy stavu hello obsahují název problematické uzlu hello.

Po použití dílčích oprav úspěšném dokončení v uzlu hello problematické, jsou automaticky vymazány hello sestavy.

#### <a name="hello-node-agent-ntservice-is-down"></a>Hello NTService agenta uzlu je mimo provoz

Pokud hello NTService agenta uzlu je vypnutý na uzlu, vygeneruje se sestavy stavu úroveň pro upozornění před hello Služba agenta uzlu.

#### <a name="hello-repair-manager-service-is-not-enabled"></a>není povolená služba manager oprava Hello

Pokud hello opravy Správce služby nebyla nalezena v clusteru hello, vygeneruje se pro hello Služba koordinátora sestavy stavu úroveň pro upozornění.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

OTÁZKY. **Proč se při spuštěné aplikaci orchestration oprava hello vidí Moje clusteru v chybovém stavu?**

A. Během procesu instalace hello hello oprava orchestration aplikace zakáže nebo restartování uzlů, které mohou dočasně hello stav clusteru hello směrem dolů.

Na základě hello zásady pro aplikace hello buď jeden uzel můžete přejděte během operace oprav *nebo* celá doména upgradu můžete současně přejděte.

Hello konec hello instalace aktualizace Windows hello, které jsou opětovně uzly povolena post restartování.

V následujícím příkladu hello hello clusteru se, že tooan chybový stav dočasně vzhledem k tomu, že dva uzly byly dolů a hello MaxPercentageUnhealthNodes zásad byla porušena. Chyba Hello je dočasný, dokud probíhá hello opravy operaci.

![Bitové kopie není v pořádku clusteru](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Pokud hello problém přetrvá, podívejte se na část věnovanou řešení potíží toohello.

OTÁZKY. **Oprava aplikace orchestration je ve stavu upozornění**

A. Zkontrolujte toosee v případě sestavy stavu odeslány proti hello aplikace hello hlavní příčinu. Upozornění hello obvykle obsahuje podrobnosti o problému hello. Pokud je přechodný problém hello, aplikace hello je očekávané tooauto obnovit z tohoto stavu.

OTÁZKY. **Co mám dělat, když můj clusteru není v pořádku a je potřeba toodo aktualizaci naléhavé operačního systému?**

A. Hello oprava orchestration aplikace neinstaluje aktualizace při hello clusteru není v pořádku. Zkuste toobring clusteru tooa stavu v pořádku toounblock hello oprava orchestration aplikace pracovního postupu.

OTÁZKY. **Proč opravy napříč clustery trvá tak dlouho toorun?**

A. Hello čas potřebný hello oprava orchestration aplikace je ve většině případů závisí na hello následující faktory:

- zásady Hello hello Služba koordinátora. 
  - Hello výchozí zásady `NodeWise`, výsledkem opravy jenom jeden uzel v čase. Zejména v případě hello větší clusterů, doporučujeme použít hello `UpgradeDomainWise` zásad tooachieve rychlejší opravy napříč clustery.
- Hello počet aktualizací, které jsou k dispozici ke stažení a instalaci. 
- Hello průměrný čas potřebný toodownload a nainstalovat aktualizace, které by neměl překročit několik hodin.
- výkon Hello hello virtuálních počítačů a šířku pásma sítě.

OTÁZKY. **Proč vidí některé aktualizace ve výsledcích Windows Update získat prostřednictvím rozhraní api REST, ale není v rámci hello historii služby Windows Update v počítači?**

A. Některé aktualizace produktu potřebovat toobe změnami svoji historii příslušné aktualizace nebo opravy. Např: Aktualizace programu Windows Defender se nezobrazí v historii služby Windows Update v systému Windows Server 2016.

## <a name="disclaimers"></a>Právní omezení

- Hello oprava orchestration aplikace přijímá hello koncového uživatele licenční smlouvy služby Windows Update jménem uživatele hello. Volitelně může být vypnuto hello nastavení v konfiguraci hello aplikace hello.

- Hello oprava orchestration aplikace shromažďuje telemetrická data tootrack využití a výkonu. telemetrie aplikace Hello následuje hello nastavení runtime Service Fabric hello nastavení telemetrie, (který je ve výchozím).

## <a name="troubleshooting"></a>Řešení potíží

### <a name="a-node-is-not-coming-back-tooup-state"></a>Uzel není vracející se zpět tooup stavu

**uzel Hello se mohla zastavit v zakázání stavu, protože**:

Kontrola zabezpečení čeká na vyřízení. tooremedy této situaci, ujistěte se, že dostatek uzlů, jsou k dispozici v dobrém stavu.

**uzel Hello se mohla zastavit v zakázaném stavu, protože**:

- Hello uzlu zakázal ručně.
- Hello uzel byl zakázán z důvodu tooan probíhající infrastrukturu Azure úlohy.
- uzel Hello byla dočasně zakázána správcem hello oprava orchestration aplikace toopatch hello uzlu.

**Hello uzlu se mohla zastavit v dolní stav protože**:

- uzel Hello byla zařazena v dolů stavu ručně.
- Hello uzlu právě probíhá restartování počítače (který může být aktivovány hello oprava orchestration aplikace).
- uzel Hello je mimo provoz z důvodu tooa vadný virtuálního počítače nebo počítače nebo síťové problémy s připojením.

### <a name="updates-were-skipped-on-some-nodes"></a>Aktualizace, které byly přeskočeny na některých uzlech

Hello oprava orchestration aplikace pokusí tooinstall aktualizace podle toohello přeplánování zásad pro Windows. Služba Hello pokusí toorecover hello uzlu a přeskočit zásady podle toohello aplikace hello aktualizace.

V takovém případě se vygeneruje sestavy stavu úroveň pro upozornění před hello Služba agenta uzlu. Hello výsledek pro služby Windows Update také obsahuje hello možných příčin selhání hello.

### <a name="hello-health-of-hello-cluster-goes-tooerror-while-hello-update-installs"></a>Hello stav clusteru hello přejde tooerror při instalaci aktualizace hello

Vadný služby Windows update můžete zahrnout dolů hello stavu aplikace nebo clusteru v konkrétním uzlu nebo doméně pro upgrade. Hello oprava orchestration aplikace ze všechny následné operace služby Windows Update, dokud nebude hello clusteru znovu v pořádku.

Správce musí zasáhnout a zjistit, proč k problému z důvodu aplikace hello nebo clusteru tooWindows aktualizace.

## <a name="release-notes-"></a>Poznámky k verzi:

### <a name="version-110"></a>Verze 1.1.0
- Veřejné verze

### <a name="version-111"></a>Verze 1.1.1
- Pevná chyby ve SetupEntryPoint z NodeAgentService, která zabránila instalace NodeAgentNTService.

### <a name="version-120-latest"></a>Verze 1.2.0 (nejnovější)

- Opravy chyb kolem systém restartovat pracovního postupu.
- Oprava chyby při vytváření úlohy RM kvůli toowhich Kontrola stavu během přípravy úlohy oprava nebyla děje podle očekávání.
- Změněné hello režimu spuštění pro službu systému windows POANodeSvc z toodelayed automaticky automaticky.
