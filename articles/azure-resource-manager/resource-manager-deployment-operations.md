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
# <a name="view-deployment-operations-with-azure-resource-manager"></a><span data-ttu-id="563cd-103">Operace nasazení zobrazení pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="563cd-103">View deployment operations with Azure Resource Manager</span></span>


<span data-ttu-id="563cd-104">Můžete zobrazit hello operací pro nasazení prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="563cd-104">You can view hello operations for a deployment through hello Azure portal.</span></span> <span data-ttu-id="563cd-105">Můžete mít nejvíc zajímat zobrazení hello operations, pokud obdržíte chybu během nasazení, tento článek zaměřuje na zobrazení operace, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="563cd-105">You may be most interested in viewing hello operations when you have received an error during deployment so this article focuses on viewing operations that have failed.</span></span> <span data-ttu-id="563cd-106">portál Hello poskytuje rozhraní, které vám umožní tooeasily najít hello chyby a zjistit potenciální opravy.</span><span class="sxs-lookup"><span data-stu-id="563cd-106">hello portal provides an interface that enables you tooeasily find hello errors and determine potential fixes.</span></span>

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a><span data-ttu-id="563cd-107">Portál</span><span class="sxs-lookup"><span data-stu-id="563cd-107">Portal</span></span>
<span data-ttu-id="563cd-108">operace nasazení hello toosee, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="563cd-108">toosee hello deployment operations, use hello following steps:</span></span>

1. <span data-ttu-id="563cd-109">Pro skupinu prostředků hello zahrnutých v hello nasazení Všimněte si hello stav posledního nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="563cd-109">For hello resource group involved in hello deployment, notice hello status of hello last deployment.</span></span> <span data-ttu-id="563cd-110">Tento stav tooget můžete vybrat další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="563cd-110">You can select this status tooget more details.</span></span>
   
    ![Stav nasazení](./media/resource-manager-deployment-operations/deployment-status.png)
2. <span data-ttu-id="563cd-112">Zobrazí informace o nedávné historii nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="563cd-112">You see hello recent deployment history.</span></span> <span data-ttu-id="563cd-113">Vyberte hello nasazení, který selhal.</span><span class="sxs-lookup"><span data-stu-id="563cd-113">Select hello deployment that failed.</span></span>
   
    ![Stav nasazení](./media/resource-manager-deployment-operations/select-deployment.png)
3. <span data-ttu-id="563cd-115">Vyberte toosee odkaz hello popis důvod, proč hello nasazení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="563cd-115">Select hello link toosee a description of why hello deployment failed.</span></span> <span data-ttu-id="563cd-116">Záznam DNS hello v bitové kopii hello níže, není jedinečný.</span><span class="sxs-lookup"><span data-stu-id="563cd-116">In hello image below, hello DNS record is not unique.</span></span>  
   
    ![Zobrazit neúspěšné nasazení](./media/resource-manager-deployment-operations/view-error.png)
   
    <span data-ttu-id="563cd-118">Tato chybová zpráva by vám měly dostatečně pro vás toobegin řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="563cd-118">This error message should be enough for you toobegin troubleshooting.</span></span> <span data-ttu-id="563cd-119">Ale pokud potřebujete další informace o tom, kterým byly dokončeny úlohy, můžete zobrazit operace hello jak je znázorněno v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="563cd-119">However, if you need more details about which tasks were completed, you can view hello operations as shown in hello following steps.</span></span>
4. <span data-ttu-id="563cd-120">Všechny operace nasazení hello si můžete prohlédnout v hello **nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="563cd-120">You can view all hello deployment operations in hello **Deployment** blade.</span></span> <span data-ttu-id="563cd-121">Vyberte všechny operace toosee další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="563cd-121">Select any operation toosee more details.</span></span>
   
    ![Zobrazit operace](./media/resource-manager-deployment-operations/view-operations.png)
   
    <span data-ttu-id="563cd-123">V takovém případě uvidíte, že byly úspěšně vytvořeny hello účet úložiště, virtuální sítě a skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="563cd-123">In this case, you see that hello storage account, virtual network, and availability set were successfully created.</span></span> <span data-ttu-id="563cd-124">Hello veřejné IP adresy se nezdařilo a dalším prostředkům nebyly uplatněny.</span><span class="sxs-lookup"><span data-stu-id="563cd-124">hello public IP address failed, and other resources were not attempted.</span></span>
5. <span data-ttu-id="563cd-125">Události pro hello nasazení můžete zobrazit výběrem **události**.</span><span class="sxs-lookup"><span data-stu-id="563cd-125">You can view events for hello deployment by selecting **Events**.</span></span>
   
    ![zobrazení událostí](./media/resource-manager-deployment-operations/view-events.png)
6. <span data-ttu-id="563cd-127">Zobrazit všechny události hello hello nasazení a vyberte všechny další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="563cd-127">You see all hello events for hello deployment and select any one for more details.</span></span> <span data-ttu-id="563cd-128">Všimněte si, příliš hello ID korelace.</span><span class="sxs-lookup"><span data-stu-id="563cd-128">Notice too hello correlation IDs.</span></span> <span data-ttu-id="563cd-129">Tato hodnota může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-129">This value can be helpful when working with technical support tootroubleshoot a deployment.</span></span>
   
    ![Zobrazit události](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a><span data-ttu-id="563cd-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="563cd-131">PowerShell</span></span>
1. <span data-ttu-id="563cd-132">Celkový stav nasazení, použijte hello hello tooget **Get-AzureRmResourceGroupDeployment** příkaz.</span><span class="sxs-lookup"><span data-stu-id="563cd-132">tooget hello overall status of a deployment, use hello **Get-AzureRmResourceGroupDeployment** command.</span></span> 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   <span data-ttu-id="563cd-133">Nebo můžete filtrovat výsledky hello pouze nasazení, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="563cd-133">Or, you can filter hello results for only those deployments that have failed.</span></span>

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. <span data-ttu-id="563cd-134">Každé nasazení obsahuje více operací.</span><span class="sxs-lookup"><span data-stu-id="563cd-134">Each deployment includes multiple operations.</span></span> <span data-ttu-id="563cd-135">Každé operace představuje krok v procesu nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="563cd-135">Each operation represents a step in hello deployment process.</span></span> <span data-ttu-id="563cd-136">toodiscover, co se pokazilo k nasazení, musíte obvykle toosee podrobnosti o hello operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-136">toodiscover what went wrong with a deployment, you usually need toosee details about hello deployment operations.</span></span> <span data-ttu-id="563cd-137">Zobrazí se stav hello hello operací s **Get-AzureRmResourceGroupDeploymentOperation**.</span><span class="sxs-lookup"><span data-stu-id="563cd-137">You can see hello status of hello operations with **Get-AzureRmResourceGroupDeploymentOperation**.</span></span>

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    <span data-ttu-id="563cd-138">Která vrací více operací s každým z nich v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="563cd-138">Which returns multiple operations with each one in hello following format:</span></span>

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. <span data-ttu-id="563cd-139">tooget Další informace o neúspěšné operace načtení hello vlastnosti pro operace s **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="563cd-139">tooget more details about failed operations, retrieve hello properties for operations with **Failed** state.</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    <span data-ttu-id="563cd-140">Které vrátí že všechny hello neúspěšné operace s každé z nich v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="563cd-140">Which returns all hello failed operations with each one in hello following format:</span></span>

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

    <span data-ttu-id="563cd-141">Poznámka: hello serviceRequestId a hello trackingId pro operaci hello.</span><span class="sxs-lookup"><span data-stu-id="563cd-141">Note hello serviceRequestId and hello trackingId for hello operation.</span></span> <span data-ttu-id="563cd-142">Hello serviceRequestId může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-142">hello serviceRequestId can be helpful when working with technical support tootroubleshoot a deployment.</span></span> <span data-ttu-id="563cd-143">Použijete hello trackingId v hello další krok toofocus na konkrétní operace.</span><span class="sxs-lookup"><span data-stu-id="563cd-143">You will use hello trackingId in hello next step toofocus on a particular operation.</span></span>
4. <span data-ttu-id="563cd-144">tooget hello stavovou zprávu konkrétní operace se nezdařila, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="563cd-144">tooget hello status message of a particular failed operation, use hello following command:</span></span>

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    <span data-ttu-id="563cd-145">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="563cd-145">Which returns:</span></span>

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. <span data-ttu-id="563cd-146">Všechny operace nasazení v Azure zahrnuje obsah žádosti a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="563cd-146">Every deployment operation in Azure includes request and response content.</span></span> <span data-ttu-id="563cd-147">obsah žádosti Hello je, co jste poslali tooAzure během nasazení (například vytvořit virtuální počítač, disk operačního systému a další prostředky).</span><span class="sxs-lookup"><span data-stu-id="563cd-147">hello request content is what you sent tooAzure during deployment (for example, create a VM, OS disk, and other resources).</span></span> <span data-ttu-id="563cd-148">obsah odpovědi Hello je Azure odeslána zpět ze svého požadavku nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-148">hello response content is what Azure sent back from your deployment request.</span></span> <span data-ttu-id="563cd-149">Během nasazení, můžete použít **DeploymentDebugLogLevel** toospecify paramenter, který hello požadavku nebo odpovědi jsou uchovány v protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="563cd-149">During deployment, you can use **DeploymentDebugLogLevel** paramenter toospecify that hello request and/or response are retained in hello log.</span></span> 

  <span data-ttu-id="563cd-150">Získat tyto informace z protokolu hello a uložit ho místně pomocí hello následující příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="563cd-150">You get that information from hello log, and save it locally by using hello following PowerShell commands:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a><span data-ttu-id="563cd-151">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="563cd-151">Azure CLI</span></span>

1. <span data-ttu-id="563cd-152">Získat hello celkový stav nasazení s hello **skupiny azure nasazení zobrazit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="563cd-152">Get hello overall status of a deployment with hello **azure group deployment show** command.</span></span>

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  <span data-ttu-id="563cd-153">Jeden z hello vrátil hodnoty je hello **correlationId**.</span><span class="sxs-lookup"><span data-stu-id="563cd-153">One of hello returned values is hello **correlationId**.</span></span> <span data-ttu-id="563cd-154">Tato hodnota se používá tootrack souvisejících událostí a může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-154">This value is used tootrack related events, and can be helpful when working with technical support tootroubleshoot a deployment.</span></span>

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. <span data-ttu-id="563cd-155">operace hello toosee pro nasazení, použijte:</span><span class="sxs-lookup"><span data-stu-id="563cd-155">toosee hello operations for a deployment, use:</span></span>

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a><span data-ttu-id="563cd-156">REST</span><span class="sxs-lookup"><span data-stu-id="563cd-156">REST</span></span>

1. <span data-ttu-id="563cd-157">Získat informace o nasazení s hello [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="563cd-157">Get information about a deployment with hello [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operation.</span></span>

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    <span data-ttu-id="563cd-158">V odpovědi hello, Všimněte si, zejména hello **provisioningState**, **correlationId**, a **chyba** elementy.</span><span class="sxs-lookup"><span data-stu-id="563cd-158">In hello response, note in particular hello **provisioningState**, **correlationId**, and **error** elements.</span></span> <span data-ttu-id="563cd-159">Hello **correlationId** slouží tootrack souvisejících událostí a může být užitečné při práci s tootroubleshoot se na technickou podporu nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-159">hello **correlationId** is used tootrack related events, and can be helpful when working with technical support tootroubleshoot a deployment.</span></span>

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

2. <span data-ttu-id="563cd-160">Získat informace o operacích nasazení se hello [výpisu všech operací nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operaci.</span><span class="sxs-lookup"><span data-stu-id="563cd-160">Get information about deployment operations with hello [List all template deployment operations](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operation.</span></span> 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    <span data-ttu-id="563cd-161">Hello odpověď obsahuje informace o požadavku nebo odpovědi na co jste zadali v hello základě **debugSetting** vlastnost během nasazení.</span><span class="sxs-lookup"><span data-stu-id="563cd-161">hello response includes request and/or response information based on what you specified in hello **debugSetting** property during deployment.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="563cd-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="563cd-162">Next steps</span></span>
* <span data-ttu-id="563cd-163">Nápovědu k řešení chyb při konkrétní nasazení naleznete v tématu [řešení běžných chyb při nasazování tooAzure prostředků pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="563cd-163">For help with resolving particular deployment errors, see [Resolve common errors when deploying resources tooAzure with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="563cd-164">toolearn o používání hello aktivity v protokolech toomonitor jiné typy akcí, najdete v části [zobrazení aktivity protokoly toomanage Azure prostředky](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="563cd-164">toolearn about using hello activity logs toomonitor other types of actions, see [View activity logs toomanage Azure resources](resource-group-audit.md).</span></span>
* <span data-ttu-id="563cd-165">v tématu nasazení před provedením, toovalidate [nasazení skupiny prostředků pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="563cd-165">toovalidate your deployment before executing it, see [Deploy a resource group with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

