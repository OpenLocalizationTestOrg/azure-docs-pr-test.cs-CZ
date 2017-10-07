---
title: "aaaAzure podporu správce prostředků nástroje pro vyrovnávání zatížení | Microsoft Docs"
description: "Pomocí prostředí powershell pro službu Vyrovnávání zatížení s Azure Resource Manager. Pomocí šablony pro vyrovnávání zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3c02d9382de00fefe6af8f4f8a2b7b62b344f285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Podporu správce prostředků Azure pomocí nástroje pro vyrovnávání zatížení Azure

Azure Resource Manager je architektura hello upřednostňované správy pro služby v Azure. Azure nástroj pro vyrovnávání zatížení můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.

## <a name="concepts"></a>Koncepty

S Resource Managerem se nástroj pro vyrovnávání zatížení Azure obsahuje hello následující podřízené prostředky:

* Konfiguraci front-end IP adresy – nástroj pro vyrovnávání zatížení může obsahovat jednu nebo více adres IP front-endu známé jako virtuální IP adresy (VIP). Tyto IP adresy sloužit jako příchozího provozu hello.
* Fond adres back-end – Toto jsou IP adresy přidružené hello virtuálního počítače budou distribuována toowhich zatížení sítě rozhraní karta (NIC).
* Vyrovnávání zatížení pravidla – vlastnost rule mapuje danou front-end IP a portu kombinace tooa sadu back-end IP adresy a portu kombinaci. Nástroj pro vyrovnávání zatížení může mít několik pravidel vyrovnávání zatížení. Každé pravidlo je kombinací front-endové IP adresy a portu a back-endové IP adresy a portu přidružených k virtuálním počítačům.
* Sondy – povolit sondy jste tookeep sledovat hello stav instancí virtuálních počítačů. Pokud selže test stavu, instance virtuálního počítače hello se provedou mimo otočení automaticky.
* Příchozích pravidel NAT pravidla – definování pravidel NAT hello příchozí provoz v IP front-endu hello a distribuované toohello zpět koncovou IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Šablony Rychlý start

Azure Resource Manager můžete tooprovision vaší aplikace s použitím deklarativní šablony. S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi. Použít hello stejné šablony toorepeatedly nasazení aplikace během každé fáze životního cyklu aplikace hello.

Šablony může obsahovat definice pro virtuální počítače, virtuální sítě, skupiny dostupnosti, síťových rozhraní (NIC), účty úložiště, nástroje pro vyrovnávání zatížení, skupiny zabezpečení sítě a veřejné IP adresy. Pomocí šablony můžete vytvářet vše potřebné pro komplexní aplikace. soubor šablony Hello lze zkontrolovat do systému správy obsahu pro verzí a spolupráci.

[Další informace o šablonách](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Další informace o síťové prostředky](../virtual-network/resource-groups-networking.md)

Šablony rychlý start používá nástroj pro vyrovnávání zatížení Azure lze nalézt v [úložiště GitHub](https://github.com/Azure/azure-quickstart-templates) hostování sadu komunity generované šablon.

Příklady šablon:

* [2 virtuální počítače v Vyrovnávání zatížení a pravidel Vyrovnávání zatížení](http://go.microsoft.com/fwlink/?LinkId=544799)
* [2 virtuální počítače ve virtuální síti s pravidly interní nástroj pro vyrovnávání zatížení a nástroj pro vyrovnávání zatížení](http://go.microsoft.com/fwlink/?LinkId=544800)
* [2 virtuální počítače ve službě Vyrovnávání zatížení a nakonfigurovat pravidla NAT na hello LB](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Nastavení služby Vyrovnávání zatížení Azure, PowerShell nebo rozhraní příkazového řádku

Začínáme s rutiny Azure Resource Manager, nástroje příkazového řádku a rozhraní REST API

* [Rutiny Azure sítě](https://msdn.microsoft.com/library/azure/mt163510.aspx) lze použít toocreate nástroj pro vyrovnávání zatížení.
* [Jak toocreate Vyrovnávání zatížení pomocí Azure Resource Manager](load-balancer-get-started-ilb-arm-ps.md)
* [Správa prostředků Azure pomocí hello rozhraní příkazového řádku Azure](../xplat-cli-azure-resource-manager.md)
* [Rozhraní REST API nástroj pro vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Další kroky

Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) pro konkrétní zatížení vyrovnávání síťové přenosy chování.

Zjistěte, jak toomanage [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). To je důležité, pokud aplikace potřebuje tookeep připojení zachování připojení pro servery za službou Vyrovnávání zatížení.
