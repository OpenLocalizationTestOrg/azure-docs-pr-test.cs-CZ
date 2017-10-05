---
title: "Přehled Service Fabric a kontejnery | Microsoft Docs"
description: "Přehled Service Fabric a použití kontejnerů k nasazení aplikací mikroslužby. Tento článek obsahuje přehled použití kontejnery a možnosti dostupné v Service Fabric."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: f770b6181a99d24ea6a6e945d505da914e1b6128
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-fabric-and-containers"></a>Service Fabric a kontejnery
> [!NOTE]
> Tato funkce je ve verzi preview pro Linux.  Nasazení kontejnerů do Service Fabric clusteru v systému Windows 10 nepodporuje ještě (už brzy). 
>   

## <a name="introduction"></a>Úvod
Azure Service Fabric je [orchestrator](service-fabric-cluster-resource-manager-introduction.md) služeb mezi clustery počítačů za několik let využití a optimalizace v masivním měřítku služeb společnosti Microsoft. Služby mohou být vytvořeny v mnoha směrech pomocí [Service Fabric programovací modely](service-fabric-choose-framework.md) nasazení [spustitelné soubory hosta](service-fabric-deploy-existing-app.md). Ve výchozím nastavení Service Fabric nasadí a aktivuje tyto služby jako procesy. Procesy poskytují nejrychlejší aktivace a nejvyšší hustotu využití prostředků v clusteru. Service Fabric lze také nasadit services v kontejneru bitové kopie. Je důležité je možné kombinovat služby v procesy a služby v kontejnerech ve stejné aplikaci. 

## <a name="containers-and-service-fabric-roadmap"></a>Kontejnery a plán Service Fabric
V budoucích verzích je mnoho vylepšení plánované pro kontejner orchestration s Service Fabric. Vylepšení zahrnují funkce pro rozšířené sítě s podporou pro virtuální sítě v rámci aplikace, funkce zabezpečení, vylepšené diagnostiky a podpora nástrojů. Service Fabric umožňuje vytvářet aplikace kombinování kontejnery zabalené službou vyvinuté pomocí Service Fabric programovací modely s existující kód (například aplikace IIS MVC).  Několik takové aplikace můžete spustit v jednom clusteru. 

## <a name="what-are-containers"></a>Co jsou kontejnery?
Kontejnery jsou zapouzdřené, jednotlivě nasadit součásti, které běží jako izolovaných instancí na stejné jádra využívat výhody virtualizace, které poskytuje operační systém. Proto každou aplikaci a její runtime, závislosti a systémové knihovny spusťte uvnitř kontejneru s úplnou, soukromý přístup do kontejneru vlastní izolované zobrazení konstrukce operačního systému. Společně s přenositelnost je tato úroveň zabezpečení a prostředků izolace hlavní výhody pro kontejnery pomocí Service Fabric, který jinak běží služby v procesech.

Kontejnery jsou technologie virtualizace, která Virtualizuje příslušný operační systém z aplikací. Kontejnery zadejte neměnné prostředí pro spouštění s různým stupněm izolace aplikací. Kontejnery běží přímo nad jádra a mít pohled izolované systému souborů a dalším prostředkům. Kontejnery ve srovnání s virtuálními počítači, mají následující výhody:

* **Malé**: kontejnery použít ke zvýšení efektivity jednoho úložiště a vrstvy verze a aktualizací.
* **Rychlé**: kontejnery nemusíte spouštěcí celý operační systém, aby se mohl spustit mnohem rychlejší, obvykle v sekundách.
* **Přenositelnost**: kontejnerové aplikace image můžete přenést na spouštění v cloudu, místně, uvnitř virtuálních počítačů, nebo přímo na fyzických počítačů.
* **Zásady správného řízení prostředků**: Kontejner může omezit fyzické prostředky, které můžou využívat na jeho hostitele.

## <a name="container-types"></a>Typy kontejneru
Service Fabric podporuje kontejnery na Linuxu a Windows a na k tomu také podporuje technologie Hyper-V izolovaném režimu. 

### <a name="docker-containers-on-linux"></a>Kontejnery docker v systému Linux
Docker poskytuje vysoké úrovně rozhraní API pro vytváření a Správa kontejnerů nad kontejnery jádra systému Linux. Úložiště docker Hub je centrální úložiště pro ukládání a načítání obrázků kontejneru.
Podívejte se kurz [nasadit kontejner Docker do Service Fabric](service-fabric-get-started-containers-linux.md).

### <a name="windows-server-containers"></a>Windows Server kontejnery
Windows Server 2016 nabízí dva různé typy kontejnery, které se liší v úroveň izolace zadaná. Windows Server kontejnery a Docker kontejnery jsou podobné, protože oba mají obor názvů a soubor izolace systému, ale jádra nasdílet, na kterém běží na hostiteli. V systému Linux, tato izolace tradičně poskytl `cgroups` a `namespaces`, a Windows Server kontejnery chovají podobně.

Systém Windows Hyper-V kontejnery poskytnout další izolace a zabezpečení, protože každý kontejner nesdílí jádra operačního systému, jiné kontejnery nebo s hostiteli. S Tato vyšší úroveň izolace zabezpečení technologie Hyper-V kontejnery cílí na k tomuto účelu, víceklientské scénáře.
Podívejte se kurz [nasazení kontejneru systému Windows pro Service Fabric](service-fabric-get-started-containers.md).

Následující obrázek ukazuje různé typy virtualizace a izolaci úrovně, které jsou k dispozici v operačním systému.
![Platforma Service Fabric][Image1]

## <a name="scenarios-for-using-containers"></a>Scénáře použití kontejnerů
Zde jsou příklady typických kde kontejner je vhodné použít:

* **Služba IIS navýšení a posunutí**: Pokud máte existující [ASP.NET MVC](https://www.asp.net/mvc) aplikace, které chcete nadále používat, jejich umístění v kontejneru místo migrace je ASP.NET Core. Tyto aplikace ASP.NET MVC závisí na Internetové informační služby (IIS). Můžete balíček tyto aplikace do bitové kopie kontejner z image precreated služby IIS a jejich nasazení pomocí Service Fabric. V tématu [kontejneru bitové kopie v systému Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) informace o tom, jak vytvářet bitové kopie služby IIS.
* **Kombinovat kontejnery a mikroslužeb Service Fabric**: pomocí stávající image kontejner pro součást aplikace. Například můžete použít [kontejner nginx a SVÁŽE](https://hub.docker.com/_/nginx/) pro front-end webové aplikace a stavové služby pro více náročné výpočty back-end.
* **Snížení vlivu služby "aktivní okolí"**: možnost zásad správného řízení prostředků kontejnerů můžete použít k omezení prostředků, které služba používá na hostiteli. Pokud služby může využívat mnoho prostředků a vliv na výkon jiných (třeba dlouho běžící, dotaz jako operace), vezměte v úvahu uvedení těchto služeb do kontejnery, které mají zásady správného řízení zdrojů.

## <a name="service-fabric-support-for-containers"></a>Podpora Service Fabric pro kontejnery
Service Fabric teď podporuje nasazení kontejnerů Docker na Linux a Windows Server kontejnery v systému Windows Server 2016, společně s podporou pro technologii Hyper-V izolovaném režimu. 

V Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky. Service Fabric můžete spustit všechny kontejnery a tento scénář je podobná [spustitelné scénář hostovaného](service-fabric-deploy-existing-app.md), kde balíček existující aplikaci uvnitř kontejneru. Tento scénář je běžný případ použití pro kontejnery a příklady spuštění aplikace napsané v libovolném jazyce nebo rozhraní, ale ne pomocí předdefinovaných programovací modely Service Fabric.

Kromě toho můžete spustit služby Service Fabric uvnitř kontejnery také. Podpora pro spuštěné služby Service Fabric uvnitř kontejnery je omezená aktuálně, ale chcete vylepšit v budoucích verzích.

* **Spolehlivé bezstavové služby uvnitř kontejnery**: spolehlivé bezstavové služby pomocí služby spolehlivé programovací modely jsou podporovány pouze v systému Linux. Podpora pro spolehlivé bezstavové služby v kontejnerech Windows je plánovaná pro budoucí použití.
* **Stavové služby uvnitř kontejnery**: tyto služby používat programovacího modelu Reliable Actors nebo spolehlivé služby. Podpora pro spuštění stavové služby v kontejnerech bude přidán v budoucích vydání.

Service Fabric má několik kontejneru možnosti, které vám pomůžou vytvářet aplikace, které se skládají z mikroslužeb, která jsou kontejnerizované. Service Fabric nabízí následující možnosti pro kontejnerové služby:

* Nasazení bitové kopie kontejneru a aktivaci.
* Zásady správného řízení zdrojů.
* Ověření úložiště.
* Port kontejneru na mapování portů hostitele.
* Zjišťování – kontejnery a komunikace.
* Možnost konfigurace a nastavení proměnných prostředí.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli o kontejnery, Service Fabric je kontejner orchestrator, a že Service Fabric obsahuje funkce, které podporují kontejnery. Jako další krok jsme přejděte přes příklady každé funkce, které chcete ukazují, jak je používat.

[Nasazení kontejneru systému Windows pro Service Fabric na Windows Server 2016](service-fabric-get-started-containers.md)

[Nasadit kontejner Docker do Service Fabric v systému Linux](service-fabric-get-started-containers-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png
