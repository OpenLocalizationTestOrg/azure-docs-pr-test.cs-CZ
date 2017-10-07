---
title: "aaaDisplaying Javadoc obsah Eclipse pro hello balíček knihovny Azure pro jazyk Java"
description: "Jak toodisplay hello Javadoc obsah pro knihovny Azure hello v prostředí Eclipse."
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
ms.openlocfilehash: 8070023a24dc07eca8df906db5b8b662ceed6ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-hello-azure-libraries-package-for-java"></a>Zobrazení obsahu Javadoc v prostředí Eclipse pro hello balíček knihovny Azure pro jazyk Java
Hello Javadoc obsah hello knihovny Azure Libraries for Java lze zobrazit v rámci vašeho prostředí Eclipse tím, že přidružíte hello Javadoc obsahu toohello knihovny Azure Libraries for Java. Hello následující kroky ukazují, jak toouse tuto funkci v Eclipse.

Tento postup předpokládá, že jste už přidali hello knihovny Azure pro cesta sestavení Java tooyour.

## <a name="toodisplay-javadoc-content-in-eclipse-for-hello-azure-libraries-for-java"></a>toodisplay Javadoc obsah Eclipse pro hello knihovny Azure Libraries for Java
* V Eclipse na prohlížeči projektu klikněte v hello **odkazuje knihovny** části projektu, otevřete hello kontextovou nabídku hello knihovny Azure pro Java JAR. Například **microsoft windowsazure rozhraní api 0.1.0.jar** (číslo verze hello může být různé, závisí na verzi, které jste nainstalovali).

* Klikněte na **Vlastnosti**.

* V rámci hello **vlastnosti** dialogové okno, v levém podokně hello, klikněte na tlačítko **Javadoc umístění**. Hello **Javadoc umístění** se zobrazí dialogové okno.

* Můžete zadat **Javadoc URL**, nebo **Javadoc v archivu**.

   * Pokud se rozhodnete toospecify **Javadoc URL**, jako například použít adresy URL hello **http://dl.windowsazure.com/javadoc** nebo **http://dl.windowsazure.com/storage/javadoc**.

   * Pokud se rozhodnete toouse **Javadoc v archivu**, můžete zadat externí soubor nebo soubor pracovního prostoru.

   Zkontrolujte své volby a procházet nebo ověřit podle potřeby. Hello následující příklad přidruží hello knihovny Azure Libraries for Java hello odpovídající Javadoc JAR, který byl stažen místně tooa složku s názvem **c:\MyAzureJARs**.

   ![][ic553487]

* *Volitelný krok*: klikněte na tlačítko **ověření**. Možné problémy s hello Javadoc JAR může zobrazí tady.

* Klikněte na **OK**.

Jakmile přidružené hello knihovně, by měla zobrazovat hello Javadoc obsahu v rámci vašeho prostředí Eclipse IDE. Například pokud `blob` je definován typ `CloudBlockBlob` vašeho kódu hello tady je příklad Javadoc obsahu, který se zobrazí, když zadáte `blob.acquireLease` v kódu:

![][ic553488]

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

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
