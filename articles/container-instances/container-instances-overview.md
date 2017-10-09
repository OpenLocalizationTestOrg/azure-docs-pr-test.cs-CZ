---
title: "aaaAzure kontejner instancí přehled | Dokumentace Azure"
description: "Vysvětlení služby Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0662ede1260b15d9841bfc2c3c4cec4c30338d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances"></a>Azure Container Instances

Kontejnery se stávají rychle hello upřednostňovaný způsob, jak toopackage, nasadit a spravovat cloudové aplikace. Azure instancí kontejnerů nabízí hello nejrychlejší a nejjednodušší způsob, jak toorun kontejner v Azure, bez nutnosti tooprovision všechny virtuální počítače a bez nutnosti tooadopt vyšší úrovně služby. 

Azure Container Instances je skvělým řešením pro jakýkoli scénář, který může fungovat v izolovaných kontejnerech, včetně jednoduchých aplikací, automatizace úkolů a úloh sestavení. Pro scénáře, kde nepotřebují orchestration kontejneru, včetně zjišťování služby napříč více kontejnerů, automatické škálování a upgrady koordinované aplikací doporučujeme hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

## <a name="fast-startup-times"></a>Rychlé časy spuštění

Kontejnery nabízejí významné výhody při spouštění oproti virtuálním počítačům. Kontejner instancemi Azure můžete v sekundách bez nutnosti tooprovision hello spusťte kontejner v Azure a spravovat virtuální počítače.

## <a name="hypervisor-level-security"></a>Zabezpečení na úrovni hypervisoru

Kontejnery tradičně nabízejí izolaci závislostí aplikace a zásady správného řízení prostředků, ale nebyly považovány za dostatečně odolné pro použití v nehostinném prostředí více tenantů. Díky službě Azure Container Instances je vaše aplikace v kontejneru izolována stejně, jako by byla na virtuálním počítači.

## <a name="custom-sizes"></a>Vlastní velikosti

Kontejnery jsou obvykle optimalizované toorun, který se může značně lišit právě jednu aplikaci, ale hello konkrétním potřebám těchto aplikací. Se službou Azure Container Instances si můžete vyžádat přesně to, co potřebujete z hlediska procesorových jader a paměti. Platí podle co budete požadovat, se účtují podle hello druhé, tak můžete jemně optimalizovat výdajů na základě potřeb.

## <a name="public-ip-connectivity"></a>Připojení pomocí veřejné IP adresy

Instancemi kontejner Azure můžou zpřístupnit kontejnerů přímo toohello internet s veřejnou IP adresu. V budoucích hello jsme bude rozbalte naše sítě integrace tooinclude možnosti s virtuálními sítěmi, zatížení nástroje pro vyrovnávání a dalšími částmi základní hello Azure síťové infrastruktury.

## <a name="persistent-storage"></a>Trvalé úložiště

tooretrieve a zachování stavu s instancemi Azure kontejneru, nabízíme přímé připojení z Azure souborů sdílených složek.

## <a name="linux-and-windows-containers"></a>Kontejnery Windows a Linuxu

Kontejner instancemi Azure můžete naplánovat systému Windows a Linux kontejnery s hello stejné rozhraní API. Jednoduše znamenat hello základní typ operačního systému a všem ostatním identické.

## <a name="co-scheduled-groups"></a>Společně plánované skupiny

Azure Container Instances podporuje plánování skupin více kontejnerů, které sdílejí hostitelský počítač, místní síť, úložiště a životní cyklus. Díky tomu můžete toocombine hlavní aplikace s ostatními funguje v roli podpůrné například protokolování.

## <a name="next-steps"></a>Další kroky

Vyzkoušejte nasazení kontejneru tooAzure pomocí jednoduchého příkazu naše [Průvodce rychlým zahájením](container-instances-quickstart.md).
