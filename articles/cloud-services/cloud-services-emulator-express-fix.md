---
title: "aplikace služby Cloud Services toodebug v sadě Visual Studio express aaaSetup emulátoru | Microsoft Docs"
description: "Vysvětluje, jak tooinstall hello C++ redistributable tooenable emulátoru Express v sadě Visual Studio"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 22b20f7a-23f4-4f7f-b536-3bf1e01adcd1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 6fb506f0b1384f2e52310799eb5ae2a102d777bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-emulator-express-toodebug-cloud-services-application-in-vs-2017"></a><span data-ttu-id="7a6f0-103">Pomocí emulátoru Express aplikace toodebug cloudové služby v VS 2017</span><span class="sxs-lookup"><span data-stu-id="7a6f0-103">Use Emulator Express toodebug Cloud Services application in VS 2017</span></span>
<span data-ttu-id="7a6f0-104">Tento článek vysvětluje, jak toouse expresní emulátor toodebug cloudové služby aplikací v VS 2017.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-104">This article explains how toouse Emulator Express toodebug Cloud Services applications in VS 2017.</span></span>

## <a name="background-context"></a><span data-ttu-id="7a6f0-105">Kontext pozadí</span><span class="sxs-lookup"><span data-stu-id="7a6f0-105">Background context</span></span>
<span data-ttu-id="7a6f0-106">Expresní emulátor služby se používá ve výchozím nastavení pro ladění Cloud Services – webové a pracovní role v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-106">Emulator Express is used by default for debugging Cloud Services Web and Worker roles in Visual Studio.</span></span> <span data-ttu-id="7a6f0-107">Toto nastavení je zadán hello cloudové služby na stránce vlastností projektu.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-107">This setting is specified in hello Cloud Services project properties page.</span></span>

![Otevřít projekt vlastnosti][0]

![Expresní emulátor je vybraná jako výchozí][1]

<span data-ttu-id="7a6f0-110">Hello [Visual C++ Redistributable] [ Visual C++ Redistributable] pro Visual Studio se vyžaduje emulátorem express.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-110">hello [Visual C++ Redistributable][Visual C++ Redistributable] for Visual Studio is required by Emulator express.</span></span> <span data-ttu-id="7a6f0-111">Aktuálně není nainstalován pomocí hello úloha Azure.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-111">Currently it is not installed with hello Azure workload.</span></span> <span data-ttu-id="7a6f0-112">Při F5 gesty toodebug aplikace cloudové služby, Visual Studio by výzvu tooinstall tato součást a pokračovat v ladění.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-112">Upon F5 gesture toodebug a Cloud Services applications, Visual Studio would prompt tooinstall this component and proceed with debugging.</span></span>

![Řádku pro instalaci C++ Redistributable][2]

<span data-ttu-id="7a6f0-114">Klikněte na tlačítko Ano tooinstall C++ Redistributable.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-114">Click Yes tooinstall C++ Redistributable.</span></span>

![Nainstalujte C++ Redistributable][3]

<span data-ttu-id="7a6f0-116">Stisknutím klávesy F5 znovu toolaunch relace ladění.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-116">Press F5 again toolaunch debugging sessions.</span></span>

![Spuštění ladění][4]

![Ladění úspěšné][5]

> <span data-ttu-id="7a6f0-119">Poznámka: Instalace Visual C++ Redistributable je jednorázově úsilí.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-119">Note: Installing Visual C++ Redistributable is a onetime effort.</span></span> <span data-ttu-id="7a6f0-120">Pokud se upgrade ze starší verze sady Azure SDK a nainstalovali Express emulátoru, nebude výskytu tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-120">If you were upgrading from an older version of Azure SDK and have installed Emulator Express, then you won’t encounter this problem.</span></span>
> 
> 

## <a name="manual-workaround"></a><span data-ttu-id="7a6f0-121">Ruční řešení</span><span class="sxs-lookup"><span data-stu-id="7a6f0-121">Manual workaround</span></span>
<span data-ttu-id="7a6f0-122">Můžete taky nainstalovat hello [Visual C++ Redistributable] [ Visual C++ Redistributable] ručně a stejného efektu se použijí jako jak Visual Studio je v systému nainstalována.</span><span class="sxs-lookup"><span data-stu-id="7a6f0-122">You can also install hello [Visual C++ Redistributable][Visual C++ Redistributable] manually and same effect will be applied as how Visual Studio installed it on your system.</span></span>

<span data-ttu-id="7a6f0-123">[VCRedist_x86.exe][vcredist_x86.exe]</span><span class="sxs-lookup"><span data-stu-id="7a6f0-123">[vcredist_x86.exe][vcredist_x86.exe]</span></span>

<span data-ttu-id="7a6f0-124">[VCRedist_x64.exe][vcredist_x64.exe]</span><span class="sxs-lookup"><span data-stu-id="7a6f0-124">[vcredist_x64.exe][vcredist_x64.exe]</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a6f0-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a6f0-125">Next Steps</span></span>
<span data-ttu-id="7a6f0-126">Další informace o používání Azure počítače emulátoru toodebug aplikace cloudové služby v sadě Visual Studio: [pomocí emulátoru Express toorun a ladění cloudové služby v místním počítači][Using Emulator Express toorun and debug a cloud service on a local machine]</span><span class="sxs-lookup"><span data-stu-id="7a6f0-126">Learn more about using Azure Computer Emulator toodebug your Cloud Services applications in Visual Studio: [Using Emulator Express toorun and debug a cloud service on a local machine][Using Emulator Express toorun and debug a cloud service on a local machine]</span></span>

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Using Emulator Express toorun and debug a cloud service on a local machine]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png
