---
title: "aaaAzure koncové body služby"
description: "Popisuje nastavení hello koncový bod služby Azure v hello nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 357aa56409a894719077f2c8f302575c8ebb6883
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-endpoints"></a>Koncové body služby Azure
Koncové body služby Azure určí, že zda je aplikace nasazená tooand spravuje hello globální platformy Azure, Azure provozována společností 21Vianet v Číně, nebo soukromé platformy Azure. Hello **koncové body služby** dialogové okno vám umožní toospecify, který chcete toouse koncové body služby. tooopen hello **koncové body služby** dialogové okno, v rámci prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure**a potom klikněte na **Koncové body služby**. Nastavení hello **Active nastavit** pole určuje, které služba Azure koncové body se použije pro hello Azure projekty v aktuálním pracovním prostoru.

Následující Hello ukazuje hello **koncové body služby** dialogové okno.

![][ic719493]

## <a name="tooset-hello-service-endpoints"></a>Koncové body služby tooset hello
V hello **koncové body služby** dialogové okno, proveďte jednu z hello následující akce:

* Pokud chcete, aby toouse hello globální platformy Azure, z hello **Active nastavit** rozevíracího seznamu vyberte **windowsazure.com** a klikněte na tlačítko **OK**.

* Pokud chcete, aby toouse Azure provozované v Číně společností 21Vianet, z hello **Active nastavit** rozevíracího seznamu vyberte **windowsazure.cn (Čína)** a klikněte na tlačítko **OK**.

* Pokud chcete, aby toouse privátní platformy Azure:

  1. Klikněte na **Upravit**.

  2. Otevře dialogové okno oznamující, že hello **koncové body služby** dialogové okno se zavře a hello předvoleb nastaví soubor bude otevřen. Klikněte na **OK**.

  3. V souboru preferencesets.xml hello, vytvořte novou `preferenceset` elementu. Pro tento nový element vytvořit `name`, `blob`, `management`, `portalURL` a `publishsettings` atributy a přidejte hodnoty pro ně, které odpovídají tooyour privátní platformy Azure. Můžete použít hello hodnoty zadané pro existující hello `preferenceset` elementy jako šablony. **Poznámka:**: hello hodnota používaná pro hello `blob` atribut musí obsahovat text hello "blob" v adrese URL hello.

  4. Uložte a zavřete preferencesets.xml.

  5. Znovu otevřete hello **koncové body služby** dialogové okno.

  6. Z hello **Active nastavit** rozevíracího seznamu vyberte hello aktivní nastavit, že jste vytvořili a klikněte na tlačítko **OK**.

  7. Po vytvoření vaší privátní platformy Azure `preferenceset` elementu, můžete změnit hello přiřazených hodnot tooit kliknutím hello **upravit** tlačítka na hello **koncový bod služby** dialogové okno. Můžete také vytvořit několik privátních platformy Azure `preferenceset` prvky, pokud očekáváte.

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse] 

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
