---
title: "Jak používat modul plug-in Azure podřízený s průběžnou integraci Hudsonem | Microsoft Docs"
description: "Popisuje, jak používat modul plug-in Azure podřízený s průběžnou integraci Hudsonem."
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: c11b59f8ea432075b147a391de4b7bd3331e639e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a><span data-ttu-id="ff242-103">Jak používat modul plug-in Azure podřízený s Hudsonem průběžnou integraci</span><span class="sxs-lookup"><span data-stu-id="ff242-103">How to use the Azure slave plug-in with Hudson Continuous Integration</span></span>
<span data-ttu-id="ff242-104">Modul plug-in pro Hudsonem Azure podřízený umožňuje zřídit podřízené uzly v Azure při spuštění distribuované sestavení.</span><span class="sxs-lookup"><span data-stu-id="ff242-104">The Azure slave plug-in for Hudson enables you to provision slave nodes on Azure when running distributed builds.</span></span>

## <a name="install-the-azure-slave-plug-in"></a><span data-ttu-id="ff242-105">Instalace modulu plug-in Azure podřízený</span><span class="sxs-lookup"><span data-stu-id="ff242-105">Install the Azure Slave plug-in</span></span>
1. <span data-ttu-id="ff242-106">Na řídicím panelu Hudsonem klikněte na tlačítko **spravovat Hudsonem**.</span><span class="sxs-lookup"><span data-stu-id="ff242-106">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="ff242-107">V **spravovat Hudsonem** klikněte na **Správa modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="ff242-107">In the **Manage Hudson** page, click on **Manage Plugins**.</span></span>
3. <span data-ttu-id="ff242-108">Klikněte **dostupné** kartě.</span><span class="sxs-lookup"><span data-stu-id="ff242-108">Click the **Available** tab.</span></span>
4. <span data-ttu-id="ff242-109">Klikněte na tlačítko **vyhledávání** a typ **Azure** k omezení seznamu k příslušné moduly plug-in.</span><span class="sxs-lookup"><span data-stu-id="ff242-109">Click **Search** and type **Azure** to limit the list to relevant plug-ins.</span></span>
   
    <span data-ttu-id="ff242-110">Pokud se přihlásíte vyhledejte v seznamu dostupných modulů plug-in, zjistí Azure podřízený modulu plug-in v části **správu clusteru a distribuovat sestavení** kapitoly **ostatní** kartě.</span><span class="sxs-lookup"><span data-stu-id="ff242-110">If you opt to scroll through the list of available plug-ins, you will find the Azure slave plug-in under the **Cluster Management and Distributed Build** section in the **Others** tab.</span></span>
5. <span data-ttu-id="ff242-111">Zaškrtněte políčko **modul plug-in Azure podřízený**.</span><span class="sxs-lookup"><span data-stu-id="ff242-111">Select the checkbox for **Azure Slave Plugin**.</span></span>
6. <span data-ttu-id="ff242-112">Klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="ff242-112">Click **Install**.</span></span>
7. <span data-ttu-id="ff242-113">Restartujte Hudsonem.</span><span class="sxs-lookup"><span data-stu-id="ff242-113">Restart Hudson.</span></span>

<span data-ttu-id="ff242-114">Teď, když je nainstalovaný, bude další kroky konfigurace modulu plug-in s profilem vašeho předplatného Azure a vytvořit šablonu, která se použije při vytváření virtuálního počítače pro podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="ff242-114">Now that the plug-in is installed, the next steps would be to configure the plug-in with your Azure subscription profile and to create a template that will be used in creating the VM for the slave node.</span></span>

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a><span data-ttu-id="ff242-115">Modul plug-in Azure podřízený nakonfigurovat svůj profil předplatného</span><span class="sxs-lookup"><span data-stu-id="ff242-115">Configure the Azure Slave plug-in with your subscription profile</span></span>
<span data-ttu-id="ff242-116">Odběru profil, který se také označuje jako nastavení publikování, je soubor XML, který obsahuje zabezpečené přihlašovací údaje a doplňující informace, které budete potřebovat pro práci s Azure ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff242-116">A subscription profile, also referred to as publish settings, is an XML file that contains secure credentials and some additional information you'll need to work with Azure in your development environment.</span></span> <span data-ttu-id="ff242-117">Pokud chcete konfigurovat modul plug-in Azure podřízený, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="ff242-117">To configure the Azure slave plug-in, you need:</span></span>

* <span data-ttu-id="ff242-118">Vaše id odběru</span><span class="sxs-lookup"><span data-stu-id="ff242-118">Your subscription id</span></span>
* <span data-ttu-id="ff242-119">Certifikát pro správu pro vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="ff242-119">A management certificate for your subscription</span></span>

<span data-ttu-id="ff242-120">Ty lze najít ve vaší [odběru profil].</span><span class="sxs-lookup"><span data-stu-id="ff242-120">These can be found in your [subscription profile].</span></span> <span data-ttu-id="ff242-121">Dole je příklad profilu předplatného.</span><span class="sxs-lookup"><span data-stu-id="ff242-121">Below is an example of a subscription profile.</span></span>

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

<span data-ttu-id="ff242-122">Až budete mít vaše předplatné profilu, postupujte podle těchto kroků nakonfigurujete Azure podřízený modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="ff242-122">Once you have your subscription profile, follow these steps to configure the Azure slave plug-in.</span></span>

1. <span data-ttu-id="ff242-123">Na řídicím panelu Hudsonem klikněte na tlačítko **spravovat Hudsonem**.</span><span class="sxs-lookup"><span data-stu-id="ff242-123">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="ff242-124">Klikněte na tlačítko **konfiguraci systému**.</span><span class="sxs-lookup"><span data-stu-id="ff242-124">Click **Configure System**.</span></span>
3. <span data-ttu-id="ff242-125">Projděte dolů stránce Najít **cloudu** části.</span><span class="sxs-lookup"><span data-stu-id="ff242-125">Scroll down the page to find the **Cloud** section.</span></span>
4. <span data-ttu-id="ff242-126">Klikněte na tlačítko **přidat nové cloudové > Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="ff242-126">Click **Add new cloud > Microsoft Azure**.</span></span>
   
    ![Přidat nové cloudu][add new cloud]
   
    <span data-ttu-id="ff242-128">Zobrazí pole potřebujete-li zadat podrobnosti o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="ff242-128">This will show the fields where you need to enter your subscription details.</span></span>
   
    ![Konfigurace profilu][configure profile]
5. <span data-ttu-id="ff242-130">Zkopírujte certifikát správy a id předplatného z vašeho profilu předplatného a vložte je do příslušných polí.</span><span class="sxs-lookup"><span data-stu-id="ff242-130">Copy the subscription id and management certificate from your subscription profile and paste them in the appropriate fields.</span></span>
   
    <span data-ttu-id="ff242-131">Při kopírování id a správy certifikátu předplatného **nepodporují** zahrnout uvozovky, které uzavřete hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ff242-131">When copying the subscription id and management certificate, **do not** include the quotes that enclose the values.</span></span>
6. <span data-ttu-id="ff242-132">Klikněte na **ověřte konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="ff242-132">Click on **Verify configuration**.</span></span>
7. <span data-ttu-id="ff242-133">Po konfiguraci je ověření bylo úspěšné, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff242-133">When the configuration is verified successfully, click **Save**.</span></span>

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a><span data-ttu-id="ff242-134">Nastavení šablony virtuálního počítače pro podřízený Azure modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="ff242-134">Set up a virtual machine template for the Azure Slave plug-in</span></span>
<span data-ttu-id="ff242-135">Šablonu virtuálního počítače definuje parametry, které modul plug-in použije k vytvoření podřízený uzel v Azure.</span><span class="sxs-lookup"><span data-stu-id="ff242-135">A virtual machine template defines the parameters the plug-in will use to create a slave node on Azure.</span></span> <span data-ttu-id="ff242-136">V následujících krocích jsme budete vytvoření šablony pro virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="ff242-136">In the following steps we'll be creating template for an Ubuntu VM.</span></span>

1. <span data-ttu-id="ff242-137">Na řídicím panelu Hudsonem klikněte na tlačítko **spravovat Hudsonem**.</span><span class="sxs-lookup"><span data-stu-id="ff242-137">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="ff242-138">Klikněte na **konfiguraci systému**.</span><span class="sxs-lookup"><span data-stu-id="ff242-138">Click on **Configure System**.</span></span>
3. <span data-ttu-id="ff242-139">Projděte dolů stránce Najít **cloudu** části.</span><span class="sxs-lookup"><span data-stu-id="ff242-139">Scroll down the page to find the **Cloud** section.</span></span>
4. <span data-ttu-id="ff242-140">V rámci **cloudu** část, vyhledejte **přidat šablonu virtuálního počítače Azure** a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff242-140">Within the **Cloud** section, find **Add Azure Virtual Machine Template** and click the **Add** button.</span></span>
   
    ![Přidat šablonu virtuálního počítače.][add vm template]
5. <span data-ttu-id="ff242-142">Zadejte název cloudové služby v **název** pole.</span><span class="sxs-lookup"><span data-stu-id="ff242-142">Specify a cloud service name in the **Name** field.</span></span> <span data-ttu-id="ff242-143">Pokud název, který zadáte odkazuje na existující službu cloud, virtuální počítač se zřídí v této službě.</span><span class="sxs-lookup"><span data-stu-id="ff242-143">If the name you specify refers to an existing cloud service, the VM will be provisioned in that service.</span></span> <span data-ttu-id="ff242-144">Jinak Azure vytvoří novou.</span><span class="sxs-lookup"><span data-stu-id="ff242-144">Otherwise, Azure will create a new one.</span></span>
6. <span data-ttu-id="ff242-145">V **popis** pole, zadejte text, který popisuje šablonu, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="ff242-145">In the **Description** field, enter text that describes the template you are creating.</span></span> <span data-ttu-id="ff242-146">Tyto informace je pouze pro účely písemné a nepoužívá se v zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ff242-146">This information is only for documentary purposes and is not used in provisioning a VM.</span></span>
7. <span data-ttu-id="ff242-147">V **popisky** zadejte **linux**.</span><span class="sxs-lookup"><span data-stu-id="ff242-147">In the **Labels** field, enter **linux**.</span></span> <span data-ttu-id="ff242-148">Tento popisek slouží k identifikaci šablonu, kterou vytváříte a následně slouží k odkazování šablonu při vytváření úlohy Hudsonem.</span><span class="sxs-lookup"><span data-stu-id="ff242-148">This label is used to identify the template you are creating and is subsequently used to reference the template when creating a Hudson job.</span></span>
8. <span data-ttu-id="ff242-149">Vyberte oblast, kde bude vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ff242-149">Select a region where the VM will be created.</span></span>
9. <span data-ttu-id="ff242-150">Vyberte odpovídající velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ff242-150">Select the appropriate VM size.</span></span>
10. <span data-ttu-id="ff242-151">Zadejte účet úložiště, kde bude vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ff242-151">Specify a storage account where the VM will be created.</span></span> <span data-ttu-id="ff242-152">Ujistěte se, že je ve stejné oblasti jako cloudová služba, kterou budete používat.</span><span class="sxs-lookup"><span data-stu-id="ff242-152">Make sure that it is in the same region as the cloud service you'll be using.</span></span> <span data-ttu-id="ff242-153">Pokud chcete vytvořit nové úložiště, můžete toto pole zůstat prázdné.</span><span class="sxs-lookup"><span data-stu-id="ff242-153">If you want new storage to be created, you can leave this field blank.</span></span>
11. <span data-ttu-id="ff242-154">Doba uchování určuje počet minut, než Hudsonem odstraní nečinnosti podřízený.</span><span class="sxs-lookup"><span data-stu-id="ff242-154">Retention time specifies the number of minutes before Hudson deletes an idle slave.</span></span> <span data-ttu-id="ff242-155">Nechte na výchozí hodnotu 60.</span><span class="sxs-lookup"><span data-stu-id="ff242-155">Leave this at the default value of 60.</span></span>
12. <span data-ttu-id="ff242-156">V **využití**, vyberte vhodné podmínky, když se použije tento podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="ff242-156">In **Usage**, select the appropriate condition when this slave node will be used.</span></span> <span data-ttu-id="ff242-157">Nyní, vyberte **využívají tento uzel co nejvíce**.</span><span class="sxs-lookup"><span data-stu-id="ff242-157">For now, select **Utilize this node as much as possible**.</span></span>
    
     <span data-ttu-id="ff242-158">Formulář v tomto okamžiku by vypadat poněkud podobná této:</span><span class="sxs-lookup"><span data-stu-id="ff242-158">At this point, your form would look somewhat similar to this:</span></span>
    
     ![Konfigurace šablony][template config]
13. <span data-ttu-id="ff242-160">V **řady bitovou kopii nebo Id** budete muset určit, jaké bitové kopie systému bude nainstalována na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="ff242-160">In **Image Family or Id** you have to specify what system image will be installed on your VM.</span></span> <span data-ttu-id="ff242-161">Můžete vybrat ze seznamu rodin bitové kopie, nebo zadejte vlastní image.</span><span class="sxs-lookup"><span data-stu-id="ff242-161">You can either select from a list of image families or specify a custom image.</span></span>
    
     <span data-ttu-id="ff242-162">Pokud chcete vybrat ze seznamu rodiny bitové kopie, zadejte první znak (malá a velká písmena) název rodiny bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ff242-162">If you want to select from a list of image families, enter the first character (case-sensitive) of the image family name.</span></span> <span data-ttu-id="ff242-163">Například zadáním **U** zobrazíte seznam rodiny Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="ff242-163">For instance, typing **U** will bring up a list of Ubuntu Server families.</span></span> <span data-ttu-id="ff242-164">Jakmile vyberete ze seznamu, volaných používat nejnovější verzi této bitové kopie systému z této rodiny při zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ff242-164">Once you select from the list, Jenkins will use the latest version of that system image from that family when provisioning your VM.</span></span>
    
     ![Rodiny seznamu operačního systému][OS family list]
    
     <span data-ttu-id="ff242-166">Pokud máte vlastní image, kterou chcete použít místo toho, zadejte název této vlastní bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ff242-166">If you have a custom image that you want to use instead, enter the name of that custom image.</span></span> <span data-ttu-id="ff242-167">V seznamu nejsou zobrazeny názvy vlastní image, musíte zkontrolovat, zda je správně zadán název.</span><span class="sxs-lookup"><span data-stu-id="ff242-167">Custom image names are not shown in a list so you have to ensure that the name is entered correctly.</span></span>    
    
     <span data-ttu-id="ff242-168">V tomto kurzu zadejte **U** zobrazte seznam Image Ubuntu a vyberte **Ubuntu Server 14.04 LTS**.</span><span class="sxs-lookup"><span data-stu-id="ff242-168">For this tutorial, type **U** to bring up a list of Ubuntu images and select **Ubuntu Server 14.04 LTS**.</span></span>
14. <span data-ttu-id="ff242-169">Pro **spusťte metoda**, vyberte **SSH**.</span><span class="sxs-lookup"><span data-stu-id="ff242-169">For **Launch method**, select **SSH**.</span></span>
15. <span data-ttu-id="ff242-170">Zkopírujte následující skript a vložte **Init skriptu** pole.</span><span class="sxs-lookup"><span data-stu-id="ff242-170">Copy the script below and paste in the **Init script** field.</span></span>
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     <span data-ttu-id="ff242-171">**Init skriptu** bude proveden po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ff242-171">The **Init script** will be executed after the VM is created.</span></span> <span data-ttu-id="ff242-172">V tomto příkladu skript nainstaluje ant, Java a git.</span><span class="sxs-lookup"><span data-stu-id="ff242-172">In this example, the script installs Java, git, and ant.</span></span>
16. <span data-ttu-id="ff242-173">V **uživatelské jméno** a **heslo** pole, zadejte svoje upřednostňované hodnoty pro účet správce, který se vytvoří na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="ff242-173">In the **Username** and **Password** fields, enter your preferred values for the administrator account that will be created on your VM.</span></span>
17. <span data-ttu-id="ff242-174">Klikněte na **ověřte šablony** ke kontrole, jestli jsou parametry jste zadali platný.</span><span class="sxs-lookup"><span data-stu-id="ff242-174">Click on **Verify Template** to check if the parameters you specified are valid.</span></span>
18. <span data-ttu-id="ff242-175">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff242-175">Click on **Save**.</span></span>

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a><span data-ttu-id="ff242-176">Vytvořit úlohu Hudsonem, který běží na uzlu podřízený v Azure</span><span class="sxs-lookup"><span data-stu-id="ff242-176">Create a Hudson job that runs on a slave node on Azure</span></span>
<span data-ttu-id="ff242-177">V této části budete vytváření Hudsonem úlohu, která se spustí na podřízený uzel v Azure.</span><span class="sxs-lookup"><span data-stu-id="ff242-177">In this section, you'll be creating a Hudson task that will run on a slave node on Azure.</span></span>

1. <span data-ttu-id="ff242-178">Na řídicím panelu Hudsonem klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="ff242-178">In the Hudson dashboard, click **New Job**.</span></span>
2. <span data-ttu-id="ff242-179">Zadejte název pro úlohu, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="ff242-179">Enter a name for the job you are creating.</span></span>
3. <span data-ttu-id="ff242-180">Typ úlohy, vyberte **sestavení úloha softwaru bez stylu**.</span><span class="sxs-lookup"><span data-stu-id="ff242-180">For the job type, select **Build a free-style software job**.</span></span>
4. <span data-ttu-id="ff242-181">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff242-181">Click **OK**.</span></span>
5. <span data-ttu-id="ff242-182">Na stránce konfigurace úlohy, vyberte **omezit, kde můžete spustit tento projekt**.</span><span class="sxs-lookup"><span data-stu-id="ff242-182">In the job configuration page, select **Restrict where this project can be run**.</span></span>
6. <span data-ttu-id="ff242-183">Vyberte **uzlu a popisek nabídky** a vyberte **linux** (jsme zadali tento popisek, při vytváření šablony virtuálního počítače v předchozí části).</span><span class="sxs-lookup"><span data-stu-id="ff242-183">Select **Node and label menu** and select **linux** (we specified this label when creating the virtual machine template in the previous section).</span></span>
7. <span data-ttu-id="ff242-184">V **sestavení** klikněte na tlačítko **přidat krok sestavení** a vyberte **spustit prostředí**.</span><span class="sxs-lookup"><span data-stu-id="ff242-184">In the **Build** section, click **Add build step** and select **Execute shell**.</span></span>
8. <span data-ttu-id="ff242-185">Upravte následující skript, nahraďte **{název účtu github}**, **{název projektu}**, a **{adresáři projektu}** s příslušným hodnoty a vložit upravená skript v části textu, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="ff242-185">Edit the following script, replacing **{your github account name}**, **{your project name}**, and **{your project directory}** with appropriate values, and paste the edited script in the text area that appears.</span></span>
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. <span data-ttu-id="ff242-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff242-186">Click on **Save**.</span></span>
10. <span data-ttu-id="ff242-187">Na řídicím panelu Hudsonem najít úlohu, kterou jste právě vytvořili a klikněte na **naplánovat sestavení** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff242-187">In the Hudson dashboard, find the job you just created and click on the **Schedule a build** icon.</span></span>

<span data-ttu-id="ff242-188">Hudsonem se pak vytvořit podřízený uzel pomocí šablony vytvořené v předchozí části a spustit skript, který jste zadali v kroku sestavení pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="ff242-188">Hudson will then create a slave node using the template created in the previous section and execute the script you specified in the build step for this task.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff242-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff242-189">Next Steps</span></span>
<span data-ttu-id="ff242-190">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="ff242-190">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="ff242-191">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="ff242-191">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="ff242-192">[odběru profil]: http://go.microsoft.com/fwlink/?LinkID=396395</span><span class="sxs-lookup"><span data-stu-id="ff242-192">[subscription profile]: http://go.microsoft.com/fwlink/?LinkID=396395</span></span>

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

