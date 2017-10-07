---
title: "virtuální počítač Azure agenty aaaUse pro nepřetržitou integraci s volaných."
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
ms.openlocfilehash: 2388e6919d0280372166fbd325d80dafb00d7550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a><span data-ttu-id="af351-103">Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse</span><span class="sxs-lookup"><span data-stu-id="af351-103">Use Azure VM agents for continuous integration with Jenkins.</span></span>

<span data-ttu-id="af351-104">Tento rychlý start ukazuje, jak toouse hello agenty virtuálních počítačů Azure volaných modul plug-in toocreate agenta Linux (Ubuntu) na vyžádání v Azure.</span><span class="sxs-lookup"><span data-stu-id="af351-104">This quickstart shows how toouse hello Jenkins Azure VM Agents plugin toocreate an on-demand Linux (Ubuntu) agent in Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af351-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af351-105">Prerequisites</span></span>

<span data-ttu-id="af351-106">toocomplete tento rychlý start:</span><span class="sxs-lookup"><span data-stu-id="af351-106">toocomplete this quickstart:</span></span>

* <span data-ttu-id="af351-107">Pokud již nemáte volaných master, můžete začít s hello [šablona řešení](install-jenkins-solution-template.md)</span><span class="sxs-lookup"><span data-stu-id="af351-107">If you do not already have a Jenkins master, you can start with hello [Solution Template](install-jenkins-solution-template.md)</span></span> 
* <span data-ttu-id="af351-108">Odkazovat příliš[vytvořit objekt služby Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) Pokud již nemáte objektu zabezpečení služby Azure.</span><span class="sxs-lookup"><span data-stu-id="af351-108">Refer too[Create an Azure Service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) if you do not already have an Azure service principal.</span></span>

## <a name="install-azure-vm-agents-plugin"></a><span data-ttu-id="af351-109">Instalace modulu plug-in Azure VM Agents</span><span class="sxs-lookup"><span data-stu-id="af351-109">Install Azure VM Agents plugin</span></span>

<span data-ttu-id="af351-110">Když spustíte z hello [šablona řešení](install-jenkins-solution-template.md), modul plug-in hello agenta virtuálního počítače Azure je nainstalován v předloze volaných hello.</span><span class="sxs-lookup"><span data-stu-id="af351-110">If you start from hello [Solution Template](install-jenkins-solution-template.md), hello Azure VM Agent plugin is installed in hello Jenkins master.</span></span>

<span data-ttu-id="af351-111">Jinak, instalovat hello **agenti virtuálních počítačů Azure** modulu plug-in v rámci hello volaných řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="af351-111">Otherwise, install hello **Azure VM Agents** plugin from within hello Jenkins dashboard.</span></span>

## <a name="configure-hello-plugin"></a><span data-ttu-id="af351-112">Konfigurace modulu plug-in hello</span><span class="sxs-lookup"><span data-stu-id="af351-112">Configure hello plugin</span></span>

* <span data-ttu-id="af351-113">V rámci hello volaných řídicí panel, klikněte na **volaných spravovat -> Konfigurovat systém ->**.</span><span class="sxs-lookup"><span data-stu-id="af351-113">Within hello Jenkins dashboard, click **Manage Jenkins -> Configure System ->**.</span></span> <span data-ttu-id="af351-114">Posuňte se toohello dolní části stránky hello a najít oddíl hello s rozevíracím hello **přidat nové cloudové**.</span><span class="sxs-lookup"><span data-stu-id="af351-114">Scroll toohello bottom of hello page and find hello section with hello dropdown **Add new cloud**.</span></span> <span data-ttu-id="af351-115">Hello nabídce vyberte **agenti virtuálního počítače Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="af351-115">From hello menu, select **Microsoft Azure VM Agents**</span></span>
* <span data-ttu-id="af351-116">Z rozevíracího seznamu hello přihlašovací údaje Azure vyberte existující účet.</span><span class="sxs-lookup"><span data-stu-id="af351-116">Select an existing account from hello Azure Credentials dropdown.</span></span>  <span data-ttu-id="af351-117">tooadd nový **Microsoft Azure Service Principal** zadejte hello následující hodnoty: ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="af351-117">tooadd a new **Microsoft Azure Service Principal,** enter hello following values: Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span>

![Přihlašovací údaje Azure](./media/jenkins-azure-vm-agents/service-principal.png)

* <span data-ttu-id="af351-119">Klikněte na tlačítko **ověřte konfiguraci** toomake, že tato konfigurace profilu hello je správná.</span><span class="sxs-lookup"><span data-stu-id="af351-119">Click **Verify configuration** toomake sure that hello profile configuration is correct.</span></span>
* <span data-ttu-id="af351-120">Uložte konfiguraci hello a pokračovat dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="af351-120">Save hello configuration, and continue toohello next step.</span></span>

## <a name="template-configuration"></a><span data-ttu-id="af351-121">Konfigurace šablony</span><span class="sxs-lookup"><span data-stu-id="af351-121">Template configuration</span></span>

### <a name="general-configuration"></a><span data-ttu-id="af351-122">Obecná konfigurace</span><span class="sxs-lookup"><span data-stu-id="af351-122">General configuration</span></span>
<span data-ttu-id="af351-123">Potom nakonfigurujte šablonu pro použití toodefine agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="af351-123">Next, configure a template for use toodefine an Azure VM agent.</span></span> 

* <span data-ttu-id="af351-124">Klikněte na tlačítko **přidat** tooadd šablonu.</span><span class="sxs-lookup"><span data-stu-id="af351-124">Click **Add** tooadd a template.</span></span> 
* <span data-ttu-id="af351-125">Zadejte název nové šablony.</span><span class="sxs-lookup"><span data-stu-id="af351-125">Provide a name for your new template.</span></span> 
* <span data-ttu-id="af351-126">Pro hello štítek zadejte "ubuntu."</span><span class="sxs-lookup"><span data-stu-id="af351-126">For hello label, enter  "ubuntu."</span></span> <span data-ttu-id="af351-127">Tento popisek se používá během hello úlohy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="af351-127">This label is used during hello job configuration.</span></span>
* <span data-ttu-id="af351-128">Vyberte požadovanou oblast hello z pole se seznamem hello.</span><span class="sxs-lookup"><span data-stu-id="af351-128">Select hello desired region from hello combo box.</span></span>
* <span data-ttu-id="af351-129">Vyberte hello potřeby velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="af351-129">Select hello desired VM size.</span></span>
* <span data-ttu-id="af351-130">Zadejte název účtu úložiště Azure hello nebo necháte prázdné toouse hello výchozí název "jenkinsarmst."</span><span class="sxs-lookup"><span data-stu-id="af351-130">Specify hello Azure Storage account name or leave it blank toouse hello default name "jenkinsarmst."</span></span>
* <span data-ttu-id="af351-131">Zadejte dobu uchování hello v minutách.</span><span class="sxs-lookup"><span data-stu-id="af351-131">Specify hello retention time in minutes.</span></span> <span data-ttu-id="af351-132">Toto nastavení definuje hello počet minut, po které může volaných čekat před odstraněním automaticky nečinnosti agenta.</span><span class="sxs-lookup"><span data-stu-id="af351-132">This setting defines hello number of minutes Jenkins can wait before automatically deleting an idle agent.</span></span> <span data-ttu-id="af351-133">Pokud nechcete, aby automaticky odstraněn toobe nečinnosti agenty, zadejte 0.</span><span class="sxs-lookup"><span data-stu-id="af351-133">Specify 0 if you do not want idle agents toobe deleted automatically.</span></span>

![Obecná konfigurace](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a><span data-ttu-id="af351-135">Konfigurace image</span><span class="sxs-lookup"><span data-stu-id="af351-135">Image configuration</span></span>

<span data-ttu-id="af351-136">toocreate agenta Linux (Ubuntu), vyberte **referenční bitové kopie** a hello použijte následující konfiguraci jako příklad.</span><span class="sxs-lookup"><span data-stu-id="af351-136">toocreate a Linux (Ubuntu) agent, select **Image reference** and use hello following configuration as an example.</span></span> <span data-ttu-id="af351-137">Odkazovat příliš[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) hello nejnovější Azure podporováno obrázky.</span><span class="sxs-lookup"><span data-stu-id="af351-137">Refer too[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) for hello latest Azure supported images.</span></span>

* <span data-ttu-id="af351-138">Image Publisher (Vydavatel image): Canonical</span><span class="sxs-lookup"><span data-stu-id="af351-138">Image Publisher: Canonical</span></span>
* <span data-ttu-id="af351-139">Image Offer (Nabídka image): UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="af351-139">Image Offer: UbuntuServer</span></span>
* <span data-ttu-id="af351-140">Image Sku (Skladová jednotka image): 14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="af351-140">Image Sku: 14.04.5-LTS</span></span>
* <span data-ttu-id="af351-141">Image version (Verze image): latest (nejnovější)</span><span class="sxs-lookup"><span data-stu-id="af351-141">Image version: latest</span></span>
* <span data-ttu-id="af351-142">OS Type (Typ operačního systému): Linux</span><span class="sxs-lookup"><span data-stu-id="af351-142">OS Type: Linux</span></span>
* <span data-ttu-id="af351-143">Launch method (Metoda spuštění): SSH</span><span class="sxs-lookup"><span data-stu-id="af351-143">Launch method: SSH</span></span>
* <span data-ttu-id="af351-144">Zadejte přihlašovací údaje správce</span><span class="sxs-lookup"><span data-stu-id="af351-144">Provide an admin credentials</span></span>
* <span data-ttu-id="af351-145">Jako skript pro inicializaci virtuálního počítače zadejte:</span><span class="sxs-lookup"><span data-stu-id="af351-145">For VM initialization script, enter:</span></span>
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Konfigurace image](./media/jenkins-azure-vm-agents/image-config.png)

* <span data-ttu-id="af351-147">Klikněte na tlačítko **ověřte šablony** tooverify hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="af351-147">Click **Verify Template** tooverify hello configuration.</span></span>
* <span data-ttu-id="af351-148">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="af351-148">Click **Save**.</span></span>

## <a name="create-a-job-in-jenkins"></a><span data-ttu-id="af351-149">Vytvoření úlohy v Jenkinsu</span><span class="sxs-lookup"><span data-stu-id="af351-149">Create a job in Jenkins</span></span>

* <span data-ttu-id="af351-150">V rámci hello volaných řídicí panel, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="af351-150">Within hello Jenkins dashboard, click **New Item**.</span></span> 
* <span data-ttu-id="af351-151">Zadejte název, vyberte **Freestyle project** (Volný projekt) a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="af351-151">Enter a name and select **Freestyle project** and click **OK**.</span></span>
* <span data-ttu-id="af351-152">V hello **Obecné** karty, vyberte možnost "Omezit, kde je možné spustit projekt" a typ "ubuntu" ve výrazu popisku.</span><span class="sxs-lookup"><span data-stu-id="af351-152">In hello **General** tab, select "Restrict where project can be run" and type "ubuntu" in Label Expression.</span></span> <span data-ttu-id="af351-153">Zobrazí "ubuntu" v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="af351-153">You now see "ubuntu" in hello dropdown.</span></span>
* <span data-ttu-id="af351-154">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="af351-154">Click **Save**.</span></span>

![Nastavení úlohy](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a><span data-ttu-id="af351-156">Sestavení nového projektu</span><span class="sxs-lookup"><span data-stu-id="af351-156">Build your new project</span></span>

* <span data-ttu-id="af351-157">Vraťte se zpátky toohello volaných řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="af351-157">Go back toohello Jenkins dashboard.</span></span>
* <span data-ttu-id="af351-158">Vytvořit novou úlohu hello klikněte pravým tlačítkem, poté klikněte na tlačítko **sestavení teď**.</span><span class="sxs-lookup"><span data-stu-id="af351-158">Right-click hello new job you created, then click **Build now**.</span></span> <span data-ttu-id="af351-159">Spustí se sestavování.</span><span class="sxs-lookup"><span data-stu-id="af351-159">A build is kicked off.</span></span> 
* <span data-ttu-id="af351-160">Po dokončení sestavení hello přejděte příliš**konzole výstup**.</span><span class="sxs-lookup"><span data-stu-id="af351-160">Once hello build is complete, go too**Console output**.</span></span> <span data-ttu-id="af351-161">Uvidíte, že sestavení hello byla provedena vzdáleně na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="af351-161">You see that hello build was performed remotely on Azure.</span></span>

![Výstup konzoly](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a><span data-ttu-id="af351-163">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="af351-163">Reference</span></span>

* <span data-ttu-id="af351-164">Video Azure Friday: [Průběžná integrace pomocí Jenkinse s využitím agentů virtuálních počítačů Azure](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span><span class="sxs-lookup"><span data-stu-id="af351-164">Azure Friday video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span></span>
* <span data-ttu-id="af351-165">Informace o podpoře a možnosti konfigurace: [Wikiweb modulu plug-in Azure VM Agent pro Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span><span class="sxs-lookup"><span data-stu-id="af351-165">Support information and configuration options:  [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span></span> 

