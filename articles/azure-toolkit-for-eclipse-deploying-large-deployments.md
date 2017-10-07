---
title: "aaaDeploying velká nasazení"
description: "Zjistěte, jak hello toodeploy velkých nasazeních pomocí nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 6b1d2a7a5e49c78154fc856a221e64ca8dcfbe9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-large-deployments"></a>Nasazení velkých nasazeních
Pokud vaše nasazení je příliš velký toobe obsažené ve složce approot výchozí hello, můžete použít místní úložiště prostředků jako hello nasazení kořenové složky pro váš JDK a aplikačního serveru.

## <a name="toouse-a-local-storage-resource-as-hello-deployment-root-folder-for-large-deployments"></a>toouse místní úložiště prostředků jako hello nasazení kořenová složka pro velká nasazení
1. Vytvoření nového prostředku Místní úložiště. Hello název prostředku hello nezáleží. Prostředky úložiště jsou definované na úrovni role hello. Hello nejrychlejší způsob, jak tooaccess hello místní úložiště konfigurace dialogové okno, ve kterém můžete vytvořit nový prostředek Místní úložiště, je pomocí následujících kroků hello: klikněte pravým tlačítkem na hello role v hello **Project Exploreru** zobrazení (rozšířit vaše Projektu Azure uzel Pokud nevidíte hello role), klikněte na tlačítko **Azure**a potom klikněte na **místní úložiště**. V rámci hello **místní úložiště** dialogové okno, klikněte na tlačítko **přidat** toocreate nový prostředek Místní úložiště.

2. Sada hello potřeby tooat velikosti alespoň 2 048 MB paměti (nic méně může mít hello stejné problémy velikost souboru jako by se mohl dostat hello approot).

3. Ujistěte se, že **vyčistit hello obsah po recyklaci hello role instance** je zaškrtnuta možnost; To pomůže zabránit vzniku konfliktu s existující soubory v prostředku hello logika spuštění nasazení hello při hello role instance je recykluje.

4. Ujistěte se, že hello **ukládání proměnné prostředí hello cesta k adresáři prostředku po nasazení** hodnota řetězce toohello **DEPLOYROOT**. Dialogové okno vaše místní úložiště prostředků bude vypadat podobně jako následující toohello.

   ![][ic667943]

Případně pokud používáte **DEPLOYROOT** jako hello *název* místní prostředek, a můžete neměnit název proměnné prostředí automaticky generované hello (které se nastaví příliš **DEPLOYROOT_PATH** v takovém případě), že by fungovat i vaší aplikace.

Další informace o vytvoření prostředku Místní úložiště lze najít na [místní úložiště vlastnosti][Local storage properties].

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse] 

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
