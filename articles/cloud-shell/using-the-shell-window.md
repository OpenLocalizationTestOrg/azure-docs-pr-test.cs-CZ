---
title: "okno prostředí cloudu Azure (Preview) hello aaaUsing | Microsoft Docs"
description: "Okno prostředí cloudu Azure hello návod."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.openlocfilehash: 571db3c8e177799a9e05f38a7cf8d2a4d5f8c8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cloud-shell-window"></a><span data-ttu-id="b6e40-103">Pomocí okna hello prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="b6e40-103">Using hello Azure Cloud Shell window</span></span>

<span data-ttu-id="b6e40-104">Tento dokument popisuje, jak toouse hello okno cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6e40-104">This document explains how toouse hello Cloud Shell window.</span></span>

## <a name="concurrent-sessions"></a><span data-ttu-id="b6e40-105">Souběžných relací</span><span class="sxs-lookup"><span data-stu-id="b6e40-105">Concurrent sessions</span></span>
<span data-ttu-id="b6e40-106">Cloudové prostředí umožňuje víc souběžných relací mezi záložkách prohlížeče tím, že každý tooexist relace jako samostatný proces Bash.</span><span class="sxs-lookup"><span data-stu-id="b6e40-106">Cloud Shell enables multiple concurrent sessions across browser tabs by allowing each session tooexist as a separate Bash process.</span></span>
<span data-ttu-id="b6e40-107">Pokud ukončení relace, ujistěte se, tooexit z každé relaci okno každý proces běží nezávisle i když běží na hello stejný počítač.</span><span class="sxs-lookup"><span data-stu-id="b6e40-107">If exiting a session, be sure tooexit from each session window as each process runs independently although they run on hello same machine.</span></span>

## <a name="restart-cloud-shell"></a><span data-ttu-id="b6e40-108">Restartujte cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b6e40-108">Restart Cloud Shell</span></span>
![](media/recycle.png)
> [!WARNING]
> <span data-ttu-id="b6e40-109">Restartování prostředí cloudu obnoví stav počítače a všechny soubory ve vašem souboru není trvalý sdílené složky budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="b6e40-109">Restarting Cloud Shell will reset machine state and any files not persisted by your file share will be lost.</span></span>

* <span data-ttu-id="b6e40-110">Klikněte na ikonu restartování hello na panelu nástrojů tooreceive hello nové prostředí pro cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6e40-110">Click hello restart icon on hello toolbar tooreceive a new Cloud Shell environment.</span></span>

## <a name="minimize--maximize-cloud-shell-window"></a><span data-ttu-id="b6e40-111">Minimalizovat & maximalizace okna cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b6e40-111">Minimize & maximize Cloud Shell window</span></span>
![](media/minmax.png)
* <span data-ttu-id="b6e40-112">Klikněte na tlačítko hello minimalizovat ikonu na hello top napravo od okna toohide hello ho.</span><span class="sxs-lookup"><span data-stu-id="b6e40-112">Click hello minimize icon on hello top right of hello window toohide it.</span></span> <span data-ttu-id="b6e40-113">Klikněte na tlačítko hello cloudové prostředí ikonu znovu toounhide.</span><span class="sxs-lookup"><span data-stu-id="b6e40-113">Click hello Cloud Shell icon again toounhide.</span></span>
* <span data-ttu-id="b6e40-114">Klikněte na tlačítko hello maximalizovat ikonu tooset okno toomax výšku.</span><span class="sxs-lookup"><span data-stu-id="b6e40-114">Click hello maximize icon tooset window toomax height.</span></span> <span data-ttu-id="b6e40-115">okno tooprevious toorestore velikost, kliknutím na tlačítko Obnovit.</span><span class="sxs-lookup"><span data-stu-id="b6e40-115">toorestore window tooprevious size, click restore.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="b6e40-116">Kopírování a vkládání</span><span class="sxs-lookup"><span data-stu-id="b6e40-116">Copy and paste</span></span>
* <span data-ttu-id="b6e40-117">Windows: `Ctrl-insert` toocopy a `Shift-insert` toopaste.</span><span class="sxs-lookup"><span data-stu-id="b6e40-117">Windows: `Ctrl-insert` toocopy and `Shift-insert` toopaste.</span></span> <span data-ttu-id="b6e40-118">Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.</span><span class="sxs-lookup"><span data-stu-id="b6e40-118">Right-click dropdown can also enable copy/paste.</span></span>
  * <span data-ttu-id="b6e40-119">FireFox nebo IE nemusí podporovat schránky oprávnění správně.</span><span class="sxs-lookup"><span data-stu-id="b6e40-119">FireFox/IE may not support clipboard permissions properly.</span></span>
* <span data-ttu-id="b6e40-120">Mac OS: `Cmd-c` toocopy a `Cmd-v` toopaste.</span><span class="sxs-lookup"><span data-stu-id="b6e40-120">Mac OS: `Cmd-c` toocopy and `Cmd-v` toopaste.</span></span> <span data-ttu-id="b6e40-121">Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.</span><span class="sxs-lookup"><span data-stu-id="b6e40-121">Right-click dropdown can also enable copy/paste.</span></span>

## <a name="resize-cloud-shell-window"></a><span data-ttu-id="b6e40-122">Změnit velikost okna cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b6e40-122">Resize Cloud Shell window</span></span>
* <span data-ttu-id="b6e40-123">Klikněte na tlačítko a přetáhněte hello horní okraj hello nástrojů nahoru nebo dolů tooresize hello cloudové prostředí okno.</span><span class="sxs-lookup"><span data-stu-id="b6e40-123">Click and drag hello top edge of hello toolbar up or down tooresize hello Cloud Shell window.</span></span>

## <a name="scrolling-text-display"></a><span data-ttu-id="b6e40-124">Posouvání zobrazení textu</span><span class="sxs-lookup"><span data-stu-id="b6e40-124">Scrolling text display</span></span>
* <span data-ttu-id="b6e40-125">Posuňte se text terminálu toomove myši nebo dotykové.</span><span class="sxs-lookup"><span data-stu-id="b6e40-125">Scroll with your mouse or touchpad toomove terminal text.</span></span>

## <a name="exit-command"></a><span data-ttu-id="b6e40-126">Příkaz exit</span><span class="sxs-lookup"><span data-stu-id="b6e40-126">Exit command</span></span>
<span data-ttu-id="b6e40-127">Spuštění `exit` ukončí aktivní relace hello.</span><span class="sxs-lookup"><span data-stu-id="b6e40-127">Running `exit` terminates hello active session.</span></span> <span data-ttu-id="b6e40-128">K tomuto chování dochází ve výchozím nastavení po 20 minutách bez zásahu.</span><span class="sxs-lookup"><span data-stu-id="b6e40-128">This behavior occurs by default after 20 minutes without interaction.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6e40-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6e40-129">Next steps</span></span>
[<span data-ttu-id="b6e40-130">Rychlý start cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b6e40-130">Cloud Shell Quickstart</span></span>](quickstart.md)
