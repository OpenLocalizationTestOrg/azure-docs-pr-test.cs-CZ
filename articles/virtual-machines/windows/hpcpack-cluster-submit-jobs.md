---
title: "tooan aaaSubmit úlohy HPC Pack clusteru v Azure | Microsoft Docs"
description: "Zjistěte, jak tooset až místní počítač toosubmit úlohy clusteru HPC Pack tooan v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 2918cf633917d8730487152e6a5ddb863eb8bb5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-tooan-hpc-pack-cluster-deployed-in-azure"></a><span data-ttu-id="2de69-103">Odeslání úlohy HPC z clusteru HPC Pack tooan počítači místní nasazené v Azure</span><span class="sxs-lookup"><span data-stu-id="2de69-103">Submit HPC jobs from an on-premises computer tooan HPC Pack cluster deployed in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="2de69-104">Konfigurace místní klientský počítač toosubmit úlohy tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="2de69-104">Configure an on-premises client computer toosubmit jobs tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure.</span></span> <span data-ttu-id="2de69-105">Tento článek ukazuje, jak tooset místního počítače s klientem nástroje toosubmit úlohy přes HTTPS toohello clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="2de69-105">This article shows you how tooset up a local computer with client tools toosubmit job over HTTPS toohello cluster in Azure.</span></span> <span data-ttu-id="2de69-106">Tímto způsobem můžete několika clusteru uživatelům odeslat úlohy tooa cloudové HPC Pack clusteru, ale bez připojení přímo toohello hlavního uzlu virtuálního počítače nebo přístup k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="2de69-106">In this way, several cluster users can submit jobs tooa cloud-based HPC Pack cluster, but without connecting directly toohello head node VM or accessing an Azure subscription.</span></span>

![Odeslání úlohy clusteru tooa v Azure][jobsubmit]

## <a name="prerequisites"></a><span data-ttu-id="2de69-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2de69-108">Prerequisites</span></span>
* <span data-ttu-id="2de69-109">**Nasadit virtuální počítač Azure hlavního uzlu HPC Pack** -doporučujeme použít automatizované nástroje, jako [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) nebo [skript prostředí Azure PowerShell](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello hlavního uzlu a cluster.</span><span class="sxs-lookup"><span data-stu-id="2de69-109">**HPC Pack head node deployed in an Azure VM** - We recommend that you use automated tools such as an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/) or an [Azure PowerShell script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello head node and cluster.</span></span> <span data-ttu-id="2de69-110">Potřebujete název DNS hello hello hlavního uzlu a hello přihlašovací údaje Správce clusteru dokončit hello kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2de69-110">You need hello DNS name of hello head node and hello credentials of a cluster administrator to complete hello steps in this article.</span></span>
* <span data-ttu-id="2de69-111">**Klientský počítač** -potřebujete klientského počítače Windows nebo Windows Server, který můžete spustit HPC Pack klienta nástroje (viz [požadavky na systém](https://technet.microsoft.com/library/dn535781.aspx)).</span><span class="sxs-lookup"><span data-stu-id="2de69-111">**Client computer** - You need a Windows or Windows Server client computer that can run HPC Pack client utilities (see [system requirements](https://technet.microsoft.com/library/dn535781.aspx)).</span></span> <span data-ttu-id="2de69-112">Pokud chcete pouze toouse hello HPC Pack webový portál nebo REST API toosubmit úloh, můžete použít libovolného klientského počítače podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="2de69-112">If you only want toouse hello HPC Pack web portal or REST API toosubmit jobs, you can use any client computer of your choice.</span></span>
* <span data-ttu-id="2de69-113">**HPC Pack instalačním médiu** -tooinstall hello HPC Pack klienta nástroje, hello volné instalační balíček pro nejnovější verzi sady HPC Pack (HPC Pack 2012 R2) je k dispozici z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="2de69-113">**HPC Pack installation media** - tooinstall hello HPC Pack client utilities, hello free installation package for the latest version of HPC Pack (HPC Pack 2012 R2) is available from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="2de69-114">Ujistěte se, že si stáhnout hello stejnou verzi HPC Pack, který je nainstalován na hello hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2de69-114">Make sure that you download hello same version of HPC Pack that is installed on hello head node VM.</span></span>

## <a name="step-1-install-and-configure-hello-web-components-on-hello-head-node"></a><span data-ttu-id="2de69-115">Krok 1: Instalace a konfigurace webové komponenty hello hello hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="2de69-115">Step 1: Install and configure hello web components on hello head node</span></span>
<span data-ttu-id="2de69-116">tooenable cluster REST rozhraní toosubmit úlohy toohello přes protokol HTTPS, nakonfigurujte hello HPC Pack webové komponenty hlavního uzlu HPC Pack hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-116">tooenable a REST interface toosubmit jobs toohello cluster over HTTPS, ensure that hello HPC Pack web components are configured on hello HPC Pack head node.</span></span> <span data-ttu-id="2de69-117">Pokud již nejsou nainstalovány, nejprve nainstalujte hello webové komponenty spuštěním hello HpcWebComponents.msi instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="2de69-117">If they aren't already installed, first install hello web components by running hello HpcWebComponents.msi installation file.</span></span> <span data-ttu-id="2de69-118">Potom nakonfigurujte hello součásti spuštěním skriptu prostředí HPC PowerShell hello **Set-HPCWebComponents.ps1**.</span><span class="sxs-lookup"><span data-stu-id="2de69-118">Then, configure hello components by running hello HPC PowerShell script **Set-HPCWebComponents.ps1**.</span></span>

<span data-ttu-id="2de69-119">Podrobné postupy najdete v tématu [nainstalovat hello Microsoft HPC Pack webové komponenty](http://technet.microsoft.com/library/hh314627.aspx).</span><span class="sxs-lookup"><span data-stu-id="2de69-119">For detailed procedures, see [Install hello Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="2de69-120">Některé šablony Azure rychlý start pro HPC Pack nainstalujte a nakonfigurujte hello webové komponenty automaticky.</span><span class="sxs-lookup"><span data-stu-id="2de69-120">Certain Azure quickstart templates for HPC Pack install and configure hello web components automatically.</span></span> <span data-ttu-id="2de69-121">Pokud používáte hello [skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello clusteru, můžete volitelně nainstalovat a nakonfigurovat hello webové komponenty jako součást nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-121">If you use hello [HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello cluster, you can optionally install and configure hello web components as part of hello deployment.</span></span>
> 
> 

<span data-ttu-id="2de69-122">**tooinstall hello webové komponenty**</span><span class="sxs-lookup"><span data-stu-id="2de69-122">**tooinstall hello web components**</span></span>

1. <span data-ttu-id="2de69-123">Připojte toohello hlavního uzlu virtuálního počítače pomocí hello přihlašovací údaje Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="2de69-123">Connect toohello head node VM by using hello credentials of a cluster administrator.</span></span>
2. <span data-ttu-id="2de69-124">Ze složky, instalace sady HPC Pack hello spusťte HpcWebComponents.msi hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="2de69-124">From hello HPC Pack Setup folder, run HpcWebComponents.msi on hello head node.</span></span>
3. <span data-ttu-id="2de69-125">Postupujte podle kroků hello v hello Průvodce tooinstall hello webové komponenty</span><span class="sxs-lookup"><span data-stu-id="2de69-125">Follow hello steps in hello wizard tooinstall hello web components</span></span>

<span data-ttu-id="2de69-126">**tooconfigure hello webové komponenty**</span><span class="sxs-lookup"><span data-stu-id="2de69-126">**tooconfigure hello web components**</span></span>

1. <span data-ttu-id="2de69-127">Hello hlavního uzlu spusťte prostředí HPC PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="2de69-127">On hello head node, start HPC PowerShell as an administrator.</span></span>
2. <span data-ttu-id="2de69-128">toochange directory toohello umístění hello konfigurační skript hello zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2de69-128">toochange directory toohello location of hello configuration script, type hello following command:</span></span>
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. <span data-ttu-id="2de69-129">tooconfigure hello rozhraní REST a spusťte hello HPC webové služby, typ hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2de69-129">tooconfigure hello REST interface and start hello HPC Web Service, type hello following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. <span data-ttu-id="2de69-130">Když výzvami tooselect certifikát, zvolte hello certifikát, který odpovídá toohello veřejný název DNS hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="2de69-130">When prompted tooselect a certificate, choose hello certificate that corresponds toohello public DNS name of hello head node.</span></span> <span data-ttu-id="2de69-131">Například, pokud nasazujete hello hlavního uzlu virtuálního počítače pomocí modelu nasazení classic hello, název certifikátu hello vypadá CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="2de69-131">For example, if you deploy hello head node VM using hello classic deployment model, hello certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net.</span></span> <span data-ttu-id="2de69-132">Pokud používáte model nasazení Resource Manager hello, název certifikátu hello vypadá CN =&lt;*HeadNodeDnsName*&gt;.&lt; *oblast*&gt;. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="2de69-132">If you use hello Resource Manager deployment model, hello certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.&lt;*region*&gt;.cloudapp.azure.com.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2de69-133">Můžete vybrat tento certifikát později při odesílání úlohy toohello hlavního uzlu z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="2de69-133">You select this certificate later when you submit jobs toohello head node from an on-premises computer.</span></span> <span data-ttu-id="2de69-134">Nevyberete ani nakonfigurovat certifikát, který odpovídá názvu počítače toohello hello hlavního uzlu v doméně služby Active Directory hello (například CN =*MyHPCHeadNode.HpcAzure.local*).</span><span class="sxs-lookup"><span data-stu-id="2de69-134">Don't select or configure a certificate that corresponds toohello computer name of hello head node in hello Active Directory domain (for example, CN=*MyHPCHeadNode.HpcAzure.local*).</span></span>
   > 
   > 
5. <span data-ttu-id="2de69-135">tooconfigure hello webový portál pro úlohu odeslání, hello zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2de69-135">tooconfigure hello web portal for job submission, type hello following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. <span data-ttu-id="2de69-136">Po dokončení skriptu hello, zastavte a restartujte hello služba Plánovač úloh HPC zadáním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2de69-136">After hello script completes, stop and restart hello HPC Job Scheduler Service by typing hello following commands:</span></span>
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-hello-hpc-pack-client-utilities-on-an-on-premises-computer"></a><span data-ttu-id="2de69-137">Krok 2: Instalace na místním počítači hello HPC Pack klienta nástroje</span><span class="sxs-lookup"><span data-stu-id="2de69-137">Step 2: Install hello HPC Pack client utilities on an on-premises computer</span></span>
<span data-ttu-id="2de69-138">Pokud chcete tooinstall hello HPC Pack klienta nástroje ve vašem počítači, stáhnout instalační soubory HPC Pack (úplná instalace) z hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="2de69-138">If you want tooinstall hello HPC Pack client utilities on your computer, download the HPC Pack setup files (full installation) from hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="2de69-139">Když zahájíte instalaci hello, zvolte možnost instalace hello pro hello **HPC Pack klienta nástroje**.</span><span class="sxs-lookup"><span data-stu-id="2de69-139">When you begin hello installation, choose hello setup option for hello **HPC Pack client utilities**.</span></span>

<span data-ttu-id="2de69-140">nástroje toouse hello HPC Pack klienta toosubmit úlohy toohello hlavního uzlu virtuálního počítače, můžete také potřebovat tooexport certifikát z hlavního uzlu hello a nainstalovat na klientský počítač hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-140">toouse hello HPC Pack client tools toosubmit jobs toohello head node VM, you also need tooexport a certificate from hello head node and install it on hello client computer.</span></span> <span data-ttu-id="2de69-141">Hello certifikát musí být v. Formátu CER.</span><span class="sxs-lookup"><span data-stu-id="2de69-141">hello certificate must be in .CER format.</span></span>

<span data-ttu-id="2de69-142">**certifikát hello tooexport z hlavního uzlu hello**</span><span class="sxs-lookup"><span data-stu-id="2de69-142">**tooexport hello certificate from hello head node**</span></span>

1. <span data-ttu-id="2de69-143">Hello hlavního uzlu přidejte hello certifikátů modulu snap-in tooa konzoly Microsoft Management Console pro hello účet místního počítače.</span><span class="sxs-lookup"><span data-stu-id="2de69-143">On hello head node, add hello Certificates snap-in tooa Microsoft Management Console for hello Local Computer account.</span></span> <span data-ttu-id="2de69-144">Postup modul snap-in hello tooadd najdete v tématu [přidat hello modul Snap-in Certifikáty tooan konzoly MMC](https://technet.microsoft.com/library/cc754431.aspx).</span><span class="sxs-lookup"><span data-stu-id="2de69-144">For steps tooadd hello snap-in, see [Add hello Certificates Snap-in tooan MMC](https://technet.microsoft.com/library/cc754431.aspx).</span></span>
2. <span data-ttu-id="2de69-145">Rozbalte ve stromu konzoly hello **certifikáty – místní** > **osobní**a potom klikněte na **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="2de69-145">In hello console tree, expand **Certificates – Local Computer** > **Personal**, and then click **Certificates**.</span></span>
3. <span data-ttu-id="2de69-146">Vyhledejte hello certifikát, který jste nakonfigurovali pro hello HPC Pack webové komponenty v [krok 1: instalace a konfigurace webové komponenty hello hlavního uzlu hello](#step-1:-install-and-configure-the-web-components-on-the-head-node) (například CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="2de69-146">Locate hello certificate that you configured for hello HPC Pack web components in [Step 1: Install and configure hello web components on hello head node](#step-1:-install-and-configure-the-web-components-on-the-head-node) (for example, CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net).</span></span>
4. <span data-ttu-id="2de69-147">Klikněte pravým tlačítkem na certifikát hello a klikněte na tlačítko **všechny úlohy** > **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="2de69-147">Right-click hello certificate, and click **All Tasks** > **Export**.</span></span>
5. <span data-ttu-id="2de69-148">V Průvodci exportem certifikátu hello, klikněte na **Další**a ujistěte se, že **Ne, neexportovat privátní klíč hello** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="2de69-148">In hello Certificate Export Wizard, click **Next**, and ensure that **No, do not export hello private key** is selected.</span></span>
6. <span data-ttu-id="2de69-149">Binární X.509, kódování hello postupujte podle zbývajících kroků hello Průvodce tooexport hello certifikátu v kódování DER (. Formátu CER).</span><span class="sxs-lookup"><span data-stu-id="2de69-149">Follow hello remaining steps of hello wizard tooexport hello certificate in DER encoded binary X.509 (.CER) format.</span></span>

<span data-ttu-id="2de69-150">**tooimport hello certifikát na klientském počítači hello**</span><span class="sxs-lookup"><span data-stu-id="2de69-150">**tooimport hello certificate on hello client computer**</span></span>

1. <span data-ttu-id="2de69-151">Zkopírujte hello certifikát, který jste exportovali ze složky tooa hello hlavního uzlu na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-151">Copy hello certificate that you exported from hello head node tooa folder on hello client computer.</span></span>
2. <span data-ttu-id="2de69-152">Na klientském počítači hello spusťte certmgr.msc.</span><span class="sxs-lookup"><span data-stu-id="2de69-152">On hello client computer, run certmgr.msc.</span></span>
3. <span data-ttu-id="2de69-153">Ve Správci certifikátů rozbalte **certifikáty – aktuální uživatel** > **důvěryhodné kořenové certifikační autority**, klikněte pravým tlačítkem na **certifikáty**a potom klikněte na **všechny úlohy** > **Import**.</span><span class="sxs-lookup"><span data-stu-id="2de69-153">In Certificate Manager, expand **Certificates – Current user** > **Trusted Root Certification Authorities**, right-click **Certificates**, and then click **All Tasks** > **Import**.</span></span>
4. <span data-ttu-id="2de69-154">V hello Průvodce importem certifikátu, klikněte na **Další** a postupujte podle hello kroky tooimport hello certifikát, který jste exportovali z toohello hello hlavního uzlu úložiště důvěryhodných kořenových certifikačních autorit.</span><span class="sxs-lookup"><span data-stu-id="2de69-154">In hello Certificate Import Wizard, click **Next** and follow hello steps tooimport hello certificate that you exported from hello head node toohello Trusted Root Certification Authorities store.</span></span>

> [!TIP]
> <span data-ttu-id="2de69-155">Může se zobrazit upozornění zabezpečení, protože hello certifikační autority hlavního uzlu hello nerozpoznají hello klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="2de69-155">You might see a security warning, because hello certification authority on hello head node isn't recognized by hello client computer.</span></span> <span data-ttu-id="2de69-156">Pro účely testování můžete ignorovat, toto upozornění a dokončení importu certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-156">For testing purposes, you can ignore this warning and complete hello certificate import.</span></span>
> 
> 

## <a name="step-3-run-test-jobs-on-hello-cluster"></a><span data-ttu-id="2de69-157">Krok 3: Spuštění testu úloh na clusteru hello</span><span class="sxs-lookup"><span data-stu-id="2de69-157">Step 3: Run test jobs on hello cluster</span></span>
<span data-ttu-id="2de69-158">tooverify konfiguraci, zkuste spuštěné úlohy na hello cluster v Azure z hello místní počítač.</span><span class="sxs-lookup"><span data-stu-id="2de69-158">tooverify your configuration, try running jobs on hello cluster in Azure from hello on-premises computer.</span></span> <span data-ttu-id="2de69-159">Můžete například použít HPC Pack grafického uživatelského rozhraní nástroje nebo příkazy příkazového řádku toosubmit úlohy toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="2de69-159">For example, you can use HPC Pack GUI tools or command-line commands toosubmit jobs toohello cluster.</span></span> <span data-ttu-id="2de69-160">Můžete taky založené na webu portálu toosubmit úlohy.</span><span class="sxs-lookup"><span data-stu-id="2de69-160">You can also use a web-based portal toosubmit jobs.</span></span>

<span data-ttu-id="2de69-161">**toorun úlohy odeslání příkazy na klientském počítači hello**</span><span class="sxs-lookup"><span data-stu-id="2de69-161">**toorun job submission commands on hello client computer**</span></span>

1. <span data-ttu-id="2de69-162">Na klientském počítači, kde jsou nainstalovány nástroje klienta HPC Pack hello spusťte příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="2de69-162">On a client computer where hello HPC Pack client utilities are installed, start a Command Prompt.</span></span>
2. <span data-ttu-id="2de69-163">Zadejte příkaz Ukázka.</span><span class="sxs-lookup"><span data-stu-id="2de69-163">Type a sample command.</span></span> <span data-ttu-id="2de69-164">Například toolist všechny úlohy v hello clusteru zadejte podobné tooone příkaz Dobrý den, v závislosti na úplný název DNS hello hello hlavního uzlu následující:</span><span class="sxs-lookup"><span data-stu-id="2de69-164">For example, toolist all jobs on hello cluster, type a command similar tooone of hello following, depending on hello full DNS name of hello head node:</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    <span data-ttu-id="2de69-165">nebo</span><span class="sxs-lookup"><span data-stu-id="2de69-165">or</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > <span data-ttu-id="2de69-166">Úplný název DNS hello hello hlavního uzlu, ne hello IP adresy, použijte v adrese URL hello plánovače.</span><span class="sxs-lookup"><span data-stu-id="2de69-166">Use hello full DNS name of hello head node, not hello IP address, in hello scheduler URL.</span></span> <span data-ttu-id="2de69-167">Pokud zadáte hello IP adresu, objeví se chyba podobné příliš "hello certifikát serveru musí tooeither mít platný řetěz důvěryhodnosti nebo umístit do úložiště důvěryhodných kořenových hello toobe."</span><span class="sxs-lookup"><span data-stu-id="2de69-167">If you specify hello IP address, an error appears similar too"hello server certificate needs tooeither have a valid chain of trust or toobe placed in hello trusted root store."</span></span>
   > 
   > 
3. <span data-ttu-id="2de69-168">Po zobrazení výzvy zadejte uživatelské jméno hello (v podobě hello &lt;DomainName&gt;\\&lt;uživatelské jméno&gt;) a heslo správce clusteru HPC hello nebo jiný uživatel clusteru, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="2de69-168">When prompted, type hello user name (in hello form &lt;DomainName&gt;\\&lt;UserName&gt;) and password of hello HPC cluster administrator or another cluster user that you configured.</span></span> <span data-ttu-id="2de69-169">Můžete toostore hello pověření místně pro další operace úlohy.</span><span class="sxs-lookup"><span data-stu-id="2de69-169">You can choose toostore hello credentials locally for more job operations.</span></span>
   
    <span data-ttu-id="2de69-170">Zobrazí se seznam úloh.</span><span class="sxs-lookup"><span data-stu-id="2de69-170">A list of jobs appears.</span></span>

<span data-ttu-id="2de69-171">**na klientském počítači hello toouse Správce úloh HPC**</span><span class="sxs-lookup"><span data-stu-id="2de69-171">**toouse HPC Job Manager on hello client computer**</span></span>

1. <span data-ttu-id="2de69-172">Pokud při odesílání úlohy nebyla dříve ukládat přihlašovací údaje domény pro uživatele clusteru, můžete přidat přihlašovací údaje hello do správce přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="2de69-172">If you didn't previously store domain credentials for a cluster user when submitting a job, you can add hello credentials in Credential Manager.</span></span>
   
    <span data-ttu-id="2de69-173">a.</span><span class="sxs-lookup"><span data-stu-id="2de69-173">a.</span></span> <span data-ttu-id="2de69-174">V Ovládacích panelech na hello klientského počítače spusťte Správce přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="2de69-174">In Control Panel on hello client computer, start Credential Manager.</span></span>
   
    <span data-ttu-id="2de69-175">b.</span><span class="sxs-lookup"><span data-stu-id="2de69-175">b.</span></span> <span data-ttu-id="2de69-176">Klikněte na tlačítko **pověření systému Windows** > **přidat obecné přihlašovací údaje**.</span><span class="sxs-lookup"><span data-stu-id="2de69-176">Click **Windows Credentials** > **Add a generic credential**.</span></span>
   
    <span data-ttu-id="2de69-177">c.</span><span class="sxs-lookup"><span data-stu-id="2de69-177">c.</span></span> <span data-ttu-id="2de69-178">Zadejte hello internetovou adresu (například https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler nebo https://&lt;HeadNodeDnsName&gt;.&lt; oblast&gt;.cloudapp.azure.com/HpcScheduler) a hello uživatelské jméno (&lt;DomainName&gt;\\&lt;uživatelské jméno&gt;) a heslo správce clusteru hello nebo jiné uživatel clusteru, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="2de69-178">Specify hello Internet address (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com/HpcScheduler), and hello user name (&lt;DomainName&gt;\\&lt;UserName&gt;) and password of hello cluster administrator or another cluster user that you configured.</span></span>
2. <span data-ttu-id="2de69-179">Na klientském počítači hello spusťte Správce úloh HPC.</span><span class="sxs-lookup"><span data-stu-id="2de69-179">On hello client computer, start HPC Job Manager.</span></span>
3. <span data-ttu-id="2de69-180">V hello **vyberte hlavní uzel** dialogové okno, typ hello URL toohello hlavního uzlu v Azure (například https://&lt;HeadNodeDnsName&gt;. cloudapp.net nebo https://&lt;HeadNodeDnsName&gt;. &lt;oblast&gt;. cloudapp.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2de69-180">In hello **Select Head Node** dialog box, type hello URL toohello head node in Azure (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com).</span></span>
   
    <span data-ttu-id="2de69-181">Správce úloh HPC otevře a zobrazí se seznam úloh hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="2de69-181">HPC Job Manager opens and shows a list of jobs on hello head node.</span></span>

<span data-ttu-id="2de69-182">**toouse hello webový portál s hello hlavního uzlu**</span><span class="sxs-lookup"><span data-stu-id="2de69-182">**toouse hello web portal running on hello head node**</span></span>

1. <span data-ttu-id="2de69-183">Spustit webový prohlížeč na klientském počítači hello a zadáním jednoho z následujících adresy, v závislosti na úplný název DNS hello hlavního uzlu hello hello:</span><span class="sxs-lookup"><span data-stu-id="2de69-183">Start a web browser on hello client computer, and enter one of hello following addresses, depending on hello full DNS name of hello head node:</span></span>
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    <span data-ttu-id="2de69-184">nebo</span><span class="sxs-lookup"><span data-stu-id="2de69-184">or</span></span>
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. <span data-ttu-id="2de69-185">Hello zabezpečení zobrazeném dialogu zadejte přihlašovací údaje domény hello Správce clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-185">In hello security dialog box that appears, type hello domain credentials of hello HPC cluster administrator.</span></span> <span data-ttu-id="2de69-186">(Můžete také přidat další clusteru uživatele v různých rolích.</span><span class="sxs-lookup"><span data-stu-id="2de69-186">(You can also add other cluster users in different roles.</span></span> <span data-ttu-id="2de69-187">V tématu [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335.aspx).)</span><span class="sxs-lookup"><span data-stu-id="2de69-187">See [Managing Cluster Users](https://technet.microsoft.com/library/ff919335.aspx).)</span></span>
   
    <span data-ttu-id="2de69-188">Hello webový portál otevře toohello zobrazení seznamu úloh.</span><span class="sxs-lookup"><span data-stu-id="2de69-188">hello web portal opens toohello job list view.</span></span>
3. <span data-ttu-id="2de69-189">Klikněte na tlačítko toosubmit ukázka úlohu, která vrací řetězec hello "Hello, World" z clusteru hello **nová úloha** v levém navigačním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-189">toosubmit a sample job that returns hello string “Hello World” from hello cluster, click **New job** in hello left-hand navigation.</span></span>
4. <span data-ttu-id="2de69-190">Na hello **nová úloha** v části **ze stránek odeslání**, klikněte na tlačítko **HelloWorld**.</span><span class="sxs-lookup"><span data-stu-id="2de69-190">On hello **New Job** page, under **From submission pages**, click **HelloWorld**.</span></span> <span data-ttu-id="2de69-191">Zobrazí se stránka odeslání úlohy Hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-191">hello job submission page appears.</span></span>
5. <span data-ttu-id="2de69-192">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="2de69-192">Click **Submit**.</span></span> <span data-ttu-id="2de69-193">Pokud se zobrazí výzva, zadejte přihlašovací údaje domény hello Správce clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="2de69-193">If prompted, provide hello domain credentials of hello HPC cluster administrator.</span></span> <span data-ttu-id="2de69-194">Hello úloha odeslána a hello ID úlohy se zobrazí na hello **Mé úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="2de69-194">hello job is submitted, and hello job ID appears on hello **My Jobs** page.</span></span>
6. <span data-ttu-id="2de69-195">výsledky hello tooview hello úlohy, které jste odeslali, klikněte na tlačítko hello ID úlohy a pak klikněte na **úlohy v zobrazení** výstupu příkazu hello tooview (v části **výstup**).</span><span class="sxs-lookup"><span data-stu-id="2de69-195">tooview hello results of hello job that you submitted, click hello job ID, and then click **View Tasks** tooview hello command output (under **Output**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2de69-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2de69-196">Next steps</span></span>
* <span data-ttu-id="2de69-197">Můžete také odeslat úlohy toohello clusteru Azure s hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="2de69-197">You can also submit jobs toohello Azure cluster with hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span>
* <span data-ttu-id="2de69-198">Pokud chcete toosubmit clusteru úlohy z klienta systému Linux, najdete v části hello Python ukázku v hello [HPC Pack 2012 R2 SDK a ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="2de69-198">If you want toosubmit cluster jobs from a Linux client, see hello Python sample in hello [HPC Pack 2012 R2 SDK and Sample Code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span>

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
