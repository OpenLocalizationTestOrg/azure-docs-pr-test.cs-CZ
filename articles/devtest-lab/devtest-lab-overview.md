---
title: aaaAbout Azure DevTest Labs | Microsoft Docs
description: "Zjistěte, jak můžete DevTest Labs bylo snadné toocreate, spravovat a monitorovat virtuální počítače Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 5e5aa683f80144a2f6872c7fa328016120ea6253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-devtest-labs"></a><span data-ttu-id="23ac7-103">O Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="23ac7-103">About Azure DevTest Labs</span></span>
## <a name="overview"></a><span data-ttu-id="23ac7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="23ac7-104">Overview</span></span>
<span data-ttu-id="23ac7-105">Vývojáři a testerům, sada hledáte toosolve hello zpoždění při vytváření a správu svých prostředí tak, že přejdete toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="23ac7-105">Developers and testers are looking toosolve hello delays in creating and managing their environments by going toohello cloud.</span></span>  <span data-ttu-id="23ac7-106">Azure řeší problém hello prostředí zpoždění a umožňuje samoobslužné služby v rámci nové struktury efektivní náklady.</span><span class="sxs-lookup"><span data-stu-id="23ac7-106">Azure solves hello problem of environment delays and allows self-service within a new cost efficient structure.</span></span>  <span data-ttu-id="23ac7-107">Však vývojáři a testerům, sada stále potřebujete toospend dost času konfigurace jejich samoobslužné obsloužit prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ac7-107">However, developers and testers still need toospend considerable time configuring their self-served environments.</span></span> <span data-ttu-id="23ac7-108">Navíc rozhodují nejste jisti, jak tooleverage hello cloudu toomaximize jejich úsporu nákladů bez přidání příliš mnoho zpracovat režie.</span><span class="sxs-lookup"><span data-stu-id="23ac7-108">Also, decision makers are uncertain about how tooleverage hello cloud toomaximize their cost savings without adding too much process overhead.</span></span>

<span data-ttu-id="23ac7-109">Azure DevTest Labs je služba, která pomáhá vývojářům a testerům, sada rychle vytvořit prostředí v Azure při minimalizaci odpady a řízení nákladů.</span><span class="sxs-lookup"><span data-stu-id="23ac7-109">Azure DevTest Labs is a service that helps developers and testers quickly create environments in Azure while minimizing waste and controlling cost.</span></span> <span data-ttu-id="23ac7-110">Rychlé zřizování prostředí systému Windows a Linux pomocí opakovaně použitelné šablony a artefaktů, můžete otestovat hello nejnovější verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="23ac7-110">You can test hello latest version of your application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span> <span data-ttu-id="23ac7-111">Vaše nasazení kanálu snadno integrate DevTest Labs tooprovision na vyžádání prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ac7-111">Easily integrate your deployment pipeline with DevTest Labs tooprovision on-demand environments.</span></span> <span data-ttu-id="23ac7-112">Škálování zatížení testování zřizování více testovacích agentů a vytvořit předem zřízená prostředí pro školení a ukázky.</span><span class="sxs-lookup"><span data-stu-id="23ac7-112">Scale up your load testing by provisioning multiple test agents, and create pre-provisioned environments for training and demos.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

<span data-ttu-id="23ac7-113">DevTest Labs poskytuje následující výhody při vytváření, konfiguraci a správě developer a testovací prostředí v cloudu hello hello</span><span class="sxs-lookup"><span data-stu-id="23ac7-113">DevTest Labs provides hello following benefits in creating, configuring, and managing developer and test environments in hello cloud</span></span>

## <a name="worry-free-self-service"></a><span data-ttu-id="23ac7-114">Bez obav samoobslužné služby</span><span class="sxs-lookup"><span data-stu-id="23ac7-114">Worry-free self-service</span></span>
<span data-ttu-id="23ac7-115">DevTest Labs umožňuje snazší náklady toocontrol tím, že zásady tooset na vašem testovacím prostředí – například počet virtuálních počítačů (VM) na uživatele a počet virtuálních počítačů za testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ac7-115">DevTest Labs makes it easier toocontrol costs by allowing you tooset policies on your lab - such as number of virtual machines (VM) per user and number of VMs per lab.</span></span> <span data-ttu-id="23ac7-116">DevTest Labs také umožňuje vám toocreate zásady tooautomatically vypnout a spustit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="23ac7-116">DevTest Labs also enables you toocreate policies tooautomatically shut down and start VMs.</span></span>

## <a name="quickly-get-tooready-to-test"></a><span data-ttu-id="23ac7-117">Rychle získat tooready testu</span><span class="sxs-lookup"><span data-stu-id="23ac7-117">Quickly get tooready-to-test</span></span>
<span data-ttu-id="23ac7-118">DevTest Labs umožňuje toocreate předem zřízená prostředí s všechno, co tým potřebuje toostart vývoj a testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="23ac7-118">DevTest Labs enables you toocreate pre-provisioned environments with everything your team needs toostart developing and testing applications.</span></span> <span data-ttu-id="23ac7-119">Jednoduše deklarace identity hello prostředích, kde je nainstalován hello poslední dobrý sestavení vaší aplikace a získat hned pracovat.</span><span class="sxs-lookup"><span data-stu-id="23ac7-119">Simply claim hello environments where hello last good build of your application is installed and get working right away.</span></span> <span data-ttu-id="23ac7-120">Nebo použijte kontejnery pro vytvoření i rychlejší a zeštíhlen prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ac7-120">Or, use containers for even faster and leaner environment creation.</span></span>

## <a name="create-once-use-everywhere"></a><span data-ttu-id="23ac7-121">Vytvořte jednou, používejte všude</span><span class="sxs-lookup"><span data-stu-id="23ac7-121">Create once, use everywhere</span></span>
<span data-ttu-id="23ac7-122">Zaznamenání a sdílet šablony prostředí a artefaktů v rámci váš tým nebo organizace – všechny ve správě zdrojového kódu - toocreate developer a snadno testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="23ac7-122">Capture and share environment templates and artifacts within your team or organization - all in source control - toocreate developer and test environments easily.</span></span>

## <a name="integrates-with-your-existing-toolchain"></a><span data-ttu-id="23ac7-123">Integrace s existujícím řetězcem nástrojů</span><span class="sxs-lookup"><span data-stu-id="23ac7-123">Integrates with your existing toolchain</span></span>
<span data-ttu-id="23ac7-124">Využít předem vytvořené moduly plug-in nebo naše rozhraní API tooprovision vývoje/testování prostředí přímo z vašeho upřednostňované průběžnou integraci (konfigurace) nástroje integrované vývojové prostředí (IDE), nebo automatizované verzi kanálu.</span><span class="sxs-lookup"><span data-stu-id="23ac7-124">Leverage pre-made plug-ins or our API tooprovision Dev/Test environments directly from your preferred continuous integration (CI) tool, integrated development environment (IDE), or automated release pipeline.</span></span> <span data-ttu-id="23ac7-125">Můžete taky Naše komplexní nástroj pro příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="23ac7-125">You can also use our comprehensive command-line tool.</span></span>


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="23ac7-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23ac7-126">Next steps</span></span>
[<span data-ttu-id="23ac7-127">Koncepce DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="23ac7-127">DevTest Labs concepts</span></span>](devtest-lab-concepts.md)

