---
title: "Obrázek aaaCreate a nahrání virtuálního počítače FreeBSD | Microsoft Docs"
description: "Další informace toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello FreeBSD operačního systému toocreate virtuální počítač Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kyliel
ms.openlocfilehash: f3bd155e496f1a2713d36bb66ea9824ed4c210ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-freebsd-vhd-tooazure"></a><span data-ttu-id="88a58-103">Vytvoření a nahrání virtuálního pevného disku FreeBSD tooAzure</span><span class="sxs-lookup"><span data-stu-id="88a58-103">Create and upload a FreeBSD VHD tooAzure</span></span>
<span data-ttu-id="88a58-104">Tento článek ukazuje, jak toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello FreeBSD operačního systému.</span><span class="sxs-lookup"><span data-stu-id="88a58-104">This article shows you how toocreate and upload a virtual hard disk (VHD) that contains hello FreeBSD operating system.</span></span> <span data-ttu-id="88a58-105">Po odeslání, můžete jej jako vlastní image toocreate virtuální počítač (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-105">After you upload it, you can use it as your own image toocreate a virtual machine (VM) in Azure.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="88a58-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="88a58-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="88a58-107">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="88a58-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="88a58-109">Informace o nahrávání virtuální pevný disk pomocí modelu Resource Manager hello najdete v tématu [zde](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="88a58-109">For information about uploading a VHD using hello Resource Manager model, see [here](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88a58-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="88a58-110">Prerequisites</span></span>
<span data-ttu-id="88a58-111">Tento článek předpokládá, že máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="88a58-111">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="88a58-112">**Předplatné Azure**– Pokud nemáte účet, můžete vytvořit jeden si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="88a58-112">**An Azure subscription**--If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="88a58-113">Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="88a58-113">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="88a58-114">Jinak, zjistěte, jak příliš[vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="88a58-114">Otherwise, learn how too[create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="88a58-115">**Nástroje Azure PowerShell**– modul Azure PowerShell hello musí být nainstalován a nakonfigurován toouse vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-115">**Azure PowerShell tools**--hello Azure PowerShell module must be installed and configured toouse your Azure subscription.</span></span> <span data-ttu-id="88a58-116">modul hello toodownload, najdete v části [Azure stáhne](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="88a58-116">toodownload hello module, see [Azure downloads](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="88a58-117">Kurz, který popisuje, jak tooinstall a nakonfigurovat modul hello Zde jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="88a58-117">A tutorial that describes how tooinstall and configure hello module is available here.</span></span> <span data-ttu-id="88a58-118">Použití hello [Azure stáhne](https://azure.microsoft.com/downloads/) rutiny tooupload hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="88a58-118">Use hello [Azure Downloads](https://azure.microsoft.com/downloads/) cmdlet tooupload hello VHD.</span></span>
* <span data-ttu-id="88a58-119">**FreeBSD operační systém nainstalovaný v souboru VHD**– na podporovaný operační systém FreeBSD musí být nainstalován tooa virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="88a58-119">**FreeBSD operating system installed in a .vhd file**--A supported   FreeBSD operating system must be installed tooa virtual hard disk.</span></span> <span data-ttu-id="88a58-120">Několik nástrojů existovat toocreate soubory VHD.</span><span class="sxs-lookup"><span data-stu-id="88a58-120">Multiple tools exist toocreate .vhd files.</span></span> <span data-ttu-id="88a58-121">Můžete například použít řešení virtualizace jako soubor VHD hello toocreate technologie Hyper-V a nainstalujte hello operační systém.</span><span class="sxs-lookup"><span data-stu-id="88a58-121">For example, you can use a virtualization solution such as Hyper-V toocreate hello .vhd file and install hello operating system.</span></span> <span data-ttu-id="88a58-122">Pokyny o tom, jak tooinstall a použití technologie Hyper-V, najdete v části [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="88a58-122">For instructions about how tooinstall and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="88a58-123">Hello novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-123">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="88a58-124">Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny [convert-VHD prostředí](https://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="88a58-124">You can convert hello disk tooVHD format by using Hyper-V Manager or hello cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="88a58-125">Kromě toho je [kurz na webu MSDN o toouse FreeBSD s technologií Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span><span class="sxs-lookup"><span data-stu-id="88a58-125">In addition, there is a [tutorial on MSDN about how toouse FreeBSD with Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span></span>
>
>

<span data-ttu-id="88a58-126">Tato úloha obsahuje hello pět kroků:</span><span class="sxs-lookup"><span data-stu-id="88a58-126">This task includes hello following five steps:</span></span>

## <a name="step-1-prepare-hello-image-for-upload"></a><span data-ttu-id="88a58-127">Krok 1: Příprava hello image pro odesílání</span><span class="sxs-lookup"><span data-stu-id="88a58-127">Step 1: Prepare hello image for upload</span></span>
<span data-ttu-id="88a58-128">Na virtuálním počítači hello nainstalovanou hello FreeBSD operačního systému proveďte následující postupy hello:</span><span class="sxs-lookup"><span data-stu-id="88a58-128">On hello virtual machine where you installed hello FreeBSD operating system, complete hello following procedures:</span></span>

1. <span data-ttu-id="88a58-129">Povolte protokol DHCP.</span><span class="sxs-lookup"><span data-stu-id="88a58-129">Enable DHCP.</span></span>

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. <span data-ttu-id="88a58-130">Povolte SSH.</span><span class="sxs-lookup"><span data-stu-id="88a58-130">Enable SSH.</span></span>

    <span data-ttu-id="88a58-131">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="88a58-131">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span> <span data-ttu-id="88a58-132">Ve výchozím nastavení je povoleno po instalaci z disku FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="88a58-132">By default it is enabled after installation from FreeBSD disc.</span></span> 
3. <span data-ttu-id="88a58-133">Nastavení konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="88a58-133">Set up a serial console.</span></span>

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. <span data-ttu-id="88a58-134">Nainstalujte sudo.</span><span class="sxs-lookup"><span data-stu-id="88a58-134">Install sudo.</span></span>

    <span data-ttu-id="88a58-135">Hello kořenového účtu je zakázána v Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-135">hello root account is disabled in Azure.</span></span> <span data-ttu-id="88a58-136">To znamená, že potřebujete tooutilize sudo z Neprivilegovaný uživatelský toorun příkazy se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="88a58-136">This means you need tooutilize sudo from an unprivileged user toorun commands with elevated privileges.</span></span>

        # pkg install sudo
   
5. <span data-ttu-id="88a58-137">Požadavky pro agenta Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-137">Prerequisites for Azure Agent.</span></span>

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. <span data-ttu-id="88a58-138">Nainstalujte agenta Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-138">Install Azure Agent.</span></span>

    <span data-ttu-id="88a58-139">Hello nejnovější verzi agenta Azure hello vždy naleznete na [githubu](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="88a58-139">hello latest release of hello Azure Agent can always be found on [github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="88a58-140">Hello verze 2.0.10 + oficiálně podporuje FreeBSD 10 & 10.1 a verze hello 2.1.4 + (včetně 2.2.x) oficiálně podporuje FreeBSD 10.2 a pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="88a58-140">hello version 2.0.10 + officially supports FreeBSD 10 & 10.1, and hello version 2.1.4 + (including 2.2.x) officially supports FreeBSD 10.2 and later releases.</span></span>

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    <span data-ttu-id="88a58-141">Pro 2.0 použijeme 2.0.16 jako příklad:</span><span class="sxs-lookup"><span data-stu-id="88a58-141">For 2.0, let's use 2.0.16 as an example:</span></span>

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    <span data-ttu-id="88a58-142">Pro 2.1 použijeme 2.1.4 jako příklad:</span><span class="sxs-lookup"><span data-stu-id="88a58-142">For 2.1, let's use 2.1.4 as an example:</span></span>

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > <span data-ttu-id="88a58-143">Po instalaci agenta Azure, je vhodné tooverify, který používá:</span><span class="sxs-lookup"><span data-stu-id="88a58-143">After you install Azure Agent, it's a good idea tooverify that it's running:</span></span>
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. <span data-ttu-id="88a58-144">Zrušení zřízení hello systému.</span><span class="sxs-lookup"><span data-stu-id="88a58-144">Deprovision hello system.</span></span>

    <span data-ttu-id="88a58-145">Deprovision hello systému tooclean ho a zkontrolujte ji vhodný pro reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="88a58-145">Deprovision hello system tooclean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="88a58-146">Hello následující příkaz dojde také k odstranění hello poslední zřízené uživatelský účet a hello související data:</span><span class="sxs-lookup"><span data-stu-id="88a58-146">hello following command also deletes hello last provisioned user account and hello associated data:</span></span>

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    <span data-ttu-id="88a58-147">Nyní můžete vypnout virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="88a58-147">Now you can shut down your VM.</span></span>

## <a name="step-2-create-a-storage-account-in-azure"></a><span data-ttu-id="88a58-148">Krok 2: Vytvoření účtu úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="88a58-148">Step 2: Create a storage account in Azure</span></span>
<span data-ttu-id="88a58-149">Potřebujete účet úložiště v Azure tooupload soubor VHD, může být použité toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="88a58-149">You need a storage account in Azure tooupload a .vhd file so it can be used toocreate a virtual machine.</span></span> <span data-ttu-id="88a58-150">Můžete použít hello Azure classic portálu toocreate účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="88a58-150">You can use hello Azure classic portal toocreate a storage account.</span></span>

1. <span data-ttu-id="88a58-151">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="88a58-151">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="88a58-152">Na panelu příkazů hello, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="88a58-152">On hello command bar, select **New**.</span></span>
3. <span data-ttu-id="88a58-153">Vyberte **datové služby** > **úložiště** > **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="88a58-153">Select **Data Services** > **Storage** > **Quick Create**.</span></span>

    ![Rychlé vytvoření účtu úložiště](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. <span data-ttu-id="88a58-155">Vyplňte pole hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="88a58-155">Fill out hello fields as follows:</span></span>

   * <span data-ttu-id="88a58-156">V hello **URL** pole, zadejte toouse název subdomény hello adresa URL účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="88a58-156">In hello **URL** field, type a subdomain name toouse in hello storage account URL.</span></span> <span data-ttu-id="88a58-157">Hello záznam může obsahovat ze 3 – 24 čísla a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="88a58-157">hello entry can contain from 3-24 numbers and lowercase letters.</span></span> <span data-ttu-id="88a58-158">Tento název se změní na název hostitele hello v rámci hello adresu URL, která je použité tooaddress Azure Blob storage, Azure Queue storage nebo Azure Table storage prostředky pro předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-158">This name becomes hello host name within hello URL that is used tooaddress Azure Blob storage, Azure Queue storage, or Azure Table storage resources for hello subscription.</span></span>
   * <span data-ttu-id="88a58-159">V hello **umístění/skupině vztahů** rozevírací nabídky vyberte hello **umístění nebo skupina vztahů** pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-159">In hello **Location/Affinity Group** drop-down menu, choose hello **location or affinity group** for hello storage account.</span></span> <span data-ttu-id="88a58-160">Skupiny vztahů umožňuje uvést vaše cloudové služby a úložiště do hello stejném datovém centru.</span><span class="sxs-lookup"><span data-stu-id="88a58-160">An affinity group lets you put your cloud services and storage in hello same data center.</span></span>
   * <span data-ttu-id="88a58-161">V hello **replikace** pole, rozhodnout, zda toouse **geograficky redundantní** replikace pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-161">In hello **Replication** field, decide whether toouse **Geo-Redundant** replication for hello storage account.</span></span> <span data-ttu-id="88a58-162">Geografická replikace je zapnutá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="88a58-162">Geo-replication is turned on by default.</span></span> <span data-ttu-id="88a58-163">Tato možnost replikuje vaše data tooa sekundárního umístění, v žádné tooyou náklady, tak, aby úložiště převezme toothat umístění, pokud hlavní selhání nastane v primárním umístění hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-163">This option replicates your data tooa secondary location, at no cost tooyou, so that your storage fails over toothat location if a major failure occurs at hello primary location.</span></span> <span data-ttu-id="88a58-164">Hello sekundárního umístění je přiřazen automaticky a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="88a58-164">hello secondary location is assigned automatically and can't be changed.</span></span> <span data-ttu-id="88a58-165">Pokud potřebujete větší kontrolu nad hello umístění cloudové úložiště z důvodu toolegal požadavky nebo zásad organizace, můžete vypnout geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="88a58-165">If you need more control over hello location of your cloud-based storage due toolegal requirements or organizational policy, you can turn off geo-replication.</span></span> <span data-ttu-id="88a58-166">Upozorňujeme však, že pokud později zapnout geografická replikace, vám bude účtována jednorázové přenos dat poplatek tooreplicate vaší existující data toohello sekundárního umístění.</span><span class="sxs-lookup"><span data-stu-id="88a58-166">However, be aware that if you later turn on geo-replication, you will be charged a one-time data transfer fee tooreplicate your existing data toohello secondary location.</span></span> <span data-ttu-id="88a58-167">Služby úložiště bez geografická replikace se nabízí se slevou.</span><span class="sxs-lookup"><span data-stu-id="88a58-167">Storage services without geo-replication are offered at a discount.</span></span> <span data-ttu-id="88a58-168">Další informace o správě geografická replikace účtů úložiště najdete tady: [replikace Azure Storage](../../../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="88a58-168">More details about managing geo-replication of storage accounts can be found here: [Azure Storage replication](../../../storage/common/storage-redundancy.md).</span></span>

     ![Zadejte podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. <span data-ttu-id="88a58-170">Vyberte **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="88a58-170">Select **Create Storage Account**.</span></span> <span data-ttu-id="88a58-171">Hello účet se teď zobrazí v části **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="88a58-171">hello account now appears under **storage**.</span></span>

    ![Účet úložiště byl úspěšně vytvořen](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. <span data-ttu-id="88a58-173">Dále vytvořte kontejner pro vaše soubory nahrávat VHD.</span><span class="sxs-lookup"><span data-stu-id="88a58-173">Next, create a container for your uploaded .vhd files.</span></span> <span data-ttu-id="88a58-174">Vyberte název účtu úložiště hello a pak vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="88a58-174">Select hello storage account name, and then select **Containers**.</span></span>

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. <span data-ttu-id="88a58-176">Vyberte **vytvořit kontejner**.</span><span class="sxs-lookup"><span data-stu-id="88a58-176">Select **Create a Container**.</span></span>

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. <span data-ttu-id="88a58-178">V hello **název** pole, zadejte název vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="88a58-178">In hello **Name** field, type a name for your container.</span></span> <span data-ttu-id="88a58-179">Potom v hello **přístup** rozevírací nabídky, vyberte typ zásady přístupu, které chcete.</span><span class="sxs-lookup"><span data-stu-id="88a58-179">Then, in hello **Access** drop-down menu, select what type of access policy you want.</span></span>

    ![Název kontejneru](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > <span data-ttu-id="88a58-181">Ve výchozím kontejneru hello je privátní a můžete přistupovat pouze hello vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="88a58-181">By default, hello container is private and can only be accessed by hello account owner.</span></span> <span data-ttu-id="88a58-182">tooallow veřejný přístup pro čtení toohello objektů BLOB v kontejneru hello, ale toohello vlastnosti kontejneru a metadata, použijte hello **veřejného objektu Blob** možnost.</span><span class="sxs-lookup"><span data-stu-id="88a58-182">tooallow public read access toohello blobs in hello container, but not toohello container properties and metadata, use hello **Public Blob** option.</span></span> <span data-ttu-id="88a58-183">tooallow úplné veřejný přístup pro čtení hello kontejneru a objekty BLOB, použijte hello **veřejném kontejneru** možnost.</span><span class="sxs-lookup"><span data-stu-id="88a58-183">tooallow full public read access for hello container and blobs, use hello **Public Container** option.</span></span>
   >
   >

## <a name="step-3-prepare-hello-connection-tooazure"></a><span data-ttu-id="88a58-184">Krok 3: Příprava tooAzure připojení hello</span><span class="sxs-lookup"><span data-stu-id="88a58-184">Step 3: Prepare hello connection tooAzure</span></span>
<span data-ttu-id="88a58-185">Můžete nahrát soubor VHD, musíte tooestablish zabezpečené připojení mezi vaším počítačem a vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-185">Before you can upload a .vhd file, you need tooestablish a secure connection between your computer and your Azure subscription.</span></span> <span data-ttu-id="88a58-186">Můžete použít metoda hello Azure Active Directory (Azure AD), nebo hello certifikát metoda toodo ho.</span><span class="sxs-lookup"><span data-stu-id="88a58-186">You can use hello Azure Active Directory (Azure AD) method or hello certificate method toodo it.</span></span>

### <a name="use-hello-azure-ad-method-tooupload-a-vhd-file"></a><span data-ttu-id="88a58-187">Použít hello Azure AD metoda tooupload soubor VHD</span><span class="sxs-lookup"><span data-stu-id="88a58-187">Use hello Azure AD method tooupload a .vhd file</span></span>
1. <span data-ttu-id="88a58-188">Otevřete hello konzoly Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="88a58-188">Open hello Azure PowerShell console.</span></span>
2. <span data-ttu-id="88a58-189">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="88a58-189">Type hello following command:</span></span>  
    `Add-AzureAccount`

    <span data-ttu-id="88a58-190">Tento příkaz otevře okno přihlášení, kde se můžete přihlásit pomocí svého pracovního nebo školního účtu.</span><span class="sxs-lookup"><span data-stu-id="88a58-190">This command opens a sign-in window where you can sign in with your work or school account.</span></span>

    ![Okno prostředí PowerShell](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. <span data-ttu-id="88a58-192">Azure ověřuje a uloží hello přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="88a58-192">Azure authenticates and saves hello credential information.</span></span> <span data-ttu-id="88a58-193">Potom zavře okno hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-193">Then it closes hello window.</span></span>

### <a name="use-hello-certificate-method-tooupload-a-vhd-file"></a><span data-ttu-id="88a58-194">Použít hello certifikát metoda tooupload soubor VHD</span><span class="sxs-lookup"><span data-stu-id="88a58-194">Use hello certificate method tooupload a .vhd file</span></span>
1. <span data-ttu-id="88a58-195">Otevřete hello konzoly Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="88a58-195">Open hello Azure PowerShell console.</span></span>
2. <span data-ttu-id="88a58-196">Typ: `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="88a58-196">Type:  `Get-AzurePublishSettingsFile`.</span></span>
3. <span data-ttu-id="88a58-197">Okno prohlížeče se otevře a vyzve jste toodownload hello soubor .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="88a58-197">A browser window opens and prompts you toodownload hello .publishsettings file.</span></span> <span data-ttu-id="88a58-198">Tento soubor obsahuje informace a certifikát pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-198">This file contains information and a certificate for your Azure subscription.</span></span>

    ![Stránka pro stažení prohlížeče](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. <span data-ttu-id="88a58-200">Uložte soubor .publishsettings hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-200">Save hello .publishsettings file.</span></span>
5. <span data-ttu-id="88a58-201">Typ: `Import-AzurePublishSettingsFile <PathToFile>`, kde `<PathToFile>` je soubor .publishsettings toohello hello úplnou cestu.</span><span class="sxs-lookup"><span data-stu-id="88a58-201">Type:  `Import-AzurePublishSettingsFile <PathToFile>`, where `<PathToFile>` is hello full path toohello .publishsettings file.</span></span>

   <span data-ttu-id="88a58-202">Další informace najdete v tématu [Začínáme s Azure rutiny](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span><span class="sxs-lookup"><span data-stu-id="88a58-202">For more information, see [Get started with Azure cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span></span>

   <span data-ttu-id="88a58-203">Další informace o instalaci a konfiguraci prostředí PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="88a58-203">For more information about installing and configuring PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="step-4-upload-hello-vhd-file"></a><span data-ttu-id="88a58-204">Krok 4: Nahrání souboru VHD hello</span><span class="sxs-lookup"><span data-stu-id="88a58-204">Step 4: Upload hello .vhd file</span></span>
<span data-ttu-id="88a58-205">Při nahrávání souboru VHD hello, můžete je umístit kamkoli do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="88a58-205">When you upload hello .vhd file, you can place it anywhere within your Blob storage.</span></span> <span data-ttu-id="88a58-206">Tady jsou některé podmínky, které budete používat při nahrávání souboru hello:</span><span class="sxs-lookup"><span data-stu-id="88a58-206">Following are some terms you will use when you upload hello file:</span></span>

* <span data-ttu-id="88a58-207">**BlobStorageURL** hello adresa URL pro účet úložiště hello, kterou jste vytvořili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="88a58-207">**BlobStorageURL** is hello URL for hello storage account that you created in Step 2.</span></span>
* <span data-ttu-id="88a58-208">**YourImagesFolder** je hello kontejneru v úložiště objektů Blob místo toostore obrázků.</span><span class="sxs-lookup"><span data-stu-id="88a58-208">**YourImagesFolder** is hello container within Blob storage where you want toostore your images.</span></span>
* <span data-ttu-id="88a58-209">**VHDName** je hello štítek, který se zobrazí v Azure classic portálu tooidentify hello hello virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="88a58-209">**VHDName** is hello label that appears in hello Azure classic portal tooidentify hello virtual hard disk.</span></span>
* <span data-ttu-id="88a58-210">**PathToVHDFile** je hello úplná cesta a název souboru VHD hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-210">**PathToVHDFile** is hello full path and name of hello .vhd file.</span></span>

<span data-ttu-id="88a58-211">Hello okno Azure PowerShell, které jste použili v předchozí krok text hello zadejte:</span><span class="sxs-lookup"><span data-stu-id="88a58-211">From hello Azure PowerShell window you used in hello previous step, type:</span></span>

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-hello-uploaded-vhd-file"></a><span data-ttu-id="88a58-212">Krok 5: Vytvoření virtuálního počítače pomocí souboru nahrávat VHD hello</span><span class="sxs-lookup"><span data-stu-id="88a58-212">Step 5: Create a VM with hello uploaded .vhd file</span></span>
<span data-ttu-id="88a58-213">Po odeslání souboru VHD hello, můžete ji přidat jako seznam vlastních bitových kopií, které jsou spojeny s předplatným a vytvoření virtuálního počítače s touto bitovou kopií vlastní image toohello.</span><span class="sxs-lookup"><span data-stu-id="88a58-213">After you upload hello .vhd file, you can add it as an image toohello list of custom images that are associated with your subscription and create a virtual machine with this custom image.</span></span>

1. <span data-ttu-id="88a58-214">Hello okno Azure PowerShell, které jste použili v předchozí krok text hello zadejte:</span><span class="sxs-lookup"><span data-stu-id="88a58-214">From hello Azure PowerShell window you used in hello previous step, type:</span></span>

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of hello VHD> -OS <Type of hello OS on hello VHD>

   > [!NOTE]
   > <span data-ttu-id="88a58-215">Použijte jako typ hello operačního systému Linux.</span><span class="sxs-lookup"><span data-stu-id="88a58-215">Use Linux as hello OS type.</span></span> <span data-ttu-id="88a58-216">aktuální verzi prostředí Azure PowerShell Hello přijímá pouze "Linux" nebo "Systém Windows" jako parametr.</span><span class="sxs-lookup"><span data-stu-id="88a58-216">hello current Azure PowerShell version accepts only “Linux” or “Windows” as a parameter.</span></span>
   >
   >
2. <span data-ttu-id="88a58-217">Po dokončení předchozích kroků hello je uveden hello novou bitovou kopii, když zvolíte hello **bitové kopie** karty v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="88a58-217">After you complete hello previous steps, hello new image is listed when you choose hello **Images** tab on hello Azure classic portal.</span></span>  

    ![Vyberte bitovou kopii](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. <span data-ttu-id="88a58-219">Vytvoření virtuálního počítače z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-219">Create a virtual machine from hello gallery.</span></span> <span data-ttu-id="88a58-220">Tuto novou bitovou kopii je nyní k dispozici v části **Moje image**.</span><span class="sxs-lookup"><span data-stu-id="88a58-220">This new image is now available under **My Images**.</span></span>
4. <span data-ttu-id="88a58-221">Výběr nové image hello.</span><span class="sxs-lookup"><span data-stu-id="88a58-221">Select hello new image.</span></span> <span data-ttu-id="88a58-222">V dalším kroku projít hello výzvy tooset název hostitele, heslo, klíč SSH a tak dále.</span><span class="sxs-lookup"><span data-stu-id="88a58-222">Next, go through hello prompts tooset up a host name, password, SSH key, and so on.</span></span>

    ![Vlastní image](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. <span data-ttu-id="88a58-224">Po dokončení zřizování hello uvidíte FreeBSD virtuální počítač běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="88a58-224">After you complete hello provisioning, you'll see your FreeBSD VM running in Azure.</span></span>

    ![Obrázek FreeBSD v azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
