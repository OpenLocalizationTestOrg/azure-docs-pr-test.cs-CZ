---
title: "Operace nasazení s Azure Resource Manager | Microsoft Docs"
description: "Popisuje postup zobrazení operace nasazení Azure Resource Manager s portálem, prostředí PowerShell, rozhraní příkazového řádku Azure a rozhraní REST API."
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
ms.openlocfilehash: fb6b3b357fd1f66184e480115a9c863ba31ac193
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a><span data-ttu-id="d166a-103">Operace nasazení zobrazení pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d166a-103">View deployment operations with Azure Resource Manager</span></span>


<span data-ttu-id="d166a-104">Můžete zobrazit činnosti pro nasazení prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d166a-104">You can view the operations for a deployment through the Azure portal.</span></span> <span data-ttu-id="d166a-105">Můžete mít nejvíc zajímat zobrazení operace, když jste obdrželi chybu během nasazení, tento článek zaměřuje na zobrazení operace, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="d166a-105">You may be most interested in viewing the operations when you have received an error during deployment so this article focuses on viewing operations that have failed.</span></span> <span data-ttu-id="d166a-106">Na portálu poskytuje rozhraní, které vám umožňuje snadno najít chyby a zjistit potenciální opravy.</span><span class="sxs-lookup"><span data-stu-id="d166a-106">The portal provides an interface that enables you to easily find the errors and determine potential fixes.</span></span>

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a><span data-ttu-id="d166a-107">Portál</span><span class="sxs-lookup"><span data-stu-id="d166a-107">Portal</span></span>
<span data-ttu-id="d166a-108">Pokud chcete zobrazit operace nasazení, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d166a-108">To see the deployment operations, use the following steps:</span></span>

1. <span data-ttu-id="d166a-109">Pro skupinu prostředků zahrnutých v nasazení Všimněte si, stav posledního nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-109">For the resource group involved in the deployment, notice the status of the last deployment.</span></span> <span data-ttu-id="d166a-110">Můžete vybrat tento stav zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d166a-110">You can select this status to get more details.</span></span>
   
    ![Stav nasazení](./media/resource-manager-deployment-operations/deployment-status.png)
2. <span data-ttu-id="d166a-112">Uvidíte nedávné historii nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-112">You see the recent deployment history.</span></span> <span data-ttu-id="d166a-113">Vyberte nasazení, které se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="d166a-113">Select the deployment that failed.</span></span>
   
    ![Stav nasazení](./media/resource-manager-deployment-operations/select-deployment.png)
3. <span data-ttu-id="d166a-115">Vyberte na odkaz zobrazíte popis nezdaru nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-115">Select the link to see a description of why the deployment failed.</span></span> <span data-ttu-id="d166a-116">Na obrázku níže záznam DNS není jedinečný.</span><span class="sxs-lookup"><span data-stu-id="d166a-116">In the image below, the DNS record is not unique.</span></span>  
   
    ![Zobrazit neúspěšné nasazení](./media/resource-manager-deployment-operations/view-error.png)
   
    <span data-ttu-id="d166a-118">Tato chybová zpráva by vám měly dostatečně pro vámi na zahájení odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="d166a-118">This error message should be enough for you to begin troubleshooting.</span></span> <span data-ttu-id="d166a-119">Pokud potřebujete další podrobnosti o úkoly, které byly dokončeny, můžete zobrazit činnosti, jak je znázorněno v následujícím postupu.</span><span class="sxs-lookup"><span data-stu-id="d166a-119">However, if you need more details about which tasks were completed, you can view the operations as shown in the following steps.</span></span>
4. <span data-ttu-id="d166a-120">Můžete zobrazit všechny operace nasazení ve **nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="d166a-120">You can view all the deployment operations in the **Deployment** blade.</span></span> <span data-ttu-id="d166a-121">Vyberte všechny operace zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d166a-121">Select any operation to see more details.</span></span>
   
    ![Zobrazit operace](./media/resource-manager-deployment-operations/view-operations.png)
   
    <span data-ttu-id="d166a-123">V takovém případě uvidíte, že účet úložiště, virtuální sítě a skupina dostupnosti byly úspěšně vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="d166a-123">In this case, you see that the storage account, virtual network, and availability set were successfully created.</span></span> <span data-ttu-id="d166a-124">Veřejné IP adresy se nezdařilo a dalším prostředkům nebyly uplatněny.</span><span class="sxs-lookup"><span data-stu-id="d166a-124">The public IP address failed, and other resources were not attempted.</span></span>
5. <span data-ttu-id="d166a-125">Události pro nasazení můžete zobrazit výběrem **události**.</span><span class="sxs-lookup"><span data-stu-id="d166a-125">You can view events for the deployment by selecting **Events**.</span></span>
   
    ![zobrazení událostí](./media/resource-manager-deployment-operations/view-events.png)
6. <span data-ttu-id="d166a-127">Zobrazit všechny události pro nasazení a vyberte všechny další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d166a-127">You see all the events for the deployment and select any one for more details.</span></span> <span data-ttu-id="d166a-128">Všimněte si příliš ID korelace.</span><span class="sxs-lookup"><span data-stu-id="d166a-128">Notice too the correlation IDs.</span></span> <span data-ttu-id="d166a-129">Tato hodnota může být užitečné při práci se službou technické podpory k řešení nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-129">This value can be helpful when working with technical support to troubleshoot a deployment.</span></span>
   
    ![Zobrazit události](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a><span data-ttu-id="d166a-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d166a-131">PowerShell</span></span>
1. <span data-ttu-id="d166a-132">Chcete-li získat celkový stav nasazení, použijte **Get-AzureRmResourceGroupDeployment** příkaz.</span><span class="sxs-lookup"><span data-stu-id="d166a-132">To get the overall status of a deployment, use the **Get-AzureRmResourceGroupDeployment** command.</span></span> 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   <span data-ttu-id="d166a-133">Nebo můžete filtrovat výsledky pro jenom nasazení, které selhaly.</span><span class="sxs-lookup"><span data-stu-id="d166a-133">Or, you can filter the results for only those deployments that have failed.</span></span>

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. <span data-ttu-id="d166a-134">Každé nasazení obsahuje více operací.</span><span class="sxs-lookup"><span data-stu-id="d166a-134">Each deployment includes multiple operations.</span></span> <span data-ttu-id="d166a-135">Každé operace představuje krokem v procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-135">Each operation represents a step in the deployment process.</span></span> <span data-ttu-id="d166a-136">Pokud chcete zjistit, kde došlo k chybě s nasazením, budete muset obvykle zobrazit podrobnosti o operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-136">To discover what went wrong with a deployment, you usually need to see details about the deployment operations.</span></span> <span data-ttu-id="d166a-137">Zobrazí stav operace s **Get-AzureRmResourceGroupDeploymentOperation**.</span><span class="sxs-lookup"><span data-stu-id="d166a-137">You can see the status of the operations with **Get-AzureRmResourceGroupDeploymentOperation**.</span></span>

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    <span data-ttu-id="d166a-138">Která vrací více operací s každým z nich v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="d166a-138">Which returns multiple operations with each one in the following format:</span></span>

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. <span data-ttu-id="d166a-139">Chcete-li získat další informace o neúspěšné operace, se načíst vlastnosti pro operace s **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="d166a-139">To get more details about failed operations, retrieve the properties for operations with **Failed** state.</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    <span data-ttu-id="d166a-140">Která vrací všechny neúspěšné operace s každým z nich v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="d166a-140">Which returns all the failed operations with each one in the following format:</span></span>

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

    <span data-ttu-id="d166a-141">Poznámka: serviceRequestId a trackingId pro operaci.</span><span class="sxs-lookup"><span data-stu-id="d166a-141">Note the serviceRequestId and the trackingId for the operation.</span></span> <span data-ttu-id="d166a-142">ServiceRequestId může být užitečné při práci se službou technické podpory k řešení nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-142">The serviceRequestId can be helpful when working with technical support to troubleshoot a deployment.</span></span> <span data-ttu-id="d166a-143">Použijete trackingId a zaměřit se na konkrétní operace v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d166a-143">You will use the trackingId in the next step to focus on a particular operation.</span></span>
4. <span data-ttu-id="d166a-144">Chcete-li získat stavové zprávy konkrétní operace se nezdařila, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d166a-144">To get the status message of a particular failed operation, use the following command:</span></span>

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    <span data-ttu-id="d166a-145">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="d166a-145">Which returns:</span></span>

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. <span data-ttu-id="d166a-146">Všechny operace nasazení v Azure zahrnuje obsah žádosti a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d166a-146">Every deployment operation in Azure includes request and response content.</span></span> <span data-ttu-id="d166a-147">Obsah žádosti je, co jste poslali do Azure během nasazení (například vytvořit virtuální počítač, disk operačního systému a další prostředky).</span><span class="sxs-lookup"><span data-stu-id="d166a-147">The request content is what you sent to Azure during deployment (for example, create a VM, OS disk, and other resources).</span></span> <span data-ttu-id="d166a-148">Obsah odpovědi je Azure odeslána zpět ze svého požadavku nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-148">The response content is what Azure sent back from your deployment request.</span></span> <span data-ttu-id="d166a-149">Během nasazení, můžete použít **DeploymentDebugLogLevel** paramenter k určení, že požadavku nebo odpovědi jsou uchovány v protokolu.</span><span class="sxs-lookup"><span data-stu-id="d166a-149">During deployment, you can use **DeploymentDebugLogLevel** paramenter to specify that the request and/or response are retained in the log.</span></span> 

  <span data-ttu-id="d166a-150">Získat tyto informace z protokolu a uložit ho místně pomocí následujících příkazů prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d166a-150">You get that information from the log, and save it locally by using the following PowerShell commands:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a><span data-ttu-id="d166a-151">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d166a-151">Azure CLI</span></span>

1. <span data-ttu-id="d166a-152">Získat celkový stav nasazení pomocí **skupiny azure nasazení zobrazit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="d166a-152">Get the overall status of a deployment with the **azure group deployment show** command.</span></span>

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  <span data-ttu-id="d166a-153">Jedna z hodnot vrácených je **correlationId**.</span><span class="sxs-lookup"><span data-stu-id="d166a-153">One of the returned values is the **correlationId**.</span></span> <span data-ttu-id="d166a-154">Tato hodnota se používá ke sledování související události a může být užitečné při práci se službou technické podpory k řešení nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-154">This value is used to track related events, and can be helpful when working with technical support to troubleshoot a deployment.</span></span>

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. <span data-ttu-id="d166a-155">Pokud chcete zobrazit operace pro nasazení, použijte:</span><span class="sxs-lookup"><span data-stu-id="d166a-155">To see the operations for a deployment, use:</span></span>

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a><span data-ttu-id="d166a-156">REST</span><span class="sxs-lookup"><span data-stu-id="d166a-156">REST</span></span>

1. <span data-ttu-id="d166a-157">Získat informace o nasazení pomocí [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="d166a-157">Get information about a deployment with the [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operation.</span></span>

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    <span data-ttu-id="d166a-158">V odpovědi, Všimněte si, zejména **provisioningState**, **correlationId**, a **chyba** elementy.</span><span class="sxs-lookup"><span data-stu-id="d166a-158">In the response, note in particular the **provisioningState**, **correlationId**, and **error** elements.</span></span> <span data-ttu-id="d166a-159">**CorrelationId** slouží ke sledování související události a může být užitečné při práci se službou technické podpory k řešení nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-159">The **correlationId** is used to track related events, and can be helpful when working with technical support to troubleshoot a deployment.</span></span>

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

2. <span data-ttu-id="d166a-160">Získat informace o operacích nasazení se [výpisu všech operací nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operaci.</span><span class="sxs-lookup"><span data-stu-id="d166a-160">Get information about deployment operations with the [List all template deployment operations](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operation.</span></span> 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    <span data-ttu-id="d166a-161">Odpověď obsahuje požadavku nebo odpovědi informace, které jsou založené na co jste zadali v **debugSetting** vlastnost během nasazení.</span><span class="sxs-lookup"><span data-stu-id="d166a-161">The response includes request and/or response information based on what you specified in the **debugSetting** property during deployment.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="d166a-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d166a-162">Next steps</span></span>
* <span data-ttu-id="d166a-163">Nápovědu k řešení chyb při konkrétní nasazení naleznete v tématu [řešení běžných chyb při nasazování prostředků do Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="d166a-163">For help with resolving particular deployment errors, see [Resolve common errors when deploying resources to Azure with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="d166a-164">Další informace o použití protokoly aktivity monitorování jiné typy akcí, najdete v části [zobrazit protokoly aktivity ke správě prostředků Azure](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="d166a-164">To learn about using the activity logs to monitor other types of actions, see [View activity logs to manage Azure resources](resource-group-audit.md).</span></span>
* <span data-ttu-id="d166a-165">K ověření vašeho nasazení, než ho spustíte, najdete v části [nasazení skupiny prostředků pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="d166a-165">To validate your deployment before executing it, see [Deploy a resource group with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

