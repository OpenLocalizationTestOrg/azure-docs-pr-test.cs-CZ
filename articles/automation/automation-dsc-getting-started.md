---
title: "Začínáme s Azure Automation DSC. | Microsoft Docs"
description: "Vysvětlení a příklady nejběžnější úlohy v Azure Automation požadovaného stavu konfigurace (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 8a10d961ad7c107c68b57c64ee6c88544ff8832b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a><span data-ttu-id="01b88-103">Začínáme s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-103">Getting started with Azure Automation DSC</span></span>
<span data-ttu-id="01b88-104">Toto téma vysvětluje, jak provádět běžné úkoly s Azure Automation požadovaného stavu konfigurace (DSC), jako je například vytváření, importování a kompilování konfigurace, registrace počítače, které chcete spravovat a zobrazování sestav.</span><span class="sxs-lookup"><span data-stu-id="01b88-104">This topic explains how to do the most common tasks with Azure Automation Desired State Configuration (DSC), such as creating, importing, and compiling configurations, onboarding machines to manage, and viewing reports.</span></span> <span data-ttu-id="01b88-105">Přehled co Azure Automation DSC je, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="01b88-105">For an overview of what Azure Automation DSC is, see [Azure Automation DSC Overview](automation-dsc-overview.md).</span></span> <span data-ttu-id="01b88-106">DSC dokumentaci najdete v tématu [Přehled konfigurace prostředí Windows PowerShell požadovaného stavu](https://msdn.microsoft.com/PowerShell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="01b88-106">For DSC documentation, see [Windows PowerShell Desired State Configuration Overview](https://msdn.microsoft.com/PowerShell/dsc/overview).</span></span>

<span data-ttu-id="01b88-107">Toto téma obsahuje podrobný návod, jak pomocí Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-107">This topic provides a step-by-step guide to using Azure Automation DSC.</span></span> <span data-ttu-id="01b88-108">Pokud chcete, aby ukázkové prostředí, které jsou již nastaveny bez kroků popsaných v tomto tématu, můžete použít [následující šablony ARM](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span><span class="sxs-lookup"><span data-stu-id="01b88-108">If you want a sample environment that is already set up without following the steps described in this topic, you can use [the following ARM template](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span></span> <span data-ttu-id="01b88-109">Tato šablona nastavuje dokončené prostředí Azure Automation DSC, včetně virtuálního počítače Azure, který je spravovaný nástrojem Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-109">This template sets up a completed Azure Automation DSC environment, including an Azure VM that is managed by Azure Automation DSC.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01b88-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01b88-110">Prerequisites</span></span>
<span data-ttu-id="01b88-111">Abyste mohli dokončit příklady v tomto tématu, je potřeba splnit následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="01b88-111">To complete the examples in this topic, the following are required:</span></span>

* <span data-ttu-id="01b88-112">Účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-112">An Azure Automation account.</span></span> <span data-ttu-id="01b88-113">Pokyny k vytvoření účtu Azure Automation spustit jako najdete v tématu [Azure účet Spustit jako](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="01b88-113">For instructions on creating an Azure Automation Run As account, see [Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>
* <span data-ttu-id="01b88-114">Virtuální počítač Azure Resource Manager (ne Classic) systémem Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="01b88-114">An Azure Resource Manager VM (not Classic) running Windows Server 2008 R2 or later.</span></span> <span data-ttu-id="01b88-115">Pokyny pro vytvoření virtuálního počítače, v tématu [vytvořit svůj první virtuální počítač Windows v portálu Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="01b88-115">For instructions on creating a VM, see [Create your first Windows virtual machine in the Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span></span>

## <a name="creating-a-dsc-configuration"></a><span data-ttu-id="01b88-116">Vytvoření konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="01b88-116">Creating a DSC configuration</span></span>
<span data-ttu-id="01b88-117">Vytvoříme jednoduchou [konfigurace DSC](https://msdn.microsoft.com/powershell/dsc/configurations) zajistí se tak přítomnosti nebo absenci **Webový Server** Windows funkce (IIS), v závislosti na tom, jak přiřadit uzly.</span><span class="sxs-lookup"><span data-stu-id="01b88-117">We will create a simple [DSC configuration](https://msdn.microsoft.com/powershell/dsc/configurations) that ensures either the presence or absence of the **Web-Server** Windows Feature (IIS), depending on how you assign nodes.</span></span>

1. <span data-ttu-id="01b88-118">Spusťte Windows PowerShell ISE (nebo libovolného textového editoru).</span><span class="sxs-lookup"><span data-stu-id="01b88-118">Start the Windows PowerShell ISE (or any text editor).</span></span>
2. <span data-ttu-id="01b88-119">Zadejte následující text:</span><span class="sxs-lookup"><span data-stu-id="01b88-119">Type the following text:</span></span>
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. <span data-ttu-id="01b88-120">Uložte soubor jako `TestConfig.ps1`.</span><span class="sxs-lookup"><span data-stu-id="01b88-120">Save the file as `TestConfig.ps1`.</span></span>

<span data-ttu-id="01b88-121">Tato konfigurace volá jeden prostředek v každém uzlu bloku [WindowsFeature prostředků](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), zajistí se tak přítomnosti nebo absenci **Webový Server** funkce.</span><span class="sxs-lookup"><span data-stu-id="01b88-121">This configuration calls one resource in each node block, the [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), that ensures either the presence or absence of the **Web-Server** feature.</span></span>

## <a name="importing-a-configuration-into-azure-automation"></a><span data-ttu-id="01b88-122">Konfigurace importu do Azure Automation</span><span class="sxs-lookup"><span data-stu-id="01b88-122">Importing a configuration into Azure Automation</span></span>
<span data-ttu-id="01b88-123">V dalším kroku jsme budete importovat konfiguraci do účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-123">Next, we'll import the configuration into the Automation account.</span></span>

1. <span data-ttu-id="01b88-124">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-124">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-125">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-125">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-126">Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-126">On the **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="01b88-127">Na **konfigurace DSC** okně klikněte na tlačítko **přidání konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="01b88-127">On the **DSC Configurations** blade, click **Add a configuration**.</span></span>
5. <span data-ttu-id="01b88-128">Na **importovat konfiguraci** okně Procházet a `TestConfig.ps1` souboru ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="01b88-128">On the **Import Configuration** blade, browse to the `TestConfig.ps1` file on your computer.</span></span>
   
    ![Snímek obrazovky ** okno importu konfigurace **](./media/automation-dsc-getting-started/AddConfig.png)
6. <span data-ttu-id="01b88-130">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="01b88-130">Click **OK**.</span></span>

## <a name="viewing-a-configuration-in-azure-automation"></a><span data-ttu-id="01b88-131">Zobrazení konfigurace ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="01b88-131">Viewing a configuration in Azure Automation</span></span>
<span data-ttu-id="01b88-132">Po dokončení importu konfigurace, můžete ji zobrazit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="01b88-132">After you have imported a configuration, you can view it in the Azure portal.</span></span>

1. <span data-ttu-id="01b88-133">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-133">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-134">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-134">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-135">Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**</span><span class="sxs-lookup"><span data-stu-id="01b88-135">On the **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="01b88-136">Na **konfigurace DSC** okno, klikněte na **TestConfig** (Toto je název konfigurace, které jste importovali v předchozím postupu).</span><span class="sxs-lookup"><span data-stu-id="01b88-136">On the **DSC Configurations** blade, click **TestConfig** (this is the name of the configuration you imported in the previous procedure).</span></span>
5. <span data-ttu-id="01b88-137">Na **TestConfig konfigurace** okno, klikněte na **zdroj konfigurace zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="01b88-137">On the **TestConfig Configuration** blade, click **View configuration source**.</span></span>
   
    ![Snímek obrazovky okna TestConfig konfigurace](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    <span data-ttu-id="01b88-139">A **zdroj konfigurace TestConfig** otevře okno se zobrazí kód prostředí PowerShell pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="01b88-139">A **TestConfig Configuration source** blade opens, displaying the PowerShell code for the configuration.</span></span>

## <a name="compiling-a-configuration-in-azure-automation"></a><span data-ttu-id="01b88-140">Kompilování konfigurace ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="01b88-140">Compiling a configuration in Azure Automation</span></span>
<span data-ttu-id="01b88-141">Než použijete požadovaný stav do uzlu Konfigurace DSC, definování tohoto stavu musí být zkompilovány do minimálně jedna konfigurace uzlu (dokument MOF) a na Server pro vyžádání obsahu Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-141">Before you can apply a desired state to a node, a DSC configuration defining that state must be compiled into one or more node configurations (MOF document), and placed on the Automation DSC Pull Server.</span></span> <span data-ttu-id="01b88-142">Podrobnější popis kompilování konfigurace v Azure Automation DSC, najdete v části [kompilování konfigurace v Azure Automation DSC](automation-dsc-compile.md).</span><span class="sxs-lookup"><span data-stu-id="01b88-142">For a more detailed description of compiling configurations in Azure Automation DSC, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md).</span></span> <span data-ttu-id="01b88-143">Další informace o kompilování konfigurace najdete v tématu [konfigurace DSC](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span><span class="sxs-lookup"><span data-stu-id="01b88-143">For more information about compiling configurations, see [DSC Configurations](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span></span>

1. <span data-ttu-id="01b88-144">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-144">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-145">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-145">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-146">Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**</span><span class="sxs-lookup"><span data-stu-id="01b88-146">On the **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="01b88-147">Na **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (název dřív naimportovaný konfigurace).</span><span class="sxs-lookup"><span data-stu-id="01b88-147">On the **DSC Configurations** blade, click **TestConfig** (the name of the previously imported configuration).</span></span>
5. <span data-ttu-id="01b88-148">Na **TestConfig konfigurace** okně klikněte na tlačítko **zkompilovat**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="01b88-148">On the **TestConfig Configuration** blade, click **Compile**, and then click **Yes**.</span></span> <span data-ttu-id="01b88-149">Spustí úlohu kompilace.</span><span class="sxs-lookup"><span data-stu-id="01b88-149">This starts a compilation job.</span></span>
   
    ![Snímek obrazovky okna konfigurace TestConfig zvýraznění tlačítko kompilace](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> <span data-ttu-id="01b88-151">Při kompilaci konfigurace ve službě Azure Automation, automaticky se nasadí žádnou konfiguraci vytvořený uzel soubory MOF na server vyžádané replikace.</span><span class="sxs-lookup"><span data-stu-id="01b88-151">When you compile a configuration in Azure Automation, it automatically deploys any created node configuration MOFs to the pull server.</span></span>
> 
> 

## <a name="viewing-a-compilation-job"></a><span data-ttu-id="01b88-152">Zobrazení úlohu kompilace</span><span class="sxs-lookup"><span data-stu-id="01b88-152">Viewing a compilation job</span></span>
<span data-ttu-id="01b88-153">Po spuštění kompilace, můžete ji v zobrazit **úlohy kompilace** dlaždice v nástroji **konfigurace** okno.</span><span class="sxs-lookup"><span data-stu-id="01b88-153">After you start a compilation, you can view it in the **Compilation jobs** tile in the **Configuration** blade.</span></span> <span data-ttu-id="01b88-154">**Úlohy kompilace** dlaždice ukazuje aktuálně spuštěna, byla dokončena a neúspěšné úlohy.</span><span class="sxs-lookup"><span data-stu-id="01b88-154">The **Compilation jobs** tile shows currently running, completed, and failed jobs.</span></span> <span data-ttu-id="01b88-155">Když otevřete okno úlohy kompilace, zobrazuje informace o této úloze, včetně žádné chyby nebo upozornění došlo, vstupní parametry použít v konfiguraci a kompilace protokoly.</span><span class="sxs-lookup"><span data-stu-id="01b88-155">When you open a compilation job blade, it shows information about that job including any errors or warnings encountered, input parameters used in the configuration, and compilation logs.</span></span>

1. <span data-ttu-id="01b88-156">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-156">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-157">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-157">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-158">Na **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-158">On the **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="01b88-159">Na **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (název dřív naimportovaný konfigurace).</span><span class="sxs-lookup"><span data-stu-id="01b88-159">On the **DSC Configurations** blade, click **TestConfig** (the name of the previously imported configuration).</span></span>
5. <span data-ttu-id="01b88-160">Na **úlohy kompilace** dlaždici **TestConfig konfigurace** okně klikněte na některé z uvedených úloh.</span><span class="sxs-lookup"><span data-stu-id="01b88-160">On the **Compilation jobs** tile of the **TestConfig Configuration** blade, click on any of the jobs listed.</span></span> <span data-ttu-id="01b88-161">A **úloha kompilace** otevře se okno s názvem bez přípony s datem, která byla spuštěna úloha kompilace.</span><span class="sxs-lookup"><span data-stu-id="01b88-161">A **Compilation Job** blade opens, labeled with the date that the compilation job was started.</span></span>
   
    ![Snímek obrazovky okna úlohy kompilace](./media/automation-dsc-getting-started/CompilationJob.png)
6. <span data-ttu-id="01b88-163">Kliknutím na libovolnou dlaždici v **úloha kompilace** okno zobrazíte další podrobnosti o úloze.</span><span class="sxs-lookup"><span data-stu-id="01b88-163">Click on any tile in the **Compilation Job** blade to see further details about the job.</span></span>

## <a name="viewing-node-configurations"></a><span data-ttu-id="01b88-164">Zobrazení konfigurace uzlu</span><span class="sxs-lookup"><span data-stu-id="01b88-164">Viewing node configurations</span></span>
<span data-ttu-id="01b88-165">Úspěšné dokončení úlohy kompilace vytvoří jeden nebo více nové konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="01b88-165">Successful completion of a compilation job creates one or more new node configurations.</span></span> <span data-ttu-id="01b88-166">Konfigurace uzlu je dokument MOF, který je nasazen na server vyžádané replikace a připravené k vyžádat a použít jeden nebo více uzly.</span><span class="sxs-lookup"><span data-stu-id="01b88-166">A node configuration is a MOF document that is deployed to the pull server and ready to be pulled and applied by one or more nodes.</span></span> <span data-ttu-id="01b88-167">Konfigurace uzlu si můžete prohlédnout v účtu Automation v **konfigurace uzlu DSC** okno.</span><span class="sxs-lookup"><span data-stu-id="01b88-167">You can view the node configurations in your Automation account in the **DSC Node Configurations** blade.</span></span> <span data-ttu-id="01b88-168">Konfigurace uzlu, má název s formulářem *ConfigurationName*. *NodeName*.</span><span class="sxs-lookup"><span data-stu-id="01b88-168">A node configuration has a name with the form *ConfigurationName*.*NodeName*.</span></span>

1. <span data-ttu-id="01b88-169">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-169">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-170">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-170">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-171">Na **účet Automation** okně klikněte na tlačítko **konfigurace uzlu DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-171">On the **Automation account** blade, click **DSC Node Configurations**.</span></span>
   
    ![Snímek obrazovky okna konfigurace uzlu DSC](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a><span data-ttu-id="01b88-173">Registrace virtuální počítač Azure pro správu s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-173">Onboarding an Azure VM for management with Azure Automation DSC</span></span>
<span data-ttu-id="01b88-174">Azure Automation DSC můžete použít ke správě virtuálních počítačích Azure (Classic i Resource Manager), místní virtuální počítače, Linux počítače, virtuální počítače AWS a místní fyzických počítačů.</span><span class="sxs-lookup"><span data-stu-id="01b88-174">You can use Azure Automation DSC to manage Azure VMs (both Classic and Resource Manager), on-premises VMs, Linux machines, AWS VMs, and on-premises physical machines.</span></span> <span data-ttu-id="01b88-175">V tomto tématu se nabídneme postupy zařadit pouze virtuální počítače Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="01b88-175">In this topic, we cover how to onboard only Azure Resource Manager VMs.</span></span> <span data-ttu-id="01b88-176">Informace o registraci najdete v části Další typy počítačů, [registrace počítačů pro správu Azure Automation DSC](automation-dsc-onboarding.md).</span><span class="sxs-lookup"><span data-stu-id="01b88-176">For information about onboarding other types of machines, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md).</span></span>

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a><span data-ttu-id="01b88-177">Se budou registrovat virtuální počítač Azure Resource Manager pro správu Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-177">To onboard an Azure Resource Manager VM for management by Azure Automation DSC</span></span>
1. <span data-ttu-id="01b88-178">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-178">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-179">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-179">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-180">Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-180">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01b88-181">V **uzly DSC** okně klikněte na tlačítko **přidat virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="01b88-181">In the **DSC Nodes** blade, click **Add Azure VM**.</span></span>
   
    ![Snímek obrazovky okna uzly DSC zvýraznění na tlačítko Přidat virtuální počítač Azure](./media/automation-dsc-getting-started/OnboardVM.png)
5. <span data-ttu-id="01b88-183">V **přidat virtuální počítače Azure** okně klikněte na tlačítko **vybrat virtuální počítače chcete zařadit**.</span><span class="sxs-lookup"><span data-stu-id="01b88-183">In the **Add Azure VMs** blade, click **Select virtual machines to onboard**.</span></span>
6. <span data-ttu-id="01b88-184">V **vyberte virtuální počítače** okno Vyberte virtuální počítač, který chcete připojit a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01b88-184">In the **Select VMs** blade, select the VM you want to onboard, and click **OK**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="01b88-185">Tato hodnota musí být virtuální počítač Azure Resource Manager systémem Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="01b88-185">This must be an Azure Resource Manager VM running Windows Server 2008 R2 or later.</span></span>
   > 
   > 
7. <span data-ttu-id="01b88-186">V **přidat virtuální počítače Azure** okně klikněte na tlačítko **konfigurace registrační data**.</span><span class="sxs-lookup"><span data-stu-id="01b88-186">In the **Add Azure VMs** blade, click **Configure registration data**.</span></span>
8. <span data-ttu-id="01b88-187">V **registrace** okno, zadejte název konfigurace uzlu, kterou chcete použít pro virtuální počítač v **název konfigurace uzlu** pole.</span><span class="sxs-lookup"><span data-stu-id="01b88-187">In the **Registration** blade, enter the name of the node configuration you want to apply to the VM in the **Node Configuration Name** box.</span></span> <span data-ttu-id="01b88-188">Toto musí přesně odpovídat názvu konfigurace uzlu v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-188">This must exactly match the name of a node configuration in the Automation account.</span></span> <span data-ttu-id="01b88-189">Poskytnutí názvu v tomto okamžiku je volitelné.</span><span class="sxs-lookup"><span data-stu-id="01b88-189">Providing a name at this point is optional.</span></span> <span data-ttu-id="01b88-190">Můžete změnit konfiguraci přiřazené uzlu po registraci uzlu.</span><span class="sxs-lookup"><span data-stu-id="01b88-190">You can change the assigned node configuration after onboarding the node.</span></span>
   <span data-ttu-id="01b88-191">Zkontrolujte **restartovat uzel v případě potřeby**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="01b88-191">Check **Reboot Node if Needed**, and then click **OK**.</span></span>
   
    ![Snímek obrazovky okna registrace](./media/automation-dsc-getting-started/RegisterVM.png)
   
    <span data-ttu-id="01b88-193">Konfigurace uzlu, který jste zadali se použijí pro virtuální počítač v intervalech určeného **frekvence režimu konfigurace**, a virtuální počítač bude vyhledávat aktualizace konfigurace uzlu v intervalech určeného **obnovovací frekvence**.</span><span class="sxs-lookup"><span data-stu-id="01b88-193">The node configuration you specified will be applied to the VM at intervals specified by the **Configuration Mode Frequency**, and the VM will check for updates to the node configuration at intervals specified by the **Refresh Frequency**.</span></span> <span data-ttu-id="01b88-194">Další informace o tom, jak se používají tyto hodnoty, najdete v části [konfigurace správce místní konfigurace](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span><span class="sxs-lookup"><span data-stu-id="01b88-194">For more information about how these values are used, see [Configuring the Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span></span>
9. <span data-ttu-id="01b88-195">V **přidat virtuální počítače Azure** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01b88-195">In the **Add Azure VMs** blade, click **Create**.</span></span>

<span data-ttu-id="01b88-196">Azure se spustí proces registrace virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="01b88-196">Azure will start the process of onboarding the VM.</span></span> <span data-ttu-id="01b88-197">Po dokončení virtuálního počítače se zobrazí v **uzly DSC** okno v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-197">When it is complete, the VM will show up in the **DSC Nodes** blade in the Automation account.</span></span>

## <a name="viewing-the-list-of-dsc-nodes"></a><span data-ttu-id="01b88-198">Zobrazení seznamu uzlů DSC</span><span class="sxs-lookup"><span data-stu-id="01b88-198">Viewing the list of DSC nodes</span></span>
<span data-ttu-id="01b88-199">Můžete zobrazit seznam všech počítačů, které byly zařazený, nemá pro správu ve vašem účtu Automation v **uzly DSC** okno.</span><span class="sxs-lookup"><span data-stu-id="01b88-199">You can view the list of all machines that have been onboarded for management in your Automation account in the **DSC Nodes** blade.</span></span>

1. <span data-ttu-id="01b88-200">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-200">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-201">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-201">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-202">Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-202">On the **Automation account** blade, click **DSC Nodes**.</span></span>

## <a name="viewing-reports-for-dsc-nodes"></a><span data-ttu-id="01b88-203">Prohlížení sestav pro uzly DSC</span><span class="sxs-lookup"><span data-stu-id="01b88-203">Viewing reports for DSC nodes</span></span>
<span data-ttu-id="01b88-204">Pokaždé, když Azure Automation DSC provede kontrolu konzistence na spravovaný uzel, uzel odešle zprávu o stavu zpět na server vyžádané replikace.</span><span class="sxs-lookup"><span data-stu-id="01b88-204">Each time Azure Automation DSC performs a consistency check on a managed node, the node sends a status report back to the pull server.</span></span> <span data-ttu-id="01b88-205">Tyto sestavy můžete zobrazit v okně pro tento uzel.</span><span class="sxs-lookup"><span data-stu-id="01b88-205">You can view these reports on the blade for that node.</span></span>

1. <span data-ttu-id="01b88-206">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-206">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-207">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-207">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-208">Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-208">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01b88-209">Na **sestavy** dlaždici, klikněte na žádnou z těchto sestav v seznamu.</span><span class="sxs-lookup"><span data-stu-id="01b88-209">On the **Reports** tile, click on any of the reports in the list.</span></span>
   
    ![Snímek obrazovky okna sestavy](./media/automation-dsc-getting-started/NodeReport.png)

<span data-ttu-id="01b88-211">V okně pro jednotlivé sestavy zobrazí se následující informace o stavu odpovídající kontrolu konzistence:</span><span class="sxs-lookup"><span data-stu-id="01b88-211">On the blade for an individual report, you can see the following status information for the corresponding consistency check:</span></span>

* <span data-ttu-id="01b88-212">Sestava stavu – jestli uzlu je "Kompatibilní", "Failed" konfigurace nebo uzel "Neodpovídá" (Pokud je uzel v **applyandmonitor** režimu a počítač není v požadovaném stavu).</span><span class="sxs-lookup"><span data-stu-id="01b88-212">The report status — whether the node is "Compliant", the configuration "Failed", or the node is "Not Compliant" (when the node is in **applyandmonitor** mode and the machine is not in the desired state).</span></span>
* <span data-ttu-id="01b88-213">Čas zahájení pro kontrolu konzistence.</span><span class="sxs-lookup"><span data-stu-id="01b88-213">The start time for the consistency check.</span></span>
* <span data-ttu-id="01b88-214">Celkové doby běhu pro kontrolu konzistence.</span><span class="sxs-lookup"><span data-stu-id="01b88-214">The total runtime for the consistency check.</span></span>
* <span data-ttu-id="01b88-215">Typ kontroly konzistence.</span><span class="sxs-lookup"><span data-stu-id="01b88-215">The type of consistency check.</span></span>
* <span data-ttu-id="01b88-216">Všechny chyby, včetně kód chyby a chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="01b88-216">Any errors, including the error code and error message.</span></span> 
* <span data-ttu-id="01b88-217">Veškeré prostředky DSC v konfiguraci a stavu každého prostředku (jestli uzlu je v požadovaném stavu pro tento prostředek), můžete kliknutím na každého prostředku, chcete-li získat podrobnější informace pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="01b88-217">Any DSC resources used in the configuration, and the state of each resource (whether the node is in the desired state for that resource) — you can click on each resource to get more detailed information for that resource.</span></span>
* <span data-ttu-id="01b88-218">Název, IP adresu a režim konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="01b88-218">The name, IP address, and configuration mode of the node.</span></span>

<span data-ttu-id="01b88-219">Můžete také kliknout na **zobrazit nezpracované sestavy** zobrazíte skutečná data, která uzlu odešle na server.</span><span class="sxs-lookup"><span data-stu-id="01b88-219">You can also click **View raw report** to see the actual data that the node sends to the server.</span></span> <span data-ttu-id="01b88-220">Další informace o používání dat najdete v tématu [pomocí serveru sestav DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).</span><span class="sxs-lookup"><span data-stu-id="01b88-220">For more information about using that data, see [Using a DSC report server](https://msdn.microsoft.com/powershell/dsc/reportserver).</span></span>

<span data-ttu-id="01b88-221">Může trvat delší dobu, po uzel, který je zařazený nemá před první sestava je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="01b88-221">It can take some time after a node is onboarded before the first report is available.</span></span> <span data-ttu-id="01b88-222">Možná budete muset po čekat, až 30 minut pro první zprávu můžete zaváděním uzlu.</span><span class="sxs-lookup"><span data-stu-id="01b88-222">You might need to wait up to 30 minutes for the first report after you onboard a node.</span></span>

## <a name="reassigning-a-node-to-a-different-node-configuration"></a><span data-ttu-id="01b88-223">Opětovné přiřazování uzlu do jiného uzlu Konfigurace</span><span class="sxs-lookup"><span data-stu-id="01b88-223">Reassigning a node to a different node configuration</span></span>
<span data-ttu-id="01b88-224">Můžete přiřadit uzel, který použijete jiný uzel konfigurace než ten, který původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="01b88-224">You can assign a node to use a different node configuration than the one you initially assigned.</span></span>

1. <span data-ttu-id="01b88-225">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-225">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-226">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-226">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-227">Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-227">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01b88-228">Na **uzly DSC** okně klikněte na název uzlu, který chcete přiřadit.</span><span class="sxs-lookup"><span data-stu-id="01b88-228">On the **DSC Nodes** blade, click on the name of the node you want to reassign.</span></span>
5. <span data-ttu-id="01b88-229">V okně pro tento uzel, klikněte na **přiřazení uzlu**.</span><span class="sxs-lookup"><span data-stu-id="01b88-229">On the blade for that node, click **Assign node**.</span></span>
   
    ![Snímek obrazovky okna uzlu zvýraznění tlačítko přiřadit uzlu](./media/automation-dsc-getting-started/AssignNode.png)
6. <span data-ttu-id="01b88-231">Na **přiřadit konfigurace uzlu** okně vyberte konfigurace uzlu, ke kterému chcete přiřadit uzlu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01b88-231">On the **Assign Node Configuration** blade, select the node configuration to which you want to assign the node, and then click **OK**.</span></span>
   
    ![Snímek obrazovky okna přiřadit konfigurace uzlu](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a><span data-ttu-id="01b88-233">Zrušení registrace uzlu</span><span class="sxs-lookup"><span data-stu-id="01b88-233">Unregistering a node</span></span>
<span data-ttu-id="01b88-234">Pokud již nechcete uzlu lze spravovat pomocí Azure Automation DSC, můžete zrušení její registrace.</span><span class="sxs-lookup"><span data-stu-id="01b88-234">If you no longer want a node to be managed by Azure Automation DSC, you can unregister it.</span></span>

1. <span data-ttu-id="01b88-235">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01b88-235">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01b88-236">V nabídce centra klikněte na tlačítko **všechny prostředky** a potom název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="01b88-236">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01b88-237">Na **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="01b88-237">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01b88-238">Na **uzly DSC** okně klikněte na název uzlu, kterou chcete zrušit registraci.</span><span class="sxs-lookup"><span data-stu-id="01b88-238">On the **DSC Nodes** blade, click on the name of the node you want to unregister.</span></span>
5. <span data-ttu-id="01b88-239">V okně pro tento uzel, klikněte na **Unregister**.</span><span class="sxs-lookup"><span data-stu-id="01b88-239">On the blade for that node, click **Unregister**.</span></span>
   
    ![Snímek obrazovky okna uzlu zvýraznění tlačítko Unregister](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a><span data-ttu-id="01b88-241">Související články</span><span class="sxs-lookup"><span data-stu-id="01b88-241">Related Articles</span></span>
* [<span data-ttu-id="01b88-242">Přehled služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01b88-242">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="01b88-243">Registrace počítačů pro správu Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01b88-243">Onboarding machines for management by Azure Automation DSC</span></span>](automation-dsc-onboarding.md)
* [<span data-ttu-id="01b88-244">Přehled stavu konfigurace požadovaného prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="01b88-244">Windows PowerShell Desired State Configuration Overview</span></span>](https://msdn.microsoft.com/powershell/dsc/overview)
* [<span data-ttu-id="01b88-245">Rutiny Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01b88-245">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="01b88-246">Ceny služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01b88-246">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)

