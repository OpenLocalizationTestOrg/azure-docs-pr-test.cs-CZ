---
title: "aaaMove tooAzure virtuálních počítačů AWS Windows | Microsoft Docs"
description: "Přesuňte instanci tooAzure Windows EC2 Amazon Web Services (AWS) virtuálních počítačů pomocí Azure PowerShell."
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
ms.openlocfilehash: f912c28d3ffe585162c3add715a1318ac3cd4643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-tooazure-using-powershell"></a><span data-ttu-id="29a39-103">Přesunout virtuální počítač s Windows z tooAzure Amazon Web Services (AWS) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="29a39-103">Move a Windows VM from Amazon Web Services (AWS) tooAzure using PowerShell</span></span>

<span data-ttu-id="29a39-104">Pokud hodnotíte virtuální počítače Azure pro hostování vašich úloh, můžete exportovat existující instanci virtuálního počítače Windows EC2 Amazon Web Services (AWS) pak odeslat tooAzure hello virtuální pevný disk (VHD).</span><span class="sxs-lookup"><span data-stu-id="29a39-104">If you are evaluating Azure virtual machines for hosting your workloads, you can export an existing Amazon Web Services (AWS) EC2 Windows VM instance then upload hello virtual hard disk (VHD) tooAzure.</span></span> <span data-ttu-id="29a39-105">Jednou hello nahrání virtuálního pevného disku, můžete vytvořit nový virtuální počítač v Azure, z hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="29a39-105">Once hello VHD is uploaded, you can create a new VM in Azure from hello VHD.</span></span> 

<span data-ttu-id="29a39-106">Toto téma popisuje přesunutí jeden virtuální počítač z AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="29a39-106">This topic covers moving a single VM from AWS tooAzure.</span></span> <span data-ttu-id="29a39-107">Pokud chcete toomove virtuálních počítačů z AWS tooAzure ve velkém měřítku, najdete v části [migraci virtuálních počítačů v tooAzure Amazon Web Services (AWS) s Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="29a39-107">If you want toomove VMs from AWS tooAzure at scale, see [Migrate virtual machines in Amazon Web Services (AWS) tooAzure with Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span></span>

## <a name="prepare-hello-vm"></a><span data-ttu-id="29a39-108">Příprava hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="29a39-108">Prepare hello VM</span></span> 
 
<span data-ttu-id="29a39-109">Můžete nahrát zobecněný i specializované tooAzure virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="29a39-109">You can upload both generalized and specialized VHDs tooAzure.</span></span> <span data-ttu-id="29a39-110">Každý typ vyžaduje, abyste připravili hello virtuálních počítačů před exportem z AWS.</span><span class="sxs-lookup"><span data-stu-id="29a39-110">Each type requires that you prepare hello VM before exporting from AWS.</span></span> 

- <span data-ttu-id="29a39-111">**Zobecněný virtuální pevný disk** -zobecněný virtuální pevný disk má měl všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="29a39-111">**Generalized VHD** - a generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="29a39-112">Pokud máte v úmyslu toouse hello virtuálního pevného disku jako image toocreate nové virtuální počítače z, měli byste:</span><span class="sxs-lookup"><span data-stu-id="29a39-112">If you intend toouse hello VHD as an image toocreate new VMs from, you should:</span></span> 
 
    * <span data-ttu-id="29a39-113">[Příprava systému Windows virtuálního počítače](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="29a39-113">[Prepare a Windows VM](prepare-for-upload-vhd-image.md).</span></span>  
    * <span data-ttu-id="29a39-114">Generalize hello virtuálního počítače pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="29a39-114">Generalize hello virtual machine using Sysprep.</span></span>  

 
- <span data-ttu-id="29a39-115">**Specializuje virtuálního pevného disku** -specializované virtuálního pevného disku udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="29a39-115">**Specialized VHD** - a specialized VHD maintains hello user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="29a39-116">Pokud máte v úmyslu toouse hello virtuálního pevného disku jako-je toocreate nový virtuální počítač, je doplnit hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="29a39-116">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span>  
    * <span data-ttu-id="29a39-117">[Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="29a39-117">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span></span> <span data-ttu-id="29a39-118">**Nechcete** generalize hello virtuálního počítače pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="29a39-118">**Do not** generalize hello VM using Sysprep.</span></span> 
    * <span data-ttu-id="29a39-119">Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalované na hello virtuálního počítače (tj. nástroje VMware).</span><span class="sxs-lookup"><span data-stu-id="29a39-119">Remove any guest virtualization tools and agents that are installed on hello VM (i.e. VMware tools).</span></span> 
    * <span data-ttu-id="29a39-120">Ujistěte se, hello virtuální počítač je nakonfigurovaný toopull jeho IP adresu a nastavení DNS pomocí protokolu DHCP.</span><span class="sxs-lookup"><span data-stu-id="29a39-120">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="29a39-121">Tím se zajistí, že tento server hello získá IP adresu v rámci hello virtuální síť při spuštění.</span><span class="sxs-lookup"><span data-stu-id="29a39-121">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span>  


## <a name="export-and-download-hello-vhd"></a><span data-ttu-id="29a39-122">Export a stáhnout hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="29a39-122">Export and download hello VHD</span></span> 

<span data-ttu-id="29a39-123">Exportujte hello EC2 instance tooa virtuálního pevného disku v sady Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="29a39-123">Export hello EC2 instance tooa VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="29a39-124">Postupujte podle kroků hello popsané v tématu dokumentace Amazon hello [export Instance jako virtuálních počítačů pomocí virtuálních počítačů importu a exportu](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) a spuštění hello [vytvořit instanci export úkolů](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) příkaz tooexport hello EC2 soubor VHD tooa instance.</span><span class="sxs-lookup"><span data-stu-id="29a39-124">Follow hello steps described in hello Amazon documentation topic [Exporting an Instance as a VM Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) and run hello [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) command tooexport hello EC2 instance tooa VHD file.</span></span> 

<span data-ttu-id="29a39-125">Hello exportovaný soubor virtuálního pevného disku se uloží do sady hello Amazon S3, které určíte.</span><span class="sxs-lookup"><span data-stu-id="29a39-125">hello exported VHD file is saved in hello Amazon S3 bucket you specify.</span></span> <span data-ttu-id="29a39-126">Hello základní syntaxe pro export hello virtuálního pevného disku je menší než, právě nahraďte zástupný symbol hello v <brackets> s informacemi.</span><span class="sxs-lookup"><span data-stu-id="29a39-126">hello basic syntax for exporting hello VHD is below, just replace hello placeholder text in <brackets> with your information.</span></span>

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

<span data-ttu-id="29a39-127">Jakmile byla exportována hello virtuálního pevného disku, postupujte podle pokynů hello v [jak lze stáhnout objekt z sady S3?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello virtuálního pevného disku soubor ze sady hello S3.</span><span class="sxs-lookup"><span data-stu-id="29a39-127">Once hello VHD has been exported, follow hello instructions in [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello VHD file from hello S3 bucket.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="29a39-128">AWS účtuje poplatky přenos dat pro stahování hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="29a39-128">AWS charges data transfer fees for downloading hello VHD.</span></span> <span data-ttu-id="29a39-129">V tématu [Amazon S3 ceny](https://aws.amazon.com/s3/pricing/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="29a39-129">See [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/) for more information.</span></span>


## <a name="next-steps"></a><span data-ttu-id="29a39-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29a39-130">Next steps</span></span>

<span data-ttu-id="29a39-131">Teď můžete nahrát tooAzure hello virtuálního pevného disku a vytvořit nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="29a39-131">Now you can upload hello VHD tooAzure and create a new VM.</span></span> 

- <span data-ttu-id="29a39-132">Pokud jste spustili nástroj Sysprep na svůj zdroj příliš**generalize** je před exportem, najdete v části [nahrát zobecněný virtuální pevný disk a použít ho toocreate nové virtuální počítače v Azure](upload-generalized-managed.md)</span><span class="sxs-lookup"><span data-stu-id="29a39-132">If you ran Sysprep on your source too**generalize** it before exporting, see [Upload a generalized VHD and use it toocreate a new VMs in Azure](upload-generalized-managed.md)</span></span>
- <span data-ttu-id="29a39-133">Pokud před exportem nebyl spuštěn nástroj Sysprep, hello virtuálního pevného disku je považován za **specializované**, najdete v části [nahrát specializované tooAzure virtuální pevný disk a vytvořte nový virtuální počítač](create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="29a39-133">If you did not run Sysprep before exporting, hello VHD is considered **specialized**, see [Upload a specialized VHD tooAzure and create a new VM](create-vm-specialized.md)</span></span>

 
