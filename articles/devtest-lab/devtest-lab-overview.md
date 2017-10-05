---
title: O Azure DevTest Labs | Microsoft Docs
description: "Zjistěte, jak DevTest Labs můžete snadno vytvářet, spravovat a monitorovat virtuální počítače Azure"
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
ms.openlocfilehash: 62e2d214d6d685c7f27c8c45cae161eb25ed1cbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-azure-devtest-labs"></a><span data-ttu-id="e3d42-103">O Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="e3d42-103">About Azure DevTest Labs</span></span>
## <a name="overview"></a><span data-ttu-id="e3d42-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e3d42-104">Overview</span></span>
<span data-ttu-id="e3d42-105">Vývojáři a testerům, sada hledáte řešení zpoždění při vytváření a správu svých prostředí tak, že přejdete do cloudu.</span><span class="sxs-lookup"><span data-stu-id="e3d42-105">Developers and testers are looking to solve the delays in creating and managing their environments by going to the cloud.</span></span>  <span data-ttu-id="e3d42-106">Azure řeší problém prostředí zpoždění a umožňuje samoobslužné služby v rámci nové struktury efektivní náklady.</span><span class="sxs-lookup"><span data-stu-id="e3d42-106">Azure solves the problem of environment delays and allows self-service within a new cost efficient structure.</span></span>  <span data-ttu-id="e3d42-107">Však vývojáři a testerům, sada stále nutné tráví mnoho času konfigurace jejich samoobslužné obsloužit prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3d42-107">However, developers and testers still need to spend considerable time configuring their self-served environments.</span></span> <span data-ttu-id="e3d42-108">Rozhodují jsou také neví o tom, jak využít cloudu a zajistit tak jejich úsporu nákladů na maximum bez přidání příliš mnoho režijní náklady na proces.</span><span class="sxs-lookup"><span data-stu-id="e3d42-108">Also, decision makers are uncertain about how to leverage the cloud to maximize their cost savings without adding too much process overhead.</span></span>

<span data-ttu-id="e3d42-109">Azure DevTest Labs je služba, která pomáhá vývojářům a testerům, sada rychle vytvořit prostředí v Azure při minimalizaci odpady a řízení nákladů.</span><span class="sxs-lookup"><span data-stu-id="e3d42-109">Azure DevTest Labs is a service that helps developers and testers quickly create environments in Azure while minimizing waste and controlling cost.</span></span> <span data-ttu-id="e3d42-110">Díky rychlému zřizování prostředí systému Windows a Linux pomocí opakovaně použitelných šablon a artefaktů můžete otestovat nejnovější verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3d42-110">You can test the latest version of your application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span> <span data-ttu-id="e3d42-111">Vaše nasazení kanálu snadno integrate DevTest Labs pro zřízení prostředí na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="e3d42-111">Easily integrate your deployment pipeline with DevTest Labs to provision on-demand environments.</span></span> <span data-ttu-id="e3d42-112">Škálování zatížení testování zřizování více testovacích agentů a vytvořit předem zřízená prostředí pro školení a ukázky.</span><span class="sxs-lookup"><span data-stu-id="e3d42-112">Scale up your load testing by provisioning multiple test agents, and create pre-provisioned environments for training and demos.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

<span data-ttu-id="e3d42-113">DevTest Labs poskytuje následující výhody při vytváření, konfiguraci a správě developer a testovací prostředí v cloudu</span><span class="sxs-lookup"><span data-stu-id="e3d42-113">DevTest Labs provides the following benefits in creating, configuring, and managing developer and test environments in the cloud</span></span>

## <a name="worry-free-self-service"></a><span data-ttu-id="e3d42-114">Bez obav samoobslužné služby</span><span class="sxs-lookup"><span data-stu-id="e3d42-114">Worry-free self-service</span></span>
<span data-ttu-id="e3d42-115">DevTest Labs usnadňuje řídit náklady tím, že se vám umožní nastavit zásady na vašem testovacím prostředí – například počet virtuálních počítačů (VM) na uživatele a počet virtuálních počítačů za testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3d42-115">DevTest Labs makes it easier to control costs by allowing you to set policies on your lab - such as number of virtual machines (VM) per user and number of VMs per lab.</span></span> <span data-ttu-id="e3d42-116">DevTest Labs můžete také vytvořit zásady, které automaticky vypne a spustit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e3d42-116">DevTest Labs also enables you to create policies to automatically shut down and start VMs.</span></span>

## <a name="quickly-get-to-ready-to-test"></a><span data-ttu-id="e3d42-117">Rychle získat do připravené testu</span><span class="sxs-lookup"><span data-stu-id="e3d42-117">Quickly get to ready-to-test</span></span>
<span data-ttu-id="e3d42-118">DevTest Labs můžete vytvořit předem zřízená prostředí s vše, co tým potřebuje spustit vývoj a testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="e3d42-118">DevTest Labs enables you to create pre-provisioned environments with everything your team needs to start developing and testing applications.</span></span> <span data-ttu-id="e3d42-119">Jednoduše deklarace identity prostředí, kde je nainstalovaná poslední dobrý sestavení vaší aplikace a získat hned pracovat.</span><span class="sxs-lookup"><span data-stu-id="e3d42-119">Simply claim the environments where the last good build of your application is installed and get working right away.</span></span> <span data-ttu-id="e3d42-120">Nebo použijte kontejnery pro vytvoření i rychlejší a zeštíhlen prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3d42-120">Or, use containers for even faster and leaner environment creation.</span></span>

## <a name="create-once-use-everywhere"></a><span data-ttu-id="e3d42-121">Vytvořte jednou, používejte všude</span><span class="sxs-lookup"><span data-stu-id="e3d42-121">Create once, use everywhere</span></span>
<span data-ttu-id="e3d42-122">Zaznamenání a sdílet šablony prostředí a artefaktů v rámci váš tým nebo organizace, – vše ve správě zdrojového kódu – vytvoření developer a snadno testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3d42-122">Capture and share environment templates and artifacts within your team or organization - all in source control - to create developer and test environments easily.</span></span>

## <a name="integrates-with-your-existing-toolchain"></a><span data-ttu-id="e3d42-123">Integrace s existujícím řetězcem nástrojů</span><span class="sxs-lookup"><span data-stu-id="e3d42-123">Integrates with your existing toolchain</span></span>
<span data-ttu-id="e3d42-124">Využívání předem provedené moduly plug-in nebo našem rozhraní API pro zřízení prostředí pro vývoj/testování přímo z vaší nástroj upřednostňované průběžnou integraci (CI), integrované vývojové prostředí (IDE) nebo automatizované verzi kanálu.</span><span class="sxs-lookup"><span data-stu-id="e3d42-124">Leverage pre-made plug-ins or our API to provision Dev/Test environments directly from your preferred continuous integration (CI) tool, integrated development environment (IDE), or automated release pipeline.</span></span> <span data-ttu-id="e3d42-125">Můžete taky Naše komplexní nástroj pro příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="e3d42-125">You can also use our comprehensive command-line tool.</span></span>


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="e3d42-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3d42-126">Next steps</span></span>
[<span data-ttu-id="e3d42-127">Koncepce DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="e3d42-127">DevTest Labs concepts</span></span>](devtest-lab-concepts.md)

