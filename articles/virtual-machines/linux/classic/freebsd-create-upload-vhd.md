---
title: "Vytvoření a nahrání image virtuálního počítače FreeBSD | Microsoft Docs"
description: "Naučte se vytvořit a odeslat virtuální pevný disk (VHD), který obsahuje operační systém FreeBSD k vytvoření virtuálního počítače Azure"
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
ms.openlocfilehash: 918f454784a9676297077c2e94c3e49ab2872d2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a><span data-ttu-id="c55eb-103">Vytvoření a nahrání virtuálního pevného disku FreeBSD do Azure</span><span class="sxs-lookup"><span data-stu-id="c55eb-103">Create and upload a FreeBSD VHD to Azure</span></span>
<span data-ttu-id="c55eb-104">Tento článek ukazuje, jak vytvořit a odeslat virtuální pevný disk (VHD), který obsahuje FreeBSD operační systém.</span><span class="sxs-lookup"><span data-stu-id="c55eb-104">This article shows you how to create and upload a virtual hard disk (VHD) that contains the FreeBSD operating system.</span></span> <span data-ttu-id="c55eb-105">Po odeslání, můžete ho použít jako vlastní image pro vytvoření virtuálního počítače (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-105">After you upload it, you can use it as your own image to create a virtual machine (VM) in Azure.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c55eb-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c55eb-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c55eb-107">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="c55eb-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="c55eb-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c55eb-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="c55eb-109">Informace o nahrávání virtuální pevný disk pomocí modelu Resource Manager najdete v tématu [zde](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c55eb-109">For information about uploading a VHD using the Resource Manager model, see [here](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c55eb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c55eb-110">Prerequisites</span></span>
<span data-ttu-id="c55eb-111">Tento článek předpokládá, že máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="c55eb-111">This article assumes that you have the following items:</span></span>

* <span data-ttu-id="c55eb-112">**Předplatné Azure**– Pokud nemáte účet, můžete vytvořit jeden si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="c55eb-112">**An Azure subscription**--If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="c55eb-113">Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="c55eb-113">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="c55eb-114">Jinak, zjistěte, jak [vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c55eb-114">Otherwise, learn how to [create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="c55eb-115">**Nástroje Azure PowerShell**– modul PowerShell Azure musí být nainstalované a nakonfigurované na používání vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-115">**Azure PowerShell tools**--The Azure PowerShell module must be installed and configured to use your Azure subscription.</span></span> <span data-ttu-id="c55eb-116">Si můžete stáhnout modul [Azure stáhne](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c55eb-116">To download the module, see [Azure downloads](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="c55eb-117">Zde jsou k dispozici kurz, který popisuje, jak nainstalovat a nakonfigurovat modul.</span><span class="sxs-lookup"><span data-stu-id="c55eb-117">A tutorial that describes how to install and configure the module is available here.</span></span> <span data-ttu-id="c55eb-118">Použití [Azure stáhne](https://azure.microsoft.com/downloads/) rutiny odeslání disku VHD.</span><span class="sxs-lookup"><span data-stu-id="c55eb-118">Use the [Azure Downloads](https://azure.microsoft.com/downloads/) cmdlet to upload the VHD.</span></span>
* <span data-ttu-id="c55eb-119">**FreeBSD operační systém nainstalovaný v souboru VHD**– podporované FreeBSD operačního systému musí být nainstalována na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="c55eb-119">**FreeBSD operating system installed in a .vhd file**--A supported   FreeBSD operating system must be installed to a virtual hard disk.</span></span> <span data-ttu-id="c55eb-120">Postup vytvoření souborů VHD existovat několik nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c55eb-120">Multiple tools exist to create .vhd files.</span></span> <span data-ttu-id="c55eb-121">Řešení virtualizace, jako je například technologie Hyper-V můžete například použít k vytvoření souboru VHD a instalace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c55eb-121">For example, you can use a virtualization solution such as Hyper-V to create the .vhd file and install the operating system.</span></span> <span data-ttu-id="c55eb-122">Pokyny o tom, jak nainstalovat a používat technologie Hyper-V najdete v tématu [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="c55eb-122">For instructions about how to install and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="c55eb-123">Novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-123">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="c55eb-124">Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo rutiny [convert-VHD prostředí](https://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="c55eb-124">You can convert the disk to VHD format by using Hyper-V Manager or the cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="c55eb-125">Kromě toho je [kurz na webu MSDN o tom, jak používat FreeBSD s technologií Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span><span class="sxs-lookup"><span data-stu-id="c55eb-125">In addition, there is a [tutorial on MSDN about how to use FreeBSD with Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span></span>
>
>

<span data-ttu-id="c55eb-126">Tato úloha obsahuje pět takto:</span><span class="sxs-lookup"><span data-stu-id="c55eb-126">This task includes the following five steps:</span></span>

## <a name="step-1-prepare-the-image-for-upload"></a><span data-ttu-id="c55eb-127">Krok 1: Příprava bitové kopie pro odeslání</span><span class="sxs-lookup"><span data-stu-id="c55eb-127">Step 1: Prepare the image for upload</span></span>
<span data-ttu-id="c55eb-128">Na virtuálním počítači, kam jste nainstalovali FreeBSD operačního systému proveďte následující postupy:</span><span class="sxs-lookup"><span data-stu-id="c55eb-128">On the virtual machine where you installed the FreeBSD operating system, complete the following procedures:</span></span>

1. <span data-ttu-id="c55eb-129">Povolte protokol DHCP.</span><span class="sxs-lookup"><span data-stu-id="c55eb-129">Enable DHCP.</span></span>

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. <span data-ttu-id="c55eb-130">Povolte SSH.</span><span class="sxs-lookup"><span data-stu-id="c55eb-130">Enable SSH.</span></span>

    <span data-ttu-id="c55eb-131">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="c55eb-131">Ensure that the SSH server is installed and configured to start at boot time.</span></span> <span data-ttu-id="c55eb-132">Ve výchozím nastavení je povoleno po instalaci z disku FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="c55eb-132">By default it is enabled after installation from FreeBSD disc.</span></span> 
3. <span data-ttu-id="c55eb-133">Nastavení konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="c55eb-133">Set up a serial console.</span></span>

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. <span data-ttu-id="c55eb-134">Nainstalujte sudo.</span><span class="sxs-lookup"><span data-stu-id="c55eb-134">Install sudo.</span></span>

    <span data-ttu-id="c55eb-135">Kořenový účet je zakázán v Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-135">The root account is disabled in Azure.</span></span> <span data-ttu-id="c55eb-136">To znamená, že potřebujete využívat sudo z Neprivilegovaný uživatelský ke spuštění příkazů se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="c55eb-136">This means you need to utilize sudo from an unprivileged user to run commands with elevated privileges.</span></span>

        # pkg install sudo
   
5. <span data-ttu-id="c55eb-137">Požadavky pro agenta Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-137">Prerequisites for Azure Agent.</span></span>

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. <span data-ttu-id="c55eb-138">Nainstalujte agenta Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-138">Install Azure Agent.</span></span>

    <span data-ttu-id="c55eb-139">Nejnovější verzi agenta Azure vždy najdete na [githubu](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="c55eb-139">The latest release of the Azure Agent can always be found on [github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="c55eb-140">Verze 2.0.10 + oficiálně podporuje FreeBSD 10 & 10.1 a verze 2.1.4 + (včetně 2.2.x) oficiálně podporuje FreeBSD 10.2 a pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="c55eb-140">The version 2.0.10 + officially supports FreeBSD 10 & 10.1, and the version 2.1.4 + (including 2.2.x) officially supports FreeBSD 10.2 and later releases.</span></span>

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    <span data-ttu-id="c55eb-141">Pro 2.0 použijeme 2.0.16 jako příklad:</span><span class="sxs-lookup"><span data-stu-id="c55eb-141">For 2.0, let's use 2.0.16 as an example:</span></span>

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    <span data-ttu-id="c55eb-142">Pro 2.1 použijeme 2.1.4 jako příklad:</span><span class="sxs-lookup"><span data-stu-id="c55eb-142">For 2.1, let's use 2.1.4 as an example:</span></span>

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > <span data-ttu-id="c55eb-143">Po instalaci agenta Azure, je vhodné ověřit, zda je spuštěna:</span><span class="sxs-lookup"><span data-stu-id="c55eb-143">After you install Azure Agent, it's a good idea to verify that it's running:</span></span>
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. <span data-ttu-id="c55eb-144">Zrušení zřízení systému.</span><span class="sxs-lookup"><span data-stu-id="c55eb-144">Deprovision the system.</span></span>

    <span data-ttu-id="c55eb-145">Zrušení zřízení systému a vyčistit ho a nastavit jej jako vhodný pro reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="c55eb-145">Deprovision the system to clean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="c55eb-146">Následující příkaz taky odstraní poslední zřízené uživatelský účet a přidružená data:</span><span class="sxs-lookup"><span data-stu-id="c55eb-146">The following command also deletes the last provisioned user account and the associated data:</span></span>

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    <span data-ttu-id="c55eb-147">Nyní můžete vypnout virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c55eb-147">Now you can shut down your VM.</span></span>

## <a name="step-2-create-a-storage-account-in-azure"></a><span data-ttu-id="c55eb-148">Krok 2: Vytvoření účtu úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="c55eb-148">Step 2: Create a storage account in Azure</span></span>
<span data-ttu-id="c55eb-149">Budete potřebovat účet úložiště v Azure a nahrát soubor VHD, proto ji můžete použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c55eb-149">You need a storage account in Azure to upload a .vhd file so it can be used to create a virtual machine.</span></span> <span data-ttu-id="c55eb-150">Portál Azure classic můžete použít k vytvoření účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c55eb-150">You can use the Azure classic portal to create a storage account.</span></span>

1. <span data-ttu-id="c55eb-151">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="c55eb-151">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="c55eb-152">Na panelu příkazů vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-152">On the command bar, select **New**.</span></span>
3. <span data-ttu-id="c55eb-153">Vyberte **datové služby** > **úložiště** > **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-153">Select **Data Services** > **Storage** > **Quick Create**.</span></span>

    ![Rychlé vytvoření účtu úložiště](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. <span data-ttu-id="c55eb-155">Vyplňte pole následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c55eb-155">Fill out the fields as follows:</span></span>

   * <span data-ttu-id="c55eb-156">V **URL** pole, zadejte název subdomény pro použití v adrese URL účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c55eb-156">In the **URL** field, type a subdomain name to use in the storage account URL.</span></span> <span data-ttu-id="c55eb-157">Položka může obsahovat ze 3 – 24 čísla a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="c55eb-157">The entry can contain from 3-24 numbers and lowercase letters.</span></span> <span data-ttu-id="c55eb-158">Tento název se změní na název hostitele v rámci adresu URL, která se používá k řešení úložiště objektů Blob v Azure, Azure Queue storage nebo Azure Table storage prostředky pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="c55eb-158">This name becomes the host name within the URL that is used to address Azure Blob storage, Azure Queue storage, or Azure Table storage resources for the subscription.</span></span>
   * <span data-ttu-id="c55eb-159">V **umístění/skupině vztahů** rozevírací nabídky vyberte **umístění nebo skupina vztahů** pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c55eb-159">In the **Location/Affinity Group** drop-down menu, choose the **location or affinity group** for the storage account.</span></span> <span data-ttu-id="c55eb-160">Skupiny vztahů umožňuje umístit cloudové služby a úložiště ve stejném datovém centru.</span><span class="sxs-lookup"><span data-stu-id="c55eb-160">An affinity group lets you put your cloud services and storage in the same data center.</span></span>
   * <span data-ttu-id="c55eb-161">V **replikace** pole, rozhodnout, jestli se má používat **geograficky redundantní** replikace pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c55eb-161">In the **Replication** field, decide whether to use **Geo-Redundant** replication for the storage account.</span></span> <span data-ttu-id="c55eb-162">Geografická replikace je zapnutá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c55eb-162">Geo-replication is turned on by default.</span></span> <span data-ttu-id="c55eb-163">Tato možnost replikuje data do sekundárního umístění, bez nákladů pro vás, tak, aby úložiště převezme do tohoto umístění, pokud dojde k selhání hlavní v primární lokalitě.</span><span class="sxs-lookup"><span data-stu-id="c55eb-163">This option replicates your data to a secondary location, at no cost to you, so that your storage fails over to that location if a major failure occurs at the primary location.</span></span> <span data-ttu-id="c55eb-164">Sekundární umístění bude přiřazena automaticky a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="c55eb-164">The secondary location is assigned automatically and can't be changed.</span></span> <span data-ttu-id="c55eb-165">Pokud potřebujete větší kontrolu nad umístění cloudové úložiště z důvodu právní požadavky nebo zásad organizace, můžete vypnout geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="c55eb-165">If you need more control over the location of your cloud-based storage due to legal requirements or organizational policy, you can turn off geo-replication.</span></span> <span data-ttu-id="c55eb-166">Však mějte na paměti, že pokud později zapnout geografická replikace, vám bude účtována úplatu přenos jednorázové data replikace existující data do sekundárního umístění.</span><span class="sxs-lookup"><span data-stu-id="c55eb-166">However, be aware that if you later turn on geo-replication, you will be charged a one-time data transfer fee to replicate your existing data to the secondary location.</span></span> <span data-ttu-id="c55eb-167">Služby úložiště bez geografická replikace se nabízí se slevou.</span><span class="sxs-lookup"><span data-stu-id="c55eb-167">Storage services without geo-replication are offered at a discount.</span></span> <span data-ttu-id="c55eb-168">Další informace o správě geografická replikace účtů úložiště najdete tady: [replikace Azure Storage](../../../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="c55eb-168">More details about managing geo-replication of storage accounts can be found here: [Azure Storage replication](../../../storage/common/storage-redundancy.md).</span></span>

     ![Zadejte podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. <span data-ttu-id="c55eb-170">Vyberte **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-170">Select **Create Storage Account**.</span></span> <span data-ttu-id="c55eb-171">Účet se teď zobrazí v části **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-171">The account now appears under **storage**.</span></span>

    ![Účet úložiště byl úspěšně vytvořen](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. <span data-ttu-id="c55eb-173">Dále vytvořte kontejner pro vaše soubory nahrávat VHD.</span><span class="sxs-lookup"><span data-stu-id="c55eb-173">Next, create a container for your uploaded .vhd files.</span></span> <span data-ttu-id="c55eb-174">Vyberte název účtu úložiště a pak vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-174">Select the storage account name, and then select **Containers**.</span></span>

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. <span data-ttu-id="c55eb-176">Vyberte **vytvořit kontejner**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-176">Select **Create a Container**.</span></span>

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. <span data-ttu-id="c55eb-178">V **název** pole, zadejte název vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c55eb-178">In the **Name** field, type a name for your container.</span></span> <span data-ttu-id="c55eb-179">Potom v **přístup** rozevírací nabídky, vyberte typ zásady přístupu, které chcete.</span><span class="sxs-lookup"><span data-stu-id="c55eb-179">Then, in the **Access** drop-down menu, select what type of access policy you want.</span></span>

    ![Název kontejneru](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > <span data-ttu-id="c55eb-181">Ve výchozím kontejneru je privátní a můžete přistupovat pouze vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="c55eb-181">By default, the container is private and can only be accessed by the account owner.</span></span> <span data-ttu-id="c55eb-182">Chcete-li povolit veřejný přístup pro čtení objektů BLOB v kontejneru, ale ne vlastnosti kontejneru a metadata, použijte **veřejného objektu Blob** možnost.</span><span class="sxs-lookup"><span data-stu-id="c55eb-182">To allow public read access to the blobs in the container, but not to the container properties and metadata, use the **Public Blob** option.</span></span> <span data-ttu-id="c55eb-183">Chcete-li povolit úplné veřejný přístup pro čtení pro kontejner a objekty BLOB, použijte **veřejném kontejneru** možnost.</span><span class="sxs-lookup"><span data-stu-id="c55eb-183">To allow full public read access for the container and blobs, use the **Public Container** option.</span></span>
   >
   >

## <a name="step-3-prepare-the-connection-to-azure"></a><span data-ttu-id="c55eb-184">Krok 3: Příprava připojení k Azure</span><span class="sxs-lookup"><span data-stu-id="c55eb-184">Step 3: Prepare the connection to Azure</span></span>
<span data-ttu-id="c55eb-185">Předtím, než můžete nahrát soubor VHD, budete muset navázat zabezpečené připojení mezi vaším počítačem a vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-185">Before you can upload a .vhd file, you need to establish a secure connection between your computer and your Azure subscription.</span></span> <span data-ttu-id="c55eb-186">Můžete to udělat metodu Azure Active Directory (Azure AD) nebo metodu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c55eb-186">You can use the Azure Active Directory (Azure AD) method or the certificate method to do it.</span></span>

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a><span data-ttu-id="c55eb-187">Použijte metodu Azure AD pro nahrání souboru VHD</span><span class="sxs-lookup"><span data-stu-id="c55eb-187">Use the Azure AD method to upload a .vhd file</span></span>
1. <span data-ttu-id="c55eb-188">Otevřete konzolu prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c55eb-188">Open the Azure PowerShell console.</span></span>
2. <span data-ttu-id="c55eb-189">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c55eb-189">Type the following command:</span></span>  
    `Add-AzureAccount`

    <span data-ttu-id="c55eb-190">Tento příkaz otevře okno přihlášení, kde se můžete přihlásit pomocí svého pracovního nebo školního účtu.</span><span class="sxs-lookup"><span data-stu-id="c55eb-190">This command opens a sign-in window where you can sign in with your work or school account.</span></span>

    ![Okno prostředí PowerShell](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. <span data-ttu-id="c55eb-192">Azure ověřuje a uloží informace o přihlašovacích údajích.</span><span class="sxs-lookup"><span data-stu-id="c55eb-192">Azure authenticates and saves the credential information.</span></span> <span data-ttu-id="c55eb-193">Potom zavře okno.</span><span class="sxs-lookup"><span data-stu-id="c55eb-193">Then it closes the window.</span></span>

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a><span data-ttu-id="c55eb-194">Pomocí této metody certifikát odeslat soubor VHD</span><span class="sxs-lookup"><span data-stu-id="c55eb-194">Use the certificate method to upload a .vhd file</span></span>
1. <span data-ttu-id="c55eb-195">Otevřete konzolu prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c55eb-195">Open the Azure PowerShell console.</span></span>
2. <span data-ttu-id="c55eb-196">Typ: `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="c55eb-196">Type:  `Get-AzurePublishSettingsFile`.</span></span>
3. <span data-ttu-id="c55eb-197">Otevře okno prohlížeče a výzva ke stažení souboru .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="c55eb-197">A browser window opens and prompts you to download the .publishsettings file.</span></span> <span data-ttu-id="c55eb-198">Tento soubor obsahuje informace a certifikát pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-198">This file contains information and a certificate for your Azure subscription.</span></span>

    ![Stránka pro stažení prohlížeče](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. <span data-ttu-id="c55eb-200">Uložte soubor .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="c55eb-200">Save the .publishsettings file.</span></span>
5. <span data-ttu-id="c55eb-201">Typ: `Import-AzurePublishSettingsFile <PathToFile>`, kde `<PathToFile>` je úplná cesta k souboru .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="c55eb-201">Type:  `Import-AzurePublishSettingsFile <PathToFile>`, where `<PathToFile>` is the full path to the .publishsettings file.</span></span>

   <span data-ttu-id="c55eb-202">Další informace najdete v tématu [Začínáme s Azure rutiny](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span><span class="sxs-lookup"><span data-stu-id="c55eb-202">For more information, see [Get started with Azure cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span></span>

   <span data-ttu-id="c55eb-203">Další informace o instalaci a konfiguraci prostředí PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c55eb-203">For more information about installing and configuring PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="step-4-upload-the-vhd-file"></a><span data-ttu-id="c55eb-204">Krok 4: Nahrání souboru VHD</span><span class="sxs-lookup"><span data-stu-id="c55eb-204">Step 4: Upload the .vhd file</span></span>
<span data-ttu-id="c55eb-205">Při nahrávání souboru VHD, můžete je umístit kamkoli do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="c55eb-205">When you upload the .vhd file, you can place it anywhere within your Blob storage.</span></span> <span data-ttu-id="c55eb-206">Tady jsou některé podmínky, které budete používat při nahrávání souboru:</span><span class="sxs-lookup"><span data-stu-id="c55eb-206">Following are some terms you will use when you upload the file:</span></span>

* <span data-ttu-id="c55eb-207">**BlobStorageURL** je adresa URL pro účet úložiště, který jste vytvořili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="c55eb-207">**BlobStorageURL** is the URL for the storage account that you created in Step 2.</span></span>
* <span data-ttu-id="c55eb-208">**YourImagesFolder** je kontejneru v úložiště objektů Blob, kam chcete uložit vaše Image.</span><span class="sxs-lookup"><span data-stu-id="c55eb-208">**YourImagesFolder** is the container within Blob storage where you want to store your images.</span></span>
* <span data-ttu-id="c55eb-209">**VHDName** je štítek, který se zobrazí na portálu Azure classic k identifikaci virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="c55eb-209">**VHDName** is the label that appears in the Azure classic portal to identify the virtual hard disk.</span></span>
* <span data-ttu-id="c55eb-210">**PathToVHDFile** je úplná cesta a název souboru VHD.</span><span class="sxs-lookup"><span data-stu-id="c55eb-210">**PathToVHDFile** is the full path and name of the .vhd file.</span></span>

<span data-ttu-id="c55eb-211">V okně Azure PowerShell, které jste použili v předchozím kroku zadejte:</span><span class="sxs-lookup"><span data-stu-id="c55eb-211">From the Azure PowerShell window you used in the previous step, type:</span></span>

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a><span data-ttu-id="c55eb-212">Krok 5: Vytvoření virtuálního počítače pomocí souboru nahrávat VHD</span><span class="sxs-lookup"><span data-stu-id="c55eb-212">Step 5: Create a VM with the uploaded .vhd file</span></span>
<span data-ttu-id="c55eb-213">Po odeslání souboru VHD, můžete ho přidat bitovou kopii do seznamu vlastních bitových kopií, které jsou spojeny s předplatným a vytvoření virtuálního počítače s touto bitovou kopií vlastní.</span><span class="sxs-lookup"><span data-stu-id="c55eb-213">After you upload the .vhd file, you can add it as an image to the list of custom images that are associated with your subscription and create a virtual machine with this custom image.</span></span>

1. <span data-ttu-id="c55eb-214">V okně Azure PowerShell, které jste použili v předchozím kroku zadejte:</span><span class="sxs-lookup"><span data-stu-id="c55eb-214">From the Azure PowerShell window you used in the previous step, type:</span></span>

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > <span data-ttu-id="c55eb-215">Použijte jako typ operačního systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c55eb-215">Use Linux as the OS type.</span></span> <span data-ttu-id="c55eb-216">Aktuální verzi prostředí Azure PowerShell přijímá pouze "Linux" nebo "Systém Windows" jako parametr.</span><span class="sxs-lookup"><span data-stu-id="c55eb-216">The current Azure PowerShell version accepts only “Linux” or “Windows” as a parameter.</span></span>
   >
   >
2. <span data-ttu-id="c55eb-217">Po dokončení předchozích kroků, je uvedena nová bitová kopie, když zvolíte **bitové kopie** karta na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="c55eb-217">After you complete the previous steps, the new image is listed when you choose the **Images** tab on the Azure classic portal.</span></span>  

    ![Vyberte bitovou kopii](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. <span data-ttu-id="c55eb-219">Vytvoření virtuálního počítače z galerie.</span><span class="sxs-lookup"><span data-stu-id="c55eb-219">Create a virtual machine from the gallery.</span></span> <span data-ttu-id="c55eb-220">Tuto novou bitovou kopii je nyní k dispozici v části **Moje image**.</span><span class="sxs-lookup"><span data-stu-id="c55eb-220">This new image is now available under **My Images**.</span></span>
4. <span data-ttu-id="c55eb-221">Vyberte novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="c55eb-221">Select the new image.</span></span> <span data-ttu-id="c55eb-222">V dalším kroku projít výzev a nastavte název hostitele, heslo, klíč SSH a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c55eb-222">Next, go through the prompts to set up a host name, password, SSH key, and so on.</span></span>

    ![Vlastní image](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. <span data-ttu-id="c55eb-224">Po dokončení zřizování, uvidíte FreeBSD virtuální počítač běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="c55eb-224">After you complete the provisioning, you'll see your FreeBSD VM running in Azure.</span></span>

    ![Obrázek FreeBSD v azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
