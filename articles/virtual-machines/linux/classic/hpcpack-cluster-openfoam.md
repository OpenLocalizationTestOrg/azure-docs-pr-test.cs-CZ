---
title: "aaaRun OpenFOAM pomocí sady HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
description: "Nasazení clusteru s podporou sady Microsoft HPC Pack v Azure a spusťte úlohu OpenFOAM v několika výpočetních uzlech Linux přes sítě RDMA."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: 1a51ffe6804abb5156f01d580c9b7a4a9649377a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="693d5-103">Spuštění OpenFoam se sadou Microsoft HPC Pack v clusteru Linux RDMA v Azure</span><span class="sxs-lookup"><span data-stu-id="693d5-103">Run OpenFoam with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="693d5-104">Tento článek ukazuje jeden ze způsobů toorun OpenFoam ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="693d5-104">This article shows you one way toorun OpenFoam in Azure virtual machines.</span></span> <span data-ttu-id="693d5-105">V tomto nasazení clusteru Microsoft HPC Pack s Linux výpočetní uzly na Azure a spusťte [OpenFoam](http://openfoam.com/) úlohy s Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="693d5-105">Here, you deploy a Microsoft HPC Pack cluster with Linux compute nodes on Azure and run an [OpenFoam](http://openfoam.com/) job with Intel MPI.</span></span> <span data-ttu-id="693d5-106">RDMA podporovat virtuálních počítačích Azure můžete použít pro hello výpočetní uzly, tak, aby výpočetní uzly hello komunikovat přes síť Azure RDMA hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-106">You can use RDMA-capable Azure VMs for hello compute nodes, so that hello compute nodes communicate over hello Azure RDMA network.</span></span> <span data-ttu-id="693d5-107">Další možnosti toorun OpenFoam v Azure zahrnout plně nakonfigurované komerční bitové kopie k dispozici v hello Marketplace, jako je například na UberCloud [OpenFoam 2.3 na CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)a spuštěním na [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span><span class="sxs-lookup"><span data-stu-id="693d5-107">Other options toorun OpenFoam in Azure include fully configured commercial images available in hello Marketplace, such as UberCloud's [OpenFoam 2.3 on CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), and by running on [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="693d5-108">OpenFOAM (pro operaci otevřete pole a manipulace s) je balíček softwaru open source výpočet dynamiky kapaliny (CFD), který se často používá v inženýrství a vědecké účely, v organizacích komerční i academic.</span><span class="sxs-lookup"><span data-stu-id="693d5-108">OpenFOAM (for Open Field Operation and Manipulation) is an open-source computational fluid dynamics (CFD) software package that is used widely in engineering and science, in both commercial and academic organizations.</span></span> <span data-ttu-id="693d5-109">Obsahuje nástroje pro meshing, zejména snappyHexMesh, parallelized mesher pro komplexní geometrie CAD a před a po zpracování.</span><span class="sxs-lookup"><span data-stu-id="693d5-109">It includes tools for meshing, notably snappyHexMesh, a parallelized mesher for complex CAD geometries, and for pre- and post-processing.</span></span> <span data-ttu-id="693d5-110">Téměř všechny procesy spustit souběžně, povolíte uživatelům tootake všech výhod hardware počítače mají k dispozici.</span><span class="sxs-lookup"><span data-stu-id="693d5-110">Almost all processes run in parallel, enabling users tootake full advantage of computer hardware at their disposal.</span></span>  

<span data-ttu-id="693d5-111">Microsoft HPC Pack poskytuje funkce toorun rozsáhlé HPC a paralelní aplikace, včetně aplikací MPI, v clusterech virtuální počítače Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="693d5-111">Microsoft HPC Pack provides features toorun large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="693d5-112">HPC Pack také podporuje spuštěné Linux HPC aplikace v systému Linux výpočetního uzlu, které virtuální počítače jsou nasazené v clusteru služby HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="693d5-112">HPC Pack also supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="693d5-113">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) pro toousing Úvod Linuxových výpočetních uzlů pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="693d5-113">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction toousing Linux compute nodes with HPC Pack.</span></span>

> [!NOTE]
> <span data-ttu-id="693d5-114">Tento článek ukazuje, jak toorun úlohy Linux MPI pomocí sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="693d5-114">This article illustrates how toorun a Linux MPI workload with HPC Pack.</span></span> <span data-ttu-id="693d5-115">Předpokládá se, že máte některé znalosti s správu systému Linux a úlohy MPI systémem Linux clusterů.</span><span class="sxs-lookup"><span data-stu-id="693d5-115">It assumes you have some familiarity with Linux system administration and with running MPI workloads on Linux clusters.</span></span> <span data-ttu-id="693d5-116">Pokud používáte verze MPI a OpenFOAM liší od hello ty, které jsou uvedené v tomto článku, může být toomodify některé kroky instalace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="693d5-116">If you use versions of MPI and OpenFOAM different from hello ones shown in this article, you might have toomodify some installation and configuration steps.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="693d5-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="693d5-117">Prerequisites</span></span>
* <span data-ttu-id="693d5-118">**HPC Pack clusteru s podporou RDMA Linux výpočetní uzly** – nasazení clusteru HPC Pack velikosti A8, A9, H16r, nebo H16rm Linuxových výpočetních uzlů, buď pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) nebo [skript prostředí Azure PowerShell](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="693d5-118">**HPC Pack cluster with RDMA-capable Linux compute nodes** - Deploy an HPC Pack cluster with size A8, A9, H16r, or H16rm Linux compute nodes using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="693d5-119">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) hello požadavky a kroky pro jednu z možností.</span><span class="sxs-lookup"><span data-stu-id="693d5-119">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for hello prerequisites and steps for either option.</span></span> <span data-ttu-id="693d5-120">Pokud si zvolíte hello možnost nasazení skriptu prostředí PowerShell, najdete v části hello vzorový konfigurační soubor v hello ukázkových souborů v hello konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="693d5-120">If you choose hello PowerShell script deployment option, see hello sample configuration file in hello sample files at hello end of this article.</span></span> <span data-ttu-id="693d5-121">Pomocí této konfigurace clusteru HPC Pack toodeploy služby Azure na základě skládající se z hlavního uzlu velikosti A8 Windows Server 2012 R2 a 2 velikosti A8 SUSE Linux Enterprise Server 12 výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="693d5-121">Use this configuration toodeploy an Azure-based HPC Pack cluster consisting of a size A8 Windows Server 2012 R2 head node and 2 size A8 SUSE Linux Enterprise Server 12 compute nodes.</span></span> <span data-ttu-id="693d5-122">Nahraďte příslušnými hodnotami pro vaše předplatné a služby názvy.</span><span class="sxs-lookup"><span data-stu-id="693d5-122">Substitute appropriate values for your subscription and service names.</span></span> 
  
  <span data-ttu-id="693d5-123">**Další využití tooknow**</span><span class="sxs-lookup"><span data-stu-id="693d5-123">**Additional things tooknow**</span></span>
  
  * <span data-ttu-id="693d5-124">Linux RDMA síťové požadavky v Azure, najdete v části [vysokovýkonné výpočetní velikosti virtuálních počítačů](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="693d5-124">For Linux RDMA networking prerequisites in Azure, see [High performance compute VM sizes](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  * <span data-ttu-id="693d5-125">Pokud používáte hello možnost nasazení skriptu prostředí Powershell, nasaďte všechny hello Linux výpočetní uzly v rámci jednoho cloudu služby toouse hello RDMA síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="693d5-125">If you use hello Powershell script deployment option, deploy all hello Linux compute nodes within one cloud service toouse hello RDMA network connection.</span></span>
  * <span data-ttu-id="693d5-126">Po nasazení hello Linuxových uzlů, připojte pomocí SSH tooperform žádné další úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="693d5-126">After deploying hello Linux nodes, connect by SSH tooperform any additional administrative tasks.</span></span> <span data-ttu-id="693d5-127">Najít hello podrobnosti připojení SSH pro každý virtuální počítač s Linuxem v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="693d5-127">Find hello SSH connection details for each Linux VM in hello Azure portal.</span></span>  
* <span data-ttu-id="693d5-128">**Intel MPI** -toorun OpenFOAM na SLES 12 HPC výpočetních uzlech v Azure, musíte tooinstall hello Intel MPI knihovny 5 runtime z: hello [Intel.com lokality](https://software.intel.com/en-us/intel-mpi-library/).</span><span class="sxs-lookup"><span data-stu-id="693d5-128">**Intel MPI** - toorun OpenFOAM on SLES 12 HPC compute nodes in Azure, you need tooinstall hello Intel MPI Library 5 runtime from hello [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/).</span></span> <span data-ttu-id="693d5-129">(Intel MPI 5 je předinstalován v bitové kopie založené na CentOS HPC).  V pozdější fázi v případě potřeby nainstalujte Intel MPI na výpočetní uzly Linux.</span><span class="sxs-lookup"><span data-stu-id="693d5-129">(Intel MPI 5 is preinstalled on CentOS-based HPC images.)  In a later step, if necessary, install Intel MPI on your Linux compute nodes.</span></span> <span data-ttu-id="693d5-130">tooprepare pro tento krok po registraci pomocí Intel, postupujte podle hello odkaz hello potvrzení e-mailu toohello související webové stránce.</span><span class="sxs-lookup"><span data-stu-id="693d5-130">tooprepare for this step, after you register with Intel, follow hello link in hello confirmation email toohello related web page.</span></span> <span data-ttu-id="693d5-131">Potom kopie hello stáhnout odkaz hello .tgz soubor pro příslušnou verzi Intel MPI hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-131">Then, copy hello download link for hello .tgz file for hello appropriate version of Intel MPI.</span></span> <span data-ttu-id="693d5-132">Tento článek je založena na Intel MPI verze 5.0.3.048.</span><span class="sxs-lookup"><span data-stu-id="693d5-132">This article is based on Intel MPI version 5.0.3.048.</span></span>
* <span data-ttu-id="693d5-133">**OpenFOAM zdroj Pack** -stáhnout software hello OpenFOAM zdroj aktualizací Service Pack pro Linux z hello [OpenFOAM Foundation lokality](http://openfoam.org/download/2-3-1-source/).</span><span class="sxs-lookup"><span data-stu-id="693d5-133">**OpenFOAM Source Pack** - Download hello OpenFOAM Source Pack software for Linux from hello [OpenFOAM Foundation site](http://openfoam.org/download/2-3-1-source/).</span></span> <span data-ttu-id="693d5-134">Tento článek je založen na verzi zdroje Pack 2.3.1, k dispozici ke stažení OpenFOAM 2.3.1.tgz.</span><span class="sxs-lookup"><span data-stu-id="693d5-134">This article is based on Source Pack version 2.3.1, available for download as OpenFOAM-2.3.1.tgz.</span></span> <span data-ttu-id="693d5-135">Postupujte podle pokynů hello později v této toounpack článku a kompilace OpenFOAM hello Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="693d5-135">Follow hello instructions later in this article toounpack and compile OpenFOAM on hello Linux compute nodes.</span></span>
* <span data-ttu-id="693d5-136">**EnSight** (volitelné) – výsledky hello toosee OpenFOAM simulace, stáhněte a nainstalujte hello [EnSight](https://www.ceisoftware.com/download/) program vizualizaci a analýzu.</span><span class="sxs-lookup"><span data-stu-id="693d5-136">**EnSight** (optional) - toosee hello results of your OpenFOAM simulation, download and install hello [EnSight](https://www.ceisoftware.com/download/) visualization and analysis program.</span></span> <span data-ttu-id="693d5-137">Informace o licencování a stažení jsou v lokalitě EnSight hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-137">Licensing and download information are at hello EnSight site.</span></span>

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="693d5-138">Nastavení vzájemného vztahu důvěryhodnosti mezi výpočetní uzly</span><span class="sxs-lookup"><span data-stu-id="693d5-138">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="693d5-139">Spuštění úlohy mezi uzly ve více uzlech Linux vyžaduje hello uzly tootrust, vzájemně (podle **rsh** nebo **ssh**).</span><span class="sxs-lookup"><span data-stu-id="693d5-139">Running a cross-node job on multiple Linux nodes requires hello nodes tootrust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="693d5-140">Při vytváření clusteru HPC Pack hello s hello skript nasazení Microsoft HPC Pack IaaS, hello skript automaticky nastaví trvalé vzájemné důvěryhodnosti hello správce účtu, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="693d5-140">When you create hello HPC Pack cluster with hello Microsoft HPC Pack IaaS deployment script, hello script automatically sets up permanent mutual trust for hello administrator account you specify.</span></span> <span data-ttu-id="693d5-141">Pro uživatele bez oprávnění správce, které vytvoříte v doméně hello clusteru máte tooset až dočasné vzájemné vztah důvěryhodnosti mezi uzly hello při úlohy je přidělen toothem a odstraňte hello relace po dokončení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-141">For non-administrator users you create in hello cluster's domain, you have tooset up temporary mutual trust among hello nodes when a job is allocated toothem, and destroy hello relationship after hello job is complete.</span></span> <span data-ttu-id="693d5-142">tooestablish důvěryhodnosti pro každého uživatele, zadejte RSA páru klíčů toohello clusteru, který HPC Pack používá pro vztah důvěryhodnosti hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-142">tooestablish trust for each user, provide an RSA key pair toohello cluster that HPC Pack uses for hello trust relationship.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="693d5-143">Vygenerovat pár klíče RSA</span><span class="sxs-lookup"><span data-stu-id="693d5-143">Generate an RSA key pair</span></span>
<span data-ttu-id="693d5-144">Je snadno toogenerate pár klíče RSA, který obsahuje veřejný klíč a soukromý klíč, spuštěním hello Linux **ssh-keygen** příkaz.</span><span class="sxs-lookup"><span data-stu-id="693d5-144">It's easy toogenerate an RSA key pair, which contains a public key and a private key, by running hello Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="693d5-145">Přihlaste se tooa počítač se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="693d5-145">Log on tooa Linux computer.</span></span>
2. <span data-ttu-id="693d5-146">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="693d5-146">Run hello following command:</span></span>
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="693d5-147">Stiskněte klávesu **Enter** toouse hello výchozí nastavení až do dokončení příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-147">Press **Enter** toouse hello default settings until hello command is completed.</span></span> <span data-ttu-id="693d5-148">Nezadávejte přístupové heslo zde; Po zobrazení výzvy k zadání hesla, stačí stisknout klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="693d5-148">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Vygenerovat pár klíče RSA][keygen]
3. <span data-ttu-id="693d5-150">Změňte adresář toohello ~/.ssh adresář.</span><span class="sxs-lookup"><span data-stu-id="693d5-150">Change directory toohello ~/.ssh directory.</span></span> <span data-ttu-id="693d5-151">Hello privátní klíč uložen v id_rsa a hello veřejný klíč v id_rsa.pub.</span><span class="sxs-lookup"><span data-stu-id="693d5-151">hello private key is stored in id_rsa and hello public key in id_rsa.pub.</span></span>
   
   ![Privátní a veřejné klíče][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a><span data-ttu-id="693d5-153">Přidejte cluster HPC Pack toohello páru klíčů hello</span><span class="sxs-lookup"><span data-stu-id="693d5-153">Add hello key pair toohello HPC Pack cluster</span></span>
1. <span data-ttu-id="693d5-154">Zkontrolujte vzdálené plochy připojení tooyour hlavního uzlu pomocí účtu správce HPC Pack (hello účet správce, které můžete nastavit při spuštění skriptu nasazení hello).</span><span class="sxs-lookup"><span data-stu-id="693d5-154">Make a Remote Desktop connection tooyour head node with your HPC Pack administrator account (hello administrator account you set up when you ran hello deployment script).</span></span>
2. <span data-ttu-id="693d5-155">Použijte standardní systému Windows Server postupy toocreate účet uživatele domény v doméně služby Active Directory hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="693d5-155">Use standard Windows Server procedures toocreate a domain user account in hello cluster's Active Directory domain.</span></span> <span data-ttu-id="693d5-156">Například použijte nástroj počítače a hello uživatele služby Active Directory hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="693d5-156">For example, use hello Active Directory User and Computers tool on hello head node.</span></span> <span data-ttu-id="693d5-157">Hello příklady v tomto článku předpokládá, že vytvoříte uživatele domény s názvem hpclab\hpcuser.</span><span class="sxs-lookup"><span data-stu-id="693d5-157">hello examples in this article assume you create a domain user named hpclab\hpcuser.</span></span>
3. <span data-ttu-id="693d5-158">Vytvořte soubor s názvem C:\cred.xml a zkopírujte hello RSA klíčová data do ní.</span><span class="sxs-lookup"><span data-stu-id="693d5-158">Create a file named C:\cred.xml and copy hello RSA key data into it.</span></span> <span data-ttu-id="693d5-159">Ukázkový soubor cred.xml je na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="693d5-159">A sample cred.xml file is at hello end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. <span data-ttu-id="693d5-160">Otevřete příkazový řádek a zadejte následující příkaz údaje tooset hello data pro účet hpclab\hpcuser hello hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-160">Open a Command Prompt and enter hello following command tooset hello credentials data for hello hpclab\hpcuser account.</span></span> <span data-ttu-id="693d5-161">Použít hello **extendeddata** toopass parametr hello název souboru C:\cred.xml jste vytvořili pro hello klíčová data.</span><span class="sxs-lookup"><span data-stu-id="693d5-161">You use hello **extendeddata** parameter toopass hello name of C:\cred.xml file you created for hello key data.</span></span>
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="693d5-162">Tento příkaz se dokončí úspěšně bez výstupu.</span><span class="sxs-lookup"><span data-stu-id="693d5-162">This command completes successfully without output.</span></span> <span data-ttu-id="693d5-163">Po nastavení hello přihlašovací údaje pro hello uživatelské účty, které že budete potřebovat toorun úlohy, hello cred.xml soubor uložte na bezpečném místě, nebo ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="693d5-163">After setting hello credentials for hello user accounts you need toorun jobs, store hello cred.xml file in a secure location, or delete it.</span></span>
5. <span data-ttu-id="693d5-164">Pokud jste vygenerovali hello páru klíčů RSA na jednom z uzlů Linux, mějte na paměti klíče hello toodelete po dokončení jejich používání.</span><span class="sxs-lookup"><span data-stu-id="693d5-164">If you generated hello RSA key pair on one of your Linux nodes, remember toodelete hello keys after you finish using them.</span></span> <span data-ttu-id="693d5-165">Pokud HPC Pack najde existující id_rsa soubor nebo soubor id_rsa.pub, nenastaví až vzájemné vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="693d5-165">If HPC Pack finds an existing id_rsa file or id_rsa.pub file, it does not set up mutual trust.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="693d5-166">Nedoporučujeme spuštěna úlohu Linux jako správce clusteru na sdílený clusteru, protože úlohu odeslání správcem spouští hello kořenového účtu na uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-166">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under hello root account on hello Linux nodes.</span></span> <span data-ttu-id="693d5-167">Však úloha odeslána uživatel není správcem spouští pod místním uživatelským účtem Linux s hello stejný název jako uživatel úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-167">However, a job submitted by a non-administrator user runs under a local Linux user account with hello same name as hello job user.</span></span> <span data-ttu-id="693d5-168">V takovém případě HPC Pack nastaví vzájemné vztahu důvěryhodnosti pro tohoto uživatele Linux mezi uzly hello přidělené toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-168">In this case, HPC Pack sets up mutual trust for this Linux user across hello nodes allocated toohello job.</span></span> <span data-ttu-id="693d5-169">Můžete nastavit uživatele Linux hello ručně na hello Linuxové uzly před spuštěním úlohy hello nebo HPC Pack hello uživatele automaticky vytvoří při odeslání úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-169">You can set up hello Linux user manually on hello Linux nodes before running hello job, or HPC Pack creates hello user automatically when hello job is submitted.</span></span> <span data-ttu-id="693d5-170">Pokud HPC Pack vytvoří hello uživatele, HPC Pack neodstraní po dokončení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-170">If HPC Pack creates hello user, HPC Pack deletes it after hello job completes.</span></span> <span data-ttu-id="693d5-171">tooreduce bezpečnostní hrozby HPC Pack odebere hello klíče po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-171">tooreduce security threats, HPC Pack removes hello keys after job completion.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="693d5-172">Nastavení sdílené složky pro Linuxové uzly</span><span class="sxs-lookup"><span data-stu-id="693d5-172">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="693d5-173">Nyní nastavte standardní sdílené složky protokolu SMB ve složce hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="693d5-173">Now set up a standard SMB share on a folder on hello head node.</span></span> <span data-ttu-id="693d5-174">tooallow hello Linux uzly tooaccess soubory aplikace pomocí běžných cestu, hello připojení sdílené složky na uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-174">tooallow hello Linux nodes tooaccess application files with a common path, mount hello shared folder on hello Linux nodes.</span></span> <span data-ttu-id="693d5-175">Pokud chcete, můžete použít jinou možnost, jako je například Azure Files sdílená složka – doporučené pro mnoho scénářů - nebo sdílené složky NFS pro sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="693d5-175">If you want, you can use another file sharing option, such as an Azure Files share - recommended for many scenarios - or an NFS share.</span></span> <span data-ttu-id="693d5-176">Naleznete v souboru hello sdílení informací a podrobné pokyny v [začít pracovat s Linux výpočetní uzly v clusteru HPC Pack v Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="693d5-176">See hello file sharing information and detailed steps in [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="693d5-177">Vytvořte složku hello hlavního uzlu a sdílejte ji tooEveryone nastavením oprávnění pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="693d5-177">Create a folder on hello head node, and share it tooEveryone by setting Read/Write privileges.</span></span> <span data-ttu-id="693d5-178">Například sdílet C:\OpenFOAM hello hlavního uzlu jako \\ \\SUSE12RDMA HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="693d5-178">For example, share C:\OpenFOAM on hello head node as \\\\SUSE12RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="693d5-179">Zde *SUSE12RDMA HN* je název hostitele hello hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="693d5-179">Here, *SUSE12RDMA-HN* is hello host name of hello head node.</span></span>
2. <span data-ttu-id="693d5-180">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="693d5-180">Open a Windows PowerShell window and run hello following commands:</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

<span data-ttu-id="693d5-181">První příkaz Hello vytvoří složku s názvem /openfoam ve všech uzlech v hello LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="693d5-181">hello first command creates a folder named /openfoam on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="693d5-182">druhý příkaz Hello připojí //SUSE12RDMA-HN/OpenFOAM hello sdílené složky na uzlech hello Linux s dir_mode a file_mode too777 sadu bitů.</span><span class="sxs-lookup"><span data-stu-id="693d5-182">hello second command mounts hello shared folder //SUSE12RDMA-HN/OpenFOAM on hello Linux nodes with dir_mode and file_mode bits set too777.</span></span> <span data-ttu-id="693d5-183">Hello *uživatelské jméno* a *heslo* hello příkazu musí být hello přihlašovací údaje uživatele hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="693d5-183">hello *username* and *password* in hello command should be hello credentials of a user on hello head node.</span></span>

> [!NOTE]
> <span data-ttu-id="693d5-184">Hello "\\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="693d5-184">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="693d5-185">"\\`,"rozumí hello"," (čárku) je součástí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-185">“\\`,” means hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

## <a name="install-mpi-and-openfoam"></a><span data-ttu-id="693d5-186">Instalace MPI a OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="693d5-186">Install MPI and OpenFOAM</span></span>
<span data-ttu-id="693d5-187">toorun OpenFOAM jako úloha MPI v síti hello RDMA, musíte toocompile OpenFOAM s knihovnami Intel MPI hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-187">toorun OpenFOAM as an MPI job on hello RDMA network, you need toocompile OpenFOAM with hello Intel MPI libraries.</span></span> 

<span data-ttu-id="693d5-188">Nejprve spustit několik **clusrun** příkazy tooinstall Intel MPI knihovny (pokud ještě není nainstalovaná) a OpenFOAM na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="693d5-188">First, run several **clusrun** commands tooinstall Intel MPI libraries (if not already installed) and OpenFOAM on your Linux nodes.</span></span> <span data-ttu-id="693d5-189">Použití hello hlavního uzlu sdílení nakonfigurovali dříve tooshare hello instalačních souborů mezi uzly Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-189">Use hello head node share configured previously tooshare hello installation files among hello Linux nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="693d5-190">Tato instalace a kompilace kroky jsou příklady.</span><span class="sxs-lookup"><span data-stu-id="693d5-190">These installation and compiling steps are examples.</span></span> <span data-ttu-id="693d5-191">Budete potřebovat některé znalosti tooensure správy systému Linux správně nainstalován závislý kompilátory a knihovny.</span><span class="sxs-lookup"><span data-stu-id="693d5-191">You need some knowledge of Linux system administration tooensure that dependent compilers and libraries are installed correctly.</span></span> <span data-ttu-id="693d5-192">Vaše verze Intel MPI a OpenFOAM bude pravděpodobně nutné toomodify určité proměnné prostředí nebo jiná nastavení.</span><span class="sxs-lookup"><span data-stu-id="693d5-192">You might need toomodify certain environment variables or other settings for your versions of Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="693d5-193">Podrobnosti najdete v tématu [Intel MPI knihovny pro Průvodce instalací Linux](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) a [instalace sady zdroj OpenFOAM](http://openfoam.org/download/2-3-1-source/) pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="693d5-193">For details, see [Intel MPI Library for Linux Installation Guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) and [OpenFOAM Source Pack Installation](http://openfoam.org/download/2-3-1-source/) for your environment.</span></span>
> 
> 

### <a name="install-intel-mpi"></a><span data-ttu-id="693d5-194">Nainstalujte Intel MPI</span><span class="sxs-lookup"><span data-stu-id="693d5-194">Install Intel MPI</span></span>
<span data-ttu-id="693d5-195">Uložte hello stažený instalační balíček pro Intel MPI (l_mpi_p_5.0.3.048.tgz v tomto příkladu) v C:\OpenFoam hello hlavního uzlu tak, aby uzly Linux hello přístup z /openfoam tento soubor.</span><span class="sxs-lookup"><span data-stu-id="693d5-195">Save hello downloaded installation package for Intel MPI (l_mpi_p_5.0.3.048.tgz in this example) in C:\OpenFoam on hello head node so that hello Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="693d5-196">Spusťte **clusrun** tooinstall Intel MPI knihovny na všech uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-196">Then run **clusrun** tooinstall Intel MPI library on all hello Linux nodes.</span></span>

1. <span data-ttu-id="693d5-197">Hello následující příkazy kopírování hello instalační balíček a rozbalte ho příliš/opt/intel na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="693d5-197">hello following commands copy hello installation package and extract it too/opt/intel on each node.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. <span data-ttu-id="693d5-198">tooinstall Intel MPI knihovny tiše, použijte soubor silent.cfg.</span><span class="sxs-lookup"><span data-stu-id="693d5-198">tooinstall Intel MPI Library silently, use a silent.cfg file.</span></span> <span data-ttu-id="693d5-199">Příklad můžete najít v hello ukázkových souborů v hello konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="693d5-199">You can find an example in hello sample files at hello end of this article.</span></span> <span data-ttu-id="693d5-200">Tento soubor hello místní sdílené složky /openfoam.</span><span class="sxs-lookup"><span data-stu-id="693d5-200">Place this file in hello shared folder /openfoam.</span></span> <span data-ttu-id="693d5-201">Podrobnosti o hello silent.cfg souboru najdete v tématu [Intel MPI knihovny pro Průvodce instalací Linux - tichou instalaci](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span><span class="sxs-lookup"><span data-stu-id="693d5-201">For details about hello silent.cfg file, see [Intel MPI Library for Linux Installation Guide - Silent Installation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="693d5-202">Ujistěte se, že můžete uložit vaše silent.cfg soubor jako textový soubor s Linux konce řádků, (pouze LF, není CR LF).</span><span class="sxs-lookup"><span data-stu-id="693d5-202">Make sure that you save your silent.cfg file as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="693d5-203">Tento krok zajistí, že správně běží na uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-203">This step ensures that it runs properly on hello Linux nodes.</span></span>
   > 
   > 
3. <span data-ttu-id="693d5-204">Knihovna MPI Intel instalaci v bezobslužném režimu.</span><span class="sxs-lookup"><span data-stu-id="693d5-204">Install Intel MPI Library in silent mode.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a><span data-ttu-id="693d5-205">Konfigurace MPI</span><span class="sxs-lookup"><span data-stu-id="693d5-205">Configure MPI</span></span>
<span data-ttu-id="693d5-206">Pro testování, měli byste přidat hello následující řádky toohello /etc/security/limits.conf na každém uzlu hello Linux:</span><span class="sxs-lookup"><span data-stu-id="693d5-206">For testing, you should add hello following lines toohello /etc/security/limits.conf on each of hello Linux nodes:</span></span>

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


<span data-ttu-id="693d5-207">Po aktualizaci souboru limits.conf hello, restartujte hello Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="693d5-207">Restart hello Linux nodes after you update hello limits.conf file.</span></span> <span data-ttu-id="693d5-208">Například použijte hello **clusrun** příkaz:</span><span class="sxs-lookup"><span data-stu-id="693d5-208">For example, use hello following **clusrun** command:</span></span>

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

<span data-ttu-id="693d5-209">Po restartování počítače, zkontrolujte, zda že je této sdílené složce hello připojit jako /openfoam.</span><span class="sxs-lookup"><span data-stu-id="693d5-209">After restarting, ensure that hello shared folder is mounted as /openfoam.</span></span>

### <a name="compile-and-install-openfoam"></a><span data-ttu-id="693d5-210">Kompilace a nainstalujte OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="693d5-210">Compile and install OpenFOAM</span></span>
<span data-ttu-id="693d5-211">Uložte stažený instalační balíček hello hello tooC:\OpenFoam OpenFOAM zdroj aktualizací Service Pack (OpenFOAM-2.3.1.tgz v tomto příkladu) hello hlavního uzlu, aby uzly Linux hello přístup z /openfoam tento soubor.</span><span class="sxs-lookup"><span data-stu-id="693d5-211">Save hello downloaded installation package for hello OpenFOAM Source Pack (OpenFOAM-2.3.1.tgz in this example) tooC:\OpenFoam on hello head node so that hello Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="693d5-212">Spusťte **clusrun** příkazy hello toocompile OpenFOAM na všech uzlech Linux.</span><span class="sxs-lookup"><span data-stu-id="693d5-212">Then run **clusrun** commands toocompile OpenFOAM on all hello Linux nodes.</span></span>

1. <span data-ttu-id="693d5-213">Vytvořte složku /opt/OpenFOAM na každý uzel, Linux, kopírovat hello zdroje balíčku toothis složku a rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="693d5-213">Create a folder /opt/OpenFOAM on each Linux node, copy hello source package toothis folder, and extract it there.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. <span data-ttu-id="693d5-214">toocompile OpenFOAM s hello knihovně MPI Intel, nejprve nastavte některé proměnné prostředí pro Intel MPI i OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="693d5-214">toocompile OpenFOAM with hello Intel MPI Library, first set up some environment variables for both Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="693d5-215">Pomocí skriptu bash nazývané settings.sh tooset hello proměnné.</span><span class="sxs-lookup"><span data-stu-id="693d5-215">Use a bash script called settings.sh tooset hello variables.</span></span> <span data-ttu-id="693d5-216">Příklad můžete najít v hello ukázkových souborů v hello konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="693d5-216">You can find an example in hello sample files at hello end of this article.</span></span> <span data-ttu-id="693d5-217">Místní tohoto souboru (Uložit s konce řádků Linux) v hello sdílené složky /openfoam.</span><span class="sxs-lookup"><span data-stu-id="693d5-217">Place this file (saved with Linux line endings) in hello shared folder /openfoam.</span></span> <span data-ttu-id="693d5-218">Tento soubor zároveň obsahuje nastavení pro hello MPI a OpenFOAM runtimes použít novější toorun úlohu OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="693d5-218">This file also contains settings for hello MPI and OpenFOAM runtimes that you use later toorun an OpenFOAM job.</span></span>
3. <span data-ttu-id="693d5-219">Nainstalujte závislé balíčky vyžadované toocompile OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="693d5-219">Install dependent packages needed toocompile OpenFOAM.</span></span> <span data-ttu-id="693d5-220">V závislosti na vaší distribuci systému Linux bude pravděpodobně nutné nejprve tooadd úložiště.</span><span class="sxs-lookup"><span data-stu-id="693d5-220">Depending on your Linux distribution, you might first need tooadd a repository.</span></span> <span data-ttu-id="693d5-221">Spustit **clusrun** podobné toohello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="693d5-221">Run **clusrun** commands similar toohello following:</span></span>
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    <span data-ttu-id="693d5-222">V případě potřeby SSH tooeach Linux uzlu toorun hello příkazy tooconfirm, které fungují správně.</span><span class="sxs-lookup"><span data-stu-id="693d5-222">If necessary, SSH tooeach Linux node toorun hello commands tooconfirm that they run properly.</span></span>
4. <span data-ttu-id="693d5-223">Spusťte následující příkaz toocompile OpenFOAM hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-223">Run hello following command toocompile OpenFOAM.</span></span> <span data-ttu-id="693d5-224">Hello kompilaci proces trvá některé toocomplete čas a generuje velké množství výstup toostandard informace protokolu, takže použití hello **/ prokládaný** možnost výstup hello toodisplay prokládaný.</span><span class="sxs-lookup"><span data-stu-id="693d5-224">hello compilation process takes some time toocomplete and generates a large amount of log information toostandard output, so use hello **/interleaved** option toodisplay hello output interleaved.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > <span data-ttu-id="693d5-225">Hello "\\`" symbol v příkazu hello je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="693d5-225">hello “\\`” symbol in hello command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="693d5-226">"\\`&" znamená hello "a" je součástí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-226">“\\`&” means hello “&” is a part of hello command.</span></span>
   > 
   > 

## <a name="prepare-toorun-an-openfoam-job"></a><span data-ttu-id="693d5-227">Příprava toorun úlohu OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="693d5-227">Prepare toorun an OpenFOAM job</span></span>
<span data-ttu-id="693d5-228">Nyní get připravené toorun úlohu MPI volá sloshingTank3D, což je jedna ze hello OpenFoam ukázky, na dva uzly Linux.</span><span class="sxs-lookup"><span data-stu-id="693d5-228">Now get ready toorun an MPI job called sloshingTank3D, which is one of hello OpenFoam samples, on two Linux nodes.</span></span> 

### <a name="set-up-hello-runtime-environment"></a><span data-ttu-id="693d5-229">Nastavení prostředí runtime hello</span><span class="sxs-lookup"><span data-stu-id="693d5-229">Set up hello runtime environment</span></span>
<span data-ttu-id="693d5-230">tooset hello prostředí runtime pro MPI a OpenFOAM na uzlech hello Linux, spusťte následující příkaz v okně prostředí Windows PowerShell hlavního uzlu hello hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-230">tooset up hello runtime environments for MPI and OpenFOAM on hello Linux nodes, run hello following command in a Windows PowerShell window on hello head node.</span></span> <span data-ttu-id="693d5-231">(Tento příkaz je platný pro SUSE Linux pouze.)</span><span class="sxs-lookup"><span data-stu-id="693d5-231">(This command is valid for SUSE Linux only.)</span></span>

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a><span data-ttu-id="693d5-232">Příprava ukázková data</span><span class="sxs-lookup"><span data-stu-id="693d5-232">Prepare sample data</span></span>
<span data-ttu-id="693d5-233">Použití hello hlavního uzlu sdílení jste nakonfigurovali dříve tooshare soubory mezi uzly Linux hello (připojit jako /openfoam).</span><span class="sxs-lookup"><span data-stu-id="693d5-233">Use hello head node share you configured previously tooshare files among hello Linux nodes (mounted as /openfoam).</span></span>

1. <span data-ttu-id="693d5-234">SSH tooone vaše Linux výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="693d5-234">SSH tooone of your Linux compute nodes.</span></span>
2. <span data-ttu-id="693d5-235">Pokud jste to ještě neudělali, spusťte následující příkaz tooset hello OpenFOAM modulu runtime prostředí, hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-235">Run hello following command tooset up hello OpenFOAM runtime environment, if you haven’t already done this.</span></span>
   
   ```
   $ source /openfoam/settings.sh
   ```
3. <span data-ttu-id="693d5-236">Zkopírujte hello sloshingTank3D ukázka toohello sdílené složky a přejděte tooit.</span><span class="sxs-lookup"><span data-stu-id="693d5-236">Copy hello sloshingTank3D sample toohello shared folder and navigate tooit.</span></span>
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. <span data-ttu-id="693d5-237">Pokud použijete výchozí parametry hello této ukázky, může trvat desítkami toorun minut, proto je vhodné toomodify některé parametry toomake pracovat rychleji.</span><span class="sxs-lookup"><span data-stu-id="693d5-237">When you use hello default parameters of this sample, it can take tens of minutes toorun, so you might want toomodify some parameters toomake it run faster.</span></span> <span data-ttu-id="693d5-238">Jeden jednoduchý volba je toomodify hello čas krok proměnné deltaT a writeInterval v souboru systému/controlDict hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-238">One simple choice is toomodify hello time step variables deltaT and writeInterval in hello system/controlDict file.</span></span> <span data-ttu-id="693d5-239">Tento soubor uloží veškerá vstupní data týkající se řízení toohello čas a čtení a zápis dat řešení.</span><span class="sxs-lookup"><span data-stu-id="693d5-239">This file stores all input data relating toohello control of time and reading and writing solution data.</span></span> <span data-ttu-id="693d5-240">Můžete například změnit hodnotu hello deltaT z 0,05 too0.5 a hello hodnotu writeInterval z 0,05 too0.5.</span><span class="sxs-lookup"><span data-stu-id="693d5-240">For example, you could change hello value of deltaT from 0.05 too0.5 and hello value of writeInterval from 0.05 too0.5.</span></span>
   
   ![Umožňuje změnit proměnné krok][step_variables]
5. <span data-ttu-id="693d5-242">Zadejte požadované hodnoty pro proměnné hello v souboru systému/decomposeParDict hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-242">Specify desired values for hello variables in hello system/decomposeParDict file.</span></span> <span data-ttu-id="693d5-243">Tento příklad používá dva Linux uzly každý s 8 jádry tak nastavit numberOfSubdomains too16 a n hierarchicalCoeffs too(1 1 16), což znamená spustit OpenFOAM souběžně s 16 procesy.</span><span class="sxs-lookup"><span data-stu-id="693d5-243">This example uses two Linux nodes each with 8 cores, so set numberOfSubdomains too16 and n of hierarchicalCoeffs too(1 1 16), which means run OpenFOAM in parallel with 16 processes.</span></span> <span data-ttu-id="693d5-244">Další informace najdete v tématu [OpenFOAM uživatelská příručka: 3,4 aplikace běží paralelně](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span><span class="sxs-lookup"><span data-stu-id="693d5-244">For more information, see [OpenFOAM User Guide: 3.4 Running applications in parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span></span>
   
   ![Rozložit procesy][decompose]
6. <span data-ttu-id="693d5-246">Spuštěním následujících příkazů z hello sloshingTank3D directory tooprepare hello ukázková data hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-246">Run hello following commands from hello sloshingTank3D directory tooprepare hello sample data.</span></span>
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. <span data-ttu-id="693d5-247">Hello hlavního uzlu měli byste vidět, že se zkopírují hello ukázkových datových souborů do C:\OpenFoam\sloshingTank3D.</span><span class="sxs-lookup"><span data-stu-id="693d5-247">On hello head node, you should see hello sample data files are copied into C:\OpenFoam\sloshingTank3D.</span></span> <span data-ttu-id="693d5-248">(C:\OpenFoam je hello sdílenou složku na hello hlavního uzlu.)</span><span class="sxs-lookup"><span data-stu-id="693d5-248">(C:\OpenFoam is hello shared folder on hello head node.)</span></span>
   
   ![Datové soubory hello hlavního uzlu][data_files]

### <a name="host-file-for-mpirun"></a><span data-ttu-id="693d5-250">Soubor hostitele pro mpirun</span><span class="sxs-lookup"><span data-stu-id="693d5-250">Host file for mpirun</span></span>
<span data-ttu-id="693d5-251">V tomto kroku vytvoříte soubor hostitele (seznam výpočetní uzly) které hello **mpirun** příkaz používá.</span><span class="sxs-lookup"><span data-stu-id="693d5-251">In this step, you create a host file (a list of compute nodes) which hello **mpirun** command uses.</span></span>

1. <span data-ttu-id="693d5-252">Na jednom z uzlů hello Linux vytvořte soubor s názvem hostfile pod /openfoam, takže tento soubor lze dosáhnout za /openfoam/hostfile na všech uzlech Linux.</span><span class="sxs-lookup"><span data-stu-id="693d5-252">On one of hello Linux nodes, create a file named hostfile under /openfoam, so this file can be reached at /openfoam/hostfile on all Linux nodes.</span></span>
2. <span data-ttu-id="693d5-253">Názvy uzlu Linux zapisovat do tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="693d5-253">Write your Linux node names into this file.</span></span> <span data-ttu-id="693d5-254">V tomto příkladu hello soubor obsahuje hello následující názvy:</span><span class="sxs-lookup"><span data-stu-id="693d5-254">In this example, hello file contains hello following names:</span></span>
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > <span data-ttu-id="693d5-255">Tento soubor můžete vytvořit také v C:\OpenFoam\hostfile hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="693d5-255">You can also create this file at C:\OpenFoam\hostfile on hello head node.</span></span> <span data-ttu-id="693d5-256">Pokud zvolíte tuto možnost, uložte ho jako textový soubor s Linuxem konce řádků (pouze LF, není CR LF).</span><span class="sxs-lookup"><span data-stu-id="693d5-256">If you choose this option, save it as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="693d5-257">To zajišťuje, že ho běží správně na uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-257">This ensures that it runs properly on hello Linux nodes.</span></span>
   > 
   > 
   
   <span data-ttu-id="693d5-258">**Obálka bash skriptu**</span><span class="sxs-lookup"><span data-stu-id="693d5-258">**Bash script wrapper**</span></span>
   
   <span data-ttu-id="693d5-259">Pokud máte mnoho Linuxových uzlů a chcete, aby vaše úlohy toorun na jenom některé z nich, není vhodné toouse, pevné hostitele souboru, protože nevíte uzlů, které se přidělí tooyour úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-259">If you have many Linux nodes and you want your job toorun on only some of them, it’s not a good idea toouse a fixed host file, because you don’t know which nodes will be allocated tooyour job.</span></span> <span data-ttu-id="693d5-260">V takovém případě zápisu Obálka skriptů bash pro **mpirun** toocreate hello soubor hostitele automaticky.</span><span class="sxs-lookup"><span data-stu-id="693d5-260">In this case, write a bash script wrapper for **mpirun** toocreate hello host file automatically.</span></span> <span data-ttu-id="693d5-261">Můžete najít obálku skriptů bash příklad názvem hpcimpirun.sh na konci hello tohoto článku a uložte ho jako /openfoam/hpcimpirun.sh. Tento ukázkový skript hello následující:</span><span class="sxs-lookup"><span data-stu-id="693d5-261">You can find an example bash script wrapper called hpcimpirun.sh at hello end of this article, and save it as /openfoam/hpcimpirun.sh. This example script does hello following:</span></span>
   
   1. <span data-ttu-id="693d5-262">Nastaví hello proměnných prostředí pro **mpirun**a některé příkaz přidání parametrů toorun hello MPI úlohy přes síť hello RDMA.</span><span class="sxs-lookup"><span data-stu-id="693d5-262">Sets up hello environment variables for **mpirun**, and some addition command parameters toorun hello MPI job through hello RDMA network.</span></span> <span data-ttu-id="693d5-263">V takovém případě nastaví hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="693d5-263">In this case, it sets hello following variables:</span></span>
      
      * <span data-ttu-id="693d5-264">I_MPI_FABRICS = shm:dapl</span><span class="sxs-lookup"><span data-stu-id="693d5-264">I_MPI_FABRICS=shm:dapl</span></span>
      * <span data-ttu-id="693d5-265">I_MPI_DAPL_PROVIDER = správcem v2-ib0</span><span class="sxs-lookup"><span data-stu-id="693d5-265">I_MPI_DAPL_PROVIDER=ofa-v2-ib0</span></span>
      * <span data-ttu-id="693d5-266">I_MPI_DYNAMIC_CONNECTION = 0</span><span class="sxs-lookup"><span data-stu-id="693d5-266">I_MPI_DYNAMIC_CONNECTION=0</span></span>
   2. <span data-ttu-id="693d5-267">Vytvoří soubor hostitele podle toohello $ proměnné prostředí CCP_NODES_CORES, jež je nastavena pomocí hlavního uzlu HPC hello při aktivaci hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-267">Creates a host file according toohello environment variable $CCP_NODES_CORES, which is set by hello HPC head node when hello job is activated.</span></span>
      
      <span data-ttu-id="693d5-268">Formát Hello $CCP_NODES_CORES zahrnuje tento vzor:</span><span class="sxs-lookup"><span data-stu-id="693d5-268">hello format of $CCP_NODES_CORES follows this pattern:</span></span>
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      <span data-ttu-id="693d5-269">kde</span><span class="sxs-lookup"><span data-stu-id="693d5-269">where</span></span>
      
      * <span data-ttu-id="693d5-270">`<Number of nodes>`-hello počet uzlů přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-270">`<Number of nodes>` - hello number of nodes allocated toothis job.</span></span>  
      * <span data-ttu-id="693d5-271">`<Name of node_n_...>`-hello název každého uzlu přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-271">`<Name of node_n_...>` - hello name of each node allocated toothis job.</span></span>
      * <span data-ttu-id="693d5-272">`<Cores of node_n_...>`-hello počet jader na hello uzlu přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-272">`<Cores of node_n_...>` - hello number of cores on hello node allocated toothis job.</span></span>
      
      <span data-ttu-id="693d5-273">Například pokud hello úlohy dvou uzlů toorun, $CCP_NODES_CORES je podobná</span><span class="sxs-lookup"><span data-stu-id="693d5-273">For example, if hello job needs two nodes toorun, $CCP_NODES_CORES is similar to</span></span>
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. <span data-ttu-id="693d5-274">Volání hello **mpirun** příkazů a připojí dva parametry toohello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="693d5-274">Calls hello **mpirun** command and appends two parameters toohello command line.</span></span>
      
      * <span data-ttu-id="693d5-275">`--hostfile <hostfilepath>: <hostfilepath>`-Vytvoří hello cestu skript hello souboru hostitele hello</span><span class="sxs-lookup"><span data-stu-id="693d5-275">`--hostfile <hostfilepath>: <hostfilepath>` - hello path of hello host file hello script creates</span></span>
      * <span data-ttu-id="693d5-276">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`– Proměnná prostředí, která nastavuje hello HPC Pack hlavního uzlu, který ukládá číslo hello celkový počet jader přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-276">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` - an environment variable set by hello HPC Pack head node, which stores hello number of total cores allocated toothis job.</span></span> <span data-ttu-id="693d5-277">V takovém případě určuje hello počet procesů pro **mpirun**.</span><span class="sxs-lookup"><span data-stu-id="693d5-277">In this case, it specifies hello number of processes for **mpirun**.</span></span>

## <a name="submit-an-openfoam-job"></a><span data-ttu-id="693d5-278">Odeslat úlohu OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="693d5-278">Submit an OpenFOAM job</span></span>
<span data-ttu-id="693d5-279">Teď můžete odeslat úlohu ve Správci clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="693d5-279">Now you can submit a job in HPC Cluster Manager.</span></span> <span data-ttu-id="693d5-280">Je nutné toopass hello skriptu hpcimpirun.sh v hello příkazových řádků pro některé úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-280">You need toopass hello script hpcimpirun.sh in hello command lines for some of hello job tasks.</span></span>

1. <span data-ttu-id="693d5-281">Připojit tooyour hlavního uzlu clusteru a spusťte Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="693d5-281">Connect tooyour cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="693d5-282">**Ve správě prostředků**, zajistěte, aby hello Linux výpočetní uzly v hello **Online** stavu.</span><span class="sxs-lookup"><span data-stu-id="693d5-282">**In Resource Management**, ensure that hello Linux compute nodes are in hello **Online** state.</span></span> <span data-ttu-id="693d5-283">Pokud tomu tak není, vyberte je a klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="693d5-283">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="693d5-284">V **úlohy správy**, klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="693d5-284">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="693d5-285">Zadejte název úlohy, jako *sloshingTank3D*.</span><span class="sxs-lookup"><span data-stu-id="693d5-285">Enter a name for job such as *sloshingTank3D*.</span></span>
   
   ![Podrobnosti úlohy][job_details]
5. <span data-ttu-id="693d5-287">V **úlohy prostředky**, vyberte typ prostředku jako "Uzel" hello a nastavte too2 minimální hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-287">In **Job resources**, choose hello type of resource as “Node” and set hello Minimum too2.</span></span> <span data-ttu-id="693d5-288">Tato konfigurace se spouští úlohy hello na dva uzly Linux, z nichž každá má osm jader v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="693d5-288">This configuration runs hello job on two Linux nodes, each of which has eight cores in this example.</span></span>
   
   ![Úloha prostředky][job_resources]
6. <span data-ttu-id="693d5-290">Klikněte na tlačítko **upravit úlohy** v hello levé navigaci a potom klikněte na **přidat** tooadd úlohu toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="693d5-290">Click **Edit Tasks** in hello left navigation, and then click **Add** tooadd a task toohello job.</span></span> <span data-ttu-id="693d5-291">Přidat čtyři úlohu toohello úlohy s hello následující nastavení a příkazové řádky.</span><span class="sxs-lookup"><span data-stu-id="693d5-291">Add four tasks toohello job with hello following command lines and settings.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="693d5-292">Spuštění `source /openfoam/settings.sh` nastaví hello OpenFOAM a MPI běhová prostředí, takže každý z následujících úloh hello volá před hello OpenFOAM příkaz.</span><span class="sxs-lookup"><span data-stu-id="693d5-292">Running `source /openfoam/settings.sh` sets up hello OpenFOAM and MPI runtime environments, so each of hello following tasks calls it before hello OpenFOAM command.</span></span>
   > 
   > 
   
   * <span data-ttu-id="693d5-293">**Úloha 1**.</span><span class="sxs-lookup"><span data-stu-id="693d5-293">**Task 1**.</span></span> <span data-ttu-id="693d5-294">Spustit **decomposePar** toogenerate datové soubory pro spuštění **interDyMFoam** paralelně.</span><span class="sxs-lookup"><span data-stu-id="693d5-294">Run **decomposePar** toogenerate data files for running **interDyMFoam** in parallel.</span></span>
     
     * <span data-ttu-id="693d5-295">Přiřadit jeden úkol toohello uzlu</span><span class="sxs-lookup"><span data-stu-id="693d5-295">Assign one node toohello task</span></span>
     * <span data-ttu-id="693d5-296">**Příkazový řádek** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="693d5-296">**Command line** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="693d5-297">**Pracovní adresář** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="693d5-297">**Working directory** - /openfoam/sloshingTank3D</span></span>
     
     <span data-ttu-id="693d5-298">Viz následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-298">See hello following figure.</span></span> <span data-ttu-id="693d5-299">Podobně nakonfigurujete zbývajících úkolech hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-299">You configure hello remaining tasks similarly.</span></span>
     
     ![Podrobnosti úlohy 1][task_details1]
   * <span data-ttu-id="693d5-301">**Úloha 2**.</span><span class="sxs-lookup"><span data-stu-id="693d5-301">**Task 2**.</span></span> <span data-ttu-id="693d5-302">Spustit **interDyMFoam** v ukázce paralelní toocompute hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-302">Run **interDyMFoam** in parallel toocompute hello sample.</span></span>
     
     * <span data-ttu-id="693d5-303">Přiřazení úkolů toohello dva uzly</span><span class="sxs-lookup"><span data-stu-id="693d5-303">Assign two nodes toohello task</span></span>
     * <span data-ttu-id="693d5-304">**Příkazový řádek** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="693d5-304">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="693d5-305">**Pracovní adresář** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="693d5-305">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="693d5-306">**Úloha 3**.</span><span class="sxs-lookup"><span data-stu-id="693d5-306">**Task 3**.</span></span> <span data-ttu-id="693d5-307">Spustit **reconstructPar** toomerge hello nastaví čas adresářů z každý adresář processor_N_ do jedné sady.</span><span class="sxs-lookup"><span data-stu-id="693d5-307">Run **reconstructPar** toomerge hello sets of time directories from each processor_N_ directory into a single set.</span></span>
     
     * <span data-ttu-id="693d5-308">Přiřadit jeden úkol toohello uzlu</span><span class="sxs-lookup"><span data-stu-id="693d5-308">Assign one node toohello task</span></span>
     * <span data-ttu-id="693d5-309">**Příkazový řádek** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="693d5-309">**Command line** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="693d5-310">**Pracovní adresář** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="693d5-310">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="693d5-311">**Úloha 4**.</span><span class="sxs-lookup"><span data-stu-id="693d5-311">**Task 4**.</span></span> <span data-ttu-id="693d5-312">Spustit **foamToEnsight** v paralelní tooconvert hello OpenFOAM výsledek soubory do EnSight formátu a hello EnSight soubory umístit do adresáře s názvem Ensight v adresáři případu hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-312">Run **foamToEnsight** in parallel tooconvert hello OpenFOAM result files into EnSight format and place hello EnSight files in a directory named Ensight in hello case directory.</span></span>
     
     * <span data-ttu-id="693d5-313">Přiřazení úkolů toohello dva uzly</span><span class="sxs-lookup"><span data-stu-id="693d5-313">Assign two nodes toohello task</span></span>
     * <span data-ttu-id="693d5-314">**Příkazový řádek** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="693d5-314">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="693d5-315">**Pracovní adresář** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="693d5-315">**Working directory** - /openfoam/sloshingTank3D</span></span>
7. <span data-ttu-id="693d5-316">Přidání úkolů toothese závislosti ve vzestupném pořadí úkolů.</span><span class="sxs-lookup"><span data-stu-id="693d5-316">Add dependencies toothese tasks in ascending task order.</span></span>
   
   ![Závislosti úkolů][task_dependencies]
8. <span data-ttu-id="693d5-318">Klikněte na tlačítko **odeslání** toorun tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="693d5-318">Click **Submit** toorun this job.</span></span>
   
   <span data-ttu-id="693d5-319">Ve výchozím nastavení odešle HPC Pack úlohy hello jako váš aktuální účet přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="693d5-319">By default, HPC Pack submits hello job as your current logged-on user account.</span></span> <span data-ttu-id="693d5-320">Po kliknutí na tlačítko **odeslání**, může se zobrazit dialogové okno s výzvou tooenter hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="693d5-320">After you click **Submit**, you might see a dialog box prompting you tooenter hello user name and password.</span></span>
   
   ![Přihlašovací údaje úlohy][creds]
   
   <span data-ttu-id="693d5-322">Za určitých podmínek pamatuje HPC Pack hello vstupní před a nezobrazí toto dialogové okno informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="693d5-322">Under some conditions, HPC Pack remembers hello user information you input before and doesn’t show this dialog box.</span></span> <span data-ttu-id="693d5-323">toomake HPC Pack jej znovu zobrazit, zadejte následující příkaz na příkazovém řádku hello a pak odeslat úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-323">toomake HPC Pack show it again, enter hello following command at a Command prompt and then submit hello job.</span></span>
   
   ```
   hpccred delcreds
   ```
9. <span data-ttu-id="693d5-324">Úloha Hello trvá z desítek minut tooseveral hodin podle toohello parametrů, které jste nastavili pro ukázku hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-324">hello job takes from tens of minutes tooseveral hours according toohello parameters you have set for hello sample.</span></span> <span data-ttu-id="693d5-325">V hello heat mapa najdete v části hello úlohy spuštěné v uzlech Linux hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-325">In hello heat map, you see hello job running on hello Linux nodes.</span></span> 
   
   ![Heat mapa][heat_map]
   
   <span data-ttu-id="693d5-327">Na každém uzlu jsou osm procesy spuštěné.</span><span class="sxs-lookup"><span data-stu-id="693d5-327">On each node, eight processes are started.</span></span>
   
   ![Procesy Linux][linux_processes]
10. <span data-ttu-id="693d5-329">Po dokončení úlohy hello najdete ve složkách v rámci C:\OpenFoam\sloshingTank3D a souborů protokolů hello C:\OpenFoam výsledky úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-329">When hello job finishes, find hello job results in folders under C:\OpenFoam\sloshingTank3D, and hello log files at C:\OpenFoam.</span></span>

## <a name="view-results-in-ensight"></a><span data-ttu-id="693d5-330">Zobrazení výsledků v EnSight</span><span class="sxs-lookup"><span data-stu-id="693d5-330">View results in EnSight</span></span>
<span data-ttu-id="693d5-331">Volitelně použijte [EnSight](https://www.ceisoftware.com/) toovisualize a analýza výsledků hello hello OpenFOAM úlohy.</span><span class="sxs-lookup"><span data-stu-id="693d5-331">Optionally use [EnSight](https://www.ceisoftware.com/) toovisualize and analyze hello results of hello OpenFOAM job.</span></span> <span data-ttu-id="693d5-332">Další informace o vizualizace a animace v EnSight najdete v tématu to [video průvodce](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span><span class="sxs-lookup"><span data-stu-id="693d5-332">For more about visualization and animation in EnSight, see this [video guide](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span></span>

1. <span data-ttu-id="693d5-333">Po instalaci EnSight hello hlavního uzlu, spusťte ji.</span><span class="sxs-lookup"><span data-stu-id="693d5-333">After you install EnSight on hello head node, start it.</span></span>
2. <span data-ttu-id="693d5-334">Otevřete C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span><span class="sxs-lookup"><span data-stu-id="693d5-334">Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span></span>
   
   <span data-ttu-id="693d5-335">Zobrazí se nádrží v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-335">You see a tank in hello viewer.</span></span>
   
   ![Nádrž v EnSight][tank]
3. <span data-ttu-id="693d5-337">Vytvoření **Isosurface** z **internalMesh**a potom zvolte hello proměnná **alpha_water**.</span><span class="sxs-lookup"><span data-stu-id="693d5-337">Create an **Isosurface** from **internalMesh**, and then choose hello variable **alpha_water**.</span></span>
   
   ![Vytvoření isosurface][isosurface]
4. <span data-ttu-id="693d5-339">Nastavit barvu hello **Isosurface_part** vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-339">Set hello color for **Isosurface_part** created in hello previous step.</span></span> <span data-ttu-id="693d5-340">Například nastavte toowater blue.</span><span class="sxs-lookup"><span data-stu-id="693d5-340">For example, set it toowater blue.</span></span>
   
   ![Upravit isosurface barev][isosurface_color]
5. <span data-ttu-id="693d5-342">Vytvoření **Iso svazku** z **bariéry** výběrem **bariéry** v hello **částí** panelu a klikněte na tlačítko hello **Isosurfaces**  tlačítka na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-342">Create an **Iso-volume** from **walls** by selecting **walls** in hello **Parts** panel and click hello **Isosurfaces** button in hello toolbar.</span></span>
6. <span data-ttu-id="693d5-343">V dialogovém okně hello, vyberte **typ** jako **Isovolume** a nastavte hello minimum z **Isovolume rozsah** too0.5.</span><span class="sxs-lookup"><span data-stu-id="693d5-343">In hello dialog box, select **Type** as **Isovolume** and set hello Min of **Isovolume range** too0.5.</span></span> <span data-ttu-id="693d5-344">toocreate hello isovolume, klikněte na tlačítko **vytvořit s vybraných částí**.</span><span class="sxs-lookup"><span data-stu-id="693d5-344">toocreate hello isovolume, click **Create with selected parts**.</span></span>
7. <span data-ttu-id="693d5-345">Nastavit barvu hello **Iso_volume_part** vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="693d5-345">Set hello color for **Iso_volume_part** created in hello previous step.</span></span> <span data-ttu-id="693d5-346">Například nastavte toodeep horních blue.</span><span class="sxs-lookup"><span data-stu-id="693d5-346">For example, set it toodeep water blue.</span></span>
8. <span data-ttu-id="693d5-347">Nastavit barvu hello **bariéry**.</span><span class="sxs-lookup"><span data-stu-id="693d5-347">Set hello color for **walls**.</span></span> <span data-ttu-id="693d5-348">Například nastavte tootransparent bílé.</span><span class="sxs-lookup"><span data-stu-id="693d5-348">For example, set it tootransparent white.</span></span>
9. <span data-ttu-id="693d5-349">Klikněte na tlačítko **přehrání** toosee hello výsledky hello simulace.</span><span class="sxs-lookup"><span data-stu-id="693d5-349">Now click **Play** toosee hello results of hello simulation.</span></span>
   
    ![Výsledek nádrž][tank_result]

## <a name="sample-files"></a><span data-ttu-id="693d5-351">Ukázkové soubory</span><span class="sxs-lookup"><span data-stu-id="693d5-351">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="693d5-352">Ukázkový soubor XML konfigurace pro nasazení clusteru pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="693d5-352">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a><span data-ttu-id="693d5-353">Ukázkový soubor cred.xml</span><span class="sxs-lookup"><span data-stu-id="693d5-353">Sample cred.xml file</span></span>
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
### <a name="sample-silentcfg-file-tooinstall-mpi"></a><span data-ttu-id="693d5-354">Ukázka silent.cfg soubor tooinstall MPI</span><span class="sxs-lookup"><span data-stu-id="693d5-354">Sample silent.cfg file tooinstall MPI</span></span>
```
# Patterns used toocheck silent configuration file
#
# anythingpat - any string
# filepat     - hello file location pattern (/file/location/to/license.lic)
# lspat       - hello license server address pattern (0123@hostname)
# snpat       - hello serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components tooinstall, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path toohello cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes tooenable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes tooupdate ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a><span data-ttu-id="693d5-355">Ukázkový skript settings.sh</span><span class="sxs-lookup"><span data-stu-id="693d5-355">Sample settings.sh script</span></span>
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a><span data-ttu-id="693d5-356">Ukázkový skript hpcimpirun.sh</span><span class="sxs-lookup"><span data-stu-id="693d5-356">Sample hpcimpirun.sh script</span></span>
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run hello mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into hello hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
