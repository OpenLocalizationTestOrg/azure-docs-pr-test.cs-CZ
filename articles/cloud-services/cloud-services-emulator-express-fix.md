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
# <a name="use-emulator-express-toodebug-cloud-services-application-in-vs-2017"></a>Pomocí emulátoru Express aplikace toodebug cloudové služby v VS 2017
Tento článek vysvětluje, jak toouse expresní emulátor toodebug cloudové služby aplikací v VS 2017.

## <a name="background-context"></a>Kontext pozadí
Expresní emulátor služby se používá ve výchozím nastavení pro ladění Cloud Services – webové a pracovní role v sadě Visual Studio. Toto nastavení je zadán hello cloudové služby na stránce vlastností projektu.

![Otevřít projekt vlastnosti][0]

![Expresní emulátor je vybraná jako výchozí][1]

Hello [Visual C++ Redistributable] [ Visual C++ Redistributable] pro Visual Studio se vyžaduje emulátorem express. Aktuálně není nainstalován pomocí hello úloha Azure. Při F5 gesty toodebug aplikace cloudové služby, Visual Studio by výzvu tooinstall tato součást a pokračovat v ladění.

![Řádku pro instalaci C++ Redistributable][2]

Klikněte na tlačítko Ano tooinstall C++ Redistributable.

![Nainstalujte C++ Redistributable][3]

Stisknutím klávesy F5 znovu toolaunch relace ladění.

![Spuštění ladění][4]

![Ladění úspěšné][5]

> Poznámka: Instalace Visual C++ Redistributable je jednorázově úsilí. Pokud se upgrade ze starší verze sady Azure SDK a nainstalovali Express emulátoru, nebude výskytu tohoto problému.
> 
> 

## <a name="manual-workaround"></a>Ruční řešení
Můžete taky nainstalovat hello [Visual C++ Redistributable] [ Visual C++ Redistributable] ručně a stejného efektu se použijí jako jak Visual Studio je v systému nainstalována.

[VCRedist_x86.exe][vcredist_x86.exe]

[VCRedist_x64.exe][vcredist_x64.exe]

## <a name="next-steps"></a>Další kroky
Další informace o používání Azure počítače emulátoru toodebug aplikace cloudové služby v sadě Visual Studio: [pomocí emulátoru Express toorun a ladění cloudové služby v místním počítači][Using Emulator Express toorun and debug a cloud service on a local machine]

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
