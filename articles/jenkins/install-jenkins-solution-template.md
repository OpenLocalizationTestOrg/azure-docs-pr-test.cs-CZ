---
title: "aaaCreate volaných serveru v Azure"
description: "Nainstalujte volaných na virtuálním počítači Azure Linux z hello volaných řešení šablony a vytvoření ukázkové aplikace Java."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 82ab2ac52594acba131414b449b608978591d4b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-hello-azure-portal"></a><span data-ttu-id="96df1-103">Vytvoření serveru volaných ve virtuálním počítači Azure Linux z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="96df1-103">Create a Jenkins server on an Azure Linux VM from hello Azure portal</span></span>

<span data-ttu-id="96df1-104">Tento rychlý start ukazuje, jak tooinstall [volaných](https://jenkins.io) na Linux virtuálního počítače s Ubuntu s hello nástrojů a modulů plug-in nakonfigurovat toowork s Azure.</span><span class="sxs-lookup"><span data-stu-id="96df1-104">This quickstart shows how tooinstall [Jenkins](https://jenkins.io) on an Ubuntu Linux VM with hello tools and plug-ins configured toowork with Azure.</span></span> <span data-ttu-id="96df1-105">Až budete hotovi, budete mít v Azure spuštěný server Jenkins sestavující ukázkovou aplikaci v Javě z [GitHubu](https://github.com).</span><span class="sxs-lookup"><span data-stu-id="96df1-105">When you're finished, you have a Jenkins server running in Azure building a sample Java app from [GitHub](https://github.com).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96df1-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96df1-106">Prerequisites</span></span>

* <span data-ttu-id="96df1-107">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="96df1-107">An Azure subscription</span></span>
* <span data-ttu-id="96df1-108">TooSSH přístup na příkazovém řádku počítače (například hello Bash prostředí nebo [PuTTY](http://www.putty.org/))</span><span class="sxs-lookup"><span data-stu-id="96df1-108">Access tooSSH on your computer's command line (such as hello Bash shell or [PuTTY](http://www.putty.org/))</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-jenkins-vm-from-hello-solution-template"></a><span data-ttu-id="96df1-109">Vytvoření hello volaných virtuálního počítače ze šablony řešení hello</span><span class="sxs-lookup"><span data-stu-id="96df1-109">Create hello Jenkins VM from hello solution template</span></span>

<span data-ttu-id="96df1-110">Otevřete hello [marketplace bitovou kopii pro volaných](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) ve webovém prohlížeči a vyberte **získat IT teď** z levé straně stránky hello hello.</span><span class="sxs-lookup"><span data-stu-id="96df1-110">Open hello [marketplace image for Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) in your web browser and select  **GET IT NOW** from hello left-hand side of hello page.</span></span> <span data-ttu-id="96df1-111">Zkontrolujte hello ceny podrobnosti a vyberte **pokračovat**, pak vyberte **vytvořit** tooconfigure hello volaných server hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="96df1-111">Review hello pricing details and select **Continue**, then select **Create** tooconfigure hello Jenkins server in hello Azure portal.</span></span> 
   
![Dialogové okno na webu Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

<span data-ttu-id="96df1-113">V hello **nakonfigurovali základní nastavení** kartě, vyplňte následující pole hello:</span><span class="sxs-lookup"><span data-stu-id="96df1-113">In hello **Configure basic settings** tab, fill in hello following fields:</span></span>

![Konfigurace základního nastavení](./media/install-jenkins-solution-template/ap-basic.png)

* <span data-ttu-id="96df1-115">Jako **Název** použijte **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="96df1-115">Use **Jenkins** for **Name**.</span></span>
* <span data-ttu-id="96df1-116">Zadejte **Uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="96df1-116">Enter a **User name**.</span></span> <span data-ttu-id="96df1-117">Hello uživatelské jméno musí splňovat [specifické požadavky](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="96df1-117">hello user name must meet [specific requirements](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span></span>
* <span data-ttu-id="96df1-118">Vyberte **heslo** jako hello **typ ověřování** a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="96df1-118">Select **Password** as hello **Authentication type** and enter a password.</span></span> <span data-ttu-id="96df1-119">Hello heslo musí mít velké písmeno, číslo a jeden speciální znak.</span><span class="sxs-lookup"><span data-stu-id="96df1-119">hello password must have an upper case character, a number, and one special character.</span></span>
* <span data-ttu-id="96df1-120">Použití **myJenkinsResourceGroup** pro hello **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="96df1-120">Use **myJenkinsResourceGroup** for hello **Resource Group**.</span></span>
* <span data-ttu-id="96df1-121">Zvolte hello **východní USA** [oblast Azure](https://azure.microsoft.com/regions/) z hello **umístění** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="96df1-121">Choose hello **East US** [Azure region](https://azure.microsoft.com/regions/) from hello **Location** drop-down.</span></span>

<span data-ttu-id="96df1-122">Vyberte **OK** tooproceed toohello **konfigurace dalších možností** kartě. Zadejte jedinečný doménový název tooidentify hello volaných server a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="96df1-122">Select **OK** tooproceed toohello **Configure additional options** tab. Enter a unique domain name tooidentify hello Jenkins server and select **OK**.</span></span>

![Nastavení dalších možností](./media/install-jenkins-solution-template/ap-addtional.png)  

 <span data-ttu-id="96df1-124">Po ověření neselže, vybrat **OK** znovu z hello **Souhrn** kartě. Nakonec vyberte **nákupu** toocreate hello volaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="96df1-124">Once validation passes, select **OK** again from hello **Summary** tab. Finally, select **Purchase** toocreate hello Jenkins VM.</span></span> <span data-ttu-id="96df1-125">Když se váš server je připraven, zobrazí oznámení v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="96df1-125">When your server is ready, you get a notification in hello Azure portal:</span></span>   

![Oznámení o připravenosti Jenkinse](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-toojenkins"></a><span data-ttu-id="96df1-127">Připojit tooJenkins</span><span class="sxs-lookup"><span data-stu-id="96df1-127">Connect tooJenkins</span></span>

<span data-ttu-id="96df1-128">Tooyour virtuálního počítače (například http://jenkins2517454.eastus.cloudapp.azure.com/) přejděte v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="96df1-128">Navigate tooyour virtual machine (for example, http://jenkins2517454.eastus.cloudapp.azure.com/) in  your web browser.</span></span> <span data-ttu-id="96df1-129">Hello volaných konzoly je nedostupné přes nezabezpečený HTTP, takže pokyny jsou k dispozici na konzole volaných hello stránky tooaccess hello bezpečně z počítače pomocí tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="96df1-129">hello Jenkins console is inaccessible through unsecured HTTP so instructions are provided on hello page tooaccess hello Jenkins console securely from your computer using an SSH tunnel.</span></span>

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

<span data-ttu-id="96df1-131">Nastavit hello tunelové propojení prostřednictvím hello `ssh` příkaz na stránce hello z příkazového řádku hello, nahraďte `username` s názvem hello předtím vybrali při nastavování hello virtuálního počítače ze šablony řešení hello uživatel s oprávněními správce hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="96df1-131">Set up hello tunnel using hello `ssh` command on hello page from hello command line, replacing `username` with hello name of hello virtual machine admin user chosen earlier when setting up hello virtual machine from hello solution template.</span></span>

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

<span data-ttu-id="96df1-132">Po spuštění hello tunel přejděte toohttp://localhost:8080 / na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="96df1-132">After you have started hello tunnel, navigate toohttp://localhost:8080/ on your local machine.</span></span> 

<span data-ttu-id="96df1-133">Získáte hello počátečního hesla tak, že spustíte následující příkaz na příkazovém řádku hello během připojení prostřednictvím SSH toohello volaných VM hello.</span><span class="sxs-lookup"><span data-stu-id="96df1-133">Get hello initial password by running hello following command in hello command line while connected through SSH toohello Jenkins VM.</span></span>

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

<span data-ttu-id="96df1-134">Odemkněte hello volaných řídicím panelu hello první přihlášení pomocí toto počáteční heslo.</span><span class="sxs-lookup"><span data-stu-id="96df1-134">Unlock hello Jenkins dashboard for hello first time using this initial password.</span></span>

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-unlock.png)

<span data-ttu-id="96df1-136">Vyberte **nainstalovat navrhované modulů plug-in** na hello další stránky a pak vytvořit řídicí panel volaných správce uživatele použít tooaccess hello volaných.</span><span class="sxs-lookup"><span data-stu-id="96df1-136">Select **Install suggested plugins** on hello next page and then create a Jenkins admin user used tooaccess hello Jenkins dashboard.</span></span>

![Jenkins je připraven!](./media/install-jenkins-solution-template/jenkins-welcome.png)

<span data-ttu-id="96df1-138">Hello volaných server je nyní připraven toobuild kódu.</span><span class="sxs-lookup"><span data-stu-id="96df1-138">hello Jenkins server is now ready toobuild code.</span></span>

## <a name="create-your-first-job"></a><span data-ttu-id="96df1-139">Vytvoření první úlohy</span><span class="sxs-lookup"><span data-stu-id="96df1-139">Create your first job</span></span>

<span data-ttu-id="96df1-140">Vyberte **vytvoření nové úlohy** hello volaných konzoly, pojmenujte ji **mySampleApp** a vyberte **volný styl projektu**, pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="96df1-140">Select **Create new jobs** from hello Jenkins console, then name it **mySampleApp** and select **Freestyle project**, then select **OK**.</span></span>

![Vytvoření nové úlohy](./media/install-jenkins-solution-template/jenkins-new-job.png) 

<span data-ttu-id="96df1-142">Vyberte hello **správu zdrojového kódu** kartě, povolte **Git**a zadejte následující adresu URL v hello **adresu URL úložiště** pole:`https://github.com/spring-guides/gs-spring-boot.git`</span><span class="sxs-lookup"><span data-stu-id="96df1-142">Select hello **Source Code Management** tab, enable **Git**, and enter hello following URL in **Repository URL**  field: `https://github.com/spring-guides/gs-spring-boot.git`</span></span>

![Definování hello úložiště Git](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

<span data-ttu-id="96df1-144">Vyberte hello **sestavení** a potom vyberte **přidat krok sestavení**, **Gradle vyvolání skriptu**.</span><span class="sxs-lookup"><span data-stu-id="96df1-144">Select hello **Build** tab, then select **Add build step**, **Invoke Gradle script**.</span></span> <span data-ttu-id="96df1-145">Vyberte **Use Gradle Wrapper** (Použít obálku Gradle) a pak zadejte `complete` do pole **Wrapper location** (Umístění obálky) a `build` do pole **Tasks** (Úlohy).</span><span class="sxs-lookup"><span data-stu-id="96df1-145">Select **Use Gradle Wrapper**, then enter `complete` in **Wrapper location** and `build` for **Tasks**.</span></span>

![Použít toobuild obálku Gradle hello](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

<span data-ttu-id="96df1-147">Vyberte **Advanced...** (Upřesnit...)</span><span class="sxs-lookup"><span data-stu-id="96df1-147">Select **Advanced..**</span></span> <span data-ttu-id="96df1-148">a pak zadejte `complete` v hello **kořenové sestavit skript** pole.</span><span class="sxs-lookup"><span data-stu-id="96df1-148">and then enter `complete` in hello **Root Build script** field.</span></span> <span data-ttu-id="96df1-149">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="96df1-149">Select **Save**.</span></span>

![Pokročilé nastavení v kroku sestavení obálku Gradle hello](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-hello-code"></a><span data-ttu-id="96df1-151">Sestavení hello kódu</span><span class="sxs-lookup"><span data-stu-id="96df1-151">Build hello code</span></span>

<span data-ttu-id="96df1-152">Vyberte **sestavení teď** toocompile hello kód a balíček hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96df1-152">Select **Build Now** toocompile hello code and package hello sample app.</span></span> <span data-ttu-id="96df1-153">Pokud vaše sestavení se dokončí, vyberte hello **prostoru** propojení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="96df1-153">When your build completes, select hello **Workspace** link for hello project.</span></span>

![Vyhledejte soubor JAR toohello prostoru tooget hello ze sestavení hello](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

<span data-ttu-id="96df1-155">Přejděte příliš`complete/build/libs` a ujistěte se, hello `gs-spring-boot-0.1.0.jar` je k dispozici tooverify úspěšného buildu.</span><span class="sxs-lookup"><span data-stu-id="96df1-155">Navigate too`complete/build/libs` and ensure hello `gs-spring-boot-0.1.0.jar` is there tooverify that your build was successful.</span></span> <span data-ttu-id="96df1-156">Vaše volaných, které server je nyní připraveni toobuild vašich vlastních projektů v Azure.</span><span class="sxs-lookup"><span data-stu-id="96df1-156">Your Jenkins server is now ready toobuild your own projects in Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96df1-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96df1-157">Next Steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="96df1-158">Přidání virtuálních počítačů Azure jako agentů Jenkinse</span><span class="sxs-lookup"><span data-stu-id="96df1-158">Add Azure VMs as Jenkins agents</span></span>](jenkins-azure-vm-agents.md)
