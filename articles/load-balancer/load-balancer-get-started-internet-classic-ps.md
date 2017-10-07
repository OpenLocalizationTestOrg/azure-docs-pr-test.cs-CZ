---
title: "aaaCreate směřujících Internetu pro vyrovnávání zátěže – prostředí PowerShell Azure classic | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže v klasickém režimu pomocí prostředí PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 76d9a712a0acda223fc86b80be9c35c0ed9f3a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení (Classic) v prostředí PowerShell

> [!div class="op_single_selector"]
> * [Portál Azure Classic](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md). Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku. Tento článek se týká modelu nasazení classic hello. Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>Nastavení nástroje pro vyrovnávání zatížení pomocí prostředí PowerShell

tooset si nástroj pro vyrovnávání zatížení pomocí prostředí powershell, postupujte podle kroků hello níže:

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.
2. Po vytvoření virtuálního počítače, můžete použít tooadd rutiny prostředí PowerShell nástroje pro vyrovnávání zatížení tooa virtuálního počítače v rámci hello stejné cloudové služby.

V hello následující příklad, že přidáte skupinu nástroje pro vyrovnávání zatížení názvem toocloud "webová farma" služby "mytestcloud" (nebo myctestcloud.cloudapp.net), přidání toovirtual nástroje pro vyrovnávání zátěže hello koncové body pro hello počítačů s názvem "web1" a "web2". Hello nástroj pro vyrovnávání zatížení přijímá síťový provoz na portu 80 a vyrovnává zatížení mezi virtuálními počítači hello definované hello místní koncový bod (v této případu port 80) pomocí protokolu TCP.

### <a name="step-1"></a>Krok 1

Vytvořte skupinu s vyrovnáváním zatížení koncový bod pro server web1"hello první virtuální počítač"

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>Krok 2

Vytvořte jiný koncový bod pro hello druhý virtuální počítač "webu 2" hello pomocí stejné načíst název sady vyrovnávání

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení

Můžete odebrat AzureEndpoint tooremove koncový bod virtuálního počítače z nástroje pro vyrovnávání zatížení hello

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a>Další kroky

Můžete také [začít vytvářet interní nástroj pro vyrovnávání zatížení](load-balancer-get-started-ilb-classic-ps.md) a nakonfigurovat, jaký typ [distribučního režimu](load-balancer-distribution-mode.md) se má použít pro konkrétní chování nástroje pro vyrovnávání zatížení síťového provozu.

Pokud aplikace potřebuje tookeep připojení zachování připojení pro servery za službou Vyrovnávání zatížení, můžete lépe porozumět o [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). Pokud používáte nástroj pro vyrovnávání zatížení Azure ho pomůže toolearn o chování nečinné připojení.
