---
title: "Azure ML Workbench poznámky k sprintu 3 2018 leden"
description: "Tento dokument podrobně popisuje aktualizace pro verzi sprintu 3 Azure ML"
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: f75fcec3b722563949b6553f17c4f3db3e223675
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="sprint-3---january-2018"></a>Sprint 3 – leden 2018 

#### <a name="version-number-01171218263"></a>Číslo verze: 0.1.1712.18263

>Zde je, jak můžete [najít číslo verze](https://docs.microsoft.com/en-us/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Vítá vás čtvrtý aktualizace Azure Machine Learning Workbench. Dále jsou aktualizace a vylepšení v této sprintu. Řadu tyto aktualizace jsou vytvářeny jako přímý výsledek zpětnou vazbu od uživatelů. 

## <a name="notable-new-features-and-changes"></a>Upozorňují na důležité nové funkce a změny
- Aktualizace zásobníku ověřování vynutí přihlášení a účet výběru při spuštění

## <a name="detailed-updates"></a>Podrobné aktualizace
Následuje seznam podrobné aktualizací v oblasti Azure Machine Learning v této sprintu jednotlivých součástí.

### <a name="workbench"></a>Workbench
- Možnost instalace nebo odinstalace aplikace z panelu Přidat nebo odebrat programy
- Aktualizace zásobníku ověřování vynutí přihlášení a účet výběr na spuštění
- Vylepšené prostředí pro jednotné přihlašování na (SSO) v systému Windows
- Uživatelé, kteří patří do více klientů s odlišnými pověřeními teď bude moci přihlásit do Workbench

#### <a name="ui"></a>UI
- Obecná vylepšení a opravy chyb

### <a name="notebooks"></a>Poznámkové bloky
- Obecná vylepšení a opravy chyb

### <a name="data-preparation"></a>Příprava dat 
- Vylepšené automatické návrhy při provádění podle příkladu transformace
- Vylepšený algoritmus pro vzor frekvence inspector
- Schopnost posílání ukázkových dat a zpětnou vazbu při provádění transformací podle příkladu ![odvozena obrázek odeslat zpětnou vazbu odkaz na sloupec transformace](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Vylepšení Spark modulu Runtime
- Scala nahradili Pyspark
- Opravené neschopnost zavřete dat nelze použít pro nástroj Inspector řady čas 
- Pevné zablokování dobu provádění Data Prep pro HDI

### <a name="model-management-cli-updates"></a>Aktualizace modelu správy rozhraní příkazového řádku 
  - Vlastnictví předplatného se už nevyžaduje pro zřizování prostředků. Přispěvatel přístup do skupiny prostředků jsou dostatečné pro nastavení prostředí pro nasazení.
  - Povolené místní prostředí instalace zdarma odběrů 
