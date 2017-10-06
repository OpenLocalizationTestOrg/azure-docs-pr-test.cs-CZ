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
# <a name="service-fabric-on-linux"></a><span data-ttu-id="e316f-103">Service Fabric v systému Linux</span><span class="sxs-lookup"><span data-stu-id="e316f-103">Service Fabric on Linux</span></span>
<span data-ttu-id="e316f-104">verze preview Hello Service Fabric v systému Linux vám umožní toobuild, nasadit a spravovat vysoce dostupné a vysoce škálovatelné aplikace v systému Linux, stejně jako v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e316f-104">hello preview of Service Fabric on Linux enables you toobuild, deploy, and manage highly available, highly scalable applications on Linux just as you would on Windows.</span></span> <span data-ttu-id="e316f-105">jsou k dispozici v jazyce Java v systému Linux v přidání tooC # (.NET Core) Hello Service Fabric rozhraní (Reliable Services a Reliable Actors).</span><span class="sxs-lookup"><span data-stu-id="e316f-105">hello Service Fabric frameworks (Reliable Services and Reliable Actors) are available in Java on Linux in addition tooC# (.NET Core).</span></span>  <span data-ttu-id="e316f-106">Můžete také vytvořit [spustitelný soubor služby hosta](service-fabric-deploy-existing-app.md) s libovolný jazyk nebo rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e316f-106">You can also build [guest executable services](service-fabric-deploy-existing-app.md) with any language or framework.</span></span> <span data-ttu-id="e316f-107">Kromě toho hello preview podporuje také Orchestrace Docker kontejnery.</span><span class="sxs-lookup"><span data-stu-id="e316f-107">In addition, hello preview also supports orchestrating Docker containers.</span></span> <span data-ttu-id="e316f-108">Kontejnery docker lze spouštět spustitelné soubory hosta nebo nativní služby, Service Fabric, které používají hello Service Fabric rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e316f-108">Docker containers can run guest executables or native Service Fabric services, which use hello Service Fabric frameworks.</span></span>

<span data-ttu-id="e316f-109">Service Fabric v systému Linux je ekvivalentní tooService prostředků infrastruktury v systému Windows (s výjimkou specifika operačního systému a podpora programovacích jazyků).</span><span class="sxs-lookup"><span data-stu-id="e316f-109">Service Fabric on Linux is conceptually equivalent tooService Fabric on Windows (except for OS specifics and programming language support).</span></span> <span data-ttu-id="e316f-110">Proto většina našich [stávající dokumentaci](http://aka.ms/servicefabricdocs) platí v pomoc vám Seznamte se s technologií hello.</span><span class="sxs-lookup"><span data-stu-id="e316f-110">Thus, most of our [existing documentation](http://aka.ms/servicefabricdocs) applies in helping you get familiar with hello technology.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a><span data-ttu-id="e316f-111">Podporované operační systémy a programovací jazyky</span><span class="sxs-lookup"><span data-stu-id="e316f-111">Supported operating systems and programming languages</span></span>
<span data-ttu-id="e316f-112">Hello omezené preview podporuje vytváření hello jeden pole vývoj clustery kromě clustery toomulti počítače v Azure s Ubuntu Server 16.04.</span><span class="sxs-lookup"><span data-stu-id="e316f-112">hello limited preview supports hello creation of one-box development clusters in addition toomulti-machine clusters in Azure running Ubuntu Server 16.04.</span></span> <span data-ttu-id="e316f-113">Hello preview podporuje hello Reliable Actors a hello spolehlivé bezstavové služby rozhraní v jazyce Java a C# v přidání tooguest spustitelných souborů a Orchestrace Docker kontejnery.</span><span class="sxs-lookup"><span data-stu-id="e316f-113">hello preview supports hello Reliable Actors and hello Reliable Stateless Services frameworks in Java and C# in addition tooguest executables and orchestrating Docker containers.</span></span>  

> [!NOTE]
> <span data-ttu-id="e316f-114">Spolehlivé kolekce nejsou zatím nepodporuje v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="e316f-114">Reliable Collections are not supported in Linux yet.</span></span> <span data-ttu-id="e316f-115">Úsporný režim samostatné clustery nejsou podporovány buď – pouze jedno pole a clustery pro víc počítačů Azure Linux jsou podporovány ve verzi preview hello.</span><span class="sxs-lookup"><span data-stu-id="e316f-115">Stand alone clusters aren't supported either - only one box and Azure Linux multi-machine clusters are supported in hello preview.</span></span>
>
>


## <a name="supported-tooling"></a><span data-ttu-id="e316f-116">Podporované nástrojů</span><span class="sxs-lookup"><span data-stu-id="e316f-116">Supported tooling</span></span>
<span data-ttu-id="e316f-117">Hello preview podporuje interakci s hello clusteru pomocí Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e316f-117">hello preview supports interaction with hello cluster through Service Fabric CLI.</span></span> <span data-ttu-id="e316f-118">Pro vývojáře v jazyce Java integrace s Eclipse a Yeoman zajišťuje s Eclipse podporované v systému Linux a na OSX.</span><span class="sxs-lookup"><span data-stu-id="e316f-118">For Java developers, integration with Eclipse and Yeoman is provided with Eclipse supported on Linux and on OSX.</span></span> <span data-ttu-id="e316f-119">Hello OSX integrace používá virtuální počítač s Linuxem pod pokličkou hello prostřednictvím vagrant.</span><span class="sxs-lookup"><span data-stu-id="e316f-119">hello OSX integration uses a Linux VM under hello hood via vagrant.</span></span> <span data-ttu-id="e316f-120">Pro C# vývojáře integrace s Yeoman zajišťuje toogenerate šablon aplikací.</span><span class="sxs-lookup"><span data-stu-id="e316f-120">For C# developers, integration with Yeoman is provided toogenerate application templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e316f-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e316f-121">Next steps</span></span>

* <span data-ttu-id="e316f-122">Seznamte se s hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) programovací rozhraní</span><span class="sxs-lookup"><span data-stu-id="e316f-122">Get familiar with hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) and [Reliable Services](service-fabric-reliable-services-introduction.md) programming frameworks</span></span>
* [<span data-ttu-id="e316f-123">Příprava vývojového prostředí v Linuxu</span><span class="sxs-lookup"><span data-stu-id="e316f-123">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="e316f-124">Příprava vývojového prostředí v OSX</span><span class="sxs-lookup"><span data-stu-id="e316f-124">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="e316f-125">Vytvoření první aplikace Service Fabric Java v systému Linux</span><span class="sxs-lookup"><span data-stu-id="e316f-125">Create your first Service Fabric Java application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="e316f-126">Instalační program Service Fabric průběžnou integraci a nasazení s volaných a GitHub</span><span class="sxs-lookup"><span data-stu-id="e316f-126">Setup Service Fabric continuous integration and deployment with Jenkins and GitHub</span></span>](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [<span data-ttu-id="e316f-127">Rozdíly Service Fabric pro Windows a Linux</span><span class="sxs-lookup"><span data-stu-id="e316f-127">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
