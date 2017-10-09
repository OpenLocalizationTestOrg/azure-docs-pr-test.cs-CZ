---
title: "aaaNAMD pomocí sady Microsoft HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: 90c722f66b494335a62c0613079366ae99002076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a><span data-ttu-id="53c2a-103">Spuštění NAMD se sadou Microsoft HPC Pack ve výpočetních uzlech Linuxu v Azure</span><span class="sxs-lookup"><span data-stu-id="53c2a-103">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>
<span data-ttu-id="53c2a-104">Tento článek ukazuje jeden ze způsobů toorun Linux vysoce výkonné výpočty (HPC) zatížení na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="53c2a-104">This article shows you one way toorun a Linux high-performance computing (HPC) workload on Azure virtual machines.</span></span> <span data-ttu-id="53c2a-105">Zde můžete nastavit [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster v Azure s Linuxem výpočetních uzlů a spusťte [NAMD](http://www.ks.uiuc.edu/Research/namd/) toocalculate simulace a vizualizovat hello struktura velké biomolekulárních systému.</span><span class="sxs-lookup"><span data-stu-id="53c2a-105">Here, you set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster on Azure with Linux compute nodes and run a [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulation toocalculate and visualize hello structure of a large biomolecular system.</span></span>  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* <span data-ttu-id="53c2a-106">**NAMD** (pro program molekulární Dynamics Nanoscale) je paralelní molekulární dynamics balíček určený pro vysoce výkonné simulace velké biomolekulárních systémů, který obsahuje až toomillions atomů.</span><span class="sxs-lookup"><span data-stu-id="53c2a-106">**NAMD** (for Nanoscale Molecular Dynamics program) is a parallel molecular dynamics package designed for high-performance simulation of large biomolecular systems containing up toomillions of atoms.</span></span> <span data-ttu-id="53c2a-107">Mezi tyto systémy patří například viry, struktury buňky a velké bílkoviny.</span><span class="sxs-lookup"><span data-stu-id="53c2a-107">Examples of these systems include viruses, cell structures, and large proteins.</span></span> <span data-ttu-id="53c2a-108">NAMD škáluje toohundreds jader pro typické simulace a toomore než 500 000 jader pro největší simulace hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-108">NAMD scales toohundreds of cores for typical simulations and toomore than 500,000 cores for hello largest simulations.</span></span>
* <span data-ttu-id="53c2a-109">**Microsoft HPC Pack** poskytuje funkce toorun rozsáhlé HPC a paralelní aplikace v clusterech místního počítače nebo virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="53c2a-109">**Microsoft HPC Pack** provides features toorun large-scale HPC and parallel applications in clusters of on-premises computers or Azure virtual machines.</span></span> <span data-ttu-id="53c2a-110">Původně vyvinut jako řešení pro úlohy Windows HPC, HPC Pack teď podporuje spouštění aplikací prostředí HPC pro Linux v systému Linux výpočetní uzel virtuální počítače nasazené na clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="53c2a-110">Originally developed as a solution for Windows HPC workloads, HPC Pack now supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="53c2a-111">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) úvod.</span><span class="sxs-lookup"><span data-stu-id="53c2a-111">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction.</span></span>

<span data-ttu-id="53c2a-112">Pro další možnosti toorun Linux HPC najdete v části úlohy v Azure, [technických prostředcích pro batch a vysoce výkonné výpočty](../../../batch/batch-hpc-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="53c2a-112">For other options toorun Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/batch-hpc-solutions.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53c2a-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="53c2a-113">Prerequisites</span></span>
* <span data-ttu-id="53c2a-114">**Výpočetní uzly clusteru HPC Pack operačního systému Linux** -nasazení clusteru HPC Pack s Linux výpočetní uzly v Azure pomocí buď [šablony Azure Resource Manageru](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript prostředí Azure PowerShell](hpcpack-cluster-powershell-script.md) .</span><span class="sxs-lookup"><span data-stu-id="53c2a-114">**HPC Pack cluster with Linux compute nodes** - Deploy an HPC Pack cluster with Linux compute nodes on Azure using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="53c2a-115">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) hello požadavky a kroky pro jednu z možností.</span><span class="sxs-lookup"><span data-stu-id="53c2a-115">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for hello prerequisites and steps for either option.</span></span> <span data-ttu-id="53c2a-116">Pokud si zvolíte hello možnost nasazení skriptu prostředí PowerShell, najdete v části hello vzorový konfigurační soubor v hello ukázkových souborů v hello konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="53c2a-116">If you choose hello PowerShell script deployment option, see hello sample configuration file in hello sample files at hello end of this article.</span></span> <span data-ttu-id="53c2a-117">Tento soubor nakonfiguruje clusteru služby založené na Azure HPC Pack skládající se z hlavního uzlu systému Windows Server 2012 R2 a čtyři výpočetní uzly velké 6.6 CentOS velikost.</span><span class="sxs-lookup"><span data-stu-id="53c2a-117">This file configures an Azure-based HPC Pack cluster consisting of a Windows Server 2012 R2 head node and four size Large CentOS 6.6 compute nodes.</span></span> <span data-ttu-id="53c2a-118">Tento soubor přizpůsobte podle potřeby pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="53c2a-118">Customize this file as needed for your environment.</span></span>
* <span data-ttu-id="53c2a-119">**Soubory softwaru a kurzu NAMD** -NAMD stažení softwaru pro Linux z hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) lokality (vyžaduje registraci).</span><span class="sxs-lookup"><span data-stu-id="53c2a-119">**NAMD software and tutorial files** - Download NAMD software for Linux from hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) site (registration required).</span></span> <span data-ttu-id="53c2a-120">Tento článek je založena na NAMD verze 2.10 a používá hello [Linux-x86_64 (64-bit Intel nebo AMD s sítě Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archivu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-120">This article is based on NAMD version 2.10, and uses hello [Linux-x86_64 (64-bit Intel/AMD with Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archive.</span></span> <span data-ttu-id="53c2a-121">Také stáhnout hello [NAMD kurz soubory](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span><span class="sxs-lookup"><span data-stu-id="53c2a-121">Also download hello [NAMD tutorial files](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span></span> <span data-ttu-id="53c2a-122">soubory ke stažení Hello jsou soubory .tar a potřebujete souborů hello tooextract nástroj Windows hello hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="53c2a-122">hello downloads are .tar files, and you need a Windows tool tooextract hello files on hello cluster head node.</span></span> <span data-ttu-id="53c2a-123">soubory hello tooextract, postupujte podle pokynů hello později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="53c2a-123">tooextract hello files, follow hello instructions later in this article.</span></span> 
* <span data-ttu-id="53c2a-124">**VMD** (volitelné) – toosee hello výsledků úlohy NAMD, stáhněte a nainstalujte program molekulární vizualizace hello [VMD](http://www.ks.uiuc.edu/Research/vmd/) na počítači podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="53c2a-124">**VMD** (optional) - toosee hello results of your NAMD job, download and install hello molecular visualization program [VMD](http://www.ks.uiuc.edu/Research/vmd/) on a computer of your choice.</span></span> <span data-ttu-id="53c2a-125">aktuální verze Hello je 1.9.2.</span><span class="sxs-lookup"><span data-stu-id="53c2a-125">hello current version is 1.9.2.</span></span> <span data-ttu-id="53c2a-126">V tématu hello VMD stáhnout tooget lokality spuštěn.</span><span class="sxs-lookup"><span data-stu-id="53c2a-126">See hello VMD download site tooget started.</span></span>  

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="53c2a-127">Nastavení vzájemného vztahu důvěryhodnosti mezi výpočetní uzly</span><span class="sxs-lookup"><span data-stu-id="53c2a-127">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="53c2a-128">Spuštění úlohy mezi uzly ve více uzlech Linux vyžaduje hello uzly tootrust, vzájemně (podle **rsh** nebo **ssh**).</span><span class="sxs-lookup"><span data-stu-id="53c2a-128">Running a cross-node job on multiple Linux nodes requires hello nodes tootrust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="53c2a-129">Při vytváření clusteru HPC Pack hello s hello skript nasazení Microsoft HPC Pack IaaS, hello skript automaticky nastaví trvalé vzájemné důvěryhodnosti hello správce účtu, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="53c2a-129">When you create hello HPC Pack cluster with hello Microsoft HPC Pack IaaS deployment script, hello script automatically sets up permanent mutual trust for hello administrator account you specify.</span></span> <span data-ttu-id="53c2a-130">Pro uživatele bez oprávnění správce, které vytvoříte v doméně hello clusteru máte tooset až dočasné vzájemné vztah důvěryhodnosti mezi uzly hello při přidělení toothem úlohu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-130">For non-administrator users you create in hello cluster's domain, you have tooset up temporary mutual trust among hello nodes when a job is allocated toothem.</span></span> <span data-ttu-id="53c2a-131">Po dokončení úlohy hello pak, odstraňte hello vztah.</span><span class="sxs-lookup"><span data-stu-id="53c2a-131">Then, destroy hello relationship after hello job is complete.</span></span> <span data-ttu-id="53c2a-132">toodo to pro každého uživatele, zadejte clusteru toohello páru klíčů RSA které HPC Pack používá tooestablish hello důvěryhodný vztah.</span><span class="sxs-lookup"><span data-stu-id="53c2a-132">toodo this for each user, provide an RSA key pair toohello cluster which HPC Pack uses tooestablish hello trust relationship.</span></span> <span data-ttu-id="53c2a-133">Postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="53c2a-133">Instructions follow.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="53c2a-134">Vygenerovat pár klíče RSA</span><span class="sxs-lookup"><span data-stu-id="53c2a-134">Generate an RSA key pair</span></span>
<span data-ttu-id="53c2a-135">Je snadno toogenerate pár klíče RSA, který obsahuje veřejný klíč a soukromý klíč, spuštěním hello Linux **ssh-keygen** příkaz.</span><span class="sxs-lookup"><span data-stu-id="53c2a-135">It's easy toogenerate an RSA key pair, which contains a public key and a private key, by running hello Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="53c2a-136">Přihlaste se tooa počítač se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="53c2a-136">Log on tooa Linux computer.</span></span>
2. <span data-ttu-id="53c2a-137">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="53c2a-137">Run hello following command:</span></span>
   
   ```bash
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="53c2a-138">Stiskněte klávesu **Enter** toouse hello výchozí nastavení až do dokončení příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-138">Press **Enter** toouse hello default settings until hello command is completed.</span></span> <span data-ttu-id="53c2a-139">Nezadávejte přístupové heslo zde; Po zobrazení výzvy k zadání hesla, stačí stisknout klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="53c2a-139">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Vygenerovat pár klíče RSA][keygen]
3. <span data-ttu-id="53c2a-141">Změňte adresář toohello ~/.ssh adresář.</span><span class="sxs-lookup"><span data-stu-id="53c2a-141">Change directory toohello ~/.ssh directory.</span></span> <span data-ttu-id="53c2a-142">Hello privátní klíč uložen v id_rsa a hello veřejný klíč v id_rsa.pub.</span><span class="sxs-lookup"><span data-stu-id="53c2a-142">hello private key is stored in id_rsa and hello public key in id_rsa.pub.</span></span>
   
   ![Privátní a veřejné klíče][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a><span data-ttu-id="53c2a-144">Přidejte cluster HPC Pack toohello páru klíčů hello</span><span class="sxs-lookup"><span data-stu-id="53c2a-144">Add hello key pair toohello HPC Pack cluster</span></span>
1. <span data-ttu-id="53c2a-145">[Připojit pomocí vzdálené plochy](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello hlavního uzlu virtuální počítač pomocí hello pověření domény, které jste zadali při nasazení clusteru hello (například hpc\clusteradmin).</span><span class="sxs-lookup"><span data-stu-id="53c2a-145">[Connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello head node VM using hello domain credentials you provided when you deployed hello cluster (for example, hpc\clusteradmin).</span></span> <span data-ttu-id="53c2a-146">Spravujete hello clusteru z hlavního uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-146">You manage hello cluster from hello head node.</span></span>
2. <span data-ttu-id="53c2a-147">Použijte standardní systému Windows Server postupy toocreate účet uživatele domény v doméně služby Active Directory hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="53c2a-147">Use standard Windows Server procedures toocreate a domain user account in hello cluster's Active Directory domain.</span></span> <span data-ttu-id="53c2a-148">Například použijte nástroj počítače a hello uživatele služby Active Directory hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-148">For example, use hello Active Directory User and Computers tool on hello head node.</span></span> <span data-ttu-id="53c2a-149">Hello příklady v tomto článku předpokládá, že vytvoříte uživatele domény s názvem hpcuser v doméně hpclab hello (hpclab\hpcuser).</span><span class="sxs-lookup"><span data-stu-id="53c2a-149">hello examples in this article assume you create a domain user named hpcuser in hello hpclab domain (hpclab\hpcuser).</span></span>
3. <span data-ttu-id="53c2a-150">Přidejte hello domény uživatele toohello HPC Pack clusteru jako uživatel clusteru.</span><span class="sxs-lookup"><span data-stu-id="53c2a-150">Add hello domain user toohello HPC Pack cluster as a cluster user.</span></span> <span data-ttu-id="53c2a-151">Pokyny najdete v tématu [přidat nebo odebrat uživatele clusteru](https://technet.microsoft.com/library/ff919330.aspx).</span><span class="sxs-lookup"><span data-stu-id="53c2a-151">For instructions, see [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).</span></span>
4. <span data-ttu-id="53c2a-152">Vytvořte soubor s názvem C:\cred.xml a zkopírujte hello RSA klíčová data do ní.</span><span class="sxs-lookup"><span data-stu-id="53c2a-152">Create a file named C:\cred.xml and copy hello RSA key data into it.</span></span> <span data-ttu-id="53c2a-153">Příklad můžete najít v hello ukázkových souborů v hello konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="53c2a-153">You can find an example in hello sample files at hello end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. <span data-ttu-id="53c2a-154">Otevřete příkazový řádek a zadejte následující příkaz údaje tooset hello data pro účet hpclab\hpcuser hello hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-154">Open a Command Prompt and enter hello following command tooset hello credentials data for hello hpclab\hpcuser account.</span></span> <span data-ttu-id="53c2a-155">Použít hello **extendeddata** toopass parametr hello název souboru hello C:\cred.xml jste vytvořili pro hello klíčová data.</span><span class="sxs-lookup"><span data-stu-id="53c2a-155">You use hello **extendeddata** parameter toopass hello name of hello C:\cred.xml file you created for hello key data.</span></span>
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="53c2a-156">Tento příkaz se dokončí úspěšně bez výstupu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-156">This command completes successfully without output.</span></span> <span data-ttu-id="53c2a-157">Po nastavení hello přihlašovací údaje pro hello uživatelské účty, které že budete potřebovat toorun úlohy, hello cred.xml soubor uložte na bezpečném místě, nebo ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="53c2a-157">After setting hello credentials for hello user accounts you need toorun jobs, store hello cred.xml file in a secure location, or delete it.</span></span>
6. <span data-ttu-id="53c2a-158">Pokud jste vygenerovali hello páru klíčů RSA na jednom z uzlů Linux, mějte na paměti klíče hello toodelete po dokončení jejich používání.</span><span class="sxs-lookup"><span data-stu-id="53c2a-158">If you generated hello RSA key pair on one of your Linux nodes, remember toodelete hello keys after you finish using them.</span></span> <span data-ttu-id="53c2a-159">HPC Pack nejsou nastaveny vzájemné vztahu důvěryhodnosti, pokud najde existující soubor id_rsa nebo id_rsa.pub souboru.</span><span class="sxs-lookup"><span data-stu-id="53c2a-159">HPC Pack does not set up mutual trust if it finds an existing id_rsa file or id_rsa.pub file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53c2a-160">Nedoporučujeme spuštěna úlohu Linux jako správce clusteru na sdílený clusteru, protože úlohu odeslání správcem spouští hello kořenového účtu na uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-160">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under hello root account on hello Linux nodes.</span></span> <span data-ttu-id="53c2a-161">Úloha odeslána uživatel není správcem spouští pod místním uživatelským účtem Linux s hello stejný název jako uživatel úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-161">A job submitted by a non-administrator user runs under a local Linux user account with hello same name as hello job user.</span></span> <span data-ttu-id="53c2a-162">V takovém případě HPC Pack nastaví vzájemné vztahu důvěryhodnosti pro tohoto uživatele Linux pro všechny uzly hello přidělené toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="53c2a-162">In this case, HPC Pack sets up mutual trust for this Linux user across all hello nodes allocated toohello job.</span></span> <span data-ttu-id="53c2a-163">Můžete nastavit uživatele Linux hello ručně na hello Linuxové uzly před spuštěním úlohy hello nebo HPC Pack hello uživatele automaticky vytvoří při odeslání úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-163">You can set up hello Linux user manually on hello Linux nodes before running hello job, or HPC Pack creates hello user automatically when hello job is submitted.</span></span> <span data-ttu-id="53c2a-164">Pokud HPC Pack vytvoří hello uživatele, HPC Pack neodstraní po dokončení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-164">If HPC Pack creates hello user, HPC Pack deletes it after hello job completes.</span></span> <span data-ttu-id="53c2a-165">tooreduce ohrožení zabezpečení, hello, které jsou odebrány klíče po dokončení úlohy hello na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-165">tooreduce security threat, hello keys are removed after hello job completes on hello nodes.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="53c2a-166">Nastavení sdílené složky pro Linuxové uzly</span><span class="sxs-lookup"><span data-stu-id="53c2a-166">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="53c2a-167">Nyní nastavit sdílenou složku SMB a připojit hello sdílenou složku na všechny Linux uzly tooallow hello Linux uzly tooaccess NAMD soubory s běžné cestou.</span><span class="sxs-lookup"><span data-stu-id="53c2a-167">Now set up an SMB file share, and mount hello shared folder on all Linux nodes tooallow hello Linux nodes tooaccess NAMD files with a common path.</span></span> <span data-ttu-id="53c2a-168">Tady jsou kroky toomount sdílenou složku hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-168">Following are steps toomount a shared folder on hello head node.</span></span> <span data-ttu-id="53c2a-169">Sdílenou složku se doporučuje pro distribuce například CentOS 6.6, které aktuálně nepodporují službu Azure File hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-169">A share is recommended for distributions such as CentOS 6.6 that currently don’t support hello Azure File service.</span></span> <span data-ttu-id="53c2a-170">Pokud uzly Linux podporují sdílenou složku Azure, najdete v části [jak toouse Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="53c2a-170">If your Linux nodes support an Azure File share, see [How toouse Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="53c2a-171">Další možnosti sdílení s HPC Pack souborů, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru HPC Pack v Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="53c2a-171">For additional file sharing options with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="53c2a-172">Vytvořte složku hello hlavního uzlu a sdílejte ji tooEveryone nastavením oprávnění pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="53c2a-172">Create a folder on hello head node, and share it tooEveryone by setting Read/Write privileges.</span></span> <span data-ttu-id="53c2a-173">V tomto příkladu \\ \\CentOS66HN\Namd je název hello hello složky, kde CentOS66HN je název hostitele hello hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-173">In this example, \\\\CentOS66HN\Namd is hello name of hello folder, where CentOS66HN is hello host name of hello head node.</span></span>
2. <span data-ttu-id="53c2a-174">Vytvořte podsložku s názvem namd2 ve sdílené složce hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-174">Create a subfolder named namd2 in hello shared folder.</span></span> <span data-ttu-id="53c2a-175">V namd2 vytvořte další podsložku s názvem namdsample.</span><span class="sxs-lookup"><span data-stu-id="53c2a-175">In namd2, create another subfolder named namdsample.</span></span>
3. <span data-ttu-id="53c2a-176">Extrahujte hello NAMD souborů ve složce hello tak, že používáte verzi systému Windows **vkládání** nebo jiný nástroj Windows, který pracuje s archivy .tar.</span><span class="sxs-lookup"><span data-stu-id="53c2a-176">Extract hello NAMD files in hello folder by using a Windows version of **tar** or another Windows utility that operates on .tar archives.</span></span> 
   
   * <span data-ttu-id="53c2a-177">Extrahujte archivní vkládání NAMD hello příliš\\\\CentOS66HN\Namd\namd2.</span><span class="sxs-lookup"><span data-stu-id="53c2a-177">Extract hello NAMD tar archive too\\\\CentOS66HN\Namd\namd2.</span></span>
   * <span data-ttu-id="53c2a-178">Extrahujte soubory hello kurzu v části \\ \\CentOS66HN\Namd\namd2\namdsample.</span><span class="sxs-lookup"><span data-stu-id="53c2a-178">Extract hello tutorial files under \\\\CentOS66HN\Namd\namd2\namdsample.</span></span>
4. <span data-ttu-id="53c2a-179">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy toomount hello sdílenou složku na Linuxových uzlů hello hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-179">Open a Windows PowerShell window and run hello following commands toomount hello shared folder on hello Linux nodes.</span></span>
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="53c2a-180">První příkaz Hello vytvoří složku s názvem /namd2 ve všech uzlech v hello LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="53c2a-180">hello first command creates a folder named /namd2 on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="53c2a-181">druhý příkaz Hello připojí //CentOS66HN/Namd/namd2 hello sdílené složky do složky hello s dir_mode a file_mode too777 sadu bitů.</span><span class="sxs-lookup"><span data-stu-id="53c2a-181">hello second command mounts hello shared folder //CentOS66HN/Namd/namd2 onto hello folder with dir_mode and file_mode bits set too777.</span></span> <span data-ttu-id="53c2a-182">Hello *uživatelské jméno* a *heslo* hello příkazu musí být hello přihlašovací údaje uživatele hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-182">hello *username* and *password* in hello command should be hello credentials of a user on hello head node.</span></span>

> [!NOTE]
> <span data-ttu-id="53c2a-183">Hello "\\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="53c2a-183">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="53c2a-184">"\\`,"rozumí hello"," (čárku) je součástí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-184">“\\`,” means hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

## <a name="create-a-bash-script-toorun-a-namd-job"></a><span data-ttu-id="53c2a-185">Vytvořit úlohu NAMD toorun Bash skriptu</span><span class="sxs-lookup"><span data-stu-id="53c2a-185">Create a Bash script toorun a NAMD job</span></span>
<span data-ttu-id="53c2a-186">Potřebuje vaše úlohy NAMD *seznamu* souboru **charmrun** toodetermine hello počet uzlů toouse při spouštění NAMD procesy.</span><span class="sxs-lookup"><span data-stu-id="53c2a-186">Your NAMD job needs a *nodelist* file for **charmrun** toodetermine hello number of nodes toouse when starting NAMD processes.</span></span> <span data-ttu-id="53c2a-187">Použít skript Bash, který generuje souboru seznamu hello a spouští **charmrun** s tohoto souboru seznamu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-187">You use a Bash script that generates hello nodelist file and runs **charmrun** with this nodelist file.</span></span> <span data-ttu-id="53c2a-188">Potom můžete odeslat úlohu NAMD ve Správci clusteru HPC který volá tento skript.</span><span class="sxs-lookup"><span data-stu-id="53c2a-188">You can then submit a NAMD job in HPC Cluster Manager that calls this script.</span></span>

<span data-ttu-id="53c2a-189">Pomocí textového editoru podle vaší volby, vytvořte skript Bash ve složce /namd2 hello obsahující soubory programu hello NAMD a pojmenujte ji hpccharmrun.sh. Pro rychlé testování konceptu, zkopírujte skript hpccharmrun.sh příklad hello uvedených v hello konci tohoto článku, přejděte příliš[odeslat úlohu NAMD](#submit-a-namd-job).</span><span class="sxs-lookup"><span data-stu-id="53c2a-189">Using a text editor of your choice, create a Bash script in hello /namd2 folder containing hello NAMD program files and name it hpccharmrun.sh. For a quick proof of concept, copy hello example hpccharmrun.sh script provided at hello end of this article and go too[Submit a NAMD job](#submit-a-namd-job).</span></span>

> [!TIP]
> <span data-ttu-id="53c2a-190">Uložte skript jako textový soubor s Linuxem řádek zakončení (pouze LF, není CR LF).</span><span class="sxs-lookup"><span data-stu-id="53c2a-190">Save your script as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="53c2a-191">To zajišťuje, že ho běží správně na uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-191">This ensures that it runs properly on hello Linux nodes.</span></span>
> 
> 

<span data-ttu-id="53c2a-192">Následují informace o jaké tento skript bash.</span><span class="sxs-lookup"><span data-stu-id="53c2a-192">Following are details about what this bash script does.</span></span> 

1. <span data-ttu-id="53c2a-193">Definujte některé proměnné.</span><span class="sxs-lookup"><span data-stu-id="53c2a-193">Define some variables.</span></span>
   
   ```bash
   #!/bin/bash
   
   # hello path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. <span data-ttu-id="53c2a-194">Získáte informace o uzlu ze hello proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="53c2a-194">Get node information from hello environment variables.</span></span> <span data-ttu-id="53c2a-195">$NODESCORES ukládá seznam rozdělení slov z $CCP_NODES_CORES.</span><span class="sxs-lookup"><span data-stu-id="53c2a-195">$NODESCORES stores a list of split words from $CCP_NODES_CORES.</span></span> <span data-ttu-id="53c2a-196">$COUNT je hello velikost $NODESCORES.</span><span class="sxs-lookup"><span data-stu-id="53c2a-196">$COUNT is hello size of $NODESCORES.</span></span>
   
   ```
   # Get node information from hello environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   <span data-ttu-id="53c2a-197">Formát Hello hello $CCP_NODES_CORES proměnné je následující:</span><span class="sxs-lookup"><span data-stu-id="53c2a-197">hello format for hello $CCP_NODES_CORES variable is as follows:</span></span>
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   <span data-ttu-id="53c2a-198">Tato proměnná uvádí hello celkový počet uzlů, názvy a počet jader na každém uzlu, které jsou přiděleny toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="53c2a-198">This variable lists hello total number of nodes, node names, and number of cores on each node that are allocated toohello job.</span></span> <span data-ttu-id="53c2a-199">Například pokud úloha hello potřebuje 10 toorun jader, hello $CCP_NODES_CORES hodnotu podobně jako:</span><span class="sxs-lookup"><span data-stu-id="53c2a-199">For example, if hello job needs 10 cores toorun, hello value of $CCP_NODES_CORES is similar to:</span></span>
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. <span data-ttu-id="53c2a-200">Pokud hello $CCP_NODES_CORES proměnná není nastavena, spusťte **charmrun** přímo.</span><span class="sxs-lookup"><span data-stu-id="53c2a-200">If hello $CCP_NODES_CORES variable is not set, start **charmrun** directly.</span></span> <span data-ttu-id="53c2a-201">(To by mělo pouze nastat při spuštění tohoto skriptu přímo na Linuxových uzlů.)</span><span class="sxs-lookup"><span data-stu-id="53c2a-201">(This should only occur when you run this script directly on your Linux nodes.)</span></span>
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. <span data-ttu-id="53c2a-202">Nebo vytvořte souboru seznamu pro **charmrun**.</span><span class="sxs-lookup"><span data-stu-id="53c2a-202">Or create a nodelist file for **charmrun**.</span></span>
   
   ```
   else
     # Create hello nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write hello head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into hello nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. <span data-ttu-id="53c2a-203">Spustit **charmrun** s hello souboru seznamu, získat návratový stav a odeberte hello souboru seznamu na konci hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-203">Run **charmrun** with hello nodelist file, get its return status, and remove hello nodelist file at hello end.</span></span>
   
   <span data-ttu-id="53c2a-204">${CCP_NUMCPUS} je jiné proměnné prostředí, která nastavuje hlavního uzlu HPC Pack hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-204">${CCP_NUMCPUS} is another environment variable set by hello HPC Pack head node.</span></span> <span data-ttu-id="53c2a-205">Ukládají se hello počet celkový počet jader přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="53c2a-205">It stores hello number of total cores allocated toothis job.</span></span> <span data-ttu-id="53c2a-206">Budeme používat toospecify hello počet procesů charmrun.</span><span class="sxs-lookup"><span data-stu-id="53c2a-206">We use it toospecify hello number of processes for charmrun.</span></span>
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. <span data-ttu-id="53c2a-207">Ukončení s hello **charmrun** návratový stav.</span><span class="sxs-lookup"><span data-stu-id="53c2a-207">Exit with hello **charmrun** return status.</span></span>
   
   ```
   exit ${RTNSTS}
   ```

<span data-ttu-id="53c2a-208">Toto je hello informace v souboru seznamu hello, které hello skript generuje:</span><span class="sxs-lookup"><span data-stu-id="53c2a-208">Following is hello information in hello nodelist file, which hello script generates:</span></span>

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

<span data-ttu-id="53c2a-209">Například:</span><span class="sxs-lookup"><span data-stu-id="53c2a-209">For example:</span></span>

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a><span data-ttu-id="53c2a-210">Odeslání úlohy NAMD</span><span class="sxs-lookup"><span data-stu-id="53c2a-210">Submit a NAMD job</span></span>
<span data-ttu-id="53c2a-211">Teď je připraven toosubmit úlohu NAMD ve Správci clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="53c2a-211">Now you are ready toosubmit a NAMD job in HPC Cluster Manager.</span></span>

1. <span data-ttu-id="53c2a-212">Připojit tooyour hlavního uzlu clusteru a spusťte Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="53c2a-212">Connect tooyour cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="53c2a-213">V **Správa prostředků**, zajistěte, aby hello Linux výpočetní uzly v hello **Online** stavu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-213">In **Resource Management**, ensure that hello Linux compute nodes are in hello **Online** state.</span></span> <span data-ttu-id="53c2a-214">Pokud tomu tak není, vyberte je a klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="53c2a-214">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="53c2a-215">V **úlohy správy**, klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="53c2a-215">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="53c2a-216">Zadejte název úlohy, jako *hpccharmrun*.</span><span class="sxs-lookup"><span data-stu-id="53c2a-216">Enter a name for job such as *hpccharmrun*.</span></span>
   
   ![Nové úlohy HPC][namd_job]
5. <span data-ttu-id="53c2a-218">Na hello **podrobnosti úlohy** v části **úlohy prostředky**, vyberte typ prostředku jako hello **uzlu** a sadu hello **minimální** too3.</span><span class="sxs-lookup"><span data-stu-id="53c2a-218">On hello **Job Details** page, under **Job Resources**, select hello type of resource as **Node** and set hello **Minimum** too3.</span></span> <span data-ttu-id="53c2a-219">, jsme spustit úlohu hello na tři uzly Linux a každý uzel má čtyři jádra.</span><span class="sxs-lookup"><span data-stu-id="53c2a-219">, we run hello job on three Linux nodes and each node has four cores.</span></span>
   
   ![Úloha prostředky][job_resources]
6. <span data-ttu-id="53c2a-221">Klikněte na tlačítko **upravit úlohy** v hello levé navigaci a potom klikněte na **přidat** tooadd úlohu toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="53c2a-221">Click **Edit Tasks** in hello left navigation, and then click **Add** tooadd a task toohello job.</span></span>    
7. <span data-ttu-id="53c2a-222">Na hello **podrobností úlohy a přesměrování vstupu a výstupu** stránku hello sadu následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="53c2a-222">On hello **Task Details and I/O Redirection** page, set hello following values:</span></span>
   
   * <span data-ttu-id="53c2a-223">**Příkazový řádek**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span><span class="sxs-lookup"><span data-stu-id="53c2a-223">**Command line** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span></span>
     
     > [!TIP]
     > <span data-ttu-id="53c2a-224">Hello předchozí příkazový řádek je jeden příkaz bez konce řádků.</span><span class="sxs-lookup"><span data-stu-id="53c2a-224">hello preceding command line is a single command without line breaks.</span></span> <span data-ttu-id="53c2a-225">Ho zabalí tooappear na několika řádcích pod **příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="53c2a-225">It wraps tooappear on several lines under **Command line**.</span></span>
     > 
     > 
   * <span data-ttu-id="53c2a-226">**Pracovní adresář** -/namd2</span><span class="sxs-lookup"><span data-stu-id="53c2a-226">**Working directory** - /namd2</span></span>
   * <span data-ttu-id="53c2a-227">**Minimální** – 3</span><span class="sxs-lookup"><span data-stu-id="53c2a-227">**Minimum** - 3</span></span>
     
     ![Podrobnosti úlohy][task_details]
     
     > [!NOTE]
     > <span data-ttu-id="53c2a-229">Hello pracovní adresář tady nastavíte protože **charmrun** pokusí toonavigate toohello stejný pracovní adresář na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-229">You set hello working directory here because **charmrun** tries toonavigate toohello same working directory on each node.</span></span> <span data-ttu-id="53c2a-230">Pokud není nastavena hello pracovní adresář, HPC Pack spustí příkaz hello náhodným názvem složky vytvořené v jednom z uzlů Linux hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-230">If hello working directory isn't set, HPC Pack starts hello command in a randomly named folder created on one of hello Linux nodes.</span></span> <span data-ttu-id="53c2a-231">To způsobí, že hello následující chyba na hello dalších uzlů: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid tyto potíže odstranit, zadejte cestu ke složce, která je přístupná ve všech uzlech jako hello pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="53c2a-231">This causes hello following error on hello other nodes: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid this problem, specify a folder path that can be accessed by all nodes as hello working directory.</span></span>
     > 
     > 
8. <span data-ttu-id="53c2a-232">Klikněte na tlačítko **OK** a pak klikněte na **odeslání** toorun tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="53c2a-232">Click **OK** and then click **Submit** toorun this job.</span></span>
   
   <span data-ttu-id="53c2a-233">Ve výchozím nastavení odešle HPC Pack úlohy hello jako váš aktuální účet přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="53c2a-233">By default, HPC Pack submits hello job as your current logged-on user account.</span></span> <span data-ttu-id="53c2a-234">Dialogové okno mohou vyžadovat tooenter hello uživatelské jméno a heslo po kliknutí na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="53c2a-234">A dialog box might prompt you tooenter hello user name and password after you click **Submit**.</span></span>
   
   ![Přihlašovací údaje úlohy][creds]
   
   <span data-ttu-id="53c2a-236">Za určitých podmínek pamatuje HPC Pack hello vstupní před a nezobrazí toto dialogové okno informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="53c2a-236">Under some conditions, HPC Pack remembers hello user information you input before and doesn't show this dialog box.</span></span> <span data-ttu-id="53c2a-237">toomake HPC Pack jej znovu zobrazit, zadejte následující příkaz na příkazovém řádku hello a pak odeslat úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-237">toomake HPC Pack show it again, enter hello following command at a Command Prompt and then submit hello job.</span></span>
   
   ```command
   hpccred delcreds
   ```
9. <span data-ttu-id="53c2a-238">Úloha Hello trvá několik minut toofinish.</span><span class="sxs-lookup"><span data-stu-id="53c2a-238">hello job takes several minutes toofinish.</span></span>
10. <span data-ttu-id="53c2a-239">Najít protokol hello úlohy v \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log a hello výstupní soubory v \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span><span class="sxs-lookup"><span data-stu-id="53c2a-239">Find hello job log at \\<headnodeName>\Namd\namd2\namd2_hpccharmrun.log and hello output files in \\<headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span></span>
11. <span data-ttu-id="53c2a-240">Volitelně můžete spusťte VMD tooview výsledky úlohy.</span><span class="sxs-lookup"><span data-stu-id="53c2a-240">Optionally, start VMD tooview your job results.</span></span> <span data-ttu-id="53c2a-241">Hello kroky pro vizualizaci hello NAMD výstupní soubory (v tomto případě molekuly bílkovin ubiquitin v oblasti horních) jsou nad rámec tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="53c2a-241">hello steps for visualizing hello NAMD output files (in this case, a ubiquitin protein molecule in a water sphere) are beyond hello scope of this article.</span></span> <span data-ttu-id="53c2a-242">V tématu [NAMD kurzu](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="53c2a-242">See [NAMD Tutorial](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) for details.</span></span>
    
    ![Výsledky úlohy][vmd_view]

## <a name="sample-files"></a><span data-ttu-id="53c2a-244">Ukázkové soubory</span><span class="sxs-lookup"><span data-stu-id="53c2a-244">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="53c2a-245">Ukázkový soubor XML konfigurace pro nasazení clusteru pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="53c2a-245">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
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

### <a name="sample-credxml-file"></a><span data-ttu-id="53c2a-246">Ukázkový soubor cred.xml</span><span class="sxs-lookup"><span data-stu-id="53c2a-246">Sample cred.xml file</span></span>
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

### <a name="sample-hpccharmrunsh-script"></a><span data-ttu-id="53c2a-247">Ukázkový skript hpccharmrun.sh</span><span class="sxs-lookup"><span data-stu-id="53c2a-247">Sample hpccharmrun.sh script</span></span>
```
#!/bin/bash

# hello path of this script
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
    # If CCP_NODES_CORES is not found or is empty, just run hello charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create hello nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write hello head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into hello nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello charmrun with nodelist arg
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
