---
title: "rychlost spravovat aaa a souběžnost kódování službou Azure Media Services | Microsoft Docs"
description: "Tento článek poskytuje stručný přehled o tom, jak můžete spravovat rychlost a souběžnost kódování úlohy nebo úkoly službou Azure Media Services."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: da52a6278a3d3b084dbf5a594db37df8447bb944
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a><span data-ttu-id="8343a-103">Spravovat rychlost a souběžnost vaše kódování</span><span class="sxs-lookup"><span data-stu-id="8343a-103">Manage speed and concurrency of your encoding</span></span>

<span data-ttu-id="8343a-104">Tento článek poskytuje stručný přehled o tom, jak můžete spravovat rychlost a souběžnosti kódování úlohy a úkoly.</span><span class="sxs-lookup"><span data-stu-id="8343a-104">This article gives a brief overview of how you can manage speed and concurrency of your encoding jobs/tasks.</span></span>

## <a name="overview"></a><span data-ttu-id="8343a-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="8343a-105">Overview</span></span>

<span data-ttu-id="8343a-106">Ve službě Media Services **vyhrazený typ jednotky** určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="8343a-106">In Media Services, a **Reserved Unit Type** determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="8343a-107">Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**.</span><span class="sxs-lookup"><span data-stu-id="8343a-107">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="8343a-108">Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu.</span><span class="sxs-lookup"><span data-stu-id="8343a-108">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span> <span data-ttu-id="8343a-109">Hello [škálování kódování jednotky](media-services-scale-media-processing-overview.md) téma ukazuje tabulku, která pomáhá zajistit rozhodnutí při výběru mezi různými rychlostmi kódování.</span><span class="sxs-lookup"><span data-stu-id="8343a-109">hello [scaling encoding units](media-services-scale-media-processing-overview.md) topic shows a table that helps you make decision when choosing between different encoding speeds.</span></span>

<span data-ttu-id="8343a-110">Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision vašeho účtu pomocí **jednotek rezervovaných**.</span><span class="sxs-lookup"><span data-stu-id="8343a-110">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units**.</span></span> <span data-ttu-id="8343a-111">Hello počet jednotek rezervovaných pro zřízené určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="8343a-111">hello number of provisioned reserved units determines hello number of media tasks that can be processed concurrently in a given account.</span></span> <span data-ttu-id="8343a-112">Například pokud potom pět média úlohy bude běžet současně, pokud má váš účet jednotky rezervované pro pět, jako jsou toobe úlohy zpracování.</span><span class="sxs-lookup"><span data-stu-id="8343a-112">For example, if your account has five reserved units, then five media tasks will be running concurrently as long as there are tasks toobe processed.</span></span> <span data-ttu-id="8343a-113">zbývající úlohy Hello bude čekat ve frontě hello a bude získat zachyceny pro zpracování postupně po dokončení spuštěná úloha.</span><span class="sxs-lookup"><span data-stu-id="8343a-113">hello remaining tasks will wait in hello queue and will get picked up for processing sequentially when a running task finishes.</span></span> <span data-ttu-id="8343a-114">Pokud účet nemá žádné jednotky rezervované pro zřízení, pak úlohy bude být zachyceny postupně.</span><span class="sxs-lookup"><span data-stu-id="8343a-114">If an account does not have any reserved units provisioned, then tasks will be picked up sequentially.</span></span> <span data-ttu-id="8343a-115">V takovém případě hello čekací doba mezi jeden dokončení úkolů a hello další jeden spuštění bude záviset na hello dostupnost prostředků systému hello.</span><span class="sxs-lookup"><span data-stu-id="8343a-115">In this case, hello wait time between one task finishing and hello next one starting will depend on hello availability of resources in hello system.</span></span>

<span data-ttu-id="8343a-116">Podrobné informace a příklady, zobrazující jak tooscale kódování jednotky, najdete v části [to](media-services-scale-media-processing-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="8343a-116">For detailed information and examples that show how tooscale encoding units, see [this](media-services-scale-media-processing-overview.md) topic.</span></span>

## <a name="next-step"></a><span data-ttu-id="8343a-117">Další krok</span><span class="sxs-lookup"><span data-stu-id="8343a-117">Next step</span></span>

[<span data-ttu-id="8343a-118">Kódování jednotky škálování</span><span class="sxs-lookup"><span data-stu-id="8343a-118">Scale encoding units</span></span>](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="8343a-119">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="8343a-119">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="8343a-120">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="8343a-120">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

