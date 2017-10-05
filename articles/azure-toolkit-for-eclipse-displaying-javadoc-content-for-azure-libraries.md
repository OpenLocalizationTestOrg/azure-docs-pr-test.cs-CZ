---
title: "Zobrazování obsahu Javadoc v prostředí Eclipse pro balíček knihovny Azure pro jazyk Java"
description: "Postupy: zobrazení obsahu Javadoc pro knihovny Azure Libraries v prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Zobrazování obsahu Javadoc v prostředí Eclipse pro balíček knihovny Azure pro jazyk Java
Obsah Javadoc pro knihovny Azure Libraries for Java lze zobrazit v rámci vašeho prostředí Eclipse tím, že přidružíte Javadoc obsahu do knihovny Azure Libraries for Java. Následující kroky ukazují, jak chcete používat tuto funkci v Eclipse.

Tento postup předpokládá, že jste už přidali knihovny Azure pro jazyk Java na cestu k sestavení.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Chcete-li zobrazit obsah Javadoc v prostředí Eclipse pro knihovny Azure Libraries for Java
* V prostředí Eclipse v prohlížeči projektu klikněte v **odkazuje knihovny** části projektu, otevřete v místní nabídce knihovny Azure pro Java JAR. Například **microsoft windowsazure rozhraní api 0.1.0.jar** (číslo verze může být různé, závisí na verzi, které jste nainstalovali).

* Klikněte na **Vlastnosti**.

* V rámci **vlastnosti** dialogové okno, v levém podokně klikněte na tlačítko **Javadoc umístění**. **Javadoc umístění** se zobrazí dialogové okno.

* Můžete zadat **Javadoc URL**, nebo **Javadoc v archivu**.

   * Pokud zvolíte možnost zadat **Javadoc URL**, jako například použít adresy URL **http://dl.windowsazure.com/javadoc** nebo **http://dl.windowsazure.com/storage/javadoc**.

   * Pokud chcete použít **Javadoc v archivu**, můžete zadat externí soubor nebo soubor pracovního prostoru.

   Zkontrolujte své volby a procházet nebo ověřit podle potřeby. Následující příklad přidruží knihovny Azure Libraries for Java odpovídající Javadoc JAR který byl stažen místně do složky s názvem **c:\MyAzureJARs**.

   ![][ic553487]

* *Volitelný krok*: klikněte na tlačítko **ověření**. Možné problémy s Javadoc JAR může zobrazí tady.

* Klikněte na **OK**.

Jakmile přidružené ke knihovně, obsah Javadoc má zobrazit v rámci vašeho prostředí Eclipse IDE. Například pokud `blob` je definován typ `CloudBlockBlob` v kódu, tady je příklad Javadoc obsahu, který se zobrazí, když zadáte `blob.acquireLease` v kódu:

![][ic553488]

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

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
