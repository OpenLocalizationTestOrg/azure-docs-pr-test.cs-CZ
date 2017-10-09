---
title: "aaaHow toouse hello Azure podřízený modul plug-in s průběžnou integraci Hudsonem | Microsoft Docs"
description: "Popisuje, jak toouse hello Azure podřízený s Hudsonem průběžnou integraci modulu plug-in."
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
ms.openlocfilehash: cd6e67ad71c208aa56746aa8b70ba507da20bee9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-slave-plug-in-with-hudson-continuous-integration"></a><span data-ttu-id="f4bba-103">Jak toouse hello Azure podřízený s Hudsonem průběžnou integraci modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="f4bba-103">How toouse hello Azure slave plug-in with Hudson Continuous Integration</span></span>
<span data-ttu-id="f4bba-104">Hello Azure podřízený modulu plug-in pro Hudsonem umožňuje tooprovision podřízené uzly v Azure při spuštění distribuované sestavení.</span><span class="sxs-lookup"><span data-stu-id="f4bba-104">hello Azure slave plug-in for Hudson enables you tooprovision slave nodes on Azure when running distributed builds.</span></span>

## <a name="install-hello-azure-slave-plug-in"></a><span data-ttu-id="f4bba-105">Instalace modulu plug-in Azure podřízený hello</span><span class="sxs-lookup"><span data-stu-id="f4bba-105">Install hello Azure Slave plug-in</span></span>
1. <span data-ttu-id="f4bba-106">V hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-106">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="f4bba-107">V hello **spravovat Hudsonem** klikněte na **Správa modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-107">In hello **Manage Hudson** page, click on **Manage Plugins**.</span></span>
3. <span data-ttu-id="f4bba-108">Klikněte na tlačítko hello **dostupné** kartě.</span><span class="sxs-lookup"><span data-stu-id="f4bba-108">Click hello **Available** tab.</span></span>
4. <span data-ttu-id="f4bba-109">Klikněte na tlačítko **vyhledávání** a typ **Azure** toolimit hello seznamu toorelevant zásuvné moduly.</span><span class="sxs-lookup"><span data-stu-id="f4bba-109">Click **Search** and type **Azure** toolimit hello list toorelevant plug-ins.</span></span>
   
    <span data-ttu-id="f4bba-110">Pokud se přihlásíte tooscroll prostřednictvím hello seznamu dostupných modulů plug-in, zjistí hello Azure podřízený modulu plug-in pod hello **správu clusteru a distribuované sestavení** část v hello **ostatní** kartě.</span><span class="sxs-lookup"><span data-stu-id="f4bba-110">If you opt tooscroll through hello list of available plug-ins, you will find hello Azure slave plug-in under hello **Cluster Management and Distributed Build** section in hello **Others** tab.</span></span>
5. <span data-ttu-id="f4bba-111">Zaškrtněte políčko hello pro **modul plug-in Azure podřízený**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-111">Select hello checkbox for **Azure Slave Plugin**.</span></span>
6. <span data-ttu-id="f4bba-112">Klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-112">Click **Install**.</span></span>
7. <span data-ttu-id="f4bba-113">Restartujte Hudsonem.</span><span class="sxs-lookup"><span data-stu-id="f4bba-113">Restart Hudson.</span></span>

<span data-ttu-id="f4bba-114">Nyní je nainstalován tento modul plug-in hello, bude další kroky hello tooconfigure hello modul plug-in s profil předplatného Azure a toocreate šablonu, která se použije při vytváření hello virtuálních počítačů pro hello podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="f4bba-114">Now that hello plug-in is installed, hello next steps would be tooconfigure hello plug-in with your Azure subscription profile and toocreate a template that will be used in creating hello VM for hello slave node.</span></span>

## <a name="configure-hello-azure-slave-plug-in-with-your-subscription-profile"></a><span data-ttu-id="f4bba-115">Hello Azure podřízený modul plug-in nakonfigurovat svůj profil předplatného</span><span class="sxs-lookup"><span data-stu-id="f4bba-115">Configure hello Azure Slave plug-in with your subscription profile</span></span>
<span data-ttu-id="f4bba-116">Profil předplatné také odkazované tooas nastavení publikování, je soubor XML, který obsahuje zabezpečené přihlašovací údaje a některé další informace, které budete potřebovat toowork s Azure ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="f4bba-116">A subscription profile, also referred tooas publish settings, is an XML file that contains secure credentials and some additional information you'll need toowork with Azure in your development environment.</span></span> <span data-ttu-id="f4bba-117">tooconfigure hello Azure podřízený modul plug-in, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f4bba-117">tooconfigure hello Azure slave plug-in, you need:</span></span>

* <span data-ttu-id="f4bba-118">Vaše id odběru</span><span class="sxs-lookup"><span data-stu-id="f4bba-118">Your subscription id</span></span>
* <span data-ttu-id="f4bba-119">Certifikát pro správu pro vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="f4bba-119">A management certificate for your subscription</span></span>

<span data-ttu-id="f4bba-120">Ty lze najít ve vaší [odběru profil].</span><span class="sxs-lookup"><span data-stu-id="f4bba-120">These can be found in your [subscription profile].</span></span> <span data-ttu-id="f4bba-121">Dole je příklad profilu předplatného.</span><span class="sxs-lookup"><span data-stu-id="f4bba-121">Below is an example of a subscription profile.</span></span>

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

<span data-ttu-id="f4bba-122">Jakmile je váš profil předplatného, postupujte podle těchto kroků tooconfigure hello Azure podřízený modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="f4bba-122">Once you have your subscription profile, follow these steps tooconfigure hello Azure slave plug-in.</span></span>

1. <span data-ttu-id="f4bba-123">V hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-123">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="f4bba-124">Klikněte na tlačítko **konfiguraci systému**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-124">Click **Configure System**.</span></span>
3. <span data-ttu-id="f4bba-125">Projděte dolů hello toofind stránku hello **cloudu** části.</span><span class="sxs-lookup"><span data-stu-id="f4bba-125">Scroll down hello page toofind hello **Cloud** section.</span></span>
4. <span data-ttu-id="f4bba-126">Klikněte na tlačítko **přidat nové cloudové > Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-126">Click **Add new cloud > Microsoft Azure**.</span></span>
   
    ![Přidat nové cloudu][add new cloud]
   
    <span data-ttu-id="f4bba-128">Pole text hello, kde je nutné tooenter zobrazí podrobnosti o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="f4bba-128">This will show hello fields where you need tooenter your subscription details.</span></span>
   
    ![Konfigurace profilu][configure profile]
5. <span data-ttu-id="f4bba-130">Zkopírovat hello předplatné id a správy certifikátu z profilu předplatného a vložte je do příslušných polí hello.</span><span class="sxs-lookup"><span data-stu-id="f4bba-130">Copy hello subscription id and management certificate from your subscription profile and paste them in hello appropriate fields.</span></span>
   
    <span data-ttu-id="f4bba-131">Při kopírování id a správy certifikátu předplatného hello **nepodporují** zahrnují hello uvozovky, které uzavřete hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f4bba-131">When copying hello subscription id and management certificate, **do not** include hello quotes that enclose hello values.</span></span>
6. <span data-ttu-id="f4bba-132">Klikněte na **ověřte konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-132">Click on **Verify configuration**.</span></span>
7. <span data-ttu-id="f4bba-133">Po konfiguraci hello je ověření bylo úspěšné, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-133">When hello configuration is verified successfully, click **Save**.</span></span>

## <a name="set-up-a-virtual-machine-template-for-hello-azure-slave-plug-in"></a><span data-ttu-id="f4bba-134">Nastavení šablony virtuálního počítače pro hello Azure podřízený modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="f4bba-134">Set up a virtual machine template for hello Azure Slave plug-in</span></span>
<span data-ttu-id="f4bba-135">Šablonu virtuálního počítače definuje parametry hello hello modul plug-in použije toocreate podřízený uzel v Azure.</span><span class="sxs-lookup"><span data-stu-id="f4bba-135">A virtual machine template defines hello parameters hello plug-in will use toocreate a slave node on Azure.</span></span> <span data-ttu-id="f4bba-136">V následující kroky hello jsme budete vytvoření šablony pro virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="f4bba-136">In hello following steps we'll be creating template for an Ubuntu VM.</span></span>

1. <span data-ttu-id="f4bba-137">V hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-137">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="f4bba-138">Klikněte na **konfiguraci systému**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-138">Click on **Configure System**.</span></span>
3. <span data-ttu-id="f4bba-139">Projděte dolů hello toofind stránku hello **cloudu** části.</span><span class="sxs-lookup"><span data-stu-id="f4bba-139">Scroll down hello page toofind hello **Cloud** section.</span></span>
4. <span data-ttu-id="f4bba-140">V rámci hello **cloudu** vyhledejte **přidat šablonu virtuálního počítače Azure** a klikněte na tlačítko hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f4bba-140">Within hello **Cloud** section, find **Add Azure Virtual Machine Template** and click hello **Add** button.</span></span>
   
    ![Přidat šablonu virtuálního počítače.][add vm template]
5. <span data-ttu-id="f4bba-142">Zadejte název cloudové služby v hello **název** pole.</span><span class="sxs-lookup"><span data-stu-id="f4bba-142">Specify a cloud service name in hello **Name** field.</span></span> <span data-ttu-id="f4bba-143">Pokud zadáte název hello odkazuje tooan stávající cloudovou službu, hello virtuálního počítače se zřídí v této službě.</span><span class="sxs-lookup"><span data-stu-id="f4bba-143">If hello name you specify refers tooan existing cloud service, hello VM will be provisioned in that service.</span></span> <span data-ttu-id="f4bba-144">Jinak Azure vytvoří novou.</span><span class="sxs-lookup"><span data-stu-id="f4bba-144">Otherwise, Azure will create a new one.</span></span>
6. <span data-ttu-id="f4bba-145">V hello **popis** pole, zadejte text, který popisuje hello šablonu, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="f4bba-145">In hello **Description** field, enter text that describes hello template you are creating.</span></span> <span data-ttu-id="f4bba-146">Tyto informace je pouze pro účely písemné a nepoužívá se v zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4bba-146">This information is only for documentary purposes and is not used in provisioning a VM.</span></span>
7. <span data-ttu-id="f4bba-147">V hello **popisky** zadejte **linux**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-147">In hello **Labels** field, enter **linux**.</span></span> <span data-ttu-id="f4bba-148">Tento popisek je použité tooidentify hello šablonu, kterou vytváříte a následně použít tooreference hello šablonu při vytváření úlohy Hudsonem.</span><span class="sxs-lookup"><span data-stu-id="f4bba-148">This label is used tooidentify hello template you are creating and is subsequently used tooreference hello template when creating a Hudson job.</span></span>
8. <span data-ttu-id="f4bba-149">Vyberte oblast, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f4bba-149">Select a region where hello VM will be created.</span></span>
9. <span data-ttu-id="f4bba-150">Vyberte odpovídající velikost virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="f4bba-150">Select hello appropriate VM size.</span></span>
10. <span data-ttu-id="f4bba-151">Zadejte účet úložiště, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f4bba-151">Specify a storage account where hello VM will be created.</span></span> <span data-ttu-id="f4bba-152">Ujistěte se, že je v hello stejné oblasti jako hello cloudové služby, které budete používat.</span><span class="sxs-lookup"><span data-stu-id="f4bba-152">Make sure that it is in hello same region as hello cloud service you'll be using.</span></span> <span data-ttu-id="f4bba-153">Pokud chcete vytvořit nové úložiště toobe, můžete toto pole zůstat prázdné.</span><span class="sxs-lookup"><span data-stu-id="f4bba-153">If you want new storage toobe created, you can leave this field blank.</span></span>
11. <span data-ttu-id="f4bba-154">Doba uchování určuje hello počet minut, než Hudsonem odstraní nečinnosti podřízený.</span><span class="sxs-lookup"><span data-stu-id="f4bba-154">Retention time specifies hello number of minutes before Hudson deletes an idle slave.</span></span> <span data-ttu-id="f4bba-155">Nechte na hello výchozí hodnotu 60.</span><span class="sxs-lookup"><span data-stu-id="f4bba-155">Leave this at hello default value of 60.</span></span>
12. <span data-ttu-id="f4bba-156">V **využití**, vyberte hello vhodné podmínky, když se použije tento podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="f4bba-156">In **Usage**, select hello appropriate condition when this slave node will be used.</span></span> <span data-ttu-id="f4bba-157">Nyní, vyberte **využívají tento uzel co nejvíce**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-157">For now, select **Utilize this node as much as possible**.</span></span>
    
     <span data-ttu-id="f4bba-158">V tomto okamžiku by formulář vypadat poněkud podobný toothis:</span><span class="sxs-lookup"><span data-stu-id="f4bba-158">At this point, your form would look somewhat similar toothis:</span></span>
    
     ![Konfigurace šablony][template config]
13. <span data-ttu-id="f4bba-160">V **řady bitovou kopii nebo Id** máte toospecify jaké bitové kopie systému bude nainstalována na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="f4bba-160">In **Image Family or Id** you have toospecify what system image will be installed on your VM.</span></span> <span data-ttu-id="f4bba-161">Můžete vybrat ze seznamu rodin bitové kopie, nebo zadejte vlastní image.</span><span class="sxs-lookup"><span data-stu-id="f4bba-161">You can either select from a list of image families or specify a custom image.</span></span>
    
     <span data-ttu-id="f4bba-162">Pokud chcete tooselect ze seznamu rodiny bitové kopie, zadejte hello první znak (malá a velká písmena) název rodiny hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f4bba-162">If you want tooselect from a list of image families, enter hello first character (case-sensitive) of hello image family name.</span></span> <span data-ttu-id="f4bba-163">Například zadáním **U** zobrazíte seznam rodiny Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="f4bba-163">For instance, typing **U** will bring up a list of Ubuntu Server families.</span></span> <span data-ttu-id="f4bba-164">Jakmile vyberete ze seznamu hello volaných používat nejnovější verzi této bitové kopie systému z této rodiny hello při zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4bba-164">Once you select from hello list, Jenkins will use hello latest version of that system image from that family when provisioning your VM.</span></span>
    
     ![Rodiny seznamu operačního systému][OS family list]
    
     <span data-ttu-id="f4bba-166">Pokud máte vlastní image, které chcete toouse místo toho, zadejte název této vlastní image hello.</span><span class="sxs-lookup"><span data-stu-id="f4bba-166">If you have a custom image that you want toouse instead, enter hello name of that custom image.</span></span> <span data-ttu-id="f4bba-167">Názvy vlastních obrázků se nezobrazí v seznamu, abyste získali, že tooensure, který hello název zadán správně.</span><span class="sxs-lookup"><span data-stu-id="f4bba-167">Custom image names are not shown in a list so you have tooensure that hello name is entered correctly.</span></span>    
    
     <span data-ttu-id="f4bba-168">V tomto kurzu zadejte **U** toobring seznam Image Ubuntu a vyberte **Ubuntu Server 14.04 LTS**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-168">For this tutorial, type **U** toobring up a list of Ubuntu images and select **Ubuntu Server 14.04 LTS**.</span></span>
14. <span data-ttu-id="f4bba-169">Pro **spusťte metoda**, vyberte **SSH**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-169">For **Launch method**, select **SSH**.</span></span>
15. <span data-ttu-id="f4bba-170">Zkopírujte níže hello skriptu a vložte hello **Init skriptu** pole.</span><span class="sxs-lookup"><span data-stu-id="f4bba-170">Copy hello script below and paste in hello **Init script** field.</span></span>
    
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
    
     <span data-ttu-id="f4bba-171">Hello **Init skriptu** bude proveden po hello vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f4bba-171">hello **Init script** will be executed after hello VM is created.</span></span> <span data-ttu-id="f4bba-172">V tomto příkladu hello skript nainstaluje ant, Java a git.</span><span class="sxs-lookup"><span data-stu-id="f4bba-172">In this example, hello script installs Java, git, and ant.</span></span>
16. <span data-ttu-id="f4bba-173">V hello **uživatelské jméno** a **heslo** pole, zadejte upřednostňované hodnoty pro účet správce hello, která bude vytvořena na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f4bba-173">In hello **Username** and **Password** fields, enter your preferred values for hello administrator account that will be created on your VM.</span></span>
17. <span data-ttu-id="f4bba-174">Klikněte na **ověřte šablony** toocheck Pokud hello parametry, které jste zadali platné.</span><span class="sxs-lookup"><span data-stu-id="f4bba-174">Click on **Verify Template** toocheck if hello parameters you specified are valid.</span></span>
18. <span data-ttu-id="f4bba-175">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-175">Click on **Save**.</span></span>

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a><span data-ttu-id="f4bba-176">Vytvořit úlohu Hudsonem, který běží na uzlu podřízený v Azure</span><span class="sxs-lookup"><span data-stu-id="f4bba-176">Create a Hudson job that runs on a slave node on Azure</span></span>
<span data-ttu-id="f4bba-177">V této části budete vytváření Hudsonem úlohu, která se spustí na podřízený uzel v Azure.</span><span class="sxs-lookup"><span data-stu-id="f4bba-177">In this section, you'll be creating a Hudson task that will run on a slave node on Azure.</span></span>

1. <span data-ttu-id="f4bba-178">V hello Hudsonem řídicí panel, klikněte na **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-178">In hello Hudson dashboard, click **New Job**.</span></span>
2. <span data-ttu-id="f4bba-179">Zadejte název pro hello úlohu, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="f4bba-179">Enter a name for hello job you are creating.</span></span>
3. <span data-ttu-id="f4bba-180">Hello typ úlohy, vyberte **sestavení úloha softwaru bez stylu**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-180">For hello job type, select **Build a free-style software job**.</span></span>
4. <span data-ttu-id="f4bba-181">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-181">Click **OK**.</span></span>
5. <span data-ttu-id="f4bba-182">Na stránce konfigurace hello úlohy, vyberte **omezit, kde můžete spustit tento projekt**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-182">In hello job configuration page, select **Restrict where this project can be run**.</span></span>
6. <span data-ttu-id="f4bba-183">Vyberte **uzlu a popisek nabídky** a vyberte **linux** (jsme zadali tento popisek, při vytváření šablony virtuálního počítače hello v předchozí části hello).</span><span class="sxs-lookup"><span data-stu-id="f4bba-183">Select **Node and label menu** and select **linux** (we specified this label when creating hello virtual machine template in hello previous section).</span></span>
7. <span data-ttu-id="f4bba-184">V hello **sestavení** klikněte na tlačítko **přidat krok sestavení** a vyberte **spustit prostředí**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-184">In hello **Build** section, click **Add build step** and select **Execute shell**.</span></span>
8. <span data-ttu-id="f4bba-185">Upravit hello následující skript, nahraďte **{název účtu github}**, **{název projektu}**, a **{adresáři projektu}** s příslušným hodnoty a vložit hello Upravit skript v hello textová oblast, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f4bba-185">Edit hello following script, replacing **{your github account name}**, **{your project name}**, and **{your project directory}** with appropriate values, and paste hello edited script in hello text area that appears.</span></span>
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory tooproject
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. <span data-ttu-id="f4bba-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4bba-186">Click on **Save**.</span></span>
10. <span data-ttu-id="f4bba-187">V hello Hudsonem řídicí panel, najděte hello úlohu, kterou jste právě vytvořili a klikněte na hello **naplánovat sestavení** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f4bba-187">In hello Hudson dashboard, find hello job you just created and click on hello **Schedule a build** icon.</span></span>

<span data-ttu-id="f4bba-188">Hudsonem se pak vytvořit pomocí šablony hello vytvořili v předchozí části hello podřízený uzel a spuštění hello skriptu, který jste zadali v kroku hello sestavení pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="f4bba-188">Hudson will then create a slave node using hello template created in hello previous section and execute hello script you specified in hello build step for this task.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4bba-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4bba-189">Next Steps</span></span>
<span data-ttu-id="f4bba-190">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="f4bba-190">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[odběru profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

