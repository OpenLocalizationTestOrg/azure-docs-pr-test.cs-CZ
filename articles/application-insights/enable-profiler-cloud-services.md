---
title: "Povolit u prostředku cloudové služby Azure Application Insights profileru | Microsoft Docs"
description: "Zjistěte, jak nastavit profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 5ff062ac81dca9d8b205cec966d2a9c11a4005b6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a><span data-ttu-id="b407e-103">Povolte Insights profileru aplikace na prostředek Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b407e-103">Enable Application Insights Profiler on an Azure Cloud Services resource</span></span>

<span data-ttu-id="b407e-104">Tento návod ukazuje, jak povolit Azure Application Insights profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="b407e-104">This walkthrough demonstrates how to enable Azure Application Insights Profiler on an ASP.NET application hosted by an Azure Cloud Services resource.</span></span> <span data-ttu-id="b407e-105">Příklady zahrnují podporu pro virtuální počítače Azure, Azure Service Fabric a sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b407e-105">The examples include support for Azure Virtual Machines, virtual machine scale sets, and Azure Service Fabric.</span></span> <span data-ttu-id="b407e-106">Všechny příklady spoléhají na šablony, které podporují modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b407e-106">The examples all rely on templates that support the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="b407e-107">Další informace o modelu nasazení, zkontrolujte [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a stav svých prostředků](/azure-resource-manager/resource-manager-deployment-model).</span><span class="sxs-lookup"><span data-stu-id="b407e-107">For more information about the deployment model, review [Azure Resource Manager vs. classic deployment: Understand deployment models and the state of your resources](/azure-resource-manager/resource-manager-deployment-model).</span></span>

## <a name="overview"></a><span data-ttu-id="b407e-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="b407e-108">Overview</span></span>

<span data-ttu-id="b407e-109">Následující diagram znázorňuje, jak funguje profileru pro prostředky Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="b407e-109">The following diagram illustrates how the profiler works for Azure Cloud Services resources.</span></span> <span data-ttu-id="b407e-110">Jako příklad používá virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="b407e-110">It uses an Azure virtual machine as an example.</span></span>

<span data-ttu-id="b407e-111">![Přehled](./media/enable-profiler-compute/overview.png) ke shromažďování informací pro zpracování a zobrazení na portálu Azure, musíte nainstalovat součást agenta diagnostiky pro prostředky Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="b407e-111">![Overview](./media/enable-profiler-compute/overview.png) To collect information for processing and display on the Azure portal, you must install the Diagnostics Agent component for the Azure Cloud Services resources.</span></span> <span data-ttu-id="b407e-112">Zbytek průvodce obsahuje pokyny k instalaci a konfiguraci agenta diagnostiky povolit profileru Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b407e-112">The rest of the walkthrough provides guidance on how to install and configure the Diagnostics Agent to enable Application Insights Profiler.</span></span>

## <a name="prerequisites-for-the-walkthrough"></a><span data-ttu-id="b407e-113">Předpoklady pro Průvodce</span><span class="sxs-lookup"><span data-stu-id="b407e-113">Prerequisites for the walkthrough</span></span>

* <span data-ttu-id="b407e-114">Šablonu nasazení Resource Manager, který nainstaluje profileru agenty na virtuálních počítačích ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) nebo sady škálování ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span><span class="sxs-lookup"><span data-stu-id="b407e-114">A deployment Resource Manager template that installs the profiler agents on the VMs ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) or scale sets ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span></span>

* <span data-ttu-id="b407e-115">Instance služby Application Insights pro profilace povoleno.</span><span class="sxs-lookup"><span data-stu-id="b407e-115">An Application Insights instance enabled for profiling.</span></span> <span data-ttu-id="b407e-116">Pokyny najdete v tématu [povolit profil](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span><span class="sxs-lookup"><span data-stu-id="b407e-116">For instructions, see [Enable the profile](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span></span>

* <span data-ttu-id="b407e-117">Rozhraní .NET framework 4.6.1 v cílovému prostředku Azure Cloud Services nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="b407e-117">.NET Framework 4.6.1 or later installed on the target Azure Cloud Services resource.</span></span>

## <a name="create-a-resource-group-in-your-azure-subscription"></a><span data-ttu-id="b407e-118">Vytvořte skupinu prostředků ve vašem předplatném Azure</span><span class="sxs-lookup"><span data-stu-id="b407e-118">Create a resource group in your Azure subscription</span></span>
<span data-ttu-id="b407e-119">Následující příklad ukazuje, jak vytvořit skupinu prostředků s použitím skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b407e-119">The following example demonstrates how to create a resource group by using a PowerShell script:</span></span>

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-the-resource-group"></a><span data-ttu-id="b407e-120">Vytvořte prostředek Application Insights ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="b407e-120">Create an Application Insights resource in the resource group</span></span>
<span data-ttu-id="b407e-121">Na **Application Insights** okno, zadejte informace o vašem prostředku, jak je popsáno v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="b407e-121">On the **Application Insights** blade, enter the information for your resource, as shown in this example:</span></span> 

![Okno Application Insights](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-the-azure-resource-manager-template"></a><span data-ttu-id="b407e-123">Použít klíč instrumentace Application Insights do šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b407e-123">Apply an Application Insights instrumentation key in the Azure Resource Manager template</span></span>

1. <span data-ttu-id="b407e-124">Pokud ještě nebyly stáhne šablonu, ji stáhnout z [Githubu](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span><span class="sxs-lookup"><span data-stu-id="b407e-124">If you haven't downloaded the template yet, download it from [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span></span>

2. <span data-ttu-id="b407e-125">Vyhledání klíče Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b407e-125">Find the Application Insights key.</span></span>
   
   ![Umístění klíče](./media/enable-profiler-compute/copyaikey.png)

3. <span data-ttu-id="b407e-127">Nahraďte hodnotu šablony.</span><span class="sxs-lookup"><span data-stu-id="b407e-127">Replace the template value.</span></span>
   
   ![Hodnota nahradit v šabloně](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-to-host-the-web-application"></a><span data-ttu-id="b407e-129">Vytvoření virtuálního počítače Azure k hostování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="b407e-129">Create an Azure VM to host the web application</span></span>
1. <span data-ttu-id="b407e-130">Vytvořte zabezpečený řetězec uložit heslo.</span><span class="sxs-lookup"><span data-stu-id="b407e-130">Create a secure string to save the password.</span></span>

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. <span data-ttu-id="b407e-131">Nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b407e-131">Deploy the Azure Resource Manager template.</span></span>

   <span data-ttu-id="b407e-132">Změňte adresář v konzole PowerShell do složky, která obsahuje vaše šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="b407e-132">Change the directory in the PowerShell console to the folder that contains your Resource Manager template.</span></span> <span data-ttu-id="b407e-133">Pokud chcete nasadit šablonu, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b407e-133">To deploy the template, run the following command:</span></span>

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

<span data-ttu-id="b407e-134">Po úspěšném spuštění skriptu, byste měli najít virtuálního počítače s názvem **MyWindowsVM** ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b407e-134">After the script runs successfully, you should find a VM named **MyWindowsVM** in your resource group.</span></span>

## <a name="configure-web-deploy-on-the-vm"></a><span data-ttu-id="b407e-135">Konfigurovat Web nasadit ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="b407e-135">Configure Web Deploy on the VM</span></span>
<span data-ttu-id="b407e-136">Ujistěte se, že, nasazení webu je povolena na vašem virtuálním počítači, můžete publikovat webovou aplikaci ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b407e-136">Make sure that Web Deploy is enabled on your VM so you can publish your web application from Visual Studio.</span></span>

<span data-ttu-id="b407e-137">Na virtuálním počítači ručně prostřednictvím WebPI nainstalujte nástroj nasazení webu, najdete v tématu [instalace a konfigurace nasazení webu na IIS 8.0 nebo později](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span><span class="sxs-lookup"><span data-stu-id="b407e-137">To install Web Deploy on a VM manually via WebPI, see [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span></span> <span data-ttu-id="b407e-138">Příklad toho, jak k automatizaci instalace nasazení webu pomocí šablony Azure Resource Manager, naleznete v části [vytvořit, nakonfigurovat a nasadit webovou aplikaci pro virtuální počítač Azure](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span><span class="sxs-lookup"><span data-stu-id="b407e-138">For an example of how to automate installing Web Deploy by using an Azure Resource Manager template, see [Create, configure, and deploy a web application to an Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span></span>

<span data-ttu-id="b407e-139">Pokud nasazujete aplikaci ASP.NET MVC, přejděte na správce serveru, vyberte **přidat role a funkce** > **webového serveru (IIS)** > **Webový Server**  >  **Vývoj aplikací**a povolit technologii ASP.NET 4.5 na serveru.</span><span class="sxs-lookup"><span data-stu-id="b407e-139">If you are deploying an ASP.NET MVC application, go to Server Manager, select **Add Roles and Features** > **Web Server (IIS)** > **Web Server** > **Application Development**, and enable ASP.NET 4.5 on your server.</span></span>

![Přidat ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-the-azure-application-insights-sdk-for-your-project"></a><span data-ttu-id="b407e-141">Nainstalovat Azure Application Insights SDK pro svůj projekt</span><span class="sxs-lookup"><span data-stu-id="b407e-141">Install the Azure Application Insights SDK for your project</span></span>
1. <span data-ttu-id="b407e-142">Otevřete vaši webovou aplikaci ASP.NET v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b407e-142">Open your ASP.NET web application in Visual Studio.</span></span>

2. <span data-ttu-id="b407e-143">Klikněte pravým tlačítkem na projekt a vyberte **přidat** > **připojené služby**.</span><span class="sxs-lookup"><span data-stu-id="b407e-143">Right-click the project and select **Add** > **Connected Services**.</span></span>

3. <span data-ttu-id="b407e-144">Vyberte **Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="b407e-144">Select **Application Insights**.</span></span>

4. <span data-ttu-id="b407e-145">Postupujte podle pokynů na stránce.</span><span class="sxs-lookup"><span data-stu-id="b407e-145">Follow the instructions on the page.</span></span> <span data-ttu-id="b407e-146">Vyberte prostředek Application Insights, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b407e-146">Select the Application Insights resource that you created earlier.</span></span>

5. <span data-ttu-id="b407e-147">Vyberte **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b407e-147">Select the **Register** button.</span></span>


## <a name="publish-the-project-to-an-azure-vm"></a><span data-ttu-id="b407e-148">Publikování tohoto projektu na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="b407e-148">Publish the project to an Azure VM</span></span>
<span data-ttu-id="b407e-149">Existuje několik způsobů, jak publikovat aplikaci na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="b407e-149">There are several ways to publish an application to an Azure VM.</span></span> <span data-ttu-id="b407e-150">Jednou z možností je použití Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b407e-150">One way is to use Visual Studio 2017.</span></span>

1. <span data-ttu-id="b407e-151">Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b407e-151">Right-click the project and select **Publish**.</span></span>

2. <span data-ttu-id="b407e-152">Vyberte **Microsoft Azure Virtual Machines** jako publikování cíle a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="b407e-152">Select **Microsoft Azure Virtual Machines** as the publish target and follow the steps.</span></span>

   ![Publikování FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. <span data-ttu-id="b407e-154">Spuštění zátěžového testu proti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b407e-154">Run a load test against your application.</span></span> <span data-ttu-id="b407e-155">Měli byste vidět výsledky na webové stránce portálu pro Application Insights instance.</span><span class="sxs-lookup"><span data-stu-id="b407e-155">You should see results on the Application Insights instance portal webpage.</span></span>


## <a name="enable-the-profiler"></a><span data-ttu-id="b407e-156">Povolit profileru</span><span class="sxs-lookup"><span data-stu-id="b407e-156">Enable the profiler</span></span>
1. <span data-ttu-id="b407e-157">Přejděte na Application Insights **výkonu** a vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b407e-157">Go to your Application Insights **Performance** blade and select **Configure**.</span></span>
   
   ![Ikona konfigurace](./media/enable-profiler-compute/enableprofiler1.png)
 
2. <span data-ttu-id="b407e-159">Vyberte **povolit profileru**.</span><span class="sxs-lookup"><span data-stu-id="b407e-159">Select **Enable Profiler**.</span></span>
   
   ![Povolit ikonu profileru](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-to-your-application"></a><span data-ttu-id="b407e-161">Přidat test výkonu do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="b407e-161">Add a performance test to your application</span></span>
<span data-ttu-id="b407e-162">Takže shromažďujeme ukázková data v Application Insights profileru zobrazí, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b407e-162">Follow these steps so we can collect some sample data to be displayed in Application Insights Profiler:</span></span>

1. <span data-ttu-id="b407e-163">Přejděte do prostředku Application Insights, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b407e-163">Browse to the Application Insights resource that you created earlier.</span></span> 

2. <span data-ttu-id="b407e-164">Přejděte na **dostupnosti** okno a přidat test výkonu, který odesílá webových požadavků na adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="b407e-164">Go to the **Availability** blade and add a performance test that sends web requests to your application URL.</span></span> 

   ![Přidat test výkonu](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a><span data-ttu-id="b407e-166">Zobrazit data o výkonu</span><span class="sxs-lookup"><span data-stu-id="b407e-166">View your performance data</span></span>

1. <span data-ttu-id="b407e-167">Počkejte 10 až 15 minut profileru shromažďovat a analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="b407e-167">Wait 10-15 minutes for the profiler to collect and analyze the data.</span></span> 

2. <span data-ttu-id="b407e-168">Přejděte na **výkonu** okno v prostředku Application Insights a zobrazení, jak vaše aplikace provádí při zatížení.</span><span class="sxs-lookup"><span data-stu-id="b407e-168">Go to the **Performance** blade in your Application Insights resource and view how your application is performing when it's under load.</span></span>

   ![Zobrazení výkonu](./media/enable-profiler-compute/aiperformance.png)

3. <span data-ttu-id="b407e-170">Vyberte ikonu pod **příklady** otevřete **zobrazení trasování** okno.</span><span class="sxs-lookup"><span data-stu-id="b407e-170">Select the icon under **Examples** to open the **Trace View** blade.</span></span>

   ![Otevírání okna zobrazení trasování](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a><span data-ttu-id="b407e-172">Práce s existující šablony</span><span class="sxs-lookup"><span data-stu-id="b407e-172">Work with an existing template</span></span>

1. <span data-ttu-id="b407e-173">Vyhledejte deklaraci prostředků Azure Diagnostics do šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="b407e-173">Locate the Azure Diagnostics resource declaration in your deployment template.</span></span>
   
   <span data-ttu-id="b407e-174">Pokud nemáte deklaraci, můžete vytvořit jednu, která se podobá deklarace v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b407e-174">If you don't have a declaration, you can create one that resembles the declaration in the following example.</span></span> <span data-ttu-id="b407e-175">Můžete aktualizovat šablonu z [Průzkumníka prostředků Azure web](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b407e-175">You can update the template from the [Azure Resource Explorer website](https://resources.azure.com).</span></span>

2. <span data-ttu-id="b407e-176">Změnit vydavatele z `Microsoft.Azure.Diagnostics` k `AIP.Diagnostics.Test`.</span><span class="sxs-lookup"><span data-stu-id="b407e-176">Change the publisher from `Microsoft.Azure.Diagnostics` to `AIP.Diagnostics.Test`.</span></span>

3. <span data-ttu-id="b407e-177">Pro `typeHandlerVersion`, použijte `0.0`.</span><span class="sxs-lookup"><span data-stu-id="b407e-177">For `typeHandlerVersion`, use `0.0`.</span></span>

4. <span data-ttu-id="b407e-178">Ujistěte se, že `autoUpgradeMinorVersion` je nastaven na `true`.</span><span class="sxs-lookup"><span data-stu-id="b407e-178">Make sure that `autoUpgradeMinorVersion` is set to `true`.</span></span>

5. <span data-ttu-id="b407e-179">Přidejte nové `ApplicationInsightsProfiler` podřízený instance v `WadCfg` objekt nastavení, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b407e-179">Add the new `ApplicationInsightsProfiler` sink instance in the `WadCfg` settings object, as shown in the following example:</span></span>

```
"resources": [
        {
          "type": "extensions",
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "apiVersion": "2016-03-30",
          "properties": {
            "publisher": "AIP.Diagnostics.Test",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "WadCfg": {
                "SinksConfig": {
                  "Sink": [
                    {
                      "name": "Give a descriptive short name. E.g.: MyApplicationInsightsProfilerSink",
                      "ApplicationInsightsProfiler": "Enter the Application Insights instance instrumentation key guid here"
                    }
                  ]
                },
                "DiagnosticMonitorConfiguration": {
                    ...
                }
                ...
              }
              ...
            }
            ...
          }
          ...
]
```

## <a name="enable-the-profiler-on-virtual-machine-scale-sets"></a><span data-ttu-id="b407e-180">Povolit profileru na sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b407e-180">Enable the profiler on virtual machine scale sets</span></span>
<span data-ttu-id="b407e-181">Chcete-li zjistit, jak povolit profileru, stáhněte [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) šablony.</span><span class="sxs-lookup"><span data-stu-id="b407e-181">To see how to enable the profiler, download the [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) template.</span></span> <span data-ttu-id="b407e-182">Použijte stejné změny v šabloně virtuálního počítače k prostředku rozšíření diagnostiky pro škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b407e-182">Apply the same changes in a VM template to the diagnostics extension resource for the virtual machine scale set.</span></span>

<span data-ttu-id="b407e-183">Ujistěte se, že každá instance v sadě škálování má přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="b407e-183">Make sure that each instance in the scale set has access to the internet.</span></span> <span data-ttu-id="b407e-184">Agent profileru potom může odeslat shromážděných ukázky Application insights pro zobrazení a analýzu.</span><span class="sxs-lookup"><span data-stu-id="b407e-184">The Profiler Agent can then send the collected samples to Application Insights for display and analysis.</span></span>

## <a name="enable-the-profiler-on-service-fabric-applications"></a><span data-ttu-id="b407e-185">Povolit profileru na aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b407e-185">Enable the profiler on Service Fabric applications</span></span>
1. <span data-ttu-id="b407e-186">Zřídit cluster Service Fabric tak, aby měl rozšíření diagnostiky Azure, který nainstaluje agenta profileru.</span><span class="sxs-lookup"><span data-stu-id="b407e-186">Provision the Service Fabric cluster to have the Azure Diagnostics extension that installs the Profiler Agent.</span></span>

2. <span data-ttu-id="b407e-187">Nainstalujte Application Insights SDK do projektu a nakonfigurujte klíč Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b407e-187">Install the Application Insights SDK in the project and configure the Application Insights key.</span></span>

3. <span data-ttu-id="b407e-188">Přidejte do nástrojích telemetrie kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="b407e-188">Add application code to instrument telemetry.</span></span>

### <a name="provision-the-service-fabric-cluster-to-have-the-azure-diagnostics-extension-that-installs-the-profiler-agent"></a><span data-ttu-id="b407e-189">Zřídit cluster Service Fabric tak, aby měl rozšíření diagnostiky Azure, který nainstaluje agenta profileru</span><span class="sxs-lookup"><span data-stu-id="b407e-189">Provision the Service Fabric cluster to have the Azure Diagnostics extension that installs the Profiler Agent</span></span>
<span data-ttu-id="b407e-190">Cluster Service Fabric může být zabezpečené nebo nezabezpečené.</span><span class="sxs-lookup"><span data-stu-id="b407e-190">A Service Fabric cluster can be secure or non-secure.</span></span> <span data-ttu-id="b407e-191">Můžete nastavit jeden cluster brány, aby nezabezpečené nevyžaduje certifikát pro přístup.</span><span class="sxs-lookup"><span data-stu-id="b407e-191">You can set one gateway cluster to be non-secure so it doesn't require a certificate for access.</span></span> <span data-ttu-id="b407e-192">Clustery, které jsou hostiteli obchodní logiku a data by měla být zabezpečený.</span><span class="sxs-lookup"><span data-stu-id="b407e-192">Clusters that host business logic and data should be secure.</span></span> <span data-ttu-id="b407e-193">Můžete povolit profileru na zabezpečené i nezabezpečené clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b407e-193">You can enable the profiler on both secure and non-secure Service Fabric clusters.</span></span> <span data-ttu-id="b407e-194">Tento návod používá nezabezpečené clusteru jako příklad vysvětlit, jaké změny jsou požadovány pro povolení profileru.</span><span class="sxs-lookup"><span data-stu-id="b407e-194">This walkthrough uses a non-secure cluster as an example to explain what changes are required to enable the profiler.</span></span> <span data-ttu-id="b407e-195">Zabezpečení clusteru můžete zřídit stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b407e-195">You can provision a secure cluster in the same way.</span></span>

1. <span data-ttu-id="b407e-196">Stáhněte si [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span><span class="sxs-lookup"><span data-stu-id="b407e-196">Download [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span></span> <span data-ttu-id="b407e-197">Stejně jako u virtuálních počítačů a sady škálování virtuálního počítače, nahraďte `Application_Insights_Key` váš klíč Application Insights:</span><span class="sxs-lookup"><span data-stu-id="b407e-197">As you did for VMs and virtual machine scale sets, replace `Application_Insights_Key` with your Application Insights key:</span></span>

   ```
   "publisher": "AIP.Diagnostics.Test",
                 "settings": {
                   "WadCfg": {
                     "SinksConfig": {
                       "Sink": [
                         {
                           "name": "MyApplicationInsightsProfilerSinkVMSS",
                           "ApplicationInsightsProfiler": "[Application_Insights_Key]"
                         }
                       ]
                     },
   ```

2. <span data-ttu-id="b407e-198">Nasazení šablony pomocí skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b407e-198">Deploy the template by using a PowerShell script:</span></span>

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-the-application-insights-sdk-in-the-project-and-configure-the-application-insights-key"></a><span data-ttu-id="b407e-199">Nainstalovat Application Insights SDK do projektu a nakonfigurovat klíče Application Insights</span><span class="sxs-lookup"><span data-stu-id="b407e-199">Install the Application Insights SDK in the project and configure the Application Insights key</span></span>
<span data-ttu-id="b407e-200">Nainstalujte Application Insights SDK z [balíček NuGet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="b407e-200">Install the Application Insights SDK from the [NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="b407e-201">Ujistěte se, že instalujete stabilní verze 2.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b407e-201">Make sure that you install a stable version, 2.3 or later.</span></span> 

<span data-ttu-id="b407e-202">Informace o konfigurování služby Application Insights ve vašich projektů najdete v tématu [pomocí Service Fabric pomocí služby Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span><span class="sxs-lookup"><span data-stu-id="b407e-202">For information about configuring Application Insights in your projects, see [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span></span>

### <a name="add-application-code-to-instrument-telemetry"></a><span data-ttu-id="b407e-203">Přidání kódu aplikace do nástrojích telemetrie</span><span class="sxs-lookup"><span data-stu-id="b407e-203">Add application code to instrument telemetry</span></span>
1. <span data-ttu-id="b407e-204">Pro jakékoliv kód, který chcete instrumentace, přidat, pomocí příkazu kolem něj.</span><span class="sxs-lookup"><span data-stu-id="b407e-204">For any piece of code that you want to instrument, add a using statement around it.</span></span> 

   <span data-ttu-id="b407e-205">V následujícím příkladu `RunAsync` metoda provádí některé práce a `telemetryClient` třída zaznamená telemetrii po jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="b407e-205">In the following  example, the `RunAsync` method is doing some work, and the `telemetryClient` class captures the telemetry after it starts.</span></span> <span data-ttu-id="b407e-206">Událost musí jedinečný název v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b407e-206">The event needs a unique name across the application.</span></span>

   ```
   protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace the following sample code with your own logic
           //       or remove this RunAsync override if it's not needed in your service.

           while (true)
           {
               using( var operation = telemetryClient.StartOperation<RequestTelemetry>("[Insert_Event_Unique_Name]"))
               {
                   cancellationToken.ThrowIfCancellationRequested();

                   ++this.iterations;

                   ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", this.iterations);

                   await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
               }

           }
       }
   ```

2. <span data-ttu-id="b407e-207">Nasaďte aplikaci do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b407e-207">Deploy your application to the Service Fabric cluster.</span></span> <span data-ttu-id="b407e-208">Počkejte, než pro aplikaci spustit po dobu 10 minut.</span><span class="sxs-lookup"><span data-stu-id="b407e-208">Wait for the app to run for 10 minutes.</span></span> <span data-ttu-id="b407e-209">Pro lepší platit může spuštění zátěžového testu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b407e-209">For better effect, you can run a load test on the app.</span></span> <span data-ttu-id="b407e-210">Přejděte na portálu služby Application Insights **výkonu** okno a měli vidět příklady profilování trasování zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b407e-210">Go to the Application Insights portal's **Performance** blade, and you should see examples of profiling traces appear.</span></span>

<!---
Commenting out these sections for now
## Enable the Profiler on Cloud Services applications
[TODO]
## Enable the Profiler on classic Azure Virtual Machines
[TODO]
## Enable the Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a><span data-ttu-id="b407e-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b407e-211">Next steps</span></span>

- <span data-ttu-id="b407e-212">Hledání pomoci při řešení problémů profileru problémy v [profileru řešení potíží s](app-insights-profiler.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="b407e-212">Find help with troubleshooting profiler issues in [Profiler troubleshooting](app-insights-profiler.md#troubleshooting).</span></span>

- <span data-ttu-id="b407e-213">Další informace o profileru v [Application Insights profileru](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="b407e-213">Read more about the profiler in [Application Insights Profiler](app-insights-profiler.md).</span></span>
