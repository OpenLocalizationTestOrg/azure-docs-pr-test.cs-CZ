---
title: "bitové kopie aaaSelect virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí Azure PowerSHell toodetermine hello vydavatele, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 752edcd0935f5141832e49503ae800ea0145e219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-windows-vm-images-in-hello-azure-marketplace-with-azure-powershell"></a>Jak Image toofind virtuální počítač s Windows v nástroji hello Azure Marketplace s prostředím Azure PowerShell

Toto téma popisuje, jak Image toouse prostředí Azure PowerShell toofind virtuálních počítačů v nástroji hello Azure Marketplace. Při vytvoření virtuálního počítače s Windows, použijte tento informace toospecify image pořízenou prostřednictvím Marketplace.

Ujistěte se, zda je nainstalován a nakonfigurován hello nejnovější [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>Tabulka běžně používané bitových kopií systému Windows
| Název vydavatele | Nabídka | Skladová jednotka (SKU) |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter Server Core |
| MicrosoftWindowsServer |WindowsServer |2016 datového centra s kontejnery |
| MicrosoftWindowsServer |WindowsServer |2016. Nano Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |AKTUALIZACE SP1 2008 R2 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016 WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2 WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Vyhledání konkrétních imagí


Při vytváření nového virtuálního počítače pomocí Azure Resource Manageru, v některých případech je třeba toospecify bitovou kopii s kombinací hello hello následující vlastnosti bitové kopie:

* Vydavatel
* Nabídka
* Skladová jednotka (SKU)

Například použít tyto hodnoty s hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny prostředí PowerShell nebo pomocí šablony skupiny prostředků v které je nutné zadat typ hello toobe virtuální počítač vytvořit.

Pokud potřebujete toodetermine tyto hodnoty, můžete spustit hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), a [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) rutiny toonavigate hello Image. Můžete určit tyto hodnoty:

1. Seznam vydavatelů image hello.
2. Pro daného vydavatele vypsat jeho nabídky.
3. Pro danou nabídku vypsat její skladovou jednotku (SKU).

Nejprve seznam vydavatelů hello s hello následující příkazy:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Zadejte název vydavatele zvolené a spusťte následující příkazy hello:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Zadejte název vaší zvolené nabídka a spusťte následující příkazy hello:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Z hello výstup hello `Get-AzureRMVMImageSku` příkaz, budete mít všechny informace o hello potřebujete toospecify hello image pro nový virtuální počítač.

Úplný příklad ukazuje, Hello následující:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Výstup:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Pro vydavatele "MicrosoftWindowsServer" hello:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Výstup:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

Pro nabídka "Windows Server" hello:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Výstup:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Z tohoto seznamu, zkopírujte hello zvolený název SKU, a budete mít všechny informace hello hello `Set-AzureRMVMSourceImage` rutiny prostředí PowerShell nebo pro šablonu skupiny prostředků.

## <a name="next-steps"></a>Další kroky
Teď můžete zvolit přesněji hello obraz chcete toouse. najdete v části virtuální počítač rychle pomocí informace o obrázku hello, který jste právě najít, toocreate [vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](quick-create-powershell.md).
