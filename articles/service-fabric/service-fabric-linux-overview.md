---
title: "aaaAzure Service Fabric v systému Linux | Microsoft Docs"
description: "Clusterů Service Fabric podporují systémy Linux a Java, což znamená, že budete mít toodeploy a hostitele Service Fabric aplikace napsané v jazyce Java a C# v systému Linux."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 459afade-145d-4ee6-b72b-ddf380ccd1bf
ms.service: service-fabric
ms.devlang: Java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: SubramaR
ms.openlocfilehash: ca0e092b00faec560963c0d6cc379593d085f6a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-on-linux"></a>Service Fabric v systému Linux
verze preview Hello Service Fabric v systému Linux vám umožní toobuild, nasadit a spravovat vysoce dostupné a vysoce škálovatelné aplikace v systému Linux, stejně jako v systému Windows. jsou k dispozici v jazyce Java v systému Linux v přidání tooC # (.NET Core) Hello Service Fabric rozhraní (Reliable Services a Reliable Actors).  Můžete také vytvořit [spustitelný soubor služby hosta](service-fabric-deploy-existing-app.md) s libovolný jazyk nebo rozhraní. Kromě toho hello preview podporuje také Orchestrace Docker kontejnery. Kontejnery docker lze spouštět spustitelné soubory hosta nebo nativní služby, Service Fabric, které používají hello Service Fabric rozhraní.

Service Fabric v systému Linux je ekvivalentní tooService prostředků infrastruktury v systému Windows (s výjimkou specifika operačního systému a podpora programovacích jazyků). Proto většina našich [stávající dokumentaci](http://aka.ms/servicefabricdocs) platí v pomoc vám Seznamte se s technologií hello.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a>Podporované operační systémy a programovací jazyky
Hello omezené preview podporuje vytváření hello jeden pole vývoj clustery kromě clustery toomulti počítače v Azure s Ubuntu Server 16.04. Hello preview podporuje hello Reliable Actors a hello spolehlivé bezstavové služby rozhraní v jazyce Java a C# v přidání tooguest spustitelných souborů a Orchestrace Docker kontejnery.  

> [!NOTE]
> Spolehlivé kolekce nejsou zatím nepodporuje v systému Linux. Úsporný režim samostatné clustery nejsou podporovány buď – pouze jedno pole a clustery pro víc počítačů Azure Linux jsou podporovány ve verzi preview hello.
>
>


## <a name="supported-tooling"></a>Podporované nástrojů
Hello preview podporuje interakci s hello clusteru pomocí Service Fabric rozhraní příkazového řádku. Pro vývojáře v jazyce Java integrace s Eclipse a Yeoman zajišťuje s Eclipse podporované v systému Linux a na OSX. Hello OSX integrace používá virtuální počítač s Linuxem pod pokličkou hello prostřednictvím vagrant. Pro C# vývojáře integrace s Yeoman zajišťuje toogenerate šablon aplikací.

## <a name="next-steps"></a>Další kroky

* Seznamte se s hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) programovací rozhraní
* [Příprava vývojového prostředí v Linuxu](service-fabric-get-started-linux.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Vytvoření první aplikace Service Fabric Java v systému Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Instalační program Service Fabric průběžnou integraci a nasazení s volaných a GitHub](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [Rozdíly Service Fabric pro Windows a Linux](service-fabric-linux-windows-differences.md)
