---
title: "NAMD pomocí sady Microsoft HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
description: "Nasazení clusteru s podporou sady Microsoft HPC Pack v Azure a spustit simulaci NAMD s charmrun v několika výpočetních uzlech Linux"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: e31845f3d7aa08357b0e8a1b3b77d97302442ac3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a><span data-ttu-id="6ff67-103">Spuštění NAMD se sadou Microsoft HPC Pack ve výpočetních uzlech Linuxu v Azure</span><span class="sxs-lookup"><span data-stu-id="6ff67-103">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>
<span data-ttu-id="6ff67-104">Tento článek ukazuje jeden ze způsobů spuštění Linux vysoce výkonné výpočty (HPC) zatížení na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="6ff67-104">This article shows you one way to run a Linux high-performance computing (HPC) workload on Azure virtual machines.</span></span> <span data-ttu-id="6ff67-105">Zde můžete nastavit [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster v Azure s Linuxem výpočetních uzlů a spusťte [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulace vypočítat a vizualizovat strukturu velké biomolekulárních systému.</span><span class="sxs-lookup"><span data-stu-id="6ff67-105">Here, you set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster on Azure with Linux compute nodes and run a [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulation to calculate and visualize the structure of a large biomolecular system.</span></span>  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* <span data-ttu-id="6ff67-106">**NAMD** (pro program molekulární Dynamics Nanoscale) je paralelní molekulární dynamics balíček určený pro vysoce výkonné simulace systémů velké biomolekulárních obsahující až miliony atomů.</span><span class="sxs-lookup"><span data-stu-id="6ff67-106">**NAMD** (for Nanoscale Molecular Dynamics program) is a parallel molecular dynamics package designed for high-performance simulation of large biomolecular systems containing up to millions of atoms.</span></span> <span data-ttu-id="6ff67-107">Mezi tyto systémy patří například viry, struktury buňky a velké bílkoviny.</span><span class="sxs-lookup"><span data-stu-id="6ff67-107">Examples of these systems include viruses, cell structures, and large proteins.</span></span> <span data-ttu-id="6ff67-108">NAMD škáluje stovky jader pro typické simulace a více než 500 000 jader pro největší simulace.</span><span class="sxs-lookup"><span data-stu-id="6ff67-108">NAMD scales to hundreds of cores for typical simulations and to more than 500,000 cores for the largest simulations.</span></span>
* <span data-ttu-id="6ff67-109">**Microsoft HPC Pack** poskytuje funkce, které chcete spouštět ve velkém měřítku HPC a paralelní aplikace v clusterech místního počítače nebo virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6ff67-109">**Microsoft HPC Pack** provides features to run large-scale HPC and parallel applications in clusters of on-premises computers or Azure virtual machines.</span></span> <span data-ttu-id="6ff67-110">Původně vyvinut jako řešení pro úlohy Windows HPC, HPC Pack teď podporuje spouštění aplikací prostředí HPC pro Linux v systému Linux výpočetní uzel virtuální počítače nasazené na clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="6ff67-110">Originally developed as a solution for Windows HPC workloads, HPC Pack now supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="6ff67-111">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) úvod.</span><span class="sxs-lookup"><span data-stu-id="6ff67-111">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction.</span></span>

<span data-ttu-id="6ff67-112">Další možnosti spuštění úloh HPC pro Linux v Azure najdete v tématu [technických prostředcích pro batch a vysoce výkonné výpočty](../../../batch/batch-hpc-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6ff67-112">For other options to run Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/batch-hpc-solutions.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ff67-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6ff67-113">Prerequisites</span></span>
* <span data-ttu-id="6ff67-114">**Výpočetní uzly clusteru HPC Pack operačního systému Linux** -nasazení clusteru HPC Pack s Linux výpočetní uzly v Azure pomocí buď [šablony Azure Resource Manageru](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript prostředí Azure PowerShell](hpcpack-cluster-powershell-script.md) .</span><span class="sxs-lookup"><span data-stu-id="6ff67-114">**HPC Pack cluster with Linux compute nodes** - Deploy an HPC Pack cluster with Linux compute nodes on Azure using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="6ff67-115">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) pro požadavky a kroky pro jednu z možností.</span><span class="sxs-lookup"><span data-stu-id="6ff67-115">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for the prerequisites and steps for either option.</span></span> <span data-ttu-id="6ff67-116">Pokud zvolíte možnost nasazení skriptu prostředí PowerShell, naleznete v souboru konfigurace ukázka v ukázkové soubory na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="6ff67-116">If you choose the PowerShell script deployment option, see the sample configuration file in the sample files at the end of this article.</span></span> <span data-ttu-id="6ff67-117">Tento soubor nakonfiguruje clusteru služby založené na Azure HPC Pack skládající se z hlavního uzlu systému Windows Server 2012 R2 a čtyři výpočetní uzly velké 6.6 CentOS velikost.</span><span class="sxs-lookup"><span data-stu-id="6ff67-117">This file configures an Azure-based HPC Pack cluster consisting of a Windows Server 2012 R2 head node and four size Large CentOS 6.6 compute nodes.</span></span> <span data-ttu-id="6ff67-118">Tento soubor přizpůsobte podle potřeby pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="6ff67-118">Customize this file as needed for your environment.</span></span>
* <span data-ttu-id="6ff67-119">**Soubory softwaru a kurzu NAMD** -NAMD stažení softwaru pro Linux z [NAMD](http://www.ks.uiuc.edu/Research/namd/) lokality (vyžaduje registraci).</span><span class="sxs-lookup"><span data-stu-id="6ff67-119">**NAMD software and tutorial files** - Download NAMD software for Linux from the [NAMD](http://www.ks.uiuc.edu/Research/namd/) site (registration required).</span></span> <span data-ttu-id="6ff67-120">Tento článek je založena na NAMD verze 2.10 a používá [Linux-x86_64 (64-bit Intel nebo AMD s sítě Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archivu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-120">This article is based on NAMD version 2.10, and uses the [Linux-x86_64 (64-bit Intel/AMD with Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archive.</span></span> <span data-ttu-id="6ff67-121">Také stáhnout [NAMD kurz soubory](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span><span class="sxs-lookup"><span data-stu-id="6ff67-121">Also download the [NAMD tutorial files](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span></span> <span data-ttu-id="6ff67-122">Stahování jsou soubory .tar a potřebujete nástroj Windows extrahujte soubory z hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6ff67-122">The downloads are .tar files, and you need a Windows tool to extract the files on the cluster head node.</span></span> <span data-ttu-id="6ff67-123">Extrahujte soubory, postupujte podle pokynů dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6ff67-123">To extract the files, follow the instructions later in this article.</span></span> 
* <span data-ttu-id="6ff67-124">**VMD** (volitelné) – Chcete-li zobrazit výsledky úlohy NAMD, stáhněte a nainstalujte program molekulární vizualizace [VMD](http://www.ks.uiuc.edu/Research/vmd/) na počítači podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="6ff67-124">**VMD** (optional) - To see the results of your NAMD job, download and install the molecular visualization program [VMD](http://www.ks.uiuc.edu/Research/vmd/) on a computer of your choice.</span></span> <span data-ttu-id="6ff67-125">Aktuální verze je 1.9.2.</span><span class="sxs-lookup"><span data-stu-id="6ff67-125">The current version is 1.9.2.</span></span> <span data-ttu-id="6ff67-126">V tématu VMD ke stažení začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="6ff67-126">See the VMD download site to get started.</span></span>  

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="6ff67-127">Nastavení vzájemného vztahu důvěryhodnosti mezi výpočetní uzly</span><span class="sxs-lookup"><span data-stu-id="6ff67-127">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="6ff67-128">Spuštění úlohy mezi uzly ve více uzlech Linux vyžaduje uzly důvěřovat navzájem (podle **rsh** nebo **ssh**).</span><span class="sxs-lookup"><span data-stu-id="6ff67-128">Running a cross-node job on multiple Linux nodes requires the nodes to trust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="6ff67-129">Při vytváření clusteru HPC Pack pomocí skriptu pro nasazení Microsoft HPC Pack IaaS, skript automaticky nastaví trvalé vzájemné vztahu důvěryhodnosti pro účet správce, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="6ff67-129">When you create the HPC Pack cluster with the Microsoft HPC Pack IaaS deployment script, the script automatically sets up permanent mutual trust for the administrator account you specify.</span></span> <span data-ttu-id="6ff67-130">Pro uživatele bez oprávnění správce, které vytvoříte v doméně do clusteru budete muset nainstalovat dočasné vzájemné vztah důvěryhodnosti mezi uzly, pokud je přidělen úlohu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-130">For non-administrator users you create in the cluster's domain, you have to set up temporary mutual trust among the nodes when a job is allocated to them.</span></span> <span data-ttu-id="6ff67-131">Pak odstraňte relace po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-131">Then, destroy the relationship after the job is complete.</span></span> <span data-ttu-id="6ff67-132">Chcete-li to provést pro každého uživatele, poskytnout pár klíče RSA do clusteru, který HPC Pack používá k navázání vztahu důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="6ff67-132">To do this for each user, provide an RSA key pair to the cluster which HPC Pack uses to establish the trust relationship.</span></span> <span data-ttu-id="6ff67-133">Postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="6ff67-133">Instructions follow.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="6ff67-134">Vygenerovat pár klíče RSA</span><span class="sxs-lookup"><span data-stu-id="6ff67-134">Generate an RSA key pair</span></span>
<span data-ttu-id="6ff67-135">Snadno vygenerovat pár klíče RSA, který obsahuje veřejný klíč a soukromý klíč a spuštěním sady Linux **ssh-keygen** příkaz.</span><span class="sxs-lookup"><span data-stu-id="6ff67-135">It's easy to generate an RSA key pair, which contains a public key and a private key, by running the Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="6ff67-136">Přihlaste se na počítač se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="6ff67-136">Log on to a Linux computer.</span></span>
2. <span data-ttu-id="6ff67-137">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6ff67-137">Run the following command:</span></span>
   
   ```bash
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="6ff67-138">Stiskněte klávesu **Enter** použít výchozí nastavení, dokud se nedokončí příkaz.</span><span class="sxs-lookup"><span data-stu-id="6ff67-138">Press **Enter** to use the default settings until the command is completed.</span></span> <span data-ttu-id="6ff67-139">Nezadávejte přístupové heslo zde; Po zobrazení výzvy k zadání hesla, stačí stisknout klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="6ff67-139">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Vygenerovat pár klíče RSA][keygen]
3. <span data-ttu-id="6ff67-141">Změňte adresář na adresář ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="6ff67-141">Change directory to the ~/.ssh directory.</span></span> <span data-ttu-id="6ff67-142">Privátní klíč uložen v id_rsa a veřejný klíč v id_rsa.pub.</span><span class="sxs-lookup"><span data-stu-id="6ff67-142">The private key is stored in id_rsa and the public key in id_rsa.pub.</span></span>
   
   ![Privátní a veřejné klíče][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a><span data-ttu-id="6ff67-144">Přidat dvojici klíčů do clusteru HPC Pack</span><span class="sxs-lookup"><span data-stu-id="6ff67-144">Add the key pair to the HPC Pack cluster</span></span>
1. <span data-ttu-id="6ff67-145">[Připojit pomocí vzdálené plochy](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) k hlavnímu uzlu virtuálních počítačů využívající tuto doménu můžete zadané údaje po nasazení clusteru (například hpc\clusteradmin).</span><span class="sxs-lookup"><span data-stu-id="6ff67-145">[Connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to the head node VM using the domain credentials you provided when you deployed the cluster (for example, hpc\clusteradmin).</span></span> <span data-ttu-id="6ff67-146">Spravujete z hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6ff67-146">You manage the cluster from the head node.</span></span>
2. <span data-ttu-id="6ff67-147">Použijte standardní postupy systému Windows Server k vytvoření uživatelského účtu domény v doméně služby Active Directory do clusteru.</span><span class="sxs-lookup"><span data-stu-id="6ff67-147">Use standard Windows Server procedures to create a domain user account in the cluster's Active Directory domain.</span></span> <span data-ttu-id="6ff67-148">Například pomocí nástroje Active Directory uživatele a počítače z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-148">For example, use the Active Directory User and Computers tool on the head node.</span></span> <span data-ttu-id="6ff67-149">V příkladech v tomto článku předpokládá, že vytvoříte uživatele domény s názvem hpcuser v doméně hpclab (hpclab\hpcuser).</span><span class="sxs-lookup"><span data-stu-id="6ff67-149">The examples in this article assume you create a domain user named hpcuser in the hpclab domain (hpclab\hpcuser).</span></span>
3. <span data-ttu-id="6ff67-150">Přidejte uživatele domény do clusteru HPC Pack jako uživatel clusteru.</span><span class="sxs-lookup"><span data-stu-id="6ff67-150">Add the domain user to the HPC Pack cluster as a cluster user.</span></span> <span data-ttu-id="6ff67-151">Pokyny najdete v tématu [přidat nebo odebrat uživatele clusteru](https://technet.microsoft.com/library/ff919330.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ff67-151">For instructions, see [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).</span></span>
4. <span data-ttu-id="6ff67-152">Vytvořte soubor s názvem C:\cred.xml a zkopírujte do ní data klíče RSA.</span><span class="sxs-lookup"><span data-stu-id="6ff67-152">Create a file named C:\cred.xml and copy the RSA key data into it.</span></span> <span data-ttu-id="6ff67-153">Příklad najdete v ukázkových souborů na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="6ff67-153">You can find an example in the sample files at the end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. <span data-ttu-id="6ff67-154">Otevřete příkazový řádek a zadejte následující příkaz pro nastavení data přihlašovací údaje pro účet hpclab\hpcuser.</span><span class="sxs-lookup"><span data-stu-id="6ff67-154">Open a Command Prompt and enter the following command to set the credentials data for the hpclab\hpcuser account.</span></span> <span data-ttu-id="6ff67-155">Můžete použít **extendeddata** parametr předat název souboru C:\cred.xml jste vytvořili pro klíčová data.</span><span class="sxs-lookup"><span data-stu-id="6ff67-155">You use the **extendeddata** parameter to pass the name of the C:\cred.xml file you created for the key data.</span></span>
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="6ff67-156">Tento příkaz se dokončí úspěšně bez výstupu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-156">This command completes successfully without output.</span></span> <span data-ttu-id="6ff67-157">Po nastavení přihlašovacích údajů pro uživatelské účty, které potřebujete ke spuštění úloh, cred.xml soubor uložte na bezpečném místě, nebo ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="6ff67-157">After setting the credentials for the user accounts you need to run jobs, store the cred.xml file in a secure location, or delete it.</span></span>
6. <span data-ttu-id="6ff67-158">Pokud jste vygenerovali páru klíčů RSA na jednom z uzlů Linux, nezapomeňte odstranit klíče po dokončení jejich používání.</span><span class="sxs-lookup"><span data-stu-id="6ff67-158">If you generated the RSA key pair on one of your Linux nodes, remember to delete the keys after you finish using them.</span></span> <span data-ttu-id="6ff67-159">HPC Pack nejsou nastaveny vzájemné vztahu důvěryhodnosti, pokud najde existující soubor id_rsa nebo id_rsa.pub souboru.</span><span class="sxs-lookup"><span data-stu-id="6ff67-159">HPC Pack does not set up mutual trust if it finds an existing id_rsa file or id_rsa.pub file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ff67-160">Nedoporučujeme spuštěna úlohu Linux jako správce clusteru na sdílený clusteru, protože úloha odeslána správce spustí pomocí kořenového účtu v uzlech systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6ff67-160">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under the root account on the Linux nodes.</span></span> <span data-ttu-id="6ff67-161">Úloha odeslána uživatel není správcem spouští pod místním uživatelským účtem Linux se stejným názvem jako uživatel úlohu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-161">A job submitted by a non-administrator user runs under a local Linux user account with the same name as the job user.</span></span> <span data-ttu-id="6ff67-162">V takovém případě HPC Pack nastaví vzájemné vztahu důvěryhodnosti pro tohoto uživatele Linux pro všechny uzly, které jsou přiděleny do úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-162">In this case, HPC Pack sets up mutual trust for this Linux user across all the nodes allocated to the job.</span></span> <span data-ttu-id="6ff67-163">Můžete nastavit uživatele Linux na Linuxových uzlů ručně před spuštěním úlohy, nebo HPC Pack vytváří uživatele automaticky, když je úloha odeslána.</span><span class="sxs-lookup"><span data-stu-id="6ff67-163">You can set up the Linux user manually on the Linux nodes before running the job, or HPC Pack creates the user automatically when the job is submitted.</span></span> <span data-ttu-id="6ff67-164">Pokud uživatel vytvoří HPC Pack HPC Pack neodstraní po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-164">If HPC Pack creates the user, HPC Pack deletes it after the job completes.</span></span> <span data-ttu-id="6ff67-165">Pokud chcete zkrátit ohrožení zabezpečení, se odeberou klíče po dokončení úlohy na uzlech.</span><span class="sxs-lookup"><span data-stu-id="6ff67-165">To reduce security threat, the keys are removed after the job completes on the nodes.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="6ff67-166">Nastavení sdílené složky pro Linuxové uzly</span><span class="sxs-lookup"><span data-stu-id="6ff67-166">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="6ff67-167">Teď nastavit sdílenou složku SMB a připojit sdílenou složku na všech uzlech Linux umožňující Linuxové uzly pro přístup k souborům NAMD s běžné cestou.</span><span class="sxs-lookup"><span data-stu-id="6ff67-167">Now set up an SMB file share, and mount the shared folder on all Linux nodes to allow the Linux nodes to access NAMD files with a common path.</span></span> <span data-ttu-id="6ff67-168">Toto jsou kroky připojit sdílenou složku z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-168">Following are steps to mount a shared folder on the head node.</span></span> <span data-ttu-id="6ff67-169">Sdílenou složku se doporučuje pro distribuce například CentOS 6.6, které aktuálně nepodporují službu Azure File.</span><span class="sxs-lookup"><span data-stu-id="6ff67-169">A share is recommended for distributions such as CentOS 6.6 that currently don’t support the Azure File service.</span></span> <span data-ttu-id="6ff67-170">Pokud uzly Linux podporují sdílenou složku Azure, najdete v části [postup používání Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6ff67-170">If your Linux nodes support an Azure File share, see [How to use Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="6ff67-171">Další možnosti sdílení s HPC Pack souborů, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru HPC Pack v Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="6ff67-171">For additional file sharing options with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="6ff67-172">Vytvořte složku na hlavního uzlu a sdílejte ji pro všechny uživatele, a to nastavením oprávnění pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="6ff67-172">Create a folder on the head node, and share it to Everyone by setting Read/Write privileges.</span></span> <span data-ttu-id="6ff67-173">V tomto příkladu \\ \\CentOS66HN\Namd je název složky, kde CentOS66HN je název hostitele hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-173">In this example, \\\\CentOS66HN\Namd is the name of the folder, where CentOS66HN is the host name of the head node.</span></span>
2. <span data-ttu-id="6ff67-174">Vytvořte podsložku s názvem namd2 ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="6ff67-174">Create a subfolder named namd2 in the shared folder.</span></span> <span data-ttu-id="6ff67-175">V namd2 vytvořte další podsložku s názvem namdsample.</span><span class="sxs-lookup"><span data-stu-id="6ff67-175">In namd2, create another subfolder named namdsample.</span></span>
3. <span data-ttu-id="6ff67-176">Extrahujte soubory NAMD ve složce tak, že používáte verzi systému Windows **vkládání** nebo jiný nástroj Windows, který pracuje s archivy .tar.</span><span class="sxs-lookup"><span data-stu-id="6ff67-176">Extract the NAMD files in the folder by using a Windows version of **tar** or another Windows utility that operates on .tar archives.</span></span> 
   
   * <span data-ttu-id="6ff67-177">Extrahování archiv vkládání NAMD \\ \\CentOS66HN\Namd\namd2.</span><span class="sxs-lookup"><span data-stu-id="6ff67-177">Extract the NAMD tar archive to \\\\CentOS66HN\Namd\namd2.</span></span>
   * <span data-ttu-id="6ff67-178">Extrahujte soubory kurzu v části \\ \\CentOS66HN\Namd\namd2\namdsample.</span><span class="sxs-lookup"><span data-stu-id="6ff67-178">Extract the tutorial files under \\\\CentOS66HN\Namd\namd2\namdsample.</span></span>
4. <span data-ttu-id="6ff67-179">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy Připojit sdílenou složku na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="6ff67-179">Open a Windows PowerShell window and run the following commands to mount the shared folder on the Linux nodes.</span></span>
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="6ff67-180">První příkaz vytvoří složku s názvem /namd2 ve všech uzlech v LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="6ff67-180">The first command creates a folder named /namd2 on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="6ff67-181">V druhém příkazu připojí //CentOS66HN/Namd/namd2 sdílené složky do složky s dir_mode a file_mode bits nastavený na 777.</span><span class="sxs-lookup"><span data-stu-id="6ff67-181">The second command mounts the shared folder //CentOS66HN/Namd/namd2 onto the folder with dir_mode and file_mode bits set to 777.</span></span> <span data-ttu-id="6ff67-182">*Uživatelské jméno* a *heslo* v příkazu by měl být přihlašovací údaje uživatele z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-182">The *username* and *password* in the command should be the credentials of a user on the head node.</span></span>

> [!NOTE]
> <span data-ttu-id="6ff67-183">"\\`" Symbol v druhém příkazu je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6ff67-183">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="6ff67-184">"\\`," znamená "," (čárku) je součástí příkazu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-184">“\\`,” means the “,” (comma character) is a part of the command.</span></span>
> 
> 

## <a name="create-a-bash-script-to-run-a-namd-job"></a><span data-ttu-id="6ff67-185">Vytvoření skriptu Bash a spusťte úlohu NAMD</span><span class="sxs-lookup"><span data-stu-id="6ff67-185">Create a Bash script to run a NAMD job</span></span>
<span data-ttu-id="6ff67-186">Potřebuje vaše úlohy NAMD *seznamu* souboru **charmrun** k určení počtu uzlů pro použití při spouštění NAMD procesy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-186">Your NAMD job needs a *nodelist* file for **charmrun** to determine the number of nodes to use when starting NAMD processes.</span></span> <span data-ttu-id="6ff67-187">Použít skript Bash, který generuje souboru seznamu a spouští **charmrun** s tohoto souboru seznamu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-187">You use a Bash script that generates the nodelist file and runs **charmrun** with this nodelist file.</span></span> <span data-ttu-id="6ff67-188">Potom můžete odeslat úlohu NAMD ve Správci clusteru HPC který volá tento skript.</span><span class="sxs-lookup"><span data-stu-id="6ff67-188">You can then submit a NAMD job in HPC Cluster Manager that calls this script.</span></span>

<span data-ttu-id="6ff67-189">Pomocí textového editoru podle vaší volby, vytvořte skript Bash ve složce /namd2 obsahující programové soubory NAMD a pojmenujte ji hpccharmrun.sh. Pro rychlé testování konceptu, zkopírujte skript hpccharmrun.sh příklad uvedený na konci tohoto článku, přejděte na [odeslat úlohu NAMD](#submit-a-namd-job).</span><span class="sxs-lookup"><span data-stu-id="6ff67-189">Using a text editor of your choice, create a Bash script in the /namd2 folder containing the NAMD program files and name it hpccharmrun.sh. For a quick proof of concept, copy the example hpccharmrun.sh script provided at the end of this article and go to [Submit a NAMD job](#submit-a-namd-job).</span></span>

> [!TIP]
> <span data-ttu-id="6ff67-190">Uložte skript jako textový soubor s Linuxem řádek zakončení (pouze LF, není CR LF).</span><span class="sxs-lookup"><span data-stu-id="6ff67-190">Save your script as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="6ff67-191">To zajišťuje, že ho běží správně na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="6ff67-191">This ensures that it runs properly on the Linux nodes.</span></span>
> 
> 

<span data-ttu-id="6ff67-192">Následují informace o jaké tento skript bash.</span><span class="sxs-lookup"><span data-stu-id="6ff67-192">Following are details about what this bash script does.</span></span> 

1. <span data-ttu-id="6ff67-193">Definujte některé proměnné.</span><span class="sxs-lookup"><span data-stu-id="6ff67-193">Define some variables.</span></span>
   
   ```bash
   #!/bin/bash
   
   # The path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. <span data-ttu-id="6ff67-194">Získáte informace o uzlu ze proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="6ff67-194">Get node information from the environment variables.</span></span> <span data-ttu-id="6ff67-195">$NODESCORES ukládá seznam rozdělení slov z $CCP_NODES_CORES.</span><span class="sxs-lookup"><span data-stu-id="6ff67-195">$NODESCORES stores a list of split words from $CCP_NODES_CORES.</span></span> <span data-ttu-id="6ff67-196">$COUNT je velikost $NODESCORES.</span><span class="sxs-lookup"><span data-stu-id="6ff67-196">$COUNT is the size of $NODESCORES.</span></span>
   
   ```
   # Get node information from the environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   <span data-ttu-id="6ff67-197">Formát pro proměnnou CCP_NODES_CORES $ vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6ff67-197">The format for the $CCP_NODES_CORES variable is as follows:</span></span>
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   <span data-ttu-id="6ff67-198">Tato proměnná uvádí celkový počet uzlů, názvy a počet jader na každém uzlu, které jsou přiděleny do úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-198">This variable lists the total number of nodes, node names, and number of cores on each node that are allocated to the job.</span></span> <span data-ttu-id="6ff67-199">Například pokud úloha potřebuje 10 jader ke spuštění, $CCP_NODES_CORES hodnotu podobně jako:</span><span class="sxs-lookup"><span data-stu-id="6ff67-199">For example, if the job needs 10 cores to run, the value of $CCP_NODES_CORES is similar to:</span></span>
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. <span data-ttu-id="6ff67-200">Pokud není nastavena proměnná CCP_NODES_CORES $, spusťte **charmrun** přímo.</span><span class="sxs-lookup"><span data-stu-id="6ff67-200">If the $CCP_NODES_CORES variable is not set, start **charmrun** directly.</span></span> <span data-ttu-id="6ff67-201">(To by mělo pouze nastat při spuštění tohoto skriptu přímo na Linuxových uzlů.)</span><span class="sxs-lookup"><span data-stu-id="6ff67-201">(This should only occur when you run this script directly on your Linux nodes.)</span></span>
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. <span data-ttu-id="6ff67-202">Nebo vytvořte souboru seznamu pro **charmrun**.</span><span class="sxs-lookup"><span data-stu-id="6ff67-202">Or create a nodelist file for **charmrun**.</span></span>
   
   ```
   else
     # Create the nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write the head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into the nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. <span data-ttu-id="6ff67-203">Spustit **charmrun** s souboru seznamu získat návratový stav a odeberte souboru seznamu na konci.</span><span class="sxs-lookup"><span data-stu-id="6ff67-203">Run **charmrun** with the nodelist file, get its return status, and remove the nodelist file at the end.</span></span>
   
   <span data-ttu-id="6ff67-204">${CCP_NUMCPUS} je nastaven hlavního uzlu HPC Pack jiné proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="6ff67-204">${CCP_NUMCPUS} is another environment variable set by the HPC Pack head node.</span></span> <span data-ttu-id="6ff67-205">Ukládá číslo celkový počet jader přidělené této úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-205">It stores the number of total cores allocated to this job.</span></span> <span data-ttu-id="6ff67-206">Můžeme použít k určení počtu procesů pro charmrun.</span><span class="sxs-lookup"><span data-stu-id="6ff67-206">We use it to specify the number of processes for charmrun.</span></span>
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. <span data-ttu-id="6ff67-207">Ukončete s **charmrun** návratový stav.</span><span class="sxs-lookup"><span data-stu-id="6ff67-207">Exit with the **charmrun** return status.</span></span>
   
   ```
   exit ${RTNSTS}
   ```

<span data-ttu-id="6ff67-208">Níže najdete informace v souboru seznamu, který generuje skript:</span><span class="sxs-lookup"><span data-stu-id="6ff67-208">Following is the information in the nodelist file, which the script generates:</span></span>

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

<span data-ttu-id="6ff67-209">Například:</span><span class="sxs-lookup"><span data-stu-id="6ff67-209">For example:</span></span>

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a><span data-ttu-id="6ff67-210">Odeslání úlohy NAMD</span><span class="sxs-lookup"><span data-stu-id="6ff67-210">Submit a NAMD job</span></span>
<span data-ttu-id="6ff67-211">Nyní jste připraveni se odeslat úlohu NAMD ve Správci clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="6ff67-211">Now you are ready to submit a NAMD job in HPC Cluster Manager.</span></span>

1. <span data-ttu-id="6ff67-212">Připojení k vaší hlavního uzlu clusteru a spusťte Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="6ff67-212">Connect to your cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="6ff67-213">V **Správa prostředků**, zajistěte, aby výpočetní uzly Linux v **Online** stavu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-213">In **Resource Management**, ensure that the Linux compute nodes are in the **Online** state.</span></span> <span data-ttu-id="6ff67-214">Pokud tomu tak není, vyberte je a klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="6ff67-214">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="6ff67-215">V **úlohy správy**, klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="6ff67-215">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="6ff67-216">Zadejte název úlohy, jako *hpccharmrun*.</span><span class="sxs-lookup"><span data-stu-id="6ff67-216">Enter a name for job such as *hpccharmrun*.</span></span>
   
   ![Nové úlohy HPC][namd_job]
5. <span data-ttu-id="6ff67-218">Na **podrobnosti úlohy** v části **úlohy prostředky**, vyberte typ prostředku jako **uzlu** a nastavte **minimální** na 3.</span><span class="sxs-lookup"><span data-stu-id="6ff67-218">On the **Job Details** page, under **Job Resources**, select the type of resource as **Node** and set the **Minimum** to 3.</span></span> <span data-ttu-id="6ff67-219">, jsme spustit úlohu na tři uzly Linux a každý uzel má čtyři jádra.</span><span class="sxs-lookup"><span data-stu-id="6ff67-219">, we run the job on three Linux nodes and each node has four cores.</span></span>
   
   ![Úloha prostředky][job_resources]
6. <span data-ttu-id="6ff67-221">Klikněte na tlačítko **upravit úlohy** v levém navigačním panelu a pak klikněte na tlačítko **přidat** k přidání úkolu do úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-221">Click **Edit Tasks** in the left navigation, and then click **Add** to add a task to the job.</span></span>    
7. <span data-ttu-id="6ff67-222">Na **podrobností úlohy a přesměrování vstupu a výstupu** nastavte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6ff67-222">On the **Task Details and I/O Redirection** page, set the following values:</span></span>
   
   * <span data-ttu-id="6ff67-223">**Příkazový řádek**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span><span class="sxs-lookup"><span data-stu-id="6ff67-223">**Command line** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span></span>
     
     > [!TIP]
     > <span data-ttu-id="6ff67-224">Předchozí příkazový řádek je jeden příkaz bez konce řádků.</span><span class="sxs-lookup"><span data-stu-id="6ff67-224">The preceding command line is a single command without line breaks.</span></span> <span data-ttu-id="6ff67-225">Se zobrazí na několika řádcích pod zabalí **příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="6ff67-225">It wraps to appear on several lines under **Command line**.</span></span>
     > 
     > 
   * <span data-ttu-id="6ff67-226">**Pracovní adresář** -/namd2</span><span class="sxs-lookup"><span data-stu-id="6ff67-226">**Working directory** - /namd2</span></span>
   * <span data-ttu-id="6ff67-227">**Minimální** – 3</span><span class="sxs-lookup"><span data-stu-id="6ff67-227">**Minimum** - 3</span></span>
     
     ![Podrobnosti úlohy][task_details]
     
     > [!NOTE]
     > <span data-ttu-id="6ff67-229">Pracovní adresář tady nastavíte protože **charmrun** pokusí přejděte na stejný pracovní adresář na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-229">You set the working directory here because **charmrun** tries to navigate to the same working directory on each node.</span></span> <span data-ttu-id="6ff67-230">Pokud není nastavena pracovní adresář, HPC Pack spustí příkaz náhodným názvem složky vytvořené v jednom z uzlů Linux.</span><span class="sxs-lookup"><span data-stu-id="6ff67-230">If the working directory isn't set, HPC Pack starts the command in a randomly named folder created on one of the Linux nodes.</span></span> <span data-ttu-id="6ff67-231">To způsobí, že k následující chybě na ostatních uzlech: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` k tomuto problému nedošlo, zadejte cestu ke složce, která je přístupná ve všech uzlech jako pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="6ff67-231">This causes the following error on the other nodes: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` To avoid this problem, specify a folder path that can be accessed by all nodes as the working directory.</span></span>
     > 
     > 
8. <span data-ttu-id="6ff67-232">Klikněte na tlačítko **OK** a pak klikněte na **odeslání** ke spuštění této úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-232">Click **OK** and then click **Submit** to run this job.</span></span>
   
   <span data-ttu-id="6ff67-233">Ve výchozím nastavení HPC Pack předá úlohu jako váš aktuální účet přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ff67-233">By default, HPC Pack submits the job as your current logged-on user account.</span></span> <span data-ttu-id="6ff67-234">Dialogové okno může zobrazit výzva k zadání uživatelského jména a hesla, po kliknutí na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="6ff67-234">A dialog box might prompt you to enter the user name and password after you click **Submit**.</span></span>
   
   ![Přihlašovací údaje úlohy][creds]
   
   <span data-ttu-id="6ff67-236">Za určitých podmínek HPC Pack pamatuje informace o uživateli, zadejte před a nezobrazí tohoto dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6ff67-236">Under some conditions, HPC Pack remembers the user information you input before and doesn't show this dialog box.</span></span> <span data-ttu-id="6ff67-237">Chcete-li HPC Pack jej znovu zobrazit, zadejte následující příkaz na příkazovém řádku a potom odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-237">To make HPC Pack show it again, enter the following command at a Command Prompt and then submit the job.</span></span>
   
   ```command
   hpccred delcreds
   ```
9. <span data-ttu-id="6ff67-238">Úloha trvá několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="6ff67-238">The job takes several minutes to finish.</span></span>
10. <span data-ttu-id="6ff67-239">Najít protokol úlohy v \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log a výstupní soubory v \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span><span class="sxs-lookup"><span data-stu-id="6ff67-239">Find the job log at \\<headnodeName>\Namd\namd2\namd2_hpccharmrun.log and the output files in \\<headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span></span>
11. <span data-ttu-id="6ff67-240">Volitelně můžete spusťte VMD zobrazíte výsledky úlohy.</span><span class="sxs-lookup"><span data-stu-id="6ff67-240">Optionally, start VMD to view your job results.</span></span> <span data-ttu-id="6ff67-241">Kroky pro vizualizaci NAMD výstupní soubory (v tomto případě ubiquitin bílkovin molekuly v oblasti horních) jsou nad rámec tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="6ff67-241">The steps for visualizing the NAMD output files (in this case, a ubiquitin protein molecule in a water sphere) are beyond the scope of this article.</span></span> <span data-ttu-id="6ff67-242">V tématu [NAMD kurzu](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6ff67-242">See [NAMD Tutorial](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) for details.</span></span>
    
    ![Výsledky úlohy][vmd_view]

## <a name="sample-files"></a><span data-ttu-id="6ff67-244">Ukázkové soubory</span><span class="sxs-lookup"><span data-stu-id="6ff67-244">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="6ff67-245">Ukázkový soubor XML konfigurace pro nasazení clusteru pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ff67-245">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a><span data-ttu-id="6ff67-246">Ukázkový soubor cred.xml</span><span class="sxs-lookup"><span data-stu-id="6ff67-246">Sample cred.xml file</span></span>
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a><span data-ttu-id="6ff67-247">Ukázkový skript hpccharmrun.sh</span><span class="sxs-lookup"><span data-stu-id="6ff67-247">Sample hpccharmrun.sh script</span></span>
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
