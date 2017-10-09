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
# <a name="move-a-windows-vm-from-amazon-web-services-aws-tooazure-using-powershell"></a>Přesunout virtuální počítač s Windows z tooAzure Amazon Web Services (AWS) pomocí prostředí PowerShell

Pokud hodnotíte virtuální počítače Azure pro hostování vašich úloh, můžete exportovat existující instanci virtuálního počítače Windows EC2 Amazon Web Services (AWS) pak odeslat tooAzure hello virtuální pevný disk (VHD). Jednou hello nahrání virtuálního pevného disku, můžete vytvořit nový virtuální počítač v Azure, z hello virtuálního pevného disku. 

Toto téma popisuje přesunutí jeden virtuální počítač z AWS tooAzure. Pokud chcete toomove virtuálních počítačů z AWS tooAzure ve velkém měřítku, najdete v části [migraci virtuálních počítačů v tooAzure Amazon Web Services (AWS) s Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-hello-vm"></a>Příprava hello virtuálních počítačů 
 
Můžete nahrát zobecněný i specializované tooAzure virtuální pevné disky. Každý typ vyžaduje, abyste připravili hello virtuálních počítačů před exportem z AWS. 

- **Zobecněný virtuální pevný disk** -zobecněný virtuální pevný disk má měl všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep. Pokud máte v úmyslu toouse hello virtuálního pevného disku jako image toocreate nové virtuální počítače z, měli byste: 
 
    * [Příprava systému Windows virtuálního počítače](prepare-for-upload-vhd-image.md).  
    * Generalize hello virtuálního počítače pomocí nástroje Sysprep.  

 
- **Specializuje virtuálního pevného disku** -specializované virtuálního pevného disku udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače. Pokud máte v úmyslu toouse hello virtuálního pevného disku jako-je toocreate nový virtuální počítač, je doplnit hello následující kroky.  
    * [Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md). **Nechcete** generalize hello virtuálního počítače pomocí nástroje Sysprep. 
    * Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalované na hello virtuálního počítače (tj. nástroje VMware). 
    * Ujistěte se, hello virtuální počítač je nakonfigurovaný toopull jeho IP adresu a nastavení DNS pomocí protokolu DHCP. Tím se zajistí, že tento server hello získá IP adresu v rámci hello virtuální síť při spuštění.  


## <a name="export-and-download-hello-vhd"></a>Export a stáhnout hello virtuálního pevného disku 

Exportujte hello EC2 instance tooa virtuálního pevného disku v sady Amazon S3. Postupujte podle kroků hello popsané v tématu dokumentace Amazon hello [export Instance jako virtuálních počítačů pomocí virtuálních počítačů importu a exportu](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) a spuštění hello [vytvořit instanci export úkolů](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) příkaz tooexport hello EC2 soubor VHD tooa instance. 

Hello exportovaný soubor virtuálního pevného disku se uloží do sady hello Amazon S3, které určíte. Hello základní syntaxe pro export hello virtuálního pevného disku je menší než, právě nahraďte zástupný symbol hello v <brackets> s informacemi.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

Jakmile byla exportována hello virtuálního pevného disku, postupujte podle pokynů hello v [jak lze stáhnout objekt z sady S3?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello virtuálního pevného disku soubor ze sady hello S3. 

> [!IMPORTANT]
> AWS účtuje poplatky přenos dat pro stahování hello virtuálního pevného disku. V tématu [Amazon S3 ceny](https://aws.amazon.com/s3/pricing/) Další informace.


## <a name="next-steps"></a>Další kroky

Teď můžete nahrát tooAzure hello virtuálního pevného disku a vytvořit nový virtuální počítač. 

- Pokud jste spustili nástroj Sysprep na svůj zdroj příliš**generalize** je před exportem, najdete v části [nahrát zobecněný virtuální pevný disk a použít ho toocreate nové virtuální počítače v Azure](upload-generalized-managed.md)
- Pokud před exportem nebyl spuštěn nástroj Sysprep, hello virtuálního pevného disku je považován za **specializované**, najdete v části [nahrát specializované tooAzure virtuální pevný disk a vytvořte nový virtuální počítač](create-vm-specialized.md)

 
