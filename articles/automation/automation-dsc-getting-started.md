---
title: "aaaGetting začít s Azure Automation DSC. | Microsoft Docs"
description: "Vysvětlení a příklady hello nejběžnější úlohy v Azure Automation požadovaného stavu konfigurace (DSC)"
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
ms.openlocfilehash: 82910c96e928b9264c2e1b52a5c98dc47273dcc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a><span data-ttu-id="ae030-103">Začínáme s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ae030-103">Getting started with Azure Automation DSC</span></span>
<span data-ttu-id="ae030-104">Toto téma vysvětluje, jak toodo hello běžné úkoly s Azure Automation požadovaného stavu konfigurace (DSC), jako je například vytváření, importování a kompilování konfigurace, registrace, které počítače příliš spravovat, a zobrazování sestav.</span><span class="sxs-lookup"><span data-stu-id="ae030-104">This topic explains how toodo hello most common tasks with Azure Automation Desired State Configuration (DSC), such as creating, importing, and compiling configurations, onboarding machines too manage, and viewing reports.</span></span> <span data-ttu-id="ae030-105">Přehled co Azure Automation DSC je, najdete v části [přehled Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ae030-105">For an overview of what Azure Automation DSC is, see [Azure Automation DSC Overview](automation-dsc-overview.md).</span></span> <span data-ttu-id="ae030-106">DSC dokumentaci najdete v tématu [Přehled konfigurace prostředí Windows PowerShell požadovaného stavu](https://msdn.microsoft.com/PowerShell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="ae030-106">For DSC documentation, see [Windows PowerShell Desired State Configuration Overview](https://msdn.microsoft.com/PowerShell/dsc/overview).</span></span>

<span data-ttu-id="ae030-107">Toto téma obsahuje podrobný průvodce toousing Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ae030-107">This topic provides a step-by-step guide toousing Azure Automation DSC.</span></span> <span data-ttu-id="ae030-108">Pokud chcete, aby ukázkové prostředí, které jsou již nastaveny bez hello kroků popsaných v tomto tématu, můžete použít [hello následující šablony ARM](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span><span class="sxs-lookup"><span data-stu-id="ae030-108">If you want a sample environment that is already set up without following hello steps described in this topic, you can use [hello following ARM template](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span></span> <span data-ttu-id="ae030-109">Tato šablona nastavuje dokončené prostředí Azure Automation DSC, včetně virtuálního počítače Azure, který je spravovaný nástrojem Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ae030-109">This template sets up a completed Azure Automation DSC environment, including an Azure VM that is managed by Azure Automation DSC.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae030-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ae030-110">Prerequisites</span></span>
<span data-ttu-id="ae030-111">toocomplete hello příklady v tomto tématu, hello vyžadují se následující věci:</span><span class="sxs-lookup"><span data-stu-id="ae030-111">toocomplete hello examples in this topic, hello following are required:</span></span>

* <span data-ttu-id="ae030-112">Účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-112">An Azure Automation account.</span></span> <span data-ttu-id="ae030-113">Pokyny k vytvoření účtu Azure Automation Spustit jako najdete v tématu [Účet Spustit jako pro Azure](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="ae030-113">For instructions on creating an Azure Automation Run As account, see [Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>
* <span data-ttu-id="ae030-114">Virtuální počítač Azure Resource Manager (ne Classic) systémem Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ae030-114">An Azure Resource Manager VM (not Classic) running Windows Server 2008 R2 or later.</span></span> <span data-ttu-id="ae030-115">Pokyny pro vytvoření virtuálního počítače, v tématu [vytvořit svůj první virtuální počítač Windows v hello portálu Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="ae030-115">For instructions on creating a VM, see [Create your first Windows virtual machine in hello Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span></span>

## <a name="creating-a-dsc-configuration"></a><span data-ttu-id="ae030-116">Vytvoření konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="ae030-116">Creating a DSC configuration</span></span>
<span data-ttu-id="ae030-117">Vytvoříme jednoduchou [konfigurace DSC](https://msdn.microsoft.com/powershell/dsc/configurations) zajistí se tak hello přítomnosti nebo absenci hello **Webový Server** Windows funkce (IIS), v závislosti na tom, jak přiřadit uzly.</span><span class="sxs-lookup"><span data-stu-id="ae030-117">We will create a simple [DSC configuration](https://msdn.microsoft.com/powershell/dsc/configurations) that ensures either hello presence or absence of hello **Web-Server** Windows Feature (IIS), depending on how you assign nodes.</span></span>

1. <span data-ttu-id="ae030-118">Spusťte Windows PowerShell ISE hello (nebo libovolného textového editoru).</span><span class="sxs-lookup"><span data-stu-id="ae030-118">Start hello Windows PowerShell ISE (or any text editor).</span></span>
2. <span data-ttu-id="ae030-119">Zadejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="ae030-119">Type hello following text:</span></span>
   
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
3. <span data-ttu-id="ae030-120">Uložte soubor hello jako `TestConfig.ps1`.</span><span class="sxs-lookup"><span data-stu-id="ae030-120">Save hello file as `TestConfig.ps1`.</span></span>

<span data-ttu-id="ae030-121">Tato konfigurace volá jeden prostředek v každém uzlu bloku hello [WindowsFeature prostředků](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), který zajišťuje hello přítomnosti nebo absenci hello **Webový Server** funkce.</span><span class="sxs-lookup"><span data-stu-id="ae030-121">This configuration calls one resource in each node block, hello [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), that ensures either hello presence or absence of hello **Web-Server** feature.</span></span>

## <a name="importing-a-configuration-into-azure-automation"></a><span data-ttu-id="ae030-122">Konfigurace importu do Azure Automation</span><span class="sxs-lookup"><span data-stu-id="ae030-122">Importing a configuration into Azure Automation</span></span>
<span data-ttu-id="ae030-123">V dalším kroku jsme budete importovat hello konfigurace do hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-123">Next, we'll import hello configuration into hello Automation account.</span></span>

1. <span data-ttu-id="ae030-124">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-125">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-125">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-126">Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-126">On hello **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="ae030-127">Na hello **konfigurace DSC** okně klikněte na tlačítko **přidání konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="ae030-127">On hello **DSC Configurations** blade, click **Add a configuration**.</span></span>
5. <span data-ttu-id="ae030-128">Na hello **importovat konfiguraci** okno, procházet toohello `TestConfig.ps1` souboru ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="ae030-128">On hello **Import Configuration** blade, browse toohello `TestConfig.ps1` file on your computer.</span></span>
   
    ![Snímek obrazovky hello ** okno importu konfigurace **](./media/automation-dsc-getting-started/AddConfig.png)
6. <span data-ttu-id="ae030-130">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae030-130">Click **OK**.</span></span>

## <a name="viewing-a-configuration-in-azure-automation"></a><span data-ttu-id="ae030-131">Zobrazení konfigurace ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="ae030-131">Viewing a configuration in Azure Automation</span></span>
<span data-ttu-id="ae030-132">Po dokončení importu konfigurace, můžete ji zobrazit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ae030-132">After you have imported a configuration, you can view it in hello Azure portal.</span></span>

1. <span data-ttu-id="ae030-133">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-133">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-134">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-134">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-135">Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**</span><span class="sxs-lookup"><span data-stu-id="ae030-135">On hello **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="ae030-136">Na hello **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (to je název hello hello konfigurace, které jste importovali v předchozím postupu hello).</span><span class="sxs-lookup"><span data-stu-id="ae030-136">On hello **DSC Configurations** blade, click **TestConfig** (this is hello name of hello configuration you imported in hello previous procedure).</span></span>
5. <span data-ttu-id="ae030-137">Na hello **TestConfig konfigurace** okně klikněte na tlačítko **zdroj konfigurace zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ae030-137">On hello **TestConfig Configuration** blade, click **View configuration source**.</span></span>
   
    ![Snímek obrazovky okna konfigurace TestConfig hello](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    <span data-ttu-id="ae030-139">A **zdroj konfigurace TestConfig** otevře se okno zobrazení kódu hello prostředí PowerShell pro konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-139">A **TestConfig Configuration source** blade opens, displaying hello PowerShell code for hello configuration.</span></span>

## <a name="compiling-a-configuration-in-azure-automation"></a><span data-ttu-id="ae030-140">Kompilování konfigurace ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="ae030-140">Compiling a configuration in Azure Automation</span></span>
<span data-ttu-id="ae030-141">Před použitím požadovaný stav uzlu tooa konfigurace DSC, definování tohoto stavu musí být zkompilovány do minimálně jedna konfigurace uzlu (MOF dokumentu) a umístit na hello serveru vyžádané replikace s DSC automatizace.</span><span class="sxs-lookup"><span data-stu-id="ae030-141">Before you can apply a desired state tooa node, a DSC configuration defining that state must be compiled into one or more node configurations (MOF document), and placed on hello Automation DSC Pull Server.</span></span> <span data-ttu-id="ae030-142">Podrobnější popis kompilování konfigurace v Azure Automation DSC, najdete v části [kompilování konfigurace v Azure Automation DSC](automation-dsc-compile.md).</span><span class="sxs-lookup"><span data-stu-id="ae030-142">For a more detailed description of compiling configurations in Azure Automation DSC, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md).</span></span> <span data-ttu-id="ae030-143">Další informace o kompilování konfigurace najdete v tématu [konfigurace DSC](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span><span class="sxs-lookup"><span data-stu-id="ae030-143">For more information about compiling configurations, see [DSC Configurations](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span></span>

1. <span data-ttu-id="ae030-144">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-144">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-145">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-145">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-146">Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**</span><span class="sxs-lookup"><span data-stu-id="ae030-146">On hello **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="ae030-147">Na hello **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (hello název hello předtím naimportovali konfigurace).</span><span class="sxs-lookup"><span data-stu-id="ae030-147">On hello **DSC Configurations** blade, click **TestConfig** (hello name of hello previously imported configuration).</span></span>
5. <span data-ttu-id="ae030-148">Na hello **TestConfig konfigurace** okně klikněte na tlačítko **zkompilovat**a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ae030-148">On hello **TestConfig Configuration** blade, click **Compile**, and then click **Yes**.</span></span> <span data-ttu-id="ae030-149">Spustí úlohu kompilace.</span><span class="sxs-lookup"><span data-stu-id="ae030-149">This starts a compilation job.</span></span>
   
    ![Snímek obrazovky okna konfigurace TestConfig hello zvýraznění tlačítko kompilace](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> <span data-ttu-id="ae030-151">Při kompilaci konfigurace ve službě Azure Automation, automaticky nasadí všechny vytvořené uzlu konfigurační soubory MOF toohello vyžádání obsahu server.</span><span class="sxs-lookup"><span data-stu-id="ae030-151">When you compile a configuration in Azure Automation, it automatically deploys any created node configuration MOFs toohello pull server.</span></span>
> 
> 

## <a name="viewing-a-compilation-job"></a><span data-ttu-id="ae030-152">Zobrazení úlohu kompilace</span><span class="sxs-lookup"><span data-stu-id="ae030-152">Viewing a compilation job</span></span>
<span data-ttu-id="ae030-153">Po spuštění kompilace, můžete ji zobrazit v hello **úlohy kompilace** dlaždici v hello **konfigurace** okno.</span><span class="sxs-lookup"><span data-stu-id="ae030-153">After you start a compilation, you can view it in hello **Compilation jobs** tile in hello **Configuration** blade.</span></span> <span data-ttu-id="ae030-154">Hello **úlohy kompilace** dlaždice ukazuje aktuálně spuštěna, byla dokončena a neúspěšné úlohy.</span><span class="sxs-lookup"><span data-stu-id="ae030-154">hello **Compilation jobs** tile shows currently running, completed, and failed jobs.</span></span> <span data-ttu-id="ae030-155">Otevřete okno úlohy kompilace, se zobrazí informace o této úloze, včetně žádné chyby nebo upozornění došlo, vstupní parametry v konfiguraci hello a kompilace použity protokoly.</span><span class="sxs-lookup"><span data-stu-id="ae030-155">When you open a compilation job blade, it shows information about that job including any errors or warnings encountered, input parameters used in hello configuration, and compilation logs.</span></span>

1. <span data-ttu-id="ae030-156">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-156">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-157">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-157">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-158">Na hello **účet Automation** okně klikněte na tlačítko **konfigurace DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-158">On hello **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="ae030-159">Na hello **konfigurace DSC** okně klikněte na tlačítko **TestConfig** (hello název hello předtím naimportovali konfigurace).</span><span class="sxs-lookup"><span data-stu-id="ae030-159">On hello **DSC Configurations** blade, click **TestConfig** (hello name of hello previously imported configuration).</span></span>
5. <span data-ttu-id="ae030-160">Na hello **úlohy kompilace** dlaždice hello **TestConfig konfigurace** okně klikněte na některé z uvedených úloh hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-160">On hello **Compilation jobs** tile of hello **TestConfig Configuration** blade, click on any of hello jobs listed.</span></span> <span data-ttu-id="ae030-161">A **úloha kompilace** otevře se okno s názvem bez přípony se datum hello hello úloha kompilace byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="ae030-161">A **Compilation Job** blade opens, labeled with hello date that hello compilation job was started.</span></span>
   
    ![Snímek obrazovky okna hello úloha kompilace](./media/automation-dsc-getting-started/CompilationJob.png)
6. <span data-ttu-id="ae030-163">Kliknutím na libovolnou dlaždici v hello **úloha kompilace** okno toosee další podrobnosti o úloze hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-163">Click on any tile in hello **Compilation Job** blade toosee further details about hello job.</span></span>

## <a name="viewing-node-configurations"></a><span data-ttu-id="ae030-164">Zobrazení konfigurace uzlu</span><span class="sxs-lookup"><span data-stu-id="ae030-164">Viewing node configurations</span></span>
<span data-ttu-id="ae030-165">Úspěšné dokončení úlohy kompilace vytvoří jeden nebo více nové konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="ae030-165">Successful completion of a compilation job creates one or more new node configurations.</span></span> <span data-ttu-id="ae030-166">Konfigurace uzlu je dokument MOF, který je nasazený toohello načítacího serveru a připravena toobe vyžádat a použít jeden nebo více uzly.</span><span class="sxs-lookup"><span data-stu-id="ae030-166">A node configuration is a MOF document that is deployed toohello pull server and ready toobe pulled and applied by one or more nodes.</span></span> <span data-ttu-id="ae030-167">Konfigurace uzlu hello si můžete prohlédnout v účtu Automation v hello **konfigurace uzlu DSC** okno.</span><span class="sxs-lookup"><span data-stu-id="ae030-167">You can view hello node configurations in your Automation account in hello **DSC Node Configurations** blade.</span></span> <span data-ttu-id="ae030-168">Konfigurace uzlu má název s hello formuláře *ConfigurationName*. *NodeName*.</span><span class="sxs-lookup"><span data-stu-id="ae030-168">A node configuration has a name with hello form *ConfigurationName*.*NodeName*.</span></span>

1. <span data-ttu-id="ae030-169">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-169">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-170">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-170">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-171">Na hello **účet Automation** okně klikněte na tlačítko **konfigurace uzlu DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-171">On hello **Automation account** blade, click **DSC Node Configurations**.</span></span>
   
    ![Snímek obrazovky okna konfigurace uzlu DSC hello](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a><span data-ttu-id="ae030-173">Registrace virtuální počítač Azure pro správu s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ae030-173">Onboarding an Azure VM for management with Azure Automation DSC</span></span>
<span data-ttu-id="ae030-174">Můžete použít Azure Automation DSC toomanage virtuálních počítačích Azure (Classic i Resource Manager), místní virtuální počítače, Linux počítačů, virtuálních počítačů AWS a místní fyzických počítačů.</span><span class="sxs-lookup"><span data-stu-id="ae030-174">You can use Azure Automation DSC toomanage Azure VMs (both Classic and Resource Manager), on-premises VMs, Linux machines, AWS VMs, and on-premises physical machines.</span></span> <span data-ttu-id="ae030-175">V tomto tématu nabídneme jak tooonboard jenom virtuální počítače Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ae030-175">In this topic, we cover how tooonboard only Azure Resource Manager VMs.</span></span> <span data-ttu-id="ae030-176">Informace o registraci najdete v části Další typy počítačů, [registrace počítačů pro správu Azure Automation DSC](automation-dsc-onboarding.md).</span><span class="sxs-lookup"><span data-stu-id="ae030-176">For information about onboarding other types of machines, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md).</span></span>

### <a name="tooonboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a><span data-ttu-id="ae030-177">tooonboard virtuální počítač Azure Resource Manager pro správu Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ae030-177">tooonboard an Azure Resource Manager VM for management by Azure Automation DSC</span></span>
1. <span data-ttu-id="ae030-178">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-178">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-179">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-179">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-180">Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-180">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="ae030-181">V hello **uzly DSC** okně klikněte na tlačítko **přidat virtuální počítač Azure**.</span><span class="sxs-lookup"><span data-stu-id="ae030-181">In hello **DSC Nodes** blade, click **Add Azure VM**.</span></span>
   
    ![Snímek obrazovky okna uzly DSC hello zvýraznění hello tlačítko Přidat virtuální počítač Azure](./media/automation-dsc-getting-started/OnboardVM.png)
5. <span data-ttu-id="ae030-183">V hello **přidat virtuální počítače Azure** okně klikněte na tlačítko **vyberte virtuální počítače tooonboard**.</span><span class="sxs-lookup"><span data-stu-id="ae030-183">In hello **Add Azure VMs** blade, click **Select virtual machines tooonboard**.</span></span>
6. <span data-ttu-id="ae030-184">V hello **vyberte virtuální počítače** okně vyberte hello tooonboard a klikněte na virtuální počítač **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae030-184">In hello **Select VMs** blade, select hello VM you want tooonboard, and click **OK**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ae030-185">Tato hodnota musí být virtuální počítač Azure Resource Manager systémem Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ae030-185">This must be an Azure Resource Manager VM running Windows Server 2008 R2 or later.</span></span>
   > 
   > 
7. <span data-ttu-id="ae030-186">V hello **přidat virtuální počítače Azure** okně klikněte na tlačítko **konfigurace registrační data**.</span><span class="sxs-lookup"><span data-stu-id="ae030-186">In hello **Add Azure VMs** blade, click **Configure registration data**.</span></span>
8. <span data-ttu-id="ae030-187">V hello **registrace** okno, zadejte název hello hello konfigurace uzlu, na které má tooapply toohello virtuálních počítačů v hello **název konfigurace uzlu** pole.</span><span class="sxs-lookup"><span data-stu-id="ae030-187">In hello **Registration** blade, enter hello name of hello node configuration you want tooapply toohello VM in hello **Node Configuration Name** box.</span></span> <span data-ttu-id="ae030-188">Toto musí přesně shodovat hello název konfigurace uzlu v hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-188">This must exactly match hello name of a node configuration in hello Automation account.</span></span> <span data-ttu-id="ae030-189">Poskytnutí názvu v tomto okamžiku je volitelné.</span><span class="sxs-lookup"><span data-stu-id="ae030-189">Providing a name at this point is optional.</span></span> <span data-ttu-id="ae030-190">Konfigurace uzlu hello přiřazené po registraci hello uzlu, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="ae030-190">You can change hello assigned node configuration after onboarding hello node.</span></span>
   <span data-ttu-id="ae030-191">Zkontrolujte **restartovat uzel v případě potřeby**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae030-191">Check **Reboot Node if Needed**, and then click **OK**.</span></span>
   
    ![Snímek obrazovky okna registrace hello](./media/automation-dsc-getting-started/RegisterVM.png)
   
    <span data-ttu-id="ae030-193">Konfigurace uzlu Hello jste zadali bude použité toohello virtuálních počítačů v intervalech určeného hello **frekvence režimu konfigurace**, a hello virtuálních počítačů bude kontrolovat aktualizace konfigurace uzlu toohello v intervalech určeného hello  **Obnovovací frekvence**.</span><span class="sxs-lookup"><span data-stu-id="ae030-193">hello node configuration you specified will be applied toohello VM at intervals specified by hello **Configuration Mode Frequency**, and hello VM will check for updates toohello node configuration at intervals specified by hello **Refresh Frequency**.</span></span> <span data-ttu-id="ae030-194">Další informace o tom, jak se používají tyto hodnoty, najdete v části [hello konfigurace správce místní konfigurace](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span><span class="sxs-lookup"><span data-stu-id="ae030-194">For more information about how these values are used, see [Configuring hello Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span></span>
9. <span data-ttu-id="ae030-195">V hello **přidat virtuální počítače Azure** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ae030-195">In hello **Add Azure VMs** blade, click **Create**.</span></span>

<span data-ttu-id="ae030-196">Azure začne hello proces registrace hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ae030-196">Azure will start hello process of onboarding hello VM.</span></span> <span data-ttu-id="ae030-197">Po dokončení hello virtuálního počítače se zobrazí v hello **uzly DSC** okno v hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-197">When it is complete, hello VM will show up in hello **DSC Nodes** blade in hello Automation account.</span></span>

## <a name="viewing-hello-list-of-dsc-nodes"></a><span data-ttu-id="ae030-198">Zobrazení seznamu hello uzlů DSC</span><span class="sxs-lookup"><span data-stu-id="ae030-198">Viewing hello list of DSC nodes</span></span>
<span data-ttu-id="ae030-199">Můžete zobrazit seznam všech počítačů, které byly zařazený, nemá pro správu ve vašem účtu Automation v hello hello **uzly DSC** okno.</span><span class="sxs-lookup"><span data-stu-id="ae030-199">You can view hello list of all machines that have been onboarded for management in your Automation account in hello **DSC Nodes** blade.</span></span>

1. <span data-ttu-id="ae030-200">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-200">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-201">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-201">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-202">Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-202">On hello **Automation account** blade, click **DSC Nodes**.</span></span>

## <a name="viewing-reports-for-dsc-nodes"></a><span data-ttu-id="ae030-203">Prohlížení sestav pro uzly DSC</span><span class="sxs-lookup"><span data-stu-id="ae030-203">Viewing reports for DSC nodes</span></span>
<span data-ttu-id="ae030-204">Pokaždé, když Azure Automation DSC provede kontrolu konzistence na spravovaný uzel, uzel hello odešle serveru vyžádání back toohello sestav stavu.</span><span class="sxs-lookup"><span data-stu-id="ae030-204">Each time Azure Automation DSC performs a consistency check on a managed node, hello node sends a status report back toohello pull server.</span></span> <span data-ttu-id="ae030-205">Tyto sestavy můžete zobrazit v okně hello pro tento uzel.</span><span class="sxs-lookup"><span data-stu-id="ae030-205">You can view these reports on hello blade for that node.</span></span>

1. <span data-ttu-id="ae030-206">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-206">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-207">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-207">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-208">Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-208">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="ae030-209">Na hello **sestavy** dlaždici, klikněte na libovolný hello sestavy v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-209">On hello **Reports** tile, click on any of hello reports in hello list.</span></span>
   
    ![Snímek obrazovky okna sestavy hello](./media/automation-dsc-getting-started/NodeReport.png)

<span data-ttu-id="ae030-211">V okně hello jednotlivé sestavy můžete zjistit hello následující informace o stavu odpovídající konzistenci hello zkontrolujte:</span><span class="sxs-lookup"><span data-stu-id="ae030-211">On hello blade for an individual report, you can see hello following status information for hello corresponding consistency check:</span></span>

* <span data-ttu-id="ae030-212">Hello zprávy o stavu – hello uzel "Kompatibilní", konfigurace hello "Failed", zda je uzel hello je "Neodpovídá" (Pokud je uzel hello v **applyandmonitor** režim a hello počítač není ve stavu hello potřeby).</span><span class="sxs-lookup"><span data-stu-id="ae030-212">hello report status — whether hello node is "Compliant", hello configuration "Failed", or hello node is "Not Compliant" (when hello node is in **applyandmonitor** mode and hello machine is not in hello desired state).</span></span>
* <span data-ttu-id="ae030-213">Hello počáteční čas pro kontrolu konzistence hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-213">hello start time for hello consistency check.</span></span>
* <span data-ttu-id="ae030-214">Zkontrolujte celkové doby běhu Hello konzistenci hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-214">hello total runtime for hello consistency check.</span></span>
* <span data-ttu-id="ae030-215">Zkontrolujte typ Hello konzistence.</span><span class="sxs-lookup"><span data-stu-id="ae030-215">hello type of consistency check.</span></span>
* <span data-ttu-id="ae030-216">Všechny chyby, včetně hello chyba kódu a chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="ae030-216">Any errors, including hello error code and error message.</span></span> 
* <span data-ttu-id="ae030-217">Veškeré prostředky DSC v hello konfigurace a stavu hello každého prostředku (zda uzel hello je ve stavu hello požadovaných pro tento prostředek) – kliknutím na každého prostředku tooget podrobnější informace pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="ae030-217">Any DSC resources used in hello configuration, and hello state of each resource (whether hello node is in hello desired state for that resource) — you can click on each resource tooget more detailed information for that resource.</span></span>
* <span data-ttu-id="ae030-218">Hello název, IP adresa a režim konfigurace uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-218">hello name, IP address, and configuration mode of hello node.</span></span>

<span data-ttu-id="ae030-219">Můžete také kliknout na **zobrazit nezpracované sestavy** hello toosee skutečné se data, která hello uzlu, odešle toohello server.</span><span class="sxs-lookup"><span data-stu-id="ae030-219">You can also click **View raw report** toosee hello actual data that hello node sends toohello server.</span></span> <span data-ttu-id="ae030-220">Další informace o používání dat najdete v tématu [pomocí serveru sestav DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).</span><span class="sxs-lookup"><span data-stu-id="ae030-220">For more information about using that data, see [Using a DSC report server](https://msdn.microsoft.com/powershell/dsc/reportserver).</span></span>

<span data-ttu-id="ae030-221">Může trvat delší dobu, po uzel, který je zařazený nemá před hello první sestava je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ae030-221">It can take some time after a node is onboarded before hello first report is available.</span></span> <span data-ttu-id="ae030-222">Bude pravděpodobně nutné toowait až too30 minut pro první sestavu hello po můžete zaváděním uzlu.</span><span class="sxs-lookup"><span data-stu-id="ae030-222">You might need toowait up too30 minutes for hello first report after you onboard a node.</span></span>

## <a name="reassigning-a-node-tooa-different-node-configuration"></a><span data-ttu-id="ae030-223">Opětovné přiřazování konfigurace uzlu tooa jiného uzlu</span><span class="sxs-lookup"><span data-stu-id="ae030-223">Reassigning a node tooa different node configuration</span></span>
<span data-ttu-id="ae030-224">Toouse uzlu můžete přiřadit konfiguraci jiného uzlu, než jeden, ke které původně přiřazený hello.</span><span class="sxs-lookup"><span data-stu-id="ae030-224">You can assign a node toouse a different node configuration than hello one you initially assigned.</span></span>

1. <span data-ttu-id="ae030-225">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-225">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-226">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-226">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-227">Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-227">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="ae030-228">Na hello **uzly DSC** okně klikněte na název hello hello uzlu chcete tooreassign.</span><span class="sxs-lookup"><span data-stu-id="ae030-228">On hello **DSC Nodes** blade, click on hello name of hello node you want tooreassign.</span></span>
5. <span data-ttu-id="ae030-229">V okně hello pro tento uzel, klikněte na tlačítko **přiřazení uzlu**.</span><span class="sxs-lookup"><span data-stu-id="ae030-229">On hello blade for that node, click **Assign node**.</span></span>
   
    ![Snímek obrazovky okna uzlu hello zvýraznění tlačítko přiřadit uzlu hello](./media/automation-dsc-getting-started/AssignNode.png)
6. <span data-ttu-id="ae030-231">Na hello **přiřadit konfigurace uzlu** okně, vyberte hello uzlu Konfigurace toowhich tooassign hello uzel a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae030-231">On hello **Assign Node Configuration** blade, select hello node configuration toowhich you want tooassign hello node, and then click **OK**.</span></span>
   
    ![Snímek obrazovky okna hello přiřadit konfigurace uzlu](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a><span data-ttu-id="ae030-233">Zrušení registrace uzlu</span><span class="sxs-lookup"><span data-stu-id="ae030-233">Unregistering a node</span></span>
<span data-ttu-id="ae030-234">Pokud již nechcete uzlu toobe, spravuje Azure Automation DSC, můžete zrušení její registrace.</span><span class="sxs-lookup"><span data-stu-id="ae030-234">If you no longer want a node toobe managed by Azure Automation DSC, you can unregister it.</span></span>

1. <span data-ttu-id="ae030-235">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae030-235">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae030-236">V nabídce centra hello, klikněte na tlačítko **všechny prostředky** a pak hello název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="ae030-236">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="ae030-237">Na hello **účet Automation** okně klikněte na tlačítko **uzly DSC**.</span><span class="sxs-lookup"><span data-stu-id="ae030-237">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="ae030-238">Na hello **uzly DSC** okně klikněte na název hello hello uzlu chcete toounregister.</span><span class="sxs-lookup"><span data-stu-id="ae030-238">On hello **DSC Nodes** blade, click on hello name of hello node you want toounregister.</span></span>
5. <span data-ttu-id="ae030-239">V okně hello pro tento uzel, klikněte na tlačítko **Unregister**.</span><span class="sxs-lookup"><span data-stu-id="ae030-239">On hello blade for that node, click **Unregister**.</span></span>
   
    ![Snímek obrazovky okna uzlu hello zvýraznění tlačítko Unregister hello](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a><span data-ttu-id="ae030-241">Související články</span><span class="sxs-lookup"><span data-stu-id="ae030-241">Related Articles</span></span>
* [<span data-ttu-id="ae030-242">Přehled služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="ae030-242">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="ae030-243">Registrace počítačů pro správu Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ae030-243">Onboarding machines for management by Azure Automation DSC</span></span>](automation-dsc-onboarding.md)
* [<span data-ttu-id="ae030-244">Přehled stavu konfigurace požadovaného prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae030-244">Windows PowerShell Desired State Configuration Overview</span></span>](https://msdn.microsoft.com/powershell/dsc/overview)
* [<span data-ttu-id="ae030-245">Rutiny Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="ae030-245">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="ae030-246">Ceny služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="ae030-246">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)

