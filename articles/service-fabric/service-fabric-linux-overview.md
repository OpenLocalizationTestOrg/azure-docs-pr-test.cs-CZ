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
# <a name="service-fabric-on-linux"></a><span data-ttu-id="6e7e8-103">Service Fabric v systému Linux</span><span class="sxs-lookup"><span data-stu-id="6e7e8-103">Service Fabric on Linux</span></span>
<span data-ttu-id="6e7e8-104">Verze preview Service Fabric v systému Linux umožňuje vytvářet, nasazovat a spravovat vysoce dostupné a vysoce škálovatelné aplikace v systému Linux, stejně jako v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-104">The preview of Service Fabric on Linux enables you to build, deploy, and manage highly available, highly scalable applications on Linux just as you would on Windows.</span></span> <span data-ttu-id="6e7e8-105">Rozhraní Service Fabric (Reliable Services a Reliable Actors) jsou k dispozici v jazyce Java v systému Linux kromě C# (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="6e7e8-105">The Service Fabric frameworks (Reliable Services and Reliable Actors) are available in Java on Linux in addition to C# (.NET Core).</span></span>  <span data-ttu-id="6e7e8-106">Můžete také vytvořit [spustitelný soubor služby hosta](service-fabric-deploy-existing-app.md) s libovolný jazyk nebo rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-106">You can also build [guest executable services](service-fabric-deploy-existing-app.md) with any language or framework.</span></span> <span data-ttu-id="6e7e8-107">Kromě toho ve verzi preview podporuje také Orchestrace Docker kontejnery.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-107">In addition, the preview also supports orchestrating Docker containers.</span></span> <span data-ttu-id="6e7e8-108">Kontejnery docker lze spouštět spustitelné soubory hosta nebo nativní služby, Service Fabric, které používají rozhraní Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-108">Docker containers can run guest executables or native Service Fabric services, which use the Service Fabric frameworks.</span></span>

<span data-ttu-id="6e7e8-109">Service Fabric v systému Linux je ekvivalentní Service Fabric v systému Windows (s výjimkou specifika operačního systému a podpora programovacích jazyků).</span><span class="sxs-lookup"><span data-stu-id="6e7e8-109">Service Fabric on Linux is conceptually equivalent to Service Fabric on Windows (except for OS specifics and programming language support).</span></span> <span data-ttu-id="6e7e8-110">Proto většina našich [stávající dokumentaci](http://aka.ms/servicefabricdocs) platí v pomoc vám Seznamte se s technologií.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-110">Thus, most of our [existing documentation](http://aka.ms/servicefabricdocs) applies in helping you get familiar with the technology.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a><span data-ttu-id="6e7e8-111">Podporované operační systémy a programovací jazyky</span><span class="sxs-lookup"><span data-stu-id="6e7e8-111">Supported operating systems and programming languages</span></span>
<span data-ttu-id="6e7e8-112">Omezené verze preview podporuje vytváření clusterů vývoj jeden pole kromě clustery víc počítačů v Azure s Ubuntu Server 16.04.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-112">The limited preview supports the creation of one-box development clusters in addition to multi-machine clusters in Azure running Ubuntu Server 16.04.</span></span> <span data-ttu-id="6e7e8-113">Verze preview podporuje Reliable Actors a spolehlivé bezstavové služby rozhraní v jazyce Java a C# kromě hosta spustitelných souborů a Orchestrace Docker kontejnery.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-113">The preview supports the Reliable Actors and the Reliable Stateless Services frameworks in Java and C# in addition to guest executables and orchestrating Docker containers.</span></span>  

> [!NOTE]
> <span data-ttu-id="6e7e8-114">Spolehlivé kolekce nejsou zatím nepodporuje v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-114">Reliable Collections are not supported in Linux yet.</span></span> <span data-ttu-id="6e7e8-115">Nepodporuje samostatně clustery úsporný režim - buď pouze jedno pole a clustery pro víc počítačů Azure Linux jsou podporovány ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-115">Stand alone clusters aren't supported either - only one box and Azure Linux multi-machine clusters are supported in the preview.</span></span>
>
>


## <a name="supported-tooling"></a><span data-ttu-id="6e7e8-116">Podporované nástrojů</span><span class="sxs-lookup"><span data-stu-id="6e7e8-116">Supported tooling</span></span>
<span data-ttu-id="6e7e8-117">Verze preview podporuje interakci s clusteru pomocí Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-117">The preview supports interaction with the cluster through Service Fabric CLI.</span></span> <span data-ttu-id="6e7e8-118">Pro vývojáře v jazyce Java integrace s Eclipse a Yeoman zajišťuje s Eclipse podporované v systému Linux a na OSX.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-118">For Java developers, integration with Eclipse and Yeoman is provided with Eclipse supported on Linux and on OSX.</span></span> <span data-ttu-id="6e7e8-119">Integrace OSX používá virtuální počítač s Linuxem pod pokličkou prostřednictvím vagrant.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-119">The OSX integration uses a Linux VM under the hood via vagrant.</span></span> <span data-ttu-id="6e7e8-120">Pro C# vývojáře je k dispozici integrace s Yeoman ke generování šablon aplikací.</span><span class="sxs-lookup"><span data-stu-id="6e7e8-120">For C# developers, integration with Yeoman is provided to generate application templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e7e8-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e7e8-121">Next steps</span></span>

* <span data-ttu-id="6e7e8-122">Seznamte se s [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) programovací rozhraní</span><span class="sxs-lookup"><span data-stu-id="6e7e8-122">Get familiar with the [Reliable Actors](service-fabric-reliable-actors-introduction.md) and [Reliable Services](service-fabric-reliable-services-introduction.md) programming frameworks</span></span>
* [<span data-ttu-id="6e7e8-123">Příprava vývojového prostředí v Linuxu</span><span class="sxs-lookup"><span data-stu-id="6e7e8-123">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="6e7e8-124">Příprava vývojového prostředí v OSX</span><span class="sxs-lookup"><span data-stu-id="6e7e8-124">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="6e7e8-125">Vytvoření první aplikace Service Fabric Java v systému Linux</span><span class="sxs-lookup"><span data-stu-id="6e7e8-125">Create your first Service Fabric Java application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="6e7e8-126">Instalační program Service Fabric průběžnou integraci a nasazení s volaných a GitHub</span><span class="sxs-lookup"><span data-stu-id="6e7e8-126">Setup Service Fabric continuous integration and deployment with Jenkins and GitHub</span></span>](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [<span data-ttu-id="6e7e8-127">Rozdíly Service Fabric pro Windows a Linux</span><span class="sxs-lookup"><span data-stu-id="6e7e8-127">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
