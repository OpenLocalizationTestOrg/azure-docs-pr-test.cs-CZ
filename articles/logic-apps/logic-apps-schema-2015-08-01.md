---
title: "aaaSchema aktualizace srpen-1-2015 preview – Azure Logic Apps | Microsoft Docs"
description: "Vytvoření definice JSON pro Azure Logic Apps pomocí schématu verze 2015-08-01-preview"
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.openlocfilehash: 950cd18a27aa1859c4f0b6116de3fb8699d746c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Aktualizace schématu pro Azure Logic Apps - 1. srpna 2015 preview

Toto nové schéma a rozhraní API verze pro Azure Logic Apps zahrnuje klíčových vylepšení, které aplikace logiky více spolehlivé a snadnější toouse:

*   Hello **APIApp** typ akce je aktualizovaný tooa nové [ **APIConnection** ](#api-connections) typ akce.
*   **Opakujte** příliš přejmenován[**Foreach**](#foreach).
*   Hello [ **naslouchací proces protokolu HTTP** aplikace API](#http-listener) už nepotřebujete.
*   Volání metody podřízené pracovní postupy používá [nové schéma](#child-workflows).

<a name="api-connections"></a>
## <a name="move-tooapi-connections"></a>Přesunout tooAPI připojení

největší změnou Hello je mít toodeploy API Apps do vašeho předplatného Azure už, abyste mohli používat rozhraní API. Tady jsou hello způsoby, které můžete použít rozhraní API:

* Spravovaná rozhraní API
* Vlastní webová rozhraní API.

Každý způsob je zpracovávané trochu jinak, protože jejich správy a hostování modely se liší. Jednou z výhod tento model je již máte omezené tooresources, která jsou nasazena ve vaší skupině prostředků Azure. 

### <a name="managed-apis"></a>Spravovaná rozhraní API

Microsoft spravuje některé rozhraní API vaším jménem, jako je například Office 365, Salesforce, Twitter a FTP. Můžete použít některé spravovaná rozhraní API jako-je třeba Bing přeložit, zatímco jiné vyžadují konfiguraci. Tato konfigurace se nazývá *připojení*.

Například pokud používáte Office 365, musí vytvořit připojení, který obsahuje znaménko token Office 365. Tento token je bezpečně uložená a aktualizovat tak, aby aplikace logiky můžete vždy volat hello rozhraní API Office 365. Případně pokud chcete tooconnect tooyour SQL Server nebo server FTP, musíte vytvořit připojení, který má hello připojovací řetězec. 

V této definici, se nazývají tyto akce `APIConnection`. Tady je příklad připojení, která volá toosend Office 365 e-mailu:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Hello `host` objekt je část vstupy, kterou je jedinečný tooAPI připojení a obsahuje tažení částí: `api` a `connection`.

Hello `api` má runtime hello je adresa URL, kde spravované rozhraní API hostovaná. Zobrazí všechny dostupné hello spravuje volání rozhraní API `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Pokud používáte rozhraní API, hello rozhraní API může nebo nemusí mít žádný *parametrů připojení* definované. Pokud není hello rozhraní API, žádné *připojení* je vyžadován. Pokud nemá hello rozhraní API, musíte vytvořit připojení. Hello vytvořili připojení má hello název, který zvolíte. Pak odkazovat hello název v hello `connection` objekt uvnitř hello `host` objektu. toocreate připojení ve skupině prostředků, volání:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

S hello následující text:

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{hello name of hello storage account -- hello set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Nasadit spravované rozhraní API v šablonu Azure Resource Manager

Celou aplikaci můžete vytvořit v šablonu Azure Resource Manager, dokud interaktivní přihlašování není povinné.
Pokud přihlášení je potřeba, můžete nastavit všechno pomocí šablony Azure Resource Manager hello, ale máte pořád toovisit hello portálu tooauthorize hello připojení. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/', parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:, Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Zobrazí se v tomto příkladu, zda text hello připojení jsou jenom prostředky, které za provozu ve vaší skupině prostředků. Jejich referenční hello spravované rozhraní API k dispozici tooyou ve vašem předplatném.

### <a name="your-custom-web-apis"></a>Vlastní webová rozhraní API.

Pokud používáte vlastní rozhraní API, není spravovaný společností Microsoft ty použít předdefinované hello **HTTP** toocall akce je. Pro ideální prostředí by měl vystavit koncový bod Swagger pro vaše rozhraní API. Tento koncový bod umožňuje hello návrhář aplikace na základě logiky toorender hello vstupy a výstupy pro vaše rozhraní API. Bez Swagger Návrhář hello lze zobrazit pouze hello vstupy a výstupy jako neprůhledné objekty JSON.

Tady je hello příklad zobrazuje nové `metadata.apiDefinitionUrl` vlastnost:

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

Pokud hostujete webové rozhraní API v Azure App Service, Web API se automaticky zobrazí v hello seznam akcí, které jsou k dispozici v Návrháři hello. Pokud ne, máte toopaste v adrese URL hello přímo. koncový bod Swagger Hello musí být neověřená toobe použitelné v hello návrhář aplikace na základě logiky, i když můžete zabezpečit API hello, samotné jakékoli metody, které podporuje Swagger.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Volání nasazené aplikace API s 2015-08-01-preview

Pokud jste nasadili aplikace API, můžete zavolat hello aplikace pomocí hello **HTTP** akce.

Pokud použijete Dropbox toolist soubory, například vaše **2014-12-01-preview** definice schématu verze může mít něco podobného jako:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Hello ekvivalentní HTTP akcí, jako je například můžete vytvořit při hello parametry části hello definici aplikace logiky zůstává beze změny:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata": {
              "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method": "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Tyto vlastnosti po jednom s návodem:

| Vlastnosti akce. | Popis |
| --- | --- |
| `type` |`Http`Namísto`APIapp` |
| `metadata.apiDefinitionUrl` |toouse tato akce v hello návrhář aplikace na základě logiky, patří koncový bod metadat hello, které se vytvářejí na základě:`{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Vytvářejí na základě:`{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Vždy`POST` |
| `inputs.body` |Parametry aplikace identické toohello rozhraní API |
| `inputs.authentication` |Identické toohello ověřování aplikace API |

Tento postup by měly fungovat pro všechny akce aplikace API. Nezapomeňte však, že tato předchozí aplikace API již nejsou podporovány. Proto byste měli přesunout tooone hello dvě předchozí možnosti, spravované rozhraní API nebo hostující vaše vlastní webové rozhraní API.

<a name="foreach"></a>
## <a name="renamed-repeat-tooforeach"></a>Přejmenovat, opakujte too'foreach.

Pro předchozí verze schématu hello dostali jsme mnohem názory zákazníků, **opakovat** bylo matoucí a nebyla zachycení správně, **opakujte** je skutečně pro každou smyčku. V důsledku toho jsme přejmenovaná `repeat` příliš`foreach`. Například dříve jste by zápisu:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Teď by zápisu:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Hello funkce `@repeatItem()` byla dříve použitých tooreference hello aktuální položky se vstupní přes. Tato funkce je teď jednodušší příliš`@item()`. 

### <a name="reference-outputs-from-foreach"></a>Referenční dokumentace výstupy z 'foreach.

Pro zjednodušení, hello výstupy z `foreach` akce nejsou uzavřen do objektu s názvem `repeatItems`. Při hello výstupy z předchozí hello `repeat` byly příklad:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Nyní jsou tyto výstupy:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Dříve tooget hello akce při odkazování na těchto výstupů textu toohello:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

Teď můžete místo toho provést:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

Tyto změny hello funkce `@repeatItem()`, `@repeatBody()`, a `@repeatOutputs()` se odeberou.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Nativní naslouchací proces protokolu HTTP

Hello možnosti jsou teď integrované v naslouchací proces protokolu HTTP. Proto musíte už toodeploy aplikace API naslouchací proces protokolu HTTP. V tématu [hello úplné podrobnosti o tom toomake s zde vaše logiku aplikace koncového bodu](../logic-apps/logic-apps-http-endpoint.md). 

Tyto změny, jsme odebrali hello `@accessKeys()` funkce, které bylo nahrazeno hello `@listCallbackURL()` funkce pro získání hello koncový bod, pokud je to nezbytné. Navíc je nutné nyní zadat nejméně jedna aktivační událost v aplikaci logiky. Pokud chcete příliš`/run` hello pracovního postupu, musí mít jednu z těchto aktivační události: `manual`, `apiConnectionWebhook`, nebo `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Volat podřízené pracovní postupy

Volání metody podřízené pracovní postupy dříve, vyžaduje, budete toohello pracovního postupu, získávání hello přístupový token a vkládání hello token v definici aplikace logiky hello místo toocall tento pracovní postup podřízené. S hello nové schéma hello hello Logic Apps modul automaticky vygeneruje SAS v době běhu pro podřízený pracovní postup, nebudete mít příliš vkládání všech tajných klíčů do definice hello. Zde naleznete příklad:

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
        },
        "queries": {
            "extrafield": "specialValue"
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "conditions": []
}
```

Druhý zlepšování je že nabízíme hello podřízených pracovních úplný přístup toohello příchozího požadavku. To znamená, že lze předat parametry v hello *dotazy* části a v hello *hlavičky* objekt a že je plně definovat hello celého obsahu.

Nakonec jsou požadované změny toohello podřízený pracovní postup. Při podřízený pracovní postup může volat dříve přímo, nyní je nutné definovat koncový bod aktivační událost v pracovním postupu hello pro toocall nadřazené hello. Obecně platí, měli byste přidat aktivační událost, která má `manual` zadejte a potom pomocí této aktivační události v definici nadřazeného hello. Poznámka: hello `host` vlastnost konkrétně má `triggerName` vzhledem k tomu, že musíte určit které aktivační událost je vyvolán.

## <a name="other-changes"></a>Další změny

### <a name="new-queries-property"></a>Nové vlastnosti 'dotazy.

Všechny typy akcí teď podporují nové vstup názvem `queries`. Tento vstup může být strukturovaný objekt, místo nutnosti tooassemble hello řetězec ručně.

### <a name="renamed-parse-function-toojson"></a>Přejmenovat too'json() funkce 'parse()'.

Přidáváme další obsah typy brzy, takže jsme přejmenovat hello `parse()` funkce příliš`json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Připravuje se: Enterprise integrace rozhraní API

Nemáme spravované verze ještě hello Enterprise integrace rozhraní API, jako je AS2. Mezitím můžete svých stávajících nasazené BizTalk rozhraní API prostřednictvím hello akce HTTP. Podrobnosti najdete v tématu "Použití již nasazené aplikace API" v hello [integrace plán](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
