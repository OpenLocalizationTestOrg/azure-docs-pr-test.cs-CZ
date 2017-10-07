---
title: aaaOverview Service Fabric a kontejnery | Microsoft Docs
description: "Přehled Service Fabric a hello použijte kontejnery toodeploy mikroslužbu aplikací. Tento článek obsahuje přehled o tom, jak kontejnery lze a hello dostupné možnosti v Service Fabric."
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
ms.openlocfilehash: fce94c4b476351c90f23f706aab8bc17319cce22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-and-containers"></a>Service Fabric a kontejnery
> [!NOTE]
> Tato funkce je ve verzi preview pro Linux.  Nasazení kontejnerů, které se cluster Service Fabric tooa ve Windows 10 nepodporuje ještě (už brzy). 
>   

## <a name="introduction"></a>Úvod
Azure Service Fabric je [orchestrator](service-fabric-cluster-resource-manager-introduction.md) služeb mezi clustery počítačů za několik let využití a optimalizace v masivním měřítku služeb společnosti Microsoft. Služby mohou být vytvořeny v mnoha směrech pomocí hello [Service Fabric programovací modely](service-fabric-choose-framework.md) toodeploying [spustitelné soubory hosta](service-fabric-deploy-existing-app.md). Ve výchozím nastavení Service Fabric nasadí a aktivuje tyto služby jako procesy. Procesy poskytují hello nejrychlejší aktivace a nejvyšší využití hustotu hello prostředků v clusteru. Service Fabric lze také nasadit services v kontejneru bitové kopie. Je důležité, je možné kombinovat služby v procesy a služby v kontejnerech v hello stejná aplikace. 

## <a name="containers-and-service-fabric-roadmap"></a>Kontejnery a plán Service Fabric
V budoucích verzích je mnoho vylepšení plánované pro kontejner orchestration s Service Fabric. Vylepšení zahrnují funkce pro rozšířené sítě s podporou pro virtuální sítě v rámci aplikace, funkce zabezpečení, vylepšené diagnostiky a podpora nástrojů. Service Fabric umožňuje toocreate aplikace službou vyvinuté pomocí hello Service Fabric programovací modely kombinování kontejnery spojených s existující kód (například aplikace IIS MVC).  Několik takové aplikace můžete spustit v jednom clusteru. 

## <a name="what-are-containers"></a>Co jsou kontejnery?
Kontejnery se zapouzdřené, jednotlivě nasadit součásti, které spustit jako izolovaných instancí na hello stejné jádra tootake výhody virtualizace, které poskytuje operační systém. Proto každou aplikaci a její runtime, závislosti a systémové knihovny spusťte uvnitř kontejneru s úplnou, soukromý přístup toohello kontejneru vlastní izolované zobrazení konstrukce operačního systému. Společně s přenositelnost je tato úroveň zabezpečení a prostředků izolace hello hlavní výhody pro kontejnery pomocí Service Fabric, který jinak běží služby v procesech.

Kontejnery jsou technologie virtualizace, která Virtualizuje hello příslušný operační systém z aplikací. Kontejnery zadejte neměnné prostředí pro aplikace toorun s různým stupněm izolace. Kontejnery běží přímo nad hello jádra a mít izolované zobrazení hello systému souborů a dalším prostředkům. Porovnání toovirtual počítače kontejnery mají hello následující výhody:

* **Malé**: kontejnery používají jednoho místa a vrstva verze a aktualizace tooincrease efektivitu úložiště.
* **Rychlé**: kontejnery nemají tooboot celý operační systém, proto můžou spustit mnohem rychlejší, obvykle v sekundách.
* **Přenositelnost**: bitovou kopii kontejnerizované aplikace lze přenést toorun v cloudu hello místně, uvnitř virtuálních počítačů, nebo přímo na fyzických počítačů.
* **Zásady správného řízení prostředků**: Kontejner může omezit hello fyzické prostředky, které můžou využívat na jeho hostitele.

## <a name="container-types"></a>Typy kontejneru
Service Fabric podporuje kontejnery na Linuxu a Windows a také podporuje režimu izolace technologie Hyper-V na pozdější hello. 

### <a name="docker-containers-on-linux"></a>Kontejnery docker v systému Linux
Docker poskytuje podrobný toocreate rozhraní API a Správa kontejnerů nad kontejnery jádra systému Linux. Úložiště docker Hub je centrálním úložišti toostore a načtení Image kontejneru.
Podívejte se kurz [nasazení tooService kontejner Docker Fabric](service-fabric-get-started-containers-linux.md).

### <a name="windows-server-containers"></a>Windows Server kontejnery
Windows Server 2016 nabízí dva různé typy kontejnery, které se liší v hello úroveň izolace zadaná. Windows Server kontejnery a Docker kontejnery jsou podobné, protože mají obor názvů a souboru systému izolace ale sdílené složky hello jádra s hostitelem hello, na kterém běží na. V systému Linux, tato izolace tradičně poskytl `cgroups` a `namespaces`, a Windows Server kontejnery chovají podobně.

Kontejnery Windows Hyper-V poskytnout další izolace a zabezpečení, protože každý kontejner nesdílí jádra hello operačního systému, jiné kontejnery nebo s hello hostitele. S Tato vyšší úroveň izolace zabezpečení technologie Hyper-V kontejnery cílí na k tomuto účelu, víceklientské scénáře.
Podívejte se kurz [nasazení tooService kontejneru Windows Fabric](service-fabric-get-started-containers.md).

Hello následující obrázek znázorňuje hello různé typy virtualizace a izolaci úrovně, které jsou k dispozici v operačním systému hello.
![Platforma Service Fabric][Image1]

## <a name="scenarios-for-using-containers"></a>Scénáře použití kontejnerů
Zde jsou příklady typických kde kontejner je vhodné použít:

* **IIS navýšení a posunutí**: Pokud máte existující [ASP.NET MVC](https://www.asp.net/mvc) aplikace, které chcete toocontinue toouse, jejich umístění v kontejneru místo migrace je tooASP.NET jádra. Tyto aplikace ASP.NET MVC závisí na Internetové informační služby (IIS). Můžete balíček těchto aplikací do kontejneru bitové kopie z hello precreated bitové kopie služby IIS a jejich nasazení pomocí Service Fabric. Najdete v části [kontejneru bitové kopie v systému Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) informace o bitových kopií toocreate služby IIS.
* **Kombinovat kontejnery a mikroslužeb Service Fabric**: pomocí stávající image kontejner pro součást aplikace. Můžete například použít hello [kontejner nginx a SVÁŽE](https://hub.docker.com/_/nginx/) pro hello webového front-endu vaší aplikace a stavové služby pro hello více náročné výpočetní back-end.
* **Snížení vlivu služby "aktivní okolí"**: můžete použít možnost zásad správného řízení prostředků hello kontejnery toorestrict hello prostředků, které služba používá na hostiteli. Pokud služby může využívat mnoho zdrojů a mají vliv na výkon hello jiných (třeba dlouho běžící, dotaz jako operace), vezměte v úvahu uvedení těchto služeb do kontejnery, které mají zásady správného řízení zdrojů.

## <a name="service-fabric-support-for-containers"></a>Podpora Service Fabric pro kontejnery
Service Fabric teď podporuje nasazení kontejnerů Docker na Linux a Windows Server kontejnery v systému Windows Server 2016, společně s podporou pro technologii Hyper-V izolovaném režimu. 

V hello Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky. Service Fabric můžete spustit všechny kontejnery a scénář hello je podobné toohello [spustitelné scénář hostovaného](service-fabric-deploy-existing-app.md), kde balíček existující aplikaci uvnitř kontejneru. Tento scénář je hello běžné případ použití pro kontejnery a příklady spuštění aplikace napsané v libovolném jazyce nebo rozhraní, ale nepoužíváte programovací modely předdefinované Service Fabric hello.

Kromě toho můžete spustit služby Service Fabric uvnitř kontejnery také. Podpora pro spuštěné služby Service Fabric uvnitř kontejnery je omezena v současné době ale toobe rozšířeného v budoucích verzích.

* **Spolehlivé bezstavové služby uvnitř kontejnery**: spolehlivé bezstavové služby pomocí hello spolehlivé služby programovací modely jsou podporovány pouze v systému Linux. Podpora pro spolehlivé bezstavové služby v kontejnerech Windows je plánovaná pro budoucí použití.
* **Stavové služby uvnitř kontejnery**: tyto služby používat hello Reliable Actors nebo spolehlivé služby programovací model. Podpora pro spuštění stavové služby v kontejnerech bude přidán v budoucích vydání.

Service Fabric má několik kontejneru možnosti, které vám pomůžou vytvářet aplikace, které se skládají z mikroslužeb, která jsou kontejnerizované. Service Fabric nabízí následující možnosti pro kontejnerové služby hello:

* Nasazení bitové kopie kontejneru a aktivaci.
* Zásady správného řízení zdrojů.
* Ověření úložiště.
* Mapování portů toohost port kontejneru.
* Zjišťování – kontejnery a komunikace.
* Možnost tooconfigure a nastavení proměnných prostředí.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli o kontejnery, Service Fabric je kontejner orchestrator, a že Service Fabric obsahuje funkce, které podporují kontejnery. Jako další krok, jsme budou přenášeny po příklady jednotlivých funkcí tooshow hello můžete jak toouse je.

[Nasazení tooService kontejneru Windows Fabric na Windows Server 2016](service-fabric-get-started-containers.md)

[Nasazení tooService kontejner Docker prostředků infrastruktury v systému Linux](service-fabric-get-started-containers-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png
