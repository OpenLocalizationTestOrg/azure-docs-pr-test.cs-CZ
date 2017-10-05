---
title: "Odstraňování běžných chyb nasazení Azure | Microsoft Docs"
description: "Popisuje postup řešení běžných chyb při nasazování prostředků do Azure pomocí Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení do azure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 30adc10d01290f14a3e116813b19916fa36ab0bc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru
Toto téma popisuje, jak můžete vyřešit některé běžné chyby nasazení Azure se můžete setkat.

V tomto tématu jsou popsány následující kódy chyb:

* [AccountNameInvalid](#accountnameinvalid)
* [Ověření se nezdařilo](#authorization-failed)
* [Struktura BadRequest](#badrequest)
* [DeploymentFailed](#deploymentfailed)
* [DisallowedOperation](#disallowedoperation)
* [InvalidContentLink](#invalidcontentlink)
* [InvalidTemplate](#invalidtemplate)
* [MissingSubscriptionRegistration](#noregisteredproviderfound)
* [NotFound](#notfound)
* [NoRegisteredProviderFound](#noregisteredproviderfound)
* [OperationNotAllowed](#quotaexceeded)
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded)
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy)
* [ResourceNotFound](#notfound)
* [SkuNotAvailable](#skunotavailable)
* [StorageAccountAlreadyExists](#storagenamenotunique)
* [StorageAccountAlreadyTaken](#storagenamenotunique)

## <a name="deploymentfailed"></a>DeploymentFailed

Tento kód chyby označuje Chyba obecné nasazení, ale není kód chyby, které je třeba spustit Poradce při potížích s. Kód chyby, která ve skutečnosti pomáhá vyřešit tento problém je obvykle jednu úroveň pod tuto chybu. Například na následujícím obrázku **RequestDisallowedByPolicy** kód chyby, který je pod chyba nasazení.

![Zobrazí kód chyby](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a>SkuNotAvailable

Pokud nasazujete prostředku (obvykle virtuální počítač), může se zobrazit následující kód chyby a chybová zpráva:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

Tato chyba se zobrazí, když prostředek SKU, které jste vybrali (například velikost virtuálního počítače) není k dispozici pro umístění, které jste vybrali. Chcete-li vyřešit tento problém, musíte určit, která SKU jsou k dispozici v oblasti. Operaci REST, na portálu nebo prostředí PowerShell můžete použít k vyhledání dostupné edice.

- Pro prostředí PowerShell, použijte [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) a filtrovat podle umístění. Musíte mít nejnovější verzi prostředí PowerShell pro tento příkaz.

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  Výsledky budou zahrnovat seznam SKU pro umístění a nějaká omezení pro tento SKU.

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- Použít [portál](https://portal.azure.com), přihlaste se k portálu a přidat prostředek přes rozhraní. Při nastavování hodnoty, uvidíte dostupné edice pro tento prostředek. Není nutné k dokončení nasazení.

    ![dostupné edice](./media/resource-manager-common-deployment-errors/view-sku.png)

- Použití rozhraní REST API pro virtuální počítače, odešlete požadavek na následující:

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  Vrátí k dispozici SKU a oblastí, v následujícím formátu:

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

Pokud jste se nepodařilo najít vhodnou SKU v této oblasti nebo alternativní oblast, která splňuje vaše obchodní potřebuje, odeslání [SKU požadavek](https://aka.ms/skurestriction) pro podporu Azure.

## <a name="disallowedoperation"></a>DisallowedOperation

```
Code: DisallowedOperation
Message: The current subscription type is not permitted to perform operations on any provider 
namespace. Please use a different subscription.
```

Pokud se zobrazí tato chyba, používáte odběr, který nemá oprávnění k přístupu služby Azure než Azure Active Directory. Pokud potřebujete přístup k portálu classic, ale nejsou povoleny pro nasazení prostředků můžete mít tento typ předplatného. Chcete-li vyřešit tento problém, musíte použít odběr, který má oprávnění k nasazení prostředků.  

Chcete-li zobrazit předplatné k dispozici v prostředí PowerShell, použijte:

```powershell
Get-AzureRmSubscription
```

A pokud chcete nastavit aktuální předplatné, použijte:

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

Chcete-li zobrazit dostupné předplatné s Azure CLI 2.0, použijte:

```azurecli
az account list
```

A pokud chcete nastavit aktuální předplatné, použijte:

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a>InvalidTemplate
Tato chyba může způsobit z několika různých typů chyb.

- Chyba syntaxe

   Pokud se zobrazí chybovou zprávu, která určuje ověření šablony se nezdařilo, bude pravděpodobně problém syntaxe ve vaší šabloně.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   Tato chyba je snadné provést, protože může být složité výrazy šablony. Například následující přiřazení název pro účet úložiště obsahuje jednu sadu závorky, tři funkce, tři sady závorky, jednu sadu jednoduchých uvozovek a jednu vlastnost:

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   Pokud nezadáte syntaxe porovnávání, vytvoří šablona hodnotu, která se liší od svůj záměr.

   Po zobrazení tohoto typu chyby, pečlivě zkontrolujte syntaxi výrazu. Zvažte použití editor JSON jako [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) nebo [Visual Studio Code](resource-manager-vs-code.md), která vás upozorní chyby syntaxe.

- Segment nesprávné délky

   Další neplatný šablony chyba nastane, když název prostředku není ve správném formátu.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'The template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   Kořenové úrovně prostředku musí mít jeden menší segmentu v názvu než v typ prostředku. Každý segment je rozlišené pomocí lomítko. V následujícím příkladu, typ má dva segmenty a název jednoho segmentu, tak, aby byl **platný název**.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   Dalším příkladem je, ale **není platný název** protože má stejný počet segmentů jako typ.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   Pro podřízené prostředky typ a název mít stejný počet segmentů. Tento počet segmentů dává smysl, protože úplný název a typ pro podřízený objekt zahrnuje název nadřazeného a typu. Úplný název proto má stále jednoho menší segmentu než úplné typu.

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   Získávání segmentů správné může být složité s typy Resource Manager, které se použijí napříč zprostředkovatelé prostředků. Například použití prostředků zámku na web vyžaduje typ s čtyři segmenty. Název je tedy tři segmenty:

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- Kopírování indexu není očekáván.

   Narazíte na to **InvalidTemplate** chyba, když jste použili **kopie** element součástí šablony, která nepodporuje tento element. Element kopie lze použít pouze pro typ prostředku. Kopírování nelze použít na vlastnost v rámci typu prostředku. Například použijete kopie virtuálního počítače, ale nelze ho použít pro disky operačního systému pro virtuální počítač. V některých případech může převést prostředek podřízené na nadřazený prostředek pro vytvoření kopie smyčky. Další informace o používání kopírování najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).

- Parametr není platný

   Pokud šablona specifikuje povolené hodnoty pro parametr, a zadáte hodnotu, která není jedním z těchto hodnot, zobrazí se zpráva podobná následující chybě:

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'The provided value {parameter value}
  for the template parameter {parameter name} is not valid. The parameter value is not
  part of the allowed values
  ``` 

   Double zkontrolujte povolené hodnoty v šabloně a zadejte jeden během nasazení.

- Zjistila se cyklická závislost

   Tato chyba se zobrazí, když prostředky jsou vzájemně závislé způsobem, který zabrání spuštění nasazení. Kombinace vzájemné závislosti díky dvou nebo více prostředků čekat na další prostředky, které jsou také čekání. Například resource1 závisí na resource3, resource2 závisí na resource1 a resource3 závisí na resource2. Obvykle můžete tento problém vyřešit odstraněním nepotřebných závislosti. 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound a ResourceNotFound
Pokud vaše šablona zahrnuje název prostředku, které nelze vyřešit, obdržíte chybu podobně jako:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Pokud se pokoušíte nasadit chybějící prostředek v šabloně, zkontrolujte, jestli je potřeba přidat závislost. Správce prostředků optimalizuje nasazení tak, že vytvoříte prostředky paralelně, pokud je to možné. Pokud po jiný prostředek musí být nasazený jeden prostředek, budete muset použít **dependsOn** element v šabloně pro vytvoření závislosti na jiný prostředek. Například Pokud nasazujete webovou aplikaci, musí existovat plán služby App Service. Pokud jste nezadali, že webová aplikace závisí na plán služby App Service, vytvoří Resource Manager i prostředky ve stejnou dobu. Zobrazí se chyba oznamující, že nelze najít prostředek plánu služby App Service, protože neexistuje ještě při pokusu o nastavení vlastnosti ve webové aplikaci. Tuto chybu můžete zabránit nastavením závislost ve webové aplikaci.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Návrhy na řešení potíží s chybami závislostí, najdete v části [Zkontrolujte pořadí nasazení](#check-deployment-sequence).

Můžete také zobrazit chyba při prostředek existuje v jiné skupině prostředků než ten, který nasazován. V takovém případě použijte [resourceId funkce](resource-group-template-functions-resource.md#resourceid) získat plně kvalifikovaný název prostředku.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

Pokud pokus o použití [odkaz](resource-group-template-functions-resource.md#reference) nebo [listKeys](resource-group-template-functions-resource.md#listkeys) funkce s prostředky, které nelze vyřešit, zobrazí se následující chyba:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

Vyhledat výraz, který zahrnuje **odkaz** funkce. Zkontrolujte, zda hodnoty parametrů jsou správné.

## <a name="parentresourcenotfound"></a>ParentResourceNotFound

Pokud jeden prostředek je nadřazený na jiný prostředek, musí existovat nadřazený prostředek před vytvořením podřízené prostředků. Pokud ještě neexistuje, zobrazí se následující chyba:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

Název prostředku podřízené obsahuje název nadřazené. Například může být definovaná SQL Database jako:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Ale pokud nezadáte závislost na nadřazeném prostředku, může získat prostředek podřízené nasadit před nadřazený. Chcete-li tuto chybu vyřešit, obsahovat závislost.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists a StorageAccountAlreadyTaken
Pro účty úložiště je nutné zadat název pro prostředek, který je v rámci Azure jedinečný. Pokud nezadáte jedinečný název, zobrazí chybu jako:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Můžete vytvořit jedinečný název zřetězení vaše zásady vytváření názvů s výsledkem [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Pokud nasazení účtu úložiště se stejným názvem jako stávající účet úložiště v rámci vašeho předplatného, ale zadejte jiné umístění, zobrazí se chyba oznamující, že účet úložiště již existuje v jiném umístění. Buď existující účet úložiště odstranit, nebo zadejte stejné umístění jako stávající účet úložiště.

## <a name="accountnameinvalid"></a>AccountNameInvalid
Zobrazí **AccountNameInvalid** Chyba při pokusu o účtu úložiště poskytnout název, který obsahuje zakázané znaky. Názvy účtů úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena. [UniqueString](resource-group-template-functions-string.md#uniquestring) funkce vrátí 13 znaků. Pokud řetězení předpony **uniqueString** vést, zadejte předponu, která je 11 znaků nebo méně.

## <a name="badrequest"></a>Struktura BadRequest

Struktura BadRequest stav se můžete setkat, když zadáte neplatnou hodnotu pro vlastnost. Například pokud jste zadali nesprávnou hodnotu SKU pro účet úložiště, nasazení se nezdaří. Chcete-li určit platné hodnoty pro vlastnost, podívejte se [REST API](/rest/api) pro typ prostředku, kterou nasazujete.

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>NoRegisteredProviderFound a MissingSubscriptionRegistration
Při nasazování prostředků, může se zobrazit následující kód chyby a zpráva:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Nebo se může zobrazit podobné zpráva, že:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Zobrazí tyto chyby pro jednu ze tří důvodů:

1. Zprostředkovatel prostředků není zaregistrován pro vaše předplatné
2. Verze rozhraní API není podporována pro typ prostředku
3. Umístění není podporováno pro typ prostředku

Chybová zpráva by měla dát návrhy na podporovaném umístění a verze rozhraní API. Šablony můžete změnit na jednu z Tyhle navrhované hodnoty. Většina poskytovatelé jsou registrované automaticky pomocí portálu Azure nebo rozhraní příkazového řádku, který používáte, ale ne všechny. Pokud jste nepoužili poskytovatele určitému prostředku před, musíte k registraci tohoto zprostředkovatele. Může zjišťovat informace o poskytovatelích prostředků prostřednictvím PowerShell nebo rozhraní příkazového řádku Azure.

**Azure Portal**

Můžete zobrazit stav registrace a zaregistrovat obor názvů zprostředkovatele prostředků prostřednictvím portálu.

1. Pro vaše předplatné, vyberte **zprostředkovatelé prostředků**.

   ![Vyberte poskytovatele prostředků](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Podívejte se na seznam poskytovatelů prostředků a v případě potřeby vyberte **zaregistrovat** odkaz k registraci poskytovatele prostředků typu, který se snažíte nasadit.

   ![Zobrazit seznam poskytovatelů prostředků](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

Pokud chcete zobrazit stav vaší registrace, použijte **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Chcete-li zaregistrovat zprostředkovatele, použijte **Register-AzureRmResourceProvider** a zadejte název zprostředkovatele prostředků, které chcete zaregistrovat.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Chcete-li získat podporovaná umístění pro konkrétní typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Podporované verze rozhraní API pro konkrétní typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Azure CLI**

Chcete-li zjistit, zda zprostředkovatel registroval, použijte `azure provider list` příkaz.

```azurecli
az provider list
```

Registrace poskytovatele prostředků, použijte `azure provider register` příkaz a zadejte *obor názvů* k registraci.

```azurecli
az provider register --namespace Microsoft.Cdn
```

Pokud chcete zobrazit podporovaná umístění a verze rozhraní API pro typ prostředku, použijte:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded a OperationNotAllowed
Pokud nasazení překročí kvótu, která může být na skupinu prostředků, odběry, účty a další obory, může mít problémy. Omezit počet jader pro oblast lze nakonfigurovat například vaše předplatné. Pokud se pokusíte nasazení virtuálního počítače s více jader, než povolené množství, obdržíte chybu oznamující, že byla překročena kvóta.
Dokončení kvóty informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

Chcete-li prozkoumat vaše předplatné kvóty pro počet jader, můžete použít `azure vm list-usage` v Azure CLI. Následující příklad ukazuje, že základní kvóta pro bezplatný zkušební účet je 4:

```azurecli
az vm list-usage --location "South Central US"
```

Která vrací:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

Pokud nasadíte šablonu, která vytvoří více než čtyři jader v oblasti západní USA, dojde k chybě nasazení, bude vypadat takto:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Nebo v prostředí PowerShell, můžete použít **Get-AzureRmVMUsage** rutiny.

```powershell
Get-AzureRmVMUsage
```

Která vrací:

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

V těchto případech by měl přejděte na portál a souborů podpory problém ke zvýšení vaší kvóty pro oblast, do které chcete nasadit.

> [!NOTE]
> Nezapomeňte, že pro skupiny prostředků, se kvóty pro každou oblast jednotlivých, ne pro celé předplatné. Pokud potřebujete nasadit 30 jader v západní USA, budete muset požádat o 30 Resource Manager jader v západní USA. Pokud potřebujete nasadit 30 jader v některém z oblasti, ke které máte přístup, je třeba požádat o 30 jader Resource Manager ve všech oblastech.
>
>

## <a name="invalidcontentlink"></a>InvalidContentLink
Když zobrazí chybová zpráva:

```
Code=InvalidContentLink
Message=Unable to download deployment content from ...
```

Pokusili jste s největší pravděpodobností propojení vnořené šablony, která není k dispozici. Překontrolujte identifikátor URI, který jste zadali pro vnořené šablonu. Pokud šablona již existuje v účtu úložiště, zkontrolujte, zda že identifikátor URI je přístupný. Potřebujete předat SAS token. Další informace najdete v tématu věnovaném [použití propojených šablon s Azure Resource Managerem](resource-group-linked-templates.md).

## <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Tato chyba se zobrazí, když vaše předplatné zahrnuje zásady prostředků, které brání akce, které se pokoušíte provést během nasazení. V chybové zprávě vyhledejte identifikátor zásad.

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

V **prostředí PowerShell**, poskytovat identifikátor zásady jako **Id** parametr načíst podrobnosti o zásady, které blokované vaše nasazení.

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

V **rozhraní příkazového řádku Azure**, zadejte název definice zásady:

```azurecli
az policy definition show --name regionPolicyAssignment
```

Další informace najdete v následujících článcích:

- [Chyba RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
- [Použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).

## <a name="authorization-failed"></a>Ověření se nezdařilo
Může zobrazit chybová během nasazení, protože účet nebo instanční objekt pokusu o nasazení prostředků nemá přístup k provedení těchto akcí. Azure Active Directory, které vám umožní nebo správce pro řízení identit, které můžete přístup prostředky s vysoký stupeň přesnosti. Například pokud váš účet je přiřazen k roli čtečky, nejste schopni vytvořit prostředky. V takovém případě zobrazí chybová zpráva oznamující, že autorizace se nezdařila.

Další informace o řízení přístupu na základě rolí najdete v tématu [řízení přístupu](../active-directory/role-based-access-control-configure.md).


## <a name="next-steps"></a>Další kroky
* Další informace o auditování akce najdete v tématu [auditovat operace s Resource Managerem](resource-group-audit.md).
* Další informace o akce k určení chyby během nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
