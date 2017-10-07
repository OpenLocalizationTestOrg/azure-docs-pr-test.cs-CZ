---
title: "aaaUpload virtuálního pevného disku soubor tooAzure DevTest Labs pomocí prostředí PowerShell | Microsoft Docs"
description: "Nahrání virtuálního pevného disku soubor toolab účet úložiště pomocí prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9c3ee96e212457b0ef8203714b419350cb97f895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-powershell"></a>Nahrání virtuálního pevného disku soubor toolab účet úložiště pomocí prostředí PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

V Azure DevTest Labs může být soubory VHD použité toocreate vlastní Image, které jsou používané tooprovision virtuální počítače. Hello následující kroky vás provedou pomocí prostředí PowerShell tooupload virtuálního pevného disku soubor tooa testovacího prostředí na účet úložiště. Jakmile jste odeslali souboru virtuálního pevného disku, hello [další kroky části](#next-steps) jsou uvedeny některé články, které ilustrují, jak toocreate vlastní image z hello nahrát soubor virtuálního pevného disku. Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Hello následující kroky vás odesílání virtuálního pevného disku soubor tooAzure DevTest Labs pomocí prostředí PowerShell procházení. 

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.

1. Ze seznamu hello labs vyberte požadované prostředí hello.  

1. V okně prostředí hello vyberte **konfigurace**. 

1. V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.

1. Na hello **vlastní image** okně vyberte **+ přidat**. 

1. Na hello **vlastní image** vyberte **virtuálního pevného disku**.

1. Na hello **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Na hello **Odeslat bitovou kopii pomocí prostředí PowerShell** okno, kopie hello generované prostředí PowerShell skriptu tooa textový editor.

1. Upravit hello **LocalFilePath** parametr hello **přidat AzureVhd** rutiny toopoint toohello umístění hello chcete tooupload souboru virtuálního pevného disku.

1. V řádku prostředí PowerShell, spusťte hello **přidat AzureVhd** rutiny (s hello upravit **LocalFilePath** parametr).

> [!WARNING] 
> 
> Hello proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti hello hello souboru virtuálního pevného disku a rychlost připojení.

## <a name="next-steps"></a>Další kroky

- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí hello portálu Azure](devtest-lab-create-template.md)
- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
