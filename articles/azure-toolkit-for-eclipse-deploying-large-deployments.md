---
title: "Nasazení velkých nasazeních"
description: "Informace o nasazení pomocí sady nástrojů pro Azure pro Eclipse nasazení ve velkých organizacích."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a>Nasazení velkých nasazeních
Pokud vaše nasazení je příliš velký být obsažené ve složce approot výchozí, můžete použít místní úložiště prostředků jako kořenové složky pro nasazení pro vaše JDK a aplikačního serveru.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Chcete použít místní úložiště prostředků jako kořenové složky pro nasazení pro velká nasazení
1. Vytvoření nového prostředku Místní úložiště. Název prostředku nezáleží. Prostředky úložiště jsou definovány na úrovni role. Nejrychlejší způsob, jak získat přístup k dialogu konfigurace místní úložiště, ze kterého můžete vytvořit nový prostředek Místní úložiště, je pomocí následujících kroků: klikněte pravým tlačítkem na roli v **Project Exploreru** zobrazení (Pokud nevidíte roli, rozbalte uzel vaší Azure projektu), klikněte na tlačítko **Azure**a potom klikněte na **místní úložiště**. V rámci **místní úložiště** dialogové okno, klikněte na tlačítko **přidat** k vytvoření nového prostředku Místní úložiště.

2. Nastavte požadovanou velikost na alespoň 2048 MB (nic méně může mít stejný soubor velikost problémy jako by se mohl dostat approot).

3. Ujistěte se, že **vyčistit obsah po recyklaci role instance** je zaškrtnuta možnost; To pomůže zabránit vzniku konfliktu s existující soubory v prostředku po recyklaci role instance logika spuštění nasazení.

4. Ujistěte se, že **proměnnou prostředí ukládání cesta k adresáři prostředku po nasazení** hodnota nastavena na řetězec **DEPLOYROOT**. Dialogové okno vaše místní úložiště prostředků bude vypadat podobně jako následující.

   ![][ic667943]

Případně pokud používáte **DEPLOYROOT** jako *název* místní prostředek, a můžete neměňte název proměnné prostředí automaticky generované (který bude nastaven na **DEPLOYROOT_PATH** v takovém případě), že bude fungovat pro vaši aplikaci a.

Další informace o vytvoření prostředku Místní úložiště lze najít na [místní úložiště vlastnosti][Local storage properties].

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse] 

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
