---
title: "Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse"
description: "Agenti virtuálních počítačů Azure jako podřízené servery Jenkinse"
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 0b22a559fbc03158a6d4398603d1a7d2874d7b67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a><span data-ttu-id="ceee7-103">Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse</span><span class="sxs-lookup"><span data-stu-id="ceee7-103">Use Azure VM agents for continuous integration with Jenkins.</span></span>

<span data-ttu-id="ceee7-104">Tento rychlý start předvádí, jak pomocí modulu plug-in Jenkins Azure VM Agents vytvořit v Azure linuxového (Ubuntu) agenta na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="ceee7-104">This quickstart shows how to use the Jenkins Azure VM Agents plugin to create an on-demand Linux (Ubuntu) agent in Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ceee7-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ceee7-105">Prerequisites</span></span>

<span data-ttu-id="ceee7-106">K dokončení tohoto rychlého startu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="ceee7-106">To complete this quickstart:</span></span>

* <span data-ttu-id="ceee7-107">Pokud ještě nemáte hlavní server Jenkinse, můžete začít s využitím [šablony řešení](install-jenkins-solution-template.md).</span><span class="sxs-lookup"><span data-stu-id="ceee7-107">If you do not already have a Jenkins master, you can start with the [Solution Template](install-jenkins-solution-template.md)</span></span> 
* <span data-ttu-id="ceee7-108">Pokud ještě nemáte instanční objekt Azure, přečtěte si téma [Vytvoření instančního objektu Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ceee7-108">Refer to [Create an Azure Service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) if you do not already have an Azure service principal.</span></span>

## <a name="install-azure-vm-agents-plugin"></a><span data-ttu-id="ceee7-109">Instalace modulu plug-in Azure VM Agents</span><span class="sxs-lookup"><span data-stu-id="ceee7-109">Install Azure VM Agents plugin</span></span>

<span data-ttu-id="ceee7-110">Pokud začínáte z [šablony řešení](install-jenkins-solution-template.md), modul plug-in Azure VM Agents je nainstalovaný na hlavním serveru Jenkinse.</span><span class="sxs-lookup"><span data-stu-id="ceee7-110">If you start from the [Solution Template](install-jenkins-solution-template.md), the Azure VM Agent plugin is installed in the Jenkins master.</span></span>

<span data-ttu-id="ceee7-111">Jinak nainstalujte modul plug-in **Azure VM Agents** z řídicího panelu Jenkinse.</span><span class="sxs-lookup"><span data-stu-id="ceee7-111">Otherwise, install the **Azure VM Agents** plugin from within the Jenkins dashboard.</span></span>

## <a name="configure-the-plugin"></a><span data-ttu-id="ceee7-112">Konfigurace modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="ceee7-112">Configure the plugin</span></span>

* <span data-ttu-id="ceee7-113">Na řídicím panelu Jenkinse klikněte na **Manage Jenkins (Správa Jenkinse) -> Configure System (Konfigurace systému)**.</span><span class="sxs-lookup"><span data-stu-id="ceee7-113">Within the Jenkins dashboard, click **Manage Jenkins -> Configure System ->**.</span></span> <span data-ttu-id="ceee7-114">Přejděte do dolní části stránky a vyhledejte část s rozevíracím seznamem **Add new cloud** (Přidat nový cloud).</span><span class="sxs-lookup"><span data-stu-id="ceee7-114">Scroll to the bottom of the page and find the section with the dropdown **Add new cloud**.</span></span> <span data-ttu-id="ceee7-115">Z nabídky vyberte **Microsoft Azure VM Agents**.</span><span class="sxs-lookup"><span data-stu-id="ceee7-115">From the menu, select **Microsoft Azure VM Agents**</span></span>
* <span data-ttu-id="ceee7-116">Vyberte existující účet z rozevíracího seznamu Azure Credentials (Přihlašovací údaje Azure).</span><span class="sxs-lookup"><span data-stu-id="ceee7-116">Select an existing account from the Azure Credentials dropdown.</span></span>  <span data-ttu-id="ceee7-117">Pokud chcete přidat nový **Microsoft Azure Service Principal** (Instanční objekt Microsoft Azure), zadejte následující hodnoty: Subscription ID (ID předplatného), Client ID (ID klienta), Client Secret (Tajný klíč klienta) a OAuth 2.0 Token Endpoint (Koncový bod tokenu OAuth 2.0).</span><span class="sxs-lookup"><span data-stu-id="ceee7-117">To add a new **Microsoft Azure Service Principal,** enter the following values: Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span>

![Přihlašovací údaje Azure](./media/jenkins-azure-vm-agents/service-principal.png)

* <span data-ttu-id="ceee7-119">Klikněte na **Verify configuration** (Ověřit konfiguraci) a zkontrolujte správnost konfigurace profilu.</span><span class="sxs-lookup"><span data-stu-id="ceee7-119">Click **Verify configuration** to make sure that the profile configuration is correct.</span></span>
* <span data-ttu-id="ceee7-120">Uložte konfiguraci a pokračujte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="ceee7-120">Save the configuration, and continue to the next step.</span></span>

## <a name="template-configuration"></a><span data-ttu-id="ceee7-121">Konfigurace šablony</span><span class="sxs-lookup"><span data-stu-id="ceee7-121">Template configuration</span></span>

### <a name="general-configuration"></a><span data-ttu-id="ceee7-122">Obecná konfigurace</span><span class="sxs-lookup"><span data-stu-id="ceee7-122">General configuration</span></span>
<span data-ttu-id="ceee7-123">Dále nakonfigurujte šablonu, která se použije k definování agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="ceee7-123">Next, configure a template for use to define an Azure VM agent.</span></span> 

* <span data-ttu-id="ceee7-124">Kliknutím na **Add** (Přidat) přidejte šablonu.</span><span class="sxs-lookup"><span data-stu-id="ceee7-124">Click **Add** to add a template.</span></span> 
* <span data-ttu-id="ceee7-125">Zadejte název nové šablony.</span><span class="sxs-lookup"><span data-stu-id="ceee7-125">Provide a name for your new template.</span></span> 
* <span data-ttu-id="ceee7-126">Jako popisek zadejte ubuntu.</span><span class="sxs-lookup"><span data-stu-id="ceee7-126">For the label, enter  "ubuntu."</span></span> <span data-ttu-id="ceee7-127">Tento popisek se používá během konfigurace úlohy.</span><span class="sxs-lookup"><span data-stu-id="ceee7-127">This label is used during the job configuration.</span></span>
* <span data-ttu-id="ceee7-128">V poli se seznamem vyberte požadovanou oblast.</span><span class="sxs-lookup"><span data-stu-id="ceee7-128">Select the desired region from the combo box.</span></span>
* <span data-ttu-id="ceee7-129">Vyberte požadovanou velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ceee7-129">Select the desired VM size.</span></span>
* <span data-ttu-id="ceee7-130">Zadejte název účtu Azure Storage. Pokud pole ponecháte prázdné, použije se výchozí název jenkinsarmst.</span><span class="sxs-lookup"><span data-stu-id="ceee7-130">Specify the Azure Storage account name or leave it blank to use the default name "jenkinsarmst."</span></span>
* <span data-ttu-id="ceee7-131">Zadejte dobu uchování v minutách.</span><span class="sxs-lookup"><span data-stu-id="ceee7-131">Specify the retention time in minutes.</span></span> <span data-ttu-id="ceee7-132">Toto nastavení definuje počet minut, po které může Jenkins čekat před automatickým odstraněním nečinného agenta.</span><span class="sxs-lookup"><span data-stu-id="ceee7-132">This setting defines the number of minutes Jenkins can wait before automatically deleting an idle agent.</span></span> <span data-ttu-id="ceee7-133">Pokud nechcete, aby se nečinní agenti automaticky odstraňovali, zadejte 0.</span><span class="sxs-lookup"><span data-stu-id="ceee7-133">Specify 0 if you do not want idle agents to be deleted automatically.</span></span>

![Obecná konfigurace](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a><span data-ttu-id="ceee7-135">Konfigurace image</span><span class="sxs-lookup"><span data-stu-id="ceee7-135">Image configuration</span></span>

<span data-ttu-id="ceee7-136">Pokud chcete vytvořit linuxového (Ubuntu) agenta, vyberte **Image reference** (Odkaz na image) a použijte následující konfiguraci jako příklad.</span><span class="sxs-lookup"><span data-stu-id="ceee7-136">To create a Linux (Ubuntu) agent, select **Image reference** and use the following configuration as an example.</span></span> <span data-ttu-id="ceee7-137">Nejnovější podporované image Azure najdete na webu [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1).</span><span class="sxs-lookup"><span data-stu-id="ceee7-137">Refer to [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) for the latest Azure supported images.</span></span>

* <span data-ttu-id="ceee7-138">Image Publisher (Vydavatel image): Canonical</span><span class="sxs-lookup"><span data-stu-id="ceee7-138">Image Publisher: Canonical</span></span>
* <span data-ttu-id="ceee7-139">Image Offer (Nabídka image): UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="ceee7-139">Image Offer: UbuntuServer</span></span>
* <span data-ttu-id="ceee7-140">Image Sku (Skladová jednotka image): 14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="ceee7-140">Image Sku: 14.04.5-LTS</span></span>
* <span data-ttu-id="ceee7-141">Image version (Verze image): latest (nejnovější)</span><span class="sxs-lookup"><span data-stu-id="ceee7-141">Image version: latest</span></span>
* <span data-ttu-id="ceee7-142">OS Type (Typ operačního systému): Linux</span><span class="sxs-lookup"><span data-stu-id="ceee7-142">OS Type: Linux</span></span>
* <span data-ttu-id="ceee7-143">Launch method (Metoda spuštění): SSH</span><span class="sxs-lookup"><span data-stu-id="ceee7-143">Launch method: SSH</span></span>
* <span data-ttu-id="ceee7-144">Zadejte přihlašovací údaje správce</span><span class="sxs-lookup"><span data-stu-id="ceee7-144">Provide an admin credentials</span></span>
* <span data-ttu-id="ceee7-145">Jako skript pro inicializaci virtuálního počítače zadejte:</span><span class="sxs-lookup"><span data-stu-id="ceee7-145">For VM initialization script, enter:</span></span>
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Konfigurace image](./media/jenkins-azure-vm-agents/image-config.png)

* <span data-ttu-id="ceee7-147">Kliknutím na **Verify Template** (Ověřit šablonu) ověřte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ceee7-147">Click **Verify Template** to verify the configuration.</span></span>
* <span data-ttu-id="ceee7-148">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ceee7-148">Click **Save**.</span></span>

## <a name="create-a-job-in-jenkins"></a><span data-ttu-id="ceee7-149">Vytvoření úlohy v Jenkinsu</span><span class="sxs-lookup"><span data-stu-id="ceee7-149">Create a job in Jenkins</span></span>

* <span data-ttu-id="ceee7-150">Na řídicím panelu Jenkinse klikněte na **New Item** (Nová položka).</span><span class="sxs-lookup"><span data-stu-id="ceee7-150">Within the Jenkins dashboard, click **New Item**.</span></span> 
* <span data-ttu-id="ceee7-151">Zadejte název, vyberte **Freestyle project** (Volný projekt) a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ceee7-151">Enter a name and select **Freestyle project** and click **OK**.</span></span>
* <span data-ttu-id="ceee7-152">Na kartě **General** (Obecné) vyberte možnost „Restrict where project can be run“ (Omezit, kde je možné projekt spustit) a do pole Label Expression (Výraz popisku) zadejte ubuntu.</span><span class="sxs-lookup"><span data-stu-id="ceee7-152">In the **General** tab, select "Restrict where project can be run" and type "ubuntu" in Label Expression.</span></span> <span data-ttu-id="ceee7-153">V rozevíracím seznamu se teď zobrazí ubuntu.</span><span class="sxs-lookup"><span data-stu-id="ceee7-153">You now see "ubuntu" in the dropdown.</span></span>
* <span data-ttu-id="ceee7-154">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ceee7-154">Click **Save**.</span></span>

![Nastavení úlohy](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a><span data-ttu-id="ceee7-156">Sestavení nového projektu</span><span class="sxs-lookup"><span data-stu-id="ceee7-156">Build your new project</span></span>

* <span data-ttu-id="ceee7-157">Vraťte se na řídicí panel Jenkinse.</span><span class="sxs-lookup"><span data-stu-id="ceee7-157">Go back to the Jenkins dashboard.</span></span>
* <span data-ttu-id="ceee7-158">Klikněte pravým tlačítkem na novou úlohu, kterou jste vytvořili, a pak klikněte na **Build now** (Sestavit).</span><span class="sxs-lookup"><span data-stu-id="ceee7-158">Right-click the new job you created, then click **Build now**.</span></span> <span data-ttu-id="ceee7-159">Spustí se sestavování.</span><span class="sxs-lookup"><span data-stu-id="ceee7-159">A build is kicked off.</span></span> 
* <span data-ttu-id="ceee7-160">Jakmile bude sestavování dokončeno, přejděte na **Console output** (Výstup konzoly).</span><span class="sxs-lookup"><span data-stu-id="ceee7-160">Once the build is complete, go to **Console output**.</span></span> <span data-ttu-id="ceee7-161">Uvidíte, že se sestavení provedlo vzdáleně v Azure.</span><span class="sxs-lookup"><span data-stu-id="ceee7-161">You see that the build was performed remotely on Azure.</span></span>

![Výstup konzoly](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a><span data-ttu-id="ceee7-163">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ceee7-163">Reference</span></span>

* <span data-ttu-id="ceee7-164">Video Azure Friday: [Průběžná integrace pomocí Jenkinse s využitím agentů virtuálních počítačů Azure](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span><span class="sxs-lookup"><span data-stu-id="ceee7-164">Azure Friday video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span></span>
* <span data-ttu-id="ceee7-165">Informace o podpoře a možnosti konfigurace: [Wikiweb modulu plug-in Azure VM Agent pro Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span><span class="sxs-lookup"><span data-stu-id="ceee7-165">Support information and configuration options:  [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span></span> 

