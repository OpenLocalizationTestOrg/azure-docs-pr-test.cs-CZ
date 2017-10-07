---
title: "komplexní hodnoty aaaPass mezi šablony Azure | Microsoft Docs"
description: "Zobrazí doporučená přístupy k pomocí šablony Azure Resource Manager a propojená data o stavu tooshare komplexních objektů."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: 72df1dee351446cea6ce15269e6db288b1f1db79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="share-state-tooand-from-azure-resource-manager-templates"></a>Sdílená složka stavu tooand ze šablon Azure Resource Manager
Toto téma ukazuje osvědčené postupy pro správu a sdílení stavu v rámci šablony. Hello parametry a proměnné, které jsou uvedené v tomto tématu jsou příklady typu hello objektů můžete definovat tooconveniently uspořádání požadavků vašeho nasazení. V těchto příkladech můžete implementovat vlastní objekty s hodnoty vlastností, které dávají smysl pro vaše prostředí.

Toto téma je součástí větší dokument White Paper. Stáhnout tooread hello úplné papír [World Třída prostředků Manager šablony požadavky a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Zadejte standardní konfigurace nastavení
Místo nabízí šablonu, která poskytuje celkový flexibilitu a obrovském množství variant, je běžné vzor tooprovide výběr známé konfigurace. V důsledku toho mohou uživatelé vybrat standardní velikosti tričko například izolovaného prostoru, malé, střední a velké. Další příklady tričko velikosti jsou nabídky produktů, jako je například community edition nebo enterprise edition. V jiných případech může být určeného pro konkrétní úlohy konfigurace technologii – například mapy snížit nebo žádné sql.

S komplexních objektů můžete vytvářet proměnné, které obsahují kolekcí dat, někdy označovanou jako "kontejnery vlastností" a použít tuto deklaraci data toodrive hello prostředků ve vaší šabloně. Tento přístup poskytuje dobrý, známé konfigurace různou velikost předem konfigurovaných pro zákazníky. Bez známé konfigurace, musí uživatelé hello šablony určit velikost clusteru na své vlastní, faktorem omezení prostředků platformy a provádět matematické tooidentify hello výsledná oddíly účty úložiště a dalším prostředkům (z důvodu toocluster velikost a omezení zdrojů). Kromě toho toomaking lepší prostředí pro zákazníka hello několik známé konfigurace jsou jednodušší toosupport a mohou vám pomoci poskytovat vyšší úroveň hustotu.

Následující příklad ukazuje, jak Hello toodefine proměnné, které obsahují pro představující kolekce dat na komplexní objekty. kolekce Hello definovat hodnoty, které se používají pro velikost virtuálního počítače, nastavení sítě, nastavení operačního systému a nastavení dostupnosti.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Všimněte si, že hello **tshirtSize** zřetězí velikost trička hello zadaný pomocí parametru (**malé**, **střední**, **velké**) toohello text **tshirtSize**. Tato proměnná proměnné tooretrieve hello přidružené komplexní objekt se používá pro tato velikost tričko.

Pak můžete odkazovat tyto proměnné později v šabloně hello. zjednodušuje hello syntaxi šablony a umožňuje snadno toounderstand kontextu, Hello možnost tooreference s názvem proměnné a jejich vlastnosti. Následující ukázka Hello definuje toodeploy prostředků pomocí objektů hello uvedený výše tooset hodnoty. Například je hello velikost virtuálního počítače nastavit načtením hello hodnotu `variables('tshirtSize').vmSize` při hello hodnotu pro velikost disku hello se načítají z `variables('tshirtSize').diskSize`. Kromě toho hello URI propojené šablony je nastavený s hodnotou hello `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-tooa-template"></a>Předat stavu tooa šablony
Sdílíte stavu do šablony prostřednictvím parametrů, které poskytují přímo během nasazení.

Hello následující parametry tabulky seznamy běžně používané v šablonách.

| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| location |Řetězec z omezené seznam oblastí Azure |Hello umístění, kde jsou nasazené prostředky hello. |
| storageAccountNamePrefix |Řetězec |Jedinečný název DNS pro hello účet úložiště, kde jsou umístěné disky hello Virtuálního počítače |
| domainName |Řetězec |Název domény hello veřejně přístupná jumpbox virtuálních počítačů ve formátu hello: **{domainName}. { umístění}.cloudapp.com** například: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Řetězec |Uživatelské jméno pro hello virtuální počítače |
| adminPassword |Řetězec |Heslo pro hello virtuální počítače |
| tshirtSize |Řetězec, ze seznamu omezené nabízí tričko velikosti |Hello s názvem tooprovision velikost jednotky škálování. Například "Malá", "střední", "Velké" |
| virtualNetworkName |Řetězec |Název hello virtuální síť, která hello příjemce chce toouse. |
| enableJumpbox |Řetězec omezené seznamu (povoleno nebo zakázáno) |Parametr, který identifikuje zda tooenable jumpbox hello prostředí. Hodnoty: "povoleno", "Zakázat" |

Hello **tshirtSize** parametr použitý v předchozí části hello je definován jako:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of hello MongoDB deployment"
        }
      }
    }


## <a name="pass-state-toolinked-templates"></a>Předat stavu toolinked šablony
Při připojování toolinked šablony, můžete často použít kombinaci statických a vygeneruje proměnné.

### <a name="static-variables"></a>Statické proměnné
Statické proměnné jsou často používané tooprovide základní hodnoty, například adresy URL, které se používají v rámci šablony.

V následující výňatek ze šablony, hello `templateBaseUrl` určuje hello kořenový adresář pro šablonu hello v Githubu. Další řádek Hello vytvoří novou proměnnou `sharedTemplateUrl` který zřetězí hello základní adresu URL s hello známý název šablony hello sdílených prostředků. Níže daného řádku proměnnou komplexní objekt je použité toostore velikost trička, kde základní adresu URL hello zřetězených toohello označuje umístění konfigurace šablon a uloženy v hello `vmTemplate` vlastnost.

Hello výhody tohoto přístupu je, že pokud se změní umístění šablon hello pouze toochange hello statické proměnné na jednom místě, které vyhovují v rámci hello propojené šablony.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Vygenerovaný proměnné
Několik proměnné přidání toostatic proměnné, jsou generována dynamicky. Tato část popisuje některé běžné typy hello generovaného proměnných.

#### <a name="tshirtsize"></a>tshirtSize
Jste obeznámeni s Tato proměnná generovaný z výše uvedených příkladech hello.

#### <a name="networksettings"></a>networkSettings
V kapacitu, schopností nebo oboru řešení začátku do konce šablony hello propojených šablon obvykle vytvořit prostředky, které existují v síti. Jeden Nejjednodušším způsobem je toouse nastavení sítě toostore komplexní objekt a předat je toolinked šablony.

Níže najdete příklad komunikaci nastavení sítě.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings
Prostředky vytvořené v propojených šablon jsou často umístěny v nastavení dostupnosti. V následujícím příkladu hello název sady dostupnosti hello je zadána a také hello doména selhání a aktualizace domény počet toouse.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Pokud potřebujete více skupiny dostupnosti (například jeden pro hlavní uzly) a druhý pro datové uzly, můžete použít název jako předpony, zadejte několik skupin dostupnosti nebo postupujte podle modelu hello uvedené výše pro vytvoření proměnné pro konkrétní velikost trička.

#### <a name="storagesettings"></a>storageSettings
Podrobnosti o úložiště jsou často sdílet s propojených šablon. V následujícím příkladu hello *storageSettings* objekt poskytuje podrobnosti o hello názvy účtů a kontejner úložiště.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings
S propojených šablon může být nutné typy uzlů toopass operačního systému nastavení toovarious typů jiné známé konfigurace. Komplexní objekt operačního systému informace snadný způsob, toostore a sdílené složky a také umožňuje snazší toosupport více možností operačního systému pro nasazení.

Hello následující příklad ukazuje objekt pro *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings
Proměnnou generovaného *machineSettings* je komplexní objekt obsahující směs základní proměnných pro vytvoření virtuálního počítače. proměnné Hello zahrnují správce uživatelské jméno a heslo, předpona pro názvy virtuálních počítačů hello a odkaz na bitovou kopii operačního systému.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Všimněte si, že *osImageReference* načte hello hodnoty z hello *osSettings* proměnná definovaná v šabloně hlavní hello. To znamená, že můžete snadno změnit hello operačního systému pro virtuální počítač – zcela nebo v závislosti na hello předvoleb šablony příjemce.

#### <a name="vmscripts"></a>vmScripts
Hello *vmScripts* objektu obsahuje podrobnosti o toodownload hello skripty a spustit v instanci virtuálního počítače, včetně odkazů a vnější. Mimo zahrnovat odkazy hello infrastruktury.
Odkazy uvnitř zahrnují nainstalovaný hello nainstalovaný software a konfigurace.

Použít hello *scriptsToDownload* vlastnost toolist hello skriptů toodownload toohello virtuálních počítačů. Tento objekt obsahuje taky odkazy na řádku toocommand argumenty pro různé typy akcí. Mezi ně patří provádění hello výchozí instalace pro všechny jednotlivé uzly, instalaci, která spustí po všechny uzly, jsou nasazené a další skripty, které mohou být konkrétní tooa zadané šablony.

V tomto příkladu je z šablony použité toodeploy MongoDB, které vyžaduje arbiter toodeliver vysokou dostupnost. Hello *arbiterNodeInstallCommand* byla přidána příliš*vmScripts* tooinstall hello arbiter.

část proměnné Hello je, kde najít hello proměnné, které definují hello určitý text tooexecute hello skriptu s hello správné hodnoty.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Návratový stav ze šablony
Nejenže můžete předat data do šablony, můžete také sdílení dat toohello zpětné volání šablony. V hello **výstupy** část propojené šablony, můžete zadat dvojice klíč/hodnota, které mohou být spotřebovávána hello zdrojové šabloně.

Hello následující příklad ukazuje, jak toopass hello vygenerovaných šablonu propojené privátní IP adresu.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

V rámci hello hlavní šablony můžete tato data s hello následující syntaxi:

    "[reference('master-node').outputs.masterip.value]"

Můžete použít tento výraz v části výstupy hello nebo oddílu prostředků hello hello hlavní šablony. V části hello proměnné nelze použít hello výraz, protože závisí na stav modulu runtime hello. tooreturn tuto hodnotu z hello hlavní šablony, použijte:

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

Příklad použití hello výstupy část propojené šablony tooreturn datových disků pro virtuální počítač, najdete v části [vytváření více datových disků pro virtuální počítač](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Zadejte nastavení ověřování pro virtuální počítač
Můžete použít hello stejného vzoru zobrazovaného dříve pro nastavení ověřování hello toospecify pro konfiguraci nastavení pro virtuální počítač. Vytvoření parametru pro předávání v hello typ ověřování.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Přidáte proměnných pro hello různá ověřovací typy a proměnné toostore, jaký typ se používá pro toto nasazení na základě hello hodnoty parametru hello.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Při definování hello virtuálního počítače, nastavíte hello **osProfile** toohello proměnné, které jste vytvořili.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Další kroky
* toolearn o části hello hello šablony, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md)
* toosee hello funkce, které jsou k dispozici v rámci šablon, najdete v části [funkce šablon Azure Resource Manager](resource-group-template-functions.md)
