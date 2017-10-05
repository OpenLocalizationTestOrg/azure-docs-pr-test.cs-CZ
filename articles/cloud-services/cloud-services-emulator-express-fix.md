---
title: "Instalační program Expresní emulátor k ladění aplikací cloudové služby v sadě Visual Studio | Microsoft Docs"
description: "Popisuje postup instalace C++ redistributable povolit emulátoru Express v sadě Visual Studio"
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
ms.openlocfilehash: 05d672dcb1335c617bb8d8cae43947bcd5e9ab3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-emulator-express-to-debug-cloud-services-application-in-vs-2017"></a><span data-ttu-id="13e76-103">Použití emulátoru Express k ladění aplikací cloudové služby v VS 2017</span><span class="sxs-lookup"><span data-stu-id="13e76-103">Use Emulator Express to debug Cloud Services application in VS 2017</span></span>
<span data-ttu-id="13e76-104">Tento článek vysvětluje použití emulátoru Express k ladění aplikací cloudové služby v VS 2017.</span><span class="sxs-lookup"><span data-stu-id="13e76-104">This article explains how to use Emulator Express to debug Cloud Services applications in VS 2017.</span></span>

## <a name="background-context"></a><span data-ttu-id="13e76-105">Kontext pozadí</span><span class="sxs-lookup"><span data-stu-id="13e76-105">Background context</span></span>
<span data-ttu-id="13e76-106">Expresní emulátor služby se používá ve výchozím nastavení pro ladění Cloud Services – webové a pracovní role v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13e76-106">Emulator Express is used by default for debugging Cloud Services Web and Worker roles in Visual Studio.</span></span> <span data-ttu-id="13e76-107">Toto nastavení je zadaný na stránce vlastností projektu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="13e76-107">This setting is specified in the Cloud Services project properties page.</span></span>

![Otevřít projekt vlastnosti][0]

![Expresní emulátor je vybraná jako výchozí][1]

<span data-ttu-id="13e76-110">[Visual C++ Redistributable] [ Visual C++ Redistributable] pro Visual Studio se vyžaduje emulátorem express.</span><span class="sxs-lookup"><span data-stu-id="13e76-110">The [Visual C++ Redistributable][Visual C++ Redistributable] for Visual Studio is required by Emulator express.</span></span> <span data-ttu-id="13e76-111">Aktuálně není nainstalována se zatížením, Azure.</span><span class="sxs-lookup"><span data-stu-id="13e76-111">Currently it is not installed with the Azure workload.</span></span> <span data-ttu-id="13e76-112">Při F5 gesto k ladění aplikací cloudové služby by Visual Studio výzvu k instalaci této součásti a pokračovat v ladění.</span><span class="sxs-lookup"><span data-stu-id="13e76-112">Upon F5 gesture to debug a Cloud Services applications, Visual Studio would prompt to install this component and proceed with debugging.</span></span>

![Řádku pro instalaci C++ Redistributable][2]

<span data-ttu-id="13e76-114">Kliknutím na tlačítko Ano nainstalujte C++ Redistributable.</span><span class="sxs-lookup"><span data-stu-id="13e76-114">Click Yes to install C++ Redistributable.</span></span>

![Nainstalujte C++ Redistributable][3]

<span data-ttu-id="13e76-116">Stisknutím klávesy F5 spusťte ladění relací.</span><span class="sxs-lookup"><span data-stu-id="13e76-116">Press F5 again to launch debugging sessions.</span></span>

![Spuštění ladění][4]

![Ladění úspěšné][5]

> <span data-ttu-id="13e76-119">Poznámka: Instalace Visual C++ Redistributable je jednorázově úsilí.</span><span class="sxs-lookup"><span data-stu-id="13e76-119">Note: Installing Visual C++ Redistributable is a onetime effort.</span></span> <span data-ttu-id="13e76-120">Pokud se upgrade ze starší verze sady Azure SDK a nainstalovali Express emulátoru, nebude výskytu tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="13e76-120">If you were upgrading from an older version of Azure SDK and have installed Emulator Express, then you won’t encounter this problem.</span></span>
> 
> 

## <a name="manual-workaround"></a><span data-ttu-id="13e76-121">Ruční řešení</span><span class="sxs-lookup"><span data-stu-id="13e76-121">Manual workaround</span></span>
<span data-ttu-id="13e76-122">Můžete taky nainstalovat [Visual C++ Redistributable] [ Visual C++ Redistributable] ručně a stejného efektu se použijí jako jak Visual Studio je v systému nainstalována.</span><span class="sxs-lookup"><span data-stu-id="13e76-122">You can also install the [Visual C++ Redistributable][Visual C++ Redistributable] manually and same effect will be applied as how Visual Studio installed it on your system.</span></span>

<span data-ttu-id="13e76-123">[VCRedist_x86.exe][vcredist_x86.exe]</span><span class="sxs-lookup"><span data-stu-id="13e76-123">[vcredist_x86.exe][vcredist_x86.exe]</span></span>

<span data-ttu-id="13e76-124">[VCRedist_x64.exe][vcredist_x64.exe]</span><span class="sxs-lookup"><span data-stu-id="13e76-124">[vcredist_x64.exe][vcredist_x64.exe]</span></span>

## <a name="next-steps"></a><span data-ttu-id="13e76-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13e76-125">Next Steps</span></span>
<span data-ttu-id="13e76-126">Další informace o použití emulátoru počítače Azure k ladění aplikací cloudové služby v sadě Visual Studio: [pomocí emulátoru Express ke spouštění a ladění cloudové služby v místním počítači][Using Emulator Express to run and debug a cloud service on a local machine]</span><span class="sxs-lookup"><span data-stu-id="13e76-126">Learn more about using Azure Computer Emulator to debug your Cloud Services applications in Visual Studio: [Using Emulator Express to run and debug a cloud service on a local machine][Using Emulator Express to run and debug a cloud service on a local machine]</span></span>

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Using Emulator Express to run and debug a cloud service on a local machine]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png
