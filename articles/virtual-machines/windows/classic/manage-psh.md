---
title: "aaaManage virtuálních počítačů pomocí Azure PowerShell | Microsoft Docs"
description: "Další příkazy, které můžete použít tooautomate úkoly při správě virtuálních počítačů."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: e4ca6f098519243a321eac98b6692790fe18c22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Správa virtuálních počítačů pomocí Azure PowerShellu
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Pro běžné příkazy prostředí PowerShell pomocí modelu Resource Manager hello, najdete v části [zde](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Pomocí rutin prostředí Azure PowerShell je možné automatizovat celou řadu úloh dělat každý den toomanage virtuální počítače. Tento článek vám příklady příkazů pro jednodušší úlohy a tooarticles odkazy, který zobrazí hello příkazy pro složitější úlohy.

> [!NOTE]
> Pokud ještě nainstalován a nakonfigurován prostředí Azure PowerShell ještě můžete získat pokyny v článku hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
> 
> 

## <a name="how-toouse-hello-example-commands"></a>Jak toouse hello příklady příkazů
Budete potřebovat tooreplace některé z textu hello v hello příkazy pomocí text, který je vhodný pro vaše prostředí. Hello < a > symboly indikovat potřebujete tooreplace text. Při nahrazení textu hello odebrat hello symboly, ale ponechte hello uvozovek nahoře na místě.

## <a name="get-a-vm"></a>Získat virtuální počítač
Toto je základní úlohy, které budete používat často. Používat tooget informace o virtuální počítač, provádět úlohy na virtuálním počítači nebo získat výstup toostore v proměnné.

tooget informace o hello virtuálního počítače, spusťte tento příkaz, přičemž vše v uvozovkách hello, včetně hello < a > znaků:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

hello toostore výstup do proměnné $vm, spusťte:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-tooa-windows-based-vm"></a>Přihlaste se tooa virtuálních počítačů na bázi Windows
Spusťte tyto příkazy:

> [!NOTE]
> Hello virtuálního počítače a název cloudové služby můžete získat z hello zobrazení hello **Get-AzureVM** příkaz.
> 
> $svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "< jednotku a složku umístění toostore hello stáhnout soubor RDP, například: c:\temp >" $localFile = $localPath + "\" + $vmname +".rdp"Get-AzureRemoteDesktopFile - ServiceName $svcName-název $vmName - Místnícesta $localFile – spuštění
> 
> 

## <a name="stop-a-vm"></a>Zastavení virtuálního počítače
Spusťte tento příkaz:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Použijte tento parametr tookeep hello virtuální IP (VIP) hello cloudu služeb v případě, že je hello poslední virtuální počítač v rámci této cloudové služby. <br><br> Pokud použijete parametr StayProvisioned hello, stále platit budete pro hello virtuálních počítačů.
> 
> 

## <a name="start-a-vm"></a>Spuštění virtuálního počítače
Spusťte tento příkaz:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Připojení datového disku
Tato úloha vyžaduje několik kroků. Nejprve pomocí hello *** Add-AzureDataDisk *** tooadd hello disku toohello $vm a jeho objekt rutiny. Potom použít **aktualizace-AzureVM** rutiny tooupdate hello konfiguraci hello virtuálních počítačů.

Budete také potřebovat toodecide zda tooattach nový disk nebo jeden, který obsahuje data. Pro nový disk hello příkaz vytvoří soubor VHD hello a připojí jej.

tooattach nový disk, spusťte tento příkaz:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

tooattach stávající datový disk, spusťte tento příkaz:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

tooattach datové disky z existujícího souboru VHD v úložišti objektů blob, spusťte tento příkaz:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Vytvoření virtuálního počítače založené na systému Windows
toocreate nového systému Windows virtuálního počítače v Azure, použijte pokyny hello v [toocreate pomocí Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](create-powershell.md). Tento postup téma vás provede hello vytvoření prostředí Azure PowerShell sady příkazů, který vytvoří virtuální počítač systému Windows, který může být předkonfigurované:

* S členství v doméně služby Active Directory.
* S další disky.
* Jako člen existující Vyrovnávání zatížení sítě nastavit.
* Pomocí statické IP adresy.

