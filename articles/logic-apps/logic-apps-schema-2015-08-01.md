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
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a><span data-ttu-id="93b98-103">Aktualizace schématu pro Azure Logic Apps - 1. srpna 2015 preview</span><span class="sxs-lookup"><span data-stu-id="93b98-103">Schema updates for Azure Logic Apps - August 1, 2015 preview</span></span>

<span data-ttu-id="93b98-104">Toto nové schéma a rozhraní API verze pro Azure Logic Apps zahrnuje klíčových vylepšení, které aplikace logiky více spolehlivé a snadnější toouse:</span><span class="sxs-lookup"><span data-stu-id="93b98-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier toouse:</span></span>

*   <span data-ttu-id="93b98-105">Hello **APIApp** typ akce je aktualizovaný tooa nové [ **APIConnection** ](#api-connections) typ akce.</span><span class="sxs-lookup"><span data-stu-id="93b98-105">hello **APIApp** action type is updated tooa new [**APIConnection**](#api-connections) action type.</span></span>
*   <span data-ttu-id="93b98-106">**Opakujte** příliš přejmenován[**Foreach**](#foreach).</span><span class="sxs-lookup"><span data-stu-id="93b98-106">**Repeat** is renamed too[**Foreach**](#foreach).</span></span>
*   <span data-ttu-id="93b98-107">Hello [ **naslouchací proces protokolu HTTP** aplikace API](#http-listener) už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="93b98-107">hello [**HTTP Listener** API App](#http-listener) is no longer required.</span></span>
*   <span data-ttu-id="93b98-108">Volání metody podřízené pracovní postupy používá [nové schéma](#child-workflows).</span><span class="sxs-lookup"><span data-stu-id="93b98-108">Calling child workflows uses a [new schema](#child-workflows).</span></span>

<a name="api-connections"></a>
## <a name="move-tooapi-connections"></a><span data-ttu-id="93b98-109">Přesunout tooAPI připojení</span><span class="sxs-lookup"><span data-stu-id="93b98-109">Move tooAPI connections</span></span>

<span data-ttu-id="93b98-110">největší změnou Hello je mít toodeploy API Apps do vašeho předplatného Azure už, abyste mohli používat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93b98-110">hello biggest change is that you no longer have toodeploy API Apps into your Azure subscription so you can use APIs.</span></span> <span data-ttu-id="93b98-111">Tady jsou hello způsoby, které můžete použít rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="93b98-111">Here are hello ways that you can use APIs:</span></span>

* <span data-ttu-id="93b98-112">Spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="93b98-112">Managed APIs</span></span>
* <span data-ttu-id="93b98-113">Vlastní webová rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93b98-113">Your custom Web APIs</span></span>

<span data-ttu-id="93b98-114">Každý způsob je zpracovávané trochu jinak, protože jejich správy a hostování modely se liší.</span><span class="sxs-lookup"><span data-stu-id="93b98-114">Each way is handled slightly differently because their management and hosting models are different.</span></span> <span data-ttu-id="93b98-115">Jednou z výhod tento model je již máte omezené tooresources, která jsou nasazena ve vaší skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="93b98-115">One advantage of this model is you're no longer constrained tooresources that are deployed in your Azure resource group.</span></span> 

### <a name="managed-apis"></a><span data-ttu-id="93b98-116">Spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="93b98-116">Managed APIs</span></span>

<span data-ttu-id="93b98-117">Microsoft spravuje některé rozhraní API vaším jménem, jako je například Office 365, Salesforce, Twitter a FTP.</span><span class="sxs-lookup"><span data-stu-id="93b98-117">Microsoft manages some APIs on your behalf, such as Office 365, Salesforce, Twitter, and FTP.</span></span> <span data-ttu-id="93b98-118">Můžete použít některé spravovaná rozhraní API jako-je třeba Bing přeložit, zatímco jiné vyžadují konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="93b98-118">You can use some managed APIs as-is, such as Bing Translate, while others require configuration.</span></span> <span data-ttu-id="93b98-119">Tato konfigurace se nazývá *připojení*.</span><span class="sxs-lookup"><span data-stu-id="93b98-119">This configuration is called a *connection*.</span></span>

<span data-ttu-id="93b98-120">Například pokud používáte Office 365, musí vytvořit připojení, který obsahuje znaménko token Office 365.</span><span class="sxs-lookup"><span data-stu-id="93b98-120">For example, when you use Office 365, you must create a connection that contains your Office 365 sign-in token.</span></span> <span data-ttu-id="93b98-121">Tento token je bezpečně uložená a aktualizovat tak, aby aplikace logiky můžete vždy volat hello rozhraní API Office 365.</span><span class="sxs-lookup"><span data-stu-id="93b98-121">This token is securely stored and refreshed so that your logic app can always call hello Office 365 API.</span></span> <span data-ttu-id="93b98-122">Případně pokud chcete tooconnect tooyour SQL Server nebo server FTP, musíte vytvořit připojení, který má hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="93b98-122">Alternatively, if you want tooconnect tooyour SQL or FTP server, you must create a connection that has hello connection string.</span></span> 

<span data-ttu-id="93b98-123">V této definici, se nazývají tyto akce `APIConnection`.</span><span class="sxs-lookup"><span data-stu-id="93b98-123">In this definition, these actions are called `APIConnection`.</span></span> <span data-ttu-id="93b98-124">Tady je příklad připojení, která volá toosend Office 365 e-mailu:</span><span class="sxs-lookup"><span data-stu-id="93b98-124">Here is an example of a connection that calls Office 365 toosend an email:</span></span>

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

<span data-ttu-id="93b98-125">Hello `host` objekt je část vstupy, kterou je jedinečný tooAPI připojení a obsahuje tažení částí: `api` a `connection`.</span><span class="sxs-lookup"><span data-stu-id="93b98-125">hello `host` object is portion of inputs that is unique tooAPI connections, and contains tow parts: `api` and `connection`.</span></span>

<span data-ttu-id="93b98-126">Hello `api` má runtime hello je adresa URL, kde spravované rozhraní API hostovaná.</span><span class="sxs-lookup"><span data-stu-id="93b98-126">hello `api` has hello runtime URL of where that managed API is hosted.</span></span> <span data-ttu-id="93b98-127">Zobrazí všechny dostupné hello spravuje volání rozhraní API `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span><span class="sxs-lookup"><span data-stu-id="93b98-127">You can see all hello available managed APIs by calling `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span></span>

<span data-ttu-id="93b98-128">Pokud používáte rozhraní API, hello rozhraní API může nebo nemusí mít žádný *parametrů připojení* definované.</span><span class="sxs-lookup"><span data-stu-id="93b98-128">When you use an API, hello API might or might not have any *connection parameters* defined.</span></span> <span data-ttu-id="93b98-129">Pokud není hello rozhraní API, žádné *připojení* je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="93b98-129">If hello API doesn't, no *connection* is required.</span></span> <span data-ttu-id="93b98-130">Pokud nemá hello rozhraní API, musíte vytvořit připojení.</span><span class="sxs-lookup"><span data-stu-id="93b98-130">If hello API does, you must create a connection.</span></span> <span data-ttu-id="93b98-131">Hello vytvořili připojení má hello název, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="93b98-131">hello created connection has hello name that you choose.</span></span> <span data-ttu-id="93b98-132">Pak odkazovat hello název v hello `connection` objekt uvnitř hello `host` objektu.</span><span class="sxs-lookup"><span data-stu-id="93b98-132">You then reference hello name in hello `connection` object inside hello `host` object.</span></span> <span data-ttu-id="93b98-133">toocreate připojení ve skupině prostředků, volání:</span><span class="sxs-lookup"><span data-stu-id="93b98-133">toocreate a connection in a resource group, call:</span></span>

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

<span data-ttu-id="93b98-134">S hello následující text:</span><span class="sxs-lookup"><span data-stu-id="93b98-134">With hello following body:</span></span>

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

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a><span data-ttu-id="93b98-135">Nasadit spravované rozhraní API v šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="93b98-135">Deploy managed APIs in an Azure Resource Manager template</span></span>

<span data-ttu-id="93b98-136">Celou aplikaci můžete vytvořit v šablonu Azure Resource Manager, dokud interaktivní přihlašování není povinné.</span><span class="sxs-lookup"><span data-stu-id="93b98-136">You can create a full application in an Azure Resource Manager template as long as interactive sign-in isn't required.</span></span>
<span data-ttu-id="93b98-137">Pokud přihlášení je potřeba, můžete nastavit všechno pomocí šablony Azure Resource Manager hello, ale máte pořád toovisit hello portálu tooauthorize hello připojení.</span><span class="sxs-lookup"><span data-stu-id="93b98-137">If sign-in is required, you can set up everything with hello Azure Resource Manager template, but you still have toovisit hello portal tooauthorize hello connections.</span></span> 

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

<span data-ttu-id="93b98-138">Zobrazí se v tomto příkladu, zda text hello připojení jsou jenom prostředky, které za provozu ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="93b98-138">You can see in this example that hello connections are just resources that live in your resource group.</span></span> <span data-ttu-id="93b98-139">Jejich referenční hello spravované rozhraní API k dispozici tooyou ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="93b98-139">They reference hello managed APIs available tooyou in your subscription.</span></span>

### <a name="your-custom-web-apis"></a><span data-ttu-id="93b98-140">Vlastní webová rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93b98-140">Your custom Web APIs</span></span>

<span data-ttu-id="93b98-141">Pokud používáte vlastní rozhraní API, není spravovaný společností Microsoft ty použít předdefinované hello **HTTP** toocall akce je.</span><span class="sxs-lookup"><span data-stu-id="93b98-141">If you use your own APIs, not Microsoft-managed ones, use hello built-in **HTTP** action toocall them.</span></span> <span data-ttu-id="93b98-142">Pro ideální prostředí by měl vystavit koncový bod Swagger pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93b98-142">For an ideal experience, you should expose a Swagger endpoint for your API.</span></span> <span data-ttu-id="93b98-143">Tento koncový bod umožňuje hello návrhář aplikace na základě logiky toorender hello vstupy a výstupy pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93b98-143">This endpoint enables hello Logic App Designer toorender hello inputs and outputs for your API.</span></span> <span data-ttu-id="93b98-144">Bez Swagger Návrhář hello lze zobrazit pouze hello vstupy a výstupy jako neprůhledné objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="93b98-144">Without Swagger, hello designer can only show hello inputs and outputs as opaque JSON objects.</span></span>

<span data-ttu-id="93b98-145">Tady je hello příklad zobrazuje nové `metadata.apiDefinitionUrl` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="93b98-145">Here is an example showing hello new `metadata.apiDefinitionUrl` property:</span></span>

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

<span data-ttu-id="93b98-146">Pokud hostujete webové rozhraní API v Azure App Service, Web API se automaticky zobrazí v hello seznam akcí, které jsou k dispozici v Návrháři hello.</span><span class="sxs-lookup"><span data-stu-id="93b98-146">If you host your Web API on Azure App Service, your Web API automatically appears in hello list of actions available in hello designer.</span></span> <span data-ttu-id="93b98-147">Pokud ne, máte toopaste v adrese URL hello přímo.</span><span class="sxs-lookup"><span data-stu-id="93b98-147">If not, you have toopaste in hello URL directly.</span></span> <span data-ttu-id="93b98-148">koncový bod Swagger Hello musí být neověřená toobe použitelné v hello návrhář aplikace na základě logiky, i když můžete zabezpečit API hello, samotné jakékoli metody, které podporuje Swagger.</span><span class="sxs-lookup"><span data-stu-id="93b98-148">hello Swagger endpoint must be unauthenticated toobe usable in hello Logic App Designer, although you can secure hello API itself with whatever methods that Swagger supports.</span></span>

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a><span data-ttu-id="93b98-149">Volání nasazené aplikace API s 2015-08-01-preview</span><span class="sxs-lookup"><span data-stu-id="93b98-149">Call deployed API apps with 2015-08-01-preview</span></span>

<span data-ttu-id="93b98-150">Pokud jste nasadili aplikace API, můžete zavolat hello aplikace pomocí hello **HTTP** akce.</span><span class="sxs-lookup"><span data-stu-id="93b98-150">If you previously deployed an API App, you can call hello app with hello **HTTP** action.</span></span>

<span data-ttu-id="93b98-151">Pokud použijete Dropbox toolist soubory, například vaše **2014-12-01-preview** definice schématu verze může mít něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="93b98-151">For example, if you use Dropbox toolist files, your **2014-12-01-preview** schema version definition might have something like:</span></span>

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

<span data-ttu-id="93b98-152">Hello ekvivalentní HTTP akcí, jako je například můžete vytvořit při hello parametry části hello definici aplikace logiky zůstává beze změny:</span><span class="sxs-lookup"><span data-stu-id="93b98-152">You can construct hello equivalent HTTP action like this example, while hello parameters section of hello Logic app definition remains unchanged:</span></span>

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

<span data-ttu-id="93b98-153">Tyto vlastnosti po jednom s návodem:</span><span class="sxs-lookup"><span data-stu-id="93b98-153">Walking through these properties one-by-one:</span></span>

| <span data-ttu-id="93b98-154">Vlastnosti akce.</span><span class="sxs-lookup"><span data-stu-id="93b98-154">Action property</span></span> | <span data-ttu-id="93b98-155">Popis</span><span class="sxs-lookup"><span data-stu-id="93b98-155">Description</span></span> |
| --- | --- |
| `type` |<span data-ttu-id="93b98-156">`Http`Namísto`APIapp`</span><span class="sxs-lookup"><span data-stu-id="93b98-156">`Http` instead of `APIapp`</span></span> |
| `metadata.apiDefinitionUrl` |<span data-ttu-id="93b98-157">toouse tato akce v hello návrhář aplikace na základě logiky, patří koncový bod metadat hello, které se vytvářejí na základě:`{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span><span class="sxs-lookup"><span data-stu-id="93b98-157">toouse this action in hello Logic App Designer, include hello metadata endpoint, which is constructed from: `{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span></span> |
| `inputs.uri` |<span data-ttu-id="93b98-158">Vytvářejí na základě:`{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14`</span><span class="sxs-lookup"><span data-stu-id="93b98-158">Constructed from: `{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14`</span></span> |
| `inputs.method` |<span data-ttu-id="93b98-159">Vždy`POST`</span><span class="sxs-lookup"><span data-stu-id="93b98-159">Always `POST`</span></span> |
| `inputs.body` |<span data-ttu-id="93b98-160">Parametry aplikace identické toohello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="93b98-160">Identical toohello API App parameters</span></span> |
| `inputs.authentication` |<span data-ttu-id="93b98-161">Identické toohello ověřování aplikace API</span><span class="sxs-lookup"><span data-stu-id="93b98-161">Identical toohello API App authentication</span></span> |

<span data-ttu-id="93b98-162">Tento postup by měly fungovat pro všechny akce aplikace API.</span><span class="sxs-lookup"><span data-stu-id="93b98-162">This approach should work for all API App actions.</span></span> <span data-ttu-id="93b98-163">Nezapomeňte však, že tato předchozí aplikace API již nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="93b98-163">However, remember that these previous API Apps are no longer supported.</span></span> <span data-ttu-id="93b98-164">Proto byste měli přesunout tooone hello dvě předchozí možnosti, spravované rozhraní API nebo hostující vaše vlastní webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93b98-164">So you should move tooone of hello two other previous options, a managed API or hosting your custom Web API.</span></span>

<a name="foreach"></a>
## <a name="renamed-repeat-tooforeach"></a><span data-ttu-id="93b98-165">Přejmenovat, opakujte too'foreach.</span><span class="sxs-lookup"><span data-stu-id="93b98-165">Renamed 'repeat' too'foreach'</span></span>

<span data-ttu-id="93b98-166">Pro předchozí verze schématu hello dostali jsme mnohem názory zákazníků, **opakovat** bylo matoucí a nebyla zachycení správně, **opakujte** je skutečně pro každou smyčku.</span><span class="sxs-lookup"><span data-stu-id="93b98-166">For hello previous schema version, we received much customer feedback that **Repeat** was confusing and didn't properly capture that **Repeat** was really a for-each loop.</span></span> <span data-ttu-id="93b98-167">V důsledku toho jsme přejmenovaná `repeat` příliš`foreach`.</span><span class="sxs-lookup"><span data-stu-id="93b98-167">As a result, we have renamed `repeat` too`foreach`.</span></span> <span data-ttu-id="93b98-168">Například dříve jste by zápisu:</span><span class="sxs-lookup"><span data-stu-id="93b98-168">For example, previously you would write:</span></span>

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

<span data-ttu-id="93b98-169">Teď by zápisu:</span><span class="sxs-lookup"><span data-stu-id="93b98-169">Now you would write:</span></span>

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

<span data-ttu-id="93b98-170">Hello funkce `@repeatItem()` byla dříve použitých tooreference hello aktuální položky se vstupní přes.</span><span class="sxs-lookup"><span data-stu-id="93b98-170">hello function `@repeatItem()` was previously used tooreference hello current item being iterated over.</span></span> <span data-ttu-id="93b98-171">Tato funkce je teď jednodušší příliš`@item()`.</span><span class="sxs-lookup"><span data-stu-id="93b98-171">This function is now simplified too`@item()`.</span></span> 

### <a name="reference-outputs-from-foreach"></a><span data-ttu-id="93b98-172">Referenční dokumentace výstupy z 'foreach.</span><span class="sxs-lookup"><span data-stu-id="93b98-172">Reference outputs from 'foreach'</span></span>

<span data-ttu-id="93b98-173">Pro zjednodušení, hello výstupy z `foreach` akce nejsou uzavřen do objektu s názvem `repeatItems`.</span><span class="sxs-lookup"><span data-stu-id="93b98-173">For simplification, hello outputs from `foreach` actions are not wrapped in an object called `repeatItems`.</span></span> <span data-ttu-id="93b98-174">Při hello výstupy z předchozí hello `repeat` byly příklad:</span><span class="sxs-lookup"><span data-stu-id="93b98-174">While hello outputs from hello previous `repeat` example were:</span></span>

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

<span data-ttu-id="93b98-175">Nyní jsou tyto výstupy:</span><span class="sxs-lookup"><span data-stu-id="93b98-175">Now these outputs are:</span></span>

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

<span data-ttu-id="93b98-176">Dříve tooget hello akce při odkazování na těchto výstupů textu toohello:</span><span class="sxs-lookup"><span data-stu-id="93b98-176">Previously, tooget toohello body of hello action when referencing these outputs:</span></span>

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

<span data-ttu-id="93b98-177">Teď můžete místo toho provést:</span><span class="sxs-lookup"><span data-stu-id="93b98-177">Now you can do instead:</span></span>

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

<span data-ttu-id="93b98-178">Tyto změny hello funkce `@repeatItem()`, `@repeatBody()`, a `@repeatOutputs()` se odeberou.</span><span class="sxs-lookup"><span data-stu-id="93b98-178">With these changes, hello functions `@repeatItem()`, `@repeatBody()`, and `@repeatOutputs()` are removed.</span></span>

<a name="http-listener"></a>
## <a name="native-http-listener"></a><span data-ttu-id="93b98-179">Nativní naslouchací proces protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="93b98-179">Native HTTP listener</span></span>

<span data-ttu-id="93b98-180">Hello možnosti jsou teď integrované v naslouchací proces protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="93b98-180">hello HTTP Listener capabilities are now built in.</span></span> <span data-ttu-id="93b98-181">Proto musíte už toodeploy aplikace API naslouchací proces protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="93b98-181">So you no longer need toodeploy an HTTP Listener API App.</span></span> <span data-ttu-id="93b98-182">V tématu [hello úplné podrobnosti o tom toomake s zde vaše logiku aplikace koncového bodu](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="93b98-182">See [hello full details for how toomake your Logic app endpoint callable here](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<span data-ttu-id="93b98-183">Tyto změny, jsme odebrali hello `@accessKeys()` funkce, které bylo nahrazeno hello `@listCallbackURL()` funkce pro získání hello koncový bod, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="93b98-183">With these changes, we removed hello `@accessKeys()` function, which we replaced with hello `@listCallbackURL()` function for getting hello endpoint when necessary.</span></span> <span data-ttu-id="93b98-184">Navíc je nutné nyní zadat nejméně jedna aktivační událost v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="93b98-184">Also, you must now define at least one trigger in your logic app.</span></span> <span data-ttu-id="93b98-185">Pokud chcete příliš`/run` hello pracovního postupu, musí mít jednu z těchto aktivační události: `manual`, `apiConnectionWebhook`, nebo `httpWebhook`.</span><span class="sxs-lookup"><span data-stu-id="93b98-185">If you want too`/run` hello workflow, you must have one of these triggers: `manual`, `apiConnectionWebhook`, or `httpWebhook`.</span></span>

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a><span data-ttu-id="93b98-186">Volat podřízené pracovní postupy</span><span class="sxs-lookup"><span data-stu-id="93b98-186">Call child workflows</span></span>

<span data-ttu-id="93b98-187">Volání metody podřízené pracovní postupy dříve, vyžaduje, budete toohello pracovního postupu, získávání hello přístupový token a vkládání hello token v definici aplikace logiky hello místo toocall tento pracovní postup podřízené.</span><span class="sxs-lookup"><span data-stu-id="93b98-187">Previously, calling child workflows required going toohello workflow, getting hello access token, and pasting hello token in hello logic app definition where you want toocall that child workflow.</span></span> <span data-ttu-id="93b98-188">S hello nové schéma hello hello Logic Apps modul automaticky vygeneruje SAS v době běhu pro podřízený pracovní postup, nebudete mít příliš vkládání všech tajných klíčů do definice hello.</span><span class="sxs-lookup"><span data-stu-id="93b98-188">With hello new schema, hello Logic Apps engine automatically generates a SAS at runtime for hello child workflow so you don't have too paste any secrets into hello definition.</span></span> <span data-ttu-id="93b98-189">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="93b98-189">Here is an example:</span></span>

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

<span data-ttu-id="93b98-190">Druhý zlepšování je že nabízíme hello podřízených pracovních úplný přístup toohello příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="93b98-190">A second improvement is we are giving hello child workflows full access toohello incoming request.</span></span> <span data-ttu-id="93b98-191">To znamená, že lze předat parametry v hello *dotazy* části a v hello *hlavičky* objekt a že je plně definovat hello celého obsahu.</span><span class="sxs-lookup"><span data-stu-id="93b98-191">That means that you can pass parameters in hello *queries* section and in hello *headers* object and that you can fully define hello entire body.</span></span>

<span data-ttu-id="93b98-192">Nakonec jsou požadované změny toohello podřízený pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="93b98-192">Finally, there are required changes toohello child workflow.</span></span> <span data-ttu-id="93b98-193">Při podřízený pracovní postup může volat dříve přímo, nyní je nutné definovat koncový bod aktivační událost v pracovním postupu hello pro toocall nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="93b98-193">While you could previously call a child workflow directly, now you must define a trigger endpoint in hello workflow for hello parent toocall.</span></span> <span data-ttu-id="93b98-194">Obecně platí, měli byste přidat aktivační událost, která má `manual` zadejte a potom pomocí této aktivační události v definici nadřazeného hello.</span><span class="sxs-lookup"><span data-stu-id="93b98-194">Generally, you would add a trigger that has `manual` type, and then use that trigger in hello parent definition.</span></span> <span data-ttu-id="93b98-195">Poznámka: hello `host` vlastnost konkrétně má `triggerName` vzhledem k tomu, že musíte určit které aktivační událost je vyvolán.</span><span class="sxs-lookup"><span data-stu-id="93b98-195">Note hello `host` property specifically has a `triggerName` because you must always specify which trigger you are invoking.</span></span>

## <a name="other-changes"></a><span data-ttu-id="93b98-196">Další změny</span><span class="sxs-lookup"><span data-stu-id="93b98-196">Other changes</span></span>

### <a name="new-queries-property"></a><span data-ttu-id="93b98-197">Nové vlastnosti 'dotazy.</span><span class="sxs-lookup"><span data-stu-id="93b98-197">New 'queries' property</span></span>

<span data-ttu-id="93b98-198">Všechny typy akcí teď podporují nové vstup názvem `queries`.</span><span class="sxs-lookup"><span data-stu-id="93b98-198">All action types now support a new input called `queries`.</span></span> <span data-ttu-id="93b98-199">Tento vstup může být strukturovaný objekt, místo nutnosti tooassemble hello řetězec ručně.</span><span class="sxs-lookup"><span data-stu-id="93b98-199">This input can be a structured object, rather than you having tooassemble hello string by hand.</span></span>

### <a name="renamed-parse-function-toojson"></a><span data-ttu-id="93b98-200">Přejmenovat too'json() funkce 'parse()'.</span><span class="sxs-lookup"><span data-stu-id="93b98-200">Renamed 'parse()' function too'json()'</span></span>

<span data-ttu-id="93b98-201">Přidáváme další obsah typy brzy, takže jsme přejmenovat hello `parse()` funkce příliš`json()`.</span><span class="sxs-lookup"><span data-stu-id="93b98-201">We are adding more content types soon, so we renamed hello `parse()` function too`json()`.</span></span>

## <a name="coming-soon-enterprise-integration-apis"></a><span data-ttu-id="93b98-202">Připravuje se: Enterprise integrace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="93b98-202">Coming soon: Enterprise Integration APIs</span></span>

<span data-ttu-id="93b98-203">Nemáme spravované verze ještě hello Enterprise integrace rozhraní API, jako je AS2.</span><span class="sxs-lookup"><span data-stu-id="93b98-203">We don't have managed versions yet of hello Enterprise Integration APIs, like AS2.</span></span> <span data-ttu-id="93b98-204">Mezitím můžete svých stávajících nasazené BizTalk rozhraní API prostřednictvím hello akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="93b98-204">Meanwhile, you can use your existing deployed BizTalk APIs through hello HTTP action.</span></span> <span data-ttu-id="93b98-205">Podrobnosti najdete v tématu "Použití již nasazené aplikace API" v hello [integrace plán](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span><span class="sxs-lookup"><span data-stu-id="93b98-205">For details, see "Using your already deployed API apps" in hello [integration roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span></span> 
