---
title: "chyby nasazení Azure aaaUnderstand | Microsoft Docs"
description: "Popisuje, jak toolearn o chybách nasazení Azure."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení tooazure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: a335e121e9b908a763374907e34b1f6e823d6e96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-deployment-errors"></a>Pochopení chyby nasazení Azure
Toto téma popisuje chyby nasazení a jak můžete zjistit další informace o chybě. Chyby nasazení toocommon řešení, najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Dva typy chyb
Existují dva typy chyb, které dostanete:

* Chyby ověření
* Chyby nasazení

Hello následující obrázek znázorňuje hello protokol aktivit pro předplatné. Reprezentuje dvě nasazení. V nasazení jedné hello šablony se nezdařilo ověření (**ověřením**) a neproběhla. V hello další nasazení, šablony hello ověření proběhlo úspěšně, ale při vytváření prostředků hello (**zápisu nasazení**). 

![Zobrazí kód chyby](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Chyby ověření jsou vyvolány scénáře, které lze určit před nasazením. Obsahují chyby syntaxe v šabloně, nebo se snažíme toodeploy prostředky, které překročí vaší kvóty předplatného. Chyby nasazení jsou vyvolány podmínky, které ke kterým došlo během nasazení hello. Patří mezi ně při tooaccess na prostředek, který je nasazený paralelně.

Oba typy chyb návratový kód chyby, že používáte tootroubleshoot hello nasazení. Oba typy chyb se zobrazí v hello [protokol aktivit](resource-group-audit.md). Ale chyb při ověřování se nezobrazí v historii nasazení protože hello nasazení nikdy spuštěn.

## <a name="determine-error-code"></a>Určete kód chyby

Informace o chybě najdete prohlížením hello chybovou zprávu a hello kód chyby. Hello [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) článku jsou uvedené řešení v kódu chyby. Toto téma ukazuje, jak toouse hello Azure portálu toodiscover hello chybový kód.

### <a name="validation-errors"></a>Chyby ověření

Pokud nasazujete prostřednictvím portálu hello, zobrazí chybu ověření po odeslání svoje hodnoty.

![zobrazit chyba portálu ověření](./media/resource-manager-troubleshoot-tips/validation-error.png)

Vyberte uvítací zprávu další podrobnosti. Hello následující bitové kopie, se zobrazuje **InvalidTemplateDeployment** chyba a zprávu, která určuje zásadu blokován nasazení.

![Zobrazit podrobnosti o ověření](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Chyby nasazení

Při operaci hello ověřením úspěšně projde, ale během nasazení se nezdaří, zobrazí se chyba hello v oznámeních hello. Vyberte oznámení hello.

![Chyba oznámení](./media/resource-manager-troubleshoot-tips/notification.png)

Zobrazí podrobnosti o nasazení hello. Vyberte možnost toofind hello Další informace o chybě hello.

![nasazení se nezdařilo.](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Zobrazí zprávu a chybové kódy chyb hello. Všimněte si, že jsou dva kódy chyb. Hello první kód chyby (**DeploymentFailed**) je obecná chyba, která nenabízí hello podrobnosti můžete potřebovat toosolve hello chyby. Hello druhý kód chyby (**StorageAccountNotFound**) poskytuje podrobnosti hello potřebujete. 

![Podrobnosti o chybě](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Povolit protokolování ladění
Někdy můžete potřebovat další informace o požadavku a odpovědi toodiscover hello kde došlo k chybě. Pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure, můžete požádat, že další informace o protokolu během nasazení.

- PowerShell

   V prostředí PowerShell, nastavte hello **DeploymentDebugLogLevel** parametr tooAll, ResponseContent nebo RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Zkontrolujte obsah žádosti hello s hello následující rutiny:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Nebo hello obsahu s odpovědí:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Tyto informace můžete určit, zda hodnotu v šabloně hello je správně nastaveno.

- Azure CLI

   Zkontrolujte hello operace nasazení se hello následující příkaz:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Vnořené šablony

   informace o ladění toolog vnořené šablony, použijte hello **debugSetting** element.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Vytvořit šablonu řešení potíží
V některých případech hello nejjednodušší způsob, jak tootroubleshoot šablona je tootest části. Zjednodušená šablonu, která vám umožní můžete vytvořit toofocus na hello část, která budete mít dojem, že je příčinou chyby hello. Předpokládejme například, že jste obdrželi chybu při odkazování na prostředek. Místo práci s celou šablony, vytvořte šablonu, která vrátí hello část, která může být příčinou problému. Může pomoct určit, zda jsou předávání v hello správné parametry, pomocí šablony funkce správně, a získávání prostředků hello očekáváte.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Nebo Předpokládejme, že dochází k chybám nasazení, které budete mít dojem, že se vztahují tooincorrectly nastavit závislosti. Testování šablony porušením do zjednodušené šablony. Nejprve vytvořte šablonu, která nasadí pouze jeden prostředek (např. SQL Server). Pokud jste si jistí, že máte správně definované prostředku, přidejte na prostředek, který na něm závisí (např. SQL Database). Až budete mít tyto dva prostředky správně definované, přidejte další závislé prostředky (jako jsou zásady auditu). Mezi každou testovací nasazení odstraňte hello skupiny toomake zda jste adekvátní testování hello závislosti prostředků. 

## <a name="check-deployment-sequence"></a>Zkontrolujte pořadí nasazení

Mnoho chyb nasazení dojít, když se prostředky nasadí neočekávané pořadí. Tyto chyby jsou vyvolány při závislosti nejsou správně nastavené. Pokud chybí potřebné závislosti, jeden prostředek pokusí o toouse hodnotu pro jiný prostředek, ale hello jiných ještě neexistuje. Zobrazí chyba oznamující, že prostředek nebyl nalezen. Setkat tohoto typu chyby občas může lišit hello času nasazení pro každého prostředku. Například vaše první pokus o toodeploy vašich prostředků bude úspěšné, protože požadovaný prostředek náhodně dokončí v čase. Však druhý pokus selže, protože hello požadované prostředku se nedokončila v čase. 

Ale chcete tooavoid nastavení závislosti, které nejsou potřebné. Až budete mít nepotřebné závislosti, můžete prodloužit doba trvání hello hello nasazení tím, že prostředky, které nejsou na sobě navzájem závislé z nasazuje paralelně. Kromě toho můžete vytvořit cyklické závislosti, které blokují nasazení hello. Hello [odkaz](resource-group-template-functions-resource.md#reference) funkce vytvoří implicitní závislostí v hello odkazuje prostředků při tento prostředek je nasazena v hello stejné šablony. Proto můžete mít další závislosti než hello závislosti zadaný v hello **dependsOn** vlastnost. Hello [resourceId](resource-group-template-functions-resource.md#resourceid) funkce vytvořit implicitní závislostí nebo ověřit, zda text hello prostředků existuje.

Pokud narazíte na potíže závislostí, je potřeba toogain aspekty hello pořadí nasazení prostředků. pořadí hello tooview operace nasazení:

1. Vyberte hello historii nasazení skupiny prostředků.

   ![Vyberte historii nasazení](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Vyberte nasazení z historie hello a vyberte **události**.

   ![Vybrat nasazení události](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Zkontrolujte hello posloupnost událostí pro každého prostředku. Věnujte pozornost toohello stav jednotlivých operací. Například hello následující obrázek ukazuje tři účty úložiště, které nasadí současně. Všimněte si, že hello tři účty úložiště jsou zahájená hello stejnou dobu.

   ![Paralelní nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   Hello další obrázek ukazuje tři účty úložiště, které nejsou nasazené paralelně. Hello druhého účtu úložiště závisí na hello první účet úložiště, a účet úložiště třetí hello na hello druhého účtu úložiště. Proto hello první účet úložiště spuštěna, přijmout a dokončit před příštím spuštění hello.

   ![sekvenční nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

I pro složitější scénáře, můžete použít hello toodiscover stejným způsobem, když je nasazení spuštěno a dokončeno pro každý zdroj. Pokud je jiné než byste očekávali hello pořadí projděte toosee události vaše nasazení. Pokud ano, přehodnocovat hello závislosti pro tento prostředek.

Správce prostředků identifikuje cyklické závislosti během ověřování šablony. Vrátí, zda existuje chybovou zprávu, která konkrétně stavy cyklickou závislost. toosolve cyklická závislost:

1. V šabloně najdete hello zdroje identifikovaného v hello cyklická závislost. 
2. Pro tento prostředek, zkontrolujte hello **dependsOn** vlastnost a všechny používá hello **odkaz** toosee prostředky, ke kterým závisí na funkce. 
3. Zkontrolujte toosee tyto prostředky, které prostředky, které jsou závislé na. Postupujte podle hello závislosti, dokud si všimnete na prostředek, který závisí na původní prostředků hello.
5. Pro hello prostředků zahrnutých v hello cyklická závislost, pečlivě zkontrolujte všechny používá hello **dependsOn** vlastnost tooidentify všechny závislosti, které nejsou potřebné. Odeberte tyto závislosti. Pokud si nejste jistí, že je potřeba závislost, zkuste ji odebrat. 
6. Znovu nasaďte šablonu hello.

Odebírání hodnot z hello **dependsOn** vlastnost mohou způsobit chyby, když nasazujete hello šablony. Pokud dojde k chybě, přidejte závislosti hello zpět do šablony hello. 

Pokud tento přístup nevyřeší hello cyklická závislost, zvažte přesunutí součást logiky nasazení do podřízené prostředky (například rozšíření nebo nastavení konfigurace). Nakonfigurujte tyto podřízené prostředky toodeploy po hello prostředků zahrnutých v hello cyklická závislost. Předpokládejme například, nasazujete dva virtuální počítače, ale je nutné nastavit na každé z nich najdete toohello jiné vlastnosti. Můžete je nasadit v hello následující pořadí:

1. vm1
2. virtuálního počítače 2
3. Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2. rozšíření Hello nastaví hodnoty vm1, který získá z virtuálního počítače 2.
4. Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2. rozšíření Hello nastaví hodnoty virtuálního počítače 2, který získá ze vm1.

Hello stejný přístup funguje pro aplikace služby App Service. Zvažte přesunutí hodnoty konfigurace do podřízených prostředkem prostředek aplikace hello. Můžete nasadit dva webové aplikace v hello následující pořadí:

1. webapp1
2. webapp2
3. konfigurace pro webapp1 závisí na webapp1 a webapp2. Obsahuje nastavení aplikace s hodnotami z webapp2.
4. konfigurace pro webapp2 závisí na webapp1 a webapp2. Obsahuje nastavení aplikace s hodnotami z webapp1.


## <a name="next-steps"></a>Další kroky
* Chyby nasazení toocommon řešení, najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* toolearn o auditování akce, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).
* toolearn o akce toodetermine hello chyb během nasazení, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).
