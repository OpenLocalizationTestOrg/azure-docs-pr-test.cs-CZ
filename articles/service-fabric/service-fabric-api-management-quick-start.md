---
title: "aaaAzure Service Fabric s úvodní API Management | Microsoft Docs"
description: "Tento průvodce vám ukáže, jak tooquickly Začínáme s Azure API Management a Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: f76f3f39a92f89892d6a02ecaab1ec3d343fe2a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>Service Fabric s Azure API Management rychlý Start

Tento průvodce vám ukáže, jak tooset až Azure API Management s Service Fabric a konfigurace vašeho prvního rozhraní API operaci toosend provoz tooback endové služby v Service Fabric. toolearn Další informace o službě Azure API Management scénáře s Service Fabric, najdete v části hello [přehled](service-fabric-api-management-overview.md) článku. 

## <a name="deploy-api-management-and-service-fabric-tooazure"></a>Nasazení tooAzure API Management a Service Fabric

prvním krokem Hello je toodeploy API Management a tooAzure clusteru Service Fabric ve sdílené virtuální síti. To umožňuje API Management toocommunicate přímo pomocí Service Fabric, aby ho provádět zjišťování služby, řešení oddílu služby a předat dál provoz přímo tooany back-end služby v Service Fabric.

### <a name="topology"></a>topologie

Tento průvodce nasadí hello následující topologie tooAzure, ve které jsou v podsítích v API Management a Service Fabric hello stejné virtuální síti:

 ![Popisek obrázku][sf-apim-topology-overview]

tooget rychle začít, šablony správce prostředků slouží pro každého kroku nasazení:

 - Topologie sítě:
    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]
 - Cluster Service Fabric:
    - [cluster.JSON][cluster-arm]
    - [cluster.Parameters.JSON][cluster-parameters-arm]
 - API Management:
    - [APIM.JSON][apim-arm]
    - [APIM.Parameters.JSON][apim-parameters-arm]

### <a name="sign-in-tooazure-and-select-your-subscription"></a>Přihlaste se tooAzure a vybrat své předplatné

Tato příručka používá [prostředí Azure PowerShell][azure-powershell]. Po spuštění novou relaci prostředí PowerShell se přihlaste tooyour účet Azure a vybrat své předplatné před spuštěním příkazů Azure.
 
Přihlaste se tooyour účet Azure:

```powershell
PS > Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte novou skupinu prostředků pro vaše nasazení. Poskytněte název a umístění.

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-hello-network-topology"></a>Nasazení hello síťové topologie

prvním krokem Hello je tooset až hello síťové topologie toowhich API Management a nasadí se cluster Service Fabric hello. Hello [network.json] [ network-arm] šablony Resource Manageru je nakonfigurované toocreate virtuální sítě (virtuální sítě) se dvěma podsítěmi a skupiny zabezpečení dvě sítě (NSG). 

Hello [network.parameters.json] [ network-parameters-arm] soubor parametrů obsahuje názvy hello hello podsítě a skupin Nsg, které API Management a Service Fabric se nasadí do. Tato příručka hodnoty parametrů hello není nutné toobe změnit. API Management a služby Správce prostředků infrastruktury šablony Hello používají tyto hodnoty, pokud jsou upraveny zde, je nutné upravit je do jiné šablony Resource Manageru hello odpovídajícím způsobem. 

 1. Stáhněte si následující soubor šablony a parametry Resource Manager hello:

    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]

 2. Použijte následující prostředí PowerShell příkaz toodeploy hello Resource Manager šablony a parametr soubory pro nastavení sítě hello hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-hello-service-fabric-cluster"></a>Nasazení clusteru Service Fabric hello

Po dokončení hello síťovým prostředkům nasazení, hello dalším krokem je toodeploy toohello clusteru Service Fabric virtuální sítě v podsíti hello a NSG určené pro cluster Service Fabric hello. V tomto kurzu Šablona služby Správce prostředků infrastruktury hello je předem nakonfigurovaný toouse hello názvy hello virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku hello. 

šablony Resource Manageru clusteru Service Fabric Hello je nakonfigurované toocreate zabezpečení clusteru se certifikát zabezpečení. certifikát Hello je použité toosecure komunikaci mezi uzly pro cluster a cluster Service Fabric tooyour přístup uživatele toomanage. API Management používá tento certifikát tooaccess hello Service Fabric Naming Service pro zjišťování služby.

Tento krok vyžaduje použití certifikátu v Key Vault pro zabezpečení clusteru. Další informace o nastavení zabezpečení clusteru s Key Vault najdete v tématu [Tato příručka týkající se vytvoření clusteru v Azure pomocí Resource Manager](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> Můžete přidat ověřování Azure Active Directory přidání toohello certifikát používaný pro přístup ke clusteru. Azure Active Directory je hello doporučená způsob toomanage uživatel přístup tooyour cluster Service Fabric, ale je v tomto kurzu není nutné toocomplete. Certifikát je nutný v obou případech pro zabezpečení mezi uzly clusteru a pro ověřování Azure API Management, které aktuálně nepodporuje ověřování pomocí služby Azure Active Directory pro Service Fabric back-end.

 1. Stáhněte si následující soubor šablony a parametry Resource Manager hello:
 
    - [cluster.JSON][cluster-arm]
    - [cluster.Parameters.JSON][cluster-parameters-arm]

 2. Vyplňte hello prázdné parametry v hello `cluster.parameters.json` soubor pro vaše nasazení, včetně hello [Key Vault informace](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) pro váš cluster certifikát.

 3. Použijte následující prostředí PowerShell příkaz toodeploy hello Resource Manager šablony a parametr soubory toocreate hello cluster Service Fabric hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Nasadit službu API Management

Nakonec nasazení API Management toohello virtuální sítě v podsíti hello a NSG určená pro API Management. Není nutné toowait pro toofinish nasazení clusteru Service Fabric hello před nasazením API Management. 

V tomto kurzu hello rozhraní API správy Resource Manager šablona je předem nakonfigurovaný toouse hello názvy hello virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku hello. 

 1. Stáhněte si následující soubor šablony a parametry Resource Manager hello:
 
    - [APIM.JSON][apim-arm]
    - [APIM.Parameters.JSON][apim-parameters-arm]

 2. Vyplňte hello prázdné parametry v hello `apim.parameters.json` pro vaše nasazení.

 3. Použijte následující prostředí PowerShell příkaz toodeploy hello Resource Manager šablony a parametr soubory pro API Management hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>Konfigurace správy rozhraní API

Jakmile cluster API Management a Service Fabric se nasazuje, můžete nakonfigurovat back-end Service Fabric ve službě API Management. To vám umožní toocreate zásadu služby back-end, která odešle cluster Service Fabric tooyour provoz.

### <a name="api-management-security"></a>Zabezpečení rozhraní API Management

tooconfigure hello Service Fabric back-end, musíte nejdřív nastavení zabezpečení tooconfigure API Management. nastavení zabezpečení tooconfigure, přejděte tooyour rozhraní API správy služby v hello portálu Azure.

#### <a name="enable-hello-api-management-rest-api"></a>Povolit hello rozhraní API REST API pro správu

Hello rozhraní API REST API pro správu je aktuálně hello pouze způsob tooconfigure back-end službu. prvním krokem Hello je tooenable hello rozhraní API REST API pro správu a zabezpečte ji.

 1. V hello služba API Management, vyberte **rozhraní API pro správu - PREVIEW** pod **zabezpečení**.
 2. Zkontrolujte hello **povolit rozhraní API REST API pro správu** zaškrtávací políčko.
 3. Poznámka: adresa URL rozhraní API správy hello – Toto je adresa URL hello použijeme novější tooset až hello Service Fabric back-end
 4. Generovat **přístupový Token** výběrem datum vypršení platnosti a klíč klikněte hello **generování** tlačítko směrem k hello dolní části stránky hello.
 5. Kopírování hello **přístupový token** a uložte ho – použijeme ho v hello následující kroky. Všimněte si, že se to neliší od hello primární klíč a sekundární klíč.

#### <a name="upload-a-service-fabric-client-certificate"></a>Nahrajte certifikát klienta Service Fabric

API Management musí ověřit k vašemu clusteru Service Fabric pro zjišťování služby pomocí klientského certifikátu, který má přístup tooyour clusteru. Tento kurz používá pro jednoduchost, hello stejný certifikát zadaný při vytváření clusteru Service Fabric hello, který ve výchozím nastavení může být použité tooaccess vašeho clusteru.

 1. V hello služba API Management, vyberte **klientské certifikáty - PREVIEW** pod **zabezpečení**.
 2. Klikněte na tlačítko hello **+ přidat** tlačítko
 2. Vyberte hello soubor privátního klíče (.pfx) hello clusteru certifikátu, který jste zadali při vytváření clusteru Service Fabric, zadejte jeho název a zadejte heslo soukromého klíče hello.

> [!NOTE]
> Tento kurz používá hello stejný certifikát pro klienta ověřování a clusteru mezi uzly zabezpečení. Pokud máte jeden nakonfigurované tooaccess cluster Service Fabric můžete použít samostatný klientský certifikát.

### <a name="configure-hello-backend"></a>Konfigurace back-end hello

Teď, když je nakonfigurovaná zabezpečení rozhraní API Management, můžete nakonfigurovat hello Service Fabric back-end. Pro Service Fabric back-EndY je cluster Service Fabric hello hello back-end, nikoli konkrétní službu Service Fabric. To umožňuje jedna zásada tooroute toomore než jedna služba v clusteru hello.

Tento krok vyžaduje hello přístupový token, který jste vygenerovali dříve a hello kryptografický otisk certifikátu vašeho clusteru, že jste nahráli tooAPI správy v předchozím kroku hello.

> [!NOTE]
> Pokud samostatný klientský certifikát se používá v předchozím kroku hello pro API Management, musíte hello kryptografický otisk certifikátu klienta hello v přidání toohello clusteru kryptografický otisk certifikátu v tomto kroku.

Odešlete hello následující požadavek HTTP PUT toohello rozhraní API správy rozhraní API adresu URL jste si předtím poznamenali při povolování hello rozhraní API REST API správy tooconfigure hello Service Fabric back-end službu. Měli byste vidět `HTTP 201 Created` odpovědi při úspěšném provedení příkazu hello. Další informace o každé pole, najdete v části hello API Management [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

Příkaz HTTP a adresy URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Hlavičky požadavku:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Text žádosti:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

Hello **url** parametr tady je služba plně kvalifikovaný název služby v clusteru, jsou všechny požadavky směrovány tooby výchozí, pokud není zadán žádný název služby v zásadách back-end. Můžete použít název falešných služby, jako například "fabric: / falešných/služby" Pokud nemáte v úmyslu toohave záložní služby.

Odkazovat toohello API Management [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) další podrobnosti o každé pole.

#### <a name="example"></a>Příklad

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Nasazení služby back-end Service Fabric

Teď, když máte hello Service Fabric nakonfigurovat jako back-end tooAPI správy, můžete vytvořit zásady back-end pro vaše rozhraní API, která odesílat provoz služby tooyour Service Fabric. Ale nejdřív je potřeba služby spuštěné v Service Fabric tooaccept požadavky.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Vytvoření služby Service Fabric se koncový bod HTTP

V tomto kurzu vytvoříme základní bezstavové ASP.NET Core spolehlivé služby pomocí hello výchozí šablona projektu webového rozhraní API. Tím se vytvoří koncový bod HTTP pro vaši službu, která budete vystavit pomocí Azure API Management:

```
/api/values
```

Začněte tím, že [nastavení vývojového prostředí pro vývoj ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Až nastavíte vývojového prostředí Visual Studio spustíte jako správce a vytvoření služby ASP.NET Core:

 1. V sadě Visual Studio vyberte soubor -> Nový projekt.
 2. Vyberte šablonu hello aplikace Service Fabric v cloudu a pojmenujte ji **"ApiApplication"**.
 3. Vyberte hello ASP.NET Core šablony služby a název projektu hello **"WebApiService"**.
 4. Výběr šablony projektu hello webové rozhraní API ASP.NET Core 1.1.
 5. Po vytvoření projektu hello otevřete `PackageRoot\ServiceManifest.xml` a odeberte hello `Port` atribut z hello koncový bod prostředků konfigurace:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    To umožňuje Service Fabric toospecify port dynamicky z rozsahu portů aplikace hello, který jsme otevřít prostřednictvím hello skupinu zabezpečení sítě v modulu snap-in Šablony Resource Manageru clusteru hello povolení tooit tooflow provoz z API Management.
 
 6. Stisknutím klávesy F5 ve Visual Studio tooverify hello webového rozhraní API není k dispozici místně. 

    Otevřete Service Fabric Explorer a rozbalení tooa konkrétní instanci hello ASP.NET Core toosee hello základní adresa hello služby naslouchá na. Přidat `/api/values` toohello základní adresa a otevřete v prohlížeči. Tím se spustí hello metodu Get na hello ValuesController v šabloně hello webového rozhraní API. Vrátí hello výchozí odpovědi, které poskytuje hello šablony, pole JSON, který obsahuje dva řetězce:

    ```json
    ["value1", "value2"]`
    ```

    Toto je hello koncový bod, který budete vystavit přes správu rozhraní API v Azure.

 7. Nakonec nasazení clusteru tooyour hello aplikace v Azure. [Pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), klikněte pravým tlačítkem na projekt aplikace hello a vyberte **publikovat**. Zadejte svůj koncový bod clusteru (například `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello aplikace tooyour Service Fabric cluster v Azure.

Bezstavové služby ASP.NET Core s názvem `fabric:/ApiApplication/WebApiService` teď by měl být spuštěn v clusteru Service Fabric v Azure.

## <a name="create-an-api-operation"></a>Vytvoření operace rozhraní API

Teď máme připraven toocreate operace ve službě API Management hello toocommunicate použití tohoto externích klientů s běžící v clusteru Service Fabric hello bezstavové služby ASP.NET Core.

 1. Přihlaste se toohello portál Azure a přejděte tooyour nasazení služby API Management.
 2. V okně služby API Management hello vyberte **rozhraní API – náhled**
 3. Přidání nového rozhraní API kliknutím hello **prázdné API** pole a vyplníte dialogové okno hello:

     - **Adresu URL webové služby**: pro Service Fabric back-EndY, není tato hodnota adresy URL používá. Zde můžete zadejte libovolnou hodnotu. V tomto kurzu použít: `http://servicefabric`.
     - **Název**: zadat libovolný název pro rozhraní API. V tomto kurzu použít `Service Fabric App`.
     - **Schéma adresy URL**: Vyberte HTTP, HTTPS nebo obě možnosti. V tomto kurzu použít `both`.
     - **Přípona adresy URL rozhraní API**: Zadejte příponu pro naše rozhraní API. V tomto kurzu použít `myapp`.
 
 4. Po vytvoření hello rozhraní API klikněte na tlačítko **+ operace přidání** operace tooadd front-endové rozhraní API. Vyplňte hodnoty hello:
    
     - **Adresa URL**: vyberte `GET` a zadejte cestu adresy URL pro hello rozhraní API. V tomto kurzu použít `/api/values`.
     
       Ve výchozím nastavení zadat cestu adresy URL hello tady se cesty URL hello odesílají toohello back-end Service Fabric službu. Pokud používáte hello stejné cesty URL, zde kterou používá službu, v takovém případě `/api/values`, pak operaci hello funguje bez další úpravy. Můžete také určit cestu adresy URL tady, se liší od cesty URL hello používá váš back-end službu Service Fabric, v takovém případě budete také toospecify nutnost přepisu cestu v zásadě operaci později.
     - **Zobrazovaný název**: zadat libovolný název pro rozhraní API hello. V tomto kurzu použít `Values`.

## <a name="configure-a-backend-policy"></a>Nakonfigurujte zásady back-end

zásady back-end Hello sváže všechno, co společně. Toto je, kde konfigurujete hello back-end Service Fabric, které směrují požadavky toowhich služby. Můžete provést tuto operaci zásad tooany rozhraní API. Hello [konfiguraci back-end pro Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) poskytuje následujících hello požadavku směrování ovládací prvky: 
 - Služba instance výběru zadáním Service Fabric název instance služby, buď pevně zakódované (například `"fabric:/myapp/myservice"`) nebo vygenerovat z požadavku hello HTTP (například `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Oddíl rozlišení vygenerováním klíč oddílu pomocí žádné schéma rozdělení oddílů Service Fabric.
 - Výběr repliky pro stavové služby.
 - Řešení opakujte podmínky, které vám umožňují toospecify hello podmínky pro přeložit umístění služby a odešlete žádost.

V tomto kurzu vytvořte zásadu back-end trasy požadavky přímo toohello ASP.NET Core bezstavové služby dříve nasazené:

 1. Vyberte a upravit hello **příchozí zásady** pro hello `Values` operaci kliknutím na ikonu pro úpravu hello, a potom výběrem **zobrazení kódu**.
 2. V editoru kódu hello zásady, přidejte `set-backend-service` zásad v části příchozí zásady, jak je vidět tady a klikněte na tlačítko hello **Uložit** tlačítko:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

Úplnou sadu atributů Service Fabric back-end zásady, najdete v části toohello [dokumentace ke službě back endové rozhraní API Management](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-hello-api-tooa-product"></a>Přidání produktu tooa hello rozhraní API. 

Než bude možné volat rozhraní API hello, musí být přidané tooa produktu, kde můžete udělit přístup toousers. 

 1. V hello služba API Management, vyberte **produkty - PREVIEW**.
 2. Ve výchozím nastavení, produkty dva poskytovatelé pro správu rozhraní API: úvodní a bez omezení. Vyberte hello neomezená produktu.
 3. Vyberte rozhraní API.
 4. Klikněte na tlačítko hello **+ přidat** tlačítko.
 5. Vyberte hello `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích hello a klikněte na tlačítko hello **vyberte** tlačítko.

### <a name="test-it"></a>otestovat

Nyní můžete zkusit odesílání požadavku tooyour back-end služby v Service Fabric pomocí rozhraní API správy přímo z hello portálu Azure.

 1. V hello služba API Management, vyberte **API – PREVIEW**.
 2. V hello `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích hello vyberte hello **Test** kartě.
 3. Klikněte na tlačítko hello **odeslat** toosend tlačítko test požadavek toohello back-end službu.

## <a name="next-steps"></a>Další kroky

V tomto okamžiku byste měli mít základní instalaci pomocí Service Fabric a API Management.

Tento kurz používá ověřování basic uživatele na základě certifikátů pro váš tooget clusteru Service Fabric, kterou vytvoříte rychle. Pokročilejší ověřování uživatelů pro váš cluster Service Fabric je vhodnější s [ověřování Azure Active Directory](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Dále [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare aplikace pro provoz skutečném světě.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
