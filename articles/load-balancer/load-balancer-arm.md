---
title: "Podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení | Microsoft Docs"
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
ms.openlocfilehash: d06c924f384a2684b5a91c202039c581796c1091
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Podporu správce prostředků Azure pomocí nástroje pro vyrovnávání zatížení Azure

Azure Resource Manager je architektura upřednostňované správy pro služby v Azure. Azure nástroj pro vyrovnávání zatížení můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.

## <a name="concepts"></a>Koncepty

S Resource Managerem se nástroj pro vyrovnávání zatížení Azure obsahuje následující podřízené prostředky:

* Konfiguraci front-end IP adresy – nástroj pro vyrovnávání zatížení může obsahovat jednu nebo více adres IP front-endu známé jako virtuální IP adresy (VIP). Tyto IP adresy slouží jako vstup pro přenos.
* Fond adres back-end – Toto jsou IP adresy, které jsou spojené s virtuální počítač síťové rozhraní karta (NIC) do které budou distribuována zatížení.
* Pravidla Vyrovnávání zatížení – vlastnost rule mapuje danou front-end IP a portu kombinace na sadu back-end IP adresy a portu. Nástroj pro vyrovnávání zatížení může mít několik pravidel vyrovnávání zatížení. Každé pravidlo je kombinací front-endové IP adresy a portu a back-endové IP adresy a portu přidružených k virtuálním počítačům.
* Sondy – sondy umožňují udržování přehledu o stavu instance virtuálních počítačů. Pokud selže test stavu, instance virtuálního počítače se provedou mimo otočení automaticky.
* Příchozí pravidla NAT – příchozí provoz prostřednictvím před definováním pravidel NAT ukončení IP a distribuovat do back-end IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Šablony Rychlý start

Azure Resource Manager umožňuje zřizovat aplikace pomocí deklarativní šablony. S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi. Stejnou šablonu můžete použít k opakovanému nasazení aplikace během každé fáze životního cyklu této aplikace.

Šablony může obsahovat definice pro virtuální počítače, virtuální sítě, skupiny dostupnosti, síťových rozhraní (NIC), účty úložiště, nástroje pro vyrovnávání zatížení, skupiny zabezpečení sítě a veřejné IP adresy. Pomocí šablony můžete vytvářet vše potřebné pro komplexní aplikace. Soubor šablony lze zkontrolovat do systému správy obsahu pro verzí a spolupráci.

[Další informace o šablonách](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Další informace o síťové prostředky](../virtual-network/resource-groups-networking.md)

Šablony rychlý start používá nástroj pro vyrovnávání zatížení Azure lze nalézt v [úložiště GitHub](https://github.com/Azure/azure-quickstart-templates) hostování sadu komunity generované šablon.

Příklady šablon:

* [2 virtuální počítače v Vyrovnávání zatížení a pravidel Vyrovnávání zatížení](http://go.microsoft.com/fwlink/?LinkId=544799)
* [2 virtuální počítače ve virtuální síti s pravidly interní nástroj pro vyrovnávání zatížení a nástroj pro vyrovnávání zatížení](http://go.microsoft.com/fwlink/?LinkId=544800)
* [2 virtuální počítače ve službě Vyrovnávání zatížení a nakonfigurovat na LB pravidla NAT](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Nastavení služby Vyrovnávání zatížení Azure, PowerShell nebo rozhraní příkazového řádku

Začínáme s rutiny Azure Resource Manager, nástroje příkazového řádku a rozhraní REST API

* [Rutiny Azure sítě](https://msdn.microsoft.com/library/azure/mt163510.aspx) slouží k vytvoření služby Vyrovnávání zatížení.
* [Jak vytvořit nástroj pro vyrovnávání zatížení pomocí Azure Resource Manager](load-balancer-get-started-ilb-arm-ps.md)
* [Správa prostředků Azure pomocí Azure CLI](../xplat-cli-azure-resource-manager.md)
* [Rozhraní REST API nástroj pro vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Další kroky

Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) pro konkrétní zatížení vyrovnávání síťové přenosy chování.

Naučte se spravovat [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). To je důležité, pokud aplikace potřebuje k zachování připojení připojení pro servery za službou Vyrovnávání zatížení.
