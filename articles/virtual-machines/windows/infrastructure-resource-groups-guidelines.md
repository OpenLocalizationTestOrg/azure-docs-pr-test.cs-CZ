---
title: "aaaResource skupiny pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení skupin prostředků v služby infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0fbcabcd-e96d-4d71-a526-431984887451
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56b5670ec98bf3e80b7a622d5d760a57a7d20809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-group-guidelines-for-windows-vms"></a>Pokyny pro skupinu prostředků Azure pro virtuální počítače Windows

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení jak toologically vytváří prostředí a skupiny všechny součásti hello ve skupinách prostředků.

## <a name="implementation-guidelines-for-resource-groups"></a>Postup implementace pro skupiny prostředků
Rozhodnutí:

* Budete toobuild na skupiny prostředků hello základní infrastruktury komponenty, nebo nasazení aplikace dokončena?
* Potřebujete přístup tooResource toorestrict skupiny pomocí řízení přístupu na základě rolí?

Úlohy:

* Definujte, co základní součásti infrastruktury a vyhrazené skupiny prostředků, je nutné.
* Zkontrolujte jak tooimplement šablony Resource Manageru pro konzistentní a reprodukovatelné nasazení.
* Definujte, jaké role přístup uživatele je nutné pro řízení přístup tooResource skupiny.
* Vytvořte sadu hello skupin prostředků pomocí zásady vytváření názvů. Můžete použít Azure PowerShell nebo portálu hello.

## <a name="resource-groups"></a>Skupiny prostředků
V Azure můžete logicky seskupují související prostředky, například účty úložiště, virtuální sítě a virtuální počítače (VM) toodeploy, spravovat a udržovat je jako jedna entita. Tento přístup umožňuje snazší toodeploy aplikace při zachování hello všechny související prostředky společně z hlediska správy nebo toogrant ostatní přístup toothat skupiny prostředků. Názvy skupin prostředků, může být maximálně 90 znaků. Získat komplexnější přehled o skupinách prostředků, najdete v tématu hello [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

Klíčovou funkcí tooResource skupiny je možnost toobuild out prostředí pomocí šablon. Šablonu je jednoduše soubor JSON, který deklaruje hello úložiště, sítě a výpočetní prostředky. Můžete také definovat všechny související vlastní skripty nebo tooapply konfigurace. Pomocí těchto šablon, vytvořit konzistentní a reprodukovatelné nasazení pro vaše aplikace. Tento přístup umožňuje snadno toobuild se prostředí při vývoji a potom pomocí této stejné šablony toocreate produkční nasazení, nebo naopak. Pro lepší porozumění pomocí šablon, přečtěte si [hello názorný Průvodce šablonou](../../azure-resource-manager/resource-manager-template-walkthrough.md) který vás provede každého kroku hello vytváření šablony.

Existují dva různé přístupy, které můžete provést při návrhu prostředí ke skupinám prostředků:

* Skupiny prostředků pro každé nasazení aplikace, které kombinuje hello účty úložiště, virtuální sítě a podsítě virtuálních počítačů, načíst vyrovnávání atd.
* Centralizované skupin prostředků, které obsahují vaše základní virtuální sítě a podsítě nebo úložiště účtů. Aplikace se pak ve své vlastní skupiny prostředků, které obsahovat pouze virtuální počítače, nástroje pro vyrovnávání zatížení, síťových rozhraní, atd.

Vertikální navýšení kapacity, vytváření centralizované skupiny prostředků pro virtuální sítě a podsítě umožňuje snazší toobuild mezi různými místy síťová připojení pro možnosti hybridní připojení. alternativní způsob Hello je pro každou aplikaci toohave své vlastní virtuální síť, která vyžaduje konfiguraci a údržbu.  [Řízení přístupu na základě rolí](../../active-directory/role-based-access-control-what-is.md) zadejte pro přístup k granulární způsob toocontrol tooResource skupiny. Aplikacích v produkčním prostředí můžete řídit hello uživatelé, kteří mohou přistupovat k tyto prostředky, nebo pro prostředky infrastruktury základní hello můžete omezit pouze infrastruktury technici toowork s nimi. Vaše vlastníci aplikace mají pouze součásti aplikace toohello přístup v rámci jejich skupinu prostředků a ne hello jádro infrastrukturu Azure vašeho prostředí. Při navrhování prostředí, vezměte v úvahu hello uživatelů, které vyžadují přístup k prostředkům toohello a odpovídajícím způsobem návrh vaší skupiny prostředků. 

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

