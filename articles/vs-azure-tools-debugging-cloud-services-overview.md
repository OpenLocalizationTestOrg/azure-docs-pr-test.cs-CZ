---
title: "Možnosti pro ladění Azure cloud services | Microsoft Docs"
description: "Ladění cloudové služby Azure"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: kraigb
ms.openlocfilehash: cdcd4ca1fbc7e0a2f24122b32148cbda3d6951a0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Další informace o různých způsobech ladění cloudové služby Azure
Tento článek obsahuje odkazy na různé způsoby, jak ladit cloudové služby Azure. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Ladění cloudové služby Azure v sadě Visual Studio
Můžete ušetřit čas a peníze pomocí Azure výpočetní emulátor k ladění cloudové služby v místním počítači. Ladění služby místně, před nasazením, můžete zlepšit spolehlivost a výkon bez placení pro dobu výpočtů. Ale některé může dojít k chybám pouze při používání cloudové služby v Azure. Chyby, ke kterým dochází pouze při používání cloudové služby v Azure můžete ladit povolením vzdálené ladění při publikování služby a pak znovu připojit ladicí program na instanci role. Další informace najdete v tématu [ladění cloudové služby na místním počítači](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-azure-diagnostics"></a>Použití Azure diagnostiky 
Pokud chcete protokolovat podrobné informace z kód spuštěný v rámci role, zda role běží ve vývojovém prostředí nebo v Azure můžete použít Azure Diagnostics. Další informace najdete v tématu [povolení Azure Diagnostics ve službě Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=400450).

## <a name="using-intellitrace"></a>Použití IntelliTrace 
Pokud používáte Visual Studio Enterprise zápis role cílové rozhraní .NET Framework 4.5, můžete povolit IntelliTrace v době nasazení cloudové služby Azure ze sady Visual Studio. IntelliTrace poskytuje protokolu, který můžete použít k ladění aplikace, jako kdyby byly spuštěny v Azure pomocí sady Visual Studio. Další informace najdete v tématu [ladění publikované Cloudová služba se IntelliTrace a Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Vzdálené ladění 
Můžete povolit vzdálené ladění cloudové služby v době, kdy nasazujete službu cloudu v sadě Visual Studio. Pokud zvolíte možnost povolení vzdáleného ladění pro nasazení, na virtuální počítače, které spustí instanci každé role jsou nainstalovány služby vzdáleného ladění. Tyto služby – jako `msvsmon.exe` – není ovlivnit výkon nebo způsobit dodatečné náklady. Další informace najdete v tématu [ladění cloudové služby v Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Další kroky
- [Ladění služby Azure cloudové služby nebo virtuálního počítače v sadě Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -další podrobnosti o tom, jak ladit cloudové služby Azure.