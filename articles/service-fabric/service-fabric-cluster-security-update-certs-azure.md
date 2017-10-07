---
title: "certifikáty aaaManage v clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Popisuje, jak tooadd nové certifikáty, certifikát výměny a odebrat certifikát tooor z clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: 8e57bd95dbb800ecc04cf6988047e3abdc2fe56a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Přidat nebo odebrat certifikáty pro cluster Service Fabric v Azure
Doporučujeme Seznamte se s jak Service Fabric používá certifikáty X.509 a znát hello [clusteru scénáře zabezpečení](service-fabric-cluster-security.md). Je potřeba pochopit, jaké certifikát clusteru je a co se používá, před pokračováním.

Služby prostředků infrastruktury umožňuje zadat, že dva clusteru certifikáty, primárního a sekundárního, při konfiguraci certifikátů zabezpečení při vytváření clusteru v přidání tooclient certifikáty. Odkazovat příliš[vytváření clusteru služby azure přes portál](service-fabric-cluster-creation-via-portal.md) nebo [vytváření clusteru služby azure pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) pro podrobnosti o jejich nastavení na vytvořit čas. Pokud zadáte jenom jeden certifikát clusteru na doba pro vytvoření, pak který slouží jako primárního certifikátu hello. Po vytvoření clusteru můžete přidat nového certifikátu jako sekundární.

> [!NOTE]
> Pro cluster s podporou zabezpečení budete vždy potřebovat alespoň jeden platný clusteru (není odvolaný a ne jeho platnost) certifikát (primární nebo sekundární) nasazené (Pokud ne, přestane hello cluster fungovat). 90 dní před vypršení platnosti, dosáhnout všechny platné certifikáty hello systém vygeneruje upozornění trasování a také událost stavu upozornění na uzlu hello. Není aktuálně žádné e-mailu nebo všechna oznámení, která odesílá service fabric, v tomto tématu. 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-hello-portal"></a>Přidat certifikát sekundární clusteru pomocí portálu hello

Certifikát sekundární clusteru nelze přidat prostřednictvím hello portálu Azure. Máte toouse Azure powershell, pro který. proces Hello popsané dál v tomto dokumentu.

## <a name="swap-hello-cluster-certificates-using-hello-portal"></a>Vyměnit certifikáty hello clusteru pomocí portálu hello

Po úspěšně jste nasadili certifikát sekundární clusteru, pokud chcete tooswap hello primárního a sekundárního, pak přejděte okno toohello zabezpečení a vyberte možnost "Prohození s primární" hello z hello kontextové nabídky tooswap hello sekundární certifikátu s primární cert Hello.

![Swap certifikátu][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-hello-portal"></a>Odebrat certifikát clusteru pomocí portálu hello

Pro cluster s podporou zabezpečení budete vždy potřebovat alespoň jeden platný (není odvolaný a ne jeho platnost) certifikát (primární nebo sekundární) nasazené v opačném případě hello clusteru přestane fungovat.

tooremove sekundární certifikát se používá k zabezpečení clusteru, přejděte toohello zabezpečení okno a vyberte hello, odstranit, možnost hello místní nabídce na sekundární certifikátu hello.

Pokud vaše záměrem tooremove hello certifikát, který je označen jako primární, bude nutné tooswap její hello sekundární nejprve a pak odstraňte hello sekundární po dokončení upgradu hello.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Přidat sekundární certifikát pomocí Správce prostředků Powershell

Tento postup předpokládá se seznámíte s fungování Resource Manager a nasadili alespoň jeden pomocí šablony Resource Manageru cluster Service Fabric a hello šablona, kterou jste použili tooset až hello clusteru užitečné. Taky se předpokládá, že umíte pomocí JSON.

> [!NOTE]
> Pokud hledáte vzorové šablony a parametry, které můžete použít toofollow společně nebo jako výchozí bod, poté ji stáhnout z tohoto [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Upravte svou šablonu Resource Manager

Pro usnadnění následující společně obsahuje ukázkové 5-VM-1-NodeTypes-Secure_Step2.JSON všechny hello úpravy, které budeme provádět. Hello ukázka je dostupná v [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Ujistěte se, že toofollow všechny kroky hello**

**Krok 1:** otevřete šablony Resource Manageru hello používá toodeploy můžete clusteru. (Pokud jste si stáhli hello ukázka z hello výše úložiště, pak použít 5-VM-1-NodeTypes-Secure_Step1.JSON toodeploy zabezpečení clusteru a pak otevřete si tato šablona).

**Krok 2:** přidat **dva nové parametry** "secCertificateThumbprint" a "secCertificateUrlValue" typu "string" toohello části parametr šablony. Můžete zkopírovat hello následující fragment kódu a přidejte ho toohello šablony. V závislosti na hello zdroj šablony mohou už tyto definované, pokud ano, přesuňte toohello další krok. 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

**Krok 3:** provádět změny toohello **Microsoft.ServiceFabric/clusters** prostředku, vyhledejte definice prostředků hello "Microsoft.ServiceFabric/clusters" v šabloně. V části Vlastnosti definice zjistíte "Certifikát" JSON značku, která by měla vypadat podobně jako hello následujícím fragmentu kódu JSON:

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Přidat novou značku "thumbprintSecondary" a dejte mu hodnotu "[parameters('secCertificateThumbprint')]".  

Ano, teď hello definice prostředků by měl vypadat jako následující hello (v závislosti na vaší zdroje hello šablony, nemusí být úplně stejně jako hello fragment kódu níže). 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Pokud chcete příliš**výměny hello cert**, pak zadejte hello nového certifikátu jako primární a přesunutí hello aktuální primární jako sekundární. Výsledkem hello výměna vaše aktuální primární certifikát toohello nový certifikát v jednom kroku nasazení.

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


**Krok 4:** provádět změny příliš**všechny** hello **Microsoft.Compute/virtualMachineScaleSets** definice prostředků - vyhledání prostředků Microsoft.Compute/virtualMachineScaleSets hello definice. Posuňte se toohello "vydavatel": "Microsoft.Azure.ServiceFabric" v části "virtualMachineProfile".

V nastavení hello služby fabric vydavatele měli byste vidět zhruba takhle.

![Json_Pub_Setting1][Json_Pub_Setting1]

Přidat hello tooit položky nového certifikátu.

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

Vlastnosti Hello by teď měl vypadat takto

![Json_Pub_Setting2][Json_Pub_Setting2]

Pokud chcete příliš**výměny hello cert**, pak zadejte hello nového certifikátu jako primární a přesunutí hello aktuální primární jako sekundární. Výsledkem hello výměna vaše aktuální certifikát toohello nový certifikát v jednom kroku nasazení. 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
Vlastnosti Hello by teď měl vypadat takto

![Json_Pub_Setting3][Json_Pub_Setting3]


**Krok 5:** změnit příliš**všechny** hello **Microsoft.Compute/virtualMachineScaleSets** definice prostředků - vyhledání prostředků Microsoft.Compute/virtualMachineScaleSets hello definice. Posuňte se toohello "vaultCertificates":, v části "OSProfile". by měla vypadat přibližně takto.


![Json_Pub_Setting4][Json_Pub_Setting4]

Přidejte hello secCertificateUrlValue tooit. Použijte hello následující fragment kódu:

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
Nyní hello výsledná Json by měla vypadat přibližně takto.
![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Ujistěte se, že obsahovat opakované kroky 4 a 5 pro všechny definice prostředků Nodetypes/Microsoft.Compute/virtualMachineScaleSets hello ve vaší šabloně. Pokud jeden z nich přeskočíte, nebude získat hello certifikát nainstalovaný na tomto VMSS a budete mít vést k neočekávaným výsledkům v clusteru, včetně hello clusteru směrem dolů (Pokud skončili žádné platné certifikáty, že Hello clusteru můžete použít pro zabezpečení. Proto prosím Překontrolujte, než budete pokračovat.
> 
> 


### <a name="edit-your-template-file-tooreflect-hello-new-parameters-you-added-above"></a>Upravit šablonu soubor tooreflect hello nové parametry, které jste přidali výše
Pokud používáte ukázku hello z hello [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow hotová, můžete spustit toomake změny v hello ukázka 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON 

Upravit daný parametr šablony Resource Manageru soubor, přidejte dva nové parametry pro secCertificateThumbprint a secCertificateUrlValue hello. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-hello-template-tooazure"></a>Nasazení šablony tooAzure hello

- Můžete je nyní připraven toodeploy tooAzure vaší šablony. Otevřete příkazový řádek se verze 1 + Azure PS.
- Přihlaste tooyour účet Azure a vybrat konkrétní předplatné azure hello. Toto je důležitý krok pro zaměstnance, kteří mají přístup toomore než jedno předplatné.

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Testování hello šablony předchozí toodeploying ho. Použití hello stejnou skupinu prostředků, který váš cluster je aktuálně nasazený do.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Nasazení skupiny prostředků tooyour hello šablony. Použití hello stejnou skupinu prostředků, který váš cluster je aktuálně nasazený do. Spuštěním příkazu New-AzureRmResourceGroupDeployment hello. Není nutné toospecify hello režimu, protože hello výchozí hodnota je **přírůstkové**.

> [!NOTE]
> Pokud jste nastavili tooComplete režimu, můžete nechtěně odstranit prostředky, které nejsou ve vaší šabloně. Nepoužívejte ho v tomto scénáři.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Tady je vyplnění si příklad hello stejné prostředí powershell.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

Po dokončení nasazení hello připojit pomocí clusteru tooyour hello nový certifikát a provádět některé dotazy. Pokud jste možnost toodo. Potom můžete odstranit hello starý certifikát. 

Pokud používáte certifikát podepsaný svým držitelem, nevynechali tooimport je do místního úložiště certifikátů TrustedPeople.

```powershell
######## Set up hello certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Pro rychlou referenci tady je hello příkaz tooconnect tooa zabezpečení clusteru 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Pro rychlou referenci tady je hello příkaz tooget clusteru stavu

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-toohello-cluster"></a>Nasazení clusteru toohello certifikáty aplikace.

Můžete použít stejné kroky, jak je uvedeno v kroky 5 výše toohave hello certifikáty z keyvault toohello uzly nasazena hello. Stačí nutné definovat a použít jiné parametry.


## <a name="adding-or-removing-client-certificates"></a>Přidání nebo odebrání klientských certifikátů

V modulu snap-in Certifikáty clusteru toohello přidání můžete přidat klientské certifikáty tooperform management operace na service fabric cluster.

Můžete přidat dva druhy klientské certifikáty - správce nebo jen pro čtení. To poté mohou být operace správce toohello použité toocontrol přístup a operace dotazů na clusteru hello. Ve výchozím nastavení certifikáty clusteru hello se přidají toohello správce certifikátů seznamu povolených aplikací.

můžete zadat libovolný počet klientských certifikátů. Každý přidávání a odstraňování výsledkem cluster konfigurace aktualizace toohello service fabric


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>Přidání klientské certifikáty - správce nebo jen pro čtení přes portál

1. Přejděte okno toohello zabezpečení a vyberte hello '+ ověřování' tlačítka v okně zabezpečení hello.
2. V okně hello "přidat ověřování zvolte hello"Ověřování typu"- 'jen pro čtení klienta' nebo 'správce klienta.
3. Teď zvolte metoda autorizace hello. To znamená tooService prostředků infrastruktury, zda se má tento certifikát vyhledávání na základě hello název subjektu nebo miniaturu hello. Obecně platí není dobře zabezpečená hello toouse postupem autorizační metoda název subjektu. 

![Přidání certifikátu klienta][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-hello-portal"></a>Odstranění klientské certifikáty - správce nebo jen pro čtení pomocí hello portálu

tooremove sekundární certifikát od používá pro zabezpečení clusteru, přejděte toohello zabezpečení okno a vyberte hello 'Delete' z kontextové nabídky hello na hello konkrétní certifikát.



## <a name="next-steps"></a>Další kroky
Přečtěte si tyto články pro další informace o správě clusteru:

* [Proces upgradu Service Fabric Cluster a očekávání od vás](service-fabric-cluster-upgrade.md)
* [Instalační program přístup na základě rolí pro klienty](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


