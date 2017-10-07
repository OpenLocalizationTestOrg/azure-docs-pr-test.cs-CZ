---
title: aaaAzure Service Fabric reverse proxy | Microsoft Docs
description: "Pro komunikaci toomicroservices z vnitřní a vnější hello clusteru pomocí Service Fabric reverzní proxy server."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 0e7835a64ccd74293c7bdd8b41deae414c83dffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Reverzní proxy server v Azure Service Fabric
reverzní proxy server Hello, která je integrována do Azure Service Fabric řeší mikroslužeb v hello cluster Service Fabric, která zveřejňuje koncových bodů protokolu HTTP.

## <a name="microservices-communication-model"></a>Mikroslužeb komunikační model
Mikroslužeb v Service Fabric obvykle spustit na podmnožinu virtuální počítače v clusteru hello a můžete přesunout z jednoho virtuálního počítače tooanother z různých důvodů. Ano hello koncové body pro mikroslužeb můžete změnit dynamicky. Hello obvyklý toocommunicate toohello mikroslužbu je, že následující hello vyřešit smyčka:

1. Přeložit umístění služby hello původně prostřednictvím služby pojmenování hello.
2. Připojení ke službám toohello.
3. Určete hello příčinu selhání připojení a vyřešte umístění služby hello znovu, pokud je to nezbytné.

Tento proces obvykle zahrnuje zabalení knihovny komunikace klienta ve smyčce opakování, který implementuje hello služby řešení a zkuste to znovu zásady.
Další informace najdete v tématu [připojit a komunikovat se službami](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-hello-reverse-proxy"></a>Komunikaci pomocí reverzní proxy server hello
reverzní proxy server Hello v Service Fabric se spustí ve všech uzlech clusteru hello hello. Ji provede procesu překladu, který hello celé služby jménem klienta a pak předá požadavek klienta hello. Klienti, kteří běží na clusteru hello tedy můžete používat všechny klienta HTTP komunikace knihovny tootalk toohello cílovou službu pomocí hello reverzní proxy server, aby hello spustí místně na stejném uzlu.

![Interní komunikaci][1]

## <a name="reaching-microservices-from-outside-hello-cluster"></a>Dosažení mikroslužeb z mimo hello clusteru
Hello výchozí externí komunikaci modelu pro mikroslužeb je model opt-in, kde každé služby nelze přistupovat přímo z externích klientů. [Azure nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md), což je hranici sítě mezi mikroslužeb a externími klienty, provádí překlad síťových adres a externí předává požadavky toointernal IP: port koncové body. toomake klienty přímo přístupné tooexternal mikroslužbu koncový bod, musíte napřed nakonfigurovat tooforward provoz tooeach port, který hello služby používá nástroj pro vyrovnávání zatížení v clusteru hello. Kromě toho většina mikroslužeb, zejména stavová mikroslužeb, nemáte live na všech uzlech clusteru hello. Hello mikroslužeb můžete přesouvat mezi uzly na převzetí služeb při selhání. V takových případech nástroj pro vyrovnávání zatížení nemůže efektivně určit umístění hello z cílového uzlu hello hello repliky toowhich předávat provoz.

### <a name="reaching-microservices-via-hello-reverse-proxy-from-outside-hello-cluster"></a>Dosažení mikroslužeb přes reverzní proxy server hello z clusteru mimo hello
Namísto konfigurace hello port jednotlivé služby nástroji pro vyrovnávání zatížení, můžete nakonfigurovat pouze hello port reverzní proxy server hello nástroji pro vyrovnávání zatížení. Tato konfigurace umožňuje klientům mimo hello cluster dosáhnout služby uvnitř hello clusteru pomocí hello reverzní proxy server bez další konfigurace.

![Externí komunikace][0]

> [!WARNING]
> Když konfigurujete hello reverzní proxy server na portu nástroji pro vyrovnávání zatížení, jsou adresovatelné z mimo hello cluster všechny mikroslužeb hello clusteru, které zveřejňují koncový bod HTTP.
>
>


## <a name="uri-format-for-addressing-services-by-using-hello-reverse-proxy"></a>Formát identifikátoru URI pro adresování služby pomocí reverzní proxy server hello
Hello používá reverzní proxy server, které by měly být předávány konkrétní URI identifikátor URI formátu tooidentify hello oddílu toowhich hello příchozí žádosti o službu:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http (s):** hello reverzní proxy server může být nakonfigurované tooaccept HTTP nebo HTTPS přenosů. Předávání protokolu HTTPS, najdete v části příliš[připojení ke službám zabezpečené tooa reverzní proxy server hello](service-fabric-reverseproxy-configure-secure-communication.md) až budete mít toolisten instalace reverzní proxy server na HTTPS.
* **Cluster plně kvalifikovaný název domény (FQDN) | interní IP:** pro externí klienty můžete nakonfigurovat hello reverzní proxy server tak, aby byla dostupná prostřednictvím hello clusteru domény, například mycluster.eastus.cloudapp.azure.com. Ve výchozím nastavení spouští hello reverzní proxy server na každý uzel. Pro interní provoz hello reverzní proxy server dostupný na místního hostitele nebo na všechny IP adresy pro interní uzlu, například 10.0.0.1.
* **Port:** jde hello port, jako třeba 19081, který byl zadaný pro reverzní proxy server hello.
* **ServiceInstanceName:** jde hello plně kvalifikovaný název hello nasazena instance služby, kterou zkoušíte tooreach bez hello "fabric: /" schéma. Například tooreach hello *fabric: / myapp/Moje_služba/* služby, byste použili *myapp/Moje_služba*.

    Název instance služby Hello je malá a velká písmena. Pomocí velká a malá písmena jinou pro název instance služby hello v adrese URL hello způsobí, že hello požadavky toofail s 404 (není nalezena).
* **Přípona cesty:** jde hello skutečné cesty URL, například *myapi/hodnoty/přidat/3*, pro hello službu, která chcete tooconnect k.
* **Klíč oddílu:** u oddílů služby, je to klíč počítaného oddílu hello hello oddílu, které chcete tooreach. Všimněte si, že toto je *není* hello oddílu ID GUID. Tento parametr není vyžadována pro služby, které používají schéma oddílu singleton hello.
* **PartitionKind:** jde hello schéma oddílu služby. To může být 'Int64Range' nebo "S názvem". Tento parametr není vyžadována pro služby, které používají schéma oddílu singleton hello.
* **ListenerName** jsou koncové body hello ze služby hello hello formuláře {"Koncové body": {"Listener1": "Koncovém bodě 1", "Listener2": "Endpoint2"...}}. Když služba hello zpřístupňuje několik koncových bodů, ten identifikuje koncový bod hello tento požadavek klienta hello by měl být předán. To lze vynechat, pokud služba hello má jenom jeden naslouchací proces.
* **TargetReplicaSelector** určuje jak měla by být vybrána hello cíl replik nebo instancí.
  * Po stavová hello cílovou službu hello TargetReplicaSelector může být jedna z následujících hello: 'PrimaryReplica', 'RandomSecondaryReplica' nebo 'RandomReplica'. Pokud není tento parametr zadán, je výchozí hello 'PrimaryReplica'.
  * Po bezstavové hello cílovou službu reverzní proxy server vybere náhodných instanci hello služba oddílu tooforward hello žádosti o.
* **Časový limit:** určuje hello časový limit pro požadavek hello HTTP vytvořených službou toohello reverzní proxy server hello jménem hello požadavku klienta. Hello výchozí hodnota je 60 sekund. Toto je volitelný parametr.

### <a name="example-usage"></a>Příklad použití
Jako příklad Podívejme hello *fabric: / MyApp/Moje_služba* služby, které se otevře naslouchací proces protokolu HTTP na hello následující adresu URL:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Následují hello prostředky služby hello:

* `/index.html`
* `/api/users/<userId>`

Pokud služba hello používá hello singleton dělení schéma, hello *PartitionKey* a *PartitionKind* parametrů řetězce dotazu nejsou nutné a hello služby lze získat přístup pomocí hello brány jako:

* Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Interně:`http://localhost:19081/MyApp/MyService`

Pokud služba hello používá hello Uniform Int64 dělení schéma, hello *PartitionKey* a *PartitionKind* parametrů řetězce dotazu, musí být použité tooreach oddílu služby hello:

* Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Interně:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

cesta prostředku hello tooreach hello prostředky, které služba hello zpřístupní, jednoduše umístit po hello název služby v adrese URL hello:

* Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Interně:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Brána Hello pak předá adresa URL služby tyto požadavky toohello:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Zvláštní zpracování pro sdílení portů služby
Služba Azure Application Gateway pokusí tooresolve služby adres znovu a opakujte žádost hello, pokud služba není dostupný. Je to hlavní výhodou Application Gateway, proto kód klienta není nutné tooimplement vlastní řešení služby a vyřešit smyčky.

Obecně platí když služba není dostupná, instance služby hello nebo repliky se přesunul tooa jiný uzel jako součást životního cyklu normální. V takovém případě může zobrazit aplikační brány síťové připojení chyba označující, že koncový bod je, že nejsou otevřeny žádné delší na hello původně přeložit adresy.

Ale replik nebo instancí služby můžete sdílet hostitelský proces a může také sdílet port při hostované na základě ovladače http.sys webového serveru, včetně:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [WebListener ASP.NET Core](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

V takovém případě je pravděpodobné, že tento webový server hello je k dispozici v hello hostitelský proces a toorequests, ale hello přeložit instance služby nebo replika již není k dispozici na hostiteli hello reagovat. V takovém případě hello brány obdrží odpověď HTTP 404 z hello webový server. Proto HTTP 404 má dvě odlišné významy:

- Případ #1: adresa služby hello je správný, ale hello prostředek, který hello požadovaného uživatele neexistuje.
- Případ #2: adresa služby hello jsou nesprávné a hello prostředek, který hello požadovaného uživatele mohou existovat na jiný uzel.

prvním případě Hello je normální HTTP 404, která je považována za k chybě uživatele. V druhém případě hello však hello uživatel požadoval na prostředek, který neexistuje. Aplikační brána se nemůže toolocate, protože hello vlastní služby přesunula. Aplikace musí tooresolve hello adresu brány znovu a opakujte žádost hello.

Aplikační brána musí proto způsob toodistinguish mezi těmito dvěma případy. toomake, že rozdíl, nápovědu ze serveru hello je vyžadována.

* Ve výchozím nastavení aplikační brána předpokládá – případ #2 a znovu se pokusí tooresolve a problém žádost o hello.
* tooindicate – případ #1 tooApplication brány, služba hello by měla vrátit hello následující hlavičku HTTP odpovědi:

  `X-ServiceFabric : ResourceNotFound`

Tuto hlavičku HTTP odpovědi označuje normální HTTP 404 situaci, ve které hello požadovaný prostředek neexistuje a adresu tooresolve hello služby Application Gateway se nepokusí znovu.

## <a name="setup-and-configuration"></a>Instalace a konfigurace

### <a name="enable-reverse-proxy-via-azure-portal"></a>Povolit reverzní proxy server prostřednictvím portálu Azure

Portál Azure poskytuje možnost tooenable reverzní proxy server při vytváření nového clusteru Service Fabric.
V části **cluster Service Fabric vytvořit**, krok 2: Konfigurace clusteru, konfigurace typu uzlu, zaškrtněte políčko hello příliš "Reverzní proxy server povolit".
Pro konfiguraci zabezpečené reverzní proxy server, můžete zadat certifikát SSL v kroku 3: zabezpečení, konfigurovat nastavení zabezpečení clusteru, vyberte hello políčko příliš "Zahrnout certifikát SSL pro reverzní proxy server" a zadejte podrobnosti o certifikátu hello.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Povolit reverzní proxy server pomocí šablon Azure Resource Manageru

Můžete použít hello [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md) tooenable hello reverzní proxy server v Service Fabric pro hello clusteru.

Odkazovat příliš[konfigurace HTTPS reverzní proxy server v clusteru s podporou zabezpečení](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) pro Azure Resource Manager šablony ukázky tooconfigure zabezpečené reverzní proxy server s certifikáty vyměnit certifikát a zpracování.

Nejprve zobrazí hello šablony pro hello clusteru, které chcete toodeploy. Můžete buď použít hello ukázkové šablony, nebo vytvořit vlastní šablony Resource Manageru. Potom můžete povolit hello reverzní proxy server pomocí hello následující kroky:

1. Zadejte port pro reverzní proxy server hello v hello [oddílu parametry](../azure-resource-manager/resource-group-authoring-templates.md) hello šablony.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Zadejte hello port pro každý z objektů nodetype hello v hello **clusteru** [části Typ prostředku](../azure-resource-manager/resource-group-authoring-templates.md).

    Hello port je identifikována hello názvu parametru, reverseProxyEndpointPort.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. tooaddress hello reverzní proxy server z mimo hello clusteru Azure nastavit pravidla pro vyrovnávání zatížení Azure hello pro hello port, který jste zadali v kroku 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. tooconfigure certifikáty SSL na portu hello pro hello reverzní proxy server, přidejte hello certifikát toohello ***reverseProxyCertificate*** vlastnost hello **clusteru** [části Typ prostředku](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-hello-cluster-certificate"></a>Podpora certifikát reverzní proxy server, který se liší od certifikátu clusteru hello
 Pokud je certifikát reverzní proxy server hello liší od hello certifikát, který zabezpečuje hello clusteru, pak hello dříve zadané certifikátu by měly být nainstalovány na virtuálním počítači hello a doplnili toohello seznam řízení přístupu (ACL), takže můžete Service Fabric k němu přístup. To lze provést v hello **virtualMachineScaleSets** [části Typ prostředku](../resource-group-authoring-templates.md). Pro instalaci přidejte osProfile toohello tohoto certifikátu. Rozšíření oddílu Hello hello šablony můžete aktualizovat hello certifikátu v seznamu ACL hello.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Pokud používáte certifikáty, které se liší od hello clusteru certifikát tooenable hello reverzní proxy server na existující cluster, nainstalujte certifikát reverzní proxy server hello a aktualizovat hello seznamu ACL v clusteru hello před povolením hello reverzní proxy server. Dokončení hello [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md) nasazení pomocí nastavení hello uvedeno dříve před zahájením nasazení tooenable hello reverzní proxy server v krocích 1 – 4.

## <a name="next-steps"></a>Další kroky
* Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkového projektu na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Předávání služby toosecure HTTP s reverzní proxy server hello](service-fabric-reverseproxy-configure-secure-communication.md)
* [Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)
* [Webové rozhraní API, která používá OWIN v spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)
* [Komunikace WCF pomocí spolehlivé služby](service-fabric-reliable-services-communication-wcf.md)
* Možnosti konfigurace další reverzní proxy server, najdete v části ApplicationGateway/Http [nastavení clusteru Service Fabric přizpůsobit](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
