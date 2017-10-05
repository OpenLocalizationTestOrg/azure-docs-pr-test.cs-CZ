---
title: "Používání okna prostředí cloudu Azure (Preview) | Microsoft Docs"
description: "Návod okno prostředí cloudu Azure."
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
ms.openlocfilehash: a47961dfdaf178a6b793bd68105d9792a9275bb3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-azure-cloud-shell-window"></a><span data-ttu-id="b55a1-103">Používání okna prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="b55a1-103">Using the Azure Cloud Shell window</span></span>

<span data-ttu-id="b55a1-104">Tento dokument vysvětluje, jak použít okno cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b55a1-104">This document explains how to use the Cloud Shell window.</span></span>

## <a name="concurrent-sessions"></a><span data-ttu-id="b55a1-105">Souběžných relací</span><span class="sxs-lookup"><span data-stu-id="b55a1-105">Concurrent sessions</span></span>
<span data-ttu-id="b55a1-106">Cloudové prostředí umožňuje víc souběžných relací mezi záložkách prohlížeče tím, že každou relaci existovat jako samostatný proces Bash.</span><span class="sxs-lookup"><span data-stu-id="b55a1-106">Cloud Shell enables multiple concurrent sessions across browser tabs by allowing each session to exist as a separate Bash process.</span></span>
<span data-ttu-id="b55a1-107">Pokud ukončení relace, je nutné ukončit každé relaci okno každý proces běží nezávisle i když běží na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="b55a1-107">If exiting a session, be sure to exit from each session window as each process runs independently although they run on the same machine.</span></span>

## <a name="restart-cloud-shell"></a><span data-ttu-id="b55a1-108">Restartujte cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b55a1-108">Restart Cloud Shell</span></span>
![](media/recycle.png)
> [!WARNING]
> <span data-ttu-id="b55a1-109">Restartování prostředí cloudu obnoví stav počítače a všechny soubory ve vašem souboru není trvalý sdílené složky budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="b55a1-109">Restarting Cloud Shell will reset machine state and any files not persisted by your file share will be lost.</span></span>

* <span data-ttu-id="b55a1-110">Klikněte na ikonu restartování na panelu nástrojů pro příjem nové prostředí pro cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b55a1-110">Click the restart icon on the toolbar to receive a new Cloud Shell environment.</span></span>

## <a name="minimize--maximize-cloud-shell-window"></a><span data-ttu-id="b55a1-111">Minimalizovat & maximalizace okna cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b55a1-111">Minimize & maximize Cloud Shell window</span></span>
![](media/minmax.png)
* <span data-ttu-id="b55a1-112">Klikněte na ikonu Minimalizovat nahoře napravo od okna ho chcete skrýt.</span><span class="sxs-lookup"><span data-stu-id="b55a1-112">Click the minimize icon on the top right of the window to hide it.</span></span> <span data-ttu-id="b55a1-113">Klikněte na ikonu cloudové prostředí znovu na Zobrazit skryté.</span><span class="sxs-lookup"><span data-stu-id="b55a1-113">Click the Cloud Shell icon again to unhide.</span></span>
* <span data-ttu-id="b55a1-114">Klikněte na ikonu Maximalizovat nastavit okno na maximální výšku.</span><span class="sxs-lookup"><span data-stu-id="b55a1-114">Click the maximize icon to set window to max height.</span></span> <span data-ttu-id="b55a1-115">Pokud chcete obnovit původní velikost okna, kliknutím na tlačítko Obnovit.</span><span class="sxs-lookup"><span data-stu-id="b55a1-115">To restore window to previous size, click restore.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="b55a1-116">Kopírování a vkládání</span><span class="sxs-lookup"><span data-stu-id="b55a1-116">Copy and paste</span></span>
* <span data-ttu-id="b55a1-117">Windows: `Ctrl-insert` zkopírovat a `Shift-insert` vložit.</span><span class="sxs-lookup"><span data-stu-id="b55a1-117">Windows: `Ctrl-insert` to copy and `Shift-insert` to paste.</span></span> <span data-ttu-id="b55a1-118">Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.</span><span class="sxs-lookup"><span data-stu-id="b55a1-118">Right-click dropdown can also enable copy/paste.</span></span>
  * <span data-ttu-id="b55a1-119">FireFox nebo IE nemusí podporovat schránky oprávnění správně.</span><span class="sxs-lookup"><span data-stu-id="b55a1-119">FireFox/IE may not support clipboard permissions properly.</span></span>
* <span data-ttu-id="b55a1-120">Mac OS: `Cmd-c` zkopírovat a `Cmd-v` vložit.</span><span class="sxs-lookup"><span data-stu-id="b55a1-120">Mac OS: `Cmd-c` to copy and `Cmd-v` to paste.</span></span> <span data-ttu-id="b55a1-121">Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.</span><span class="sxs-lookup"><span data-stu-id="b55a1-121">Right-click dropdown can also enable copy/paste.</span></span>

## <a name="resize-cloud-shell-window"></a><span data-ttu-id="b55a1-122">Změnit velikost okna cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b55a1-122">Resize Cloud Shell window</span></span>
* <span data-ttu-id="b55a1-123">Klikněte na tlačítko a přetáhněte ji na horní okraj panelu nástrojů nahoru nebo dolů změňte velikost okna cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="b55a1-123">Click and drag the top edge of the toolbar up or down to resize the Cloud Shell window.</span></span>

## <a name="scrolling-text-display"></a><span data-ttu-id="b55a1-124">Posouvání zobrazení textu</span><span class="sxs-lookup"><span data-stu-id="b55a1-124">Scrolling text display</span></span>
* <span data-ttu-id="b55a1-125">Posouvání pomocí myši nebo dotykové přesunout terminálu text.</span><span class="sxs-lookup"><span data-stu-id="b55a1-125">Scroll with your mouse or touchpad to move terminal text.</span></span>

## <a name="exit-command"></a><span data-ttu-id="b55a1-126">Příkaz exit</span><span class="sxs-lookup"><span data-stu-id="b55a1-126">Exit command</span></span>
<span data-ttu-id="b55a1-127">Spuštění `exit` ukončí aktivní relace.</span><span class="sxs-lookup"><span data-stu-id="b55a1-127">Running `exit` terminates the active session.</span></span> <span data-ttu-id="b55a1-128">K tomuto chování dochází ve výchozím nastavení po 20 minutách bez zásahu.</span><span class="sxs-lookup"><span data-stu-id="b55a1-128">This behavior occurs by default after 20 minutes without interaction.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b55a1-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b55a1-129">Next steps</span></span>
[<span data-ttu-id="b55a1-130">Rychlý start cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="b55a1-130">Cloud Shell Quickstart</span></span>](quickstart.md)
