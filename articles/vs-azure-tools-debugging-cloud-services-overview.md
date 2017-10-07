---
title: "aaaOptions pro ladění Azure cloud services | Microsoft Docs"
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
ms.openlocfilehash: 8b7779ca33cfd82fd5fbccf229ea822c311bdea0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-hello-various-ways-toodebug-an-azure-cloud-service"></a>Další informace hello různé způsoby, kterými toodebug cloudové služby Azure
Tento článek obsahuje odkazy toohello různé způsoby toodebug cloudové služby Azure. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Ladění cloudové služby Azure v sadě Visual Studio
Můžete uložit čas a peníze pomocí toodebug emulátor výpočtů Azure hello cloudové služby v místním počítači. Ladění služby místně, před nasazením, můžete zlepšit spolehlivost a výkon bez placení pro dobu výpočtů. Ale některé může dojít k chybám pouze při používání cloudové služby v Azure. Chyby, ke kterým dochází pouze při používání cloudové služby v Azure můžete ladit povolením vzdálené ladění při publikování služby a potom připojení instance role tooa hello ladicí program. Další informace najdete v tématu [ladění cloudové služby na místním počítači](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-azure-diagnostics"></a>Použití Azure diagnostiky 
Můžete použít Azure Diagnostics toolog podrobné informace o spouštění kódu v rámci role, zda text hello role běží ve vývojovém prostředí hello nebo v Azure. Další informace najdete v tématu [povolení Azure Diagnostics ve službě Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=400450).

## <a name="using-intellitrace"></a>Použití IntelliTrace 
Pokud používáte Visual Studio Enterprise toowrite role cílové rozhraní .NET Framework 4.5, můžete povolit IntelliTrace v době hello nasazení cloudové služby Azure ze sady Visual Studio. Poskytuje protokolu IntelliTrace, můžete pomocí sady Visual Studio toodebug aplikace jako kdyby byly spuštěny v Azure. Další informace najdete v tématu [ladění publikované Cloudová služba se IntelliTrace a Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Vzdálené ladění 
Můžete povolit vzdálené ladění cloudové služby v době hello při nasazení hello Cloudová služba ze sady Visual Studio. Pokud si zvolíte tooenable vzdáleného ladění pro nasazení, na hello virtuální počítače spouštěné každá instance role jsou nainstalovány služby vzdáleného ladění. Tyto služby – jako `msvsmon.exe` – není ovlivnit výkon nebo způsobit dodatečné náklady. Další informace najdete v tématu [ladění cloudové služby v Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Další kroky
- [Ladění služby Azure cloudové služby nebo virtuálního počítače v sadě Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -další hello podrobnosti o tom, jak toodebug Azure cloudových služeb.
