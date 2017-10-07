---
title: "běžné chyby nasazení Azure aaaTroubleshoot | Microsoft Docs"
description: "Popisuje, jak tooresolve běžných chyb při nasazování tooAzure prostředků pomocí Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení tooazure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 8571e9941879eb5586e4258a785b6a09247da771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru
Toto téma popisuje, jak můžete vyřešit některé běžné chyby nasazení Azure se můžete setkat.

v tomto tématu jsou popsány Hello následující kódy chyb:

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

Tento kód chyby označuje Chyba obecné nasazení, ale není potřebujete toostart řešení potíží s kódem chyby hello. Kód chyby Hello, která ve skutečnosti pomáhá hello problém vyřešit, je obvykle o jednu úroveň pod tuto chybu. Například hello následující obrázek ukazuje hello **RequestDisallowedByPolicy** kód chyby, který je pod hello chyba nasazení.

![Zobrazí kód chyby](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a>SkuNotAvailable

Při nasazování prostředku (obvykle virtuální počítač), může se zobrazit následující chyba kódu a chybové zprávy hello:

```
Code: SkuNotAvailable
Message: hello requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy tooa different location.
```

Tato chyba se zobrazí, když prostředek hello SKU, které jste vybrali (například velikost virtuálního počítače) není k dispozici pro umístění hello, které jste vybrali. tooresolve tento problém, je nutné toodetermine, které SKU jsou k dispozici v oblasti. Můžete použít prostředí PowerShell, hello portálu nebo toofind operace REST dostupné edice.

- Pro prostředí PowerShell, použijte [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) a filtrovat podle umístění. Musíte mít hello nejnovější verzi prostředí PowerShell pro tento příkaz.

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  Hello výsledky obsahovat seznam SKU pro hello umístění a nějaká omezení pro tento SKU.

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- toouse hello [portál](https://portal.azure.com), přihlaste se toohello portál a přidat prostředek prostřednictvím rozhraní hello. Můžete nastavit hello hodnoty, jako vidíte hello k dispozici SKU pro tento prostředek. Není nutné toocomplete hello nasazení.

    ![dostupné edice](./media/resource-manager-common-deployment-errors/view-sku.png)

- toouse hello REST API pro virtuální počítače, odesílat hello následující požadavek:

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  Vrátí k dispozici SKU a oblastí, ve formátu hello:

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

Pokud jste nelze toofind vhodný SKU v této oblasti nebo alternativní oblast, která vyhovuje vašim obchodním potřebám, odeslání [SKU požadavek](https://aka.ms/skurestriction) tooAzure podpory.

## <a name="disallowedoperation"></a>DisallowedOperation

```
Code: DisallowedOperation
Message: hello current subscription type is not permitted tooperform operations on any provider 
namespace. Please use a different subscription.
```

Pokud se zobrazí tato chyba, kterou používáte odběr, který není povolen tooaccess služby Azure než Azure Active Directory. Pokud potřebujete tooaccess hello klasický portál ale nejsou povoleny toodeploy prostředků můžete mít tento typ předplatného. tooresolve tento problém, je nutné použít odběr, který má oprávnění toodeploy prostředky.  

tooview předplatné k dispozici v prostředí PowerShell, použijte:

```powershell
Get-AzureRmSubscription
```

A, tooset hello aktuální předplatné, použijte:

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

tooview vaše dostupných předplatných s 2.0 rozhraní příkazového řádku Azure, použijte:

```azurecli
az account list
```

A, tooset hello aktuální předplatné, použijte:

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a>InvalidTemplate
Tato chyba může způsobit z několika různých typů chyb.

- Chyba syntaxe

   Pokud se zobrazí chybovou zprávu, která určuje hello šablony se nezdařilo ověření, bude pravděpodobně problém syntaxe ve vaší šabloně.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   Tato chyba je snadno toomake, protože může být složité výrazy šablony. Například hello následující přiřazení název pro účet úložiště obsahuje jednu sadu závorky, tři funkce, tři sady závorky, jednu sadu jednoduchých uvozovek a jednu vlastnost:

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   Pokud nezadáte syntaxe porovnávání hello, vytvoří šablona hello hodnotu, která se liší od svůj záměr.

   Po zobrazení tohoto typu chyby, pečlivě zkontrolujte syntaxi výrazu hello. Zvažte použití editor JSON jako [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) nebo [Visual Studio Code](resource-manager-vs-code.md), která vás upozorní chyby syntaxe.

- Segment nesprávné délky

   Další neplatný šablony chyba nastane, když hello název prostředku není ve správném formátu hello.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'hello template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   Kořenové úrovně prostředku musí mít jeden menší segmentu v hello název než v hello typ prostředku. Každý segment je rozlišené pomocí lomítko. V následujícím příkladu hello, typ hello má dva segmenty a hello název jednoho segmentu, tak, aby byl **platný název**.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   Ale je další příklad hello **není platný název** protože má hello stejný počet segmentů jako typ hello.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   Pro podřízené prostředky typu hello a název hello mít stejný počet segmentů. Tento počet segmentů dává smysl, protože hello úplný název a typ pro podřízené hello zahrnuje hello nadřazené názvem a typem. Úplný název hello proto má stále jednoho menší segmentu než úplné typ hello.

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

   Získávání hello segmenty správné může být složité s typy Resource Manager, které se použijí napříč zprostředkovatelé prostředků. Například použití webu prostředek zámku tooa vyžaduje typ s čtyři segmenty. Název hello je proto tři segmenty:

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- Kopírování indexu není očekáván.

   To narazíte **InvalidTemplate** chyba, když jste použili hello **kopie** element tooa součástí hello šablony, která nepodporuje tento element. Lze použít pouze typ prostředku tooa element kopie hello. Nelze použít vlastnost tooa kopírování v rámci typu prostředku. Například použijete kopie tooa virtuálního počítače, ale jej nelze použít toohello OS disky pro virtuální počítač. V některých případech můžete převést podřízených prostředků tooa nadřazený prostředek toocreate kopírovací smyčkou. Další informace o používání kopírování najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).

- Parametr není platný

   Pokud šablona hello určuje povolené hodnoty pro parametr, a zadáte hodnotu, která není jedním z těchto hodnot, obdržíte zprávu podobné toohello následující chybě:

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'hello provided value {parameter value}
  for hello template parameter {parameter name} is not valid. hello parameter value is not
  part of hello allowed values
  ``` 

   Překontrolujte hello povolené hodnoty v šabloně hello a zadejte jeden během nasazení.

- Zjistila se cyklická závislost

   Tato chyba se zobrazí, když prostředky jsou vzájemně závislé způsobem, který brání hello nasazení spustit. Kombinace vzájemné závislosti díky dvou nebo více prostředků čekat na další prostředky, které jsou také čekání. Například resource1 závisí na resource3, resource2 závisí na resource1 a resource3 závisí na resource2. Obvykle můžete tento problém vyřešit odstraněním nepotřebných závislosti. 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound a ResourceNotFound
Pokud vaše šablona zahrnuje hello název prostředku, které nelze vyřešit, obdržíte chybu podobně jako:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Pokud se pokoušíte toodeploy hello chybí prostředek v šabloně hello, zkontrolujte, zda je nutné tooadd závislost. Správce prostředků optimalizuje nasazení tak, že vytvoříte prostředky paralelně, pokud je to možné. Pokud po jiný prostředek musí být nasazený jeden prostředek, je třeba toouse hello **dependsOn** element ve vaší šablony toocreate a závislost na hello jiný prostředek. Například Pokud nasazujete webovou aplikaci, musí existovat hello plán služby App Service. Pokud jste nezadali, že webová aplikace hello závisí na hello plán služby App Service, Resource Manager vytvoří v hello oba současně. Můžete zobrazit chyba oznamující tuto hello, které nelze najít prostředek plánu služby App Service, protože neexistuje ještě při pokusu o tooset vlastnost hello webové aplikace. Tuto chybu můžete zabránit nastavením hello závislostí v hello webové aplikace.

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

Můžete také zobrazit chyba při hello prostředek existuje v jiné skupině prostředků než hello jeden nasazován na. V takovém případě použijte hello [resourceId funkce](resource-group-template-functions-resource.md#resourceid) tooget hello plně kvalifikovaný název prostředku hello.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

Pokud se pokusíte toouse hello [odkaz](resource-group-template-functions-resource.md#reference) nebo [listKeys](resource-group-template-functions-resource.md#listkeys) funkce s prostředky, které nelze vyřešit, obdržíte hello následující chybě:

```
Code=ResourceNotFound;
Message=hello Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

Vyhledat výraz, který zahrnuje hello **odkaz** funkce. Dvojitý zkontrolujte, zda text hello hodnoty parametrů jsou správné.

## <a name="parentresourcenotfound"></a>ParentResourceNotFound

Pokud jeden prostředek je prostředek tooanother nadřazené, hello nadřazený prostředek musí existovat před vytvořením hello podřízených prostředků. Pokud ještě neexistuje, zobrazí hello následující chybě:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

Hello název prostředku podřízené hello obsahuje název nadřazené hello. Například může být definovaná SQL Database jako:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Ale pokud nezadáte závislost na nadřazeném prostředku hello, může získat hello podřízených prostředků nasadit před nadřazené hello. tooresolve této chyby patří závislost.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists a StorageAccountAlreadyTaken
Pro účty úložiště je nutné zadat název pro hello prostředek, který je v rámci Azure jedinečný. Pokud nezadáte jedinečný název, zobrazí chybu jako:

```
Code=StorageAccountAlreadyTaken
Message=hello storage account named mystorage is already taken.
```

Můžete vytvořit jedinečný název vaší zásady vytváření názvů s hello výsledek hello zřetězení [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Pokud nasadíte účet úložiště s hello stejný název jako existující účet úložiště v rámci vašeho předplatného, ale zadejte jiné umístění, obdržíte chybu, což značí hello účet úložiště už existuje v jiném umístění. Odstraňte stávající účet úložiště hello nebo zadat hello stejné umístění jako hello stávající účet úložiště.

## <a name="accountnameinvalid"></a>AccountNameInvalid
Zobrazí hello **AccountNameInvalid** Chyba při pokusu o toogive úložiště účet název, který obsahuje zakázané znaky. Názvy účtů úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena. Hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce vrátí 13 znaků. Pokud řetězení předponu toohello **uniqueString** vést, zadejte předponu, která je 11 znaků nebo méně.

## <a name="badrequest"></a>Struktura BadRequest

Struktura BadRequest stav se můžete setkat, když zadáte neplatnou hodnotu pro vlastnost. Například pokud jste zadali nesprávnou hodnotu SKU pro účet úložiště, hello nasazení se nezdaří. toodetermine platné hodnoty pro vlastnost, podívejte se na hello [REST API](/rest/api) pro typ prostředku hello nasazujete.

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>NoRegisteredProviderFound a MissingSubscriptionRegistration
Při nasazování prostředků, může přijímat hello následující kód chyby a zpráva:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Nebo se může zobrazit podobné zpráva, že:

```
Code: MissingSubscriptionRegistration
Message: hello subscription is not registered toouse namespace {resource-provider-namespace}
```

Zobrazí tyto chyby pro jednu ze tří důvodů:

1. Zprostředkovatel prostředků Hello není zaregistrován pro vaše předplatné
2. Verze rozhraní API není podporována pro typ prostředku hello
3. Umístění není podporováno pro typ prostředku hello

Hello chybová zpráva by měla dát návrhy hello podporované umístění a verze rozhraní API. Vaše šablona tooone Dobrý den, můžete změnit navrhované hodnoty. Většina poskytovatelů automaticky registrovaných v hello portálu nebo hello rozhraní příkazového řádku Azure, kterou používáte, ale ne všechny. Pokud jste nepoužili poskytovatele určitému prostředku před, musíte tooregister tohoto zprostředkovatele. Může zjišťovat informace o poskytovatelích prostředků prostřednictvím PowerShell nebo rozhraní příkazového řádku Azure.

**Azure Portal**

Můžete zobrazit stav registrace hello a zaregistrovat obor názvů zprostředkovatele prostředků prostřednictvím portálu hello.

1. Pro vaše předplatné, vyberte **zprostředkovatelé prostředků**.

   ![Vyberte poskytovatele prostředků](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Podívejte se na hello seznam poskytovatelů prostředků a v případě potřeby vyberte hello **zaregistrovat** poskytovatele prostředků hello tooregister propojení typu hello se pokoušíte toodeploy.

   ![Zobrazit seznam poskytovatelů prostředků](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

toosee stav registrace, použijte **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

použít tooregister zprostředkovatele, **Register-AzureRmResourceProvider** a zadejte název hello zprostředkovatele prostředků hello chcete tooregister.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

tooget hello podporované umístění pro konkrétní typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

tooget hello podporované verze rozhraní API pro konkrétní typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Azure CLI**

toosee, zda je zaregistrován hello poskytovatele, používat hello `azure provider list` příkaz.

```azurecli
az provider list
```

tooregister zprostředkovatel prostředků, použijte hello `azure provider register` příkaz a zadejte hello *obor názvů* tooregister.

```azurecli
az provider register --namespace Microsoft.Cdn
```

toosee hello podporované umístění a verze rozhraní API pro typ prostředku, použijte:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded a OperationNotAllowed
Pokud nasazení překročí kvótu, která může být na skupinu prostředků, odběry, účty a další obory, může mít problémy. Vaše předplatné může být například nakonfigurován toolimit hello počet jader pro oblast. Když zkusíte toodeploy virtuální počítač s více jader, než hello povolená výše, obdržíte chybu, která byla překročena kvóta stanovení hello.
Dokončení kvóty informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

tooexamine vaše předplatné kvóty pro počet jader, můžete použít hello `azure vm list-usage` v hello rozhraní příkazového řádku Azure. Hello následující příklad znázorňuje tento hello základní kvóta pro 4 je bezplatný zkušební účet:

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

Pokud nasadíte šablonu, která vytvoří více než čtyři jader v oblasti západní USA hello, dojde k chybě nasazení, bude vypadat takto:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Nebo v prostředí PowerShell, můžete použít hello **Get-AzureRmVMUsage** rutiny.

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

V těchto případech by měl přejděte toohello portál a souborů podporu problém tooraise vaší kvóty pro hello oblast, do kterého chcete toodeploy.

> [!NOTE]
> Mějte na paměti, že pro skupiny prostředků, kvóty hello je pro každou oblast jednotlivých, ne pro celé předplatné hello. Pokud potřebujete toodeploy 30 jader v západní USA, máte tooask pro 30 Resource Manager jader v západní USA. Pokud potřebujete toodeploy 30 jader v některém z oblasti toowhich hello máte přístup, je třeba požádat o 30 jader Resource Manager ve všech oblastech.
>
>

## <a name="invalidcontentlink"></a>InvalidContentLink
Když hello chybová zpráva:

```
Code=InvalidContentLink
Message=Unable toodownload deployment content from ...
```

Pokusili jste s největší pravděpodobností tooa toolink vnořené se šablonu, která není k dispozici. Překontrolujte hello URI, které jste zadali pro vnořené šablonu hello. Pokud šablona hello existuje v účtu úložiště, zajistěte, aby hello identifikátor URI je přístupný. Může být nutné toopass SAS token. Další informace najdete v tématu věnovaném [použití propojených šablon s Azure Resource Managerem](resource-group-linked-templates.md).

## <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Tato chyba se zobrazí, když vaše předplatné zahrnuje zásady prostředků, které brání akce se pokoušíte tooperform během nasazení. V hello chybová zpráva vyhledejte identifikátor zásady hello.

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

V **prostředí PowerShell**, poskytovat identifikátor zásady jako hello **Id** parametr tooretrieve podrobnosti o hello zásady, které blokované vaše nasazení.

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

V **rozhraní příkazového řádku Azure**, poskytovat hello název definice zásady hello:

```azurecli
az policy definition show --name regionPolicyAssignment
```

Další informace najdete v tématu hello následující články:

- [Chyba RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
- [Použijte zásady toomanage prostředků a řízení přístupu](resource-manager-policy.md).

## <a name="authorization-failed"></a>Ověření se nezdařilo
Může zobrazit chybová během nasazení, protože hello účet nebo instanční objekt pokus toodeploy hello prostředků nemá přístup k tooperform tyto akce. Azure Active Directory umožňuje vám nebo vaší toocontrol správce identit, které mají přístup k jaké prostředkům s vysoký stupeň přesnosti. Například pokud váš účet je přiřadit role Čtenář toohello, nemůžete se může toocreate prostředky. V takovém případě zobrazí chybová zpráva oznamující, že autorizace se nezdařila.

Další informace o řízení přístupu na základě rolí najdete v tématu [řízení přístupu](../active-directory/role-based-access-control-configure.md).


## <a name="next-steps"></a>Další kroky
* toolearn o auditování akce, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).
* toolearn o akce toodetermine hello chyb během nasazení, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).
