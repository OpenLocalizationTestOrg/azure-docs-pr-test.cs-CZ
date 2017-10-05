---
title: "Odeslání úlohy HPC Pack clusteru v Azure | Microsoft Docs"
description: "Zjistěte, jak nastavit na místní počítač k odesílání úloh do clusteru HPC Pack v Azure"
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
ms.openlocfilehash: d5953f1e1dd2deb4d871bd67352a6a5b2ae13dbf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a><span data-ttu-id="5bab6-103">Odeslání úloh HPC z místního počítače do clusteru HPC Pack nasazeného v Azure</span><span class="sxs-lookup"><span data-stu-id="5bab6-103">Submit HPC jobs from an on-premises computer to an HPC Pack cluster deployed in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="5bab6-104">Konfigurace klientského počítače k odesílání úloh do k místní [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="5bab6-104">Configure an on-premises client computer to submit jobs to a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure.</span></span> <span data-ttu-id="5bab6-105">Tento článek ukazuje, jak nastavit místního počítače s klientskými nástroji se odeslat úlohu přes HTTPS do clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="5bab6-105">This article shows you how to set up a local computer with client tools to submit job over HTTPS to the cluster in Azure.</span></span> <span data-ttu-id="5bab6-106">Tímto způsobem můžete několika uživatelům clusteru odesílání úloh do clusteru HPC Pack založené na cloudu, ale bez připojení přímo k hlavnímu uzlu virtuálního počítače nebo přístup k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="5bab6-106">In this way, several cluster users can submit jobs to a cloud-based HPC Pack cluster, but without connecting directly to the head node VM or accessing an Azure subscription.</span></span>

![Odeslání úlohy do clusteru s podporou v Azure][jobsubmit]

## <a name="prerequisites"></a><span data-ttu-id="5bab6-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5bab6-108">Prerequisites</span></span>
* <span data-ttu-id="5bab6-109">**Nasadit virtuální počítač Azure hlavního uzlu HPC Pack** -doporučujeme použít automatizované nástroje, jako [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) nebo [skript prostředí Azure PowerShell](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) nasazení hlavního uzlu a clusteru.</span><span class="sxs-lookup"><span data-stu-id="5bab6-109">**HPC Pack head node deployed in an Azure VM** - We recommend that you use automated tools such as an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/) or an [Azure PowerShell script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) to deploy the head node and cluster.</span></span> <span data-ttu-id="5bab6-110">Potřebujete název DNS hlavního uzlu a přihlašovací údaje Správce clusteru k dokončení kroků v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5bab6-110">You need the DNS name of the head node and the credentials of a cluster administrator to complete the steps in this article.</span></span>
* <span data-ttu-id="5bab6-111">**Klientský počítač** -potřebujete klientského počítače Windows nebo Windows Server, který můžete spustit HPC Pack klienta nástroje (viz [požadavky na systém](https://technet.microsoft.com/library/dn535781.aspx)).</span><span class="sxs-lookup"><span data-stu-id="5bab6-111">**Client computer** - You need a Windows or Windows Server client computer that can run HPC Pack client utilities (see [system requirements](https://technet.microsoft.com/library/dn535781.aspx)).</span></span> <span data-ttu-id="5bab6-112">Pokud chcete používat k odesílání úloh HPC Pack webový portál nebo REST API, můžete použít libovolného klientského počítače podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="5bab6-112">If you only want to use the HPC Pack web portal or REST API to submit jobs, you can use any client computer of your choice.</span></span>
* <span data-ttu-id="5bab6-113">**HPC Pack instalačním médiu** – k instalaci nástroje klienta HPC Pack volné instalační balíček pro nejnovější verzi sady HPC Pack (HPC Pack 2012 R2) je k dispozici z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="5bab6-113">**HPC Pack installation media** - To install the HPC Pack client utilities, the free installation package for the latest version of HPC Pack (HPC Pack 2012 R2) is available from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="5bab6-114">Ujistěte se, že si stáhnout stejnou verzi nástroje HPC Pack, který je nainstalován na hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5bab6-114">Make sure that you download the same version of HPC Pack that is installed on the head node VM.</span></span>

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a><span data-ttu-id="5bab6-115">Krok 1: Instalace a konfigurace webové komponenty hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="5bab6-115">Step 1: Install and configure the web components on the head node</span></span>
<span data-ttu-id="5bab6-116">Povolit rozhraní REST k odesílání úloh do clusteru pomocí protokolu HTTPS, nakonfigurujte webové komponenty HPC Pack hlavního uzlu HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="5bab6-116">To enable a REST interface to submit jobs to the cluster over HTTPS, ensure that the HPC Pack web components are configured on the HPC Pack head node.</span></span> <span data-ttu-id="5bab6-117">Pokud již nejsou nainstalovány, nejprve nainstalujte webové komponenty spuštěním HpcWebComponents.msi instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="5bab6-117">If they aren't already installed, first install the web components by running the HpcWebComponents.msi installation file.</span></span> <span data-ttu-id="5bab6-118">Potom nakonfigurujte komponenty spuštěním skriptu prostředí HPC PowerShell **Set-HPCWebComponents.ps1**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-118">Then, configure the components by running the HPC PowerShell script **Set-HPCWebComponents.ps1**.</span></span>

<span data-ttu-id="5bab6-119">Podrobné postupy najdete v tématu [nainstalovat webové komponenty Microsoft HPC Pack](http://technet.microsoft.com/library/hh314627.aspx).</span><span class="sxs-lookup"><span data-stu-id="5bab6-119">For detailed procedures, see [Install the Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="5bab6-120">Některé šablony Azure rychlý start pro HPC Pack instalace a konfigurace webové komponenty automaticky.</span><span class="sxs-lookup"><span data-stu-id="5bab6-120">Certain Azure quickstart templates for HPC Pack install and configure the web components automatically.</span></span> <span data-ttu-id="5bab6-121">Pokud použijete [skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) k vytvoření clusteru, můžete volitelně nainstalovat a nakonfigurovat webové komponenty jako součást nasazení.</span><span class="sxs-lookup"><span data-stu-id="5bab6-121">If you use the [HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) to create the cluster, you can optionally install and configure the web components as part of the deployment.</span></span>
> 
> 

<span data-ttu-id="5bab6-122">**Chcete-li nainstalovat webové komponenty**</span><span class="sxs-lookup"><span data-stu-id="5bab6-122">**To install the web components**</span></span>

1. <span data-ttu-id="5bab6-123">Připojte k hlavnímu uzlu virtuálního počítače s použitím pověření správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="5bab6-123">Connect to the head node VM by using the credentials of a cluster administrator.</span></span>
2. <span data-ttu-id="5bab6-124">Ve složce instalace sady HPC Pack spusťte z hlavního uzlu HpcWebComponents.msi.</span><span class="sxs-lookup"><span data-stu-id="5bab6-124">From the HPC Pack Setup folder, run HpcWebComponents.msi on the head node.</span></span>
3. <span data-ttu-id="5bab6-125">Postupujte podle kroků v průvodci k instalaci webové komponenty</span><span class="sxs-lookup"><span data-stu-id="5bab6-125">Follow the steps in the wizard to install the web components</span></span>

<span data-ttu-id="5bab6-126">**Konfigurace webové komponenty**</span><span class="sxs-lookup"><span data-stu-id="5bab6-126">**To configure the web components**</span></span>

1. <span data-ttu-id="5bab6-127">Z hlavního uzlu spusťte prostředí HPC PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="5bab6-127">On the head node, start HPC PowerShell as an administrator.</span></span>
2. <span data-ttu-id="5bab6-128">Chcete-li změnit adresář, do umístění souboru konfigurační skript, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5bab6-128">To change directory to the location of the configuration script, type the following command:</span></span>
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. <span data-ttu-id="5bab6-129">Nakonfigurujte rozhraní REST a spuštění služby webové prostředí HPC, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5bab6-129">To configure the REST interface and start the HPC Web Service, type the following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. <span data-ttu-id="5bab6-130">Po zobrazení výzvy k výběru certifikátu, vyberte certifikát, který odpovídá názvu DNS veřejné hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5bab6-130">When prompted to select a certificate, choose the certificate that corresponds to the public DNS name of the head node.</span></span> <span data-ttu-id="5bab6-131">Například pokud nasadíte hlavního uzlu virtuálního počítače pomocí modelu nasazení classic, název certifikátu vypadá CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="5bab6-131">For example, if you deploy the head node VM using the classic deployment model, the certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net.</span></span> <span data-ttu-id="5bab6-132">Pokud používáte model nasazení Resource Manager, název certifikátu vypadá CN =&lt;*HeadNodeDnsName*&gt;.&lt; *oblast*&gt;. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="5bab6-132">If you use the Resource Manager deployment model, the certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.&lt;*region*&gt;.cloudapp.azure.com.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5bab6-133">Můžete vybrat tento certifikát později při odesílání úlohy k hlavnímu uzlu z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="5bab6-133">You select this certificate later when you submit jobs to the head node from an on-premises computer.</span></span> <span data-ttu-id="5bab6-134">Nevyberete ani nakonfigurovat certifikát, který odpovídá názvu počítače hlavního uzlu v doméně služby Active Directory (například CN =*MyHPCHeadNode.HpcAzure.local*).</span><span class="sxs-lookup"><span data-stu-id="5bab6-134">Don't select or configure a certificate that corresponds to the computer name of the head node in the Active Directory domain (for example, CN=*MyHPCHeadNode.HpcAzure.local*).</span></span>
   > 
   > 
5. <span data-ttu-id="5bab6-135">Pokud chcete nakonfigurovat na webový portál pro úlohy, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5bab6-135">To configure the web portal for job submission, type the following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. <span data-ttu-id="5bab6-136">Po dokončení skriptu, zastavte a restartujte služby plánovače úloh HPC zadáním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="5bab6-136">After the script completes, stop and restart the HPC Job Scheduler Service by typing the following commands:</span></span>
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a><span data-ttu-id="5bab6-137">Krok 2: Instalace klienta nástroje HPC Pack na místním počítači</span><span class="sxs-lookup"><span data-stu-id="5bab6-137">Step 2: Install the HPC Pack client utilities on an on-premises computer</span></span>
<span data-ttu-id="5bab6-138">Pokud chcete nainstalovat klienta nástroje HPC Pack ve vašem počítači, stáhnout instalační soubory HPC Pack (úplná instalace) z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="5bab6-138">If you want to install the HPC Pack client utilities on your computer, download the HPC Pack setup files (full installation) from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="5bab6-139">Abyste před zahájením instalace, zvolte možnost instalační program **HPC Pack klienta nástroje**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-139">When you begin the installation, choose the setup option for the **HPC Pack client utilities**.</span></span>

<span data-ttu-id="5bab6-140">Pomocí nástrojů sady HPC Pack klienta k odesílání úloh do hlavního uzlu virtuálního počítače, musíte také exportovat certifikát z hlavního uzlu a nainstalujte ji na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="5bab6-140">To use the HPC Pack client tools to submit jobs to the head node VM, you also need to export a certificate from the head node and install it on the client computer.</span></span> <span data-ttu-id="5bab6-141">Certifikát musí být v. Formátu CER.</span><span class="sxs-lookup"><span data-stu-id="5bab6-141">The certificate must be in .CER format.</span></span>

<span data-ttu-id="5bab6-142">**Export certifikátu z hlavního uzlu**</span><span class="sxs-lookup"><span data-stu-id="5bab6-142">**To export the certificate from the head node**</span></span>

1. <span data-ttu-id="5bab6-143">Z hlavního uzlu přidáte modul snap-in Certifikáty do konzoly Microsoft Management Console pro účet místního počítače.</span><span class="sxs-lookup"><span data-stu-id="5bab6-143">On the head node, add the Certificates snap-in to a Microsoft Management Console for the Local Computer account.</span></span> <span data-ttu-id="5bab6-144">Postup pro přidání modulu snap-in, najdete v části [přidat modul Snap-in Certifikáty do konzoly MMC](https://technet.microsoft.com/library/cc754431.aspx).</span><span class="sxs-lookup"><span data-stu-id="5bab6-144">For steps to add the snap-in, see [Add the Certificates Snap-in to an MMC](https://technet.microsoft.com/library/cc754431.aspx).</span></span>
2. <span data-ttu-id="5bab6-145">Ve stromu konzoly rozbalte **certifikáty – místní** > **osobní**a potom klikněte na **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-145">In the console tree, expand **Certificates – Local Computer** > **Personal**, and then click **Certificates**.</span></span>
3. <span data-ttu-id="5bab6-146">Vyhledejte certifikát, který jste nakonfigurovali pro komponenty webové HPC Pack [krok 1: instalace a konfigurace webové komponenty hlavního uzlu](#step-1:-install-and-configure-the-web-components-on-the-head-node) (například CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="5bab6-146">Locate the certificate that you configured for the HPC Pack web components in [Step 1: Install and configure the web components on the head node](#step-1:-install-and-configure-the-web-components-on-the-head-node) (for example, CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net).</span></span>
4. <span data-ttu-id="5bab6-147">Pravým tlačítkem na certifikát a klikněte na tlačítko **všechny úlohy** > **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-147">Right-click the certificate, and click **All Tasks** > **Export**.</span></span>
5. <span data-ttu-id="5bab6-148">V Průvodci exportem certifikátu klikněte na **Další**a ujistěte se, že **Ne, neexportovat soukromý klíč** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="5bab6-148">In the Certificate Export Wizard, click **Next**, and ensure that **No, do not export the private key** is selected.</span></span>
6. <span data-ttu-id="5bab6-149">Postupujte podle zbývajících kroků tohoto průvodce můžete exportovat certifikát v binární kódování DER X.509 (. Formátu CER).</span><span class="sxs-lookup"><span data-stu-id="5bab6-149">Follow the remaining steps of the wizard to export the certificate in DER encoded binary X.509 (.CER) format.</span></span>

<span data-ttu-id="5bab6-150">**Při importu certifikátu v klientském počítači**</span><span class="sxs-lookup"><span data-stu-id="5bab6-150">**To import the certificate on the client computer**</span></span>

1. <span data-ttu-id="5bab6-151">Zkopírujte certifikát, který jste exportovali z hlavního uzlu do složky v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="5bab6-151">Copy the certificate that you exported from the head node to a folder on the client computer.</span></span>
2. <span data-ttu-id="5bab6-152">Na klientském počítači spusťte certmgr.msc.</span><span class="sxs-lookup"><span data-stu-id="5bab6-152">On the client computer, run certmgr.msc.</span></span>
3. <span data-ttu-id="5bab6-153">Ve Správci certifikátů rozbalte **certifikáty – aktuální uživatel** > **důvěryhodné kořenové certifikační autority**, klikněte pravým tlačítkem na **certifikáty**a potom klikněte na **všechny úlohy** > **Import**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-153">In Certificate Manager, expand **Certificates – Current user** > **Trusted Root Certification Authorities**, right-click **Certificates**, and then click **All Tasks** > **Import**.</span></span>
4. <span data-ttu-id="5bab6-154">V Průvodci importem certifikátu klikněte na **Další** a postupujte podle kroků pro import certifikátu, který jste exportovali z hlavního uzlu do úložiště důvěryhodných kořenových certifikačních autorit.</span><span class="sxs-lookup"><span data-stu-id="5bab6-154">In the Certificate Import Wizard, click **Next** and follow the steps to import the certificate that you exported from the head node to the Trusted Root Certification Authorities store.</span></span>

> [!TIP]
> <span data-ttu-id="5bab6-155">Může se zobrazit upozornění zabezpečení, protože nebyla rozpoznána certifikační autority z hlavního uzlu v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="5bab6-155">You might see a security warning, because the certification authority on the head node isn't recognized by the client computer.</span></span> <span data-ttu-id="5bab6-156">Pro účely testování můžete toto upozornění ignorovat a import certifikátu dokončit.</span><span class="sxs-lookup"><span data-stu-id="5bab6-156">For testing purposes, you can ignore this warning and complete the certificate import.</span></span>
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a><span data-ttu-id="5bab6-157">Krok 3: Spuštění testovací úlohy v clusteru</span><span class="sxs-lookup"><span data-stu-id="5bab6-157">Step 3: Run test jobs on the cluster</span></span>
<span data-ttu-id="5bab6-158">Pokud chcete ověřit konfiguraci, zkuste spustit úlohy v clusteru v Azure z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="5bab6-158">To verify your configuration, try running jobs on the cluster in Azure from the on-premises computer.</span></span> <span data-ttu-id="5bab6-159">Můžete například použít HPC Pack grafického uživatelského rozhraní nástroje nebo příkazy příkazového řádku k odesílání úloh do clusteru.</span><span class="sxs-lookup"><span data-stu-id="5bab6-159">For example, you can use HPC Pack GUI tools or command-line commands to submit jobs to the cluster.</span></span> <span data-ttu-id="5bab6-160">Webový portál můžete taky použít k odesílání úloh.</span><span class="sxs-lookup"><span data-stu-id="5bab6-160">You can also use a web-based portal to submit jobs.</span></span>

<span data-ttu-id="5bab6-161">**Ke spuštění úlohy odesílání příkazů v klientském počítači**</span><span class="sxs-lookup"><span data-stu-id="5bab6-161">**To run job submission commands on the client computer**</span></span>

1. <span data-ttu-id="5bab6-162">Na klientském počítači, kde jsou nainstalovány nástroje sady HPC Pack klienta spusťte příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="5bab6-162">On a client computer where the HPC Pack client utilities are installed, start a Command Prompt.</span></span>
2. <span data-ttu-id="5bab6-163">Zadejte příkaz Ukázka.</span><span class="sxs-lookup"><span data-stu-id="5bab6-163">Type a sample command.</span></span> <span data-ttu-id="5bab6-164">Například pro zobrazení seznamu všech úloh v clusteru, zadejte příkaz podobný jednu z těchto, v závislosti na úplný název DNS hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="5bab6-164">For example, to list all jobs on the cluster, type a command similar to one of the following, depending on the full DNS name of the head node:</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    <span data-ttu-id="5bab6-165">nebo</span><span class="sxs-lookup"><span data-stu-id="5bab6-165">or</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > <span data-ttu-id="5bab6-166">Úplný název DNS hlavního uzlu, ne IP adresy, použijte v adrese URL plánovače.</span><span class="sxs-lookup"><span data-stu-id="5bab6-166">Use the full DNS name of the head node, not the IP address, in the scheduler URL.</span></span> <span data-ttu-id="5bab6-167">Pokud zadáte IP adresu, chyba se zobrazí podobná "certifikát serveru musí mít buď platný řetěz certifikátů nebo umístit do úložiště důvěryhodných kořenových."</span><span class="sxs-lookup"><span data-stu-id="5bab6-167">If you specify the IP address, an error appears similar to "The server certificate needs to either have a valid chain of trust or to be placed in the trusted root store."</span></span>
   > 
   > 
3. <span data-ttu-id="5bab6-168">Po zobrazení výzvy zadejte uživatelské jméno (ve formě &lt;DomainName&gt;\\&lt;uživatelské jméno&gt;) a heslo správce clusteru HPC nebo jiný uživatel clusteru, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="5bab6-168">When prompted, type the user name (in the form &lt;DomainName&gt;\\&lt;UserName&gt;) and password of the HPC cluster administrator or another cluster user that you configured.</span></span> <span data-ttu-id="5bab6-169">Můžete k uložení pověření místně pro další operace úlohy.</span><span class="sxs-lookup"><span data-stu-id="5bab6-169">You can choose to store the credentials locally for more job operations.</span></span>
   
    <span data-ttu-id="5bab6-170">Zobrazí se seznam úloh.</span><span class="sxs-lookup"><span data-stu-id="5bab6-170">A list of jobs appears.</span></span>

<span data-ttu-id="5bab6-171">**Na klientském počítači používat Správce úloh HPC**</span><span class="sxs-lookup"><span data-stu-id="5bab6-171">**To use HPC Job Manager on the client computer**</span></span>

1. <span data-ttu-id="5bab6-172">Pokud při odesílání úlohy nebyla dříve ukládat přihlašovací údaje domény pro uživatele clusteru, můžete přidat přihlašovací údaje do správce přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5bab6-172">If you didn't previously store domain credentials for a cluster user when submitting a job, you can add the credentials in Credential Manager.</span></span>
   
    <span data-ttu-id="5bab6-173">a.</span><span class="sxs-lookup"><span data-stu-id="5bab6-173">a.</span></span> <span data-ttu-id="5bab6-174">V Ovládacích panelech klientského počítače spusťte Správce přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5bab6-174">In Control Panel on the client computer, start Credential Manager.</span></span>
   
    <span data-ttu-id="5bab6-175">b.</span><span class="sxs-lookup"><span data-stu-id="5bab6-175">b.</span></span> <span data-ttu-id="5bab6-176">Klikněte na tlačítko **pověření systému Windows** > **přidat obecné přihlašovací údaje**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-176">Click **Windows Credentials** > **Add a generic credential**.</span></span>
   
    <span data-ttu-id="5bab6-177">c.</span><span class="sxs-lookup"><span data-stu-id="5bab6-177">c.</span></span> <span data-ttu-id="5bab6-178">Zadejte internetovou adresu (například https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler nebo https://&lt;HeadNodeDnsName&gt;.&lt; oblast&gt;.cloudapp.azure.com/HpcScheduler) a uživatelské jméno (&lt;DomainName&gt;\\&lt;uživatelské jméno&gt;) a heslo správce clusteru nebo jiný uživatel clusteru, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="5bab6-178">Specify the Internet address (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com/HpcScheduler), and the user name (&lt;DomainName&gt;\\&lt;UserName&gt;) and password of the cluster administrator or another cluster user that you configured.</span></span>
2. <span data-ttu-id="5bab6-179">Na klientském počítači spusťte Správce úloh HPC.</span><span class="sxs-lookup"><span data-stu-id="5bab6-179">On the client computer, start HPC Job Manager.</span></span>
3. <span data-ttu-id="5bab6-180">V **vyberte hlavní uzel** dialogu, zadejte adresu URL k hlavnímu uzlu v Azure (například https://&lt;HeadNodeDnsName&gt;. cloudapp.net nebo https://&lt;HeadNodeDnsName&gt;.&lt; oblast&gt;. cloudapp.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5bab6-180">In the **Select Head Node** dialog box, type the URL to the head node in Azure (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com).</span></span>
   
    <span data-ttu-id="5bab6-181">Správce úloh HPC otevře a zobrazí se seznam úloh z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5bab6-181">HPC Job Manager opens and shows a list of jobs on the head node.</span></span>

<span data-ttu-id="5bab6-182">**Použití webového portálu systémem hlavního uzlu**</span><span class="sxs-lookup"><span data-stu-id="5bab6-182">**To use the web portal running on the head node**</span></span>

1. <span data-ttu-id="5bab6-183">Na klientském počítači spustit webový prohlížeč a zadejte jednu z následujících adres, v závislosti na úplný název DNS hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="5bab6-183">Start a web browser on the client computer, and enter one of the following addresses, depending on the full DNS name of the head node:</span></span>
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    <span data-ttu-id="5bab6-184">nebo</span><span class="sxs-lookup"><span data-stu-id="5bab6-184">or</span></span>
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. <span data-ttu-id="5bab6-185">V dialogovém okně zabezpečení, který se zobrazí zadejte přihlašovací údaje Správce clusteru HPC domény.</span><span class="sxs-lookup"><span data-stu-id="5bab6-185">In the security dialog box that appears, type the domain credentials of the HPC cluster administrator.</span></span> <span data-ttu-id="5bab6-186">(Můžete také přidat další clusteru uživatele v různých rolích.</span><span class="sxs-lookup"><span data-stu-id="5bab6-186">(You can also add other cluster users in different roles.</span></span> <span data-ttu-id="5bab6-187">V tématu [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335.aspx).)</span><span class="sxs-lookup"><span data-stu-id="5bab6-187">See [Managing Cluster Users](https://technet.microsoft.com/library/ff919335.aspx).)</span></span>
   
    <span data-ttu-id="5bab6-188">Na webový portál se otevře zobrazení seznamu úloh.</span><span class="sxs-lookup"><span data-stu-id="5bab6-188">The web portal opens to the job list view.</span></span>
3. <span data-ttu-id="5bab6-189">K odeslání vzorku úlohu, která vrátí řetězec "Hello World" z clusteru, klikněte na tlačítko **nová úloha** v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="5bab6-189">To submit a sample job that returns the string “Hello World” from the cluster, click **New job** in the left-hand navigation.</span></span>
4. <span data-ttu-id="5bab6-190">Na **nová úloha** v části **ze stránek odeslání**, klikněte na tlačítko **HelloWorld**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-190">On the **New Job** page, under **From submission pages**, click **HelloWorld**.</span></span> <span data-ttu-id="5bab6-191">Zobrazí se stránka odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="5bab6-191">The job submission page appears.</span></span>
5. <span data-ttu-id="5bab6-192">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="5bab6-192">Click **Submit**.</span></span> <span data-ttu-id="5bab6-193">Pokud se zobrazí výzva, zadejte přihlašovací údaje domény Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="5bab6-193">If prompted, provide the domain credentials of the HPC cluster administrator.</span></span> <span data-ttu-id="5bab6-194">Je úloha odeslána a ID úlohy se zobrazí na **Mé úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="5bab6-194">The job is submitted, and the job ID appears on the **My Jobs** page.</span></span>
6. <span data-ttu-id="5bab6-195">Pokud chcete zobrazit výsledky úlohy, které jste odeslali, klikněte na úlohu s ID a potom klikněte na **úlohy v zobrazení** k zobrazení výstupu příkazu (v části **výstup**).</span><span class="sxs-lookup"><span data-stu-id="5bab6-195">To view the results of the job that you submitted, click the job ID, and then click **View Tasks** to view the command output (under **Output**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bab6-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5bab6-196">Next steps</span></span>
* <span data-ttu-id="5bab6-197">Můžete také odesílání úloh do clusteru Azure s [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="5bab6-197">You can also submit jobs to the Azure cluster with the [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span>
* <span data-ttu-id="5bab6-198">Pokud chcete k odesílání úloh clusteru z klienta Linux, viz ukázka Pythonu ve [HPC Pack 2012 R2 SDK a ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="5bab6-198">If you want to submit cluster jobs from a Linux client, see the Python sample in the [HPC Pack 2012 R2 SDK and Sample Code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span>

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
