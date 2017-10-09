---
title: "aaaNetwork přehled zprostředkovatele prostředků | Microsoft Docs"
description: "Další informace o hello nového poskytovatele síťových prostředků ve službě Správce prostředků Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 81b8f51fe8ee180d8f7885c6e04eb953904d7be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-resource-provider"></a>Poskytovatel síťových prostředků
Podpůrný potřeba v dnešních obchodní úspěch, je hello možnost toobuild a spravovat aplikace využívající rozsáhlé sítě agilní, flexibilní, zabezpečení a opakovatelných způsobem. Azure Resource Manager umožňuje toocreate můžete takové aplikace jako jedinou kolekci prostředků do skupiny prostředků. Tyto prostředky se spravují prostřednictvím různých poskytovatelů prostředků v části správce prostředků.

Azure Resource Manager závisí na různých prostředků zprostředkovatelé tooprovide přístup tooyour prostředky. Existují tři hlavní prostředků zprostředkovatelé: sítě, úložiště a výpočty. Tento dokument popisuje hello charakteristiky a výhod hello poskytovatele síťových prostředků, včetně:

* **Metadata** – můžete přidat informace tooresources pomocí značek. Tyto značky mohou být použité tootrack využití prostředků v rámci skupiny prostředků a předplatná.
* **Větší kontrolu nad síti** – síťové prostředky jsou volně vázány a je mohli řídit podrobnější způsobem. To znamená, že máte větší flexibilitu při správě síťových prostředků hello.
* **Rychlejší konfigurace** -vzhledem k tomu, že jsou volně vázány síťovým prostředkům, můžete vytvořit a orchestraci síťové prostředky paralelně. To má výrazně snížit čas konfigurace.
* **Řízení přístupu na základě role** -RBAC poskytuje výchozí role, s konkrétní bezpečnostní obor v přidání tooallowing hello vytvoření vlastních rolí zabezpečení správy.
* **Jednodušší správu a nasazení** -je snazší toodeploy a spravovat aplikace, protože můžete můžete vytvořit celý zásobník aplikací jako jedinou kolekci prostředků ve skupině prostředků. A rychlejší toodeploy, protože můžete nasadit jednoduše poskytnutím datové části JSON šablony.
* **Rychlé přizpůsobení** – můžete použít vlastní opakovatelných a rychlé tooenable deklarativní stylu šablony nasazení.
* **Přizpůsobení opakovatelných** – můžete použít vlastní opakovatelných a rychlé tooenable deklarativní stylu šablony nasazení.
* **Rozhraní pro správu** -pomocí některé z následujících rozhraní toomanage hello vaše prostředky:
  * REST API založené na
  * PowerShell
  * .NET SDK
  * Node.JS SDK
  * Java SDK
  * Azure CLI
  * Portál Preview
  * Jazyk šablony Resource Manageru

## <a name="network-resources"></a>Síťové prostředky
Teď můžete spravovat síťovým prostředkům nezávisle, místo nutnosti je spravovat prostřednictvím jednoho výpočetních prostředků (virtuální počítač). Tím se zajistí vyšší stupeň pružnost a přizpůsobivost v skládání komplexní a velké škálování infrastruktury ve skupině prostředků.

Koncepční zobrazení nasazení ukázkové zahrnující víceúrovňových aplikací je uveden níže. Každý prostředek, který se zobrazí, jako jsou síťové adaptéry, veřejné IP adresy a virtuálních počítačů, lze spravovat nezávisle.

![Model prostředků sítě](./media/resource-groups-networking/Figure2.png)

Každý prostředek obsahuje společnou sadu vlastností a jejich jednotlivé vlastnost nastavit. běžné vlastnosti Hello jsou:

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **Jméno** |Název prostředku jedinečné. Každý typ prostředku má svou vlastní omezení pro pojmenovávání. |NIC01 PIP01, VM01, |
| **location** |Oblast Azure, ve které hello nachází prostředků |westus eastus |
| **ID** |Jedinečný identifikátor URI na základě identifikace |/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP |

Můžete zkontrolovat hello jednotlivé vlastnosti prostředků v následujících částech hello.

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Rozhraní pro správu
Můžete spravovat pomocí různých rozhraní prostředků Azure sítě. V tomto dokumentu se zaměříme na kabely o těchto rozhraní: rozhraní API REST a šablony.

### <a name="rest-api"></a>REST API
Jak už bylo zmíněno dříve, je možné spravovat síťovým prostředkům prostřednictvím různých rozhraní, včetně REST API, sady .NET SDK, Node.JS SDK, sady Java SDK, prostředí PowerShell, rozhraní příkazového řádku, portálu Azure a šablony.

rozhraní Rest API Hello požadavkům toohello specifikace protokolu HTTP 1.1. Hello obecné URI struktura hello rozhraní API je uveden níže:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

A parametry hello do složených závorek představují hello následující prvky:

* **id předplatného** -svoje id předplatného Azure.
* **– obor názvů zprostředkovatele prostředků** – obor názvů pro používaný zprostředkovatel hello. Hodnota Hello poskytovatele síťových prostředků hello je *Microsoft.Network*.
* **Název oblasti** – název oblasti Azure hello

Hello následující metody HTTP jsou podporovány při provádění toohello volání rozhraní REST API:

* **UVEĎTE** – použité toocreate prostředek daného typu, upravte vlastnosti prostředku nebo změnit přidružení mezi prostředky.
* **ZÍSKAT** – používá informace tooretrieve zřízené prostředku.
* **Odstranit** -použít toodelete existující prostředek.

Hello požadavku a odpovědi požadavkům tooa Formát datové části JSON. Další podrobnosti najdete v tématu [rozhraní API pro správu prostředků Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="resource-manager-template-language"></a>Jazyk šablony Resource Manageru
V přidání toomanaging prostředky imperativní (přes rozhraní API nebo sady SDK) můžete také použít deklarativní programování toobuild styl a spravovat síťovým prostředkům pomocí hello jazyk šablony Resource Manageru.

Ukázka reprezentace šablony najdete níže –

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Šablona Hello je primárně popis JSON hello prostředků a hodnoty instance hello vložit prostřednictvím parametrů. Následující příklad Hello lze použít toocreate virtuální síť s 2 podsítě.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Máte možnost hello ručně zadáním hodnot parametru hello při použití šablony, nebo můžete použít soubor s parametry. Následující příklad Hello ukazuje možné sadu toobe hodnoty parametru použít s hello šablony výše:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Hello hlavní výhody použití šablony jsou:

* Můžete vytvořit komplexní infrastruktury ve skupině prostředků v deklarativní stylu. Dobrý den orchestraci vytváření hello prostředků, včetně správy závislostí, zpracovává pomocí Správce prostředků.
* Hello infrastruktury vytvořením opakovatelných způsobem v různých oblastech a v oblasti jednoduše změnou parametry.
* deklarativní styl Hello vede doba realizace tooshorter ve vytváření šablony hello a zavádí hello infrastruktury.

Ukázka šablony, najdete v části [šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).

Další informace o hello jazyk šablony Resource Manageru, najdete v části [jazyk šablony Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

výše uvedené šablony ukázka Hello používá hello virtuální síť a podsíť prostředky. Existují jiným síťovým prostředkům, které můžete použít jak je uvedeno dále:

### <a name="using-a-template"></a>Pomocí šablony
TooAzure služby ze šablony můžete nasadit pomocí prostředí PowerShell, AzureCLI, nebo provedením toodeploy klikněte na z webu GitHub. toodeploy služby ze šablony v Githubu, spusťte hello následující kroky:

1. Otevřete soubor template3 hello z Githubu. Jako příklad, otevřete [virtuální síť se dvěma podsítěmi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klikněte na **nasazení tooAzure**a potom se přihlaste na toohello portálu Azure pomocí svých přihlašovacích údajů.
3. Ověření hello šablony a potom klikněte na **Uložit**.
4. Klikněte na tlačítko **upravit parametry** a vyberte umístění, jako například *západní USA*pro hello virtuální sítě a podsítě.
5. V případě potřeby změňte hello **ADDRESSPREFIX** a **SUBNETPREFIX** parametry a pak klikněte na tlačítko **OK**.
6. Klikněte na tlačítko **vyberte skupinu prostředků** a potom klikněte na skupinu prostředků hello chcete tooadd hello virtuální sítě a podsítě, které. Alternativně můžete vytvořit novou skupinu prostředků kliknutím **nebo vytvořit nový**.
7. Klikněte na možnost **Vytvořit**. Všimněte si dlaždice zobrazení hello **nasazení zřizování šablony**. Po dokončení nasazení hello, zobrazí se dole podobné tooone obrazovky.

![Ukázka nasazení šablony](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a>Další kroky
[Jazyk šablony Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure sítě – běžně používané šablony](https://github.com/Azure/azure-quickstart-templates)

[Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md)

[Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)

