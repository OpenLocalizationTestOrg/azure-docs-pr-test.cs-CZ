---
title: "Koncové body služby Azure"
description: "Popisuje nastavení koncový bod služby Azure v sadě nástrojů Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9c6125ec-7278-461e-b69c-ed56e844f742
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 6059c292c2687f1bf3d9be04c03aaaaf6adde945
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-endpoints"></a>Koncové body služby Azure
Koncové body služby Azure určí, že jestli vaše aplikace je nasazená a spravuje globální platformy Azure, Azure provozována společností 21Vianet v Číně, nebo soukromé platformy Azure. **Koncové body služby** dialogové okno umožňuje určit, které koncové body služby, kterou chcete použít. Otevřete **koncové body služby** dialogové okno, v rámci prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure**a potom klikněte na **koncové body služby**. Nastavení **Active nastavit** pole určuje, které koncovým bodům služby Azure se použije pro Azure projekty v aktuálním pracovním prostoru.

Následující ukazuje **koncové body služby** dialogové okno.

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Chcete-li nastavit koncové body služby
V **koncové body služby** dialogové okno, proveďte jednu z následujících akcí:

* Pokud chcete použít globální platformy Azure z **Active nastavit** rozevíracího seznamu vyberte **windowsazure.com** a klikněte na tlačítko **OK**.

* Pokud chcete používat Azure provozované v Číně, společností 21Vianet z **Active nastavit** rozevíracího seznamu vyberte **windowsazure.cn (Čína)** a klikněte na tlačítko **OK**.

* Pokud chcete použít privátní platformy Azure:

  1. Klikněte na **Upravit**.

  2. Otevře dialogové okno, vás informuje o který **koncové body služby** zavřou dialogové okno a otevřou se nastaví soubor předvoleb. Klikněte na **OK**.

  3. V souboru preferencesets.xml, vytvořte novou `preferenceset` elementu. Pro tento nový element vytvořit `name`, `blob`, `management`, `portalURL` a `publishsettings` atributů a přidejte hodnoty pro ně, které odpovídají do vaší privátní platformy Azure. Můžete použít hodnoty zadané pro stávající `preferenceset` elementy jako šablony. **Poznámka:**: hodnota použitá `blob` atribut musí obsahovat text "blob" v adrese URL.

  4. Uložte a zavřete preferencesets.xml.

  5. Otevřete **koncové body služby** dialogové okno.

  6. Z **Active nastavit** rozevíracím seznamu vyberte aktivní nastavit, že jste vytvořili a klikněte na tlačítko **OK**.

  7. Po vytvoření vaší privátní platformy Azure `preferenceset` elementu, můžete změnit hodnoty přiřadit klepnutím **upravit** v tlačítko **koncový bod služby** dialogové okno. Můžete také vytvořit několik privátních platformy Azure `preferenceset` prvky, pokud očekáváte.

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse] 

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
