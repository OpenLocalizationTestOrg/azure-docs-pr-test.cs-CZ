---
title: "Schéma se aktualizuje srpen-1-2015 preview – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 35d7a56d5607dcc18a4407c65b92962d3d0dcd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a><span data-ttu-id="1b36c-103">Aktualizace schématu pro Azure Logic Apps - 1. srpna 2015 preview</span><span class="sxs-lookup"><span data-stu-id="1b36c-103">Schema updates for Azure Logic Apps - August 1, 2015 preview</span></span>

<span data-ttu-id="1b36c-104">Toto nové schéma a rozhraní API verze pro Azure Logic Apps zahrnuje klíčových vylepšení, které aplikace logiky spolehlivější a jednodušší použít:</span><span class="sxs-lookup"><span data-stu-id="1b36c-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier to use:</span></span>

*   <span data-ttu-id="1b36c-105">**APIApp** typ akce se aktualizuje na nový [ **APIConnection** ](#api-connections) typ akce.</span><span class="sxs-lookup"><span data-stu-id="1b36c-105">The **APIApp** action type is updated to a new [**APIConnection**](#api-connections) action type.</span></span>
*   <span data-ttu-id="1b36c-106">**Opakujte** je přejmenován na [ **Foreach**](#foreach).</span><span class="sxs-lookup"><span data-stu-id="1b36c-106">**Repeat** is renamed to [**Foreach**](#foreach).</span></span>
*   <span data-ttu-id="1b36c-107">[ **Naslouchací proces protokolu HTTP** aplikace API](#http-listener) už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="1b36c-107">The [**HTTP Listener** API App](#http-listener) is no longer required.</span></span>
*   <span data-ttu-id="1b36c-108">Volání metody podřízené pracovní postupy používá [nové schéma](#child-workflows).</span><span class="sxs-lookup"><span data-stu-id="1b36c-108">Calling child workflows uses a [new schema](#child-workflows).</span></span>

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a><span data-ttu-id="1b36c-109">Přesunout do rozhraní API připojení</span><span class="sxs-lookup"><span data-stu-id="1b36c-109">Move to API connections</span></span>

<span data-ttu-id="1b36c-110">Největší změnou je, že už máte k nasazení aplikace API do vašeho předplatného Azure, abyste mohli používat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-110">The biggest change is that you no longer have to deploy API Apps into your Azure subscription so you can use APIs.</span></span> <span data-ttu-id="1b36c-111">Zde jsou způsoby, které můžete použít rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="1b36c-111">Here are the ways that you can use APIs:</span></span>

* <span data-ttu-id="1b36c-112">Spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1b36c-112">Managed APIs</span></span>
* <span data-ttu-id="1b36c-113">Vlastní webová rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-113">Your custom Web APIs</span></span>

<span data-ttu-id="1b36c-114">Každý způsob je zpracovávané trochu jinak, protože jejich správy a hostování modely se liší.</span><span class="sxs-lookup"><span data-stu-id="1b36c-114">Each way is handled slightly differently because their management and hosting models are different.</span></span> <span data-ttu-id="1b36c-115">Jednou z výhod tohoto modelu je, že jste se k prostředkům, které jsou nasazeny ve vaší skupině prostředků Azure už omezená.</span><span class="sxs-lookup"><span data-stu-id="1b36c-115">One advantage of this model is you're no longer constrained to resources that are deployed in your Azure resource group.</span></span> 

### <a name="managed-apis"></a><span data-ttu-id="1b36c-116">Spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1b36c-116">Managed APIs</span></span>

<span data-ttu-id="1b36c-117">Microsoft spravuje některé rozhraní API vaším jménem, jako je například Office 365, Salesforce, Twitter a FTP.</span><span class="sxs-lookup"><span data-stu-id="1b36c-117">Microsoft manages some APIs on your behalf, such as Office 365, Salesforce, Twitter, and FTP.</span></span> <span data-ttu-id="1b36c-118">Můžete použít některé spravovaná rozhraní API jako-je třeba Bing přeložit, zatímco jiné vyžadují konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1b36c-118">You can use some managed APIs as-is, such as Bing Translate, while others require configuration.</span></span> <span data-ttu-id="1b36c-119">Tato konfigurace se nazývá *připojení*.</span><span class="sxs-lookup"><span data-stu-id="1b36c-119">This configuration is called a *connection*.</span></span>

<span data-ttu-id="1b36c-120">Například pokud používáte Office 365, musí vytvořit připojení, který obsahuje znaménko token Office 365.</span><span class="sxs-lookup"><span data-stu-id="1b36c-120">For example, when you use Office 365, you must create a connection that contains your Office 365 sign-in token.</span></span> <span data-ttu-id="1b36c-121">Tento token je bezpečně uložená a aktualizovat tak, aby aplikace logiky můžete vždy volat rozhraní API Office 365.</span><span class="sxs-lookup"><span data-stu-id="1b36c-121">This token is securely stored and refreshed so that your logic app can always call the Office 365 API.</span></span> <span data-ttu-id="1b36c-122">Případně pokud se chcete připojit k serveru SQL nebo FTP, musí vytvořit připojení, který má připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="1b36c-122">Alternatively, if you want to connect to your SQL or FTP server, you must create a connection that has the connection string.</span></span> 

<span data-ttu-id="1b36c-123">V této definici, se nazývají tyto akce `APIConnection`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-123">In this definition, these actions are called `APIConnection`.</span></span> <span data-ttu-id="1b36c-124">Tady je příklad připojení, která volá Office 365 odeslat e-mail:</span><span class="sxs-lookup"><span data-stu-id="1b36c-124">Here is an example of a connection that calls Office 365 to send an email:</span></span>

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

<span data-ttu-id="1b36c-125">`host` Objekt je část vstupy, kterou je jedinečné pro připojení k rozhraní API a obsahuje tažení částí: `api` a `connection`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-125">The `host` object is portion of inputs that is unique to API connections, and contains tow parts: `api` and `connection`.</span></span>

<span data-ttu-id="1b36c-126">`api` Má modul runtime je adresa URL, kde spravované rozhraní API hostovaná.</span><span class="sxs-lookup"><span data-stu-id="1b36c-126">The `api` has the runtime URL of where that managed API is hosted.</span></span> <span data-ttu-id="1b36c-127">Zobrazí všechny dostupné spravované rozhraní API voláním `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-127">You can see all the available managed APIs by calling `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span></span>

<span data-ttu-id="1b36c-128">Pokud používáte rozhraní API, rozhraní API může nebo nemusí mít žádný *parametrů připojení* definované.</span><span class="sxs-lookup"><span data-stu-id="1b36c-128">When you use an API, the API might or might not have any *connection parameters* defined.</span></span> <span data-ttu-id="1b36c-129">Pokud není rozhraní API, žádné *připojení* je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="1b36c-129">If the API doesn't, no *connection* is required.</span></span> <span data-ttu-id="1b36c-130">Pokud je rozhraní API, musíte vytvořit připojení.</span><span class="sxs-lookup"><span data-stu-id="1b36c-130">If the API does, you must create a connection.</span></span> <span data-ttu-id="1b36c-131">Vytvořené připojení má název, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="1b36c-131">The created connection has the name that you choose.</span></span> <span data-ttu-id="1b36c-132">Pak odkazovat na název v `connection` objektu uvnitř `host` objektu.</span><span class="sxs-lookup"><span data-stu-id="1b36c-132">You then reference the name in the `connection` object inside the `host` object.</span></span> <span data-ttu-id="1b36c-133">Chcete-li vytvořit připojení ve skupině prostředků, volejte:</span><span class="sxs-lookup"><span data-stu-id="1b36c-133">To create a connection in a resource group, call:</span></span>

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

<span data-ttu-id="1b36c-134">Spolu s následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="1b36c-134">With the following body:</span></span>

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a><span data-ttu-id="1b36c-135">Nasadit spravované rozhraní API v šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1b36c-135">Deploy managed APIs in an Azure Resource Manager template</span></span>

<span data-ttu-id="1b36c-136">Celou aplikaci můžete vytvořit v šablonu Azure Resource Manager, dokud interaktivní přihlašování není povinné.</span><span class="sxs-lookup"><span data-stu-id="1b36c-136">You can create a full application in an Azure Resource Manager template as long as interactive sign-in isn't required.</span></span>
<span data-ttu-id="1b36c-137">Pokud přihlášení je potřeba, můžete nastavit všechno pomocí šablony Azure Resource Manager, ale máte přejděte na portál pro autorizaci připojení.</span><span class="sxs-lookup"><span data-stu-id="1b36c-137">If sign-in is required, you can set up everything with the Azure Resource Manager template, but you still have to visit the portal to authorize the connections.</span></span> 

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

<span data-ttu-id="1b36c-138">Zobrazí se v tomto příkladu, že připojení jsou jenom prostředky, které za provozu ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="1b36c-138">You can see in this example that the connections are just resources that live in your resource group.</span></span> <span data-ttu-id="1b36c-139">Jejich referenční spravovaná rozhraní API k dispozici v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="1b36c-139">They reference the managed APIs available to you in your subscription.</span></span>

### <a name="your-custom-web-apis"></a><span data-ttu-id="1b36c-140">Vlastní webová rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-140">Your custom Web APIs</span></span>

<span data-ttu-id="1b36c-141">Pokud používáte vlastní rozhraní API, není spravovaný společností Microsoft ty použít integrované **HTTP** akce je volat.</span><span class="sxs-lookup"><span data-stu-id="1b36c-141">If you use your own APIs, not Microsoft-managed ones, use the built-in **HTTP** action to call them.</span></span> <span data-ttu-id="1b36c-142">Pro ideální prostředí by měl vystavit koncový bod Swagger pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-142">For an ideal experience, you should expose a Swagger endpoint for your API.</span></span> <span data-ttu-id="1b36c-143">Tento koncový bod umožňuje návrháře logiku aplikace k vykreslení vstupy a výstupy pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-143">This endpoint enables the Logic App Designer to render the inputs and outputs for your API.</span></span> <span data-ttu-id="1b36c-144">Bez Swagger návrháře lze zobrazit pouze vstupy a výstupy jako neprůhledné objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="1b36c-144">Without Swagger, the designer can only show the inputs and outputs as opaque JSON objects.</span></span>

<span data-ttu-id="1b36c-145">Tady je příklad zobrazuje nové `metadata.apiDefinitionUrl` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="1b36c-145">Here is an example showing the new `metadata.apiDefinitionUrl` property:</span></span>

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

<span data-ttu-id="1b36c-146">Pokud hostujete webové rozhraní API v Azure App Service, Web API se automaticky zobrazí v seznamu akcí, které jsou k dispozici v návrháři.</span><span class="sxs-lookup"><span data-stu-id="1b36c-146">If you host your Web API on Azure App Service, your Web API automatically appears in the list of actions available in the designer.</span></span> <span data-ttu-id="1b36c-147">Pokud ne, je nutné vložit v adrese URL přímo.</span><span class="sxs-lookup"><span data-stu-id="1b36c-147">If not, you have to paste in the URL directly.</span></span> <span data-ttu-id="1b36c-148">Koncový bod Swagger musí neověřené možné používat v návrháři aplikace logiky, i když můžete zabezpečit samotné rozhraní API pomocí jakékoli metody, které podporuje Swagger.</span><span class="sxs-lookup"><span data-stu-id="1b36c-148">The Swagger endpoint must be unauthenticated to be usable in the Logic App Designer, although you can secure the API itself with whatever methods that Swagger supports.</span></span>

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a><span data-ttu-id="1b36c-149">Volání nasazené aplikace API s 2015-08-01-preview</span><span class="sxs-lookup"><span data-stu-id="1b36c-149">Call deployed API apps with 2015-08-01-preview</span></span>

<span data-ttu-id="1b36c-150">Pokud jste nasadili aplikace API, můžete zavolat aplikaci pomocí **HTTP** akce.</span><span class="sxs-lookup"><span data-stu-id="1b36c-150">If you previously deployed an API App, you can call the app with the **HTTP** action.</span></span>

<span data-ttu-id="1b36c-151">Pokud používáte Dropbox seznam souborů, například vaše **2014-12-01-preview** definice schématu verze může mít něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="1b36c-151">For example, if you use Dropbox to list files, your **2014-12-01-preview** schema version definition might have something like:</span></span>

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

<span data-ttu-id="1b36c-152">Ekvivalentní akci HTTP, jako je například můžete vytvořit při části Parametry definici aplikace logiky zůstává beze změny:</span><span class="sxs-lookup"><span data-stu-id="1b36c-152">You can construct the equivalent HTTP action like this example, while the parameters section of the Logic app definition remains unchanged:</span></span>

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

<span data-ttu-id="1b36c-153">Tyto vlastnosti po jednom s návodem:</span><span class="sxs-lookup"><span data-stu-id="1b36c-153">Walking through these properties one-by-one:</span></span>

| <span data-ttu-id="1b36c-154">Vlastnosti akce.</span><span class="sxs-lookup"><span data-stu-id="1b36c-154">Action property</span></span> | <span data-ttu-id="1b36c-155">Popis</span><span class="sxs-lookup"><span data-stu-id="1b36c-155">Description</span></span> |
| --- | --- |
| `type` |<span data-ttu-id="1b36c-156">`Http`Namísto`APIapp`</span><span class="sxs-lookup"><span data-stu-id="1b36c-156">`Http` instead of `APIapp`</span></span> |
| `metadata.apiDefinitionUrl` |<span data-ttu-id="1b36c-157">Pokud chcete používat tuto akci v návrháři aplikace logiky, patří koncový bod metadat, které se vytvářejí na základě:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span><span class="sxs-lookup"><span data-stu-id="1b36c-157">To use this action in the Logic App Designer, include the metadata endpoint, which is constructed from: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span></span> |
| `inputs.uri` |<span data-ttu-id="1b36c-158">Vytvářejí na základě:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14`</span><span class="sxs-lookup"><span data-stu-id="1b36c-158">Constructed from: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14`</span></span> |
| `inputs.method` |<span data-ttu-id="1b36c-159">Vždy`POST`</span><span class="sxs-lookup"><span data-stu-id="1b36c-159">Always `POST`</span></span> |
| `inputs.body` |<span data-ttu-id="1b36c-160">Stejný jako parametry aplikace API</span><span class="sxs-lookup"><span data-stu-id="1b36c-160">Identical to the API App parameters</span></span> |
| `inputs.authentication` |<span data-ttu-id="1b36c-161">Shodné s ověřováním aplikace API</span><span class="sxs-lookup"><span data-stu-id="1b36c-161">Identical to the API App authentication</span></span> |

<span data-ttu-id="1b36c-162">Tento postup by měly fungovat pro všechny akce aplikace API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-162">This approach should work for all API App actions.</span></span> <span data-ttu-id="1b36c-163">Nezapomeňte však, že tato předchozí aplikace API již nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="1b36c-163">However, remember that these previous API Apps are no longer supported.</span></span> <span data-ttu-id="1b36c-164">Proto byste měli přesunout do jednoho z následujících dvou dalších předchozích možností, spravované rozhraní API nebo hostující vaše vlastní webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b36c-164">So you should move to one of the two other previous options, a managed API or hosting your custom Web API.</span></span>

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a><span data-ttu-id="1b36c-165">Přejmenovat 'opakovat' do 'foreach.</span><span class="sxs-lookup"><span data-stu-id="1b36c-165">Renamed 'repeat' to 'foreach'</span></span>

<span data-ttu-id="1b36c-166">Pro předchozí verze schématu dostali jsme mnohem názory zákazníků, **opakovat** bylo matoucí a nebyla zachycení správně, **opakujte** je skutečně pro každou smyčku.</span><span class="sxs-lookup"><span data-stu-id="1b36c-166">For the previous schema version, we received much customer feedback that **Repeat** was confusing and didn't properly capture that **Repeat** was really a for-each loop.</span></span> <span data-ttu-id="1b36c-167">V důsledku toho jsme přejmenovaná `repeat` k `foreach`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-167">As a result, we have renamed `repeat` to `foreach`.</span></span> <span data-ttu-id="1b36c-168">Například dříve jste by zápisu:</span><span class="sxs-lookup"><span data-stu-id="1b36c-168">For example, previously you would write:</span></span>

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

<span data-ttu-id="1b36c-169">Teď by zápisu:</span><span class="sxs-lookup"><span data-stu-id="1b36c-169">Now you would write:</span></span>

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

<span data-ttu-id="1b36c-170">Funkce `@repeatItem()` byl dříve používán k odkazování s aktuální položkou. probíhá přes vstupní.</span><span class="sxs-lookup"><span data-stu-id="1b36c-170">The function `@repeatItem()` was previously used to reference the current item being iterated over.</span></span> <span data-ttu-id="1b36c-171">Tato funkce je nyní zjednodušenou `@item()`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-171">This function is now simplified to `@item()`.</span></span> 

### <a name="reference-outputs-from-foreach"></a><span data-ttu-id="1b36c-172">Referenční dokumentace výstupy z 'foreach.</span><span class="sxs-lookup"><span data-stu-id="1b36c-172">Reference outputs from 'foreach'</span></span>

<span data-ttu-id="1b36c-173">Pro zjednodušení, výstupy z `foreach` akce nejsou uzavřen do objektu s názvem `repeatItems`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-173">For simplification, the outputs from `foreach` actions are not wrapped in an object called `repeatItems`.</span></span> <span data-ttu-id="1b36c-174">Při výstupy z předchozí `repeat` byly příklad:</span><span class="sxs-lookup"><span data-stu-id="1b36c-174">While the outputs from the previous `repeat` example were:</span></span>

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

<span data-ttu-id="1b36c-175">Nyní jsou tyto výstupy:</span><span class="sxs-lookup"><span data-stu-id="1b36c-175">Now these outputs are:</span></span>

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

<span data-ttu-id="1b36c-176">Dříve Chcete-li získat k tělu akce při odkazování na těchto výstupů:</span><span class="sxs-lookup"><span data-stu-id="1b36c-176">Previously, to get to the body of the action when referencing these outputs:</span></span>

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

<span data-ttu-id="1b36c-177">Teď můžete místo toho provést:</span><span class="sxs-lookup"><span data-stu-id="1b36c-177">Now you can do instead:</span></span>

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

<span data-ttu-id="1b36c-178">Tyto změny funkce `@repeatItem()`, `@repeatBody()`, a `@repeatOutputs()` se odeberou.</span><span class="sxs-lookup"><span data-stu-id="1b36c-178">With these changes, the functions `@repeatItem()`, `@repeatBody()`, and `@repeatOutputs()` are removed.</span></span>

<a name="http-listener"></a>
## <a name="native-http-listener"></a><span data-ttu-id="1b36c-179">Nativní naslouchací proces protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="1b36c-179">Native HTTP listener</span></span>

<span data-ttu-id="1b36c-180">Možnosti naslouchací proces protokolu HTTP jsou teď součástí.</span><span class="sxs-lookup"><span data-stu-id="1b36c-180">The HTTP Listener capabilities are now built in.</span></span> <span data-ttu-id="1b36c-181">Proto již nepotřebujete k nasazení aplikace API naslouchací proces protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b36c-181">So you no longer need to deploy an HTTP Listener API App.</span></span> <span data-ttu-id="1b36c-182">V tématu [úplné podrobnosti o tom váš koncový bod aplikace logiky s zde](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="1b36c-182">See [the full details for how to make your Logic app endpoint callable here](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<span data-ttu-id="1b36c-183">Tyto změny, jsme odebrali `@accessKeys()` funkce, které bylo nahrazeno `@listCallbackURL()` funkce pro získání koncového bodu, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="1b36c-183">With these changes, we removed the `@accessKeys()` function, which we replaced with the `@listCallbackURL()` function for getting the endpoint when necessary.</span></span> <span data-ttu-id="1b36c-184">Navíc je nutné nyní zadat nejméně jedna aktivační událost v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1b36c-184">Also, you must now define at least one trigger in your logic app.</span></span> <span data-ttu-id="1b36c-185">Pokud chcete `/run` pracovní postup, musí mít jednu z těchto aktivační události: `manual`, `apiConnectionWebhook`, nebo `httpWebhook`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-185">If you want to `/run` the workflow, you must have one of these triggers: `manual`, `apiConnectionWebhook`, or `httpWebhook`.</span></span>

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a><span data-ttu-id="1b36c-186">Volat podřízené pracovní postupy</span><span class="sxs-lookup"><span data-stu-id="1b36c-186">Call child workflows</span></span>

<span data-ttu-id="1b36c-187">Volání podřízených pracovních požadované dříve, přejdete do pracovního postupu, získání tokenu přístupu a vkládání token v definici aplikace logiky, kde chcete volat podřízené postupu.</span><span class="sxs-lookup"><span data-stu-id="1b36c-187">Previously, calling child workflows required going to the workflow, getting the access token, and pasting the token in the logic app definition where you want to call that child workflow.</span></span> <span data-ttu-id="1b36c-188">S tímto schématem modul Logic Apps automaticky vygeneruje SAS v době běhu pro podřízené pracovní postup, nemusíte vložte všech tajných klíčů do definice.</span><span class="sxs-lookup"><span data-stu-id="1b36c-188">With the new schema, the Logic Apps engine automatically generates a SAS at runtime for the child workflow so you don't have to paste any secrets into the definition.</span></span> <span data-ttu-id="1b36c-189">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="1b36c-189">Here is an example:</span></span>

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

<span data-ttu-id="1b36c-190">Druhý zlepšování je že nabízíme podřízených pracovních úplný přístup k příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="1b36c-190">A second improvement is we are giving the child workflows full access to the incoming request.</span></span> <span data-ttu-id="1b36c-191">To znamená, že můžete předat parametry v *dotazy* části a v *hlavičky* objekt a že můžete definovat plně celého obsahu.</span><span class="sxs-lookup"><span data-stu-id="1b36c-191">That means that you can pass parameters in the *queries* section and in the *headers* object and that you can fully define the entire body.</span></span>

<span data-ttu-id="1b36c-192">Nakonec nejsou požadované změny pro podřízený pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="1b36c-192">Finally, there are required changes to the child workflow.</span></span> <span data-ttu-id="1b36c-193">Při podřízený pracovní postup může volat dříve přímo, nyní je nutné definovat koncový bod aktivační událost v pracovním postupu pro nadřazený prvek volat.</span><span class="sxs-lookup"><span data-stu-id="1b36c-193">While you could previously call a child workflow directly, now you must define a trigger endpoint in the workflow for the parent to call.</span></span> <span data-ttu-id="1b36c-194">Obecně platí, měli byste přidat aktivační událost, která má `manual` zadejte a potom pomocí této aktivační události v nadřazené definici.</span><span class="sxs-lookup"><span data-stu-id="1b36c-194">Generally, you would add a trigger that has `manual` type, and then use that trigger in the parent definition.</span></span> <span data-ttu-id="1b36c-195">Poznámka: `host` vlastnost konkrétně má `triggerName` vzhledem k tomu, že musíte určit které aktivační událost je vyvolán.</span><span class="sxs-lookup"><span data-stu-id="1b36c-195">Note the `host` property specifically has a `triggerName` because you must always specify which trigger you are invoking.</span></span>

## <a name="other-changes"></a><span data-ttu-id="1b36c-196">Další změny</span><span class="sxs-lookup"><span data-stu-id="1b36c-196">Other changes</span></span>

### <a name="new-queries-property"></a><span data-ttu-id="1b36c-197">Nové vlastnosti 'dotazy.</span><span class="sxs-lookup"><span data-stu-id="1b36c-197">New 'queries' property</span></span>

<span data-ttu-id="1b36c-198">Všechny typy akcí teď podporují nové vstup názvem `queries`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-198">All action types now support a new input called `queries`.</span></span> <span data-ttu-id="1b36c-199">Tento vstup může být strukturovaný objekt, místo nutnosti ke kompilaci řetězec ručně.</span><span class="sxs-lookup"><span data-stu-id="1b36c-199">This input can be a structured object, rather than you having to assemble the string by hand.</span></span>

### <a name="renamed-parse-function-to-json"></a><span data-ttu-id="1b36c-200">Funkce přejmenován 'parse()' 'json().</span><span class="sxs-lookup"><span data-stu-id="1b36c-200">Renamed 'parse()' function to 'json()'</span></span>

<span data-ttu-id="1b36c-201">Přidáváme další obsah typy brzy, takže jsme přejmenován `parse()` funkci, aby se `json()`.</span><span class="sxs-lookup"><span data-stu-id="1b36c-201">We are adding more content types soon, so we renamed the `parse()` function to `json()`.</span></span>

## <a name="coming-soon-enterprise-integration-apis"></a><span data-ttu-id="1b36c-202">Připravuje se: Enterprise integrace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1b36c-202">Coming soon: Enterprise Integration APIs</span></span>

<span data-ttu-id="1b36c-203">Nemáme spravované verze ještě Enterprise integrace rozhraní API, jako je AS2.</span><span class="sxs-lookup"><span data-stu-id="1b36c-203">We don't have managed versions yet of the Enterprise Integration APIs, like AS2.</span></span> <span data-ttu-id="1b36c-204">Mezitím můžete vaší existující nasazené API BizTalk pomocí akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b36c-204">Meanwhile, you can use your existing deployed BizTalk APIs through the HTTP action.</span></span> <span data-ttu-id="1b36c-205">Podrobnosti najdete v tématu "Použití již nasazené aplikace API" v [integrace plán](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span><span class="sxs-lookup"><span data-stu-id="1b36c-205">For details, see "Using your already deployed API apps" in the [integration roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span></span> 
