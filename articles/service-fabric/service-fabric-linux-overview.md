---
title: "Azure Service Fabric v systému Linux | Microsoft Docs"
description: "Clusterů Service Fabric podporovat Linux a Java, což znamená, že budete moci nasadit a hostitele Service Fabric aplikace napsané v jazyce Java a C# v systému Linux."
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
ms.openlocfilehash: dddc9f698d9776999d406117b46285a0f90d9620
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="service-fabric-on-linux"></a>Service Fabric v systému Linux
Verze preview Service Fabric v systému Linux umožňuje vytvářet, nasazovat a spravovat vysoce dostupné a vysoce škálovatelné aplikace v systému Linux, stejně jako v systému Windows. Rozhraní Service Fabric (Reliable Services a Reliable Actors) jsou k dispozici v jazyce Java v systému Linux kromě C# (.NET Core).  Můžete také vytvořit [spustitelný soubor služby hosta](service-fabric-deploy-existing-app.md) s libovolný jazyk nebo rozhraní. Kromě toho ve verzi preview podporuje také Orchestrace Docker kontejnery. Kontejnery docker lze spouštět spustitelné soubory hosta nebo nativní služby, Service Fabric, které používají rozhraní Service Fabric.

Service Fabric v systému Linux je ekvivalentní Service Fabric v systému Windows (s výjimkou specifika operačního systému a podpora programovacích jazyků). Proto většina našich [stávající dokumentaci](http://aka.ms/servicefabricdocs) platí v pomoc vám Seznamte se s technologií.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a>Podporované operační systémy a programovací jazyky
Omezené verze preview podporuje vytváření clusterů vývoj jeden pole kromě clustery víc počítačů v Azure s Ubuntu Server 16.04. Verze preview podporuje Reliable Actors a spolehlivé bezstavové služby rozhraní v jazyce Java a C# kromě hosta spustitelných souborů a Orchestrace Docker kontejnery.  

> [!NOTE]
> Spolehlivé kolekce nejsou zatím nepodporuje v systému Linux. Nepodporuje samostatně clustery úsporný režim - buď pouze jedno pole a clustery pro víc počítačů Azure Linux jsou podporovány ve verzi preview.
>
>


## <a name="supported-tooling"></a>Podporované nástrojů
Verze preview podporuje interakci s clusteru pomocí Service Fabric rozhraní příkazového řádku. Pro vývojáře v jazyce Java integrace s Eclipse a Yeoman zajišťuje s Eclipse podporované v systému Linux a na OSX. Integrace OSX používá virtuální počítač s Linuxem pod pokličkou prostřednictvím vagrant. Pro C# vývojáře je k dispozici integrace s Yeoman ke generování šablon aplikací.

## <a name="next-steps"></a>Další kroky

* Seznamte se s [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) programovací rozhraní
* [Příprava vývojového prostředí v Linuxu](service-fabric-get-started-linux.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Vytvoření první aplikace Service Fabric Java v systému Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Instalační program Service Fabric průběžnou integraci a nasazení s volaných a GitHub](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [Rozdíly Service Fabric pro Windows a Linux](service-fabric-linux-windows-differences.md)
