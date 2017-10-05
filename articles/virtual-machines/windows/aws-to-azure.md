---
title: "Přesunout virtuální počítače Windows AWS do Azure | Microsoft Docs"
description: "Přesuňte instanci Windows EC2 Amazon Web Services (AWS) pro virtuální počítače Azure pomocí Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: cynthn
ms.openlocfilehash: 7d2b498d3f84c4fd6cccf97c6d7781f293f5b395
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-azure-using-powershell"></a><span data-ttu-id="ebf3e-103">Přesunout virtuální počítač s Windows z Amazon Web Services (AWS) do Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ebf3e-103">Move a Windows VM from Amazon Web Services (AWS) to Azure using PowerShell</span></span>

<span data-ttu-id="ebf3e-104">Pokud hodnotíte virtuální počítače Azure pro hostování vašich úloh, můžete exportovat existující instanci virtuálního počítače Windows EC2 Amazon Web Services (AWS) pak nahrajte virtuální pevný disk (VHD) do Azure.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-104">If you are evaluating Azure virtual machines for hosting your workloads, you can export an existing Amazon Web Services (AWS) EC2 Windows VM instance then upload the virtual hard disk (VHD) to Azure.</span></span> <span data-ttu-id="ebf3e-105">Po nahrání virtuálního pevného disku můžete vytvořit nový virtuální počítač v Azure z disku VHD.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-105">Once the VHD is uploaded, you can create a new VM in Azure from the VHD.</span></span> 

<span data-ttu-id="ebf3e-106">Toto téma popisuje přesunutí jeden virtuální počítač z AWS do Azure.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-106">This topic covers moving a single VM from AWS to Azure.</span></span> <span data-ttu-id="ebf3e-107">Pokud chcete přesunout virtuální počítače z AWS do Azure ve velkém měřítku, přečtěte si téma [migraci virtuálních počítačů v Amazon Web Services (AWS) do Azure s Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ebf3e-107">If you want to move VMs from AWS to Azure at scale, see [Migrate virtual machines in Amazon Web Services (AWS) to Azure with Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span></span>

## <a name="prepare-the-vm"></a><span data-ttu-id="ebf3e-108">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ebf3e-108">Prepare the VM</span></span> 
 
<span data-ttu-id="ebf3e-109">Specializované i zobecněný virtuální pevné disky můžete nahrát do Azure.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-109">You can upload both generalized and specialized VHDs to Azure.</span></span> <span data-ttu-id="ebf3e-110">Každý typ vyžaduje, abyste připravili virtuální počítač před exportem z AWS.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-110">Each type requires that you prepare the VM before exporting from AWS.</span></span> 

- <span data-ttu-id="ebf3e-111">**Zobecněný virtuální pevný disk** -zobecněný virtuální pevný disk má měl všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-111">**Generalized VHD** - a generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="ebf3e-112">Pokud máte v úmyslu použít virtuální pevný disk jako bitovou kopii k vytvoření nové virtuální počítače z, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebf3e-112">If you intend to use the VHD as an image to create new VMs from, you should:</span></span> 
 
    * <span data-ttu-id="ebf3e-113">[Příprava systému Windows virtuálního počítače](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="ebf3e-113">[Prepare a Windows VM](prepare-for-upload-vhd-image.md).</span></span>  
    * <span data-ttu-id="ebf3e-114">Generalize virtuální počítač pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-114">Generalize the virtual machine using Sysprep.</span></span>  

 
- <span data-ttu-id="ebf3e-115">**Specializuje virtuálního pevného disku** -specializované virtuálního pevného disku uchovává uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-115">**Specialized VHD** - a specialized VHD maintains the user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="ebf3e-116">Pokud máte v úmyslu použít virtuální pevný disk jako-je chcete vytvořit nový virtuální počítač, zkontrolujte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-116">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span>  
    * <span data-ttu-id="ebf3e-117">[Příprava virtuálního pevného disku Windows nahrát do Azure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="ebf3e-117">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md).</span></span> <span data-ttu-id="ebf3e-118">**Nechcete** generalize virtuální počítač pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-118">**Do not** generalize the VM using Sysprep.</span></span> 
    * <span data-ttu-id="ebf3e-119">Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalovány do virtuálního počítače (tj. nástroje VMware).</span><span class="sxs-lookup"><span data-stu-id="ebf3e-119">Remove any guest virtualization tools and agents that are installed on the VM (i.e. VMware tools).</span></span> 
    * <span data-ttu-id="ebf3e-120">Zajistěte, aby že virtuální počítač nakonfigurovaný tak, aby jeho IP adresu a nastavení DNS pomocí protokolu DHCP pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-120">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="ebf3e-121">To zajistí, že server získá IP adresu v rámci virtuální sítě, při spuštění.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-121">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span>  


## <a name="export-and-download-the-vhd"></a><span data-ttu-id="ebf3e-122">Export a stáhnout virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="ebf3e-122">Export and download the VHD</span></span> 

<span data-ttu-id="ebf3e-123">Exportujte EC2 instance virtuálního pevného disku v sady Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-123">Export the EC2 instance to a VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="ebf3e-124">Postupujte podle kroků popsaných v tématu dokumentace Amazon [export Instance jako virtuálních počítačů pomocí virtuálních počítačů importu a exportu](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) a spusťte [vytvořit instanci export úkolů](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) příkaz pro export EC2 instance do souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-124">Follow the steps described in the Amazon documentation topic [Exporting an Instance as a VM Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) and run the [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) command to export the EC2 instance to a VHD file.</span></span> 

<span data-ttu-id="ebf3e-125">Exportovaný soubor virtuálního pevného disku se uloží do sady Amazon S3, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-125">The exported VHD file is saved in the Amazon S3 bucket you specify.</span></span> <span data-ttu-id="ebf3e-126">Základní syntaxe pro export virtuálního pevného disku je menší než, nahraďte zástupný text v <brackets> s informacemi.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-126">The basic syntax for exporting the VHD is below, just replace the placeholder text in <brackets> with your information.</span></span>

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

<span data-ttu-id="ebf3e-127">Jakmile byla exportována virtuálního pevného disku, postupujte podle pokynů v [jak lze stáhnout objekt z sady S3?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) ke stažení souboru virtuálního pevného disku z sady S3.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-127">Once the VHD has been exported, follow the instructions in [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) to download the VHD file from the S3 bucket.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="ebf3e-128">Přenos dat poplatky AWS poplatky za stahování virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-128">AWS charges data transfer fees for downloading the VHD.</span></span> <span data-ttu-id="ebf3e-129">V tématu [Amazon S3 ceny](https://aws.amazon.com/s3/pricing/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-129">See [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/) for more information.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ebf3e-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebf3e-130">Next steps</span></span>

<span data-ttu-id="ebf3e-131">Nyní můžete nahrávat VHD do Azure a vytvořit nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ebf3e-131">Now you can upload the VHD to Azure and create a new VM.</span></span> 

- <span data-ttu-id="ebf3e-132">Pokud jste spustili nástroj Sysprep na svůj zdroj k **generalize** je před exportem, najdete v části [nahrát zobecněný virtuální pevný disk a použít ho k vytvoření nové virtuální počítače v Azure](upload-generalized-managed.md)</span><span class="sxs-lookup"><span data-stu-id="ebf3e-132">If you ran Sysprep on your source to **generalize** it before exporting, see [Upload a generalized VHD and use it to create a new VMs in Azure](upload-generalized-managed.md)</span></span>
- <span data-ttu-id="ebf3e-133">Pokud před exportem nebyl spuštěn nástroj Sysprep, je považován za virtuální pevný disk **specializované**, najdete v části [nahrát specializované VHD do Azure a vytvoření nového virtuálního počítače](create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="ebf3e-133">If you did not run Sysprep before exporting, the VHD is considered **specialized**, see [Upload a specialized VHD to Azure and create a new VM](create-vm-specialized.md)</span></span>

 
