---
title: "použití spřažení relace aaaEnable hello nástrojů Azure pro Eclipse"
description: "Zjistěte, jak pomocí spřažení relace tooenable hello nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 523e728c58bda95e7af4b242e831694eb6d75cb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-session-affinity"></a>Povolit spřažení relace
V rámci hello nástrojů Azure pro prostředí Eclipse můžete povolit spřažení relace HTTP, nebo "trvalé relace", pro své role. Hello následující obrázek ukazuje hello **Vyrovnávání zatížení** funkce spřažení relace hello tooenable pro dialogové okno Vlastnosti:

![][ic719492]

## <a name="tooenable-session-affinity-for-your-role"></a>spřažení relace tooenable pro vaši roli
1. Klikněte pravým tlačítkem na roli hello v prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **Azure**a potom klikněte na **Vyrovnávání zatížení**.

2. V hello **vlastnosti pro vyrovnávání zatížení WorkerRole1** dialogové okno:

   a. Zkontrolujte **spřažení relace povolit HTTP (trvalé relace) pro tuto roli.**

   b. Pro **vstupní koncový bod toouse**, vyberte toouse vstupní koncový bod, například **http (veřejné: 80, privátní: 8080)**. Aplikace musí používat tento koncový bod jako svůj koncový bod HTTP. Víc koncových bodů můžete povolit pro vaši roli, ale pouze jeden z nich můžete vybrat toosupport trvalé relace.

   c. Sestavení aplikace.

Jakmile bude povoleno, pokud máte více než jednu instanci role, požadavky HTTP z konkrétního klienta bude pokračovat zpracovanou hello stejné role instance.

Hello nástrojů Eclipse umožňuje to nainstalováním speciální IIS modul s názvem směrování (žádostí na aplikace) do každé instance role. Směrování žádostí na aplikace bude přesměrována instanci příslušné role toohello požadavky HTTP. Hello toolkit automaticky změní konfiguraci koncového bodu hello vybrané tak, aby příchozí přenos HTTP hello je první software směrované toohello směrování žádostí na aplikace. Sada nástrojů Hello také vytvoří nový vnitřní koncový bod, který je nakonfigurovaný toolisten na váš server Java. To je koncový bod hello používá směrování žádostí na aplikace tooreroute hello HTTP provoz toohello příslušné role instance. Tímto způsobem, každá instance role ve vašem nasazení s více instancemi slouží jako reverzní proxy server pro všechny hello ostatní instance povolení trvalé relace.

## <a name="notes-about-session-affinity"></a>Poznámky o spřažení relace
* Spřažení relace v emulátoru služby výpočty hello nefunguje. nastavení Hello lze použít v emulátoru služby výpočty hello bez zasahování vašeho procesu sestavení nebo výpočetní emulátor spuštění, ale hello samotné funkce nefunguje v emulátoru služby výpočty hello.

* Povolení spřažení relace, bude mít za následek zvýšení hello množství místa na disku zabírá nasazení v Azure, jako další software bude možné stáhnout a nainstalovat do instance role při spuštění služby v hello cloudu Azure.

* Hello čas tooinitialize každou roli bude trvat déle.

* Vnitřní koncový bod, toofunction jako provoz rerouter, jak je uvedeno nahoře, bude přidána.


## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse] 

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How tooMaintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
