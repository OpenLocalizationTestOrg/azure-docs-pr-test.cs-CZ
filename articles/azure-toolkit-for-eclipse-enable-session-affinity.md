---
title: "Povolit spřažení relace pomocí sady nástrojů pro Azure pro Eclipse"
description: "Zjistěte, jak chcete povolit spřažení relace pomocí sady nástrojů pro Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a>Povolit spřažení relace
V rámci sady nástrojů Azure pro prostředí Eclipse můžete povolit spřažení relace HTTP, nebo "trvalé relace", pro své role. Na následujícím obrázku **Vyrovnávání zatížení** dialogové okno Vlastnosti použít k povolení této funkce spřažení relace:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Chcete-li povolit spřažení relace pro vaši roli
1. Klikněte pravým tlačítkem na roli v prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **Azure**a potom klikněte na **Vyrovnávání zatížení**.

2. V **vlastnosti pro vyrovnávání zatížení WorkerRole1** dialogové okno:

   a. Zkontrolujte **spřažení relace povolit HTTP (trvalé relace) pro tuto roli.**

   b. Pro **vstupní koncový bod používat**, vyberte vstupní koncový bod chcete použít, například **http (veřejné: 80, privátní: 8080)**. Aplikace musí používat tento koncový bod jako svůj koncový bod HTTP. Víc koncových bodů můžete povolit pro vaši roli, ale můžete vybrat jen jeden z nich pro podporu trvalé relace.

   c. Sestavení aplikace.

Jakmile bude povoleno, pokud máte více než jednu instanci role, bude požadavky HTTP z konkrétního klienta zpracovanou stejnou instanci role.

Sada nástrojů Eclipse umožňuje to nainstalováním speciální IIS modul s názvem směrování (žádostí na aplikace) do každé instance role. Směrování žádostí na aplikace přesměrovává požadavky HTTP na instanci příslušné role. Sada nástrojů lokalita automaticky překonfiguruje vybraný koncový bod, aby příchozí provoz protokolu HTTP, je nejprve směrovaly softwaru směrování žádostí na aplikace. Sada nástrojů taky vytvoří novou vnitřní koncový bod, Java server je nakonfigurován pro naslouchání na. To je s koncovým bodem používaným směrování žádostí na aplikace přesměrovat provoz protokolu HTTP k instanci příslušné role. Tímto způsobem, každá instance role ve vašem nasazení s více instancemi slouží jako reverzní proxy server pro všechny ostatní instance povolení trvalé relace.

## <a name="notes-about-session-affinity"></a>Poznámky o spřažení relace
* Spřažení relace v emulátoru služby výpočty v nefunguje. Nastavení můžete použít v emulátoru služby výpočty v bez zasahování vašeho procesu sestavení nebo výpočetní emulátor spuštění, ale funkce nefunguje v emulátoru služby výpočty v.

* Povolení spřažení relace, bude mít za následek zvýšení množství místa na disku zabírá nasazení v Azure, jako další software bude stažen a nainstalován do instance role při spuštění služby v cloudu Azure.

* Čas potřebný k inicializaci každou roli bude trvat déle.

* Vnitřní koncový bod, aby fungoval jako rerouter provozu jak je uvedeno nahoře, bude přidána.


## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse] 

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
