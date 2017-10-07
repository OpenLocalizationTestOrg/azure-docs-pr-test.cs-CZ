---
title: "operace aaaDeployment pomocí Azure Resource Manageru | Microsoft Docs"
description: "Popisuje, jak se operace nasazení Azure Resource Manager tooview s hello portál, prostředí PowerShell, rozhraní příkazového řádku Azure a rozhraní REST API."
services: azure-resource-manager,virtual-machines
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 01/13/2017
ms.author: tomfitz
ms.openlocfilehash: ba4823ca73caca83dfc07c99d736344ef8b7b54d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Operace nasazení zobrazení pomocí Azure Resource Manageru


Můžete zobrazit hello operací pro nasazení prostřednictvím hello portálu Azure. Můžete mít nejvíc zajímat zobrazení hello operations, pokud obdržíte chybu během nasazení, tento článek zaměřuje na zobrazení operace, které selhaly. portál Hello poskytuje rozhraní, které vám umožní tooeasily najít hello chyby a zjistit potenciální opravy.

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a>Portál
operace nasazení hello toosee, použijte hello následující kroky:

1. Pro skupinu prostředků hello zahrnutých v hello nasazení Všimněte si hello stav posledního nasazení hello. Tento stav tooget můžete vybrat další podrobnosti.
   
    ![Stav nasazení](./media/resource-manager-deployment-operations/deployment-status.png)
2. Zobrazí informace o nedávné historii nasazení hello. Vyberte hello nasazení, který selhal.
   
    ![Stav nasazení](./media/resource-manager-deployment-operations/select-deployment.png)
3. Vyberte toosee odkaz hello popis důvod, proč hello nasazení se nezdařilo. Záznam DNS hello v bitové kopii hello níže, není jedinečný.  
   
    ![Zobrazit neúspěšné nasazení](./media/resource-manager-deployment-operations/view-error.png)
   
    Tato chybová zpráva by vám měly dostatečně pro vás toobegin řešení potíží. Ale pokud potřebujete další informace o tom, kterým byly dokončeny úlohy, můžete zobrazit operace hello jak je znázorněno v hello následující kroky.
4. Všechny operace nasazení hello si můžete prohlédnout v hello **nasazení** okno. Vyberte všechny operace toosee další podrobnosti.
   
    ![Zobrazit operace](./media/resource-manager-deployment-operations/view-operations.png)
   
    V takovém případě uvidíte, že byly úspěšně vytvořeny hello účet úložiště, virtuální sítě a skupinu dostupnosti. Hello veřejné IP adresy se nezdařilo a dalším prostředkům nebyly uplatněny.
5. Události pro hello nasazení můžete zobrazit výběrem **události**.
   
    ![zobrazení událostí](./media/resource-manager-deployment-operations/view-events.png)
6. Zobrazit všechny události hello hello nasazení a vyberte všechny další podrobnosti. Všimněte si, příliš hello ID korelace. Tato hodnota může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.
   
    ![Zobrazit události](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. Celkový stav nasazení, použijte hello hello tooget **Get-AzureRmResourceGroupDeployment** příkaz. 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Nebo můžete filtrovat výsledky hello pouze nasazení, které selhaly.

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. Každé nasazení obsahuje více operací. Každé operace představuje krok v procesu nasazení hello. toodiscover, co se pokazilo k nasazení, musíte obvykle toosee podrobnosti o hello operace nasazení. Zobrazí se stav hello hello operací s **Get-AzureRmResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Která vrací více operací s každým z nich v hello následující formát:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. tooget Další informace o neúspěšné operace načtení hello vlastnosti pro operace s **se nezdařilo** stavu.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Které vrátí že všechny hello neúspěšné operace s každé z nich v hello následující formát:

  ```powershell
  provisioningOperation : Create
  provisioningState     : Failed
  timestamp             : 2016-06-14T21:54:55.1468068Z
  duration              : PT3.1449887S
  trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
  serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
  statusCode            : BadRequest
  statusMessage         : @{error=}
  targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                          Microsoft.Network/publicIPAddresses/myPublicIP;
                          resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}
  ```

    Poznámka: hello serviceRequestId a hello trackingId pro operaci hello. Hello serviceRequestId může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení. Použijete hello trackingId v hello další krok toofocus na konkrétní operace.
4. tooget hello stavovou zprávu konkrétní operace se nezdařila, hello použijte následující příkaz:

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Která vrací:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. Všechny operace nasazení v Azure zahrnuje obsah žádosti a odpovědi. obsah žádosti Hello je, co jste poslali tooAzure během nasazení (například vytvořit virtuální počítač, disk operačního systému a další prostředky). obsah odpovědi Hello je Azure odeslána zpět ze svého požadavku nasazení. Během nasazení, můžete použít **DeploymentDebugLogLevel** toospecify paramenter, který hello požadavku nebo odpovědi jsou uchovány v protokolu hello. 

  Získat tyto informace z protokolu hello a uložit ho místně pomocí hello následující příkazy prostředí PowerShell:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure CLI

1. Získat hello celkový stav nasazení s hello **skupiny azure nasazení zobrazit** příkaz.

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  Jeden z hello vrátil hodnoty je hello **correlationId**. Tato hodnota se používá tootrack souvisejících událostí a může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. operace hello toosee pro nasazení, použijte:

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a>REST

1. Získat informace o nasazení s hello [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operaci.

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    V odpovědi hello, Všimněte si, zejména hello **provisioningState**, **correlationId**, a **chyba** elementy. Hello **correlationId** slouží tootrack souvisejících událostí a může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.

  ```json
  { 
    ...
    "properties": {
      "provisioningState":"Failed",
      "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
      ...
      "error":{
        "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
        "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
      }  
    }
  }
  ```

2. Získat informace o operacích nasazení se hello [výpisu všech operací nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operaci. 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    Hello odpověď obsahuje informace o požadavku nebo odpovědi na co jste zadali v hello základě **debugSetting** vlastnost během nasazení.

  ```json
  {
    ...
    "properties": 
    {
      ...
      "request":{
        "content":{
          "location":"West US",
          "properties":{
            "accountType": "Standard_LRS"
          }
        }
      },
      "response":{
        "content":{
          "error":{
            "message":"Conflict","code":"Conflict"
          }
        }
      }
    }
  }
  ```


## <a name="next-steps"></a>Další kroky
* Nápovědu k řešení chyb při konkrétní nasazení naleznete v tématu [řešení běžných chyb při nasazování tooAzure prostředků pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* toolearn o používání hello aktivity v protokolech toomonitor jiné typy akcí, najdete v části [zobrazení aktivity protokoly toomanage Azure prostředky](resource-group-audit.md).
* v tématu nasazení před provedením, toovalidate [nasazení skupiny prostředků pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

